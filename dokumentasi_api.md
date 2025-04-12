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