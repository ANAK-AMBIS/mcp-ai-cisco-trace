# Claude Code

Daftarkan sekali (scope user = berlaku di semua project):

```powershell
claude mcp add --scope user --transport stdio packet-tracer "--" python -m packet_tracer_mcp --stdio
```

> Tanda kutip pada `"--"` wajib di PowerShell. Tanpa itu, PowerShell
> menelan pemisah `--` dan Claude gagal dengan `error: unknown option '-m'`.
> Di `cmd.exe` / Git Bash / Linux / macOS, kutipnya tidak perlu.

Verifikasi:

```powershell
claude mcp list
# cari: packet-tracer … ✓ Connected
```
