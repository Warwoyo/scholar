# Scholarship Tracker

Scholarship Tracker adalah aplikasi web sederhana untuk mencatat dan memantau peluang beasiswa dalam satu tabel. Anda dapat menyimpan nama program, negara, pendanaan, universitas, tenggat, persyaratan, tautan sumber, dan perkembangan pendaftaran, lalu menyesuaikan kolom sesuai kebutuhan.

Aplikasi berjalan di browser menggunakan HTML, CSS, dan JavaScript tanpa framework, server aplikasi, akun, atau database eksternal. Data disimpan sebagai berkas JSON yang Anda pilih atau unduh. Antarmuka aplikasi saat ini menggunakan bahasa Inggris.

## Fitur

- Menambah, mengubah, dan menghapus catatan beasiswa.
- Mencari teks pada seluruh kolom teks, termasuk kolom yang disembunyikan.
- Memfilter berdasarkan negara, pendanaan, status pendaftaran, dan tenggat; mengurutkan tabel melalui judul kolom.
- Mengubah nilai langsung dari tabel dengan klik dua kali pada sel yang mendukungnya.
- Menambah, mengubah, menyembunyikan, mengurutkan ulang, dan menghapus kolom.
- Memvalidasi isi database dan nilai kolom, serta mendeteksi perubahan berkas dari luar aplikasi sebelum penyimpanan langsung.
- Membuka database JSON, menyimpan perubahan, dan mengunduh salinan cadangan.

## Struktur proyek

```text
.
├── index.html         # Antarmuka, gaya, dan seluruh logika aplikasi
├── scholarships.json  # Contoh database kosong dengan kolom bawaan
└── README.md
```

## Persyaratan dan menjalankan aplikasi

Gunakan browser modern yang mendukung JavaScript dan elemen dialog HTML. Untuk pengalaman penyimpanan langsung, gunakan browser yang mendukung File System Access API, misalnya Chrome atau Edge pada desktop. Dukungan dapat berbeda menurut browser dan lingkungan; jika pemilih berkas untuk penyimpanan langsung tidak tersedia, aplikasi menggunakan mode impor/ekspor.

1. Unduh atau kloning repositori ini, lalu masuk ke direktori proyek.
2. Jalankan server HTTP lokal, misalnya jika Python 3 sudah terpasang:

   ```bash
   python3 -m http.server 8000
   ```

3. Buka `http://localhost:8000` di browser.
4. Pilih **Open** untuk membuka `scholarships.json`, atau **Create** untuk membuat database baru.

Tidak ada dependensi yang perlu dipasang atau proses build. Anda juga dapat mencoba membuka `index.html` langsung, tetapi ketersediaan akses berkas bergantung pada browser dan konteks keamanannya. `localhost` dianjurkan agar fitur browser bekerja dengan lebih konsisten. Hentikan server dengan `Ctrl+C` di terminal.

## Cara menggunakan

### Membuka atau membuat database

- **Open** membuka database JSON yang sudah ada. Pada browser yang mendukung akses langsung, pilih berkas dan berikan izin akses saat diminta. Jika tidak didukung, aplikasi akan menggunakan pemilih berkas biasa.
- **Import JSON** membaca berkas melalui pemilih biasa dan menggunakan mode impor/ekspor, termasuk pada browser yang mendukung akses langsung.
- **Create** membuat database kosong dengan kolom bawaan. Jika pemilih penyimpanan tersedia, pilih lokasi berkas untuk menyimpannya; jika tidak, database baru berada di memori sampai Anda mengunduh salinannya.

Berkas contoh `scholarships.json` berisi struktur awal tanpa catatan. Data tidak otomatis dibuka saat halaman dimuat; pilih berkas setiap kali memulai sesi.

### Mengelola beasiswa

1. Setelah database dibuka atau dibuat, klik **+ Add Scholarship**.
2. Isi **Name** yang wajib diisi dan kolom lain yang diperlukan. Tanggal menggunakan format `YYYY-MM-DD`; tautan **Source** harus menggunakan HTTP atau HTTPS.
3. Klik **Save Changes** untuk memasukkan perubahan ke database di memori, kemudian klik **Save** untuk menyimpan atau mengekspor berkas.
4. Gunakan **Edit** pada baris untuk mengubah seluruh isinya, atau **Delete** untuk menghapusnya. Penghapusan memerlukan konfirmasi.

Klik dua kali sel yang mendukung pengeditan untuk mengubah nilainya langsung. Tekan `Enter` atau pindahkan fokus untuk menerapkan perubahan; `Escape` membatalkannya. Kolom teks panjang dan URL diedit melalui tombol **Edit**.

Kolom bawaan meliputi **Name**, **Country**, **Funding**, **Major**, **University**, **Provider**, **Deadline**, **Date Selection**, **Apply?**, **Requirements**, **Source**, dan **Notes**. Status **Apply?** tersedia sebagai `Belum`, `Pending`, atau `Sudah`, dengan `Belum` sebagai nilai awal.

### Pencarian, filter, dan tampilan

Masukkan kata kunci pada **Search scholarships**. Gunakan filter **Country**, **Funding**, **Apply?**, atau **Deadline by** untuk mempersempit hasil; filter tenggat mencakup tanggal yang sama atau lebih awal dari tanggal yang dipilih. Filter bawaan hanya muncul jika kolom yang sesuai masih ada dan memiliki tipe yang sesuai. Klik judul kolom untuk mengurutkan, dan klik lagi untuk membalik urutan. **Clear filters** menghapus pencarian dan filter.

Klik **Columns** untuk mengatur kolom:

- Centang atau hapus centang untuk menampilkan atau menyembunyikan kolom; nilai pada kolom tersembunyi tetap ada.
- Gunakan tombol panah untuk mengubah urutan, atau **Add Column** untuk menambahkan kolom.
- Tipe yang tersedia adalah text, long text, number, date, URL, select, dan checkbox. Kolom dapat diberi nilai awal dan ditandai wajib; tipe select memerlukan daftar pilihan, satu per baris.
- **Edit** mengubah definisi kolom. Aplikasi memvalidasi nilai seluruh catatan dan menolak perubahan yang tidak dapat diterapkan; jika pilihan select yang digunakan dihapus, tentukan penggantinya saat diminta.
- **Delete** menghapus kolom beserta nilainya dari semua catatan. Kolom `name` tidak dapat dihapus dan harus tetap wajib.

Perubahan kolom juga harus disimpan ke berkas melalui **Save**.

### Menyimpan dan mencadangkan data

Perubahan awalnya berada di memori browser. Tombol **Save Changes** dalam formulir belum menulis berkas. Perhatikan indikator di bagian atas:

| Status | Arti |
| --- | --- |
| **No Database** | Belum ada database yang dibuka. |
| **Unsaved Changes** | Ada perubahan di memori yang belum disimpan langsung. |
| **Saving…** | Penyimpanan berlangsung. |
| **Saved** | Tidak ada perubahan sejak database dibuka atau terakhir disimpan langsung. |
| **Save Failed** | Penyimpanan gagal; perubahan masih berada di memori. |

- **Save** menulis ke berkas yang dibuka atau dibuat dengan akses langsung. Pintasan `Ctrl+S` atau `Cmd+S` juga dapat digunakan.
- Dalam mode impor/ekspor, **Save** mengunduh salinan JSON; unduhan tidak menimpa berkas asli secara otomatis. Pindahkan atau ganti berkas secara manual jika diperlukan. Status perubahan belum tersimpan dapat tetap muncul setelah unduhan.
- **Save As** memilih berkas tujuan baru pada browser yang mendukungnya. Jika tidak tersedia, tombol ini mengunduh salinan JSON.
- **Export Backup** mengunduh salinan JSON kapan saja dan tidak menandai perubahan di memori sebagai tersimpan. Simpan cadangan di lokasi terpisah.

Jika berkas diubah oleh aplikasi lain setelah dibuka, penyimpanan langsung menampilkan pilihan untuk membatalkan, mengekspor salinan, memuat ulang, atau menimpa berkas. Memuat ulang membuang perubahan di memori setelah konfirmasi; periksa versi yang ingin dipertahankan sebelum menimpa. Browser juga dapat memperingatkan Anda ketika halaman ditutup dengan perubahan yang belum tersimpan.

Data diproses di browser dan aplikasi tidak memiliki fitur sinkronisasi atau pengiriman data ke server. Berkas JSON dan cadangannya dapat berisi informasi pribadi; kelola akses dan tempat penyimpanannya sesuai kebutuhan.

## Format database

Database menggunakan JSON dengan `formatVersion` bernilai `1`:

- `metadata`: nama database serta waktu pembuatan dan pembaruan dalam format UTC.
- `columns`: definisi kolom, termasuk ID, label, tipe, kewajiban, dan pilihan atau nilai awal jika ada.
- `view.visibleColumnIds`: ID kolom yang ditampilkan.
- `records`: daftar catatan dengan ID, waktu pembuatan dan pembaruan, serta objek `values` yang berisi nilai untuk setiap kolom.

Gunakan `scholarships.json` sebagai contoh struktur lengkap. Saat membuka berkas, aplikasi memvalidasi versi, metadata, kolom, tampilan, dan nilai seluruh catatan. JSON sembarang atau database dengan versi lain tidak otomatis dimigrasikan. Jika mengedit JSON secara manual, buat cadangan terlebih dahulu dan pastikan setiap catatan memiliki nilai untuk setiap kolom sesuai tipenya.

## Pemecahan masalah

- **Berkas tidak dapat dibuka:** pastikan JSON valid dan mengikuti format versi 1. Pesan kesalahan aplikasi menunjukkan bagian yang perlu diperiksa.
- **Perubahan tidak masuk ke berkas asli:** pastikan Anda menggunakan akses langsung dan memberikan izin menulis. Dalam mode impor/ekspor, gunakan berkas hasil unduhan untuk mengganti berkas lama secara manual.
- **Penyimpanan gagal:** periksa izin berkas, coba **Save** lagi, atau gunakan **Export Backup** untuk menyelamatkan salinan perubahan.
- **Tidak menemukan catatan:** periksa kata kunci dan filter, lalu gunakan **Clear filters**.
- **Perubahan kolom ditolak:** periksa nilai yang disebutkan dalam pesan validasi, tipe kolom, pilihan select, serta nilai awal untuk kolom wajib.

## Kontribusi dan lisensi

Kode sumber tersedia di repositori ini. Kontribusi dapat dilakukan dengan melaporkan masalah atau mengusulkan perubahan melalui mekanisme yang tersedia pada tempat repositori dihosting. Jelaskan perubahan dan cara pengujiannya agar mudah ditinjau.

Repositori ini belum menyertakan berkas lisensi. Pemilik proyek perlu memilih dan menambahkan lisensi open source sebelum orang lain dapat mengandalkan izin penggunaan, modifikasi, dan distribusi yang dinyatakan secara jelas.
