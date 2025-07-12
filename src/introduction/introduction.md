# Fyrox'a Giriş



Fyrox, her türlü oyun için uygun, zengin özelliklere sahip, genel amaçlı bir oyun motorudur.


küçük veya orta büyüklükteki dünyalara sahip oyunları çalıştırabilir, büyük dünyalar için ise muhtemelen bazı manuel çalışmalar gerekecektir.
 


Motorla yapılan oyunlar masaüstü platformlarda (PC, Mac, Linux) ve Web'de (WebAssembly) çalışabilir. Mobil

gelecek sürümlerde planlanmaktadır.



## Motor ne yapabilir?



Hemen hemen her tür oyun veya etkileşimli uygulama oluşturabilirsiniz. Motorun yapabileceklerinin bazı örnekleri 

şunlardır:

![Station Iapetus](game_example1.jpg)
![Fish Folly](game_example2.jpg)
![2D Platformer](game_example3.jpg)

## Motor nasıl çalışır?



Motor, aktif olarak kullanacağınız iki bölümden oluşur: çerçeve ve editör. Çerçeve, motorun temelini oluşturur ve görüntüleme, ses, komut dosyaları, eklentiler vb. işlemleri yönetir. Editör ise oyun dünyaları oluşturmak, varlıkları yönetmek, oyun nesnelerini, komut dosyalarını ve daha fazlasını düzenlemek için kullanılabilecek birçok araç içerir.

![Fish Folly](editor.png)

## Programlama dilleri

Oyununuzun tamamı, güvenliği ve hızı ile Rust ile yazılabilir. Ancak,

istediğiniz herhangi bir betik dilini kullanabilirsiniz, ancak diğer dillerde yerleşik destek olmayabilir ve bunu manuel olarak uygulamanız gerekebilir.

## Motor Özellikleri

Bu, motor özelliklerinin aşağı yukarı eksiksiz (ancak güncel olmayabilir) bir listesidir:

### Genel

- Olağanüstü güvenlik, güvenilirlik ve hız.

- PC (Windows, Linux, macOS), Android, [Web (WebAssembly) desteği](https://fyrox.rs/examples).

- Modern, PBR renderleme boru hattı.

- Kapsamlı [belgeler](https://docs.rs/Fyrox).

- [Kılavuz kitap](https://fyrox-book.github.io)

- 2D desteği.

- Entegre editör.

- Hızlı yinelemeli derleme.

- Klasik nesne yönelimli tasarım.

- Çok sayıda örnek.

### Rendering

- Özel gölgelendiriciler, malzemeler ve render teknikleri.

- Fizik tabanlı render.

- Metalik iş akışı.

- Yüksek dinamik aralık (HDR) render.

- Ton eşleme.

- Renk derecelendirme.

- Otomatik pozlama.

- Gama düzeltme.

- Ertelenmiş gölgelendirme.

- Yönlü ışık.

- Nokta ışıklar + gölgeler.

- Spot ışıklar + gölgeler.

- Ekran Alanı Ortam Kapama (SSAO).

- Yumuşak gölgeler.

- Hacimsel ışık (spot, nokta).

- Toplu işleme.

- Örnekleme.

- Hızlı Yaklaşık Kenar Yumuşatma (FXAA).

- Normal eşleme.

- Paralaks haritalama.

- Doku içinde render.

- Şeffaf nesneler için ileri render.

- Gökyüzü kutusu.

- Ertelenmiş çıkartmalar.

- Çoklu kamera render.

- Işık haritalama.

- Yumuşak parçacıklar.

- Tamamen özelleştirilebilir köşe formatı.

- Sıkıştırılmış doku desteği.

- İsteğe bağlı yüksek kaliteli mip-map oluşturma.

### Scene

- Çoklu sahne.

- Tam özellikli sahne grafiği.

- Ayrıntı düzeyi (LOD) desteği.

- GPU Skinning.

- Çeşitli sahne düğümleri:

- Pivot.

- Kamera.

- Decal.

- Mesh.

    - Parçacık sistemi.

    - Sprite.

- Çok katmanlı arazi.

- Rectangle (2D Sprites)

- Rigid body + Rigid Body 2D

- Collider + Collider 2D

- Joint + Joint 2D

### Ses



- [HRTF desteği ile yüksek kaliteli binaural ses](https://github.com/FyroxEngine/Fyrox/tree/master/fyrox-sound).

- Genel ve uzamsal ses kaynakları.

- Büyük sesler için yerleşik akış.

- Ham örnek çalma desteği.

- WAV/OGG formatı desteği.

- Mükemmel konumlandırma ve binaural efektler için HRTF desteği.

- Yankı efekti.



### Seri hale getirme

- Güçlü seri hale getirme sistemi

- Motorun neredeyse tüm öğeleri seri hale getirilebilir

- Kendi seri hale getirme kodunuzu yazmanıza gerek yoktur.


### Animasyon


- Animasyon karıştırma durum makinesi - Unity Engine'deki Mecanim'e benzer.

- Animasyon yeniden hedefleme - animasyonu bir modelden diğerine yeniden eşleyebilir.


### Varlık yönetimi


- Gelişmiş varlık yöneticisi.

- Tamamen asenkron varlık yükleme.

- PNG, JPG, TGA, DDS vb. dokular.

- FBX model yükleyici.

- WAV, OGG ses formatları.

- Sıkıştırılmış doku desteği (DXT1, DXT3, DTX5).


### Yapay Zeka (AI)


- AI yol bulucu.

- Navmesh.

- Davranış ağaçları.

### Kullanıcı Arayüzü (UI)



- Çok sayıda widget içeren [gelişmiş düğüm tabanlı UI](https://github.com/FyroxEngine/Fyrox/tree/master/fyrox-ui).

- 32'den fazla widget

- Güçlü düzen sistemi.

- Tam TTF/OTF yazı tipi desteği.
- Mesaj aktarımına dayalı.

- Tamamen özelleştirilebilir.

- GAPI'dan bağımsız.

- İşletim sisteminden bağımsız.

- Düğme widget'ı.

- Kenarlık widget'ı.

- Tuval widget'ı.

- Renk seçici widget'ı.

- Renk alanı widget'ı.

- Onay kutusu widget'ı.

- Dekoratör widget'ı.

- Açılır liste widget'ı.

- Izgara widget'ı.
- Resim widget'ı.

- Liste görünümü widget'ı.

- Açılır widget'ı.

- İlerleme çubuğu widget'ı.

- Kaydırma çubuğu widget'ı.

- Kaydırma paneli widget'ı.

- Kaydırma görüntüleyici widget'ı.

- Yığın paneli widget'ı.

- Sekme kontrol widget'ı.

- Metin widget'ı.

- Metin kutusu widget'ı.
- Ağaç widget'ı.

- Pencere widget'ı.

- Dosya tarayıcı widget'ı.

- Dosya seçici widget'ı.

- Yerleştirme yöneticisi widget'ı.

- NumericUpDown widget'ı.

- `Vector3<f32>` düzenleyici widget'ı.

- Menü widget'ı.

- Menü öğesi widget'ı.

- Mesaj kutusu widget'ı.

- Sarmalama paneli widget'ı.

- Eğri düzenleyici widget'ı.

- Kullanıcı tanımlı widget.

### Fizik



- Gelişmiş fizik [rapier](https://github.com/dimforge/rapier) fizik motoru sayesinde)

- Rigid bodies.

- Zengin çeşitli çarpışmalar.

- Eklemler.

- Ray cast.

- Diğer birçok kullanışlı özellik.

- 2D desteği.
