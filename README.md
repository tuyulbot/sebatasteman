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
  otp VARCHAR(6) DEFAULT NULL,
  is_verified BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS produk (
    id INT AUTO_INCREMENT PRIMARY KEY,                  -- Kolom id untuk keperluan lain (jika diperlukan)
    kode_produk VARCHAR(20),  -- Kode produk sebagai primary key
    nama VARCHAR(50) NOT NULL,              -- Nama produk
    plp VARCHAR(20),                        -- Kolom untuk menyimpan huruf, angka, simbol
    plp_nomor VARCHAR(20),                  -- Nomor PLP
    harga INT NOT NULL,                      -- Harga produk
    deskripsi TEXT                          -- Deskripsi produk
);

CREATE TABLE transaksi (
    id INT AUTO_INCREMENT PRIMARY KEY,
    trx_id VARCHAR(20) NOT NULL UNIQUE,
    kode_produk VARCHAR(20),
    nomor_tujuan VARCHAR(20),
    status VARCHAR(20),
    hasil_eksekusi TEXT,
    nama_produk VARCHAR(50),
    nama_user VARCHAR(50),
    email_user VARCHAR(100),
    sisa_saldo INT DEFAULT NULL,
    waktu TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
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

| Endpoint           | Method | x-api-key | Deskripsi                                    |
|--------------------|--------|----------------------------------------------|
| /register          | POST   |  FALSE    | Registrasi user API, Lanjut proses /verif-otp    |
| /verif-otp         | POST   |  FALSE    | Verifikasi OTP untuk registrasi user API    |
| /generate-key      | POST   |  TRUE     | Generate ulang (rotasi) API Key dan Private Key, harus pakai API-Key lama |
| /produk            | GET    |  TRUE     | List semua produk PPOB                       |
| /cek-produk/{kode_produk}   | GET    |  TRUE    | Detail produk PPOB dengan id tertenu         |
| /beli              | POST   |  TRUE     | Mulai transaksi pembelian PPOB               |
| /status/{trx_id}   | GET    |  TRUE     | Lihat status & hasil satu transaksi          |
| /riwayat           | GET    |  TRUE     | Lihat riwayat transaksi terakhir (100 data)  |

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
  "message": "Registrasi sukses. OTP telah dikirim ke email Anda."
}
```

### 2. Verifikasi OTP (`/verif-otp`)

**POST /verif-otp**

```json
{
  "email": "rama@email.com",
  "otp": "087766"
}
```
**Response:**
```json
{
  "code": 200,
  "status": "success",
  "message": "Verifikasi berhasil. Simpan baik-baik API Key dan Private Key berikut.",
  "data": {
    "api_key": "1234567890",
    "privat-key": "1234567890"
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
  "kode_produk": "XCV12YT",
  "nomor_tujuan": "081212xxxx"
}
```
**Response:**
```json
{
  "code": 200,
  "status": "success",
  "message": "Transaksi berhasil dibuat",
  "data": {
    "id":"HIDEBOT993666",
    "kode-produk":"XCVYT12",
    "nomor-tujuan":"081212xxxx",
    "nama-produk":"Xtra Combo YouTube 1 Tahun",
    "status":"pending",
    "info":"Proses transaksi sedang berjalan, cek status untuk hasilnya."
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