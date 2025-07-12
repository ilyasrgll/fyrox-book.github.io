# Terrain

Terrain, her bir hücrenin farklı yüksekliğe sahip olabileceği tek tip bir hücre ızgarasını temsil eden bir sahne düğümüdür. Terrain için yaygın olarak kullanılan diğer

adlar arasında heightmap bulunmaktadır. Terrain, açık dünya oyunları için haritalar oluşturmak amacıyla kullanılır; tepeler,
dağlar, platolar, yollar vb. oluşturmak için kullanılır.

![terrain](./terrain.png)

## Temel kavramlar



Terrain'leri kullanmaya başlamadan önce anlamanız gereken birkaç temel kavram vardır. Bu kavramlar,

tasarım kararlarını ve olası kullanım durumlarını anlamanıza yardımcı olacaktır.

### Yükseklik Haritası



Daha önce de belirtildiği gibi, arazi, hücrelerin X ve Z koordinatlarının sabit değerlere sahip olduğu, Y

ise değişebilen tek tip bir ızgaradır. Bu durumda, X ve Z koordinatlarını hesaplamak için yalnızca genişlik, yükseklik ve çözünürlük sayısal parametrelerini saklayabiliriz,

 Y ise hücrelerin yüksekliklerini değiştirmek için kullanılan ayrı bir dizide saklanır. Bu diziye _yükseklik haritası_ denir.

![terrain mesh](./terrain_mesh.png)

### Katmanlar



Katman, arazi ağlarına uygulanan malzeme + maskedir. Maske, malzemenin arazinin hangi kısımlarında görünür olacağını veya olmayacağını tanımlayan ayrı bir gri tonlamalı dokudur.

 Maskedeki beyaz pikseller malzemenin görünür olmasını sağlarken, siyah pikseller malzemenin

tamamen şeffaf olmasını sağlar. İkisi arasındaki her şey, katmanlar arasında yumuşak geçişler oluşturmanıza yardımcı olur. İşte çoklu katmanlara ilişkin basit bir

örnek:

![terrain layers layout](./terrain_layers_layout.png)

3 katman vardır: 1 - kir, 2 - çim, 3 - kayalar ve çim. Gördüğünüz gibi, her katman arasında yumuşak geçişler vardır,

 bu katman maskesiyle elde edilir.



Her katman ayrı bir malzeme kullanır ve bu malzeme, Denetçi'deki ilgili özellik düzenleyicisinden düzenlenebilir:

![terrain layer material](./terrain_layer_material.png)

## Editörde arazi oluşturma



`create -> terrain` seçeneğine tıklayarak bir arazi düğümü oluşturabilirsiniz. Bu, sabit genişlik, yükseklik ve


çözünürlüğe sahip bir arazi oluşturacaktır (bkz. [sınırlamalar](./terrain_node.md#limitations-and-known-issues)). Arazi oluşturulduktan sonra, Dünya Görüntüleyicisi'nde

onu seçin ve araç çubuğundaki Tepe simgesine tıklayın. Bu, arazi düzenlemeyi etkinleştirir ve fırça seçenekleri paneli

de görünmelidir. Tüm adımları gösteren aşağıdaki resme bakın:

![terrain editing](./terrain_editing.png)

İmlecin altındaki arazideki yeşil rectangle, mevcut fırçayı temsil eder. Fırça seçeneklerini

`Brush Options` penceresinde düzenleyebilirsiniz:

![brush options](./brush_options.png)

- *Shape:* Dairesel veya dikdörtgen bir fırça seçin. Dairesel bir fırça seçildiğinde, çapını ayarlamak için bir kontrol görünür. Dikdörtgen bir fırça seçildiğinde, genişlik ve uzunluk için kontroller görünür. Yeşil dikdörtgenin boyutu, bu kontrollere göre fırçanın boyutunu yansıtacak şekilde değişir.

- *Mode:* Fırçanın gerçekleştireceği arazi düzenleme işlemini seçin.
    
- *Yükselt veya Alçalt:* Mevcut değeri sabit bir miktar değiştirir. Sayı pozitif olduğunda, değer artar. Sayı negatif olduğunda, değer azalır. Fırça hedefi “Yükseklik Haritası” olduğunda, bu arazi yükseltmek veya alçaltmak için kullanılabilir. Fırça darbesinin başlangıcında `Shift` tuşu basılı tutulduğunda, yükseltme veya alçaltma sayısı negatif hale gelir, böylece yükseltme işlemi alçaltma işlemine dönüşür.

    - *Değer Atama:* Mevcut değeri verilen bir değerle değiştirir. Örneğin, belirli bir yükseklikte bir plato oluşturmak istiyorsanız, bu modu seçip fırça değeri olarak istediğiniz yüksekliği girebilirsiniz.

    - *Düzleştir:* Tabanı, tıkladığınız noktadan  fırçayı sürüklediğiniz yere kadar yayarak düzleştirir. *Değer Atama* ile aynı şekilde çalışır, ancak fırça darbesinin başladığı yerdeki arazi değerinden otomatik olarak alındığı için istediğiniz değeri belirtmeniz gerekmez.

- *Düzleştir:* Fırçanın dokunduğu arazinin her noktası için, o değeri yakındaki değerlerin ortalamasıyla değiştirir. Bu, arazi değerlerindeki keskin geçişleri azaltma eğilimindedir.

- *Hedef:* Fırça ile düzenlenebilecek arazinin birçok yönü vardır ve bu kontrol, düzenleyeceğiniz yönü seçmenizi sağlar. “Yükseklik Haritası” olarak ayarlandığında fırça, arazinin yüksekliğini değiştirir. “Katman Maskesi” olarak ayarlandığında, seçilen indeksle katmanın şeffaflığını değiştirir. Maskeler, seçilen fırça modundan bağımsız olarak her zaman 0 ile 1 arasında kalır, çünkü 0 tamamen şeffaf ve 1 katmanın tamamen opak olduğunu temsil eder.

- *Dönüştür:* Bu, fırçanın şekline uygulanan 2x2 matrisidir ve dikdörtgen bir fırçayı döndürme, eğirme veya uzatma gibi doğrusal dönüşümler yapmanızı sağlar. Çoğu amaç için, \\(\begin{bmatrix}1&0\\\\0&1\end{bmatrix}\\) kimlik matrisi iyi sonuç verir, çünkü bu, fırçanın şekline herhangi bir değişiklik uygulamayan varsayılan değerdir. Matris tersine çevrilemezse, yok sayılır.

- *Sertlik:* Bir fırçanın etkisi, tüm alanına eşit olarak uygulanması gerekmez. Bir fırçanın *sertliği*, fırçanın ne kadarının tam etkiyi alacağını kontrol eder. Sertlik 0 olduğunda, sadece fırçanın tam merkezi tam etkiyi alırken, fırçanın geri kalanı tam etkiden kenarlarda etkisiz hale gelir. Sertlik 1 veya daha büyük olduğunda, fırçanın tamamı tam etkiyi alır. Değer 0'dan küçükse, fırçanın merkezi bile tam etkiyi almaz.

- *Alfa:* \\(\alpha\\) değeri, arazinin mevcut değeri ile fırçanın tam etkisinin üreteceği değer arasında doğrusal olarak enterpolasyon yapar. \\(v_0\\) arazideki bir noktanın mevcut değeri ve \\(v_1\\) fırçanın tam etkisidir. \\(v_0\\) arazi üzerindeki bir noktanın geçerli değeri ve \\(v_1\\) fırçanın tam etkisi ise, fırçanın uygulayacağı gerçek etki \\((1 - \alpha) * v_0 + \alpha * v_1\\) olacaktır. \\(\alpha\\) değerinin 0 ile 1 arasında olması gerekmez. 0'dan küçük değerler fırçanın etkisini tersine çevirirken, 1'den büyük değerler fırçanın etkisini abartır. 0'a yakın değerler, birden fazla fırça darbesi ile kademeli olarak efekt uygulayarak ince ayar yapmak için kullanılabilir.



Her fırça darbesi, fare düğmesi basıldığında başlayan ve fare düğmesi bırakıldığında biten bağımsız bir işlem olarak ele alınır. Aynı arazi alanında fareyi tekrar tekrar sürüklemek, fırçanın etkisini artırmaz çünkü bunların tümü aynı fırça darbesinin parçasıdır, ancak fare düğmesini tekrar tekrar basıp bırakmak, birden fazla fırça darbesi olarak sayıldığından fırçanın etkisinin tekrar tekrar uygulanmasına neden olur.

## Koddan arazi oluşturma

Arazi fırçaları, `fyrox::scene:terrain::Brush` ve `fyrox::scene::terrain::BrushContext` kullanılarak koddan araziyi düzenlemek için de kullanılabilir.



`Brush` yapısı, her bir fırça seçeneği için alanlara sahiptir ve `BrushContext` yapısı, bir `Brush` kabul etmek ve


onu araziye uygulamak için yöntemlere sahiptir. BrushContext, yeni bir vuruş başlatmanıza, vuruş sırasında damgalama ve bulaştırma işlemleri gerçekleştirmenize ve ardından vuruşu sonlandırmanıza

olarak yapılandırılmış fırça vuruşunu araziye yazmak için kullanabilirsiniz. Ayrıca, kısmen tamamlanmış bir vuruşu araziye `flush` ile aktararak,

fırça vuruşunun araziye tek seferde görünmek yerine birden fazla karede animasyonlu olarak gösterilmesini sağlayabilirsiniz.



BrushContext tarafından sağlanan yöntemlerin listesi aşağıda verilmiştir:

```rust,no_run
fn start_stroke(&mut self, terrain: &Terrain, brush: Brush)
```

Çizimin geri kalanında kullanılacak fırçayı seçmek için bunu çağırın. Bu noktada `BrushContext`, verilen fırçanın hedefindeki verileri temsil etmek için

arazinin hangi dokuları kullandığını kaydeder. ve bu dokular, `end_stroke` sonunda çağrıldığında nihai olarak değiştirilecek olan dokulardır
.

```rust,no_run
fn stamp(&mut self, terrain: &Terrain, position: Vector3<f32>)
```

Bunu, fırçayı arazi üzerinde tek bir noktaya damgalamak için çağırın. Bir vuruş zaten başlatılmış olmalıdır, çünkü bu, bir vuruşu oluşturabilecek birçok işlemden sadece
biri olabilir.


Arazi değiştirilmez; sadece verilen konumu dünya uzayından arazi doku uzayına çevirmek için kullanılır.
Bu damganın arazi üzerindeki sonuçlarını gerçekten görmek için, `flush` veya `end_stroke` çağrılmalıdır.

Konum araziye yansıtıldığından, konumun y koordinatı yok sayılır.

```rust,no_run
fn smear(&mut self, terrain: &Terrain, start: Vector3<f32>, end: Vector3<f32>)
```

Bir leke, damga gibidir, ancak `başlangıç` ile `son` arasındaki çizgi boyunca fırça ile sürekli boyama yapar.

Yine, boyama yapılacak fırçayı seçmek için bir vuruşun önceden başlatılmış olması gerekir ve sonuçlar
arazi üzerinde hemen görünmez.

```rust,no_run
fn flush(&mut self)
```

Bunu çağırarak, kısmen tamamlanmış bir fırça darbesinden kaynaklanan değişikliklerin dahil edilmesi için araziyi güncellemeye zorlayabilirsiniz.

Bir fırça darbesi birden fazla kareye çiziliyorsa, her karenin sonunda `flush` çağrısı yapmak mantıklı olacaktır.

`flush` yöntemi, araziyi geçirmeyi gerektirmez çünkü `BrushContext`, araziyi güncellemek için hangi dokuların

değiştirilmesi gerektiğini zaten bilir.

```rust,no_run
fn end_stroke(&mut self)
```

Bunu çağırarak, vuruştan kaynaklanan değişiklikleri dahil etmek için araziyi güncelleyin ve o vuruşa ait tüm verileri silin,

 böylece bağlam yeni bir vuruşa başlamaya hazır hale gelir.

```rust,no_run
fn shape(&mut self) -> &mut BrushShape
```

Bu, fırçanın şekline değiştirilebilir erişim sağlar ve yeni bir vuruş başlatmadan şekli değiştirilebilir.

```rust,no_run
fn hardness(&mut self) -> &mut f32
```

Bu, fırçanın sertliğine değiştirilebilir erişim sağlar ve yeni bir vuruş başlatmadan sertliği değiştirmeyi mümkün kılar.



Vuruşun ortasında fırçanın alfa ve modunu değiştirmek için de benzer yöntemler vardır, ancak bunlar pratik kullanım için uygun değildir

çizim fırçaları bu tür değişikliklere iyi tepki vermez. Yeni bir fırça modu gerekiyorsa, yeni bir çizim başlatmak en iyisidir.

 Çizimin ortasında fırçanın hedefini değiştirmek özellikle mümkün değildir, çünkü bu, `BrushContext` iç durumunun diğer ayrıntılarının güncellenmesini gerektirir
.



İşte `BrushContext` kullanımına bir örnek:

```rust,no_run
{{#include ../code/snippets/src/scene/terrain.rs:create_random_two_layer_terrain}}
```

Gördüğünüz gibi oldukça fazla kod var, ideal olarak her zaman editör kullanmalısınız, çünkü her şeyi koddan halletmek

çok sıkıcı olabilir. Yürütmenin sonucu (tüm dokular doğru ayarlanmışsa) şunun gibi olabilir

(kodu her çalıştırdığınızda arazinin rastgele olacağını unutmayın):

![terrain from code](./terrain_random.png)

## Fizik



Varsayılan olarak, araziler ilgili fiziksel gövdeye ve şekle sahip değildir, bunlar manuel olarak eklenmelidir.

Heightmap şekline sahip bir çarpıştırıcı ile statik bir rigid body düğümü oluşturun ([çarpıştırıcılar hakkında daha fazla bilgi için](../physics/collider.md)). Ardından


arazi parçasını rigid body'ye ekleyin. Arazinin orijini Heightmap rigid body'den farklıdır, bu nedenle fiziksel temsiline uyması için araziyi

ofsetlemeniz gerekir. Fiziksel şekilleri görmek ve araziyi hareket ettirmek için editör ayarlarında fizik görselleştirmeyi etkinleştirin.

 Şimdi araziyi hareket ettirmek için arazi yerine gövdeyi hareket ettirmelisiniz (parent-child

[ilişkiler](../beginning/scene_and_scene_graph.md#local-and-global-coordinates)).

## Performans



Arazi renderleme karmaşıklığı, arazinin katman sayısı ile doğrusal bir bağımlılığa sahiptir. Her katman, motorun

farklı dokular ve maskelerle arazinin geometrisini yeniden renderlemesini gerektirir. Tipik katman sayısı 4 ila 8 arasındadır. Örneğin,

bir arazi şu katmanlara sahip olabilir: toprak, çim, kaya, kar. Bu nispeten hafif bir şemadır. Her durumda,

her yeni katmanın performansınızı nasıl etkilediğini anlamak için kare süresini ölçmelisiniz.



## Parçalama



Arazi kendisi herhangi bir geometri veya renderleme verisi tanımlamaz, bunun yerine bu amaçla bir veya daha fazla parça kullanır. Her


parça bir “alt arazi” olarak düşünülebilir. Arazinin herhangi bir tarafından istediğiniz sayıda parçayı “istifleyebilirsiniz”. Bunu yapmak için

, her eksen boyunca bir parça aralığı tanımlarsınız. Bu, arazinizi belirli bir

yönünde genişletmeniz gerektiğinde çok kullanışlıdır. Tek bir parça ile (her iki eksende `0..1` aralığı) bir arazi oluşturduğunuzu, ancak aniden


yeni oyun konumları eklemek için araziyi genişletmeniz gerektiğini fark ettiğinizi düşünün. Bu durumda,

istenen eksende parça aralığını değiştirebilirsiniz. Örneğin, tek parçanızın sağına yeni bir konum eklemek istiyorsanız,

`width_chunks` aralığını `0..2` olarak değiştirin ve `length_chunks` değerini olduğu gibi bırakın (`0..1`). Bu şekilde arazi genişletilecek ve

yeni konumu şekillendirmeye başlayabilirsiniz.

## Ayrıntı düzeyi



Arazi, otomatik LOD sistemine sahiptir, bu da en yakın kısımlarının en yüksek
kalitede (yükseklik haritasının ve maskelerin çözünürlüğü ile tanımlanır) render edileceği, en uzak kısımlarının ise
en düşük kalitede render edileceği anlamına gelir. Bu, GPU yükünü etkili bir şekilde dengeler ve düşük ek yük ile büyük arazileri render etmenizi sağlar
.



LOD sistemini etkileyen ana parametre, render için kullanılacak yamanın boyutunu tanımlayan `block_size` (`Terrain::set_block_size`) parametresidir.
 Bu parametre, yükseklik haritasının boyutunu dörtlü ağaç algoritması kullanılarak sabit bir blok kümesine bölmek için kullanılır
.



Mevcut uygulama, yama morflaması olmadan CDLOD algoritmasının değiştirilmiş bir sürümünü kullanır. Görünüşe göre buna gerek yoktur,
çünkü köşe gölgelendiricideki bilineer filtreleme, dikişlerin oluşmasını engeller.



Mevcut uygulama, ortalama düşük-orta seviye GPU'larda yaklaşık bir milisaniye içinde 4096x4096 yükseklik haritası çözünürlüğünde devasa arazileri (64x64 km) render etmeyi mümkün kılar.

## Limitations and known issues

Henüz araziye delik açmanın bir yolu yoktur, bu da mağara oluşturmayı imkansız hale getirir. Ayrıca çıkıntı oluşturmanın da bir yolu yoktur,

 bunu taklit etmek için ayrı ağlar kullanın. Daha fazla bilgi için [izleme sorunu](https://github.com/FyroxEngine/Fyrox/issues/351) bölümüne bakın

.
