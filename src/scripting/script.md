# Scripts (Komut Dosyası)

Script - bir sahne düğümüne atanabilen oyun verileri ve mantığı için bir konteynerdir. Fyrox, scripting için Rust kullanır, 

bu nedenle scriptler yerel kod kadar hızlıdır. Her sahne düğümüne istediğiniz sayıda script atayabilirsiniz.



## Scriptleri (Komut Dosyasıları) Ne Zaman Kullanmalı, Ne Zaman Kullanmamalı



Scriptler, sahne düğümlerine veri ve bazı mantık eklemek için kullanılır. Bununla birlikte, scriptleri

oyununuzun genel durumunu tutmak için kullanmamalısınız (bunun için oyun eklentinizi kullanın). Örneğin, oyun öğeleriniz, 

botlar, oyuncu, seviye vb. için scriptleri kullanın. Öte yandan, liderlik tabloları, oyun menüleri, ilerleme bilgileri vb. için scriptleri **kullanmayın**.



Ayrıca, kasıtlı olarak Game <-> UI ayrıştırma nedenleriyle komut dosyaları UI widget'larına atanamaz. Tüm kullanıcı arayüzü 

bileşenleri, oyununuzun oyun eklentisinde oluşturulmalı ve yönetilmelidir.



## Script (Komut dosyası) Yapısı



Tipik bir komut dosyası yapısı şuna benzer:

```rust,no_run
{{#include ../code/snippets/src/scripting/example.rs:example_script}}
```

Her komut dosyası aşağıdaki özellikleri uygulamalıdır:



- `Visit`, serileştirme/serileştirmeyi kaldırma işlevini uygular ve editör tarafından nesnenizi bir sahne dosyasına kaydetmek için kullanılır.

- `Reflect`, komut dosyası alanlarını yinelemek, değerlerini ayarlamak, yollarından alanları bulmak vb. için bir yol sağlayan derleme zamanı yansıtma işlevini uygular.

- `Debug` - hata ayıklama işlevselliği sağlar, çoğunlukla editörün yapıyı ve alanlarını dizeye dönüştürmesi için kullanılır.

- `Clone` - yapınızı klonlanabilir hale getirir, nesneleri klonlayabildiğimiz için komut dosyası örneğinin de 
klonlanmasını isteriz.

- `Default` uygulaması çok önemlidir - komut dosyası sistemi, komut dosyalarınızı varsayılan durumda oluşturmak için bunu kullanır.

Bazı verileri ayarlamak vb. için gereklidir. Özel bir durum varsa, komut dosyanız için gerekliyse her zaman kendi `Default` uygulamanızı uygulayabilirsiniz.

- `TypeUuidProvider`, türünüz için benzersiz bir kimlik eklemek için kullanılır. Her komut dosyası **mutlaka** benzersiz bir kimliğe sahip olmalıdır, aksi takdirde 

motor komut dosyalarınızı kaydedip yükleyemez. Yeni bir UUID oluşturmak için [Çevrimiçi UUID Oluşturucu](https://www.uuidgenerator.net/) veya UUID oluşturabilen başka bir araç kullanın.

- `ComponentProvider` - `#[component(include)]` özniteliği ile işaretlenmiş komut dosyasının iç alanlarına erişim sağlar.



`#[visit(optional)]` özniteliği, bazı alanlar eksik veya değiştirildiğinde serileştirme hatalarını bastırmak için kullanılır.

## Script (Komut Dosyası) Şablonu Oluşturucu

Yeni bir komut dosyası için gerekli tüm hazır kodları oluşturmak için `fyrox-template` aracını kullanabilirsiniz, bu yeni komut dosyaları eklemeyi

çok daha kolay hale getirir. Yeni bir komut dosyası oluşturmak için `script` komutunu kullanın:

```shell
fyrox-template script --name MyScript
```

`game/src` dizininde `my_script.rs` adında yeni bir dosya oluşturulacak ve gerekli kodlarla doldurulacaktır.

Yeni komut dosyasını `lib.rs` dosyasına şu şekilde eklemeyi unutmayın:

```rust,no_run,compile_fail
// Use your script name instead of `my_script` here.
pub mod my_script;
```

Oluşturulan her yöntemde yer alan yorumlar, hangi kodun nereye yerleştirilmesi gerektiğini ve her yöntemin amacını anlamanıza yardımcı olacaktır.

> ⚠️ Her yeni komut dosyasının `PluginConstructor::register` içinde kaydedilmesi gerektiğini unutmayın, aksi takdirde
> komut dosyasını düzenleyicide bir düğüme atayamazsınız. Daha fazla bilgi için bir sonraki bölüme bakın.

## Script (Komut Dosyası) Kaydı

Her komut dosyası kullanılmadan önce kaydedilmelidir, aksi takdirde motor komut dosyanızı “görmez” ve onu bir nesneye atamanıza izin vermez.
`PluginConstructor` özelliği, komut dosyalarını kaydetmek için `register` yöntemine sahiptir. Bir komut dosyasını kaydetmek için,onu komut dosyası oluşturucular listesine şu şekilde kaydetmeniz gerekir:

```rust,no_run
{{#include ../code/snippets/src/scripting/example.rs:register}}
```

Her komut dosyası türü (yukarıdaki kod parçasında `MyScript`, bunu kendi komut dosyası türünüzle değiştirmeniz gerekir) 

[ScriptConstructorsContainer::add](https://docs.rs/fyrox/latest/fyrox/script/constructor/struct.ScriptConstructorContainer.html#method.add) 
yöntemiyle kaydedilmelidir. Bu yöntem, genel bir argüman olarak komut dosyası türünü ve düzenleyicide gösterilecek adını kabul eder.

 Ad isteğe bağlıdır ve yalnızca düzenleyicide kullanılır. Adı istediğiniz zaman değiştirebilirsiniz, mevcut sahneler bozulmaz.

## Script (Komut Dosyası) Eki

Bir komut dosyası atamak ve çalışırken görmek için düzenleyiciyi çalıştırın, bir nesne seçin ve Denetçi'de `Scripts` özelliğini bulun.

Küçük `+` düğmesine tıklayın ve yeni eklenen girişin açılır listesinden komut dosyanızı seçin. Komut dosyasını 
çalışırken görmek için “Çal/Durdur” düğmesine tıklayın. Editör, oyunu editörde sahne aktif haldeyken ayrı bir işlemde çalıştıracaktır.



Komut dosyası koddan bir sahne düğümüne eklenebilir:

```rust, no_run
{{#include ../code/snippets/src/scripting/example.rs:add_my_script}}
```

Yeni atanan komut dosyasının başlatılması ve güncellenmesi, motorun bir sonraki güncelleme tikinde gerçekleşecektir.

## Script (Komut Dosyası) Bağlamı

Script context, scriptlerden motor ve oyun durumunu değiştirmek için kullanılabilecek ortama erişim sağlar. Tipik

context içeriği şuna benzer:

```rust,no_run
{{#include ../code/snippets/src/scripting/context.rs:context}}
```

- `dt` - son kareden bu yana geçen süre. Değişkenin değeri uygulamaya göre tanımlanır, genellikle saniyenin 1/60'ı (0,016) gibi bir değerdir.

- `elapsed_time` - oyunun başlangıcından bu yana geçen süre (saniye cinsinden).

- `plugins` - kayıtlı tüm eklentilere değiştirilebilir bir referans, herhangi bir nesneye ait olmayan bazı “global” oyun verilerine erişmenizi sağlar.

 Örneğin, bir eklenti oyuncu kontrolleri için kullanılan tuş atamalarını saklayabilir, 


`plugins` alanını kullanarak ve istediğiniz eklentiyi bulabilirsiniz. Tek bir eklenti olması durumunda, referansı
 
`context.plugins[0].cast::<MyPlugin>().unwrap()` çağrısı kullanarak belirli bir türe dönüştürmeniz yeterlidir.

- `handle` - komut dosyasının atandığı düğümün (üst düğüm) tanıtıcısı. Düğümü

`context.scene.graph[handle]` çağrısı kullanarak ödünç alabilirsiniz. Belirli bir düğüm türüne referans elde etmek için tür dönüştürme kullanılabilir.

- `scene` - komut dosyasının üst sahnesine bir referans, sahne içeriğine tam erişim sağlar ve 
sahne düğümleri eklemenizi/değiştirmenizi/kaldırmanızı

- `scene_handle` - komut dosyası örneğinin ait olduğu sahnenin bir tanıtıcısı.


- `resource_manager` - kaynak yöneticisine bir referans, varlıkları yüklemek ve örneklendirmek için kullanabilirsiniz.
 
- `message_sender` - bir mesaj gönderici. Bu gönderici aracılığıyla gönderilen her mesaj, her 

- `ScriptTrait::on_message` yöntemine aktarılır.

- `message_dispatcher` - bir mesaj dağıtıcı. Belirli bir türdeki mesajları almanız gerekiyorsa,
bir türe açıkça abone olmanız gerekir.

- `task_pool` - eşzamansız görev yönetimi için görev havuzu.

- `graphics_context` - Motorun mevcut grafik bağlamı.

- `user_interfaces` - motorun kullanıcı arayüzü konteynerine bir referans. Motor, en az bir kullanıcı arayüzünün varlığını garanti eder. Buna referans almak için `context.user_interfaces.first()/first_mut()` kullanın.

- `script_index` - komut dosyasının dizini. Bu dizini asla kaydetmeyin, bu bağlam var olduğu sürece geçerlidir!

## Yürütme emri

Komut dosyalarının yöntemleri için kesin olarak tanımlanmış bir yürütme sırası vardır (yürütme sırası doğrusaldır ve **komut dosyasının bulunduğu grafiğin gerçek ağaç yapısına bağlı değildir**):

- `on_init` - her komut dosyası örneği için ilk olarak çağrılır

- `on_start` - her `on_init` çağrıldıktan sonra çağrılır


- `on_update` - bir render karesi başına sıfır veya daha fazla kez çağrılır. Motor, mantığın güncelleme hızını sabitler, bu nedenle oyununuz 15 FPS'de çalışıyorsa, mantık yine 60 FPS'de çalışır ve `on_update` her kare için 4 kez çağrılır. FPS çok yüksekse, yöntem hiç çağrılmayabilir. Örneğin, oyununuz 240 FPS'de çalışıyorsa, `on_update` 4 kare başına bir kez çağrılır.

- `on_message` - gelen her mesaj için bir kez çağrılır.

- `on_os_event` - gelen her işletim sistemi olayı için bir kez çağrılır.

- `on_deinit` - güncelleme döngüsünün sonunda, komut dosyası (veya üst düğüm) silinmek üzereyken bir kez çağrılır.



Bir sahne düğümüne birden fazla komut dosyası atanmışsa, bunlar sahne düğümüne atandıkları sırayla yukarıda açıklanan şekilde işlenir.

## Mesaj iletimi

Fyrox'un komut dosyası sistemi, komut dosyaları için mesaj iletimi destekler. Mesaj iletimi, bir düğüme, düğüm hiyerarşisine veya tüm grafiğe bazı veriler (mesaj) göndermenizi sağlayan bir mekanizmadır. Her komut dosyası belirli bir mesaj türüne abone olabilir. Bu, komut dosyalarını birbirinden ayırmanın etkili bir yoludur. Örneğin, oyununuzda bazı olayları algılayıp yanıtlamak isteyebilirsiniz. Bu durumda, olay gerçekleştiğinde, bir tür mesaj gönderirsiniz ve her “abone”  buna tepki verir. Bu şekilde aboneler gönderen(ler) hakkında hiçbir şey bilmezler; yalnızca mesaj verilerini bazı eylemleri gerçekleştirmek için kullanırlar. Mesaj aktarımının yararlı olabileceği basit bir örnek, oyununuzdaki bazı olaylara tepki vermeniz gerektiğinde ortaya çıkar.

 Oyununuzda silahlarınız olduğunu ve bazı hedefler vurulduğunda farklı renkte yanıp sönen lazer nişangahları olduğunu düşünün. oyununda silahların olduğunu ve bu silahların, bir hedef vurulduğunda farklı bir renkte yanıp sönen lazer nişangahları olduğunu düşün. Çok basit bir yaklaşımda, tüm lazer nişangahlarını, mermilerin kesiştiği tüm noktaları işleyerek yönetebilirsin, ancak bu, lazer nişangahları ile mermiler arasında çok sıkı bir bağlantı oluşturur. Bu tamamen gereksiz bir bağlantıdır ve mesaj aktarımı kullanılarak gevşetilebilir. Lazer nişangahlarını doğrudan yönetmek yerine, tek yapman gereken `ActorDamaged { actor: Handle<Node>, attacker: Handle<Node> }` mesajını yayınlamak yeterlidir. Lazer nişangahı da bu mesaja abone olabilir ve gelen tüm mesajları işleyerek `attacker` ile lazer nişangahının sahibini karşılaştırabilir ve vuruşun `attacker` tarafından yapılıp yapılmadığını kontrol edebilir ve farklı bir renkle yanıp sönebilir. Kodda bu şöyle görünür:

```rust,no_run
{{#include ../code/snippets/src/scripting/mod.rs:message_passing}}
```

Birkaç önemli nokta vardır:



- Komut dosyası örneğini bir mesaj türüne açıkça abone olmalısınız, aksi takdirde bu türdeki mesajlar komut dosyanıza iletilmez. Bu, mesaj dağıtıcı kullanılarak yapılır: `ctx.message_dispatcher.subscribe_to::<Message>(ctx.handle);`.  Bu, `on_start` yönteminde yapılmalıdır, ancak çalışma zamanında abone olma/abonelikten çıkma da mümkündür.

- Mesajlara yalnızca özel `on_message` yönteminde tepki verebilirsiniz - burada sadece
desen eşleştirme kullanarak mesaj türünü kontrol etmeniz ve yararlı bir işlem yapmanız gerekir.



Tüm durumlarda mesaj aktarımını kullanmaya çalışın, gevşek bağlama kod kalitesini ve okunabilirliği önemli ölçüde artırır, ancak

basit projelerde tamamen göz ardı edilebilir.

## Diğer scriptlerin (Komut Dosyalarının) Verilerine Erişmek

Her komut dosyası bir sahne düğümünde “yaşar”, bu nedenle başka bir komut dosyasından bir komut dosyası verisine erişmek için önce o komut dosyasının bulunduğu sahne düğümünün tanıtıcısını bilmeniz gerekir. Bunu şu şekilde yapabilirsiniz:

```rust,no_run
{{#include ../code/snippets/src/scripting/mod.rs:access_other_1}}
{{#include ../code/snippets/src/scripting/mod.rs:access_other_2}}
```

Bu örnekte iki komut dosyası türü vardır: `MyScript` ve `MyOtherScript`. Şimdi iki sahne düğümü olduğunu düşünün, ilki `MyScript`, ikincisi `MyOtherScript` içerir. `MyScript`,
`second_node` alanında bir tanıtıcı saklayarak ikinci düğümü bilir. `MyScript`, `MyOtherScript`
iç sayacını `60.0`'a sayana kadar bekler ve ardından günlüğe bir mesaj yazdırır. Bu kod, değiştirilemez ödünç alma işlemi yapar ve 
diğer komut dosyalarının verilerini değiştirmenize izin vermez. Değiştirilebilir erişim istiyorsanız, `try_get_script_of_mut`
yöntemini (veya alternatif kod için `try_get_script_mut` yöntemini) kullanın.


`MyScript`'in `second_node` alanı genellikle düzenleyicide atanır, ancak aşağıdaki kodu kullanarak sahnenizde de bu düğümü bulabilirsiniz:

```rust,no_run
{{#include ../code/snippets/src/scripting/mod.rs:find_node}}
```

Bu kod, `SomeName` adlı bir düğümü arar ve onun tanıtıcısını komut dosyasında `second_node` değişkenine atar.

## Scripttler'den (Komut Dosyaları'ndan) Eklentilere Erişmek

Bazen komut dosyalarından eklenti verilerine erişmek gerekebilir, bunun çeşitli nedenleri olabilir, örneğin botları listesine bir bot kaydetmeniz gerekebilir. Bu liste daha sonra AI'nın her karede tüm sahne grafiğinde arama yapmadan hedefleri aramak için kullanılabilir.


Eklentilere komut dosyalarından erişmek çok kolaydır, tek yapmanız gereken `ctx.plugins`'den `get/get_mut` yöntemini çağırmaktır:

```rust,no_run
{{#include ../code/snippets/src/scripting/mod.rs:access_plugin}}
```

Bu örnekte Bot komut dosyası, başlangıçta global bot listesine kendini kaydeder ve yok edildiğinde kaydı silinir.

 `update` komutu, bu listede hedefleri aramak için kullanılır. 



Çok oyunculu oyunlarda, eklenti sunucu/istemci örneklerini depolayabilir ve komut dosyaları bunlara kolayca erişerek diğer oyunculara ağ üzerinden mesaj gönderebilir. Genel olarak, eklentileri komut dosyalarınız için isteğe bağlı, global bir veri depolama alanı olarak kullanabilirsiniz.