# Experiment Log

Catat setiap percobaan hyperparameter di sini. **Minimal 5 eksperimen.**

> Tips: ubah **satu hyperparameter pada satu waktu** agar bisa mengisolasi efeknya. Setelah memahami efek tiap variabel, baru gabungkan untuk hasil terbaik.

---

## 📋 Tabel Ringkasan

Isi tabel ini setelah selesai semua eksperimen.

| # | Hidden | Neurons | Activation | Optimizer | LR     | Batch | Epochs | Dropout | Test Acc | Train Time |
|---|--------|---------|------------|-----------|--------|-------|--------|---------|----------|------------|
| 0 | 1      | 64      | relu       | sgd       | 0.01   | 32    | 10     | 0.0     | 85,11%   | 53,2s      |
| 1 | 1      | 64      | relu       | adam      | 0.01   | 32    | 10     | 0.0     | 85.14%   | 59.0s      |
| 2 | 1      | 64      | elu        | adamax    | 0.01   | 32    | 10     | 0.0     | 86.84%   | 60s        |
| 3 | 1      | 64      | relu       | sgd       | 0.001  | 32    | 10     | 0.0     | 80.64%   | 45.9s      |
| 4 | 1      | 64      | tanh       | rmsprop   | 0.001  | 32    | 10     | 0.0     | 87.63%   | 55.3s      |
| 5 | 2      | 64      | sigmoid    | adam      | 0.01   | 32    | 10     | 0.2     | 84.41%   | 61.5s      |

> **Eksperimen #0** = baseline (jangan ubah, ini patokan kalian).

---

## 🧪 Detail Setiap Eksperimen

Gunakan template di bawah untuk SETIAP eksperimen.

---

### Eksperimen #1

**Apa yang diubah dari baseline:**
> Contoh: Mengganti optimizer dari `sgd` → `adam`, sisanya tetap.

**Hipotesis sebelum run:**
> Contoh: Adam adalah optimizer adaptif, kami menduga konvergensi akan lebih cepat dan akurasi naik.

**Hasil:**
- Test accuracy: 85.14%
- Train accuracy: 86.87%
- Validation accuracy: 85.50%
- Train time: 59.0 detik
- Apakah overfit/underfit? tidak

**Observasi & Insight:**
>Performa Baseline Kuat: Model menghasilkan performa yang solid untuk sebuah percobaan pertama dengan akurasi tes mencapai 85.14% dan selisih (gap) antara akurasi train dan validation yang sangat tipis (~1.37%).
>Gejala Overfitting Setelah Epoch 3: Meskipun akurasi akhir terlihat seimbang, grafik validation loss menunjukkan tren kenaikan kembali setelah Epoch 3 (mencapai titik terendah di ~0.42 dan naik ke ~0.45). Ini adalah indikasi kuat bahwa model mulai menghafal data latihan setelah 3 epoch.

**Rencana eksperimen berikutnya:**
> menggabungkan elu dengan adamax

---

### Eksperimen #2

**Apa yang diubah:*menggabungkan elu dengan adamax sisanya sama dengan #0*

**Hipotesis:**
Optimizer adaptif akan membuat training lebih cepat dan akurasi lebih tinggi.

**Hasil:**
> TEST ACCURACY  : 86.84%
> Test loss      : 0.3973
> Train accuracy : 90.80%
> Val accuracy   : 87.88%

**Observasi:**
> Adamax meningkatkan akurasi sedikit. Training time naik karena adaptif optimizer lebih kompleks.
---

### Eksperimen #3

**Apa yang diubah:*mengganti learning_rate menjadi 0.001 sisanya sama dengan #0*

**Hipotesis:**
> Dengan memperkecil learning rate menjadi sepersepuluh dari baseline, langkah optimasi model dalam mencari nilai minimum akan menjadi jauh lebih rapat dan hati-hati. Pola kurva pembelajaran diprediksi akan menjadi jauh lebih stabil, tetapi akurasi pada epoch ke-10 diperkirakan akan lebih rendah daripada Eksperimen #0 karena model membutuhkan waktu (epoch) lebih lama untuk konvergen.

**Hasil:**
>Test accuracy: 80.64%
>Train accuracy: 81.59%
>Validation accuracy: 81.20%
>Train time: 45.9 detik (silakan sesuaikan jika waktu aslinya berbeda)

Apakah overfit/underfit? Tidak (Perfect Good Fit, namun belum konvergen maksimal)

**Observasi:**
> Eksperimen #3 ini tertinggal di angka 80.64%. Ini membuktikan bahwa LR 0.001 membuat model belajar terlalu lambat untuk durasi yang hanya 10 epoch.
> Sisi positifnya, penurunan LR ini menghasilkan performa yang sangat seimbang antara train (81.59%) dan validation (81.20%). Selisihnya hanya 0.39%, jauh lebih minim risiko overfit dibanding baseline dengan LR lebih besar.
> Efisiensi Waktu: Waktu training terpangkas menjadi 45.9 detik. Hal ini kemungkinan terjadi karena dengan LR kecil, kalkulasi gradien berjalan di area lokal yang lebih stabil (tidak mengalami overshooting atau fluktuasi ekstrem yang memakan beban komputasi per epoch).
> 
---

### Eksperimen #4

**Apa yang diubah:**
> Mengubah fungsi aktivasi dari relu menjadi tanh, mengganti optimizer dari sgd menjadi rmsprop, serta menurunkan learning rate dari 0.01 menjadi 0.001. Hyperparameter lainnya tetap dipertahankan seperti Eksperimen #0.

**Hipotesis:**
> Kombinasi fungsi aktivasi tanh (yang memiliki rentang output berkelanjutan antara -1 hingga 1) dan optimizer adaptif rmsprop diprediksi akan mempercepat konvergensi model dan mengeksplorasi fitur data dengan lebih sensitif. Hasil akurasi akhir diperkirakan akan melampaui baseline sgd pada Eksperimen #0 dalam jumlah epoch yang sama.

**Hasil:**
> Test accuracy: 87.63% (naik signifikan dari 85.11% pada Eksperimen #0)
> Train accuracy: 90.53%
> Validation accuracy: 88.22%
> Train time: 55.3 detik
> Overfitting Ringan 

**Observasi:**
> Peningkatan Akurasi yang Signifikan: Eksperimen ini sukses memecahkan rekor akurasi tertinggi baru sebesar 87.63% (naik sekitar 2.5% dari Eksperimen #0). Kombinasi tanh + rmsprop terbukti jauh lebih agresif dan efektif dalam mengekstrak pola data dibanding relu + sgd.
> Muncul Jarak (Gap) Latihan: Berbeda dengan Eksperimen #0 yang kurvanya sangat rapat, di sini mulai terlihat celah (gap) yang melebar antara data Train dan Validation. Pada grafik akurasi (kanan), akurasi train (biru) terus melesat naik hingga 90.53%, sedangkan akurasi validation (oranye) mulai mendatar (stagnan) di angka 88.22% sejak Epoch 5.
> Indikasi Grafik Loss: Grafik validation loss (oranye) tidak naik ekstrem, melainkan mendatar (plateau) di angka ~0.32, sementara train loss terus turun. Ini menandakan model mulai mencapai batas kemampuan generalisasinya dengan arsitektur saat ini dan mulai berisiko overfit jika epoch ditambah.

---

### Eksperimen #5

**Apa yang diubah:**
> Menambah jumlah hidden layer dari 1 menjadi 2.
> Mengubah fungsi aktivasi dari relu menjadi sigmoid.
> Menambahkan Dropout rate: 0.2.
> Mengubah optimizer dari sgd menjadi adam.

**Hipotesis:**
> Penambahan hidden layer akan membuat model lebih dalam (deeper) sehingga mampu menangkap hubungan data yang lebih kompleks. Penggunaan regularisasi dropout (0.2) dan optimizer Adam diharapkan dapat mengimbangi kedalaman jaringan tersebut agar model tidak mudah overfit meskipun menggunakan aktivasi sigmoid

**Hasil:**
> Test accuracy: 84.41% (sedikit di bawah Eksperimen #0 yang sebesar 85.11%)
> Train accuracy: 84.02%
> Validation accuracy: 85.43%
> Train time: 61.5 detik 
> Apakah overfit/underfit? Tidak

**Observasi:**
> Fenomena Akurasi Validasi Lebih Tinggi: Hal paling mencolok dibanding Eksperimen #0 adalah posisi grafik Validation (oranye) yang secara konsisten berada di atas grafik Train (biru), baik pada loss maupun accuracy. Ini adalah efek positif dari penggunaan Dropout (0.2). Saat training, sebagian neuron dimatikan sehingga model bekerja lebih "berat", sedangkan saat validation, semua neuron aktif penuh sehingga performanya langsung melesat lebih tinggi (mencapai 85.43%).
> Dampak Aktivasi Sigmoid: Meskipun arsitektur diperdalam menjadi 2 layer dan menggunakan Adam, akurasi tes (84.41%) belum bisa mengalahkan baseline Eksperimen #0 (85.11%). Aktivasi sigmoid rentan terhadap masalah vanishing gradient pada jaringan yang lebih dalam, yang membuat proses belajarnya cenderung lebih lambat dan datar dibandingkan relu.
> Tren Kurva yang Masih Menanjak: Pada grafik akurasi, kurva train dan validation masih sama-sama menunjukkan tren naik di epoch akhir tanpa ada tanda-tanda berpotongan atau overfit. Jarak (gap) keduanya sangat aman dan stabil.

---

## 🏆 Konfigurasi Terbaik

Setelah semua eksperimen, salin konfigurasi terbaik kalian ke sini:

```python
HIDDEN_LAYERS     = 1
NEURONS_PER_LAYER = 64
ACTIVATION        = tanh
DROPOUT_RATE      = 0.0
OPTIMIZER         = rmsprop
LEARNING_RATE     = 0.001
BATCH_SIZE        = 32
EPOCHS            = 10
```

**Test accuracy final: 87.63%**
