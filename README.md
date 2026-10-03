# mcp-ai-cisco-trace

Bangun topologi Cisco Packet Tracer pakai bahasa alami.
Repo ini berisi konfigurasi [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
agar AI client bisa mengendalikan Packet Tracer: membaca topologi live,
menambah device, menarik kabel, mengatur IP, menjalankan ping,
sampai screenshot canvas.

Server MCP-nya client-agnostic — pilih config sesuai aplikasi di `configs/`.

## Fitur (disediakan oleh server, jalan di semua client)

- 📡 Baca topologi live: device, IP, link, status bridge
- 🛠️ Tambah device/kabel + set IP langsung dari chat
- ✅ Verifikasi: ping antar-segmen, health check, lease DHCP
- 📸 Screenshot canvas otomatis

## Syarat (sama untuk semua client)

- Python 3.11+
- Cisco Packet Tracer 8.x+
- Extension **MCP Control Center V5** (file `.pts`) terpasang di PT:
  **Extensions → Scripting → Configure PT Script Modules → Add…**,
  lalu buka **Extensions → MCP BUILDER**
- Install server MCP-nya sekali:
  ```powershell
  pip install packet-tracer-mcp
  ```

## Setup per client

| Client | Cara |
|---|---|
| OpenCode 1.x | Copy `configs/opencode.json` ke `~/.config/opencode/opencode.json`, restart OpenCode |
| Claude Code | `claude mcp add --scope user --transport stdio packet-tracer "--" python -m packet_tracer_mcp --stdio` (tanda kutip di `"--"` wajib di PowerShell) |
| Cursor | Merge `configs/cursor.json` ke `~/.cursor/mcp.json`, reload window |
| VS Code (Copilot) | Copy `configs/vscode-mcp.json` menjadi `.vscode/mcp.json` di folder project |
| Claude Desktop | Merge `configs/claude-desktop.json` ke `%APPDATA%\Claude\claude_desktop_config.json`, restart aplikasi |

Cek koneksi (prompt ke AI Anda):
> *"cek pt_bridge_status, apakah Packet Tracer sudah CONNECTED?"*

Kalau jawabannya CONNECTED (HTTP atau file-bridge), berarti siap dipakai.
Detail tiap langkah + troubleshooting ada di [`docs/setup.md`](docs/setup.md).

## Verifikasi topologi

Checklist uji konektivitas, DHCP, dan simulasi ada di
[`docs/verifikasi.md`](docs/verifikasi.md).

## Catatan

- `configs/opencode.json` memakai format OpenCode **1.x** (`mcp` + `enabled`).
  OpenCode **V2** memakai format berbeda (`mcp.servers` + `disabled`) —
  jangan campur, sesuaikan dengan versi yang terinstall (`opencode --version`).
- File ini tidak berisi API key / rahasia apa pun, aman di-share publik.
- Jangan commit `service.json` milik OpenCode atau file `bridge_token`
  (keduanya sudah di-`.gitignore`).
