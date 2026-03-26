# 🌊 Flood Prediction Project Documentation

Proyek ini bertujuan untuk memprediksi probabilitas terjadinya banjir menggunakan teknik regresi. Berdasarkan hasil evaluasi, model Linear Regression dipilih sebagai model terbaik karena memiliki akurasi ($R^2$ Score) tertinggi dan tingkat kesalahan terkecil dibandingkan algoritma lainnya.

## 📁 Struktur Repositori
```
├── API-Model/
│   ├── main.py              # Script utama untuk menjalankan API prediksi
│   └── model/
│       ├── model_lr_final.joblib  # Model Linear Regression yang sudah dilatih
│       └── scaler.joblib          # File scaler untuk standarisasi data
├── training_data/
│   ├── training.ipynb       # Notebook proses analisis, EDA, dan pelatihan model
│   └── requirements.txt     # Daftar pustaka (library) yang dibutuhkan
└── flood/
    ├── train.csv            # Dataset utama untuk pelatihan
    └── test.csv             # Dataset untuk pengujian
```

## 🛠️ Alur Kerja Machine Learning (Workflow)

Proyek ini mengikuti alur standar Supervised Learning untuk memastikan model dapat digeneralisasi dengan baik pada data baru.
1. Exploratory Data Analysis (EDA): Menganalisis korelasi antar fitur lingkungan terhadap probabilitas banjir.
2. Data Preprocessing:
* Penghapusan fitur yang tidak relevan (seperti id).
* Data Splitting: Membagi data menjadi Training Set dan Test Set sebelum proses scaling.
3. Feature Scaling: Menggunakan StandardScaler untuk menstandarisasi fitur agar memiliki rata-rata 0 dan varians 1.
4. Model Training: Melatih beberapa algoritma regresi (Linear Regression, Gradient Boosting, Lars).
5. Model Evaluation: Membandingkan performa menggunakan metrik MSE, RMSE, dan $R^2$.

## 📊 Hasil Evaluasi Model
| Model | MAE | RMSE | R2 Score |
| :--- | :---: | :---: | :---: |
| **Linear Regression** | **0.0162** | **0.0205** | **0.8365** |
| Gradient Boosting | 0.0260 | 0.0315 | 0.6134 |
| Lars | 0.0408 | 0.0507 | 0.0000 |

Kesimpulan: Linear Regression adalah model terbaik dengan kemampuan menjelaskan variasi data sebesar 83.65%.

## 🚀 Implementasi Kode (API-Model)
File main.py menggunakan FastAPI (atau framework serupa) untuk menyediakan layanan prediksi. Berikut adalah logika inti dalam pemrosesan datanya:

1. Memuat Model dan Scaler
Model dimuat menggunakan pustaka joblib agar siap melakukan inferensi tanpa melatih ulang.
```
import joblib

model = joblib.load('model/model_lr_final.joblib')
scaler = joblib.load('model/scaler.joblib')
```
2. Proses Prediksi
Data input harus melewati tahap scaling yang sama dengan data pelatihan sebelum dimasukkan ke model.
```
# Contoh data input (20 fitur)
features = [intensity, monsoon, deforestation, ...] 

# Standarisasi data
features_scaled = scaler.transform([features])

# Prediksi
probability = model.predict(features_scaled)
```

## 📝 Daftar Fitur Prediktor
Model ini mempertimbangkan 20 faktor lingkungan, antara lain:

* Intensitas Alam: MonsoonIntensity, ClimateChange, RiverManagement.
* Faktor Infrastruktur: DamsQuality, DeterioratingInfrastructure, InadequatePlanning.
* Faktor Lingkungan: Deforestation, Urbanization, Siltation, WetlandLoss.

## 💻 Cara Menjalankan
1. Install dependensi:

```
pip install -r training_data/requirements.txt
```

2. Jalankan notebook training.ipynb untuk melihat proses training model.
3. Jalankan API-Model/main.py untuk mengaktifkan layanan prediksi menggunakan api dengan menggunakan .

## 🚀 Panduan Pengujian API Flood Prediction (Localhost)

Dokumentasi ini menjelaskan cara melakukan request ke API prediksi banjir yang berjalan di komputer lokal Anda.

### 1. Persiapan Server
Sebelum memulai, pastikan server FastAPI Anda sudah berjalan. Buka terminal di folder API-Model dan jalankan:
```
uvicorn main:app --reload
```
Default URL: http://localhost:8000

### 2. Menggunakan Postman (Visual)
Postman adalah cara paling direkomendasikan untuk melihat hasil prediksi secara rapi.

Langkah-langkah:
1. Method & URL: Ubah metode menjadi POST dan masukkan URL: http://localhost:8000/predict.

2. Headers: Pastikan terdapat key Content-Type dengan nilai application/json.

3. Body:

* Pilih tab Body.

* Pilih opsi raw.

Ubah dropdown di ujung kanan dari Text menjadi JSON.

4. Input Data: Masukkan JSON berikut (20 fitur):
```
{
  "MonsoonIntensity": 5,
  "TopographyDrainage": 5,
  "RiverManagement": 5,
  "Deforestation": 5,
  "Urbanization": 5,
  "ClimateChange": 5,
  "DamsQuality": 5,
  "Siltation": 5,
  "AgriculturalPractices": 5,
  "Encroachments": 5,
  "IneffectiveDisasterPreparedness": 5,
  "DrainageSystems": 5,
  "CoastalVulnerability": 5,
  "Landslides": 5,
  "Watersheds": 5,
  "DeterioratingInfrastructure": 5,
  "PopulationScore": 5,
  "WetlandLoss": 5,
  "InadequatePlanning": 5,
  "PoliticalFactors": 5
}
```
5. Klik Send. Hasil prediksi (0.0 - 1.0) akan muncul di kolom Response.

### 3. Menggunakan Terminal (cURL)
catatan: saya menggunakan linux fedora 

Salin dan tempel perintah berikut di terminal Anda:
```
curl -X 'POST' \
  'http://localhost:8000/predict' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "MonsoonIntensity": 5, "TopographyDrainage": 5, "RiverManagement": 5,
  "Deforestation": 5, "Urbanization": 5, "ClimateChange": 5,
  "DamsQuality": 5, "Siltation": 5, "AgriculturalPractices": 5,
  "Encroachments": 5, "IneffectiveDisasterPreparedness": 5,
  "DrainageSystems": 5, "CoastalVulnerability": 5, "Landslides": 5,
  "Watersheds": 5, "DeterioratingInfrastructure": 5, "PopulationScore": 5,
  "WetlandLoss": 5, "InadequatePlanning": 5, "PoliticalFactors": 5
}'
```