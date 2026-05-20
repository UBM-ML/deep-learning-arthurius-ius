# Refleksi Tim

> Jawaban dalam Bahasa Indonesia. Maksimal 1 halaman (~500 kata). Yang dinilai adalah **kedalaman pemahaman**, bukan panjang tulisan.

---

## 1. Parameter vs Hyperparameter

**Jawaban:**

> **Parameter** adalah nilai-nilai yang dipelajari secara otomatis oleh model selama proses training — dalam kasus kami, ini adalah **bobot (weights)** dan **bias** di setiap layer jaringan saraf. Nilai-nilai ini awalnya diinisialisasi secara acak, lalu terus diperbarui oleh algoritma backpropagation di setiap iterasi.
>
> **Hyperparameter** adalah nilai yang **kami tentukan sendiri sebelum training dimulai** dan tidak berubah selama proses berjalan. Contohnya: `learning rate`, `batch size`, `jumlah epochs`, `jumlah unit per layer`, dan `jumlah hidden layer`.
>
> Singkatnya: parameter berubah saat training berjalan (dioptimasi oleh model), sedangkan hyperparameter adalah "pengaturan mesin" yang kami pilih manual sebelum menekan tombol train.

---

## 2. Hyperparameter dengan Dampak Terbesar

**Jawaban:**

> Menurut pengamatan kami, **learning rate** memiliki dampak paling besar terhadap akurasi dan stabilitas training. Ketika learning rate terlalu besar, kurva loss melonjak tidak stabil bahkan divergen. Ketika terlalu kecil, loss turun sangat lambat dan model butuh epoch jauh lebih banyak untuk konvergen.
>
> Selain itu, **jumlah hidden layer dan unit** juga sangat berpengaruh — menambah kapasitas model membantu menangkap pola yang lebih kompleks pada Fashion-MNIST, yang terlihat dari kurva training accuracy yang lebih tinggi. Namun jika terlalu besar tanpa regularisasi, validation accuracy mulai tertinggal (overfitting).

---

## 3. Learning Rate

**Jawaban:**

> Ketika kami set `LEARNING_RATE = 1.0`, kurva loss **tidak turun secara konsisten** — malah naik-turun drastis dan tidak stabil. Pada beberapa eksperimen, loss bahkan menjadi `NaN` (divergen).
>
> Ini sesuai dengan rumus:
> $$W_j = W_j - \lambda \frac{\partial F(W_j)}{\partial W_j}$$
> Jika $\lambda$ (learning rate) terlalu besar, setiap pembaruan bobot "melompat" terlalu jauh melampaui titik minimum loss. Model tidak pernah berhasil "mendarat" di lembah optimal — ia terus berosilasi atau bahkan bergerak menjauhi solusi. Ibarat mencari titik terendah lembah tapi melangkah terlalu besar sehingga malah loncat ke bukit seberang.

---

## 4. Batch Size & Trade-off

**Jawaban:**

> | Aspek | Batch Size Kecil (16) | Batch Size Besar (256) |
> |---|---|---|
> | Waktu training | Lebih lambat per epoch | Lebih cepat per epoch |
> | Stabilitas loss | Fluktuatif / noisy | Lebih smooth dan stabil |
> | Akurasi akhir | Sedikit lebih tinggi | Sedikit lebih rendah / comparable |
>
> Pengamatan ini **sesuai dengan teori**: batch kecil menghasilkan gradient yang lebih "noisy" (karena dihitung dari sedikit sampel), tapi noise ini justru bisa membantu model keluar dari local minima. Batch besar memberikan estimasi gradient yang lebih akurat tapi berisiko terjebak di local minima yang tajam dan membutuhkan lebih banyak memori GPU.

---

## 5. Feed Forward & Back Propagation

**Jawaban:**

> Pada salah satu eksperimen kami dengan **60.000 sampel training**, **batch size = 32**, dan **epochs = 20**:
>
> $$\text{Jumlah backprop} = \frac{60.000}{32} \times 20 = 1.875 \times 20 = \textbf{37.500 kali}$$
>
> Setiap iterasi backpropagation, **semua bobot dan bias diperbarui** menggunakan gradien dari loss. Di iterasi pertama, bobot masih hampir acak dan loss sangat tinggi. Di iterasi terakhir, bobot sudah "terpahat" untuk memetakan fitur piksel ke label kategori pakaian dengan jauh lebih presisi — loss rendah dan pola seperti tekstur, tepi, dan bentuk sudah terekam dalam parameter model.

---

## 6. Kapan Deep Learning Tepat Digunakan?

**Jawaban:**

> Fashion-MNIST **bisa** diselesaikan dengan machine learning klasik — Logistic Regression dan Random Forest pun mampu mencapai akurasi ~85-88%. Namun deep learning (CNN khususnya) mencapai >92% karena mampu belajar **fitur hierarkis** secara otomatis: tepi → tekstur → bentuk → objek, tanpa feature engineering manual.
>
> Argumen kami: untuk dataset gambar berskala besar dan tugas klasifikasi visual yang kompleks, deep learning lebih tepat karena skalabilitas dan kemampuannya mengekstrak representasi otomatis. Untuk dataset kecil atau fitur tabular, ML klasik sering lebih efisien dan lebih mudah diinterpretasi.

---

## 7. Refleksi Tim

**Jawaban:**

> **Tantangan terbesar:** Memilih kombinasi hyperparameter yang tepat tanpa tahu "titik mulai" yang baik — trial and error membutuhkan waktu dan kadang hasilnya tidak konsisten antar run karena inisialisasi acak.
>
> **Pelajaran terpenting:** Tidak ada satu hyperparameter pun yang bisa dioptimasi secara isolasi — semuanya saling berinteraksi. Learning rate yang bagus untuk batch size 32 bisa gagal total di batch size 256.
>
> **Jika ada waktu lebih:** Kami ingin mencoba **learning rate scheduler** (menurunkan learning rate secara bertahap) dan **dropout** untuk melihat apakah overfitting bisa dikurangi secara signifikan, serta membandingkan performa MLP vs CNN pada dataset yang sama.
