# Ti Ecosystem — Repository Map & Development Guide

> **Ngày cập nhật:** 2026-07-18
> **Mục đích:** Thống nhất danh sách repo, vai trò, URL chuẩn và quy trình phát triển hệ sinh thái Ti.
> **Account:** `thien123331` (working) + `LDoVietNam` (legacy/gateway).

---

## 1. Cấu trúc tổng quan

```
                         ┌─ TiBrain (brain) ─────────────┐
                         │  thien123331/tibrain (Go)      │
                         │  thien123331/CLIProxyAPI-RTK  │
                         └───────────────────────────────┘
                                    │
        ┌─────────────── Gateway / Router ───────────────┐
        │  LDoVietNam/tirouter-consolidated (CANONICAL)   │
        │    = Tirouter + Routerddd(Go) + router(TS)     │
        └────────────────────────────────────────────────┘
                                    │
   ┌──────────────────── Clients (3 nền tảng) ────────────────────┐
   │  LDoVietNam/Tiiextension (Chrome/Firefox MV3)                │
   │  LDoVietNam/Ti-CLI (Go CLI)                                  │
   │  LDoVietNam/Ti-Android (Kotlin)                             │
   └────────────────────────────────────────────────────────────┘

   Docs: LDoVietNam/docs (MDX)
```

---

## 2. Danh sách repo chuẩn (Canonical)

### 2.1 Account `thien123331` (working)
| Repo | URL | Vai trò | Trạng thái |
|---|---|---|---|
| **tibrain** | `https://github.com/thien123331/tibrain` | TiBrain core (Go): RAG, MCP Hub, cognitive memory, prompt intelligence | ✅ active, local sync |
| **CLIProxyAPI-RTK** | `https://github.com/thien123331/CLIProxyAPI-RTK` | RTK binary — giảm token CLI 60-90% | ✅ active |
| **Ti** | `https://github.com/thien123331/Ti` | stub rỗng (0KB) | ⚠️ empty, candidate xoá |

### 2.2 Account `LDoVietNam` (legacy/gateway)
| Repo | URL | Vai trò | Trạng thái |
|---|---|---|---|
| **tirouter-consolidated** | `https://github.com/LDoVietNam/tirouter-consolidated` | **Router gộp chuẩn** (Tirouter + Routerddd Go + router TS) | ✅ CANONICAL, đã push |
| **Tiiextension** | `https://github.com/LDoVietNam/Tiiextension` | Extension client MV3 (background runtime/session/task) | ⚠️ local commit, push block do secret |
| **Ti-CLI** | `https://github.com/LDoVietNam/Ti-CLI` | CLI client (Go) | ✅ active, đã push |
| **Ti-Android** | `https://github.com/LDoVietNam/Ti-Android` | Android client (Kotlin) | ✅ active, đã push |
| **docs** | `https://github.com/LDoVietNam/docs` | Tài liệu hệ sinh thái (MDX) | ✅ active |
| **ti-internal-cli** | `https://github.com/LDoVietNam/ti-internal-cli` | Internal CLI (57KB, có code) | ✅ kept |
| **tirouter-worker-runtime** | `https://github.com/LDoVietNam/tirouter-worker-runtime` | Worker runtime (48KB, có code) | ✅ kept |

### 2.3 Repo đã gộp / lỗi thời (không dùng trực tiếp)
| Repo | URL | Ghi chú |
|---|---|---|
| tirouter (cũ) | `https://github.com/LDoVietNam/tirouter` | Thay bằng `tirouter-consolidated`. Master nhiễm secret `.env` → push block. Cần archive/xoá. |
| Routerddd | `https://github.com/LDoVietNam/Routerddd` | Đã gộp vào `tirouter-consolidated`. |
| router | `https://github.com/LDoVietNam/router` | Đã gộp vào `tirouter-consolidated`. |
| tibrain (cũ) | `https://github.com/LDoVietNam/tibrain` | Stale 2026-06-22, đã **archived**. |
| OpenClaw-APK | `https://github.com/openclaw/openclaw` | Fork, KHÔNG thuộc Ti ecosystem. |

---

## 3. Vai trò chi tiết

### 3.1 TiBrain (`thien123331/tibrain`)
- HTTP server đơn tại port `1810`: REST + MCP(SSE) + Browser UI.
- Subsystems: RAGSystemManager (FTS5+vector), MCPHubClient, CognitiveMemory, PromptIntelligence, TiAgentOrchestrator.
- Driver: `modernc.org/sqlite` (pure-Go, no CGO). Single `*sql.DB` qua `internal/db`.
- Local: `Z:\01_PROJECTS\apps\tibrain` → remote `thien123331/tibrain`.

### 3.2 Router (`LDoVietNam/tirouter-consolidated`) — CANONICAL
- Gộp 3 router cũ: `Tirouter` (master scripts) + `Routerddd` (Go core: `apps/router/layers/*`) + `router` (TS/Electron frontend, branch `optimize/rtk-caveman`).
- Cấu trúc: `CLIProxyAPI/` (RTK Go), `Routerddd/` (Go), `router-ts/` (TS frontend), `claude-nim/`, `control-plane/`, `TirouterHooks/`.
- Local: `Z:\01_PROJECTS\apps\Tirouter` (branch `consolidate/routers` = commit `253decfd`, đã push lên `tirouter-consolidated`).

### 3.3 Clients
| Client | Ngôn ngữ | Local path | Remote |
|---|---|---|---|
| Tiiextension | TS/Vite MV3 | `Z:\01_PROJECTS\apps\Tiiextension` | `LDoVietNam/Tiiextension` (chưa push) |
| Ti-CLI | Go | `Z:\01_PROJECTS\apps\Ti-CLI` | `LDoVietNam/Ti-CLI` (đã push) |
| Ti-Android | Kotlin | `Z:\01_PROJECTS\apps\Ti-Android` | `LDoVietNam/Ti-Android` (đã push) |

---

## 4. Quy trình phát triển chuẩn

### 4.1 Clone / Setup
```powershell
# Brain
git clone https://github.com/thien123331/tibrain Z:\01_PROJECTS\apps\tibrain
# Router (canonical)
git clone https://github.com/LDoVietNam/tirouter-consolidated Z:\01_PROJECTS\apps\Tirouter
# Clients
git clone https://github.com/LDoVietNam/Tiiextension Z:\01_PROJECTS\apps\Tiiextension
git clone https://github.com/LDoVietNam/Ti-CLI        Z:\01_PROJECTS\apps\Ti-CLI
git clone https://github.com/LDoVietNam/Ti-Android    Z:\01_PROJECTS\apps\Ti-Android
```

### 4.2 Git auth (quan trọng)
- Dùng `gh` keyring (account `LDoVietNam` hoặc `thien123331` đã login).
- **KHÔNG set env `GH_TOKEN`/`GITHUB_TOKEN`** nếu token invalid — nó ghi đè lên `gh` keyring và gây lỗi push (`Invalid username or token`).
- Khi push bị lỗi auth: `Remove-Item -Force .git/index.lock` (nếu kẹt lock do process `codex`), rồi `git push` lại với env token đã unset.

### 4.3 Secret hygiene (BẮT BUỘC)
- Repo `LDoVietNam/tirouter` BẬT **Push Protection + Secret Scanning** → không push `.env`, `config.yaml`, `*.key`, `server.out`, OAuth secrets.
- Pattern bị block: `ghp_`, `github_pat_`, `sk-`, `AKIA`, `GOCSPX`, `Iv1.`, `-----BEGIN`, `client_secret`.
- Luôn có `.gitignore` loại `.env`, `node_modules`, `dist`, `build`, `*.log`, secrets.
- Nếu clone repo chứa secret (vd `router-ts/tests/unit/oauth-providers-config.test.ts`), scrub thành `REMOVED_FOR_SAFETY` trước khi commit.

### 4.4 Commit / Push
```powershell
git add .
git reset -q HEAD -- '.env' 'node_modules' 'dist' 'build' '*.log'   # exclude junk
git commit -m "feat: ..."
git push origin <branch>
```

---

## 5. Tồn đọng cần xử lý (backlog)

| # | Việc | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | Push `Tiiextension` | ⚠️ BLOCK | 141 secret-hits (dashboard/assets/*.js + RELEASE-MANIFEST.json). Cần scrub rồi push. |
| 2 | Archive/xoá `tirouter` cũ | ⏸ chưa | Đã có `tirouter-consolidated` thay thế. |
| 3 | Xoá `thien123331/Ti` + `LDoVietNam/LD` | ⏸ chưa | Repo rỗng. |
| 4 | Review orphan repos | ⏸ chưa | 186router, orca, sub2api, OmniRoute, filebrowser, goclaw... (chưa gộp vào Ti). |

---

## 6. Liên kết nhanh
- Brain: `thien123331/tibrain`
- Router: `LDoVietNam/tirouter-consolidated`
- Clients: `LDoVietNam/Tiiextension` · `LDoVietNam/Ti-CLI` · `LDoVietNam/Ti-Android`
- Docs: `LDoVietNam/docs`

---
*Generated 2026-07-18. Source of truth: GitHub API (gh) + local git. Không chỉnh sửa tay các URL ở mục 2 mà không cập nhật inventory memory.*
