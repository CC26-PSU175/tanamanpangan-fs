<img src="frontend/src/assets/favicon.png" width="400" alt="Chloris Garden">
# Chloris Garden - Plant Disease Classification App

Aplikasi fullstack untuk klasifikasi kesehatan tanaman pangan menggunakan kecerdasan buatan. Pengguna dapat mengunggah foto daun tanaman dan mendapatkan hasil deteksi penyakit beserta informasi penanganannya secara real-time.

---

## 📁 Struktur Repository

```
tanamanpangan-fs/
├── frontend/        # Aplikasi web React + Vite + Tailwind CSS
├── backend/         # REST API Node.js + Express + Prisma + PostgreSQL
└── ml-service/      # Microservice inferensi model AI (FastAPI + TensorFlow)
```

---

## 🧩 Arsitektur Sistem

```
                ┌─────────────┐       REST API        ┌─────────────────┐
                │   Frontend  │ ──────────────────▶  │    Backend       │
                │  (React)    │ ◀──────────────────  │  (Express.js)    │
                └─────────────┘                       └────────┬────────┘
                                                            │ HTTP (POST /predict)
                                                            ▼
                                                    ┌─────────────────┐
                                                    │   ML Service    │
                                                    │  (FastAPI +     │
                                                    │  TensorFlow)    │
                                                    └─────────────────┘
```

- **Frontend** mengirimkan gambar ke Backend melalui REST API.
- **Backend** menyimpan gambar ke AWS S3, lalu meneruskan URL gambar ke ML Service.
- **ML Service** menjalankan inferensi model dan mengembalikan label penyakit beserta confidence score.
- Hasil klasifikasi disimpan ke database PostgreSQL (via Prisma) dan dikembalikan ke Frontend.

---

## 🚀 Memulai Pengembangan Lokal

### Prasyarat

- Node.js >= 18
- Python >= 3.10
- PostgreSQL
- AWS S3 bucket (untuk penyimpanan gambar)

### Langkah Setup

1. **Clone repository**
   ```bash
   git clone https://github.com/Klasifikasi-Kesehatan-Tanaman/tanamanpangan-fs.git
   cd tanamanpangan-fs
   ```

2. **Jalankan setiap layanan** sesuai petunjuk README di masing-masing folder:
   - [`frontend/README.md`](./frontend/README.md)
   - [`backend/README.md`](./backend/README.md)
   - [`ml-service/README.md`](./ml-service/README.md)

---

## ✨ Fitur Utama

- **Klasifikasi penyakit tanaman** berbasis gambar menggunakan model deep learning (EfficientNet + SE Block)
- **Dashboard statistik** ringkasan aktivitas scan pengguna
- **Riwayat klasifikasi** dengan filter, pencarian, dan paginasi
- **Autentikasi pengguna** dengan JWT (register, login, forgot/reset password)
- **Manajemen profil** termasuk foto profil via AWS S3
- **Ensiklopedia penyakit** — daftar penyakit beserta gejala, penyebab, dan cara penanganan

---

## 🦠 Penyakit yang Didukung (Saat ini)

| Label Model      | Nama Penyakit   |
|------------------|-----------------|
| `bacterial_spot` | Bacterial Spot  |
| `early_blight`   | Early Blight    |
| `late_blight`    | Late Blight     |
| `healthy`        | Sehat           |

---

## 🛠️ Tech Stack

| Komponen    | Teknologi                                      |
|-------------|------------------------------------------------|
| Frontend    | React 19, Vite, Tailwind CSS 4, React Router   |
| Backend     | Node.js, Express 5, Prisma, PostgreSQL, JWT    |
| ML Service  | Python, FastAPI, TensorFlow 2.15, EfficientNet |
| Storage     | AWS S3                                         |

---

## 👥 Kontribusi

Kontribusi sangat disambut! Silakan buat *issue* atau *pull request* untuk perbaikan dan penambahan fitur.

---

## 📄 Lisensi

Proyek ini merupakan proyek open-source. Silakan merujuk ke masing-masing subfolder untuk informasi lisensi lebih lanjut.