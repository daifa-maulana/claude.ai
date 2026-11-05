# 🤖 CommuteMate - AI Prompting Playbook & Cheat Sheet
**Buku Panduan Prompting AI untuk Tim Pengembang (PM, UI/UX, Frontend, Backend)**  
*Versi: 1.0.0 | Standar Komunikasi AI untuk Efisiensi Rekayasa Perangkat Lunak*

---

## 1. Formula Emas Prompting Profesional: Kerangka R-C-T-C-O

Agar AI (Claude, Antigravity, ChatGPT) memberikan kode dan solusi yang **langsung akurat tanpa halusinasi**, gunakan rumus 5 elemen ini:

$$\mathbf{Prompt} = \text{Role (Peran)} + \text{Context (Konteks Dokumen)} + \text{Task (Tugas Spesifik)} + \text{Constraints (Batasan)} + \text{Output (Format Hasil)}$$

| Elemen | Penjelasan | Contoh Penggunaan |
|---|---|---|
| **Role (R)** | Tentukan peran spesifik AI | *"Bertindaklah sebagai Senior Flutter Architect..."* |
| **Context (C)** | Hubungkan dengan dokumen proyek | *"Berdasarkan file `API_SPECIFICATION.md` dan skema database `ERD_DATABASE.md`..."* |
| **Task (T)** | Perintah tugas spesifik & jelas | *"Buatkan controller autentikasi Sanctum dan endpoint register/login..."* |
| **Constraints (C)** | Aturan wajib & larangan teknis | *"Wajib Flutter 3.24, gunakan `Color.withOpacity` (hindari `withValues`), offline-first tanpa API berbayar."* |
| **Output (O)** | Format keluaran yang diinginkan | *"Berikan kode lengkap tanpa pemotongan (no placeholder `// TODO`), sertakan komentar penjelas dalam Bahasa Indonesia."* |

---

## 2. Template Prompt Siap Pakai untuk Setiap Peran Tim

---

### A. Template untuk Project Manager (Anda)

#### Prompt 1: Membuat Update Harian / Sprint ke Grup WhatsApp Tim
```text
Bertindaklah sebagai IT Project Manager profesional.
Berdasarkan file docs/01_project_management/TODO_LIST.md di proyek CommuteMate, buatkan pesan update progres mingguan yang ramah, memotivasi, dan terstruktur untuk dikirim ke grup WhatsApp tim (4 orang: PM, UI/UX, Frontend, Backend).

Format pesan:
1. Sapaan singkat & ringkasan target sprint minggu ini.
2. Checklist tugas per peran (apa yang harus diselesaikan masing-masing orang).
3. Reminder komitmen teknis (offline-first & bebas kartu kredit).
Gunakan bahasa santai khas mahasiswa/startup tapi tetap rapi dan profesional.
```

#### Prompt 2: Memecah Fitur Menjadi User Story & Task Trello/Notion
```text
Berdasarkan fitur "Smart Wake-Up & Don't Leave Me" pada PRD.md, pecahkan fitur ini menjadi 4 sub-task spesifik untuk:
1. UI/UX Designer (wireframe & user testing)
2. Frontend Developer (sensor GPS & geofencing widget)
3. Backend Developer (API endpoint & skema database)
Sertakan Acceptance Criteria (kriteria lolos uji) untuk masing-masing task dengan format "Given - When - Then".
```

---

### B. Template untuk Frontend Developer (Flutter)

#### Prompt 3: Membuat Halaman UI dengan Device Preview
```text
Bertindaklah sebagai Senior Flutter Developer.
Saya ingin membuat layar "Dashboard Utama" untuk aplikasi CommuteMate dengan spesifikasi berikut:
1. Mengikuti desain tokens pada docs/02_ui_ux_testing/USER_FLOWS_AND_WIREFRAMES.md:
   - Warna utama Navy (#1C0FB3), aksen Cyan (#5BB8F5), background (#F4F6FB).
   - Floating Card putih dengan radius 20px dan shadow lembut.
2. Tampilkan 2x3 Grid fitur: KinetoGuard, Smart Wake-Up, Checklist, Nearby Spot, Splitter, dan Solo Safety.
3. Gunakan Curved Wave Bottom Navigation Bar dengan Floating Action Button (FAB) Cyan di tengah.
4. Pastikan kompatibel dengan Flutter 3.24+ (gunakan Color.withOpacity, jangan gunakan withValues).
5. Bungkus dengan device_preview agar bisa dites langsung di browser Chrome.

Tolong berikan kode lengkap main.dart dan dashboard_screen.dart yang siap dijalankan.
```

#### Prompt 4: Implementasi Algoritma Gyroscope KinetoGuard
```text
Bertindaklah sebagai Flutter Mobile Specialist.
Tolong buatkan class `SensorService` dan widget `KinetoGuardOverlay` menggunakan package `sensors_plus: ^5.0.1`.
Kebutuhan:
1. Dengarkan stream accelerometer dan hitung sudut roll & pitch kendaraan.
2. Gunakan Low-Pass Filter (alpha = 0.15) agar pergerakan titik halus dan tidak bergetar liar.
3. Gambar titik-titik lembut di tepi layar kiri dan kanan yang bergerak berlawanan arah dengan gaya inersia kendaraan.
4. Pastikan method dispose() membatalkan stream subscription untuk mencegah kebocoran memori baterai.
Sertakan komentar dokumentasi pada setiap baris logika perhitungannya.
```

---

### C. Template untuk Backend Developer (Laravel 11)

#### Prompt 5: Membuat Migration & Model Eloquent dari ERD
```text
Bertindaklah sebagai Laravel 11 Backend Architect.
Berdasarkan dokumen docs/03_backend_api/ERD_DATABASE.md, tolong buatkan:
1. Migration database lengkap untuk tabel: `users`, `trips`, `destination_stops`, `nearby_spots`, `checklist_items`, `expenses`, dan `safety_contacts`.
2. Model Eloquent lengkap beserta relasinya (hasMany, belongsTo) dan cast tipe datanya (misal: latitude/longitude ke float, is_checked ke boolean).
3. Gunakan tipe data UUID untuk primary key trips dan destination_stops.
Berikan kode migrasi dan model yang mengikuti standar PSR-12 dan Laravel 11.
```

#### Prompt 6: Membuat REST Controller & Endpoint API
```text
Berdasarkan dokumen docs/03_backend_api/API_SPECIFICATION.md, tolong buatkan:
1. Controller `NearbySpotController.php` dengan method `index()` dan `offlinePack()`.
2. Method `index()` menerima query param (lat, lng, category, radius_km) dan menghitung jarak terdekat menggunakan rumus Haversine query SQL.
3. Kembalikan response dalam standar envelope JSON format:
   { "success": true, "message": "...", "data": [...] }
4. Gunakan FormRequest untuk validasi input.
```

---

### D. Template untuk UI/UX Designer & User Testing

#### Prompt 7: Menganalisis & Menghitung Skor Hasil SUS Testing
```text
Bertindaklah sebagai UX Researcher Specialist.
Berikut adalah data mentah jawaban skala Likert (1 - 5) dari 5 responden untuk 10 butir kuesioner SUS aplikasi CommuteMate:
- Responden 1: [4, 2, 5, 1, 4, 2, 5, 2, 4, 1]
- Responden 2: [5, 1, 4, 1, 5, 1, 4, 1, 5, 2]
- Responden 3: [4, 2, 4, 2, 4, 2, 4, 2, 4, 2]
- Responden 4: [5, 1, 5, 1, 5, 1, 5, 1, 5, 1]
- Responden 5: [3, 3, 4, 2, 4, 2, 3, 2, 4, 2]

Tolong:
1. Hitung kontribusi skor tiap butir ganjil (skor - 1) dan butir genap (5 - skor).
2. Hitung total skor akhir SUS tiap responden dan nilai rata-rata keseluruhan.
3. Tentukan predikat (Grade A/B/C/D), tingkat penerimaan (Acceptability), dan interpretasi kelayakan aplikasi untuk sidang akhir proyek.
```

---

### E. Template Sakti Saat Menemukan Error (Debugging Formula)

Saat kode Anda mengalami error, **jangan hanya kirim: "kok error?"**. Gunakan template ini:

```text
Saya mengalami error saat menjalankan [Flutter / Laravel / Database].

1. Perintah yang dijalankan: [misal: flutter run -d chrome atau php artisan migrate]
2. Pesan Error Lengkap:
```
[Tempel log error terminal di sini]
```
3. File terkait: [misal: lib/screens/map_screen.dart]
4. Kode di sekitar baris error:
```dart
[Tempel potongan kode 10-15 baris terkait]
```

Tolong analisis:
- Apa akar penyebab error ini?
- Bagaimana perbaikan kode yang tepat tanpa merusak fungsi yang sudah ada?
```

---

## 3. Tips Tambahan Agar Respon AI Selalu Berkualitas
1. **Sebutkan Nama File Dokumen**: Karena semua dokumen sudah di-push ke GitHub (`PRD.md`, `ERD_DATABASE.md`, dll), sebut nama filenya agar AI membaca aturan yang sudah disepakati.
2. **One Feature at a Time**: Jangan minta AI membuat 7 fitur sekaligus dalam 1 prompt. Buat bertahap per fitur (misal: selesaikan KinetoGuard dulu $\to$ baru lanjut Smart Wake-Up).
3. **Minta Penjelasan Sederhana**: Jika ada kode rumit yang dibuat AI, selalu tambahkan di akhir: *"Tolong jelaskan secara singkat baris mana yang paling penting agar bisa saya jelaskan saat presentasi ke dosen."*

