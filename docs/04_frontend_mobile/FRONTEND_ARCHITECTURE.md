# 📱 CommuteMate - Frontend Architecture & Device Preview Guide
**Dokumen Panduan Frontend Developer (Flutter 3.24+)**  
*Versi: 1.0.0 | State Management: Provider | Offline Engine: SQLite / SharedPreferences*

---

## 1. Arsitektur Folder Proyek (Feature-First Clean Pattern)

Struktur kode Flutter dirancang dengan pendekatan modular berbasis fitur (*Feature-First*) agar mudah dikembangkan bersama oleh tim dan mudah dipelihara.

```text
mobile/
├── lib/
│   ├── core/                        # Modul Inti & Fondasi Aplikasi
│   │   ├── constants/
│   │   │   ├── app_colors.dart      # Navy #1C0FB3, Cyan #5BB8F5, Slate #F4F6FB
│   │   │   ├── app_dimensions.dart  # Radius 20px, Padding 16px/24px
│   │   │   └── app_typography.dart  # Font Poppins / Plus Jakarta Sans
│   │   ├── database/
│   │   │   ├── database_helper.dart # Inisialisasi SQLite lokal (Offline Database)
│   │   │   └── tables/              # Schema tabel offline (checklists, spots, trips)
│   │   ├── network/
│   │   │   ├── api_client.dart      # Dio / Http client dengan Interceptor Bearer Token
│   │   │   └── api_endpoints.dart   # URL constants ke Laravel backend
│   │   └── services/
│   │       ├── sensor_service.dart  # Accelerometer & Gyroscope stream
│   │       ├── geofence_service.dart# GPS background proximity & Haversine calculator
│   │       └── audio_service.dart   # 100 Hz tone generator & ringtone player
│   │
│   ├── features/                    # Modul 7 Fitur Utama
│   │   ├── 01_splash_onboarding/    # Animated splash & permission flow
│   │   ├── 02_kinetoguard/          # Peripheral visual motion HUD & dot horizon
│   │   ├── 03_smart_wakeup/         # GPS alarm 1.5 KM & exit checklist pop-up
│   │   ├── 04_travel_checklist/     # Pre-trip packing tracker (Offline CRUD)
│   │   ├── 05_nearby_spot/          # Emergency POI finder & offline JSON reader
│   │   ├── 06_trip_pocket/          # Solo budget & Fair Splitter (WhatsApp exporter)
│   │   └── 07_solo_safety/          # 1-Tap realistic fake call simulator
│   │
│   ├── shared_widgets/              # Komponen Reusable
│   │   ├── soft_card.dart           # Floating card putih dengan shadow lembut
│   │   ├── curved_nav_bar.dart      # Organic wave bottom bar dengan FAB Cyan
│   │   ├── status_chip.dart         # Pill tag status sensor & gps
│   │   └── custom_button.dart       # Primary rounded button
│   │
│   └── main.dart                    # Entry point aplikasi + Device Preview wrapper
```

---

## 2. Rahasia Tampilan Phone Preview di Browser (Seperti Foto Referensi)

Foto yang dikirimkan menampilkan aplikasi yang berjalan di browser (`localhost`) dengan **frame bezel fisik smartphone realistis** lengkap dengan Dynamic Island / Speaker, tombol fisik samping, dan kontrol rotasi.

### Menggunakan Package `device_preview`
Untuk mendapatkan tampilan persis seperti pada foto saat presentasi ke dosen atau saat tim UI/UX menguji desain di laptop:

1. **Tambahkan dependencies di `pubspec.yaml`**:
```yaml
dependencies:
  flutter:
    sdk: flutter
  provider: ^6.1.2
  device_preview: ^1.2.0        # <--- Kunci tampilan frame HP di browser
  sqflite: ^2.3.3+1
  path_provider: ^2.1.4
  sensors_plus: ^5.0.1          # Baca gyro & accelerometer
  geolocator: ^12.0.0            # Hitung jarak geofence
  audioplayers: ^6.0.0          # Suara alarm & 100 Hz soundscape
  http: ^1.2.1
  shared_preferences: ^2.2.3
```

2. **Implementasikan di `lib/main.dart`**:
```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:device_preview/device_preview.dart';

void main() {
  runApp(
    DevicePreview(
      // Aktifkan preview hanya saat mode debug (atau kIsWeb di browser)
      enabled: !kReleaseMode,
      builder: (context) => const CommuteMateApp(),
    ),
  );
}

class CommuteMateApp extends StatelessWidget {
  const CommuteMateApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'CommuteMate',
      debugShowCheckedModeBanner: false,
      // Integrasi DevicePreview dengan tema dan navigasi
      useInheritedMediaQuery: true,
      locale: DevicePreview.locale(context),
      builder: DevicePreview.appBuilder,
      theme: ThemeData(
        fontFamily: 'Poppins',
        scaffoldBackgroundColor: const Color(0xFFF4F6FB),
        primaryColor: const Color(0xFF1C0FB3),
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF1C0FB3),
          secondary: const Color(0xFF5BB8F5),
        ),
      ),
      home: const MainNavigationShell(),
    );
  }
}
```

3. **Jalankan di Chrome / Edge**:
```bash
flutter run -d chrome
```
> **Hasil**: Browser akan membuka `http://localhost:<port>` dengan bezel smartphone iPhone / Android yang interaktif, bisa diganti jenis perangkatnya, diubah orientasi potret/lanskap, dan dites langsung tanpa perlu emulator Android Studio yang berat!

---

## 3. Strategi Penyimpanan Offline-First (SQLite + SharedPreferences)

Agar aplikasi **tetap ringan**, hemat kuota, dan tidak bergantung pada internet saat perjalanan di daerah tanpa sinyal:

```mermaid
flowchart TD
    UserAction[Aksi Pengguna: Buka Checklist / Cari Spot] --> CheckCache[Periksa Local Database SQLite]
    CheckCache --> HasData{Data Lokal Ada?}
    HasData -- Ada --> RenderUI[Langsung Render UI Instan (0 ms Latency)]
    HasData -- Kosong --> CheckNet{Ada Koneksi Internet?}
    CheckNet -- Ya --> FetchAPI[Tarik Data dari Laravel REST API]
    FetchAPI --> SaveSQLite[Simpan ke SQLite sebagai Cache Permanen]
    SaveSQLite --> RenderUI
    CheckNet -- Tidak --> FallbackSeed[Gunakan Default Seed Dataset Bawaan APK]
    FallbackSeed --> RenderUI
    
    RenderUI --> OfflineEdit[Pengguna Menambah / Ceklis Barang]
    OfflineEdit --> MarkDirty[Tandai Flag: is_synced = 0 di SQLite]
    MarkDirty --> BackgroundSync[Ketika Internet Terkoneksi: Otomatis Sinkronisasi ke Backend]
```

### Schema Database SQLite Lokal (`commute_mate.db`):
- **Tabel `local_checklists`**: Menyimpan checklist barang bawaan penumpang.
- **Tabel `cached_nearby_spots`**: Menyimpan titik POI (Toilet, SPBU, Rest Area) rute umum Jawa Barat & Trans Jawa.
- **Tabel `local_trips`**: Menyimpan data perjalanan aktif dan koordinat alarm geofence.

---

## 4. Implementasi Sensor KinetoGuard (Anti-Motion Sickness)

Menggunakan package `sensors_plus` untuk membaca kemiringan kendaraan secara halus (*low-pass filtered*) agar titik horizon perifer bergerak secara alami:

```dart
// Rumus Low-Pass Filter untuk pergerakan titik halus di layar
double filteredPitch = 0.0;
double filteredRoll = 0.0;
const double alpha = 0.15; // Faktor kehalusan redaman guncangan

accelerometerEvents.listen((AccelerometerEvent event) {
  // Hitung sudut kemiringan berdasarkan gravitasi
  double targetPitch = event.y / 9.8;
  double targetRoll = event.x / 9.8;
  
  // Terapkan filter agar titik tidak bergetar liar
  filteredPitch = (alpha * targetPitch) + ((1.0 - alpha) * filteredPitch);
  filteredRoll = (alpha * targetRoll) + ((1.0 - alpha) * filteredRoll);
  
  notifyListeners();
});
```

---

## 5. Aturan Kode Khusus (Code Standards)
1. **Flutter 3.24 Compatibility**:
   - ⚠️ Hindari pemanggilan `Color.withValues()` karena menyebabkan error runtime pada sebagian compiler.
   - ✅ Selalu gunakan format standar: `color.withOpacity(0.12)`.
2. **Resource Management**:
   - Semua `StreamSubscription` sensor (GPS & Gyro) wajib di-`cancel()` saat widget di-dispose agar tidak menguras baterai ponsel penumpang.

