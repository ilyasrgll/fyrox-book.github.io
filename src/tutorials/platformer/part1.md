# Character Controller

## İçindekiler

- [2D Platform Oyunu Öğreticisi](#2d-platformer-tutorial)
  - [İçindekiler](#içindekiler)
  - [Giriş](#giriş)
  - [Proje](#proje)
  - [Editörün Kullanımı](#Editorun-Kullanımı)
  - [Komut Dosyaları - Oyuncu](#Scriptler-(Komut-dosyaları)-Player)
  - [Animasyon](#animasyon)
  - [Sonuç](#sonuç)

## Giriş



Bu eğitimde, 2D platform oyunumuz için bir karakter denetleyicisi oluşturacağız. Eğitimi tamamladıktan sonra elde edeceğiniz sonuçlar şunlar olacaktır:

<iframe width="560" height="315" src="https://youtube.com/embed/EcvtwEkBxNU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Öğreticinin kaynak kodunu [burada](https://github.com/fyrox-book/fyrox-book.github.io/tree/main/src/code/tutorials/platformer) bulabilirsiniz,

deposu klonlayıp `platformer` dizininde `cargo run --package editor --release` komutunu çalıştırarak kendiniz test edebilirsiniz.

## Proje

Özel küçük araç olan `fyrox-template` kullanarak yeni bir proje oluşturarak başlayalım. Bu araç, tüm şablon parçalarını tek bir komutla oluşturmanıza olanak tanır. Aşağıdaki komutu kullanarak yükleyin:

```shell
cargo install fyrox-template
```

Projenin oluşturulmasını istediğiniz klasöre gidin ve aşağıdaki komutu uygulayın:

```shell
fyrox-template init --name platformer --style 2d
```

Araç iki argüman kabul eder - proje adı ve stil, biz 2D oyunla ilgileniyoruz, bu yüzden stil 2D olarak ayarlanmıştır. Proje oluşturulduktan sonra, iki komutu ezberlemelisiniz:

- `cargo run --package editor --release` - oyununuz ekli olarak editörü başlatır, editör oyununuzu içinde çalıştırmanıza ve oyun öğelerini düzenlemenize olanak tanır. Yalnızca geliştirme amacıyla kullanılmak üzere tasarlanmıştır.

- `cargo run --package executor --release` - oyununuzun sevk edilebilir (örneğin bir mağazaya) üretim ikili dosyasını oluşturur ve çalıştırır.

`platformer` dizinine gidin ve `cargo run --package editor --release` komutunu çalıştırın. Bir süre sonra editörün açıldığını göreceksiniz:

![editor](editor.png)

cHarika! Artık oyunumuzu yapmaya başlayabiliriz. `game/src/lib.rs` dosyasına gidin - oyun mantığınızın bulunduğu yer burasıdır, görebileceğiniz gibi `fyrox-template` sizin için oldukça fazla kod üretir. Hangi yerin ne için olduğu hakkında küçük açıklamalar vardır. Her yöntem hakkında daha fazla bilgi için lütfen [belgelere](https://docs.rs/fyrox/latest/fyrox/plugin/trğait.Plugin.html) bakın.

## Editorun Kullanımı

Şimdilik tek bir satır kod bile yazmamıza gerek yok, sahneyi tamamen editörde oluşturabiliriz. 

Bu bölümde sahne oluşturma sürecini adım adım anlatacağız. Sonuç olarak şuna benzer bir şey elde edeceğiz:

![editor with scene](editor_with_scene.png)

İlk olarak, bazı assetlere ihtiyacımız var. Gerekli olanların tümünü (ve biraz daha fazlasını) ayrı bir zip arşivinde hazırladım, böylece internette assetler aramanıza gerek kalmayacak. assetleri [buradan](assets.zip) indirin ve projenizin kök klasöründeki `data` klasörüne açın.

Sahneyi doldurmaya başlayalım. Editörü çalıştırın ve oluşturulan sahneden tüm içeriği kaldırın. 2D oyun yapacağımız için, editörün kamera modunu sahne önizleme penceresinin üst araç çubuğunda `2D` olarak değiştirin. Şimdi sahneyi bazı nesnelerle doldurmamız gerekiyor, basit bir zemin bloğu ekleyerek başlayacağız. World Viewer'da sahnenin __ROOT__ öğesine sağ tıklayın ve Add Child -> Physics2D -> Rigid Body'yi seçin. Bu, zemin bloğu için bir rigid body oluşturacaktır. Rigid body i seçin ve `Inspector`'da `Body Type`'ı `Static` olarak ayarlayın. Böylece fizik motoruna zemin bloğumuzun hareket etmemesi ve kaya gibi sağlam olması gerektiğini bildirmiş oluruz. Her rigid body bir collider a ihtiyaç duyar, aksi takdirde fizik motoru çarpışmaları nasıl işleyeceğini bilemez.  `Inspector`'da katı cisme sağ tıklayın ve `Add Child -> Physics2D -> Collider`'ı tıklayın. rigid body e yeni bir callider ekledik, varsayılan olarak `1,0` metre yüksekliğinde ve genişliğinde `Cuboid` şekline sahiptir. Son olarak, rigiy body e bazı grafikler eklememiz gerekiyor, rigid body sağ tıklayın ve `Add Child -> 2D -> Rectangle` seçeneğine tıklayın. Bu, basit bir 2D sprite ekler, onu seçin ve ayarlayın. Bunun için, inspector'da `Material` özelliğini bulun, yanındaki `Edit` düğmesine tıklayın ve `diffuseTexture` özelliğini ayarlayın. Bunu yapmak için, varlık tarayıcısından özelliğe dokunup sürükleyin. Benim sahnemde üç sprite kullanacağım.

- `data/tiles/13.png` - sol zemin bloğu

- `data/tiles/14.png` - orta zemin bloğu

- `data/tiles/15.png` - sağ zemin bloğu

Diğer dokuları kullanabilir ve seviyenizi istediğiniz gibi oluşturabilirsiniz. Tüm bu adımları tamamladıktan sonra aşağıdaki gibi bir sonuç elde etmelisiniz:

![editor_step1](editor_step1.png)

Blokun katı gövdesini seçip `Ctrl+C` ve ardından `Ctrl+V` tuşlarına basarak bloğu kopyalayın, kopyalanan sprite'a gidin ve

dokuyu bloğun sol veya sağ ucuna değiştirin. `Move Tool` aracını kullanarak bloğu istediğiniz yere taşıyın (ayrıca

`File -> Setting` menüsüne gidip `Move Interaction Mode` için `Snap To Grid` seçeneğini ayarlayarak ızgara yakalama özelliğini de kullanabilirsiniz). Aynısını karşı uç için de yapın,

sonuçta şuna benzer bir şey elde etmelisiniz:

![editor_step2](editor_step2.png)

İsterseniz, daha fazla platform eklemek için bu adımları tekrarlayın. Yeni bir sprite oluşturarak bazı arka plan nesneleri de ekleyebilirsiniz

(`__ROOT__` üzerine sağ tıklayın ve `Add Child -> 2D -> Rectangle` seçeneğine tıklayın) ve ona bir doku atayın:

![editor_step3](editor_step3.png)

Dünya düzenlemenin son adımı olarak, kutular gibi bazı dinamik nesneler ekleyelim. Rastgele bir zemin bloğu seçin, sert gövdesini seçin ve
klonlayın. Kopyasının gövde türünü `Dinamik` olarak değiştirin. Şimdi sprite dokusunu bir kutuya değiştirin (`data/objects/Crate.png` dosyasını
`Texture` alanına sürükleyip bırakın) ve kutuyu birkaç kez kopyalayın, şöyle bir şey elde etmelisiniz:

![editor_step4](editor_step4.png)

Şimdi oyuncuya geçelim. Her zamanki gibi, yeni bir katı cisim oluşturarak başlayalım, ona bir 2D çarpıştırıcı ekleyelim ve şeklini kapsül olarak ayarlayalım. parametreleri - `Başlangıç = 0.0, 0.0` ve `Bitiş = 0.0, 0.3`. Rigid body'ye bir 2D sprite (dikdörtgen) ekleyin ve dokusunu `data/characters/adventurer/adventurer-Sheet.png` olarak ayarlayın. Tek bir kareyi görmek için uv rect'i `(0.0, 0.0, 0.143, 0.091)` olarak ayarlayın. Ayrıca bir kameraya da ihtiyacımız var, aksi takdirde hiçbir şey göremeyiz. Bunu oyuncunun katı cisminin bir alt öğesi olarak ekleyin. Varsayılan olarak,  kameramızın arka planı olmayacak, siyah bir “boşluk” olacak, bu pek iyi değil, bunu düzeltelim. Kamerayı seçin ve `Skybox` özelliğini `Some` olarak ayarlayın. Şimdi varlık tarayıcısına gidin ve `data/background/BG.png` dosyasını bulun, sürükleyip `Skybox` özelliğinin `Front` alanına bırakın. Uzak düzlem mesafesini `20.0` gibi bir değere ayarlamayı unutmayın,  aksi takdirde arka plan görüntüsünün sadece bir kısmını görürsünüz. Her şey doğru yapıldıysa, şuna benzer bir sonuç almalısınız:

![editor_step5](editor_step5.png)

Sahneyi `Dosya -> Sahneyi Kaydet` seçeneğine giderek kaydedin. Artık sahne önizleyicisinin üst kısmındaki `Oynat/Durdur` düğmesini kullanarak oyunu çalıştırabiliriz.
 Sahne önizlemesinde gördüğünüzün aynısını görmelisiniz, ancak
rigid body şekilleri, düğüm sınırları vb. gibi hizmet grafikleri hariç. Artık komut dosyalarını yazmaya başlayabiliriz.

Son hazırlık adımı olarak, başlangıçta tüm varlıkları içe aktaralım, böylece bunları manuel olarak bulmanıza gerek kalmaz.
game/src/lib.rs dosyasının başına aşağıdaki kodu ekleyin:

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:imports}}
```

## Scriptler (Komut dosyaları) Player

Sahne, komut dosyalarını eklemeye başlamak için ihtiyacımız olan hemen hemen her şeye sahip. `Player` komut dosyasından başlayıp karakterimizi

hareket ettireceğiz. `game/src/lib.rs` dosyasına gidin ve dosyanın sonuna aşağıdaki kod parçasını ekleyin:

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/residuals.rs:player_stub_script}}
```

Bu, herhangi bir komut dosyasının tipik bir “iskeletidir”. Şu an için yöntemleri neredeyse boştur, çok yakında gerçek kodlarla dolduracağız. En önemli kısımları gözden geçirelim. Kod parçacığı, `#[derive(Visit, Inspect, Debug, Clone, Default)]` özniteliklerine sahip `Player` yapı tanımından başlar:

- `Visit` - serileştirme/serileştirmeyi kaldırma işlevini uygular, editör tarafından nesnenizi bir sahne dosyasına kaydetmek için kullanılır.

- `Inspect` - türünüzün alanları için meta veriler oluşturur - başka bir deyişle, editörün yapınızın içindekileri “görmesini” ve proc-macro öznitelikleri aracılığıyla alanlara ekli ek bilgileri göstermesini sağlar.

- `Reflect` - editörün nesnelerinizi değiştirebilmesini sağlayan derleme zamanı yansıtmayı uygular.

- `Reflect` - editörün nesnelerinizi değiştirebilmesini sağlayan derleme zamanı yansıtma işlevini gerçekleştirir.

- `Debug` - hata ayıklama işlevini sağlar, çoğunlukla editörün konsola çıktı alabilmesi içindir.

- `Clone` - yapınızı klonlanabilir hale getirir, buna neden ihtiyacımız var? Nesneleri klonlayabiliriz ve ayrıca komut dosyası örneğinin de kopyalanmasını

- `Default` uygulaması çok önemlidir - komut dosyası sistemi, komut dosyalarınızı varsayılan durumda oluşturmak için bunu kullanır.

Buna bazı verileri ayarlamak vb. için gereklidir. Özel bir durum varsa, komut dosyanız için gerekliyse her zaman kendi `Default` uygulamanızı uygulayabilirsiniz.

- `TypeUuidProvider`, türünüz için benzersiz bir kimlik eklemek için kullanılır, her komut dosyası **mutlaka* benzersiz bir kimliğe sahip olmalıdır, aksi takdirde motor komut dosyalarınızı kaydedemez ve yükleyemez. Yeni bir UUID oluşturmak için [Çevrimiçi UUID Oluşturucu](https://www.uuidgenerator.net/) veya UUID oluşturabilen başka bir araç kullanın.



Son olarak, `Player` için `ScriptTrait`'i uygularız. Bir dizi yöntemi vardır ve isimleri kendilerini açıklar.
[Belgelerde](https://docs.rs/fyrox/latest/fyrox/script/trait.ScriptTrait.html) Komut dosyasını düzenleyicide kullanabilmemiz için, komut dosyasının var olduğunu motoruna bildirmeliyiz, yani komut dosyasını kaydetmeliyiz.

`PluginConstructor` özelliğinin uygulamasındaki `register` yöntemini hatırlıyor musunuz? Bu yöntem tam olarak komut dosyalarını kaydetmek içindir, uygulamasını aşağıdaki

kod parçacığıyla değiştirin:

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:register}}
```

Artık motor bizim komut dosyamızı tanıyor ve kullanabilecek. Şu anki haliyle pek bir işe yaramıyor ama şimdiden
oyuncuya atayabiliriz. Oyuncunun rigid body düğümünü seçin ve `Inspector`'da `Script`'i bulun, ilgili

açılır listeden `Player`'ı seçin ve hepsi bu kadar - komut dosyası atandı:

![script_selection](script_selection.png)

Editörden komut dosyası özelliklerini nasıl düzenleyeceğimizi öğrenelim. Bir sonraki bölümde, karakterinize anahtar kare animasyonu ekleyeceğiz.

Bu, motorun ve editörün komut dosyalarındaki kullanıcı tanımlı özelliklerle nasıl çalıştığını öğrenmek için mükemmel bir fırsat. Oyuncuyu canlandırmak için

önce onun sprite'ını almamız gerekiyor. `Player` yapısına gerekli alanı ekleyerek başlayalım:

```rust,no_run,compile_fail
{{#include ../../code/tutorials/platformer/game/src/lib.rs:sprite_field}} 
```

Bunu ekledikten sonra, editör alanı görebilecek ve size Inspector'da düzenleme olanağı sağlayacaktır. Sprite'ın doğru tanıtıcısını script özelliklerindeki ilgili alana atamak için, `Alt` tuşunu basılı tutun ve sprite düğümünü dünya görüntüleyiciden player script'indeki ilgili alana sürüklemeye başlayın. Fare düğmesini bırakın ve  her şey yolundaysa, alan “Atanmamış” dışında farklı bir şey “söylemelidir”. Tamam, bu noktada komut dosyası özellikleriyle nasıl çalışılacağını öğrendik, şimdi oyuncu için temel hareketleri eklemeye başlayabiliriz.

`Player` yapısına gidin ve aşağıdaki alanları ekleyin:

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:movement_fields}}
```

Bu alanlar, oyuncunun hareketinden sorumlu klavye tuşlarının durumunu depolayacaktır. Şimdi `on_os_event` için aşağıdaki kodu ekleyin:

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:on_os_event}}
```

Kod, işletim sistemi olaylarına yanıt verir ve buna göre dahili hareket bayraklarını değiştirir. Şimdi bu bayrakları bir şekilde kullanmamız gerekiyor, `on_update` zamanı geldi. Bu yöntem her karede çağrılır ve oyun mantığını buraya yerleştirmenize olanak tanır:

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:on_update_begin}}
{{#include ../../code/tutorials/platformer/game/src/lib.rs:on_update_closing_bracket_2}}
{{#include ../../code/tutorials/platformer/game/src/lib.rs:on_update_closing_bracket_1}}
```

Sonunda ilginç bir kod. İlk olarak, komut dosyasının atandığı düğümün 2d rigid body olup olmadığını kontrol ediyoruz, ardından
hareket bayraklarını ve yatay hızı kontrol ediyoruz ve gövdeye hız uyguluyoruz. Hız iki şekilde uygulanır:
zıplama düğmesine basıldıysa - yatay hız ve zıplama için biraz dikey hız uygulayın. Zıplama düğmesine basılmadıysa -sadece yatay hızı değiştirin - bu, oyuncunun serbest düşüş yapmasını sağlar.


Editörü çalıştırın ve oyun moduna girin, her şeyin doğru çalışıp çalışmadığını kontrol etmek için `[A][D][Boşluk]` düğmelerine basın - oyuncu yatay olarak hareket etmeli ve zıplayabilmelidir. Sağdaki kutulara zıplayabilir ve onları çıkıntıdan itebilirsiniz. 

Hareket çalışıyor, ancak oyuncu yönünü değiştirmiyor, sola gidersek - her şey normal görünüyor (animasyon olmamasına rağmen), ancak sağa gidersek - oyuncu geriye doğru hareket ediyor gibi görünüyor. Bunu, oyuncunun sprite'ının yatay ölçeğini değiştirerek düzeltelim. Yukarıdaki kodun `if let ...` bloğunun sonuna aşağıdaki kodu ekleyin:

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:sprite_scaling}}
```

Yorumlar burada neler olduğunu açıklığa kavuşturmalıdır, ancak kısaca, oyuncu hareket halindeyse oyuncunun sprite'ının yatay ölçeğini değiştiriyoruz.  `current_scale.x.copysign(-x_speed)` satırı kafa karıştırıcı olabilir, ne işe yarar? Mevcut yatay ölçeğin işaretini `x_speed`'in ters işareti ile değiştirir.



Şimdi oyunu çalıştırırsanız, oyuncu hız vektörüne bağlı olarak doğru yöne “bakacaktır”.

## Animasyon



2D oyun yaptığımız için, anahtar karelerin sürekli değişmesine dayalı basit animasyonlar kullanacağız. Başka bir deyişle,

oyuncunun vücut sprite'ının dokusunu değiştireceğiz. Şansımıza, motorda sprite sheet animasyonları yerleşik olarak bulunuyor. 

Player'a aşağıdaki alanları ekleyin:

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:animation_fields}}
```

Şu anda sadece varsayılan değerleri aktarıyoruz.

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:animation_fields_defaults_begin}}
{{#include ../../code/tutorials/platformer/game/src/lib.rs:animation_fields_defaults_end}}
```

Oyuncu, gelecekteki derslerde birden fazla animasyon kullanacak, ancak şimdilik sadece iki animasyon kullanacak: boşta ve koşma.

Şimdi bir şekilde animasyonları değiştirmemiz gerekiyor. Player içindeki on_update'e gidin ve

x_speed bildiriminin

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:animation_selection}}
```

HBurada, koşma animasyonunun `1` indeksinde ve boşta animasyonunun `0` indeksinde olacağını varsayıyoruz. Ayrıca, mevcut animasyondaki dokuyu oyuncunun sprite'ına uygulamamız ve `on_update`'in sonuna aşağıdaki satırları eklememiz gerekiyor

```rust,no_run
{{#include ../../code/tutorials/platformer/game/src/lib.rs:applying_animation}}
```

Kod oldukça basit - önce indeksine göre mevcut animasyona bir referans almaya çalışıyoruz, ve başarılı olursak onu güncelliyoruz. Bir sonraki adımda, sprite'ı alıp mevcut animasyonun mevcut karesini atıyoruz.



Şimdi tekrar editöre gidip animasyonları `Player`'a eklememiz, oyuncunun rigid body'sini seçmemiz ve `Inspector`'da `Script` bölümünü bulmamız gerekiyor. Buraya iki animasyon ekleyin:

![editor_step6](editor_step6.png)

Animasyonları doldurduktan ve etkinleştirdikten sonra oyunu çalıştırabilirsiniz ve karakteriniz animasyonları

doğru şekilde oynatmalıdır.



## Sonuç



Bu eğitimde, motorun yeni komut dosyası sisteminin temellerini öğrendik. Oluşturduğumuz oyun çok 

basit, ancak bu sadece başlangıç. Düşmanlar, silahlar, toplanabilir öğeler vb. için daha fazla komut dosyası eklemek kolaydır.
