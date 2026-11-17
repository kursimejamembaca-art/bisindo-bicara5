# BISINDO Bicara

Penerjemah bahasa isyarat BISINDO berbasis kamera, dengan MediaPipe Hand Landmarker.
Semua pemrosesan dilakukan di perangkat. Frame kamera tidak dikirim ke server mana pun.

## Fitur

**Penerjemah (halaman utama)**
- Terjemahan langsung dari kamera: kata atau huruf yang dikenali muncul di kotak terjemahan dan dibacakan.
- Huruf dan angka dikenali per frame dari pose tangan. Kata dikenali dari urutan gerakan sekitar 1 detik.
- Kalimat dikenali dari rekaman gerakan utuh yang sudah dilatih.
- Galeri: pilih video, lalu hasil analisisnya tampil di halaman Hasil.

**Latih**
- Rekam latihan: tulis nama kata, huruf, angka, atau kalimat, lalu tekan Mulai rekam dan Berhenti & simpan. Ada hitung mundur sebelum rekaman dimulai.
- Latih dari video: pilih satu atau beberapa video untuk nama yang Anda tulis.
- Daftar kata terlatih: jumlah contoh per kata, Ubah nama, Hapus per kata, dan Contoh untuk melihat dan menghapus contoh satu per satu.
- Uji akurasi per kata, dengan tabel yang bisa disalin.
- Unduh dan Impor data latih (format JSON), sebagai cadangan.

**Panduan**
- Petunjuk penggunaan dan kosakata dasar.

## Menjalankan di komputer

1. Pasang Node.js 18 atau lebih baru.
2. Jalankan `npm install`. Perintah ini menyalin file wasm dan mengunduh model `hand_landmarker.task` (sekitar 7 MB) ke `public/models`. Internet diperlukan saat instalasi.
3. Jalankan `npm run dev`, lalu buka alamat yang ditampilkan.
4. Untuk produksi: `npm run build`. Hasilnya ada di folder `dist/`.

Jika model gagal diunduh saat instalasi, jalankan ulang `npm run prepare-assets`.

## Menjalankan di GitHub Pages

1. Buat repositori di GitHub, lalu unggah seluruh isi folder ini:
   ```
   git init
   git add .
   git commit -m "Versi awal BISINDO Bicara"
   git branch -M main
   git remote add origin https://github.com/USERNAME/NAMA-REPO.git
   git push -u origin main
   ```
2. Buka **Settings → Pages**, lalu pada **Source** pilih **GitHub Actions**.
3. Setiap `push` ke `main` menjalankan `.github/workflows/deploy.yml`, yang membangun dan menerbitkan aplikasi.
4. Buka `https://USERNAME.github.io/NAMA-REPO/` dari HP. Alamat HTTPS ini membuat kamera aktif.

## Catatan penting

- **Kamera hanya berjalan di HTTPS** atau `localhost`.
- **Data latih disimpan di IndexedDB** pada browser perangkat itu. Data tidak ikut ke perangkat lain, dan hilang jika data situs dihapus. Gunakan tombol **Unduh** di halaman Latih untuk membuat cadangan, lalu **Impor** di perangkat lain.
- Data latih lama dari versi `localStorage` dipindahkan otomatis saat aplikasi pertama kali dibuka.
- Setelah dibuka sekali dengan koneksi, aplikasi dan model disimpan oleh service worker sehingga bisa dipakai offline.

## Batasan yang masih ada

- Kata membutuhkan sekitar setengah detik gerakan sebelum bisa dikenali. Ini bagian dari cara model membaca urutan gerakan, dan mengurangi jeda ini menurunkan akurasi.
- Kalimat dikenali sebagai satu gerakan utuh. Kalimat yang lebih dari 6 detik belum didukung.
- Akurasi bergantung pada jumlah dan variasi contoh latih. Latih setiap kata dengan beberapa orang dan beberapa posisi tangan.

## Struktur

- `index.html` — tampilan
- `src/style.css` — gaya
- `src/main.js` — kamera, MediaPipe, pengenalan, dan latihan
- `src/db.js` — penyimpanan IndexedDB
- `public/` — gambar, manifest, service worker, model, dan wasm
- `scripts/prepare-assets.mjs` — menyiapkan wasm dan model
