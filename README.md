## Ringkasan

Proyek ini adalah tugas praktikum mandiri dengan **bidang AI Machine Learning (klasifikasi teks)** dan studi kasus **deteksi spam email** pada layanan email kampus (Sistem dan Teknologi Informasi). Sistem membaca teks email berbahasa Indonesia, lalu menentukan apakah email itu **spam** atau **ham** (email sah).

## Deskripsi Masalah

Layanan email kampus menerima banyak pesan setiap hari, termasuk spam dan phishing (misalnya undian palsu, pinjaman online, atau permintaan verifikasi kata sandi). Akibatnya:

- pengguna berisiko tertipu dan kehilangan data pribadi;
- email penting (jadwal ujian, tagihan UKT) mudah terlewat;
- tim layanan TI kewalahan menulis dan memperbarui aturan penyaring secara manual.

## Pendekatan

1. **Pembersihan teks:** huruf kecil, tautan diganti token `tautan`, angka diganti token `angka`, simbol dihapus.
2. **Ekstraksi fitur:** TF-IDF dengan unigram dan bigram.
3. **Model yang dibandingkan:** Multinomial Naive Bayes, Logistic Regression, dan Linear SVM.
4. **Evaluasi:** accuracy, precision, recall, F1, confusion matrix, dan 5-fold cross-validation.
5. **Analisis tambahan:** kata penciri tiap kelas, analisis kesalahan, pengaruh ambang batas, dan kurva belajar.

## Dataset

Dataset yang dipakai adalah **dataset sintetis buatan sendiri** (dibuat oleh `src/buat_dataset.py`) agar tidak memakai email pribadi.

| Keterangan | Nilai |
|---|---|
| Jumlah email | 3.000 (spam 1.074, ham 1.926) |
| Data latih / uji | 2.400 / 600 (pembagian 80% / 20%) |
| Jumlah fitur TF-IDF | 1.144 |
| Kasus sulit | Email sah yang memuat kata seperti "gratis", "diskon", "verifikasi", "segera" |
| Noise label | 2% label sengaja dibalik untuk meniru kesalahan pelabelan |

## Struktur Repositori

```
deteksi-spam-email/
├── README.md
├── data/
│   └── emails_sintetis.csv           # dataset hasil buat_dataset.py
├── notebook/
│   └── deteksi_spam_email.ipynb      # demo utama (Google Colab)
├── src/
│   ├── buat_dataset.py               # pembuat dataset sintetis
│   ├── deteksi_spam.py               # pelatihan dan evaluasi model
│   └── eksperimen_tambahan.py        # ambang batas dan kurva belajar
├── laporan/
│   ├── Laporan_Deteksi_Spam_Email.docx
│   ├── Laporan_Deteksi_Spam_Email.pdf
│   └── gambar/                       # grafik yang dipakai laporan
└── slide/
    └── Slide_Deteksi_Spam_Email.pptx
```

## Cara Menjalankan

### Opsi 1: Google Colab (paling mudah)

1. Klik tombol **Buka di Colab** di bagian atas, atau unggah `notebook/deteksi_spam_email.ipynb` ke [colab.research.google.com](https://colab.research.google.com).
2. Pilih **Runtime > Run all**.

Tidak perlu mengunggah dataset atau menginstal pustaka apa pun, karena sel pertama notebook membuat dataset secara otomatis.

### Opsi 2: Komputer lokal

Membutuhkan Python 3.9 atau lebih baru.

```bash
git clone https://github.com/USERNAME/deteksi-spam-email.git
cd deteksi-spam-email
pip install pandas scikit-learn matplotlib
python src/buat_dataset.py
python src/deteksi_spam.py
```

Perintah di atas dijalankan dari folder utama repositori. `deteksi_spam.py` menyimpan gambar hasil ke folder `laporan/gambar/`.

## Hasil

Evaluasi pada 600 email uji dengan model Naive Bayes:

| Metrik | Nilai |
|---|---|
| Accuracy | 98,83% |
| Precision | 97,71% |
| Recall | 99,07% |
| F1-score | 98,38% |
| F1 cross-validation (5-fold) | 96,87% ± 0,74% |

Confusion matrix: TN 380, FP 5, FN 2, TP 213.

Ketiga model menghasilkan nilai yang sama pada data uji ini. Ketujuh email yang salah diklasifikasikan ternyata berasal dari label yang sengaja dibalik (noise label), bukan kelemahan model.

> **Catatan penting:** dataset bersifat sintetis dan dibuat dari templat, sehingga angka di atas **tidak mewakili kinerja pada email nyata**. Email nyata jauh lebih beragam (bahasa campuran, ejaan tidak baku, upaya pengelabuan oleh pengirim spam). Untuk hasil yang lebih realistis, notebook menyediakan sel opsional yang memakai dataset publik *SMS Spam Collection*.

## Manfaat, Keterbatasan, dan Risiko

- **Manfaat:** kotak masuk lebih bersih, risiko phishing berkurang, dan beban helpdesk TI menurun.
- **Keterbatasan:** selalu ada kesalahan klasifikasi, hasil bergantung pada kualitas data dan label, hanya menganalisis teks, dan perlu dilatih ulang secara berkala.
- **Risiko etika dan privasi:** sistem membaca isi email pribadi (perlu mengacu pada UU No. 27 Tahun 2022 tentang Pelindungan Data Pribadi), ada risiko *false positive* (email sah terkarantina), bias terhadap gaya bahasa tertentu, dan kurangnya transparansi. Mitigasinya berupa karantina (bukan penghapusan), pengawasan manusia, pembatasan tujuan pemrosesan, dan audit berkala.

## Teknologi

Python, pandas, NumPy, scikit-learn, matplotlib, Google Colab.

## Luaran Tugas

- Laporan: `laporan/Laporan_Deteksi_Spam_Email.pdf` (versi Word tersedia di folder yang sama)
- Slide presentasi: `slide/Slide_Deteksi_Spam_Email.pptx`
- Notebook demo: `notebook/deteksi_spam_email.ipynb`
