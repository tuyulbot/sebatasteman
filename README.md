```markdown
# PPOB API High Performance

API berbasis FastAPI untuk transaksi PPOB (pulsa/token listrik/produk digital), mendukung autentikasi API-Key yang aman, sistem user, business logic async siap ratusan concurrent user, dan response data konsisten JSON. Disiapkan untuk production dengan basis MySQL.


## Struktur Direktori

```
ppob-api/
├── app.py        # API utama (endpoint, authentikasi, response wrapper)
├── utils.py      # Generator API Key, hash function
├── proses.py     # Dummy proses PPOB eksternal (optional, custom logic di sini)
├── .env          # Config koneksi DB (jangan commit ke repo public!)
└── README.md     # Dokumentasi ini
```

---

## 1. Install Dependencies

Buat virtualenv (opsional), lalu install:

```bash
pip install requirements.txt
```

---

## 2. Konfigurasi Database

### .env contoh:

```
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=ppob
```

### SQL Struktur Database

```sql
CREATE DATABASE IF NOT EXISTS ppob CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE ppob;

CREATE TABLE user (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nama VARCHAR(50) NOT NULL,
  email VARCHAR(100) NOT NULL UNIQUE,
  hp VARCHAR(20),
  saldo INT DEFAULT 0,
  api_key VARCHAR(64) NOT NULL UNIQUE,
  private_key VARCHAR(64) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE produk (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nama VARCHAR(50),
  harga INT,
  deskripsi TEXT
);

CREATE TABLE transaksi (
  id INT AUTO_INCREMENT PRIMARY KEY,
  trx_id VARCHAR(20) NOT NULL UNIQUE,
  produk_id INT,
  nomor_tujuan VARCHAR(20),
  nominal INT,
  status VARCHAR(20),
  hasil_eksekusi TEXT,
  waktu TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Seed sample produk PPOB
INSERT INTO produk (nama, harga, deskripsi) VALUES
('Pulsa 10k', 12000, 'Pulsa Reguler 10.000'),
('Pulsa 20k', 22000, 'Pulsa Reguler 20.000'),
('Token Listrik 50k', 51000, 'Token Listrik Prabayar 50.000')
ON DUPLICATE KEY UPDATE nama=VALUES(nama);
```

---

## 3. Menjalankan Server

```bash
uvicorn app:app --reload
```
Akses di http://localhost:8000  
Swagger documentasi otomatis: http://localhost:8000/docs

---

## 4. Endpoint & Contoh Request

| Endpoint           | Method | Deskripsi                                    |
|--------------------|--------|----------------------------------------------|
| /produk            | GET    | List semua produk PPOB                       |
| /beli              | POST   | Mulai transaksi pembelian PPOB               |
| /status/{trx_id}   | GET    | Lihat status & hasil satu transaksi          |
| /riwayat           | GET    | Lihat riwayat transaksi terakhir (100 data)  |
| /generate-key      | POST   | Generate ulang (rotasi) API Key dan Private Key, harus pakai API-Key lama |
| /register          | POST   | Registrasi user API, generate API/privkey |

### 1. Register User (`/register`)

**POST /register**

```json
{
  "nama": "Rama",
  "email": "rama@email.com",
  "hp": "081212xxxxxx"
}
```
**Response:**
```json
{
  "code": 201,
  "status": "success",
  "message": "Registrasi sukses",
  "data": {
    "api_key": "...",
    "private_key": "...",
    "info": "API Key dan Private Key hanya tampil saat register! Simpan baik-baik."
  }
}
```

### 2. Generate ulang API Key/private key (`/generate-key`)

**POST /generate-key**  
Header:  
`x-api-key: <api_key_lama>`

**Response:**
```json
{
  "code": 201,
  "status": "success",
  "message": "API Key dan Private Key telah diperbarui.",
  "data": {
    "api_key": "...",
    "private_key": "...",
    "info": "Yang lama sudah tidak berlaku. Simpan baik-baik!"
  }
}
```

### 3. List Produk (`/produk`)

**GET /produk**  
Header: `x-api-key: ...`
```json
{
  "code": 200,
  "status": "success",
  "message": "Daftar produk",
  "data": [
    {
      "id": 1,
      "nama": "Pulsa 10k",
      "harga": 12000,
      "deskripsi": "Pulsa Reguler 10.000"
    }
    // ...
  ]
}
```

### 4. Beli Produk (`/beli`)

**POST /beli**  
Header: `x-api-key: ...`
```json
{
  "produk_id": 2,
  "nomor_tujuan": "081212xxxx",
  "nominal": 20000
}
```
**Response:**
```json
{
  "code": 200,
  "status": "success",
  "message": "Transaksi berhasil dibuat",
  "data": {
    "id": "HIDEBOT123456",
    "status": "pending"
  }
}
```

### 5. Cek Status Transaksi (`/status/{trx_id}`)

**GET /status/HIDEBOT123456**  
Header: `x-api-key: ...`

**Response jika sukses:**
```json
{
  "code": 200,
  "status": "success",
  "message": "Status transaksi ditemukan",
  "data": {
    "trx_id": "HIDEBOT123456",
    "produk_id": 2,
    "nomor_tujuan": "081212xxxx",
    "nominal": 20000,
    "status": "sukses",
    "hasil_eksekusi": "...",
    "waktu": "2024-06-09T12:34:56"
  }
}
```

**Response jika tidak ada:**
```json
{
  "code": 404,
  "status": "error",
  "message": "Transaksi tidak ditemukan",
  "data": null
}
```

### 6. Riwayat Transaksi (`/riwayat`)

**GET /riwayat**  
Header: `x-api-key: ...`
```json
{
  "code": 200,
  "status": "success",
  "message": "Riwayat transaksi",
  "data": [
    // list transaksi user (max 100)
  ]
}
```

---

## 5. Catatan Keamanan & Pengembangan

- Hanya `api_key` yang dikirim di Header pada setiap request.
- Endpoint proteksi: Semuanya wajib autentikasi API-Key, hanya `/register` yang free.
- Kunci lama akan digantikan jika generate key baru.
- Untuk production, tambahkan rate limit, logging, dsb sesuai best-practices API.
- Proses logic PPOB pada `proses.py` bisa diganti dengan logic real PPOB sesuai integrasi provider-mu.

---

## 6. Kontribusi & Lisensi

Silakan gunakan, modifikasi, dan distribusikan sesuai kebutuhan.  
Open source, for community!

---

Happy coding 🚀
```
---