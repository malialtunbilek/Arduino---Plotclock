# Arduino---Plotclock

Arduino ile Saati Yazan Robot - Plotclock


Merhaba arkadaşlar, bu yazımızda Arduino ile mevcut saat bilgisini, kullandığı kalem sayesinde önündeki tahtaya yazan bir robotik kol yapacağız.


Bu projenin mantığını basitce anlatmak gerekirse Arduino’nun RTC modülünü kullanarak mevcut saati kendi kendine yazan bir robot koldur. Saat bilgisine göre devrede bulunan servo motorların hareketini belirliyoruz ve bu servo motorlar robot kolun ucundaki kaleme rakamları yazabilmesi için belirli hareketleri yaptırıyor ve kalemin alt kısmında bulunan tahtaya mevcut saati yazıyor. 

Not : Arduino’ nun gücünü kestiğimiz zaman RTC’ deki saat bilgisi ilk değerini alır. O yüzden Arduino’ nun gücünü kestikten sonra tekrar güç vermeden önce kodumuzda RTC’nin ilk saat bilgisi değerini o anki mevcut saat ile değiştirmeniz gerekir eğer robotunuzun güncel saati yazmasını istiyorsanız tabi 😊


Plotclock  projesi için gerekli malzemeler ;

-	2 adet Tower Pro SG90 RC Mini Servo Motor
-	1 adet HD1900MG Servo motor
-	Arduino Uno
-	DS1307 RTC modülü
-	Erkek Jumper Kablo
-	Beyaz tahta kalemi
-	Mekanik parçalar (3D yazıcıdan çıktı alabilirsiniz.)





RTC modülü nedir ? 







İngilizce açılımı Real Time Clock yani Gerçek Zaman Saati olan RTC modülü, projelerinizde zamanı bilgisinin gerektiği durumlarda kullanabileceğiniz bir modüldür.  DS1307 entegresi ile yapılan bir gerçek zaman devresi ile saate göre açılıp kapanan röleler, tarih veya saate göre veri kaydetme gibi konularda ihtiyaçlarınızı karşılayabilirsiniz.

Üzerinde DS1307 RTC ve 24C32 EEPROM entegrelerini barındıran bu pratik kartı hassas saat  bilgisi gerektiren projelerinizde kullanabilirsiniz. I2C bağlantıya sahiptir. Üzerinde RTC modülü için CR2032 saat pili yuvası bulunur.
Bu projemizde de kalemimizi tutan robot kolumuza saat ve dakika bilgisini sağlamak için RTC modülü kullanılmıştır.



Şimdi işe koyulalım 😊

Gerekli malzemeleri belirttiğimize ve kısaca çalışma mantığınıda anladığımıza göre haydi projeyi yapmaya başlayalım ;

Projeye başlarken gelin ilk önce mekanizmayı kuralım. Bunun için bize gerekli olan parçaları 3D printerdan bastırabilirsiniz. Kullandığımız mekanik parçaların 3D dosyalarını aşağıdan indirebilirsiniz ; 

 (stl dosyalarını indirmek için ) 



Montaj


![deneme](C:\Users\Mehmet\Desktop\plotclock_parca_acıklama.jpg)










1)	İlk önce 1 numaralı parça ile 2 numaralı parçayı birleştiriyoruz.
2)	1 numaralı parçayı 3 numaralı parçanın alt kısımdaki deliğinde vida ile birleştiriyoruz
3)	3 ve 4 numaralı parçayı alt alta tutturup 3 numaralı parçayı bunları tuttrumak için birleştiriyoruz
4)	Sağ ve sol servoları 4 numaralı parçanın içindeki boşluklara geçiyoruz
5)	8 ve 9 numaralı parçaları servoların kafalarına vidalıyoruz 
6)	Daha sonra 8 ve 9 numaralı parçaları 10 ve 11 numaralı parçalar ile vidalıyoruz	. 11 parçanın sol servo tarafına denk gelmesi gerekiyor
7)	12 numaralı parçanın bir tarafını lifting servo ile vidalıyoruz diğer tarafını da 3 numaralı parçanın çıkıntılı tarafında olan boşluktan vidalıyoruz.
8)	14. Parçayı 2. parçada bulunan boşluktan çentikle tutturuyoruz.
9)	Sonra olarak 15 numaralı parçayı 2 numaralı parçaya takıyoruz.





Montajımızı bitirdikten sonra aşağıdaki gibi bir görüntü elde ederiz.














Devre Şeması

Bu projenin devre şemasını aşağıdaki gibi kuruyoruz ; 




 
Mekanik aksamımız ve devre şemamızda hazır ise Arduino kodumuzun algoritmasından bahsedelim;
Kodumuzda ilk olarak kullanacağımız değişkenleri tanımladık daha sonra servolar ve RTC için gerekli olan kütüphaneleri çağırdık. Bu projede önemli noktalardan biriside sağ ve sol servoların hareketiyle uçlarındaki kaleme rakam hareketlerini yaptırabilmek, bunun için her bir rakamın yazılabilmesi için
düzlemde x ve y hareketleri ile tanımlıyoruz. Motorların mevcut saatte hangi rakamların bulunduğunu bildirmek ve hangi rakamları yazacağını ise kontrol yapılarıyla sağlıyoruz.  

