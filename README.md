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

Penjelasan ukuran kernel:
- Mean Filtering (3x3): Kernel 3x3 dipilih untuk mean filtering karena menawarkan keseimbangan yang baik antara penghalusan noise dan preservasi detail gambar. Ini adalah ukuran kernel terkecil yang masih memberikan efek penghalusan yang signifikan. Kernel yang lebih besar akan menghasilkan efek penghalusan yang lebih kuat tetapi juga dapat mengaburkan detail gambar lebih banyak.

- Median Filtering (5x5): Untuk median filtering, kernel yang lebih besar (5x5) dipilih karena median filter secara inheren lebih baik dalam mempertahankan tepi sambil menghilangkan noise. Ukuran 5x5 memungkinkan penghapusan noise yang lebih efektif, terutama untuk noise "salt and pepper", sambil tetap menjaga sebagian besar detail gambar. Kernel yang lebih besar juga lebih tahan terhadap outlier dibandingkan dengan kernel 3x3.

Proses keseluruhan ini memungkinkan analisis komparatif yang komprehensif antara gambar asli dan hasil dari dua metode filtering yang berbeda. Dengan menampilkan semua gambar secara berdampingan, pengguna dapat dengan mudah mengevaluasi efektivitas masing-masing metode filtering dalam mengurangi noise dan mempertahankan detail gambar yang penting.
