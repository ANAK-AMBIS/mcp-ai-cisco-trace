# Checklist verifikasi topologi

Contoh memakai subnet `192.168.[NN].0/24` (`[NN]` = dua digit terakhir NIM).
Ganti `[NN]` dengan angka Anda.

## 1. Inventaris (via AI)

> *"tampilkan pt_query_topology"*

Cocokkan: 2 switch, 1 hub, 1 server, 2 access point,
4 laptop (wireless), 6 PC (3 statis + 3 DHCP).

## 2. Pengalamatan IP (via AI)

> *"tampilkan pt_inspect_ports, pastikan tidak ada IP duplikat"*

Yang diharapkan:

| Device | IP | Mode |
|---|---|---|
| PC statis | `192.168.[NN].1 – .3` | statis (`dhcp=false`) |
| Server | `192.168.[NN].10` | statis |
| PC DHCP + laptop | `192.168.[NN].100 – .150` | DHCP (`dhcp=true`) |
| Gateway | kosong (`0.0.0.0`) | tidak ada router |

Tambahan:

> *"jalankan pt_health_check"*

Harus `healthy`: 0 down link, 0 IP duplikat.

## 3. Ping antar-segmen (via AI)

> *"ping dari PC statis ke server, dari PC DHCP ke server,
> dari laptop ke server, dan dari laptop ke PC statis
> (pakai pt_verify_connectivity)"*

Semua harus `Sent = 4, Received = 4, Lost = 0`.
Ping pertama yang gagal (ARP) diulangi sekali.

## 4. Simulasi + Event List (manual di GUI)

1. Simulation mode → Edit Filters → hanya ICMP.
2. Add Simple PDU untuk tiap pasangan segmen → **Play sekali**.
3. Event List harus menunjukkan request sampai + reply kembali,
   tanpa baris merah/`Dropped`.
4. Uji switch vs hub: PDU dalam satu switch (unicast ke 1 port)
   vs PDU lewat hub (flood ke semua port, mungkin ada collision
   saat trafik simultan — itu normal dan justru bahan laporan).

## 5. Screenshot bukti

`func pt_screenshot` menyimpan canvas ke file (bukan console PT).
Untuk bukti console (`Reply from …`), foto manual jendela
Command Prompt tiap device (Win+Shift+S).
