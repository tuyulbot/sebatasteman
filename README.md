# PPOB API (High Performance, Asynchronous, FastAPI + MySQL)

API sederhana untuk transaksi pembelian produk PPOB, inquiry status, dan riwayat transaksi, berbasis FastAPI (Python) dan MySQL.  
Dirancang untuk mendukung ratusan user secara concurrent (asynchronous & high performance).

---

## STRUKTUR FOLDER

```
ppob-api/
├── app.py        # API utama (FastAPI + MySQL + worker async)
├── proses.py     # Simulasi eksekusi proses PPOB eksternal
├── README.md     # Dokumentasi instalasi & penggunaan
```

---

## 1. PERSYARATAN

- **Python 3.8+**
- **MySQL / MariaDB** aktif sebagai database
- Privilege untuk create database dan tabel
- Internet untuk install dependensi Python

---

## 2. INSTALL DEPENDENSI PYTHON

Jalankan perintah berikut di terminal/cmd pada folder `ppob-api`:

```
pip3 install fastapi uvicorn aiomysql python-dotenv
```

---

## 3. INISIALISASI DATABASE MYSQL

Login ke MySQL, lalu jalankan SQL berikut untuk membuat dan seed database:

```sql
CREATE DATABASE IF NOT EXISTS ppob CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE ppob;

CREATE TABLE IF NOT EXISTS produk (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nama VARCHAR(50),
  harga INT,
  deskripsi TEXT
);

CREATE TABLE IF NOT EXISTS transaksi (
  id INT AUTO_INCREMENT PRIMARY KEY,
  produk_id INT,
  nomor_tujuan VARCHAR(30),
  nominal INT,
  status ENUM('pending','proses','sukses','gagal'),
  waktu TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  hasil_eksekusi TEXT,
  FOREIGN KEY (produk_id) REFERENCES produk(id)
);

CREATE TABLE IF NOT EXISTS user (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nama VARCHAR(50) NOT NULL,
  email VARCHAR(100) NOT NULL UNIQUE,
  hp VARCHAR(20),
  saldo INT DEFAULT 0,
  api_key VARCHAR(64) NOT NULL UNIQUE,
  private_key VARCHAR(64) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO produk (nama, harga, deskripsi) VALUES
('Pulsa 10k', 12000, 'Pulsa Reguler 10.000'),
('Pulsa 20k', 22000, 'Pulsa Reguler 20.000'),
('Token Listrik 50k', 51000, 'Token Listrik Prabayar 50.000')
ON DUPLICATE KEY UPDATE nama=VALUES(nama);
```

> **JANGAN LUPA:**  
> Ubah konfigurasi database MySQL jika perlu: `host`, `user`, `password`, di dalam file `app.py` bagian variable `DB_CONFIG`.

---

## 4. MENJALANKAN API

Pastikan berada di dalam folder `ppob-api`, lalu jalankan:

```
uvicorn app:app --reload
```

- Jika ingin listen di public (deploy server):  
  `uvicorn app:app --host 0.0.0.0 --port 8000 --workers 4`

---

## 5. ENDPOINTS API

| Endpoint           | Method | Deskripsi                                    |
|--------------------|--------|----------------------------------------------|
| /produk            | GET    | List semua produk PPOB                       |
| /beli              | POST   | Mulai transaksi pembelian PPOB               |
| /status/{trx_id}   | GET    | Lihat status & hasil satu transaksi          |
| /riwayat           | GET    | Lihat riwayat transaksi terakhir (100 data)  |
| /generate-key      | POST   | Generate ulang (rotasi) API Key dan Private Key, harus pakai API-Key lama |
| `/register`        | POST   | Registrasi user API, generate API/privkey |

### Contoh Request:

`POST /register`

**Request Body (JSON):**
```json
{
    "nama": "Budi Santoso",
    "email": "budi@mail.com",
    "hp": "08123456789",
    "saldo": 0
}
```
- `nama` *(string, required)*: Nama Lengkap user
- `email` *(string, required)*: Email user (harus unique)
- `hp` *(string, optional)*: Nomor HP (boleh kosong)
- `saldo` *(number, optional)*: Saldo awal (default 0, opsional)

#### **Response**

HTTP 200 Success:
```json
{
    "api_key": "cf68e...a109f",         // Token API untuk akses endpoint lain (header)
    "private_key": "fac21...4fa2f",     // Token private, tampil hanya sekali!
    "info": "API Key dan Private Key simpan baik-baik, hanya tampil saat register."
}
```
> **Note:**  
> - Simpan baik-baik `api_key` dan `private_key` (private_key hanya ditampilkan sekali saat register, tidak bisa diambil ulang).
> - `api_key` digunakan untuk akses seluruh endpoint, dikirim via header:  
>   ```
>   x-api-key: <api_key>
>   ```
> - Jika email sudah terpakai, akan return error 409 Conflict.

---

### **Contoh Curl Request**

```bash
curl -X POST http://localhost:8000/register \
-H "Content-Type: application/json" \
-d '{
    "nama": "Budi Santoso",
    "email": "budi@mail.com",
    "hp": "08123456789"
}'
```

**Response jika sukses:**
```json
{
    "api_key": "270ae3ec3bccc524ab84f2a5995687b1b48ea5b99204b7c5ad2fcbf4ad1de832",
    "private_key": "f7ad44e72ccb6828e10fd549396eb412ae2b8df3ef17d628dce86f62152b0b24",
    "info": "API Key dan Private Key simpan baik-baik, hanya tampil saat register."
}
```

---

### **Error Response Sample**
Jika email sudah pernah terdaftar:
```json
{
    "detail": "User/email sudah terdaftar"
}
```

---

### **Lanjutan:**

Setelah berhasil register, gunakan API-Key pada setiap request ke endpoint lain, misalnya:

```bash
curl -H "x-api-key: 270ae3ec..." http://localhost:8000/produk
```

---

### **Ringkasan**

- Register user (POST /register), dapet api_key & private_key.
- Simpan baik-baik output response.
- Gunakan api_key pada header `x-api-key` agar bisa akses endpoint selanjutnya (termasuk generate-key, /produk, /beli, /status, /riwayat, dll).


**POST /beli**

Headers:
x-api-key: <api_key>

Body JSON:
```json
{
  "produk_id": 1,
  "nomor_tujuan": "08123456789",
  "nominal": 10000
}
```

Response:
```json
{
  "id": 4,
  "status": "pending"
}
```

**GET /status/4**

Headers:
x-api-key: <api_key>

Response:
```json
{
  "id": 4,
  "produk_id": 1,
  "nomor_tujuan": "08123456789",
  "nominal": 10000,
  "status": "sukses",
  "waktu": "...",
  "hasil_eksekusi": "Transaksi PPOB berhasil! ..."
}
```

**POST /generate-key**
Headers:
x-api-key: <api_key_lama>

Response:
```json
{
    "api_key": "...",
    "private_key": "...",
    "info": "API Key dan Private Key telah diperbarui. Simpan baik-baik."
}
```

Semua endpoint bisa dicoba langsung lewat:
[http://localhost:8000/docs](http://localhost:8000/docs)

---

## 6. LOGIKA FILE proses.py

`app.py` menjalankan `proses.py` (secara async) setiap ada permintaan pembelian.  
File ini mensimulasikan transaksi PPOB dengan delay beberapa detik dan hasil random (sukses/gagal).

**Modifikasi saja proses.py sesuai kebutuhan integrasi PPOB asli-mu.**

---

## 7. TROUBLESHOOTING

- Koneksi database gagal?  
  - Pastikan MySQL sudah berjalan.
  - Sesuaikan DB_CONFIG di app.py.
- Tidak ada produk?  
  - Cek tabel produk di database sudah terisi.
- Port 8000 bentrok?  
  - Ganti argumen port uvicorn.

---

## 8. PENGEMBANGAN LANJUTAN

- Ingin fitur otentikasi JWT?  
- Ingin ada endpoint tambah produk?
- Ingin filter histori by user?

Tinggal tambahkan di dalam FastAPI, atau request pada author repo ini!

---

## 9. KONTRIBUSI

Bebas digunakan, sharing, dikembangkan open source.  
Kredensial dan password sebaiknya amankan bila deploy production.

---

Happy coding & semoga bermanfaat!

```
