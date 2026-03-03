# Laporan Praktikum Sistem Operasi Jobsheet 2

<h4> Nama   : Muhammad Unggul Satria Adjie <h4>
<h4> NIM    : 254107020040 <h4>
<h4> Kelas  : TI-1G <h4>

## 1.1.3 Filosofi Ini Diterapkan
1. Contoh Membaca Informasi CPU:
```    
cat / proc / cpuinfo
```
cat / proc / meminfo
```
```
![CPU Information](Images/infomemori.png"cpu")
![CPU Information](Images/contoh.png"cpu")

## 1.2.3 Melihat File Descriptor
1. Melihat FIle Descriptor:
```
ls -l / proc / $$ / fd
```
![FIle Descriptor](Images/1.2.5fileDirectory.png"FIleDescriptor")

## Standart Input
1. Membaca Input dari file.txt
```
sort < file . txt
```
![Input dari file.txt](Images/1.3.3inputFile.png"FIle.txt")
2. Menghitung jumlah baris dari file
```
wc -l < / etc / passwd
```
![menghitung jumlah baris dari file](Images/1.3.3daftar.png"FIle.txt")

3. Contoh Penggunaan
```
Mengirim email dengan heredoc
cat << EOF
To : user@example . com
Subject : Test Email
Ini adalah isi email .
Baris kedua .
EOF
# Membuat file konfigurasi
cat << 'EOF ' > config . txt
server = localhost
port =8080
EOF
```
![Percobaan](Images/1.4peprcobaanEmail.png"percobaan")

## 1.3.5 Standar Error (stderr)
1. Menyimpan error ke file
```
find / - name "*. conf " 2 > errors . txt
```
![///](Images/"///")
2. Membuang error
```
grep " pattern " file . txt 2 > / dev / null
```
![///](Images/"///")
3. Append error ke file
```
command 2 > > error - log . txt
```
![///](Images/"///")

## 1.4 Menggabungkan stdout dan stderr
1. Mengarahkan stderr ke stdout (Metode Lama)
```
command > output.txt 2>&1
```
2. Mengarahkan keduanya secara ringkas (Metode Modern)
```
command &> output.txt
```
3. Menambahkan (Append) keduanya ke file
```
command &>> output.txt
```

## 1.5 Pipeline ( | )
1. Menghubungkan output perintah pertama ke input perintah kedua
```
cat /etc/passwd | cut -d: -f1 | sort
```
2. Mencari proses tertentu dan menghitung jumlahnya
```
ps aux | grep "python" | wc -l
```
## 1.6 Perintah tee
1. Menampilkan output di terminal sekaligus menyimpannya ke file
```
ls -l | tee daftar_file.txt
```
2. Menambahkan output ke file tanpa menghapus isi sebelumnya (Append)
```
uptime | tee -a log_sistem.txt
```
## 1.7 Xargs: Mengonversi Output Menjadi Argumen
1. Menghapus file berdasarkan daftar yang ditemukan
```
find . -name "*.tmp" | xargs rm
```
2. Membatasi jumlah argumen per pemanggilan perintah
```
cat daftar_web.txt | xargs -n 1 curl -I
```
## 1.8 Substitusi Perintah (Command Substitution)
1. Menggunakan backticks ( ` ) atau $( )

```
echo "Hari ini adalah $(date)"
```
2. Menghitung jumlah file dalam variabel
```
FILE_COUNT=$(ls | wc -l)
echo "Ada $FILE_COUNT file di direktori ini"
```
## 1.11 Latihan
1. Latihan 3.1: Daftar 10 File Terbesar
Menampilkan 10 file terbesar di /var/log/
```
sudo du -ah /var/log/ | sort -rh | head -n 10 | tee large-logs.txt 2> error.log
```
![Latihan3.1](Images/Latihan3.1.png"Latihan3.1")
2. Latihan 3.2: Ekstrak Username dari /etc/passwd
```
cut -d: -f1 /etc/passwd | sort > sorted-users.txt

cut -d: -f1 /etc/passwd | sort | tee sorted-users.txt
```
![Latihan3.2](Images/Latihan3.2.png"Latihan3.2")
3. Latihan 3.3: Script Monitoring CPU dan Memory
```
for i in {1..12}; do
    echo "--- $(date) ---" | tee -a monitor.log
    top -bn1 | grep "Cpu(s)" | tee -a monitor.log
    free -m | tee -a monitor.log
    sleep 5
done
```
![Latihan3.3](Images/Latihan3.3.png"Latihan3.3")
4. Latihan 3.4: Mencari File Konfigurasi
```
find / -name "*.conf" 2> /dev/null | tee list_conf.txt | wc -l

find /etc -name "*.conf" 2> /dev/null | tee list_conf.txt | wc -l
```
![Latihan3.4](Images/Latihan3.4.png"Latihan3.4")
5. Latihan 3.5: Script Backup dengan Log
```
tar -cvf backup.tar ~/Documents 2> >(while read line; do echo "$(date): $line"; done > backup-error.log) | while read line; do echo "$(date): $line"; done | tee backup-success.log

# Perintah backup dengan pemisahan log dan timestamp
tar -cvf backup.tar ~/Documents > >(while read line; do echo "$(date): $line"; done | tee backup-success.log) 2> >(while read line; do echo "$(date): $line"; done | tee backup-error.log)
```
![Latihan3.5](Images/Latihan3.5.png"Latihan3.5")