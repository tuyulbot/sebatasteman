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

### **2. Cara Request Endpoint**

#### Get Produk:

 - Contoh Request
```python
import requests

url = "https://api.tuyull.my.id/api/v1/produk"
headers = {
    "Authorization": "0a1ccba4-e6fc-498c-af2f-5f889c765aaa"
}

response = requests.get(url, headers=headers)

# Tampilkan hasil respons
print(response.status_code)
print(response.json())
```

 - Contoh Respon
```{
  "status": "success",
  "total": 2,
  "data": [
    {
      "nama_paket": "Xtra Combo Spesial 8GB",
      "harga_panel": 3000,
      "kode_buy": "pancinganv1",
      "deskripsi": "Rincian Benefit\\n\\n- Kuota Utama: 1.00 GB\\n   * 00:00 - 24:00\\n- YouTube: 1.00 GB\\n- Kuota Area Semua Jaringan: 5.00 GB\\n   * 00:00 - 24:00\\n- Kuota Youtube: 1.00 GB\\n- Telp ke Semua Operator: 300 detik\\n   * 00:00-24:00\\n\\nNoted:\\nGunakan kode buy yang ada di kode_buy sesuai nama paket yang ingin di buy di gunakan nanti pada payload pembelian\\nSuport pembayaran (dana,shopee,gopay)"
    },
    {
      "nama_paket": "Edukasi 2GB",
      "harga_panel": 100,
      "kode_buy": "no_pancingan",
      "deskripsi": "Rincian Benefit \\n\\n- Paket internet edukasi: 2.00 GB\\n   * 00:00-24:00\\n\\nNoted:\\nGunakan kode buy yang ada di kode_buy sesuai nama paket yang ingin di buy di gunakan nanti pada payload pembelian\\nSuport pembayaran (dana,shopee,gopay,pulsa)"
    }
  ]
}
```