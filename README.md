# 📦 E-Ticaret Ürün Yorumları Duygu Analizi  
*PySpark · Google Colab · Türkçe Metin*

---

## 🚀 Projenin Amacı
Türkiye pazarından toplanan **ürün yorumlarını** otomatik olarak  
`negative / positive / neutral` kategorilerine ayırmak.

- **Olumsuz** geri bildirim → hızlı aksiyon  
- **Olumlu** yorum → pazarlama vitrininde öne çıkarma  
- **Nötr** yorum → ürün geliştirme içgörüsü

---

## 🗂️ Veri Kümesi

| Dosya | Kaynak | Satır | Sütunlar |
|-------|--------|-------|----------|
| `e-ticaret_urun_yorumlari.csv` | Kaggle | ≈ 3 000 | `Metin`, `Durum` (0 = neg, 1 = pos, 2 = neu) |

---

## 🛠️ Kullanılan Teknolojiler

| Katman | Teknoloji |
|--------|-----------|
| Veri İşleme | **Apache Spark 3** (PySpark API) |
| Çalışma Ortamı | **Google Colab** (Python 3.10 · OpenJDK 8) |
| ML | Spark ML (Pipeline, Logistic Regression) |
| Görselleştirme | Matplotlib |
| Değerlendirme | Spark ML `MulticlassClassificationEvaluator`, scikit-learn `confusion_matrix` |
| Sürüm Kontrol | Git + GitHub |

---


## 🔄 İş Akışı

```mermaid
graph TD
  A[Colab Ortam Kurulumu] --> B[CSV Veri Okuma]
  B --> C[Ön İşleme Pipeline<br>(Tokenizer ➔ StopWords ➔ HashingTF ➔ IDF)]
  C --> D[Lojistik Regresyon Eğitimi]
  D --> E[Doğrulama & Karışıklık Matrisi]
  E --> F[Inference Pipeline<br>(API'ye Hazır Çıktı)]
