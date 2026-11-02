# 🔌 CommuteMate - REST API Specification & Contract
**Dokumen Panduan Backend Developer (Laravel 11) & Frontend Developer (Flutter)**  
*Versi: 1.0.0 | Base URL: `http://localhost:8000/api` | Auth: Bearer Token (Laravel Sanctum)*

---

## 1. Standar Format Request & Response

### A. HTTP Headers
Semua permintaan (requests) harus menyertakan headers standar berikut:
```http
Accept: application/json
Content-Type: application/json
Authorization: Bearer <sanctum_plain_text_token>
```

### B. Struktur Respon Standar (Envelope Format)

**Respon Berhasil (HTTP 200 / 201):**
```json
{
  "success": true,
  "message": "Operasi berhasil dilakukan.",
  "data": { ... }
}
```

**Respon Gagal Validasi (HTTP 422 Unprocessable Entity):**
```json
{
  "success": false,
  "message": "Validasi data gagal.",
  "errors": {
    "email": ["Format email tidak valid."],
    "password": ["Kata sandi minimal 8 karakter."]
  }
}
```

**Respon Kesalahan Server / Auth (HTTP 401 / 500):**
```json
{
  "success": false,
  "message": "Sesi telah berakhir atau tidak memiliki otorisasi.",
  "data": null
}
```

---

## 2. Autentikasi & Akun Pengguna (`/api/auth`)

### 1. Registrasi Akun Baru
- **Endpoint**: `POST /auth/register`
- **Auth**: Publik (Tanpa Token)
- **Request Body**:
```json
{
  "name": "Daffa Penumpang",
  "email": "daffa@example.com",
  "password": "password123",
  "password_confirmation": "password123"
}
```
- **Response (201 Created)**:
```json
{
  "success": true,
  "message": "Akun berhasil didaftarkan.",
  "data": {
    "user": {
      "id": 1,
      "name": "Daffa Penumpang",
      "email": "daffa@example.com",
      "avatar_url": null,
      "created_at": "2026-11-02T10:00:00.000000Z"
    },
    "token": "1|qXyZ123456789abcdef..."
  }
}
```

### 2. Masuk Akun (Login)
- **Endpoint**: `POST /auth/login`
- **Auth**: Publik
- **Request Body**:
```json
{
  "email": "daffa@example.com",
  "password": "password123",
  "device_name": "Flutter Mobile App"
}
```
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "Login berhasil.",
  "data": {
    "user": {
      "id": 1,
      "name": "Daffa Penumpang",
      "email": "daffa@example.com"
    },
    "token": "2|Abc987654321..."
  }
}
```

### 3. Profil Pengguna Terautentikasi
- **Endpoint**: `GET /auth/me`
- **Auth**: Bearer Token
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "Profil berhasil dimuat.",
  "data": {
    "id": 1,
    "name": "Daffa Penumpang",
    "email": "daffa@example.com",
    "preferences": {
      "motion_sensitivity": "medium",
      "alarm_sound": "gentle_chime",
      "alarm_buffer_km": 1.5
    }
  }
}
```

### 4. Keluar Akun (Logout)
- **Endpoint**: `POST /auth/logout`
- **Auth**: Bearer Token
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "Sesi berhasil dihapus.",
  "data": null
}
```

---

## 3. Manajemen Perjalanan & Smart Wake-Up (`/api/trips`)

### 1. Daftar Perjalanan Pengguna
- **Endpoint**: `GET /trips`
- **Auth**: Bearer Token
- **Query Params**: `status` (active / completed / cancelled)
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "Daftar perjalanan berhasil dimuat.",
  "data": [
    {
      "id": "uuid-trip-001",
      "title": "Mudik Akhir Pekan Bandung - Tasik",
      "origin_name": "Terminal Cicaheum",
      "destination_name": "Pool Primajasa Tasikmalaya",
      "status": "active",
      "stops_count": 2,
      "created_at": "2026-11-02T12:00:00Z"
    }
  ]
}
```

### 2. Buat Perjalanan Baru dengan Titik Alarm (Destination Stop)
- **Endpoint**: `POST /trips`
- **Auth**: Bearer Token
- **Request Body**:
```json
{
  "title": "Perjalanan ke Kantor Sudirman",
  "origin_name": "Stasiun Bogor",
  "destination_name": "Stasiun Sudirman",
  "destination_stop": {
    "stop_name": "Stasiun Sudirman",
    "latitude": -6.2023,
    "longitude": 106.8228,
    "alarm_radius_meters": 1500,
    "alarm_sound": "zen_bell"
  }
}
```
- **Response (201 Created)**:
```json
{
  "success": true,
  "message": "Perjalanan berhasil dibuat.",
  "data": {
    "id": "uuid-trip-002",
    "title": "Perjalanan ke Kantor Sudirman",
    "status": "active",
    "destination_stop": {
      "id": 12,
      "stop_name": "Stasiun Sudirman",
      "latitude": -6.2023,
      "longitude": 106.8228,
      "alarm_radius_meters": 1500,
      "is_alarm_triggered": false
    }
  }
}
```

---

## 4. Nearby Spot Explorer (`/api/nearby-spots`)

### 1. Pencarian Titik Darurat Terdekat (Online Query)
- **Endpoint**: `GET /nearby-spots`
- **Auth**: Publik / Bearer Token
- **Query Params**:
  - `lat` (float, required): Latitude penumpang (-6.9175)
  - `lng` (float, required): Longitude penumpang (107.6191)
  - `category` (string, optional): `toilet` | `rest_area` | `spbu` | `food` | `minimarket`
  - `radius_km` (int, optional, default: 5): Radius pencarian (KM)
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "Nearby spots ditemukan.",
  "data": [
    {
      "id": 101,
      "name": "SPBU Pertamina 34.40116 & Toilet Bersih",
      "category": "spbu",
      "latitude": -6.9182,
      "longitude": 107.6205,
      "distance_meters": 230,
      "distance_human": "230 m",
      "has_toilet": true,
      "has_musholla": true,
      "is_24_hours": true,
      "address": "Jl. LLRE Martadinata No. 45"
    },
    {
      "id": 102,
      "name": "Indomaret Point Rest Point",
      "category": "minimarket",
      "latitude": -6.9195,
      "longitude": 107.6230,
      "distance_meters": 540,
      "distance_human": "540 m",
      "has_toilet": true,
      "has_musholla": false,
      "is_24_hours": true,
      "address": "Jl. Riau No. 88"
    }
  ]
}
```

### 2. Download Offline Pack (Dataset Kebutuhan Offline)
Fitur unggulan: Mengunduh dataset POI rute umum agar saat jaringan seluler putus di pegunungan/tol, aplikasi tetap bisa mencari toilet/SPBU.
- **Endpoint**: `GET /nearby-spots/offline-pack`
- **Auth**: Publik / Bearer Token
- **Query Params**: `region=jawa_barat`
- **Response (200 OK)**:
```json
{
  "success": true,
  "version": "2026.11.01",
  "region": "Jawa Barat & Rute Tol Trans-Jawa",
  "total_records": 450,
  "data": [
    {
      "id": 1,
      "name": "Rest Area KM 72 Cipularang",
      "category": "rest_area",
      "lat": -6.6432,
      "lng": 107.4521,
      "toilet": true,
      "spbu": true,
      "atm": true
    }
  ]
}
```

---

## 5. Travel Checklist (`/api/checklists`)

### 1. Sinkronisasi Checklist Offline ke Server (Sync Batch)
- **Endpoint**: `POST /checklists/sync`
- **Auth**: Bearer Token
- **Request Body**:
```json
{
  "items": [
    {
      "client_id": "local-item-1",
      "title": "Dompet & KTP",
      "category": "essential",
      "is_checked": true,
      "is_exit_reminder": true
    },
    {
      "client_id": "local-item-2",
      "title": "Powerbank 10.000 mAh",
      "category": "gadget",
      "is_checked": false,
      "is_exit_reminder": true
    }
  ]
}
```
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "2 checklist item berhasil disinkronkan.",
  "data": {
    "synced_count": 2,
    "last_sync_timestamp": "2026-11-02T15:30:00Z"
  }
}
```

---

## 6. Trip Pocket & Fair Splitter (`/api/trips/{id}/expenses`)

### 1. Tambah Pengeluaran Rombongan
- **Endpoint**: `POST /trips/{id}/expenses`
- **Auth**: Bearer Token
- **Request Body**:
```json
{
  "title": "Tiket Masuk & Bensin Tol",
  "amount": 160000,
  "paid_by_name": "Daffa",
  "category": "transport",
  "split_rule": "exclude_driver",
  "driver_name": "Andi (Pengemudi)",
  "passengers": ["Daffa", "Budi", "Citra", "Eka"]
}
```
- **Response (201 Created)**:
```json
{
  "success": true,
  "message": "Pengeluaran berhasil dicatat.",
  "data": {
    "id": 55,
    "title": "Tiket Masuk & Bensin Tol",
    "total_amount": 160000,
    "driver_excluded": true,
    "split_per_person": 40000,
    "breakdown": [
      { "name": "Andi (Pengemudi)", "owe": 0, "status": "Gratis Apresiasi Driver" },
      { "name": "Budi", "owe": 40000 },
      { "name": "Citra", "owe": 40000 },
      { "name": "Eka", "owe": 40000 },
      { "name": "Daffa", "owe": 0, "status": "Talangan (Sudah Bayar)" }
    ]
  }
}
```

### 2. Dapatkan Rekapitulasi Tagihan & WhatsApp Template
- **Endpoint**: `GET /trips/{id}/settlement`
- **Auth**: Bearer Token
- **Response (200 OK)**:
```json
{
  "success": true,
  "data": {
    "trip_title": "RoadTrip Akhir Pekan",
    "total_trip_cost": 320000,
    "driver_appreciation_active": true,
    "whatsapp_message_raw": "🚗 *REKAP PATUNGAN COMMUTEMATE*\nTrip: RoadTrip Akhir Pekan\nTotal Biaya: Rp 320.000\n*Apresiasi Driver Aktif: Driver tidak dibebani biaya*\n\n📌 *Rincian Transfer ke Daffa:*\n- Budi: Rp 80.000\n- Citra: Rp 80.000\n- Eka: Rp 80.000\n\nRekening BCA: 123456789 a/n Daffa\nTerima kasih teman-teman! 🎉"
  }
}
```

---

## 7. Solo Safety Guardian (`/api/safety`)

### 1. Sinkronisasi Kontak Darurat
- **Endpoint**: `POST /safety/contacts`
- **Auth**: Bearer Token
- **Request Body**:
```json
{
  "contacts": [
    {
      "name": "Ibu",
      "phone": "+6281234567890",
      "is_primary": true
    },
    {
      "name": "Sahabat Kampus (Rian)",
      "phone": "+6289876543210",
      "is_primary": false
    }
  ]
}
```
- **Response (200 OK)**:
```json
{
  "success": true,
  "message": "Kontak darurat berhasil disimpan.",
  "data": { "saved_count": 2 }
}
```
