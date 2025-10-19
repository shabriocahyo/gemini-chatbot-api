# Gemini Chatbot — README

**Deskripsi singkat**

Proyek ini adalah chatbot web sederhana yang memanfaatkan **Gemini (Generative AI)** sebagai backend AI. Aplikasi terdiri dari:

- Backend: Node.js + Express yang menerima permintaan chat dan meneruskan percakapan ke Gemini (melalui SDK atau REST).
- Frontend: Static files (`public/`) berisi HTML/CSS/JS untuk UI chat.

Tujuan README ini: memberikan petunjuk langkah-demi-langkah untuk men-setup, menjalankan, dan menguji proyek sehingga bisa Anda gunakan sebagai tugas akhir atau demo.

---

## Struktur direktori (disarankan)
```
gemini-chatbot-api/
├─ public/
│  ├─ index.html
│  ├─ style.css
│  └─ script.js
├─ .env
├─ .gitignore
├─ package.json
└─ index.js
```

---

## Prasyarat
- Node.js v18+ terinstall (cek `node -v`).
- Akun dengan akses ke Gemini API (API key).
- Editor (mis. VS Code).

---

## Variabel environment (.env)
Buat file `.env` di root proyek dan isi seperti berikut (contoh):

```
GEMINI_API_KEY=isi_api_key_anda_di_sini
GEMINI_MODEL=gemini-1.5-flash
PORT=3000
```

> **Jangan** commit file `.env` ke GitHub. Masukkan `.env` ke `.gitignore`.

---

## Instalasi & dependensi
Di folder proyek jalankan:

```bash
npm init -y
npm install express dotenv cors @google/genai
# opsional (untuk development):
npm install --save-dev nodemon
```

Contoh `package.json` scripts sederhana:

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js"
}
```

---

## Menjalankan proyek (lokal)
1. Pastikan `.env` sudah terisi.
2. Install dependensi (`npm install`).
3. Jalankan server:

```bash
npm start
# atau saat development
npm run dev
```

4. Buka browser ke `http://localhost:3000/`.

---

## Endpoint API
Backend menyediakan endpoint HTTP POST sederhana untuk chat:

`POST /api/chat`

- Body JSON yang dikirim ke backend harus berisi `messages` (array objek `{ role, content }`). Contoh:

```json
{
  "messages": [
    {"role": "user", "content": "Halo, siapa kamu?"}
  ]
}
```

- Response sukses berisi:

```json
{ "result": "Teks jawaban dari Gemini..." }
```

### Contoh curl
```bash
curl -X POST http://localhost:3000/api/chat   -H "Content-Type: application/json"   -d '{"messages":[{"role":"user","content":"Halo"}] }'
```

---

## Contoh integrasi dengan SDK (ringkasan)
Jika Anda menggunakan package `@google/genai` (atau SDK sesuai materi):
- Inisialisasi client dengan `GEMINI_API_KEY`.
- Panggil method generatif (mis. `generateContent`) dengan parameter `model` dan `input` / `messages`.
- Sesuaikan transformasi `messages` ke format yang diminta SDK (biasa: `role` + `parts` atau `content`).

> Catatan: Jika SDK berbeda versi, cek dokumentasi SDK untuk cara inisialisasi dan format response.

---

## Frontend (public/)
- `index.html`: Form sederhana untuk kirim pesan dan menampilkan percakapan.
- `script.js`: Menyimpan history (array `messages`), mengirim `fetch('/api/chat')`, menampilkan placeholder "thinking..." dan mengganti saat jawaban datang.
- `style.css`: Styling UI sederhana.

Pastikan request di frontend mengirim `messages` yang berisi seluruh riwayat percakapan jika ingin multi-turn context.

---

## Testing & Debugging
- Untuk cek backend: gunakan Postman / curl langsung ke `/api/chat`.
- Periksa log terminal untuk error (mis. masalah API key, network, atau format response).
- Jika tidak mendapat jawaban dari Gemini: cek `GEMINI_API_KEY`, nama model (`GEMINI_MODEL`), dan rate limits.

Masalah umum dan solusinya:
- `401 Unauthorized` → API key salah atau tidak aktif.
- `timeout` atau `500` → periksa payload yang dikirim, atau lihat pesan error penuh dari server.
- `CORS` → biasanya tidak terjadi karena frontend dilayani oleh Express `static`, tetapi bila frontend terpisah, aktifkan `cors()` di Express.

---

## Tips Prompt Engineering & Pengaturan Model
- Berikan konteks singkat pada setiap percakapan (system prompt atau messages pertama) untuk perilaku yang konsisten.
- Gunakan parameter seperti `temperature` atau `max_tokens` (jika tersedia pada SDK) untuk mengatur kreativitas dan panjang jawaban.
- Batasi panjang history jika ingin menghemat token / biaya—simpan ringkasan percakapan alih-alih seluruh pesan bila perlu.

---

## Keamanan & Privasi
- Jangan masukkan API key ke repository publik.
- Validasi input user di backend untuk mencegah injeksi payload berbahaya.
- Jika menyimpan percakapan, pastikan data sensitif dihapus atau disamarkan.

---

## Deployment singkat
- Anda bisa deploy ke platform seperti Heroku, Vercel (backend di serverless function), atau layanan VPS.
- Pastikan environment variables di-set di platform hosting.
- Jika menggunakan Vercel/Netlify untuk frontend dan hosting terpisah, jangan lupa mengizinkan CORS atau menggunakan server proxy.

---

## Cara submit (untuk tugas)
1. Pastikan repository Git diinisialisasi dan sudah commit semua file (kecuali `.env`).
2. Push ke GitHub:
```bash
git remote add origin https://github.com/username/your-repo.git
git branch -M main
git push -u origin main
```
3. Sertakan link repo di form submit pada LMS dan tuliskan instruksi singkat cara run di README (yang ini).

---

## README singkat untuk penilaian (copy-paste jika perlu)
```
Project: Gemini Chatbot
Run: npm install && npm start
Open: http://localhost:3000/
Env: set GEMINI_API_KEY and GEMINI_MODEL in .env
```

---

## Lisensi & Credits
- Anda bebas mengadaptasi README ini untuk tugas Anda.
- Sebutkan sumber materi kursus Anda jika diperlukan pada cover/penjelasan tugas.

---

Jika Anda mau, saya bisa:
- Meng-generate file README sebagai file markdown yang bisa Anda download, atau
- Langsung buat semua file proyek (`index.js`, `public/*`, `package.json`) di canvas juga.

Ketik salah satu pilihan: **`download readme`** atau **`buat file proyek`** jika ingin saya lanjutkan.
