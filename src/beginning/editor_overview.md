# FyroxEd Genel Bakış


FyroxEd - Fyrox'un yerel editörüdür ve tek bir amaçla oluşturulmuştur:
nispeten az çabayla oyununuzu baştan sona oluşturmanıza yardımcı olan entegre bir oyun geliştirme ortamı sunmak.


Editörde çok fazla zaman geçireceksiniz, bu nedenle editöre aşina olmalı ve temel işlevlerini öğrenmelisiniz.
Bu bölümde temel bilgiler verilecek, ileri düzey konular ise ilgili bölümlerde ele alınacaktır.


## Windows


Editörü ilk kez açtığınızda, karşınıza çıkan pencerelerin, düğmelerin, listelerin vb. sayısı sizi şaşırtabilir.
Her pencere farklı bir amaca hizmet eder, ancak hepsi birlikte çalışarak oyununuzu oluşturmanıza yardımcı olur. editörün ekran görüntüsüne bir göz atalım ve her bir parçasının ne işe yaradığını öğrenelim (geliştirme oldukça hızlı olduğundan ve görüntüler kolayca güncelliğini yitirebileceğinden, bunların zamanla değişebileceğini lütfen unutmayın):

![Windows](./overview.png)

- **World viewer** - sahnedeki tüm nesneleri ve bunların ilişkilerini gösterir. Sahnenin içeriğini hiyerarşik bir biçimde inceleme ve düzenleme olanağı sağlar. 

- **Sahne önizlemesi** - sahneyi hata ayıklama bilgileri ve çeşitli düzenleyiciye özgü nesnelerle (gizmo, varlık simgeleri,

vb.) birlikte görüntüler. Çeşitli varlıkları seçmenize, taşımanıza, döndürmenize, ölçeklemenize, silmenize vb. olanak tanır. Sol tarafındaki **Araç çubuğu**,

 bağlama bağlı olarak kullanılabilir araçları gösterir.

Toolar
- **Inspector** - seçilen nesnenin çeşitli özelliklerini değiştirmenize olanak tanır.

- **Message Log** - editörden gelen önemli mesajları görüntüler.

- **Navmesh Paneli** - navigasyon ağları oluşturmanıza, silmenize ve düzenlemenize olanak tanır.

- **Komut Yığını** - en son eylemlerinizi görüntüler ve değişiklikleri geri almanıza veya yeniden yapmanıza olanak tanır.

- **Asset Browser** - oyununuzun varlıklarını incelemenizi ve sahnedeki kaynakları örneklendirmenizi sağlar.

- **Audio Context** - sahnenin ses bağlamının ayarlarını (genel ses seviyesi, kullanılabilir ses kanalları, efektler,

vb.)

## Scene oluşturma veya yükleme

FyroxEd sahnelerle çalışır - bir sahne, oyun öğelerini içeren bir kaptır, bir seferde bir sahne oluşturabilir ve düzenleyebilirsiniz.
Editörle çalışmaya başlamak için bir sahne yüklemeniz gerekir. Bir sahne oluşturmak için `File-> New Scene` seçeneğine gidin.


Mevcut bir sahneyi yüklemek için, `file-> Load` seçeneğine gidin ve dosya tarayıcıdan istediğiniz sahneyi seçin. Son açılan
sahneleri, `File-> Recent Scenes` seçeneğine gidip istediğinizi seçerek daha hızlı yükleyebilirsiniz.

## Sahneyi Doldurma


Bir sahne çeşitli oyun öğeleri içerebilir. Bunları oluşturmanın iki eşdeğer yolu vardır:


- Ana menüden `Oluştur` seçeneğine gidip açılır menüden istediğiniz öğeyi seçerek.
- `Dünya Görüntüleyicisi`nde bir oyun öğesine sağ tıklayıp `Alt Öğe Oluştur` alt menüsünden istediğiniz öğeyi seçerek.


Genellikle 3D modelleme yazılımlarında (Blender, 3Ds Max, Maya vb.) oluşturulan karmaşık nesneler çeşitli formatlarda kaydedilebilir.
Fyrox, hemen hemen tüm 3D modelleme yazılımları tarafından desteklenen FBX formatını destekler. Bu tür nesneleri istediğinizi sürükleyip `Scene Preview` üzerine bırakarak kolayca oluşturabilirsiniz. Sürüklerken, nesnenin bir önizlemesi de görünecektir. 


Aynı işlemi, düzenleyicide oluşturulan diğer sahnelerle (`rgs` dosyaları) de yapabilirsiniz. Örneğin, birkaç nesne içeren bir sahne
ve bazı komut dosyaları ile sahne oluşturabilir ve bunları diğer sahnelerde yeniden kullanabilirsiniz. Bu tür sahnelere [prefabs](../scene/prefab.md) adı verilir.

## Scene kaydetme

TÇalışmanızı kaydetmek için, `File -> Save` seçeneğine gidin. Yeni bir sahne kaydediyorsanız, editör sizden bir dosya adı ve

sahnenin kaydedileceği yolu belirtmenizi isteyecektir. Bir dosyadan yüklenen sahneler otomatik olarak yüklendikleri yola kaydedilecektir.

## Geri alma ve yeniden yapma

FyroxEd, eylemlerinizi hatırlar ve bunlarla yapılan değişiklikleri geri almanıza ve yeniden yapmanıza olanak tanır. Değişiklikleri geri almak veya yeniden yapmak için `Edit -> Undo/Redo` seçeneğine gidin veya şu kısayolları kullanın: `Ctrl+Z` - geri almak için, `Ctrl+Y` - yeniden yapmak için.

## Kontroller

Çoğu zaman kullanacağınız bir dizi kontrol tuşu vardır, bunların hemen hepsi `Scene Preview` penceresinde çalışır:

### Editör kamera hareketi

Hareket kontrollerini etkinleştirmek için `Sahne Önizleme` penceresinde `[Sağ Fare Tuşu]` tuşunu basılı tutun:

- `[W][S][A][D]` - Kamerayı ileri/geri/sola/sağa hareket ettir

- `[Boşluk][Q]/[E]` - Kamerayı yukarı/aşağı hareket ettir
  - `[Ctrl]` - Hızlandır


  - `[Shift]`- Yavaşlat


### Diğerleri
- `[Sol Fare Tuşu]` - Seç

- `[Orta Fare Tuşu]` - Görüntüleme düzleminde kamerayı kaydır

- `[1]` - Etkileşim modunu seç

- `[2]` - Etkileşim modunu taşı

- `[3]` - Etkileşim modunu ölçeklendir

- `[4]` - Etkileşim modunu döndür

- `[5]` - Navigasyon ağı düzenleme modu

- `[6]` - Arazi düzenleme etkileşim modu

- `Ctrl]+[Z]` - Geri al

- `Ctrl]+[Y]` - Yeniden yap

- `[Delete]` - Mevcut seçimi sil.

## Oynatma Modu



Editörün en önemli özelliklerinden biri, oyunu ayrı bir işlemde çalıştırmanıza olanak tanımasıdır. Oynatma Moduna girmek veya çıkmak için `scene priwiew` penceresinin üst kısmındaki `Oynat/Durdur`

düğmesini kullanın. Oynatma Modundayken editör kullanıcı arayüzünün kilitleneceğini unutmayın


.
 


Oynatma Modu yalnızca `fyrox-template` ile oluşturulan projeler (veya benzer yapıya sahip projeler) için etkinleştirilebilir. Editör


, oyununuzu ayrı bir işlemde derlemek ve çalıştırmak için `cargo` komutlarını çağırır. Oyunu ayrı bir işlemde çalıştırmak,

oyununuz çökerse editörün de çökmemesini sağlar ve ayrıca oyun ile editör arasında mükemmel bir izolasyon sağlar,

oyunu çalıştırarak editörü bozma ihtimalini ortadan kaldırır.

## Ek Yardımcı Programlar



Hayatınızı kolaylaştıracak bir dizi güçlü yardımcı program da bulunmaktadır. Bunlar ana menünün `Utils` bölümünde bulunabilir:



- Curve Editor - oyun parametreleri için karmaşık kurallar oluşturmak üzere eğri kaynakları oluşturmanıza ve düzenlemenize olanak tanır.

- Path Fixer - sahnelerinizdeki hatalı kaynak referanslarını düzeltmenize yardımcı olur.
