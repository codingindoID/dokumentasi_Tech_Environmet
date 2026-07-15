<h1>Membuat bat backup DATABASE</h1>
pada contoh kali ini saya mengunakan db pgsql :

```cmd

@echo off
set BACKUP_DIR=E:\LOKASI_PATH
if not exist "%BACKUP_DIR%" mkdir "%BACKUP_DIR%"
for /f %%i in ('powershell -NoProfile -Command "Get-Date -Format yyyy-MM-dd_HH-mm-ss"') do set DATETIME=%%i
"D:\Program Files\PostgreSQL\14\bin\pg_dump.exe" ^
-h IP_SERVER ^
-p 5432 ^
-U postgres ^
-F c ^
-b ^
-v ^
-d nama_database ^
-f "%BACKUP_DIR%\nama_saat_disimpan_%DATETIME%.backup"

```

jika membutuhkan password :

```cmd
set PGPASSWORD=password_server
```
