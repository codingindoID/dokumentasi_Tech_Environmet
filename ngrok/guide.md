# Instalasi Ngrok di Windows

Dokumentasi ini digunakan untuk menghubungkan aplikasi Laravel yang berjalan di localhost agar dapat diakses melalui internet, misalnya untuk kebutuhan:

- Payment Webhook (Komerce, Midtrans, Xendit, dll)
- WhatsApp Webhook
- API Callback
- Testing dari perangkat lain

---

# 1. Install Ngrok

## Menggunakan Winget (Disarankan)

Buka Command Prompt atau PowerShell.

```bash
winget install ngrok.ngrok
```

Jika muncul:

```
Found an existing package already installed.
```

berarti ngrok sudah terinstall.

---

# 2. Jika Perintah ngrok Tidak Dikenali

Contoh error:

```
'ngrok' is not recognized as an internal or external command
```

Cari lokasi file `ngrok.exe`.

Contoh:

```
E:\MASTER\ngrok-v3-stable-windows-amd64
```

Masuk ke folder tersebut.

```cmd
cd /d E:\MASTER\ngrok-v3-stable-windows-amd64
```

Cek versi.

```cmd
ngrok.exe version
```

Contoh hasil:

```
ngrok version 3.3.1
```

---

# 3. Membuat Akun Ngrok

Daftar akun:

https://dashboard.ngrok.com/signup

Login ke dashboard:

https://dashboard.ngrok.com

---

# 4. Ambil Authtoken

Masuk ke menu

```
Getting Started
    └── Your Authtoken
```

atau buka

https://dashboard.ngrok.com/get-started/your-authtoken

Copy token tersebut.

Contoh:

```
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

# 5. Login ke Ngrok

Jalankan

```cmd
ngrok.exe config add-authtoken TOKEN_ANDA
```

Contoh

```cmd
ngrok.exe config add-authtoken xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Jika berhasil akan muncul

```
Authtoken saved to configuration file
```

---

# 6. Jika Muncul Error YAML

Misalnya

```
field authtoken not found
```

Buka file

```
C:\Users\<USERNAME>\AppData\Local\ngrok\ngrok.yml
```

Pastikan hanya berisi

```yaml
version: "3"

agent:
  authtoken: TOKEN_ANDA
```

Jangan sampai ada dua baris

```yaml
authtoken:
```

karena akan menyebabkan error.

---

# 7. Menjalankan Laravel

Masuk ke folder project.

```bash
php artisan serve
```

Hasil

```
INFO Server running on

http://127.0.0.1:8000
```

---

# 8. Menjalankan Ngrok

Buka terminal baru.

Masuk ke folder ngrok.

```cmd
cd /d E:\MASTER\ngrok-v3-stable-windows-amd64
```

Jalankan

```cmd
ngrok.exe http 8000
```

Jika berhasil akan muncul

```
Session Status    online

Forwarding

https://xxxx.ngrok-free.app
↓

http://localhost:8000
```

URL HTTPS tersebut digunakan sebagai URL publik.

---

# 9. Browser Warning

Untuk akun Free, ketika URL dibuka di browser akan muncul halaman

```
You are about to visit
```

Klik

```
Visit Site
```

Halaman ini hanya muncul satu kali pada browser tersebut.

> Payment Gateway (Webhook) tetap dapat mengakses URL tersebut tanpa masalah.

---

# 10. Dashboard Monitoring

Ngrok menyediakan dashboard lokal.

```
http://127.0.0.1:4040
```

Fitur:

- Request masuk
- Response
- Header
- Body
- Replay Request
- Status Code

Sangat berguna untuk debugging webhook.

---

# 11. Contoh Penggunaan Webhook Laravel

Misalnya route

```php
Route::post('/payment/webhook', [PaymentController::class, 'callback']);
```

Jika URL Ngrok adalah

```
https://abcd.ngrok-free.app
```

Maka webhook menjadi

```
https://abcd.ngrok-free.app/payment/webhook
```

---

# 12. Logging Laravel

Tambahkan logging pada controller.

```php
use Illuminate\Support\Facades\Log;

public function callback(Request $request)
{
    Log::info('Webhook', [
        'payload' => $request->all()
    ]);

    return response()->json([
        'success' => true
    ]);
}
```

Lihat hasilnya pada

```
storage/logs/laravel.log
```

---

# 13. Tips

- Jalankan Laravel terlebih dahulu.
- Jalankan Ngrok setelah Laravel aktif.
- Pastikan port sesuai dengan Laravel (`8000`).
- Jangan membagikan Authtoken ke orang lain.
- Jika Authtoken bocor, lakukan **Rotate Token** di Dashboard Ngrok.

---

# Alur Lengkap

```
Laravel
        │
        │ localhost:8000
        ▼
+----------------+
|     Ngrok      |
+----------------+
        │
        │ HTTPS
        ▼
https://xxxx.ngrok-free.app
        │
        ▼
Payment Gateway
(Komerce / Midtrans / Xendit)
        │
        ▼
Laravel Webhook
```

---

# Selesai

Sekarang aplikasi lokal sudah dapat menerima callback dari internet menggunakan Ngrok.
