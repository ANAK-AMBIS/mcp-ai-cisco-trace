# mcp-ai-cisco-trace

Bangun topologi **Cisco Packet Tracer** pakai bahasa alami.

Repo ini berisi konfigurasi [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) agar AI client bisa mengendalikan Packet Tracer: membaca topologi live, menambah device, menarik kabel, mengatur IP, menjalankan ping, sampai mengambil screenshot canvas.

Server MCP-nya **client-agnostic**. Pilih config sesuai aplikasi Anda di folder [`configs/`](configs/).

## Daftar Isi

- [Fitur](#fitur)
- [Syarat](#syarat)
- [Instalasi](#instalasi)
  - [1. Pasang extension di Packet Tracer](#1-pasang-extension-di-packet-tracer)
  - [2. Pasang server MCP](#2-pasang-server-mcp)
  - [3. Konfigurasi AI client](#3-konfigurasi-ai-client)
  - [4. Cek koneksi](#4-cek-koneksi)
- [Verifikasi topologi](#verifikasi-topologi)
- [Catatan](#catatan)
- [Kredit](#kredit)

## Fitur

Disediakan oleh server dan berjalan di semua client:

| | Fitur | Keterangan |
|---|---|---|
| 📡 | Baca topologi live | Device, IP, link, dan status bridge |
| 🛠️ | Bangun topologi | Tambah device/kabel dan set IP langsung dari chat |
| ✅ | Verifikasi | Ping antar-segmen, health check, lease DHCP |
| 📸 | Screenshot | Capture canvas otomatis |

## Syarat

- Python **3.11+**
- Cisco Packet Tracer **8.x+**
- Extension **MCP Control Center V5** terpasang di Packet Tracer
  - File sudah disertakan: [`extensions/V5.2.pts`](extensions/V5.2.pts)
  - Panduan bergambar: lihat langkah di bawah atau [`docs/setup.md`](docs/setup.md)

## Instalasi

### 1. Pasang extension di Packet Tracer

1. Buka Packet Tracer, lalu menu **Extensions → Scripting → Configure PT Script Modules…**

   ![Menu Extensions → Scripting](docs/images/01-extensions-menu.png)

2. Di dialog **Configure PT Script Modules**, klik **Add…** lalu pilih file `extensions/V5.2.pts`.

   ![Dialog Configure PT Script Modules](docs/images/02-configure-script-modules.png)

3. Pastikan **MCP-BUILDER** muncul di **Script Module List**, lalu klik **OK**.

   ![MCP-BUILDER terpasang](docs/images/03-mcp-builder-installed.png)

4. Buka **Extensions → MCP BUILDER**. Jendela connect akan terbuka otomatis, tidak ada setting tambahan.

### 2. Pasang server MCP

Cukup sekali:

```powershell
pip install packet-tracer-mcp
```

### 3. Konfigurasi AI client

| Client | Cara |
|---|---|
| **OpenCode 1.x** | Copy `configs/opencode.json` ke `~/.config/opencode/opencode.json`, lalu restart OpenCode |
| **Claude Code** | Jalankan perintah di bawah tabel |
| **Cursor** | Merge `configs/cursor.json` ke `~/.cursor/mcp.json`, lalu reload window |
| **VS Code (Copilot)** | Copy `configs/vscode-mcp.json` menjadi `.vscode/mcp.json` di folder project |
| **Claude Desktop** | Merge `configs/claude-desktop.json` ke `%APPDATA%\Claude\claude_desktop_config.json`, lalu restart aplikasi |

Perintah untuk Claude Code:

```powershell
claude mcp add --scope user --transport stdio packet-tracer "--" python -m packet_tracer_mcp --stdio
```

> ⚠️ Tanda kutip pada `"--"` **wajib** di PowerShell.

Setelah config terpasang, **restart AI client** supaya config terbaca.

### 4. Cek koneksi

Kirim prompt ini ke AI Anda:

> *"cek pt_bridge_status, apakah Packet Tracer sudah CONNECTED?"*

Jika jawabannya **CONNECTED** (via HTTP atau file-bridge), berarti siap dipakai.
Detail tiap langkah dan troubleshooting ada di [`docs/setup.md`](docs/setup.md).

## Verifikasi topologi

Checklist uji konektivitas, DHCP, dan simulasi ada di [`docs/verifikasi.md`](docs/verifikasi.md).

## Catatan

- `configs/opencode.json` memakai format OpenCode **1.x** (`mcp` + `enabled`). OpenCode **V2** memakai format berbeda (`mcp.servers` + `disabled`). Jangan dicampur; sesuaikan dengan versi terpasang (`opencode --version`).
- Repo ini tidak berisi API key atau rahasia apa pun, sehingga aman dibagikan publik.
- Jangan commit `service.json` milik OpenCode atau file `bridge_token` (keduanya sudah masuk `.gitignore`).

## Kredit

Extension `V5.2.pts` adalah karya [Mateo Andres Soto Gareca (Mats2208)](https://github.com/Mats2208/MCP-Packet-Tracer) (tertera di dialog About: v0.5.2, ID `com.matsoto.mcpbuilder`).

File ini disertakan agar teman sekelas tidak perlu mengunduh terpisah. Versi terbaru selalu tersedia di [releases resmi](https://github.com/Mats2208/MCP-Packet-Tracer/releases/latest).
