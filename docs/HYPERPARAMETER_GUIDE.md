# Hyperparameter Cheat Sheet

Panduan singkat — bukan jawaban pasti. Deep learning sering *counter-intuitive*, jadi tetap eksperimen!

## Jumlah Hidden Layer

| Nilai | Efek umum |
|---|---|
| 1 | Cukup untuk data sederhana, cepat training |
| 2–3 | Sweet spot untuk banyak masalah MLP |
| ≥4 | Butuh hati-hati: gradient vanishing, butuh lebih banyak data |

## Jumlah Neuron per Layer

- Terlalu sedikit (<16): **underfitting** — model tidak punya kapasitas
- Terlalu banyak (>1024): **overfitting** + lambat + boros memori
- Rule of thumb: mulai dari nilai antara input_size dan output_size

## Fungsi Aktivasi

| Aktivasi | Karakteristik |
|---|---|
| `relu` | Default modern. Cepat, sederhana. |
| `tanh` | Output di [-1, 1]. Lebih lambat dari ReLU. |
| `sigmoid` | Output di [0, 1]. Hindari di hidden layer (gradient vanishing). |
| `elu` / `selu` | Variant ReLU yang lebih halus, kadang lebih akurat. |

## Optimizer

| Optimizer | Kapan dipakai |
|---|---|
| `sgd` | Klasik. Butuh tuning learning rate manual. |
| `adam` | Default modern. Adaptif, biasanya konvergen cepat. |
| `adamax` | Variant Adam, lebih stabil pada beberapa kasus. |
| `rmsprop` | Bagus untuk RNN, tetap kompetitif di MLP. |

## Learning Rate (λ)

Ini hyperparameter **paling sensitif**. Lihat rumus dari kuliah:

$$W_j = W_j - \lambda \frac{\partial F(W_j)}{\partial W_j}$$

| Nilai | Efek |
|---|---|
| 1.0 | Terlalu besar → loss meledak (NaN) |
| 0.1 | Besar — bisa skip optimum |
| 0.01 | Default SGD |
| 0.001 | Default Adam — biasanya aman |
| 0.0001 | Lambat — butuh banyak epoch |

**Tips:** kalau loss naik bukan turun, learning rate kalian terlalu besar.

## Batch Size

Trade-off yang ada di slide kuliah:

| Batch Size | Plus | Minus |
|---|---|---|
| Kecil (16–32) | Lebih stabil, sering konvergen lebih baik | Lambat |
| Besar (128+) | Cepat per-epoch, hemat waktu | Butuh memori besar, kadang akurasi turun |

## Epochs

- Terlalu sedikit → **underfitting** (model belum sempat belajar)
- Terlalu banyak → **overfitting** (model menghafal train set)
- Lihat kurva: berhenti saat **val_loss mulai naik** lagi

## Cara Membaca Kurva Training

| Pola yang terlihat | Diagnosis |
|---|---|
| Loss train & val turun bersama → flat | Sehat, mungkin masih bisa dilatih lebih lama |
| Loss train terus turun, val mulai naik | **Overfitting** — kurangi epoch atau tambah dropout |
| Loss train tidak turun banyak | **Underfitting** — tambah kapasitas model |
| Loss naik-turun liar / NaN | **Learning rate terlalu besar** |

## Strategi Eksperimen yang Efisien

1. **Pertahankan kontrol** — ubah satu hyperparameter pada satu waktu
2. **Mulai dari hyperparameter paling sensitif:** learning rate → optimizer → arsitektur → epoch
3. **Catat semua** — hasil yang tidak diharapkan justru paling informatif!
4. **Gunakan kurva**, bukan hanya angka akhir
