# 🏢 Network Infrastructure Design — Startup Office (30 Users)

> Simulasi desain jaringan untuk kantor startup dengan 3 divisi menggunakan Cisco Packet Tracer.
> Mencakup VLAN segmentation, inter-VLAN routing, DHCP server, dan Access Control List (ACL).

---

## 📋 Skenario

Sebuah perusahaan startup dengan 30 karyawan membutuhkan infrastruktur jaringan yang:
- Memisahkan traffic antar divisi menggunakan VLAN
- Memberikan IP otomatis ke semua workstation via DHCP
- Membatasi akses ke Server Room hanya untuk divisi tertentu (ACL)

---

## 🗂️ Desain VLAN

| VLAN ID | Nama       | Network           | Range DHCP          | Gateway        |
|---------|------------|-------------------|---------------------|----------------|
| 10      | HRD        | 192.168.10.0/24   | .100 – .150         | 192.168.10.1   |
| 20      | Developer  | 192.168.20.0/24   | .100 – .150         | 192.168.20.1   |
| 30      | Server     | 192.168.30.0/24   | Static (.10)        | 192.168.30.1   |

---

## 🔒 Kebijakan Akses (ACL)

| Sumber       | Tujuan      | Akses     |
|--------------|-------------|-----------|
| HRD (V10)    | Server (V30)| ❌ Ditolak |
| Developer (V20) | Server (V30) | ✅ Diizinkan |
| Semua VLAN   | Internet    | ✅ Diizinkan |

---

## 🖥️ Topologi Jaringan

![Topologi](images/topologi.png)

---

## ⚙️ Tech Stack

| Komponen | Detail |
|----------|--------|
| Simulasi | Cisco Packet Tracer |
| Router   | Cisco 2911 |
| Switch   | Cisco 2960-24TT |
| Metode Routing | Router-on-a-Stick (subinterface) |
| DHCP | Dikonfigurasi di Router |
| Firewall | Extended Named ACL |

---

## 📁 Struktur Repo

```
network-infrastructure-portfolio/
├── README.md
├── network-design.pkt          # File Packet Tracer
└── images/
    ├── topologi.png            # Diagram topologi keseluruhan
    ├── vlan-brief.png          # Output show vlan brief
    ├── ping-hrd-to-dev.png     # HRD → Developer (berhasil)
    ├── ping-hrd-to-server.png  # HRD → Server (gagal/blocked)
    └── ping-dev-to-server.png  # Developer → Server (berhasil)
```

---

## 🚀 Cara Membuka Project

1. Install [Cisco Packet Tracer](https://www.netacad.com) (gratis, butuh akun Netacad)
2. Download file `network-design.pkt`
3. Buka file tersebut di Packet Tracer
4. Klik PC mana saja → Desktop → Command Prompt untuk test ping

---

## ✅ Hasil Verifikasi

### VLAN Assignment
<img width="641" height="297" alt="image" src="https://github.com/user-attachments/assets/e1aa4683-3d78-46b9-b06c-5e6b8e67c0d3" />

### Ping Test — HRD ke Developer (Berhasil ✅)
![Ping HRD to Dev](images/ping-hrd-to-dev.png)

### Ping Test — HRD ke Server (Diblokir ACL ❌)
![Ping HRD to Server](images/ping-hrd-to-server.png)

### Ping Test — Developer ke Server (Berhasil ✅)
![Ping Dev to Server](images/ping-dev-to-server.png)

---

## 💡 Keputusan Desain

**Kenapa Router-on-a-Stick?**
Menggunakan satu physical interface router dengan beberapa subinterface untuk efisiensi hardware. Cocok untuk skala kantor kecil-menengah yang tidak membutuhkan Layer 3 switch.

**Kenapa Extended ACL?**
Extended ACL memungkinkan filtering berdasarkan IP sumber DAN tujuan sekaligus, sehingga bisa menerapkan kebijakan akses yang lebih spesifik dibanding Standard ACL.

**Kenapa DHCP di Router?**
Untuk jaringan skala kecil, centralized DHCP di router lebih simpel dan tidak membutuhkan dedicated DHCP server, mengurangi jumlah perangkat yang perlu dikelola.

---

## 🧠 Tantangan & Solusi

| Tantangan | Solusi |
|-----------|--------|
| Port Gi0/1 Switch tidak muncul sebagai trunk di `show vlan brief` | Normal — trunk port memang tidak masuk kolom VLAN, verifikasi dengan `show interfaces trunk` |
| PC tidak dapat IP dari DHCP | Pastikan port Switch sudah di-assign ke VLAN yang benar dan PC di-set ke mode DHCP |
