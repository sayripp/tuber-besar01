# Dokumentasi Teknis: Guardian of Archipelago

Dokumen ini menjelaskan detail teknis dari implementasi program permainan kuis **Guardian of Archipelago**. Program ini dibangun menggunakan bahasa C dengan pendekatan prosedural, memanfaatkan struktur data untuk manajemen *state* pemain dan bank soal, serta manajemen file untuk persistensi data.

## 1. Arsitektur Program

Program ini dirancang sebagai aplikasi *Console Application* (CLI) yang berjalan secara sekuensial. Arsitektur utama terdiri dari beberapa komponen berikut:

* **Global State Management**: Menggunakan variabel global (`player`, `questions`) untuk menyimpan status permainan yang aktif agar dapat diakses oleh berbagai fungsi tanpa *passing parameter* yang berlebihan.
* **Dynamic Memory Allocation**: Menggunakan `malloc` dan `free` untuk memuat bank soal ke dalam memori sesuai dengan tingkat kesulitan yang dipilih (Mudah/Sulit), sehingga penggunaan memori lebih efisien.
* **File Persistence**: Sistem penyimpanan data berbasis teks (`savedata.txt`) untuk menyimpan progres pemain (nama, poin, nyawa, rank) agar dapat dimuat kembali (*load*) pada sesi berikutnya.
* **Modular Functions**: Pemisahan logika program ke dalam fungsi-fungsi spesifik (seperti logika *gameplay*, tampilan menu, dan manajemen file).

## 2. Struktur Data
Program mendefinisikan tiga `struct` utama untuk merepresentasikan entitas dalam permainan:

### A. Struct Player
Menyimpan informasi profil dan statistik pemain saat ini.
```c
typedef struct {
    char name[50];       // Nama pemain (username)
    int point;           // Poin saat ini
    int health_point;    // Sisa nyawa
    int hint;            // Sisa bantuan (hint)
    int rank;            // Tingkat ranking (0-4)
    int right_answers;   // Total jawaban benar (akumulatif)
} Player;
````

### B. Struct Question

Menyimpan detail pertanyaan, jawaban, dan status penggunaannya dalam satu sesi.

```c
typedef struct {
    char question[255];  // Teks pertanyaan
    char answer[50];     // Kunci jawaban
    char hints[100];     // Teks petunjuk
    int used_question;   // Flag (1 jika sudah muncul, 0 jika belum)
} Question;
```

### C. Struct Redeem

Menyimpan data item yang dapat ditukarkan di menu Redeem Store.

```c
typedef struct {
    char names[50];       // Nama item
    char description[100];// Deskripsi efek item
    int price;            // Harga dalam poin
} Redeem;
```

## 3\. Daftar Fungsi & Deskripsi

Berikut adalah rincian fungsi-fungsi yang membangun logika program:

### Fungsi Inisialisasi & Utilitas

| Nama Fungsi | Deskripsi |
| :--- | :--- |
| `main()` | *Entry point* program. Menginisialisasi *seed* random, memanggil login, dan loop menu utama. |
| `tampilkanSelamatDatang()` | Menampilkan *banner* ASCII art selamat datang. |
| `userRank()` | Mengembalikan string nama rank (Bronze - Master) berdasarkan integer rank pemain. |

### Fungsi Manajemen Data (File I/O)

| Nama Fungsi | Deskripsi |
| :--- | :--- |
| `playerLogin()` | Menangani input username dan memuat data dari `savedata.txt` jika user memilih *load game*. |
| `saveGame()` | Menyimpan data pemain saat ini ke `savedata.txt`. Menggunakan file *temporary* untuk teknik *rewrite* yang aman. |
| `showLeaderboard()` | Membaca `savedata.txt`, melakukan *sorting* data berdasarkan jawaban benar, dan menampilkan/menulis ke `leaderboard.txt`. |

### Fungsi Logika Permainan (Core Gameplay)

| Nama Fungsi | Deskripsi |
| :--- | :--- |
| `mainMenu()` | Menampilkan opsi navigasi utama (Mulai, Rank, Redeem, dll). |
| `levelChoice()` | Meminta user memilih tingkat kesulitan (Mudah/Sulit) dan memanggil inisialisasi soal yang sesuai. |
| `easyQuestion()` | Mengalokasikan memori dan mengisi array `questions` dengan soal-soal tingkat mudah. |
| `hardQuestion()` | Mengalokasikan memori dan mengisi array `questions` dengan soal-soal tingkat sulit. |
| `startGame()` | Loop utama kuis. Menangani pengacakan soal, input jawaban, validasi jawaban (case-insensitive), sistem hint, dan perhitungan skor/nyawa. |
| `menuIfPlayerDead()` | Menu darurat yang muncul jika HP pemain mencapai 0 (Game Over). |

### Fungsi Fitur Tambahan

| Nama Fungsi | Deskripsi |
| :--- | :--- |
| `updateRank()` | Memperbarui status integer `player.rank` berdasarkan jumlah `right_answers`. |
| `displayRank()` | Menampilkan informasi visual mengenai rank pemain saat ini. |
| `listRedeem()` | Menampilkan toko penukaran poin dan menangani logika pembelian item (Hint, HP, Double Point). |

## 4\. Alur Program (Program Flow)

1.  **Start Up**:

      * Program dimulai, *seed* waktu diinisialisasi untuk fungsi `rand()`.
      * User diminta memasukkan Username.
      * User memilih: Load Game lama atau New Game.

2.  **Main Menu Loop**:
    User disajikan menu utama dengan opsi:

      * **1. Mulai**: Masuk ke pemilihan level.
      * **2. Lihat Rank**: Cek status rank.
      * **3. Redeem**: Tukar poin.
      * **4. Leaderboard**: Lihat klasemen.
      * **5. Save Game**: Simpan progres manual.
      * **6. Keluar**: Terminasi program.

3.  **Gameplay Loop (Jika memilih Mulai)**:

      * **Pilih Level**: User memilih Mudah atau Sulit. Fungsi `easyQuestion` atau `hardQuestion` dipanggil untuk mengisi memori.
      * **Reset**: Status `used_question` direset.
      * **Loop Pertanyaan (10 Soal)**:
          * Sistem memilih indeks soal secara acak yang belum pernah digunakan.
          * Soal ditampilkan.
          * User input jawaban (atau ketik 'y' untuk hint).
          * **Validasi**: Input user dikonversi ke *lowercase* dan dicocokkan dengan jawaban benar.
          * **Benar**: Poin bertambah (cek status *double points*), `right_answers` bertambah.
          * **Salah**: Nyawa (`health_point`) berkurang.
          * **Cek Nyawa**: Jika nyawa \<= 0, permainan berhenti dan masuk ke `menuIfPlayerDead`.
      * **Selesai**: Jika 10 soal selesai, *autosave* dilakukan.

4.  **Save System**:

      * Sistem membaca file database.
      * Mencari apakah nama user sudah ada.
      * Jika ada, data lama ditimpa dengan data baru. Jika tidak, data baru ditambahkan (Append).

5.  **Exit**:

      * Memori untuk array `questions` dibebaskan (`free`).
      * Program selesai.

## 5\. File Eksternal

Program ini berinteraksi dengan file-file berikut dalam direktori kerja:

  * `../test/savedata.txt`: Database utama penyimpanan profil pemain. Format penyimpanan per blok user dipisahkan oleh separator.
  * `temp_savedata.txt`: File sementara yang dibuat saat proses penyimpanan data untuk menghindari korupsi data.
  * `leaderboard.txt`: File output yang dihasilkan saat user mengakses menu Leaderboard.

```
```
