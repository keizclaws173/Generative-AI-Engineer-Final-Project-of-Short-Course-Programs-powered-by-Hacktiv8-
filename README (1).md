# Kak Rumi — Chatbot AI Teman Belajar (Gemini API)

Final project: chatbot berbasis LLM yang memakai **Gemini API** (Google) dengan use case **education bot**. Chatbot memiliki sejumlah parameter kreatif yang bisa diatur langsung dari antarmuka.

## 1. Use Case

**Education Bot / Study Assistant** — "Kak Rumi" adalah asisten belajar untuk siswa dan mahasiswa Indonesia. Bot berperan seperti kakak tingkat yang membimbing lewat percakapan: menjawab pertanyaan pelajaran, membuatkan jadwal belajar, memberi kuis singkat, dan menjaga konteks percakapan agar terasa personal.

## 2. Arsitektur Singkat

```
Browser (index.html)
   │  fetch()  +  header x-goog-api-key
   ▼
Gemini API  (generateContent, model: gemini-3.6-flash)
   │
   ▼
Balasan teks → dirender sebagai bubble chat
```

- Frontend murni HTML/CSS/JavaScript (tanpa framework), mudah di-deploy ke **GitHub Pages**.
- API key **tidak ditulis di kode**. Pengguna menempelkannya di panel "Koneksi Gemini API"; key hanya disimpan di memori browser selama halaman terbuka.
- Riwayat percakapan (`conversationHistory`) dikirim ulang di setiap request sebagai `contents` → implementasi fitur **memory**.
- System prompt dibangun dinamis oleh `buildSystemPrompt()` dan dikirim lewat `systemInstruction`.

## 3. Parameter Konfigurasi Kreatif

| Parameter | Implementasi | Efek terhadap perilaku bot |
|---|---|---|
| **Domain pengetahuan** | Dropdown: Umum, Matematika, Bahasa & Sastra, Sains, Persiapan Ujian | Mengubah instruksi keahlian pada system prompt |
| **Gaya bahasa** | Toggle Santai / Formal | Mengubah nada bicara (bahasa santai suportif vs baku akademis) |
| **Tingkat kreativitas** | Toggle Konsisten / Seimbang / Kreatif | Mengubah instruksi gaya jawaban (presisi vs analogi & mnemonik) |
| **Pilihan model** | Dropdown model Gemini | Memilih model yang dipakai untuk menjawab |
| **Memory** | Riwayat percakapan dikirim penuh tiap request | Bot mengingat nama, topik, dan kesulitan yang sudah disebut dalam sesi |
| **Rekomendasi** | Tombol aksi cepat: jadwal belajar, kuis singkat, ringkasan sesi | Prompt siap-pakai yang meminta rekomendasi berbasis konteks percakapan |

> Catatan: dokumentasi Gemini menyatakan parameter `temperature`, `top_p`, dan `top_k` sudah deprecated pada Gemini 3.6 Flash dan model sesudahnya (diabaikan, dan di generasi mendatang menghasilkan error). Karena itu tingkat kreativitas diatur lewat **system instruction**, bukan slider temperature.

## 4. Cara Menjalankan

1. Dapatkan API key gratis di [Google AI Studio](https://aistudio.google.com/apikey).
2. Buka `index.html` (klik dua kali, atau lewat GitHub Pages / server statis):

```bash
python3 -m http.server 8000
# lalu buka http://localhost:8000
```

3. Tempel API key di panel kiri, pilih pengaturan, lalu mulai bertanya.

> Jangan pernah menulis API key langsung di file kode atau meng-commit-nya ke GitHub.

## 5. Deploy ke GitHub Pages

1. Upload `index.html` dan `README.md` ke repo (branch `main`).
2. **Settings → Pages**, pilih branch `main` dan folder `/ (root)`, lalu Save.
3. Halaman live tersedia di `https://<username>.github.io/<nama-repo>/`.

## 6. Struktur File

```
├── index.html   # seluruh UI + logika chatbot + pemanggilan Gemini API
└── README.md    # dokumentasi ini
```

## 7. Pengembangan Lanjutan

- Backend kecil (Node.js/Flask) agar API key tersimpan di server.
- Simpan riwayat ke database agar memory bertahan lintas sesi.
- Integrasi API eksternal, misalnya kalender akademik untuk jadwal belajar otomatis.
