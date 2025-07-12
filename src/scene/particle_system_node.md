# Parçacık sistemi



Parçacık sistemi, karmaşık görsel efektler (VFX) oluşturmak için kullanılan bir sahne düğümüdür.

Aynı anda çok sayıda parçacık üzerinde çalışarak, çok sayıda parçacığın dahil olduğu karmaşık simülasyonlar yapmanızı sağlar. Genellikle

parçacık sistemleri aşağıdaki görsel efektleri oluşturmak için kullanılır: duman, kıvılcımlar, kan sıçramaları, buhar vb.

![smoke](./particle_system_example.png)

## Temel Kavramlar



Parçacık sistemi, sistemdeki her parçacık için _tek_ bir doku kullanır, yalnızca Kırmızı kanal kullanılır. Kırmızı kanal, tüm parçacıklar için alfa olarak yorumlanır

.



Her parçacık, parçacık sisteminin `Acceleration` parametrelerinden etkilenir. Bu parametre, her parçacığın hızını etkileyecek ivmeyi

(m/s<sup>2</sup> cinsinden) tanımlar. Yerçekimini simüle etmek için kullanılır.

### Parçacık



Parçacık, her zaman kameraya bakan bir dokuya sahip bir kare (dörtgen değil, bu önemlidir) şeklindedir.

Aşağıdaki özelliklere sahiptir:



- `Konum` - parçacık sisteminin _yerel_ koordinatlarında bir konumu tanımlar (bu, bir parçacık

sistemini döndürdüğünüzde tüm parçacıkların da döneceği anlamına gelir).

- `Hız` - partikülün yerel konumunu değiştirmek için kullanılacak hız vektörünü (yerel koordinatlarda) tanımlar

her kare.

- `Boyut` - parçacığın kare şeklinin boyutu (metre cinsinden).

- `Boyut Değiştirici` - her karede Boyut'a eklenecek sayısal bir değer (metre/saniye cinsinden), parçacıkların boyutunu değiştirmek için kullanılır

.

- `Ömür` - parçacığın aktif olabileceği süre (saniye cinsinden).

- `Rotation` - parçacığın kamera ekseni etrafında dönüşünü tanımlayan açı (radyan cinsinden) (saat yönünde).

- `Rotation Speed` - parçacığın dönüş hızı (saniye başına radyan cinsinden, rad/s).

- `Color` - parçacığın RGBA rengi.

### Yayıcılar

Parçacık sistemi, parçacıkların ortaya çıkacağı bir dizi bölgeyi tanımlamak için _yayıcılar_ kullanır ve ayrıca parçacıkların başlangıç aralıklarını da tanımlar.

Parçacık sistemi, parçacıklar oluşturmak için en az bir yayıcıya sahip olmalıdır.



Yayıcı, aşağıdaki türlerden biri olabilir:

- `Cuboid` - parçacıkları küboid şeklinde eşit olarak yayar, şekil döndürülemez, sadece kaydırılabilir.

- `Sphere` - parçacıkları küre şeklinde eşit olarak yayar.

- `Cylinder` - parçacıkları silindir şeklinde eşit olarak yayar, şekil döndürülemez, sadece kaydırılabilir.



Her yayıcı, oluşturulan her parçacığın _başlangıç_ değerlerini etkileyen sabit bir parametre kümesine sahiptir:



- `Position` - yayıcı kendi _yerel_ konumuna sahiptir (ana parçacık sistemi düğümüne göre konum), bu, uzayda birden fazla bölgeden aynı anda parçacıklar oluşturabilen karmaşık parçacık sistemleri oluşturmanıza yardımcı olur.


- `Max Particles` - oluşturulabilecek maksimum parçacık sayısı.
 Varsayılan olarak, bu değer `None`'dur, yani

sınır yoktur.

- `Spawn Rate` - hız (saniye başına birim cinsinden), yayıcının parçacıkları ne kadar hızlı üreteceğini belirler.

- `Lifetime Range` - parçacık ömrü değerleri için sayısal aralık (saniye cinsinden). Aralığın başlangıcı ne kadar düşükse,

 üretilen parçacıkların ömrü o kadar kısa olur ve tersi de geçerlidir.

- `Boyut Aralığı` - parçacık boyutu için sayısal aralık (metre cinsinden).

- `Boyut Değiştirici Aralığı` - parçacık boyutu değiştirici parametresi için sayısal aralık (saniye başına metre, m/s).

- `X/Y/Z Hız Aralığı` - ilgili hız ekseni (X, Y, Z) için sayısal aralık (saniye başına metre, m/s)

ekseni boyunca başlangıç hızını tanımlar.

- `Rotasyon Aralığı` - yeni bir parçacığın başlangıç rotasyonu için sayısal aralık (radyan cinsinden).

- `Rotasyon Hızı Aralığı` - yeni bir parçacığın rotasyon hızı için sayısal aralık (saniye başına radyan cinsinden, rad/s).

**Önemli:** Her aralık (Ömür Aralığı, Boyut Aralığı vb.) parametresi, bir parçacığın ilgili

parametresi için _rastgele_ bir değer üretir. Üretilen değerlerin her seferinde farklı olmasını sağlamak için mevcut rastgele sayı üretecinin tohumunu (`fyrox::core::thread_rng()`)

değiştirebilirsiniz.

## Nasıl oluşturulur

Parçacık sistemi oluşturmanın birden fazla yolu vardır, mevcut ihtiyaçlarınıza en uygun olanı seçin.

### Editör kullanarak

Parçacık sistemi oluşturmanın en iyi yolu, editörde yapılandırmaktır. Koddan oluşturmak da mümkündür (aşağıya bakın),
ancak çok sayıda parametre olduğu için çok daha zordur ve sezgisel olmayabilir. Editör, sonucu görmenizi
ve çok hızlı bir şekilde ayarlamanızı sağlar. `create -> Particle System` ile bir parçacık sistemi oluşturun ve ardından
özelliklerini düzenlemeye başlayabilirsiniz. Varsayılan olarak, yeni parçacık sistemi bir Sphere parçacık yayıcıya sahiptir, Inspector'da `Emitters` özelliğinin sağındaki `+`
düğmesine tıklayarak yeni yayıcılar ekleyebilirsiniz (veya `-` düğmesine tıklayarak kaldırabilirsiniz). İşte basit bir örnek:

![particle system](./particle_system.png)

Şimdi istediğiniz parametreleri ayarlamaya başlayın. Belirli bir efekti nasıl elde edeceğinize dair herhangi bir öneride bulunmak zordur,
burada sadece pratik yapmak önemlidir.

### Kodu kullanma



Koddan da parçacık sistemleri oluşturabilirsiniz (prosedürel olarak oluşturulmuş efektlere ihtiyacınız varsa):

```rust,no_run
{{#include ../code/snippets/src/scene/particle_system.rs:create_smoke}}
```

Bu kod, yumuşak bir şekilde kaybolan duman efekti oluşturur (renk ömrü boyunca gradyan kullanarak). Daha fazla bilgi için

[API belgeleri](https://docs.rs/fyrox/latest/fyrox/scene/particle_system/index.html) partikül sistemi bölümüne bakın.

### Prefab kullanma



Editörde oluşturulmuş parçacık sistemleri oluşturmanız gerekiyorsa, her zaman prefabları kullanabilirsiniz. İstediğiniz

parçacık sistemine sahip bir sahne oluşturun ve ardından sahnenize [örnekleyin](../resources/model.md#instantiation).

## Yumuşak parçacıklar



Fyrox, parçacıklar ve sahne geometrisi arasındaki keskin geçişleri yumuşatan “yumuşak parçacıklar” adlı özel bir teknik kullandı:

![soft particles](./soft_particles.png)

Bu teknik, özellikle duman, sis vb. gibi efektler için kullanışlıdır, çünkü bu efektlerde parçacıklar ile sahne geometrisi arasındaki “kenar”ın görünmesini istemezsiniz.

 Bu efekti, `Yumuşak Sınır Keskinlik Faktörü` kullanarak ayarlayabilirsiniz; değer ne kadar büyükse

kenar o kadar “keskin” olur ve tersi de geçerlidir.

## Emisyonun yeniden başlatılması

`particle_system.clear_particles()` yöntemini çağırarak parçacık sistemlerini “başlangıç” durumuna “geri alabilirsiniz”. Bu,

 oluşturulan tüm parçacıkları siler ve emisyon baştan başlar.

## Parçacık sistemlerini etkinleştirme veya devre dışı bırakma



Varsayılan olarak, tüm parçacık sistemleri etkindir. Bazen bir parçacık sistemi oluşturmak, ancak etkinleştirmemek

(örneğin, bazı gecikmeli efektler için) gerekebilir. Bunu, `particle_system.set_enabled(true/false)`

yöntemini çağırarak yapabilirsiniz. Devre dışı bırakılan parçacık sistemleri yine çizilir, ancak emisyon ve animasyon durdurulur. Parçacık

sistemini tamamen gizlemek için `particle_system.set_visibility(false)` yöntemini kullanın.



## Performans



Partikül sistemleri, çok düşük ek yükle milyonlarca partikülü çizmek için optimize edilmiş özel bir renderer kullanır, ancak partiküller CPU tarafında simüle edilir ve her birinde çok sayıda partikül bulunan çok sayıda partikül sistemi olduğunda genel performansı önemli ölçüde etkileyebilir.



## Sınırlamalar



Partikül sistemleri ışıklandırma ile etkileşime girmez, bu da partiküllerin sahnedeki ışık kaynakları tarafından aydınlatılmayacağı anlamına gelir.
