# MLT2-NOVIAHERLINDAMARIUS
m496y1045
M04

## Project overview

Perusahaan *e-commerce* bernama sepatu nike, membutuhkan sistem rekomendasi produk sepatu untuk *platform* *e-commerce* mereka. Kondisi *platform* masih baru dan penggunannya masih sedikit, sehingga untuk mengatasi kondisi *cold start* tersebut sistem rekomendasi yang sesuai yaitu *content-based filtering*. Sistem rekomendasi yang dibuat akan merokemendasikan produk sepatu Nike berdasarkan kategori atau subkategorinya.


M.Rizki Putra Utama (2019)  melakukan penelitian tentang *E-Commerce Recommender System* (*Content-Based Filtering*) menggunakan metode *Gradient boosting* dan menghasilkan rata-rata *precision* untuk setiap kategori di antara 67.03 - 73.14% [[1]](http://strategi.it.maranatha.edu/index.php/strategi/article/view/17)  Sejalan dengan hal tersebut, solusi yang ditawarkan pada kasus ini adalah sistem rekomendasi (*content based filtering*) dengan menggunakan metode *Cosine Similarity* dan *Euclidean Similarity* dengan metrik yang digunakan metrik *precision*.

## Business Understanding
### Problem Statements


Bagaimana cara membuat sistem rekomendasi dengan metode terbaik untuk rekomendasi produk sepatu berdasarkan kategori atau subkategorinya?


### Goals

Mengetahui cara membuat sistem rekomendasi dengan metode terbaik untuk rekomendasi produk sepatu Nike berdasarkan kategori atau subkategorinya.

### Solution Statements

Menawarkan solusi sistem rekomendasi dengan jenis *content based filtering*  yang menghasilkan rekomendasi produk sepatu berdasarkan kategori atau subkategorinya. Untuk mendapatkan solusi terbaik, akan digunakan dua model (metode) yang berbeda yaitu *Cosine Similarity* dan *Euclidean Similarity*.


## Data Understanding

Tabel 1. Informasi Dataset

| | Keterangan |
|---|---|
| Sumber | [Kaggle - Nike Sportwear Product Dataset](https://www.kaggle.com/datasets/polartech/nike-sportwear-product-dataset) |
| Jumlah Data | 229471 |
| *Usability* | 2.94 |
| Lisensi | *Unknown* |
| *Rating* | *None* |
| Jenis dan Ukuran Berkas | csv (4 MB) |

### Deskripsi Variabel

Berdasarkan informasi dari sumber dataset, variabel-variabel pada dataset adalah sebagai berikut:

- DEPARTMENT: Departemen tempat produk sepatu Nike dijual
- CATEGORY: Kategori sepatu Nike
- SUBCATEGORY: Sub-kategori sepatu Nike
- SKU: *Stock Keeping Unit* untuk manajemen inventaris sepatu Nike
- SKUVARIANT: Varian dari SKU
- PRODUCTNAME: Nama produk sepatu Nike
- PRODUCTID: ID unik untuk setiap produk sepatu Nike
- TITLE: Judul yang ditambahkan pada produk sepatu Nike
- PRODUCTTYPE: Jenis produk sepatu Nike
- PRODUCTURL: URL yang mengarah pada setiap produk sepatu Nike
- PRODUCTSIZE: Ukuran produk sepatu Nike
- LABEL: Label yang diberikan pada setiap produk sepatu Nike
- ISBESTSELLER: Penanda produk apakah laku keras atau tidak
- COLOR: Warna setiap produk sepatu Nike
- BRAND: Nama brand sepatu Nike
- AVAILABILITY: Ketersediaan produk sepatu Nike
- CURRENCY: Nilai pertukaran mata uang
- PRICECURRENT: Harga saat ini untuk setiap produk sepatu Nike
- PRICE_RETAIL: Harga eceran untuk setiap produk sepatu Nike

Berikut contoh lima data teratas pada dataset:

Tabel 2. Tampilan Lima Teratas pada Dataset

|index|DEPARTMENT|CATEGORY|SUBCATEGORY|SKU|SKU\_VARIANT|PRODUCT\_NAME|PRODUCT\_ID|TITLE|PRODUCT\_TYPE|PRODUCT\_URL|PRODUCT\_SIZE|LABEL|IS\_BESTSELLER|COLOR|BRAND|AVAILABILITY|CURRENCY|PRICE\_CURRENT|PRICE\_RETAIL|RunDate|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|Kids|Accessories and Equipment|Hats|14024803|28003080|Nike Heritage86|12231348|Kids' Adjustable Hat|APPAREL|https://www\.nike\.com/gb/t/heritage86-adjustable-hat-dnXHHk/AJ3651-010|ONE SIZE|IN\_STOCK|false|Light Menta/Barely Green|Nike Sportswear|IN\_STOCK|GBP|16\.95|16\.95|2022-08-31 13:23:12|
|1|Kids|Accessories and Equipment|Hats|12232054|21458460|Nike Heritage86|12231348|Kids' Adjustable Hat|APPAREL|https://www\.nike\.com/gb/t/heritage86-adjustable-hat-dnXHHk/AJ3651-010|ONE SIZE|IN\_STOCK|false|White/Black|Nike Sportswear|IN\_STOCK|GBP|16\.95|16\.95|2022-08-31 13:23:12|
|2|Kids|Accessories and Equipment|Hats|12231348|21453989|Nike Heritage86|12231348|Kids' Adjustable Hat|APPAREL|https://www\.nike\.com/gb/t/heritage86-adjustable-hat-dnXHHk/AJ3651-010|ONE SIZE|IN\_STOCK|false|Black/White|Nike Sportswear|IN\_STOCK|GBP|16\.95|16\.95|2022-08-31 13:23:12|
|3|Kids|Accessories and Equipment|Hats|13907994|27674553|Nike Heritage86|13907994|Kids' Adjustable Hat|APPAREL|https://www\.nike\.com/gb/t/heritage86-adjustable-hat-0SLGgN/AJ3651-413|ONE SIZE|IN\_STOCK|false|University Blue/White|Nike Sportswear|IN\_STOCK|GBP|12\.95|12\.95|2022-08-31 13:23:12|
|4|Kids|Accessories and Equipment|Hats|13907995|27674554|Nike Heritage86|13907994|Kids' Adjustable Hat|APPAREL|https://www\.nike\.com/gb/t/heritage86-adjustable-hat-0SLGgN/AJ3651-413|ONE SIZE|IN\_STOCK|false|Doll/Black|Nike Sportswear|SOLD\_OUT|GBP|12\.95|12\.95|2022-08-31 13:23:12|

### Menentukan Fitur yang Akan Digunakan

Fitur yang akan digunakan adalah `PRODUCT_NAME`, `PRODUCT_ID` dan `SUBCATEGORY`. Pada kasus ini kita lebih mengutamakan penggunaan `SUBCATEGORY` daripada `CATEGORY` karena memiliki nilai yang lebih variatif yang dapat dilihat pada Tabel 3.

Tabel 3. Jumlah Banyak *Data Point*

| Fitur | Jumlah |
|---|:---:|
| Produk Nike | 1972 |
| Kategori | 3 |
| Sub Kategori | 29 |

Tabel 4. Tampilan Lima Teratas pada Dataset setelah Pemilihan Fitur

|index|PRODUCT\_ID|PRODUCT\_NAME|SUBCATEGORY|
|---|---|---|---|
|0|12231348|Nike Heritage86|Hats|
|1|12231348|Nike Heritage86|Hats|
|2|12231348|Nike Heritage86|Hats|
|3|13907994|Nike Heritage86|Hats|
|4|13907994|Nike Heritage86|Hats|

## Data Preparation
### Menangani Missing Value

Untuk mendeteksi *missing value* digunakan fungsi isnull().sum() dan untuk mendeteksi nilai NAN digunakan isna().sum().

Tabel 5. Hasil Deteksi *Missing Value*

| Fitur | Jumlah *Missing Value* |
|---|:---:|
| PRODUCT_ID   |   0 |
| PRODUCT_NAME  |   0 |
| SUBCATEGORY  |   0 |

Tabel 6. Hasil Deteksi Nilai NAN

| Fitur | Jumlah Nilai NAN |
|---|:---:|
| PRODUCT_ID   |   0 |
| PRODUCT_NAME  |   0 |
| SUBCATEGORY  |   0 |

Dari Tabel 5. dan Tabel 6. terlihat bahwa setiap fitur tidak memiliki Missing Value (NULL) maupun Nilai NAN sehingga dapat dilanjutkan ke tahapan selanjutnya yaitu menghilangkan data duplikat.

### Menghilangkan data Duplikat

Pada tahap ini, akan dilakukan penghapusan data yang duplikat berdasarkan nama produk. Tujuannya adalah agar sistem rekomendasi yang dibuat dapat menghasilkan rekomendasi yang berbeda tanpa ada nama produk yang muncul lebih dari satu kali. Berikut perbandingan banyak data sebelum dan sesudah penghapusan data duplikat.

Tabel 7. Perbandingan Banyak Data Sebelum dan Sesudah Penghapusan Data Duplikat

| Banyak Data (*Before*) | Banyak Data (*After*) |
|:---:|:---:|
| 229471 | 1972 |

## Modelling

Pada tahap ini, untuk sistem rekomendasi yang dibuat akan menggunakan model dengan metode Cosine Similarity dan Euclidean Similarity. Namun sebelum itu akan dilakukan perubahan tipe data dari kategorikal menjadi data numerik menggunakan metode `TF-IDF Vectorizer`.

### TF-IDF Vectorizer

Pada tahap ini akan dilakukan ekstraksi fitur sekaligus menyeleksi fitur mana saja yang sering muncul dan jarang muncul. Berikut daftar fitur hasil `TF-IDF Vectorizer`.

Tabel 8. Daftar Fitur Hasil `TF-IDF Vectorizer`

| Nama Fitur |
|---|
| 'accessories' |
| 'all' |
| 'and' |
| 'backpacks' |
| 'bags' |
| 'basketball' |
| 'bras' |
| 'by' |
| 'clothing' |
| 'dresses' |
| 'equipment' |
| 'football' |
| 'gym' |
| 'hats' |
| 'hoodies' |
| 'jackets' |
| 'jerseys' |
| 'jordan' |
| 'kits' |
| 'leggings' |
| 'lifestyle' |
| 'matching' |
| 'nike' |
| 'running' |
| 'sets' |
| 'shirts' |
| 'shoes' |
| 'shorts' |
| 'skirts' |
| 'socks' |
| 'sport' |
| 'sports' |
| 'sweatshirts' |
| 'swimwear' |
| 'tights' |
| 'tops' |
| 'tracksuits' |
| 'training' |
| 'trousers' |
| 'you' |

Kemudian, dilakukan *one-hot-encoding* pada setiap produk terhadap fitur-fitur yang dihasilkan oleh `TF-IDF Vectorizer`. Berikut matrik TF-IDF yang dihasilkan.

Tabel 9. Matrik TF-IDF dengan 10 Sampel pada Kolom dan 5 Sampel pada Baris

|PRODUCT\_NAME|dresses|clothing|leggings|training|jackets|sets|nike|all|bras|trousers|
|---|---|---|---|---|---|---|---|---|---|---|
|RB Leipzig 2021/22 Stadium Home|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Jordan x Nina Chanel Abney|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.7071067811865476|0\.0|
|Milwaukee Bucks Showtime|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Air Jordan 11 CMFT Low|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Nike Dri-FIT Strike Winter Warrior|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.4769218050530363|0\.0|0\.0|

Pada Tabel 9. di atas, matrik TF-IDF memiliki ukuran 1972 x 1972 dimana 1972 menyatakan banyak produk sepatu Nike yang berbeda.

### Cosine Similarity

Rumus *Cosine Similarity* didefinisikan sebagai berikut [[2]](file:///C:/Users/ASUS/Downloads/10955-25430-1-PB.pdf):
$$\text{S}_{\text{Cosinus}}(\vec{x},\vec{y})=\cos(\theta)=\frac{\vec{x}\cdot\vec{y}}{\|\vec{x}\|\|\vec{y}\|}$$
Dengan:
* $\theta$ merupakan sudut antara vektor $\vec{x}$ dan $\vec{y}$.
* $\|\vec{x}\|$ dan $\|\vec{y}\|$ berturut-turut adalah besar atau panjang vektor $\vec{x}$ dan $\vec{y}$. 

Catatan: 
$$\|\vec{x}\|=\sqrt{\sum_{i=1}^n {x_{i}^2}}$$
dimana $x_{i}$ merupakan elemen vektor $\vec{x}$ posisi ke $i$.

Kelebihan metode ini adalah tidak bergantung pada besarnya vektor. Contoh vektor $\vec{x}=[2, 0, 4]$ dengan vektor $\vec{y}=[1, 0, 2]$ yang memiliki arah yang sama namun berbeda besarannya (akibat berbeda nilai pada fiturnya). Jika vektor $\vec{x}$ dan $\vec{y}$ dihitung tingkat kemiripan atau relevansi dengan metode ini maka nilainya $1$ (kemiripan penuh). Namun kelebihan ini dapat menjadi kekurangan jika pada kasus tertentu, makna frekuensi kemunculan fitur menjadi penting. Sedangkan pada kasus ini, *Cosine Similarity* aman digunakan karena sudah dilakukan tahap *one-hot-encoding* pada matrik tfidf. Sehingga frekuensi tiap kategori pada produk mempunyai bobot yang sama yaitu 0 (tidak ada) atau 1 (ada).

Untuk implementasinya menggunakan fungsi `cosine_similarity()` dari *library* sklearn dengan lama waktu komputasinya sebagai berikut.
```
Execution Time Cosine Similarity (Seconds) : 0.038852691650390625
```

Dengan hasil matrik *Cosine Similarity* nya sebagai berikut.

Tabel 10. Matrik *Cosine Similarity* dengan 5 Sampel pada Kolom dan 10 Sampel pada Baris

|PRODUCT\_NAME|NikeLab Collection|Nike Dri-FIT Alpha|Nike Air Force 1 Mid|Nike Huarache SE|Nike Air Max 1|
|---|---|---|---|---|---|
|S\.C\.Corinthians 2021/22 Stadium Away|0\.1093656508222228|0\.0|0\.0|0\.0|0\.0|
|Nike Sportswear Solo Swoosh|0\.0|0\.0|0\.0|0\.0|0\.0|
|Nike Air Zoom Pegasus 38|0\.0|0\.0|0\.0|0\.0|0\.0|
|Los Angeles Lakers DNA|0\.0|0\.0|0\.0|0\.0|0\.0|
|41mm Black/Multi-coloured|0\.08891952793314876|0\.0|0\.0|0\.0|0\.0|
|Boston Celtics Spotlight|1\.0|0\.0|0\.0|0\.0|0\.0|
|F\.C\. Barcelona Stadium|0\.07349796337404635|0\.0|0\.0|0\.0|0\.0|
|Nike Dri-FIT ACG UV 'Snowgrass'|0\.10855949464808363|0\.0|0\.0|0\.0|0\.0|
|Nike Sportswear Heritage 86|0\.08891952793314876|0\.0|0\.0|0\.0|0\.0|
|Nike Air Max 270|0\.0|0\.0|1\.0|1\.0|1\.0|

### Euclidean Similarity

*Euclidean* Distances didefinisikan sebagai berikut:

$$ \text{D}_{\text{Euclidean}}(\vec{x},\vec{y})=\sqrt{\sum^{n}_{i=1}(x_{i}-y_{i})^2} $$

Dengan $x_{i}$ dan $y_{i}$ berturut-turut adalah elemen ke $i$ pada vektor $\vec{x}$ dan $\vec{y}$.

Dalam buku berjudul `Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow, 2nd Edition` karya Aurelien Geron (2019) mendefinisikan *Euclidean Similarity* [[3]](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/)

$$ \text{S}_{\text{Euclidean}}(\vec{x},\vec{y})=\frac{1}{1+\text{D}_{\text{Euclidean}}(\vec{x}, \vec{y})} $$

Kelebihan Euclidean adalah dapat memperoleh nilai perbedaan antara dua vektor yang sama arahnya namun beda besarannya. Sedangkan kekurangan algoritma ini adalah fitur dengan frekuensi kemunculan paling banyak akan mendominasi fitur lain dalam hasil komputasi jarak euclideannya.

Untuk implementasinya menggunakan fungsi `euclidean_distances()` dari *library* sklearn dengan lama waktu komputasinya sebagai berikut.

```
Execution Time Euclidean Similarity (Seconds) : 0.11328577995300293
```

Dengan hasil matrik *Euclidean Similarity* nya sebagai berikut.

Tabel 11. Matrik *Euclidean Similarity* dengan 5 Sampel pada Kolom dan 10 Sampel pada Baris

|PRODUCT\_NAME|Nike ACG &quot;Fruit and Veggies&quot;|PG 6|Nike HyperCharge 682ml \(approx\.\)|Serena Design Crew|Golden State Warriors Standard Issue|
|---|---|---|---|---|---|
|Nike Dri-FIT Alpha|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Nike Repel UV D\.Y\.E\.|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Paris Saint-Germain Anthem|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|NikeLab Collection Beanie|0\.42945525262157536|0\.45555418028229927|1\.0|0\.42945525262157536|0\.42615787347032513|
|Nike Sportswear Therma-FIT City Series|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Jordan Max Aura 3|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Nike Utility Power|0\.4266606468256324|0\.4142135623730951|0\.42430488627175017|0\.4266606468256324|0\.4239937729253267|
|Nike Zoom Mercurial Superfly 9 Pro FG|0\.4142135623730951|1\.0|0\.45555418028229927|0\.4142135623730951|0\.4142135623730951|
|Nike ACG Therma-FIT|0\.4282204699105377|0\.4142135623730951|0\.4255546561881756|0\.4282204699105377|0\.42520312675484434|
|Liverpool F\.C\. 2021/22 Home|0\.42945525262157536|0\.45555418028229927|1\.0|0\.42945525262157536|0\.42615787347032513|

### Mendapatkan Rekomendasi

Pada tahap ini akan dilakukan pengujian hasil top-10 rekomendasi produk sepatu . Kemudian untuk sampel yang dipilih adalah sebagai berikut.

Tabel 12. Sampel Produk Sepatu yang Digunakan 

|index|PRODUCT\_ID|PRODUCT\_NAME|SUBCATEGORY|
|---|---|---|---|
|47584|13821933|NikeCourt Dri-FIT|Skirts and Dresses|

#### Rekomendasi dengan Cosine Similarity

Tabel 13. Top-10 Rekomendasi dengan Cosine Similarity

|index|PRODUCT\_NAME|PRODUCT\_ID|SUBCATEGORY|
|---|---|---|---|
|0|Jordan Next Utility Capsule|13428905|Skirts and Dresses|
|1|Nike Sportswear Tech Pack|13816582|Skirts and Dresses|
|2|Nike Sportswear|14041176|Skirts and Dresses|
|3|Nike ESC|13460930|Skirts and Dresses|
|4|Nike x sacai|12944380|Skirts and Dresses|
|5|Nike Dri-FIT UV Ace|13914220|Skirts and Dresses|
|6|Air Jordan|12987908|Skirts and Dresses|
|7|Nike Club Skirt|13809358|Skirts and Dresses|
|8|NikeCourt Dri-FIT Victory|13094040|Skirts and Dresses|
|9|Jordan New Classics Capsule|13428872|Skirts and Dresses|

Pada Tabel 13. di atas, terlihat bahwa untuk 10 hasil rekomendasi dengan *Cosine Similarity* mempunyai kategori yang sama dengan sampel yaitu `Skirts and Dresses` sehingga top-10 rekomendasi di atas relevan dengan sampel.

#### Rekomendasi dengan Euclidean Similarity

Tabel 14. Top-10 Rekomendasi dengan Euclidean Similarity

|index|PRODUCT\_NAME|PRODUCT\_ID|SUBCATEGORY|
|---|---|---|---|
|0|Jordan Next Utility Capsule|13428905|Skirts and Dresses|
|1|Nike Sportswear Tech Pack|13816582|Skirts and Dresses|
|2|Nike Sportswear|14041176|Skirts and Dresses|
|3|Nike ESC|13460930|Skirts and Dresses|
|4|Nike x sacai|12944380|Skirts and Dresses|
|5|Nike Dri-FIT UV Ace|13914220|Skirts and Dresses|
|6|Air Jordan|12987908|Skirts and Dresses|
|7|Nike Club Skirt|13809358|Skirts and Dresses|
|8|NikeCourt Dri-FIT Victory|13094040|Skirts and Dresses|
|9|Jordan New Classics Capsule|13428872|Skirts and Dresses|

Pada Tabel 14. di atas, terlihat bahwa untuk 10 hasil rekomendasi dengan *Euclidean Similarity* mempunyai kategori yang sama dengan sampel yaitu `Skirts and Dresses` sehingga top-10 rekomendasi di atas relevan dengan sampel.

## Evaluasi

$$\text{Recommender system precision (P)} = \frac{\text{\#of our recommendation that relevant}}{\text{\#of item we recommend}}\times 100% $$

Dari hasil rekomendasi di atas, dapat diketahui bahwa `NikeCourt Dri-FIT` termasuk ke dalam kategori `Skirts and Dresses`. Dari 10 produk yang direkomendasikan, berikut nilai _precision_ pada model _cosine similarity_ dan _euclidean distance_.

Tabel 15. Komparasi Metrik *Precision*

|Model | Sesuai | Tidak Sesuai |Total| Precision |
|---|---|---|---|---|
|_Cosine Similarity_|10|0|10|100%|
|_Euclidean Similarity_|10|0|10|100%|
 
Pada tabel di atas, terlihat bahwa model *Cosine Similiarity* dan *Euclidean Distance* memiliki nilai _precision_ yang sama pada top-10 rekomendasi di atas.

Selain dari nilai _precision_, lama komputasi setiap metode juga perlu dipertimbangkan. Berikut perbandingannya:

Tabel 16. Komparasi Waktu Komputasi

|index|Cosine Similarity|Euclidean Similarity|
|---|---|---|
|Time \(Seconds\)|0\.038852691650390625|0\.11328577995300293|

Berdasarkan output di atas, waktu komputasi pada metode Cosine Similarity (0.03885 detik) lebih cepat dibandingkan Euclidean Similarity (0.11328 detik).

Jadi dapat disimpulkan bahwa model terbaik untuk sistem rekomendasi produk sepatu adalah model dengan metode *Cosine Similarity*.

## Daftar Referensi
[1]M.Rizki Putra Utama (2019) [Tautan] (http://strategi.it.maranatha.edu/index.php/strategi/article/view/17)
[2]Rizki Tri Wahyuni (2017) [Tautan] (file:///C:/Users/ASUS/Downloads/10955-25430-1-PB.pdf)
[3]Aurelien Geron (2019) [Tautan] (https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/)
