# Rectangle düğümü



Rectangle, en basit “2D” düğümdür ve “2D” grafikler oluşturmak için kullanılabilir. 2D burada tırnak içinde yazılmıştır çünkü düğüm,

 motorun diğer tüm öğeleri gibi aslında bir 3D düğümdür. Aşağıda, rectangle düğümleri ve 

ortografik kamera kullanılarak oluşturulmuş bir örnek sahne gösterilmektedir:

![2d scene](2d_scene.PNG)

Gördüğünüz gibi, 2D oyunlar için iyi bir temel oluşturmaktadır.

## Nasıl oluşturulur

Rectangle düğümleri oluşturmak için RectangleBuilder'ı kullanın:

```rust,no_run
{{#include ../code/snippets/src/scene/rectangle.rs:create_rect}}
```

## Render için görüntü kısmını belirleme




Varsayılan olarak, Rectangle düğümü render için görüntünün tamamını kullanır, ancak bazı uygulamalar için bu yeterli değildir. Örneğin,



2D varlıklarınızı canlandırmak için sprite sayfalarını kullanmak isteyebilirsiniz. Bu durumda, görüntünün sadece bir kısmını

kullanabilmeniz gerekir. Bunu, Rectangle düğmesinin `set_uv_rect` yöntemini kullanarak yapabilirsiniz. Aşağıda, bir Rectangle düğmesi tarafından kullanılacak görüntünün sağ üst çeyreğini ayarlama örneği verilmiştir:

```rust,no_run
{{#include ../code/snippets/src/scene/rectangle.rs:set_2nd_quarter_image_portion}}
```

UV rectangle'ın her bir parçası orantılıdır. Örneğin 0.5, %50, 1.5 = %150 vb. anlamına gelir. Genişlik

veya yükseklik 1.0'ı aşarsa ve kullanılan doku ilgili eksende Wrapping moduna ayarlanmışsa, görüntü eksenler boyunca döşenecektir.

## Animasyon

See [Sprite Animation](../animation/spritesheet/spritesheet.md) chapter for more info.

## Performans



Dikdörtgenler, aynı anda çok sayıda dikdörtgeni işlemek için büyük ölçüde optimize edilmiş özel bir işleyici kullanır, böylece 

dikdörtgenleri 2D oyunlarda hemen hemen her şey için kullanabilirsiniz.