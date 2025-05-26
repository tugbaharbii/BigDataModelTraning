# BigDataModelTraning

# 📦 E-Ticaret Ürün Yorumları Duygu Analizi  
*PySpark + Google Colab*

---

## 🚀 Projenin Amacı
Türkiye pazarından toplanmış ürün yorumlarını **otomatik olarak**  
“negative / positive / neutral” sınıflarına ayırmak.

- **Olumsuz** geri bildirimleri hızla saptayıp aksiyon alma  
- **Olumlu** yorumları pazarlama vitrininde öne çıkarma  
- **Nötr** geri bildirimlerden geliştirme fikirleri çıkarma  

---

## 🗂️ Veri Kümesi

| Dosya | Kaynak | Boyut | Sütunlar |
|-------|--------|-------|----------|
| `e-ticaret_urun_yorumlari.csv` | Kaggle | ≈ 3 000 satır | `Metin`, `Durum` (0/1/2) |

---

## 🛠️ Kullanılan Teknolojiler

| Katman | Teknoloji |
|--------|-----------|
| Veri İşleme | **Apache Spark 3** (PySpark API) |
| Ortam | **Google Colab** (Python 3.10 - OpenJDK 8) |
| Görselleştirme | Matplotlib |
| Değerlendirme | Spark ML `MulticlassClassificationEvaluator`, scikit-learn `confusion_matrix` |
| Sürüm Kontrol | Git + GitHub |

---

## 🔄 İş Akışı Özeti

1. **Ortam Kurulumu**

   ```bash
   !apt-get update -qq
   !apt-get install -y openjdk-8-jdk-headless -qq
   export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
