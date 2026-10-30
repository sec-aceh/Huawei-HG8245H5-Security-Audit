# Perintah OpenSSL

## Periksa handshake TLS

```bash
openssl s_client \
  -connect ROUTER_IP:80 \
  -servername ROUTER_IP \
  </dev/null
```

Perhatikan versi TLS, cipher suite yang dinegosiasikan, informasi sertifikat, dan hasil verifikasi.

## Catatan interpretasi

- Hasil satu handshake hanya menunjukkan parameter yang dinegosiasikan pada koneksi tersebut.
- `Verify return code` berkaitan dengan validasi sertifikat oleh trust store klien.
- Jangan mempublikasikan output sensitif seperti session material, cookie, atau token.
