---
template: blog-post
title: Uzaktan Kumandalı Araba
slug: /DIY-car
date: 2024-10-16 23:40
description: How to draw a character
featuredImage: /assets/yaratik.jpg
---

Donanımsal proje üretmenin bende hep ayrı bir yeri olmuştur. Çünkü sade yazılımsal projeler bir bilgisayar/telefonun içinde çalışabilen projelerdir. Oysaki donanımsal projeler kendi başına var olabilen, kendisini sergileyebilen projelerdir. React nativde kendimi geliştirmek için yan projeler yaparken acaba nasıl bir yerden donanımsal proje yapabilirim diye düşünüyordum. Bir gün aklıma bluetooth ile telefondan kontrol edilen uzaktan kumandalı araba yapma fikri geldi.

İnternette "do it yourself(DIY)" konseptinde hobi olarak yapılmış pek çok proje örneği vardı. Araştırmalarım sonucunda en çok dikkatimi çeken projeler arasında Arduino Uno ile bir araba yapma ve bu aracı React Native kullanarak geliştirdiğim bir mobil uygulama üzerinden yönetme fikri yer aldı. Bu doğrultuda bir proje geliştirmeye karar verdim.

Gereken parçaların hepsini içeren bir kit satın aldım. Kitin içinde araç şasisi, 4 adet motor ve tekerler, L298n motor sürücü, Arduino Uno ve HC-06 bluetooth modülü vardı. Yeni model Ardino Unolarda entegre bluetooth modülü var fakat bendeki kit eski model bir Arduino içeriyordu. Bu yüzden harici bir modül (HC-06) ile haberleşme sağlanıyordu.

Aracın montajını yaptım. Kodu için internetteki yapılmış projelerden bir kodu biraz değiştirerek kendi isterlerime uyarladım. Arduino C/C++'a çok benzer bir programlama dili ile geliştiriliyor. Üniversitede okurken C ve C++ la çok ödev, proje yaptığı için hiç zorluk çekmeden arabayı programlayabildim.

Canavar isimli sade react native cli ile projeyi başlattım. Neden expo kullanmadın diye soracak arkadaşlara açıklayayım: Benim geliştirme yaptığım tarihlerde expo ile bluetooth paketlerini kullanmak için expo eject yapılması gerekiyordu. Zaten bu noktada proje expo frameworkünün dışına çıkmış, "sade" react native projesinden pekte bir farkı kalmıyordu. Uygulama React Navigation ile iki ekrandan oluşuyordu. Giriş ekranında bluethoot kullanım izni verilmiş mi, bluetooth açık mı kontroller yapılıp, bağlanabilir cihazlar listelenip kullanıcıdan aracın seçilmesini bekliyordu.

Araca bağlandıktan sonra ikinci ekrana yani kontrol ekranına geçiliyor. Bu ekranda sağ baş parmakta aracın ileri geri hareketini yöneten dikey eksende oynatılan bir slider, sol baş parmağa denk gelen kısımda ise aracın sağ sol hareketini yöneten yatay eksende oynatılan bir slider yaptım. Tıpkı küçükken bana alınan oyuncak uzaktan kumandalı arabamın kumanda düzeni gibi. Slider için ilk olarak hazır bir üçüncü parti elaman kullanmayı düşündüm ama hazır elemanın görünümü tam olarak benim aklımdaki gibi olmayacağı için kendim yaptım. Software Mansion firmasının Reanimated ve Gesture Handler paketleriyle.

![Canavar uygulama ekranları](/assets/canavarScreens.png)

Uygulamayı tamamladım fakat bir sorun var! Araç çalışırken birden çalışmaktan vaz geçiyor. Yani araçla oynarken rastgele bir anda bağlantı kopuyor ve benim uygulamadan tekrar bağlantı yapmam gerekiyor. Bunun sebebini araştırınca HC-06 eski nesil bluetooth kullandığı için fazla enerji gereksinimi olan bir modülmüş ve düşük akım geldiği anda kendini kapatıp baştan başlıyormuş. Çözüm ise daha modern bir teknoloji olan Bluetooth Low Energy(BLE) ile çalışan ESP32 mikro denetleyici ile arabayı sürmek. Hem parça değişimi hemde haberleşme altyapısı değişeceği için yeni bir bluetooth kütüphanesine geçiş yapması hevesimi bitirmişti. Canavar projesini bu "bazen" meydana gelen kusurlu hali ile sonlandırmaya karar verdim.

Bir zaman sonra projeye dair yeniden heveslenince ESP32 sipariş ettim ve projeyi yapmaya koyuldum. Hem bu süreçte expo frameworkünde bazı gelişmeler oldu. Artık expo projeleri prebuild edilerek expo ekosisteminin içinde kalarak bluetooth kütüphaneleri gibi native modüllerle uyumlu hale geldi. Bende zaten arabanın merkezindeki parça olan Arduinoyu değiştirmişim, mobil uygulamayıda sıfırdan expo projesi olarak başlatayım. Canavar projesinin üstüne geliştirme yapmayayım. O github hesabımda kendi başına bir proje olarak dursun, bende yeni proje başlatayım dedim. Yeni projenin adı: Yaratık. Canavardan çokta uzaklaşmak istemedim.

ESP32'de aynı Arduino dili ile kodlanıyordu fakat bluetooth altyapısı değiştiği için Canavar projesindeki Arduino kodu tam olarak işime yaramıyordu. Okunan paketler ve bunların anlamlandırılması mantığı aynı olacaktı fakat bluetooth ile kendini yayınlama ve bağlanma fonksiyonlarının değişmesi gerekiyordu. İnternetteki ESP için yazılmış BLE örnek projelerinden birini kendi ihtiyacıma göre düzenledim. Mobil uygulamada expo ile projeyi başlattım. React Navigation yerine Expo Router kullandım. Yine iki ekranlı uygulama. Yine ilk ekran bluetooth arama bağlanma, ikinci ekranda kontroller oldu. Uygulama mantığı neredeyse Canavar projesinin aynısı. sadece bluetooth için BLE PLX kütüphanesine geçtiğimden bluetooth fonksiyonları değişti.

Ve sonuç olarak kopma sorunu olmayan uzaktan kumandalı araba projem tamamlandı.

<iframe src="https://github.com/user-attachments/assets/4f1630d6-d9e8-458a-a2fa-44d42532aa0f" width="600" height="350"></iframe>

Altta hem Canavar projesinin hemde Yaratık projesinin kaynaklarını içeren linkleri paylaşıyorum. Eğer sizde kendiniz uzaktan kumandalı araba yapmayı düşünüyorsanız şahsen ESP32 içeren Yaratık projemi kullanmanızı tavsiye ederim.

Yaratık: [https://github.com/salihbektas/react-native-esp32-bluetooth-RC-car](https://github.com/salihbektas/react-native-esp32-bluetooth-RC-car)

Canavar: [https://github.com/salihbektas/ReactNative-Arduino-Bluetooth-RC-Car](https://github.com/salihbektas/ReactNative-Arduino-Bluetooth-RC-Car)
