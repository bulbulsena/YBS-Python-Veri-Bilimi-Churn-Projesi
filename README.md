# Maliyet Odaklı Müşteri Kaybı (Churn) Tahmini ve Finansal Simülasyon

**Bursa Uludağ Üniversitesi — Yönetim Bilişim Sistemleri Bölümü** **Python ile Veri Bilimi Dersi Dönem Sonu Projesi (Tema 2 — Hata Maliyeti Odaklı Müşteri Analitiği)**

---

## 📌 Proje Özeti
Bu projede, standart makine öğrenmesi yaklaşımlarının (%50 sabit eşik değeri) ötesine geçilerek, **işletme maliyetleri ve hata zararları** göz önünde bulundurulan bir müşteri kaybı (churn) tahmin modeli geliştirilmiştir. Proje kapsamında harici bir makroekonomik veri seti sisteme entegre edilmiş, iş mantığına dayalı yeni özellikler türetilmiş ve şirket kârlılığını maksimize eden **Optimum Karar Eşiği (Threshold Optimization)** hesaplanarak somut bir finansal simülasyon (ROI) sunulmuştur.

---

## 🚀 Zorunlu Proje Kriterleri & Uygulama Muhteviyatı

### 1. Veri Harmanlama (Data Fusion)
Projede tek bir hazır veri setiyle yetinilmemiş, iki farklı kaynaktan gelen veriler zaman/dönem bazlı olarak başarıyla birleştirilmiştir:
* **Ana Veri Seti:** IBM Telco Customer Churn veri seti (Müşteri demografileri, hizmet bilgileri ve churn durumu).
* **Harici Veri Seti:** TCMB (Türkiye Cumhuriyeti Merkez Bankası) gerçek USD/TRY aylık geçmiş kur verileri (`kur_verisi.csv`).
* *Entegrasyon:* İki veri seti `pandas.merge_asof` yöntemi kullanılarak müşteri abonelik dönemlerine göre hatasız bir şekilde harmanlanmıştır.

### 2. İş Mantığına Dayalı Özellik Mühendisliği (Feature Engineering)
Model başarımını artırmak ve iş stratejilerine yön vermek amacıyla tamamen özgün **4 yeni değişken** türetilmiştir:
1.  `aylik_maliyet_yuku`: Müşterinin aylık harcamasının, kur riski dönemindeki maliyet yüküne oranı.
2.  `kur_donem_riski`: Kur hareketlerine göre dönemlerin risk seviyesi (Düşük, Orta, Çok Yüksek).
3.  `toplam_hizmet_sayisi`: Müşterinin şirketten satın aldığı toplam ek hizmet sayısı.
4.  `sadakat_skoru`: Müşteri bağlılığı ve aldığı hizmet yoğunluğunun ağırlıklı kombinasyonu.

### 3. İş Senaryosu & Finansal Simülasyon (Threshold Optimization)
* **Maliyet Matrisi:** Gerçek hayattaki iş senaryosuna uygun olarak; bir churn müşterisini kaçırmanın maliyeti (False Negative = 520 TL), sadık bir müşteriye gereksiz kampanya yapma maliyetinden (False Positive = 100 TL) **5.2 kat daha yüksek** olarak tanımlanmıştır.
* **Optimizasyon:** Standart `0.50` karar eşiği yerine şirket kârını maksimize eden optimum eşik değeri **`0.35`** olarak tespit edilmiştir.
* **Finansal Çıktı:** Karar eşiğinin 0.35'e çekilmesiyle sadece test setinde **29.770 TL net kâr** elde edilmiştir (Tüm müşteri tabanına ölçeklendiğinde yaklaşık ~150.000 TL kazanç).

### 4. Model Doğrulama & Sızıntı (Data Leakage) Testi
* **Overfitting Kontrolü:** 5-Katlı Çapraz Doğrulama (Stratified 5-Fold CV) skoru (**0.846**) ile saf test seti skoru (**0.837**) arasındaki farkın %1'den küçük olması, modelin ezberlemediğini ve yüksek genelleme yeteneğine sahip olduğunu kanıtlamaktadır.
* **Sızıntı Kontrolü:** Hedef değişken (Churn) ile özellikler arasındaki korelasyonlar incelenmiş, en yüksek korelasyonun 0.396 (Contract) olduğu görülerek modelde hiçbir veri sızıntısı (data leakage) olmadığı doğrulanmıştır.

### 5. Açıklanabilir Yapay Zeka (XAI - SHAP)
Model kararlarının şeffaflığı için SHAP kütüphanesi kullanılmıştır. Analiz sonucunda, türetilen `aylik_maliyet_yuku` değişkeninin, sözleşme türünden (`Contract`) sonra **modeli etkileyen en önemli 2. faktör** olduğu tespit edilerek özellik mühendisliğinin başarısı kanıtlanmıştır.

---

## 📂 Repo İçeriği

* `Churn_Analiz_Projesi_FINAL.ipynb` -> Veri önişleme, modelleme ve simülasyon kodlarını içeren Jupyter Notebook dosyası.
* `Churn_Rapor_SenaBulbul.pdf` -> Yönetici özeti niteliğindeki 5 sayfalık resmi proje raporu.
* `WA_Fn-UseC_-Telco-Customer-Churn.csv` -> IBM Telekomünikasyon müşteri veri seti.
* `kur_verisi.csv` -> TCMB entegrasyonu için hazırlanan makroekonomik döviz verisi.
* `*.png / *.jpg` -> Raporda ve analizde kullanılan görsel grafikler (EDA, ROC Eğrisi, Threshold Simülasyonu, SHAP Önem Sırası).

---

## 🛠️ Kullanılan Teknolojiler
* **Programlama Dili:** Python 3.x
* **Veri Analizi & Manipülasyon:** `pandas`, `numpy`
* **Makine Öğrenmesi & Doğrulama:** `scikit-learn`, `LightGBM / XGBoost`
* **Açıkl
