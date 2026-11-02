# 📋 MASTER TO-DO LIST & SPRINT ROADMAP
## Project: CommuteMate (Passenger & Solo Trip Utility Kit)
> **Owner**: Lead Project Manager  
> **Team Size**: 4 Personel (PM, UI/UX & Tester, Frontend, Backend)  
> **Location**: `C:\Users\Lenovo\daifa\commute_mate\`  
> **Status**: Fresh Architecture (Dimulai dari Nol)

---

## 🏗️ 0. Tech Stack & Repository Setup (Status: In Progress)

- [x] **Inisialisasi Direktori Monorepo Proyek**:
  - `docs/` (Dokumentasi Lengkap 4 Role)
  - `mobile/` (Aplikasi Mobile Flutter)
  - `backend/` (REST API Server Laravel 11)
- [ ] **Inisialisasi Git Repository**:
  - [ ] `git init` di folder `commute_mate/`
  - [ ] Setup file `.gitignore` (Flutter, Laravel, Environment)
  - [ ] Pembuatan branch utama: `main`, `development`, `feature/mobile`, `feature/backend`
- [ ] **Tech Stack Lock (Resmi Disahkan)**:
  - **Frontend**: Flutter (Dart) + Provider State Management + SQLite (Offline-First) + Device Preview
  - **Backend**: Laravel 11 (PHP 8.2+) + MySQL + Laravel Sanctum
  - **Dokumentasi & API Contract**: Postman Collection & OpenAPI/Markdown
  - **UI/UX Testing**: System Usability Scale (SUS) 10 Kuesioner

---

## 🏃 Sprint 1: Project Management & Fondasi Dokumentasi (Role: PM)
*Target: Seluruh requirement terkunci, arsitektur jelas, dan tugas terdistribusi.*

- [x] **PRD Final**: Spesifikasi 7 pilar fitur utama (KinetoGuard, Smart Wake-Up, Checklist, Nearby Spot, Split Bill, Fake Call, Soundscape).
- [x] **RACI Matrix**: Pembagian peran tanggung jawab 4 anggota tim.
- [ ] **Sprint Planning & Timeline**: Jadwal 4 minggu menuju sidang/demo.
- [ ] **Monitoring & Daily Standup Check**: Pemantauan blockers tim FE & BE.

---

## 🗄️ Sprint 2: Backend Architecture & REST API (Role: Backend Developer)
*Target: Database MySQL terancang di ERD, migrations Laravel jalan, dan endpoints siap diuji di Postman.*

- [ ] **Perancangan Database (ERD)**:
  - [ ] Desain tabel `users`, `trips`, `destination_stops`, `nearby_spots`, `checklist_items`, `expenses`, `safety_contacts`.
  - [ ] Dokumentasi Data Dictionary & Relasi Foreign Key.
- [ ] **Scaffolding Proyek Laravel 11**:
  - [ ] Setup Laravel di folder `backend/` (`composer create-project laravel/laravel .`)
  - [ ] Konfigurasi database `.env` (MySQL)
- [ ] **Database Migrations & Seeders**:
  - [ ] Migration untuk 7 tabel utama.
  - [ ] Seeder data bawaan (Dummy rute Jakarta-Bandung, 10 data Nearby Spot: Toilet, SPBU, Rest Area).
- [ ] **Pembuatan REST API Controller**:
  - [ ] `POST /api/auth/register` & `POST /api/auth/login` (Laravel Sanctum)
  - [ ] `GET /api/nearby-spots` (Filter: category toilet/spbu/rest_area)
  - [ ] `GET /api/destination-stops` (Daftar halte & koordinat geofencing)
  - [ ] `POST /api/trips/sync` (Sinkronisasi offline-to-online data perjalanan & pengeluaran)
- [ ] **API Documentation**:
  - [ ] Export file Postman Collection JSON di `docs/03_backend_api/postman_collection.json`.

---

## 🎨 Sprint 3: UI/UX & User Testing (Role: UI/UX Designer & Tester)
*Target: Desain Soft UI konsisten dan instrumen pengujian SUS siap uji ke pengguna.*

- [ ] **Figma Design System & Prototype**:
  - [ ] Ekspor token warna Soft UI (`#1C0FB3`, `#5BB8F5`, `#4CAF1F`, `#FFB84D`).
  - [ ] Alur 3 layar utama (Dashboard 2x3 tile, List search, Peta stacked panels).
- [ ] **Penyusunan Kuesioner SUS (System Usability Scale)**:
  - [ ] Pembuatan 10 pertanyaan standar skala Likert 1–5 di Google Form.
  - [ ] Target pengujian: Minimal 10 responden mahasiswa komuter (Target skor: $\ge 75$).
- [ ] **Laporan Analisis Hasil Pengujian (Usability Report)**:
  - [ ] Menghitung skor SUS dan kesimpulan kenyamanan fitur anti-mual & alarm.

---

## 📱 Sprint 4: Frontend Mobile Development (Role: Frontend Developer)
*Target: Aplikasi Flutter modular berjalan mulus, ada bodi HP preview, dan offline cache aktif.*

- [ ] **Migrasi & Scaffolding Flutter di `mobile/`**:
  - [ ] Struktur folder bersih: `core/`, `models/`, `screens/`, `widgets/`, `services/`.
- [ ] **Setup Device Preview**:
  - [ ] Integrasi mockup bingkai HP realistis (seperti di video TikTok).
- [ ] **Implementasi State Management (Provider)**:
  - [ ] `TripProvider` (status alarm, sisa jarak KM, halte tujuan).
  - [ ] `MotionCuesProvider` (status on/off titik anti-mual, opasitas).
  - [ ] `ChecklistProvider` (daftar checklist persiapan & barang bawaan).
  - [ ] `ExpenseProvider` (mode solo uang saku vs mode rombongan split bill).
- [ ] **Offline Database (SQLite / Hive)**:
  - [ ] Menyimpan data Nearby Spot & Checklist secara offline agar tidak butuh kuota saat di jalan.
- [ ] **Integrasi API Client (HTTP / Dio)**:
  - [ ] Mengambil data halte dan spot dari backend Laravel saat online.

---

## 🚀 Sprint 5: Integrasi Client-Server & Demo Sidang (Role: All Team)
*Target: Demo lancar di HP Infinix dan laptop, bahan presentasi dosen siap 100%.*

- [ ] **Testing Integrasi Full-Flow**:
  - [ ] Buka aplikasi -> Splash Screen -> Login/Offline Guest -> Buka Peta -> Cek Nearby Spot -> Nyalakan Alarm -> Uji Cues Anti-Mual -> Catat Pengeluaran -> Sync ke Laravel.
- [ ] **Bahan Sidang Tugas Akhir**:
  - [ ] Slide Pitch Deck 10 Halaman.
  - [ ] Video Demo 60 Detik di HP Infinix.
  - [ ] Dokumen Laporan Akhir (Bab 1 s/d Bab 5).

