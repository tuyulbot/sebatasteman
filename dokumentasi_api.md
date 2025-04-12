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

 - Contoh Respon Succes
```json
{
  "status": "success",
  "total": 2,
  "data": [
    {
      "nama_paket": "Xtra Combo Spesial 8GB",
      "harga_panel": 3000,
      "kode_buy": "pancinganv1",
      "payment_suport": "dana, shopee, gopay",
      "deskripsi": "Rincian Benefit\\n\\n- Kuota Utama: 1.00 GB\\n   * 00:00 - 24:00\\n- YouTube: 1.00 GB\\n- Kuota Area Semua Jaringan: 5.00 GB\\n   * 00:00 - 24:00\\n- Kuota Youtube: 1.00 GB\\n- Telp ke Semua Operator: 300 detik\\n   * 00:00-24:00\\n   * Expired: 30 Hari"
    },
    {
      "nama_paket": "Edukasi 2GB",
      "harga_panel": 100,
      "kode_buy": "no_pancingan",
      "payment_suport": "dana, shopee, gopay, pulsa",
      "deskripsi": "Rincian Benefit \\n\\n- Paket internet edukasi: 2.00 GB\\n   * 00:00-24:00\\n   * Expired: 1 Hari"
    }
  ]
}
```
#### Deskripsi Get Produk:
parsing json dari respon endpoint get produk dan ambil data nama_paket, harga_panel, kode_buy, payment_suport untuk digunakan proses endpoint post pembelian


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
    "kode": "pancinganv1",  # kode di ambil dri responen get produk (kode_buy)
    "nama_paket": "Xtra Combo Spesial 8GB", # nama paket di ambil dri respon get produk (nama_paket)
    "nomor_hp": "087777334618", # nomor hp bisa input dengan format (08 / 628)
    "payment": "nganu", # payment di ambil dri respon get produk ambil salah satu contoh: dana/pulsa atau yang lain 
    "id_telegram": "7902668644", # id_telegram di wajibkan terdaftar di bot dan ada saldonya:v
    "password": "tuyulbot" # Password minta ke admin 
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

print(response.status_code)
print(response.json())
```

 - Contoh respon sukses pembelian payment dana misal
```json
{
  "status": "success",
  "code": 0,
  "info_saldo_panel": {
    "id_telegram": "7902668644",
    "role": "super_reseller",
    "harga_awal": 3000,
    "diskon": 0,
    "saldo_dipotong": 3000,
    "saldo_sisa": 9800
  },
  "data": {
    "status": "success",
    "data": {
      "code": "000",
      "status": "SUCCESS",
      "data": {
        "can_trigger_rating": false,
        "deeplink": "https://m.dana.id/n/cashier/new/checkout?bizNo=20250413111212800110166281146645811&timestamp=1744481226887&originSourcePlatform=IPG&mid=216620000035833566172&did=216650000204712460170&sid=216660000204589345173&sign=bqKvAWshQtsojimCWpR1PznxE1%2BtSOT9Sqw9NvjDCC55vrP%2FzL9HOWtYqL1sbBsahItFcGKEw2%2Fy2mFmNO8oQxqwf4icTSJZSGft4dA73bHK%2FuG1HDIpYiiiBgK4sd6z9MoO6IGyMEkuF0TnoswyJHiqPpUkpUoQldnOvQd%2BtyGmuzdyRyC6Z9RbTMz3OxEOjFsp1qX879yapmyNL6iiP1Bpy5mhcr3SwuVGtXUChTwEpqJaOB6Bd3AOHplrT5jW755P2ru1hKMyheUJfAKGVWP1s015h4CJNyBpCxh5Zlk6FNT9OVk9b4tE1BeU2QD2FEoV%2FD%2FnMwTuLXOps4nWQQ%3D%3D&forceToH5=false",
        "details": [
          {
            "amount": 0,
            "campaign_id": "",
            "code": "U0NfX0FQEHWxiz_B2JfrZV2p766lcIf5dYsrsLBdHay6Hor6tnEMIPcQCIbpjG3RC1Mh4ySwj9RbGBBxWT-MEtg",
            "name": "Xtra Combo Special 8GB",
            "status": "SUCCESS",
            "tax": 0
          },
          {
            "amount": 1000,
            "campaign_id": "",
            "code": "U0NfXzwSKEDyOvmopAfNKwGF02L-XTbcqWvIkk5EJQnQymMC1DOfpFp5P0KMWst1U7A52gGVHV2XcLwZzFl3QIE",
            "name": "Edukasi 2GB",
            "status": "SUCCESS",
            "tax": 0
          }
        ],
        "have_offer": false,
        "is_myxl_wallet": false,
        "migration_type": "",
        "payment_method": "DANA",
        "points_gained": 0,
        "ticket_number": "",
        "total_amount": 16850,
        "total_discount": 0,
        "total_fee": 0,
        "total_tax": 0,
        "transaction_code": "3768542"
      }
    },
    "info_saldo_panel": {
      "id_telegram": "7902668644",
      "role": "super_reseller",
      "harga_awal": 3000,
      "diskon": 0,
      "saldo_dipotong": 3000,
      "saldo_sisa": 9800
    }
  },
  "stderr": ""
}
```

#### Deskripsi
parsing json respon pada api pembelian ambil bagian deplink untuk proses pembayaran contoh dana, payment lain sama ambil deplinknya 

 - Contoh request sukses pembelian payment pulsa
```json
{
  "status": "success",
  "code": 0,
  "info_saldo_panel": {
    "id_telegram": "7902668644",
    "role": "super_reseller",
    "harga_awal": 100,
    "diskon": 0,
    "saldo_dipotong": 100,
    "saldo_sisa": 6600
  },
  "data": {
    "status": "success",
    "data": {
      "code": "000",
      "status": "SUCCESS",
      "data": {
        "benefit_type": "",
        "can_trigger_rating": false,
        "deeplink": "",
        "details": [
          {
            "amount": 1000,
            "campaign_id": "",
            "code": "U0NfXzQeW2zAAXtWowpia0axznYrgxmwaPtZxPEnes-uzpKJ1LkoqvpFRH_5mwUOvsrM2b1Qgq4iq1rUBbm1q44",
            "name": "Edukasi 2GB",
            "status": "SUCCESS",
            "tax": 0
          }
        ],
        "have_offer": false,
        "payment_method": "BALANCE",
        "points_gained": 0,
        "total_amount": 1000,
        "total_discount": 0,
        "total_tax": 0,
        "transaction_code": "1509410"
      }
    },
    "info_saldo_panel": {
      "id_telegram": "7902668644",
      "role": "super_reseller",
      "harga_awal": 100,
      "diskon": 0,
      "saldo_dipotong": 100,
      "saldo_sisa": 6600
    }
  },
  "stderr": ""
}
```

#### Deskripsi
Jika respon json pembelian payment pulsa seperti di atas maka proses pembelian sukses