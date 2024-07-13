# PA-PC_202231069_FAZA-DZOUKHAR_D

## UAS PENGOLAHAN CITRA D

### MATERI: SEGMENTASI DAUN

#### Langkah - Langkah Pengerjaan:
#### Langkah pertama yang harus dilakukan ialah, membuat folder dan memasukkan gambar yang ingin diuji ke folder tersebut. Jika sudah buka python dan mulai membuat baris kode.

#### Import Library
``` bash
import cv2
import numpy as np
import matplotlib.pyplot as plt
```
#### Dalam proyek ini, saya memanfaatkan tiga library utama yang sangat penting untuk pengolahan citra digital. Pertama, saya menggunakan OpenCV (cv2), sebuah library open-source yang kaya akan fungsi-fungsi untuk manipulasi dan analisis gambar. OpenCV menyediakan berbagai alat yang diperlukan untuk membaca, menulis, dan memproses gambar digital. Kedua, saya menggunakan NumPy (np), library fundamental untuk komputasi ilmiah dalam Python. NumPy sangat efisien dalam menangani operasi array dan matriks, yang sangat penting dalam pengolahan citra karena gambar digital pada dasarnya adalah array multi-dimensi. Terakhir, saya menggunakan Matplotlib (plt) untuk visualisasi data. Matplotlib memungkinkan saya untuk membuat plot dan grafik yang informatif, yang sangat berguna untuk menampilkan hasil pengolahan citra secara visual. Penggunaan alias seperti 'cv2', 'np', dan 'plt' tidak hanya mempersingkat penulisan kode, tetapi juga meningkatkan keterbacaan dan efisiensi dalam pengembangan program.

#### Mengimpor Gambar
``` bash
image = cv2.imread('nanas.png')
```
#### Menampilkan Gambar
``` bash
cv2.imshow("Citra Asli", image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
#### Langkah pertama adalah mengimpor gambar yang akan diproses. Saya menggunakan fungsi `cv2.imread()` untuk membaca file gambar 'nanas.png'. Fungsi ini membaca gambar dalam format `BGR (Blue, Green, Red)`, yang merupakan format default OpenCV. Setelah gambar berhasil diimpor, saya menampilkannya menggunakan `cv2.imshow()`. Fungsi ini membuka jendela baru dan menampilkan gambar dengan judul yang saya tentukan, dalam hal ini 'Citra Asli'. Untuk memastikan jendela gambar tetap terbuka dan dapat diamati, saya menggunakan `cv2.waitKey(0)`. Parameter 0 berarti jendela akan tetap terbuka sampai pengguna menekan tombol apapun. Ini memberikan kesempatan untuk mengamati gambar sebelum melanjutkan ke langkah berikutnya. Akhirnya, saya memanggil `cv2.destroyAllWindows()` untuk menutup semua jendela OpenCV yang terbuka ketika pengguna selesai mengamati gambar. Proses ini penting untuk memverifikasi bahwa gambar telah berhasil diimpor dan sesuai dengan yang diharapkan sebelum melanjutkan ke tahap pengolahan lebih lanjut.

#### Mengubah Gambar dari BGR ke HSV
``` bash
hsv_image = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
```
#### Fungsi `cv2.cvtColor(image, cv2.COLOR_BGR2HSV)` digunakan untuk mengonversi gambar dari format  `BGR (Blue, Green, Red)` ke format `HSV (Hue, Saturation, Value)`. Konversi ini penting dalam pengolahan citra karena ruang warna HSV lebih cocok untuk analisis warna dan segmentasi objek. Dalam format HSV, komponen warna lebih terpisah, sehingga memudahkan dalam mendeteksi dan mengisolasi objek berdasarkan warna tertentu. Hasil dari fungsi ini adalah gambar yang siap untuk pemrosesan lebih lanjut, seperti deteksi warna atau segmentasi berdasarkan rentang warna yang ditentukan. 

#### Batasan Warna Kuning
``` bash
lower_yellow = np.array([0, 50, 100], dtype="uint8")
upper_yellow = np.array([30, 255, 255], dtype="uint8")
```
#### Baris kode di atas digunakan untuk mendefinisikan rentang warna kuning dalam format `HSV (Hue, Saturation, Value)`. Variabel `lower_yellow` menentukan batas bawah dari warna kuning, dengan nilai hue 0, saturasi 50, dan nilai 100. Sementara itu, `upper_yellow` menetapkan batas atas, dengan nilai hue 30, saturasi 255, dan nilai 255 untuk value. Rentang ini sangat berguna dalam pengolahan citra untuk mendeteksi objek berwarna kuning dalam gambar, seperti buah nanas, dengan membuat masker berdasarkan batasan warna yang telah ditentukan.

#### Batasan Warna Hijau
``` bash
lower_green = np.array([31, 60, 5], dtype="uint8")
upper_green = np.array([150, 255, 255], dtype="uint8")
```
#### Baris kode di atas digunakan untuk mendefinisikan rentang warna hijau dalam format `HSV (Hue, Saturation, Value)`. Variabel `lower_green` menetapkan batas bawah dengan nilai hue 31, saturasi 60, dan value 5, sementara `upper_green` menetapkan batas atas dengan nilai hue 150, saturasi 255, dan value 255. Rentang ini sangat penting dalam pengolahan citra untuk mendeteksi objek berwarna hijau, seperti daun, dalam gambar. Dengan mendefinisikan batasan ini, kita dapat membuat masker yang memungkinkan pemisahan objek hijau dari latar belakang dalam proses analisis gambar lebih lanjut.

#### Pembuatan Masker untuk Deteksi Daun dan Buah Nanas
``` bash
mask_pineapple = cv2.inRange(hsv_image, lower_yellow, upper_yellow)
mask_leaves = cv2.inRange(hsv_image, lower_green, upper_green)
```
#### Menggunakan fungsi cv2.inRange(), saya menciptakan masker biner yang mengidentifikasi piksel-piksel dalam rentang warna yang telah ditentukan. Masker ini berfungsi sebagai stensil digital, memungkinkan saya untuk memisahkan objek yang diinginkan dari latar belakangnya. Proses ini melibatkan dua tahap: pertama, saya membuat masker untuk area berwarna kuning (buah nanas) menggunakan rentang warna yang telah ditentukan sebelumnya. Kedua, saya membuat masker serupa untuk area berwarna hijau (daun). Hasil dari proses ini adalah dua masker terpisah: satu untuk nanas dan satu untuk daun. Masker-masker ini berupa citra biner di mana piksel putih (nilai 255) menandakan area yang sesuai dengan rentang warna yang ditentukan, sementara piksel hitam (nilai 0) menandakan area yang tidak sesuai. Pendekatan ini memungkinkan saya untuk secara efektif mengisolasi bagian-bagian spesifik dari gambar berdasarkan karakteristik warnanya.

#### Segmentasi Buah Nanas dan Daun Menggunakan Masker
``` bash
segmented_pineapple = cv2.bitwise_and(image, image, mask=mask_pineapple)
segmented_leaves = cv2.bitwise_and(image, image, mask=mask_leaves)
```
#### Proses segmentasi dilakukan dengan menerapkan operasi bitwise AND antara gambar asli dan masker yang telah dibuat. Metode ini secara efektif mengisolasi objek yang diinginkan, dalam hal ini nanas dan daun, dari elemen lain dalam gambar. Saya menggunakan fungsi `cv2.bitwise_and()` untuk melakukan operasi ini. Pertama, saya menerapkan operasi `AND` antara gambar asli dan masker nanas, menghasilkan gambar yang hanya menampilkan area nanas. Kemudian, saya melakukan proses serupa untuk daun. Hasil dari operasi ini adalah dua gambar terpisah: satu menampilkan hanya buah nanas, dan yang lain menampilkan hanya daun. Piksel-piksel yang tidak termasuk dalam masker akan menjadi hitam, secara efektif menghilangkan latar belakang dan objek yang tidak diinginkan. Pendekatan ini memungkinkan saya untuk memfokuskan analisis pada bagian-bagian spesifik dari gambar tanpa gangguan dari elemen-elemen lain.

#### Menghapus Daun dari Masker Nanas
``` bash
mask_pineapple_no_leaves = cv2.bitwise_not(mask_leaves)
mask_pineapple_no_leaves = cv2.bitwise_and(mask_pineapple, mask_pineapple_no_leaves)
```
#### Dalam proses segmentasi citra nanas, saya melakukan langkah tambahan untuk meningkatkan akurasi dengan menghapus area daun dari masker nanas. Pertama, saya menggunakan fungsi `cv2.bitwise_not()` untuk menginversi masker daun, mengubah area daun menjadi hitam dan area lainnya menjadi putih. Selanjutnya, saya menerapkan masker inversi ini ke masker nanas original menggunakan `cv2.bitwise_and()`. Proses ini secara efektif menghilangkan area yang teridentifikasi sebagai daun dari masker nanas, menghasilkan masker nanas yang lebih akurat. Metode ini sangat berguna dalam meningkatkan presisi segmentasi, terutama ketika objek yang dianalisis memiliki kemiripan warna dengan objek di sekitarnya. Hasil akhirnya adalah sebuah masker yang lebih tepat untuk digunakan dalam tahap segmentasi selanjutnya, memungkinkan isolasi citra nanas yang lebih baik dari lingkungan sekitarnya

#### Menampilkan Hasil Segmentasi
``` bash
fig, axs = plt.subplots(2, 2, figsize=(12, 10))

axs[0, 0].imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
axs[0, 0].set_title('Nanas Asli')

axs[0, 1].imshow(mask_pineapple_no_leaves, cmap='gray')
axs[0, 1].set_title('Mask Nanas (Tanpa Daun)')

axs[1, 0].imshow(cv2.cvtColor(segmented_pineapple, cv2.COLOR_BGR2RGB))
axs[1, 0].set_title('Segmentasi Nanas')

axs[1, 1].imshow(cv2.cvtColor(segmented_leaves, cv2.COLOR_BGR2RGB))
axs[1, 1].set_title('Segmentasi Daun')

plt.show()
```
#### Untuk visualisasi hasil, saya menggunakan Matplotlib untuk membuat tampilan yang terdiri dari empat subplot: gambar asli, masker nanas tanpa daun, segmentasi nanas, dan segmentasi daun. Pendekatan ini memungkinkan perbandingan visual yang efektif antara gambar asli dan hasil segmentasi. Saya menggunakan fungsi `plt.subplots()` untuk membuat figur dengan ukuran 12x10 inci yang terdiri dari 2 baris dan 2 kolom. Pada subplot pertama (0,0), saya menampilkan gambar asli yang telah dikonversi dari BGR ke RGB untuk visualisasi yang akurat. Subplot kedua (0,1) menampilkan masker nanas tanpa daun dalam skala abu-abu, memberikan gambaran jelas tentang area yang terdeteksi sebagai nanas. Subplot ketiga (1,0) menunjukkan hasil segmentasi nanas, di mana hanya area nanas yang terlihat berwarna. Terakhir, subplot keempat (1,1) menampilkan hasil segmentasi daun. Setiap subplot diberi judul yang sesuai menggunakan fungsi `set_title()`. Dengan menyajikan hasil dalam format ini, saya dapat dengan mudah membandingkan dan menganalisis efektivitas proses segmentasi yang telah dilakukan.

#### Dari Praktikum tersebut, dapat diambil teori pendukung yang relevan:
1. Pengolahan Citra Digital: Projek ini sesuai dengan nama matakuliahnya yang mana menggunakan teknik-teknik pengolahan citra digital untuk menganalisis dan memanipulasi gambar nanas. Pengolahan citra digital adalah proses menggunakan algoritma komputer untuk melakukan operasi pada gambar digital guna mengekstrak informasi yang berguna atau meningkatkan kualitas gambar.
2. Konversi Warna: Pada projek ini, saya menggunakan dua ruang warna: `BGR dan HSV`. BGR adalah kepanjangan dari Blue, Green, Red yang mana merupakan format warna default yang digunakan oleh OpenCV. Adapun HSV adalah kepanjangan dari Hue, Saturation, dan Values. Ruang warna alternatif yang lebih efektif untuk segmentasi berdasarkan warna karena memisahkan informasi warna (hue) dari intensitas (value) dan kejernihan (saturation).
3. Segmentasi Citra: Teknik untuk memisahkan objek yang diinginkan (dalam projek ini, nanas dan daun) dari latar belakang atau objek lain dalam gambar. Projek ini menggunakan segmentasi berbasis warna.
4. Bitwise Operations: Operasi logika bitwise (AND, OR, NOT) digunakan untuk memanipulasi masker dan menggabungkan atau memisahkan area yang telah disegmentasi.
5. Color Detection:Teknik untuk mendeteksi dan mengisolasi objek berdasarkan warnanya, yang dalam projek ini digunakan untuk memisahkan nanas (kuning) dari daunnya (hijau).
6. Image Masking: Teknik untuk menyembunyikan atau mengungkapkan bagian tertentu dari gambar menggunakan masker biner.

