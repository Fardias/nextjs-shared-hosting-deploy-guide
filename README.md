
# Deploy Next.js 15 (App Router) ke cPanel dengan Node.js (DomaiNesia)

Panduan ini menjelaskan langkah demi langkah untuk mendeploy aplikasi **Next.js 15 (App Router)** ke cPanel menggunakan fitur **Node.js App** dari DomaiNesia.

---

## 🧱 Persiapan Awal

Pastikan kamu sudah memiliki:

- Project **Next.js 15** (App Router).
- Akses ke **cPanel** dengan fitur **Setup Node.js App** (DomaiNesia sudah mendukung).
- File manager cPanel.
- Folder `node_modules` dan `src` **tidak perlu disertakan** saat upload.

---

## ✍️ Tambahkan `server.js` di Root Project

Buat file `server.js` di direktori root project Next.js kamu, lalu isi dengan kode berikut:

```js
import { createServer } from 'http';
import { parse } from 'url';
import next from 'next';

const port = process.env.PORT || 3000;
const hostname = '0.0.0.0';
const dev = process.env.NODE_ENV !== 'production';

const app = next({ dev, hostname, port });
const handle = app.getRequestHandler();

app.prepare().then(() => {
  createServer((req, res) => {
    const parsedUrl = parse(req.url, true);
    handle(req, res, parsedUrl);
  }).listen(port, hostname, () => {
    console.log(
      `> Server listening at http://${hostname}:${port} as ${
        dev ? 'development' : 'production'
      }`
    );
  });
});
```

---

## ⚙️ Ubah Script di `package.json`

Edit bagian `scripts` di `package.json` seperti ini:

```json
"scripts": {
  "start": "NODE_ENV=production node server.js",
  ...
}
```

---

## 🏗️ Build Project

Di terminal lokal kamu:

```bash
npm run build
```

Ini akan menghasilkan folder `.next` dan file-file siap produksi.

---

## 🗂️ Persiapkan File Upload

1. **Pilih semua file** di direktori project kecuali:
   - `node_modules`
   - `src`
2. **Kompres** file dan folder yang dipilih ke dalam satu file `.zip`.

---

## 🌐 Setup Node.js di cPanel

1. Login ke **cPanel**, buka menu **Setup Node.js App**.
2. Klik tombol **Create Application**, lalu isi seperti ini:

   - **Node.js version**: `22.14.0`
   - **Application mode**: `production`
   - **Application root**: `mynextjsapp` (ini nama folder upload kamu)
   - **Application URL**: `https://mynextjsapp.com` *(sesuaikan dengan domain/subdomain)*
   - **Application startup file**: `server.js`

3. Tambahkan Environment Variable:

   - **Name**: `NODE_ENV`
   - **Value**: `production`

   *(Jika kamu upload file `.env`, kamu tidak perlu menuliskan variabelnya lagi di sini)*

---

## 📁 Upload dan Ekstrak File

1. Masuk ke **File Manager** di cPanel.
2. Arahkan ke folder **Application root** yang sudah kamu tentukan, misalnya `mynextjsapp`.
3. Upload file `.zip` hasil kompres tadi.
4. Setelah upload selesai, **ekstrak** file tersebut di folder yang sama.

---

## 🚀 Jalankan Aplikasi Node.js

1. Kembali ke menu **Setup Node.js App**.
2. Klik **Stop App** terlebih dahulu.
3. Klik **Run NPM Install** dan tunggu proses selesai.
4. Setelah selesai, klik **Start App**.

---

## ✅ Selesai!

Sekarang aplikasi **Next.js 15 (App Router)** kamu sudah berhasil berjalan di cPanel menggunakan Node.js 🎉

---

## 📌 Catatan Tambahan

- Pastikan file `server.js` ada di root folder aplikasi (sesuai `Application startup file`).
- Gunakan file `.env` jika ada konfigurasi environment tambahan.
- Kamu bisa cek log aplikasi di panel **Node.js App** > **View Logs** jika mengalami error saat start.

---
