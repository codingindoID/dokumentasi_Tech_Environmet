# aktifkan 2fitur di windows

tekan

```cmd
windows + R

optionalfeatures
```

cari dan centang:

- virtual machine platform
- windows subSystem for linux

lalu restart

# Install ubuntu

```cmd
wsl --install -d Ubuntu
```

lalu cek dan jalankan

```cmd
wsl
```

jika error jalankan dulu

```cmd
wsl --update
```

# masuk ubuntu

update dulu

```cmd
sudo apt update
sudo apt upgrade -y
```

lalu

# Install paket yang dibutuhkan

```cmd
sudo apt install -y \
ca-certificates \
curl \
gnupg \
lsb-release
```

# Tambahkan Docker GPG Key

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

# Tambahkan Repository Docker

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

# Update lagi

```bash
sudo apt update
```

# install docker engine

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# Jalankan Docker

sudo service docker start

Cek status:

```bash
sudo service docker status
```

# Test Docker

```bash
sudo docker run hello-world
```

Kalau muncul pesan:

Hello from Docker!

berarti Docker Engine berhasil terpasang.

# Setelah itu

Agar tidak perlu selalu menggunakan sudo:

```bash
sudo usermod -aG docker $USER
```

Lalu keluar dari WSL:

```bash
exit
```

# masuk ulang

```bash
wsl
```

# cek docker

```bash
docker ps
```

seharusnya muncul

```bash
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

# BERHASIL
