# 🚆 CommuteMate (Monorepo)
> **All-in-One Passenger Utility & Solo Travel Companion**  
> *Solusi Utilitas Terintegrasi Penumpang: Anti-Mabuk Kendaraan (KinetoGuard), Smart Wake-Up Geofence, Pre-Trip Packing, Emergency Spot Finder, Fair Splitter, dan Solo Safety Guardian.*

---

## 👥 Struktur Tim & Pembagian Peran (Role Assignment)

| No | Peran Proyek | Anggota Tim | Tanggung Jawab Utama | Dokumen Panduan Kerja |
|:---:|---|---|---|---|
| **1** | **Project Manager (PM)** | *Ketua Tim / Anda* | Sprint backlog, scope project, timeline, sinkronisasi antar tim | [`PRD.md`](./PRD.md), [`TODO.md`](./TODO.md) |
| **2** | **UI/UX Designer & Tester** | *Anggota 2* | Desain wireframe Figma, prototipe interaktif, pengujian SUS | [`USER_FLOWS_AND_WIREFRAMES.md`](./docs/02_ui_ux_testing/USER_FLOWS_AND_WIREFRAMES.md), [`USABILITY_TESTING_SUS.md`](./docs/02_ui_ux_testing/USABILITY_TESTING_SUS.md) |
| **3** | **Frontend Mobile Developer** | *Anggota 3* | Flutter 3.24+, State Provider, SQLite offline, Device Preview browser | [`FRONTEND_ARCHITECTURE.md`](./docs/04_frontend_mobile/FRONTEND_ARCHITECTURE.md) |
| **4** | **Backend API Developer** | *Anggota 4* | Laravel 11 REST API, MySQL, Sanctum Auth, dataset POI offline pack | [`ERD_DATABASE.md`](./docs/03_backend_api/ERD_DATABASE.md), [`API_SPECIFICATION.md`](./docs/03_backend_api/API_SPECIFICATION.md) |

---

## 📂 Struktur Direktori Monorepo

```text
commute_mate/
├── docs/                                # Dokumentasi Standar Industri IT
│   ├── 01_project_management/           # PRD & Sprint Backlog
│   │   ├── PRD_COMMUTEMATE_FINAL.md
│   │   └── TODO_LIST.md
│   ├── 02_ui_ux_testing/                # User Flows, Wireframe, & SUS Testing
│   │   ├── USER_FLOWS_AND_WIREFRAMES.md
│   │   └── USABILITY_TESTING_SUS.md
│   ├── 03_backend_api/                  # ERD, Model Relasi, & REST API Contract
│   │   ├── ERD_DATABASE.md
│   │   └── API_SPECIFICATION.md
│   └── 04_frontend_mobile/              # Arsitektur Flutter, SQLite, & Device Preview
│       └── FRONTEND_ARCHITECTURE.md
│
├── mobile/                              # Source Code Klien Mobile (Flutter 3.24+)
├── backend/                             # Source Code Server REST API (Laravel 11)
├── PRD.md                               # Root Master PRD
├── TODO.md                              # Root Master Sprint Backlog
└── README.md                            # Executive Project Handbook
```

---

## 🚀 Panduan Memulai Cepat (Quick Start)

### 1. Pratinjau Mobile di Browser (Tampilan HP Interaktif - Seperti Referensi Anda)
Untuk menjalankan pratinjau Flutter di browser dengan frame smartphone fisik:
```bash
cd mobile
flutter pub get
flutter run -d chrome
```
*Aplikasi akan terbuka di browser dengan bezel iPhone/Android interaktif menggunakan `device_preview`.*

### 2. Menjalankan Backend Laravel REST API
```bash
cd backend
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan serve
```
*API berjalan pada `http://localhost:8000/api`.*

---

## 💡 7 Fitur Utama CommuteMate
1. **KinetoGuard HUD**: Visual cue pergerakan horizon perifer anti mabuk kendaraan (*Sensory Conflict Theory*).
2. **Smart Wake-Up & "Don't Leave Me"**: Alarm GPS 1.5 KM sebelum stasiun/tujuan + checklist darurat barang bawaan saat turun.
3. **Pre-Trip Travel Checklist**: Manajemen barang bawaan penting (Dompet, HP, Charger, Tiket, KTP) secara offline.
4. **Nearby Spot Explorer**: Penunjuk titik darurat penumpang (Toilet, Rest Area, SPBU, Tempat Makan, ATM).
5. **Trip Pocket & Fair Splitter**: Pencatat uang saku solo dan pembagi tagihan roadtrip dengan toggle *Apresiasi Driver*.
6. **Solo Safety Guardian**: Simulasi panggilan masuk palsu 1-tap untuk keamanan penumpang solo di taksi/kendaraan umum.
7. **Anti-Nausea Soundscape**: Stimulasi suara frekuensi 100 Hz untuk meredakan ketegangan organ telinga dalam.
