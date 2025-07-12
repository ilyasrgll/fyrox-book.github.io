# Rigid body node



Rigid body node, motorun ana fiziksel varlıklarından biridir. Rigid body node'lar yerçekimi, 

dış kuvvetler ve diğer rigid body'lerden etkilenebilir. Nesneleriniz için doğal fiziksel davranışa ihtiyaç duyduğunuz her yerde rigid body node kullanın.

## Nasıl oluşturulur



RigidBodyBuilder'ı kullanarak bir rigid body örneği oluşturun:

```rust,no_run
{{#include ../code/snippets/src/scene/rigid_body.rs:create_cube_rigid_body}}
```

## Çarpışanlar



Rigid body, simülasyona düzgün bir şekilde katılmak için en az bir çarpışana sahip olmalıdır. Birden fazla çarpışan, basit şekillerden karmaşık şekiller oluşturmak için kullanılabilir. Bu şekilde içbükey nesneler oluşturabilirsiniz. Her çarpışan, bir rigid body'nin doğrudan alt düğümü olmalıdır.

Editörde şöyle görünebilir:

![colliders](./colliders.png)

Burada `Box` düğümü, `Rigid Body 2D`'nin bir örneğidir ve bir alt öğesi olan `Collider 2D` ile bazı sprite'lara sahiptir. Bu 


yapısı (bir rigid body'nin bir alt öğesi olarak bir collider'ı olduğunda), fizik motorunun doğru çalışması için zorunludur! Collider, bir rigid body olmadan çalışmaz (fizik simülasyonuna katılmaz) ve bir rigid body, bir collider olmadan çalışmaz.

Bu, hem 2D hem de 3D için geçerlidir.



Bir nesnenin grafiksel temsilinin (`Mesh`, `Sprite` vb. gibi bazı düğümler) bir rigid body'ye
bağlanması gerektiğini unutmayın. Aksi takdirde, rigid body hareket eder, ancak grafiksel temsil hareket etmez. 
bunu tersi şekilde de düzenleyebilirsiniz: bir grafik düğümü, bir çarpışan nesneye sahip bir rigid body'ye sahip olabilir, ancak bunun için rigid body'nin 

kinematik olması gerekir. Bu, [hit boxes](./collider.md#using-colliders-for-hit-boxes) veya fiziksel temsili olması gereken, ancak grafik düğümüyle birlikte hareket eden diğer nesneleri oluşturmak için kullanılır.


## Kuvvet ve tork



Herhangi bir rigid body'ye kuvvet ve tork uygulayabilirsiniz, ancak sadece dinamik cisimler etkilenir.

Bir rigid body'ye kuvvet uygulamak için iki yol vardır: kütle merkezinde veya cismin belirli bir noktasında:

```rust,no_run
{{#include ../code/snippets/src/scene/rigid_body.rs:apply_force_and_torque}}
```

## Kinematik rigid body



Bazen bir rigid body'nin pozisyonu/rotasyonu üzerinde doğrudan kontrol sahibi olmak ve fizik motoruna bu body için simülasyon yapmamasını söylemek isteyebilirsiniz.

Bu, rigid body'yi _kinematic_ yaparak elde edilebilir:

```rust,no_run
{{#include ../code/snippets/src/scene/rigid_body.rs:create_kinematic_rigid_body}}
```

## Sürekli çarpışma algılama



Hızlı hareket eden rigid body'ler diğer nesnelerin içinden “uçarak geçebilir” (örneğin, bir mermi çok hızlı hareket ediyorsa duvarları tamamen yok sayabilir).

 Bu, ayrık hesaplama nedeniyle olur. Bu durum, sürekli çarpışma algılama kullanılarak düzeltilebilir.

Bunu etkinleştirmek için RigidBodyBuilder'ın .with_ccd_enabled(state) veya RigidBody'nin .set_ccd_enabled(state) kullanın.

## Dominance



Dominance, rigid body'lere uygulanan kuvvetlerin önceliğini belirlemenizi sağlar. Hangi rigid body'nin hangi rigid body'yi etkileyebileceğini tanımlar

cisimlerin etkilenebileceğini tanımlar. Örneğin, aktörler için en yüksek hakimiyeti ayarlayabilir ve diğer her şeyin hakimiyetini sıfırda bırakabilirsiniz. Bu şekilde

aktörler diğer tüm dinamik cisimleri itebilir, ancak dinamik cisimler aktörleri etkilemez. Bu, aktörlerinizin çevredeki nesneler tarafından itilmesini istemediğinizde kullanışlıdır (örneğin, birisi bir aktöre bir kutu atarsa, aktörün hakimiyeti daha yüksekse kutu hareket etmez).

## 2D rigid bodies



2D rigid bodies, simülasyonun oXY düzleminde gerçekleşmesi ve Z koordinatının göz ardı edilmesi dışında 3D ile hiçbir farkı yoktur.