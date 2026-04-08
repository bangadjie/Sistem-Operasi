# Laporan Praktikum Sistem Operasi Jobsheet 6

<h4> Nama   : Muhammad Unggul Satria Adjie <h4>
<h4> NIM    : 254107020040 <h4>
<h4> Kelas  : TI-1G <h4>

## Praktikum 6.1 — Mengenali Bash dan Menyiapkan
1. Lihat shell login dan shell aktif saat ini:
```
echo " Shell login : $SHELL "
echo " Shell aktif : $0"
bash -- version | head -n 1
```
Kode 1.1: Melihat shell login dan shell aktif
2. Lihat proses shell yang sedang berjalan:
```
echo $$
ps -p $$ -o pid , ppid , args =
```
Kode 1.2: Melihat proses shell aktif
2 1 Bash Shell dan Shell Basic
1.1 Pengenalan Bash sebagai Shell Default di Linux
3. Buat workspace praktikum:
```
mkdir -p ~/ praktikum - os / week07 - bash /{ bin , backup , logs ,
sampel , ruang - nama }
cd ~/ praktikum - os / week04 - bash
pwd
```
Kode 1.3: Membuat workspace Bash
4. Buat beberapa file contoh yang akan dipakai pada praktikum berikutnya:
```
touch sample - app . conf
touch logs / app -01. log logs / app -02. log logs / app -03. log
touch sampel / catatan - a . txt sampel / catatan - b . txt
touch sampel / backup -01. tar sampel / backup -02. tar
touch sampel / laporan - harian . log sampel / laporan -
mingguan . log sampel / laporan - bulanan . log
touch "ruang - nama / laporan server april .txt"
touch "ruang - nama / backup [ mingguan ] server . conf "
ls -R
```
Kode 1.4: Membuat file contoh untuk praktikum lanjutan

## Praktikum 6.2 — Membuat Ringkasan Sesi Terminal
1. Masuk ke workspace praktikum:
```
cd ~/ praktikum - os / week04 - bash
```
Kode 1.5: Masuk ke workspace praktikum
2. Simpan informasi sesi terminal ke file laporan:
```
{
echo "=== RINGKASAN SESI BASH ==="
date
echo " User : $( whoami )"
echo " Hostname : $( hostname )"
echo " Shell login : $SHELL "
echo " Shell aktif : $0"
1 Bash Shell dan Shell Basic 3
1.2 Konfigurasi Bash (.bashrc dan .bash_profile)
echo "PID shell : $$"
echo " Direktori : $(pwd)"
} | tee session - info . txt
```
Kode 1.6: Membuat ringkasan sesi terminal
3. Verifikasi isi file laporan:
```
cat session - info . txt
```
Kode 1.7: Membaca ringkasan sesi
## Praktikum 6.3 — Menambahkan Konfigurasi Aman pada .bashrc
Tujuan: memahami peran .bashrc dan menerapkan perubahan secara aman.
1. Lihat file konfigurasi Bash pada home directory:
```
ls - la ~ | grep -E 'bashrc | bash_profile | profile '
```
Kode 1.8: Melihat file konfigurasi Bash
2. Buat backup .bashrc:
```
cp ~/. bashrc ~/. bashrc . bak - praktikum
```
Kode 1.9: Backup .bashrc
3. Tambahkan blok konfigurasi praktikum:
```
cat <<'EOF ' >> ~/. bashrc
# --- Praktikum Bash Shell ---
export PRAKTIKUM_BASH_DIR =" $HOME / praktikum -os/week04 -
bash "
export EDITOR = nano
# --- End Praktikum Bash Shell ---
EOF
```
Kode 1.10: Menambahkan konfigurasi ke .bashrc
4. Terapkan konfigurasi tanpa logout:
```
source ~/. bashrc
echo " $PRAKTIKUM_BASH_DIR "
echo " $EDITOR "
```
Kode 1.11: Memuat ulang .bashrc
## Praktikum 6.4 — Menyiapkan .bash_profile untuk Shell Login
1. Backup .bash_profile jika sudah ada:
```
[ -f ~/. bash_profile ] && cp ~/. bash_profile ~/.
bash_profile . bak - praktikum
```
Kode 1.12: Backup .bash_profile jika tersedia
2. Tambahkan konfigurasi login shell:
cat <<'EOF ' >> ~/. bash_profile
### --- Praktikum Bash Login Shell ---
```
if [ -f ~/. bashrc ]; then
. ~/. bashrc
fi
echo " Login Bash pada $( date '+%F %T ')" >> " $HOME /
praktikum -os/week07 - bash /login - audit .log"
```
### --- End Praktikum Bash Login Shell ---
EOF
Kode 1.13: Menambahkan konfigurasi ke .bash_profile
3. Uji dengan membuka login shell baru:
```
bash -l
tail -n 3 ~/ praktikum - os / week07 - bash / login - audit . log
exit
```
Kode 1.14: Menguji .bash_profile dengan login shell

## Praktikum 6.5 — Membedakan Variabel Shell dan Environment Variable
1. Buat variabel lokal:
```
KELAS_OS =" Sistem Operasi A"
echo " $KELAS_OS "
```
Kode 1.15: Membuat variabel shell lokal
2. Buka subshell dan cek apakah variabel masih ada:
```
bash
echo " $KELAS_OS "
exit
```
Kode 1.16: Menguji variabel lokal pada subshell
3. Sekarang ubah menjadi environment variable:
```
export KELAS_OS =" Sistem Operasi A"
bash
echo " $KELAS_OS "
exit
```
Kode 1.17: Membuat environment variable
4. Lihat isi PATH dan lokasi beberapa perintah:
```
echo " $PATH "
which bash
type ls
```
Kode 1.18: Melihat PATH dan lokasi perintah
## Praktikum 6.6 — Menambahkan Direktori Script Pribadi ke PATH
1. Pastikan direktori bin praktikum tersedia:
```
mkdir -p ~/ praktikum - os / week07 - bash / bin
```
Kode 1.19: Menyiapkan direktori bin
2. Tambahkan direktori tersebut ke PATH melalui .bashrc:
cat <<'EOF ' >> ~/. bashrc
### --- Praktikum PATH ---
```
export PATH =" $HOME / praktikum -os/week07 - bash /bin : $PATH "
```
### --- End Praktikum PATH ---
```
EOF
source ~/. bashrc
echo " $PATH "
```
Kode 1.20: Menambahkan direktori bin ke PATH
3. Buat script ringkasan sistem:
```
cat <<'EOF ' > ~/ praktikum - os / week07 - bash / bin / ringkas -
sistem
#!/ usr/bin/env bash
echo " Hostname : $( hostname )"
echo " User : $( whoami )"
echo " Uptime : $( uptime -p)"
echo " Disk / :"
df -h /
EOF
chmod + x ~/ praktikum - os / week07 - bash / bin / ringkas - sistem
```
Kode 1.21: Membuat script ringkas-sistem
4. Jalankan script dari direktori yang berbeda:
```
cd ~
ringkas - sistem
```
Kode 1.22: Menjalankan script dari sembarang lokasi
## Praktikum 6.7 — Membuat Alias Produktivitas Dasar
1. Tambahkan alias ke .bashrc:
cat <<'EOF ' >> ~/. bashrc
### --- Praktikum Alias ---
```
alias ll ='ls -lah --color = auto '
alias hist10 ='history | tail -n 10 '
alias cdbashlab ='cd $HOME / praktikum -os/week04 - bash '
```
### --- End Praktikum Alias ---
```
EOF
source ~/. bashrc
```
Kode 1.23: Menambahkan alias ke .bashrc
2. Uji alias:
```
ll
hist10
cdbashlab
pwd
type ll
Kode 1.24: Menguji alias
```
## Praktikum 6.8 — Membuat Fungsi Backup Konfigurasi
1. Siapkan file konfigurasi contoh:
```
echo " PORT =8080 " > ~/ praktikum - os / week07 - bash / sample -
app . conf
cat ~/ praktikum - os / week07 - bash / sample - app . conf
```
Kode 1.25: Membuat file konfigurasi contoh
2. Tambahkan fungsi ke .bashrc:
```
cat <<'EOF ' >> ~/. bashrc
```
### --- Praktikum Fungsi Shell ---
```
backup_conf () {
if [ $# -ne 1 ]; then
echo " Usage : backup_conf <file >"
return 1
fi
local src ="$1"
local dst =" $HOME / praktikum -os/week07 - bash / backup "
if [ ! -f " $src " ]; then
echo " File tidak ditemukan : $src "
return 2
fi
mkdir -p " $dst "
cp -- " $src " " $dst /$( basename " $src ").$( date +%F -%H%
M%S).bak"
echo " Backup selesai di $dst "
}
```
### --- End Praktikum Fungsi Shell ---
```
EOF
source ~/. bashrc
```
Kode 1.26: Menambahkan fungsi backup_conf ke .bashrc
3. Uji fungsi:
```
backup_conf ~/ praktikum - os / week07 - bash / sample - app . conf
ls - lah ~/ praktikum - os / week07 - bash / backup
type backup_conf
```
Kode 1.27: Menguji fungsi backup_conf
## Praktikum 6.9 — Menggunakan Completion Dasar dan Melihat History
1. Pastikan file contoh tersedia:
```
cd ~/ praktikum - os / week07 - bash / sampel
touch laporan - harian . log laporan - mingguan . log laporan -
bulanan . log
ls
```
Kode 1.28: Menyiapkan file untuk completion
2. Uji completion file:
a) Ketik cat lap lalu tekan Tab dua kali.
b) Amati daftar file yang memiliki prefix lap.
c) Ketik lebih spesifik, misalnya cat laporan-h lalu tekan Tab.
3. Jalankan beberapa perintah sederhana:
```
pwd
ls - lah
date
whoami
history | tail -n 10
```
Kode 1.29: Menjalankan beberapa perintah untuk history
## PPraktikum 6.10 — Menelusuri Perintah Diagnostik dengan History
1. Jalankan beberapa perintah diagnostik:
```
df -h
free -h
uptime
ps aux | head
```
Kode 1.30: Menjalankan perintah diagnostik
2. Cari ulang perintah diagnostik dari history:
```
history | grep -E 'df -h| free -h| uptime |ps aux '
```
Kode 1.31: Mencari perintah dari history
3. Jalankan ulang salah satu perintah berdasarkan nomor history:
```
! < NOMOR_HISTORY_ANDA >
```
Kode 1.32: Menjalankan ulang perintah dari history
4. Simpan potongan history ke file dokumentasi:
```
history | tail -n 20 > ~/ praktikum - os / week07 - bash / diag
- history . txt
cat ~/ praktikum - os / week07 - bash / diag - history . txt
```
Kode 1.33: Menyimpan history ke file

## Praktikum 6.11 — Mencoba Wildcard Dasar
1. Masuk ke direktori sampel:
```
cd ~/ praktikum - os / week07 - bash / sampel
ls
```
Kode 1.34: Masuk ke direktori sampel
2. Coba beberapa pola wildcard:
```
ls *. log
ls catatan -?. txt
ls backup -0[12]. tar
```
Kode 1.35: Mencoba wildcard dasar
3. Coba beberapa ekspansi lain:
```
echo log -{ pagi , siang , malam }. txt
echo ~
echo ~/ praktikum - os / week04 - bash
```
Kode 1.36: Mencoba brace dan tilde expansion

## Praktikum 6.12 — Mengarsipkan Banyak Log Sekaligus
1. Siapkan file log tambahan:
```
cd ~/ praktikum - os / week07 - bash / logs
touch access -01. log access -02. log access -03. log
ls
```
Kode 1.37: Menyiapkan file log tambahan
2. Preview file yang akan diproses:
```
echo *. log
echo access -0?. log
```
Kode 1.38: Preview hasil wildcard
3. Pindahkan semua file log ke folder arsip:
```
mkdir -p arsip - log
mv *. log arsip - log /
ls arsip - log
```
Kode 1.39: Memindahkan file log ke folder arsip
4. Kompres folder arsip:
```
tar - czf arsip - log - $ ( date +% F ) . tar . gz arsip - log
ls - lah
```
Kode 1.40: Membuat arsip terkompresi
## Praktikum 6.13 — Membedakan Single Quote, Double Quote, dan Escape
1. Uji single quote dan double quote:
```
echo '$USER bekerja di $HOME '
echo " $USER bekerja di $HOME "
```
Kode 1.41: Menguji perbedaan jenis quote
2. Uji escape karakter spasi:
```
cd ~/ praktikum - os / week07 - bash / ruang - nama
ls laporan \ server \ april . txt
```
Kode 1.42: Menggunakan escape pada path
3. Uji akses file yang sama dengan double quote:
```
cat " laporan server april .txt"
```
Kode 1.43: Menggunakan double quote pada path

## Praktikum 6.14 — Menangani File dengan Nama Sulit Secara Aman
1. Pastikan file target tersedia:
```
cd ~/ praktikum - os / week07 - bash / ruang - nama
ls - lah
```
Kode 1.44: Memastikan file target tersedia
2. Salin file dengan nama kompleks ke folder backup:
```
cp -- " backup [ mingguan ] server . conf " \
" $HOME / praktikum -os/week07 - bash / backup /backup -
mingguan - server . conf "
```
Kode 1.45: Menyalin file dengan nama kompleks secara aman
3. Gunakan variabel untuk memproses path dengan aman:

```
file_asli =" $HOME / praktikum -os/week07 - bash /ruang - nama /
backup [ mingguan ] server . conf "
file_salinan =" $HOME / praktikum -os/week07 - bash / backup /
backup - mingguan -server -v2. conf "
cp -- " $file_asli " " $file_salinan "
ls - lah " $HOME / praktikum -os/week07 - bash / backup "
```
Kode 1.46: Menggunakan variabel berisi path dengan aman
4. Tampilkan daftar file hasil backup:
```
for file in " $HOME "/ praktikum - os / week07 - bash / backup /*;
do
printf 'Hasil backup : %s\n' " $file "
done
```
Kode 1.47: Menampilkan file hasil backup dengan aman
## 1.8 Tugas Praktikum

## Tugas Praktikum 1 — Toolkit Bash Administrator Pribadi
## Tugas Praktikum 2 — Audit File Konfigurasi dan Logging Aman
## Tugas Praktikum 3 — Mini Health Check Harian Server
## Tugas Praktikum 4 — Penanganan File dengan Nama Kompleks dan Arsip Aman