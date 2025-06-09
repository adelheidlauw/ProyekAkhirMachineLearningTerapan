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
- Sarwar, B., George K., Joseph K., and John R.. "Item-based collaborative filtering recommendation algorithms." In Proceedings of the 10th international conference on World Wide Web, pp. 285-295. 2001.

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

The Movie Dataset adalah dataset yang diambil dari Kaggle yang berisikan data-data film dari The Movie Database (TMDb) dan sistem rekomendasi MovieLens. Sumber dataset dapat diakses dan diunduh dari Kaggle dengan judul (The Movies Dataset)[https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset].
Dataset ini sangat cocok untuk membangun sistem rekomendasi karena menyediakan dua komponen utama. 
1. `movies_df` : Berisi metadata film ekstensif dari `movies_metadata.csv` berjumlah 45.000 film dari TMDb. Deskripsi fitur `movies_df` (24 kolom awal):
   - `adult` : `object` - Menunjukkan apakah film tersebut ditujukan untuk penonton dewasa (True/False). Beberapa entri mungkin tidak dalam format boolean yang benar.
   - `belongs_to_collection`: `object` - Informasi JSON tentang koleksi film (misalnya, bagian dari seri film).
   - `budget` : `object` - Anggaran produksi film dalam format string. Beberapa entri mungkin non-numerik.
   - `genres` : `object` - Informasi JSON tentang genre film (misalnya, Comedia, Drama).
   - `homepage` : `object` - URL situs web resmi film.
   - `id` : `object` - ID film atau kode unik untuk setiap film berbeda. Awalnya bertipe `object` karena ada beberapa entri non-numerik, perlu dikonversi ke numerik.
   - `imbd_id` : `object` - ID unik film di IMBDd.
   - `original_language` : `object` - Kode bahasa asli film (misalnya, en, fr).
   - `original_title` : `object` - Judul asli film sebelum translasi.
   - `overview` : `object` - Ringkasan singkat atau sinopsis film.
   - `popularity` : `object` - Skor popularitas film. Beberapa entri mungkin non-numerik.
   - `poster_path` : `object` - Path ke gambar poster film.
   - `production_companies` : `object` - Informasi JSON tentang perusaahn produksi film.
   - `production _countries` : `object` ` Informasi JSON tenatng negara produksi film.
   - `release_date`: `object`  - Tanggal rilis film. Beberapa entri mungkin tidak dalam format tanggal yang valid.
   - `revenue` : `float64` - Pendapatan box office film.
   - `runtime` : `float64` - Durasi film dalam menit.
   - `spoken_languages` : `object` - Informmasi JSON tentang bahasa yang digunakan dalam film.
   - `status` : `object` - Status produksi film (misalnya, *Released*, *Post Production*).
   - `tagline` : `object` - Slogan atau *tagline* film.
   - `title` : `object` - Judul film yang ditampilkan.
   - `video` : `object` - Menunjukkan apakah film memiliki video ttrailer (True/False).
   - `vote_average` : `float64` - Rata-rata *rating* pengguna.
   - `vote_count` : `float64` - Jumlah total voting pengguna.
2. `ratings_df` : Berisi `rating_small.csv` yang merupakan data rating *user* berjumlah 100.000 rating dari 700 *user* dari 9.000 film. Variabel yang digunakan sebagai berikut:
  - `userID` : `int64` - ID *user* atau kode unik *user*.
  - `movieID` : `int64` - Kode unik untuk film, dan setiap film memiliki kode unik yang berbeda.
  - `rating` : `float64` - Penilaian dari *user* terhadap film.
  - `timestamp` : `int64` - Waktu (dalam format Unix epoch time) ketika rating diberikan. Informasi ini bisa berguna untuk analisis tren atau memabngun model berbasis waktu, namun tidak digunakan langsung dalam model *Collaborative Filtering* saat ini.

### Tahapan Eksplorasi Data (Exploratory Data Analysis - EDA)
- `info()` : Untuk menampilkan ringkasan informasi tentang dataset.
- `head()` : Untuk menampilkan 5 baris pertama dari dataset.
- `describe()` : Untuk menghasilkan statistika deskriptif untuk kolom-kolom numerik.
- `isnull().sum()` : Membuat DataFrame baru dengan nilai True di mana ada nilai kosong dan False di tempat lain dan juga menghitung jumlah nilai yang hilang. 
- `to_numeric()` : Mengubah kolom menjadi tipe data numerik.
- `errors='coerce'` : Jika kolom yang dipilih tidak bisa diubah menjadi angka, maka nilai tersebut diubah menjadi NaN.
- `dropna()` : Menghapus semua baris yang mempunya nilai NaN di kolom terpilih.
- `astype(int)` : Mengubah tipe data kolom terpilih menjadi integer agar lebih sesuai.

## Data Preprocessing
Tujuan pra-pemrosesan adalah untuk membersihkan dan menyiapkan data agar sesuai untuk model sistem rekomendasi. Untuk Item-Based Collaborative Filtering, kita perlu membuat matriks pengguna-item (User-Item Matrix) di mana barisnya adalah pengguna, kolomnya adalah item, dan nilai di selnya adalah rating.
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

### Langkah-langkah Data Preprocessing:
1. Standardisasi Judul Film (`movies_df`):
   - `movies_df['year'] = pd.to_datetime(movies_df['release_date'], errors='coerce').dt.year.fillna('').astype(str)`:
     Baris ini bertujuan untuk mengekstraksi tahun rilis dari kolom `release_date`. `pd.to_datetime` mencoba mengonversi `release_date` menjadi format tanggal; `errors='coerce'` akan mengubah nilai yang tidak valid menjadi `NaT` (Not a Time). Kemudian, `.dt.year` mengambil tahun, `.fillna('')` mengisi nilai yang hilang dengan string kosong, dan `.astype(str)` mengubahnya menjadi tipe data string. Ini dilakukan untuk mengatasi potensi duplikasi judul film yang sama tetapi dirilis pada tahun yang berbeda.
   - `movies_df['full_title'] = movies_df['title'].str.strip() + ' (' + movies_df['year'].str.strip() + ')'`: Kolom baru `full_title` dibuat dengan menggabungkan judul film (`title`) dan tahun rilis (`year`). Penggunaan `.str.strip()` membantu menghilangkan spasi ekstra di awal atau akhir string. Format `Judul (Tahun)` ini menciptakan identifikasi unik untuk setiap film.
   - `movies_df['full_title'] = movies_df['full_title'].replace(' \(\)', '', regex=True)`: Baris ini menghilangkan `()` dari `full_title` jika `year` kosong atau tidak valid (misalnya, jika `release_date` asli tidak ada).
   - `movies_df['full_title'] = movies_df['full_title'].fillna(movies_df['title'])`: Baris ini memastikan bahwa jika `full_title` masih `NaN` (mungkin karena `title` atau `year` asli bermasalah), maka akan diisi kembali dengan nilai dari kolom `title` asli.
   - `movies_df_filtered = movies_df[['id', 'full_title']].copy()`: Membuat DataFrame baru yang hanya berisi kolom `id` dan `full_title` yang sudah distandardisasi. Penggunaan `.copy()` mencegah `SettingWithCopyWarning`.
   - `movies_df_filtered.rename(columns={'id': 'movieId', 'full_title': 'title'}, inplace=True)`: Mengganti nama kolom `id` menjadi `movieId` dan `full_title` menjadi `title` agar konsisten dengan nama kolom di `ratings_df`, yang diperlukan untuk penggabungan selanjutnya.
2. Menggabungkan DataFrame `ratings_df` dengan `movies_df_filtered`:
   `data = pd.merge(ratings_df, movies_df_filtered, on='movieId', how='inner')`: Kedua DataFrame (`ratings_df` dan `movies_df_filtered`) digabungkan berdasarkan kolom `movieId`. `how='inner'` memastikan hanya baris-baris (film) yang memiliki `movieId` yang cocok di kedua DataFrame yang akan disertakan dalam DataFrame `data` yang baru. Hasilnya adalah satu DataFrame yang berisi `userId`, `movieId`, `rating`, dan `title` (yaitu `full_title` yang sudah distandardisasi).
3. Membuat Matriks *User_Item* (Pivot Table):
   - `user_movie_matrix = data.pivot_table(index='userId', columns='title', values='rating')`: Baris ini adalah langkah krusial untuk membuat matriks *User-Item*.
      - `index='userId'`: Setiap baris matriks akan mewakili seorang pengguna.
      - `columns='title'`: Setiap kolom matriks akan mewakili sebuah film (dengan judul yang sudah distandardisasi).
      - `values='rating'`: Nilai di setiap sel matriks adalah rating yang diberikan oleh `userId` tertentu untuk `title` film tertentu. Jika seorang pengguna belum memberikan rating pada film tertentu, sel tersebut akan berisi `NaN`.
   - `print("\n--- User-Movie Matrix (Partial View) ---")` dan `print(user_movie_matrix.head())`: Menampilkan lima baris pertama dari matriks yang baru dibuat untuk melihat strukturnya.
   - `print("\nUkuran User-Movie Matrix:", user_movie_matrix.shape)`: Menampilkan dimensi (jumlah pengguna vs. jumlah film) dari matriks tersebut.
4. Menangani Nilai `NaN di Matriks *User-Item*:
   - `user_movie_matrix_filled = user_movie_matrix.fillna(0)`: Nilai `NaN` dalam `user_movie_matrix` diisi dengan 0. Dalam konteks sistem rekomendasi, mengisi `NaN` dengan 0 sering kali berarti bahwa tidak adanya rating dianggap sebagai tidak adanya interaksi atau bahwa pengguna tidak memiliki preferensi (netral) terhadap film tersebut. Ini penting karena algoritma kesamaan memerlukan input numerik.
   - `print("\n--- User-Movie Matrix Setelah Mengisi NaN dengan 0 (Partial View) ---")` dan `print(user_movie_matrix_filled.head())`: Menampilkan kembali lima baris pertama setelah pengisian NaN untuk memverifikasi perubahannya.

## Modeling
Untuk Item-Based Collaborative Filtering, kita perlu menghitung kesamaan antar film. Kesamaan ini dihitung berdasarkan pola rating yang diberikan oleh pengguna.

Representasi Item: Setiap film direpresentasikan sebagai sebuah vektor rating dari semua pengguna.

Matriks Kesamaan: Kita akan menghitung Cosine Similarity antar film. Cosine Similarity adalah metrik umum untuk mengukur kesamaan arah antara dua vektor non-nol. Nilai Cosine Similarity berkisar dari -1 (berlawanan sempurna) hingga 1 (sama persis), dengan 0 menunjukkan ketidakserupaan.

### Algoritma Rekomendasi:

- Pilih film yang dijadikan dasar rekomendasi.
- Dapatkan skor kesamaan film tersebut dengan semua film lain dari matriks kesamaan.
- Urutkan film-film lain berdasarkan skor kesamaan secara menurun.
- Saring film yang sudah ditonton oleh pengguna (untuk implementasi lebih lanjut) atau film itu sendiri.
- Ambil N film teratas sebagai rekomendasi.

### Fungsi Rekomendasi 
Menggunakan fungsi 'get_item_based_recommendations' berfunsgi menjadi sistem rekomendasi untuk mencari dan memberikan daftar film lainnya yang mirip dengan film yang dimasukkan. 

## Contoh Output Rekomendasi
Setelah model kesamaan antar item (`item_similarity_df`) dibuat dan fungsi rekomendasi (`get_item_based_recommendations`) didefinisikan, selanjutnya dapat menguji sistem rekomendasi dengan memberikan beberapa juudl film sebagai *input*. Berikut adalah contoh hasil rekomendasi untuk beberapa film:
```
Total film unik di matriks kesamaan: 2830

--- Rekomendasi untuk 'Pulp Fiction (1994)' ---
Menggunakan judul: 'Pulp Fiction (1994.0)'
title
Lord of Illusions (1995.0)     0.468896
Ödipussi (1988.0)              0.456318
Eaten Alive! (1980.0)          0.456318
The Inquisitor (1981.0)        0.456318
CQ (2001.0)                    0.456318
Modern Life (2008.0)           0.453065
The Cardinal (1963.0)          0.443426
Beste Zeit (2008.0)            0.424130
Monsieur Batignole (2002.0)    0.419873
What Lies Beneath (2000.0)     0.405616
Name: Pulp Fiction (1994.0), dtype: float64

--- Rekomendasi untuk 'Toy Story (1995)' ---
Judul 'Toy Story (1995)' tidak ditemukan dalam format apapun di dataset.

--- Rekomendasi untuk 'Shawshank Redemption, The (1994)' ---
Menggunakan judul: 'The Shawshank Redemption (1994.0)'
title
Natural Born Killers (1994.0)                 0.723102
The Rolling Stones: Gimme Shelter (1970.0)    0.721118
1408 (2007.0)                                 0.700140
The Cider House Rules (1999.0)                0.700140
What Lies Beneath (2000.0)                    0.700140
Groundhog Day (1993.0)                        0.700140
Helen (2009.0)                                0.700140
A Trip to the Moon (1902.0)                   0.700140
Ladyhawke (1985.0)                            0.700140
The Birds (1963.0)                            0.700140
Name: The Shawshank Redemption (1994.0), dtype: float64

--- Rekomendasi untuk 'Film Fiksi Fantasi (2025)' ---
Film 'Film Fiksi Fantasi (2025)' tidak ditemukan dalam dataset kesamaan.
Mohon periksa ejaan atau format judul yang benar.
Tidak ada rekomendasi yang ditemukan untuk 'Film Fiksi Fantasi (2025)'.
```
Dari contoh di atas, terlihat bahwa sistem berhasil memberikan rekomendasi film yang mirip dengan "Pulp Fiction (1994)" dan "The Shawshank Redemption (1994)". Sistem juga menangani kasus di mana film yang dicari tidak ditemukan dalam dataset, seperti "Toy Stoty (1995)" (kemungkinan karena perbedaan format judul) dan "Film Fiksi Fantasi (2025)" yang memang tidak ada. Ini menunjukkan kemampuan model untuk menentukan kemiripan berdasarkan pola rating dan memberikan *feedback* yang sesuai jika film tiddak ditemukan. 

## Evaluation
Evaluasi merupakan tahapan krusial untuk mengukur seberapa efektif dan akurat sistem rekomendasi yang telah dibangun. Selain pengujian fungsional dasar (seperti memastikan film ditemukan dan rekomendasi dihasilkan), evaluasi kuantitatif dengan metrik yang relevan sangat penting untuk memahami kinerja model dalam merekomendasikan item yang relevan kepada pengguna.

### Metrik Evaluasi Kuantitatif
Untuk sistem rekomendasi berbasis item, metrik yang umum digunakan untuk mengukur kinerja Top-N rekomendasi adalah Precision@K dan Recall@K. Metrik-metrik ini mengukur seberapa baik sistem dalam mengidentifikasi item yang relevan dalam daftar rekomendasi teratas (Top-N).

1. Precision@K (Precision at K):
- Definisi: Mengukur proporsi item relevan dalam daftar `K` rekomendasi teratas yang diberikan oleh sistem.
- Interpretasi: Menjawab pertanyaan: "Dari semua item yang direkomendasikan, berapa banyak yang benar-benar relevan (disukai/diinteraksikan oleh pengguna)?"
- Relevansi: Penting untuk mengukur "kualitas" rekomendasi. Nilai Precision@K yang tinggi menunjukkan bahwa sebagian besar rekomendasi yang diberikan adalah item yang benar-benar diinginkan oleh pengguna.
2. Recall@K (Recall at K):
- Definisi: Mengukur proporsi item relevan yang sebenarnya dari seluruh item relevan yang mungkin ada (di dataset uji) yang berhasil ditemukan dan disertakan dalam daftar `K` rekomendasi teratas.
- Interpretasi: Menjawab pertanyaan: "Dari semua item relevan yang seharusnya ditemukan untuk pengguna ini, berapa banyak yang berhasil ditemukan oleh sistem dalam daftar rekomendasi teratas?"
- Relevansi: Penting untuk mengukur "cakupan" sistem. Nilai Recall@K yang tinggi menunjukkan bahwa sistem mampu menangkap sebagian besar item yang relevan bagi pengguna.
3.  F1-score@K (F1-score at K):
- Definisi: Merupakan rata-rata harmonis dari Precision@K dan Recall@K, memberikan keseimbangan antara keduanya.
- Interpretasi: Memberikan gambaran tunggal tentang kinerja model ketika Precision dan Recall sama-sama penting.

### Metodologi Perhitungan
Untuk menghitung metrik ini, langkah-langkah yang dilakukan:

1. Pembagian Data Per Pengguna: Dataset rating awal (`data`) dibagi menjadi set pelatihan (training) dan set pengujian (testing) secara terpisah untuk setiap pengguna. Ini dilakukan dengan membagi rating yang dimiliki oleh seorang pengguna ke dalam dua subset (misalnya, 70% untuk pelatihan, 30% untuk pengujian).
2. Identifikasi Item Relevan: Untuk setiap pengguna di set pengujian, film-film yang diberi rating tinggi (misalnya, rating ≥4.0) diidentifikasi sebagai "item relevan" (ground truth).
3. Generasi Rekomendasi: Berdasarkan film-film yang dirating oleh pengguna di set pelatihan, sistem rekomendasi menghasilkan daftar Top-K film yang direkomendasikan. Film yang sudah pernah dirating oleh pengguna di set pelatihan dikecualikan dari daftar rekomendasi.
4. Perhitungan Komponen Metrik: Untuk setiap pengguna, dihitung:
- True Positives (TP): Jumlah item yang direkomendasikan DAN relevan (ada di daftar item relevan set pengujian).
- False Positives (FP): Jumlah item yang direkomendasikan TAPI tidak relevan.
- False Negatives (FN): Jumlah item yang relevan TAPI tidak direkomendasikan.
5. Perhitungan Precision@K dan Recall@K Individual: Precision@K dan Recall@K dihitung untuk setiap pengguna berdasarkan TP, FP, dan FN.
6. Agregasi: Rata-rata dari semua nilai Precision@K dan Recall@K dari seluruh pengguna dihitung untuk mendapatkan metrik kinerja model secara keseluruhan. F1-score@K kemudian dihitung dari rata-rata Precision dan Recall.

### Hasil Evaluasi Kuantitatif
Berdasarkan perhitungan metrik evaluasi yang dilakukan di notebook pada model Item-Based Collaborative Filtering ini dengan `K = 10` (jumlah rekomendasi teratas yang dipertimbangkan), diperoleh hasil sebagai berikut:

Average Precision@10: `[INSERT_NILAI_PRECISION_DISINI]`
Interpretasi: Nilai ini menunjukkan bahwa rata-rata sekitar `[INSERT_NILAI_PRECISION_DISINI x 100]%` dari 10 film yang direkomendasikan kepada pengguna adalah film yang relevan atau disukai oleh pengguna tersebut di data uji.
Average Recall@10: `[INSERT_NILAI_RECALL_DISINI]`
Interpretasi: Nilai ini menunjukkan bahwa rata-rata sistem berhasil menangkap sekitar `[INSERT_NILAI_RECALL_DISINI x 100]%` dari total film relevan yang seharusnya ditemukan untuk pengguna di data uji.
Average F1-score@10: `[INSERT_NILAI_F1_SCORE_DISINI]`
Interpretasi: Nilai ini mencerminkan keseimbangan antara presisi dan cakupan rekomendasi, dengan nilai `[INSERT_NILAI_F1_SCORE_DISINI]` menunjukkan kinerja model secara keseluruhan dalam mengidentifikasi item relevan.

### Pengujian Fungsional (Verifikasi)
Selain metrik kuantitatif, pengujian fungsional juga dilakukan untuk memverifikasi bahwa sistem dapat mencari judul film dengan benar dan memberikan rekomendasi. Ini mencakup:

- Membuat daftar semua judul film yang dikenal oleh sistem.
- Mencari film spesifik (misalnya, 'Pulp Fiction (1994)', 'Toy Story (1995)', 'The Shawshank Redemption (1994)') untuk memastikan keberadaan dan format judul yang benar.
- Menampilkan contoh rekomendasi untuk judul yang ada dan feedback untuk judul yang tidak ditemukan, seperti yang telah ditunjukkan pada bagian Modeling. Ini memastikan bahwa fungsi `get_item_based_recommendations` bekerja sesuai harapan dan memberikan output yang informatif kepada pengguna.

