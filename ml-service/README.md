# ML Service — Plant Disease Classification Microservice

Microservice inferensi model AI untuk mendeteksi penyakit tanaman. Dibangun dengan **FastAPI** dan **TensorFlow**, layanan ini menerima URL gambar dari backend, menjalankan prediksi, dan mengembalikan label penyakit beserta confidence score.

---

## 📁 Struktur Folder

```
ml-service/
├── main.py           # Entry point aplikasi FastAPI
├── model.keras       # File model TensorFlow yang telah dilatih
└── requirements.txt  # Dependensi Python
```

---

## 🧠 Arsitektur Model

Model menggunakan arsitektur **EfficientNet** sebagai backbone dengan penambahan custom layer:

- **SEBlock (Squeeze-and-Excitation Block)** — meningkatkan representasi fitur channel-wise.
- **FocalLoss** — loss function untuk menangani ketidakseimbangan kelas.

Input gambar akan di-resize ke **224×224 piksel** dan diproses menggunakan `preprocess_input` standar EfficientNet sebelum inferensi.

---

## 🏷️ Label Kelas

| Indeks | Label            | Keterangan            |
|--------|------------------|-----------------------|
| 0      | `bacterial_spot` | Bacterial Spot        |
| 1      | `early_blight`   | Early Blight          |
| 2      | `healthy`        | Tanaman Sehat         |
| 3      | `late_blight`    | Late Blight           |

---

## 🔌 API Endpoint

### `GET /`
Mengecek status layanan.

**Response:**
```json
{
  "status": "running",
  "service": "ML Inference Service"
}
```

---

### `POST /predict`
Menjalankan klasifikasi penyakit dari URL gambar.

**Request Body:**
```json
{
  "imageUrl": "https://your-s3-bucket.amazonaws.com/path/to/image.jpg"
}
```

**Response Sukses (`200 OK`):**
```json
{
  "label": "bacterial_spot",
  "confidenceScore": 0.97
}
```

**Response Error:**

| Kode | Penyebab                                       |
|------|------------------------------------------------|
| 400  | Gagal mengambil gambar dari URL yang diberikan |
| 500  | Model belum berhasil dimuat / error inferensi  |

---

## ⚙️ Instalasi & Menjalankan Lokal

### Prasyarat

- Python >= 3.10

### Langkah Setup

1. **Masuk ke folder ml-service**
   ```bash
   cd ml-service
   ```

2. **Buat virtual environment** (disarankan)
   ```bash
   python -m venv venv
   source venv/bin/activate        # Linux/macOS
   venv\Scripts\activate           # Windows
   ```

3. **Install dependensi**
   ```bash
   pip install -r requirements.txt
   ```

4. **Jalankan server**
   ```bash
   uvicorn main:app --host 0.0.0.0 --port 8000 --reload
   ```

5. **Akses dokumentasi API interaktif**
   ```
   http://localhost:8000/docs
   ```

---

## 📦 Dependensi Utama

| Package      | Versi   | Keterangan                          |
|--------------|---------|-------------------------------------|
| fastapi      | 0.110.0 | Framework web async Python          |
| uvicorn      | 0.28.0  | ASGI server                         |
| tensorflow   | 2.15.0  | Framework deep learning             |
| pillow       | 10.2.0  | Pemrosesan gambar                   |
| numpy        | 1.26.4  | Komputasi numerik                   |
| pydantic     | 2.6.4   | Validasi data request/response      |
| requests     | 2.31.0  | HTTP client untuk fetch gambar S3   |

---

## 📝 Catatan

- File `model.keras` harus tersedia di direktori yang sama dengan `main.py`.
- Layanan ini dirancang untuk dipanggil secara internal oleh **Backend**, bukan oleh Frontend secara langsung.
- Gambar diambil langsung dari URL S3 yang diberikan oleh Backend, sehingga URL tersebut harus dapat diakses secara publik atau memiliki presigned URL yang valid.