# Çarpışan düğüm



Çarpışan, çarpışma algılama, temas manifold oluşturma vb. için kullanılan geometrik bir şekildir. Çarpışanlar

rigid body ile birlikte kullanılır ve rigid body'nin çarpışmalara katılmasını sağlar.
**Önemli:** Çarpışanlar sadece rigid body ile birlikte çalışır! Çarpışanlar, bir rigid body'nin doğrudan çocukları olmadıkça motor tarafından kullanılmaz.

Daha fazla bilgi için [bu bölümü](./rigid_body.md#colliders) okuyun.

## Şekiller



Collider hemen hemen her şekle sahip olabilir, motor 3D için aşağıdaki şekilleri sunar:



- Top - dinamik küre şekli.

- Silindir - dinamik silindir şekli.

- Koni - dinamik koni şekli.

- Küboid - dinamik kutu şekli.

- Kapsül - dinamik kapsül şekli.

- Segment - dinamik segment (“çizgi”) şekli

- Üçgen - basit dinamik üçgen şekli

- Üçgen ağ - statik içbükey şekil, herhangi bir statik seviye geometrisiyle (duvar, zemin, tavan,

diğer her şey)

- Yükseklik alanı - statik yükseklik alanı şekli, arazilerle birlikte kullanılabilir.

- Çok yüzlü - dinamik içbükey şekil.



Ayrıca, 2D için benzer ancak daha küçük bir set vardır (çünkü bazı şekiller 2D'de bozulur):



- Top - dinamik daire şekli.

- Küboid - dinamik rectangle şekli.

- Kapsül - dinamik kapsül şekli.

- Segment - dinamik segment (“çizgi”) şekli.

- Üçgen - dinamik üçgen şekli.

- Trimesh - statik üçgen ağ şekli.

- Heightfield - statik yükseklik alanı şekli.



Her iki listede de _Dynamic_ , bu tür şekillerin _dynamic_ rigid bodies ile birlikte kullanılabileceği, tüm çarpışmaları doğru şekilde işleyeceği ve simülasyonun olması gerektiği gibi görüneceği anlamına gelir. _Static_ , bu tür şekillerin yalnızca _static_

rigid bodies ile birlikte kullanılması gerektiği anlamına gelir.



## Nasıl oluşturulur



ColliderBuilder'ı kullanarak koddan istediğiniz şekle sahip bir çarpışıcı örneği oluşturun.

```rust,no_run
{{#include ../code/snippets/src/scene/collider.rs:create_capsule_collider}}
```

Editörde `MainMenu -> Create -> Physics -> Collider` komutunu kullanabilir veya `World Viewer` içinde bir düğüme sağ tıklayıp

`Add Child -> Physics -> Collider` seçeneğini seçebilirsiniz. Çarpışan nesneler, rigid body nesnelerinin doğrudan alt öğeleri olmalıdır, çarpışan nesneler tek başlarına hiçbir şey yapmazlar

!

## Collision filtreleme



Bazen çeşitli çarpışan gruplar arasında çarpışmayı önlemek gerekir. Fyrox, tam da bu amaçla bit bazında çarpışma 

filtrelemeyi destekler. Örneğin, iki çarpışan grubunuz olabilir: aktörler ve güçlendiriciler, ve

aktörlerin güçlendiricilerle çarpışmaları tamamen yok saymasını (ve tersi) istiyorsunuz. Bu durumda, aktörler için çarpışma

gruplarını şu şekilde ayarlayabilirsiniz:

![actors collision groups](./collision_groups_a.png)

Ve güçlendiriciler için çarpışma gruplarını şu şekilde ayarlayın:

![powerups collision groups](./collision_groups_b.png)

Gördüğünüz gibi, aktörler ve güçlendiriciler artık ayrı `üyelikler` (okuma - gruplar) ve filtrelere sahiptir. Bu şekilde, aktörler

her şeyle çarpışacak, ancak güçlendiricilerle çarpışmayacak ve bunun tersi de geçerli olacaktır.

## Hit kutuları için çarpışmaların kullanılması



Çarpışmaları kullanarak oyun karakterleriniz için hit kutularını simüle edebilirsiniz. Bu,

`KinematicPositionBased` türüne sahip bir rigid body ve uygun bir çarpışma nesnesi oluşturarak yapılabilir. Son adım olarak, bu nesneyi karakterinizin modelindeki bir kemiğe eklemeniz gerekir.

İşte editörden hızlı bir örnek:

![hitbox](./hitbox.png)

Gördüğünüz gibi, rigid body bir kapsül çarpıştırıcıya sahiptir ve gövde boyun kemiğine bağlıdır. Gövde

KinematicPositionBased tipindedir, bu da gövdenin simüle edilmeyeceğini, bunun yerine konumunun 

ana kemiğin konumu ile senkronize edileceğini garanti eder.



Oyununuzda vuruş kutularını gerçekten kullanmak için, bir vuruş taraması gerçekleştirmek üzere ışın izleme kullanabilir veya 

temas bilgilerini kullanarak vuruş kutusunun temas ettiği öğeleri alabilirsiniz. Bkz. bölümün [Işın izleme](./ray.md) bölümü.