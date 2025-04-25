# **Modul: Analisis dan Visualisasi Data Saham S&P 500**

## **Tujuan**
Modul ini bertujuan untuk memandu pengguna dalam melakukan analisis sederhana terhadap data harga saham indeks S&P 500 (`.SPX`). Pengguna akan belajar cara membaca data, menghitung pengembalian logaritmik (log returns), menghitung volatilitas historis, dan memvisualisasikan data tersebut menggunakan Python.

---

## **Langkah-Langkah**

### **1. Persiapan Lingkungan**
1. Pastikan Anda memiliki Python terinstal di komputer Anda.
2. Instal library yang diperlukan dengan menjalankan perintah berikut di terminal:
   ```bash
   pip install numpy pandas matplotlib
   ```

---

### **2. Import Library**
Gunakan library berikut untuk memproses data dan membuat visualisasi:
```python
import numpy as np
import pandas as pd
from pylab import plt, mpl

plt.style.use('ggplot')  # Mengatur gaya visualisasi
mpl.rcParams['font.family'] = 'serif'  # Mengatur font untuk grafik
%matplotlib inline  # Menampilkan grafik langsung di notebook
```

---

### **3. Membaca Data**
Cobalah membaca file CSV yang berisi data harga saham. Jika file tidak ditemukan, buat dataset simulasi:
```python
try:
    data = pd.read_csv('../../source/tr_eikon_eod_data.csv',
                       index_col=0, parse_dates=True)
except FileNotFoundError:
    # Membuat dataset simulasi
    dates = pd.date_range(start='2010-01-01', end='2024-06-29', freq='B')
    spx_values = np.cumprod(1 + np.random.normal(0, 0.001, len(dates))) * 1000
    data = pd.DataFrame(spx_values, index=dates, columns=['.SPX'])
```

---

### **4. Membersihkan Data**
Hapus nilai kosong (jika ada) dan tampilkan informasi dataset:
```python
data = pd.DataFrame(data['.SPX'])  # Memastikan hanya kolom .SPX yang digunakan
data.dropna(inplace=True)  # Menghapus nilai kosong
data.info()  # Menampilkan informasi dataset
```

---

### **5. Menghitung Statistik**
Tambahkan kolom baru untuk pengembalian logaritmik dan volatilitas historis:
```python
data['rets'] = np.log(data / data.shift(1))  # Pengembalian logaritmik
data['vola'] = data['rets'].rolling(252).std() * np.sqrt(252)  # Volatilitas historis
```

---

### **6. Visualisasi Data**
Buat grafik untuk menampilkan harga saham dan volatilitas historis:
```python
data[['.SPX', 'vola']].plot(subplots=True, figsize=(10, 6));
```

---

### **7. Output**
Setelah menjalankan semua langkah, Anda akan mendapatkan:
1. Informasi dataset, seperti jumlah entri, tipe data, dan penggunaan memori.
2. Grafik harga saham (`.SPX`) dan volatilitas historis (`vola`) dalam dua subplot.

---

## **Hasil Akhir**
Modul ini membantu Anda memahami cara memproses data saham, menghitung metrik penting seperti pengembalian logaritmik dan volatilitas, serta memvisualisasikan data untuk analisis lebih lanjut.

--- 

Jika Anda memiliki pertanyaan atau kendala, silakan tanyakan!