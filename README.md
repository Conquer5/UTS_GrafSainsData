# UTS Graf Sains Data: Sistem Rekomendasi RetailRocket

| | |
|---|---|
| **Nama** | Rafly Anugrah Syahputra |
| **NIM** | 23031554195 |
| **Mata kuliah** | Graf Sains Data (2023A) |
| **Tugas** | Ujian Tengah Semester (UTS) |

Proyek ini memodelkan interaksi pengunjung dengan produk e-commerce sebagai graf di **Neo4j**, lalu memakai graf itu untuk merekomendasikan item dengan **cosine similarity** dan **KNN berbasis item**. 

## Dataset

[RetailRocket Recommender System Dataset](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset) (Roman Zykov). Notebook hanya memakai `events.csv`.

| Fold | Periode | Fungsi |
|---|---|---|
| P1 | 22 Juli – 5 Agustus 2015 | training (diimpor ke Neo4j) |
| P2 | 6 – 19 Agustus 2015 | testing |

Hasil penyaringan tersimpan di `data/events_filtered.csv` (581.848 baris).

## Isi notebook

1. Tugas pendahuluan dan koneksi Neo4j
2. Impor data P1 ke graf `Visitor` → `Item` (`VIEWED`, `ADDED_TO_CART`, `BOUGHT`)
3. Komposisi subgraph, VSM user dan item, serta proyeksi graf
4. Perbandingan proyeksi dan pemilihan proyeksi Item
5. Cosine similarity, lima item terdekat, dan relasi `SIMILAR_TO`
6. Rekomendasi KNN (k = 5) dan evaluasi Hit@5 serta NDCG@5

## Struktur folder

```
.
├── UTS_Graf_Rafly Anugrah Syahputra.ipynb   # notebook utama
├── data/events_filtered.csv                 # data hasil filter P1 + P2
├── archive/                                 # dataset mentah (tidak di-commit)
├── docker-compose.yml                       # Neo4j
└── requirements.txt
```

## Cara menjalankan

1. Unduh dataset dari Kaggle dan letakkan `events.csv` di `archive/`.
2. Pasang dependensi:
   ```bash
   python -m venv myvenv
   source myvenv/bin/activate
   pip install -r requirements.txt
   ```
3. Nyalakan Neo4j (membutuhkan Docker):
   ```bash
   docker compose up -d
   ```
4. Jalankan semua sel pada notebook. Impor ke Neo4j memakan waktu beberapa menit.

Koneksi bisa diatur lewat variabel lingkungan `NEO4J_URI`, `NEO4J_USER`, `NEO4J_PASSWORD`, dan `NEO4J_DATABASE`. Nilai bawaannya cocok dengan `docker-compose.yml`. Kata sandi bawaan `password123` hanya untuk pengembangan lokal.

## Hasil singkat

- Graf training berisi 183.787 visitor, 83.501 item, dan 333.738 relasi.
- Proyeksi Item dipilih karena memori CSR-nya lebih kecil, walaupun 48,7% itemnya terisolasi.
- Pada user contoh, Hit@5 dan NDCG@5 bernilai 0. Hasil ini belum bisa dipakai untuk menilai model karena baru satu user.
