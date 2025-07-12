# Birinci Şahıs Nişancı Oyunu Eğitimi

Bu eğitimde, birinci şahıs nişancı oyunu oluşturacağız.

Başlamadan önce, proje oluşturmayı ve oyunu ve editörü çalıştırmayı bildiğinizden emin olun. Önce 
[bu bölümü](../../../beginning/scripting.md) okuyun ve bir dizinde aşağıdaki 
komutu çalıştırarak yeni bir proje oluşturarak başlayalım:

```shell
fyrox-template init --name=fps --style=3d
```

Bu komut, içinde birkaç proje bulunan yeni bir kargo çalışma alanı oluşturacaktır. Bu eğitimde sadece `game` klasörüyle ilgileniyoruz.


```text
fps
├───data
├───editor
│   └───src
├───executor
│   └───src
├───executor-android
│   └───src
├───executor-wasm
│   └───src
└───game
    └───src
```

## Oyuncu Prefab

Öncelikle oyuncu için bir [prefab](../../../scene/prefab.md) oluşturalım. Birinci şahıs nişancı oyunları karakterler için oldukça basit 
düzenler kullanır - genellikle, üstünde bir kamera bulunan fiziksel bir kapsüldür. Aşağıdaki komutu kullanarak editörü çalıştırın:
```shell
cargo run --package editor
```

![editor startup](editor_1.png)

Varsayılan olarak, `scene.rgs` sahnesi yüklenir ve bu bizim ana sahnemizdir, ancak oyuncu prefabrikimiz için ayrı bir sahneye ihtiyacımız var.
`file` menüsüne gidin ve `new scene` seçeneğine tıklayın. Sahneyi `data/player` klasörüne `player.rgs` olarak kaydedin. 

Harika, artık prefab'ı oluşturmaya hazırız. World Viewer'da `__ROOT__` düğümüne sağ tıklayın ve `Replace Node`'u bulun 
ve orada `Physics -> Rigid Body`'yi seçin. Bunu yaparak, sahnenin kök düğümünü rijit bir cisim ile değiştirdik. 
Bu, oyuncumuz hareket edeceği için gereklidir.

![replace node](editor_2.png)

Rigid body'yi seçin ve `X/Y/Z Rotation Locked` özelliklerini `true`, `Can Sleep` özelliğini `false` olarak ayarlayın. İlk üç özellik, rigid body'nin istenmeyen dönüşlerini önler ve son özellik, rigid body'nin simülasyonlardan hariç tutulmasını önler.

![rigid body props](rigid_body_props.png)

Fark edebileceğiniz gibi, düzenleyici kök düğümün yanına küçük bir “uyarı” simgesi ekledi - bu, katı cismin çarpışma algılayıcısı olmadığını belirtir.
Bunu düzeltelim:

![collider node](editor_3.png)

Varsayılan olarak, düzenleyici bir küp çarpıştırıcı oluşturur, ancak bizim bir kapsüle ihtiyacımız var. Bunu Inspector'da değiştirelim:

![change collider type](editor_4.png)

Şimdi çarpışmanın boyutunu değiştirelim, çünkü varsayılan değerler insansı bir karakter için orantısızdır:

![collider properties](editor_5.png)

Bu şekilde kapsül daha ince ve daha uzun olur, bu da yaklaşık 1,8 m boyundaki bir kişiye karşılık gelir. Şimdi bir [kamera](../../../scene/camera_node.md) eklememiz gerekiyor, çünkü onsuz hiçbir şey göremeyiz.

![camera](editor_6.png)

Kamerayı kapsülün üstüne şu şekilde yerleştirin:

![camera position](editor_7.png)

Harika, bu noktada prefabrik yapıyı neredeyse tamamladık. Sahneyi kaydedin (`file -> save scene`) ve kod yazmaya başlayalım.

## Kod

Şimdi karakterimizi yönlendirecek bazı kodlar yazmaya başlayabiliriz. Oyun mantığı [scripts](../../../scripting/script.md) içinde bulunur.
`fps` dizinine gidin ve orada aşağıdaki komutu çalıştırın:

```shell
fyrox-template script --name=player
```

Bu komut, `game/src` klasöründe oyuncumuz için yeni bir komut dosyası oluşturur. Ardından, `lib.rs` dosyasındaki içe aktarmaları aşağıdakilerle değiştirin:

```rust
{{#include ../../../code/tutorials/fps/game/src/snips/lib.rs:player_imports}}
```

ve ardından, içe aktarmaların ardından `pub mod player;` ekleyerek yeni modülü `lib.rs` modülüne ekleyin:

```rust
{{#include ../../../code/tutorials/fps/game/src/snips/lib.rs:player_mod_reg}}
```

Tüm komut dosyaları motorda açıkça kaydedilmelidir, aksi takdirde çalışmayacaktır. Bunu yapmak için, aşağıdaki satırları `register` yöntemine ekleyin:

```rust
{{#include ../../../code/tutorials/fps/game/src/lib.rs:player_script_reg}}
```

Harika, yeni komut dosyası kaydedildi, şimdi `player.rs` modülüne geçip temel karakter denetleyicisini yazmaya başlayabiliriz.
Önce içe aktarmayı aşağıdaki şekilde değiştirin:

```rust
{{#include ../../../code/tutorials/fps/game/src/snips/player.rs:player_imports}}
```


 Giriş işleme ile başlayalım.
İlk olarak, `Player` yapısına aşağıdaki alanları ekleyin:

```rust
{{#include ../../../code/tutorials/fps/game/src/player.rs:input_fields}}
```

İlk dört alan dört yönde hareketi, son iki alan ise kamera dönüşünü kontrol eder.
Şimdi yapmamız gereken şey, gelen işletim sistemi olaylarına doğru şekilde tepki vererek az önce tanımladığımız değişkenleri değiştirmektir.
Aşağıdaki kodu `on_os_event` yöntemine şu şekilde ekleyin:

```rust
{{#include ../../../code/tutorials/fps/game/src/player.rs:on_os_event}}
```

Bu kod iki ana bölümden oluşur:

- Kamera dönüşleri için ham fare girişi işleme: Kamerayı dikey eksen etrafında döndürmek için yatay hareketi kullanıyoruz
ve kamerayı yatay eksen etrafında döndürmek için dikey fare hareketini kullanıyoruz.
- Hareket için klavye girişi işleme.

Bu sadece dahili komut dosyası değişkenlerini değiştirir ve temel olarak başka hiçbir şeyi etkilemez.

Şimdi kamera dönüşünü ekleyelim, önce kamera tutamağını bilmemiz gerekiyor. Aşağıdaki alanı `Player` yapısına ekleyin:

```rust
{{#include ../../../code/tutorials/fps/game/src/player.rs:camera_field}}
```

Bu alanı daha sonra düzenleyicide atayacağız, şimdilik koda odaklanalım. Aşağıdaki kod parçasını `on_update`'in başına ekleyin:

```rust
{{#include ../../../code/tutorials/fps/game/src/player.rs:camera_rotation}}
```

Bu kod parçası nispeten basittir: ilk olarak, sahne grafiğindeki kamerayı 
kullanarak onun tutamağını ödünç almaya çalışıyoruz, başarılı olursa, Y ve X eksenleri etrafındaki dönüşleri temsil eden iki kuaterniyon oluşturuyor ve bunları 
basit çarpma işlemiyle birleştiriyoruz. 

```rust
{{#include ../../../code/tutorials/fps/game/src/player.rs:on_update_begin}}
{{#include ../../../code/tutorials/fps/game/src/player.rs:on_update_end}}
```

Bu kod, WSAD tuşlarından herhangi biri basıldığında hareketi kontrol eder. İlk olarak, bu komut dosyasının atandığı düğümü ödünç almaya çalışır, ardından WSAD tuşlarından herhangi biri basılı olup olmadığını kontrol eder ve düğümün temel vektörlerini kullanarak yeni bir hız vektörü oluşturur. Son adım olarak, vektörü normalleştirir (uzunluğunu bir birim yapar) ve bunu katı cisim hızına ayarlar.

Komut dosyamız neredeyse hazır, şimdi tek yapmamız gereken onu oyuncunun prefabrikine atamak. Editörde `player.rgs` prefabrikini açın, `Player` düğümünü seçin ve Player komut dosyasını atayın.



Komut dosyamız neredeyse hazır, şimdi tek yapmamız gereken onu oyuncunun prefabrikine atamak. Editörde `player.rgs` prefabrikini açın, `Player` düğümünü seçin ve ona Player komut dosyasını atayın. Kamera tutamağını ayarlamayı unutmayın (küçük yeşil düğmeye tıklayarak ve listeden Kamera'yı seçerek):

![script instance](script_instance.png)

Harika, artık oyuncu hareketini tamamladık. Ana sahnemizde test edebiliriz, ancak önce basit bir seviye oluşturalım.
`scene.rgs` dosyasını açın ve çarpışıcı içeren bir katı cisim oluşturun. Rigid body'nin alt öğesi olarak bir küp ekleyin ve onu bir tür zemin şekline getirin.
Çarpışmayı seçin ve `Shape`'ini `Trimesh` olarak ayarlayın, oraya bir geometri kaynağı ekleyin ve onu zemine yönlendirin. 
Rigid body'yi seçin ve türünü `Static` olarak ayarlayın. Küpün daha iyi görünmesi için ona bir doku da ekleyebilirsiniz.
Şimdi sahnede oyuncu prefab'ımızı oluşturabiliriz. Bunu yapmak için, Asset Browser'da `player.rgs`'yi bulun, üzerine tıklayın,

Şimdi sahnede oyuncu prefabrikimizi örneklendirebiliriz. Bunu yapmak için, Varlık Tarayıcısında `player.rgs` dosyasını bulun, üzerine tıklayın, düğmeyi basılı tutun, fareyi sahnenin üzerine getirin ve düğmeyi bırakın. Bundan sonra prefabrik, imleç konumunda şu şekilde örneklendirilmelidir:

![prefab instance](prefab_instance.png)

Bundan sonra, “Oynat” düğmesine (sahne önizlemesinin üzerindeki yeşil üçgen) tıklayabilirsiniz ve şuna benzer bir şey görmelisiniz:

![running game](running_game_1.png)

WSAD tuşlarını kullanarak yürümek ve fareyi kullanarak kamerayı döndürmek mümkün olmalıdır.

## Sonuç

Bu eğitimde, klavyeyi kullanarak hareket etmenizi ve fareyi kullanarak etrafınıza bakmanızı sağlayan temel bir karakter denetleyicisi oluşturduk. Bu eğitim, kendi oyununuzu oluşturmanıza yardımcı olacak, motorda kullanılan ana geliştirme stratejilerini gösterdi. Bir sonraki eğitimde silahlar ekleyeceğiz.
