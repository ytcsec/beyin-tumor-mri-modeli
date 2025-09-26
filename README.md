# Beyin Tümör MRI Modeli

## Giriş

Bu proje, beyin tümörü MR görüntülerini dört sınıfa ayırmak için derin öğrenme tabanlı bir yaklaşım kullanır: **glioma**, **meningioma**, **no tumor**, ve **pituitary**. Kaggle'dan alınan veri seti, eğitim ve test verilerini içeren iki ana klasörden oluşur (`Training` ve `Testing`). Veri seti, 128x128 piksel boyutuna getirilmiş görüntüler içerir ve model, TensorFlow ve Keras kullanılarak geliştirilmiştir.

### Kullanılan Algoritmalar ve Teknikler
- **Evrişimli Sinir Ağı (CNN)**: Üç bloklu bir CNN modeli, 32, 64 ve 128 filtreli konvolüsyon katmanları, batch normalizasyon, max pooling ve dropout katmanları içerir.
- **Veri Ön İşleme**: Görüntüler 128x128 piksele yeniden boyutlandırılmış, normalizasyon uygulanmış (piksel değerleri [0,1] aralığına ölçeklendirilmiş) ve veri artırma (data augmentation) teknikleri kullanılmıştır.
- **Optimizasyon**: Performansı artırmak için `cache` ve `prefetch` ile veri pipeline'ı optimize edilmiştir.
- **Eğitim Süreci**: Model, `adam` optimizasyon algoritması ve `sparse_categorical_crossentropy` kayıp fonksiyonu ile 16 epoch boyunca eğitilmiştir.
- **Değerlendirme**: Eğitim ve doğrulama veri setleri üzerinde doğruluk (accuracy) ve kayıp (loss) metrikleri hesaplanmıştır.

Projenin teknik detayları, Jupyter Notebook dosyalarında markdown hücreleri ile açıklanmıştır. Notebooklar, veri ön işleme, model mimarisi, eğitim süreci ve sonuç analizini kapsamaktadır.

## Metrikler

Modelin performansı, eğitim ve doğrulama veri setleri üzerinde değerlendirilmiştir. Aşağıdaki metrikler, 16 epoch sonunda elde edilmiştir:

- **Eğitim Doğruluğu**: Yaklaşık %95 (son epoch'ta gözlemlenen değer).
- **Doğrulama Doğruluğu**: Yaklaşık %90 (son epoch'ta gözlemlenen değer).
- **Eğitim Kayıp Değeri**: ~0.15 (son epoch'ta).
- **Doğrulama Kayıp Değeri**: ~0.25 (son epoch'ta).

### Yorum
- Model, eğitim veri setinde yüksek bir doğruluk oranı elde etmiş, ancak doğrulama setinde biraz daha düşük bir performans göstermiştir. Bu, modelin hafif bir **overfitting** eğilimi gösterdiğini işaret edebilir.
- Dropout katmanları (0.2, 0.3, 0.4, 0.5 oranları) ve veri artırma teknikleri, overfitting'i azaltmak için etkili olmuş, ancak daha fazla veri artırma veya düzenlileştirme teknikleri (ör. L2 regularization) performansı iyileştirebilir.
- Sınıf dağılımı analizi, veri setindeki sınıfların dengeli olduğunu göstermiştir. Ancak, **no tumor** sınıfının diğer sınıflara göre biraz daha az örneğe sahip olması, bu sınıfta yanlış sınıflandırmalara yol açabilir.
- Confusion matrix ve classification report gibi ek metrikler, modelin sınıf bazında performansını daha detaylı analiz etmek için kullanılabilir.

Bu metrikler, modelin beyin tümörü sınıflandırması için umut verici bir başlangıç sunduğunu, ancak gerçek dünya uygulamaları için daha fazla optimizasyon gerektiğini göstermektedir.

## Ekler

### Veri Görselleştirme
Proje kapsamında, eğitim ve test veri setlerinin sınıf dağılımları görselleştirilmiştir. Ayrıca, normalize edilmiş örnek görüntüler matplotlib ve seaborn kullanılarak incelenmiştir. Bu görselleştirmeler, veri setinin yapısını ve sınıfların dağılımını anlamak için faydalı olmuştur.

### Deployment
Proje, Streamlit kullanılarak bir web arayüzüne deploy edilebilir. Bu amaçla, `UI` klasöründe örnek bir Streamlit scripti bulunmaktadır. Script, kullanıcıların MR görüntülerini yükleyerek sınıflandırma sonuçlarını almasına olanak tanır. Deployment süreci, modelin gerçek dünya senaryolarında kullanılabilirliğini test etmek için önemli bir adımdır.

### GPU Kullanımı
Eğitim süreci, Kaggle'ın GPU desteği kullanılarak optimize edilmiştir. TensorFlow'un GPU uyumlu kütüphaneleri, modelin eğitim süresini önemli ölçüde azaltmıştır. Özellikle, veri pipeline'ında kullanılan `cache` ve `prefetch` teknikleri, GPU kullanımını daha verimli hale getirmiştir.

## Sonuç ve Gelecek Çalışmalar

Bu proje, beyin tümörü sınıflandırması için sağlam bir temel oluşturmuştur. Elde edilen %90'lık doğrulama doğruluğu, modelin potansiyelini gösterse de, aşağıdaki alanlarda iyileştirmeler yapılabilir:
- **Veri Artırma**: Daha karmaşık veri artırma teknikleri (ör. rotasyon, kaydırma, zoom) eklenerek modelin genelleme yeteneği artırılabilir.
- **Model Mimarisi**: Daha derin veya önceden eğitilmiş modeller (ör. ResNet, EfficientNet) transfer öğrenimi ile kullanılarak performans iyileştirilebilir.
- **Gerçek Zamanlı Veri**: Gerçek zamanlı veri toplama ve işleme için bir pipeline geliştirilebilir. Örneğin, hastanelerden gelen yeni MR görüntüleri ile model sürekli güncellenebilir.
- **Arayüz Geliştirme**: Streamlit tabanlı arayüz, kullanıcı dostu özellikler (ör. görsel sonuç analizi, hata raporları) eklenerek geliştirilebilir.
- **Klinik Entegrasyon**: Modelin, radyologların teşhis sürecine destek olmak için bir klinik karar destek sistemi olarak entegre edilmesi hedeflenebilir.

Gelecekte, bu projeyi tıbbi görüntüleme alanında daha geniş bir veri setiyle test etmek ve farklı derin öğrenme mimarilerini keşfetmek istiyorum. Ayrıca, modelin gerçek dünya senaryolarında kullanılabilirliğini artırmak için bir mobil uygulama geliştirilmesi de uzun vadeli bir hedef olabilir.

## Linkler

- [Kaggle Notebook: Brain Tumor MRI Classification](https://www.kaggle.com/code/your-username/brain-tumor-mri-classification)
- [Kaggle Dataset: Brain Tumor MRI Dataset](https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset)
