#  Analisis Kualitas Udara Jakarta dengan IBM Granite LLM

Proyek ini menggunakan Large Language Model (**LLM**) dari IBM Granite (via Replicate) untuk menganalisis data polusi udara di Jakarta. Model akan memberikan insight kesehatan dan rekomendasi kebijakan berdasarkan data seperti PM10, PM2.5, SO2, CO, O3, dan NO2.

---

## Tujuan

- Memberikan **insight naratif** dari data kualitas udara.
- Mengidentifikasi **dampak kesehatan** dari parameter polusi.
- Menyarankan **rekomendasi** kebijakan dan perbaikan kualitas udara.
- Memanfaatkan **LLM** untuk menjembatani pemahaman masyarakat awam terhadap data lingkungan.

---

## Dataset

Dataset memiliki kolom-kolom berikut:

- `tanggal` – Tanggal pengambilan sampel udara
- `location` – Lokasi pengamatan
- `pm10`, `pm25` – Partikulat berukuran besar dan kecil
- `so2`, `co`, `o3`, `no2` – Gas-gas polutan udara

**Contoh data:**

| tanggal     | location | pm10 | pm25 | so2 | co  | o3  | no2 |
|-------------|----------|------|------|-----|-----|-----|-----|
| 2023-07-01  | Jakarta  | 112  | 85   | 8   | 0.7 | 35  | 20  |

---

## Teknologi yang Digunakan

-  `Python`
-  `pandas` – Manipulasi data
-  `langchain` – Interaksi dengan LLM
-  `replicate` – Menjalankan model Granite via API
-  **Model LLM:** `ibm-granite/granite-3.3-8b-instruct`

---

##  Cara Menjalankan

1. **Install dependensi**

```bash
pip install pandas langchain replicate
```

2. **Setup API Token di Replicate**

3. **Load data & jalankan prompt**

```bash
from langchain_community.llms import Replicate
import pandas as pd
import os

os.environ["REPLICATE_API_TOKEN"] = "tokenmasing2"

# Load data
df = pd.read_csv("data_kualitas_udara.csv")

# Setup LLM
llm = Replicate(
    model="ibm-granite/granite-3.3-8b-instruct",
    model_kwargs={
        "temperature": 0.2,
        "max_new_tokens": 1024,
        "top_p": 0.9
    }
)

# Prompt
prompt = f"""
Berikut adalah data kualitas udara di Jakarta:

Contoh data (10 baris pertama):
{df[['tanggal', 'location', 'pm10', 'pm25', 'so2', 'co', 'o3', 'no2']].head(10).to_string(index=False)}

Tolong jawab beberapa hal berikut:

1. Bagaimana dampaknya terhadap kesehatan?
2. Apa rekomendasi perbaikan untuk kualitas udara di lokasi tersebut?

Berikan jawaban dalam paragraf yang rapi dan mendalam.
"""

# Run model
response = llm.invoke(prompt)
print(response)
```

📋 Contoh Output
1. Dampak Kesehatan:
Nilai PM2.5 dan PM10 yang tinggi menunjukkan risiko tinggi terhadap gangguan pernapasan seperti asma dan bronkitis, terutama pada anak-anak dan lansia...

2. Rekomendasi:
Perlu pengurangan emisi kendaraan bermotor, peningkatan transportasi publik, dan kontrol industri...

## Manfaat Proyek
- Mengedukasi masyarakat umum tentang kualitas udara

- Membantu pemangku kebijakan membuat keputusan berbasis data

- Menggunakan teknologi AI untuk solusi lingkungan berkelanjutan
