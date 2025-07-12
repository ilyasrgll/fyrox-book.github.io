# Animation Editor

![anim editor](./anim_editor.png)

Animasyon Düzenleyici, animasyonlar oluşturmanıza ve önizlemenize yardımcı olan bir araçtır. Bu, hemen hemen her türlü sayısal özelliği canlandırmak için kullanılabilen güçlü bir araçtır.
 Üç ana bölümden oluşur:



1. `Araç Çubuğu` - animasyonun belirli bir bölümünü (ad, uzunluk, hız vb.) değiştiren bir dizi araç içerir

2. `Parça Listesi` - canlandırılacak düğümlerin parçalarının bir listesini içerir.

3. `Eğri Düzenleyici` - eğri düzenleyici, sayısal bir parametrenin zaman içindeki davranışını düzenlemenizi sağlar.


Düzenleyici iki şekilde açılabilir: `Utils -> Animation Editor` kullanarak veya bir animasyon oynatıcı düğümü seçip

 
denetçide `Open Animation Editor` düğmesine tıklayarak.

![open1](./ae_open1.png)

![open2](./ae_open2.png)

Her iki durumda da düzenleme için bir animasyon oynatıcı seçmeniz gerekir.

## Typical Workflow

İlk olarak, bir animasyon oluşturmanız veya import etmeniz (#animation-importing) gerekir, ardından zaman dilimini
istenen aralığa ayarlamanız gerekir (aşağıdaki bölümdeki [Zaman Dilimi](#toolbar) bölümüne bakın), ardından istenen 
özellikleri için birkaç parça eklemeniz ve son olarak bazı anahtarlar eklemeniz gerekir. Sonuçları istediğiniz zaman [önizleyebilirsiniz](#preview-mode).
Önizleme modundayken animasyonu değiştirmeye çalıştığınızda, önizleme modundaki tüm değişiklikler geri alınır
ve ancak o zaman yaptığınız değişiklikler uygulanır.

## Toolbar

Araç çubuğu, animasyonun belirli bir bölümünü (ad, uzunluk, hız vb.) değiştiren bir dizi araç içerir. Şu

şekilde görünür:

![toolbar](./ae_toolbar.png)

1. `Animasyon Adı` - şu anda seçili olan animasyonun adı.


2. `Animasyon Ekle` - sol taraftaki metin kutusuna yazılan adla yeni bir boş animasyon ekler.
 
3. `Animasyon İçe Aktar` - animasyon içe aktarma işlemini başlatır. Daha fazla bilgi için [Animasyon İçe Aktarma](#animation-importing) bölümüne bakın.

4. `Animasyonu Yeniden İçe Aktar` - animasyonu harici bir dosyadan yeniden içe aktarır. Animasyonun içeriğini değiştirmeniz, ancak referanslarını geçerli tutmanız gerektiğinde kullanışlıdır.

5. `Animasyonu Yeniden Adlandır` - sol taraftaki metin kutusundaki adla, seçili animasyonun adını değiştirir.

6. `Animasyon Seçici` - düzenlenmekte olan animasyonu değiştirmenizi sağlar.

7. `Animasyonu Sil` - seçili animasyonu siler, mümkünse listeden son animasyonu seçmeye çalışır.

8. `Animasyonu Çoğalt` - Seçili animasyonu kopyalar.

9. `Animasyonu Döngüye Al` - Seçili animasyonun döngüye alınmasını etkinleştirir veya devre dışı bırakır.

10. `Animasyonu Etkinleştir` - Seçili animasyonu etkinleştirir veya devre dışı bırakır.

11. `Animasyon Hızı` - Seçili animasyonun yeni oynatma hızını ayarlar.

12. `Zaman Dilimi` - Seçili animasyonun başlangıç ve bitiş zamanını tanımlayan bir zaman aralığı (saniye cinsinden). Aralık

eğri düzenleyicide vurgulanır.

13. `Kök Hareket` - Kök hareket ayarlarını açar. Daha fazla bilgi için [Kök Hareket](#root-motion) bölümüne bakın.

14. `Önizleme Anahtarı` - animasyon önizlemesini etkinleştirir veya devre dışı bırakır. Daha fazla bilgi için [`Önizleme Modu`](#preview-mode) bölümüne bakın.

15. `Oynat/Duraklat` - seçili animasyonu oynatır veya duraklatır (yalnızca önizleme modunda izin verilir).

16. `Durdur` - seçili animasyonu durdurur (yalnızca önizleme modunda izin verilir).

## Track List

Parça listesi, canlandırılacak düğümlerin parçalarının bir listesini içerir. Şu şekilde görünür:

![track list](./ae_track_list.png)

1. `Filtre Çubuğu` - filtreye uyan isimlere sahip parçaları bularak parça listesini filtreler. Bunu,

belirli bir sahne düğümüne ait parçaları bulmak için kullanabilirsiniz.

2. `Filtreyi Temizle` - filtreyi temizler, parça listesi bundan sonra tüm parçaları gösterir.

3. `Tümünü Daralt` - listedeki tüm parçaları daraltır.

4. `Tümünü Genişlet` - listedeki tüm parçaları genişletir.

5. `Parça` - bir dizi parametrik eğri içeren parça.

6. `Parça Bileşeni Eğrisi` - belirli bir parçanın animasyonu için veri kaynağı görevi gören parametrik eğri.

7. `Parça Değiştir` - bir parçayı etkinleştirir veya devre dışı bırakır; devre dışı bırakılan parçaların özelliklerine “dokunulmaz”.

8. `Parça Ekle` - özellik bağlama işlemini başlatır, daha fazla bilgi için [Özellik Bağlama](#property-binding) bölümüne bakın.

### Track Context Menu

![context menu](./ae_track_context_menu.png)

- `Seçili Parçaları Sil` - seçili parçaları siler;

Ctrl tuşunu basılı tutarak birden fazla parçayı seçerek aynı anda silebilirsiniz.

## Curve Editor

Eğri düzenleyici, parametrik eğrileri (her seferinde bir tane) düzenlemenizi sağlar. Bir eğri, sıfır veya daha fazla anahtar kareden oluşur ve 

mevcut ve bir sonraki kareler arasında çeşitli geçiş kuralları vardır. Düzenleyici şöyle görünür:

![curve editor](./ae_curve_editor.png)

1. `Zaman Cetveli` - zaman değerlerini ve seçili animasyonun tüm sinyallerini gösterir. Zaman cetveline tıklandığında

oynatma imleci tıklama konumuna taşınır. İmleci tıklayıp farenin sol tuşunu basılı tutarak fareyi hareket ettirerek imleci taşıyabilirsiniz. 

Animasyon sinyalleri de aynı şekilde taşınabilir.

2. `Parametrik Eğri` - bir değerin zaman içinde nasıl değiştiğini tanımlayan bir eğri.

3. `Zaman Başparmağı` - animasyon oynatma imleci, yalnızca önizleme için kullanışlıdır.

4. `Animasyon Sinyali` - oynatma imleci üzerinden geçtiğinde animasyon olayları üretecek bazı animasyon sinyalleri.

### Time Ruler Context Menu

![time ruler context menu](./ae_time_ruler_context_menu.png)

- `Sinyali Kaldır` - fare imlecinin altındaki animasyon sinyalini kaldırır.

- `Sinyal Ekle` - fare imlecinin konumuna yeni bir animasyon sinyali ekler.

### Key Frame Context Menu

![key frame context menu](./ae_key_frame_context_menu.png)

- `Konum` - önemli bir konumu gösterir ve değiştirmenize olanak tanır. Kesin değerler ayarlamak için kullanışlıdır.
 
- `Değer` - bir anahtar değeri gösterir ve değiştirmenize izin verir. Kesin değerler ayarlamak için kullanışlıdır.

- `Anahtar Ekle` - eğriye yeni bir anahtar ekler.

- `Kaldır` - seçili tüm anahtarları kaldırır. Birden fazla anahtarı, kutu seçimi (fareyi tıklayıp sürükleyerek

kutuyu etkinleştirin) veya `Ctrl` tuşunu basılı tutarken ayrı anahtarları tıklayarak seçebilirsiniz.

- `Anahtar...` - anahtarın enterpolasyon türünü değiştirmenizi sağlar. Aşağıdaki değerlerden biri olabilir: Sabit, Doğrusal,

Kübik.

- `Sığdırmak için yakınlaştır` - tüm eğrinin görünüm alanına sığması için yakınlaştırma değerlerini (her iki eksen için) ve görünüm konumunu bulmaya çalışır.

## Property Binding

Bir özelliği canlandırmak için tek yapmanız gereken, iz listesinin altındaki `İz Ekle...` düğmesine tıklamak,
canlandırmak istediğiniz düğümü seçmek ve ardından canlandırılacak özelliği seçmektir. Ardından iki pencere açılacaktır:

![step1](./ae_add_track_select_node.png)

![step2](./ae_add_track_select_property.png)

Herhangi bir pencerede `İptal` düğmesine tıklayarak özellik bağlamayı istediğiniz zaman iptal edebilirsiniz. Yalnızca sayısal özellikleri canlandırabileceğinizi, bu nedenle pencerede her özelliğin gösterilmediğini unutun.

## Animation Importing

Animasyonlar ayrı dosyalarda saklanabilir, ancak motorun hepsinin tek bir Animasyon Oynatıcıda olması gerekir.
Harici bir kaynaktan (örneğin bir FBX) bir animasyonu animasyon oynatıcıya eklemek için animasyon 
içe aktarmayı kullanabilirsiniz. Bunu yapmak için, animasyon içe aktarma simgesine tıklayın ve ardından harici animasyon dosyasında animasyonlu olan hiyerarşinin kök düğümünü
seçin, ardından animasyon dosyasını seçin ve `Tamam`'a tıklayın. Motor, animasyonu içe aktarmaya çalışır
ve onu verilen hiyerarşiye eşler. Eşleme, düğüm adları kullanılarak yapılır, bu nedenle animasyonlu düğüm adları hem 
sahnenizde hem de harici animasyon dosyanızda eşleşmelidir.

![step1](./ae_import_select_target_node.png)

![step2](./ae_import_select_animation.png)

Mevcut animasyonların içeriği yeniden içe aktararak değiştirilebilir. Animasyonunuzu yeniden içe aktarmak için iki dairesel ok bulunan düğmeye tıklayın .
 Bu, animasyonunuzu harici bir düzenleyicide (örneğin Blender) değiştirdiyseniz ve
değişiklikleri oyununuzda uygulamak istiyorsanız

## Preview Mode

Önizleme modu, animasyonunuzu görüntülemenize ve hata ayıklamanıza yardımcı olur. Modu etkinleştirdikten sonra, animasyonu

`Oynat/Duraklat` düğmesine tıklayarak oynatmanız gerekir:

![anim editor](./anim_editor.gif)

Sahne üzerinde yapılan herhangi bir önemli değişiklik, önizleme modunu otomatik olarak devre dışı bırakarak yapılan tüm değişiklikleri
animasyon oynatarak geri alır.

## Root Motion

Daha fazla bilgi için [Kök Hareket bölümü](root_motion/root_motion.md) bölümüne bakın.

## Limitations

Şu anda düzenleyicide dopesheet modu yoktur, bir seferde yalnızca bir sayısal parametreyi düzenleyebilirsiniz. Ayrıca, 

yakalama modu da yoktur - bu, düzenleyicinin sahnedeki değişikliklerinizi otomatik olarak animasyona eklediği özel bir moddur.

Bu sınırlamalar gelecek sürümlerde kaldırılacaktır.