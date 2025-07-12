# Işık düğümü



Motor, çeşitli ışık kaynakları ile karmaşık bir aydınlatma sistemi sunar.



## Işık türleri



Üç ana ışık kaynağı türü vardır: yönlü, nokta ve spot ışıklar.



### Yönlü ışık



Yönlü ışığın bir konumu yoktur, ışınları her zaman paraleldir ve uzayda belirli bir yönü vardır.

Gerçek hayatta yönlü ışığa örnek olarak Güneş verilebilir. Noktasal bir ışık olsa bile,

Dünya'dan çok uzakta olduğu için ışınlarının her zaman paralel olduğunu varsayabiliriz. Yönlü ışık kaynakları dış mekan

sahneleri için uygundur.



Yönlü bir ışık kaynağı şu şekilde oluşturulabilir:

```rust,no_run
{{#include ../code/snippets/src/scene/light.rs:create_directional_light}}
```

Varsayılan olarak, ışık kaynağı “zemini” aydınlatacak şekilde yönlendirilir. Başka bir deyişle, yönü

`(0.0, -1.0, 0.0)` vektörüne bakacaktır. Oluştururken yerel dönüşümünü ayarlayarak istediğiniz gibi döndürebilirsiniz.

Şunun gibi:

```rust,no_run
{{#include ../code/snippets/src/scene/light.rs:create_oriented_directional_light}}
```

### Nokta ışık



Nokta ışık, her yöne ışık yayan bir ışık kaynağıdır, bir konumu vardır ancak yönü yoktur.

Nokta ışık kaynağı örneği: ampul.

```rust,no_run
{{#include ../code/snippets/src/scene/light.rs:create_point_light}}
```

### Spot ışığı



Spot ışığı, koni şeklinde ışık yayan bir ışık kaynağıdır, bir konumu ve yönü vardır.

Spot ışığı kaynağına bir örnek: el feneri.

```rust,no_run
{{#include ../code/snippets/src/scene/light.rs:create_spot_light}}
```

## Işık saçılması

![scattering](scattering.png)

Spot ve nokta ışıklar ışık saçılma efektini destekler. Sisli bir havada el fenerinizle yürüdüğünüzü hayal edin,

sis el fenerinizden gelen ışığı saçarak “ışık hacmi”ni görmenizi sağlar. Işık saçılma

**varsayılan olarak etkindir**, bu yüzden etkinleştirmek için hiçbir şey yapmanız gerekmez. Ancak, bazı durumlarda devre dışı bırakmak isteyebilirsiniz

. Bunu, bir ışık kaynağı oluştururken veya mevcut ışık kaynağındaki ışık saçılma seçeneklerini değiştirerek yapabilirsiniz.

Bunu nasıl yapacağınızla ilgili küçük bir örnek aşağıda verilmiştir.

```rust,no_run
{{#include ../code/snippets/src/scene/light.rs:disable_light_scatter}}
```

Ayrıca, her renk kanalı için saçılma miktarını değiştirebilirsiniz. Bunu kullanarak

[Rayleigh saçılması](https://en.wikipedia.org/wiki/Rayleigh_scattering)

```rust,no_run
{{#include ../code/snippets/src/scene/light.rs:use_rayleigh_scattering}}
```

## Gölgeler

Varsayılan olarak, ışık kaynakları gölgeler oluşturur. Bunu, bir ışık kaynağının `set_cast_shadows` yöntemini kullanarak değiştirebilirsiniz.

Gölgeleri dikkatli bir şekilde yönetmelisiniz: Gölgeler performansı en çok etkileyen unsurlardır, performansı iyi seviyede tutmak için gölge oluşturabilen ışık kaynaklarının sayısını mümkün olduğunca düşük tutmalısınız.

Ayrıca, gerektiğinde gölgeleri açıp kapatabilirsiniz:

```rust,no_run
{{#include ../code/snippets/src/scene/light.rs:switch_shadows}}
```

Her ışık gölge oluşturmamalıdır, örneğin bir oyuncunun sadece uzaktan görebileceği küçük bir ışığın

gölgeleri devre dışı bırakılabilir. Sahnenize göre uygun değerleri ayarlamalısınız, sadece şunu unutmayın: gölgeler ne kadar az

olursa performans o kadar iyi olur. En pahalı gölgeler nokta ışıklardan, en ucuz gölgeler spot ışıklardan ve yönlü

ışıklardan

## Performans



Işıklar ucuz değildir, her ışık kaynağı performansı bir şekilde etkiler. Genel bir kural olarak, ışık kaynağı sayısını

makul seviyede tutmaya çalışın ve özellikle küçük bir alanda çok sayıda ışık kaynağı oluşturmaktan kaçının.

Işığın “kaplaması” gereken alan ne kadar az olursa, performans o kadar yüksek olur. Bu, çok sayıda küçük ışık kaynağını ücretsiz olarak kullanabileceğiniz anlamına gelir.
