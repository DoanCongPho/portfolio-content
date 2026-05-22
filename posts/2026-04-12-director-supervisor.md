---
title: "Director supervisor: ba tín hiệu để biết khi nào agent đang đi lạc"
date: "2026-04-12"
tags: ["langgraph", "agents", "rag"]
summary: "Khi nào nên inject coaching cue vào system prompt? Note về thiết kế Director supervisor cho Workplace NPC Engine — semantic loop detection, progress stall, và domain drift."
---

Trong [Workplace NPC Engine](https://github.com/DoanCongPho/workplace-npc-engine), tôi gặp một vấn đề kinh điển của multi-agent systems: agent biết cách trả lời, nhưng không biết khi nào nên **dừng** trả lời theo cách đó.

Cụ thể: hai NPC AI co-worker (CEO và CHRO) đôi khi rơi vào loop ngữ nghĩa — họ paraphrase cùng một ý ba bốn lần liên tiếp, hoặc trượt khỏi domain ban đầu (workplace coaching) sang chủ đề khác mà người chơi không yêu cầu.

Giải pháp: một **Director supervisor agent** chạy song song với hai NPC, không tham gia trực tiếp vào cuộc thoại, nhưng quan sát và quyết định khi nào cần inject một coaching cue vào system prompt của NPC chính.

## Ba tín hiệu detection

Director theo dõi ba tín hiệu độc lập:

### 1. Semantic loop detection

Embed mỗi message gần nhất bằng `all-MiniLM-L6-v2`, so cosine similarity giữa N message cuối. Nếu trung bình > 0.82, đánh dấu là loop.

```python
def detect_loop(history, model, n=4, threshold=0.82):
    if len(history) < n:
        return False
    embs = model.encode([m.content for m in history[-n:]])
    sims = [cosine(embs[i], embs[i+1]) for i in range(len(embs)-1)]
    return sum(sims) / len(sims) > threshold
```

### 2. Progress stall

Mỗi turn, agent phải advance ít nhất một sub-goal trong state machine. Nếu 3 turn liên tiếp không có sub-goal nào move forward, là stall.

### 3. Domain drift

Cosine similarity giữa embedding của câu trả lời và centroid của domain reference set. Nếu < 0.45, agent đã trôi khỏi domain.

## Vì sao cần ba tín hiệu, không phải một?

Tôi đã thử dùng chỉ semantic loop detection trước. Nó bắt được paraphrase loop nhưng **không** bắt được trường hợp agent trả lời mạch lạc, không lặp — nhưng đang đi lạc khỏi mục tiêu coaching. Domain drift là tín hiệu bổ sung cho case đó.

Progress stall là tín hiệu sớm nhất trong ba cái — nó kích hoạt trước khi loop hoặc drift xuất hiện rõ ràng. Trong production, ~70% coaching cue được trigger bởi progress stall.

## Kết quả sau hai tuần

- Conversation length tăng từ trung bình 8 turn lên 14 turn trước khi player rage-quit (đo bằng việc đóng tab giữa chừng).
- 23% conversation có ít nhất một coaching cue được inject; trong số đó, 81% tiếp tục thêm ≥3 turn nữa.
- False positive (Director inject cue khi không cần thiết) ở khoảng 12% — vẫn cao, đang điều chỉnh threshold.

> Bài học chính: supervisor không phải là một agent thông minh hơn các agent khác. Nó là một agent với **góc nhìn khác** — quan sát meta-state mà các agent chính không có quyền truy cập.

Sẽ viết tiếp về cách rapport scoring với exponential momentum được tích hợp với Director ở post sau.
