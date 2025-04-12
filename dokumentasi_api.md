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

#### List Payment 
- **dana**
- **shopeepay**
- **gopay**
- **pulsa**

### **2. Cara Request Endpoint**

#### Get Produk:

 - Contoh Request Simpel Python
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
```json
{
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
#### Deskripsi Get Produk:
parsing json dari respon endpoint get produk dan ambil data nama_paket, harga_panel, kode_buy untuk proses pembelian


#### Post Pembelian:

 - Contoh Request Simpel Python
```python
import requests
import json

url = "https://api.tuyull.my.id/api/v1/dor"

headers = {
    "Content-Type": "application/json",
    "Authorization": "0a1ccba4-e6fc-498c-af2f-5f889c765aaa"
}

payload = {
    "kode": "pancinganv1",  # kode di ambil dri endpoint get produk (kode_buy)
    "nama_paket": "Xtra Combo Spesial 8GB", # nama paket di ambil dri api endpoint get produk (nama_paket)
    "nomor_hp": "087777334618", # nomor hp bisa input dengan format (08 / 628)
    "payment": "nganu",
    "id_telegram": "7902668644",
    "password": "tuyulbot"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

print(response.status_code)
print(response.json())
```
