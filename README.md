# MNIST Learning Strategy: L1, L2, Dropout, dan Early Stopping

Tugas **Assignment 2** mata kuliah **Neural Network**, Magister Kecerdasan Artifisial, Universitas Gadjah Mada.

- **Nama:** Nabila Hermawan
- **NIM:** 	25/572261/PPA/07190

## Tentang proyek

Proyek ini melatih jaringan saraf tiruan sederhana untuk mengenali angka tulisan tangan 0 sampai 9 dari dataset **MNIST**. Model dasar (baseline) lalu dibandingkan dengan lima percobaan untuk mengurangi *overfitting* (model terlalu menghafal data latihan).

## Data

| Bagian | Jumlah gambar |
|---|---|
| Latihan (training) | 55.000 |
| Validasi | 5.000 |
| Test | 10.000 |

Setiap gambar berukuran 28 x 28 piksel (hitam-putih).

## Model

`Flatten` -> `Dense(64, relu)` -> `Dense(10, softmax)`, optimizer Adam, loss `categorical_crossentropy`, 10 epoch, batch size 100. Total parameter: 50.890.

## Percobaan

| Percobaan | Pengaturan |
|---|---|
| Baseline | Tanpa tambahan |
| 2a. L1 | rate = 0,0001 |
| 2b. L2 | rate = 0,001 |
| 2c. Dropout | rate = 0,3 |
| 2d. Early Stopping | monitor `val_loss`, patience = 3, maksimal 50 epoch |
| 2e. L1 + Dropout | L1 = 0,0001 dan Dropout = 0,3 |

## Hasil

| Percobaan | Epoch | Akurasi train | Akurasi validasi | Akurasi test | Selisih (train - val) |
|---|---|---|---|---|---|
| Baseline | 10 | 0,9851 | 0,9712 | 0,9738 | 0,0139 |
| L1 (0,0001) | 10 | 0,9753 | 0,9786 | 0,9719 | -0,0033 |
| L2 (0,001) | 10 | 0,9724 | 0,9756 | 0,9697 | -0,0032 |
| Dropout (0,3) | 10 | 0,9612 | 0,9750 | 0,9696 | -0,0138 |
| Early Stopping | 12 | 0,9903 | 0,9770 | 0,9722 | 0,0133 |
| L1 + Dropout | 10 | 0,9490 | 0,9732 | 0,9692 | -0,0242 |

Catatan: pada Early Stopping, angka train dan validasi diambil dari epoch 12, sedangkan akurasi test dihitung dari model epoch 9 (kondisi terbaik).

![Perbandingan akurasi](Results/perbandingan.png)

Grafik loss dan akurasi tiap percobaan ada di folder [`results`](results).

## Kesimpulan

Model dasar sudah baik dengan akurasi test 97,38% dan overfitting ringan. L1, L2, dan Dropout menghilangkan overfitting tersebut, sedangkan Early Stopping menghentikan latihan di epoch 12 sebelum overfitting bertambah. Namun, tidak ada percobaan yang mengalahkan baseline di data test dan selisihnya sangat kecil. Kombinasi L1 + Dropout memberi hasil paling rendah (96,92%) karena model terlalu dibatasi.

## Cara menjalankan

**Di Google Colab (disarankan):** buka notebook, lalu pilih *Runtime > Run all*.

**Di komputer sendiri:**

```bash
pip install -r requirements.txt
jupyter notebook Assignment2_MNIST_Learning_Strategy.ipynb
```

Sel paling akhir di notebook memakai `google.colab` untuk mengunduh gambar, sehingga hanya berjalan di Colab. Sel tersebut boleh dilewati jika dijalankan di komputer sendiri.

## Isi repository

```
.
├── README.md
├── requirements.txt
├── Assignment2_MNIST_Learning_Strategy.ipynb
└── results/
    ├── ringkasan_hasil.csv
    ├── perbandingan.png
    ├── baseline_curves.png
    ├── L1_1e-4_curves.png
    ├── L2_1e-3_curves.png
    ├── Dropout_0.3_curves.png
    ├── EarlyStopping_curves.png
    └── L1+Dropout_curves.png
```
