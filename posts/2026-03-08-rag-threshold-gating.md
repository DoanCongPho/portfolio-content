---
title: "Cosine threshold 0.55: con số mặc định không có nghĩa là con số đúng"
date: "2026-03-08"
tags: ["rag", "embeddings", "experiments"]
summary: "Note ngắn về quá trình tune similarity threshold cho RAG retrieval. Vì sao 0.55 là điểm gate ở dự án này, và tại sao con số đó hoàn toàn không transferable."
---

Khi xây RAG retrieval pipeline cho Workplace NPC Engine, tôi dùng `nomic-embed-text` qua Ollama và FAISS làm vector store. Câu hỏi: với mỗi user query, document nào nên được inject vào context?

Cách dễ nhất là top-k. Nhưng top-k luôn trả về k document — kể cả khi không có document nào thực sự liên quan. Kết quả: prompt pollution. Agent nhận được context rác và trả lời lạc đề.

## Threshold gating thay vì top-k

Tôi đổi sang threshold gating: chỉ inject document nào có cosine similarity với query ≥ một ngưỡng cụ thể.

Câu hỏi tiếp theo: **ngưỡng đó là bao nhiêu?**

## Quá trình tune

Tôi build một eval set 120 query, chia làm ba loại:

| Loại | Số lượng | Mô tả |
|---|---|---|
| In-domain, có context | 50 | Có ít nhất 1 document thực sự trả lời được |
| In-domain, không có context | 40 | Câu hỏi đúng domain nhưng repo không có tài liệu liên quan |
| Out-of-domain | 30 | Câu hỏi sai domain hoàn toàn |

Mỗi query, chạy retrieval với 8 threshold khác nhau từ 0.3 đến 0.7, đo:
- **Precision**: trong các document được retrieve, bao nhiêu thực sự liên quan?
- **Recall**: trong các document thực sự liên quan, bao nhiêu được retrieve?
- **Empty-rate**: bao nhiêu query không retrieve được document nào (đúng với loại 2 và 3, sai với loại 1)?

Threshold 0.55 cho điểm cân bằng nhất:

```
threshold  precision  recall  empty(correct)  empty(wrong)
0.45         0.62      0.91        72%           14%
0.50         0.71      0.86        81%           9%
0.55         0.79      0.78        87%           5%
0.60         0.86      0.61        92%           3%
```

Ở 0.60 precision tốt hơn nhưng recall giảm mạnh — agent thường xuyên thiếu context cần thiết. Ở 0.50, agent retrieve quá nhiều rác.

## Tại sao 0.55 không phải con số có thể tái sử dụng

Hai dự án khác nhau, threshold tối ưu sẽ khác nhau. Vì:

1. **Embedding model khác nhau** sẽ có distribution similarity khác nhau. `text-embedding-3-small` của OpenAI có distribution khác hẳn `nomic-embed-text`.
2. **Domain khác nhau** sẽ có vocabulary overlap khác nhau. Domain hẹp (medical, legal) có baseline similarity cao hơn domain rộng.
3. **Query style khác nhau** — câu hỏi ngắn vs câu hỏi dài, chat vs search query, đều ảnh hưởng.

> Mỗi khi tôi đọc bài blog hoặc tutorial nói "set threshold to 0.7 for cosine similarity," tôi luôn nghĩ: số đó đến từ đâu? Eval set nào? Embedding model nào?

Bài học: con số trong README của ai khác là **điểm khởi đầu**, không phải đáp án. Nếu bạn không có eval set của riêng mình, threshold gating cũng chỉ là top-k với extra steps.
