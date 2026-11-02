# 📱 CommuteMate - User Flows & Wireframe Specifications
**Dokumen Panduan UI/UX Designer & User Testing**  
*Versi: 1.0.0 | Status: Official Engineering Specs*

---

## 1. Arsitektur Informasi & Peta Navigasi (Navigation Map)

Aplikasi CommuteMate dirancang dengan pola navigasi **Single-Screen Focused + Floating Curved Wave Bar** dengan Tombol Aksi Utama (FAB) di tengah.

```mermaid
graph TD
    Splash[0. Animated Splash Screen] --> Onboarding[1. Onboarding & Sensor Permission]
    Onboarding --> LoginModal[2. Login / Register / Guest Mode]
    LoginModal --> Home[3. Dashboard Beranda]
    
    Home --> Tab1[Tab 1: Dashboard Ringkasan & 2x3 Grid]
    Home --> Tab2[Tab 2: Travel Checklist Pack]
    Home --> FAB((FAB: KinetoGuard HUD Mode))
    Home --> Tab3[Tab 3: Nearby Spot Explorer]
    Home --> Tab4[Tab 4: Trip Pocket & Fair Splitter]
    
    Tab1 --> WakeUpScreen[Fitur: Smart Wake-Up & Exit Checklist]
    Tab1 --> FakeCallScreen[Fitur: Solo Safety Guardian]
    Tab1 --> ZenReaderScreen[Fitur: Anti-Nausea Soundscape & Zen Reader]
    
    WakeUpScreen --> GeofenceTrigger[Alarm 1.5 KM Sebelum Tiba]
    GeofenceTrigger --> ExitModal[Modal Dialog: Jangan Ada yang Ketinggalan!]
    
    FakeCallScreen --> TimerSetup[Set Waktu Panggilan 10s - 5m]
    TimerSetup --> CallIncoming[Layar Panggilan Masuk Realistis]
```

---

## 2. End-to-End User Flows (7 Fitur Utama)

### Flow 1: Onboarding, Sensor Permission & Mode Akses
```mermaid
sequenceDiagram
    autonumber
    actor User as Pengguna (Penumpang)
    participant App as CommuteMate Mobile
    participant Sensor as Gyroscope & GPS Native
    participant Storage as Local SQLite / API
    
    User->>App: Membuka Aplikasi
    App->>User: Menampilkan Animated Splash (Logo & Gelombang Cyan)
    App->>User: Layar Edukasi Kinetosis & Utilitas Penumpang
    User->>App: Tap "Mulai Petualangan"
    App->>Sensor: Meminta Izin Akses Sensor Gerak (Sensors) & Lokasi (GPS)
    Sensor-->>App: Izin Diberikan (Granted)
    App->>User: Opsi Masuk (Login / Register / "Masuk sebagai Tamu / Offline")
    User->>App: Memilih "Mode Tamu (Offline)"
    App->>Storage: Inisialisasi Database SQLite Lokal
    App->>User: Masuk ke Dashboard Utama
```

---

### Flow 2: KinetoGuard (HUD Anti-Mabuk Perjalanan)
Fitur inti pencegah mabuk kendaraan menggunakan peripheral visual cue berbasis teori *Sensory Conflict*.

```mermaid
flowchart TD
    Start[User di dalam Bus/Mobil/Kereta] --> TapFAB[Tap Tombol FAB Cyan di Tengah]
    TapFAB --> OpenHUD[Buka KinetoGuard Overlay HUD]
    OpenHUD --> SensorListen[Sensor IMU: Baca Akselerasi X, Y, Z]
    SensorListen --> CalcRollPitch[Hitung Roll & Pitch Dinamis Kendaraan]
    CalcRollPitch --> AnimateDots[Animasi Titik/Garis Horizon Virtual di Pinggir Layar]
    
    AnimateDots --> UserReading{Pengguna Sambil Membaca / Scrolling?}
    UserReading -- Ya --> EyesNoticeDots[Mata Menangkap Gerakan Visual Perifer Sesuai Gaya Gravitasi]
    EyesNoticeDots --> ResolveConflict[Menghilangkan Konflik Organ Vestibular & Visual]
    ResolveConflict --> NauseaPrevented[Mual & Pusing Tercegah]
    UserReading -- Tidak --> KeepFloating[Floating Widget tetap aktif di latar belakang]
    
    KeepFloating --> CloseHUD[Tap Tombol Tutup / Minimize]
    CloseHUD --> BackToApp[Kembali ke Layar Sebelumnya]
```

---

### Flow 3: Smart Wake-Up & "Don't Leave Me" Exit Checklist
Alarm jarak berbasis geofence yang membangunkan penumpang 1.5 km sebelum tujuan dan memunculkan pop-up checklist barang bawaan.

```mermaid
flowchart TD
    StartWake[User hendak tidur di bus/kereta] --> OpenWakeUp[Buka Fitur Smart Wake-Up]
    OpenWakeUp --> SearchDest[Ketik / Pilih Destinasi Turun]
    SearchDest --> SetBuffer[Tentukan Radius Bangun: Default 1.5 KM]
    SetBuffer --> ActivateAlarm[Tap 'Aktifkan Smart Wake-Up']
    
    ActivateAlarm --> BackgroundGPS[Geolocator Memantau Jarak via Haversine Formula]
    BackgroundGPS --> DistanceCheck{Jarak sisa <= 1.5 KM?}
    DistanceCheck -- Belum --> WaitNext[Interval Cek GPS Hemat Baterai Tiap 20 Detik]
    WaitNext --> BackgroundGPS
    
    DistanceCheck -- Ya --> RingAlarm[Bunyikan Alarm Suara Lembut Bertahap + Getar]
    RingAlarm --> ShowExitModal[Munculkan Layar Modal: 'JANGAN TINGGALKAN BARANG!']
    ShowExitModal --> CheckItems[User Cek: Dompet? HP? Tiket? Bagasi?]
    CheckItems --> UserConfirms[Semua Dicentang -> Tap 'Saya Sudah Aman Turun']
    UserConfirms --> StopAlarm[Matikan Alarm & Catat Riwayat Perjalanan]
```

---

### Flow 4: Pre-Trip Travel Checklist
```mermaid
flowchart LR
    OpenPack[Buka Menu Checklist] --> ViewDefault[Lihat Barang Bawaan Standar: Dompet, HP, Charger, Tiket, KTP]
    ViewDefault --> AddItem[Tap '+ Tambah Barang Kustom']
    AddItem --> InputTitle[Ketik Nama Barang & Kategori]
    InputTitle --> SaveLocal[Simpan ke SQLite Lokal]
    SaveLocal --> TogglePack[Tap Checkbox saat Memasukkan ke Tas]
    TogglePack --> ProgressCalc[Update Progress Bar: 4/5 (80% Siap)]
```

---

### Flow 5: Nearby Spot Passenger Explorer (Tempat Darurat Terdekat)
```mermaid
flowchart TD
    OpenNearby[Buka Menu Nearby Spot] --> DetectLoc[Ambil Koordinat Terkini GPS]
    DetectLoc --> SelectCat{Pilih Kategori Kebutuhan}
    SelectCat --> Cat1[Toilet Umum]
    SelectCat --> Cat2[Rest Area]
    SelectCat --> Cat3[SPBU / Pertamina]
    SelectCat --> Cat4[Tempat Makan / Kuliner Singgah]
    SelectCat --> Cat5[Minimarket / ATM Darurat]
    
    Cat1 & Cat2 & Cat3 & Cat4 & Cat5 --> QueryLocal[Filter dari Dataset SQLite Lokal - Mode Offline]
    QueryLocal --> SortDistance[Urutkan Berdasarkan Jarak Terdekat (Meter/KM)]
    SortDistance --> RenderCards[Tampilkan List Card: Jarak, Fasilitas, Estimasi Jalan/Berkendara]
    RenderCards --> TapCard[Tap Card Pilihan]
    TapCard --> ShowRoute[Tampilkan Arah & Pin Peta Sederhana]
```

---

### Flow 6: Trip Pocket & Fair Splitter (Bagi Tagihan & Uang Saku)
```mermaid
flowchart TD
    OpenSplit[Buka Trip Pocket & Splitter] --> ChooseMode{Pilih Mode Penggunaan}
    
    ChooseMode -- Solo Passenger --> SoloMode[Mode Solo: Catat Uang Saku Harian & Pengeluaran]
    SoloMode --> InputSolo[Input: Beli Cemilan Rest Area Rp 25.000]
    InputSolo --> CalcRemain[Update Sisa Budget Saku]
    
    ChooseMode -- Group RoadTrip --> GroupMode[Mode RoadTrip Bareng Rombongan]
    GroupMode --> AddFriends[Tambah Nama Teman: Andi, Budi, Citra, Daffa]
    GroupMode --> AddBill[Input Pengeluaran: Tol Rp 80.000, Bensin Rp 150.000]
    GroupMode --> DriverToggle{Aktifkan 'Apresiasi Driver'?}
    DriverToggle -- Ya (Aktif) --> ExcludeDriver[Beban Driver Rp 0, Tagihan Dibagi Rata ke Penumpang Saja]
    DriverToggle -- Tidak --> SplitAll[Tagihan Dibagi Rata Termasuk Driver]
    
    ExcludeDriver & SplitAll --> SummaryScreen[Layar Ringkasan Pembagian Tagihan]
    SummaryScreen --> ExportWA[Tap 'Bagikan ke WhatsApp']
    ExportWA --> GenerateMarkdown[Generate Pesan Teks Rapi Otomatis & Buka WhatsApp]
```

---

### Flow 7: Solo Safety Guardian (Panggilan Darurat Palsu)
```mermaid
flowchart LR
    DangerFeel[Penumpang merasa tidak aman / curiga di taksi] --> QuickTap[Buka Solo Safety Guardian]
    QuickTap --> SelectDelay[Pilih Jeda Waktu: 10 Detik / 30 Detik / Sekarang]
    SelectDelay --> SelectCaller[Pilih Identitas Penelepon: 'Ayah' / 'Kakak' / 'Polisi']
    SelectCaller --> WaitDelay[Layar Tetap Tenang / Terkunci]
    WaitDelay --> TriggerCall[Layar Menampilkan Panggilan Telepon Masuk Persis Asli]
    TriggerCall --> PlayRingtone[Suara Nada Dering Standar Ponsel Berbunyi]
    PlayRingtone --> AcceptCall[User Geser Jawab: 'Halo Ayah, ini aku udah di deket jembatan...']
```

---

## 3. Spesifikasi Wireframe & Komponen Visual

### A. Palet Warna (Design Tokens)
- **Primary Navy / Royal Blue**: `#1C0FB3` (Elegan, kredibel, memberikan rasa aman)
- **Accent Motion Cyan**: `#5BB8F5` (Dinamis, aksen futuristik, visual guide)
- **Light Slate Background**: `#F4F6FB` (Bersih, ramah mata saat kendaraan berguncang)
- **Soft Card Surface**: `#FFFFFF` dengan shadow `rgba(70, 80, 160, 0.08)` dan radius `20px`
- **Warning / Emergency Amber**: `#FF9F43` (Untuk status alarm dan pengingat)
- **Success Safe Mint**: `#10B981` (Untuk checklist selesai dan radius aman)

### B. Anatomi Komponen Antarmuka (UI Anatomy)
1. **Header Penumpang**:
   - Status Perjalanan Aktif (misal: *"Menuju: Terminal Leuwipanjang - 4.2 KM tersisa"*).
   - Chip Indikator Status Sensor (GPS Aktif, KinetoGuard Siap).
2. **Dashboard 2x3 Grid (Soft Touch Cards)**:
   - Kartu 1: *KinetoGuard HUD* (Icon Wave Gyro, Aksen Biru Muda).
   - Kartu 2: *Smart Wake-Up* (Icon Lonceng Geofence, Badge Jarak).
   - Kartu 3: *Travel Checklist* (Icon Koper & Check, Progress Bar `4/5`).
   - Kartu 4: *Nearby Spot Explorer* (Icon Peta & Pin, Kategori Cepat).
   - Kartu 5: *Fair Splitter & Pocket* (Icon Dompet & Bagi Tagihan).
   - Kartu 6: *Solo Safety Guardian* (Icon Perisai & Telepon Cepat).
3. **Curved Wave Bottom Navigation Bar**:
   - 4 Tab Menu: Beranda (Home), Checklist (Packing), Tempat (Nearby), Dompet (Splitter).
   - Floating Action Button (FAB) di tengah dengan warna Cyan `#5BB8F5` untuk aktivasi instan KinetoGuard.

