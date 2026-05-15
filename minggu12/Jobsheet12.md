# Laporan Praktikum Sistem Operasi Jobsheet 12

<h4> Nama   : Muhammad Unggul Satria Adjie <h4>
<h4> NIM    : 254107020040 <h4>
<h4> Kelas  : TI-1G <h4>

## 1.1 Pengenalan systemd
```
# periksa PID 1
ps -p 1 -o pid , comm , args =
# output : 1 systemd / sbin / init ( atau /lib/ systemd / systemd )
# verifikasi versi systemd
systemctl -- version
# analisis waktu boot layanan -- berapa lama tiap layanan butuh start
systemd - analyze
systemd - analyze blame
systemd - analyze blame | head -20
# lihat rantai kritis boot
systemd - analyze critical - chain
```
## Praktek 10.1: Amati Layanan Aktif Saat Boot
1. Lihat semua layanan yang sedang berjalan.
```
systemctl list - units -- type = service -- state = running
# catat berapa banyak layanan yang aktif
```
2. Lihat semua unit service yang ada (aktif maupun tidak).
```
systemctl list - unit - files -- type = service | head -30
# enabled = akan start otomatis saat boot
# disabled = tidak start otomatis , bisa dijalankan manual
# static = tidak bisa di - enable / disable , hanya dipanggil oleh layanan lain
```
3. Analisis waktu boot dan temukan layanan paling lambat.
```
systemd - analyze
systemd - analyze blame | head -15
```

### Tantangan
Identifikasi tiga layanan dengan waktu inisialisasi terlama menggunakan `systemd-analyze`
blame. Gunakan pipeline dari Bab 3 `(| sort -rh | head -3)` untuk mempercepat pencariannya. Untuk setiap layanan, cari tahu fungsinya dengan systemctl cat nama-layanan.
Tuliskan nama layanan, waktu inisialisasinya, dan penjelasan singkat fungsinya.
![ ](Images/ttg%209.1.png "  ")


## 1.2 Mengelola Layanan dengan systemctl
1. 
```
# periksa status lengkap layanan
sudo systemctl status ssh
# mulai layanan yang sedang mati
sudo systemctl start ssh
# hentikan layanan
sudo systemctl stop ssh
# restart : hentikan lalu jalankan ulang
sudo systemctl restart ssh
# reload : muat ulang konfigurasi tanpa mematikan proses utama
sudo systemctl reload ssh
# periksa apakah layanan sedang berjalan ( untuk skrip )
systemctl is - active ssh
# output : active atau inactive
# periksa apakah layanan akan start saat boot
systemctl is - enabled ssh
# output : enabled atau disabled
```
2. 
```
# aktifkan layanan agar start otomatis saat boot
sudo systemctl enable ssh
# aktifkan sekaligus jalankan sekarang juga
sudo systemctl enable -- now ssh
# nonaktifkan auto - start ( layanan masih bisa dijalankan manual )
sudo systemctl disable ssh
# nonaktifkan sekaligus hentikan sekarang
sudo systemctl disable -- now ssh
# blokir total layanan -- tidak bisa dijalankan manual maupun otomatis
sudo systemctl mask cups
# untuk membuka blokir :
sudo systemctl unmask cups
```
3. 
```
# semua unit service yang sedang berjalan
systemctl list - units -- type = service -- state = running
# semua unit yang gagal
systemctl -- failed
# semua unit service dan status boot mereka
systemctl list - unit - files -- type = service
# lihat dependensi sebuah layanan
systemctl list - dependencies ssh
```
## Praktek 10.2: Kelola Layanan SSH
1. Periksa status SSH secara menyeluruh.
```
systemctl status ssh
systemctl is - active ssh
systemctl is - enabled ssh
```
2. Lakukan restart dan pantau perubahannya.
```
sudo systemctl restart ssh
systemctl status ssh
# perhatikan : Loaded , Active , dan Main PID bisa berubah setelah restart
```
3. Lihat dependensi SSH.
```
systemctl list - dependencies ssh
# layanan lain yang harus aktif sebelum SSH bisa berjalan
```
4. Cek semua unit yang gagal di sistem
```
systemctl -- failed
# jika ada , ini adalah daftar layanan yang butuh perhatian
```
![ ](Images/9.2.2.png "  ")
### Tantangan
Buat skrip Bash (referensi Bab 7) bernama `cek-layanan.sh` yang memeriksa status daftar layanan
dari sebuah berkas teks. Berkas teks `daftar-layanan.txt` berisi satu nama layanan per baris (isi
minimal: ssh, cron, rsyslog). Skrip membaca setiap nama layanan, memeriksa statusnya dengan
`systemctl is-active`, lalu menulis laporan ke berkas `laporan-layanan.log` dengan format:
[TANGGAL] nama-layanan: ACTIVE/INACTIVE. Gunakan date untuk mendapatkan tanggal.
![ ](Images/ttg%209.2.png "  ")

## 1.3 Membuat Berkas Unit Kustom
1. 
```
[ Unit ]
Description = Nama Deskriptif Layanan
Documentation = man : nama - program (8)
After = network . target
Wants = network - online . target
[ Service ]
Type = simple
User = www - data
Group = www - data
WorkingDirectory =/ opt / aplikasi
ExecStart =/ usr / local / bin / aplikasi -- port 8080
ExecReload =/ bin / kill - HUP $MAINPID
Restart = on - failure
RestartSec =5 s
StandardOutput = journal
StandardError = journal
[ Install ]
WantedBy = multi - user . target
```
## Praktek 10.3: Buat Layanan Sederhana dari Skrip Bash
1. Siapkan konten yang akan dilayani.
```
cd ~/ lab - os / chapter10 - services
mkdir -p situs - demo
nano situs - demo / index . html
# Tulis isi berkas berikut
<! DOCTYPE html >
< html >
< body >
<h1 > Halo dari layanan systemd kustom ! </ h1 >
<p > Layanan ini dibuat pada praktek Bab 10. </ p >
</ body >
</ html >
```
2. Buat skrip wrapper untuk server HTTP
```
nano ~/ lab - os / chapter10 - services / jalankan - server . sh
# Tulis isi berkas berikut
#!/ bin / bash
DIREKTORI =" $HOME /lab -os/ chapter10 - services /situs - demo "
PORT =9090
echo " Memulai server di port $PORT ..."
exec python3 -m http . server $PORT -- directory " $DIREKTORI "
chmod + x ~/ lab - os / chapter10 - services / jalankan - server . sh
```
3. Buat berkas unit systemd untuk layanan ini.
```
nano ~/ lab - os / chapter10 - services / demo - web . service
# Tulis isi berkas berikut
[ Unit ]
Description = Demo Web Server Praktek Bab 10
After = network . target
[ Service ]
Type = simple
User = nama - pengguna - kamu
WorkingDirectory =/ home / nama - pengguna - kamu / lab - os / chapter10 - services /
situs - demo
ExecStart =/ usr / bin / python3 -m http . server 9090
Restart = on - failure
RestartSec =3 s
[ Install ]
WantedBy = multi - user . target
# salin ke lokasi unit systemd
sudo cp ~/ lab - os / chapter10 - services / demo - web . service / etc / systemd /
system / demo - web . service
# minta systemd membaca ulang berkas unit yang baru dibuat
sudo systemctl daemon - reload
```
4. Jalankan layanan dan verifikasi.
```
sudo systemctl start demo - web
systemctl status demo - web
# coba akses layanan
curl http :// localhost :9090
```
5. Uji fitur restart otomatis.
```
# lihat PID proses saat ini
systemctl status demo - web | grep " Main PID"
# hentikan proses secara paksa ( simulasi crash )
sudo kill -9 $ ( systemctl show demo - web -- property = MainPID -- value )
# tunggu beberapa detik lalu cek -- systemd harus menghidupkannya
kembali
sleep 5
systemctl status demo - web
# PID akan berubah karena proses baru dijalankan
```
6. Bersihkan layanan uji setelah selesai.
```
sudo systemctl disable -- now demo - web
sudo rm / etc / systemd / system / demo - web . service
sudo systemctl daemon - reload
```
![ ](Images/9.3A.png "  ")
### Tantangan:
Modifikasi berkas unit demo-web.service sebelum menghapusnya: tambahkan
`RestartSec=10s` agar sistemmenunggu 10 detik sebelum mencoba restart, dan tambahkan
`Environment="PORT=9091"` lalu ubah ExecStart agar menggunakan variabel tersebut. Aktifkan
layanan dengan enable dan W`antedBy=multi-user.target`, lalu uji apakah layanan aktif
setelah `systemctl daemon-reload`. Dokumentasikan perbedaan perilaku dibanding versi
sebelumnya
![ ](Images/9.3A.png "  ")

## 1.4 Logging dengan journalctl
1. 
```
# semua log sejak boot saat ini
journalctl -b
# log untuk unit / layanan tertentu
journalctl -u ssh
journalctl -u ssh -b
# -b membatasi ke log sejak boot saat ini saja
# N baris terakhir
journalctl -u ssh -n 50
# ikuti log secara real - time ( seperti tail -f)
journalctl -u ssh -f
# filter berdasarkan waktu
journalctl -u ssh -- since "1 hour ago"
journalctl -u nginx -- since " 2024 -01 -15 08:00 " -- until " 2024 -01 -15 10:00 "
journalctl -u ssh -- since today
# filter berdasarkan prioritas
# 0= emerg , 1= alert , 2= crit , 3= err , 4= warning , 5= notice , 6= info , 7= debug
journalctl -p err
journalctl -u ssh -p warning
# tampilkan tanpa pager ( berguna untuk pipeline )
journalctl -u ssh --no - pager | grep " Failed "
```
2. 
```
# buat direktori penyimpanan journal permanen
sudo mkdir -p / var / log / journal
sudo systemd - tmpfiles -- create -- prefix / var / log / journal
# restart journald untuk menerapkan perubahan
sudo systemctl restart systemd - journald
# verifikasi -- sekarang ada direktori dengan nama mesin
ls / var / log / journal /
```
3. 
```
# lihat berapa banyak ruang yang dipakai journal
journalctl -- disk - usage
# hapus log lama ( pertahankan 1 minggu saja )
sudo journalctl -- vacuum - time =7 d
# hapus log lama ( pertahankan maksimal 500 MB)
sudo journalctl -- vacuum - size =500 M
```

## Praktek 10.4: Filter dan Analisis Log Layanan
1. Lihat log SSH dari satu jam terakhir
```
journalctl -u ssh -- since "1 hour ago" --no - pager
# jika tidak ada log SSH , gunakan layanan lain yang aktif
journalctl -u cron -- since "1 hour ago" --no - pager
```
2. Filter log berprioritas error ke atas.
```
journalctl -b -p err --no - pager
# ini menampilkan semua error dan yang lebih serius sejak boot
```
3. Ikuti log secara real-time sambil memicu aktivitas.
```
# jalankan di terminal pertama :
journalctl -u ssh -f
# di terminal kedua , coba login SSH ke localhost
# ssh localhost
# lalu lihat apa yang muncul di terminal pertama
```
4. Ekstrak log ke berkas untuk analisis.
```
cd ~/ lab - os / chapter10 - services
# simpan semua log layanan ssh dari hari ini ke berkas
journalctl -u ssh -- since today --no - pager > log - ssh - hari - ini . txt
# hitung jumlah baris log
wc -l log - ssh - hari - ini . txt
# jika ada , cari baris yang mengandung kata " error " atau " failed "
grep -i " error \| failed " log - ssh - hari - ini . txt | head -20
```
![ ](Images/9.3B.png "  ")
### Tantangan
Ekstrak semua log dengan prioritas error (-p err) dari 24 jam terakhir untuk layanan SSH, simpan
ke berkas `error-ssh-24jam.txt`. Gunakan pipeline dari Bab 3 untuk menghitung total jumlah
baris error dengan `wc -l`, lalu tampilkan 10 pesan error yang paling sering muncul menggunakan
`sort | uniq -c | sort -rn | head -10`. Tuliskan perintah lengkap yang kamu gunakan.
![ ](Images/9.3B%20ttg.png "  ")

## 1.5 Konfigurasi Layanan Jaringan
1. 
```
# pastikan SSH server terinstal
sudo apt install -y openssh - server
# periksa status
systemctl status ssh
# pastikan SSH aktif dan mulai otomatis saat boot
sudo systemctl enable -- now ssh
# verifikasi port 22 mendengarkan
ss - tlnp | grep :22
# atau
netstat - tlnp | grep :22
```
2. 
```
# lihat konfigurasi aktif ( bukan komentar )
grep -v "^#" / etc / ssh / sshd_config | grep -v "^$"
# opsi yang sering dikonfigurasi :
# Port 22 -- port yang didengarkan ( ubah untuk keamanan )
# PermitRootLogin no -- larang login langsung sebagai root
# PasswordAuthentication yes -- izinkan login dengan password ( ubah ke no
untuk keamanan lebih tinggi )
# PubkeyAuthentication yes -- izinkan login dengan kunci publik
# MaxAuthTries 3 -- maksimum percobaan login sebelum koneksi
ditolak
# AllowUsers alice bob -- hanya pengguna tertentu yang boleh SSH
```
3. 
```
# 1. buat backup konfigurasi sebelum mengubah
sudo cp / etc / ssh / sshd_config / etc / ssh / sshd_config . backup
# 2. ubah konfigurasi dengan editor
sudo nano / etc / ssh / sshd_config
# 3. validasi sintaks -- JANGAN skip langkah ini
sudo sshd -t
# jika tidak ada output , sintaks benar
# jika ada pesan error , perbaiki dulu sebelum lanjut
# 4. muat ulang konfigurasi ( lebih aman dari restart )
sudo systemctl reload ssh
# atau restart jika diperlukan
sudo systemctl restart ssh
# 5. verifikasi layanan masih berjalan
systemctl status ssh
```
4. 
```
# lihat semua port TCP yang mendengarkan
ss - tlnp
# -t: TCP saja
# -l: hanya yang sedang listen ( mendengarkan )
# -n: tampilkan angka ( jangan resolve nama )
# -p: tampilkan proses yang menggunakan socket
# cari port SSH secara spesifik
ss - tlnp | grep ssh
```
## 
1. Tujuan: menerapkan kebijakan umur password dan mengamati efeknya.
```
# set aging policy untuk userA
sudo chage -M 60 -W 7 -m 1 userA
sudo chage -l userA
# paksa userA ganti password saat login pertama
sudo chage -d 0 userA
# kunci password userB
sudo passwd -l userB
sudo passwd -S userB
# unlock kembali
sudo passwd -u userB
sudo passwd -S userB
```
2. Buat backup dan ubah port SSH.
```
sudo cp / etc / ssh / sshd_config / etc / ssh / sshd_config . backup . lab12
# ubah port dari 22 ke 2222 ( atau port lain yang belum dipakai )
sudo sed -i 's /^# Port 22/ Port 2222/ ' / etc / ssh / sshd_config
# jika baris Port 22 tidak dikomentari :
# sudo sed -i 's/^ Port 22/ Port 2222/ ' /etc/ssh/ sshd_config
# verifikasi perubahan
grep "^ Port " / etc / ssh / sshd_config
```
3. Validasi konfigurasi dan restart layanan.
```
# WAJIB : validasi sintaks sebelum restart
sudo sshd -t
echo " Kode keluar sshd -t: $?"
# kode 0 berarti sintaks valid
# restart layanan
sudo systemctl restart ssh
systemctl status ssh
```
4. Verifikasi port baru dengan ss.
```
ss - tlnp | grep ssh
# seharusnya menampilkan port 2222 , bukan 22
# simpan hasil ke berkas bukti
ss - tlnp | grep ssh > ~/ lab - os / chapter10 - services / bukti - port - ssh . txt
cat ~/ lab - os / chapter10 - services / bukti - port - ssh . txt
```
5. Kembalikan port SSH ke 22 setelah praktek.
```
sudo cp / etc / ssh / sshd_config . backup . lab12 / etc / ssh / sshd_config
sudo sshd -t
sudo systemctl restart ssh
ss - tlnp | grep ssh
# harus kembali ke port 22
```
![ ](Images/9.3C%20.png "  ")
### Tantangan
Ubah konfigurasi SSH untuk menambahkan dua pengaturan keamanan: PermitRootLogin no
(larang login root langsung) dan MaxAuthTries 3 (maksimal tiga kali percobaan). Lakukan
dengan urutan yang aman: backup, edit, validasi dengan `sshd -t, reload`. Verifikasi perubahan
dengan `grep -E "PermitRoot|MaxAuth" /etc/ssh/sshd_config`. Kemudian periksa log
SSH untuk memastikan tidak ada error setelah perubahan dengan `journalctl -u ssh -n 20`.
Referensi Bab 2 untuk penggunaan ss dan Bab 9 untuk keamanan pengguna.
![ ](Images/9.3c%20ttg.png "  ")

# 1.7 Latihan
## Latihan 10.1 Audit Layanan dan Analisis Boot
1. Jalankan systemctl list-units –type=service –state=running dan catat semua
layanan aktif. Pilih tiga layanan yang kamu kenal, periksa status masing-masing dengan
systemctl status, dan jelaskan fungsinya.
2. Jalankan systemd-analyze blame dan identifikasi lima layanan dengan waktu inisialisasi
terlama. Tampilkan hasilnya menggunakan pipeline: systemd-analyze blame | head -5.
3. Jalankan systemctl –failed dan dokumentasikan hasilnya. Jika ada layanan yang gagal, cari
tahu penyebabnya dengan journalctl -u nama-layanan -n 30.
![ ](Images/9.4.1.png "  ")

## Latihan 10.2 Layanan Kustom dengan Restart Otomatis
1. Buat skrip Bash (referensi Bab 7) bernama monitor-disk.sh yang setiap 30 detik menuliskan
penggunaan disk ke berkas log. Gunakan df -h dan date.
2. Buat berkas unit /etc/systemd/system/monitor-disk.service untuk menjalankan skrip
tersebut dengan konfigurasi: Restart=always, RestartSec=5s, dan berjalan sebagai pengguna kamu sendiri.
3. Aktifkan dan jalankan layanan. Verifikasi dengan systemctl status dan pastikan log masuk
ke journal.
4. Simulasikan crash dengan membunuh proses secara paksa (kill -9), tunggu 10 detik, dan
verifikasi bahwa layanan hidup kembali secara otomatis.
5. Bersihkan: nonaktifkan layanan dan hapus berkas unit setelah selesai.
![ ](Images/9.4%20ttg1.png "  ")

## Latihan 10.3 Investigasi Log dan Keamanan SSH
1. Gunakan journalctl -b -p err untuk menemukan semua error sejak boot terakhir. Simpan hasilnya ke berkas dan hitung jumlah baris dengan wc -l.
2. Lakukan tiga perubahan keamanan pada /etc/ssh/sshd_config: tambahkan PermitRootLogin
no, MaxAuthTries 3, dan LoginGraceTime 30. Ikuti alur aman: backup, edit, validasi sshd
-t, reload.
3. Setelah reload, verifikasi tiga hal: layanan masih berjalan (systemctl status ssh), port
masih mendengarkan (ss -tlnp | grep ssh), dan konfigurasi baru terbaca (grep -E
"PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config).
4. Kembalikan konfigurasi SSH ke kondisi semula menggunakan berkas backup.
![ ](Images/9.4%20ttg2.png "  ")