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

The Movie Dataset adalah dataset yang diambil dari Kaggle yang berisikan data-data film dari The Movie Database (TMDb) dan sistem rekomendasi MovieLens. Dataset ini sangat cocok untuk membangun sistem rekomendasi karena menyediakan dua komponen utama: 
1. `movies_df` : berisi metadata film ekstensif dari movies_metadata.csv berjumlah 45.000 film dari TMDb. Variabel yang digunakan sebagai berikut:
   - `id` : ID film atau kode unik untuk setiap film berbeda.
   - `title` : Judul film, berhubungan dengan `id`.
   - `release_date`: Tanggal film tayang.
2. `ratings_df` : berisi rating_small.csv yang merupakan data rating *user* berjumlah 100.000 rating dari 700 *user* dari 9.000 film. Variabel yang digunakan sebagai berikut:
  - `userID` : ID *user* atau kode unik *user*.
  - `movieID` : Kode unik untuk film, dan setiap film memiliki kode unik yang berbeda.
  - `rating` : Penilaian dari *user* terhadap film.

### Tahapan Eksplorasi Data (Exploratory Data Analysis - EDA)
- `info()` : Untuk menampilkan ringkasan informasi tentang dataset.
- `head()` : Untuk menampilkan 5 baris pertama dari dataset.
- `describe()` : Untuk menghasilkan statistika deskriptif untuk kolom-kolom numerik.
- `isnull().sum()` : Membuat DataFrame baru dengan nilai True di mana ada nilai kosong dan False di tempat lain dan juga menghitung jumlah nilai yang hilang. 
- `to_numeric()` : Mengubah kolom menjadi tipe data numerik.
- `errors='coerce'` : Jika kolom yang dipilih tidak bisa diubah menjadi angka, maka nilai tersebut diubah menjadi NaN.
- `dropna()` : Menghapus semua baris yang mempunya nilai NaN di kolom terpilih.
- `astype(int)` : Mengubah tipe data kolom terpilih menjadi integer agar lebih sesuai.

```
# Informasi dataset ratings
print("Info DataFrame Ratings")
ratings_df.info()
print("5 Baris Pertama DataFrame Ratings")
print(ratings_df.head())
print("Statistik Deskriptif DataFrame Ratings")
print(ratings_df.describe())
print("Jumlah Nilai Hilang DataFrame Ratings")
print(ratings_df.isnull().sum())
```

```
# Informasi Dataset Movies
print("Info DataFrame Movies")

# --- Perbaikan untuk masalah DtypeWarning dan AttributeError pada kolom 'id' ---
# Coba konversi kolom 'id' ke numerik. Gunakan errors='coerce' untuk mengubah nilai non-numerik menjadi NaN.
movies_df['id'] = pd.to_numeric(movies_df['id'], errors='coerce')

# Hapus baris yang memiliki nilai NaN di kolom 'id' (ini adalah baris yang tidak bisa dikonversi ke numerik)
movies_df = movies_df.dropna(subset=['id'])

# Sekarang, konversi ke integer (setelah membersihkan NaN)
movies_df['id'] = movies_df['id'].astype(int)
# --- Akhir perbaikan kolom 'id' ---

movies_df.info()
print("5 Baris Pertama DataFrame Movies")
print(movies_df.head())
print("Jumlah Nilai Hilang DataFrame Movies")
print(movies_df.isnull().sum())
```

## Data Preprocessing
Tujuan pra-pemrosesan adalah untuk membersihkan dan menyiapkan data agar sesuai untuk model sistem rekomendasi. Untuk Item-Based Collaborative Filtering, kita perlu membuat matriks pengguna-item (User-Item Matrix) di mana barisnya adalah pengguna, kolomnya adalah item, dan nilai di selnya adalah rating.
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

### Langkah-langkah Data Preprocessing:
1. Standardisasi Judul Film: Gabungkan `title` dengan tahun rilis `release_date` untuk membuat judul unik.
2. Menggabungkan dataframe `ratings_df` dengan `movies_df` untuk mendapatkan judul film yang distandardisasi.
   Penggabungan kedua dataframe ini untuk mendapatkan judul film yang distandarisasi menggunakan inner join untuk memastikan hanya film yang ada di kedua dataset yang disertakan.
4. Membuat matriks pivot (User-Item Matrix).
5. Menangani nilai NaN di matriks pivot (mengganti dengan 0).

```
# Standardisasi Judul Film: Tambahkan tahun rilis ke judul untuk mengatasi duplikasi nama film.
# Pertama, ekstrak tahun dari 'release_date'
movies_df['year'] = pd.to_datetime(movies_df['release_date'], errors='coerce').dt.year.fillna('').astype(str)

# Buat kolom 'full_title' dengan format "Judul (Tahun)"
# Gunakan .str.strip() untuk membersihkan spasi ekstra
movies_df['full_title'] = movies_df['title'].str.strip() + ' (' + movies_df['year'].str.strip() + ')'

# Jika judul atau tahun mungkin NaN/kosong setelah konversi
# Hapus " ()" jika tahun kosong atau judul asli memang tidak ada tahun
movies_df['full_title'] = movies_df['full_title'].replace(' \(\)', '', regex=True)
movies_df['full_title'] = movies_df['full_title'].fillna(movies_df['title']) # Jika full_title masih NaN, gunakan title asli

# Pilih kolom yang relevan dan ganti nama 'id' menjadi 'movieId'
movies_df_filtered = movies_df[['id', 'full_title']].copy()
movies_df_filtered.rename(columns={'id': 'movieId', 'full_title': 'title'}, inplace=True)

# Gabungkan ratings_df dengan movies_df_filtered untuk mendapatkan judul film yang distandardisasi
# Gunakan inner join untuk memastikan hanya film yang ada di kedua dataset yang disertakan.
data = pd.merge(ratings_df, movies_df_filtered, on='movieId', how='inner')

# Buat matriks user-item (pivot table)
# rows: userId, columns: title (full_title), values: rating
user_movie_matrix = data.pivot_table(index='userId', columns='title', values='rating')

print("\n--- User-Movie Matrix (Partial View) ---")
print(user_movie_matrix.head())
print("\nUkuran User-Movie Matrix:", user_movie_matrix.shape)

# Mengisi NaN dengan 0 (untuk perhitungan kesamaan, ini artinya tidak ada rating)
user_movie_matrix_filled = user_movie_matrix.fillna(0)
print("\n--- User-Movie Matrix Setelah Mengisi NaN dengan 0 (Partial View) ---")
print(user_movie_matrix_filled.head())
```

## Modeling
Untuk Item-Based Collaborative Filtering, kita perlu menghitung kesamaan antar film. Kesamaan ini dihitung berdasarkan pola rating yang diberikan oleh pengguna.

Representasi Item: Setiap film direpresentasikan sebagai sebuah vektor rating dari semua pengguna.

Matriks Kesamaan: Kita akan menghitung Cosine Similarity antar film. Cosine Similarity adalah metrik umum untuk mengukur kesamaan arah antara dua vektor non-nol. Nilai Cosine Similarity berkisar dari -1 (berlawanan sempurna) hingga 1 (sama persis), dengan 0 menunjukkan ketidakserupaan.

### Algoritma Rekomendasi:

Pilih film yang dijadikan dasar rekomendasi.
Dapatkan skor kesamaan film tersebut dengan semua film lain dari matriks kesamaan.
Urutkan film-film lain berdasarkan skor kesamaan secara menurun.
Saring film yang sudah ditonton oleh pengguna (untuk implementasi lebih lanjut) atau film itu sendiri.
Ambil N film teratas sebagai rekomendasi.

```
# Transpose matriks untuk menghitung kesamaan antar film (item) baris adalah judul film, kolom adalah userId, dan nilai adalah rating.
item_user_matrix = user_movie_matrix_filled.T

print("\n--- Item-User Matrix (Partial View) ---")
print(item_user_matrix.head())
print("\nUkuran Item-User Matrix:", item_user_matrix.shape)

# Menghitung Cosine Similarity antar film
item_similarity_matrix = cosine_similarity(item_user_matrix)

# Membuat DataFrame dari matriks kesamaan untuk akses yang lebih mudah
item_similarity_df = pd.DataFrame(item_similarity_matrix, index=item_user_matrix.index, columns=item_user_matrix.index)

print("\n--- Item Similarity Matrix (Partial View) ---")
print(item_similarity_df.head())
print("\nUkuran Item Similarity Matrix:", item_similarity_df.shape)
```

### Fungsi Rekomendasi 
Menggunakan fungsi 'get_item_based_recommendations' berfunsgi menjadi sistem rekomendasi untuk mencari dan memberikan daftar film lainnya yang mirip dengan film yang dimasukkan. 

```
def get_item_based_recommendations(movie_title, item_similarity_df, num_recommendations=10):
    
    if movie_title not in item_similarity_df.columns:
        print(f"Film '{movie_title}' tidak ditemukan dalam dataset kesamaan.")
        print("Mohon periksa ejaan atau format judul yang benar.")
        return pd.Series()

    # Dapatkan skor kesamaan untuk film ini dengan semua film lain
    similar_movies = item_similarity_df[movie_title].sort_values(ascending=False)

    # Hapus film itu sendiri dari daftar rekomendasi
    similar_movies = similar_movies.drop(movie_title, errors='ignore')

    # Ambil N rekomendasi teratas
    recommendations = similar_movies.head(num_recommendations)

    return recommendations
```

## Evaluation
Proses evaluasi ini merupakan proses testing program sistem rekomendasi yang telah dibuat. Tujuannya untuk memastikan bahwa judul film yang ingin diuji benar-benar ada di dalam dataset dalam format yang benar.Berikut cara kerja program:
- 'sorted_titles = sorted(item_similarity_df.columns.tolist())' : Berfungsi untuk membuat daftar semua judul film yang dikenal oleh sistem.
   'item_similarity_df.columns.tolist()': Mengambil semua nama kolom dari 'item_similarity_df'. Kolom-kolom ini adalah judul film yang sudah distandardisasi dan ada di matriks kemiripan Anda. Hasilnya adalah sebuah daftar judul film.
   'sorted(...)': Mengurutkan daftar judul film tersebut secara alfabetis.
   'sorted_titles = ...' : Menyimpan daftar judul film yang sudah diurutkan ini ke dalam variabel sorted_titles.


```
print("Testing Rekomendasi")

print("Contoh Judul Film yang Ada di Matriks Kesamaan (sorted)")
# Urutkan secara alfabetis untuk memudahkan pencarian visual
sorted_titles = sorted(item_similarity_df.columns.tolist())

print(sorted_titles[0:20]) # Munculin 20 judul pertama

# Cari film spesifik
print(f"Judul yang mengandung 'Pulp' (case-insensitive)")
pulp_titles = [title for title in sorted_titles if 'pulp' in title.lower()]
print(pulp_titles)

print(f"Judul yang mengandung 'Toy Story' (case-insensitive)")
toy_story_titles = [title for title in sorted_titles if 'toy story' in title.lower()]
print(toy_story_titles)

print(f"Judul yang mengandung 'Shawshank' (case-insensitive)")
shawshank_titles = [title for title in sorted_titles if 'shawshank' in title.lower()]
print(shawshank_titles)
```
