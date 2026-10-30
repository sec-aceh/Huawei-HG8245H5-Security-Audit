# Ringkasan Audit Keamanan — Huawei HG8245H5

## 1. Tujuan

Mendokumentasikan pemeriksaan awal terhadap router Huawei HG8245H5 milik sendiri melalui jaringan lokal, menggunakan pendekatan non-destruktif.

## 2. Ruang lingkup

- Perangkat: Huawei HG8245H5
- Alamat target: disamarkan sebagai `ROUTER_IP`
- Lingkungan: jaringan lokal milik pemilik perangkat
- Fokus: enumerasi port, antarmuka web, TLS, header HTTP, dan tinjauan statis JavaScript
- Tidak termasuk: brute force, bypass autentikasi, eksploitasi, DoS, atau perubahan konfigurasi

## 3. Identifikasi dari halaman web

HTML halaman utama memuat informasi berikut:

- `ProductName`: `HG8245H5`
- `APPVersion`: `1.1.1.1`
- `CfgMode`: `INATELKOM`
- `ProductType`: `1`

`APPVersion` tersebut dicatat sebagai nilai yang ditemukan pada HTML, bukan dipastikan sebagai versi firmware lengkap. Variabel nama pengguna yang terlihat di JavaScript juga bukan bukti bahwa kredensial tersebut valid.

## 4. Ringkasan hasil

| Komponen | Hasil pengamatan | Status |
|---|---|---|
| TCP/23 | Telnet terdeteksi terbuka | Perlu ditinjau |
| TCP/53 | DNS terdeteksi terbuka | Perlu ditinjau |
| TCP/80 | Layanan SSL/HTTP terdeteksi | Perlu ditinjau |
| TLS | TLS 1.1 dan 1.2 teramati | Hardening |
| Sertifikat | Nama sertifikat tidak cocok dengan IP target; verifikasi lokal gagal | Hardening |
| HTTP headers | X-Frame-Options SAMEORIGIN dan no-store teramati pada halaman utama | Kontrol positif pada rute yang diperiksa |
| Login JavaScript | Password di-Base64 sebelum form dikirim | Perlu interpretasi hati-hati |
| Token endpoint | GET menghasilkan string 32 karakter heksadesimal setelah BOM dihapus | Belum dapat disimpulkan |

## 5. Kesimpulan

Pemeriksaan ini menghasilkan beberapa temuan konfigurasi yang layak ditinjau, terutama layanan Telnet lokal dan dukungan TLS 1.1. Belum ada bukti yang cukup untuk menyatakan adanya authentication bypass, remote code execution, kebocoran kredensial, atau kerentanan yang dapat dieksploitasi.

## 6. Rekomendasi defensif

1. Periksa apakah Telnet dapat dinonaktifkan atau dibatasi ke jaringan administrasi.
2. Perbarui firmware hanya melalui kanal resmi ISP/vendor dan sesuai prosedur perangkat.
3. Jika tersedia, nonaktifkan TLS 1.1 dan gunakan TLS 1.2 atau lebih baru.
4. Gunakan sertifikat manajemen yang sesuai dengan nama host/IP bila firmware mendukungnya.
5. Ganti kata sandi administrator dengan kata sandi unik dan kuat.
6. Nonaktifkan administrasi jarak jauh dari WAN jika tidak dibutuhkan.
7. Tinjau perangkat klien dan jaringan Wi-Fi; gunakan WPA2-AES/WPA3 sesuai dukungan perangkat.
8. Simpan bukti audit tanpa token, cookie, kredensial, atau data konfigurasi privat.

## 7. Batasan

Hasil hanya menggambarkan perangkat dan konfigurasi pada saat pengujian. Hasil tidak dapat digeneralisasi ke semua HG8245H5 atau versi firmware lainnya.
