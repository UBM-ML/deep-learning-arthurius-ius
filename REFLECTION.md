# Refleksi Tim

> Jawaban dalam Bahasa Indonesia. Maksimal 1 halaman (~500 kata). Yang dinilai adalah **kedalaman pemahaman**, bukan panjang tulisan.

---

## 1. Parameter vs Hyperparameter

**Jawaban:**

> **Parameter** adalah nilai yang dipelajari model secara otomatis selama training — yaitu **bobot (weights)** dan **bias** di setiap layer. Nilai ini diinisialisasi acak di awal, lalu diperbarui ribuan kali oleh backpropagation berdasarkan gradien loss.
>
> **Hyperparameter** adalah nilai yang **kami tentukan sendiri sebelum training dimulai** dan tidak berubah selama proses berjalan. Dalam eksperimen kami, hyperparameter yang divariasikan antara lain: `learning rate` (0.01 / 0.001), `optimizer` (SGD / Adam / Adamax), `jumlah hidden layer` (1 / 2), `neurons per layer` (64 / 128), `fungsi aktivasi` (ReLU / ELU / Sigmoid), `dropout rate` (0.0 / 0.2), dan `jumlah epochs` (10 / 20).
>
> Singkatnya: parameter berubah *selama* training dioptimasi oleh model, sedangkan hyperparameter adalah "konfigurasi mesin" yang kami pilih *sebelum* training dimulai.

---

## 2. Hyperparameter dengan Dampak Terbesar

**Jawaban:**

> Berdasarkan eksperimen kami, **kombinasi arsitektur dan learning rate** memberikan dampak terbesar. Eksperimen #4 (2 hidden layer, 128 neuron, LR 0.001, Adam, 20 epoch, Dropout 0.2) menghasilkan akurasi tertinggi **88.33%** — naik signifikan dari baseline #0 yang hanya **85.11%**.
>
> Sebaliknya, Eksperimen #3 membuktikan dampak learning rate: hanya dengan menurunkan LR dari 0.01 → 0.001 tanpa perubahan lain, akurasi justru **turun menjadi 80.64%** karena model tidak punya cukup epoch untuk konvergen. Ini menunjukkan bahwa learning rate tidak bisa diisolasi — ia berinteraksi erat dengan jumlah epoch. Kurva loss pada #3 masih menurun di epoch akhir, menandakan model belum konvergen.

---

## 3. Learning Rate

**Jawaban:**

> Dari Eksperimen #3 (LR = 0.001, hanya 10 epoch), kurva loss turun sangat lambat dan stabil, tetapi belum mencapai titik minimum — akurasi tertahan di 80.64%. Ini adalah efek LR terlalu kecil untuk durasi training yang singkat.
>
> Mengacu pada rumus:
> $$W_j = W_j - \lambda \frac{\partial F(W_j)}{\partial W_j}$$
> Jika $\lambda$ terlalu kecil, setiap langkah pembaruan bobot sangat kecil sehingga model butuh jauh lebih banyak iterasi untuk konvergen. Sebaliknya, jika $\lambda$ terlalu besar (misalnya 1.0), model melompat terlalu jauh melewati titik minimum — loss akan melonjak tidak stabil, bahkan bisa divergen. Eksperimen #4 menemukan keseimbangan: LR 0.001 tetapi dikompensasi dengan epoch yang lebih banyak (20), menghasilkan kurva loss yang halus dan konvergensi yang lebih dalam.

---

## 4. Batch Size & Trade-off

**Jawaban:**

> Seluruh eksperimen kami menggunakan batch size 32, sehingga perbandingan langsung antar batch size tidak kami lakukan secara eksplisit. Namun dari observasi kurva loss di semua eksperimen, batch size 32 terbukti menghasilkan kurva yang cukup stabil.
>
> Berdasarkan teori:
> - **Batch size kecil (misal 16):** gradient dihitung dari sedikit sampel → lebih *noisy*, kurva loss berfluktuasi, tetapi noise ini bisa membantu keluar dari local minima.
> - **Batch size besar (misal 256):** gradient lebih akurat dan stabil, kurva loss lebih mulus, tetapi berisiko terjebak di local minima yang tajam dan butuh memori lebih besar.
>
> Pengamatan ini konsisten dengan teori di slide kuliah. Batch size 32 berada di titik tengah yang seimbang antara stabilitas dan kecepatan konvergensi.

---

## 5. Feed Forward & Back Propagation

**Jawaban:**

> Mengambil **Eksperimen #4** sebagai contoh (Fashion-MNIST: 60.000 sampel training, batch size = 32, epochs = 20):
>
> $$\text{Jumlah backprop} = \frac{60.000}{32} \times 20 = 1.875 \times 20 = \textbf{37.500 kali}$$
>
> Di **iterasi pertama**, bobot dan bias masih hampir acak — loss sangat tinggi dan prediksi model nyaris asal tebak. Di **iterasi terakhir** setelah 37.500 kali pembaruan, bobot sudah "terpahat" secara presisi: setiap neuron telah belajar merespons pola tertentu seperti tepi, tekstur, atau bentuk pakaian. Hasilnya, train accuracy mencapai 90.14% dan test accuracy 88.33% — jauh dari kondisi acak di awal.

---

## 6. Kapan Deep Learning Tepat Digunakan?

**Jawaban:**

> Dari eksperimen kami, bahkan arsitektur MLP sederhana (1 layer, 64 neuron) sudah mencapai 85.11% — angka yang juga bisa dicapai oleh model ML klasik seperti Logistic Regression atau SVM. Ini menunjukkan Fashion-MNIST **tidak mutlak membutuhkan** deep learning.
>
> Namun, peningkatan ke arsitektur lebih dalam (Eksperimen #4: 2 layer, 128 neuron) mengangkat akurasi ke 88.33% tanpa feature engineering manual — deep learning belajar representasi hierarkis (tepi → tekstur → bentuk) secara otomatis. Eksperimen #5 juga menunjukkan bahwa pilihan arsitektur yang salah (Sigmoid pada jaringan dalam) justru menurunkan performa akibat vanishing gradient.
>
> **Kesimpulan:** Untuk Fashion-MNIST, ML klasik bisa menjadi solusi yang lebih cepat dan ringan. Deep learning lebih tepat ketika data berdimensi tinggi, fiturnya implisit (sulit direkayasa manual), dan ada kebutuhan untuk skalabilitas ke dataset yang lebih besar dan kompleks.

---

## 7. Refleksi Tim

**Jawaban:**

> **Tantangan terbesar:** Menyadari bahwa hyperparameter tidak bisa dioptimasi secara terpisah. Eksperimen #3 mengajarkan kami bahwa menurunkan learning rate saja justru menurunkan akurasi — butuh kompensasi berupa epoch lebih banyak, seperti yang berhasil di Eksperimen #4. Mencari keseimbangan antar hyperparameter ini membutuhkan intuisi yang hanya datang dari banyak percobaan.
>
> **Pelajaran terpenting:** Observasi kurva loss dan accuracy jauh lebih informatif daripada sekadar melihat angka akurasi akhir. Dari kurva, kami bisa mendeteksi apakah model belum konvergen (loss masih turun di epoch akhir seperti #3), sudah overfitting (validation loss naik), atau sudah seimbang seperti #4. Eksperimen #5 juga mengajarkan bahwa fungsi aktivasi Sigmoid rentan *vanishing gradient* pada jaringan lebih dalam, terlihat dari kurva yang lebih datar dan akurasi yang tidak melampaui baseline ReLU.
>
> **Jika ada waktu lebih:** Kami ingin mencoba **learning rate scheduler** (menurunkan LR secara bertahap saat training berjalan), eksperimen dengan batch size berbeda (16 dan 256) untuk memvalidasi teori secara langsung, serta mencoba arsitektur **CNN** untuk melihat seberapa besar lompatan akurasi yang bisa dicapai dibanding MLP pada data gambar ini.
