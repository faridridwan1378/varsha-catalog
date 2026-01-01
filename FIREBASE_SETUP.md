# 🔥 Panduan Lengkap Setting Firebase untuk Varsha Catalog

Panduan ini menjelaskan langkah demi langkah cara menghubungkan katalog Varsha dengan Firebase Firestore untuk menyimpan data produk dan pengaturan toko secara online.

---

## 📋 Daftar Isi
1. [Membuat Akun Firebase](#1-membuat-akun-firebase)
2. [Membuat Project Baru](#2-membuat-project-baru)
3. [Mendaftarkan Web App](#3-mendaftarkan-web-app)
4. [Membuat Database Firestore](#4-membuat-database-firestore)
5. [Mengatur Security Rules](#5-mengatur-security-rules)
6. [Menghubungkan ke Varsha Catalog](#6-menghubungkan-ke-varsha-catalog)
7. [Upload Data Awal](#7-upload-data-awal)
8. [Troubleshooting](#8-troubleshooting)

---

## 1. Membuat Akun Firebase

### Jika belum punya akun Google:
1. Buka [accounts.google.com](https://accounts.google.com)
2. Klik **"Buat akun"**
3. Ikuti proses pendaftaran

### Jika sudah punya akun Google:
1. Buka [Firebase Console](https://console.firebase.google.com)
2. Klik **"Sign in"** atau **"Masuk"**
3. Login dengan akun Google Anda

---

## 2. Membuat Project Baru

### Langkah-langkah:

1. Di Firebase Console, klik tombol **"Create a project"** atau **"Buat project"**

2. **Masukkan nama project:**
   ```
   varsha-catalog
   ```
   > Nama bisa disesuaikan, contoh: varsha-fruits, toko-varsha, dll

3. **Google Analytics** (opsional):
   - Bisa diaktifkan atau dinonaktifkan
   - Untuk katalog sederhana, pilih **"Disable"** saja
   - Klik **"Create project"**

4. Tunggu beberapa detik hingga project selesai dibuat

5. Klik **"Continue"** untuk masuk ke dashboard project

---

## 3. Mendaftarkan Web App

### Langkah-langkah:

1. Di halaman **Project Overview**, cari icon **Web** `</>` 
   - Terletak di bagian tengah halaman
   - Di bawah tulisan "Get started by adding Firebase to your app"

2. Klik icon **Web** `</>`

3. **Masukkan nickname app:**
   ```
   Varsha Web Catalog
   ```

4. **Firebase Hosting:** 
   - ❌ Jangan centang (tidak diperlukan)
   - Kita akan pakai GitHub Pages

5. Klik **"Register app"**

6. **⚠️ PENTING: Salin Konfigurasi!**
   
   Akan muncul kode seperti ini:
   ```javascript
   const firebaseConfig = {
     apiKey: "AIzaSyBxxxxxxxxxxxxxxxxxxxxxxx",
     authDomain: "varsha-catalog.firebaseapp.com",
     projectId: "varsha-catalog",
     storageBucket: "varsha-catalog.appspot.com",
     messagingSenderId: "123456789012",
     appId: "1:123456789012:web:abcdef123456"
   };
   ```

7. **Salin nilai-nilai berikut** (tanpa tanda kutip):
   - `apiKey` → AIzaSyBxxxxxxxxxxxxxxxxxxxxxxx
   - `authDomain` → varsha-catalog.firebaseapp.com
   - `projectId` → varsha-catalog
   - `storageBucket` → varsha-catalog.appspot.com
   - `messagingSenderId` → 123456789012
   - `appId` → 1:123456789012:web:abcdef123456

8. Klik **"Continue to console"**

---

## 4. Membuat Database Firestore

### Langkah-langkah:

1. Di sidebar kiri, klik **"Build"** → **"Firestore Database"**

2. Klik tombol **"Create database"**

3. **Pilih lokasi server:**
   - Pilih **"asia-southeast1 (Singapore)"** untuk performa terbaik di Indonesia
   - Atau biarkan default

4. **Pilih Security Rules:**
   - ⚠️ Pilih **"Start in test mode"**
   - Ini memungkinkan baca/tulis tanpa autentikasi
   - Klik **"Create"**

5. Tunggu beberapa detik hingga database selesai dibuat

---

## 5. Mengatur Security Rules

### Untuk Development/Testing:

Rules default test mode (berlaku 30 hari):
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.time < timestamp.date(2025, 2, 10);
    }
  }
}
```

### Untuk Production (Lebih Aman):

1. Di Firestore, klik tab **"Rules"**

2. Ganti dengan rules berikut:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Semua orang bisa baca
    match /{document=**} {
      allow read: if true;
    }
    
    // Hanya bisa tulis dari domain tertentu
    match /products/{productId} {
      allow write: if true; // Untuk admin via web
    }
    
    match /settings/{settingId} {
      allow write: if true; // Untuk admin via web
    }
  }
}
```

3. Klik **"Publish"**

> 💡 **Tips:** Untuk keamanan lebih, Anda bisa menambahkan Firebase Authentication nanti.

---

## 6. Menghubungkan ke Varsha Catalog

### Langkah-langkah:

1. Buka katalog Varsha di browser

2. Klik menu **"Admin"** di navigasi bawah

3. Masukkan PIN: **`1234`** (default)

4. Klik tab **"Firebase"**

5. Masukkan konfigurasi yang sudah disalin:

   | Field | Contoh Nilai |
   |-------|--------------|
   | API Key | AIzaSyBxxxxxxxxxxxxxxxxxxxxxxx |
   | Auth Domain | varsha-catalog.firebaseapp.com |
   | Project ID | varsha-catalog |
   | Storage Bucket | varsha-catalog.appspot.com |
   | Messaging Sender ID | 123456789012 |
   | App ID | 1:123456789012:web:abcdef123456 |

6. Klik tombol **"Simpan & Hubungkan"**

7. Perhatikan indikator di header:
   - 🟢 **Terhubung** = Berhasil!
   - 🔴 **Error** = Periksa konfigurasi

---

## 7. Upload Data Awal

### Setelah terhubung:

1. Masih di tab **"Firebase"** di panel Admin

2. Klik tombol **"Upload Data ke Firebase"**

3. Tunggu hingga muncul notifikasi **"Semua data berhasil diupload!"**

4. Untuk memverifikasi, buka Firebase Console:
   - Klik **"Firestore Database"**
   - Anda akan melihat collection:
     - `products` → berisi semua produk
     - `settings` → berisi pengaturan toko

---

## 8. Troubleshooting

### ❌ Error: "Firebase belum terhubung"
**Solusi:**
- Pastikan semua field konfigurasi terisi
- Periksa apakah ada typo di API Key atau Project ID
- Pastikan Firestore Database sudah dibuat

### ❌ Error: "Permission denied"
**Solusi:**
- Buka Firebase Console → Firestore → Rules
- Pastikan menggunakan **test mode** atau rules yang benar
- Klik "Publish" setelah mengubah rules

### ❌ Status tetap "Connecting..."
**Solusi:**
- Refresh halaman
- Periksa koneksi internet
- Coba hapus konfigurasi dan masukkan ulang

### ❌ Data tidak muncul setelah refresh
**Solusi:**
- Pastikan sudah klik "Upload Data ke Firebase"
- Periksa di Firebase Console apakah data sudah masuk
- Klik "Download Data dari Firebase" untuk sinkronisasi

### ❌ Produk hilang / kembali ke default
**Solusi:**
- Data mungkin belum di-upload ke Firebase
- Klik "Upload Data ke Firebase" untuk backup
- Selalu pastikan indikator menunjukkan "Terhubung"

---

## 📱 Tips Penggunaan

### Untuk Admin:
1. Selalu pastikan status **"Terhubung"** sebelum edit data
2. Setelah edit produk, data otomatis tersinkronisasi
3. Gunakan "Upload Data" untuk backup manual
4. Ubah PIN default untuk keamanan

### Untuk Multi-Device:
1. Setelah setup Firebase, buka katalog di device lain
2. Login Admin dengan PIN yang sama
3. Masukkan konfigurasi Firebase yang sama
4. Data akan otomatis tersinkronisasi

### Untuk GitHub Pages:
1. Konfigurasi Firebase tersimpan di localStorage browser
2. Setiap pengunjung perlu input konfigurasi sendiri (jika ingin jadi admin)
3. User biasa bisa langsung lihat katalog tanpa perlu konfigurasi

---

## 🔐 Keamanan

### Rekomendasi:
1. **Ubah PIN default** dari 1234 ke kombinasi unik
2. **Jangan share** konfigurasi Firebase ke publik
3. **Gunakan rules yang ketat** untuk production
4. **Aktifkan Firebase Authentication** untuk keamanan ekstra

### Rules Production yang Lebih Aman:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Publik hanya bisa baca
    match /products/{productId} {
      allow read: if true;
      allow write: if false; // Disable untuk publik
    }
    
    match /settings/{settingId} {
      allow read: if true;
      allow write: if false;
    }
  }
}
```

> ⚠️ Dengan rules ini, Anda perlu menggunakan Firebase Console langsung untuk edit data.

---

## 📞 Bantuan

Jika mengalami kesulitan:
1. Baca dokumentasi resmi: [Firebase Documentation](https://firebase.google.com/docs)
2. Cek Firebase Status: [status.firebase.google.com](https://status.firebase.google.com)
3. Komunitas: [Stack Overflow Firebase](https://stackoverflow.com/questions/tagged/firebase)

---

## ✅ Checklist Setup

- [ ] Buat akun Google (jika belum ada)
- [ ] Login ke Firebase Console
- [ ] Buat project baru
- [ ] Daftarkan Web App
- [ ] Salin konfigurasi
- [ ] Buat Firestore Database (test mode)
- [ ] Input konfigurasi di Varsha Admin
- [ ] Klik "Simpan & Hubungkan"
- [ ] Verifikasi status "Terhubung"
- [ ] Upload data awal
- [ ] Ubah PIN default
- [ ] Selesai! 🎉

---

**Selamat! Katalog Varsha Anda sekarang terhubung dengan Firebase! 🍊🔥**
