---
title: "Lần đầu thi hackathon: 36 tiếng, một team, và một sản phẩm"
date: "2026-05-22"
tags: ["hackathon", "lotushacks", "edtech", "ai-agents"]
summary: "Note lại trải nghiệm Lotushacks 2026 (x HackHarvard x GenAI Fund) — quá trình brainstorm ý tưởng, lý do tụi mình chọn edtech, và những thứ học được từ team khác mà mình thấy đáng giá hơn cả giải thưởng."
---

![Lotushacks 2026 — venue](https://raw.githubusercontent.com/DoanCongPho/portfolio-content/main/posts/images/lotushacks-2026/SON07529.JPG)

Chào mọi người, đây là lần đầu mình tham gia hackathon — [Lotushacks 2026](https://lotushacks2026.devpost.com/), tổ chức bởi Lotushacks × HackHarvard Undergraduate Hackathon × GenAI Fund. 36 tiếng, một team, một sản phẩm để giải một vấn đề có thật. Post này mình note lại quá trình từ brainstorm tới demo, và quan trọng hơn — những thứ mình học được mà mình nghĩ đáng để chia sẻ với các bạn đang cân nhắc thi.

## Hackathon là gì, và vì sao dev nên thi

Hackathon, nói đơn giản, là một cuộc thi mà bạn có một khoảng thời gian rất ngắn — thường 24 đến 48 tiếng — để build ra một sản phẩm hoàn chỉnh (hoặc gần hoàn chỉnh) cùng team. Ban tổ chức cho sẵn một vài **track** (chủ đề lớn — ví dụ edtech, fintech, healthcare, social impact), bạn pick track, brainstorm idea, code, demo, pitch trước ban giám khảo.

Với Lotushacks năm nay, vì là giải về AI, ban tổ chức tài trợ credit từ một loạt service: tinyfish, ElevenLabs, Trae, Exa, và đặc biệt là **$100 OpenAI credit cho từng người**. Đến giờ mình vẫn còn credit để xài cho việc học.

Vì sao mình nghĩ một software dev nên thi ít nhất một lần:

- **Bạn build dưới constraint thật.** Không có thời gian over-engineer. Cái gì không cần thiết thì cắt. Cái gì cần thì làm ngay. Đây là kĩ năng mà đọc sách không dạy được — chỉ có deadline mới dạy.
- **Bạn thấy được code mình ở một tốc độ khác.** Bình thường mình hay loay hoay với một feature mấy ngày. Ở hackathon, một feature phải xong trong vài tiếng. Bạn nhận ra mình thực sự có thể nhanh hơn mình tưởng.
- **Bạn được tiếp xúc với tooling mà bình thường tự xài thì tiếc tiền.** Credit AI, vector DB hosted, API premium — sponsor cho hết.
- **Network.** Mình quen được các bạn cùng lĩnh vực mà sau hackathon vẫn còn giữ liên lạc.

## Quá trình brainstorm: những idea không được chọn

![Team brainstorm session](https://raw.githubusercontent.com/DoanCongPho/portfolio-content/main/posts/images/lotushacks-2026/SON06626.JPG)

Trước hackathon, team mình gặp vài buổi ở cafe để bàn idea. Mình note lại mấy idea đáng nhớ — kể cả những cái cuối cùng không chọn — vì process loại idea cũng là một bài học:

**1. Automated outreach system.** User input thông tin về service / sản phẩm của công ty, hệ thống tự match với potential leads và gửi email outreach. Vì giải về AI, team định scale thêm: thay vì email thì gọi điện trực tiếp, agent take note nội dung cuộc gọi rồi follow-up tự động. Idea này khá ổn nhưng cảm giác hơi gần với mấy sản phẩm sales automation đang có trên thị trường.

**2. Micro-aggression detection.** Idea này đến từ một bạn trong team đọc paper HCI. Vấn đề: trong môi trường làm việc đa văn hóa, một câu nói vô tình có thể vi phạm chuẩn mực văn hóa / tôn giáo của đồng nghiệp mà mình không biết (ví dụ làm việc với colleague Ấn Độ mà nói chuyện về beefsteak). Hệ thống sẽ detect những phát ngôn này và notify người dùng kèm recommendation về cách phrasing lại. Vui và sáng tạo, nhưng tụi mình thấy hơi niche — khó scale.

**3. Process pipeline generation.** Lấy cảm hứng từ [roadmap.sh](https://roadmap.sh/). Idea: cho user input một mục tiêu (ví dụ "trở thành ML engineer"), hệ thống generate pipeline học tập tùy chỉnh. Cái này thú vị nhưng tụi mình cảm giác chưa thấy được edge so với mấy LLM playground hiện tại.

Cuối cùng tụi mình chọn track **Education / EdTech** với một product có pain point rõ ràng hơn.

## ScholarPath: sản phẩm cuối cùng

Link devpost: [ScholarPath](https://devpost.com/software/scholarpath-19ufyc).

Insight ban đầu: học sinh cấp 3 và sinh viên VN khi tìm học bổng / chương trình du học thường phải search rời rạc — mỗi trường một website, mỗi học bổng một format. Không có chỗ tập trung để **match** profile với cơ hội phù hợp. Tụi mình muốn build cái app đó.

Bốn feature chính:

- **AI chatbox matching.** User chat với hệ thống về ability, interest, financial situation, hệ thống lưu progressive profile của user và đặt câu hỏi follow-up để lấy thêm insight. Output: danh sách học bổng / chương trình match.
- **Dataset xây từ crawler.** Tụi mình viết crawler để pull thông tin từ các trang học bổng public và format về cùng một schema chung — `header`, `body { requirements, estimate_fee }`. Tất cả structured để search và filter được.
- **Check My Fit.** User chọn một university / course cụ thể, hệ thống đánh giá match score trên thang 100 dựa trên profile hiện tại — và quan trọng hơn, chỉ ra **bạn còn thiếu gì** để đủ điều kiện.
- **Alumni Match.** Lấy ý tưởng từ LinkedIn — connect user với alumni của trường / chương trình để xin kinh nghiệm thực tế và hỏi về những thông tin không có trên brochure.

Stack chính: Next.js cho frontend, Python cho crawler và pipeline, OpenAI cho LLM layer, vector store cho semantic match.

## Cảm nhận: cực mà vui

![Đêm code và pizza](https://raw.githubusercontent.com/DoanCongPho/portfolio-content/main/posts/images/lotushacks-2026/z7855231961729_d624af1d8363b163892b929ab34bed5b.jpg)

Cực thì có cực thật:

- Tranh nhau từng cái gối ngủ.
- Xếp hàng để được ăn.
- Thức khuya code, thiếu ngủ, nhìn lung tung.

Nhưng overall, **vui hơn cực rất nhiều**. Pizza ngon, có photobooth, có mấy buổi hội thảo AI xen kẽ giữa các phiên code, được ngồi code xuyên đêm với anh em cùng team, connect với các bạn cùng lĩnh vực. Đây là loại trải nghiệm mà bạn sẽ nhớ rất lâu — không phải vì nó dễ chịu, mà vì nó intense.

## Những thứ học được — chủ yếu từ team khác

Thành thật mà nói, kĩ năng code và software process của mình có cải thiện sau 3 ngày — nhưng phần học được nhiều nhất lại đến từ việc **nhìn các team khác làm gì**. Mình bị choáng vì trong 36 tiếng họ build ra được những thứ vừa sáng tạo, vừa thực tế. Kể vài cái mình ấn tượng nhất:

- **[Coco](https://devpost.com/software/coco-nhaui0)** — education video tương tác cho trẻ em. Họ build một website render video kèm cơ chế để trẻ em tương tác trực tiếp với video (kiểu pause + prompt). Một product có chất lượng giáo dục thật, không phải demo cho có.
- **[SOSnow](https://devpost.com/software/sosnow-d0eplh)** — app cứu hộ với AI agent túc trực điện thoại 24/7. Chỗ hay nhất: vì dùng sóng điện thoại nên không cần wifi — nạn nhân chỉ cần gọi tới số, agent take note thông tin và gửi cứu hộ. Agent có thể scale ra nhiều máy nên đủ throughput. Idea này thật sự practical với context Việt Nam.
- **[FocusIQ](https://devpost.com/software/focusiq)** — nhóm này đem cả phần cứng vào hackathon để detect distraction khi user học bài. Notify khi user "out of the zone". Mức độ commit của họ khiến mình thấy mình chưa go hard enough.

Còn rất nhiều project khác mình không liệt kê — các bạn search devpost của Lotushacks 2026 sẽ thấy.

## Closing

Hackathon để lại trong mình quá nhiều kỷ niệm và bài học cho một event chỉ vỏn vẹn 3 ngày. Nếu bạn đang làm trong lĩnh vực công nghệ và còn đang lưỡng lự — go for it. Cái mất nhiều nhất là vài đêm thiếu ngủ. Cái nhận được, theo trải nghiệm của mình, đáng hơn nhiều.

Bài tiếp theo bạn muốn mình chia sẻ về điều gì? Có thể là deep-dive vào kiến trúc của ScholarPath, hoặc cách team mình phân chia 36 tiếng để không miss deadline — reach out mình ở **congpho802@gmail.com**.
