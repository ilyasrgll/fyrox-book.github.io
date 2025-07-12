# Sprite



Sprite, her zaman kameraya bakan bir dörtgen ağdır. Boyutu, rengi, “bakış” ekseni etrafındaki dönüşü ve bir dokusu vardır.

Sprite'lar çoğunlukla parlayan plazma gibi mermiler ve her zaman kameraya bakması gereken nesneler için kullanışlıdır.

> ⚠️ Sprite'ların 2D oyunlarda kullanılmak üzere tasarlanmadığını, yalnızca 3D oyunlarda kullanılabileceğini unutmayın.
> 2D sprite'lara ihtiyacınız varsa [Rectangle node](./rectangle.md) kullanın.

## Nasıl oluşturulur

Bir sprite örneği `SpriteBuilder` kullanılarak oluşturulabilir:

```rust,no_run
{{#include ../code/snippets/src/scene/sprite.rs:create_sprite}}
```

Yapıcıda `.with_material` yöntemi kullanılarak dokulu bir sprite oluşturulabilir:

```rust,no_run
{{#include ../code/snippets/src/scene/sprite.rs:create_sprite_with_texture}}
```

Bu kodun her sprite için bir malzeme oluşturduğunu lütfen unutmayın. Aynı anda çok sayıda sprite kullanıyorsanız, bu çok verimsiz olabilir.

 Mümkünse, aynı malzeme kaynağını birden fazla sprite arasında paylaşın. Aksi takdirde, her sprite

ayrı bir çizim çağrısında işlenecek ve genel performans çok düşük olacaktır.

## Animasyon



Daha fazla bilgi için [Sprite Animasyonu](../animation/spritesheet/spritesheet.md) bölümüne bakın.

## Genel kurallar



Sprite'lar **çok sayıda parçacık içeren** görsel efektler oluşturmak için **kullanılmamalıdır**. Bunun için 


[parçacık sistemleri](particle_system_node.md) kullanmalısınız. Neden? Parçacık sistemleri,

aynı anda çok büyük miktarda parçacığı yönetmek için çok iyi optimize edilmiştir, ancak spriteler değildir. Her sprite, 

parçacık sistemlerinde parçacık olarak kullanılmak için oldukça ağırdır ve çok fazla bellek tüketen birçok “yararsız” bilgi içerir.