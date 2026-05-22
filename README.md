# portfolio-content

Content source cho [doancongpho.dev](https://doancongpho.dev). Repo này chứa nội dung — bài viết, profile, dự án — được fetch trực tiếp từ frontend qua GitHub Raw API và Contents API.

Mỗi lần push lên `main`, frontend sẽ revalidate (TTL 3600s) — không cần redeploy.

## Cấu trúc

```
portfolio-content/
├── profile/
│   └── about.json          # name, bio, skills, social links
├── posts/
│   ├── 2026-04-12-foo.md   # frontmatter + markdown body
│   └── ...
└── projects/
    └── projects.json       # mảng các dự án
```

## Schema

### `profile/about.json`

```json
{
  "name": "Đoàn Công Pho",
  "role": "Software Engineer · AI Agent Systems · Saigon",
  "bio": "Một câu hoặc đoạn ngắn — sẽ hiển thị ở hero.",
  "skills": ["Python", "LangGraph", "FAISS"],
  "links": {
    "github": "https://github.com/...",
    "linkedin": "https://...",
    "email": "you@example.com",
    "leetcode": "https://..."
  }
}
```

### `posts/<slug>.md`

```markdown
---
title: "Tiêu đề bài viết"
date: "2026-04-12"
tags: ["rag", "agents"]
summary: "Một câu mô tả — hiển thị ở list bài viết."
---

Body markdown ở đây. Hỗ trợ GFM (tables, task lists) và code blocks với syntax highlighting.
```

File name = slug. Ví dụ `2026-04-12-director-supervisor.md` → URL `/posts/2026-04-12-director-supervisor`.

### `projects/projects.json`

```json
[
  {
    "title": "Tên dự án",
    "description": "Mô tả ngắn — đoạn này hiển thị dưới tiêu đề.",
    "tags": ["LangGraph", "FAISS"],
    "period": "2026-04 → present",
    "url": "https://demo.example.com",
    "repo": "https://github.com/..."
  }
]
```

Chỉ `title`, `description`, và `tags` là bắt buộc. `period`, `url`, `repo` đều optional.

## Setup

1. Tạo repo `DoanCongPho/portfolio-content` (public) trên GitHub.
2. Copy nội dung từ folder `content-example/` của repo frontend vào root của repo này.
3. Push lên `main`.
4. Frontend sẽ tự fetch — TTL 3600s.

Để force revalidate ngay, dùng [On-Demand Revalidation](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#on-demand-revalidation) (chưa wired — sẽ thêm sau nếu cần).
