# Guardian of Archipelago : Indonesia Geography

**Guardian of Archipelago** adalah permainan kuis interaktif berbasis *Command Line Interface* (CLI) yang ditulis menggunakan bahasa C. Permainan ini dirancang untuk menguji dan meningkatkan wawasan pemain mengenai geografi Indonesia, termasuk pengetahuan tentang ibu kota provinsi, pulau, danau, hingga budaya daerah.

## 📝 Deskripsi Proyek

Program ini mensimulasikan permainan cerdas cermat dengan sistem poin, nyawa (Health Points), dan tingkatan ranking. Pemain akan ditantang dengan berbagai pertanyaan seputar Indonesia. Semakin banyak jawaban benar, semakin tinggi ranking yang akan dicapai. Pemain juga harus memanajemen poin mereka untuk membeli bantuan (hint) atau nyawa tambahan agar tidak *Game Over*.

## ✨ Fitur Utama

* **Dua Tingkat Kesulitan**:
    * **Mudah**: Fokus pada pengetahuan dasar seperti Ibu Kota Provinsi.
    * **Sulit**: Mencakup pengetahuan umum yang lebih luas (gunung, danau, fauna endemik, dll).
* **Sistem Akun & Save Data**: Kemajuan permainan (poin, rank, nyawa) tersimpan secara otomatis. Pemain dapat melanjutkan permainan dengan login menggunakan username yang sama.
* **Sistem Ranking**: Terdapat 5 tingkatan rank berdasarkan jumlah jawaban benar:
    1. Bronze
    2. Platinum
    3. Champs
    4. Premaster
    5. Master
* **Toko Penukaran (Redeem Store)**: Poin yang dikumpulkan dapat ditukarkan dengan:
    * Tambahan Hint (Petunjuk).
    * Tambahan Nyawa (Health Point).
    * Double Poin untuk pertanyaan selanjutnya.
* **Leaderboard**: Menampilkan papan peringkat pemain berdasarkan jumlah jawaban benar tertinggi.
* **Mekanisme Permainan**:
    * Pertanyaan acak (Randomized).
    * Toleransi jawaban (case-insensitive).
    * Sistem Hint untuk membantu menjawab soal sulit.

## 🚀 Cara Menjalankan Program

Pastikan Anda telah menginstal compiler C (seperti GCC) di komputer Anda.

1.  **Clone atau Download** repository ini.
2.  Buka terminal atau command prompt di direktori folder proyek.
3.  **Kompilasi** kode sumber `ProyekKelompok6.c`:
    ```bash
    gcc ProyekKelompok6.c -o game
    ```
4.  **Jalankan** program:
    * **Windows**:
        ```bash
        game.exe
        ```
    * **Linux/Mac**:
        ```bash
        ./game
        ```
5.  Masukkan username Anda dan selamat bermain!

## 👥 Tim Pengembang

Proyek ini dikembangkan oleh:

* **Fawwaz Ajjihad** (L0125099)
* **Syarif Ahmad Hidayat** (L0125115)
* **Rangga Budi Satria** (L0125143)
