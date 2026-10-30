# Tinjauan Statis Alur Login

## Sumber yang diperiksa

- HTML halaman login yang disimpan sebagai `/tmp/huawei-page.html` pada lingkungan audit.
- JavaScript `md5.js`, `RndSecurityFormat.js`, dan `safelogin.js`.
- Endpoint lokal `/asp/GetRandCount.asp` hanya diamati melalui satu permintaan GET yang tercatat.

File sumber asli tidak disertakan dalam repositori publik ini.

## Pengamatan pada HTML login

Fungsi `LoginSubmit(val)` yang diperiksa melakukan hal-hal berikut:

1. Memeriksa bahwa username dan password tidak kosong.
2. Menghapus cookie.
3. Mengirim permintaan AJAX POST sinkron ke `/asp/GetRandCount.asp`.
4. Menambahkan nilai username ke form.
5. Menambahkan nilai password setelah diproses oleh fungsi `base64encode()`.
6. Menambahkan bahasa.
7. Menetapkan token yang diterima sebagai parameter `x.X_HW_Token`.
8. Mengatur action form ke `/login.cgi`, kemudian mengirim form.

## Pengamatan JavaScript lain

- `md5.js` menyediakan implementasi MD5 dan fungsi terkait.
- `RndSecurityFormat.js` menyediakan implementasi SHA-256; pengujian mandiri terhadap input `abc` menghasilkan digest SHA-256 yang dikenal.
- Tidak ditemukan pemanggilan `MD5()` atau `RndSecurityFormat()` yang terlihat dalam jalur `LoginSubmit()` yang diperiksa.
- `safelogin.js` memiliki fungsi `Submit(type)` untuk pengiriman form lain dan menambahkan `x.X_HW_Token` dari elemen `onttoken`. Hubungan elemen tersebut dengan halaman login yang diperiksa tidak dapat dipastikan dari bukti yang tersedia.

## Pengamatan endpoint token

Satu permintaan GET ke `/asp/GetRandCount.asp` mengembalikan HTTP 200 dan body yang tampak seperti 32 karakter heksadesimal setelah tiga byte UTF-8 BOM di awal respons diabaikan.

Nilai token aktual sengaja tidak dicantumkan dalam laporan atau bukti publik.

## Interpretasi dan batasan

- Base64 adalah encoding, bukan enkripsi.
- Karena halaman diakses melalui TLS, keberadaan Base64 di JavaScript saja tidak membuktikan bahwa password dikirim tanpa perlindungan transport.
- Permintaan GET yang berhasil tidak membuktikan perilaku endpoint saat menerima POST seperti yang digunakan oleh login.
- Tidak ada pengujian yang membuktikan entropi token, sifat acak, prediktabilitas, masa berlaku, anti-replay, session binding, atau validasi sisi server.
- Keberadaan kode MD5/SHA-256 di file JavaScript tidak membuktikan bahwa algoritme tersebut digunakan untuk autentikasi.

## Kesimpulan

Alur login hanya ditinjau secara statis dan satu respons token diamati. Bukti yang tersedia belum cukup untuk menyatakan adanya kelemahan autentikasi atau kebocoran password.
