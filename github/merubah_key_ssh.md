# Panduan Konfigurasi Custom SSH Key untuk Git di cPanel

Dokumentasi ini digunakan sebagai referensi ketika menghubungkan cPanel ke Git remote repository (GitHub / GitLab / Bitbucket) menggunakan SSH Key dengan nama kustom (bukan default `id_rsa`).

---

## 1. Cek SSH Key yang Tersedia

Jalankan perintah ini di Terminal cPanel untuk melihat file key:

```bash
ls -la ~/.ssh
```

**Identifikasi file:**
- File private key: `wts` (tanpa ekstensi, izin akses `600`)
- File public key: `wts.pub` (berakhiran `.pub`)

---

## 2. Salin Public Key ke Remote Repository

Tampilkan isi public key di terminal:

```bash
cat ~/.ssh/wts.pub
```

1. Salin seluruh output teks yang muncul (dimulai dari `ssh-rsa ...` hingga baris terakhir).
2. Buka layanan Git remote Anda:
   - **GitHub (Per-repo):** Masuk ke repo > **Settings** > **Deploy Keys** > **Add deploy key** (Centang *Allow write access* jika butuh push).
   - **GitHub (Global akun):** Buka **Settings** akun > **SSH and GPG keys** > **New SSH key**.
   - **GitLab:** Buka repo > **Settings** > **Repository** > **Deploy keys** (atau User Settings > **SSH Keys**).
3. Tempel (*paste*) isi key lalu simpan.

---

## 3. Konfigurasi `~/.ssh/config`

Secara default, Git di cPanel hanya mencari file `~/.ssh/id_rsa`. Agar SSH mengenali file `wts`, buat file config:

Buka editor file config:
```bash
nano ~/.ssh/config
```

Masukkan konfigurasi berikut (sesuaikan host jika menggunakan GitHub atau GitLab):

### Untuk GitHub:
```text
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/wts
    IdentitiesOnly yes
```

### Untuk GitLab:
```text
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/wts
    IdentitiesOnly yes
```

> **Navigasi Nano:**
> - Simpan: Tekan `Ctrl + O` lalu tekan `Enter`
> - Keluar: Tekan `Ctrl + X`

---

## 4. Set Hak Akses (Permissions)

SSH mewajibkan izin akses file bersifat privat dan ketat agar koneksi tidak ditolak (*Permission Denied*):

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/wts
chmod 644 ~/.ssh/wts.pub
```

---

## 5. Uji Koneksi SSH

Jalankan pengujian koneksi ke remote host:

### GitHub:
```bash
ssh -T git@github.com
```

### GitLab:
```bash
ssh -T git@gitlab.com
```

> **Catatan:**
> Jika muncul pesan:
> `Are you sure you want to continue connecting (yes/no/[fingerprint])?`
> Ketik `yes` lalu tekan **Enter**.

**Tanda Berhasil:**
- GitHub: `Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.`
- GitLab: `Welcome to GitLab, @<username>!`

---

## 6. Update Repository di cPanel

Setelah koneksi terverifikasi:
1. Masuk ke **cPanel** > menu **Git™ Version Control**.
2. Klik **Manage** pada direktori repositori (misalnya `/home/codingin/public_html/warungtepisungai.com`).
3. Masuk ke tab **Pull or Deploy**.
4. Klik **Update from Remote**.