# Cursor IDE Handbook

**Tài liệu toàn diện về Cursor IDE** - bao gồm Rules, Prompts, Debug templates, Best Practices cho nhiều ngôn ngữ và framework.

> Cursor = VS Code + AI layer. Tài liệu này giúp bạn khai thác tối đa sức mạnh của Cursor IDE trong mọi dự án phần mềm.

---

## Ai nên đọc?

- Developer muốn **dùng Cursor hiệu quả** (không chỉ bấm Tab)
- Team lead muốn **chuẩn hóa** cách team dùng AI coding
- Người mới chuyển từ VS Code / Copilot sang Cursor
- Developer làm việc với C, C++, C#, PHP, Python, JavaScript/TypeScript

---

## Cấu trúc dự án

```
cursor-ide-handbook/
│
├── README.md                        ← Bạn đang ở đây
├── cursor-ide-handbook.md           ← Handbook chính (mục lục tổng, 22 chương)
├── prompts-cpp.md                   ← C++ prompts (Modern C++17/20/23)
│
├── c/                               ← Ngôn ngữ C
│   ├── README.md                       Core C (memory, pointers, build system)
│   └── embedded.md                     Lập trình nhúng (STM32, AVR, ESP32, RTOS)
│
├── csharp/                          ← Ngôn ngữ C#
│   ├── README.md                       Core C# / .NET 8 (async, EF Core, DI, CQRS)
│   └── aspnet.md                       ASP.NET Core (Web API, Minimal API, SignalR, Blazor)
│
├── php/                             ← Ngôn ngữ PHP
│   ├── README.md                       PHP thuần (OOP, PDO, Security, Design Patterns)
│   └── laravel.md                      Laravel (Eloquent, Sanctum, Jobs/Events/Queues)
│
├── python/                          ← Ngôn ngữ Python
│   ├── README.md                       Core Python (type hints, async, pytest)
│   ├── web.md                          Flask + FastAPI + Django
│   ├── data.md                         NumPy + Pandas + Matplotlib/Seaborn/Plotly
│   └── ai.md                          RAG + LLM + Computer Vision + ML/DL
│
└── javascript/                      ← JavaScript / TypeScript
    ├── README.md                       JS/TS Core + Node.js
    ├── react.md                        React 19 (hooks, state, TanStack Query)
    ├── nextjs.md                       Next.js 15 (App Router, Server Components)
    ├── nestjs.md                       NestJS (Modules, Guards, CQRS, BullMQ)
    ├── angular.md                      Angular 17+ (Signals, Standalone, NgRx)
    └── vue.md                          Vue 3 (Composition API, Pinia, Nuxt 3)
```

---

## Nội dung chính

### Handbook ([cursor-ide-handbook.md](./cursor-ide-handbook.md))

| Chương | Nội dung |
|--------|---------|
| 0-1 | Cursor IDE là gì, Cài đặt & Setup |
| 2 | Core Skills (Tab, Inline Edit, Chat, Composer, Agent) |
| 3 | Context System (`@file`, `@codebase`, `@terminal`, `@image`, `@git`...) |
| 4-5 | Composer & Agent Mode |
| 6 | Rules System (`.cursorrules`, `.cursor/rules/`) |
| 7 | Model Selection (Claude, GPT-4o, Gemini) |
| 8 | Shortcuts & Commands |
| 9 | Notepads |
| 10 | Prompt Library (bug fix, refactor, test, security, performance) |
| 11 | Prompt theo ngôn ngữ (link tới các thư mục con) |
| 12-14 | Workflow, Debug, Terminal AI |
| 15-16 | Extensions, Privacy & Security |
| 17-22 | Resources, Cheat Sheet, FAQ, Team Checklist |

### Prompt files theo ngôn ngữ

Mỗi file ngôn ngữ đều chứa:

- **Rules template** (`.cursorrules` + `.cursor/rules/*.mdc`)
- **Prompt Library** (tạo module, refactor, CRUD, API, v.v.)
- **Debug Prompts** (lỗi phổ biến + cách fix)
- **Testing Prompts** (unit test, integration, E2E)
- **Best Practices & Anti-patterns** (DO / DON'T)
- **Code ví dụ thực tế**

---

## Cách sử dụng

### 1. Đọc Handbook trước

Bắt đầu với [cursor-ide-handbook.md](./cursor-ide-handbook.md) để hiểu tổng quan về Cursor IDE.

### 2. Chọn ngôn ngữ bạn dùng

Mở thư mục ngôn ngữ tương ứng, copy rules vào `.cursorrules` hoặc `.cursor/rules/` của dự án.

### 3. Copy prompt khi cần

Khi cần tạo feature, fix bug, viết test... mở file prompt tương ứng, copy template và điền thông tin cụ thể.

### Ví dụ nhanh

**Bước 1**: Tạo file `.cursorrules` ở root dự án, copy rules từ file ngôn ngữ tương ứng.

**Bước 2**: Mở Cursor Chat (`Ctrl+L`) hoặc Composer (`Ctrl+I`), paste prompt:

```
@codebase

Create a REST API controller for User with CRUD endpoints.
Follow existing project structure.
List all files changed.
```

**Bước 3**: Review diff, accept, chạy test.

---

## Ngôn ngữ & Framework được hỗ trợ

| Ngôn ngữ | Frameworks / Chủ đề |
|----------|---------------------|
| **C** | Core C, Lập trình nhúng (STM32, AVR, ESP32, FreeRTOS, Zephyr), Drivers, CAN/Modbus/MQTT |
| **C++** | Modern C++17/20/23, RAII, Smart Pointers, Templates, Concepts, Ranges, Coroutines |
| **C#** | .NET 8, EF Core, Clean Architecture, DDD, ASP.NET Core, Minimal API, SignalR, Blazor |
| **PHP** | PHP 8.3+, PSR-12, OOP, PDO, Laravel (Eloquent, Sanctum, Jobs, Events, Policies) |
| **Python** | Flask, FastAPI, Django, NumPy, Pandas, Matplotlib, RAG, LLM, Computer Vision, PyTorch |
| **JavaScript** | TypeScript, Node.js, React, Next.js, NestJS, Angular, Vue.js, Nuxt 3 |

---

## Yêu cầu

- [Cursor IDE](https://cursor.com) (phiên bản mới nhất)
- Kiến thức cơ bản về ngôn ngữ lập trình bạn sử dụng
- (Khuyến nghị) Tài khoản Cursor Pro để truy cập đầy đủ các AI models

---

## Đóng góp

Nếu bạn muốn đóng góp thêm prompts, rules, hoặc sửa lỗi:

1. Fork repo
2. Tạo branch mới (`git checkout -b feature/ten-feature`)
3. Thêm/sửa nội dung
4. Tạo Pull Request

**Quy tắc đóng góp:**
- Giữ format thống nhất với các file hiện có
- Mỗi prompt phải có: mô tả mục đích + requirements rõ ràng
- Test prompt trên Cursor trước khi submit
- Viết bằng tiếng Việt (nội dung giải thích) + tiếng Anh (prompts/rules)

---

## License

Tài liệu này được chia sẻ tự do cho mục đích học tập và sử dụng nội bộ.

---

> Được tạo bằng Cursor IDE.
