# 🧪 Marketing A/B Testing Analysis
### Statistical Hypothesis Testing for Ad Campaign Effectiveness

<p align="left">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Statsmodels-Statistics-4C72B0?style=flat"/>
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white"/>
  <img src="https://img.shields.io/badge/Status-Completed-2ea44f?style=flat"/>
  <img src="https://img.shields.io/badge/p--value-0.0000-red?style=flat"/>
</p>

> Menganalisis efektivitas iklan produk (Ad) dibanding Public Service Announcement (PSA) menggunakan **Two-Sample Z-Test** untuk menentukan apakah perbedaan conversion rate antara dua grup signifikan secara statistik.

---

## 📌 Table of Contents
- [Business Problem](#-business-problem)
- [Dataset](#-dataset)
- [Workflow](#-workflow)
- [Objective & Metric](#1-objective--metric)
- [Hypothesis](#2-hypothesis-formulation)
- [Experiment Design](#3-experiment-design)
- [Statistical Test Results](#-statistical-test-results)
- [Business Impact](#-business-impact)
- [How to Run](#-how-to-run)

---

## 💼 Business Problem

Tim marketing ingin mengetahui apakah menampilkan **iklan produk** lebih efektif dalam mendorong konversi pembelian dibandingkan menampilkan **Public Service Announcement (PSA)**.

Keputusan ini berdampak langsung pada:
- Alokasi anggaran pemasaran
- Strategi penayangan iklan digital
- Potensi peningkatan revenue perusahaan

> **Pertanyaan Bisnis:** Apakah penayangan iklan produk (Ad) secara signifikan meningkatkan conversion rate dibanding PSA?

---

## 🗃️ Dataset

| Info | Detail |
|------|--------|
| Sumber | [Kaggle — Marketing A/B Testing](https://www.kaggle.com/datasets/faviovaz/marketing-ab-testing) |
| File | `marketing_AB.csv` |
| Total User | 588,101 |
| Kolom Utama | `user id`, `test group`, `converted`, `total ads`, `most ads day`, `most ads hour` |
| Target Metrik | `converted` (True/False) |
| Grup | `ad` = iklan produk · `psa` = public service announcement |

---

## 🔄 Workflow

```
Business Problem Definition
        ↓
Objective & Metric Identification
        ↓
Hypothesis Formulation  (H0 vs H1)
        ↓
Experiment Design
  ↓ Group Split · Sample Size · Randomization · Duration
Statistical Testing  (Two-Sample Z-Test)
        ↓
Decision & Business Recommendation
```

---

## 1. Objective & Metric

**Tujuan Utama:**
Menguji apakah penayangan iklan produk (Ad) secara statistik meningkatkan konversi pembelian dibandingkan PSA.

**Metrik Utama:**
| Metrik | Definisi | Sumber Kolom |
|--------|----------|-------------|
| **Conversion Rate** | % user yang melakukan pembelian | `converted` (True/False) |

**Kenapa Conversion Rate?**
- Langsung mengukur dampak bisnis (pembelian nyata)
- Tersedia secara eksplisit di dataset
- Dapat dibandingkan antar grup secara langsung

---

## 2. Hypothesis Formulation

| | Hipotesis |
|--|-----------|
| **H₀** (Null) | Tidak ada perbedaan signifikan conversion rate antara grup Ad dan PSA |
| **H₁** (Alternatif) | Conversion rate grup Ad **lebih tinggi** dari grup PSA |

**Kriteria Keputusan:**
```
Significance Level (α) = 0.05

Jika p-value < 0.05  →  Tolak H₀  (iklan terbukti efektif)
Jika p-value ≥ 0.05  →  Gagal Tolak H₀  (tidak ada bukti cukup)
```

---

## 3. Experiment Design

### 3.1 Pembagian Grup

| Grup | Perlakuan | Jumlah User |
|------|-----------|:-----------:|
| **Control** (PSA) | Melihat Public Service Announcement | 23,524 |
| **Treatment** (Ad) | Melihat iklan produk | 564,577 |
| **Total** | | **588,101** |

### 3.2 Sample Size Calculation

Estimasi minimum sampel yang dibutuhkan agar hasil valid secara statistik:

| Parameter | Nilai |
|-----------|:-----:|
| Baseline Conversion Rate | 10.00% |
| Target Conversion Rate | 12.00% |
| Significance Level (α) | 0.05 |
| Statistical Power (1-β) | 0.80 |
| Effect Size (Cohen's h) | 0.0640 |
| **Minimum per Grup** | **3,835 user** |
| **Total Minimum** | **7,670 user** |

> ✅ Dataset jauh melebihi minimum — **588,101 user** — sehingga hasil pengujian sangat reliable.

### 3.3 Randomization

Setiap user dialokasikan secara acak ke salah satu grup menggunakan `numpy.random.choice`:

```python
df['test group'] = np.random.choice(['control', 'treatment'], size=len(df), replace=True)
```

**Hasil distribusi randomisasi:**
| Grup | Proporsi |
|------|:--------:|
| Control | 50.05% |
| Treatment | 49.95% |

> ✅ Selisih < 0.1% — randomisasi berjalan sempurna, tidak ada bias alokasi.

### 3.4 Durasi Pengujian

| Aspek | Rekomendasi |
|-------|-------------|
| Durasi minimum | 2–4 minggu |
| Alasan | Menangkap variasi hari (weekday vs weekend) |
| Monitoring | Cek daily agar tidak ada novelty effect |
| Stop condition | Setelah minimum sample size terpenuhi |

---

## 📊 Statistical Test Results

**Metode:** Two-Sample Z-Test for Proportions (`proportions_ztest`)

```
=== Hasil Uji Proporsi Dua Sampel (Z-Test) ===

Jumlah konversi (Ad)  : 14,423 dari 564,577 user  →  CR = 2.55%
Jumlah konversi (PSA) :    420 dari  23,524 user  →  CR = 1.79%

Statistik Z  :  7.3701
P-value      :  0.0000
```

**Keputusan:**
```
p-value (0.0000) < α (0.05)  →  TOLAK H₀
```

> ✅ **Terdapat perbedaan signifikan secara statistik** antara conversion rate grup Ad (2.55%) dan PSA (1.79%).

---

## 💡 Business Impact

| Insight | Implikasi |
|---------|-----------|
| Iklan produk menghasilkan CR 2.55% vs PSA 1.79% | Iklan terbukti ~43% lebih efektif dalam mendorong konversi |
| p-value = 0.0000 | Hasil sangat signifikan — bukan karena kebetulan |
| Sample size 588K >> minimum 7.6K | Hasil sangat reliable, dapat dijadikan dasar keputusan |

**Rekomendasi Strategis:**
- ✅ **Alokasikan lebih banyak anggaran** untuk penayangan iklan produk
- ✅ **Kurangi porsi PSA** pada slot iklan berbayar
- ✅ **Optimalkan jam tayang** — analisis `most ads hour` untuk peak conversion time
- ✅ **Lakukan segmentasi** — analisis `most ads day` untuk hari terbaik penayangan

---

## ✅ Strengths & ⚠️ Areas for Improvement

**Kelebihan:**
- Hipotesis jelas dan dapat diuji secara statistik
- Sample size dihitung secara proper (power analysis)
- Randomisasi terbukti seimbang (50/50)
- Z-Test sesuai untuk perbandingan proporsi dua sampel besar
- Hasil sangat signifikan (p ≈ 0)

**Peluang Peningkatan:**
- Analisis segmentasi (per hari, per jam) untuk insight yang lebih dalam
- Uji praktical significance — perbedaan 0.76% apakah cukup material secara bisnis?
- Tambahkan confidence interval untuk estimasi rentang perbedaan CR
- Pertimbangkan Multiple Testing Correction jika ada lebih dari satu metrik

---

## 🔧 Tech Stack

```python
import pandas as pd                                        # Data manipulation
import numpy as np                                         # Randomization
import matplotlib.pyplot as plt                            # Visualization
import seaborn as sns                                      # Visualization

from statsmodels.stats.proportion import proportions_ztest # Z-Test
from statsmodels.stats.proportion import proportion_effectsize
from statsmodels.stats.power import NormalIndPower         # Sample size calculation
```

---

## 🚀 How to Run

```bash
# 1. Clone repository
git clone https://github.com/YOUR_USERNAME/ab-testing-marketing.git
cd ab-testing-marketing

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run notebook
jupyter notebook notebook/AB_testing.ipynb
```

Or open directly in Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/YOUR_COLAB_LINK_HERE)

---

## 👩‍💻 Author

**Lhedya Monica Ismon**
Data Science & Data Analyst Bootcamp — [Dibimbing.id](https://dibimbing.id)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/YOUR_PROFILE)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github)](https://github.com/YOUR_USERNAME)

---

<p align="center"><i>Made with curiosity and lots of ☕ | 2025</i></p>
