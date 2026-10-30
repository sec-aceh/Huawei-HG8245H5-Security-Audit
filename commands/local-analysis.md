# Analisis File Lokal

Contoh untuk meninjau file yang telah disimpan dari perangkat milik sendiri.

## Cari referensi login dan token di HTML

```bash
grep -nEi -B5 -A12 \
  'onttoken|GetRandCount|x.X_HW_Token|LoginSubmit' \
  /tmp/huawei-page.html
```

## Cari pola relevan pada JavaScript

```bash
grep -nEi \
  'randcount|token|x_hw_token|onttoken|GetRandCnt|Math\.random|Date\(' \
  ./*.js
```

## Periksa digest SHA-256 file

```bash
sha256sum ./*.js
```

## Periksa body respons lokal tanpa menampilkan nilai token

Jika file berisi respons token, jangan mencetak nilainya ke terminal yang direkam atau ke log publik. Untuk dokumentasi, cukup catat status HTTP, panjang respons, dan format umum setelah menghapus BOM, tanpa menyimpan nilai token.
