# P2_Komnum


## Identitas Kelompok 3
| Name                                  | NRP         | Kelas      |
| ---                                   | ---         | -----------|
| Gabrielle Immanuel Osvaldo Kurniawan  | 5025211135  | Komnum  D  |
| Rr. Diajeng Alfisyahrinnisa Anandha   | 5025211147  | Komnum  D  |
| Victor Gustinova                      | 5025211159  | Komnum  D  |

## Apa yang dimaksud dengan integrasi romberg?

> Integrasi Romberg adalah sebuah teknik yang digunakan untuk menghitung nilai numerik dari integral definit dari suatu fungsi. ini adalah salah satu metode yang digunakan dalam teknik numerik untuk menghitung integral secara 'tepat' dan 'akurat'. Integrasi Romberg bertujuan untuk mengurangi error yang terjadi saat menghitung integral dengan menggunakan metode lain seperti metode trapesium atau metode simpson <br />

> Untuk menghitung integral menggunakan integrasi Romberg, pertama-tama kita perlu menghitung nilai integral dengan menggunakan metode lain seperti metode trapesium atau metode Simpson dengan menggunakan jumlah yang berbeda dari subinterval. Kemudian, kita dapat menggunakan teknik interpolasi yang disebut interpolasi Richardson untuk menghitung nilai integral yang lebih akurat dari fungsi yang bersangkutan. <br />

> Integrasi Romberg biasanya digunakan untuk menghitung integral dari fungsi yang sulit diintegrasikan secara analitik atau jika kita tidak memiliki rumus analitik untuk fungsi tersebut. Metode ini juga berguna jika kita membutuhkan hasil yang sangat akurat dari integral tersebut. 

## Penjelasan Kode Integrasi Romberg

### 1. Definisikan fungsi yang ingin diintegrasi yaitu `1 / (1 + x)` serta batas integrasi `a` sebagai batas bawah dan `b` sebagai batas atas

``` Volt
double f(double x) {
  // Definisi fungsi
  return 1/ (1 + x); // Contoh: integrasikan 1 / (1 + x) dari 0 hingga 1
}

double a = 0; // Batas bawah integrasi
double b = 1; // Batas atas integrasi

```

### 2. Tentukan array 2D untuk menyimpan hasil antara metode integrasi Romberg. Ukuran array harus sama dengan jumlah tingkat akurasi yang diinginkan + 1

``` Volt
const int n = 8; // Jumlah tingkat akurasi yang diinginkan
double R[n+1][n+1]; // Array 2D untuk menyimpan hasil intermediate
```

### 3. Inisialisasi array dengan hasil trapezoidal rule untuk setiap tingkat akurasi yang diinginkan. Aturan trapesium adalah metode sederhana untuk mendekati integral tertentu dari suatu fungsi yang didasarkan pada pembagian interval integrasi menjadi subinterval yang lebih kecil dan menggunakan trapesium yang dibentuk oleh fungsi dan sumbu x untuk mendekati luas di bawah kurva.

``` Volt
// Inisialisasi array dengan hasil aturan trapezoidal
for (int i = 1; i <= n; i++) {
  double h = (b - a) / pow(2, i - 1); // Lebar trapezoid
  double sum = 0;
for (int j = 1; j <= pow(2, i - 1); j++) {
  double x0 = a + (j - 1)*h;
  double x1 = a + j * h;
  sum += (f(x0) + f(x1)) * h / 2;
}

R[i][1] = sum;
```

### 4. Kita gunakan metode ekstrapolasi Richardson untuk meningkatkan akurasi integrasi. Metode ekstrapolasi Richardson adalah teknik untuk memperkirakan nilai suatu fungsi pada tingkat ketelitian yang lebih tinggi dengan menggunakan nilai fungsi pada pada tingkat ketelitian yang lebih rendah. 

``` Volt
// Gunakan metode ekstrapolasi Richardson untuk meningkatkan akurasi
for (int i = 2; i <= n; i++) {
  for (int j = 2; j <= i; j++) {
    R[i][j] = (pow(4, j - 1) * R[i][j - 1] - R[i - 1][j - 1]) / (pow(4, j - 1) - 1);
		cout << "Nilai integral dengan n = 2^" << i << ": " << R[i][j] << endl;
  }
}
```

### 5. Hasil akhir akan disimpan dalam `R[n][n]`

``` Volt
double result = R[n][n]; // Final result
```

## Complete code

``` Volt
#include <cmath>
#include <iostream>
using namespace std;

double f(double x) {
  // Definisi fungsi
	return 1/ (1 + x); // Contoh: integrasikan 1 / (1 + x) dari 0 hingga 1
}

double romberg(double a, double b, int n) {
double R[n + 1][n + 1]; // 2D array untuk menyimpan hasil intermediate

  // Inisialisasi array dengan hasil aturan trapezoidal
	for (int i = 1; i <= n; i++) {
    	double h = (b - a) / pow(2, i - 1); // Lebar trapezoid
    	double sum = 0;
    	for (int j = 1; j <= pow(2, i - 1); j++) {
    		double x0 = a + (j - 1)*h;
     		double x1 = a + j * h;
      		sum += (f(x0) + f(x1)) * h / 2;
    	}
    	R[i][1] = sum;
	}

	cout << "Nilai integral dengan n = 2^" << 0 << ": " << R[1][1] << endl;
    cout << "Nilai integral dengan n = 2^" << 1 << ": " << R[2][1] << endl;

  // Gunakan metode ekstrapolasi Richardson untuk meningkatkan akurasi
	for (int i = 2; i <= n; i++) {
    	for (int j = 2; j <= i; j++) {
      		R[i][j] = (pow(4, j - 1) * R[i][j - 1] - R[i - 1][j - 1]) / (pow(4, j - 1) - 1);
			cout << "Nilai integral dengan n = 2^" << i << ": " << R[i][j] << endl;
		}
	}

  	// Hasil akhir tersimpan di R[n][n]
	return R[n][n];
}

int main() {
	double a = 0; // Batas bawah integrasi
  	double b = 1; // Batas atas integrasi
  	int n = 8; // Jumlah tingkat akurasi yang diinginkan

  	double result = romberg(a, b, n);
  	printf("Result: %f\n", result);
  	
	return 0;
}

```

### Hasil dari `1 / (1 + x)` dengan `batas atas = 1`, `batas bawah = 0`, dan `n = 8` adalah
<p align="center">
<img width="300" alt="image" src="https://user-images.githubusercontent.com/91377782/208889350-f5d11e8e-899b-4c6f-a274-f7588cc85f7c.png">
</p>


> Sehingga, metode integrasi romberg adalah metode yang sangat efisien untuk mendekati integral tertentu dari suatu fungsi. Ini sangat berguna untuk fungsi yang tidak dapat diintegrasikan secara analitik atau untuk fungsi dengan singularitas atau sharp corners yang menyulitkan penggunaan metode lain.





