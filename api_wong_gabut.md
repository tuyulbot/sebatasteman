
# Dokumentasi API Wong Gabut

## 1. Login Nomor

### 1.1 Ringkasan Endpoint

| Endpoint           | HTTP | Path / Query                             | Kegunaan                                     |
|--------------------|------|------------------------------------------|----------------------------------------------|
| **minta-otp**      | GET  | `/minta-otp?nomor_hp=62xxxxxxxxxx`       | Meminta kode OTP                             |
| **verif-otp**      | GET  | `/verif-otp?nomor_hp=62xxxxxxxxxx&kode_otp=123456` | Verifikasi OTP & login                       |
| **cek-login**      | GET  | `/cek-login?nomor=62xxxxxxxxxx`          | Cek status login nomor                       |
| **ubah-password**  | GET  | `/ubah-password?id_telegram=ID&password_lama=OLD&password_baru=NEW` | Ganti password panel                         |

> **Header Wajib**  
> `Authorization: <API-KEY>`

### 1.2 Contoh Curl

```bash
curl -X GET 'https://api.tuyull.my.id/api/v1/minta-otp?nomor_hp=6287777334689' -H 'Authorization: <API-KEY>'
```

## 2. Pembelian Paket

### 2.1 Alur Singkat

1. **GET /produk**: Ambil daftar paket + `kode_buy`
2. **POST /dor**: Kirim `kode_buy`, detail pemesan, dan metode pembayaran

### 2.2 Endpoints

| Endpoint | HTTP | Path | Deskripsi |
|----------|------|------|-----------|
| **produk** | GET | `/produk` | Daftar semua produk aktif |
| **dor** | POST | `/dor` | Melakukan pembelian paket |

### 2.3 Field penting `dor`

| Field           | Tipe   | Keterangan                                                                           |
|-----------------|--------|---------------------------------------------------------------------------------------|
| `kode`          | string | **kode_buy** hasil dari endpoint **produk**                                           |
| `nama_paket`    | string | Persis seperti di daftar produk                                                       |
| `nomor_hp`      | string | Nomor pengelola / penerima paket                                                      |
| `payment`       | string | `dana`, `shopee`, `gopay`, `pulsa`, dll – harus sesuai `payment_suport` produk        |
| `id_telegram`   | string | ID user di panel                                                                      |
| `password`      | string | Password panel                                                                        |

### 2.4 Contoh Curl (pembelian)

```bash
curl -X POST 'https://api.tuyull.my.id/api/v1/dor' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: <API-KEY>' \
  -d '{
        "kode": "pancinganv1",
        "nama_paket": "Xtra Combo Spesial 8GB",
        "nomor_hp": "6287777334689",
        "payment": "dana",
        "id_telegram": "790266",
        "password": "pass123"
      }'
```

## 3. Manajemen Akrab

### 3.1 Biaya & Action

| Action        | Biaya (Rp) | Deskripsi                            |
|---------------|-----------:|--------------------------------------|
| `add`         | 1 000      | Tambah anggota                       |
| `edit`        | 500        | Edit kuota bersama                   |
| `info`        | 0          | Info anggota + kuota                 |
| `kick`        | 0          | Keluarkan anggota                    |
| `slot`        | 0          | Lihat slot kosong/terisi             |
| `bekasankick` | 500        | Auto-kick slot kuota 0 KB            |
| `bekasan`     | 500        | Cek slot kuota 0 KB (tanpa kick)     |
| `extraslot`   | 500        | Beli slot tambahan (butuh pulsa 20K) |

### 3.2 Payload umum

| Field           | Wajib | Keterangan |
|-----------------|:----:|------------|
| `action`        | ✔︎ | Salah satu dari tabel di atas |
| `id_telegram`   | ✔︎ | ID user panel |
| `password`      | ✔︎ | Password panel |
| `nomor_hp`      | ✔︎ | Nomor pengelola (harus sudah login OTP) |
| Field lain      | - | Tergantung action (lihat detail) |

## 4. Tools

| Action        | Endpoint  | Kegunaan                                     | Extra Field |
|---------------|-----------|----------------------------------------------|-------------|
| `cek_saldo`   | `/api/tools` | Lihat saldo panel                           | - |
| `cek_kuota`   | `/api/tools` | Cek kuota & pulsa (nomor harus login)       | `nomor_hp` |
| `cek_dompul`  | `/api/tools` | Kuota via API Dompul                        | `nomor_hp` |

## 5. Format Tanggal & Waktu

| Contoh Unix | Contoh Output | Keterangan pola |
|-------------|---------------|-----------------|
| `1759872000`| `07-06-2025`  | `strftime("%d-%m-%Y")` |
| `1759872000`| `07-06-2025 17:00:00` | `strftime("%d-%m-%Y %H:%M:%S")` |
