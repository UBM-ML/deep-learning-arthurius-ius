# DL Tuning Challenge — Group Assignment

> **Mata Kuliah:** DSB07 – Machine Learning
> **Topik:** Introduction of Deep Learning
> **Format:** Kelompok (3–4 orang)
> **Platform:** Google Colab + GitHub Classroom

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan tugas ini, mahasiswa diharapkan dapat:

1. Membedakan **parameter** (bobot, bias) dan **hyperparameter** (learning rate, batch size, dll) dalam praktik
2. Memahami pengaruh setiap hyperparameter terhadap kinerja model
3. Mengamati proses **feed forward** dan **back propagation** melalui kurva training
4. Membuat keputusan berbasis-data untuk meningkatkan akurasi model

## 🏆 Tantangan

Diberikan sebuah model neural network sederhana yang dilatih pada dataset **Fashion-MNIST** (klasifikasi 10 kelas pakaian).

**Tugas kelompok:**
> Capai **akurasi setinggi mungkin** pada test set dengan **HANYA mengubah hyperparameter** pada Section "🎛️ HYPERPARAMETER PANEL" di notebook. Dataset dan struktur evaluasi **tidak boleh** diubah.

**Baseline accuracy:** ~85% (kelompok diharapkan mencapai >88%)

## 📋 Aturan Main

✅ **Yang BOLEH diubah:**
- Jumlah hidden layer
- Jumlah neuron per layer
- Fungsi aktivasi (relu, tanh, sigmoid, dll)
- Optimizer (Adam, SGD, RMSprop, dll)
- Learning rate
- Batch size
- Jumlah epoch
- Teknik regularisasi sederhana (Dropout) — opsional

❌ **Yang TIDAK BOLEH diubah:**
- Dataset (Fashion-MNIST yang sudah disediakan)
- Train/test split
- Cara menghitung akurasi akhir
- Bagian "🔒 LOCKED" pada notebook

## 📝 Deliverables

Push ke repo ini sebelum deadline:

1. **`notebooks/solution.ipynb`** — notebook final dengan konfigurasi terbaik kalian (dan output sudah ter-run)
2. **`EXPERIMENTS.md`** — log minimal **5 eksperimen** (gunakan template di file tersebut)
3. **`REFLECTION.md`** — refleksi singkat (maks 1 halaman) menjawab pertanyaan refleksi
4. Update `TEAM.md` dengan nama anggota & NIM

## 🚀 Cara Memulai

1. Accept GitHub Classroom assignment → repo otomatis di-clone untuk tim kalian
2. Buka `notebooks/starter.ipynb` di Google Colab:
   - Cara cepat: ganti `github.com` pada URL notebook menjadi `colab.research.google.com/github`
   - Atau: di Colab, `File → Open notebook → GitHub tab → paste URL repo`
3. Jalankan baseline → catat akurasi awal di `EXPERIMENTS.md`
4. Mulai bereksperimen! Catat setiap percobaan.
5. Saat sudah menemukan konfigurasi terbaik, save sebagai `notebooks/solution.ipynb`
6. Commit & push ke GitHub

## 📊 Penilaian

| Komponen | Bobot |
|---|---|
| Kelengkapan & kualitas log eksperimen (`EXPERIMENTS.md`) | 30% |
| Kualitas refleksi (`REFLECTION.md`) | 30% |
| Akurasi final pada test set | 20% |
| Presentasi & teamwork | 20% |

> **Catatan:** Akurasi tertinggi BUKAN penentu utama. Yang lebih penting adalah *reasoning* — apakah kalian memahami **mengapa** suatu hyperparameter berpengaruh seperti yang teramati?

## 📚 Referensi Singkat

- Lihat `docs/HYPERPARAMETER_GUIDE.md` untuk cheat sheet
- Slide kuliah Pertemuan 12: *Introduction of Deep Learning*

## ❓ Pertanyaan?

Buka issue di repo ini atau tanyakan di forum kelas.

---

*Selamat ber-eksperimen! Ingat: deep learning adalah seni *trial-and-error* yang terstruktur.* 🧠⚙️
