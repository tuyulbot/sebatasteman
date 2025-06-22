###        Dokumentasi api wong gabut

<details> <summary>1. Login Nomor (klik untuk lihat)</summary>

  - List API:
```bash
- minta-otp (digunakan untuk meminta kode otp)
- verif-otp (digunakan untuk memverifikasi kode otp)
- cek-login (digunakan untuk memeriksa nomor sudah login apa blom)
- ubah-password (digunakan untuk mengubah password)
```

  - Minta Otp
 ```bash
curl -X GET 'https://api.tuyull.my.id/api/v1/minta-otp?nomor_hp=(ganti dengan nomor hp)' -H 'Authorization:(ganti dengan api-key , minta ke admin)'
 ```

  - Respon Sukses
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
```bash
curl -X GET 'https://api.tuyull.my.id/api/v1/verif-otp?nomor_hp=(ganti dengan nomor hp)&kode_otp=(masukan otp)' -H 'Authorization:(ganti dengan api-key , minta ke admin)'
```

- Respon sukses
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "Login nomor success",
    "data": {
      "access_token": "eyJhbGciOiJSUzI1",
      "expires_in": 299,
      "refresh_expires_in": 7775999,
      "refresh_token": "eyJhbGciOiJIUzI1NiIsIn",
      "token_type": "Bearer",
      "id_token": "eyJhbGciOiJSU0EtT0FFUCIsImVuYyI6IkEyNTZDQ",
      "not-before-policy": 0,
      "session_state": "9ac34118-5ba1-4f6a-a90e-7290f28952ff",
      "scope": "openid profile email"
    }
  },
  "stderr": ""
}
```

  - Cek Login:
```bash
curl -X GET 'https://api.tuyull.my.id/api/v1/cek-login?nomor=(masukan nomor hp)' -H 'Authorization:(ganti dengan api-key , minta ke admin)'
```

  - Respon Succes
```json
{
  "status": "success",
  "message": "Nomor sudah login"
}
```

  - Rubah password
```bash
curl -X GET 'http://api.tuyull.my.id:5500/api/v1/ubah-password?id_telegram=(masukan id telegram)&password_lama=(masukan passwd lama)&password_baru=(masukan passwd baru)' -H 'Authorization:(ganti dengan api-key , minta ke admin)'
```

  - Respon
```json
{
  "status": "success",
  "message": "Password berhasil diperbarui"
}
```
</details>

<details> <summary>2. Pembelian paket (klik untuk lihat)</summary>

  - List Api:
```bash
- produk (digunakan untuk melihat produk yang tersedia)
- dor (digunakan untuk pembelian)
```

  - list kode_buy:
```bash
Noted :
Slalu melihat kode_buy di api get produk, di stiap paket stiap paket ada kode_buy di gunakan untuk proses pembelian, jika tidak sesuai dengan kode_buy di pastikan pembelian gagal
  
- no_pancingan
- pancinganv1
- addon_dana
- addon_satuan
```

  - Get Produk:
```bash
curl -X GET 'https://api.tuyull.my.id/api/v1/produk' -H 'Authorization:(ganti dengan api-key , minta ke admin)'
```

  - Respon
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

  - Pembelian:
```bash
curl -X POST 'https://api.tuyull.my.id/api/v1/dor' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api-key , minta ke admin)' -H ':' -d '{
 "kode": "(ganti dengan kode_buy sesuai nama_paket yang mau di beli, cek api get produk untuk melihat",
 "nama_paket": "(ganti dengan nama paket yang ada di api get produk)",
 "nomor_hp": "(masukan nomor hp)",
 "payment": "(masukan method pembayaran yang suport sesuai nama_paket yang mau di beli, cek api get produk untuk melihat)",
 "id_telegram": "(masukan id telegram)",
 "password": "(masukan password)"
}'
```

- respon sukses (method payment ewallet)
```json
{
  "status": "success",
  "code": 0,
  "info_saldo_panel": {
    "id_telegram": "790266",
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

- respon sukses (method payment pulsa)
```json
{
  "status": "success",
  "code": 0,
  "info_saldo_panel": {
    "id_telegram": "790266",
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
      "id_telegram": "790266",
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

<details> <summary>3. Managemen Akrab (klik untuk lihat)</summary>

  - Sebelom menggunakan API nomor di wajibkan login otp terlebih dahulu, api login sudah di sediakan
  - Berikut action yang bisa di gunakan di API
```bash
1. add (digunakan untuk menambhkn anggota) fee 1k
2. edit (digunakan untuk mengedit kuota bersama) fee 500p
3. info (digunakan untuk mendapatkan informasi anggota dan detail kuota) free
4. kick (digunakan untuk mengeluarkan anggota dari paket akrab) free
5. slot (digunakan untuk mendapatkan slot yang tersedia / kosong) free
6. bekasankick (digunakan untuk mencari perslot yang sisa kuota 0kb otomatis terkick dri anggota Akrab) fee 500p
7. extraslot (digunakan untuk membeli slot tambahan akrab, sediakan pulsa di nomor 20k) fee 500p
```

  - Berikut contoh curl stiap action :

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
 "input_gb": (ganti dengan input gb, contoh: 10 = 10gb, jika ingin edit kuber 0kb di kasih tanda kurung " contoh : "0") 
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

  - action (kick)
```bash
curl -X POST 'https://api.tuyull.my.id/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "kick",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(ganti nomor pengelola, wajib login terlebih dahulu)",
 "nomor_slot": (ganti dengan nomor slot yang mau di add, contoh: 1)
}'
```

  - respon sukses action (kick)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "Kick anggota berhasil",
    "data": {
      "slot": 3,
      "nomor-anggota": "628778177",
      "alias": "Rendii",
      "slot_expiration": "2025-06-26 17:00:00"
    },
    "info_saldo_panel": {
      "id_telegram": "790266",
      "role": "super_reseller",
      "saldo_tersedia": 3323700,
      "catatan": "Saldo tidak dipotong bukan action add"
    }
  },
  "stderr": ""
}
```

  - action (info)
```bash
curl -X POST 'https://api.tuyull.my.id/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "info",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(ganti nomor pengelola, wajib login terlebih dahulu)"
}'
```

  - respon sukses action (info)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "nomor-pengelola": "62878560",
    "jumlah_slot": 3,
    "data_slot": [
      {
        "slot-ke": 1,
        "alias": "indr",
        "nomor": "628317866",
        "slot-id": 14015674,
        "sisa-add": 0,
        "kuota-bersama": 0,
        "pemakaian-kuota-bersama": 0,
        "benefit": "Lokal + RW + Nasional",
        "total-kuota-benefit": 25,
        "sisa-kuota-benefit": 22.6
      },
      {
        "slot-ke": 2,
        "alias": "Real",
        "nomor": "6287859",
        "slot-id": 14015675,
        "sisa-add": 2,
        "kuota-bersama": 25,
        "pemakaian-kuota-bersama": 25,
        "benefit": "Lokal + RW + Nasional",
        "total-kuota-benefit": 40,
        "sisa-kuota-benefit": 5.63
      },
      {
        "slot-ke": 3,
        "alias": "",
        "nomor": "",
        "slot-id": 14015683,
        "sisa-add": 0,
        "kuota-bersama": 0,
        "pemakaian-kuota-bersama": 0,
        "benefit": "Lokal + RW + Nasional",
        "total-kuota-benefit": 0,
        "sisa-kuota-benefit": 0
      }
    ],
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

  - action (slot)
```bash
curl -X POST 'https://api.tuyull.my.id/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "slot",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(ganti nomor pengelola, wajib login terlebih dahulu)"
}'
```

  - respon sukses action (slot)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "nomor-pengelola": "6287856",
    "jumlah_slot": 3,
    "data_slot": [
      {
        "slot-ke": 1,
        "alias": "indri",
        "nomor": "628317",
        "sisa-add": 0,
        "slot-id": 14015,
        "family-member-id": "RlBNQ19f3rZslGkdQKZ_VLRArKk"
      },
      {
        "slot-ke": 2,
        "alias": "Real",
        "nomor": "62878590",
        "sisa-add": 2,
        "slot-id": 14015,
        "family-member-id": "RlBNQ19fY8xjcquF6en1Wu01cI_3k"
      },
      {
        "slot-ke": 3,
        "alias": "",
        "nomor": "",
        "sisa-add": 0,
        "slot-id": 140156,
        "family-member-id": "RlBNQ19fHjxf1Q3FkiSMLMRV"
      }
    ],
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

  - action (bekasankick)
```bash
curl -X POST 'https://api.tuyull.my.id/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "bekasankick",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(ganti nomor pengelola, wajib login terlebih dahulu)"
}'
```

  - respon sukses action (bekasankick)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "nomor-pengelola": "6287856093773",
    "jumlah_slot": 3,
    "data_slot": [
      {
        "slot-ke": 1,
        "alias": "ind",
        "nomor": "6283178",
        "slot-id": 1401,
        "family-member-id": "RlBNQ19fCXrWze9Sju23SLR",
        "sisa-add": 0,
        "kuota-bersama": 0,
        "pemakaian-kuota-bersama": 0,
        "benefit": "Lokal + RW + Nasional",
        "total-kuota-benefit": 25,
        "sisa-kuota-benefit": 21.35
      },
      {
        "slot-ke": 2,
        "alias": "Real",
        "nomor": "6287859",
        "slot-id": 14015,
        "family-member-id": "RlBNQ19fSbC78tZDaKnVD",
        "sisa-add": 2,
        "kuota-bersama": 25,
        "pemakaian-kuota-bersama": 0,
        "benefit": "Lokal + RW + Nasional",
        "total-kuota-benefit": 40,
        "sisa-kuota-benefit": 0,
        "kick-otomatis": true,
        "hapus_status": "berhasil kick dari paket Akrab karena sisa kuota benefit 0kb"
      },
      {
        "slot-ke": 3,
        "alias": "",
        "nomor": "",
        "slot-id": 14015,
        "family-member-id": "RlBNQ19fD1Sc8v5",
        "sisa-add": 0,
        "kuota-bersama": 0,
        "pemakaian-kuota-bersama": 0,
        "benefit": "Lokal + RW + Nasional",
        "total-kuota-benefit": 0,
        "sisa-kuota-benefit": 0,
        "kick-otomatis": true,
        "hapus_status": "berhasil kick dari paket Akrab karena sisa kuota benefit 0kb"
      }
    ],
    "info_saldo_panel": {
      "id_telegram": "7902",
      "role": "super_reseller",
      "saldo_tersedia": 3314700,
      "catatan": "Saldo tidak dipotong bukan action add"
    }
  },
  "stderr": ""
}
```

  - action (extraslot)
```bash
curl -X POST 'https://api.tuyull.my.id/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "extraslot",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(ganti nomor pengelola, wajib login terlebih dahulu)"
}'
```

  - respon action (extraslot)
```json
blom ada data
```
</details>

<details> <summary>4. Tools (klik untuk lihat)</summary>

  - List action Api tools :
```bash
- cek_saldo (mengecek saldo panel)
- cek_kuota (mengecek kuota dan pulsa, nomor wajib sudah login otp)
- cek_dompul (mengecek kuota via api dompul)
```

  - action (cek_saldo)
```bash
curl -X POST 'https://api.tuyull.my.id/api/tools' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "cek_saldo",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)"
}'
```

  - respon action (cek_saldo)
```json

```

  - action (cek_kuota)
```bash
curl -X POST 'https://api.tuyull.my.id/api/tools' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "cek_kuota",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(masukan nomor hp, wajib login terlebih dahulu)"
}'
```

  - respon action (cek_kuota)
```json

```

  - action (cek_dompul)
```bash
curl -X POST 'https://api.tuyull.my.id/api/tools' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "cek_dompul",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(masukan nomor hp)"
}'
```

  - respon action (cek_dompul)
```json

```
</details>