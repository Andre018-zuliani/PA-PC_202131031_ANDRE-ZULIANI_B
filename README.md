
### UAS_Andre Zuliani_202131031
#### Teori Yang mendukung projek terkait: Projek A

* pengolahan citra merupakan suatu cara mengusahakan suatu citra menjadi citra lain yang lebih sempurna atau yang diinginkan.

* Berdasarkan bentuk sinyal penyusunnya, citra dapat digolongkan menjadi dua jenis yaitu citra analog dan citra digital. Citra analog adalah citra yang dibentuk dari sinyal analog yang bersifat kontinyu, sedangkan citra digital adalah citra yang dibentuk dari sinyal digital yang bersifat diskrit.

* Citra analog dihasilkan dari alat akuisisi citra analog, contohnya adalah mata manusia dan kamera analog. Gambaran yang tertangkap oleh mata manusia dan foto atau film yang tertangkap oleh kamera analog merupakan contoh dari citra analog. Citra tersebut memiliki kualitas dengan tingkat kerincian (resolusi) yang sangat baik tetapi memiliki kelemahan di antaranya adalah tidak dapat disimpan, diolah, dan diduplikasi di dalam komputer.

* Citra digital merupakan representasi dari fungsi intensitas cahaya dalam bentuk diskrit pada bidang dua dimensi. Citra tersusun oleh sekumpulan piksel (picture element) yang memiliki koordinat (x,y) dan amplitudo f(x,y). Koordinat (x,y) menunjukkan letak/posisi piksel dalam suatu citra, sedangkan amplitudo f(x,y) menunjukkan nilai intensitas warna citra.

# Binary
* Pada umumnya, berdasarkan kombinasi warna pada piksel, citra dibagi menjadi tiga jenis yaitu citra RGB, citra grayscale, dan citra biner.

* Setiap piksel pada citra RGB, memiliki intensitas warna yang merupakan kombinasi dari tiga nilai intensitas pada kanal R, G, dan B. Sebagai contoh, suatu piksel yang memiliki nilai intensitas warna sebesar 255 pada kanal merah, 255 pada kanal hijau, dan 0 pada kanal biru akan menghasilkan warna kuning. Pada contoh lain, suatu piksel yang memiliki nilai intensitas warna sebesar 255 pada kanal merah, 102 pada kanal hijau, dan 0 pada kanal biru akan menghasilkan warna orange. Banyaknya kombinasi warna piksel yang mungkin pada citra RGB truecolor 24-bit adalah sebanyak 256 x 256 x 256 = 16.777.216.

* Jenis citra yang kedua adalah citra grayscale. Citra grayscale merupakan citra yang nilai intensitas pikselnya didasarkan pada derajat keabuan. Pada citra grayscale 8-bit, derajat warna hitam sampai dengan putih dibagi ke dalam 256 derajat keabuan di mana warna hitam sempurna direpresentasikan dengan nilai 0 dan putih sempurna dengan nilai 255. Citra RGB dapat dikonversi menjadi citra grayscale sehingga dihasilkan hanya satu kanal warna. Persamaan yang umumnya digunakan untuk mengkonversi citra RGB truecolor 24-bit menjadi citra grayscale 8-bit adalah

Grayscale = 0.2989*R + 0.5870*G + 0.1140*B

Grayscale adalah nilai intensitas citra grayscale,
* R adalah nilai intensitas piksel pada kanal merah,
* G adalah nilai intensitas piksel pada kanal hijau, dan
* B adalah nilai intensitas piksel pada kanal biru.

# thresholding
* Jenis citra yang ketiga adalah citra biner. Citra biner adalah citra yang pikselnya memiliki kedalaman bit sebesar 1 bit sehingga hanya memiliki dua nilai intensitas warna yaitu 0 (hitam) dan 1 (putih). Citra grayscale dapat dikonversi menjadi citra biner melalui proses thresholding. 

* Dalam proses thresholding, dibutuhkan suatu nilai threshold sebagai nilai pembatas konversi. Nilai intensitas piksel yang lebih besar atau sama dengan nilai threshold akan dikonversi menjadi 1. Sedangkan nilai intensitas piksel yang kurang dari nilai threshold akan dikonversi menjadi 0. Misalnya nilai threshold yang digunakan adalah 128, maka piksel yang mempunyai intensitas kurang dari 128 akan diubah menjadi 0 (hitam) dan yang lebih dari atau sama dengan 128 akan diubah menjadi 1 (putih).

# Edges
* Tepi (edge) adalah perubahan nilai intensitas derajat keabuan yang mendadak (besar) dalam jarak yang singkat.

* Tepi dapat diorientasikan dengan suatu arah, dan arah ini berbeda-beda bergantung pada perubahan intensitas. 

* Deteksi tepi adalah operasi yang dijalankan untuk mendeteksi garis tepi yang membatasi dua wilayah citra homogen yang memiliki tingkat kecerahan yang berbeda.

* Deteksi tepi pada suatu citra adalah suatu proses yang menghasilkan tepi-tepi dari objek-obyek citra.

* Deteksi tepi sebagian besar digunakan untuk pengukuran, deteksi, dan perubahan lokasi pada gambar. Untuk mendeteksi tepi objek, gambar diubah ke dalam skala warna grayscale atau abu-abu.

* Tepi adalah fitur dasar dari sebuah gambar. Dalam sebuah objek, bagian yang paling jelas adalah tepi dan garis.

* Dengan bantuan tepi dan garis, struktur objek dapat diketahui. Itu sebabnya mendeteksi tepi adalah teknik yang sangat penting dalam pemrosesan grafik dan ekstraksi fitur. 





## Source Code

# Import library
    import cv2 
    import numpy as np 
    import matplotlib.pyplot as plt             
    import matplotlib.patches as patches 

    image = cv2.imread("plat.jpg") 

    image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB) 

    vertices = np.array([[540, 460], [520, 680], [1020, 720], [1020, 500]], dtype=np.int32) 

    mask = np.zeros(image.shape[:2], dtype=np.uint8) 

    cv2.fillPoly(mask, [vertices], (255)) 

    masked_image = cv2.bitwise_and(image_rgb, image_rgb, mask=mask)

    cv2.polylines(masked_image, [vertices.reshape((-1, 1, 2))], True, (0, 255, 0), thickness=2)

    x, y, w, h = cv2.boundingRect(vertices)
# Crop Image
    cropped_image = masked_image[y:y+h, x:x+w]

    gray = cv2.cvtColor(cropped_image, cv2.COLOR_RGB2GRAY)
# Binary
    ret, binary = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)
# Edges
    edges = cv2.Canny(gray, 170, 200)
    fig, axs = plt.subplots(2, 2, figsize=(10, 10))

    axs[0, 0].imshow(image_rgb) 
    axs[0, 0].add_patch(patches.Polygon(vertices, closed=True, linewidth=2, edgecolor='lime', facecolor='none')) 
    axs[0, 0].set_title('Gambar Asli') 

    axs[0, 1].imshow(cropped_image) 
    axs[0, 1].set_title('Plat Terdeteksi') 

    axs[1, 0].imshow(binary, cmap='gray') 
    axs[1, 0].set_title('Binary Image') 

    axs[1, 1].imshow(edges, cmap='gray') 
    axs[1, 1].set_title('Edges Image') 

    plt.tight_layout() 
    plt.show() 

## PENJELASAN SOURCE CODE
    pip install opencv-python
pertama-tama menginstall opencv-pyhton di cmd jupter notebooknya yang berfungsi untuk melakukan operasi pengolahan gambar dengan Python.

# Import library
    
    import cv2 
    import numpy as np 
    import matplotlib.pyplot as plt             
    import matplotlib.patches as patches 

source code import cv2 ini berfungsi sebagai mengimpor modul yang menyediakan berbagai fungsi dan algoritma untuk pengolahan gambar dan video.

numpy as np sendiri untuk manipulasi array dan operasi matematika pada data numerik. 

import matplotlib.pyplot as plt sebagai modul yang digunakan untuk membuat visualisasi grafik dan plot, termasuk menampilkan gambar.

import matplotlib.patches as patches sebagai modul yang menyediakan berbagai bentuk geometri seperti segi empat, lingkaran, dan poligon, yang dapat digunakan untuk menandai atau menyoroti bagian gambar.

## Plat Terdeteksi, Binary Image, & Edges Image

    image = cv2.imread("plat.jpg") 
untuk membaca gambar dengan nama file "plat.jpg" menggunakan fungsi imread dari modul cv2

    image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB) 
untuk mengubah format warna gambar dari BGR (Blue-Green-Red) menjadi RGB (Red-Green-Blue).

    vertices = np.array([[540, 460], [520, 680], [1020, 720], [1020, 500]], dtype=np.int32)
untuk membuat array NumPy vertices yang berisi sekumpulan titik koordinat.

    mask = np.zeros(image.shape[:2], dtype=np.uint8) 
untuk membuat array NumPy mask yang berisi matriks nol dengan dimensi yang sama dengan gambar image.

    cv2.fillPoly(mask, [vertices], (255)) 
untuk mengisi poligon yang didefinisikan oleh vertices dalam array mask dengan nilai piksel 255.

    masked_image = cv2.bitwise_and(image_rgb, image_rgb, mask=mask)
untuk menerapkan operasi bitwise AND pada dua gambar dengan menggunakan mask sebagai masker.

    cv2.polylines(masked_image, [vertices.reshape((-1, 1, 2))], True, (0, 255, 0), thickness=2)
untuk menggambar poligon dengan garis tepi pada masked_image menggunakan vertices sebagai titik koordinat.

    x, y, w, h = cv2.boundingRect(vertices)
untuk menghitung kotak pembatas (bounding box) dari poligon yang didefinisikan oleh vertices.

    cropped_image = masked_image[y:y+h, x:x+w]
untuk memotong bagian gambar masked_image sesuai dengan kotak pembatas yang didefinisikan oleh x, y, w, dan h.

    gray = cv2.cvtColor(cropped_image, cv2.COLOR_RGB2GRAY)
untuk mengubah gambar yang dipotong (cropped_image) menjadi citra dalam skala abu-abu (grayscale).

    ret, binary = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)
untuk melakukan thresholding pada citra dalam skala abu-abu (gray) dengan menggunakan metode binary thresholding.

    edges = cv2.Canny(gray, 170, 200)
untuk mendeteksi tepi pada citra dalam skala abu-abu (gray) menggunakan operator Canny.

    fig, axs = plt.subplots(2, 2, figsize=(10, 10))
untuk membuat sebuah objek gambar (figure) dan sekelompok sumbu (axes) dengan tata letak 2x2.

    axs[0, 0].imshow(image_rgb) 
untuk menampilkan gambar asli (image_rgb) pada sumbu yang terletak pada indeks [0, 0]

    axs[0, 0].add_patch(patches.Polygon(vertices, closed=True, linewidth=2, edgecolor='lime', facecolor='none')) 

untuk menambahkan poligon dengan tepi pada gambar tersebut.

    axs[0, 0].set_title('Gambar Asli') 
untuk memberikan judul "Gambar Asli" pada sumbu tersebut.

    axs[0, 1].imshow(cropped_image) 
untuk menampilkan gambar yang telah dipotong (cropped_image) pada sumbu yang terletak pada indeks [0, 1].

    axs[0, 1].set_title('Plat Terdeteksi') 
untuk memberikan judul "Plat Terdeteksi" pada sumbu tersebut.

    axs[1, 0].imshow(binary, cmap='gray') 
untuk menggunakan skema warna "gray" saat menampilkan citra biner, sehingga piksel dengan nilai 0 akan ditampilkan sebagai hitam dan piksel dengan nilai 255 akan ditampilkan sebagai putih.

    axs[1, 0].set_title('Binary Image') 
untuk memberikan judul "Binary Image" pada sumbu tersebut.

    axs[1, 1].imshow(edges, cmap='gray') 
untuk menampilkan gambar tepi (edges) pada sumbu yang terletak pada indeks [1, 1]. Argumen cmap='gray' digunakan untuk menggunakan skema warna "gray" saat menampilkan gambar tepi, sehingga piksel dengan nilai tinggi akan ditampilkan sebagai putih dan piksel dengan nilai rendah akan ditampilkan sebagai hitam.

    axs[1, 1].set_title('Edges Image') 
untuk memberikan judul "Edges Image" pada sumbu tersebut.

    plt.tight_layout()  
untuk melakukan penyesuaian tata letak sumbu agar sesuai dan terlihat rapi dalam objek gambar.

    plt.show() 
untuk menampilkan objek gambar yang telah dibuat dengan menggunakan perintah-perintah sebelumnya.
## Jurnal Yang terkait

* [Link ke jurnal 1](https://ojs.unud.ac.id/index.php/merpati/article/download/17893/11623)

* [Link ke jurnal 2](https://media.neliti.com/media/publications/242431-deteksi-tepi-pada-citra-digital-mengguna-8d61e40d.pdf)

* [Link ke jurnal 3](https://www.researchgate.net/publication/336136200_Pengolahan_Citra_Kutu_Kebul_untuk_Mendeteksi_Jumlah_Kutu_Kebul_pada_Citra_Daun/fulltext/5d963958a6fdccfd0e744445/)

* [Link ke jurnal 4](Pengolahan-Citra-Kutu-Kebul-untuk-Mendeteksi-Jumlah-Kutu-Kebul-pada-Citra-Daun.pdf)

* [Link ke jurnal 5](https://journal.uii.ac.id/Snati/article/view/1521)
