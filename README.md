# METODE FILTERING CITRA

## 1. Penjelasan Tahapan Cara Menyelesaikan Proyek Secara Rinci

### a) Import Library:
   - cv2 (OpenCV): Library ini adalah toolkit open-source yang kuat untuk pemrosesan gambar dan computer vision. Ia menyediakan berbagai fungsi yang dioptimalkan untuk operasi gambar, termasuk pembacaan dan penulisan file gambar, transformasi warna, filtering, deteksi tepi, dan banyak lagi. Dalam proyek ini, cv2 digunakan terutama untuk membaca gambar, mengonversi warna, dan menerapkan median filter.
   
   - matplotlib: Ini adalah library visualisasi data yang sangat fleksibel untuk Python. Matplotlib memungkinkan pembuatan berbagai jenis plot dan grafik, termasuk penampilan gambar. Dalam konteks proyek ini, matplotlib digunakan untuk membuat subplot dan menampilkan hasil-hasil filtering secara berdampingan, memungkinkan perbandingan visual yang mudah antara gambar asli dan hasil filtering.
   
   - numpy: Numpy adalah library fundamental untuk komputasi numerik di Python. Ia menyediakan dukungan untuk array dan matriks besar multidimensi, bersama dengan koleksi fungsi matematika tingkat tinggi untuk beroperasi pada array-array ini. Dalam pemrosesan gambar, numpy sangat penting karena gambar pada dasarnya adalah array multidimensi, dan numpy memungkinkan manipulasi efisien pada level pixel.

### b) Membaca Gambar:
   - Fungsi cv2.imread("Renata.jpg") digunakan untuk membaca file gambar dari disk dan mengubahnya menjadi array numpy. Fungsi ini secara default membaca gambar dalam mode BGR (Blue, Green, Red), yang merupakan standar warna yang digunakan oleh OpenCV. Parameter tambahan dapat ditambahkan untuk mengontrol cara gambar dibaca, misalnya membacanya langsung sebagai grayscale atau dengan alpha channel.
   
   - Hasil dari cv2.imread() adalah array 3D untuk gambar berwarna, di mana dua dimensi pertama mewakili tinggi dan lebar gambar dalam pixel, dan dimensi ketiga mewakili saluran warna (biasanya 3 untuk gambar BGR). Jika gambar tidak dapat dibaca (misalnya, file tidak ditemukan), fungsi ini akan mengembalikan None, sehingga penting untuk memeriksa apakah pembacaan berhasil sebelum melanjutkan pemrosesan.
   
   - Setelah gambar dibaca, ia disimpan dalam variabel 'img' yang akan digunakan dalam operasi-operasi selanjutnya. Penting untuk dicatat bahwa gambar yang dibaca dengan cara ini disimpan dalam memori RAM, sehingga untuk gambar yang sangat besar, perlu diperhatikan penggunaan memori.

### c) Mendapatkan Dimensi Gambar:
   - Atribut shape dari array numpy digunakan untuk mendapatkan dimensi gambar. Untuk gambar berwarna, img.shape mengembalikan tuple dengan tiga nilai: (tinggi, lebar, saluran). Tinggi dan lebar merepresentasikan jumlah pixel dalam masing-masing dimensi, sementara saluran biasanya bernilai 3 untuk gambar BGR atau RGB.
   
   - Baris kode [baris, kolom] = img.shape[:2] mengekstrak dua nilai pertama dari tuple shape, yang merepresentasikan tinggi (jumlah baris) dan lebar (jumlah kolom) gambar. Teknik slicing [:2] digunakan untuk mengambil dua elemen pertama dari tuple, mengabaikan informasi tentang jumlah saluran.
   
   - Mengetahui dimensi gambar sangat penting untuk berbagai operasi pemrosesan gambar, terutama saat kita perlu melakukan iterasi melalui pixel-pixel atau saat menerapkan filter. Dimensi ini juga berguna untuk memahami ukuran gambar dan mungkin diperlukan untuk operasi seperti scaling atau cropping.

### d) Konversi Warna:
   - BGR ke RGB: Fungsi cv2.cvtColor(img, cv2.COLOR_BGR2RGB) digunakan untuk mengubah format warna gambar dari BGR (standar OpenCV) ke RGB. Konversi ini penting karena banyak library lain, termasuk matplotlib, mengharapkan gambar dalam format RGB. Proses konversi ini pada dasarnya menukar saluran blue dan red dalam array gambar.
   
   - RGB ke Grayscale: Selanjutnya, cv2.cvtColor(img, cv2.COLOR_RGB2GRAY) digunakan untuk mengonversi gambar RGB menjadi grayscale. Konversi ke grayscale mengurangi gambar tiga saluran menjadi satu saluran intensitas. Proses ini melibatkan pembobotan dan penjumlahan nilai-nilai dari tiga saluran warna, biasanya menggunakan formula: Y = 0.299R + 0.587G + 0.114B, di mana Y adalah nilai grayscale, dan R, G, B adalah nilai red, green, dan blue.
   
   - Konversi ke grayscale sangat berguna dalam banyak algoritma pemrosesan gambar karena ia menyederhanakan gambar sambil mempertahankan informasi struktural yang penting. Gambar grayscale memerlukan lebih sedikit memori dan komputasi untuk diproses, dan banyak operasi seperti deteksi tepi atau thresholding biasanya dilakukan pada gambar grayscale.

### e) Implementasi Mean Filtering:
   - Sebelum menerapkan mean filter, salinan gambar grayscale dibuat menggunakan .copy() dan dikonversi ke tipe float. Membuat salinan memastikan bahwa gambar asli tidak dimodifikasi, dan konversi ke float memungkinkan perhitungan pecahan yang akurat selama proses filtering.
   
   - Filter mean 3x3 diterapkan secara manual menggunakan nested loop yang mengiterasi melalui setiap pixel dalam gambar. Untuk setiap pixel, jendela 3x3 di sekitarnya dipertimbangkan. Proses ini melibatkan pengambilan nilai dari 9 pixel (pixel saat ini dan 8 tetangganya), menjumlahkannya, dan kemudian membagi jumlah tersebut dengan 9 untuk mendapatkan nilai rata-rata.
   
   - Implementasi manual ini memberikan kontrol penuh atas proses filtering dan memungkinkan modifikasi mudah seperti mengubah ukuran kernel atau menerapkan pembobotan khusus. Namun, pendekatan ini dapat menjadi lambat untuk gambar besar karena kompleksitas waktu O(n^2) untuk gambar n x n pixel. Untuk aplikasi praktis dengan gambar besar, fungsi bawaan seperti cv2.blur() akan lebih efisien.

### f) Implementasi Median Filtering:
   - Fungsi cv2.medianBlur(img, 5) digunakan untuk menerapkan median filter dengan kernel 5x5 pada gambar. Median filtering adalah teknik pengurangan noise non-linear yang efektif, terutama untuk menghilangkan noise "salt and pepper" sambil mempertahankan tepi gambar.
   
   - Proses median filtering melibatkan pengambilan semua pixel dalam jendela 5x5, mengurutkan nilai-nilai ini, dan kemudian mengambil nilai tengah (median) untuk menggantikan pixel pusat. Ini efektif dalam menghilangkan noise ekstrem karena outlier cenderung berada di ujung urutan dan tidak mempengaruhi nilai median.
   
   - Penggunaan fungsi bawaan OpenCV untuk median filtering sangat efisien karena dioptimalkan untuk kinerja. Implementasi ini jauh lebih cepat daripada implementasi manual, terutama untuk kernel dan gambar yang besar. Ukuran kernel 5x5 dipilih karena menawarkan pengurangan noise yang lebih kuat dibandingkan dengan kernel 3x3, sambil tetap mempertahankan sebagian besar detail gambar.

### g) Visualisasi Hasil:
   - Fungsi plt.subplots(2, 2, figsize=(15, 15)) digunakan untuk membuat grid 2x2 subplot dengan ukuran total 15x15 inci. Ini memungkinkan penampilan empat gambar berbeda secara berdampingan, memfasilitasi perbandingan visual yang mudah antara gambar asli dan hasil-hasil filtering.
   
   - Untuk setiap subplot, fungsi imshow() digunakan untuk menampilkan gambar. Parameter cmap='gray' ditambahkan untuk memastikan tampilan yang konsisten untuk gambar grayscale. Penggunaan cmap='gray' penting karena tanpanya, matplotlib mungkin menerapkan peta warna default yang bisa menghasilkan tampilan berwarna yang tidak diinginkan untuk gambar grayscale.
   
   - Setiap subplot diberi judul menggunakan fungsi set_title(), yang membantu dalam mengidentifikasi setiap gambar dengan jelas. Judul-judul ini memudahkan pembaca untuk dengan cepat memahami apa yang ditampilkan di setiap subplot tanpa harus merujuk kembali ke kode.

## Penjelasan mengenai ukuran kernel yang di pakai:
- Mean Filtering (3x3): Kernel 3x3 dipilih untuk mean filtering karena menawarkan keseimbangan yang baik antara penghalusan noise dan preservasi detail gambar. Ini adalah ukuran kernel terkecil yang masih memberikan efek penghalusan yang signifikan. Kernel yang lebih besar akan menghasilkan efek penghalusan yang lebih kuat tetapi juga dapat mengaburkan detail gambar lebih banyak.

- Median Filtering (5x5): Untuk median filtering, kernel yang lebih besar (5x5) dipilih karena median filter secara inheren lebih baik dalam mempertahankan tepi sambil menghilangkan noise. Ukuran 5x5 memungkinkan penghapusan noise yang lebih efektif, terutama untuk noise "salt and pepper", sambil tetap menjaga sebagian besar detail gambar. Kernel yang lebih besar juga lebih tahan terhadap outlier dibandingkan dengan kernel 3x3.

Proses keseluruhan ini memungkinkan analisis komparatif yang komprehensif antara gambar asli dan hasil dari dua metode filtering yang berbeda. Dengan menampilkan semua gambar secara berdampingan, pengguna dapat dengan mudah mengevaluasi efektivitas masing-masing metode filtering dalam mengurangi noise dan mempertahankan detail gambar yang penting.

## 2. Teori yang Mendukung Proyek

### a) OpenCV untuk Median Filtering:
OpenCV, sebuah library komputer vision yang kuat, menyediakan fungsi cv2.medianBlur() yang efisien untuk menerapkan median filter pada gambar. Fungsi ini bekerja dengan mengganti nilai setiap pixel dalam gambar dengan nilai median dari pixel-pixel tetangganya dalam area kernel yang ditentukan. Proses ini melibatkan pengambilan semua nilai pixel dalam kernel, mengurutkannya, dan kemudian memilih nilai tengah sebagai output. Implementasi OpenCV sangat dioptimalkan untuk kinerja, memanfaatkan paralelisasi dan optimasi low-level untuk memproses gambar dengan cepat, bahkan untuk ukuran kernel yang besar. Penggunaan fungsi bawaan ini tidak hanya menghemat waktu pengembangan tetapi juga secara signifikan meningkatkan efisiensi komputasi dibandingkan dengan implementasi manual.

### b) Perhitungan Manual untuk Mean Filtering:
Mean filtering adalah teknik penghalusan gambar yang melibatkan perhitungan rata-rata nilai pixel dalam area kernel tertentu, biasanya 3x3. Dalam implementasi manual, setiap pixel dalam gambar diproses dengan menghitung rata-rata dari 9 pixel (pixel itu sendiri dan 8 tetangganya) dalam jendela 3x3 yang berpusat pada pixel tersebut. Proses ini melibatkan penjumlahan nilai-nilai pixel dalam kernel dan kemudian membagi jumlah tersebut dengan 9 untuk mendapatkan nilai rata-rata. Implementasi manual ini memberikan kontrol penuh atas proses filtering dan memungkinkan modifikasi mudah seperti penerapan pembobotan khusus pada pixel-pixel tertentu dalam kernel. Meskipun lebih lambat dibandingkan fungsi bawaan, pendekatan manual ini sangat bermanfaat untuk pemahaman mendalam tentang bagaimana filter bekerja pada level pixel.

### c) Median Filtering:
Median filtering adalah teknik pengurangan noise non-linear yang sangat efektif, terutama untuk menghilangkan noise "salt and pepper" dalam gambar. Metode ini bekerja dengan mengurutkan semua nilai pixel dalam kernel (misalnya 3x3 atau 5x5) dan mengambil nilai tengah (median) sebagai output untuk pixel pusat. Keunggulan utama median filtering adalah kemampuannya untuk menghilangkan noise ekstrem tanpa mempengaruhi tepi gambar secara signifikan, karena outlier cenderung berada di ujung urutan dan tidak mempengaruhi nilai median. Median filter juga efektif dalam mempertahankan diskontinuitas dalam gambar, yang berarti dapat mempertahankan ketajaman tepi lebih baik dibandingkan dengan mean filter. Namun, median filtering dapat membutuhkan waktu komputasi yang lebih lama dibandingkan mean filtering, terutama untuk ukuran kernel yang besar, karena proses pengurutan yang terlibat.

### d) Mean Filtering:
Mean filtering, juga dikenal sebagai box filtering atau average filtering, adalah teknik penghalusan gambar linear yang sederhana namun efektif. Metode ini bekerja dengan mengganti nilai setiap pixel dengan rata-rata nilai pixel-pixel dalam kernel yang mengelilinginya. Mean filtering sangat efektif dalam mengurangi noise Gaussian, yang merupakan jenis noise yang umum dalam banyak aplikasi pemrosesan gambar. Namun, kelemahan utama dari mean filtering adalah kecenderungannya untuk mengaburkan tepi dan detail halus dalam gambar, karena tidak membedakan antara noise dan fitur gambar yang penting. Efek pengaburan ini menjadi lebih signifikan dengan meningkatnya ukuran kernel. Meskipun demikian, mean filtering tetap menjadi pilihan populer untuk pra-pemrosesan gambar dalam banyak aplikasi karena kesederhanaannya dan efektivitasnya dalam mengurangi variasi intensitas lokal dalam gambar.

## 1. Analisis Hasil Proyek

### a) Mean Filtering:

* Efektif dalam mengurangi noise umum dan menghaluskan gambar:
Mean filtering bekerja dengan sangat baik dalam mengurangi noise acak yang tersebar merata di seluruh gambar, seperti noise Gaussian. Proses pengambilan rata-rata dari pixel-pixel tetangga secara efektif menekan fluktuasi intensitas lokal yang disebabkan oleh noise. Hasil dari mean filtering adalah gambar yang terlihat lebih halus dan "lembut", dengan berkurangnya variasi tajam antar pixel yang berdekatan.

* Dapat mengaburkan tepi dan detail halus dalam gambar:
Salah satu kelemahan utama dari mean filtering adalah kecenderungannya untuk mengaburkan tepi dan detail halus dalam gambar. Ini terjadi karena filter tidak membedakan antara variasi intensitas yang disebabkan oleh noise dan yang disebabkan oleh fitur gambar yang sebenarnya. Akibatnya, area dengan perubahan intensitas yang tajam, seperti tepi objek, dapat menjadi kurang tajam setelah filtering. Detail-detail halus seperti tekstur atau fitur kecil dalam gambar juga dapat hilang atau berkurang kejelasannya.

* Hasil cenderung lebih halus dibandingkan dengan gambar asli:
Gambar yang dihasilkan dari mean filtering cenderung memiliki tampilan yang lebih halus dan kurang "kasar" dibandingkan dengan gambar asli. Efek penghalusan ini dapat terlihat sebagai pengurangan kontras lokal dan hilangnya sebagian tekstur halus. Meskipun ini dapat menghasilkan gambar yang terlihat lebih "bersih", terkadang juga dapat menghilangkan karakter atau detail penting dari gambar asli, terutama jika ukuran kernel yang digunakan terlalu besar.

* Baik untuk mengurangi variasi gradual dalam intensitas pixel:
Mean filtering sangat efektif dalam mengurangi variasi gradual dalam intensitas pixel. Ini membuat metode ini ideal untuk menghaluskan area dengan perubahan intensitas yang perlahan, seperti gradien warna atau bayangan halus. Kemampuan ini juga membuat mean filtering berguna dalam pra-pemrosesan gambar untuk tugas-tugas seperti segmentasi atau deteksi objek, di mana pengurangan variasi lokal dapat membantu dalam identifikasi area yang lebih homogen.

### b) Median Filtering:

* Sangat efektif dalam menghilangkan noise "salt and pepper":
Median filtering unggul dalam menghilangkan noise "salt and pepper", yang ditandai dengan pixel-pixel terisolasi dengan intensitas yang sangat tinggi atau sangat rendah. Keefektifan ini disebabkan oleh sifat non-linear dari median filter, di mana nilai-nilai ekstrem (outlier) tidak mempengaruhi hasil seperti pada mean filter. Dalam kasus noise "salt and pepper", pixel-pixel noise akan berada di ujung distribusi nilai dalam kernel dan karenanya tidak akan dipilih sebagai nilai median, secara efektif menghilangkan noise tersebut.

* Lebih baik dalam mempertahankan tepi dibandingkan mean filter:
Median filtering memiliki keunggulan dalam mempertahankan tepi dan batas-batas tajam dalam gambar dibandingkan dengan mean filtering. Ini karena median filter tidak menciptakan nilai-nilai pixel baru, melainkan selalu memilih salah satu nilai yang ada dalam kernel. Akibatnya, transisi tajam antara area gelap dan terang (yang mendefinisikan tepi) cenderung dipertahankan, menghasilkan gambar yang lebih tajam dan jelas dibandingkan dengan hasil mean filtering.

* Dapat menghilangkan detail halus jika ukuran kernel terlalu besar:
Meskipun median filtering efektif dalam mempertahankan tepi, penggunaan ukuran kernel yang terlalu besar dapat menyebabkan hilangnya detail halus dalam gambar. Ini terjadi karena filter median menggantikan setiap pixel dengan nilai median dari area yang lebih luas, yang dapat menghilangkan fitur-fitur kecil atau tekstur halus yang ukurannya lebih kecil dari kernel. Oleh karena itu, pemilihan ukuran kernel yang tepat sangat penting untuk menyeimbangkan antara pengurangan noise dan preservasi detail.

* Hasil cenderung terlihat lebih "bersih" dari noise dibandingkan mean filter:
Hasil dari median filtering sering kali terlihat lebih "bersih" dan bebas dari noise dibandingkan dengan hasil mean filtering, terutama untuk jenis noise impulsif. Ini karena median filter sangat efektif dalam menghilangkan nilai-nilai ekstrem tanpa mempengaruhi nilai-nilai pixel yang lebih representatif. Hasilnya adalah gambar yang terlihat lebih jernih dengan noise yang berkurang secara signifikan, sambil tetap mempertahankan sebagian besar struktur dan detail penting dari gambar asli.

## Kesimpulan:
Kedua metode filtering ini, mean dan median, memiliki kelebihan dan kekurangan masing-masing yang membuatnya sesuai untuk skenario yang berbeda. Pilihan antara keduanya sangat tergantung pada jenis noise yang ada dalam gambar dan hasil akhir yang diinginkan. Median filter umumnya lebih disukai untuk menghilangkan noise "salt and pepper" dan mempertahankan tepi, sementara mean filter lebih baik untuk mengurangi noise Gaussian dan menghaluskan gambar secara umum. Dalam prakteknya, pemilihan metode filtering yang tepat sering kali memerlukan eksperimen dan penyesuaian berdasarkan karakteristik spesifik dari gambar yang sedang diproses dan tujuan akhir dari pemrosesan tersebut.
