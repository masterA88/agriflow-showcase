Bahasa: **Bahasa Indonesia** · [English](./README.en.md)

<p align="center"><img src="assets/logo-mark.png" alt="Logo AgriFlow" width="220"/></p>

<h1 align="center">AgriFlow</h1>

<p align="center"><strong>Platform ketahanan pangan Jawa Timur: mencocokkan daerah surplus dengan daerah defisit.</strong></p>

<p align="center">
  <a href="https://www.agriflow.farm/"><img src="https://img.shields.io/badge/Coba%20sekarang-agriflow.farm-1B5E20?style=for-the-badge" alt="agriflow.farm"/></a>
  <img src="https://img.shields.io/badge/PIDI%20DIGDAYA-Hackathon%202026-4CAF50?style=for-the-badge" alt="PIDI DIGDAYA Hackathon 2026"/>
</p>

---

## 🌐 Website: [agriflow.farm](https://www.agriflow.farm/)

AgriFlow dapat diakses di **[www.agriflow.farm](https://www.agriflow.farm/)**. Buka halamannya, klik **"Lihat Dashboard sebagai Tamu"**, dan Anda langsung bisa melihat peta, rekomendasi distribusi, prakiraan harga, dan simulasi what-if tanpa membuat akun. Akun dinas dan mitra masuk lewat tombol **"Masuk untuk dinas & mitra"**.

<p align="center"><img src="assets/landing.png" alt="Halaman depan agriflow.farm" width="100%"/></p>

> Repositori ini hanya berisi penjelasan dan tangkapan layar. Kode sumber AgriFlow disimpan di repositori privat dan tetap dikembangkan di sana.

---

## Masalahnya

Di satu kabupaten panen melimpah dan harga jatuh, sementara di kabupaten sebelah pasokan tipis dan harga naik. Jumlah pangannya sering cukup; yang salah adalah arah distribusinya. Selama ini mencocokkan kedua daerah itu dikerjakan manual dan lambat, sehingga pemerintah daerah baru tahu ada masalah setelah harga bergerak.

## Yang dikerjakan AgriFlow

AgriFlow menghitung neraca pangan tiap kabupaten/kota (produksi dikurangi konsumsi), lalu memasangkan daerah surplus dengan daerah defisit. Cakupannya **38 kabupaten/kota di Jawa Timur** dan **enam komoditas**: beras premium, beras medium, cabai merah, cabai rawit, bawang merah, dan bawang putih.

Tiga fungsinya:

| Fungsi | Isinya |
|---|---|
| **Deteksi** | Menandai lonjakan dan penurunan harga yang tidak wajar dari harga harian, setelah pola musiman dikeluarkan |
| **Prediksi** | Prakiraan harga 30 hari ke depan per kabupaten/kota, lengkap dengan rentang ketidakpastian |
| **Distribusi** | Rekomendasi pengiriman dari daerah surplus ke daerah defisit, dengan bobot yang mendahulukan daerah tertinggal |

---

## Cara kerjanya

```
   SUMBER DATA RESMI                   MESIN AGRIFLOW                     AKSES
  ┌───────────────────┐     ┌──────────────────────────────────┐
  │ BPS 2022          │     │ 1. Neraca pangan per kab/kota     │
  │ produksi, konsumsi│────▶│ 2. Deteksi anomali harga          │──┬──▶ Dashboard web
  │ Siskaperbapo+PIHPS│     │ 3. Prakiraan harga 30 hari        │  │    (agriflow.farm)
  │ harga harian      │     │ 4. Pencocokan surplus ke defisit  │  │
  │ IPM 2024          │     │    (4 lapis, alokasi LP optimal)  │  └──▶ Bot WhatsApp
  │ jarak jalan (OSRM)│     └──────────────────────────────────┘       (Indonesia & Jawa)
  └───────────────────┘
```

**1. Data resmi, bukan data karangan.** Produksi dan konsumsi berasal dari BPS 2022. Harga harian 38 kabupaten/kota diambil dari Siskaperbapo Jawa Timur, dengan PIHPS sebagai cadangan. IPM 2024 dipakai sebagai dasar bobot keadilan. Jarak dihitung dari rute jalan sungguhan, bukan garis lurus. Kalau sumber tidak memuat sebuah angka, dashboard menampilkan kosong, bukan tebakan.

**2. Neraca pangan.** Untuk tiap komoditas dan tiap kabupaten/kota, mesin menghitung apakah daerah itu surplus atau defisit, dan berapa tonnya.

**3. Deteksi anomali.** Harga harian dibersihkan dari pola musimannya (misalnya siklus menjelang Lebaran), lalu diperiksa dengan metode Hampel/MAD yang tahan terhadap pencilan. Hasilnya dua hal: peringatan di dashboard, dan saringan agar data harga yang janggal tidak ikut memengaruhi rekomendasi distribusi.

**4. Prakiraan harga.** Harga 30 hari ke depan diperkirakan dengan model deret waktu TimesFM 2.0, disertai pita P10 sampai P90. Sebagai cadangan tersedia model musiman sederhana yang sudah diuji ulang pada data historis (MAPE 10,8%).

**5. Pencocokan empat lapis.** Setiap pasangan surplus-defisit melewati empat tahap:

| Lapis | Pertanyaan yang dijawab |
|---|---|
| Batasan keras | Apakah rutenya bisa ditempuh sebelum komoditas rusak? Apakah jaraknya masuk akal? |
| Penilaian | Seberapa baik pasangan ini dilihat dari jarak, volume, selisih harga, masa simpan, dan iklim rute? |
| Keadilan | Apakah tujuannya daerah tertinggal (IPM rendah) yang perlu didahulukan? |
| Alokasi | Berapa ton yang dikirim ke mana, dihitung sebagai program linear sehingga hasilnya optimal secara keseluruhan, bukan asal pasangan terdekat |

**6. Bisa dijelaskan.** Setiap rekomendasi punya kartu **"Mengapa match ini"** yang memperlihatkan asal, tujuan, jarak, skor tiap dimensi, dan alasan pemilihannya. Tidak ada kotak hitam.

---

## Tampilan

**Beranda dashboard.** Ringkasan surplus, defisit, kebutuhan yang tertutup, dan potensi nilai, ditambah peta surplus (hijau) dan defisit (merah) serta rekomendasi teratas.

<p align="center"><img src="assets/dashboard.png" alt="Beranda dashboard AgriFlow" width="100%"/></p>

**Rekomendasi distribusi.** Semua pasangan yang disarankan mesin, lengkap dengan rincian skor per dimensi. Daftarnya bisa diunduh sebagai CSV.

<p align="center"><img src="assets/rekomendasi.png" alt="Rekomendasi distribusi" width="100%"/></p>

**Harga & prakiraan.** Harga 90 hari terakhir, prakiraan 30 hari dengan pita ketidakpastian, dan daftar anomali harga untuk kabupaten/kota yang dipilih.

<p align="center"><img src="assets/forecast.png" alt="Harga dan prakiraan" width="100%"/></p>

**Simulasi what-if.** Jalankan ulang mesin dengan skenario seperti erupsi Semeru, banjir, penutupan Jembatan Suramadu, H-14 Idul Fitri, atau kenaikan BBM, lalu bandingkan hasilnya dengan kondisi hari ini.

<p align="center"><img src="assets/simulasi.png" alt="Simulasi what-if" width="100%"/></p>

**Bot WhatsApp.** Petani bisa menanyakan harga, mencari pembeli atau pemasok, dan melihat prakiraan lewat chat, dalam Bahasa Indonesia maupun Bahasa Jawa.

| Bahasa Indonesia | Bahasa Jawa |
|:---:|:---:|
| <img src="assets/whatsapp-id.png" alt="Bot WhatsApp Bahasa Indonesia" width="100%"/> | <img src="assets/whatsapp-jawa.png" alt="Bot WhatsApp Bahasa Jawa" width="100%"/> |

**Tampilan ponsel.** Halaman depan menyesuaikan layar ponsel.

<p align="center"><img src="assets/mobile-landing.png" alt="agriflow.farm di ponsel" width="300"/></p>

---

## Status saat ini

- Berjalan di produksi di [agriflow.farm](https://www.agriflow.farm/), dengan akses tamu untuk peninjauan.
- Mesin versi 1.1.0, dengan 702 uji otomatis yang lulus.
- Sudah dicoba oleh lima penguji awal (petani dan satu peneliti), dan kebutuhannya divalidasi lewat wawancara dengan empat petani lintas komoditas.

## Batasan yang kami akui

- Cakupannya baru Jawa Timur. Perluasan ke daerah lain bergantung pada ketersediaan data publik per kabupaten.
- Data produksi dan konsumsi memakai tahun 2022, tahun terlengkap di semua sumber.
- Prakiraan TimesFM 2.0 belum diuji ulang (backtest) pada data ini; angka akurasi yang sudah terukur berasal dari model cadangan.
- AgriFlow mempertemukan penjual dan pembeli, tetapi belum memfasilitasi transaksi.

---

## Tim

| Nama | Peran | LinkedIn |
|---|---|---|
| Chelsea | Data Analyst | [chelseaayu](https://linkedin.com/in/chelseaayu) |
| Hilmi | Data Architect | [hilmi888](https://linkedin.com/in/hilmi888/) |
| Monika | UX Researcher | [monika-hermiani](https://linkedin.com/in/monika-hermiani) |
| Irpan | Data Engineer | [irpanpilihanrambe](https://linkedin.com/in/irpanpilihanrambe) |

Dibangun untuk PIDI DIGDAYA x Hackathon 2026 Bank Indonesia.

## Kontak

Hilmi · [master-hilmi.vercel.app](https://master-hilmi.vercel.app/)

Ingin bekerja sama atau menguji coba AgriFlow di daerah Anda? Hubungi kami lewat tautan di atas.

<p align="center"><em>Deteksi · Prediksi · Distribusi, untuk ketahanan pangan Indonesia.</em></p>
