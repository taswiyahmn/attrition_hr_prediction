# Bussiness Understanding
Link Dashoboard: https://public.tableau.com/views/attrition-dashboard/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

## Project overview

Attrition atau tingkat keluar-masuknya karyawan merupakan salah satu indikator penting dalam manajemen sumber daya manusia. Tingginya tingkat attrition dapat berdampak signifikan pada stabilitas tim, biaya rekrutmen, serta produktivitas perusahaan secara keseluruhan, karena tingginya pergantian tenaga kerja dapat mengganggu kesinambungan operasional dan budaya organisasi [1], [2].

Oleh karena itu, memahami faktor-faktor yang memengaruhi keputusan karyawan untuk keluar dari perusahaan menjadi sangat krusial bagi departemen HR dalam menyusun strategi retensi yang tepat [3]. Pendekatan analitik berbasis data terbukti efektif dalam mengidentifikasi pola-pola yang tidak terlihat secara kasat mata, serta membantu dalam membangun sistem peringatan dini terhadap risiko resignasi karyawan [4].

Melalui proyek ini, dilakukan analisis data karyawan untuk mengidentifikasi pola dan variabel yang paling berpengaruh terhadap attrition. Proses analisis mencakup eksplorasi data, visualisasi melalui Tableau dashboard yang mendukung interpretasi cepat terhadap tren dan anomali dalam data [5], serta penerapan model machine learning seperti Random Forest yang secara luas digunakan untuk evaluasi feature importance karena kemampuannya menangani dataset yang kompleks dan non-linear [6], [7].

Hasil analisis ini bertujuan memberikan wawasan berbasis data guna mendukung pengambilan keputusan yang lebih strategis dalam mempertahankan karyawan dan meningkatkan stabilitas organisasi [8].

[1] J. Hausknecht, J. Rodda, and M. Howard, "Targeted employee retention: Performance-based and job-related differences in reported reasons for staying," Human Resource Management, vol. 48, no. 2, pp. 269–288, 2009. [Online]. Available: https://doi.org/10.1002/hrm.20279  
[2] T. Hinkin and C. Tracey, "The cost of turnover: Putting a price on the learning curve," Cornell Hotel and Restaurant Administration Quarterly, vol. 41, no. 3, pp. 14–21, 2000. [Online]. Available: https://doi.org/10.1177/001088040004100313  
[3] S. Allen, "Retaining talent: Replacing misconceptions with evidence-based strategies," SHRM Foundation Effective Practice Guidelines Series, 2008. [Online]. Available: https://www.shrm.org/foundation/ourwork/initiatives/resources-from-past-initiatives/pages/retainingtalent.aspx  
[4] K. A. Shaw, "Using predictive analytics for employee retention," Strategic HR Review, vol. 12, no. 1, pp. 12–17, 2013. [Online]. Available: https://doi.org/10.1108/14754391311282436  
[5] S. Few, Now You See It: Simple Visualization Techniques for Quantitative Analysis, Analytics Press, 2009.  
[6] L. Breiman, "Random Forests," Machine Learning, vol. 45, no. 1, pp. 5–32, 2001. [Online]. Available: https://doi.org/10.1023/A:1010933404324  
[7] G. Lemaître, F. Nogueira, and C. Aridas, "Imbalanced-learn: A Python Toolbox to Tackle the Curse of Imbalanced Datasets in Machine Learning," Journal of Machine Learning Research, vol. 18, no. 17, pp. 1–5, 2017. [Online]. Available: http://jmlr.org/papers/v18/16-365.html  
[8] R. Bassi, "Harnessing predictive analytics to improve employee retention," People and Strategy, vol. 29, no. 2, pp. 46–51, 2006.

## Permasalahan (Problem Statement)

1. Tingkat attrition di atas 10% menunjukkan banyak karyawan yang keluar dari perusahaan.
2. Perusahaan Jaya Jaya Maju belum memiliki sistem atau alat yang mumpuni untuk memantau data karyawan secara real-time dan analitis.
3. HR belum memanfaatkan data karyawan secara optimal untuk mengetahui pola-pola attrition dan faktor yang memengaruhinya.

## Tujuan (Goals)

1. Mengidentifikasi elemen-elemen paling berpengaruh yang menyebabkan tingginya tingkat attrition di perusahaan, berdasarkan pendekatan berbasis data.  
2. Melakukan evaluasi terhadap kelengkapan dan mutu data internal HR perusahaan untuk memastikan bahwa data tersebut dapat digunakan secara efektif dalam proses analisis attrition.  
3. Merancang strategi dan memberikan rekomendasi yang bersifat actionable dan didukung oleh hasil analisis untuk membantu menurunkan tingkat attrition ke level yang lebih sehat.  
4. Meningkatkan retensi karyawan melalui intervensi berbasis data yang dirancang secara tepat sasaran berdasarkan temuan dari proses analisis.

## Pendekatan Solusi (Solution Approach)

1. Menerapkan model machine learning Random Forest yang mampu memberikan prediksi dalam bentuk probabilitas, untuk mengidentifikasi karyawan yang memiliki kecenderungan tinggi untuk melakukan resign.  
2. Menambahkan model K-Nearest Neighbors (KNN) sebagai model pembanding, guna mengevaluasi pengaruh skala data dan memperoleh wawasan tambahan dari pendekatan yang berbeda.  
3. Mengelompokkan karyawan ke dalam beberapa segmen berdasarkan nilai probabilitas hasil prediksi Random Forest, agar dapat dilakukan intervensi yang lebih spesifik terhadap kelompok yang berisiko tinggi.

## Data Understanding and Data Preparation
### Setup Environment (Google Colab)
Proyek ini dijalankan di Google Colab dengan Python versi 3.8.16. Berikut adalah daftar library utama yang digunakan:

- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `tensorflow`
- `joblib`
- `scipy`
- `shap`

Untuk memastikan semua library tersedia, jalankan perintah berikut di awal notebook:

```python
!pip install pandas numpy matplotlib seaborn tensorflow joblib scipy shap
```

### Deskripsi Dataset

Sumber data: Dicoding Github  
Link: https://github.com/dicodingacademy/dicoding_dataset/tree/main/employee  
Jumlah data:  
• Baris: 1470  
• Kolom: 35  

Kondisi data:  
• Terdapat kemungkinan keberadaan outlier.  
• Kolom Attrition mengandung 412 nilai null.  
• Tidak ditemukan duplikasi data.

### Penjelasan Kolom

Dataset ini berisi informasi demografis, metrik pekerjaan, serta indikator attrition. Beberapa kolom penting di antaranya:  
| Variabel               | Deskripsi                                |
|-----------------------|-----------------------------------------|
| EmployeeId            | ID unik karyawan                         |
| Attrition             | Apakah karyawan mengundurkan diri? (0=tidak, 1=ya) |
| Age                   | Usia karyawan                           |
| BusinessTravel        | Frekuensi perjalanan dinas              |
| DailyRate             | Gaji harian                            |
| Department            | Departemen tempat bekerja               |
| DistanceFromHome      | Jarak rumah ke kantor                   |
| Education             | Tingkat pendidikan (1–5)                |
| EducationField        | Bidang studi                           |
| EnvironmentSatisfaction | Kepuasan lingkungan kerja (1–4)      |
| Gender                | Jenis kelamin                          |
| HourlyRate            | Gaji per jam                          |
| JobInvolvement        | Tingkat keterlibatan kerja (1–4)      |
| JobLevel              | Level jabatan (1–5)                    |
| JobRole               | Jenis pekerjaan                       |
| JobSatisfaction       | Kepuasan kerja (1–4)                   |
| MaritalStatus         | Status pernikahan                      |
| MonthlyIncome         | Pendapatan bulanan                     |
| MonthlyRate           | Gaji per bulan                        |
| NumCompaniesWorked    | Jumlah perusahaan sebelumnya           |
| Over18                | Apakah berusia di atas 18 tahun        |
| OverTime              | Lembur atau tidak                     |
| PercentSalaryHike     | Kenaikan gaji tahunan                  |
| PerformanceRating     | Penilaian kinerja (1–4)                |
| RelationshipSatisfaction | Kepuasan hubungan kerja (1–4)       |
| StandardHours         | Jam kerja standar                     |
| StockOptionLevel      | Level opsi saham                      |
| TotalWorkingYears     | Total pengalaman kerja                 |
| TrainingTimesLastYear | Jumlah pelatihan tahun lalu           |
| WorkLifeBalance       | Keseimbangan kerja dan hidup (1–4)   |
| YearsAtCompany        | Lama bekerja di perusahaan             |
| YearsInCurrentRole    | Lama menjabat posisi saat ini           |
| YearsSinceLastPromotion | Lama sejak promosi terakhir           |
| YearsWithCurrManager  | Lama bekerja dengan manajer saat ini   |


## EDA (Exploratory Data Analysis)
- **Pemeriksaan Tipe Data**

![image](https://github.com/user-attachments/assets/0d178295-7fcc-43c7-8c5e-472c97d316d4)

Setelah dilakukan pemeriksaan tipe data, semua kolom memiliki tipe data yang sudah sesuai dengan kebutuhan proses pemodelan. Tidak ditemukan ketidaksesuaian yang memerlukan konversi tipe data secara eksplisit.  

- **Pemeriksaan Duplikat dan Null**

![image](https://github.com/user-attachments/assets/acec1242-7853-4ff7-8f75-6fca0d5e0c0d)

Tidak ditemukan data yang bersifat duplikat. Namun, sebanyak 412 baris terdeteksi memiliki nilai null pada kolom Attrition, yang akan dipisahkan pada tahap data preparation karena memengaruhi validitas analisis supervised learning.  

- **Distribusi Label Attrition**

![image](https://github.com/user-attachments/assets/a7331c1d-8905-47e8-a4b9-fd6dfdf231a4)

Jumlah karyawan yang masih bertahan (label 0) adalah 879, sementara yang telah keluar (label 1) adalah 179. Ini menunjukkan adanya ketidakseimbangan data, yang umum ditemukan dalam kasus klasifikasi attrition, dan perlu dipertimbangkan dalam pemilihan metrik evaluasi.  

- **Distribusi Gender**

![image](https://github.com/user-attachments/assets/f406edbe-df5e-4824-bc05-5c51c75cb435)

Terdapat 882 karyawan laki-laki dan 588 perempuan. Meskipun jumlah laki-laki lebih banyak, perbedaannya tidak terlalu timpang, sehingga tidak menimbulkan bias ekstrem dalam klasifikasi berbasis gender.  

- **Distribusi Attrition per Departemen**

![image](https://github.com/user-attachments/assets/227c7dfa-d3f7-4392-892f-6e8f4a9cf306)

Departemen Research & Development memiliki angka attrition tertinggi secara absolut (107 kasus), diikuti oleh Sales (66), dan Human Resources (6). Namun, distribusi ini juga harus dilihat berdasarkan proporsi jumlah karyawan per departemen untuk mendapatkan insight yang lebih adil.  

- **Heatmap Korelasi**

![image](https://github.com/user-attachments/assets/24bef1ed-627e-4cf0-9f20-5249da275f50)

Visualisasi korelasi menunjukkan bahwa kolom seperti StandardHours dan EmployeeCount tidak memiliki variasi yang berarti, sehingga dianggap tidak informatif dan dihapus dari proses pemodelan.  

- **Boxplot Usia vs Attrition**

![image](https://github.com/user-attachments/assets/fb27d8cc-15b0-4ad9-93f5-db21bc95ef81)

Karyawan yang bertahan cenderung berada pada rentang usia 30-45 tahun, sedangkan mereka yang resign umumnya berusia lebih muda, yaitu sekitar 28–40 tahun. Ini mengindikasikan bahwa usia bisa menjadi faktor signifikan dalam kecenderungan untuk keluar.  

- **Violin Plot Lama Bekerja vs Attrition**

![image](https://github.com/user-attachments/assets/d9e52c0f-f22c-46ba-9c8f-d0946e2c9f39)

Karyawan dengan label attrition 0 umumnya memiliki masa kerja antara 5 hingga 10 tahun, sementara yang keluar (label 1) cenderung belum mencapai 5 tahun. Hal ini bisa menjadi indikator rendahnya retensi pada karyawan baru.  

- **Distribusi Pendidikan**

![image](https://github.com/user-attachments/assets/770d93cd-9995-441a-85da-120f01ccf40b)

Mayoritas karyawan adalah lulusan Bachelor (572 orang), disusul oleh Master (398), College (282), Below College (170), dan Doctor (48). Komposisi ini menunjukkan bahwa perusahaan sebagian besar merekrut tenaga kerja dengan latar belakang sarjana.  

- **Distribusi Job Satisfaction dan Work Life Balance**

![image](https://github.com/user-attachments/assets/c2035864-b303-4917-8e9b-898acf130d3c)

Sebagian besar karyawan merasa cukup hingga sangat puas dengan pekerjaannya (rating 3 dan 4), namun terdapat 569 orang dengan rating 1 dan 2, yang perlu menjadi perhatian. Untuk keseimbangan kerja-hidup, lebih dari 70% memberikan rating 3, tetapi 424 karyawan memberikan rating rendah (1 dan 2), yang mengindikasikan adanya ruang untuk perbaikan.

## Data Cleaning dan Data Preparation
### Umum
- Dataset asli disalin ke dalam variabel df_cleaned untuk menjaga integritas data asli selama proses analisis.
- Dataset dipisah menjadi df_train (tanpa null pada kolom Attrition) untuk pelatihan model dan df_unlabeled (dengan null) yang akan digunakan untuk interpretasi hasil prediksi.
- Kolom yang tidak relevan seperti EmployeeId, Over18, dan StandardHours dihapus karena tidak memiliki kontribusi informasi bagi model.
- Kolom kategori seperti Gender dan OverTime dikonversi ke bentuk numerik (0 dan 1), sedangkan kolom seperti BusinessTravel, Department, dan MaritalStatus dikonversi dengan one-hot encoding.
- df_train diduplikasi menjadi df_knn dan df_rf untuk masing-masing model guna mencegah data leakage.
- Dilakukan split data (train-test) untuk masing-masing model.

### KNN
- Pipeline preprocessing dibangun dengan StandardScaler untuk memastikan setiap fitur memiliki skala yang seragam, mengingat KNN sangat sensitif terhadap skala data.

### Random Forest
- Tidak ada perlakuan khusus seperti knn, menggunakan hasil dari data preparation secara umum

## Modelling and Result
### KNN
**Proses Implementasi**

![image](https://github.com/user-attachments/assets/6190cbc9-71df-459c-abf1-6313908ece5b)

Model KNN dibangun dalam bentuk pipeline yang terdiri dari dua tahap: normalisasi data menggunakan StandardScaler, dan klasifikasi menggunakan KNN dengan k=5. Proses ini dilakukan untuk mengotomatisasi dan menyederhanakan pipeline dari preprocessing hingga prediksi.

**Kelebihan**
- Sederhana dan mudah diimplementasikan.
-	Tidak memerlukan pelatihan model eksplisit (lazy learner).
-	Cukup efektif untuk dataset kecil hingga menengah.

**Kekurangan**
-	Rentan terhadap fitur yang tidak diskalakan.
-	Performa menurun pada dataset dengan banyak fitur (curse of dimensionality).
-	Lebih lambat dalam proses prediksi karena menghitung jarak ke seluruh data pelatihan.
________________________________________
### Random Forest
**Proses Implementasi**

![image](https://github.com/user-attachments/assets/988ee061-72e8-4211-b8e8-8e2a5ab867c9)

Model Random Forest dibangun sebagai ensemble dari beberapa decision tree. Model ini dilatih menggunakan subset acak dari data dan fitur, sehingga meningkatkan generalisasi. Model juga mampu memberikan output probabilitas, yang dimanfaatkan untuk segmentasi risiko attrition.

**Interpretasi (Model Terpilih)**

![image](https://github.com/user-attachments/assets/eedc5962-848c-407e-8b19-b72e2cfa9ba0)

1.	Model menghasilkan dua bentuk output: prediksi kelas dan probabilitas prediksi. Probabilitas ini digunakan untuk mengukur tingkat keyakinan model terhadap prediksinya.
2.	Diterapkan aturan modifikasi prediksi:
    - Jika probabilitas prediksi di bawah 0.5, maka diasumsikan model tidak yakin, dan prediksi dibalik.
    - Jika probabilitas di atas 0.5, maka prediksi dipertahankan.
3.	List kolom-kolom pada dataset yang memiliki kontribusi dari yang terbesar hingga terkecil berdasarkan model terpilih yaitu random forest.

**Kelebihan**
= Lebih akurat dan stabil dibandingkan model tunggal seperti Decision Tree.
- Tahan terhadap overfitting karena menggunakan teknik bagging.
- Mampu menangani fitur numerik dan kategorikal secara bersamaan.

**Kekurangan**
- Interpretasi lebih kompleks dibandingkan model linear.
- Memerlukan lebih banyak sumber daya komputasi.
- Lebih sulit untuk menjelaskan keputusan model secara langsung kepada stakeholder non-teknis.

## Evaluasi Model
### Evaluasi Model KNN

![image](https://github.com/user-attachments/assets/1160b350-3864-4bf8-9f2d-11e4149d3c56)

Model KNN menunjukkan nilai akurasi yang cukup tinggi sebesar 83,02%, namun metrik recall tergolong sangat rendah (10,26%). Hal ini berarti bahwa dari seluruh karyawan yang sebenarnya mengalami attrition, hanya sekitar 10% yang berhasil dikenali oleh model. Artinya, model banyak melewatkan kasus attrition yang sebenarnya terjadi (false negative tinggi).
Meski precision cukup baik (80%), artinya jika model memprediksi seseorang akan resign, kemungkinan besar prediksi tersebut benar. Namun karena recall-nya rendah, model ini belum efektif untuk tujuan mitigasi attrition karena terlalu banyak kasus resign yang tidak terdeteksi.

### Evaluasi Model Random Forest

![image](https://github.com/user-attachments/assets/4b8b68e5-66b3-4c59-a8a6-4e2921da3a62)

Model Random Forest memberikan performa yang sedikit lebih baik dibandingkan KNN dalam hal akurasi (84,43%) dan recall (15,38%). Yang paling menonjol, nilai precision mencapai 100%, yang berarti setiap prediksi model terhadap karyawan yang resign benar-benar terjadi. Ini sangat ideal untuk strategi intervensi, karena perusahaan bisa fokus langsung ke karyawan yang berisiko tinggi tanpa khawatir kesalahan prediksi.
Walaupun recall-nya masih rendah, model ini lebih andal untuk digunakan sebagai dasar segmentasi risiko karyawan karena prediksinya tidak menghasilkan false positive (FP = 0). Dengan demikian, intervensi dapat difokuskan pada kasus yang benar-benar valid.

## Kesimpulan
### Evaluasi Relevansi dengan Hasil Tujuan
1. Permasalahan: Tingginya Tingkat Attrition (>10%)
**Solusi**:
Analisis feature importance dari model Random Forest menunjukkan bahwa kompensasi finansial (gaji, rate harian, bulanan, dan per jam) merupakan faktor utama penyebab karyawan mengundurkan diri. Selain itu, lembur berlebih, usia muda, masa kerja singkat, dan jarak rumah ke kantor juga berkontribusi signifikan.
➡️ Rekomendasi tindakan:
- Tinjau dan tingkatkan struktur kompensasi secara kompetitif.
- Buat program pengurangan lembur dan peningkatan work-life balance.
- Sediakan fleksibilitas kerja, seperti opsi hybrid atau remote untuk karyawan yang tinggal jauh.
- Fokus pada strategi retensi untuk karyawan muda dan baru bergabung, seperti mentorship dan pengembangan karier.

2. Permasalahan: Ketiadaan Sistem Pemantauan Karyawan yang Efektif
**Solusi**:
Perusahaan belum memiliki sistem yang mampu memantau faktor-faktor risiko attrition secara real-time. Padahal data yang ada sudah cukup bersih dan bisa dianalisis lebih lanjut.
➡️ Rekomendasi tindakan:
- Bangun business dashboard interaktif yang menampilkan metrik penting seperti tingkat attrition per departemen, tren resign bulanan, dan kelompok risiko tinggi.
- Tambahkan sistem early warning untuk mengidentifikasi potensi resign berdasarkan pola perilaku (lembur tinggi, usia muda, masa kerja pendek, dsb).

3. Permasalahan: Belum Optimalnya Pemanfaatan Data HR
**Solusi**:
Data HR sudah cukup baik untuk dianalisis, namun belum digunakan secara maksimal untuk mendukung pengambilan keputusan berbasis data. Beberapa variabel penting masih bisa dikembangkan.
➡️ Rekomendasi tindakan:
- Lanjutkan pengembangan data dengan menambahkan fitur-fitur tambahan seperti absensi, evaluasi kerja, kepuasan karyawan, dan data keluhan.
- Gunakan model prediktif seperti Random Forest untuk segmentasi risiko attrition secara berkala.
- Lakukan pelatihan data literacy bagi tim HR agar mereka dapat lebih memahami dan menggunakan data untuk merancang kebijakan retensi.
