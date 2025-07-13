🌆🌡️ Şehir Bazlı Hava Sıcaklığı Tahmini – SARIMA Modeli
🎯 Projenin Amacı
🏙️ Farklı şehirler için geçmiş hava sıcaklığı verilerinden yola çıkarak günlük veya aylık ortalama sıcaklık tahmini yapmak.

📝 Kullanıcıdan şehir adı, tahmin tipi (günlük/aylık) ve tahmin tarihi input olarak alınır; istenilen şehir ve zaman aralığına özel SARIMA tabanlı zaman serisi modeliyle tahmin yapılır.

⚙️ Teknik İş Akışı ve Kodun Adım Adım Özeti
📥 Veri Yükleme

cities.csv dosyası pandas ile okunur.

Tarih sütunu (date) datetime formatına çevrilir.

🎛️ Input ile Parametre Seçimi

Kullanıcıdan şehir adı ve tahmin tipi (günlük/aylık) input ile alınır.

Tahmin tipi günlük ise kullanıcıdan tahmin tarihi (YYYY-MM-DD), aylık ise (YYYY-MM) olarak ayrıca istenir.

Bu inputlar sayesinde model, istenen şehri ve zaman aralığını dinamik olarak analiz eder.

🧹 Veri Filtreleme ve Hazırlama

Sadece seçilen şehir için veri filtrelenir (df_city).

Eksik veriler interpolasyon yöntemiyle doldurulur, uç/aykırı değerler temizlenir.

Günlük tahmin için son 30-60 gün, aylık için tam yıl verisi dikkate alınır.

📊 Zaman Serisi Oluşturma

Günlük tahminde haftalık sezonluk (7), aylıkta yıllık sezonluk (12) kullanılır.

Zaman serisinin son kısmı test seti olarak ayrılır (günlükte genellikle son 30 gün, aylıkta son 12 ay).

🔬 SARIMA Modelinin Kullanılma Nedeni

SARIMA (Seasonal ARIMA), sezonluk ve trendli zaman serilerinde (ör: sıcaklık) hem geçmiş trendi hem de mevsimselliği hesaba katar.

Kodda model parametreleri olarak (order=(1,1,1), seasonal_order=(1,1,1,seasonality)) kullanılır.

SARIMA, sadece ileriye dönük tahmin yapmakla kalmaz, aynı zamanda geçmiş dönemdeki test verisi üzerinde de gerçekçilik sağlar.

🏗️ Model Eğitimi ve Tahmin

Model yalnızca eğitim setiyle eğitilir.

Test setine karşı “gerçek geçmişi bilmeden” tahmin yürütülür.

Böylece modelin gerçek doğruluğu bilimsel olarak objektif şekilde ölçülür (overfitting önlenir).

🧪 Test Verisinin Önemi

Modelin performansı, daha önce görmediği (future data gibi davranan) test verisi üzerinde hesaplanır.

Bu bilimsel olarak bir modelin gerçek dünyadaki tahmin gücünü ölçmenin tek yoludur.

Kodda RMSE (Root Mean Squared Error) ile hata miktarı hesaplanır: Düşük RMSE yüksek doğruluk demektir.

📅 Gelecek Tahmini ve Güven Aralığı

Eğitim sonrası model, seçilen şehir ve tarihe kadar geleceğe dönük tahmin üretir.

Kod, bu tahminin yanında %95 güven aralığını da grafikle gösterir.

Bu güven aralığı, modelin tahmin ettiği değerin belirsizliğini/olasılık sınırlarını ortaya koyar.

Geleceğe dair tahminlerde her zaman bir belirsizlik vardır; güven aralığı dar ise model o kadar emindir.

📈 Grafiklerin Teknik İçeriği

Tüm gerçek sıcaklık değerleri (mavi çizgi)

Test dönemi gerçek sıcaklık (yeşil çizgi)

Test dönemi model tahmini (kırmızı kesikli çizgi)

Test başlangıcı (dikey gri çizgi) — burada model geçmişi görmeyi bırakıp sadece geleceği tahmin etmeye başlar

Gelecek tahmin ve %95 güven aralığı (turuncu/şeffaf bant)

Grafikler, modelin hem geçmişte (test) ne kadar başarılı olduğunu, hem de geleceğe dair öngörüsünü aynı anda görmeyi sağlar.

🧩 Kodun Güçlü Yönleri
⚡ Parametrik: Her şehir ve her zaman aralığı için yeniden kullanılabilir.

🧑‍🔬 Bilimsel: Hem backtest (geçmiş doğrulama) hem de gerçek zamanlı tahmin aynı kodda entegre.

🌀 Sezonluk yapıları (mevsimsel değişimler, yıllık/dönemsel döngü) tespit ve modellemede SARIMA sayesinde çok daha başarılı.

✅ Gerçekçi performans ölçümü: Sadece eğitime değil, test verisine bakılarak objektif başarı analizi.

🚀 Kullanım ve Uygulama
📂 cities.csv dosyasını ana dizinde tut.

🛠️ Gerekli kütüphaneleri yükle: pandas, numpy, matplotlib, statsmodels.

💻 Notebook’u başlat veya script’i çalıştır.

🏙️ İstenilen şehir, tahmin tipi ve tarih bilgilerini input ile gir.

📊 Sonuçları grafikler ve hata metrikleriyle birlikte görüntüle.

🧠 Bilimsel ve Pratik Sonuç
🌍 Bu yapı, hava sıcaklığı gibi doğal olarak sezonluk ve trendli (yani yıl boyu tekrar eden dalgalanmalı) seriler için endüstri standardı bir yöntemdir.

👩‍💻 Kod hem akademik sunumda, hem sektörel uygulamada veri bilimciye veya karar vericiye gerçekçi öngörüler sunar.

🕵️‍♂️ Herhangi bir şehir veya dönem için modelin güvenilirliğini, öngörü kabiliyetini ve potansiyel hata payını anlamak mümkündür.

NOT: Aylık veri analizi çok iyi sonuçlar verirken günlük sonuçlar çok iyi sonuç vermez. bunun sebebi:
SARIMA’nın Doğası 
SARIMA zaman serisinin trend ve mevsimsel bileşenlerini yakalamada iyi, fakat “spike” yani ani, öngörülemez değişimlerde başarısız.

Günlük sıcaklık tahmini için; dışsal değişkenli, daha kompleks modeller (ör: Prophet, LSTM, Random Forest, hatta hava tahmini için meteorolojik modeller) gerekebilir.

SARIMA’nın tüm “gücünün” çıktığı alan aylık ve daha üst zaman pencereleri.
Anahtar Kelimeler:

SARIMA

Zaman Serisi Analizi

Time Series Forecasting

Hava Sıcaklığı Tahmini

Weather Prediction

Şehir Bazlı Tahmin

City-based Forecast

Günlük Tahmin

Daily Forecast

Aylık Tahmin

Monthly Prediction

Mevsimsellik

Seasonality

Trend Analizi

Trend Analysis

Eğitim Seti

Training Set

Test Seti

Model Performansı

Model Performance

RMSE

Eksik Veri Doldurma

Data Interpolation

Outlier Cleaning

Aykırı Değer Temizleme

Pandas

Python

Matplotlib

Statsmodels

Veri Gürültüsü

Data Noise

Oynaklık

Volatility

Model Accuracy

Güven Aralığı

Confidence Interval

Grafiksel Raporlama

Visualization

Backtesting

Model Sınırları

Model Limitations

Exogenous Variables

İklim Analizi

Climate Analysis

Parametrik Modelleme

Parametric Modeling

Zaman Dizisi Modeli

Time Series Model

Doğrulama

Validation

Sezonluk Farklılıklar

Seasonal Differences

Otomatik Şehir Analizi

Auto City Analysis

Yüksek Frekanslı Tahmin

High Frequency Prediction

Veri Bilimi

Data Science

Makine Öğrenmesi

Machine Learning

Tahmin Edilebilirlik

Predictability

Grafiksel Analiz

Graphical Analysis




