# Panduan Instalasi dan Penggunaan Containerd

## Pendahuluan

**Containerd** adalah runtime container open-source yang ringan dan efisien, digunakan untuk menjalankan container di berbagai lingkungan cloud dan infrastruktur. Containerd banyak digunakan sebagai runtime di Kubernetes dan mendukung berbagai format image container seperti OCI dan Docker.

## Instalasi Containerd

### 1. Persiapan Sistem

Pastikan sistem Anda diperbarui sebelum menginstal Containerd.

```bash
sudo apt-get update
sudo apt-get upgrade
```

### 2. Instalasi Containerd

Anda dapat menginstal Containerd melalui package manager `apt` pada distribusi Ubuntu.

#### a. Instalasi Containerd dari Repositori Resmi

Instal Containerd menggunakan perintah berikut:

```bash
sudo apt-get install -y containerd
```

#### b. Instalasi dari Source atau Versi Terbaru

Jika Anda memerlukan versi terbaru, Anda bisa menginstalnya dari binary yang tersedia di [GitHub Releases](https://github.com/containerd/containerd/releases).

1. **Unduh Binary Containerd**:

   ```bash
   wget https://github.com/containerd/containerd/releases/download/v1.7.0/containerd-1.7.0-linux-amd64.tar.gz
   ```

2. **Ekstrak dan Instal**:

   ```bash
   sudo tar Cxzvf /usr/local containerd-1.7.0-linux-amd64.tar.gz
   ```

3. **Konfigurasi Systemd**:

   Buat file service untuk Containerd di `/etc/systemd/system/containerd.service` dengan isi sebagai berikut:

   ```ini
   [Unit]
   Description=containerd container runtime
   Documentation=https://containerd.io
   After=network.target

   [Service]
   ExecStart=/usr/local/bin/containerd
   Restart=always
   RestartSec=5
   Delegate=yes
   KillMode=process
   OOMScoreAdjust=-999

   [Install]
   WantedBy=multi-user.target
   ```

4. **Mulai dan Aktifkan Containerd**:

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start containerd
   sudo systemctl enable containerd
   ```

### 3. Verifikasi Instalasi

Untuk memverifikasi apakah Containerd sudah berjalan dengan benar, gunakan perintah berikut:

```bash
sudo systemctl status containerd
```

Anda akan melihat status `active (running)` jika Containerd sudah berjalan dengan baik.

## Penggunaan Containerd

### 1. Menarik (Pull) Image

Anda bisa menggunakan `ctr`, CLI dari Containerd, untuk menarik image dari Docker Hub atau registry lainnya. Sebagai contoh, untuk menarik image `alpine`:

```bash
sudo ctr image pull docker.io/library/alpine:latest
```

### 2. Menjalankan Container

Setelah image berhasil diunduh, Anda bisa menjalankan container menggunakan perintah berikut:

```bash
sudo ctr run -t --rm docker.io/library/alpine:latest mycontainer /bin/sh
```

- **Penjelasan**:
  - `run`: Perintah untuk menjalankan container.
  - `-t`: Opsi untuk membuka terminal interaktif di dalam container.
  - `--rm`: Menghapus container setelah berhenti.
  - `docker.io/library/alpine:latest`: Image yang digunakan.
  - `mycontainer`: Nama unik untuk container.
  - `/bin/sh`: Perintah yang dijalankan di dalam container (shell).

### 3. Mengelola Container

#### a. Melihat Daftar Image yang Diunduh

```bash
sudo ctr image list
```

#### b. Melihat Daftar Container yang Berjalan

```bash
sudo ctr container list
```

#### c. Menghentikan Container

Untuk menghentikan container yang sedang berjalan:

```bash
sudo ctr task kill mycontainer
```

#### d. Menghapus Container

Jika Anda ingin menghapus container:

```bash
sudo ctr container delete mycontainer
```

#### e. Menghapus Image

Untuk menghapus image yang tidak lagi diperlukan:

```bash
sudo ctr image remove docker.io/library/alpine:latest
```

### 4. Menggunakan Pods

Selain menjalankan container individual, Containerd juga mendukung penggunaan pods.

```bash
sudo ctr run --rm --net-host docker.io/library/alpine:latest mypod -- sh -c "echo Hello from Containerd"
```

### 5. Melihat Log Container

Anda bisa melihat log dari container dengan perintah:

```bash
sudo ctr task logs mycontainer
```

## Kesimpulan

Containerd adalah runtime container yang ringan dan efisien, ideal untuk berbagai skenario containerisasi. Dengan `ctr`, Anda bisa melakukan berbagai operasi dasar dan lanjutan pada container dengan mudah.

---

Anda dapat menggunakan file ini sebagai dokumentasi untuk instalasi dan penggunaan Containerd. Apakah ada hal lain yang ingin Anda tambahkan atau modifikasi?