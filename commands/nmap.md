# Perintah Nmap

Ganti `ROUTER_IP` dengan alamat router milik sendiri atau perangkat yang secara eksplisit diizinkan untuk diuji.

## Pemindaian layanan terbatas

```bash
nmap -sV -sC -p 23,53,80 ROUTER_IP
```

## Pemindaian lebih luas

```bash
nmap -v -A ROUTER_IP
```

`-A` mengaktifkan deteksi OS, deteksi versi, skrip default, dan traceroute. Jalankan hanya pada jaringan yang berada dalam kendali atau izin penguji.

## Simpan output

```bash
mkdir -p evidence
nmap -sV -sC -p 23,53,80 ROUTER_IP \
  -oN evidence/nmap-results-local.txt
```

Sebelum commit, tinjau dan samarkan IP privat, hostname, MAC address, serta informasi unik lainnya.
