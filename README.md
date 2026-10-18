# Kuis Pendekatan Supervisi dan Kontinum Perilaku Supervisi

Kuis online 50 soal pilihan ganda, waktu 60 menit. Peserta mengisi Nama, NIM, dan Kelas. Skor dan pembahasan tampil setelah kuis dikumpulkan.

## Cara memasang di GitHub (tanpa Git, lewat browser)

1. Login ke https://github.com, klik **+** (kanan atas) → **New repository**.
2. Isi nama repository, misalnya `kuis-supervisi`. Pilih **Public**. Klik **Create repository**.
3. Klik **uploading an existing file**, lalu seret `index.html` dan `README.md` ke halaman itu.
4. Klik **Commit changes**.
5. Buka **Settings → Pages**.
6. Pada **Build and deployment → Source**, pilih **Deploy from a branch**.
7. Pada **Branch**, pilih `main` dan folder `/ (root)`, lalu klik **Save**.
8. Tunggu 1–3 menit. Alamat kuis akan muncul di atas halaman Pages:
   `https://USERNAME.github.io/kuis-supervisi/`

Bagikan alamat tersebut kepada mahasiswa.

## Cara memasang lewat Git (terminal)

```bash
git init
git add index.html README.md
git commit -m "Kuis supervisi"
git branch -M main
git remote add origin https://github.com/USERNAME/kuis-supervisi.git
git push -u origin main
```
Lalu aktifkan Pages seperti langkah 5–8 di atas.

## Pengaturan

Buka `index.html`, cari blok `CONFIG` di bagian atas script:

| Pengaturan | Fungsi |
|---|---|
| `DURASI_MENIT` | Waktu pengerjaan (default 60) |
| `ACAK_SOAL` | `true` = urutan soal diacak per peserta |
| `ACAK_OPSI` | `true` = urutan opsi A–D diacak per peserta |
| `SCRIPT_URL` | URL Google Apps Script untuk menampung hasil (opsional, lihat di bawah) |

Untuk menguji ulang di perangkat sendiri, tambahkan `?reset=1` di akhir alamat (contoh: `.../kuis-supervisi/?reset=1`).

## Menampung hasil mahasiswa di Google Sheets (opsional)

Tanpa langkah ini, hasil hanya tampil di layar mahasiswa. Dengan langkah ini, setiap pengumpulan masuk ke spreadsheet Anda.

1. Buat Google Sheet baru. Pada baris 1 isi judul kolom: `waktu`, `nama`, `nim`, `kelas`, `skor`, `benar`, `salah`, `kosong`, `mulai`, `selesai`, `jawaban`.
2. Klik **Extensions → Apps Script**, hapus isinya, lalu tempel kode berikut:

```javascript
function doPost(e) {
  const d = JSON.parse(e.postData.contents);
  SpreadsheetApp.getActiveSpreadsheet().getActiveSheet().appendRow([
    new Date(), d.nama, "'" + d.nim, d.kelas, d.skor, d.benar, d.salah,
    d.kosong, d.mulai, d.selesai, d.jawaban
  ]);
  return ContentService.createTextOutput("ok");
}
```

3. Klik **Deploy → New deployment → Web app**. Isi **Execute as: Me** dan **Who has access: Anyone**. Klik **Deploy**, lalu salin URL yang muncul.
4. Tempel URL itu ke `SCRIPT_URL` di `index.html`, lalu commit ulang ke GitHub.

Kolom `jawaban` berisi 50 huruf berurutan sesuai nomor soal asli (bukan urutan acak peserta), `-` berarti tidak dijawab.

## Keterbatasan

GitHub Pages hanya menyajikan halaman statis, sehingga kunci jawaban ikut terkirim ke browser dan bisa dilihat siapa pun yang membuka *View Source*. Cocok untuk kuis latihan atau formatif. Untuk ujian yang menentukan nilai akhir, gunakan pengawasan langsung atau platform dengan penilaian di sisi server (misalnya Google Forms mode kuis).
