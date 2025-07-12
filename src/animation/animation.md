# Animasyon



Animasyon, bir dizi anahtar kare kullanarak sahne düğümlerinin özelliklerini çalışma zamanında değiştirmenize olanak tanır. Animasyon,
 her biri bir sahne düğümünün bir özelliğine bağlı olan birden çok parçadan oluşur. Bir parça, sayılardan (`bool` dahil) başlayıp 2/3/4 boyutlu vektörlerle biten
herhangi bir sayısal özelliği canlandırabilir.
Her bileşen (sayı, x/y/z/w vektör bileşenleri) bir _parametrik eğri_ içinde saklanır. Her parametrik eğri sıfır veya daha fazla _anahtar kare_ içerir.

Grafiksel olarak bu şekilde gösterilebilir:

```text
                                         Timeline
                                            v
  Time   > |---------------|------------------------------------>
           |               |
  Track1 > | node.position |                                     
           |   X curve     |..1..........5...........10..........
           |   Y curve     |..2.........-2..................1....  < Curve key frames
           |   Z curve     |..1..........9......................4
           |_______________|  
  Track2   | node.property |                                  
           | ............  |.....................................
           | ............  |.....................................
           | ............  |.....................................
```

Her anahtar kare, enterpolasyon moduna sahip bir gerçek sayıdır. Enterpolasyon modu, motora anahtar kareler arasındaki ara değerleri nasıl hesaplayacağını söyler.

 Animasyonlarda üç tür enterpolasyon kullanılır

(isterseniz “sıkıcı matematik” kısmını atlayabilirsiniz):



- **Constant** - ara değer, ikisinin en soldaki değeri kullanılarak hesaplanır. Sabit “interpolasyon”

 genellikle adım adım davranış oluşturmak için kullanılır, en yaygın durum iki boolean değerini “interpolasyon” yapmaktır.

- **Linear** - ara değer, doğrusal interpolasyon `i = left + (right - left) / t` kullanılarak hesaplanır,

  
burada `t = (time_position - left) / (right - left)`. `t` her zaman `0..1` aralığındadır. Doğrusal interpolasyon genellikle

  iki değer arasında “düz” geçişler oluşturmak için kullanılır.

- **Kübik** - ara değer Hermite kübik spline kullanılarak hesaplanır:
  `i = (2t^3 - 3t^2 + 1) * left + (t^3 - 2t^2 + t) * left_tangent + (-2t^3 + 3t^2) * right + (t^3 - t^2) * right_tangent`,
  
burada `t = (time_position - left) / (right - left)` (`t` is always in `0..1` range), `left_tangent` and `right_tangent`

  genellikle `tan(angle)`'dir. Kübik enterpolasyon genellikle iki değer arasında “pürüzsüz” geçişler oluşturmak için kullanılır.

## Web Demosu



Bu [web demosunda](https://fyrox.rs/assets/demo/animation/index.html) animasyon sisteminin özelliklerini keşfedebilirsiniz.

Bu demo PC'lerde çalışmak üzere tasarlanmıştır ve mobil cihazlarda test edilmemiştir.

## Parça bağlama



Her parça, adıyla veya özel bir bağlama ile bir düğümdeki bir özelliğe her zaman bağlanır. Ad, yansıma kullanarak özelliği almak için kullanılır,
 özel bağlama ise yerleşik özellikleri almak için daha hızlı bir yoldur. Genellikle konum, ölçek ve döndürme animasyonları için kullanılır
(bunlar her sahne düğümünde bulunan en yaygın özelliklerdir).



## Zaman dilimi ve döngü

Eğrilerdeki anahtar kareler zaman içinde rastgele konumlara yerleştirilebilir, ancak animasyonlar genellikle belirli bir zaman diliminde oynatılır.
Varsayılan olarak, her animasyon belirli bir zaman diliminde sonsuza kadar oynatılır. Buna _animasyon döngüsü_ denir ve her iki
oynatma yönünde de çalışır.



## Hız

Oynatma hızını geniş bir aralıkta değiştirebilirsiniz, varsayılan olarak her animasyonun oynatma hızı çarpanı 1,0 olarak ayarlanmıştır. Çarpan,
 animasyonun ne kadar daha hızlı (>1) veya daha yavaş (<1) oynatılması gerektiğini belirtir. Negatif hız çarpanı değerleri oynatmayı tersine çevirir.



## Animasyonları etkinleştirme veya devre dışı bırakma

Bazen bir animasyonu devre dışı bırakmak/etkinleştirmek veya etkin olup olmadığını kontrol etmek gerekebilir. Bunu, ilgili yöntemler olan

`Animation::set_enabled` ve `Animation::is_enabled` çiftini kullanarak yapabilirsiniz.

## Sinyaller

Sinyal, animasyon zaman çizelgesinde belirli bir zaman konumunda bulunan adlandırılmış bir işaretçidir. Sinyal, animasyonun oynatma
zamanı sinyalin konumunu soldan sağa (veya oynatma yönüne bağlı olarak tersi) geçerse bir olay yayar. Sinyaller genellikle


belirli eylemleri zaman içindeki bir konuma eklemek için kullanılır. Örneğin, bir yürüme animasyonunuz var ve karakterin ayakları yere değdiğinde
sesleri yaymak istiyorsunuz. Bu durumda, her ayağın yere değdiği anlara birkaç sinyal eklemeniz gerekir.

Bundan sonra tek yapmanız gereken, animasyon olaylarını tek tek almak ve ilgili sesleri yaymaktır. Daha fazla bilgi için ilgili 
[bölüme](signal.md) bakın.

## Koddan Oluşturma



Genellikle animasyonlar editörden veya bazı harici araçlardan oluşturulur ve ardından motora içe aktarılır. Aşağıdaki örneği denemeden önce,

 `AnimationPlayer` düğümüyle ilgili belgeleri okuyun, bu diğer düğümleri canlandırmak için çok daha uygun bir yoldur.

 Düğüm editörden oluşturulabilir ve herhangi bir kod yazmanıza bile gerek yoktur.

Prosedürel animasyonlar oluşturmanız gerekiyorsa **sadece** aşağıdaki örnek kodu kılavuz olarak kullanın:

```rust,no_run
{{#include ../code/snippets/src/animation/mod.rs:create_animation}}
```

Yukarıdaki kod, bir düğümü X ekseni boyunca çeşitli şekillerde hareket ettiren basit bir animasyon oluşturur. Animasyonun

kullanımı, örneği tamamlamak amacıyla yapılmıştır. Gerçek oyunlarda, animasyonu bir animasyon

oynatıcı sahne düğümüne eklemeniz gerekir; bu, işi sizin için yapacaktır.



## İçe Aktarma



Dış kaynaklardan (FBX dosyaları gibi) animasyon içe aktarmak da mümkündür. Bunu iki ana

yol ile yapabilirsiniz: koddan veya düzenleyiciden. Aşağıdaki bölümlerde her iki yolun nasıl kullanıldığı gösterilmektedir.

### Editörden



İlk olarak, 3D modelinizin sahnede örneklendiğinden emin olun. Aşağıdaki örnekte sahnede `agent.fbx`

örneği içerir (bunu yapmak için, 3D modelinizi Varlık Tarayıcıdan sahneye sürükleyip bırakın). Bir animasyonu içe aktarmak için 

bir `Animation Player` sahne düğümü oluşturmanız, [Animation Editor](anim_editor.md) penceresini açmanız ve

aşağı ok simgeli düğmeyi tıklamanız gerekir:

![Step 1](import_animation_1.png)

Şimdi, animasyonunuzu içe aktaracağınız 3D modelinizin kök düğümünü seçmeniz gerekir. Genellikle bu düğüm,

3D modelinizle aynı ada sahiptir (aşağıdaki ekran görüntüsünde `agent.fbx`):

![Step 2](import_animation_2.png)

Son olarak, içe aktarmak istediğiniz animasyonu seçmeniz gerekir:

![Step 3](import_animation_3.png)

Her şey doğruysa, `Önizleme` onay kutusunu tıklayarak animasyonunuzu önizleyebilirsiniz:

![Step 4](import_animation_4.png)

### Koddan



Önceki bölümde yaptığınızın aynısını koddan da yapabilirsiniz:

```rust,no_run
{{#include ../code/snippets/src/animation/mod.rs:create_animated_character}}
```

Gördüğünüz gibi, bu kod ilk olarak bir 3D model örneği oluşturur. Ardından bir animasyon yükler ve

animasyon oynatıcıda örneğini oluşturur. Bu kodun, bir yürütücü tarafından çalıştırılması gereken bir gelecek üreten `async` kullandığını lütfen unutmayın.

 Bunu çağrı yerinde yürütmek için `block_on` yöntemini kullanabilirsiniz (bu, WebAssembly'de çalışmaz).
Tüm bu sıkıcı kodu gizlediği ve tüm platformlarda eşzamansız yüklemeyi düzgün bir şekilde işlediği için, kodlama yaklaşımını tercih etmeniz önerilir

.

## Animasyon Oynatma



Animasyonlar, ilgili animasyon oynatıcısının `Auto Apply` özelliği

`true` olarak ayarlanmışsa otomatik olarak oynatılır. Animasyon oynatıcı birden fazla animasyon içerebileceğinden, tüm animasyonlar aynı anda oynatılır. 

Animasyonları gerektiğinde etkinleştirebilir/devre dışı bırakabilirsiniz. Bunun için kodda animasyonların adını bulun ve `Enabled` özelliğini değiştirin:

```rust,no_run
{{#include ../code/snippets/src/animation/mod.rs:enable_animation}}
```

Bu kod, çalışma zamanında animasyon özelliklerini değiştirmek için de kullanılabilir. Bunu yapmak için, `set_enabled` ifadesini

`set_speed`, `set_loop`, `set_root_motion_settings` vb. gibi başka yöntemlerle değiştirin.