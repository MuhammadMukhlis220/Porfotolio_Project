# Customer Segmentation Analysis Using Brazilian E-Commerce Public Dataset by Olist

![a](https://play-lh.googleusercontent.com/eqLTXWdyygKUf85JsCXmcLSr1GnoYNLJfFVCmY-N8xGFr2T3PWwNcFdJ2Sx7MwcO6ac)

### **Contributor :**
- Kevin Pratama
- Muhammad Mukhlis
- Sari Amalia

## **Contents**

1. Business Problem Understanding
2. Data Understanding
3. EDA (Explanatory Data Analysis) & Data Preprocessing
4. Data Analysis & Modeling
5. Conclusion & Recommendation

------------

## **1. Business Problem Understanding**

**Context**

Di Brazil, *online shopping* menjadi salah satu kebiasaan yang berkembang dengan cepat. Menurut [Valor Capital Group](https://valorcapitalgroup.com/case-studies/olist-redesigned-the-marketplace-business-model-to-fit-the-realities-of-ecommerce-in-brazil/#:~:text=Through%20a%20single%2C%20seamless%20integration%2C%20Olist%20would%20provide,customer%20service%2C%20and%20payments%20in%20a%20single%20place), *online seller* di Brazil menghadapi beban operasional yang harus diintegrasikan secara manual pada berbagai *marketplace* sehingga Olist hadir untuk memecahkan masalah tersebut.
Olist merupakan sebuah *startup* yang berdiri pada tahun 2015 dan berhasil menjadi *unicorn* pada tahun 2021 setelah mendapat investasi sebesar USD 186 juta ([Valor Capital Group](https://valorinternational.globo.com/business/news/2021/12/15/olist-is-brazils-newest-unicorn.ghtml)). Bila dilihat dari website [Olist](https://olist.com/pt-br/) perusahaan ini memiliki 4 produk yang ditawarkan, salah satunya adalah Olist Store yang bergerak di bidang *e-commerce* dan menjadi subjek yang akan dibahas. Dilansir dari website [PitchBook](https://pitchbook.com/profiles/company/102473-65#overview), *platform* Olist menghubungkan pengusaha dengan *online seller* dan memberi akses kepada penjual untuk mengiklankan dan menjual di *marketplaces* tanpa masalah, serta memungkinkan perusahaan retail untuk menjangkau *marketplaces* internasional.

Olist menyediakan layanan operasional menyeluruh kepada *merchants* dengan mengelola katalog produk, inventaris, harga, *fulfillment*, layanan *customer*, dan pembayaran dalam satu tempat. Penjual hanya perlu menginput produk-produk yang akan dijual sekali saja pada Olist Store yang kemudian akan diintegrasikan oleh Olist ke berbagai *e-commerce* lain.

Berdasarkan website [Similarweb](https://www.similarweb.com/website/olist.com/#ranking) Olist Store (salah satu produk Olist) berada pada peringkat 43 untuk kategori *e-commerce* di Brazil. Pada dasarnya masyarakat akan lebih mengenal nama-nama perusahaan yang masuk kategori kelas atas. Mengingat Olist Store ada di peringkat 43, Olist Store perlu "merangkak" naik supaya masyarakat bisa mengenal Olist Store lebih dalam. Peringkat tersebut juga tergolong rendah untuk perusahaan sekelas Olist yang sudah mendapat status sebagai *unicorn*. Menurut [Think With Google](https://www.thinkwithgoogle.com/feature/online-shopping-trends/landing?lang=pt_BR), masyarakat di Brazil menjadikan *free shipping*, *discounts and promotions*, dan *lower prices* sebagai tiga elemen utama yang memengaruhi keputusan berbelanja mereka. Namun, juga terdapat masyarakat yang tidak terlalu memedulikan ketiga elemen tersebut sebagai pertimbangan ketika berbelanja. Oleh karena itu, Olist Store membutuhkan segmentasi pasar berdasarkan ketiga elemen tersebut sehingga marketing akan lebih tepat sasaran dan akan lebih banyak *customer* yang melakukan transaksi di Olist Store. Hal ini diharapkan dapat membantu Olist Store mendapatkan peringkat yang lebih baik dan lebih banyak masyakarat yang berbelanja di Olist store. Semakin banyak pembeli, maka profit yang akan didapat Olist Store akan semakin tinggi.

**Problem Statement**

*RFM* merupakan teknik segmentasi *customer* yang menggunakan pengamatan terhadap perilaku berbelanja *customer*. RFM merupakan singkatan dari *Recency*. *Frequency*, dan *Monetary Value*.

*RFM* dapat dilakukan secara manual dan otomatis. Segmentasi yang dilakukan secara manual akan memakan sangat banyak waktu dan tenaga. Oleh karena itu, segmentasi dapat dilakukan dengan cara mengotomatisasi proses bisnis. Salah satu cara tersebut adalah dengan menggunakan *tool* khusus untuk proses segmentasi *customer*. CDP (*Customer Data Platform*) dimanfaatkan sebagai sumber untuk mendapatkan data terkait perilaku *customer* sehingga segmentasi menjadi lebih akurat. RFM cocok digunakan untuk segmentasi *customer* karena mempertimbangkan *customer behavior* yang dilihat dari ketiga aspek berikut.

RECENCY (R): Waktu berbelanja terakhir<br>
FREQUENCY (F): Jumlah pembelian<br>
MONETARY VALUE (M): Total uang pembelanjaan<br>

**Analisis yang akurat mengenai segmentasi *customer* akan membantu perusahaan dalam menerapkan strategi marketing yang efektif**.

**Goals**

Dalam analisis ini, data diperoleh langsung dari Olist Store dan akan digunakan untuk menentukan segmentasi konsumen. Hal ini diperlukan dengan tujuan perusahaan **dapat meningkatkan peringkatnya agar masyarakat lebih mengenal Olist Store dengan cara menerapkan strategi marketing yang efektif** untuk masing-masing segmen.

**Analytic Approach**

Hal pertama yang harus dilakukan adalah dengan menganalisis seluruh transaksi yang pernah terjadi untuk menemukan pola dari fitur-fitur yang diberikan dan yang membedakan setiap *customer*. Langkah berikutnya akan dilakukan segmentasi RFM yang akan membantu perusahaan menentukan segmentasi konsumen.

**Metric Evaluation**

Evaluasi metrik yang akan digunakan adalah metode *silhouette* dan *elbow* untuk seluruh model.
1. *Elbow Method*
2. *Silhouette Method*

## **2. Data Understanding**

**Dataset source :** https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?select=olist_products_dataset.csv

**Data Info**

Data ini dibuat oleh *Olist* dan berisikan informasi *customer, seller, product, order, review, dan geolocation* dari Olist Store di Brazil mulai tahun 2016 hingga 2018

### **2.1 Customers Dataset**

Dataset ini memiliki informasi mengenai *customers* dan lokasinya. Dataset ini digunakan untuk mengidentifikasi *unique customers* pada dataset *orders* dan untuk mendapatkan *orders delivery location*-nya.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Customer_ID             | object    | key to the orders dataset. Each order has a unique customer_id |
| Customer_Unique_ID      | object    | unique identifier of a customer |
| Customer_Zip_Code_Prefix | int64     | first five digits of customer zip code |
| Customer_City           | object    | customer city name |
| Customer_State          | object    | customer state |

### **2.2 Geolocation Dataset**

Dataset ini memiliki informasi mengenai kodepos di Brazil beserta koordinat *latitude* dan *longitude*-nya. Dataset ini digunakan untuk mem-*plot* peta dan menemukan jarak antara penjual dan pembeli.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Geolocation_Zip_Code_Prefix   | int64 | first 5 digits of zip code |
| Geolocation_Latitude   | float64   | latitude |
| Geolocation_Longitude   | float64   | longitude |
| Geolocation_City  | object    | city name |
| Geolocation_State | object    | state |


### **2.3 Order Items Dataset**

Dataset ini memiliki informasi mengenai *items* yang dibeli dalam setiap pesanan.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Order_ID  | object    | order unique identifier |
| Order_Item_ID | int64 | sequential number identifying number of items included in the same order |
| Product_ID    | object    | product unique identifier |
| Seller_ID | object  | seller unique identifier |
| Shipping_Limit_Date   | object    | Shows the seller shipping limit date for handling the order over to the logistic partner |
| Price | float64   | item price |
| Freight_Value | float64   | item freight value item (if an order has more than one item the freight value is splitted between items) |

### **2.4 Order Payment Dataset**

Dataset ini memiliki informasi mengenai metode pembayaran.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Order_ID  | object    | unique identifier of an order |
| Payment_Sequential    | int64 | a customer may pay an order with more than one payment method. If he does so, a sequence will be created to |
| Payment_Type  | object    | method of payment chosen by the customer |
| Payment_Installments  | int64 | number of installments chosen by the customer |
| Payment_Value | float64   | transaction value |

### **2.5 Order Reviews Dataset**

Dataset ini memiliki informasi mengenai *review* yang dibuat oleh *customer*. 

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Review_ID | object    | unique review identifier |
| Order_ID  | object | unique order identifier |
| Review_Score  | int64    | Note ranging from 1 to 5 given by the customer on a satisfaction survey |
| Review_Comment_Title  | object | Comment title from the review left by the customer, in Portuguese |
| Review_Comment_Message    | object   | Comment message from the review left by the customer, in Portuguese |
| Review_Creation_Date  | object   | Shows the date in which the satisfaction survey was sent to the customer |
| Review_Answer_Timestamp   | object   | Shows satisfaction survey answer timestamp |

### **2.6 Orders Dataset**

Dataset ini merupakan dataset utama. Dataset ini memiliki informasi mengenai masing-masing order.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Order_ID  | object    | unique identifier of the order |
| Customer_ID   | object | key to the customer dataset. Each order has a unique customer_id |
| Order_Status  | object    | Reference to the order status (delivered, shipped, etc) |
| Order_Purchase_Timestamp  | object | Shows the purchase timestamp |
| Order_Approved_Timestamp | object   | Shows the payment approval timestamp |
| Order_Delivered_Carrier_Date     | object   | Shows the order posting timestamp. When it was handled to the logistic partner |
| Order_Delivered_Customer_Date | object   | Shows the actual order delivery date to the customer |
| Order_Estimated_Delivery_Date | object   | Shows the estimated delivery date that was informed to customer at the purchase moment |

### **2.7 Products Dataset**

Dataset ini memiliki informasi mengenai data produk yang dijual di Olist Store.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Product_ID    | object    | unique product identifier |
| Product_Category_Name | object | root category of product, in Portuguese |
| Product_Name_Length   | float64    | number of characters extracted from the product name |
| Product_Description_Length    | float64 | number of characters extracted from the product description |
| Product_Photos_Qty    | float64   | number of product published photos |
| Product_Weight_gr  | float64   | product weight measured in grams |
| Product_Length_cm | float64   | product length measured in centimeters |
| Product_Height_cm | float64   | product height measured in centimeters |
| Product_Width_cm  | float64   | product width measured in centimeters |

### **2.8 Sellers Dataset**

Dataset ini memiliki informasi mengenai penjual yang menyelesaikan order yang dibuat di Olist Store. Dengan dataset ini kita dapat menemukan lokasi penjual dan mengidentifikasi penjual mana yang memenuhi pembelian.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Seller_ID | object    | seller unique identifier |
| Seller_Zip_Code_Prefix   | int64  | first 5 digits of seller zip code |
| Seller_City   | object    | seller city name |
| Seller_State  | object | seller state |

### **2.9 Product Category Name Translation Dataset**

Dataset ini memiliki informasi mengenai terjemahan `product_category_name` dari bahasa portugis ke bahasa inggris.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| Product_Category_Name           | object    | category name in Portuguese |
| Product_Category_Name_English     | object  | category name in English |

### **2.10 Data Schema**

Data ini terbagi menjadi 9 dataset. hubungan antar dataset dapat dilihat pada gambar schema berikut.
![a](https://i.ibb.co/xS6Q78b/schema.png)

## **3. EDA (Explanatory Data Analysis) & Data Preprocessing**

Proses yang dilakukan pada tahap ini adalah sebagai berikut.
- Check Duplicate Data
- Check Missing Value
- Check Outlier
- Check Data Distribution
- Check Data Anomaly

## **4. Data Analysis & Modeling**

### **4.1 Data Analysis**

Proses yang dilakukan pada tahap ini adalah sebagai berikut.
- Analisis Total Order by Month
- Analisis Review Score Proportion
- Analisis Review Score by Month
- Analisis Review Score 1 & 2 by Month
- Word Cloud Review Comment Message (Review Score 1 & 2)
- Analisis Delivery Time (Seller Confirmed, Seller to Carrier, Carrier to Buyer)
- Cohort Analysis untuk New Customer Retention
- Analisis Total New Customer by Month
- Analisis Total Order by Hour
- Analisis Total Order by Day

### **4.2 Manual Segmentation Using RFM Feature**

Proses yang dilakukan pada tahap ini adalah sebagai berikut.
- Membuat Fitur RFM (Recency, Frequency, Monetary)
- Menentukan Nilai Segmentasi dari Fitur RFM

### **4.3 K-Means Clustering Using RFM Feature**

Pada K-Means *Clustering*, berikut poin-poin yang akan dibahas.

1. Menentukan K Value untuk *clustering*
2. *Fitting Model* untuk memprediksi *Cluster*
3. *Cluster Visualization*

### **4.4 Agglomerative Clustering Using RFM Feature**

Pada Agglomerative *Clustering*, berikut poin-poin yang akan dibahas dari setiap *linkage* (*ward*, *complete*, *average*).

1. Menentukan K Value untuk *clustering*
2. *Fitting Model* untuk memprediksi *Cluster*
3. *Cluster Visualization*

### **4.5 DBSCAN Clustering Using RFM Feature**

Pada DBSCAN *Clustering*, berikut poin-poin yang akan dibahas.

1. Menentukan K Value untuk *clustering*
2. *Fitting Model* untuk memprediksi *Cluster*
3. *Cluster Visualization*

### **4.6 K-Means Clustering Using RFM Feature (20000 sample)**

Pada K-Means *Clustering*, berikut poin-poin yang akan dibahas.

1. Menentukan K Value untuk *clustering*
2. *Fitting Model* untuk memprediksi *Cluster*
3. *Cluster Visualization*

### **4.7 Choosing Cluster Segmentation**

Dari plot hasil segmentasi manual terlihat pembagian segmen kurang tergambar dengan baik karena masih banyak kelompok segmen yang saling tumpang tindih, selain itu segmentasi manual juga memerlukan waktu yang cukup lama dalam menentukan karakteristik tiap *cluster*. Maka dari itu kami lebih memilih menggunakan *machine learning*.

Dari setiap model yang digunakan, segmentasi *customer* dikelompokkan berdasarkan nilai *monetary*. Terlihat hanya Model `K-Means` yang mampu menyegmentasikan kelompok *customer* dengan baik dibandingkan dengan model lainnya. Hal ini dikarenakan pada model lain, terlihat segmentasi pelanggan pada sejumlah titik masih membentuk lebih dari satu cluster yang seharusnya hanya satu cluster saja dan proporsi jumlah segmentasi berbeda jauh dibandingkan dengan model `K-Means`. Oleh karena itu, `K-Means` menjadi model yang dipilih berdasarkan alasan tersebut.

### **4.8 K-Means Model Segmentation Analysis**

Proses yang dilakukan pada tahap ini adalah sebagai berikut.
- Analisis Categorical Feature
  - Proporsi Berdasarkan Product_Category_Name
  - Proporsi Berdasarkan Customer_City
  - Proporsi Berdasarkan Payment_Type
- Analisis Numerical Feature
  - Data Correlation
  - Proporsi Berdasarkan Price
  - Proporsi Berdasarkan Payment_Installments

## **5. Conclusion & Recommendation**

### **5.1 Conclusion**

Pada project ini digunakan dua jenis segmentasi untuk menganalisis *customer* berdasarkan perilaku berbelanjanya masing-masing, yaitu segmentasi secara manual dan segmentasi menggunakan *machine learning*. Jenis model *machine learning* yang digunakan adalah `K-Means`, `Agglomerative`, dan `DBSCAN`. 

Pada *RFM analysis* secara manual dengan seluruh data, kami mendapatkan 7 segmentasi *customer* sebagai berikut.
- Best Customers: 10 (0.01%)
- Loyal Customers: 856 (0.90%)
- Potential Loyal Customers: 23332 (24.63%)
- Big Spenders: 23337 (24.64%)
- Economic Customers: 23644 (24.96%)
- Churn Customers: 17872 (18.87%)
- Dead Customers: 5669 (5.99%)

Pada *RFM analysis* menggunakan `K-Means` dengan data *sample* sebanyak 20000, kami mendapatkan 3 segmentasi *customer* yang berkelompok berdasarkan *monetary* sebagai berikut.
- *cluster* 0: 18886 (94.43%)
- *cluster* 1: 42 (0.21%)
- *cluster* 2: 1072 (5.36%)

Pada *RFM analysis* menggunakan `Agglomerative Ward` dengan data *sample* sebanyak 20000, kami mendapatkan 3 segmentasi *customer* yang berkelompok berdasarkan *monetary* sebagai berikut.
- *cluster* 0: 1388 (6.94%)
- *cluster* 1: 18607 (93.04%)
- *cluster* 2: 5 (0.02%)

Pada *RFM analysis* menggunakan `Agglomerative Complete` dengan data *sample* sebanyak 20000, kami mendapatkan 3 segmentasi *customer* yang berkelompok berdasarkan *monetary* sebagai berikut.
- *cluster* 0: 5 (0.02%)
- *cluster* 1: 21 (0.10%)
- *cluster* 2: 19974 (99.87%)

Pada *RFM analysis* menggunakan `Agglomerative Average` dengan data *sample* sebanyak 20000, kami mendapatkan 3 segmentasi *customer* yang berkelompok berdasarkan *monetary* sebagai berikut.
- *cluster* 0: 11 (0.05%)
- *cluster* 1: 1 (0.005%)
- *cluster* 2: 19988 (99.94%)

Pada *RFM analysis* menggunakan `DBSCAN` dengan data *sample* sebanyak 20000, kami mendapatkan 12 segmentasi *customer* yang berkelompok berdasarkan *monetary* sebagai berikut.
- *cluster* -1: 209 (1.04%)
- *cluster* 0: 16853 (84.26%)
- *cluster* 1: 2175 (10.88%)
- *cluster* 2: 191 (0.96%)
- *cluster* 3: 416 (2.08%)
- *cluster* 4: 37 (0.18%)
- *cluster* 5: 52 (0.26%)
- *cluster* 6: 15 (0.08%)
- *cluster* 7: 25 (0.12%)
- *cluster* 8: 10 (0.05%)
- *cluster* 9: 8 (0.04%)
- *cluster* 10: 9 (0.04%)

Pada *RFM analysis* menggunakan `K-Means` dengan seluruh data, kami mendapatkan 3 segmentasi *customer* yang berkelompok berdasarkan *monetary* sebagai berikut.
- *cluster* 0: 87647 (92.53%)
- *cluster* 1: 516 (0.54%)
- *cluster* 2: 6557 (6.92%)

Dari seluruh proses segmentasi yang sudah dicoba terlihat hanya model `K-Means` yang mampu menyegmentasikan kelompok *customer* dengan baik dibandingkan dengan segmentasi lainnya. Dapat terlihat segmentasi pelanggan pada sejumlah titik di segmentasi lainnya masih membentuk lebih dari satu *cluster* yang seharusnya hanya satu *cluster* saja dan proporsi jumlah segmentasi berbeda jauh dibandingkan dengan model `K-Means`. Oleh karena itu, `K-Means` menjadi model yang dipilih berdasarkan alasan tersebut. 

Kami menggunakan dua jenis data dalam pemodelan K-Means yaitu data yang utuh dan data yang hanya memiliki 20000 *sample*. Dari kedua pemodelan tersebut kami memilih model yang menggunakan data utuh dikarenakan semakin banyak data, hasil kelompok segmentasi lebih akurat.

Segmentasi K-Means dikelompokkan berdasarkan nilai *monetary* (jumlah uang yang dihabiskan untuk berbelanja). Hal ini diketahui dari hasil perbandingan grafik RFM. *Cluster* 0 yang menjadi *cluster* dengan *customer* yang total belanjanya paling sedikit, dan *cluster* 1 menjadi *cluster* dengan *customer* yang total belanjanya paling banyak. Sedangkan *cluster* 2 menjadi *cluster* dengan *customer* yang total belanjanya berada di antara *cluster* 0 dan *cluster* 1.

- *cluster* 0 (*low monetary*): 87647 (92.53%)
- *cluster* 1 (*high monetary*): 516 (0.54%)
- *cluster* 2 (*medium monetary*): 6557 (6.92%)

Karakteristik tiap *cluster*:
- Berdasarkan `Product_Category_Name`
    - *cluster* 0: paling banyak membeli kategori barang dengan urutan `bed_bath_table`, `health_beauty`, `sports_leisure`, `furniture_decor`, dan `computers_accessories`.
    - *cluster* 2: Hampir sama dengan *cluster* 0, namun yang berbeda kategori `sports_leisure` digantikan dengan `watches_gifts`.
    - *cluster* 1: Terdapat perbedaan dengan *cluster* lainnya yaitu kategori yang paling banyak dibeli adalah `computers_accessories` dan `garden_tools`.
<br><br>


- Berdasarkan `Customer_City`
    Tanpa melihat `sao paulo` dan `rio de janeiro` karena proporsi kedua kota tersebut terlalu tinggi dibandingkan kota lainnya.
    - *cluster* 0: 3 kota yang paling banyak ditempati adalah dengan urutan `belo horizonte`, `brasilia`, dan `curitiba`
    - *cluster* 2: Tidak ada perbedaan dengan *cluster* 0.
    - *cluster* 1: 3 kota yang paling banyak ditempati adalah dengan urutan `porto alegre`, `salvador`, dan `guarulhos` (2 kota terakhir adalah kota dengan penghasilan yang tinggi).
<br><br>

- Berdasarkan `Payment_Type`
    - *cluster* 0: `Payment_Type` yang paling banyak digunakan adalah dengan urutan `credit_card`, `boleto`, `voucher`, dan `debit_card`.
    - *cluster* 2: Tidak ada perbedaan dengan *cluster* 0, namun proporsi penggunaan `voucher` hampir sama dengan penggunaan `boleto`.
    - *cluster* 1: Terdapat perbedaan dengan *cluster* lainnya yaitu proporsi penggunaan `voucher` lebih banyak sehingga berada di urutan kedua.
<br><br>

- Berdasarkan `Price`
    - *cluster* 0: *Range* `Price` cluster ini berada di *range* yang rendah (*range* 0 - 400)
    - *cluster* 2: *Range* `Price` cluster ini berada di setiap *range* dengan proporsi yang merata
    - *cluster* 1: *Range* `Price` cluster ini berada di setiap *range* dengan proporsi terbesarnya berada pada range >1000
<br><br>

- Berdasarkan `Payment_Installments`
    - *cluster* 0: *Range* `Payment_Installments` yang memiliki proporsi terbesar adalah *range* `1-6` diikuti dengan kelipatan selanjutnya.
    - *cluster* 2: Tidak ada perbedaan dengan *cluster* 0, namun terjadi peningkatan proporsi pembayaran cicilan pada *range* `7-12`.
    - *cluster* 1: Tidak ada perbedaan dengan *cluster* sebelumnya.


Terjadi peningkatan order sejak bulan Januari hingga puncaknya pada bulan November 2017 ketika masyarakat Brazil merayakan `Thanksgiving` dan `Black Friday`. Pada Desember 2017 jumlah transaksi menurun karena masyarakat tidak lagi banyak melakukan transaksi sebab sudah dilakukan pada `Black Friday`. Setelah itu order mengalami stagnan karena nilai *review* mengalami penurunan secara drastis sejak November 2017 dan mencapai titik terendahnya terjadi pada bulan Maret 2018. Dari `Review_Comment_Message` yang memiliki nilai 1 dan 2 terlihat kata-kata yang sering muncul berkaitan dengan pengiriman. 

Setelah diselidiki, waktu yang dibutuhkan *seller* dari pesanan dikonfirmasi hingga kurir berangkat meningkat ketika jumlah pesanan meningkat. Hal ini terjadi karena baik *seller* maupun kurir memiliki kuota minimal untuk memberangkatkan pesanan. Selain itu waktu pengiriman barang menuju *customer* bisa mencapai 13 hari. Hal ini menjadi alasan banyaknya *review* yang bernilai 1 dan 2 yang mengakibatkan jumlah order sejak bulan November 2017 mengalami *sideways*. 

Berdasarkan `Cohort analysis`, mayoritas yang berbelanja di Olist Store adalah *customer* baru.

Order banyak terjadi dari pukul 10 pagi sampai dengan pukul 22 malam.

Jumlah order semakin mendekati *weekend* mengalami penurunan. Salah satu hal yang mungkin menjadi penyebab adalah masyarakat lebih memilih berbelanja secara *offline* pada *weekend*.

Rata-rata *price* yang dibayarkan oleh *cluster* di atas rata-rata *cluster* lainnya, sehingga *cluster* 1 menjadi prioritas utama Olist Store agar terus berbelanja dengan jumlah yang besar di Olist Store.

### **5.2 Recommendation**

Bila dilihat berdasarkan segmentasi *customer*, rekomendasi yang dapat diberikan sebagai berikut.

*High monetary*
- Menawarkan produk *Premium* dan *special editions*
- Prioritas untuk membeli produk terbaru seperti `computers_accessories` yang menjadi kategori produk paling banyak dibeli 
- Memberikan prioritas layanan pelanggan baik dari Olist Store maupun *seller*

*Low monetary*
- Memberikan voucher berupa diskon atau cashback
- Menurut [mayple](https://www.mayple.com/blog/customer-segmentation-examples) sejumlah orang di segmentasi ini memiliki karakteristik FOMO (Fear of Missing Out). Oleh karena itu Olist Store dapat memanfaatkan kondisi tersebut untuk mengajak kembali pelanggan ini dengan *email* dan *retargeting campaign*
- Lebih sering menampilkan kategori produk yang paling banyak dibeli pada kelompok ini yaitu `bed_bath_table` di *homepage* *customer*

*Medium monetary*
- Memberikan voucher berupa diskon atau cashback namun jumlahnya lebih sedikit dari kelompok *low monetary*
- Lebih sering menampilkan kategori produk yang paling banyak dibeli pada kelompok ini yaitu `bed_bath_table` di *homepage* *customer*

Bila dilihat dari seluruh *customer*, rekomendasi yang dapat diberikan sebagai berikut.
- Melihat lama waktu seller dari pesanan dikonfirmasi hingga kurir berangkat membutuhkan rata-rata kurang lebih tiga hari, Olist Store perlu mempersingkat waktu tersebut dengan target maksimal dua hari seperti yang dilakukan [Tokopedia](https://www.tokopedia.com/help/article/batas-waktu-respon-pesanan-dan-konfirmasi-pengiriman).

- Melihat lama waktu kurir mengirim barang ke *customer* membutuhkan waktu yang sangat lama, Olist perlu menambah jasa pengiriman lainnya sehingga tidak terjadi penumpukan pengiriman pada *partner* jasa pengiriman.

- Untuk mengatasi sedikitnya pelanggan yang melakukan *repeat order*, Olist Store dapat menerapkan strategi *marketing* berupa pemberian *voucher* diskon atau *cashback* kepada *customer* lama agar *customer* kembali berminat untuk melakukan transaksi di kemudian hari.

- Memberikan program *loyalty* dengan sejumlah *milestone* yang didapatkan dari setiap transaksi yang dilakukan. Setiap *milestone* akan memiliki keuntungan yang berbeda-beda; semakin tinggi pencapaian, semakin besar keuntungan yang didapatkan.

- Mengadakan *event* baru seperti "Kejar Diskon Spesial" yang diadakan oleh [Tokopedia](https://www.tokopedia.com/discovery/kejar-diskon). *Event* ini mengadakan diskon di jam-jam tertentu sesuai dengan waktu padatnya *customer* berbelanja.

- Lebih banyak mengiklankan Olist Store di kota Salvador dan Guarulhos karena kedua kota tersebut termasuk ke dalam *top* 5 kelompok *high monetary customer* dan termasuk kota yang menurut [Average Salary Survey](https://www.averagesalarysurvey.com/brazil) masyarakatnya berpenghasilan tinggi.

- *Customer* Olist Store lebih suka melakukan pembayaran dengan metode cicilan, sehingga Olist Store perlu berkerja sama dengan lebih banyak bank agar *customer* memiliki lebih banyak pilihan pembayaran ketika berbelanja karena bank menjadi jembatan antara *customer* dan Olist Store dalam hal transaksi.

- Karena jumlah order semakin mendekati *weekend* mengalami penurunan yang disebabkan oleh banyaknya masyarakat yang lebih memilih untuk berbelanja di *offline store*, Olist Store dapat bekerja sama dengan *offline store* untuk memberikan reward berupa *voucher* yang dapat digunakan untuk berbelanja di Olist Store pada *weekend*.


