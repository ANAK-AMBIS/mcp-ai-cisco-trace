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
- Extension **MCP Control Center V5** terpasang di PT — file-nya sudah
  included di [`extensions/V5.2.pts`](extensions/V5.2.pts),
  panduan bergambar di [`docs/setup.md`](docs/setup.md)
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

## Tutorial pasang extension MCP di Packet Tracer (bergambar)

File `.pts`-nya sudah ada di [`extensions/V5.2.pts`](extensions/V5.2.pts).

1. Buka Packet Tracer → menu **Extensions** → **Scripting** →
   **Configure PT Script Modules…**

   ![Menu Extensions → Scripting](docs/images/01-extensions-menu.png)

2. Di dialog **Configure PT Script Modules**, klik **Add…** lalu pilih file
   `extensions/V5.2.pts`.

   ![Dialog Configure PT Script Modules](docs/images/02-configure-script-modules.png)

3. Pastikan **MCP-BUILDER** muncul di **Script Module List**, lalu klik **OK**.

   ![MCP-BUILDER terpasang](docs/images/03-mcp-builder-installed.png)

4. Buka **Extensions → MCP BUILDER**. Jendela connect otomatis, tidak ada
   setting tambahan. Restart AI client supaya config-nya terbaca.

Setelah itu lanjut ke [Setup per client](#setup-per-client) dan tes
`pt_bridge_status` seperti di atas.

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
- Extension `V5.2.pts` adalah karya
  [Mateo Andres Soto Gareca (Mats2208)](https://github.com/Mats2208/MCP-Packet-Tracer)
  (tertera di dialog About-nya: v0.5.2, ID `com.matsoto.mcpbuilder`).
  Disertakan di sini agar teman sekelas tidak perlu download terpisah;
  versi terbaru selalu di
  [releases resmi](https://github.com/Mats2208/MCP-Packet-Tracer/releases/latest).
