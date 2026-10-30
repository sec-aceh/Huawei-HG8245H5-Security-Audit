# Analisis TLS

## Perintah yang digunakan

```bash
openssl s_client -connect ROUTER_IP:80 -servername ROUTER_IP </dev/null
```

Untuk pemeriksaan HTTP lokal dengan curl:

```bash
curl -k -i --max-time 10 https://ROUTER_IP:80/
curl -k -sSI https://ROUTER_IP:80/
```

`-k` melewati verifikasi sertifikat. Gunakan hanya pada perangkat sendiri dalam jaringan lokal untuk keperluan diagnostik. Jangan menjadikan opsi ini kebiasaan untuk situs publik.

## Pengamatan yang tercatat

- TLS 1.1 dan TLS 1.2 teramati.
- Kunci publik RSA pada sertifikat berukuran 2048 bit.
- Salah satu handshake menegosiasikan `ECDHE-RSA-AES128-SHA256`.
- Beberapa cipher suite CBC lama teramati.
- Sertifikat menunjukkan subject `Huawei Technologies Co., Ltd`, yang tidak cocok dengan IP target yang digunakan.
- Verifikasi sertifikat lokal menghasilkan `Verify return code: 21` pada satu pemeriksaan.

## Interpretasi

- Dukungan TLS 1.1 merupakan temuan hardening karena versi tersebut sudah usang dibanding TLS 1.2/1.3.
- RSA 2048-bit tidak dengan sendirinya menunjukkan kelemahan kunci.
- Cipher suite yang dinegosiasikan pada satu handshake tidak mewakili seluruh konfigurasi server.
- Ketidakcocokan nama sertifikat dan kegagalan trust chain menyebabkan klien tidak dapat memverifikasi identitas server secara normal. Ini bukan bukti bahwa handshake TLS gagal.
- `Verify return code: 21` menunjukkan sertifikat tidak dapat diverifikasi sebagai trusted oleh konfigurasi trust store klien saat itu.

## Rekomendasi

Jika didukung firmware, gunakan TLS 1.2 atau lebih baru, sertifikat yang sesuai dengan nama host manajemen, dan cipher suite modern. Periksa pembaruan firmware melalui ISP/vendor.
