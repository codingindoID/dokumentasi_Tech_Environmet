# Deploy Source ke cPanel via GitHub

Dokumentasi ini berisi dua cara deploy project dari GitHub ke cPanel dengan lebih rapi dan mudah diikuti.

## Prasyarat

Sebelum memulai, pastikan hal-hal berikut sudah tersedia:

- Akun hosting dengan cPanel
- Akses ke cPanel
- Repo project sudah ada di GitHub
- Git sudah tersedia di server (jika ingin menggunakan metode 1)

---

## Metode 1: Menggunakan Git Version Control di cPanel (Direkomendasikan)

Jika cPanel kamu mendukung fitur Git Version Control, kamu bisa menarik source langsung dari repo GitHub.

### 1. Pastikan Git diaktifkan di cPanel

Ikuti langkah berikut:

- Masuk ke cPanel
- Cari menu Git Version Control
- Jika fitur tersebut tersedia, lanjut ke langkah berikutnya
- Jika tidak tersedia, gunakan Metode 2

### 2. Tambahkan SSH Key ke GitHub

Agar cPanel dapat mengakses repo private, tambahkan SSH key ke akun GitHub.

#### Langkah-langkah

1. Buka cPanel → Terminal atau SSH Access
2. Jalankan perintah berikut untuk membuat SSH key:

```bash
ssh-keygen -t rsa -b 4096 -C "email@example.com"
```

3. Tekan Enter terus sampai proses selesai tanpa memasukkan passphrase
4. Tampilkan public key:

```bash
cat ~/.ssh/id_rsa.pub
```

5. Salin hasilnya dan tambahkan ke GitHub:
   - GitHub → Settings → SSH and GPG keys → New SSH Key
   - Beri nama, misalnya: cPanel Deploy
   - Paste isi public key
   - Klik Add SSH Key

### 3. Tambahkan Repository di cPanel

Langkah berikutnya:

- Kembali ke cPanel → Git Version Control
- Klik Create Repository
- Masukkan Clone URL menggunakan SSH, misalnya:

```bash
git@github.com:username/repo.git
```

- Pilih lokasi penyimpanan di hosting
- Klik Create dan tunggu proses clone selesai

### 4. Tarik Update Secara Manual

Setiap ada perubahan di GitHub, kamu bisa masuk ke menu Git Version Control di cPanel lalu klik Update.

### 5. Tarik Update Secara Otomatis

Jika ingin update otomatis setiap ada perubahan:

- Masuk ke cPanel → Cron Jobs
- Tambahkan cron job dengan perintah berikut:

```bash
cd /home/username/public_html && git pull origin main
```

- Sesuaikan folder sesuai project kamu
- Atur jadwal setiap 5 atau 10 menit

---

## Metode 2: Manual dengan SSH (Jika Tidak Ada Git Version Control)

Jika fitur Git Version Control tidak tersedia, kamu bisa melakukan deploy secara manual lewat SSH.

### 1. Akses cPanel melalui SSH

Jika hosting mendukung SSH, masuk dengan perintah berikut:

```bash
ssh username@domain.com -p 22
```

### 2. Tambahkan SSH Key ke GitHub

Lakukan langkah yang sama seperti pada Metode 1, bagian 2.

### 3. Clone Repository

Masuk ke folder yang diinginkan, misalnya public_html:

```bash
cd public_html
git clone git@github.com:username/repo.git .
```

### 4. Tarik Update Secara Manual

Saat ingin update, jalankan:

```bash
cd public_html
git pull origin main
```

### 5. Tarik Update Secara Otomatis (Opsional)

Gunakan cron job seperti pada Metode 1, langkah 5.

---

## Catatan Penting

- Pastikan folder tujuan sudah benar, misalnya public_html atau subdomain tertentu
- Gunakan branch yang sesuai, biasanya main atau master
- Jika terjadi error autentikasi, cek kembali SSH key dan izin akses GitHub
