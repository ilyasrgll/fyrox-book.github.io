# Root Motion

Kök hareketi, hiyerarşideki bir düğümden fiziksel bir kapsüle hareketi aktaran özel bir tekniktir.

 Bu kapsül daha sonra gerçek hareketi gerçekleştirmek için kullanılır. İşlem sırasında şöyle görünür:

<iframe width="750" height="410" src="https://youtube.com/embed/0lG8Spzk128" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Videonun ilk bölümünde görebileceğiniz gibi, karakterin hareketi daha çok yerin üzerinde süzülüyormuş gibi görünüyor.
 Bunun nedeni, fiziksel kapsülün gerçek hareketinin karakterin hareketiyle senkronize olmamasıdır .
 Kök hareketi, animasyon hiyerarşisinin bazı kök düğümlerinin hareketini (bu karakterde kalçalar) alıp fiziksel kapsüle aktararak bu sorunu tam olarak giderir
. Bu,  gerçek hareketin animasyonda “pişirilen” hareketle tamamen senkronize olmasını sağlar.



Kök hareketi ayrıca bazı güzel efektlere sahiptir - karakterinizi yalnızca animasyondaki hareketle hareket ettirebilirsiniz vebu, %99 oranında mükemmel şekilde çalışır. Animasyonlar ayrıca, fiziksel kapsüle çıkarılabilen ve uygulanabilen bazı dönüşler içerebilir. Bir sonraki harika özellik, karakterinizin asla fiziksel kapsülünün dışına çıkmamasıdır. Bu, büyük hareketler içeren animasyonları oynatırken karakterin duvarlara girmesini önler.



Genel olarak, mümkün olduğunda karakterleriniz için kök hareketle yönlendirilen hareketi tercih etmelisiniz. Bunun nedeni, karakter hareketleriyle ilgili birçok yaygın sorunu ortadan kaldırmasıdır. Ayrıca 2D dünyaya da uygulanabilir ve aynı şekilde çalışır. 

## Nasıl etkinleştirilir

Animasyon düzenleyicide `RM` düğmesine tıklayarak açılan açılır menüden etkinleştirebilir/devre dışı bırakabilir/ayar yapabilirsiniz.

Kök hareketinin _her animasyon için_ ayrı ayrı yapılandırılması gerektiğini unutmayın. Çoğu animasyon

kök harekete hiç ihtiyaç duymaz.

![root motion](../ae_rm.png)

Burada en önemli kısım `Root` tutamağıdır, bu, animasyonunuzla hareket eden bir kök düğüme ayarlanmalıdır, genellikle
“hips” veya benzeri bir isimle adlandırılır:

![root node](../ae_root_node.png)

Bundan sonra, eksenler için filtreler uygulamanız gerekir - hareket animasyonlarının çoğu oXZ düzleminde “çalışır”, bu nedenle Y ekseni
yoksayılmalıdır. Ayrıca, animasyonunuzda herhangi bir dönüş yoksa, dönme kısmını da filtreleyebilirsiniz.
Alternatif olarak, aynı işlemi koddan da yapabilirsiniz:

```rust,no_run
{{#include ../../code/snippets/src/animation/root_motion.rs:setup_root_motion}}
```

Bu kod, yukarıdaki ekran görüntülerindeki düzenleyiciyle hemen hemen aynı işlevi görür. Bu işlevin argümanları

şunlardır:

- `animation_player` - tüm animasyonlarınızın depolandığı animasyon oynatıcısına bir tanıtıcı,

- `animation` - kök hareketi etkinleştirmek istediğiniz animasyonun tanıtıcısı (tanıtıcıyı
[AnimationContainer::find_by_name_ref](https://docs.rs/fyrox/latest/fyrox/animation/struct.AnimationContainer.html#method.find_by_name_ref) 
yöntemini kullanarak elde edebilirsiniz).

- `root_node` - karakterinizin hiyerarşisinin kök düğümüne bir tanıtıcı, genellikle “Hips”
veya “Pelvis” gibi bir adla adlandırılır.

- `ctx` - mevcut komut dosyanızın komut dosyası bağlamı.

## Nasıl kullanılır

Animasyonlardan çıkarılan doğrudan kök hareket değerleri tek başlarına pek bir işe yaramaz ve %99 oranında
karakterinizi canlandıran bir [durum makinesi](../blending.md) 'den ortalama kök hareket değerlerini almanız gerekir. Bunun
nedeni, animasyon karıştırma durum makinesinin tüm aktif animasyon kaynaklarından gelen kök hareketleri düzgün bir şekilde karıştırmasıdır. 
Genel olarak şöyle görünebilir:



```rust ,no_run

{{#include ../../code/snippets/src/animation/root_motion.rs:fetch_and_apply_root_motion}}

```



Bu kod, geçerli kare için geçerli **yerel uzay ofsetini** çıkarır ve ardından ofseti
dünya uzay koordinatlarına dönüştürür. Son olarak, ofseti geçerli delta zamanı (`1.0 / ctx.dt`) kadar azaltarak
yeni hız vektörünü elde eder ve bu vektör daha sonra rijit gövdeye (oyuncunun kapsülü) uygulanır.



Bu işlevdeki argümanlar şunlardır: 



- `absm` Animation Blending State Machine düğümünün bir örneğine bir tanıtıcı

- `rigid_body` karakteriniz tarafından kullanılan rigid body'ye bir tanıtıcı

- `model` - karakterinizin 3D modelinin kök düğümüne bir tanıtıcı.

### Ham kök hareket değerleri



Herhangi bir nedenle animasyonlardan ham kök hareket değerlerine ihtiyacınız varsa, bunları[Animation::root_motion](https://docs.rs/fyrox/latest/fyrox/animation/struct.Animation.html#method.root_motion)yöntemini kullanarak doğrudan



## Kök hareketi ile prosedürel hareketi birleştirme



Bazen kök hareketi ile bazı prosedürel hareketleri birleştirmeniz gerekebilir (örneğin, zıpladıktan sonra oluşan atalet).
Bu, iki hız vektörü ekleyerek oldukça kolay bir şekilde yapılabilir - biri kök hareketinden, diğeri
prosedürel hareketten.