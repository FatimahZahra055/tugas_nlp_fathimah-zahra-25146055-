# Analisis Sentimen Ulasan Produk E-Commerce (Bahasa Indonesia)

Repositori ini memuat implementasi prapemrosesan teks (*text preprocessing*) dan eksplorasi distribusi label sentimen pada dataset ulasan produk *e-commerce* berbahasa Indonesia menggunakan Python.

- **Penyusun:** Fathimah Zahra (`25146055`)
- **Repositori GitHub:** [tugas_nlp_fathimah-zahra-25146055-.git](https://github.com/FatimahZahra055/tugas_nlp_fathimah-zahra-25146055-.git)
- **Sumber Dataset:** [dipawidia/ecommerce-product-reviews-sentiment](https://huggingface.co/datasets/dipawidia/ecommerce-product-reviews-sentiment) (Hugging Face, split `train`)

---

## Struktur Repositori

| Nama Berkas | Deskripsi |
| :--- | :--- |
| `ecommerce_sentiment_preprocessing.ipynb` | Jupyter Notebook berisi kode pemuatan dataset, eksplorasi distribusi label, fungsi prapemrosesan teks, serta evaluasi panjang ulasan. |
| `distribusi_sentimen.png` | Grafik batang perbandingan frekuensi ulasan berlabel sentimen negatif (`0`) dan positif (`1`). |
| `Laporan_Preprocessing_Ulasan_Ecommerce.pdf` | Laporan tertulis yang merangkum metodologi, sampel perbandingan teks, dan evaluasi dampak prapemrosesan. |
| `README.md` | Dokumentasi utama proyek dan ringkasan hasil eksperimen. |

---

## Informasi & Distribusi Dataset

Dataset dimuat menggunakan pustaka Hugging Face `datasets` (`load_dataset`) pada partisi `train` dengan karakteristik sebagai berikut:

| Parameter | Keterangan |
| :--- | :--- |
| **Total Data** | 11.606 ulasan produk *e-commerce* |
| **Atribut / Kolom** | `review` (teks ulasan asli), `translate` (terjemahan), `sentimen` (label biner) |
| **Sentimen Negatif (`0`)** | 5.803 ulasan (50,0%) |
| **Sentimen Positif (`1`)** | 5.803 ulasan (50,0%) |
| **Keseimbangan Kelas** | Seimbang (*balanced dataset*), sehingga tidak diperlukan teknik *oversampling* maupun *undersampling*. |

---

## Tahapan Prapemrosesan Teks

Seluruh ulasan pada kolom `review` diproses secara berurutan melalui lima tahapan utama:

1. **Case Folding** — Mengubah seluruh karakter huruf menjadi huruf kecil (*lowercase*) agar perbedaan kapitalisasi tidak menghasilkan duplikasi fitur.
2. **Cleaning** — Menghapus tautan URL (`http`/`https`/`www`), `@mention`, `#hashtag`, angka, tanda baca, dan karakter non-alfabetis, serta merapikan spasi berlebih.
3. **Tokenisasi** — Memecah string teks bersih menjadi daftar satuan kata (*token*) alfabetis menggunakan pola ekspresi reguler `[a-z]+`.
4. **Stopword Removal** — Menyaring kata umum berbahasa Indonesia yang tidak membawa muatan sentimen menggunakan daftar *stopword* dari pustaka `NLTK`.
5. **Stemming** — Mengubah setiap kata berimbuhan menjadi bentuk kata dasarnya (*root word*) menggunakan pustaka `Sastrawi` (contoh: `membeli` $\rightarrow$ `beli`, `kualitasnya` $\rightarrow$ `kualitas`).

---

## Hasil Akhir: Perbandingan Sebelum & Sesudah Prapemrosesan

Berikut adalah 10 contoh sampel pertama (indeks `0`–`9`) dari dataset sebelum dan sesudah melewati seluruh tahapan prapemrosesan:

| Indeks | Label | Sebelum Prapemrosesan | Setelah Prapemrosesan |
| :---: | :---: | :--- | :--- |
| **[0]** | `0` (Negatif) | Suara dan mic puas. Hanya saya pesan yg nyala dan sudah sy pastikan lg klik yg nyala tp yg datang ga nyala | suara mic puas pesan yg nyala sy pasti lg klik yg nyala tp yg ga nyala |
| **[1]** | `1` (Positif) | Good. Mudah-mudahan awet. Makasih | good mudah mudah awet makasih |
| **[2]** | `0` (Negatif) | kondisi dus robek2, tolong di perhatikan lagi | kondisi dus robek tolong perhati |
| **[3]** | `0` (Negatif) | Kualitas produk baik. Harga terjangkau dan pengirimannya cepet banget. Udh 3x order disini oke lah tp sayangnya klo buat kado gabisa nitip pake kartu ucapan atau dibungkus pake kertas kado :( | kualitas produk harga jangkau kirim cepet banget udh order oke tp sayang klo kado gabisa nitip pake kartu ucap bungkus pake kertas kado |
| **[4]** | `0` (Negatif) | Overall bagus, tombol berfungsi semuanya, tapi packaging kurang karena sudah penyot dan robek sedikit | overall bagus tombol fungsi packaging penyot robek |
| **[5]** | `1` (Positif) | Produk ini original, Dan kualitasnya sangat baik. kecepatan pengiriman sanagt cepat, recommended seller. | produk original kualitas cepat kirim sanagt cepat recommended seller |
| **[6]** | `0` (Negatif) | kualitas sangat jelek. bocor | kualitas jelek bocor |
| **[7]** | `1` (Positif) | terimakasih banyak ka ya, barang sudah diterima dengan baik. sukses terus ka | terimakasih ka ya barang terima sukses ka |
| **[8]** | `0` (Negatif) | Body nya lebih berat dari cm 1579, spiral nya lebih tipis dibanding cm 1579, semoga awet, beli CM 1659 penasaran Aja, yang cm 1579 udah mau rusak mesin nya, Harga lebih murah 4k dari cm 1579 Fast delivery, courier Ramah sangat, ty seller | body nya berat cm spiral nya tipis banding cm moga awet beli cm penasaran aja cm udah rusak mesin nya harga murah cm fast delivery courier ramah ty seller |
| **[9]** | `0` (Negatif) | pesanan di terima tanggal 27 January 2021 tetapi baru bisa saya buka pagi ini tanggal 29 January 2021 pukul 06.00 dan ternyata tab SKETSA ADVAN tidak bisa di nyalakan sama sekali dan di cas juga posisi Tab tidak bisa menyala | pesan terima tanggal january buka pagi tanggal january tab sketsa advan nyala cas posisi tab nyala |

---

## Pengaruh Prapemrosesan terhadap Kualitas Data

- **Reduksi Derau & Dimensi Fitur:** Penghapusan URL, angka, dan tanda baca mencegah model mempelajari token spesifik yang tidak relevan serta memperkecil ukuran kosakata unik (*vocabulary size*).
- **Standardisasi Bentuk Kata:** *Case folding* dan *stemming* menyatukan variasi kata berimbuhan ke bentuk dasar yang konsisten sehingga menekan redundansi fitur.
- **Peningkatan Rasio Sinyal Sentimen:** *Stopword removal* mengeliminasi kata tugas yang netral sehingga kata bermuatan polaritas menjadi lebih dominan.
- **Efisiensi Panjang Dokumen:** Rata-rata jumlah kata per ulasan berkurang dari **15,8 kata** menjadi **11,2 kata** (penyusutan sebesar **29,1%**), membuat komputasi pemodelan lebih ringan tanpa kehilangan inti informasi ulasan.

---

## Cara Menjalankan Notebook

1. Kloning repositori ini ke komputer lokal:
   ```bash
   git clone https://github.com/FatimahZahra055/tugas_nlp_fathimah-zahra-25146055-.git
   cd tugas_nlp_fathimah-zahra-25146055-
   ```
2. Pasang pustaka Python yang diperlukan:
   ```bash
   pip install datasets pandas matplotlib nltk PySastrawi jupyter
   ```
3. Buka dan jalankan seluruh sel pada berkas `ecommerce_sentiment_preprocessing.ipynb`.

---

## Identitas Penyusun

- **Nama:** Fathimah Zahra
- **NIM:** 25146055