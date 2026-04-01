# Laporan Praktikum Sistem Operasi Jobsheet 4

<h4> Nama   : Muhammad Unggul Satria Adjie <h4>
<h4> NIM    : 254107020040 <h4>
<h4> Kelas  : TI-1G <h4>

## Latihan 1

1.

```
$ cd
$ pwd
$ $ls -al
$ cd .
$ pwd
$ cd ..
$ pwd
$ ls -al
$ cd ..
$ pwd
$ ls -al
$ cd /etc
$ ls -al | more
$ cat passwd
$ cd -
$ pwd
```

![///](Images/ls-al.png"ls -al")
![///](Images/ls-al2.png"ls -al")
![///](Images/ls-al3.png"ls -al")
![///](Images/ls-almore.png"ls -al")
![///](Images/lastPWD.png"ls -al")

2. Lanjutkan penelusuran pohon pada sistem file menggunakan cd, ls, pwd dan cat. Telusuri direktory/bin, /usr/bin, /sbin, /tmp dan/boot.
   ![///](Images/lsbin.png"lsbin.png")
   ![///](Images/lsusrbin.png"lsusrbin.png")
   ![///](Images/lssbin.png"lssbin.png")
   ![///](Images/lstmp.png"lstmp.png")
   ![///](Images/lsboot.png"lsboot.png")

3. Telusuri direktory/dev. Identifikasi perangkat yang tersedia. Identifikasi tty (termninal) Anda (ketik who am 1); siapa pemilih tty Anda (gunakan ls -1)
   ![///](Images/lsdev.png"lsdev.png")

4. Telusuri derectory/proc. Tampilkan isi file interrupts, devices, cpuinfo, meminfo dan uptime menggunakan perintah cat. Dapatkah Anda melihat mengapa directory/proc disebut pseudo-filesystem yang memungkinkan akses ke struktur data kernel ?

   ![///](Images/lsproc.png"lsproc.png")
   ![///](Images/catinterrup.png"catinterrup.png")
   ![///](Images/catdevice.png"catdevice.png")
   ![///](Images/catCpuinfo.png"catCpuinfo.png")
   ![///](Images/catMeminfo.png"catMeminfo.png")
   ![///](Images/catUptime.png"catUptime.png")

5. Ubahlah direktory home ke user lain secara langsung menggunakan ed username.

6. Ubah kembali ke direktory home Anda.

7. Buat subdirektory work dan play.

8. Hapus subdirektory work.

9. Copy file/etc/passwd ke direktory home Anda.

10. Pindahkan ke subirectory play.

11. Ubahlah ke subdirektory play dan buat symbolic link dengan nama terminal yang menunjuk ke perangkat tty. Apa yang terjadi jika melakukan hard link ke perangkat tty?

12. Buatlah file bernama hello.txt yang berisi kata 'hello word". Dapatkah Anda gunakan ep" menggunakan "terminal" sebagai file asal untuk menghasilkan efek yang sama?

13. Copy hello.txt ke terminal. Apa yang terjadi?

14. Masih direktory home, copy keseluruhan direktory play ke direktory bernama work menggunakan symbolic link.

15. Hapus direktory work dan isinya dengan satu perintah
    ![alt text](image.png)
