# Perintah curl

Contoh berikut ditujukan untuk antarmuka lokal router sendiri.

## Ambil halaman utama dan header

```bash
curl -k -i --max-time 10 https://ROUTER_IP:80/
curl -k -sSI --max-time 10 https://ROUTER_IP:80/
```

## Simpan header dan body ke file lokal

```bash
curl -k -sS --max-time 10 \
  -D /tmp/router-headers.txt \
  -o /tmp/router-page.html \
  https://ROUTER_IP:80/
```

Opsi `-k` melewati verifikasi sertifikat. Ini berguna untuk diagnosis sertifikat lokal yang tidak cocok, tetapi tidak memperbaiki sertifikat dan tidak boleh digunakan untuk menyembunyikan masalah validasi pada penggunaan normal.

Jangan menyimpan atau mempublikasikan respons autentikasi, token, cookie, atau konfigurasi privat tanpa redaksi.
