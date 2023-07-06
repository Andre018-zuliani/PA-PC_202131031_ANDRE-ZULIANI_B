
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

selanjutnya kita akan mengimport cv2 dari file pyhton dan numpy untuk numerik serta matplotlib untuk ukuran dan perhitungan agar bisa diolah gambarnya.
## Plat Terdeteksi, Binary Image, & Edges Image.

    image = cv2.imread("plat.jpg") 
    image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB) 
selanjutnya dari sini saya membuat gambar saya"plat.jpg" agar terbaca oleh jupyter notebooknya, dan gambar tersebut diberi variabelnya dengan nama image yang akan di beri warna rgb.

    vertices = np.array([[540, 460], [520, 680], [1020, 720], [1020, 500]], dtype=np.int32)
    mask = np.zeros(image.shape[:2], dtype=np.uint8) 
    cv2.fillPoly(mask, [vertices], (255)) 
    masked_image = cv2.bitwise_and(image_rgb, image_rgb, mask=mask)
selanjutnya saya disini membuat array berupa mask dan pemberian bitwise 

    cv2.polylines(masked_image, [vertices.reshape((-1, 1, 2))], True, (0, 255, 0), thickness=2)
    x, y, w, h = cv2.boundingRect(vertices)
disini saya memberi tanda pada pada gambar yang akan dituju

    cropped_image = masked_image[y:y+h, x:x+w]
    gray = cv2.cvtColor(cropped_image, cv2.COLOR_RGB2GRAY)
disni saya membuat gambarnya ke crop dari gambarnya dan agar terlihat dengan jelas platnya serta diberi warna RGB.

    ret, binary = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)
selanjutnya saya memberi warna binary dengan threshold

    edges = cv2.Canny(gray, 170, 200)
disini saya membuar edges untuk mendeteksi tepi pada citra dalam skala abu-abu (gray) menggunakan operator Canny.

    fig, axs = plt.subplots(2, 2, figsize=(10, 10))
    axs[0, 0].imshow(image_rgb) 
    axs[0, 0].add_patch(patches.Polygon(vertices, closed=True, linewidth=2, edgecolor='lime', facecolor='none')) 

lalu disni saya membuat gambarnya tampil di programnya menggunakan fig axes.

    axs[0, 0].set_title('Gambar Asli') 
    axs[0, 1].imshow(cropped_image) 
    axs[0, 1].set_title('Plat Terdeteksi') 
    axs[1, 0].imshow(binary, cmap='gray') 
    axs[1, 0].set_title('Binary Image') 
    axs[1, 1].imshow(edges, cmap='gray') 
    axs[1, 1].set_title('Edges Image') 
lalu saya membuat tampilan satu persatu dari gambar yang saya olah dari 'gambar asli','plat terdeteksi','binary image', dan 'edges image'.

    plt.tight_layout()  
    plt.show() 
dan terakhir saya membuat gambarnya agar terlihat simetris dan rapih.
## Jurnal Yang terkait

* [Link ke jurnal 1](https://ojs.unud.ac.id/index.php/merpati/article/download/17893/11623)

* [Link ke jurnal 2](https://media.neliti.com/media/publications/242431-deteksi-tepi-pada-citra-digital-mengguna-8d61e40d.pdf)

* [Link ke jurnal 3](https://www.researchgate.net/publication/336136200_Pengolahan_Citra_Kutu_Kebul_untuk_Mendeteksi_Jumlah_Kutu_Kebul_pada_Citra_Daun/fulltext/5d963958a6fdccfd0e744445/)

* [Link ke jurnal 4](Pengolahan-Citra-Kutu-Kebul-untuk-Mendeteksi-Jumlah-Kutu-Kebul-pada-Citra-Daun.pdf)

* [Link ke jurnal 5](https://journal.uii.ac.id/Snati/article/view/1521)
