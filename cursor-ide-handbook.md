# 📘 CURSOR IDE HANDBOOK  
**Full Skills • Rules • Prompts • Best Practices**  
*(Tài liệu nội bộ – cập nhật dựa trên awesome-cursorrules & kinh nghiệm thực tế)*

> Cursor = VS Code + AI layer siêu mạnh  
> Mục tiêu: Giúp team code nhanh hơn, sạch hơn, ít bug hơn với Cursor.

---

## 📌 Mục lục

- [0. Cursor IDE là gì?](#0-cursor-ide-là-gì)
- [1. Cài đặt & Setup ban đầu](#1-cài-đặt--setup-ban-đầu)
- [2. Core Skills – Kỹ năng cốt lõi](#2-core-skills--kỹ-năng-cốt-lõi)
- [3. Context System – Bí quyết làm Cursor mạnh hơn ChatGPT](#3-context-system--bí-quyết-làm-cursor-mạnh-hơn-chatgpt)
- [4. Composer – Xây dựng feature end-to-end](#4-composer--xây-dựng-feature-end-to-end)
- [5. Agent Mode – AI tự làm việc như senior dev](#5-agent-mode--ai-tự-làm-việc-như-senior-dev)
- [6. Rules – Phần quan trọng nhất](#6-rules--phần-quan-trọng-nhất)
  - [6.1 Rules là gì & Tại sao cần?](#61-rules-là-gì--tại-sao-cần)
  - [6.2 Cấu trúc Rules tốt](#62-cấu-trúc-rules-tốt)
  - [6.3 Rules Universal (dùng ngay)](#63-rules-universal-dùng-ngay)
  - [6.4 Rules theo ngôn ngữ & framework](#64-rules-theo-ngôn-ngữ--framework)
  - [6.5 Thư mục .cursor/rules/ (cách mới)](#65-thư-mục-cursorrules-cách-mới)
  - [6.6 Quy tắc Prompt & Output Format chuẩn team](#66-quy-tắc-prompt--output-format-chuẩn-team)
  - [6.7 Anti-patterns khi viết Rules](#67-anti-patterns-khi-viết-rules)
- [7. Model Selection – Chọn AI model phù hợp](#7-model-selection--chọn-ai-model-phù-hợp)
- [8. Commands & Shortcuts quan trọng](#8-commands--shortcuts-quan-trọng)
- [9. Notepads – Ghi chú context dùng lại](#9-notepads--ghi-chú-context-dùng-lại)
- [10. Prompt Library – Bộ prompt chuẩn](#10-prompt-library--bộ-prompt-chuẩn)
- [11. Prompt theo ngôn ngữ (file riêng)](#11-prompt-theo-ngôn-ngữ-file-riêng)
- [12. Workflow chuẩn khi làm feature lớn](#12-workflow-chuẩn-khi-làm-feature-lớn)
- [13. Debug hiệu quả trong Cursor](#13-debug-hiệu-quả-trong-cursor)
- [14. Terminal AI Integration](#14-terminal-ai-integration)
- [15. Extensions nên cài](#15-extensions-nên-cài)
- [16. Privacy & Security](#16-privacy--security)
- [17. Nguồn tài nguyên Rules & Prompts tốt nhất](#17-nguồn-tài-nguyên-rules--prompts-tốt-nhất)
- [18. Cheat Sheet – Tóm tắt để master Cursor](#18-cheat-sheet--tóm-tắt-để-master-cursor)
- [19. Daily Prompt Pack – Dùng hàng ngày](#19-daily-prompt-pack--dùng-hàng-ngày)
- [20. FAQ – Câu hỏi thường gặp](#20-faq--câu-hỏi-thường-gặp)
- [21. Checklist triển khai Cursor cho team](#21-checklist-triển-khai-cursor-cho-team)
- [22. Kết luận & Mẹo pro](#22-kết-luận--mẹo-pro)

---

## 0. Cursor IDE là gì?

Cursor là **VS Code fork** được tích hợp **AI layer sâu** (có thể dùng các model như Claude, GPT, Gemini, v.v.).

### ✅ Giống VS Code ~95%

- Giao diện, phím tắt, extensions
- Terminal, debugger, git, explorer, search...
- Cài được extensions từ VS Code Marketplace

### 🔥 Khác biệt lớn (điểm mạnh của Cursor)

- **Composer** (tạo/sửa nhiều file cùng lúc)
- **Agent Mode** (AI tự lập kế hoạch & thực thi)
- **Context system** (`@file`, `@folder`, `@codebase`, `@web`, `@git`, `@image`...)
- **Rules system** (`.cursorrules` hoặc `.cursor/rules/`)
- **Notepads** (ghi chú context dùng lại được)
- **Inline Edit** (Ctrl+K / Cmd+K)
- **Tab AI Autocomplete** (thông minh hơn Copilot ở nhiều case)
- **Chat sidebar** tích hợp context codebase
- **Terminal AI** (Ctrl+K trong terminal để AI viết/sửa command)
- **Multi-model support** (Claude, GPT-4, Gemini, v.v. – chuyển đổi linh hoạt)

---

## 1. Cài đặt & Setup ban đầu

### 1.1 Import settings từ VS Code (khuyến nghị)

1. Tải & cài Cursor: https://cursor.com  
2. Mở Cursor lần đầu → chọn **Import from VS Code**
3. Cursor sẽ copy:
   - `settings.json`
   - keybindings
   - extensions (những cái tương thích)

> 📌 Nếu không thấy option import: vào Settings và tìm `Import`.

---

### 1.2 Nếu extension không import được

- Mở tab **Extensions**
- Search và cài lại thủ công (giống hệt VS Code)

> ⚠️ Một số extension phụ thuộc vào VS Code core API có thể hoạt động không ổn định, nhưng đa số dùng được.

---

### 1.3 Setup khuyến nghị (Best Defaults)

| Setting | Recommended |
|--------|------------|
| Theme | One Dark Pro / Dracula / GitHub Dark |
| Font | JetBrains Mono / Fira Code / Cascadia Code |
| Format on Save | ✅ ON |
| ESLint + Prettier | ✅ ON (nếu dùng JS/TS) |
| TypeScript strict | ✅ ON (nếu dùng TS) |
| Auto Save | ON nếu team thích workflow nhanh |
| Bracket Pair Colorization | ✅ ON |
| Minimap | OFF (tiết kiệm không gian) |
| Word Wrap | ON cho markdown, OFF cho code |

### 1.4 Cấu hình AI Model

Vào **Settings → Models** để:
- Chọn model mặc định (Claude, GPT-4, Gemini...)
- Thêm API key nếu dùng model riêng
- Chọn model khác nhau cho Chat vs Composer vs Tab

---

## 2. Core Skills – Kỹ năng cốt lõi

| Kỹ năng | Khi nào dùng | Phím tắt (default) | Mẹo hay |
|--------|--------------|--------------------|--------|
| **Tab Autocomplete** | Viết code mới, boilerplate, props, types | `Tab` | Viết comment chi tiết trước → AI đoán chuẩn hơn |
| **Inline Edit** | Sửa nhanh, refactor, convert JS→TS | `Ctrl+K` / `Cmd+K` | Chọn vùng code → prompt → accept |
| **Chat Sidebar** | Hỏi logic, debug, giải thích code | `Ctrl+L` / `Cmd+L` | Luôn dùng `@file` hoặc `@codebase` |
| **Quick Question** | Hỏi nhanh về đoạn code đang chọn | `Ctrl+Enter` | Highlight code rồi hỏi |
| **Composer** | Feature lớn, sửa nhiều file | `Ctrl+I` / `Cmd+I` | Dùng `@codebase` + prompt rõ |
| **Agent Mode** | Task phức tạp, refactor lớn | Toggle Agent trong Composer | Yêu cầu AI plan trước |
| **Terminal AI** | Viết command phức tạp, fix lỗi terminal | `Ctrl+K` trong terminal | Mô tả việc cần làm bằng tiếng Anh |

### 2.1 Tab Autocomplete nâng cao

- **Viết comment trước** → Tab sẽ generate code theo comment
- **Viết function signature** → Tab tự hoàn thành body
- **Partial accept**: nhấn `Ctrl+→` để accept từng từ thay vì cả suggestion
- **Reject**: nhấn `Esc` để từ chối suggestion

### 2.2 Inline Edit (Ctrl+K) nâng cao

```text
# Ví dụ: chọn 1 function → Ctrl+K → gõ:
"Convert to async/await, add proper error handling with try/catch"

# Ví dụ: chọn CSS → Ctrl+K → gõ:
"Convert to Tailwind classes"

# Ví dụ: chọn C code → Ctrl+K → gõ:
"Add null checks for all pointer parameters"
```

---

## 3. Context System – Bí quyết làm Cursor mạnh hơn ChatGPT

Cursor mạnh nhờ **context chính xác**.

### 3.1 Các loại context quan trọng

| Context | Khi nào dùng | Ví dụ | Độ lớn task |
|--------|--------------|------|------------|
| `@file` | Task tập trung 1 file | `@src/components/Button.tsx` | Nhỏ |
| `@folder` | Task liên quan 1 thư mục | `@src/components/ui` | Vừa |
| `@codebase` | Task lớn, bug khó, cần hiểu dự án | `@codebase` | Lớn |
| `@web` | Tra cứu tài liệu / API mới | `@web nextjs.org/docs` | Bổ sung |
| `@git` | Xem thay đổi gần đây, diff, commit history | `@git` | Bổ sung |
| `@definitions` | Xem định nghĩa symbols trong code | `@definitions myFunction` | Nhỏ |
| `@docs` | Tham khảo documentation đã index | `@docs React` | Bổ sung |
| `@image` | Truyền ảnh (UI mockup, screenshot lỗi) | Kéo thả ảnh vào chat | Bổ sung |
| `@terminal` | Lấy output terminal gần nhất | `@terminal` | Bổ sung |
| `@symbols` | Tìm symbol/class/function trong project | `@symbols UserService` | Nhỏ |
| `@notepad` | Dùng notepad đã lưu làm context | `@notepad project-spec` | Bổ sung |

### 3.2 Quy tắc vàng
- Task nhỏ → chỉ `@file`
- Task vừa → `@folder` + 1-2 `@file`
- Task lớn → `@codebase` + mô tả rõ feature/bug
- Debug → `@terminal` + `@file` (paste lỗi + file liên quan)
- Review code → `@git` + `@file`

### 3.3 Kết hợp context nâng cao

```text
# Ví dụ: Fix bug dựa trên screenshot + terminal error
@terminal
@image [kéo thả screenshot lỗi]
@file src/controllers/UserController.cs

Please analyze the error shown in terminal and screenshot,
then fix it in the specified file.
```

```text
# Ví dụ: Implement feature dựa trên UI mockup
@image [kéo thả mockup]
@folder src/components/
@docs TailwindCSS

Build this UI exactly as shown in the mockup using existing components.
```

```text
# Ví dụ: Review thay đổi gần nhất
@git
@codebase

Review the latest changes. Check for:
- potential bugs
- missing error handling
- convention violations
```

> ⚠️ Đừng dùng `@codebase` cho task nhỏ → tốn token, chậm, đôi khi AI "sửa quá tay".

---

## 4. Composer – Xây dựng feature end-to-end

Composer là chế độ mạnh nhất của Cursor.

### 4.1 Khi nào nên dùng Composer?
- Tạo module CRUD
- Authentication flow
- Dashboard/admin panel
- Thêm API endpoints + validation
- Refactor cấu trúc project

### 4.2 Prompt Composer template chuẩn

```text
@codebase

Goal: Implement [tên feature]

Requirements:
- [UI requirements]
- [backend/logic requirements]
- [validation + error handling]
- [loading states + toast notifications]

Constraints:
- Follow existing project structure & naming convention
- Do NOT add new external libraries
- Use existing UI components (shadcn/ui, MUI, Tailwind...)
- Write clean, modular, typed code

Output format:
1. List all files to be created/modified
2. Show code changes with file path
3. Brief explanation of each change
```

### 4.3 Composer Best Practices
- Luôn yêu cầu **liệt kê file thay đổi**
- Nếu task lớn: bảo AI **split theo phases**
- Nếu AI định sửa quá nhiều file: bảo "minimize changes"
- Dùng **"Apply All"** chỉ khi đã review kỹ từng diff
- Nếu sai: dùng **Undo** (Ctrl+Z) để rollback

---

## 5. Agent Mode – AI tự làm việc như senior dev

Agent Mode = Composer + khả năng tự lập kế hoạch + tự sửa nhiều file + chạy terminal commands.

### 5.1 Khi nào dùng Agent Mode?
- Refactor architecture
- Migration library/framework
- Fix bug liên quan nhiều module
- Thêm feature lớn có nhiều bước
- Setup project từ đầu

### 5.2 Prompt Agent Mode tốt

```text
@codebase

You are a senior full-stack engineer with 10+ years experience.

Task: [mô tả task chi tiết]

Instructions:
- Think step-by-step before making any changes
- Analyze current codebase structure first
- Only modify/create necessary files
- Follow existing architecture & patterns
- Do NOT introduce new dependencies
- Handle errors & edge cases properly

After finishing:
- List all changed/created files
- Explain the reasoning behind major decisions
```

### 5.3 Agent Mode cho các ngôn ngữ khác nhau

```text
# C/C++ project
@codebase
You are a senior C/C++ systems engineer.
Task: [mô tả]
- Use CMake/Makefile as build system
- Follow existing memory management patterns
- Check all return values, handle errors
- No memory leaks – free all allocated memory
- Run build to verify compilation
```

```text
# C# / .NET project
@codebase
You are a senior .NET engineer.
Task: [mô tả]
- Follow existing solution/project structure
- Use dependency injection patterns
- Implement proper IDisposable where needed
- Add XML doc comments
- Run dotnet build to verify
```

```text
# PHP project
@codebase
You are a senior PHP developer.
Task: [mô tả]
- Follow PSR-12 coding standards
- Use type declarations (PHP 8+)
- Follow existing framework patterns (Laravel/Symfony)
- Validate all user input
- Run composer test to verify
```

```text
# Python project
@codebase
You are a senior Python engineer.
Task: [mô tả]
- Follow PEP 8 and existing project style
- Add type hints to all functions
- Use virtual environment packages only
- Write docstrings for public functions
- Run pytest to verify
```

### 5.4 Cảnh báo khi dùng Agent
- Agent có thể "tự tin sửa sai" → phải review diff kỹ
- Không nên dùng Agent cho task nhỏ
- Agent có thể chạy terminal commands → giám sát output
- Nếu Agent "looping" (lặp đi lặp lại sửa/lỗi): dừng lại, cho context rõ hơn

---

## 6. Rules – Phần quan trọng nhất

### 6.1 Rules là gì & Tại sao cần?

Rules là luật cố định áp dụng cho mọi tương tác AI trong Cursor.

**Không có Rules → hậu quả:**
- Style code không thống nhất
- AI tự ý thêm thư viện mới
- Dùng `any` trong TypeScript, `void*` trong C, `dynamic` trong C#
- Viết code dài, khó maintain
- Không theo convention dự án
- Không handle error / memory đúng cách

---

### 6.2 Cấu trúc Rules tốt

Một file `.cursorrules` tốt thường có 5 nhóm:

1. **General** – Quy tắc chung
2. **Language-specific** – theo ngôn ngữ
3. **Framework-specific** – React, Next.js, FastAPI, Laravel, .NET...
4. **Error handling & Security**
5. **Output format** – format trả lời

---

### 6.3 Rules Universal (dùng ngay)

Tạo file `.cursorrules` ở root dự án:

```txt
You are an expert software engineer.

# General
- Strictly follow existing project structure, naming conventions and coding style.
- Write clean, readable, maintainable code.
- Prefer simplicity over cleverness.
- Do NOT introduce new dependencies unless explicitly asked.
- Keep changes minimal and focused.
- Always handle errors and edge cases.

# TypeScript
- No 'any' type - always use explicit types.
- Prefer interface over type when possible.
- Use async/await instead of .then/.catch chains.

# C / C++
- Always check return values and handle errors.
- Free all dynamically allocated memory – no memory leaks.
- Use const where possible.
- Prefer stack allocation over heap when feasible.

# C#
- Use async/await for I/O operations.
- Implement IDisposable for unmanaged resources.
- Use nullable reference types.
- Follow .NET naming conventions (PascalCase for public, _camelCase for private).

# PHP
- Follow PSR-12 coding standards.
- Use strict types: declare(strict_types=1).
- Use type declarations for parameters and return types.
- Validate and sanitize all user input.

# Python
- Follow PEP 8 style guide.
- Add type hints to all function signatures.
- Use docstrings for public functions and classes.
- Prefer f-strings over .format() or % formatting.

# React / Next.js
- Use functional components + hooks only.
- Keep components small and composable.
- Use Tailwind CSS or shadcn/ui for styling if the project already uses them.

# Backend / API
- Always validate input data.
- Handle errors properly with meaningful messages.
- Return correct HTTP status codes.
- Log errors with context information.

# Output Format
- Always start with: "Files changed/created:"
- List each file path clearly.
- Explain briefly what was changed and why.
- Use code blocks with correct language identifiers.
```

---

### 6.4 Rules theo ngôn ngữ & framework (từ awesome-cursorrules)

> 📌 Repo tham khảo phổ biến nhất:  
> https://github.com/PatrickJS/awesome-cursorrules

| Ngôn ngữ / Framework | Rules nổi bật | Gợi ý file name |
|----------------------|--------------|----------------|
| C | memory safety, null checks, MISRA | `c-best-practices.cursorrules` |
| C++ | RAII, smart pointers, modern C++ | `cpp-modern.cursorrules` |
| C# / .NET | async/await, DI, nullable refs | `csharp-dotnet.cursorrules` |
| PHP / Laravel | PSR-12, strict_types, Eloquent | `php-laravel.cursorrules` |
| Python | type hints, PEP8, pytest | `python-best-practices.cursorrules` |
| TypeScript + Next.js | strict typing, no any, server/client rules | `nextjs-typescript.cursorrules` |
| FastAPI | Pydantic validation, routers/services | `python-fastapi.cursorrules` |
| Django | ORM best practices, migrations | `python-django.cursorrules` |
| Go | explicit error handling, stdlib first | `go-backend.cursorrules` |
| Java Spring Boot | layered architecture, DTO validation | `java-spring.cursorrules` |
| Laravel | Eloquent, FormRequest validation | `laravel.cursorrules` |
| Rust | Result/Option, avoid unsafe | `rust.cursorrules` |
| Solidity | gas optimization, reentrancy guard | `solidity.cursorrules` |

---

### 6.5 Thư mục .cursor/rules/ (cách mới)

Cursor hỗ trợ thư mục `.cursor/rules/` cho phép **tách rules thành nhiều file** thay vì dồn vào 1 file `.cursorrules`.

#### Cấu trúc thư mục

```
project-root/
├── .cursor/
│   └── rules/
│       ├── general.mdc          # Quy tắc chung
│       ├── c-cpp.mdc            # Rules cho C/C++
│       ├── csharp.mdc           # Rules cho C#
│       ├── php.mdc              # Rules cho PHP
│       ├── python.mdc           # Rules cho Python
│       ├── typescript.mdc       # Rules cho TypeScript
│       ├── security.mdc         # Security rules
│       └── output-format.mdc    # Output format rules
├── .cursorrules                 # (legacy – vẫn hoạt động)
└── src/
```

#### Ưu điểm cách mới
- **Modular**: dễ quản lý khi project lớn
- **Conditional**: có thể set rule chỉ apply cho file type nhất định
- **Team-friendly**: dễ review/merge trên git
- **Override**: file-specific rules override general rules

#### Ví dụ file `.cursor/rules/general.mdc`

```
---
description: General coding rules for all files
globs: *
---

- Write clean, readable code
- Follow existing project conventions
- Keep changes minimal
- Always handle errors
- No new dependencies unless asked
```

#### Ví dụ file `.cursor/rules/c-cpp.mdc`

```
---
description: Rules for C and C++ files
globs: *.c, *.cpp, *.h, *.hpp
---

- Check all pointer parameters for NULL
- Free all dynamically allocated memory
- Use const correctness
- Prefer stack over heap allocation
- Follow existing naming convention (snake_case or camelCase)
- Include proper header guards (#pragma once or #ifndef)
```

---

### 6.6 Quy tắc Prompt & Output Format chuẩn team

Để AI trả output "dễ review", nên enforce thêm:

```txt
# Team Output Convention
- Always list files modified/created at the top.
- Provide code in file-separated blocks.
- If unsure, ask a clarifying question instead of guessing.
- Never rewrite unrelated parts of the codebase.
- Use the same language as the existing codebase (don't mix tabs/spaces, naming styles).
```

---

### 6.7 Anti-patterns khi viết Rules

❌ Rules quá dài, quá chung chung  
❌ Rules mâu thuẫn nhau (ví dụ vừa "no dependencies" vừa "use any library")  
❌ Rules kiểu "Always do everything perfectly" (AI sẽ ignore)  
❌ Rules không nhắc đến output format → output loạn  
❌ Copy rules từ ngôn ngữ khác mà không điều chỉnh (vd: TypeScript rules cho C project)  
❌ Rules quá strict gây AI "bó tay" không generate được code

---

## 7. Model Selection – Chọn AI model phù hợp

### 7.1 Các model phổ biến trong Cursor

| Model | Điểm mạnh | Điểm yếu | Dùng khi |
|-------|-----------|-----------|----------|
| **Claude 4 Sonnet** | Code chất lượng cao, hiểu context tốt, ít hallucinate | Chậm hơn GPT-4o-mini | Task phức tạp, refactor lớn, review code |
| **Claude 4 Opus** | Mạnh nhất cho reasoning phức tạp | Chậm, tốn token | Architecture decisions, debug cực khó |
| **GPT-4o** | Nhanh, đa năng, context window lớn | Đôi khi verbose | Task vừa, coding chung |
| **GPT-4o-mini** | Rất nhanh, rẻ | Kém hơn ở task phức tạp | Task nhỏ, autocomplete, câu hỏi nhanh |
| **Gemini 2.5 Pro** | Context window cực lớn (1M tokens) | Chưa ổn định bằng Claude/GPT | Cần đọc/xử lý file rất lớn |
| **cursor-small** | Nhanh nhất, built-in | Yếu nhất | Tab completion, edit nhỏ |

### 7.2 Chiến lược chọn model

```
Tab Autocomplete  → cursor-small hoặc GPT-4o-mini (nhanh)
Chat hỏi nhanh    → GPT-4o-mini hoặc Claude Sonnet
Inline Edit       → Claude Sonnet hoặc GPT-4o
Composer          → Claude Sonnet hoặc Claude Opus
Agent Mode        → Claude Sonnet (cân bằng tốc độ/chất lượng)
Debug phức tạp    → Claude Opus
```

### 7.3 Chuyển model giữa chừng

- Trong Chat/Composer: click tên model ở góc → đổi model
- Nếu output không tốt: đổi sang model mạnh hơn + prompt lại
- Nếu task đơn giản: dùng model nhẹ để tiết kiệm token

---

## 8. Commands & Shortcuts quan trọng

### 8.1 Phím tắt phổ biến

| Action | Windows/Linux | macOS |
|--------|--------------|-------|
| Inline Edit | `Ctrl+K` | `Cmd+K` |
| Chat Sidebar | `Ctrl+L` | `Cmd+L` |
| Composer | `Ctrl+I` | `Cmd+I` |
| Quick Question | `Ctrl+Enter` | `Cmd+Enter` |
| Accept suggestion | `Tab` | `Tab` |
| Partial accept (1 từ) | `Ctrl+→` | `Cmd+→` |
| Toggle chat | `Ctrl+Shift+L` | `Cmd+Shift+L` |
| Terminal AI | `Ctrl+K` (in terminal) | `Cmd+K` (in terminal) |
| New chat | `Ctrl+Shift+N` | `Cmd+Shift+N` |
| Focus editor | `Escape` | `Escape` |

### 8.2 Command Palette (Ctrl+Shift+P)

Các lệnh hữu ích:
- `Cursor: New Chat` – Mở chat mới
- `Cursor: Toggle AI Panel` – Ẩn/hiện panel AI
- `Cursor: Open Settings` – Cài đặt Cursor
- `Cursor: Reset Conversation` – Reset context chat

> 📌 Shortcut có thể thay đổi tùy OS/settings.

---

## 9. Notepads – Ghi chú context dùng lại

Notepads là tính năng cho phép lưu **context snippets** để dùng lại nhiều lần.

### 9.1 Khi nào dùng Notepads?

- Lưu **project specification** (yêu cầu kỹ thuật)
- Lưu **API documentation** (endpoints, schema)
- Lưu **coding conventions** đặc thù team
- Lưu **prompt templates** hay dùng
- Lưu **database schema** để AI hiểu data model

### 9.2 Cách tạo & dùng Notepad

1. Mở panel Notepads (sidebar)
2. Tạo notepad mới, đặt tên có nghĩa (vd: `project-spec`, `api-schema`, `db-models`)
3. Viết nội dung vào notepad
4. Trong Chat/Composer, gõ `@notepad <tên>` để dùng

### 9.3 Ví dụ Notepad hữu ích

**Notepad: `project-spec`**
```text
Project: E-commerce platform
Tech stack: C# .NET 8 / Entity Framework Core / SQL Server
Architecture: Clean Architecture (Domain, Application, Infrastructure, API)
Auth: JWT Bearer tokens
Naming: PascalCase for public, _camelCase for private fields
```

**Notepad: `db-schema`**
```text
Users: Id (int PK), Email (nvarchar), PasswordHash, CreatedAt
Products: Id (int PK), Name, Price (decimal), CategoryId (FK)
Orders: Id (int PK), UserId (FK), TotalAmount, Status, CreatedAt
OrderItems: Id (int PK), OrderId (FK), ProductId (FK), Quantity, Price
```

> 📌 Notepad rất hữu ích khi làm việc với C/C++/C#/PHP/Python vì có thể lưu struct definitions, class diagrams, build instructions, v.v.

---

## 10. Prompt Library – Bộ prompt chuẩn

### 10.1 Fix Bug

```text
@codebase

Bug description:
[ mô tả lỗi ]

Expected behavior:
[ mong đợi ]

Actual behavior:
[ hiện tại ]

Steps to reproduce:
1. ...
2. ...

Please:
- Find root cause
- Fix the bug
- Add tests/logging if needed
- Do NOT break existing functionality
- List changed files
```

---

### 10.2 Refactor code

```text
Refactor the selected code (or @file) to be:
- More readable
- More maintainable
- Follow clean code principles

Keep exact same behavior.
Do NOT add new dependencies.
```

---

### 10.3 Tạo feature đầy đủ (Composer)

```text
@codebase

Goal: Build feature [name]

Requirements:
- UI:
- Backend:
- Validation:
- Loading + error states:

Constraints:
- Follow existing style
- No new dependencies
- Minimal changes

Output:
- List files changed
- Explain decisions
```

---

### 10.4 Generate unit tests

```text
@codebase

Write comprehensive unit tests for [file/module/function]
- Use existing test framework (vitest/jest/pytest/xunit/phpunit/gtest...)
- Cover happy path, edge cases, error cases
- Mock external dependencies if needed
```

---

### 10.5 Generate documentation

```text
@codebase

Generate documentation for this module:
- What it does
- How to use it
- Example usage
- Parameters / Return values
- Common pitfalls
```

---

### 10.6 Security review prompt

```text
@codebase

Review this code for security vulnerabilities.
Focus on:
- injection risks (SQL injection, command injection, XSS)
- auth/session issues
- unsafe deserialization
- missing validation
- buffer overflow risks (for C/C++)
- memory leaks (for C/C++/C#)

Suggest fixes with minimal changes.
```

---

### 10.7 Performance review prompt

```text
@codebase

Review this code for performance issues.
Focus on:
- unnecessary allocations / copies
- N+1 query problems
- missing caching opportunities
- inefficient algorithms (O(n²) that could be O(n log n))
- memory usage patterns

Suggest optimizations with benchmarking approach.
```

---

### 10.8 Code review prompt

```text
@git
@codebase

Review the recent changes for:
- Logic correctness
- Error handling completeness
- Security vulnerabilities
- Performance issues
- Convention violations
- Missing edge cases

Provide feedback as a list with severity (critical/warning/suggestion).
```

---

## 11. Prompt theo ngôn ngữ (thư mục riêng)

Mỗi ngôn ngữ/framework được tổ chức thành **thư mục riêng** với các file chuyên sâu:

### 11.1 C (thư mục [c/](./c/))

| File | Nội dung chính |
|------|---------------|
| [c/README.md](./c/README.md) | C core: memory management, pointer safety, Makefile/CMake, testing |
| [c/embedded.md](./c/embedded.md) | Lập trình nhúng: STM32, AVR, ESP32, RTOS, drivers, protocols (UART/SPI/I2C/CAN) |

### 11.2 C++ (file [prompts-cpp.md](./prompts-cpp.md))

> C++ vẫn giữ dạng file đơn vì tập trung vào modern C++ core.

| Nội dung | Mô tả |
|----------|-------|
| Modern C++ (17/20/23) | RAII, smart pointers, concepts, ranges, coroutines |
| Templates & Generic | CRTP, metaprogramming, compile-time |
| STL & Performance | Algorithms, containers, benchmarking |

### 11.3 C# (thư mục [csharp/](./csharp/))

| File | Nội dung chính |
|------|---------------|
| [csharp/README.md](./csharp/README.md) | C# core: async/await, EF Core, DI, CQRS, Clean Architecture, DDD |
| [csharp/aspnet.md](./csharp/aspnet.md) | ASP.NET Core: Web API, Minimal API, Auth (JWT/Identity), SignalR, Blazor |

### 11.4 PHP (thư mục [php/](./php/))

| File | Nội dung chính |
|------|---------------|
| [php/README.md](./php/README.md) | PHP thuần: OOP, PDO, Design Patterns, Security (SQL injection, XSS), PHPUnit |
| [php/laravel.md](./php/laravel.md) | Laravel: Eloquent, Controllers, Form Requests, Jobs/Events/Queues, Policies, Sanctum |

### 11.5 Python (thư mục [python/](./python/))

| File | Nội dung chính |
|------|---------------|
| [python/README.md](./python/README.md) | Python core: type hints, dataclass, async, CLI, testing (pytest), tooling |
| [python/web.md](./python/web.md) | Web frameworks: Flask, FastAPI, Django (DRF, Channels) |
| [python/data.md](./python/data.md) | Data Science: NumPy, Pandas, Matplotlib/Seaborn/Plotly, ETL pipelines |
| [python/ai.md](./python/ai.md) | AI/ML: RAG, LLM (OpenAI/Claude/Ollama), Computer Vision (YOLO/OpenCV), PyTorch, scikit-learn |

### 11.6 JavaScript / TypeScript (thư mục [javascript/](./javascript/))

| File | Nội dung chính |
|------|---------------|
| [javascript/README.md](./javascript/README.md) | JS/TS core: TypeScript strict, Node.js, ESLint, testing, Error handling |
| [javascript/react.md](./javascript/react.md) | React 19: hooks, state (Zustand/Redux), TanStack Query, React Hook Form |
| [javascript/nextjs.md](./javascript/nextjs.md) | Next.js 15: App Router, Server Components, Server Actions, SSR/SSG/ISR |
| [javascript/nestjs.md](./javascript/nestjs.md) | NestJS: Modules, Guards, Pipes, TypeORM/Prisma, CQRS, BullMQ |
| [javascript/angular.md](./javascript/angular.md) | Angular 17+: Standalone, Signals, RxJS, NgRx, Reactive Forms |
| [javascript/vue.md](./javascript/vue.md) | Vue 3: Composition API, Pinia, Nuxt 3, Composables |

### 11.7 Cấu trúc tổng thể

```
ebook_for_cursor/
├── cursor-ide-handbook.md       ← Handbook chính
├── prompts-cpp.md               ← C++ (file đơn)
│
├── c/                           ← C language
│   ├── README.md                   Core C
│   └── embedded.md                 Lập trình nhúng
│
├── csharp/                      ← C#
│   ├── README.md                   Core C# / .NET
│   └── aspnet.md                   ASP.NET Core
│
├── php/                         ← PHP
│   ├── README.md                   PHP thuần
│   └── laravel.md                  Laravel
│
├── python/                      ← Python
│   ├── README.md                   Core Python
│   ├── web.md                      Flask / FastAPI / Django
│   ├── data.md                     NumPy / Pandas / Matplotlib
│   └── ai.md                      RAG / LLM / Computer Vision
│
└── javascript/                  ← JavaScript / TypeScript
    ├── README.md                   JS/TS Core + Node.js
    ├── react.md                    React
    ├── nextjs.md                   Next.js
    ├── nestjs.md                   NestJS
    ├── angular.md                  Angular
    └── vue.md                      Vue.js
```

> 📌 Mỗi file chứa: Rules template (.cursorrules + .mdc), Prompt library, Debug prompts, Testing prompts, Best practices & Anti-patterns.

---

## 12. Workflow chuẩn khi làm feature lớn

### 12.1 Recommended flow

1. Tạo/cập nhật `.cursorrules` (hoặc `.cursor/rules/`)
2. Tạo Notepad với spec/requirements nếu phức tạp
3. Mở Composer (`Ctrl+I`)
4. Dùng prompt chi tiết + `@codebase` + `@notepad`
5. Review diff → accept từng file
6. Chuyển Agent Mode nếu cần multi-step
7. Inline Edit (`Ctrl+K`) để polish code
8. Chạy build/test/lint
9. Fix bug nhỏ bằng Chat hoặc Inline Edit
10. Commit & push

### 12.2 Review checklist khi accept changes
- Có phá naming convention không?
- Có thêm dependency lạ không?
- Có "rewrite" file không liên quan không?
- Có thêm `any` (TS), `void*` (C), `dynamic` (C#) không?
- Có handle error đầy đủ chưa?
- Có memory leak không? (C/C++)
- Có SQL injection / XSS risk không? (PHP/Python/C#)
- Code có compile/build thành công không?

---

## 13. Debug hiệu quả trong Cursor

### 13.1 Debug truyền thống (VS Code style)
- Breakpoints
- Watch variables
- Debug console
- Conditional breakpoints
- Call stack inspection

### 13.2 Debug bằng AI (nhanh hơn)

```text
@codebase

Error stacktrace:
[paste full error]

Related file:
@file src/utils/api.ts

Please:
- Identify root cause
- Suggest fix
- Show exact code change
```

### 13.3 Debug theo ngôn ngữ

```text
# C/C++ – Segfault / Memory error
@terminal
@file src/main.c

Segmentation fault khi chạy. Output từ Valgrind/AddressSanitizer:
[paste output]

Find the memory error and fix it.
```

```text
# C# – Exception
@terminal
@file Controllers/UserController.cs

Exception: NullReferenceException at line X.
Stack trace:
[paste stack trace]

Find root cause and add null checks.
```

```text
# PHP – Runtime error
@terminal
@file app/Http/Controllers/OrderController.php

Error: Call to a member function on null.
[paste full error with stack trace]

Fix the bug and add proper null checking.
```

```text
# Python – Traceback
@terminal
@file src/services/data_processor.py

Traceback:
[paste full traceback]

Find root cause, fix bug, add proper exception handling.
```

> 📌 Tips: luôn paste **full stacktrace** và **steps reproduce**.

---

## 14. Terminal AI Integration

### 14.1 AI trong Terminal

Nhấn `Ctrl+K` (hoặc `Cmd+K`) **trong terminal** để nhờ AI viết command.

```text
# Ví dụ: mô tả bằng tiếng Anh
"Find all .c files larger than 100KB modified in the last 7 days"
→ AI sẽ generate: find . -name "*.c" -size +100k -mtime -7

"Build the C++ project with debug symbols using CMake"
→ AI sẽ generate: cmake -DCMAKE_BUILD_TYPE=Debug .. && make -j$(nproc)

"Run PHP unit tests with coverage report"
→ AI sẽ generate: php artisan test --coverage
```

### 14.2 Fix lỗi terminal

Khi terminal hiện lỗi:
1. Chọn đoạn error trong terminal
2. Nhấn `Ctrl+L` (Chat) → AI phân tích lỗi
3. Hoặc dùng `@terminal` trong chat

### 14.3 Kết hợp Terminal + Chat

```text
@terminal

The build just failed. Analyze the error and tell me:
1. What went wrong
2. Which file(s) need to be fixed
3. The exact fix needed
```

---

## 15. Extensions nên cài

### 15.1 Must-have (mọi ngôn ngữ)
- **Error Lens** – Hiện lỗi inline trực tiếp trên code
- **GitLens** – Git blame, history chi tiết
- **Path Intellisense** – Auto-complete đường dẫn file
- **TODO Highlight** – Highlight TODO/FIXME/HACK comments
- **Bracket Pair Colorizer** (built-in) – Tô màu ngoặc

### 15.2 Cho C / C++
- **C/C++** (Microsoft) – IntelliSense, debugging, code browsing
- **CMake Tools** – Build, configure CMake projects
- **C/C++ Extension Pack** – Bộ extensions đầy đủ
- **CodeLLDB** – Debugger cho C/C++ (alternative)
- **Clang-Format** – Auto-format code

### 15.3 Cho C# / .NET
- **C# Dev Kit** (Microsoft) – Full .NET development
- **C#** (Microsoft) – IntelliSense, debugging
- **.NET Install Tool** – Quản lý .NET SDK
- **NuGet Gallery** – Quản lý packages
- **Entity Framework Tools** – EF Core migrations

### 15.4 Cho PHP
- **PHP Intelephense** – IntelliSense mạnh cho PHP
- **PHP Debug** – Xdebug integration
- **Laravel Extension Pack** – Nếu dùng Laravel
- **PHP CS Fixer** – Auto-format PSR-12
- **Composer** – Dependency management

### 15.5 Cho Python
- **Python** (Microsoft) – IntelliSense, debugging
- **Pylance** – Type checking nhanh
- **Python Debugger** – Debugging Python
- **Black Formatter** – Auto-format PEP 8
- **Ruff** – Linter cực nhanh cho Python

### 15.6 Cho Web/Full-stack
- **ESLint** – Linting JavaScript/TypeScript
- **Prettier** – Code formatter
- **Tailwind CSS IntelliSense** – Nếu dùng Tailwind
- **Thunder Client** / **REST Client** – Test API
- **Docker** – Container management
- **Prisma** – Nếu dùng Prisma ORM

---

## 16. Privacy & Security

### 16.1 Cursor Privacy Mode

Cursor có **Privacy Mode** (Settings → Privacy):
- **Enabled**: Code **KHÔNG** được gửi lên server để train model
- **Disabled**: Code có thể được dùng để cải thiện AI

> ⚠️ Với production code / code nhạy cảm: **BẬT Privacy Mode**.

### 16.2 Những gì được gửi lên server

| Dữ liệu | Khi nào gửi | Cách giảm thiểu |
|---------|-------------|----------------|
| Code snippet đang edit | Khi dùng Chat/Composer/Inline | Chỉ select đoạn code cần thiết |
| File content | Khi dùng `@file` | Không gửi file chứa secrets |
| Terminal output | Khi dùng `@terminal` | Cẩn thận với credentials trong terminal |
| Codebase index | Khi dùng `@codebase` | Dùng `.cursorignore` để exclude files |

### 16.3 File `.cursorignore`

Tạo file `.cursorignore` ở root project (cú pháp giống `.gitignore`):

```
# Không index các file nhạy cảm
.env
.env.*
*.pem
*.key
credentials.json
secrets/
config/production.yml

# Không index thư mục lớn không cần
node_modules/
vendor/
__pycache__/
bin/
obj/
```

### 16.4 Best practices bảo mật
- Không paste API keys / passwords vào Chat
- Dùng `.cursorignore` cho files nhạy cảm
- Bật Privacy Mode cho production repos
- Review AI output trước khi commit
- Không để AI tự chạy commands nếu chưa tin tưởng output

---

## 17. Nguồn tài nguyên Rules & Prompts tốt nhất

### 17.1 Rules
- https://github.com/PatrickJS/awesome-cursorrules
- https://cursor.directory (community rules marketplace)

### 17.2 Prompt collections (dùng được cho Cursor)
- Search GitHub: `cursor prompts`
- Search GitHub: `cursor rules`
- Awesome ChatGPT Prompts (adapt được cho Cursor)

### 17.3 Documentation chính thức
- https://docs.cursor.com – Cursor official docs
- https://cursor.com/changelog – Changelog & new features

### 17.4 Community workflow tips
- Reddit: r/cursor
- Discord: Cursor community
- YouTube: search "Cursor IDE tutorial"
- Twitter/X: #CursorIDE

---

## 18. Cheat Sheet – Tóm tắt để master Cursor

✅ Import settings & extensions từ VS Code  
✅ Setup linting + formatter phù hợp ngôn ngữ  
✅ Dùng đúng `@file` / `@folder` / `@codebase`  
✅ Kết hợp `@terminal` + `@image` khi debug  
✅ Tạo file `.cursorrules` hoặc `.cursor/rules/` ở root  
✅ Tạo Notepads cho project spec & schema  
✅ Composer cho feature mới  
✅ Agent Mode cho task lớn, giám sát chặt  
✅ Review diff kỹ trước khi accept  
✅ Chọn model phù hợp (Sonnet cho code, mini cho task nhỏ)  
✅ Debug bằng AI nhanh hơn debug thủ công  
✅ Bật Privacy Mode cho code nhạy cảm  
✅ Dùng `.cursorignore` để bảo vệ secrets  

---

## 19. Daily Prompt Pack – Dùng hàng ngày

### 19.1 Prompt: Feature nhỏ
```text
@codebase
Implement this small feature following existing style.
Keep changes minimal.
List all modified files.
```

### 19.2 Prompt: Fix bug nhanh
```text
@codebase
Fix this bug. Explain root cause clearly.
Do not introduce new dependencies.
```

### 19.3 Prompt: Refactor clean code
```text
Refactor this code for better readability and maintainability.
Keep exact same behavior.
```

### 19.4 Prompt: Generate tests
```text
@codebase
Write unit/integration tests for the selected code / @file.
Cover main cases + edge cases.
```

### 19.5 Prompt: Explain code
```text
@file [filepath]
Explain this code in detail:
- What it does step by step
- Why it's written this way
- Potential issues or improvements
```

### 19.6 Prompt: Convert / Migrate
```text
@file [filepath]
Convert this code from [language/framework A] to [language/framework B].
Keep the same logic and behavior.
Follow [target] best practices.
```

---

## 20. FAQ – Câu hỏi thường gặp

### Q1: Có cần học Cursor nếu biết VS Code?
**Không cần học lại IDE.**  
Chỉ cần học thêm: Rules + Context + Composer/Agent.

### Q2: Có nên dùng `@codebase` mọi lúc?
Không. Dùng khi task lớn. Task nhỏ chỉ cần `@file`.

### Q3: Cursor có thể thay dev không?
Không. Nhưng có thể thay **60–80% việc lặp lại** nếu dùng đúng.

### Q4: AI sửa code sai thì sao?
Review diff, reject phần sai. Luôn chạy test/build.

### Q5: Cursor có làm lộ code không?
Tùy setting privacy. Bật Privacy Mode + dùng `.cursorignore` cho code nhạy cảm.

### Q6: Dùng Cursor cho C/C++ có tốt không?
Rất tốt. AI hiểu C/C++ rất sâu: memory management, pointer arithmetic, templates, v.v. Cần setup rules đúng.

### Q7: Dùng model nào cho ngôn ngữ hệ thống (C/C++)?
Claude Sonnet hoặc Opus – hiểu tốt nhất về memory safety, undefined behavior, và system programming.

### Q8: Cursor có hỗ trợ PHP/Laravel không?
Có. Cài PHP Intelephense extension + viết rules cho PSR-12 / Laravel conventions.

### Q9: Python data science (pandas, numpy) dùng Cursor được không?
Được. Dùng `@file` cho notebook/script + rules về type hints và docstrings.

### Q10: `.cursorrules` hay `.cursor/rules/` – nên dùng cái nào?
- Project nhỏ: `.cursorrules` (1 file, đơn giản)
- Project lớn/nhiều ngôn ngữ: `.cursor/rules/` (modular, file-specific)
- Có thể dùng cả hai cùng lúc.

---

## 21. Checklist triển khai Cursor cho team

### 21.1 Chuẩn hóa project trước
- Linting config chuẩn (ESLint / clang-format / phpcs / ruff / dotnet-format)
- Formatter + format on save
- Husky + lint-staged / pre-commit hooks (optional)
- Build system rõ ràng (Makefile / CMake / dotnet build / composer / pip)

### 21.2 Chuẩn hóa Cursor
- Tạo `.cursorrules` hoặc `.cursor/rules/` chung
- Share prompt templates nội bộ
- Quy định output format
- Tạo Notepads chung cho project spec
- Thống nhất model mặc định cho team

### 21.3 Quy định review
- Không merge code AI chưa chạy test
- PR bắt buộc review
- Không accept "Accept All" khi Composer sửa nhiều file
- Chạy linter/formatter trước khi commit
- Check memory safety (C/C++) và security (PHP/Python/C#) trước khi merge

---

## 22. Kết luận & Mẹo pro

Cursor không chỉ là "Copilot tốt hơn".

Khi dùng đúng cách (**Rules + Context + Composer + Agent**), Cursor có thể:
- tăng tốc code feature
- giảm boilerplate
- refactor nhanh
- debug nhanh
- tạo tests/docs
- hỗ trợ mọi ngôn ngữ (C, C++, C#, PHP, Python, TS, Go, Rust, Java...)

### 🔥 Mẹo pro cuối cùng
- Prompt càng rõ → output càng chuẩn
- Review diff từng file
- Agent Mode dùng cho task lớn, giám sát chặt
- Update `.cursorrules` khi project thay đổi convention
- Dùng model mạnh (Claude Sonnet/Opus) cho task khó
- Tạo Notepads cho spec/schema để AI hiểu dự án sâu hơn
- Dùng `.cursorignore` để bảo vệ secrets
- Bật Privacy Mode cho production code
- Kết hợp `@terminal` + `@image` khi debug
- Viết comment chi tiết trước khi gõ Tab → AI autocomplete chính xác hơn

---

🚀 **Chúc bạn code nhanh & ít bug hơn với Cursor!**
