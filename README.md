###        Tutorial pembuatan donat bolong kentang
###           Semoga membantu:v

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
curl -X POST 'https://api.hidepulsa.com/api/v1/minta-otp' -H 'Content-Type:application/json' -H 'Authorization:(ganti api key)' -H ':' -d '{
 "id_telegram": "idtele",
 "password": "password",
 "nomor_hp": "nomor hp"
}'
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
curl -X POST 'https://api.hidepulsa.com/api/v1/verif-otp' -H 'Content-Type:application/json' -H 'Authorization:(api key)' -H ':' -d '{
 "id_telegram": "idtele",
 "password": "password",
 "nomor_hp": "nomor hp",
    "kode_otp": "otp"
}'
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
curl -X POST 'https://api.hidepulsa.com/api/v1/cek-login' -H 'Content-Type:application/json' -H 'Authorization:(api key)' -H ':' -d '{
 "id_telegram": "id tele",
 "password": "password",
 "nomor_hp": "nomor hp"
}'
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
curl -X GET 'http://api.hidepulsa.com/api/v1/ubah-password?id_telegram=(masukan id telegram)&password_lama=(masukan passwd lama)&password_baru=(masukan passwd baru)' -H 'Authorization:(ganti dengan api-key , minta ke admin)'
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
- addon_slow ( di khusukan untuk pembelian addon xuts dan xutp)
```

  - Get Produk:
```bash
curl -X GET 'https://api.hidepulsa.com/api/v1/produk' -H 'Authorization:(ganti dengan api-key , minta ke admin)'
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
curl -X POST 'https://api.hidepulsa.com/api/v1/dor' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api-key , minta ke admin)' -H ':' -d '{
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

  - Pembelian (addon_slow)
```bash
curl -X POST 'https://api.hidepulsa.com/api/v1/dor' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api-key , minta ke admin)' -H ':' -d '{
 "kode": "addon_slow",
 "nama_paket": "(ganti dengan nama paket yang ada di api get produk)",
 "nomor_hp": "(masukan nomor hp)",
 "payment": "(masukan method pembayaran yang suport sesuai nama_paket yang mau di beli, cek api get produk untuk melihat)",
 "id_telegram": "(masukan id telegram)",
 "password": "(masukan password)"
}'
```

  - Respon (addon_slow)
```json
{
  "status": "processing",
  "message": "Proses pembelian sedang berjalan, gunakan processId untuk cek status.",
  "processId": "b328393e-2549-4a37-8b03-4dce43aac4a5"
}
```
  - Deskripsi:
Ambil processId dari respon di atas dan gunakan untuk cek status pembelian menggunakan endpoint di bawah ini

  - Cek Status Pembelian (addon_slow)
```bash
curl -X GET 'https://api.tuyull.my.id/api/v1/dor/status/(ganti dengan proccessId yang di salin dri pembelian respon (addon_slow)' -H 'Authorization:(ganti dengan api-key)'
```

  - Respon cek status pembelian (addon_slow)
```json
{
  "status": "success",
  "processId": "1ff3bc4d-d488-4ddc-ab10-9b24e5a131ca",
  "result": {
    "status": "success",
    "data": {
      "code": "213",
      "code_detail": "213",
      "status": "FAILED",
      "message": "422 -> Failed call ipaas purchase, with status code:422 : null, ",
      "title": "",
      "description": ""
    }
  },
  "created_at": "2025-07-13T08:09:42.000Z",
  "updated_at": "2025-07-13T08:20:28.000Z",
  "saldo_dipotong": 500,
  "keterangan": "Saldo telah dipotong sebesar 500"
}
```

  - Deskripsi:
Jika respon sudah seperti di atas , maka pembelian sudah berhasil dan saldo sudah dipotong
</details>

<details> <summary>3. Managemen Akrab (klik untuk lihat)</summary>

  - Sebelom menggunakan API nomor di wajibkan login otp terlebih dahulu, api login sudah di sediakan
  - Berikut action yang bisa di gunakan di API
```bash
Tambahan : action : help (untuk melihat stiap description action di endpoint /api/akrab)
1. add (digunakan untuk menambhkn anggota) fee 600p
2. edit (digunakan untuk mengedit kuota bersama) fee 400p
3. info (digunakan untuk mendapatkan informasi anggota dan detail kuota) free
4. kick (digunakan untuk mengeluarkan anggota dari paket akrab) free
5. slot (digunakan untuk mendapatkan slot yang tersedia / kosong) free
6. bekasankick (digunakan untuk mengecek perslot yang sisa kuota 0kb otomatis terkick dri anggota Akrab) fee 250p
7. bekasan (digunakan untuk mengecek perslot yang sisa kuota 0kb tanpa adany proses kick) fee 150p
8. extraslot (digunakan untuk membeli slot tambahan akrab, sediakan pulsa di nomor 20k) free
```

  - Berikut contoh curl stiap action :

  - Tambahan action help
```bash
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "help",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)"
}'
```
  - Respon sukses action (help)
```json
{
  "status": "success",
  "message": "Daftar fee dan deskripsi untuk setiap action",
  "data": [
    {
      "action": "add",
      "fee": 850,
      "deskripsi": "Menambahkan anggota baru pada slot yang dipilih",
      "catatan": "Saldo akan terpotong jika bukan admin/premium"
    },
    {
      "action": "edit",
      "fee": 350,
      "deskripsi": "Mengubah kuota bersama pada slot yang dipilih",
      "catatan": "Saldo akan terpotong jika bukan admin/premium"
    },
    {
      "action": "bekasankick",
      "fee": 250,
      "deskripsi": "pengecekan kuota & pengekickan anggota yang kuotanya habis(otomatis)",
      "catatan": "Saldo akan terpotong jika bukan admin/premium"
    },
    {
      "action": "bekasan",
      "fee": 150,
      "deskripsi": "pengecekan kuota & tanpa pengekickan anggota yang kuotanya habis",
      "catatan": "Saldo akan terpotong jika bukan admin/premium"
    },
    {
      "action": "list",
      "fee": 0,
      "deskripsi": "Melihat daftar slot yang tersedia pada nomor pengelola",
      "catatan": "Gratis / tidak ada pemotongan saldo"
    },
    {
      "action": "help",
      "fee": 0,
      "deskripsi": "Melihat daftar fee & deskripsi action",
      "catatan": "Gratis / tidak ada pemotongan saldo"
    }
  ]
}
```

  - action (add)
```bash
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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

  - action (bekasan)
```bash
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "bekasan",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(ganti nomor pengelola, wajib login terlebih dahulu)"
}'
```

  - respon sukses action (bekasan)
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
        "sisa-kuota-benefit": 0
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
        "sisa-kuota-benefit": 0
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
curl -X POST 'https://api.hidepulsa.com/api/akrab' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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
- cek_sms (mengecek sms gagal dri myxl untuk pembelian addon xuts && xutp )
- cek_tt  (mengecek diskon nomor untuk paket unli turbo TikTok dll)
```

  - action (cek_saldo)
```bash
curl -X POST 'https://api.hidepulsa.com/api/tools' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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
curl -X POST 'https://api.hidepulsa.com/api/tools' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
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
curl -X POST 'https://api.hidepulsa.com/api/tools' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "cek_dompul",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(masukan nomor hp)"
}'
```

  - respon action (cek_dompul)
```json

```

  - action (cek_sms)
```bash
curl -X POST 'https://api.hidepulsa.com/api/tools' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "cek_sms",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(masukan nomor hp)"
}'
```

  - respon action (cek_dompul)
```json

```

  - action (cek_tt)
```bash
curl -X POST 'https://api.hidepulsa.com/api/tools' -H 'Content-Type:application/json' -H 'Authorization:(ganti dengan api key, minta ke admin)' -H ':' -d '{
 "action": "cek_tt",
 "id_telegram": "(ganti dengan id telegram)",
 "password": "(ganti dengn password, minta ke admin)",
 "nomor_hp": "(masukan nomor hp)",
 "is_enterprise": "False"
}'
```

  - respon action (cek_tt)
```json
{
  "status": "success",
  "code": 0,
  "data": [
    {
      "number": 1,
      "name": "Premium",
      "price": 50000,
      "original_price": 165000,
      "discount": 70,
      "validity": ""
    },
    {
      "number": 2,
      "name": "Super",
      "price": 30000,
      "original_price": 125000,
      "discount": 76,
      "validity": ""
    },
    {
      "number": 3,
      "name": "Standard",
      "price": 20000,
      "original_price": 85000,
      "discount": 77,
      "validity": ""
    },
    {
      "number": 4,
      "name": "Basic",
      "price": 10000,
      "original_price": 55000,
      "discount": 82,
      "validity": ""
    },
    {
      "number": 5,
      "name": "Netflix",
      "price": 25000,
      "original_price": 50000,
      "discount": 50,
      "validity": ""
    },
    {
      "number": 6,
      "name": "TikTok",
      "price": 25000,
      "original_price": 50000,
      "discount": 50,
      "validity": ""
    }
  ],
  "stderr": ""
}
```

  - Noted : untuk pembelian tiktok stiap nomor harga paket tiktok berbeda2, harga paket tiktok yang bisa di beli harga 30.000 pasti bisa di beli
</details>


<details> <summary>5. Pembelian produk PPOB, paket, dll (klik untuk lihat)</summary>

  - Deskripsi
```ini
Action Support:
  - cekproduk = cek produk yang tersedia
  - beliproduk = untuk pembelian paket

=> cekproduk (Payload)
  1. Global (Tanpa Filter):
     {
        "action": "cekproduk",
        "id_telegram": "<ID_TELEGRAM_USER>",
        "password": "<PASSWORD_USER>"
     }
    
    Catatan : harga tidak di tampilkan pada pengecekan global (Tanpa Filter)
  
  2. Filtered (category, brand, type):
     {
        "action": "cekproduk",
        "id_telegram": "<ID_TELEGRAM_USER>",
        "password": "<PASSWORD_USER>",
        "category": "data",
        "brand": "XL",
        "type": "Bebas Puas 5rb"
     }

    Catatan : untuk mengetahui category, brand, type yang tersedia bisa di cek menggunakan payload Global (Tanpa Filter) setelah dapat category, brand , dan type yang mau di cek akan muncul harga paketnya dan bisa langsung membeli setelah tau buyer_sku_code
```

  - Request cek produk
```bash
curl -X POST 'https://api.hidepulsa.com/api/ppob' -H 'Content-Type:application/json' -H 'Authorization:(Ganti api key)' -d '{
 "action": "cekproduk",
 "id_telegram": "ganti idtelegram",
 "password": "ganti password"
}'
```

  - Respon cek produk
```json
{
  "status": "success",
  "hasil": {
    "data": [
      {
        "product_name": "XL Data Pure Games 1 GB 30 Hari",
        "category": "Data",
        "brand": "XL",
        "type": "Games",
        "buyer_sku_code": "DRP1",
        "buyer_product_status": true,
        "seller_product_status": true,
        "unlimited_stock": true,
        "stock": 0,
        "multi": true,
        "start_cut_off": "23:0",
        "end_cut_off": "23:0",
        "desc": "DRP GAMES 1GB 30D"
      },
      {
        "product_name": "Indosat Tambah Masa Aktif Kartu 15 Hari",
        "category": "Masa Aktif",
        "brand": "INDOSAT",
        "type": "Umum",
        "buyer_sku_code": "IDMA15",
        "buyer_product_status": true,
        "seller_product_status": false,
        "unlimited_stock": true,
        "stock": 0,
        "multi": true,
        "start_cut_off": "23:45",
        "end_cut_off": "0:15",
        "desc": "-"
      }
    ]
  },
  "stderr":""
}
```

  Catatan : seperti sebelomnya yang sudah aku jelaskan pengecekn produk global tidak akan muncul harga, untuk melihat harga bisa di cek menggunakan filtered ambil (category, brand, type ), setelah dapat akan menampilkan detail produk sesuai type, untuk pembelin ambil (caetegory, brand, type, buyer_sku_code) untuk pembelian

  - Pembelian
```bash
curl -X POST 'https://api.hidepulsa.com/api/ppob' -H 'Content-Type:application/json' -H 'Authorization:(GANTI API KEY)' -d '{
 "action": "beliproduk",
 "id_telegram": "ganti idtelegram",
 "password": "ganti password",
 "category": "ganti category (cek bagian Request Cek Produk)",
 "brand": "ganti brand (cek bagian Request Cek Produk)",
 "type": "ganti type (cek bagian Request Cek Produk)",
 "ref_id": "(contoh ref_id : HIDEPULSA77657 / bebas untuk kode pembelian)", 
 "buyer_sku_code": "ganti kode produk (cek bagian Request Cek Produk)",
 "nomor_buyer": "ganti nomor buyer"
}'
```

  - Respon
```json
{
  "status": "success",
  "hasil": {
    "data": {
      "ref_id": "HIDEPULSA666599",
      "customer_no": "0877773",
      "buyer_sku_code": "prerl6lpy",
      "message": "Transaksi Pending",
      "status": "Pending",
      "rc": "03",
      "sn": "",
      "price": 5400
    },
    "sisa_saldo": 0,
    "harga_markup": 5400,
    "nama_produk": "XL HOTROD 500 MB 7 Hari",
    "ref_id": "HIDEPULSA666599"
  },
  "stderr": ""
}
```

  Catatan: Untuk melihat status pembayaran sukses / gagal bisa di request ulang dengan data payload yang sama ref_id, nomor_buyer, dan buyer_sku_code, sampe menemukan status Sukses, contoh

```json
{
  "status": "success",
  "message": "Update status transaksi",
  "hasil": {
    "ref_id": "HIDEPULSA666599",
    "customer_no": "087777",
    "buyer_sku_code": "prerl6lpy",
    "message": "Transaksi Sukses",
    "status": "Sukses",
    "rc": "00",
    "sn": "25090206768477",
    "price": 5400,
    "nama_produk": "XL HOTROD 500 MB 7 Hari"
  }
}
```

</details>

<details> <summary>6. Managemen Circel (klik utuk lihat)</summary>

  - List Action 
```ini
  => help = ( cek fee per action)
  => validasi_nomor = (validasi nomor anggota sebelum invit ke anggota circel)
  => create = (pembuatan group circel dan invit 1 anggota, sudah auto acc anggota)
  => invite = (Invite anggota circel baru, sudah auto acc anggota )
  => info = ( info detail anggota + sisa kuota)
  => bonus = ( Klaim bonus yang tersedia)
  => kick = ( kick anggota di circel anda)
```

  <details> <summary> => Action (help) (klik utuk lihat)</summary>

    - Action (help)
  ```bash
curl -X POST 'https://api.hidepulsa.com/api/circle' -H 'Content-Type:application/json' -H 'Authorization:(ganti api-key)' -H ':' -d '{
{
 "action": "help",
 "id_telegram": "68639",
 "password": "5aeb1fb7b"
}'
  ```

  - Respon sukses action (info)
```json
{
  "status": "success",
  "message": "Daftar fee dan deskripsi untuk setiap action",
  "data": [
    {
      "action": "create",
      "fee": 500,
      "deskripsi": "Membuat grup circel baru dan invit 1 anggota ( auto acc undangan circel )",
      "catatan": "Saldo akan terpotong jika bukan admin"
    },
    {
      "action": "invite",
      "fee": 300,
      "deskripsi": "Mengundang anggota baru ke grup circel yang sudah dibuat ( auto acc undangan circel )",
      "catatan": "Saldo akan terpotong jika bukan admin"
    },
    {
      "action": "info",
      "fee": 0,
      "deskripsi": "info detail grup circel dan sisa kuota",
      "catatan": "Gratis / tidak ada pemotongan saldo"
    },
    {
      "action": "bonus",
      "fee": 200,
      "deskripsi": "klaim bonus circel yang tersedia",
      "catatan": "Saldo akan terpotong jika bukan admin"
    }
  ]
}
```
  </details>

  <details> <summary> => Action (validasi_nomor) (klik utuk lihat)</summary>

    - Action (validasi_nomor)
  ```bash
curl -X POST 'https://api.hidepulsa.com/api/circle' -H 'Content-Type:application/json' -H 'Authorization:(ganti api-key)' -H ':' -d '{
{
 "action": "validasi_nomor",
 "id_telegram": "68639",
 "password": "5aeb1fb7b",
 "nomor_admin": "nomor pengelola yang mau di pake",
 "nomor_anggota": "nomor anggota"
}'
  ```

  - Respon nomor valid, action (validasi_nomor)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "code": "200-2001",
    "message": "Nomor valid lanjut proses invite anggota circel.",
    "detail": "Input msisdn is eligible",
    "info_saldo_panel": {
      "id_telegram": "6863",
      "role": "admin",
      "saldo_tersedia": 50000,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```
  - Respon nomor tidak valid, action (validasi_nomor)
```json
{
  "status": "error",
  "code": 0,
  "data": {
    "status": "error",
    "code": "400-4004",
    "message": "Response code tidak dikenali. Silakan cek kembali.",
    "detail": "User is already a participant of another family",
    "info_saldo_panel": {
      "id_telegram": "6863",
      "role": "admin",
      "saldo_tersedia": 50000,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```
  </details>


  <details> <summary> => Action (create) (klik utuk lihat)</summary>

    - Action (create)
  ```bash
curl -X POST 'https://api.hidepulsa.com/api/circle' -H 'Content-Type:application/json' -H 'Authorization:(ganti api-key)' -H ':' -d '{
{
 "action": "create",
 "id_telegram": "68639",
 "password": "5aeb1fb7b",
 "nomor_admin": "nomor pengelola yang mau di pake",
 "nomor_anggota": "nomor anggota",
 "nama_group": "nama group",
 "nama_admin": "nama pengelola circel",
 "nama_anggota": "nama anggota circel"
}'
  ```

  - Respon sukses action (create)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "Berhasil membuat grup dan menerima undangan.",
    "details": {
      "group_name": "hideen",
      "nomor_pengelola": "62877",
      "nomor_member": "6287",
      "owner_name": "kontollo",
      "member_name": "kuio",
      "group_id": "U0NfX2Yk6KBveJ0d8GedSlWWeoPB",
      "member_id": "U0NfX3mYZurpSVvlNt4nxwKSvg0r"
    },
    "data": {
      "code": "000",
      "status": "SUCCESS",
      "data": {
        "response_code": "200-00",
        "message": "Successfully processed the request"
      }
    },
    "info_saldo_panel": {
      "id_telegram": "686",
      "role": "admin",
      "saldo_tersedia": 50000,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```
  </details>

  <details> <summary> => Action (invite) (klik utuk lihat)</summary>

    - Action (invite)
  ```bash
curl -X POST 'https://api.hidepulsa.com/api/circle' -H 'Content-Type:application/json' -H 'Authorization:(ganti api-key)' -H ':' -d '{
 "action": "invite",
 "id_telegram": "68639",
 "password": "5aeb1fb7b",
 "nomor_admin": "nomor pengelola yang mau di pake",
 "nomor_anggota": "nomor anggota",
 "nama_anggota": "nama anggota circel"
}'
  ```

  - Respon Sukses action (invite)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "Berhasil invit dan menerima undangan.",
    "details": {
      "nomor_pengelola": "62877",
      "nomor_member": "62819",
      "member_name": "kuujjo",
      "group_id": "U0NfX5f82L5h0ikjGg_pU0jW0",
      "member_id": "U0NfX9PzelAbtQbg9oWr_2_"
    },
    "data": {
      "code": "000",
      "status": "SUCCESS",
      "data": {
        "response_code": "200-00",
        "message": "Successfully processed the request"
      }
    },
    "info_saldo_panel": {
      "id_telegram": "686",
      "role": "admin",
      "saldo_tersedia": 50000,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```
  </details>

  <details> <summary> => Action (info) (klik utuk lihat)</summary>

    - Action (info)
  ```bash
curl -X POST 'https://api.hidepulsa.com/api/circle' -H 'Content-Type:application/json' -H 'Authorization:(ganti api-key)' -H ':' -d '{
 "action": "info",
 "id_telegram": "68639",
 "password": "5aeb1fb7b",
 "nomor_admin": "nomor pengelola yang mau di pake"
}'
  ```

  - Respon sukses action (info)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "code": "000",
    "status": "SUCCESS",
    "data": {
      "summary": {
        "group_id": "U0NfXy8d1KKDoQuNLJD_oK7684nQ3HoX0BcjaXGe1zoA9V0UtyNadXAeyEwASySSzeAeWr_EeUTWxfactcHZH3Y",
        "group_name": "C12",
        "created_at": {
          "tanggal": "2025-06-09 18:51:20"
        },
        "total_free_slot": 5,
        "total_paid_slot": 0,
        "bonus_remaining": 0,
        "package": {
          "name": "Kuota Circle",
          "icon_url": "https://rinjani.myxl.xlaxiata.co.id/2023-01/internet2x.png?VersionId=null"
        },
        "detail_kuota": {
          "total-member": 1,
          "benefit": {
            "name": "Kuota Utama & Bonus",
            "total": "25.00 GB",
            "sisa": "24.68 GB",
            "pemakaian": "328.00 MB"
          }
        }
      },
      "members": [
        {
          "status": "ACTIVE",
          "member_id": "U0NfX42qN6lZ3ijQi56CPGJm0eLICk1Gw4mmgmTZAFdwgbiWOptalSO99Pt8BGAFISBeV2efeizZU869o1ZLDCE",
          "member_name": "C12",
          "msisdn": "62877656",
          "join_date": "2025-06-09 18:51:20",
          "member_role": "PARENT",
          "slot_type": "FREE",
          "subscriber_number": "U0NfX-su-4x6a16pd8RH8rlup8LYhwGT8bD1ui5YCfrbhCXnPkDVprnaXc3-sJnlmnrYesUttz_UHt923_IBhpT3",
          "total": "25.00 GB",
          "pemakaian": "2.00 KB",
          "tersisa": "25.00 GB"
        },
        {
          "nomor": 1,
          "status": "ACTIVE",
          "member_id": "U0NfX7jwhjvKjyUj9oFfWDQ2wp-2QjubD5OeLuXVvvnf1RBN3ryyD-OmlBs6x_Kj1rWXXwOl02fwIzCGmkVQC1A",
          "member_name": "g",
          "msisdn": "6287741",
          "join_date": "2025-06-09 18:51:59",
          "member_role": "MEMBER",
          "slot_type": "FREE",
          "subscriber_number": "U0NfXzlSAGDNmT3vX5WQxHs4WitGetNlstGjBiFnAmOED0kh1z6HFtosQNUtl5Ur28UmFBsB55FySrJBlX6qkCr9",
          "total": "25.00 GB",
          "pemakaian": "2.00 KB",
          "tersisa": "25.00 GB"
        }
      ]
    },
    "info_saldo_panel": {
      "id_telegram": "68639",
      "role": "admin",
      "saldo_tersedia": 50000,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```
  </details>

  <details> <summary>=> Action (bonus) (klik utuk lihat)</summary>

    - keterangan untuk action (bonus)
  ```ini
  Pada bagian "list_bonus" bisa di isi seperti ini sesuai kebutuhan:
    => "list_bonus": "list" = ( untuk melihat list bonus yang tersedia)
    => "list_bonus": "all" = (untuk mengeklaim semua bonus yang tersedia)
    => "list_bonus": "1" = (untuk mengklaim bonus satuan, contoh klaim bonus nomor 1 maka yang ke klaim bonus nomor 1)

  Noted: 
    => jika respon klaim bonus ada pesan seperti ini (HTTP_CLIENT_RESPONSE_ERR) coba di retry kembali apinya
  ```

  - Action (bonus)
```bash
curl -X POST 'https://api.hidepulsa.com/api/circle' -H 'Content-Type:application/json' -H 'Authorization:(ganti api-key)' -H ':' -d '{
 "action": "bonus",
 "id_telegram": "68639",
 "password": "5aeb1fb7b",
 "nomor_admin": "nomor pengelola yang mau di pake"
 "list_bonus": "bisa di cek di keterangan untuk action (bonus) jika bingung"
}'
```

  - Respon list action (bonus)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "Ditemukan 1 bonus.",
    "count": 1,
    "bonuses": [
      {
        "index": 1,
        "title": "Welcome Bonus, 250MB",
        "description": "-",
        "action_param": "U0NfXyAo6RnRgqSKs",
        "status": "-"
      }
    ],
    "info_saldo_panel": {
      "id_telegram": "13165",
      "role": "admin",
      "saldo_tersedia": 150125,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```

  - Respon sukses klaim bonus (satuan), action (kick)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "Bonus berhasil diklaim.",
    "group_name": "hhjk",
    "owner_name": "",
    "total_bonus": 1,
    "sukses": 1,
    "gagal": 0,
    "bonus": [
      {
        "index": 1,
        "nama_bonus": "Welcome Bonus XL Circle",
        "status": "SUCCESS",
        "pesan": ""
      }
    ],
    "info_saldo_panel": {
      "id_telegram": "1316596",
      "role": "admin",
      "saldo_tersedia": 150125,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```

  - Respon Eror klaim bonus action (bonus)
```json
{
  "status": "error",
  "code": 0,
  "data": {
    "status": "error",
    "message": "Beberapa bonus gagal diklaim.",
    "group_name": "PRIBADI",
    "owner_name": "AKU",
    "total_bonus": 1,
    "sukses": 0,
    "gagal": 1,
    "bonus": [
      {
        "index": 1,
        "nama_bonus": "Bonus Kuota Circle",
        "status": "FAILED",
        "pesan": "HTTP_CLIENT_REQUEST_ERR"
      }
    ],
    "info_saldo_panel": {
      "id_telegram": "1316596",
      "role": "admin",
      "saldo_tersedia": 150125,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```
  </details>

  <details> <summary>=> Action (kick) (klik utuk lihat)</summary>

    - keterangan untuk action (kick)
  ```ini
  Pada bagian "kick_nomor" bisa di isi seperti ini sesuai kebutuhan:
    => "kick_nomor": "list" = ( untuk melihat list anggota)
    => "kick_nomor": "1" = (untuk mengkick anggota dari circel anda, contoh klaim bonus nomor 1 maka yang terkick di nomor 1)
  ```

  - Action (kick)
```bash
curl -X POST 'https://api.hidepulsa.com/api/circle' -H 'Content-Type:application/json' -H 'Authorization:(ganti api-key)' -H ':' -d '{
 "action": "kick",
 "id_telegram": "68639",
 "password": "5aeb1fb7b",
 "nomor_admin": "nomor pengelola yang mau di pake"
 "kick_nomor": "bisa di cek di keterangan untuk action (kick) jika bingung"
}'
```

  - Respon list action (kick)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "mode": "list",
    "noted": "Jika status di member CANCELLED anggota tersebut sudah tidak aktif di Circle / sudah kick.",
    "total_member_non_parent": 4,
    "members": [
      {
        "index": 1,
        "member_name": "anuuer",
        "member_role": "MEMBER",
        "slot_type": "FREE",
        "status": "CANCELLED",
        "msisdn": "62877"
      },
      {
        "index": 2,
        "member_name": "anuer",
        "member_role": "MEMBER",
        "slot_type": "FREE",
        "status": "CANCELLED",
        "msisdn": "62877"
      },
      {
        "index": 3,
        "member_name": "anuer",
        "member_role": "MEMBER",
        "slot_type": "FREE",
        "status": "CANCELLED",
        "msisdn": "62877"
      },
      {
        "index": 4,
        "member_name": "jhgf",
        "member_role": "MEMBER",
        "slot_type": "FREE",
        "status": "CANCELLED",
        "msisdn": "628"
      }
    ],
    "info_saldo_panel": {
      "id_telegram": "131659",
      "role": "admin",
      "saldo_tersedia": 150125,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```

  - Respon succes kick action (kick)
```json
{
  "status": "success",
  "code": 0,
  "data": {
    "status": "success",
    "message": "kick_member circle berhasil",
    "group_id": "U0NfX86ao6m",
    "member": {
      "member_id": "U0NfX8CpsymMkxk8kZ",
      "member_name": "g",
      "member_role": "MEMBER",
      "slot_type": "FREE",
      "msisdn": "62877413"
    },
    "info_saldo_panel": {
      "id_telegram": "131659",
      "role": "admin",
      "saldo_tersedia": 150125,
      "catatan": "Saldo tidak dipotong"
    }
  },
  "stderr": ""
}
```
  </details>
</details>
