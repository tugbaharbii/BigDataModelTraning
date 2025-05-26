# BigDataModelTraning

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
1️⃣ Ortam Kurulumu
bash
Kopyala
Düzenle
!apt-get update -qq
!apt-get install -y openjdk-8-jdk-headless -qq
export JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
2️⃣ CSV Okuma
python
Kopyala
Düzenle
spark_df = (
    spark.read.option("delimiter",";").option("header","true")
         .csv("/content/e-ticaret_urun_yorumlari.csv")
)
spark_df.printSchema()
spark_df.show(5, truncate=False)
3️⃣ Ön İşleme & Özellik Çıkarımı
Aşama	Spark Bileşeni	Açıklama
① Tokenize	Tokenizer	Metni kelimelere böl
② Stop Word	StopWordsRemover	TR + özel liste
③ TF	HashingTF (20 000 özellik)	
④ IDF	IDF	Nadir kelimelere ağırlık
⑤ Label Index	StringIndexer	
⑥ Model	LogisticRegression	

python
Kopyala
Düzenle
pipeline = Pipeline(stages=[
    label_indexer, tokenizer, stopper, hashingTF, idf, lr
])
4️⃣ Model Eğitimi
python
Kopyala
Düzenle
train_df, val_df = spark_df.randomSplit([0.8, 0.2], seed=42)
model = pipeline.fit(train_df)
5️⃣ Doğrulama Sonuçları
python
Kopyala
Düzenle
preds = model.transform(val_df)
accuracy = MulticlassClassificationEvaluator(
              labelCol="labelIndex", predictionCol="prediction",
              metricName="accuracy").evaluate(preds)
print(f"Validation Accuracy : {accuracy:.3f}")
Sınıf	Precision	Recall
Negative	0.91	0.83
Positive	0.85	0.89
Neutral	0.41	0.48

Genel doğruluk ≈ % 82,5

6️⃣ Karışıklık Matrisi
python
Kopyala
Düzenle
from sklearn.metrics import confusion_matrix
import matplotlib.pyplot as plt, numpy as np

cm = confusion_matrix(
    preds.select("labelIndex").toPandas(),
    preds.select("prediction").toPandas(),
    labels=[0,1,2]
)
classes = ["negative","positive","neutral"]
plt.imshow(cm, cmap="Blues")
plt.xticks(np.arange(3), classes); plt.yticks(np.arange(3), classes)
plt.title("Confusion Matrix"); plt.xlabel("Predicted"); plt.ylabel("True")
for i in range(3):
    for j in range(3):
        plt.text(j,i, cm[i,j], ha="center", va="center")
plt.show()
7️⃣ Inference Pipeline (Üretim)
python
Kopyala
Düzenle
inference_model = PipelineModel(stages=model.stages[1:])  # StringIndexer hariç
sentences = ["Ürün beklediğim gibi gelmedi.", "Kargo çok hızlıydı!", ...]
test_df = spark.createDataFrame([Row(Metin=s) for s in sentences])
pred_df = inference_model.transform(test_df)

to_label = udf(lambda i: model.stages[0].labels[int(i)], StringType())
result = (pred_df
          .withColumn("label", to_label("prediction"))
          .select("Metin","label","probability"))
result.show(truncate=False)
📈 Öne Çıkan Sonuçlar
% 82,5 genel doğruluk

Negatif yorumların %91’i, pozitiflerin %85’i doğru tahmin edildi.

Neutral sınıf zayıf; veri dengesizliği ve belirsiz dil yapısı etkili.

🔧 Gelecek İyileştirmeler
Bi/tri-gram özellikleri

Lemmatization / stemming (Spark NLP)

CrossValidator ile regParam, elasticNet, numFeatures taraması

Sınıf ağırlığı veya veri çoğaltma (neutral)

Alternatif modeller: LinearSVC, NaiveBayes

📑 Çalıştırma Adımları
Colab’te bu not defterini açın

Kurulum hücresini çalıştırın

Hücreleri sırasıyla çalıştırarak modeli eğitin

sentences listesine kendi cümlenizi ekleyin, sonucu inceleyin

🤝 Katkı & Lisans
Pull Request ve Issue’lara açığız.
Detaylar için LICENSE ve CONTRIBUTING.md dosyalarına göz atın.

Hazırlayan : Kaan Şengün · “AI Prensesi” 🛸✨
© 2025

Kopyala
Düzenle








Araçlar



ChatGPT hata yapabilir. Önemli bil
