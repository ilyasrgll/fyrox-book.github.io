# Temel düğüm



Temel düğüm, hiyerarşik bilgileri (üst düğüme bir tanıtıcı ve alt düğümlere bir dizi tanıtıcı),
yerel ve genel dönüşümleri, adı, etiketini, ömrünü vb. depolayan bir sahne düğümüdür. Kendini tanımlayan bir adı vardır ve
diğer tüm sahne düğümleri için (kompozisyon yoluyla) temel düğüm olarak kullanılır.



Grafiksel bilgisi yoktur, bu nedenle her zaman görünmez, ancak alt düğümler için bir “konteyner” olarak kullanışlıdır
.



## Nasıl oluşturulur



Pivot düğümünün bir örneğini oluşturmak için `PivotBuilder` kullanın (`Base` düğümünün yalnızca diğer düğüm türlerini oluşturmak için kullanıldığını unutmayın

):

```rust,no_run
{{#include ../code/snippets/src/scene/base.rs:build_node}}
```

## Karmaşık bir hiyerarşi oluşturma



Bazı düğümlerin karmaşık bir hiyerarşisini oluşturmak için, `BaseBuilder`'ın `.with_children()` yöntemini kullanın. Bu yöntem,
herhangi bir karmaşıklıkta bir hiyerarşi oluşturmanıza olanak tanır:

```rust,no_run
{{#include ../code/snippets/src/scene/base.rs:build_complex_node}}
```

Bir `Camera` örneği oluştururken, ona yeni bir `BaseBuilder` örneği aktardığımızı unutmayın. Bu
örnek, bazı özellikleri ve bir dizi alt düğümü ayarlamak için de kullanılabilir.
“Akıcı sözdizimi” kullanılması zorunlu değildir, yukarıdaki kod parçacığı şu şekilde yeniden yazılabilir:

```rust,no_run
{{#include ../code/snippets/src/scene/base.rs:build_complex_node_flat}}
```

Ancak, hiyerarşik görünümü kaybettiği ve nesneler arasındaki ilişkileri anlamak daha zor olduğu için daha az bilgilendirici görünür.

## Dönüştür

Temel düğüm, düğümü istediğiniz gibi çevirmenize/ölçeklendirmenize/döndürmenize vb. olanak tanıyan yerel bir dönüştürme özelliğine sahiptir. Örneğin, bir düğümü belirli bir konuma taşımak için şunu kullanabilirsiniz:

```rust,no_run
{{#include ../code/snippets/src/scene/base.rs:translate_node}}
```

Aşağıdaki gibi birden fazla `set_x` çağrısını zincirleyebilirsiniz:

```rust,no_run
{{#include ../code/snippets/src/scene/base.rs:transform_node}}
```

Dönüşümler hakkında daha fazla bilgiyi [burada](./transform.md) bulabilirsiniz.

## Görünürlük

`Base` düğümü, yerel görünürlük ve genel görünürlük (üst zincirin görünürlüğü dahil) hakkındaki tüm bilgileri depolar.
Düğümün görünürlüğünü değiştirmek, uzak nesneleri gizleyerek performansı artırmak (ancak 
bunun için ayrıntı düzeyi kullanılması şiddetle tavsiye edilir) veya sahnedeki bazı nesneleri gizlemek için yararlı olabilir. Görünürlüğü ayarlamak veya almak için üç ana yöntem
vardır:



- `set_visibility` - bir düğümün yerel görünürlüğünü ayarlar.
- `visibility` - bir düğümün mevcut yerel görünürlüğünü döndürür.
- `global_visibility` - bir düğümün birleşik görünürlüğünü döndürür. Hiyerarşideki her ana düğümün görünürlüğünü içerir,
 bu nedenle, bazı alt düğümleri olan bir ana düğümünüz varsa ve ana düğümün görünürlüğünü `false` olarak ayarlarsanız, yerel görünürlük `true` olsa bile
alt düğümlerin genel görünürlüğü de `false` olur. Bu, çok sayıda alt düğümü olan karmaşık
nesneleri gizlemek için kullanışlı bir tekniktir.

## Sahne düğümlerini etkinleştirme/devre dışı bırakma

Bir sahne düğümü etkinleştirilebilir veya devre dışı bırakılabilir. Devre dışı bırakılan düğümler oyun döngüsünden çıkarılır ve CPU tüketimi neredeyse sıfırdır
(motorun sınırlamaları nedeniyle global dönüşüm/görünürlük/etkin durumları hala güncellenir). Bir düğümü devre dışı bırakmak,
 bazı hiyerarşileri tamamen dondurmanız ve tekrar etkinleştirilene kadar bu durumda tutmanız gerektiğinde yararlı olabilir.
Performansı artırmak için oyuncunun etkileşime giremeyeceği bir sahnenin bazı kısımlarını devre dışı bırakmak yararlı olabilir.
Etkin durumun görünürlük gibi hiyerarşik olduğunu unutmayın. Bazı alt düğümleri olan bir üst düğümü devre dışı bıraktığınızda,
alt düğümler de devre dışı bırakılır.