# Penjelasan project
- Teori Deteksi Garis
Deteksi garis padacitra dapat diperoleh melalui penggunaan “cadar” (mask) dengan contoh tercantum pada Gambar 10.4 (Gonzalez & Woods, 2002). Cadar (a) berguna untuk memperoleh garis horizontal, cadar (b) untuk mendapatkan garis yang berorientasi 45 derajat, cadar (c) untuk memperoleh garis tegak, dan cadar (d) untuk mendapatkan garis yang berorientasi -45derajat. bagian  pinggir  citra  yang  tidak  diproses  akan dibuangsekiranya  argumen  ketiga  tidak  diberikan  atau  diberi  nilai false.   Nilaitruepada  argumen ketiga akan membuat bagian tepi yang tidak diproses berwarna hitam, membentuk seperti  bingkai. Argumen  keempat  digunakan  untuk  menentukan  eksekusi uint8atau  tidak.  Dalam  banyak  hal,  argumen  keempat  tidak  perlu  disebutkankarena umumnya memang hasil deteksi tepi akan memotong nilai yang di bawah nol dan yang di atas 255. Namun, di belakang akan ada aplikasi yang memerlukan fungsi deteksi tetapi tanpa perlu melakukan pemotongan seperti itu.
- Teori Deteksi Garis Zebra Cross
Teori deteksi garis Zebra Cross pada pengolahan citra adalah pendekatan yang digunakan untuk mengidentifikasi dan mengekstraksi garis-garis zebra cross atau lintasan pejalan kaki di permukaan jalan. Tujuan utama dari deteksi garis zebra cross adalah untuk mendukung sistem pengenalan atau peringatan pada kendaraan pintar atau sistem bantu pengemudi.

Berikut adalah beberapa langkah umum yang terlibat dalam deteksi garis zebra cross pada pengolahan citra:

1. Pra-pemrosesan citra: Citra input biasanya akan diubah menjadi citra grayscale dan kemudian ditingkatkan kualitasnya melalui operasi penghilangan noise, normalisasi kontras, atau peningkatan kejelasan.

2. Segmentasi citra: Pada langkah ini, garis zebra cross perlu diisolasi dari latar belakang. Metode segmentasi seperti metode ambang (thresholding) atau segmentasi berbasis region (region-based segmentation) dapat digunakan untuk memisahkan garis zebra cross dari elemen lain dalam citra.

3. Ekstraksi fitur: Setelah segmentasi, fitur-fitur penting dari garis zebra cross dapat diekstraksi. Misalnya, ekstraksi fitur berbasis tepi (edge-based) dapat dilakukan dengan menggunakan operator deteksi tepi seperti operator Sobel, Canny, atau Prewitt. Metode ekstraksi fitur lainnya adalah Transformasi Hough untuk mengidentifikasi garis secara parametrik.

4. Klasifikasi: Setelah ekstraksi fitur, klasifikasi dilakukan untuk membedakan garis zebra cross dari objek atau elemen lainnya yang ada dalam citra. Algoritma klasifikasi seperti Support Vector Machines (SVM), Neural Networks, atau algoritma pembelajaran mesin lainnya dapat digunakan dalam tahap ini.

5. Post-prosesing: Hasil deteksi garis zebra cross sering memerlukan langkah-langkah tambahan seperti menghapus deteksi yang salah atau menggabungkan deteksi yang berdekatan untuk menghasilkan hasil akhir yang lebih baik.

Penting untuk dicatat bahwa deteksi garis zebra cross pada pengolahan citra adalah topik penelitian yang aktif, dan berbagai metode dan pendekatan terus dikembangkan untuk meningkatkan keakuratan dan kehandalan deteksi.
 - Penjelasan Source Code
 Berikut adalah penjelasan detail mengenai source code tersebut:

1. `import cv2`: Mengimpor modul OpenCV yang digunakan untuk pemrosesan gambar dan video.

2. `import numpy as np`: Mengimpor modul NumPy yang digunakan untuk manipulasi data array.

3. `import matplotlib.pyplot as plt`: Mengimpor modul matplotlib.pyplot yang digunakan untuk menampilkan gambar dan plot.

4. `def detect_zebra_cross(image)`: Mendefinisikan fungsi `detect_zebra_cross` yang mengambil gambar sebagai input dan mengembalikan gambar dengan garis zebra cross yang terdeteksi.

5. `hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)`: Mengubah mode warna gambar dari BGR menjadi HSV (Hue, Saturation, Value) untuk lebih mudah mendeteksi objek berwarna putih.

6. `lower_white = np.array([0, 0, 200])` dan `upper_white = np.array([180, 30, 255])`: Mengatur rentang nilai HSV untuk mendeteksi warna putih. Rentang ini akan digunakan untuk membuat mask yang hanya mengizinkan piksel dengan nilai HSV dalam rentang ini.

7. `mask = cv2.inRange(hsv, lower_white, upper_white)`: Membuat mask menggunakan nilai HSV yang telah ditentukan sebelumnya. Mask adalah gambar biner di mana piksel putih menunjukkan piksel yang memenuhi kriteria warna putih, dan piksel hitam menunjukkan piksel yang tidak memenuhi kriteria tersebut.

8. `kernel = np.ones((5, 5), np.uint8)`: Membuat kernel yang akan digunakan untuk operasi morfologi.

9. `opening = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)`: Melakukan operasi opening pada mask untuk menghilangkan noise dan menghaluskan tepi objek putih.

10. `edges = cv2.Canny(opening, 50, 150)`: Menerapkan deteksi tepi Canny pada gambar hasil operasi opening untuk mendapatkan tepi objek putih yang lebih jelas.

11. `dilated_edges = cv2.dilate(edges, kernel, iterations=1)`: Melakukan dilasi pada tepi yang terdeteksi untuk menghubungkan segmen tepi yang berdekatan.

12. `lines = cv2.HoughLinesP(dilated_edges, 1, np.pi / 180, 100, minLineLength=40, maxLineGap=5)`: Menggunakan transformasi Hough probabilistik untuk mendeteksi garis pada gambar. Parameter-parameter seperti jarak resolusi piksel, sudut resolusi radian, ambang batas, panjang garis minimum, dan jarak maksimum antara segmen garis digunakan untuk mengontrol proses deteksi.

13. `if lines is not None:`: Memeriksa apakah garis berhasil terdeteksi.

14. `line_image = np.zeros_like(image)`: Membuat gambar kosong dengan ukuran yang sama dengan gambar asli untuk menampilkan garis yang terdeteksi.

15. Loop `for line in lines:`: Melakukan iterasi pada setiap garis yang terdeteksi.

16. `x1, y1, x2, y2 = line[0]`: Mendapatkan koordinat titik awal (x

1, y1) dan titik akhir (x2, y2) dari garis.

17. `cv2.line(line_image, (x1, y1), (x2, y2), (0, 0, 255), 18)`: Menggambar garis pada gambar line_image dengan menggunakan titik awal dan akhir, serta warna merah.

18. `result = cv2.addWeighted(image, 1.0, line_image, 1.0, 0.0)`: Menggabungkan gambar asli dengan gambar line_image dengan memberikan bobot yang sama, sehingga garis zebra cross terdeteksi tampak pada gambar output.

19. `image = cv2.imread('jln.jpg')`: Membaca gambar dengan nama file 'jln.jpg' dan menyimpannya dalam variabel image.

20. `result_rgb = cv2.cvtColor(result, cv2.COLOR_BGR2RGB)`: Mengubah mode warna gambar dari BGR menjadi RGB untuk tampilan dengan matplotlib.

21. `plt.imshow(result_rgb)`: Menampilkan gambar hasil deteksi zebra cross.

22. `plt.axis('off')`: Menonaktifkan tampilan sumbu x dan y pada plot.

23. `plt.show()`: Menampilkan plot dengan gambar hasil deteksi zebra cross.

 

 


## Jurnal Terkait 

[Jurnal](https://ieeexplore.ieee.org/document/8709718)

