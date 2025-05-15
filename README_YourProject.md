YMT5270 Ara Sınav Projesi: Orange ile Veri Analizi ve Makine Öğrenmesi
Öğrenci Bilgileri
•	Ad Soyad: Ayça AFACAN
•	Öğrenci Numarası: 242137113
•	E-posta: aycafacan8@gmail.com
Proje Özeti
Bu projede ilk olarak veri temizleme ve ön işleme adımları uygulanmıştır. Eksik veriler analiz edilerek uygun yöntemlerle işlenmiştir. Ardından, aykırı değerler Z-skoru ve kutu grafikleri ile tespit edilmiştir. Kategorik değişkenler one-hot encoding yöntemiyle kodlanmıştır.
Random forest algoritması kullanılmıştır ve %78’in üzerinde doğruluk oranı ile başarılı bir model olmuştur. 
Veri Seti
Veri Seti Bilgileri
•	Veri Seti Adı: Heart Disease 
•	Kaynak: https://archive.ics.uci.edu/dataset/45/heart%2Bdisease
•	Veri Seti Boyutu: 303 satır 14 sütun
Veri Seti Tanımı
Bu çalışmada, kalp hastalığı teşhisini tahmin etmeye yönelik açık kaynaklı “heart.csv” veri seti kullanılmıştır. Veri seti, bireylerin yaş, cinsiyet, kan basıncı, kolesterol, maksimum kalp atım hızı gibi çeşitli sağlık birimlerini içermektedir. Hedef değişken target, kişinin kalp hastalığına sahip olup olmadığını belirtir (1 = hasta, 0 = sağlıklı).


Öznitelik Açıklamaları
Öznitelik Adı	Veri Tipi	Açıklama	Örnek Değer
Age	Sayısal	Kişinin yaşını temsil eder	63
Sex	Kategorik	Kişinin cinsiyetini belirtir 1=erkek,0=kadın	1
Cp	Sayısal	Göğüs ağrısı tipidir. 0=tipik anjina,1=atipik a.,2=ağrısız ve 3=asimptomatik	3
Trestbps	Sayısal	İstirahat halindeki kan basıncı	145
Chol	Sayısal	Serum kolesterol seviyesi	233
Fbs	Kategorik	Açlık kan şekeri seviyesi 120 mg/dl'nin üzerinde mi? 0=hayır,1=evet	1
Restecg	Sayısal	İstirahat halindeki EKG sonuçları. 0=normal,1=anormal,2=sol ventrikül	0
Thalach	Sayısal	Kişinin egzersiz sırasında ulaştığı maksimum kalp atış hızı.	150
Exang	Kategorik	Egzersize bağlı göğüs ağrısı oluştu mu? 0=hayır,1=evet	0
Oldpeak	Sayısal	Egzersizden sonra ST segmentinde oluşan depresyon	2.3
Slope	Sayısal	Egzersiz sırasında ST segmentinin eğimi. 0=düz,1=yukarı eğimli,2=aşağı eğimli ventrikül	0
Ca	Sayısal/Kategorik	Floroskopi ile boyanmış majör damar sayısı.	0
Thal	Sayısal	Talasemi durumu. 0=bilinmiyor, 1=normal, 2=sabit bozuklık ve 3=reversibl bozuklık	1
Target	Kategorik	Kalp hastalığı durumu. 0=sağlıklı, 1=hasta	1
Keşifsel Veri Analizi (Explanatory Data Analysis - EDA)
Temel İstatistikler
Ölçüm	Açıklama
count	        Her sütunda kaç geçerli (eksik olmayan) değer olduğu
mean	        Ortalama değer
std	        Standart sapma
min	        Minimum değer
25%	        1. çeyrek dilim
50% (median)	        Medyan (orta değer)
75%	        3. çeyrek dilim
max	        Maksimum değer.  
Bu istatistikler, veri dağılımı hakkındadır. Örneğin, yaş (age) için ortalama, medyan ve maksimum değerleri karşılaştırarak yaş dağılımında çarpıklık olup olmadığını gözlemleyebiliriz.

 [görsel1 - [Distributions](https://github.com/FiratUniversity-FerhatUcarsLAB/ymt5270-arasinav-projesi-aycafacan/blob/main/img/Ekran%20Resmi%202025-05-14%2014.53.31.png) ]

Veri Ön İşleme
a) Eksik Veriler
•	df.isnull().sum() ve missing_percentage = (df.isnull().sum() / len(df)) * 100 ile eksik veriler analiz edildi.
•	Ayrıca missingno.matrix(df) ile eksik veriler görselleştirildi.
•	Ancak kodda eksik verilerin temizlenmesi (örn. silinmesi veya doldurulması) yapılmadı. Uygulamada eksik veri bulunmadığı için bu adım atlanmıştır. 
b) Aykırı Değerler
•	sns.boxplot(x=df['age']) ile aykırı değerler boxplot üzerinden görselleştirildi.
•	z_scores = stats.zscore(...) ile Z-skoru hesaplandı. Bu sayede ±3 standart sapmanın dışındaki aykırı değerler tespit edilebilir.
c) Veri Normalizasyonu
•	Kodda ölçekleme işlemi yapılmamış. Bu tür işlemler özellikle distance-based algoritmalar için önemlidir ama Logistic Regression genelde one-hot encoded veriyle iyi çalıştığı için burada yapılmamıştır.
d) Kategorik Verilerin Kodlanması
•	pd.get_dummies(..., drop_first=True) ile nominal kategoriler one-hot encoding'e çevrilmiş.
•	drop_first=True parametresi ile her kategoriden bir değer düşülerek çoklu doğrusal bağlantı (multicollinearity) riski azaltılmış.
e) Diğer
•	Özellikler (X) ve hedef (y) ayrımı yapılmış.
•	Eğitim ve test verileri %66/%34 oranında ayrılmış 

Görselleştirmeler


Görselleştirme 1: [Distributions]
[distributions görsel   https://github.com/FiratUniversity-FerhatUcarsLAB/ymt5270-arasinav-projesi-aycafacan/blob/main/img/Ekran%20Resmi%202025-05-14%2014.53.31.png]
  
Distributions widget, veri setindeki sayısal ve kategorik özniteliklerin değer dağılımını görselleştirmek için kullanılır. Histogram ve çubuk grafiklerle değişkenlerin nasıl dağıldığını gösterir, sınıflara göre karşılaştırma yaparak aykırı değerleri ve veri dengesizliklerini analiz etmeye yardımcı olur. Veri setinde erkekler sayıca daha fazladır. Kadınlarda da hasta olanların oranı erkeklere göre daha yüksektir.
Görselleştirme 2: [Test and Score]
[test and score görsel  https://github.com/FiratUniversity-FerhatUcarsLAB/ymt5270-arasinav-projesi-aycafacan/blob/main/img/Ekran%20Resmi%202025-05-15%2012.50.49.png]


 










Test and Score widget’ı, eğitilmiş modelleri test etmek için kullanılır. Veriyi, eğitim ve test verisi olarak böler. Bu veri setini %66 eğitim ve %34 test olacak şekilde böldük.
Öznitelik İlişkileri
a) Korelasyon Matrisi
correlation_matrix = df.corr()
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm')
•	Bu matris, sayısal değişkenler arasındaki doğrusal ilişkileri gösterir.
•	Örneğin: thalach (maksimum kalp atım hızı) ile target pozitif korelasyona sahipse, yüksek kalp atım hızı hastalık riskinin düşük olabileceğini gösterir.
b) Dağılım Grafiği (Scatterplot)
sns.scatterplot(x='age', y='chol', hue='target', data=df)
•	Yaş ve kolesterol arasındaki ilişkiyi ve bu ilişkinin hedef değişkene göre değişimini gösterir.
c) Boxplot (Hedef ile yaş ilişkisi)
sns.boxplot(x='target', y='age', data=df)
•	Kalp hastalığı olan ve olmayan grupların yaş dağılımı bu grafikle analiz edilmiştir.
Makine Öğrenmesi Uygulaması
Kullanılan Yöntem
a) Yöntem: Lojistik Regresyon
model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)
•	Lojistik regresyon sınıflandırma için kullanılan doğrusal bir modeldir.
•	max_iter=1000 ile modelin maksimum iterasyon sayısı arttırılarak öğrenme sürecinin tamamlanması sağlanmış.

Modeller ve Parametreler
b) Model Performansı
accuracy_score(y_test, y_pred)
classification_report(y_test, y_pred)
confusion_matrix(y_test, y_pred)

Metot	Açıklama
Accuracy Score	     Modelin genel doğruluk oranı
Classification Report	     Precision, recall, f1-score gibi sınıfa özel performans ölçümleri
Confusion Matrix	     Gerçek ve tahmin edilen değerlerin karşılaştırılması (True      Positive, False Negative vs.)
•	Görselleştirilmiş konfüzyon matrisi ile modelin hangi sınıfta daha başarılı olduğunu görmek kolaylaşır.
[logistic regression görsel]
 





Model Değerlendirmesi
Doğruluk (Accuracy): Modelin doğru sınıflandırdığı örneklerin tüm örnekler içindeki oranını ifade eder. Bu projede modelin doğruluğu %78 olarak hesaplanmıştır.
F1-Score: Precision ve Recall değerlerinin harmonik ortalamasıdır. Dengesiz veri setlerinde özellikle önemlidir. Pozitif sınıf için F1 skoru 0.78 olarak bulunmuştur.
Hassasiyet (Recall): Gerçek pozitiflerin ne kadarını doğru tahmin ettiğimizi gösterir. Yani modelin hastalıklı bireyleri ne kadar doğru yakalayabildiğini ifade eder. Bu projede 0.78’dir.
Kesinlik (Precision): Modelin pozitif tahminlerinden ne kadarı gerçekten pozitifti sorusunun cevabıdır. 0.78 değeriyle modelin pozitif tahminlerinin büyük çoğunluğu doğru çıkmıştır.

Metrikler
Metrik	Değer
Doğruluk (Accuracy)	0.78
F1- Score (Pozitif Sınıf)	0.78
Hassasiyet (Recall)	0.78
Kesinlik (Precision)	0.78
Sonuçların Yorumlanması
•	Modelin doğruluk oranı (örnek olarak): Doğruluk Skoru: 0.75 gibidir.
•	Lojistik regresyon modeli, kategorik verilerin one-hot encoding ile işlenmesinden sonra etkili çalışmıştır.
•	Veri setinde eksik veri olmadığı için bu adım atlanmış.
•	Aykırı değerler tespit edilse de modelden çıkarılmamış; bu, model performansını bir miktar etkileyebilir.
•	Korelasyon matrisinden çıkan sonuçlara göre thalach, cp, exang, oldpeak gibi öznitelikler hedef değişkenle önemli ilişki göstermektedir.
•	Kodda sadece bir model denenmiş. Alternatif algoritmalar (Random Forest, SVM, XGBoost) ile karşılaştırma yapılması model performansını daha doğru değerlendirmeyi sağlar.
Orange İş Akışı
[orange iş akışı görsel     https://github.com/FiratUniversity-FerhatUcarsLAB/ymt5270-arasinav-projesi-aycafacan/blob/main/img/Ekran%20Resmi%202025-05-12%2011.21.41.png]

1.	File
2.	Data Table
3.	Learner Items (Logistic Regression, Random Forest, SVM)
4.	Test and Score
5.	Confusion Matrix
6.	Data Table (1)
Sonuç ve Öneriler
Bu çalışmada, "heart.csv" veri seti kullanılarak kalp hastalığı tahmini için temel bir makine öğrenmesi süreci gerçekleştirilmiştir. Veriye ilişkin ön işleme adımları eksik değer analizi, kategorik değişkenlerin one-hot encoding yöntemiyle dönüştürülmesi, aykırı değerlerin tespiti (boxplot ve Z-skoru), korelasyon analizi ve veri görselleştirme ile sistematik şekilde uygulanmıştır. Bu süreç, modelin daha sağlıklı öğrenebilmesi adına önemli bir temizlik ve dönüşüm aşamasını temsil etmektedir.
Modelleme aşamasında lojistik regresyon algoritması tercih edilmiştir ve veriler %66 eğitim, %34 test olacak şekilde ayrılmıştır. Modelin test verisi üzerindeki doğruluk skoru yaklaşık olarak %78 seviyesinde gerçekleşmiştir. Konfüzyon matrisi analizi, modelin hem pozitif (hastalıklı) hem de negatif (sağlıklı) sınıfları ayırt etme konusundaki başarısını açıkça göstermektedir. Ayrıca sınıflandırma raporunda yer alan precision, recall ve F1-score değerleri de modelin genel doğruluğuna katkı sağlamıştır.
Veri setindeki yaş, kolesterol ve hedef değişkenleri arasındaki ilişkiler görselleştirmeler aracılığıyla analiz edilmiş ve bu değişkenlerin hastalık sınıflandırmasında önemli rol oynadığı gözlemlenmiştir. Özellikle yaş ve kolesterol seviyelerinin belirli aralıklarda daha fazla risk teşkil ettiği anlaşılmıştır.
Çıkarımlar ve Öneriler
•	Veri ön işleme aşamaları modelin doğruluğu açısından kritik rol oynamaktadır. Aykırı değerlerin belirlenmesi ve uygun şekilde yönetilmesi, modelin öğrenme sürecine pozitif katkı sağlamaktadır.
•	Lojistik regresyon basit yapılı ve yorumlanabilir bir modeldir; ancak daha yüksek doğruluk oranları için rastgele orman (Random Forest), destek vektör makineleri (SVM) ya da gradyan artırma (Gradient Boosting) gibi daha gelişmiş algoritmalarla karşılaştırmalı analizler yapılabilir.
•	Veri seti dengeli görünmekle birlikte daha büyük ve çeşitli veri kümeleriyle modelin genellenebilirliği artırılabilir.

Kaynaklar
1.	Orange Data Mining youtube kanalı.
2.	Juan Luis Restituyo youtube kanalı.
3.	ChatGPT
4.	UC Irvine Machine Learning Repository
5.	Medium.com
Ekler
Orange Proje Dosyası
https://github.com/FiratUniversity-FerhatUcarsLAB/ymt5270-arasinav-projesi-aycafacan/blob/main/project/YMO.ows
Veri Seti Dosyası veya Bağlantısı
https://archive.ics.uci.edu/dataset/45/heart%2Bdisease  
![image](https://github.com/user-attachments/assets/6cf1c26c-c497-4ba2-8030-a16bf4c29726)
