# Laporan Proyek Sistem Rekomendasi Film - Faturohman Wicaksono

---

## **Project Overview**

### Latar Belakang

Di era digital saat ini, di mana platform *streaming* film menyediakan katalog yang sangat luas dengan jutaan judul, pengguna seringkali menghadapi fenomena "pilihan berlebihan" (*over-choice*). Hal ini menyebabkan pengguna menghabiskan lebih banyak waktu untuk mencari film daripada menikmati tontonan itu sendiri, yang pada akhirnya dapat mengurangi kepuasan pengguna dan memicu *churn* (berhenti berlangganan) pada layanan. Kebutuhan akan personalisasi yang efektif menjadi krusial untuk meningkatkan pengalaman pengguna dan mendorong interaksi yang lebih dalam dengan konten.

### Mengapa Proyek Ini Penting?

Proyek sistem rekomendasi film ini menjadi sangat penting karena beberapa alasan:

1.  **Meningkatkan Kepuasan Pengguna:** Dengan menyediakan rekomendasi yang relevan dan dipersonalisasi, pengguna dapat dengan mudah menemukan film yang sesuai dengan selera mereka, mengurangi waktu pencarian, dan meningkatkan pengalaman menonton secara keseluruhan.
2.  **Meningkatkan Keterlibatan (Engagement):** Rekomendasi yang tepat dapat mendorong pengguna untuk menonton lebih banyak film, menjelajahi genre baru, dan berinteraksi lebih aktif dengan platform, yang pada gilirannya meningkatkan durasi sesi dan frekuensi kunjungan.
3.  **Meningkatkan Pendapatan Bisnis:** Bagi penyedia layanan, rekomendasi yang efektif dapat berkorelasi langsung dengan peningkatan metrik bisnis seperti tingkat konversi penjualan/tontonan, retensi pelanggan, dan loyalitas merek. Film-film baru atau *niche* juga bisa mendapatkan visibilitas yang layak.
4.  **Mengatasi Tantangan Skala:** Katalog film yang terus berkembang memerlukan sistem cerdas yang mampu mengelola dan menyajikan konten secara efisien tanpa memerlukan intervensi manual yang besar.

### Hasil Riset/Referensi Terkait

Sistem rekomendasi telah terbukti menjadi komponen integral dalam platform digital modern. Studi menunjukkan dampak signifikan mereka, seperti laporan Amazon yang menyebutkan bahwa 35% penjualannya berasal dari rekomendasi (Amazon, "The Power of Recommendations", 2013). Netflix juga mengklaim bahwa personalisasi dan rekomendasi secara substansial mengurangi *churn* pelanggan (Netflix, "The Netflix Recommender System: Algorithms, Business Cases, and Innovations", 2015).

Secara fundamental, sistem rekomendasi bertujuan untuk mengatasi masalah informasi yang berlebihan (*information overload*). Pendekatan *Collaborative Filtering*, khususnya, telah menjadi pilar dalam bidang ini. Salah satu publikasi seminal yang menjelaskan dasar-dasar dan aplikasi Collaborative Filtering adalah **"Item-Based Collaborative Filtering Recommendation Algorithms" oleh Sarwar et al. (2001)**, yang menguraikan metode-metode untuk merekomendasikan item berdasarkan kemiripan antar-item dari pola interaksi pengguna. Proyek ini mengimplementasikan prinsip-prinsip dari model rekomendasi yang telah terbukti efektif dalam industri, termasuk Content-Based Filtering dan Collaborative Filtering berbasis Neural Network, yang merupakan evolusi dari metode-metode dasar tersebut.

---

## **Business Understanding**

### Problem Statements

1.  Bagaimana membantu pengguna menavigasi dan menemukan film yang paling relevan dengan selera mereka di tengah lautan konten yang terus bertambah?
2.  Bagaimana menciptakan rekomendasi film yang lebih cerdas dan personal dengan mengintegrasikan wawasan dari data perilaku penonton serta karakteristik intrinsik film, demi melampaui batasan metode rekomendasi tunggal?

### Goals

1.  Jawaban untuk Pernyataan Masalah 1:Membangun sistem rekomendasi film yang adaptif dan komprehensif dengan Content-Based Filtering dan Collaborative Filtering untuk meningkatkan relevansi dan pengalaman personalisasi.
2.  Jawaban untuk Pernyataan Masalah 2: Menyajikan rekomendasi Top-N film yang disesuaikan secara individual untuk setiap pengguna, disertai dengan tingkat akurasi prediksi yang dapat dipertanggungjawabkan.

### Solution Approach

Untuk meraih tujuan di atas dan menyelesaikan pernyataan masalah, kami mengajukan dua pendekatan utama dalam membangun sistem rekomendasi, yang kemudian dapat diintegrasikan dalam kerangka hibrida:

1.  **Content-Based Filtering (Pendekatan Berbasis Konten):**
    * **Tujuan:** Untuk mengidentifikasi dan menyarankan film-film yang memiliki karakteristik konten yang serupa dengan apa yang disukai pengguna di masa lalu.
    * **Metodologi Implementasi:**
        * Memanfaatkan **genre film** sebagai atribut metadata utama.
        * Menggunakan **TF-IDF Vectorization** untuk mengubah deskripsi genre menjadi representasi numerik yang terbobot, menunjukkan relevansi unik setiap genre dalam konteks film.
        * Mengukur tingkat kesamaan antar film melalui perhitungan **Cosine Similarity** dari vektor TF-IDF genre mereka, sehingga film dengan profil genre yang serupa akan memiliki skor kesamaan yang tinggi.

2.  **Collaborative Filtering (Pendekatan Kolaboratif Berbasis Jaringan Saraf):**
    * **Tujuan:** Untuk memprediksi preferensi rating pengguna terhadap film yang belum mereka tonton, berdasarkan pola interaksi kolektif dari seluruh basis pengguna.
    * **Metodologi Implementasi:**
        * Menggunakan arsitektur **Neural Network** (`RecommenderNet`) yang melibatkan **embedding** (representasi vektor padat) untuk setiap pengguna dan film. Embedding ini secara implisit menangkap fitur-fitur laten yang membentuk preferensi dan karakteristik.
        * Prediksi rating dihasilkan dari produk titik (*dot product*) antara embedding pengguna dan film, ditambah dengan bias khusus pengguna dan film, yang mewakili kecenderungan rata-rata masing-masing.
        * Dibangun menggunakan *framework* **TensorFlow/Keras** untuk skalabilitas dan efisiensi.
        * Pelatihan model dioptimalkan dengan algoritma **Adam**, meminimalkan fungsi kerugian **Mean Squared Error (MSE)**, dan kinerja dipantau menggunakan metrik **Mean Absolute Error (MAE)**.
        * Integrasi *callback* **Early Stopping** diterapkan selama proses pelatihan untuk secara otomatis menghentikan pelatihan pada titik optimal, mencegah *overfitting* dan memastikan model yang dihasilkan memiliki kemampuan generalisasi terbaik.

---

## **Data Understanding**

Data yang digunakan dalam proyek ini adalah dataset rating dan metadata film dari platform Movielens.

* **Shape Data:** 
    * Dataset `ratings.csv`: 25000095 baris dan 4 kolom.
    * Dataset `movies.csv`: 62423 baris dan 3 kolom.

* **Kondisi Data:**
    * Data `ratings.csv` berisi informasi `userId`, `movieId`, `rating`, dan `timestamp`.
    * Data `movies.csv` berisi `movieId`, `title`, dan `genres`. Kolom `genres` dapat berisi beberapa genre yang dipisahkan oleh `|`. Terdapat juga potensi nilai kosong pada kolom `genres` yang perlu ditangani nantinya dengan mengubah isinya menjadi `Unknown`.

* **Tautan Sumber Data:**
    * Dataset ini dapat diuakses dari situs kaggle dengan author parasharmanas: [Kaggle](https://www.kaggle.com/datasets/parasharmanas/movie-recommendation-system).

* **Variabel/Fitur pada Data:**
    * **`ratings.csv`:**
        * `userId`: ID unik pengguna yang memberikan rating. (Numerik)
        * `movieId`: ID unik film yang diberi rating. (Numerik)
        * `rating`: Nilai rating yang diberikan pengguna untuk film tersebut (skala 0.5-5.0). (Numerik)
        * `timestamp`: Waktu rating diberikan (dalam format Unix timestamp). (Numerik)
    * **`movies.csv`:**
        * `movieId`: ID unik film. (Numerik)
        * `title`: Judul film. (Teks)
        * `genres`: Genre film, bisa lebih dari satu dan dipisahkan oleh `|`. (Teks)

* **Exploratory Data Analysis (EDA) dan Insight:**

    Melalui tahapan EDA, beberapa insight kunci telah diidentifikasi dari dataset:

   ### Outlier Check
   * **Boxplot Rating menggunakan IQR**
     ![boxplot](https://github.com/user-attachments/assets/cbccd677-d983-4453-9e83-18a9e4b35bd6)

        Analisis:

     - Kotak (Box):
             - Sisi kiri kotak (Q1) berada di sekitar 3.
             - Sisi kanan kotak (Q3) berada di sekitar 4.
            - Artinya, 50% dari rating berada antara 3 dan 4.
         - Garis Tengah dalam Kotak (Median/Q2): Garis vertikal di dalam kotak (median) berada di sekitar 3.5. Ini menunjukkan bahwa sebagian besar rating cenderung mengumpul di sisi kanan distribusi (nilai yang lebih tinggi) dalam kotak IQR.
         - Whiskers:
             - Whisker kiri membentang hingga sekitar 2.
             - Whisker kanan membentang hingga 5.

         Outlier: Terlihat ada beberapa titik individual di sebelah kiri whisker kiri, yaitu pada nilai 0.5 dan 1. Ini adalah outlier yang terdeteksi. Nilai rating 0.5 dan 1 jauh lebih rendah dibandingkan sebagian besar rating lainnya.

     * **Boxplot Timestamp menggunakan IQR**
       ![boxplot](https://github.com/user-attachments/assets/ea8dbb56-98bf-4620-8885-9029d2455938)

          Analisis:

        - Kotak (Box): Kotak biru yang tebal mewakili IQR, yaitu rentang antara kuartil pertama (Q1) dan kuartil ketiga (Q3).
          - Sisi kiri kotak (Q1) berada di sekitar 1.05×109.
          - Sisi kanan kotak (Q3) berada di sekitar 1.45×109.
          - Ini berarti 50% data tengah timestamp berada dalam rentang ini.
        - Garis Tengah dalam Kotak (Median/Q2): Garis vertikal di dalam kotak (median) berada tepat di tengah kotak, sekitar 1.25×109. Ini menunjukkan bahwa distribusi timestamp cukup simetris di bagian tengahnya.
        - Whiskers: Garis horizontal yang membentang dari kotak disebut whiskers.
          - Whisker kiri membentang hingga sekitar 0.8×109.
          - Whisker kanan membentang hingga sekitar 1.6×109.
          - Whiskers ini biasanya menunjukkan rentang data dalam 1.5×IQR dari Q1 dan Q3.

         Outlier: Tidak ada titik individual yang terlihat di luar whiskers. Ini menunjukkan bahwa berdasarkan metode deteksi outlier IQR standar, tidak ada outlier yang terdeteksi secara signifikan dalam kolom timestamp. Data timestamp cenderung terdistribusi dengan baik tanpa nilai ekstrem yang jauh.


       **Insight Keseluruhan**

         - Distribusi timestamp cenderung normal/simetris dan bersih dari outlier. Ini adalah kabar baik karena data waktu biasanya sangat penting dan outlier dapat menunjukkan masalah dalam pengumpulan data atau peristiwa yang sangat jarang terjadi. Dengan distribusi yang simetris, rata-rata dan median akan menjadi ukuran pemusatan yang representatif.
         - Distribusi rating cenderung miring ke kiri (negatively skewed) dengan outlier di sisi rendah.
           - Kemiringan: Median rating (3.5) lebih dekat ke Q3 (4) daripada ke Q1 (3), dan sebagian besar data terkonsentrasi di nilai rating yang lebih tinggi (3−4). Ini menunjukkan bahwa mayoritas pengguna memberikan rating yang relatif tinggi (cenderung 3 ke atas).
           - Outlier: Adanya outlier pada rating 0.5 dan 1 adalah hal yang signifikan.
               - Potensi Interpretasi Outlier:
                   - Data Entry Error: Bisa jadi kesalahan saat memasukkan data.
                   - Produk/Layanan Buruk: Menunjukkan pengalaman yang sangat buruk dari sebagian kecil pengguna.
                   - Uji Coba/Bot: Mungkin ada rating awal atau rating dari bot/sistem yang tidak representatif.
                   - Pengguna yang Sangat Kritis: Sebagian kecil pengguna yang memberikan rating sangat rendah secara konsisten.
                   - Bug/Masalah Teknis: Pengguna memberikan rating rendah karena mengalami masalah teknis dengan produk/layanan.

         Data rating akan diubah skalanya ke dalam rentang antara 0 dan 1 menggunakan metode Min-Max Scaling. Hal ini bertujuan untuk menormalisasi nilai rating sehingga semua nilai, termasuk outlier, berada dalam rentang yang seragam, yang dapat membantu algoritma sistem rekomendasi yang sensitif terhadap skala data.      


    ### Univariate Analysis EDA
    * **Frekuensi Genre Film:** 
      ![Univariate](https://github.com/user-attachments/assets/ab47ea19-a492-4249-b0e9-f6f99cee2b1c)
      

      Analisis:

       - Dominasi Genre Drama: Genre "Drama" adalah yang paling sering muncul dalam dataset, dengan jumlah kemunculan jauh di atas genre lainnya (lebih dari 25.000). Ini menunjukkan bahwa film bergenre drama sangat banyak diproduksi atau populer dalam dataset ini.

       - Genre Populer Lainnya: "Comedy" juga sangat populer, menempati posisi kedua dengan jumlah kemunculan sekitar 16.000. Diikuti oleh "Thriller" dan "Romance" yang masing-masing memiliki sekitar 8.000-9.000 kemunculan.

       - Genre dengan Frekuensi Sedang: "Action", "Horror", "Documentary", "Crime", dan "(no genres listed)" (kemungkinan kategori untuk film yang tidak memiliki genre spesifik terdaftar) memiliki frekuensi yang cukup mirip, berkisar antara 4.000 hingga 7.000.

       - Genre Kurang Populer: Genre-genre seperti "Adventure", "Sci-Fi", "Children", "Animation", "Mystery", "Fantasy", "War", "Western", "Musical", "Film-Noir", dan "IMAX" memiliki frekuensi yang jauh lebih rendah, dengan "IMAX" menjadi yang paling jarang muncul (kurang dari 1.000).

       - Persebaran yang Tidak Merata: Distribusi genre dalam dataset ini sangat tidak merata, dengan beberapa genre yang sangat mendominasi dan banyak genre lain yang jarang muncul.

      Insight:

       - Preferensi Produksi/Konsumsi: Dataset ini kemungkinan besar mencerminkan preferensi produksi film atau preferensi penonton yang lebih condong ke genre drama dan komedi. Produser film mungkin menemukan bahwa genre-genre ini memiliki pasar yang lebih besar atau lebih mudah untuk diproduksi.

       - Kesenjangan Konten: Ada kesenjangan yang signifikan dalam representasi genre. Jika dataset ini digunakan untuk rekomendasi film, sistem mungkin akan lebih sering merekomendasikan film drama atau komedi karena ketersediaan datanya yang melimpah.

       - Potensi Pasar Niche: Meskipun beberapa genre jarang, ini bisa menunjukkan potensi pasar niche yang belum sepenuhnya dieksplorasi. Misalnya, meskipun "IMAX" jarang, jika ada permintaan yang signifikan untuk film-film tersebut, ini bisa menjadi area pertumbuhan.
     

    * **Distribusi Tahun Rilis Film:**
      ![Univariate](https://github.com/user-attachments/assets/475d3012-d0df-473d-821e-5c004c660db4)

      Analisis:

        - Peningkatan Produksi Film yang Konsisten: Secara keseluruhan, terlihat jelas tren peningkatan jumlah film yang dirilis dari waktu ke waktu. Dimulai dari frekuensi yang sangat rendah di akhir abad ke-19, jumlah film yang dirilis terus meningkat.

        - Periode Awal (Pra-1940-an): Pada periode awal perfilman (sebelum sekitar tahun 1940), jumlah film yang dirilis sangat rendah. Ada peningkatan bertahap yang lambat.

        - Pertumbuhan Pasca-Perang Dunia (1940-an - 1980-an): Setelah sekitar tahun 1940-an, frekuensi rilis film mulai menunjukkan pertumbuhan yang lebih nyata, meskipun masih relatif stabil di angka ratusan hingga sekitar 1500 film per tahun pada periode 1960-an hingga 1980-an.

        - Akselerasi di Era Modern (Akhir 1990-an - 2010-an): Terjadi peningkatan yang sangat tajam dalam jumlah rilis film mulai akhir tahun 1990-an hingga puncak di sekitar tahun 2010-an. Ini adalah periode dengan pertumbuhan produksi film yang paling pesat.

        - Puncak Produksi (Sekitar 2010-2015): Jumlah film yang dirilis mencapai puncaknya di sekitar tahun 2010-2015, dengan frekuensi tertinggi mendekati 7500 film per tahun.

        - Penurunan Ringan Menjelang Akhir Data (Setelah 2015): Ada sedikit penurunan frekuensi setelah puncaknya, sekitar tahun 2015 hingga menjelang tahun 2020. Ini mungkin menunjukkan sedikit perlambatan, atau bisa juga karena data untuk tahun-tahun terakhir belum sepenuhnya lengkap.



      Insight:

        - Evolusi Industri Film: Grafik ini dengan jelas menggambarkan evolusi dan pertumbuhan industri film selama lebih dari satu abad. Peningkatan yang eksponensial di era modern menunjukkan bahwa pembuatan film menjadi lebih mudah diakses, lebih terjangkau, atau permintaan akan konten film meningkat drastis.

        - Dampak Teknologi: Peningkatan tajam di akhir abad ke-20 dan awal abad ke-21 kemungkinan besar didorong oleh kemajuan teknologi. Digitalisasi dalam produksi, distribusi, dan konsumsi film (misalnya, kemunculan DVD, Blu-ray, internet, dan platform streaming) telah memungkinkan lebih banyak film untuk diproduksi dan didistribusikan.

        - Demokratisasi Pembuatan Film: Alat-alat produksi yang semakin terjangkau dan platform distribusi digital (seperti YouTube, Vimeo, dan platform film indie) telah memungkinkan lebih banyak individu dan kelompok kecil untuk membuat dan merilis film, bukan hanya studio besar. Ini bisa menjelaskan lonjakan frekuensi.

        - Perubahan Lanskap Media: Peningkatan ini juga mencerminkan perubahan dalam lanskap media secara keseluruhan, di mana konten visual menjadi semakin sentral dalam konsumsi hiburan.


    * **Frequency dari Masing-Masing Value**
      ![univariate](https://github.com/user-attachments/assets/aa2c7fd9-3841-4683-b587-4b117d4fdb8a)

      Analisis:

      Dari grafik, kita dapat mengamati hal-hal berikut:

        - Rating Paling Dominan adalah 4.0: Nilai rating 4.0 memiliki frekuensi tertinggi, yaitu di atas 6 juta. Ini menunjukkan bahwa nilai 4.0 adalah rating yang paling sering diberikan oleh pengguna dalam dataset ini.

        - Rating Populer Lainnya:
            - Rating 3.0 menempati posisi kedua dengan frekuensi mendekati 5 juta.
            - Rating 5.0 berada di posisi ketiga dengan frekuensi sekitar 3.5 juta.
            - Rating 3.5 dan 4.5 juga cukup sering diberikan, masing-masing di atas 3 juta dan di atas 2 juta.

        - Penurunan Frekuensi Seiring Penurunan Rating (secara umum): Secara umum, semakin rendah nilai rating, semakin rendah pula frekuensinya.
            - Rating 2.0 dan 2.5 memiliki frekuensi di atas 1 juta.
            - Rating 1.0 dan 1.5 memiliki frekuensi di bawah 1 juta.
            - Rating 0.5 adalah yang paling jarang diberikan, dengan frekuensi kurang dari 500 ribu.

        - Distribusi Preferensi Positif: Sebagian besar rating cenderung berada di sisi positif (yaitu 3.0 ke atas), menunjukkan bahwa secara keseluruhan, item yang diberi rating dalam dataset ini diterima dengan cukup baik oleh pengguna. Rating di bawah 3.0 memiliki frekuensi yang jauh lebih rendah.

      Insight:

        - Kepuasan Pengguna Cenderung Tinggi: Distribusi rating yang condong ke nilai-nilai yang lebih tinggi (terutama 4.0, 3.0, dan 5.0) menunjukkan bahwa mayoritas pengguna merasa puas atau cukup puas dengan item yang mereka nilai. Jika ini adalah dataset rating film, ini berarti sebagian besar film diterima dengan baik.

        - Sistem Rating Terbiasa pada Nilai Tertentu: Orang-orang mungkin cenderung memilih nilai "bulat" seperti 4.0, 3.0, atau 5.0 daripada nilai setengah seperti 3.5 atau 4.5, meskipun nilai setengah juga cukup sering digunakan. Preferensi untuk 4.0 sebagai yang tertinggi menunjukkan "sweet spot" bagi pengguna, mungkin sebagai bentuk rating "sangat bagus tapi tidak sempurna."

        - Ulasan Negatif Lebih Jarang Diberikan: Fakta bahwa rating rendah (1.0, 0.5) jauh lebih jarang menunjukkan bahwa pengguna lebih jarang memberikan rating yang sangat negatif. Ini bisa berarti item yang dinilai memang berkualitas baik secara umum, atau orang cenderung tidak memberi rating jika pengalaman mereka sangat buruk, atau mereka lebih memilih untuk tidak memberi rating sama sekali daripada memberi rating yang sangat rendah.


    * **Distribusi dari Rating Film**
      ![univariate](https://github.com/user-attachments/assets/1e6c709f-c25d-491f-8c38-05cbcd1cdddf)

      Analisis:


      Dari grafik, kita dapat mengamati hal-hal berikut:

        - Puncak yang Jelas pada Rating Bulat dan Setengah: Kurva merah menunjukkan puncak frekuensi yang sangat tajam pada nilai rating bulat (misalnya, 3.0, 4.0, 5.0) dan nilai rating setengah (misalnya, 3.5, 4.5). Ini menunjukkan bahwa pengguna cenderung memberikan rating pada skala yang jelas ini.

        - Rating 4.0 Paling Sering Diberikan: Puncak tertinggi pada kurva merah dan batang histogram terbesar berada di sekitar rating 4.0, dengan frekuensi mendekati 3.5×107 (35 juta). Ini berarti rating 4.0 adalah yang paling populer dalam dataset ini.

        - Rating 3.0 dan 5.0 Juga Sangat Populer: Rating 3.0 menunjukkan puncak frekuensi yang signifikan (sekitar 2.4×107 atau 24 juta), dan rating 5.0 juga memiliki puncak yang tinggi (sekitar 1.8×107 atau 18 juta).

        - Rating Setengah Juga Sering Digunakan: Meskipun tidak setinggi rating bulat, rating 3.5 dan 4.5 juga memiliki puncak yang menonjol (sekitar 1.5×107 dan 1.1×107 masing-masing), menunjukkan bahwa pengguna sering menggunakan skala setengah poin.

        - Rating Rendah Kurang Sering Diberikan: Frekuensi rating yang lebih rendah (di bawah 3.0) jauh lebih kecil. Ada puncak-puncak kecil di 0.5, 1.0, 1.5, 2.0, dan 2.5, tetapi frekuensinya secara signifikan lebih rendah dibandingkan dengan rating yang lebih tinggi. Puncak terkecil ada di 0.5 dan 1.5.

        - Distribusi Tidak Normal: Distribusi rating ini jelas tidak mengikuti distribusi normal (bell curve). Sebaliknya, ini adalah distribusi multimodal dengan beberapa puncak yang berbeda, yang merupakan karakteristik umum dari data rating diskrit atau semi-diskrit di mana pengguna memiliki preferensi untuk nilai-nilai tertentu.

      Insight:

        - Preferensi Skala Jelas: Pengguna cenderung memberikan rating pada poin-poin yang mudah dikenali pada skala (misalnya, 1, 2, 3, 4, 5 atau 0.5, 1.5, 2.5, 3.5, 4.5), daripada nilai-nilai di antaranya. Ini adalah perilaku umum dalam sistem rating bintang.

        - Kepuasan Pengguna Cenderung Positif: Mayoritas rating yang diberikan condong ke sisi positif dari skala (3.0 ke atas). Ini menunjukkan bahwa secara keseluruhan, item (film) dalam dataset ini cenderung diterima dengan baik oleh audiensnya.

        - Tantangan untuk Membedakan Kualitas Menengah: Karena banyak rating mengelompok pada nilai-nilai tertentu, mungkin sulit untuk membedakan nuansa kualitas antara film-film yang semuanya menerima rating "sangat bagus" (misalnya, banyak film akan mendapatkan 4.0 atau 3.5).

        - Pengaruh "Pembulatan" Rating: Puncak-puncak tajam menunjukkan bahwa pengguna cenderung "membulatkan" rating mereka ke nilai terdekat yang mereka rasa paling cocok, daripada menggunakan setiap kemungkinan nilai desimal. Ini adalah fenomena umum di banyak platform ulasan.

    * **Distribusi Rating dalam Rentang Waktu**
    ![Univariate](https://github.com/user-attachments/assets/4c011ff4-6836-4bed-844b-d807de5f3c87)
    
      Analisis:

        - Awal Data (1995-1999): Dataset rating dimulai pada awal 1995 dengan jumlah rating yang relatif rendah. Ada peningkatan yang signifikan pada tahun 1996, mencapai sekitar 500.000 rating, kemudian menurun lagi.

        - Puncak Pertama (Sekitar Tahun 2000): Terjadi lonjakan besar dalam jumlah rating sekitar tahun 1999-2000, dengan puncaknya mencapai lebih dari 900.000 rating. Ini menunjukkan periode aktivitas rating yang sangat tinggi.

        - Periode Fluktuatif (2000-2015): Setelah puncak di tahun 2000, jumlah rating menunjukkan penurunan yang cukup signifikan dan fluktuatif, dengan beberapa puncak lebih kecil (misalnya, sekitar 2004 dan 2007) dan lembah-lembah. Tren umumnya menurun dari puncak tahun 2000 hingga sekitar tahun 2014-2015.

        - Puncak Kedua (Sekitar Tahun 2015-2016): Terjadi kenaikan tajam lagi dalam jumlah rating mulai sekitar tahun 2015, mencapai puncak kedua yang signifikan (sekitar 550.000-600.000 rating) di tahun 2015-2016. Ini menandai kebangkitan aktivitas rating.

        - Penurunan Menuju Akhir Data (2016-2019): Setelah puncak kedua, jumlah rating kembali menunjukkan tren penurunan hingga akhir data pada tahun 2019.

      Insight:

        - Daur Hidup Platform/Dataset: Pola fluktuasi ini sangat mungkin mencerminkan daur hidup platform atau situs web tempat rating ini dikumpulkan. Puncak di tahun 2000 bisa jadi karena popularitas awal platform, kampanye promosi, atau periode di mana platform tersebut menjadi sangat dominan. Penurunan setelahnya bisa jadi karena munculnya pesaing, perubahan preferensi pengguna, atau perubahan internal pada platform.

        - Perubahan Perilaku Pengguna: Lonjakan kedua di tahun 2015-2016 bisa menunjukkan kebangkitan minat, mungkin karena fitur baru, peluncuran ulang platform, atau perubahan signifikan dalam cara orang berinteraksi dengan konten online dan memberikan ulasan/rating. Penurunan setelah itu mungkin karena kejenuhan pasar atau pergeseran ke platform lain.

        - Dampak Tren Teknologi/Internet: Puncak di awal tahun 2000-an dan pertengahan 2010-an juga bisa berkorelasi dengan era-era tertentu dalam perkembangan internet dan media digital. Misalnya, pertumbuhan internet di awal 2000-an dan kemudian popularitas media sosial/platform streaming di pertengahan 2010-an.

        - Kualitas dan Relevansi Data: Jika data ini digunakan untuk membangun sistem rekomendasi, penting untuk mempertimbangkan relevansi rating lama. Rating dari tahun 2000 mungkin tidak seakurat atau se-relevan rating dari tahun 2018 dalam memprediksi preferensi pengguna saat ini, terutama jika selera film atau tren media telah berubah.

        - Aktivitas Pengguna: Grafik ini adalah indikator langsung dari aktivitas pengguna dalam memberikan rating dari waktu ke waktu. Fluktuasi besar menunjukkan bahwa volume interaksi pengguna tidak konstan.

      Secara keseluruhan, distribusi stempel waktu ini memberikan gambaran dinamis tentang bagaimana aktivitas pemberian rating telah berubah selama lebih dari dua dekade, yang kemungkinan besar dipengaruhi oleh faktor-faktor seperti evolusi platform, tren teknologi, dan perilaku pengguna.

    ### Bivariate Analysis EDA

    * **Pada Bivariate Analysis saya melakukan inner joint merge untuk melihat insight dari kedua dataset.**
    * **Rata-rata Rating per Genre**
      ![Bivariate](https://github.com/user-attachments/assets/30ccb755-f21e-4122-b6fe-823604555db5)

      Analisis:

        - Genre dengan Rating Rata-rata Tertinggi:
            - Film-Noir memiliki rating rata-rata tertinggi, mendekati 4.0.
        War juga memiliki rating rata-rata yang sangat tinggi, sedikit di bawah Film-Noir.
            - Documentary, Crime, Drama, dan Mystery menyusul di belakang, semuanya memiliki rating rata-rata di atas 3.5.

        - Genre dengan Rating Rata-rata Menengah:
            - Genre-genre seperti Animation, IMAX, Western, Musical, Romance, Thriller, Adventure, Fantasy, Sci-Fi, dan Action memiliki rating rata-rata di kisaran 3.3 hingga 3.6. Ini menunjukkan bahwa film-film dalam genre ini umumnya diterima dengan baik, tetapi tidak seistimewa genre teratas.

        - Genre dengan Rating Rata-rata Terendah:
            - Children, Comedy, dan "(no genres listed)" memiliki rating rata-rata yang lebih rendah, di kisaran 3.2 hingga 3.4.
            - Horror berada di posisi paling bawah dengan rating rata-rata terendah, sekitar 3.25.

        - Rentang Rating Rata-rata yang Relatif Sempit: Meskipun ada perbedaan, rentang rating rata-rata secara keseluruhan tidak terlalu lebar, yaitu dari sekitar 3.25 (Horror) hingga hampir 4.0 (Film-Noir). Ini menunjukkan bahwa sebagian besar genre memiliki kualitas rata-rata yang cukup baik dalam dataset ini.

      Insight:

        - Kualitas yang Dirasakan vs. Popularitas: Menariknya, genre "Drama" yang merupakan genre paling sering muncul (berdasarkan grafik sebelumnya) memiliki rata-rata rating yang tinggi (di atas 3.5), menunjukkan bahwa popularitasnya juga diimbangi dengan penerimaan yang baik. Namun, genre yang paling populer kedua, "Comedy," justru berada di antara genre dengan rating rata-rata terendah. Ini mengisyaratkan bahwa film komedi mungkin diproduksi dalam jumlah besar, tetapi tidak selalu mendapatkan apresiasi rating setinggi genre lain.

        - Apresiasi untuk Genre Niche/Klasik: Genre seperti Film-Noir dan War, yang mungkin tidak diproduksi sebanyak genre populer lainnya, menunjukkan rating rata-rata yang sangat tinggi. Ini bisa berarti bahwa film-film dalam genre ini seringkali dianggap sebagai "karya klasik" atau menarik bagi audiens yang sangat menghargai kualitas tertentu.

        - Harapan Audiens: Rating rata-rata yang rendah untuk Horror dapat menunjukkan bahwa genre ini, meskipun memiliki penggemarnya, mungkin lebih sulit untuk memenuhi harapan audiens atau mungkin memiliki variasi kualitas yang lebih besar. Atau, sifat genre horor itu sendiri (menakutkan, mengganggu) mungkin memicu rating yang lebih terpolarisasi.

        - Implikasi untuk Produser/Distributor: Jika dataset ini merepresentasikan preferensi publik secara luas, produser mungkin akan lebih aman berinvestasi pada genre Drama, Crime, atau Documentary yang memiliki peluang tinggi untuk diterima dengan baik. Sementara itu, untuk genre seperti Comedy atau Horror, meskipun popular, mungkin diperlukan strategi yang lebih kuat untuk memastikan kualitas dan kepuasan penonton.

        - Kategori "(no genres listed)": Fakta bahwa kategori "(no genres listed)" memiliki rating rata-rata yang rendah (mirip dengan Children dan Comedy) bisa menunjukkan bahwa film-film tanpa klasifikasi genre yang jelas cenderung kurang mendapatkan apresiasi. Ini mungkin karena kesulitan dalam menemukan audiens target atau indikasi kualitas yang kurang terdefinisi.

      Secara keseluruhan, grafik ini memberikan pemahaman tentang bagaimana berbagai genre film dipersepsikan dalam hal kualitas (berdasarkan rating rata-rata) dan dapat menjadi panduan berharga bagi pembuat film, kritikus, dan bahkan penonton.

    * **Rata-rata Rating berdasarkan Tahun Rilis**
      ![Bivariate](https://github.com/user-attachments/assets/bf17b26d-3c4b-49f7-945a-887a34e7f634)

      Analisis:

        - Periode Awal (Pra-1920):
            - Pada awal sejarah perfilman (sekitar 1870-an hingga awal 1900-an), rating rata-rata film cenderung berfluktuasi dan relatif rendah, sering di bawah 3.0, bahkan turun mendekati 1.5-2.0 pada beberapa titik di akhir 1880-an.
            - Ada lonjakan sporadis, menunjukkan mungkin ada beberapa film awal yang sangat dihargai, tetapi secara keseluruhan kualitasnya bervariasi.

        - Peningkatan Tajam (Sekitar 1920-an):
            - Terjadi peningkatan drastis dalam rating rata-rata di sekitar tahun 1920-an. Dari sekitar 2.5-3.0, rating rata-rata melonjak cepat hingga di atas 3.5. Ini adalah periode transisi yang signifikan.

        - Era Keemasan/Stabilitas Tinggi (Sekitar 1925 - 1970-an):
            - Setelah peningkatan tajam, rata-rata rating film relatif stabil dan tinggi, sebagian besar berkisar antara 3.5 hingga 4.0. Ada fluktuasi kecil, tetapi secara umum ini menunjukkan periode di mana film-film secara konsisten mendapatkan rating yang tinggi. Ini sering disebut sebagai "Era Keemasan Hollywood" atau periode di mana film-film klasik diproduksi.Puncak sering mencapai atau bahkan sedikit melebihi 4.0.

        - Penurunan Bertahap (Sekitar 1970-an - Akhir 1990-an):
            - Mulai sekitar tahun 1970-an atau awal 1980-an, rata-rata rating mulai menunjukkan tren penurunan bertahap. Meskipun masih relatif tinggi, ada penurunan dari level di atas 3.5 menjadi di bawah 3.5 pada akhir 1990-an.

        - Stabilitas Modern (Akhir 1990-an - 2020):
            - Sejak sekitar akhir 1990-an hingga saat ini (sekitar 2020), rata-rata rating cenderung stabil kembali di sekitar 3.4 hingga 3.6. Ada fluktuasi, tetapi tidak ada penurunan tajam atau peningkatan signifikan seperti di awal abad ke-20.

      Insight:

      - Evolusi Kualitas Film (Persepsi): Grafik ini dapat diinterpretasikan sebagai refleksi persepsi kualitas film dari waktu ke waktu.
            - Periode awal yang berfluktuasi menunjukkan eksperimentasi dan perkembangan format film.
            - Lonjakan di tahun 1920-an mungkin bertepatan dengan peningkatan artistik dan teknis dalam perfilman (misalnya, era film bersuara), yang meningkatkan daya tarik dan kualitas yang dirasakan.
            - Periode stabilitas tinggi di pertengahan abad ke-20 menunjukkan konsistensi dalam produksi film berkualitas tinggi atau terbentuknya standar perfilman.

      - Perubahan Perilaku Rating/Penonton: Penurunan bertahap setelah 1970-an hingga stabilisasi di era modern bisa disebabkan oleh beberapa faktor:
            - Peningkatan Volume Produksi: Seperti yang terlihat dari grafik distribusi tahun rilis sebelumnya, jumlah film yang diproduksi melonjak drastis di era modern. Semakin banyak film dirilis, semakin besar kemungkinan adanya variasi kualitas, yang bisa menurunkan rata-rata.
            - Perubahan Ekspektasi Penonton: Audiens mungkin menjadi lebih kritis seiring waktu, atau standar hiburan telah meningkat karena persaingan dari media lain.
            - Demokratisasi Rating: Dengan kemudahan akses untuk memberikan rating (melalui internet, aplikasi), lebih banyak orang dengan preferensi dan standar yang beragam dapat berpartisipasi, yang bisa meratakan atau sedikit menurunkan rata-rata dibandingkan era ketika rating mungkin lebih didominasi oleh kritikus atau penggemar yang sangat berdedikasi.

        - Dampak Nostalgia/Kanon: Film-film yang dirilis di era "emas" (sekitar 1925-1970) mungkin mendapatkan rating lebih tinggi saat ini karena status "klasik" atau "masterpiece" yang disandang sebagian di antaranya, serta kecenderungan nostalgia. Film-film lama yang buruk mungkin tidak banyak dinilai atau bahkan dilupakan, meninggalkan rata-rata yang lebih tinggi untuk film-film yang bertahan dalam ingatan kolektif.

        - Tren Kualitas Saat Ini: Rata-rata rating yang stabil di era modern (sekitar 3.4-3.6) menunjukkan bahwa meskipun volume produksi tinggi, kualitas film secara umum tetap berada pada tingkat yang solid, tidak terlalu rendah tetapi juga jarang mencapai puncak "sempurna" seperti di era emas.

    * **Rata-rata Rating VS Angka Rating dari Film**
      ![Bivariate](https://github.com/user-attachments/assets/bee8bf0e-566f-4b87-90fc-a93fce51372b)

      Analisis:

        - Konsentrasi Rating pada Jumlah Rating Rendah:
          - Pada sisi kiri grafik (jumlah rating yang sangat rendah, terutama di bawah 10 rating), titik-titik (film) terlihat sangat padat dan tersebar secara vertikal di seluruh rentang rating. Ini menunjukkan bahwa film dengan sedikit rating dapat memiliki rata-rata rating dari yang sangat rendah (0.5) hingga sangat tinggi (5.0).
          - Ini adalah hal yang wajar: sebuah film yang hanya memiliki 1 atau 2 rating bisa dengan mudah mendapatkan rating rata-rata 5.0 (jika kedua rating adalah 5) atau 0.5 (jika ratingnya 0.5).

        - Penyempitan Rentang Rating Seiring Bertambahnya Jumlah Rating:
            - Seiring dengan meningkatnya "Number of Ratings" (bergerak ke kanan pada sumbu X), penyebaran vertikal rating rata-rata mulai menyempit.
            - Film dengan jumlah rating yang sangat tinggi (misalnya, 10^3 atau 10^4 ke atas) cenderung memiliki rata-rata rating yang terkonsentrasi di sekitar nilai tengah hingga tinggi (sekitar 3.0 hingga 4.5). Sangat jarang menemukan film dengan jumlah rating yang sangat banyak tetapi rata-rata ratingnya sangat rendah (< 2.0).
            - Ini menunjukkan efek "hukum angka besar": semakin banyak rating yang diterima sebuah film, semakin representatif rating rata-ratanya terhadap konsensus umum, dan ekstremitas (rating sangat tinggi atau sangat rendah) menjadi lebih sulit dipertahankan.

        - Korelasi Positif yang Lemah:
            - Ada sedikit kecenderungan positif secara keseluruhan; film dengan jumlah rating yang lebih banyak cenderung memiliki rata-rata rating yang sedikit lebih tinggi. Namun, ini bukan korelasi yang kuat dan banyak film dengan jumlah rating menengah masih memiliki rating rata-rata yang bervariasi.

        - Bentuk Kerucut/Corong:
            - Grafik ini memiliki bentuk seperti kerucut atau corong yang terbuka ke kiri (jumlah rating rendah) dan menyempit ke kanan (jumlah rating tinggi). Ini adalah pola yang sangat umum dalam analisis data rating.

      Insight:

        - Keandalan Rating: Jumlah rating yang diterima oleh sebuah film adalah indikator penting dari keandalan rata-rata ratingnya.
            - Rating film dengan sedikit ulasan (cold start problem): Film dengan hanya beberapa rating tidak bisa diandalkan untuk menilai kualitasnya karena ratingnya sangat volatil. Sistem rekomendasi harus berhati-hati saat merekomendasikan film ini.
            - Rating film dengan banyak ulasan: Film dengan ribuan atau puluhan ribu rating cenderung memiliki rata-rata rating yang lebih stabil dan lebih mencerminkan kualitas yang disepakati oleh audiens luas.

        - Filter untuk Kualitas: Jika ingin menemukan film "berkualitas tinggi", sebaiknya tidak hanya mencari film dengan rating rata-rata 5.0, tetapi juga mempertimbangkan jumlah rating yang mendukung rata-rata tersebut. Film dengan rating 4.0 dan 10.000 rating kemungkinan besar "lebih baik" secara objektif daripada film dengan rating 5.0 dan hanya 5 rating.

        - Popularitas vs. Kualitas: Film dengan jumlah rating yang sangat tinggi seringkali adalah film-film yang sangat populer. Fakta bahwa film-film ini cenderung memiliki rata-rata rating di atas 3.0-3.5 menunjukkan bahwa popularitas sering kali berkorelasi dengan setidaknya tingkat kualitas yang baik, bukan hanya sekadar hype.

    * **Rata-rata Rating seiring Waktu berdasarkan Bulan**
      ![Bivariate](https://github.com/user-attachments/assets/d4c1b892-5d8b-47b4-8dcd-9c86ee83ecea)

      Analisis:

        - Volatilitas Tinggi di Awal Periode (Akhir 1996 - Awal 1997):
            - Pada awal grafik, terlihat fluktuasi yang sangat besar dan tajam. Ada lonjakan rating rata-rata yang sangat tinggi, mencapai puncaknya di atas 4.1, diikuti oleh penurunan drastis.
            - Volatilitas ini kemungkinan besar disebabkan oleh jumlah rating yang sangat sedikit pada bulan-bulan awal platform atau dataset, sehingga satu atau dua rating tinggi/rendah dapat sangat mempengaruhi rata-rata.

        - Periode Penurunan Bertahap (Sekitar 1997 - 2004):
            - Setelah volatilitas awal, rata-rata rating menunjukkan tren penurunan umum dari sekitar 3.6-3.7 pada tahun 1997-1998 menjadi di bawah 3.4 pada tahun 2004. Meskipun ada fluktuasi bulanan, tren jangka panjangnya adalah menurun.

        - Periode Peningkatan Bertahap (Sekitar 2004 - 2014):
            - Mulai sekitar tahun 2004, tren mulai berbalik. Rata-rata rating mulai menunjukkan peningkatan yang konsisten, dari titik terendah di bawah 3.4 menjadi di atas 3.7 pada tahun 2014. Ini adalah periode pemulihan dan peningkatan kualitas rata-rata yang dirasakan.

        - Fluktuasi Menurun (Sekitar 2014 - 2019):
            - Setelah mencapai puncaknya sekitar tahun 2014, rata-rata rating kembali menunjukkan sedikit tren penurunan atau stabilisasi dengan fluktuasi yang cukup signifikan, kembali ke kisaran 3.5-3.6 pada akhir data di tahun 2019.

        - Fluktuasi Bulanan yang Konsisten: Di seluruh grafik (setelah volatilitas awal), terlihat adanya fluktuasi bulanan yang cukup reguler. Ini mungkin disebabkan oleh faktor musiman (misalnya, film-film yang dirilis pada musim tertentu atau kebiasaan rating pengguna).

      Insight:

        - Daur Hidup Kualitas Konten/Platform: Perubahan tren rata-rata rating ini bisa mencerminkan evolusi kualitas film yang dirilis dari waktu ke waktu, atau evolusi perilaku pengguna dalam memberikan rating pada platform tersebut.
            - Penurunan di awal mungkin menunjukkan bahwa seiring bertambahnya jumlah pengguna dan konten, rating menjadi lebih beragam dan tidak lagi didominasi oleh "film-film favorit" awal.
            - Peningkatan di pertengahan 2000-an bisa jadi merupakan periode di mana film-film secara umum dipandang lebih baik, atau standar penilaian pengguna sedikit melunak, atau platform berhasil menarik pengguna yang cenderung memberikan rating lebih tinggi.

        - Pengaruh Jumlah Rating per Bulan: Volatilitas yang tinggi di awal adalah indikasi bahwa jumlah rating per bulan masih sangat kecil. Seiring berjalannya waktu, jumlah rating per bulan meningkat (seperti yang terlihat pada grafik distribusi timestamp sebelumnya), yang membuat rata-rata rating menjadi lebih stabil dan representatif, meskipun masih menunjukkan tren.

        - Musiman/Tren Pasar: Fluktuasi bulanan dapat mengisyaratkan adanya musim-musim tertentu di mana film-film berkualitas tinggi dirilis (misalnya, musim penghargaan di akhir tahun) atau periode di mana pengguna lebih aktif memberikan rating.

        - Kualitas Terukur dari Waktu ke Waktu: Meskipun rata-rata rating tidak pernah mencapai level ekstrim seperti di awal data, ada tren jangka panjang yang menunjukkan naik turunnya persepsi kualitas film secara kolektif dari tahun ke tahun.

    * **Distribusi Rata-rata Rating per User**
      ![Bivariate](https://github.com/user-attachments/assets/8a2960b4-85b4-4066-9c11-2c44fe50f9d6)

      Analisis:

        - Distribusi Mirip Kurva Bell (Skewed): Histogram ini menunjukkan distribusi yang kurang lebih berbentuk kurva bell (mirip distribusi normal), tetapi sedikit miring (skewed) ke kiri, atau lebih tepatnya, memiliki ekor yang lebih panjang di sisi kiri (rating rendah). Ini menunjukkan bahwa sebagian besar pengguna cenderung memberikan rating rata-rata yang cukup tinggi.

        - Puncak Konsentrasi (Mode) di Sekitar 3.5 - 4.0:
            - Jumlah pengguna tertinggi (lebih dari 20.000) memiliki rata-rata rating di kisaran 3.5 hingga 4.0.
            - Puncak distribusi terlihat jelas di antara 3.5 dan 4.0, menunjukkan bahwa ini adalah rata-rata rating paling umum yang diberikan oleh pengguna.

        - Mayoritas Pengguna Memberikan Rating Positif:
            - Sebagian besar pengguna memiliki rata-rata rating di atas 3.0.
            - Semakin rendah rata-rata rating, semakin sedikit jumlah penggunanya. Sangat sedikit pengguna yang memiliki rata-rata rating di bawah 2.0. Ini berarti jarang ada pengguna yang secara konsisten memberikan rating yang sangat rendah.

        - Ekor Kiri yang Lebih Panjang: Ada sejumlah kecil pengguna yang memberikan rata-rata rating yang sangat rendah (mendekati 0-1), meskipun jumlahnya sangat kecil dibandingkan dengan pengguna yang memberikan rating lebih tinggi. Ini menunjukkan adanya "kritikus keras" atau pengguna yang sangat selektif.

        - Ekor Kanan yang Lebih Pendek: Jumlah pengguna yang memberikan rating rata-rata mendekati 5.0 (sempurna) juga ada, tetapi lebih sedikit dibandingkan dengan puncak, menunjukkan bahwa tidak banyak pengguna yang selalu memberikan rating sempurna.

      Insight:

        - Pengguna Cenderung Bersikap Positif: Secara umum, pengguna dalam dataset ini cenderung memberikan rating yang positif. Ini bisa berarti bahwa item yang dinilai (misalnya, film) secara umum berkualitas baik, atau ada kecenderungan alami pengguna untuk tidak terlalu kritis atau memberikan rating yang lebih tinggi.

        - Identifikasi Jenis Pengguna:
            - Mayoritas (sekitar 3.5-4.0): Ini adalah pengguna "rata-rata" yang memberikan rating wajar, mungkin cenderung sedikit positif.
            - Kritikus Keras (rata-rata rendah): Kelompok kecil pengguna ini secara konsisten memberikan rating yang rendah, mungkin karena standar yang sangat tinggi atau memang tidak menyukai sebagian besar item yang dinilai.
            - "Fans" (rata-rata tinggi mendekati 5.0): Ada juga kelompok kecil pengguna yang secara konsisten memberikan rating sangat tinggi, mungkin karena mereka hanya menilai film yang sangat mereka sukai atau memiliki toleransi yang tinggi.

    * **Rata-rata Jumlah Genre per Film berdasarkan Tahun Rilis**
      ![Bivariate](https://github.com/user-attachments/assets/225eba08-4160-4879-a023-6cef0d5e6d90)

      Analisis:

        - Periode Awal (Pra-1900): Pada awal dataset, rata-rata jumlah genre per film adalah 1.0. Ini berarti film-film yang sangat awal kemungkinan besar hanya diberi satu genre saja, atau mungkin kategorisasi genre belum sekompleks sekarang.

        - Peningkatan Volatilitas (Sekitar 1900 - 1920):
            - Sekitar awal abad ke-20, grafik menunjukkan peningkatan yang signifikan dan sangat fluktuatif dalam rata-rata jumlah genre. Ada lonjakan tajam hingga di atas 1.8, diikuti oleh penurunan, dan kemudian naik turun lagi.
            - Volatilitas tinggi di periode ini bisa jadi karena jumlah film yang sedikit per tahun, atau sistem kategorisasi genre yang belum matang atau konsisten pada masa itu.

        - Tren Kenaikan dan Puncak (Sekitar 1920 - 1940-an):
            - Setelah tahun 1920-an, ada tren kenaikan yang lebih stabil dalam rata-rata jumlah genre.
            - Puncak tertinggi terjadi sekitar akhir 1930-an hingga awal 1940-an, di mana rata-rata jumlah genre per film mencapai lebih dari 2.0 (yaitu, rata-rata setiap film memiliki dua genre atau lebih).

        - Periode Fluktuatif yang Menurun (1940-an - Awal 1990-an):
            - Setelah puncaknya di era 1940-an, rata-rata jumlah genre per film menunjukkan tren penurunan umum, meskipun dengan fluktuasi yang signifikan. Rata-rata ini bergerak turun dari sekitar 2.0 menjadi sekitar 1.7-1.8.

        - Kenaikan dan Stabilisasi Kembali (Awal 1990-an - Awal 2000-an):
            - Sekitar awal 1990-an, rata-rata jumlah genre mulai menunjukkan tren kenaikan lagi, mencapai puncak lokal di awal 2000-an, kembali di atas 2.0.

        - Penurunan Akhir (Sekitar 2010 - 2020):
            - Setelah sekitar tahun 2010, ada tren penurunan yang cukup jelas dalam rata-rata jumlah genre per film, turun dari sekitar 2.0 menjadi sekitar 1.6-1.7 pada tahun 2020.

      Insight:

        - Evolusi Kategorisasi Genre: Grafik ini menunjukkan evolusi bagaimana film dikategorikan atau seberapa kompleksitas genre film dari waktu ke waktu.
            - Awalnya, film mungkin sangat sederhana sehingga hanya butuh satu genre, atau definisi genre belum luas.
            - Peningkatan di awal abad ke-20 mungkin mencerminkan perkembangan narasi film yang lebih kompleks, yang membutuhkan lebih dari satu label genre.

        - Tren Hibridisasi Genre:
            - Puncak di era 1930-an/1940-an dan awal 2000-an mungkin mengindikasikan periode di mana film-film hibrida genre (misalnya, Action-Comedy, Sci-Fi Thriller) menjadi lebih umum atau lebih sering diklasifikasikan dengan multiple genre.
            - Penurunan di tahun-tahun terakhir (setelah 2010) bisa berarti ada pergeseran kembali ke film dengan genre yang lebih "murni" atau sistem klasifikasi genre yang lebih ketat, atau mungkin ada tren untuk membatasi jumlah tag genre yang diberikan.
    ### Multivariate Analysis EDA

    * **Heatmap Korelasi variabel Numerik**
      ![Multivariate](https://github.com/user-attachments/assets/1233bf60-1caa-468b-81af-c91e35bd9792)

      Analisis dan Insight:

        - Korelasi Diri (Diagonal):
            - rating dengan rating: 1.00
            - timestamp dengan timestamp: 1.00
            - year dengan year: 1.00
      
      
        Ini adalah hal yang wajar karena setiap variabel berkorelasi sempurna dengan dirinya sendiri.

        - Korelasi rating dengan Variabel Lain:
            - rating dengan timestamp: 0.01 (sangat mendekati nol).
            - rating dengan year: -0.08 (sangat mendekati nol, sedikit negatif).
            - Insight: Ini menunjukkan bahwa rating film hampir tidak memiliki korelasi dengan timestamp (kapan rating diberikan) atau year (tahun rilis film). Artinya, seiring berjalannya waktu, rating rata-rata film (atau rating individual) tidak secara signifikan cenderung naik, turun, atau berubah secara linier. Ini penting karena:
                - Rating film lama tidak secara inheren lebih baik atau lebih buruk daripada film baru hanya karena usianya.
                - Perilaku pemberi rating tidak menunjukkan bias temporal yang kuat (yaitu, orang tidak tiba-tiba menjadi lebih pelit atau lebih murah hati dengan rating mereka seiring waktu).

        - Korelasi timestamp dengan Variabel Lain:
            - timestamp dengan rating: 0.01 (sudah dibahas).
            - timestamp dengan year: 0.32 (positif, moderat).
            - Insight: Ada korelasi positif moderat antara timestamp (kapan rating diberikan) dan year (tahun rilis film). Ini masuk akal karena:
                - Semakin baru tahun rilis film, semakin mungkin film tersebut dinilai pada timestamp yang lebih baru juga.
                - Orang cenderung menilai film yang baru dirilis atau film yang sedang populer saat ini. Ini juga bisa berarti bahwa pengguna cenderung memberikan rating secara aktif untuk film-film dari era mereka sendiri atau yang baru mereka tonton.

    * **Rata-rata Rating berdasarkan Tahun Rilis untuk Genre Populer**
      ![Multivariate](https://github.com/user-attachments/assets/3925ce4a-0230-4d8c-9092-0c355f4ac3e4)

      Insight:

        - Perbedaan Kualitas yang Dirasakan antar Genre: Meskipun semua genre mengalami pasang surut dalam rating rata-rata mereka sepanjang sejarah, ada perbedaan konsisten dalam bagaimana genre-genre tertentu dipersepsikan.
            - Drama seringkali mempertahankan rata-rata rating yang relatif tinggi, menunjukkan bahwa film-film drama sering diterima dengan baik oleh penonton.
            - Comedy secara konsisten terlihat di bagian bawah dari genre-genre populer ini, mengindikasikan bahwa meskipun film komedi mungkin populer dalam jumlah (seperti yang terlihat di grafik frekuensi genre sebelumnya), kualitas yang dirasakan (berdasarkan rating) tidak selalu setinggi genre lain. Mungkin standar humor sangat subjektif atau kualitas komedi sangat bervariasi.
            - Thriller menunjukkan fluktuasi, tetapi seringkali berada di tengah hingga atas, menunjukkan genre ini umumnya menghadirkan pengalaman yang memuaskan.

        - Dampak Tren Industri dan Preferensi Audiens: Perubahan rata-rata rating per genre dari waktu ke waktu mencerminkan bagaimana selera penonton berubah, bagaimana genre-genre berkembang, dan mungkin juga bagaimana kualitas produksi genre tertentu berfluktuasi.

        - Fokus pada Genre: Bagi pembuat film atau platform konten, analisis ini dapat membantu mengidentifikasi genre mana yang cenderung dihargai oleh penonton dari waktu ke waktu. Misalnya, jika ingin membuat film yang cenderung mendapatkan rating tinggi, drama atau thriller mungkin pilihan yang lebih "aman" dibandingkan komedi.


---

## **Data Preparation**

Tahap Data Preparation adalah langkah krusial untuk mengubah data mentah menjadi format yang sesuai dan optimal untuk proses *modeling* sistem rekomendasi, disini saya melakukan efisiensi dan pembatasan dataset menjadi 15000 records saja dan memuat kembali datasetnya. Teknik-teknik yang diterapkan dan alasannya adalah sebagai berikut:

1.  **Pengisian Nilai Kosong pada Genre (`movies['genres'].fillna('Unknown')`)**
    * **Teknik:** Imputasi dengan string kosong.
    * **Proses:** Mengganti setiap nilai `NaN` (Not a Number) dalam kolom `genres` di DataFrame `movies` dengan string kosong (`''`).
    * **Alasan:** `TfidfVectorizer` (yang digunakan untuk Content-Based Filtering) mengharapkan input berupa string. Nilai `NaN` dapat menyebabkan error atau perilaku yang tidak terduga. Menggantinya dengan string kosong memastikan bahwa setiap entri dapat diproses.

2.  **TF-IDF Vectorization pada Genre**
    * **Teknik:** *Text Vectorization* menggunakan TF-IDF (Term Frequency-Inverse Document Frequency).
    * **Proses:**
        * `TfidfVectorizer(token_pattern=r"[^|]+")` diinisialisasi untuk memecah genre yang dipisahkan oleh `|` (pipa) menjadi token terpisah.
        * `tfidf.fit_transform(movies['genres'])` mempelajari kosakata genre unik (`fit`) dan kemudian mengubah setiap daftar genre film menjadi representasi numerik (vektor TF-IDF) (`transform`). Hasilnya adalah `tfidf_matrix`.
    * **Alasan:**
        * **Kuantifikasi Teks:** Mengubah data teks (genre) menjadi format numerik yang dapat digunakan oleh algoritma machine learning.
        * **Pembobotan Relevansi:** TF-IDF memberikan bobot yang lebih tinggi pada genre yang unik dan penting untuk suatu film (muncul sering di film tersebut tetapi jarang di film lain), membantu mengidentifikasi karakteristik genre yang paling membedakan.

3.  **Perhitungan Cosine Similarity**
    * **Teknik:** Penghitungan metrik kesamaan.
    * **Proses:** Menggunakan `cosine_similarity(tfidf_matrix, tfidf_matrix)` untuk menghitung matriks kesamaan kosinus antar setiap pasangan film berdasarkan vektor TF-IDF genre mereka. Hasilnya adalah `cosine_sim`.
    * **Alasan:** Matriks ini adalah inti dari Content-Based Filtering. Ini mengukur seberapa mirip profil genre antara dua film, yang kemudian digunakan untuk merekomendasikan film serupa.

4.  **Pemetaan Judul ke Indeks (`indices`)**
    * **Teknik:** Pembuatan kamus/peta.
    * **Proses:** Membuat `pandas.Series` (`indices`) di mana indeksnya adalah judul film dan nilainya adalah indeks numerik film dalam DataFrame `movies`. `.drop_duplicates()` digunakan untuk menangani judul film yang mungkin sama.
    * **Alasan:** Memungkinkan pencarian cepat indeks film berdasarkan judulnya, yang diperlukan untuk mengakses skor kesamaan dalam `cosine_sim` dan informasi film dalam `movies_df`.

5.  **Label Encoding untuk User ID dan Movie ID**
    * **Teknik:** *Categorical Feature Encoding* menggunakan `LabelEncoder`.
    * **Proses:**
        * `user_enc = LabelEncoder()` dan `movie_enc = LabelEncoder()` diinisialisasi.
        * `ratings['user'] = user_enc.fit_transform(ratings['userId'])` dan `ratings['movie'] = movie_enc.fit_transform(ratings['movieId'])` mengubah ID pengguna dan ID film asli menjadi label numerik kontinu dari 0 hingga $N-1$ (untuk pengguna) atau $M-1$ (untuk film).
    * **Alasan:**
        * **Persyaratan Model:** Model Neural Network, khususnya *embedding layers*, memerlukan input integer yang berurutan dan dimulai dari 0 sebagai indeks. ID asli pengguna atau film seringkali tidak berurutan atau terlalu besar.
        * **Efisiensi:** Menghemat memori dan mempercepat komputasi dalam model dengan menggunakan indeks yang lebih kecil dan padat.

6.  **Skalasi Min-Max pada Rating (`rating_scaled`)**
    * **Teknik:** Normalisasi data.
    * **Proses:** Mengubah nilai rating asli (`ratings['rating']`) ke rentang antara 0 dan 1 menggunakan rumus: `X_scaled = (X - X_min) / (X_max - X_min)`.
    * **Alasan:**
        * **Persyaratan Model Neural Network:** Banyak algoritma ML, terutama jaringan saraf, bekerja lebih baik ketika fitur input dan target output dinormalisasi. Ini membantu proses optimisasi (mencegah *exploding/vanishing gradients*) dan mempercepat konvergensi.
        * **Kompatibilitas Fungsi Aktivasi:** Output layer model menggunakan fungsi aktivasi `sigmoid` yang menghasilkan nilai antara 0 dan 1, sehingga normalisasi target ke rentang yang sama sangat penting.

7.  **Pembagian Data (Train-Test Split)**
    * **Teknik:** Pemisahan dataset.
    * **Proses:** Menggunakan `train_test_split(x, y_scaled, test_size=0.2, random_state=42)` untuk membagi data pasangan pengguna-film (`x`) dan rating yang diskalakan (`y_scaled`) menjadi set pelatihan (80%) dan set pengujian (20%).
    * **Alasan:**
        * **Evaluasi Tidak Bias:** Memastikan bahwa model dievaluasi pada data yang belum pernah dilihatnya selama pelatihan. Ini memberikan estimasi yang realistis tentang bagaimana model akan bekerja pada data baru.
        * **Pencegahan Overfitting:** Memungkinkan pemantauan kinerja model pada data validasi selama pelatihan, yang krusial untuk mendeteksi dan mencegah *overfitting*.

---

## **Modeling and Result**

Kami mengembangkan sistem rekomendasi dengan dua pendekatan berbeda untuk menyelesaikan permasalahan yang diuraikan di atas: Content-Based Filtering dan Collaborative Filtering berbasis Neural Network.

### **1. Content-Based Filtering (CBF)**

**Penjelasan Sistem Rekomendasi:**
Sistem rekomendasi Content-Based Filtering merekomendasikan film kepada pengguna berdasarkan kemiripan atribut konten film. Dalam kasus ini, atribut utamanya adalah genre. Ketika pengguna menyukai sebuah film, sistem akan mencari film lain yang memiliki profil genre serupa menggunakan representasi TF-IDF dan menghitung kesamaan menggunakan Cosine Similarity.

**Implementasi:**
Fungsi `get_recommendations` mengambil judul film sebagai input, mencari indeksnya, menghitung skor kesamaan kosinus dengan semua film lain berdasarkan genre, mengurutkannya, dan mengembalikan Top-N film teratas yang paling mirip.

**Kelebihan Pendekatan CBF:**
* **Tidak Memerlukan Data Pengguna Lain (Cold Start Item):** Ini adalah keuntungan besar. CBF dapat merekomendasikan item baru bahkan jika item tersebut belum pernah dinilai atau dibeli oleh pengguna lain. Selama item tersebut memiliki atribut, ia dapat dimasukkan dalam rekomendasi. Ini sangat berguna untuk startup atau platform dengan konten baru yang terus ditambahkan.
* **Transparansi dan Penjelasan (Explainability):** Lebih mudah untuk menjelaskan mengapa sebuah item direkomendasikan. Sistem dapat mengatakan, "Kami merekomendasikan film ini karena Anda menyukai film X, dan film ini memiliki genre yang sama (Action, Sci-Fi) dan aktor yang sama (Tom Cruise)." Ini meningkatkan kepercayaan pengguna.
* **Fleksibilitas untuk Preferensi Niche/Unik:** CBF sangat baik dalam merekomendasikan item yang sesuai dengan preferensi yang sangat spesifik atau niche dari pengguna. Jika pengguna memiliki selera yang sangat unik, CBF dapat terus mencari item dengan atribut yang sangat spesifik tersebut.
* **Tidak Terdampak Masalah Cold Start Pengguna (hingga taraf tertentu):** Meskipun tidak sepenuhnya kebal, jika seorang pengguna baru telah berinteraksi dengan beberapa item, bahkan jika jumlahnya sedikit, CBF dapat mulai membangun profil preferensi mereka dan memberikan rekomendasi yang relevan.

**Kekurangan Pendekatan CBF:**
* **Over-specialization (Terlalu Spesialis):** Ini adalah kelemahan terbesar CBF. Karena hanya merekomendasikan item yang mirip dengan apa yang sudah disukai pengguna, sistem cenderung merekomendasikan item yang sangat mirip dengan apa yang telah dikonsumsi pengguna di masa lalu. Pengguna mungkin tidak akan menemukan item baru yang menarik di luar profil preferensi mereka yang sudah ada (tidak ada "serendipity" atau kejutan yang menyenangkan).
  - Contoh: Jika Anda hanya menonton film Action, Anda hanya akan direkomendasikan film Action, dan tidak akan pernah diperkenalkan pada genre lain yang mungkin Anda sukai, seperti Thriller Komedi.
* **Ketergantungan pada Ketersediaan Atribut Item:** Kualitas rekomendasi sangat bergantung pada kekayaan dan akurasi atribut item. Jika atribut item terbatas, tidak akurat, atau tidak lengkap, maka rekomendasi yang dihasilkan mungkin buruk.
  - Contoh: Jika semua film hanya memiliki atribut "Judul" dan "Tahun Rilis", sulit untuk menemukan kesamaan yang berarti.
* **Masalah Cold Start Pengguna (Tingkat Awal):** Meskipun lebih baik daripada Collaborative Filtering murni dalam hal ini, CBF masih membutuhkan beberapa interaksi awal dari pengguna untuk membangun profil preferensi yang cukup. Jika seorang pengguna baru belum berinteraksi dengan item apa pun, sistem tidak memiliki data untuk membangun profil dan tidak dapat merekomendasikan apa pun.
* **Kesulitan dalam Membangun Profil Pengguna dari Interaksi Implisit:** Jika data interaksi pengguna hanya berupa "klik" atau "lihat" (interaksi implisit), bukan rating eksplisit, membangun profil preferensi yang akurat bisa jadi tantangan.
* **Biaya Pemrosesan Atribut:** Untuk dataset yang sangat besar dengan banyak item dan atribut yang kompleks (misalnya, konten gambar atau video), memproses dan mengekstrak fitur atribut bisa sangat mahal secara komputasi.

**Top-N Recommendation Output:**
| Movie ID | Judul Film                                                                 |
|----------|-----------------------------------------------------------------------------|
| 60       | Indian in the Cupboard, The (1995)                                          |
| 126      | NeverEnding Story III, The (1994)                                           |
| 1009     | Escape to Witch Mountain (1975)                                             |
| 2043     | Darby O'Gill and the Little People (1959)                                   |
| 2093     | Return to Oz (1985)                                                         |
| 2161     | NeverEnding Story, The (1984)                                               |
| 2162     | NeverEnding Story II: The Next Chapter, The (1990)                          |
| 2399     | Santa Claus: The Movie (1985)                                               |
| 4896     | Harry Potter and the Sorcerer's Stone (a.k.a. Harry Potter and the Philosopher's Stone) (2001) |
| 31447    | Magic in the Water (1995)                                                   |


### **2. Collaborative Filtering (CF)**

**Penjelasan Sistem Rekomendasi**
Sistem rekomendasi **Collaborative Filtering berbasis Neural Network (RecommenderNet)** memprediksi rating yang mungkin diberikan seorang pengguna untuk sebuah film, dengan mempelajari pola interaksi kolektif antara pengguna dan film. 

Model ini menggunakan **representasi tersembunyi (embeddings)** untuk setiap pengguna dan film, serta **bias**, untuk menangkap preferensi dan karakteristik yang tidak terlihat.

**Implementasi**
Model `RecommenderNet` dibuat dengan:
- **Embedding layers** untuk pengguna dan film (`embedding_size=50`)
- **Bias layers** untuk pengguna dan film
- Dalam metode `call`, model menghitung **dot product** antara vektor embedding pengguna dan film, ditambahkan bias, lalu dilewatkan melalui **fungsi aktivasi sigmoid** untuk memprediksi rating yang diskalakan.
- Model dilatih menggunakan **optimizer Adam** dan **fungsi loss MSE (Mean Squared Error)**.

Fungsi `get_top_n_recommendations_cf` kemudian menggunakan model yang sudah dilatih ini untuk memprediksi rating film-film yang belum ditonton oleh pengguna, kemudian meranking dan menyajikan **Top-N rekomendasi**.

Berikut adalah parameter yang digunakan pada saat melatih model:
- **x:** Data input yang digunakan untuk melatih model. Dalam konteks model rekomendasi ini, x adalah array NumPy yang berisi pasangan ID pengguna yang dienkode (user) dan ID film yang dienkode (movie) untuk setiap interaksi rating. Ini adalah fitur yang akan dipelajari model untuk membuat prediksi.

- **y:** Data target (label) yang sesuai dengan data input x, digunakan sebagai ground truth (nilai sebenarnya) saat melatih model. Untuk model ini, y adalah rating film yang telah diskalakan (y_train_scaled) ke rentang antara 0 dan 1. Model berusaha memprediksi nilai-nilai ini seakurat mungkin.

- **batch_size:** Menentukan berapa banyak sampel data dari x dan y yang akan diproses dalam satu kelompok (batch) sebelum bobot internal model diperbarui.Ukuran batch 32 adalah umum digunakan karena memberikan keseimbangan yang baik antara efisiensi komputasi dan stabilitas gradien.

- **epochs:** Menentukan berapa kali seluruh dataset pelatihan (x_train, y_train_scaled) akan dilewatkan sepenuhnya ke model.Disini diset 100, yang merupakan batas atas. Artinya, model akan memproses seluruh data pelatihan hingga 100 kali.

- **verbose:** Mengontrol seberapa banyak informasi log yang akan ditampilkan selama proses pelatihan.verbose=1 menampilkan progress bar yang memberikan indikasi kemajuan pelatihan untuk setiap epoch, serta nilai metrik (seperti loss dan mae) di akhir setiap epoch.

- **validation_data:** Sebuah tuple yang berisi data (x_test, y_test_scaled) yang digunakan untuk mengevaluasi kinerja model di akhir setiap epoch. Data ini adalah subset data yang tidak digunakan untuk melatih model. Tujuannya adalah untuk memantau overfitting. Dengan membandingkan kinerja model pada data pelatihan dan data validasi, kita dapat melihat apakah model mulai menghafal data pelatihan daripada belajar pola yang dapat digeneralisasi.

- **callbacks:** Sebuah list (daftar) berisi objek callback yang ingin dijalankan selama berbagai tahap pelatihan. Callbacks adalah fungsi khusus yang dapat disuntikkan ke dalam siklus pelatihan untuk melakukan tindakan tertentu.


Hasil yang didapat dari training model tersebut yaitu:
- **Loss data training (MSE) :** 0.0265
- **MAE data training :** 0.1218
- **Loss data test (MSE) :** 0.0387
- **MAE data test :** 0.`1528  

**Kelebihan Pendekatan CF**
- **Menemukan Serendipity**: Mampu merekomendasikan film yang tidak memiliki atribut konten yang mirip, tetapi disukai oleh pengguna lain dengan selera serupa.
- **Tidak Bergantung pada Atribut Item**: Tidak memerlukan analisis atribut item secara eksplisit, sehingga fleksibel untuk berbagai jenis item.
- **Mempelajari Pola Kompleks**: Mampu mempelajari pola interaksi yang kompleks dan tersembunyi yang sulit terlihat oleh metode Content-Based.

**Kekurangan Pendekatan CF**
- **Cold Start Problem (Pengguna/Item Baru)**: Sulit merekomendasikan item atau pengguna baru yang belum memiliki riwayat interaksi.
- **Sparsity Problem**: Kinerja dapat menurun pada dataset yang sangat jarang, di mana sebagian besar pengguna hanya berinteraksi dengan sedikit item.
- **Bias Popularitas**: Cenderung merekomendasikan item yang populer karena memiliki lebih banyak data interaksi.



**Top-N Recommendation:**
Pada saat melakukan pengetesan model menggunakan user random dengan kode:
`example_user_id = ratings['userId'].sample(1, random_state=42).iloc[0]`
Kode ini digunakan untuk mengambil secara acak satu userId dari dataset ratings dan menyimpannya ke dalam variabel example_user_id menghasilkan output sebagai berikut:

Top-10 Rekomendasi untuk User ID: 84

| movieId | Title                                                                 | Genres                                    | Predicted Rating |
|---------|-----------------------------------------------------------------------|-------------------------------------------|------------------|
| 50      | Usual Suspects, The (1995)                                            | Crime\|Mystery\|Thriller                  | 4.187977         |
| 527     | Schindler's List (1993)                                               | Drama\|War                                | 4.175990         |
| 1197    | Princess Bride, The (1987)                                            | Action\|Adventure\|Comedy\|Fantasy\|Romance | 4.125735       |
| 1704    | Good Will Hunting (1997)                                              | Drama\|Romance                            | 4.058734         |
| 457     | Fugitive, The (1993)                                                  | Thriller                                  | 4.021883         |
| 953     | It's a Wonderful Life (1946)                                          | Children\|Drama\|Fantasy\|Romance         | 3.974468         |
| 150     | Apollo 13 (1995)                                                      | Adventure\|Drama\|IMAX                    | 3.970052         |
| 1198    | Raiders of the Lost Ark (Indiana Jones and the Raiders of the Lost Ark) (1981) | Action\|Adventure             | 3.962517         |
| 1291    | Indiana Jones and the Last Crusade (1989)                             | Action\|Adventure                         | 3.962214         |
| 68237   | Moon (2009)                                                           | Drama\|Mystery\|Sci-Fi\|Thriller          | 3.961492         |


## Evaluation

### **Content Based Filtering (CBF)**

Untuk Content-Based Filtering, evaluasi dilakukan secara **inspeksi visual**, yang secara konseptual menerapkan prinsip **Precision@N**.

**Metrik Evaluasi yang Digunakan:**

* **Precision@N (Presisi pada N Rekomendasi Teratas):**
    * **Tujuan:** Mengukur proporsi item relevan di antara N item teratas yang direkomendasikan. Ini memberikan gambaran seberapa banyak rekomendasi yang benar-benar sesuai dengan kriteria relevansi yang ditentukan.
    * **Rumus Konseptual:**
$$\text{Precision} = \frac{TP}{TP + FP}$$
        
        Di mana:
        * **TP (True Positive):** Jumlah item relevan yang berhasil direkomendasikan dalam N teratas.
        * **FP (False Positive):** Jumlah item yang direkomendasikan dalam N teratas, tetapi sebenarnya tidak relevan.
        * **N:** Jumlah rekomendasi teratas yang ditampilkan (dalam kasus ini, 10).
    * **Cara Metrik Tersebut Bekerja dalam Konteks Ini:**
        Dalam evaluasi ini, "relevansi" sebuah film ditentukan oleh kemiripan genrenya dengan film referensi. Inspeksi visual berfungsi sebagai cara kualitatif untuk mengukur TP dan FP ini.

**Hasil Proyek Berdasarkan Metrik Evaluasi (Inspeksi Visual):**
Melalui inspeksi visual pada daftar Top-10 rekomendasi yang dihasilkan oleh model CBF untuk film-film referensi (misalnya, 'Jumanji (1995)' dan 'Toy Story (1995)'), hasilnya menunjukkan tingkat relevansi genre yang tinggi. Film-film yang direkomendasikan secara konsisten memiliki genre yang sangat mirip dengan film referensinya.

  - Contoh: Untuk 'Jumanji' (Adventure|Children|Fantasy), rekomendasi Top-10 banyak berisi film-film dengan genre yang sama atau serupa (seperti 'The Indian in the Cupboard' yang juga Adventure|Children|Fantasy). Demikian pula untuk 'Toy Story', rekomendasi banyak mengandung genre 'Animation' dan 'Children'.
  - Interpretasi: Secara kualitatif, observasi ini mengindikasikan bahwa sistem CBF memiliki Precision yang tinggi. Ini berarti sebagian besar rekomendasi yang disajikan adalah "True Positive" berdasarkan kesamaan genre, menunjukkan bahwa sistem efektif dalam menemukan item serupa berdasarkan atribut kontennya.

### **Collaborative Filtering (CBF)**

Evaluasi untuk model Collaborative Filtering difokuskan pada akurasi prediksi rating yang dihasilkan oleh Neural Network. Model dievaluasi pada data uji yang belum pernah dilihat selama pelatihan untuk mengukur kemampuan generalisasinya.
Metrik Evaluasi yang Digunakan:

- Mean Squared Error (MSE):
    - Tujuan: Mengukur rata-rata dari kuadrat perbedaan antara rating yang diprediksi model dan rating asli. MSE sangat sensitif terhadap kesalahan besar karena mengkuadratkan perbedaannya.
Rumus: $$\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$ Di mana: $n$ adalah jumlah prediksi, $y_i$ adalah nilai rating asli, dan $\hat{y}_i$ adalah nilai rating yang diprediksi model.
Cara Metrik Tersebut Bekerja: Model berusaha meminimalkan nilai MSE ini selama pelatihan.

- Mean Absolute Error (MAE):
  - Tujuan: Mengukur rata-rata dari nilai absolut perbedaan antara rating yang diprediksi model dan rating asli. MAE memberikan gambaran yang lebih intuitif tentang rata-rata "jarak" kesalahan prediksi.
  - Rumus: $$\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$$ Di mana: $n$ adalah jumlah prediksi, $y_i$ adalah nilai rating asli, dan $\hat{y}_i$ adalah nilai rating yang diprediksi model.
  - Cara Metrik Tersebut Bekerja: MAE lebih mudah diinterpretasikan karena satuannya sama dengan skala rating. Ini menunjukkan rata-rata besar kesalahan tanpa memperhatikan arahnya.  

- Visualisasi Proses Pelatihan (Loss & MAE per epoch):
  ![plot](https://github.com/user-attachments/assets/0c566b33-3044-44c6-aa22-3bac013fcb58)

  Interpretasi Plot:
  Plot menunjukkan penurunan cepat pada kurva Train Loss/MAE dan Validation Loss/MAE di awal pelatihan, menandakan pembelajaran yang efisien. Namun, setelah sekitar 15-20 epoch, Validation Loss dan Validation MAE mulai mendatar atau sedikit meningkat, sementara Train Loss/MAE terus menurun. Ini adalah indikasi overfitting. Penggunaan Early Stopping sangat krusial di sini, memastikan pelatihan berhenti pada titik optimal (ketika kinerja validasi terbaik dicapai) dan mencegah model menghafal data pelatihan secara berlebihan.

Hasil Metrik:

  - Test Loss (MSE): 0.0383
  - Test Mean Absolute Error (MAE): 0.1526
  - Test MAE (skala rating asli): 0.6866

Interpretasi Hasil:

  - Nilai Test Loss (MSE) sebesar 0.0383 pada skala rating 0-1 menunjukkan bahwa model berhasil meminimalkan kesalahan kuadratiknya pada data yang tidak terlihat, yang merupakan indikasi kinerja optimasi yang baik.
  - Test MAE (0.1526 pada skala 0-1) berarti rata-rata, prediksi model meleset sekitar 0.1526 unit dari rating sebenarnya pada skala yang dinormalisasi.
  - Yang paling penting untuk interpretasi praktis adalah Test MAE (skala rating asli) sebesar 0.6866. Angka ini berarti bahwa, secara rata-rata, prediksi rating film oleh model meleset sekitar 0.6866 poin dari rating asli yang diberikan pengguna (pada skala rating 0.5 hingga 5.0). Ini menunjukkan tingkat akurasi yang cukup baik dan dapat diterima untuk sebuah sistem rekomendasi, mengindikasikan bahwa model efektif dalam memprediksi preferensi pengguna.

## Conclusion

**Kesimpulan**

Proyek ini telah berhasil mengembangkan dan mengevaluasi sistem rekomendasi film hibrida yang menggabungkan kekuatan pendekatan Content-Based Filtering (CBF) dan Collaborative Filtering (CF). Tujuan utama adalah untuk membantu pengguna menavigasi katalog film yang luas dan menemukan rekomendasi yang dipersonalisasi dan relevan, sekaligus mengatasi keterbatasan yang melekat pada metode rekomendasi tunggal.

Berdasarkan hasil evaluasi:

- Content-Based Filtering (CBF) menunjukkan efektivitas tinggi dalam merekomendasikan film berdasarkan kemiripan konten.
    - Evaluasi kualitatif melalui inspeksi visual mengonfirmasi bahwa rekomendasi CBF sangat selaras dengan genre film referensi. Misalnya, film-film seperti 'Jumanji' atau 'Toy Story' menghasilkan rekomendasi yang secara konsisten memiliki genre yang sangat mirip (Adventure|Children|Fantasy atau Animation|Children|Comedy).
    - Hal ini secara konseptual mengindikasikan Precision yang tinggi, di mana sebagian besar item yang direkomendasikan adalah "True Positive" berdasarkan kesamaan genre. Ini membuktikan bahwa sistem CBF efektif dalam menemukan item-item serupa berdasarkan karakteristik intrinsiknya.

Collaborative Filtering (CF) berbasis Neural Network (RecommenderNet) berhasil memprediksi preferensi rating pengguna dengan akurasi yang baik.

   - Model CF dilatih menggunakan embedding pengguna dan film, serta berhasil meminimalkan kesalahan prediksi pada data uji.
   - Metrik evaluasi menunjukkan:
       - Test Loss (MSE) sebesar 0.0383 (pada skala rating 0-1).
       - Test Mean Absolute Error (MAE) sebesar 0.1526 (pada skala rating 0-1).
       - Yang terpenting, Test MAE (skala rating asli) sebesar 0.6866. Angka ini menunjukkan bahwa, rata-rata, prediksi rating model meleset kurang dari 0.7 poin dari rating asli pengguna pada skala 0.5-5.0. Ini adalah indikator akurasi yang solid dan dapat diterima untuk sistem rekomendasi.
   - Analisis plot pelatihan (Loss dan MAE per Epoch) juga mengonfirmasi bahwa model belajar dengan efisien meskipun menunjukkan tanda-tanda overfitting yang berhasil ditangani oleh mekanisme Early Stopping, memastikan model yang dihasilkan memiliki kemampuan generalisasi yang optimal.

Dengan akurasi prediksi rating yang terbukti dan kemampuan untuk menyediakan rekomendasi berbasis konten, sistem ini menjadi alat yang efektif untuk meningkatkan keterlibatan pengguna dan mempermudah penemuan film di platform.

---

## **References**

* [Pedregosa, F., et al. (2011). Scikit-learn: Machine Learning in Python. *Journal of Machine Learning Research*, 12, 2825-2830.](https://scikit-learn.org/stable/)
* [ Sarwar, B., Karypis, G., Konstan, J., & Riedl, J.(2001). Item-Based Collaborative Filtering Recommendation Algorithms. *Proceedings of the 10th International Conference on World Wide Web*, 285-295. ](https://doi.org/10.1145/371920.372071)
