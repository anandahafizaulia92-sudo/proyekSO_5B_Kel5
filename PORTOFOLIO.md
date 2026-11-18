# PORTOFOLIO PROYEK AKHIR SISTEM OPERASI

## 1. Identitas

| Informasi        | Keterangan                                                                  |
| ---------------- | --------------------------------------------------------------------------- |
| Nama Lengkap     | Ananda Hafiz Aulia                                                          |
| NIM              | 2510512203                                                                  |
| Kelas            | E                                                                           |
| Kelompok         | Kelompok 5                                                                  |
| Mata Kuliah      | Sistem Operasi                                                              |
| Judul Proyek     | Pilihan 5B — Sistem Antrean Pekerjaan dan Monitoring Proses Linux           |
| Nama Repository  | `proyek-so`                                                                 |
| Repository Utama | [GitHub - proyekSO_5B_Kel5](https://github.com/iiqbalitoo/proyekSO_5B_Kel5) |

## 2. Asal Proyek

Proyek ini merupakan bagian dari Project Akhir mata kuliah Sistem Operasi dengan pilihan topik 5B, yaitu **Sistem Antrean Pekerjaan dan Monitoring Proses Linux**.

Pengembangan dilakukan secara berkelompok dengan menggunakan Bash Shell Script pada lingkungan Linux Ubuntu melalui Windows Subsystem for Linux 2 (WSL2).

Proyek ini bertujuan untuk menerapkan konsep Sistem Operasi melalui pengelolaan antrean pekerjaan, pemrosesan tugas oleh worker, pengaturan status pekerjaan, penguncian proses, serta monitoring aktivitas sistem.

**Catatan kepemilikan proyek:**

Proyek `proyek-so` merupakan hasil kerja sama seluruh anggota kelompok, bukan karya yang sepenuhnya dikembangkan secara pribadi. Portofolio ini menjelaskan kontribusi, pekerjaan, pengujian, dan pemahaman saya terhadap bagian yang menjadi tanggung jawab pribadi.

### Anggota Kelompok

1. Muhammad Iqbal Rizqullah
2. Harsya Maulana Syahbani
3. Ananda Hafiz Aulia
4. Muhammad Fathir Natadiwangsa
5. Rifqi Naufal Anwar
6. Waldan Nur Rabbani

## 3. Kontribusi Pribadi

Dalam proyek ini, saya bertanggung jawab pada pengujian dan dokumentasi mekanisme penjadwalan antrean FIFO serta penguncian worker pada file `worker.sh`.

Rincian kontribusi yang saya kerjakan adalah:

1. **Pengujian penjadwalan FIFO**

   - Menguji proses pengambilan pekerjaan dari direktori `data/queue/`.
   - Memastikan pekerjaan diproses berdasarkan urutan antrean.
   - Memeriksa perpindahan file pekerjaan dari direktori `queue/` ke `running/`, kemudian ke `done/` atau `failed/` sesuai hasil eksekusi.

2. **Pengujian mekanisme Lock**

   - Menguji penggunaan `flock` pada `worker.sh`.
   - Memastikan hanya satu worker yang dapat memproses antrean pada satu waktu.
   - Menguji penolakan ketika worker kedua dijalankan saat lock masih aktif.

3. **Dokumentasi pengujian**

   - Menyusun bukti pengujian kondisi positif dan negatif.
   - Mendokumentasikan hasil eksekusi melalui file log dan tangkapan layar.
   - Menyimpan bukti pengujian pada direktori `evidence/`.

4. **Penyusunan laporan**

   - Berkontribusi dalam penyusunan Bab 2: Desain Sistem dan Spesifikasi bersama Waldan.
   - Menyusun Subbab 3.2: Penjadwalan FIFO (`worker.sh`).
   - Mengisi bagian Matriks Kontribusi sesuai pekerjaan yang dilakukan.

## 4. Bukti Kontribusi

Bukti kontribusi saya dapat diperiksa melalui repository utama kelompok dan dokumentasi pengujian.

### Repository dan Commit

- Repository utama: [https://github.com/iiqbalitoo/proyekSO_5B_Kel5](https://github.com/iiqbalitoo/proyekSO_5B_Kel5)
- Commit bukti pengujian Nanda: [https://github.com/iiqbalitoo/proyekSO_5B_Kel5/commit/c17018a](https://github.com/iiqbalitoo/proyekSO_5B_Kel5/commit/c17018a)

### Berkas Bukti Pengujian

| Berkas                       | Keterangan                                                              |
| ---------------------------- | ----------------------------------------------------------------------- |
| `evidence/nanda-positif.txt` | Bukti log pengujian antrean FIFO dan keberhasilan pemrosesan pekerjaan. |
| `evidence/nanda-negatif.txt` | Bukti penolakan worker kedua ketika worker pertama masih memegang lock. |

### Dokumentasi Laporan

Kontribusi dokumentasi dapat ditemukan pada:

- Bab 2: Desain Sistem dan Spesifikasi.
- Bab 3.2: Penjadwalan FIFO (`worker.sh`).

Bukti tersebut digunakan untuk menunjukkan proses pengujian, hasil yang diperoleh, dan dokumentasi pekerjaan yang menjadi tanggung jawab saya.

## 5. Skenario Pengujian Individu

### 5.1 Tujuan Pengujian

Pengujian individu dilakukan untuk membuktikan dua mekanisme utama pada `worker.sh`, yaitu:

1. Pekerjaan diproses secara berurutan berdasarkan prinsip FIFO (*First In, First Out*).
2. Worker kedua tidak dapat berjalan ketika worker pertama masih memegang lock.

Pengujian ini bertujuan mengurangi risiko pemrosesan ganda dan *race condition* akibat beberapa worker mengakses antrean secara bersamaan.

### 5.2 Kondisi Positif: Pengujian FIFO

**Tujuan:**

Memastikan pekerjaan yang masuk lebih dahulu akan diproses lebih dahulu oleh worker.

**Langkah pengujian:**

1. Membersihkan kondisi pengujian menggunakan script reset.

2. Mengajukan pekerjaan pertama menggunakan script pengajuan, misalnya `tests/ok.sh`.

3. Mengajukan pekerjaan kedua menggunakan `tests/tidur.sh 2`.

4. Menjalankan worker menggunakan perintah:

   ```bash
   bash scripts/worker.sh
   ```

5. Memeriksa hasil pemrosesan melalui log pada `output/jobs.log`.

6. Memastikan pekerjaan pertama masuk ke tahap `RUNNING` dan `SUCCESS` sebelum pekerjaan kedua diproses.

**Hasil pengujian:**

Berdasarkan hasil pengujian, pekerjaan diproses secara berurutan. Log menunjukkan bahwa pekerjaan pertama mencapai status `SUCCESS` sebelum pekerjaan kedua mulai diproses.

Dengan demikian, pengujian menunjukkan bahwa mekanisme pengambilan pekerjaan pada worker berjalan sesuai urutan FIFO berdasarkan penamaan file antrean.

### 5.3 Kondisi Negatif: Pengujian Lock

**Tujuan:**

Memastikan worker kedua ditolak ketika worker pertama masih aktif dan memegang lock.

**Langkah pengujian:**

1. Menjalankan worker pertama di latar belakang.

2. Memastikan worker pertama masih menjalankan proses pengolahan antrean.

3. Menjalankan perintah worker kedua ketika lock masih aktif:

   ```bash
   bash scripts/worker.sh
   ```

4. Memeriksa respons yang ditampilkan oleh terminal.

**Hasil pengujian:**

Worker kedua tidak dapat mengambil alih proses karena lock masih digunakan oleh worker pertama.

Pesan penolakan yang dihasilkan adalah:

```text
Ditolak: worker lain sedang berjalan
```

Hasil tersebut menunjukkan bahwa mekanisme lock berhasil mencegah dua worker menjalankan pemrosesan antrean secara bersamaan.

### 5.4 Verifikasi Hasil

Hasil pengujian diverifikasi melalui dua sumber bukti:

- File `evidence/nanda-positif.txt` yang berisi catatan urutan perubahan status pekerjaan.
- File `evidence/nanda-negatif.txt` yang berisi pesan penolakan worker kedua.

Selain itu, tangkapan layar pengujian digunakan sebagai dokumentasi pendukung dalam Subbab 3.2 laporan kelompok.

## 6. Analisis Konsep Sistem Operasi

### 6.1 Antrean Aplikasi FIFO dan Penjadwalan CPU

FIFO (*First In, First Out*) pada proyek ini merupakan mekanisme antrean di tingkat aplikasi. Pekerjaan yang masuk lebih dahulu akan diambil lebih dahulu oleh worker.

Mekanisme tersebut berbeda dengan penjadwalan CPU yang dilakukan oleh kernel Linux.

- **Antrean aplikasi FIFO:** Mengatur urutan pekerjaan yang akan diproses oleh worker.
- **Penjadwalan CPU:** Mengatur pembagian waktu penggunaan prosesor kepada proses atau thread yang sedang berjalan.

Dengan demikian, FIFO pada proyek ini tidak menggantikan scheduler CPU Linux. FIFO hanya menentukan urutan pengambilan pekerjaan, sedangkan kapan proses memperoleh waktu CPU tetap diatur oleh kernel.

### 6.2 Sinkronisasi Proses dan Critical Section

Penggunaan `flock` pada `worker.sh` merupakan bentuk sinkronisasi untuk mengatur akses terhadap sumber daya bersama.

Bagian yang perlu dilindungi adalah proses pengambilan dan pemindahan pekerjaan dari antrean, karena beberapa worker yang mengakses bagian tersebut secara bersamaan berpotensi mengambil pekerjaan yang sama.

Bagian tersebut dapat dipahami sebagai *critical section*, yaitu bagian program yang mengakses sumber daya bersama dan perlu dilindungi dari akses bersamaan yang tidak terkendali.

Pada proyek ini, lock digunakan agar hanya satu worker yang dapat memasuki proses pemrosesan antrean pada satu waktu.

### 6.3 Pencegahan Race Condition

*Race condition* dapat terjadi ketika beberapa proses mengakses atau mengubah sumber daya yang sama secara bersamaan sehingga hasilnya bergantung pada urutan eksekusi.

Contohnya, dua worker berpotensi membaca pekerjaan yang sama dari direktori antrean sebelum salah satunya memindahkan file tersebut.

Penggunaan `flock` membantu mencegah kondisi tersebut dengan membatasi worker yang dapat memproses antrean secara bersamaan.

Mekanisme ini membuat proses pengambilan dan pemindahan pekerjaan lebih terkendali.

### 6.4 Analisis Potensi Deadlock

*Deadlock* merupakan kondisi ketika beberapa proses saling menunggu sumber daya yang sedang dipegang oleh proses lain sehingga tidak dapat melanjutkan eksekusi.

Dalam implementasi proyek ini, penggunaan `flock -n` membuat worker mencoba memperoleh lock secara non-blocking.

Apabila lock sudah digunakan oleh worker lain, worker yang baru dijalankan langsung ditolak dan keluar dari program. Dengan demikian, worker kedua tidak menunggu lock dilepaskan.

Berdasarkan mekanisme tersebut, skenario pengujian yang dilakukan tidak menunjukkan terjadinya deadlock.

Namun, hal ini bukan berarti seluruh kemungkinan deadlock pada sistem sudah dapat dihilangkan. Jika sistem dikembangkan dengan beberapa lock atau sumber daya yang saling bergantung, potensi deadlock tetap perlu dianalisis lebih lanjut.

## 7. Kendala dan Penyelesaian

Selama proses pengerjaan dan pengujian bagian `worker.sh`, terdapat beberapa kendala teknis yang saya alami.

### 7.1 Pengujian Urutan FIFO

**Kendala:**

Pada tahap pengujian, diperlukan pembuktian bahwa pekerjaan benar-benar diproses berdasarkan urutan masuk, bukan hanya berdasarkan hasil akhir bahwa semua pekerjaan berhasil.

**Penyelesaian:**

Saya memeriksa perubahan status pekerjaan melalui `output/jobs.log`, mulai dari `WAITING`, `RUNNING`, hingga `SUCCESS`. Saya menggunakan lebih dari satu pekerjaan pengujian agar urutan pemrosesan dapat dibandingkan dengan jelas.

### 7.2 Pengujian Lock Antar-Worker

**Kendala:**

Pengujian lock harus dilakukan ketika worker pertama masih aktif. Jika worker pertama sudah selesai, lock akan dilepaskan sehingga worker kedua dapat berjalan. Kondisi tersebut tidak dapat membuktikan penolakan worker ganda.

**Penyelesaian:**

Saya menjalankan worker pertama di latar belakang, kemudian mencoba menjalankan worker kedua ketika lock masih aktif. Hasilnya, worker kedua menampilkan pesan:

```text
Ditolak: worker lain sedang berjalan
```

Pengujian ini membantu memastikan bahwa mekanisme lock bekerja sesuai tujuan.

### 7.3 Pengelolaan File Bukti Pengujian

**Kendala:**

Selama proses pengujian dan perbaikan, file bukti pada direktori `evidence/` sempat perlu dibuat ulang karena isi bukti sebelumnya berubah atau tertimpa.

**Penyelesaian:**

Saya menjalankan kembali pengujian, mengambil catatan log yang benar, kemudian menyimpan ulang hasilnya ke dalam `evidence/nanda-positif.txt` dan `evidence/nanda-negatif.txt`. Setelah itu, saya memeriksa kembali file sebelum melakukan commit.

### 7.4 Kendala Git Saat Melakukan Push

**Kendala:**

Ketika melakukan push ke repository utama, Git menolak pengiriman karena repository remote sudah memiliki perubahan yang belum terdapat pada repository lokal.

**Penyelesaian:**

Saya menyimpan sementara perubahan lokal menggunakan `git stash`, mengambil perubahan terbaru dari repository remote menggunakan `git pull --rebase origin main`, kemudian melakukan push kembali menggunakan:

```bash
git push origin main
```

Setelah push berhasil, perubahan lokal yang disimpan sementara dikembalikan menggunakan:

```bash
git stash pop
```

Proses ini membantu menyelaraskan perubahan lokal dengan repository utama tanpa langsung membuang pekerjaan yang sedang dilakukan.

### 7.5 Perbedaan Perintah WSL dan Ubuntu

**Kendala:**

Pada awal pemeriksaan lingkungan, saya menjalankan perintah `wsl --version` langsung dari terminal Ubuntu. Perintah tersebut tidak dikenali karena merupakan perintah pengelolaan WSL dari sisi Windows.

Selain itu, perintah `tree` tidak tersedia pada instalasi Ubuntu yang digunakan.

**Penyelesaian:**

Saya menjalankan `wsl --version` melalui Windows PowerShell, sedangkan informasi distribusi, kernel, dan Bash diperiksa dari terminal Ubuntu menggunakan:

```bash
cat /etc/os-release
uname -r
bash --version
```

Untuk memeriksa struktur direktori, saya menggunakan perintah `find` sebagai alternatif:

```bash
find . -maxdepth 2 -type d -not -path './.git*' | sort
```

Dari kendala ini, saya memahami perbedaan antara perintah pengelolaan WSL di Windows dan perintah Linux yang dijalankan di dalam distribusi Ubuntu.

## 8. Refleksi

### 8.1 Hal yang Sudah Dikuasai

Melalui pengerjaan proyek ini, saya memperoleh pemahaman dan pengalaman dalam beberapa hal:

1. Memahami konsep dasar antrean FIFO pada tingkat aplikasi.
2. Memahami cara kerja worker dalam mengambil dan memproses pekerjaan.
3. Menggunakan Bash Shell Script untuk menjalankan dan mengelola proses.
4. Memahami penggunaan `flock` untuk membatasi akses bersamaan terhadap sumber daya.
5. Memahami hubungan antara sinkronisasi proses, critical section, dan pencegahan race condition.
6. Membaca dan menganalisis log untuk memverifikasi hasil pengujian.
7. Menggunakan Ubuntu melalui WSL2 untuk pengembangan dan pengujian.
8. Menggunakan Git untuk menyimpan perubahan dan berkolaborasi melalui repository kelompok.

### 8.2 Hal yang Masih Perlu Dipelajari

Saya menyadari masih terdapat beberapa materi yang perlu dipelajari lebih lanjut, antara lain:

1. Memahami algoritma penjadwalan CPU Linux secara lebih mendalam.
2. Mempelajari mekanisme sinkronisasi proses lainnya, seperti semaphore dan mutex.
3. Memahami analisis deadlock secara lebih mendalam, termasuk kondisi-kondisi yang dapat menyebabkannya.
4. Meningkatkan kemampuan debugging Bash Script dan penanganan kesalahan proses.
5. Memahami pengelolaan proses Linux melalui PID, sinyal, dan monitoring sistem.
6. Meningkatkan pemahaman penggunaan Git, khususnya dalam menyelesaikan konflik dan mengelola perubahan bersama.

### 8.3 Kesimpulan Refleksi

Proyek ini memberikan pengalaman praktis dalam menerapkan beberapa konsep Sistem Operasi melalui Bash Shell Script dan lingkungan Linux. Saya tidak hanya mempelajari konsep FIFO dan lock secara teori, tetapi juga menguji bagaimana keduanya bekerja melalui skenario positif dan negatif.

Saya juga belajar bahwa keberhasilan sebuah program tidak cukup dibuktikan dengan program yang dapat berjalan. Diperlukan pengujian, pencatatan hasil, analisis, serta dokumentasi agar perilaku sistem dapat dijelaskan dan dipertanggungjawabkan.

Melalui kontribusi pada `worker.sh`, penyusunan laporan, dan pengujian individu, saya memperoleh pemahaman yang lebih baik mengenai antrean pekerjaan, sinkronisasi proses, dan penggunaan Git dalam pengembangan proyek secara berkelompok.

---

**Dokumen ini disusun sebagai dokumentasi kontribusi pribadi Ananda Hafiz Aulia dalam Project Akhir Sistem Operasi Pilihan 5B.**
