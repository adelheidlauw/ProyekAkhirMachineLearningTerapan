# Laporan Proyek Machine Learning - Adelheid Chantal Lauw

## Project Overview

Sistem rekomendasi merupakan tulang punggung platform digital modern, membantu pengguna menavigasi lautan konten dan produk. Di era di mana pilihan tidak terbatas, seperti film di layanan streaming, pengguna sering kali kesulitan menemukan apa yang benar-benar sesuai dengan selera mereka. Proyek ini bertujuan untuk mengatasi masalah "overload informasi" ini dengan membangun sistem rekomendasi film yang efektif, membantu pengguna menemukan film baru yang mungkin mereka sukai berdasarkan preferensi sebelumnya.

Dalam pengembangan sistem rekomendasi, salah satu pendekatan yang telah terbukti efektid adalah *Collaborative Filtering* yang metodenya berbasis item (*Item-Based Collaborative Filtering*). Menurut studi seminal yang dilakukan oleh Sarwar et al. (2001) dalam paper ["Item-Based Collaborative Filtering Recommendation Algorithms"](https://d1wqtxts1xzle7.cloudfront.net/106844907/p519-libre.pdf?1698016290=&response-content-disposition=inline%3B+filename%3DItem_based_collaborative_filtering_recom.pdf&Expires=1748863792&Signature=Udl3ODQZ3mJle1DCTTb1ns-XWy4HG6wY06Hw~2MblBB05FgO6DfwnYo-MsCyUxr4iWvopIcCG1ZpiOrwQWKKLk2aYRaGoW~8-tcmSy2F2~xKtPw283PCr-xsUcg7tIYqgiPMErhXEF17Uli5~ZbS84lk0uXrdcnD8sAiFKxtOKOUJLVYRE6rJmYFzBTuxO4wZ6YLWPbM8Fd0gR3BNbgENECweAxROxJM6t2g6R1RsCT~77QYA3kqHtsMjr1cQOUmZGSbmFQ7acKGHIPxqfAYlYv42JfuRjwX7GeYqGrG4b7KpsGa3wohVM2CDQeqEQc0OtznfxUiCVbU8FDFKTZsHw__&Key-Pair-Id=APKAJLOHF5GGSLRBV4ZA), pendekatan ini menawarkan solusi skalabel dan efisien untuk mengatasi tantangan sparsity (data jarang) dan skalabilitas yang sering ditemui pada metode berbasis pengguna. Paper tersebut secara rinci menjelaskan bagaimana kemiripan antar item dihitung—seringkali menggunakan metrik seperti cosine similarity—berdasarkan pola rating yang diberikan oleh banyak pengguna, memungkinkan sistem untuk merekomendasikan item yang serupa dengan apa yang disukai pengguna di masa lalu.

Penulis memilih Item-Based Collaborative Filtering karena dataset yang digunakan kaya akan data interaksi pengguna (rating film). Pendekatan ini sangat efektif dalam menemukan item-item yang "tersembunyi" namun relevan, dan juga mampu memberikan rekomendasi yang lebih beragam daripada Content-based Filtering karena tidak terbatas pada metadata item yang tersedia.

Kelebihan dari Item-Based Recommendation:
1. Stabilitas dan skalabilitas.
2. Rekomndasi yang lebih jelas.
3. Efisiensi rekomendasi *real-time*.
Kekurangan dari Item-Based Recommendation:
1. *Cold-Start Problem* untuk item baru. 
   Jika ada film atau lagu baru yang belum memiliki rating banyak dari penggunanya, sistem akan kesulitan untuk menghitung kemiripan dengan film atau lagu lainnya.
2. Keterbatasan pada item yang tidak biasa
   Jika ada item-item yang sangat jarang dinilai atau hanya disukai oleh segelintir *user* memungkin tidak memiliki cukup data untuk menghitung kemiripan yang benar.
3. Ketergantungan pada kualitas rating.
   Jika penilaian *user* tidak akurat atau bias, dapat mempengaruhi pada hasil sistem rekomendasi.
   
### Mengapa masalah rekomendasi perlu diselesaikan?
Adanya "overload informasi" dalam sebuah film atau musik, membuat pengguna merasa kebingungan untuk memilih film yang ingin ditonton atau musik yang akan didengar. Sebagai contoh nyata saat ini, Netflix memiliki ribuan film dan serial, Spotify memiliki jutaan lagu yang bisa didengar, dan E-Commerce memiliki sangat banyak produk yang dijual oleh para *seller*. Jika tidak ada sistem rekomendasi dapat membuat pengguna merasa frustasi karena terllau banyak pilihan yang bisa membuat pengguna kewalahan dan akhirnya malas untuk memilih. Dengan begitu terjaidnya penurunan data tarik pengguna untuk menggunakan aplikasi. Dari *sisi developer* /penjual/ *seller* pendapatan berkurang dan mungkin saja barang yang dijual ini tersembunyi karena tertumpuk dengan pilihan yang sangat banyak. 

## Referensi:
- Sarwar, Badrul, George Karypis, Joseph Konstan, and John Riedl. "Item-based collaborative filtering recommendation algorithms." In Proceedings of the 10th international conference on World Wide Web, pp. 285-295. 2001.

## Business Understanding

Seiring dengan pesatnya pertumbuhan platform streaming film dan koleksi digital, pengguna dihadapkan pada volume konten yang sangat besar, seringkali mencapai ribuan hingga jutaan judul. Fenomena ini, yang dikenal sebagai information overload, menimbulkan beberapa tantangan signifikan bagi pengguna maupun penyedia layanan:

### Problem Statements
Permasalahan yang ditemui sebagai berikut:
- Kesulitan dalam penemuan konten.
  *User* sering kali merasa kewalahan dengan banyaknya pilihan film yang tersedia, sehingga mereka kesulitan untuk memutuskan film apa yang akan ditonton. Hal ini dapat berujung pada waktu pencarian yang lama, frustasi, hingga tidak jadi menonton. 
- Rendahnya tingkat engagement pengguna.
  Tanpa panduan yang efektif, pengguna mungkin melewatkan film-film yang sebenarnya sangat relevan atau menarik bagi mereka. Akibatnya, interaksi *user* dengan platform (seperti durasi tonton atau frekuensi kunjungan) dapat menurun.
- Potensi kehilangan pendapatan.
  Bagi penyedia layanan, rendahnya engagement pengguna dapat berdampak langsung pada retensi pelanggan dan potensi monetisasi. *User* yang tidak menemukan nilai atau konten yang relevan cenderung kurang loyal terhadap platform.

### Goals

Tujuan proyek dibuat memiliki beberapa tujuan utama:
- Mempermudah penemuan film yang relevan.
  Untuk membantu *user* menemukan film yang sesuai dengan preferensi secara efisien, mengurangi waktu pencarian, dan meningkatkan kepuasan *user*
- Meningkatkan engagement dan retensi *user*.
  Bagi penyedia layanan, rendahnya engagement pengguna dapat berdampak langsung pada retensi pelanggan dan potensi monetisasi. *User* yang tidak menemukan nilai atau konten yang relevan cenderung kurang loyal terhadap platform.
- Memaksimalkan eksposur konten.
  Sistem ini bertujuan untuk meningkatkan visibilitas film-film yang mungkin cocok dengan selera pengguna namun sulit ditemukan di antara koleksi yang masif.

    ### Solution statements
    - Pendekatan Utama: Item-Based Collaborative Filtering:
      Pendekatan ini akan menjadi inti dari sistem rekomendasi yang dibangun. Ide dasarnya adalah merekomendasikan film kepada seorang pengguna berdasarkan film-film lain yang serupa dengan film-film yang telah disukai pengguna tersebut di masa lalu. Kesamaan antar film dihitung berdasarkan pola rating yang diberikan oleh seluruh pengguna (misalnya, jika banyak pengguna yang suka Film A dan Film B, maka Film A dan Film B dianggap mirip). Metode ini efektif dalam menemukan rekomendasi yang "tidak terduga" dan tidak bergantung pada metadata item. Algoritma yan digunakan adalah Cosine Similiraty untuk menghitung kesamaan antar vektor rating film. 
    - Pendekatan Alternatif: Model-Based Collaborative Filtering (Matrix Factorization seperti SVD):
      Sebagai pengembangan di masa depan, pendekatan berbasis model Matrix Factorization dapat dipertimbangkan. Pendekatan ini bekerja dengan menguraikan matriks interaksi pengguna-item menjadi faktor-faktor laten tersembunyi yang merepresentasikan karakteristik pengguna dan item.

## Data Understanding
The Movie Dataset adalah dataset yang memiliki 45.000 filmdaru . Ukurannya yang ringkas (100.000 rating) memungkinkan pemrosesan yang cepat dan efisien, sehingga ideal untuk demonstrasi dan pembelajaran. Struktur datanya juga relatif bersih dan mudah dipahami, meminimalkan kebutuhan pra-pemrosesan yang kompleks. Format judul film yang konsisten juga membantu menghindari masalah pencarian judul yang sering muncul pada dataset yang lebih besar. Dataset ini terdiri dari 100.000 rating dari 943 pengguna untuk 1682 film. Setiap pengguna telah menilai setidaknya 20 film. Data rating diberikan pada skala 1-5 bintang.
File utama yang akan kita gunakan: 
- `u.data`: Berisi rating (user ID, item ID, rating, timestamp).
- `u.item`: Berisi informasi film (item ID, judul film, tanggal rilis, genre, dll.). Kita hanya akan menggunakan item ID dan judul film.

Selanjutnya, uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:  

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- accepts : merupakan jenis pembayaran yang diterima pada restoran tertentu.
- cuisine : merupakan jenis masakan yang disajikan pada restoran.
- dst

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data beserta insight atau exploratory data analysis.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
