# Panduan: Website Gratis + APK Logbook Kimia WWTP

Semua langkah di bawah **gratis** dan bisa dilakukan dari browser, tanpa memasang Android Studio.

---

## Pilihan A (disarankan): GitHub, website + APK otomatis

Hasilnya:
- Website: `https://NAMA-AKUN.github.io/NAMA-REPO/`
- APK: di halaman **Releases** repo, dan ada link **"Unduh aplikasi Android"** di website

### 1. Buat repo
1. Daftar/masuk di https://github.com (gratis).
2. Klik **New repository**, isi nama (misal `kimia-wwtp`), pilih **Public** (GitHub Pages gratis hanya untuk repo publik), lalu klik **Create repository**.

### 2. Unggah file
1. Ekstrak `kimia-wwtp.zip` di komputer.
2. Di halaman repo, klik **uploading an existing file**, lalu seret **semua isi** folder hasil ekstrak (folder `web`, `android`, file `PANDUAN.md`, dan lainnya). Klik **Commit changes**.
3. **Penting:** folder `.github` sering tidak ikut terunggah karena tersembunyi. Buat manual:
   - Klik **Add file → Create new file**
   - Nama file: `.github/workflows/build.yml`
   - Salin isi file `build.yml` dari zip (buka dengan Notepad), tempel, lalu klik **Commit changes**.

### 3. Aktifkan website
1. Buka **Settings → Pages**.
2. Di **Build and deployment → Source**, pilih **GitHub Actions**.

### 4. Tunggu build
1. Buka tab **Actions**. Proses "Build APK & Website" berjalan sekitar 5–8 menit.
2. Kalau sudah berhasil (centang hijau):
   - Website ada di **Settings → Pages** (alamatnya ditampilkan di atas).
   - APK ada di **Releases** (kolom kanan halaman repo), file `kimia-wwtp.apk`.
3. Kalau langkah 3 dilakukan setelah build pertama, buka **Actions → Build APK & Website → Run workflow** untuk menjalankan ulang.

### 5. Pasang APK di HP
1. Buka website atau halaman Releases dari HP, lalu unduh `kimia-wwtp.apk`.
2. Buka file tersebut. Android akan meminta izin **"Instal aplikasi tidak dikenal"** untuk browser/File Manager, izinkan.
3. Klik **Instal**. Ikon "Kimia WWTP" akan muncul di layar HP.

> Play Protect mungkin memperingatkan karena aplikasi tidak berasal dari Play Store. Pilih **Instal saja**.

---

## Pilihan B (paling cepat): tanpa GitHub

1. Buka https://app.netlify.com/drop, lalu seret **folder `web`** ke halaman itu. Website langsung online gratis (buat akun gratis agar link tidak kedaluwarsa).
2. **Pasang di HP tanpa APK:** buka link website di Chrome Android, ketuk menu ⋮, lalu pilih **Tambahkan ke layar utama / Instal aplikasi**. Aplikasi akan terbuka layar penuh dan tetap bisa dipakai offline.
3. **Kalau tetap butuh file APK:** buka https://www.pwabuilder.com, masukkan link website, klik **Package for stores → Android → Generate**, lalu unduh paketnya. Di dalamnya ada file `.apk` yang siap dipasang.

---

## Sinkron tim: semua HP melihat data yang sama (opsional, gratis)

Tanpa langkah ini, data tersimpan **di masing-masing perangkat**. Untuk satu plant dengan banyak operator shift, aktifkan sinkron lewat Firebase. Paket gratisnya (Spark) jauh lebih dari cukup untuk logbook kimia.

1. Buka https://console.firebase.google.com, klik **Create a project** (Google Analytics boleh dimatikan).
2. **Build → Authentication → Get started → Sign-in method**, aktifkan **Anonymous**.
3. **Build → Firestore Database → Create database**, pilih lokasi `asia-southeast2 (Jakarta)`, lalu mode **production**.
4. Di tab **Rules** Firestore, hapus isinya, tempel isi file `firestore.rules`, lalu klik **Publish**.
5. **Project settings (⚙) → General → Your apps**, klik ikon **Web `</>`**, beri nama, lalu daftarkan. Salin blok `firebaseConfig = { ... }`.
6. **Authentication → Settings → Authorized domains**, tambahkan domain website Anda (misal `nama-akun.github.io`) dan `appassets.androidplatform.net` (untuk APK).
7. Di aplikasi, buka **Pengaturan & backup**, tempel `firebaseConfig`, lalu klik **Buat** untuk membuat ID tim. Klik **Simpan & sambungkan**.
8. Di HP/komputer lain, masukkan **konfigurasi dan ID tim yang sama persis**.
9. Data yang sudah tercatat di perangkat bisa dikirim ke cloud dengan tombol **Kirim data lokal ke cloud**.

**Catatan keamanan:** ID tim berfungsi seperti kata sandi bersama. Bagikan hanya ke operator, dan jangan ditempel di grup publik. Saat sinyal hilang, aplikasi tetap bisa mencatat dan akan mengirim data otomatis begitu online lagi.

---

## Penting dibaca

- **Backup rutin.** Gunakan **Pengaturan & backup → Ekspor backup** minimal sebulan sekali. Tanpa sinkron tim, data hilang kalau aplikasi di-uninstall, data browser dihapus, atau HP direset.
- **Update aplikasi.** Ubah file di folder `web/` di GitHub. APK baru dan website akan dibuat otomatis. APK baru bisa dipasang menimpa yang lama tanpa kehilangan data.
- **Kunci tanda tangan APK** (`android/app/wwtp-release.jks`, kata sandi `kimiawwtp2026`) sudah disertakan agar setiap versi APK bisa saling menimpa. Jangan dihapus atau diganti. Karena repo publik, siapa pun bisa melihat kunci ini. Ini tidak masalah untuk aplikasi internal plant, tapi **jangan pakai kunci ini untuk upload ke Play Store**.
- **Ekspor laporan.** Di Riwayat dan Rekap bulanan, gunakan **Unduh CSV** (bisa dibuka di Excel) atau **Salin ke Excel**. Di APK, file tersimpan di folder `Download/KimiaWWTP`.
