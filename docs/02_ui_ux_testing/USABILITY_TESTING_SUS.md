# 🧪 CommuteMate - Usability Testing & SUS Evaluation Guide
**Dokumen Panduan UI/UX Designer & User Testing Specialist**  
*Metodologi: System Usability Scale (SUS) Standard (Brooke, 1996) & Scenario-Based Testing*

---

## 1. Rencana Pengujian Pengguna (User Testing Plan)

### A. Profil Partisipan Uji
- **Jumlah Responden**: 5 Partisipan (Sesuai kaidah Nielsen Norman Group untuk mendeteksi 85% masalah kegunaan).
- **Karakteristik**: Mahasiswa / komuter yang sering menggunakan transportasi umum (bus, kereta komuter, taksi online, travel antarkota) atau road trip bersama teman.
- **Kondisi Pengujian**:
  1. *Lab / Controlled Test*: Menggunakan laptop dengan browser Chrome (`device_preview`) untuk evaluasi alur antarmuka dan visual.
  2. *Field Test*: Pengujian langsung di dalam kendaraan bergerak untuk menguji respon KinetoGuard dan keakuratan Smart Wake-Up.

---

## 2. Skenario Tugas Pengujian (Usability Task Scenarios)

Penguji (UI/UX Tester) memberikan instruksi berikut kepada pengguna tanpa mengarahkan tombol mana yang harus ditekan:

| No | Nama Tugas | Instruksi untuk Partisipan | Kriteria Keberhasilan (Success Metric) |
|---|---|---|---|
| **T1** | **Aktivasi KinetoGuard** | *"Anda sedang duduk di bus dan mulai merasa pusing saat membaca ponsel. Aktifkan fitur anti-mual."* | Pengguna menekan tombol FAB Cyan di navigasi bawah atau kartu KinetoGuard dalam < 5 detik. |
| **T2** | **Pasang Alarm Bangun** | *"Anda ingin tidur selama perjalanan menuju 'Stasiun Sudirman' dan ingin dibangunkan 1.5 KM sebelum sampai."* | Pengguna memilih destinasi, mengatur radius geofence, dan mengaktifkan alarm. |
| **T3** | **Checklist Persiapan** | *"Sebelum berangkat, pastikan dompet dan tiket sudah dibawa, lalu tambahkan barang kustom 'Obat Anti Mual'."* | Pengguna mencentang 2 item dan berhasil menambahkan 1 item baru ke daftar. |
| **T4** | **Cari Toilet Terdekat** | *"Tiba-tiba Anda ingin buang air kecil saat di perjalanan. Temukan toilet atau SPBU terdekat."* | Pengguna membuka Nearby Spot Explorer, memilih kategori 'Toilet/SPBU', dan melihat jarak. |
| **T5** | **Bagi Tagihan RoadTrip** | *"Anda baru saja membayar bensin sebesar Rp 150.000 untuk 3 orang penumpang. Driver tidak dibebankan. Buat rinciannya untuk dikirim ke WhatsApp."* | Pengguna mengaktifkan toggle 'Apresiasi Driver' dan meng-copy ringkasan tagihan WhatsApp. |
| **T6** | **Simulasi Panggilan Aman** | *"Anda merasa sopir taksi mencurigakan. Pancing panggilan telepon palsu dalam 10 detik untuk alasan keamanan."* | Pengguna membuka Solo Safety, memilih timer 10 detik, dan berhasil memunculkan layar panggilan masuk. |

---

## 3. Kuesioner System Usability Scale (SUS) - Bahasa Indonesia

Setelah menyelesaikan seluruh tugas, partisipan mengisi 10 butir pernyataan SUS dengan skala Likert 1 (Sangat Tidak Setuju) hingga 5 (Sangat Setuju):

| No | Pernyataan Instrumen SUS | 1 (STS) | 2 (TS) | 3 (N) | 4 (S) | 5 (SS) |
|---|---|:---:|:---:|:---:|:---:|:---:|
| **Q1** | Saya merasa ingin sering menggunakan aplikasi CommuteMate saat bepergian. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q2** | Saya merasa aplikasi ini terlalu rumit untuk dipahami. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q3** | Saya merasa aplikasi ini mudah digunakan sejak pertama kali mencoba. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q4** | Saya merasa membutuhkan bantuan orang lain atau teknisi untuk bisa menggunakan aplikasi ini. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q5** | Saya merasa berbagai fitur di aplikasi ini terintegrasi dengan sangat baik. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q6** | Saya merasa ada terlalu banyak hal yang tidak konsisten di dalam aplikasi ini. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q7** | Saya yakin kebanyakan orang akan cepat belajar menggunakan aplikasi ini. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q8** | Saya merasa aplikasi ini membingungkan dan canggung saat dioperasikan. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q9** | Saya merasa sangat percaya diri saat menggunakan fitur-fitur di aplikasi ini. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |
| **Q10**| Saya harus belajar banyak hal terlebih dahulu sebelum bisa lancar menggunakan aplikasi ini. | ⭕ | ⭕ | ⭕ | ⭕ | ⭕ |

---

## 4. Rumus Perhitungan Skor SUS (Scoring Formula)

```text
Skor Akhir SUS = Total Poin Kontribusi x 2.5
Rentang Skor: 0 s/d 100
```

### Langkah Perhitungan:
1. **Untuk Pertanyaan Bernomor Ganjil (Q1, Q3, Q5, Q7, Q9 - Pernyataan Positif)**:
   $$\text{Poin} = \text{Skor Pengguna} - 1$$
2. **Untuk Pertanyaan Bernomor Genap (Q2, Q4, Q6, Q8, Q10 - Pernyataan Negatif)**:
   $$\text{Poin} = 5 - \text{Skor Pengguna}$$
3. **Jumlahkan seluruh poin dari 10 pertanyaan** (Hasil total berada di antara 0 s/d 40).
4. **Kalikan total poin dengan 2.5** untuk mendapatkan skala akhir 0 s/d 100.

---

## 5. Standar Tolok Ukur Keberhasilan (SUS Benchmark Scale)

Target nilai aplikasi CommuteMate untuk rilis produksi dan penilaian mata kuliah:

```text
Skor SUS    | Predikat Huruf | Tingkat Penerimaan | Deskripsi (Adjective)
--------------------------------------------------------------------------
>= 80.3     | Grade A        | Acceptable         | Excellent / Luar Biasa
68.0 - 80.2 | Grade B        | Acceptable         | Good (Standar Rata-Rata Industri = 68)
51.0 - 67.9 | Grade C        | Marginal           | OK / Butuh Perbaikan Alur
< 51.0      | Grade F        | Not Acceptable     | Poor / Gagal Uji Kegunaan
```

> **Target Tim CommuteMate**:  
> **Skor SUS Minimal: $\ge 75.0$ (Grade B+ / Good)** dengan **Completion Rate Tugas $\ge 90\%$**.

