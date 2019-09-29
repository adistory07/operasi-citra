# Tugas Operasi-operasi Citra

Anggota:
- Defrian Afandi (09021281722075)
- Farhan Furqan (09021281722045)
- M Imam Pratama (09021281722063)

## Dasar Pengoperasian Citra pada Python 3

Untuk implementasi operasi-operasi citra pada program, kami menggunakan bahasa
pemrograman Python 3 dan library PIL (Python Image Library) pada python.

untuk menginstall library-nya, jalankan command berikut:

```
pip3 install pillow
```

Untuk menggunakannya pada program python, masukkan snippet berikut pada bagian
atas program

```python
from PIL import Image
```

Sebelum melakukan operasi terhadap citra digital, kita harus mengaksesnya dulu
seperti berikut:

```python
CITRA = Image.open('gambar.jpg')
PIXEL = CITRA.load()
```

Untuk mengetahui ukuran gambar yang akan kita olah, gunakan variable `size`
seperti berikut:

```python
print(CITRA.size)
print(CITRA.size[0])  # ukuran horizontal
print(CITRA.size[1])  # ukuran vertikal
```

Berikut contoh outputnya:

```
(5120, 2880)
5120
2880
```

Untuk mengkonversi suatu citra RGB ke grayscale, berikut caranya:

```python
# sumber: https://stackoverflow.com/a/18778280/9157799
CITRA_GRAYSCALE = Image.open('gambar.jpg').convert('L')
```

Jika kita telah selesai melakukan operasi-operasi terhadap suatu citra, kita
perlu menyimpan hasil pengolahan citranya dengan cara berikut:

```python
CITRA_GRAYSCALE.save('gambar_grayscale.jpg')
```

## Operasi pada aras titik

Berikut cara mengakses pixel dan nilai RGB dari suatu citra:

```python
x = 3
y = 8
print(PIXEL[x, y])   # nilai RGB dari pixel (x, y)
print(PIXEL[x, y][0])  # nilai R dari pixel (x, y)
print(PIXEL[x, y][1])  # nilai G dari pixel (x, y)
print(PIXEL[x, y][2])  # nilai B dari pixel (x, y)
```

Dan berikut cara mengubah RGB dari suatu pixel:

```python
PIXEL[3, 8] = (0, 0, 0)  # mengubah pixel (3, 8) menjadi warna hitam
```

Untuk citra grayscale, berikut caranya:

```python
# jangan lupa akses pixel dari citra grayscale
PIXEL_GRAYSCALE = CITRA_GRAYSCALE.load()

PIXEL_GRAYSCALE[3, 8] = 128
print(PIXEL_GRAYSCALE[3, 8])
```

### Citra Biner

untuk membuat citra biner, kita menggunakan citra grayscale. Berikut programnya:

```python
from PIL import Image

# fungsi citra biner
def citra_biner(nilai_ambang):
    # konversi gambar RGB ke grayscale
    # https://stackoverflow.com/a/18778280/9157799
    CITRA_GRAYSCALE = Image.open('gambar.jpg').convert('L')
    PIXEL_GRAYSCALE = CITRA_GRAYSCALE.load()

    ukuran_horizontal = CITRA_GRAYSCALE.size[0]
    ukuran_vertikal = CITRA_GRAYSCALE.size[1]

    for x in range(ukuran_horizontal):
        for y in range(ukuran_vertikal):
            if PIXEL_GRAYSCALE[x, y] < nilai_ambang:
                PIXEL_GRAYSCALE[x, y] = 0
            else:
                PIXEL_GRAYSCALE[x, y] = 255

    nama_setelah_disave = 'gambar_biner_' + str(nilai_ambang) + '.jpg'
    CITRA_GRAYSCALE.save(nama_setelah_disave)


citra_biner(50)
citra_biner(128)
citra_biner(200)
citra_biner(230)
```

Berikut gambar asli sebelum diolah:
![gambar asli](gambar.jpg "gambar asli")

Setelah dilakukan operasi citra biner dengan batas ambang 50:
![](img/gambar_biner_50.jpg)

Setelah dilakukan operasi citra biner dengan batas ambang 128:
![](img/gambar_biner_128.jpg)

Setelah dilakukan operasi citra biner dengan batas ambang 200:
![](img/gambar_biner_200.jpg)

Setelah dilakukan operasi citra biner dengan batas ambang 230:
![](img/gambar_biner_230.jpg)

### Citra Negatif

untuk membuat citra negatif, berikut programnya:

```python
from PIL import Image

CITRA = Image.open('gambar.jpg')

ukuran_horizontal = CITRA.size[0]
ukuran_vertikal = CITRA.size[1]

PIXEL = CITRA.load()

for x in range(ukuran_horizontal):
    for y in range(ukuran_vertikal):
        R = 255 - PIXEL[x, y][0]
        G = 255 - PIXEL[x, y][1]
        B = 255 - PIXEL[x, y][2]
        PIXEL[x, y] = (R, G, B)

CITRA.save('gambar_negatif.jpg')
```

Masih menggunakan gambar yang sama, berikut hasil gambar setelah dilakukan operasi citra negatif:
![](img/gambar_negatif.jpg)

### Citra Pencerahan

untuk operasi pencerahan citra, berikut programnya:

```python
from PIL import Image

def clipping(intensitas):
    if intensitas < 0:
        return 0
    if intensitas > 255:
        return 255
    return intensitas

def atur_pencerahan(nilai_pencerahan, nama_setelah_disave):
    CITRA = Image.open('gambar.jpg')
    PIXEL = CITRA.load()

    ukuran_horizontal = CITRA.size[0]
    ukuran_vertikal = CITRA.size[1]

    for x in range(ukuran_horizontal):
        for y in range(ukuran_vertikal):
            R = clipping(PIXEL[x, y][0] + nilai_pencerahan)
            G = clipping(PIXEL[x, y][1] + nilai_pencerahan)
            B = clipping(PIXEL[x, y][2] + nilai_pencerahan)
            PIXEL[x, y] = (R, G, B)

    CITRA.save(nama_setelah_disave)


atur_pencerahan(80, 'gambar_dicerahkan.jpg')
atur_pencerahan(-80, 'gambar_digelapkan.jpg')
```

Gambar yang sama, berikut hasil gambar setelah dilakukan operasi citra pencerahan;

Dengan intensitas +50:

![](img/gambar_dicerahkan.jpg)

Dengan intensitas -50:

![](img/gambar_digelapkan.jpg)