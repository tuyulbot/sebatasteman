###        Dokumentasi api wong gabut

<details> <summary>1. Persiapan yang dibutuhkan (klik untuk lihat)</summary>

  - URL API: https://api.tuyull.my.id/

  - Header (dengan Content-Type):
```json
{
  "Content-Type": "application/json",
  "Authorization": "api-key" //Chat admin
}
```

  - Header (tanpa Content-Type):
```json
{
  "Authorization": "api-key" //Chat admin
}
```
</details>

<details> <summary>2. Cara Login Nomor (klik untuk lihat)</summary>

 - Minta Otp

    - Contoh Request simpel python
```python
import requests

# URL dan headers
url = 'https://api.tuyull.my.id/api/v1/minta-otp?nomor_hp=0877773346'
headers = {
    'Authorization': "api-key" #Chat admin
}

# Melakukan request GET
response = requests.get(url, headers=headers)
print(f"Response : {response})
```

 - Deskripsi:
Rubah nomor_hp yang mau minta kode otp

    - Contoh Respon Sukses
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "OTP request berhasil",
    "data": {
      "expires_in": 300,
      "max_validation_attempt_suspend_duration": 900,
      "max_validation_attempt": 5,
      "next_resend_allowed_at": 60,
      "max_validation_suspend_duration": 900,
      "max_request_attempt": 4,
      "max_request_suspend_duration": 900,
      "subscriber_id": "0PkeR6m9eQk2OhyEDY0WcA==",
      "type": "PREPAID"
    },
    "delete_status": "Data dengan nomor 6287777334689 berhasil dihapus (jika ada).",
    "insert_status": "Data berhasil disimpan ke database dengan ID 33261"
  },
  "stderr": ""
}
```

 - Verifikasi otp

    - Contoh Request
```python
import requests

# URL dan headers
url = 'https://api.tuyull.my.id/api/v1/verif-otp?nomor_hp=0877773346&kode_otp=876677
headers = {
    'Authorization': "api-key" #Chat admin
}

# Melakukan request GET
response = requests.get(url, headers=headers)
print(f"Response : {response})
```

 - Deskripsi:
Rubah nomor_hp yang sebelomnya pas request minta-otp dan rubah kode_otp yang di dapatkan di sms

    - Contoh Respon sukses
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "Login nomor success",
    "data": {
      "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI4cHNfTzV4VEhnd3MzcW4tYlRiUk1yay1yeHFsemtrdEl6M2UtLXlkRzdjIn0.eyJleHAiOjE3NDQ2NjI2NTQsImlhdCI6MTc0NDY2MjM1NCwianRpIjoiMzg5NzZhMjktNzVmNy00NDEyLThkZjctNDhiYzgzYjEwZTNlIiwiaXNzIjoiaHR0cHM6Ly9nZWRlLmNpYW0ueGxheGlhdGEuY28uaWQvcmVhbG1zL3hsLWNpYW0iLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMWM0YTgzYjUtNzI3YS00MzBiLWJkMTMtOTY2ZGM4ZWQxZGE0IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiZjE2MGZhMTItZGIzZC00MDcyLTg3MDktYzc3OGMxNzNmZWU3Iiwic2Vzc2lvbl9zdGF0ZSI6IjlhYzM0MTE4LTViYTEtNGY2YS1hOTBlLTcyOTBmMjg5NTJmZiIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiLyoiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbImRlZmF1bHQtcm9sZXMteGwtY2lhbSIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIGVtYWlsIiwic2lkIjoiOWFjMzQxMTgtNWJhMS00ZjZhLWE5MGUtNzI5MGYyODk1MmZmIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJuYW1lIjoiLSIsInByZWZlcnJlZF91c2VybmFtZSI6IjYyODc3NzczMzQ2ODkiLCJtc2lzZG4iOiI2Mjg3Nzc3MzM0Njg5IiwiZmFtaWx5X25hbWUiOiItIn0.SSUClQknyngDGnm7eYhz6Xnu1y9LDr9teEMSpb-ZVObJqkHqac_cxGWkcZINgmFvWpbLI1ACF9f_2fwMi7Q36VJKuCwQcHfoUvX9PfMqNyNBV1DTcsqbkqDoSvqffoFLfV06KFOBdAk-KWGErmkH0f_qywbGKzD3jFPy6U2irZvex-IPVmYKQwBV3BpiJYB-_d7Zz8_DD5PykCmpQfpxJZsTO5SRBEyAjIzqa0t2v8LQLJGSiqh3Mupe0DShbWe84FpiL433W_glntwYgiiiUTPLakSNEwodD-OKZMa-0PqP2E6vRTnKF7lJt9JA0Gebd3tlncZUsEYFnhqjOSXWuw",
      "expires_in": 299,
      "refresh_expires_in": 7775999,
      "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJhZTQzZmFiZi1iOGFlLTQxYmUtOGUwOS1iODJkYWM3NGU4MjAifQ.eyJleHAiOjE3NTI0MzgzNTQsImlhdCI6MTc0NDY2MjM1NCwianRpIjoiODI0MmNhNTktYzhmMS00YzhmLWJiNDQtNDIyN2NlNGI0MTgyIiwiaXNzIjoiaHR0cHM6Ly9nZWRlLmNpYW0ueGxheGlhdGEuY28uaWQvcmVhbG1zL3hsLWNpYW0iLCJhdWQiOiJodHRwczovL2dlZGUuY2lhbS54bGF4aWF0YS5jby5pZC9yZWFsbXMveGwtY2lhbSIsInN1YiI6IjFjNGE4M2I1LTcyN2EtNDMwYi1iZDEzLTk2NmRjOGVkMWRhNCIsInR5cCI6IlJlZnJlc2giLCJhenAiOiJmMTYwZmExMi1kYjNkLTQwNzItODcwOS1jNzc4YzE3M2ZlZTciLCJzZXNzaW9uX3N0YXRlIjoiOWFjMzQxMTgtNWJhMS00ZjZhLWE5MGUtNzI5MGYyODk1MmZmIiwic2NvcGUiOiJvcGVuaWQgcHJvZmlsZSBlbWFpbCIsInNpZCI6IjlhYzM0MTE4LTViYTEtNGY2YS1hOTBlLTcyOTBmMjg5NTJmZiJ9.yiXWsbQmhO2FH_JfLKL3kvc5-TDq-hMudxlxIHWv9ww",
      "token_type": "Bearer",
      "id_token": "eyJhbGciOiJSU0EtT0FFUCIsImVuYyI6IkEyNTZDQkMtSFM1MTIiLCJjdHkiOiJKV1QiLCJraWQiOiJzNG8zMm1hbl9jbXRyU1dILUEtZWNhTF94N2dvbzVyZE1iVFpMd3ZiaUljIn0.PG4pLOB4mBX-_CDYHNjQZf53M9EcvWBH3OCbw8d7fjqQ6dwK20xvTVAxdIb9SHVi9cXtQGk1PwriIFDsvltMrSiPePoAF-AYDGqn1ZYHmqjRaj08l7bexmJjRoZHqjKzusrDk31a5rOxCi3NcBICf68GO9G2WEVmBk2ShYD99NQmpo0CjQFc_OnhbQJ_egrBnnGwaiZxUbyu7XC6sfFRPwNouZEQEAZGaMxwENTTT7mJOzfLzuX2h4VC7rWbGSVFtxr3_n2-p6Dz6hu3v0s3KvUznpz86zr5yFOYHP7imf87E_9THzKvsQPQjZKF4jncPY6LqnKasnW1_YQbKh8VjA.8z_f7bfoH3ABb0yYs5BNJg.c77AjaKUwO72lS5ZcLN5HUo8PC-2AQjGbjs4tSCDn9PsrKZkfQf69i2iJWbNtaqYkmQX1tR9XLMZ5Klh204HhKkIz0B8ARoz74r3VYw804xTa6QW-wC81d3jFBb295i6Gb5E3FT3ruoGS9AJfdmTBikgtptan_pAtDTRXKXy3WQ2cYGFrdTR2Maw9g2tqfyu_LWP0_KPwOL7dNaKXmmNVtNsunYcsA8nd5Hog5bKu5tU0h8r6pqFMPAAxeAUYk8kHbYDMEFfN7Y43lQtlLTmiMawbTRQh7WSS0qdipDBiLlewsI0TKh0bdz5u5beiXLWkK5csj1ewfQamCC1mvfRAPADORlcE-lzKmI5weDrp9pGykiSvE3BiH1MD8z5GOQpckVmChn1IzndyE219baUH3DjInGyurKsUb5PAJy-deXPngz11H5HzC_jhrCcvQMEcQJGCABPa8BGYD-sY_HiKWcI9T48SN6VBKCZv-uAkrbcICok3vw0ZCEyu_mo1E9M8gqvfikom0XwsE0dyDx04ugUUExyBHY7hdRhQ2DNSE1K6FOqQcTKCy0DiDNWtb78LUiB-qLD7VhxTFn0jfe6SxaApqxrLQBS5cbJTZlQpDO8H7rQtzIID17DIQP97SkeU9c2HsDgCRzGDicYatJfu4n-cmwsHogjefy-6Dm2mKU9YIaMpod1NedOpa5h89qAbuBc4agvu7ZK6UXnAgCVSUOBA-BrDtw3DSO4htU4qr85R30ogH5QXXbFau5ZZDsqSkxJVpu477zWGNBxUj5VGfgVlRd1zqeZz-5PI8sPWcCPBEBIND_SCwsAOcCL8nQ5ldYHLiC9cWqH5tH1jP8mNLypJ6fxK33CJaqqqkU1RAFtl9M47fSeOE3gGR9TxmhIYj6CsPlsKpuY9-Nuaf79BCjJ4h7nq-XiskGwC2sUtvFdT4X-EHAnW7mOHkTtIEBF76tw8N8YCQ6jP19aYL7k-VVzLq0mulPmRDf2UqRv2pxOgJNGRu4qciEP38kRbXr1FqlBNSQ6HNkNkJ89GV51VERodLFRE_ozaB9pJZSP8sT5TmvHdVrQ-J_5QGfA6rEiXBgogpIP0mfGEB5qozhrm46V0DLi5BmytX_88QDPEG6PiEEkCnDclWNy8rezSRCNsc8N7cdckI0JqKfg3GGixpSNUswvsp-O-DJyL3V15uRPs0vZlDXjymdtLe_ql-DAJ_I5aG93RczTH7hxo8zWFkRp4dR47HTJdT16P9CpM6XEL3JNR8EHT-V2EIiGmmOEgUeZlRBrkxuH8t0dUq2IHnR3z2YNzRao0197NMJ-wujhh0a3aRtihFUjsblYSZ04By-yr58Wy70xcojO_X0NZHeXecZe0kkM2h6v1KxMl9m8ljQNkxDNKxbVAJRZw_VtJVQM8VlBRpWJPA4lHFEetmFdgr9-GcCBY4ImBoKtHJbxxbDSCZxB2JWGTuAZvqvLHTuQWAQrpEfVpWMn1ZMMhcRUogrxyLDXByglyKBl5F0i9xnjyvwLWIROf4SoFFS9FRxj6nkq5caDhNpJKn2wrlJ3tkIjdL_k4Y-JibRIH4KlXtVQx325ExsxGthv81yhno5XeYknjLbzTF0skK5hpT641ZSRovkWNQJ3_rPO5UGOCOJ73wBfCEVPwaIiVEJYiginYh0DLPsZEJ8ibrKSPNqfQxx4ggEYMfr_C89r-fhIwN1KXbNFqypnLjELcJJpbwZ6-ZyZ73cB6dq4EJHiEeF06o0BCORNLjnWOLqYXv43OEUKPm-tUFpdAPyqG4cskhDZ7_VnW8_MWjEVGUIv4Yau4Vu36Dk5U9uCBT2-d8IkvY_dlG2bc3FC4xFYRcLgHDsZYUb9JuV-K9PKcM7EOYkhaeTdmywHb8EZx99rx8-1gH5zulDzYESBgYUuge_YfJwjh_5S0rGMxX6Uu99L1ANZGIcdrUWVaHmMxVgzrcJnhtPC0R7sdRy1tgafSr-0sjgTXbtqfT3d7zEZh0_13_xk94CbJa5F-SzzRhzHzuqdMH0Tbwbut0NuAn_HCiqgP4NzeAbUfUsoi-5wpy5e0IlcxIXEvpICOiv6AC9CtpIRizuiAEh3gA0n_BC6TVqpffmrBtnyNTxAMM9_LQ3Xn_sw-RV1XEJy32vFvCqjq2lT-fZB3TtAacqvjtZ_xBvMgAstTChxcm97KSif6RpoEaSxH94uIAv2YaLrLcqZuH7jWtUy0tXbWUzSJlrZ52yAYar0LPizeR1gG68BXsO09l6KiOcRDSeHLwoPI3J1iBYwT8f3ElttHV5wcOc.BR-1pEphlBgcHpZ8DGypatvPpnTXWAwYYEzAlhHeWm8",
      "not-before-policy": 0,
      "session_state": "9ac34118-5ba1-4f6a-a90e-7290f28952ff",
      "scope": "openid profile email"
    }
  },
  "stderr": ""
}
```

- Cek Login:

    - Contoh Request Simpel Python
```python
import requests

url = "https://api.tuyull.my.id/api/v1/cek-login?nomor=(ganti dengan nomor yang mau di cek)"
headers = {
    "Authorization": "api-key" //Chat admin
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
  "message": "Nomor sudah login"
}
```
</details>

<details> <summary>3. Cara Pembelian dan rubah password pembelian (klik untuk lihat)</summary>

 - Rubah Password Pembelian

    - Contoh Request Simpel Python
```python
import requests

# URL dan headers
url = 'https://api.tuyull.my.id/api/v1/ubah-password?id_telegram=2233444&password_lama=nganuu&password_baru=tested
headers = {
    'Authorization': "api-key" #Chat admin
}

# Melakukan request GET
response = requests.get(url, headers=headers)
print(f"Response : {response})
```

 - Deskripsi:
Masukan id_telegram, masukan password_lama ( chat admin untuk mendapatkan pw lama ), masukan password_baru anda dan simpan password anda secara aman jangan di kasih tau sama orang lain

    - Contoh Respon Succes
```json
{
  "status": "success",
  "message": "Password berhasil diperbarui"
}
```

 - Get Produk:

    - Contoh Request Simpel Python
```python
import requests

url = "https://api.tuyull.my.id/api/v1/produk"
headers = {
    "Authorization": "api-key" //Chat admin
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
 - Deskripsi Get Produk:
parsing json dari respon endpoint get produk dan ambil data nama_paket, harga_panel, kode_buy, payment_suport untuk digunakan proses endpoint post pembelian


 - Post Pembelian:

    - Contoh Request Simpel Python
```python
import requests
import json

url = "https://api.tuyull.my.id/api/v1/dor"

headers = {
    "Content-Type": "application/json",
    "Authorization": "api-key" //Chat admin
}

payload = {
    "kode": "pancinganv1",  # kode di ambil dri responen get produk (kode_buy)
    "nama_paket": "Xtra Combo Spesial 8GB", # nama paket di ambil dri respon get produk (nama_paket)
    "nomor_hp": "087777334618", # nomor hp bisa input dengan format (08 / 628)
    "payment": "nganu", # payment di ambil dri respon get produk ambil salah satu contoh: dana/pulsa atau yang lain 
    "id_telegram": "7902668", # id_telegram di wajibkan terdaftar di bot dan ada saldonya:v
    "password": "tuyul" # Password minta ke admin 
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

 - Deskripsi
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

 - Deskripsi
Jika respon json pembelian payment pulsa seperti di atas maka proses pembelian sukses
</details>

<details> <summary>4. Pembelian paket add on xcs satuan dan all addon</summary>

  - Post Pembelian Add on Satuan:

    - List Kode paket:
        1 . Premium 30 hari
        2 . Super 30 hari
        3 . Standard  30 hari
        4 . Basic  30 hari
        5 . Netflix 
        6 .  TikTok 
        7 . YouTube 
        8 . Viu 
        9 . Joox 
        10 . iFlix 
        11 . Vidio (skip gaush di tmbhin gpp)
        12 . Premium 7 hari
        13 . Super 7 hari
        14 . Standard 7 hari
        15 . Basic 7 hari

    - Contoh Request Simpel Python
```python
import requests
import json

url = "https://api.tuyull.my.id/api/v1/dor"

headers = {
    "Content-Type": "application/json",
    "Authorization": "api-key" //Chat admin
}

payload = {
    "kode": "addon_satuan",  # kode di ambil dri responen get produk (kode_buy)
    "nama_paket": "Add On XCS Satuan", # nama paket di ambil dri respon get produk (nama_paket)
    "nomor_hp": "087777334618", # nomor hp bisa input dengan format (08 / 628)
    "payment": "pulsa", # payment menggunakan pulsa only
    "id_telegram": "7902668", # id_telegram di wajibkan terdaftar di bot dan ada saldonya:v
    "password": "tuyul", # Password minta ke admin,
    "kode_paket": "1" #Kode paket bisa di lihat di List Kode Paket di atas
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

print(response.status_code)
print(response.json())
```

    - Contoh respon sukses pembelian
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

  - Post Pembelian All Add On:

    - List Paket yang di dapat ( jika hoki masuk smua ):
        1 . Premium 30 hari
        2 . Super 30 hari
        3 . Standard  30 hari
        4 . Basic  30 hari
        5 . Netflix 
        6 .  TikTok 
        7 . YouTube 
        8 . Viu 
        9 . Joox 
        10 . iFlix 

    - Contoh Request Simpel Python
```python
import requests
import json

url = "https://api.tuyull.my.id/api/v1/dor"

headers = {
    "Content-Type": "application/json",
    "Authorization": "api-key" //Chat admin
}

payload = {
    "kode": "all_addon",  # kode di ambil dri responen get produk (kode_buy)
    "nama_paket": "All Add On XCS", # nama paket di ambil dri respon get produk (nama_paket)
    "nomor_hp": "087777334618", # nomor hp bisa input dengan format (08 / 628)
    "payment": "pulsa", # payment menggunakan pulsa only
    "id_telegram": "7902668", # id_telegram di wajibkan terdaftar di bot dan ada saldonya:v
    "password": "tuyul" # Password minta ke admin,
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

print(response.status_code)
print(response.json())
```

    - Contoh respon
```json
{
  "status": "processing",
  "message": "Proses pembelian sedang berjalan, gunakan processId untuk cek status.",
  "processId": "c180e97b-8af0-4628-aa00-db231a191b40"
}
```
    - Deskripsi
Simpan "processId": "c180e97b-8af0-4628-aa00-db231a191b40" untuk pengecekan status proses pembelian sudah sukses apa blom

    - Pengeceakn proses pembelian khusus all add on 

- Contoh Request Simpel Python
```python
import requests

url = "https://api.tuyull.my.id/api/v1/dor/status/(ganti dengan prosessId yang di dapat)"
headers = {
    "Authorization": "api-key" //Chat admin
}

response = requests.get(url, headers=headers)

# Tampilkan hasil respons
print(response.status_code)
print(response.json())
```

    - Contoh respon jika proses pembelian sukses
```json
{
  "status": "success",
  "processId": "c180e97b-8af0-4628-aa00-db231a191b40",
  "result": {
    "status": "success",
    "data": {
      "summary": [
        "1. Premium (done)",
        "2. Super (done)",
        "3. Standard (done)",
        "4. Basic (done)",
        "5. Netflix (done)",
        "6. TikTok (done)",
        "7. YouTube (done)",
        "8. Viu (done)",
        "9. Joox (done)",
        "10. iFlix (done)",
        "11. (SKIPPED)"
      ],
      "note": "Langsung membeli paket xcs 8gb"
    }
  },
  "created_at": "2025-05-27T21:07:09.000Z",
  "updated_at": "2025-05-27T21:11:09.000Z"
}
```

- History pembelian khusus all add on

- Contoh Request Simpel Python
```python
import requests

url = "https://api.tuyull.my.id/api/v1/dor/history/(ganti dengan id telegram yang di pake saat pembelian)"
headers = {
    "Authorization": "api-key" //Chat admin
}

response = requests.get(url, headers=headers)

# Tampilkan hasil respons
print(response.status_code)
print(response.json())
```

    - Contoh respon jika proses pembelian sukses
```json
{
  "status": "success",
  "history": [
    {
      "processId": "6742a2d7-7945-4b79-96e8-a4301d913efd",
      "nomorHp": "087777334614",
      "namaPaket": "All Add On XCS",
      "status": "success",
      "saldoDipotong": 14000,
      "result": {
        "status": "success",
        "data": {
          "summary": [
            "1. Premium (done)",
            "2. Super (done)",
            "3. Standard (done)",
            "4. Basic (done)",
            "5. Netflix (done)",
            "6. TikTok (done)",
            "7. YouTube (done)",
            "8. Viu (done)",
            "9. Joox (done)",
            "10. iFlix (done)",
            "11. (SKIPPED)"
          ],
          "note": "Langsung membeli paket xcs 8gb"
        }
      },
      "createdAt": "2025-05-28T03:23:18.000Z",
      "updatedAt": "2025-05-28T03:27:21.000Z"
    }
    ]
}
```

</details>


<details> <summary>5. Managemen Akrab (klik untuk lihat)</summary>

  - Sebelom menggunakan API nomor di wajibkan login otp terlebih dahulu, api login sudah di sediakan
  - Berikut action yang bisa di gunakan di API
```bash
1. add (digunakan untuk menambhkn anggota) fee 1k
2. edit (digunakan untuk mengedit kuota bersama) free
3. info (digunakan untuk mendapatkan informasi anggota dan detail kuota) free
4. kick (digunakan untuk mengeluarkan anggota dari paket akrab) free
```

    Berikut contoh curl stiap action :

  - action (add)
```bash
curl -X POST 'https://api.tuyull.my.id/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "add",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(ganti nomor pengelola, wajib login terlebih dahulu)",
 "nomor_slot": (ganti dengan nomor slot yang mau di add, contoh: 1),
 "nomor_anggota": "(ganti dengan nomor anggota yang mau di add)",
 "nama_anggota": "(ganti dengan nama anggota yang mau di add)",
 "nama_admin": "(ganti dengan nama admin bebas)"
}'
```

  - respon sukses action (add)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "succes",
    "message": "Anggota berhasil ditambahkan",
    "details": {
      "nomor-pengelola": "6287856",
      "nomor-slot": 3,
      "nomor-anggota": "6287781",
      "nama-admin": "Rendii",
      "nama-anggota": "Fauzan"
    },
    "waktu-eksekusi": {
      "time": "20.48 detik",
      "note": "Jika time kurang dri 20 detik nomor anggota nyangkut di akrab lama"
    },
    "data": {
      "code": "214",
      "code_detail": "214",
      "status": "FAILED",
      "message": "HTTP_CLIENT_RESPONSE_BODY_ERR",
      "title": "",
      "description": ""
    },
    "info_saldo_panel": {
      "id_telegram": "79026",
      "role": "super_reseller",
      "harga": 1000,
      "saldo_dipotong": 1000,
      "saldo_sisa": 3323700
    }
  },
  "stderr": ""
}
```

  - action (edit)
```bash
curl -X POST 'https://api.tuyull.my.id/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "edit",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(ganti nomor pengelola, wajib login terlebih dahulu)",
 "nomor_slot": (ganti dengan nomor slot yang mau di add, contoh: 1),
 "input_gb": (ganti dengan input gb, contoh: 10 = 10gb)
}'
```

  - respon sukses action (edit)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "Kuota Bersama berhasil diperbarui.",
    "data": {
      "nomor-pengelola": "6287856",
      "slot": 3,
      "original-allocation": "75.00 GB",
      "new-allocation": "5.00 GB"
    },
    "info_saldo_panel": {
      "id_telegram": "79026",
      "role": "super_reseller",
      "saldo_tersedia": 3323700,
      "catatan": "Saldo tidak dipotong bukan action add"
    }
  },
  "stderr": ""
}
```



</details>