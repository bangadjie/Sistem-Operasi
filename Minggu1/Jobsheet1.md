# Laporan Praktikum Sistem Operasi Jobsheet 1

<h4> Nama   : Muhammad Unggul Satria Adjie <h4>
<h4> NIM    : 254107020040 <h4>
<h4> Kelas  : TI-1G <h4>

## 1.6. Instalasi Sistem Operasi
### 1.6.4. Praktik: Instalasi Ubuntu Server 22.04 LTS
![](Images/ubuntu.png "")
![](Images/virtualBox.png "")
![](Images/login.png "")
![](Images/hasilLogin.png "")
## 1.7. Partisi Disk dan Sistem File
### 1.7.3. Skema Partisi Linux
* Karena saya sudah mendownload dan sudah setting keseluruhan virtual box, disini saya memberikan dokumentasi dari virtual box langsung dan bagian linux ketika sudah di jalankan.

### 1.7.6. Praktik: Manual Partitioning

1. Check disk usage
```
 df -h
```
![](Images/1.7.6df-h.png "")

2. Check disk partitions
```
 lsblk
 sudo fdisk -l
```
![](Images/1.7.6sudoLsblk.png "")

3. Check filesystem type
```
 lsblk -f
 blkid
```
![](Images/1.7.6blkid.png "")

4. Mount additional filesystem manually
```
 sudo mkdir /mnt/data
 sudo mount /dev/sdb1 /mnt/data
```
![](Images/1.7.6mkdir.png "")

5. Permanent mount - edit /etc/fstab
```
 cat /etc/fstab
```
![](Images/1.7.6cat.png "")

6. Format template untuk /etc/fstab:
```
Format : <device > <mount point > <type > <options > <dump > <pass >
```

7. UUID - based mount ( preferred )
```
 UUID=xxx -xxx -xxx /data ext4 defaults 0 2
```
![](Images/1.7.6uuid.png "")

## 1.8. Konfigurasi Post-Installation
### 1.8.1. System Updates
1. Update package lists from repositories
```
sudo apt update
```
![](Images/8.1%201.png "")

2. Upgrade installed packages to latest versions
```
sudo apt upgrade
```
![](Images/8.1%202.png "")

3. Full system upgrade (handles dependencies)
```
sudo apt full-upgrade
```
![](Images/8.1%203.png "")

4. Remove packages yang tidak dibutuhkan
```
sudo apt autoremove
```
![](Images/8.1%204.png "")

5. Clean package cache
```
sudo apt clean
```
![](Images/8.1%205.png "")

### 1.8.2. Software Installation Essentials
1. Development tools
```
sudo apt install -y build-essential
```
![](Images/8.2%201.png "")

2. Version control
```
sudo apt install -y git
```
![](Images/8.2%202.png "")

3. Network utilities
```
sudo apt install -y curl wget net-tools
```
![](Images/8.2%203.png "")

4. Text editors
```
sudo apt install -y vim nano
```
![](Images/8.24.png "")

5. System monitoring
```
sudo apt install -y htop iotop
```
![](Images/8.2%205.png "")

6. Complete essentials in one command
```
sudo apt install -y 
build-essential 
git 
curl 
wget 
vim 
htop 
tree 
net-tools 
openssh-server
```
![](Images/8.2%206.png "")

### 1.8.3. Verifikasi Koneksi Jaringan
1. Check IP address
```
ip addr show
```
![](Images/8.31.png "")

2. Test internet connectivity
```
ping -c 4 google.com
```
![](Images/8.32.png "")

3. Check DNS
```
cat /etc/resolv.conf
```
![](Images/8.33.png "")

### 1.8.4. Akses SSH (Opsional)
1. Check if SSH installed dan running
```
sudo systemctl status ssh
```
![](Images/841.png "")

2. Get IP address untuk remote access
```
ip addr show
```
![](Images/842.png "")

### 1.8.5. Menampilkan Informasi Sistem
1. Install neofetch
```
sudo apt install -y neofetch
```
![](Images/851.png "")

2. Run neofetch
```
neofetch
```
![](Images/852.png "")

### Perintah Informasi Sistem Dasar
1. Informasi OS
```
cat /etc/os-release
hostnamectl
```
![](Images/861.png "")

2. Versi kernel
```
unamer
```
![](Images/862.png "")

3. Informasi CPU
```
1scpu
```
![](Images/863.png "")

4. Informasi memory
```
free -h
```
![](Images/864.png "")

5. Informasi disk
```
df -h
lsblk
```
![](Images/865.png "")

6. System uptime
```
uptime
```
![](Images/866.png "")

7. Who is logged in
```
who
```
![](Images/867.png "")

## 1.10. Latihan
### 1.10.1. Latihan Konseptual
#### Latihan 1.1
Jelaskan 5 fungsi utama sistem operasi dengan contoh konkret dari minimal 2 OS berbeda (Windows, macOS, atau Linux).

Jawab : 
1. Manajemen Proses (Mengatur Jalannya Aplikasi)
Fungsi ini bertugas mengatur aplikasi mana yang boleh berjalan dan memastikan CPU (otak komputer) membagi waktunya dengan adil agar tidak terjadi tabrakan antar program.
* Contoh di Windows: Menggunakan Task Manager untuk melihat aplikasi yang sedang berjalan dan mematikan program yang macet (Not Responding).
* Contoh di Linux: Menggunakan perintah top atau htop di Terminal untuk memantau proses sistem secara langsung.

2. Manajemen Memori (Mengatur RAM)
Sistem operasi bertugas mengalokasikan ruang di memori utama (RAM) untuk setiap aplikasi yang sedang dibuka agar data tidak saling tumpang tindih.
* Contoh di Windows: Memanfaatkan fitur Virtual Memory (Pagefile) untuk meminjam kapasitas harddisk saat RAM fisik sudah hampir penuh.
* Contoh di Linux: Menggunakan area khusus bernama Swap Partition sebagai cadangan memori jika RAM utama sudah habis terpakai.

3. Manajemen File (Mengatur Penyimpanan Data)
Fungsi ini mengelola cara data disimpan, disusun dalam folder, dan diberi nama agar pengguna mudah mencarinya kembali.
* Contoh di Windows: Mengatur penyimpanan data ke dalam partisi yang berbeda, seperti Drive C: untuk sistem dan Drive D: untuk data pribadi.
* Contoh di Linux: Mengatur semua file dalam satu struktur hirarki tunggal yang dimulai dari akar atau Root (/).

4. Manajemen Perangkat Keras (Menghubungkan Hardware)
Sistem operasi bertindak sebagai penghubung antara perangkat keras (seperti printer, mouse, atau keyboard) agar bisa dikenali dan digunakan oleh aplikasi.
* Contoh di Windows: Otomatis mencari dan menginstal Driver melalui Windows Update saat perangkat baru dihubungkan.
* Contoh di Linux: Driver perangkat keras biasanya sudah tertanam di dalam sistem (Kernel), sehingga printer atau webcam sering kali bisa langsung dipakai tanpa instalasi tambahan.

5. Manajemen Keamanan (Melindungi Sistem)
Fungsi ini menjaga data dan sistem dari akses pengguna yang tidak sah serta mengatur hak akses setiap orang yang menggunakan komputer tersebut.
* Contoh di Windows: Munculnya jendela konfirmasi User Account Control (UAC) yang meminta izin sebelum aplikasi melakukan perubahan pada sistem.
* Contoh di Linux: Penggunaan perintah sudo yang mengharuskan pengguna memasukkan password admin setiap kali ingin menginstal aplikasi atau mengubah pengaturan sistem.

#### Latihan 1.2
Kapan sebaiknya menggunakan Windows vs Linux vs macOS? Analisis berdasarkan use case: gaming, development, server, creative work, dan enterprise.

Jawab : 
1. Gaming (Bermain Game)
* Terbaik: Windows.
Hampir semua game terbaru dikembangkan khusus untuk Windows. Dukungan driver kartu grafis dan teknologi seperti DirectX 12 menjadikannya pilihan nomor satu untuk performa gaming maksimal.

2. Development (Pemrograman)
* Terbaik: macOS atau Linux.
Keduanya berbasis Unix yang sangat stabil dan ramah bagi programmer. macOS menjadi unggulan karena bisa digunakan untuk membangun aplikasi di semua platform (Android, iOS, Web), sementara Linux unggul karena sangat ringan dan gratis.

3. Server (Pusat Data)
*  Terbaik: Linux.
Sangat efisien dalam penggunaan sumber daya karena bisa dijalankan tanpa antarmuka grafis (hanya teks). Linux dikenal sangat jarang crash dan bisa menyala bertahun-tahun tanpa perlu restart.

4. Creative Work (Desain & Video)
*  Terbaik: macOS.
Sudah menjadi standar industri kreatif karena akurasi warna layarnya yang tinggi. Memiliki aplikasi eksklusif kelas dunia seperti Final Cut Pro untuk editing video dan Logic Pro untuk produksi musik.

5. Enterprise (Kantor & Bisnis)
*  Terbaik: Windows.
Memiliki sistem manajemen terpusat yang sangat matang. Admin IT perusahaan dapat dengan mudah mengontrol ribuan komputer karyawan, mengatur keamanan data, dan pembaruan sistem secara serentak dari satu tempat.

### 1.10.2. Latihan Praktikal
#### Latihan 1.3
Install Ubuntu Server 22.04 LTS di VirtualBox dengan langkah berikut:
1. Download Ubuntu Server ISO dari website resmi
2. Create VM baru di VirtualBox (RAM: 2GB, Disk: 25GB)
3. Install dengan automatic partitioning (guided)
4. Buat user account dengan password yang kuat
5. Reboot dan login ke sistem
6. Dokumentasikan proses instalasi dengan screenshot key steps

Jawab :
1. Karena saya menggunakan ubuntu via wsl maka saya mengunduh ubuntu di microsoft store
    ![](Images/10.2%20Install.png "") 
2. Karena saya menggunakan Ubuntu via WSL, saya tidak perlu membuat VM secara manual. WSL sudah terintegrasi langsung dengan Windows, sehingga alokasi RAM dan Disk dikelola otomatis oleh sistem tanpa perlu aplikasi VirtualBox.
3. Karena saya menggunakan Ubuntu via WSL, langkah partisi tidak perlu dilakukan. Proses instalasi dari Microsoft Store sudah mengatur sistem file secara otomatis dalam bentuk Virtual Hard Disk, jadi kita tinggal pakai saja.
4. ![](Images/10.2%20login.png "") 
5. ![](Images/10.2%20final.png "") 

#### Latihan 1.4
Setelah instalasi Ubuntu Server, lakukan tasks berikut:
1. Update package list: sudo apt update
2. Upgrade packages: sudo apt upgrade
3. Install neofetch: sudo apt install neofetch
4. Jalankan neofetch dan screenshot hasilnya
5. Check disk usage dengan df -h
6. Check memory dengan free -h
7. Dokumentasikan output dari setiap command

Jawab :
1. ![](Images/8.1%201.png "")
2. ![](Images/8.1%202.png "")
3. ![](Images/851.png "") 
4. ![](Images/1.4%20neofetch.png "") 
5. ![](Images/1.7.6%20df.png "")
6. ![](Images/864.png "")

#### Latihan 1.5
Eksplorasi sistem yang baru diinstall:
1. Tampilkan informasi OS: cat /etc/os-release
2. Tampilkan versi kernel: uname -r
3. List partisi: lsblk
4. Check network connectivity: ping -c 4 google.com
5. Install dan jalankan htop untuk melihat resource usage
6. Buat laporan singkat tentang konfigurasi sistem Anda

Jawab :

1. Versi Sistem
* Sistem menggunakan Ubuntu 22.04, yaitu salah satu versi Linux yang stabil dan umum digunakan. Karena berjalan di dalam Windows (WSL2), Linux dapat digunakan secara bersamaan tanpa harus menghapus Windows.

2. Mesin Inti (Kernel)
* Versi mesin sistem yang digunakan adalah versi terbaru untuk standar WSL. Hal ini membuat sistem lebih aman dan mendukung fitur-fitur terbaru dari Microsoft maupun Linux.

3. Kapasitas Penyimpanan
* Hardisk Utama: Tersedia ruang penyimpanan sekitar 400 MB, yang tergolong sangat luas untuk kebutuhan data.
* Memori Tambahan (Swap): Tersedia cadangan sebesar 1 GB untuk membantu kinerja jika memori utama (RAM) mulai penuh.

4. Tes Internet
* Koneksi internet berjalan dengan lancar. Berdasarkan pengujian ke Google, responnya cepat dengan rata-rata waktu 27 milidetik. Hal ini memastikan proses unduh atau pembaruan aplikasi tidak akan mengalami kendala.

5. Kondisi Performa (RAM & CPU)

Kondisi beban kerja sistem saat ini sangat ringan:
* Prosesor (CPU): Hampir tidak ada beban kerja (sangat lega), sehingga performa tetap maksimal.
* Memori (RAM): Baru terpakai sedikit (401 MB) dari total kapasitas 3.7 GB. Sistem masih memiliki banyak ruang untuk menjalankan aplikasi lain tanpa terasa lambat.

Kesimpulan:
Sistem telah terpasang dengan benar, koneksi internet aktif dan stabil, serta performa perangkat secara keseluruhan sangat ringan dan responsif.

### 1.10.3. Latihan Refleksi
#### Latihan 1.6
Ceritakan pengalaman Anda dengan sistem operasi:
1. Sistem operasi apa yang Anda gunakan sehari-hari? (Windows, macOS, Linux, atau lainnya)
2. Berapa lama Anda menggunakan sistem operasi tersebut?
3. Apa yang Anda sukai dari sistem operasi tersebut?
4. Apa tantangan atau masalah yang pernah Anda hadapi?
5. Apakah Anda pernah menggunakan sistem operasi lain? Bandingkan pengalaman Anda.
6. Setelah mempelajari bab ini, apakah ada sistem operasi lain yang ingin Anda coba? Mengapa?
Tulis refleksi Anda dalam 300-500 kata disertai dengan dokumentasi.

Jawab   :
1. Sistem Operasi yang Digunakan
Sistem operasi utama yang digunakan saat ini adalah Windows. OS ini dipilih karena fleksibilitasnya dalam mendukung berbagai jenis aktivitas, mulai dari produktivitas hingga hiburan.
![](Images/1.6%20Wind.png "") 

2. Durasi Penggunaan
Pengalaman menggunakan Windows sudah cukup lama, diperkirakan sudah lebih dari 10 tahun. Selama rentang waktu tersebut, penggunaan sudah mencakup berbagai versi Windows yang ada.

3. Alasan Memilih Windows
Alasan utama bertahan menggunakan Windows adalah tampilannya yang sangat mudah dipahami oleh siapa saja (user-friendly). Selain itu, Windows sangat "gaming-able", artinya hampir semua game populer di dunia pasti bisa berjalan dengan lancar di sistem operasi ini.

4. Kendala atau Masalah yang Pernah Dialami
Masalah yang paling diingat adalah saat memiliki laptop dengan versi Windows yang sudah lama dan tidak bisa lagi diperbarui (update). Kendalanya muncul ketika ada pembaruan sistem yang membuat beberapa software menjadi tidak mendukung (not support). Akibatnya, harus repot mencari versi software lama yang sesuai, bahkan ada beberapa aplikasi penting yang akhirnya tidak bisa dijalankan sama sekali di laptop tersebut.
![](Images/1.6%20BS.jpg "") 

5. Pengalaman Menggunakan Sistem Operasi Lain
Pernah mencoba macOS sebagai perbandingan. Perbedaan yang paling terasa adalah macOS sangat unggul dan nyaman untuk multitasking (membuka banyak aplikasi sekaligus). Selain itu, desain antarmuka macOS terlihat lebih rapi dan elegan. Namun, kekurangannya adalah macOS sangat tidak disarankan untuk gaming, tidak seperti Windows.

6. OS yang Ingin Dipelajari Lebih Dalam
Sistem operasi yang paling ingin dipelajari lebih lanjut adalah macOS. Hal ini dikarenakan masih ada rasa penasaran dan belum sempat mengeksplorasi fitur-fitur teknis di dalamnya secara lebih mendalam.
![](Images/1.6%20macos.jpg "") 