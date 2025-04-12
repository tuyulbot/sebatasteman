Berikut adalah dokumentasi lengkap untuk API yang telah Anda buat, termasuk penjelasan tentang payload untuk setiap kode dan cara request untuk setiap endpoint.

### **1. Persiapan yang di butuhkan**

#### Endpoint GET Produk:
  - **https://api.tuyull.my.id/api/v1/produk**

#### Endpoint Post Pembelian:
  - **https://api.tuyull.my.id/api/v1/dor**

#### Header Endpoint Post Pembelian:
```json
{
  "Content-Type": "application/json",
  "Authorization": "0a1ccba4-e6fc-498c-af2f-5f889c765aaa"
}
```

#### Header Endpoint Get Produk:
```json
{
  "Authorization": "0a1ccba4-e6fc-498c-af2f-5f889c765aaa"
}
```

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