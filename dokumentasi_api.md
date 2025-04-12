Berikut adalah dokumentasi lengkap untuk API yang telah Anda buat, termasuk penjelasan tentang payload untuk setiap kode dan cara request untuk setiap endpoint.

### **1. Persiapan yang di butuhkan**
#### Header:
Contoh
`GET /api/v1/ping`

#### Deskripsi:
Endpoint ini hanya untuk memeriksa apakah server API Anda berjalan dengan baik.

#### Response:
```json
{
  "status": "success",
  "message": "pong"
}
```

#### Cara Request:
- URL: `/api/v1/ping`
- Method: `GET`
- Response: 200 OK

---

### **2. Dor Endpoint**
#### Endpoint:
`POST /api/v1/dor`

#### Deskripsi:
Endpoint ini digunakan untuk menjalankan berbagai jenis perintah berdasarkan kode yang diberikan. Kode yang valid akan menjalankan perintah Python dengan data yang diterima dari request.

#### Payload:
```json
{
  "kode": "uts",                  // Kode perintah yang diinginkan (lihat di bawah untuk daftar kode)
  "nama_paket": "paket1",         // Nama paket yang dibeli
  "nomor_hp": "1234567890",       // Nomor HP yang ingin diproses
  "payment": "dana",              // Metode pembayaran (optional, lihat daftar di bawah)
  "id_telegram": "123456789",     // ID Telegram pengguna
  "password": "password_user"     // Password minta ke admin
}
```

#### Daftar Kode Perintah:
- **uts**
- **utu2**
- **pancinganv2**
- **no_pancingan**
- **pancinganv1**

#### Metode Pembayaran:
- **dana**
- **shopeepay**
- **gopay**
- **pulsa**

#### Response:
Jika request berhasil:
```json
{
  "status": "success",
  "code": 0,
  "info_saldo_panel": {
    "id_telegram": "123456789",
    "role": "user",
    "harga_awal": 10000,
    "diskon": 3000,
    "saldo_dipotong": 7000,
    "saldo_sisa": 3000
  },
  "data": { 
    // Data output dari perintah Python
  },
  "stderr": ""   // Jika ada error di stderr
}
```

Jika terjadi kesalahan:
```json
{
  "status": "error",
  "message": "Pesan kesalahan"
}
```

#### Cara Request:
- URL: `/api/v1/dor`
- Method: `POST`
- Header: 
  - Content-Type:application/json
  - Authorization: `Bearer YOUR_API_KEY`
- Body: JSON seperti di atas

---

### **3. Produk Endpoint**
#### Endpoint:
`GET /api/v1/produk`

#### Deskripsi:
Endpoint ini mengembalikan daftar produk yang tersedia dalam sistem beserta harga dan deskripsi singkat.

#### Response:
```json
{
  "status": "success",
  "total": 3,          // Jumlah total produk
  "data": [
    {
      "nama_paket": "paket1",     // Nama paket produk
      "harga_panel": 10000,       // Harga panel
      "kode_buy": "abcd1234",     // Kode beli produk
      "deskripsi": "Deskripsi singkat produk"  // Deskripsi produk
    },
    {
      "nama_paket": "paket2",
      "harga_panel": 15000,
      "kode_buy": "abcd5678",
      "deskripsi": "Deskripsi singkat produk"
    }
  ]
}
```

#### Cara Request:
- URL: `/api/v1/produk`
- Method: `GET`
- Header: 
  - Content-Type:application/json
  - Authorization: `Bearer YOUR_API_KEY`

---

### **4. Error Handling**
Setiap endpoint di atas sudah memiliki mekanisme error handling yang akan mengembalikan status 400 atau 500 dengan pesan yang relevan. Berikut adalah beberapa kode status yang mungkin dikembalikan:

- **400 Bad Request**: Request tidak valid, parameter yang diperlukan tidak ada atau salah.
- **403 Forbidden**: Token API tidak valid.
- **404 Not Found**: Data yang dicari tidak ditemukan.
- **429 Too Many Requests**: IP diblokir karena melebihi batas rate-limiting.
- **500 Internal Server Error**: Terjadi kesalahan pada server saat memproses request.

---

### **Rate Limiting dan IP Blocking**
- Setiap IP hanya bisa melakukan maksimal **200 request per menit**.
- Jika melebihi batas, IP akan diblokir selama **5 menit**.

Jika IP diblokir, response yang diterima adalah:
```json
{
  "status": "error",
  "message": "Ahahahahah kasian ga bisa akses:v"
}
```

---

### **Autentikasi API**
Setiap request ke endpoint API harus menggunakan header **Authorization** dengan format `Bearer YOUR_API_KEY`. Jika header tidak ada atau tidak valid, server akan merespons dengan status `401 Unauthorized`.

Contoh header:
```
Content-Type:application/json
Authorization: Bearer YOUR_API_KEY
```

Jika token yang digunakan salah atau tidak sesuai, response-nya adalah:
```json
{
  "status": "error",
  "message": "Unauthorized"
}
```

---

### **Contoh Curl Requests**
1. **Ping Request:**
```bash
curl -X GET http://localhost:PORT/api/v1/ping
```

2. **Dor Request (POST):**
```bash
curl -X POST http://localhost:PORT/api/v1/dor \
    -H "Authorization: Bearer YOUR_API_KEY" \
    -H "Content-Type: application/json" \
    -d '{
          "kode": "uts",
          "nomor_hp": "1234567890",
          "payment": "dana",
          "id_telegram": "123456789",
          "nama_paket": "paket1",
          "password": "password_user"
        }'
```

3. **Produk Request (GET):**
```bash
curl -X GET http://localhost:PORT/api/v1/produk \
    -H "Authorization: Bearer YOUR_API_KEY"
```

---
