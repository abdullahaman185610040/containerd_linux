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

### 3. Verifikasi Instalasi

Untuk memverifikasi apakah Containerd sudah berjalan dengan benar, gunakan perintah berikut:

```bash
sudo systemctl status containerd
```

Anda akan melihat status `active (running)` jika Containerd sudah berjalan dengan baik.

Berikut adalah contoh file Markdown yang berisi panduan menjalankan container dengan Containerd dan cara mengerjakan tugas di dalamnya. Anda dapat menyalin teks ini ke dalam file `.md` di GitHub atau dokumen lainnya.

---

# Panduan Menjalankan Container dan Mengerjakan Tugas dengan Containerd

## 1. Menarik (Pull) Image

Langkah pertama adalah menarik (download) image dari Docker Hub atau registry lainnya. Misalnya, kita akan menggunakan image `alpine`, distribusi Linux yang ringan:

```bash
sudo ctr image pull docker.io/library/alpine:latest
```

Perintah ini akan mengunduh image `alpine` versi terbaru (`latest`) ke sistem Anda.

## 2. Menjalankan Container

Setelah image berhasil diunduh, jalankan container menggunakan perintah berikut:

```bash
sudo ctr run -t --rm docker.io/library/alpine:latest mycontainer /bin/sh
```

### Penjelasan:
- `ctr run`: Perintah untuk menjalankan container.
- `-t`: Membuka terminal interaktif di dalam container.
- `--rm`: Menghapus container setelah selesai dijalankan.
- `docker.io/library/alpine:latest`: Image yang digunakan untuk membuat container.
- `mycontainer`: Nama unik untuk container yang Anda jalankan.
- `/bin/sh`: Shell yang akan dijalankan di dalam container.

## 3. Mengerjakan Tugas di Dalam Container

Setelah menjalankan perintah di atas, Anda akan masuk ke dalam container dan dapat menjalankan berbagai perintah di dalamnya.

### Contoh 1: Membuat File dan Direktori di Dalam Container

Setelah masuk ke dalam container, Anda dapat membuat direktori dan file:

```sh
mkdir /mydir
echo "Hello from Containerd" > /mydir/myfile.txt
cat /mydir/myfile.txt
```

Perintah ini akan membuat direktori `mydir`, menulis teks ke file `myfile.txt`, dan kemudian menampilkan konten file tersebut.

### Contoh 2: Memasang Paket (Jika Perlu)

Jika Anda memerlukan paket tambahan di dalam container, Anda bisa menginstalnya menggunakan `apk`, package manager yang digunakan oleh `alpine`:

```sh
apk update
apk add curl
```

Perintah ini akan menginstal `curl` di dalam container, yang dapat digunakan untuk berbagai operasi jaringan.

## 4. Keluar dari Container

Setelah selesai mengerjakan tugas di dalam container, Anda bisa keluar dari shell dan menghentikan container dengan mengetik `exit`:

```sh
exit
```

Karena kita menggunakan opsi `--rm` saat menjalankan container, container ini akan secara otomatis dihapus setelah Anda keluar.

## 5. Memverifikasi Penghapusan Container

Jika Anda ingin memastikan bahwa container telah dihapus setelah selesai, Anda bisa memeriksa daftar container yang berjalan:

```bash
sudo ctr container list
```

Jika container telah dihapus, Anda tidak akan melihatnya dalam daftar.


## 6. Menggunakan Pods

Selain menjalankan container individual, Containerd juga mendukung penggunaan pods.

```bash
sudo ctr run --rm --net-host docker.io/library/alpine:latest mypod -- sh -c "echo Hello from Containerd"
```

## Kesimpulan

Containerd adalah runtime container yang ringan dan efisien, ideal untuk berbagai skenario containerisasi. Dengan `ctr`, Anda bisa melakukan berbagai operasi dasar dan lanjutan pada container dengan mudah.
