# WhatsApp Business Platform (Meta Cloud API) - Ringkasan Tarif

sumber :
https://www.facebook.com/business/help/563317871449819
https://www.facebook.com/business/help/2225184664363779?id=2129163877102343

> **Dokumen Ringkasan**
> Versi: Juli 2026
> Platform: Meta WhatsApp Business Platform (Cloud API)

---

# Pendahuluan

Mulai **1 Juli 2025**, Meta mengubah sistem penagihan WhatsApp Business Platform dari **Conversation Pricing** menjadi **Message Pricing**.

Artinya, biaya tidak lagi dihitung berdasarkan percakapan (conversation), tetapi berdasarkan **Template Message** yang berhasil dikirim.

Sedangkan balasan biasa (Free-form Message) selama **Customer Service Window (24 jam)** tetap gratis.

---

# Perubahan Sistem Tarif

## Sebelum 1 Juli 2025

Menggunakan **Conversation Pricing**.

Karakteristik:

- Biaya dihitung per Conversation.
- Conversation berlaku selama 24 jam.
- Selama 24 jam dapat mengirim banyak pesan tanpa biaya tambahan.

### Contoh

```
10.00

↓

Kirim Template Reminder

↓

Conversation terbuka

↓

Selama 24 jam dapat mengirim:

- Text
- Gambar
- PDF
- Lokasi
- Audio

↓

Tetap dihitung 1 Conversation
```

---

## Setelah 1 Juli 2025

Menggunakan **Message Pricing**.

Karakteristik:

- Biaya dihitung setiap Template Message.
- Setiap Template merupakan satu transaksi.
- Tidak lagi berdasarkan Conversation.

### Contoh

```
Template Reminder

↓

Bayar 1x

Template Promo

↓

Bayar lagi

Template OTP

↓

Bayar lagi
```

---

# Kategori Pesan

## 1. Marketing

Digunakan untuk:

- Promo
- Diskon
- Voucher
- Produk Baru
- Campaign
- Penawaran

### Contoh

```
Halo Bapak Andi,

Nikmati Diskon MCU 30%.

Klik tombol berikut.
```

**Karakteristik**

- Wajib menggunakan Template
- Selalu dikenakan biaya

---

## 2. Utility

Digunakan untuk informasi transaksi atau layanan.

Contoh:

- Reminder kontrol
- Invoice
- Status pembayaran
- Nomor antrian
- Status pesanan
- Jadwal pelayanan

### Contoh

```
Halo Bapak Andi,

Jadwal kontrol Anda:

Sabtu, 26 Juli 2026
09.00 WIB
```

**Karakteristik**

- Menggunakan Template
- Dikenakan biaya apabila dikirim di luar Customer Service Window
- Dalam kondisi tertentu dapat gratis apabila masih berada dalam Customer Service Window sesuai kebijakan Meta

---

## 3. Authentication

Digunakan untuk:

- OTP
- Login
- Reset Password
- Verifikasi Akun

### Contoh

```
Kode OTP Anda

483927
```

**Karakteristik**

- Wajib menggunakan Template
- Selalu dikenakan biaya

---

## 4. Service

Merupakan percakapan biasa antara pelanggan dan bisnis.

### Contoh

```
Pasien:
Jam buka poli berapa?

↓

Admin:
Poli buka pukul 07.00–20.00 WIB.
```

**Karakteristik**

- Tidak menggunakan Template
- Gratis selama Customer Service Window

---

# Customer Service Window (24 Jam)

Window dimulai saat pelanggan mengirim pesan.

Misal:

```
10.00

↓

Customer

Halo

↓

Window aktif

24 Jam
```

Selama 24 jam, bisnis dapat mengirim:

- Text
- Image
- PDF
- Video
- Audio
- Lokasi
- Free-form Message

Tanpa perlu menggunakan Template untuk balasan biasa.

Apabila sudah lewat 24 jam sejak pesan terakhir pelanggan, maka bisnis **harus menggunakan Template Message** untuk menghubungi kembali pelanggan.

---

# Meta Ads Window (72 Jam)

Apabila pelanggan berasal dari:

- Click to WhatsApp Ads
- Facebook Ads
- Instagram Ads

Maka Meta memberikan **72 jam masa komunikasi**.

Dalam periode tersebut berlaku kebijakan khusus sesuai aturan Meta yang memungkinkan pengiriman pesan tertentu tanpa biaya template.

---

# Estimasi Tarif Indonesia

> **Catatan:** Tarif dapat berubah sewaktu-waktu mengikuti kebijakan Meta.

| Kategori       |         Estimasi Tarif |
| -------------- | ---------------------: |
| Marketing      |        ± Rp597 / pesan |
| Utility        |        ± Rp367 / pesan |
| Authentication |        ± Rp367 / pesan |
| Service        | Gratis (24 Jam Window) |

---

# Contoh Perhitungan

Misalkan rumah sakit mengirim:

| Jenis            | Jumlah |
| ---------------- | -----: |
| Reminder Kontrol |  1.000 |
| Promo MCU        |    500 |
| OTP              |    200 |

### Perhitungan

```
Utility

1.000 × Rp367

= Rp367.000
```

```
Marketing

500 × Rp597

= Rp298.500
```

```
Authentication

200 × Rp367

= Rp73.400
```

## Total

```
Rp367.000
+ Rp298.500
+ Rp73.400
----------------
Rp738.900
```

---

# Studi Kasus Rumah Sakit

## Reminder Kontrol

```
Halo Pak Budi,

Besok kontrol
Jam 09.00 WIB
```

Kategori:

> Utility

---

## Promo Medical Check Up

```
Diskon MCU 30%

Berlaku sampai akhir bulan.
```

Kategori:

> Marketing

---

## OTP Login

```
Kode Anda

182738
```

Kategori:

> Authentication

---

## Pasien Bertanya

```
Pasien

Jam buka poli?

↓

Admin

07.00 - 20.00 WIB
```

Kategori:

> Service (Gratis)

---

# Implementasi pada WA Gateway Laravel

Sebaiknya setiap pesan menyimpan informasi berikut.

```text
id
recipient
template_name
category
status
message_id
billable
sent_at
created_at
```

Kategori dapat berupa:

```text
marketing
utility
authentication
service
```

Dengan data tersebut aplikasi dapat membuat:

- Dashboard penggunaan WhatsApp API
- Rekap pesan per kategori
- Estimasi biaya harian
- Estimasi biaya bulanan
- Statistik Template
- Monitoring pengiriman

---

# Kesimpulan

- Mulai **1 Juli 2025**, Meta menggunakan **Per-Message Pricing**.
- Setiap **Template Message** dikenakan biaya sesuai kategori.
- **Free-form Message** tetap gratis selama berada dalam **Customer Service Window (24 jam)**.
- Marketing merupakan kategori dengan tarif paling tinggi.
- Utility digunakan untuk notifikasi transaksi seperti reminder, invoice, dan jadwal kontrol.
- Authentication digunakan untuk OTP dan verifikasi akun.
- Untuk aplikasi WA Gateway Laravel, disarankan menyimpan kategori pesan dan status penagihan agar dapat menghasilkan laporan penggunaan serta estimasi biaya secara otomatis.

---

# Referensi

- Meta Business Help – WhatsApp Business Pricing
- Meta Developers – WhatsApp Business Platform Pricing
- WhatsApp Business Platform Documentation
