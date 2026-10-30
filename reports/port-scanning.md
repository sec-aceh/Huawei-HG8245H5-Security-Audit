# Analisis Enumerasi Port

## Perintah yang tercatat

```bash
nmap -v -A -sV ROUTER_IP
nmap -sV -sC -p 23,53,80 ROUTER_IP
```

Catatan: `-A` sudah mengaktifkan beberapa fitur deteksi lanjutan, termasuk deteksi versi, skrip default, deteksi OS, dan traceroute. Menambahkan `-sV` pada perintah pertama bersifat redundan.

## Hasil yang dilaporkan

| Port | Status | Layanan yang teridentifikasi | Catatan |
|---|---|---|---|
| 23/tcp | open | Telnet / Huawei gateway Telnet daemon | Menambah permukaan serangan lokal |
| 53/tcp | open | domain (DNS) | Belum diuji sebagai open resolver |
| 80/tcp | open | ssl/http | Antarmuka manajemen menggunakan TLS pada port non-standar |

Pada pemindaian awal, Nmap juga melaporkan 997 port lain sebagai `filtered`. Status `filtered` berarti Nmap tidak dapat memastikan apakah port tersebut terbuka atau tertutup karena paket kemungkinan disaring.

## Interpretasi

- **TCP/23:** Port terbuka bukan bukti bahwa login berhasil atau bahwa perangkat dapat dieksploitasi. Rekomendasi: nonaktifkan Telnet jika tidak diperlukan, atau batasi aksesnya.
- **TCP/53:** Port DNS terbuka tidak membuktikan bahwa router menjadi open resolver. Pengujian lebih lanjut harus dibatasi pada jaringan milik sendiri dan dilakukan tanpa menghasilkan beban berlebih.
- **TCP/80:** Nmap mengidentifikasi layanan SSL/HTTP. Port non-standar bukan kerentanan dengan sendirinya.
- **Filtered:** Jangan menyebut port filtered sebagai port open.

## Batasan

Catatan yang tersedia tidak memuat seluruh output mentah Nmap. Karena itu, laporan ini hanya mencantumkan hasil yang benar-benar tercatat dalam catatan audit, bukan mengarang detail versi atau fingerprint tambahan.
