# Kamera düğümü

Kamera, sahnenizi herhangi bir noktadan ve herhangi bir yönde “gözlemlemenizi” sağlayan özel bir sahne düğümüdür.
Şu anda motor, frustum hacmi olarak temsil edilebilen yalnızca perspektif kameraları desteklemektedir. Frustum ile “kesişen” her şey

![Frustum](./frustum.svg)

## Nasıl oluşturulur?

Kamera düğümünün bir örneği `CameraBuilder` kullanılarak oluşturulabilir: 
```rust,no_run
{{#include ../code/snippets/src/scene/camera.rs:create_camera}}
```

Oryantasyon ve konum her zamanki gibi `BaseBuilder` içinde ayarlanmalıdır.

## Projeksiyon modları

Projeksiyon modu, sahnenizin render edildikten sonra nasıl görüneceğini belirler. İki projeksiyon modu mevcuttur.

### Perspektif

Perspektif projeksiyon, uzak nesneleri küçültür ve paralel çizgileri birleştirir. 3D oyunlar için en yaygın projeksiyon türüdür. Varsayılan olarak, her kamera perspektif projeksiyon kullanır.

Bu, frustum hacmini tanımlayan üç parametre ile tanımlanır: Bu, frustum hacmini tanımlayan üç 
parametre ile tanımlanır:

- Görüş açısı
- Yakın kırpma düzlemi konumu
- Uzak kırpma düzlemi konumu

İşte perspektif projeksiyonlu bir kamera oluşturmanın basit bir örneği:

```rust,no_run
{{#include ../code/snippets/src/scene/camera.rs:create_perspective_camera}}
```

### Ortografik

- Dikey Boyut
- Yakın Kesme Düzlemi
- Uzak Kesme Düzlemi

Dikey boyut, “kutunun” dikey eksende ne kadar büyük olacağını tanımlar, yatay boyut ise dikey boyuttan, dikey boyutu en boy oranıyla çarparak elde edilir.


Ortografik projeksiyonlu bir kamera oluşturmanın basit bir örneği aşağıda verilmiştir:

```rust,no_run
{{#include ../code/snippets/src/scene/camera.rs:create_orthographic_camera}}
```

## Performans

Her kamera, motorun sahneyi bir kez daha yeniden oluşturmasını gerektirir ve bu, çok fazla kaynak gerektiren (hem CPU hem de GPU) bir işlem olabilir.


GPU yükünü azaltmak için, Uzak Kırpma Düzlemini mümkün olan en düşük değerlerde tutmaya çalışın. Örneğin, kapalı bir ortamda (çok sayıda koridor, küçük odalar vb.) bir oyun yapıyorsanız, Uzak Kırpma Düzlemini oyununuzda “görünebilecek” maksimum mesafeye ayarlayın. 
En büyük nesne bir koridor ise, Uzak Kırpma Düzlemini bu uzunluğu biraz aşacak şekilde ayarlayın. 
Bu, motoru sınırların dışındaki her şeyi kırpmaya ve bu tür nesneleri çizmemeye zorlayacaktır.

## Skybox

Dış mekan sahnelerinde genellikle ulaşılamayan uzak nesneler bulunur; bunlar dağlar, gökyüzü, uzak ormanlar vb. olabilir.
Bu tür nesneler önceden render edilip kameranın etrafındaki büyük bir küpün üzerine uygulanabilir; bu küp her zaman ilk olarak render edilir ve sahnenizin arka planını oluşturur.
Bir Skybox oluşturmak ve bunu bir kameraya ayarlamak için aşağıdaki kodu kullanabilirsiniz:

```rust,no_run,edition2018
{{#include ../code/snippets/src/scene/camera.rs:create_camera_with_skybox}}
```

## Renk derecelendirme arama tabloları

Renk derecelendirme Arama Tabloları (LUT), karenizin renk alanını dönüştürmenizi sağlar. Muhtemelen herkes, aksiyon Meksika'da gerçekleştiğinde her şeyin sarımsı bir renk aldığı ünlü “Meksika” film efektini görmüştür. Bu, renk derecelendirme LUT efektiyle yapılır. Akıllıca kullanıldığında, sahnenizin algısını önemli ölçüde iyileştirebilir.

İşte renk düzeltmesi yapılmamış aynı sahne ve “mexico” renk düzeltmesi yapılmış başka bir örnek:

Aynı sahnenin renk düzeltmesi yapılmamış hali ile “Meksika” renk düzeltmesi yapılmış hali aşağıda gösterilmiştir:

| Scene                                                 | Look-up-table                     |
|-------------------------------------------------------|-----------------------------------|
| ![No Color Correction](./no_color_correction.PNG)     | ![Neutral LUT](./lut_neutral.jpg) |
| ![With Color Correction](./with_color_correction.PNG) | ![Neutral LUT](./lut_mexico.jpg)  |

Renk derecelendirme LUT'unu kullanmak için şunu yapabilirsiniz:
```rust,no_run
{{#include ../code/snippets/src/scene/camera.rs:create_camera_with_lut}}
```

## Seçim 

Bazı oyunlarda sahnedeki nesneleri fare ile seçmeniz gerekebilir. Bunu yapmak için, önce ekran üzerindeki bir noktayı dünyadaki bir ışına dönüştürmeniz gerekir. `Camera`, tam da bu amaçla `make_ray` yöntemine sahiptir:

```rust,no_run
{{#include ../code/snippets/src/scene/camera.rs:make_picking_ray}}
```

Işın daha sonra [fizik varlıkları üzerinde ışın atışı yapmak için kullanılabilir](../physics/ray.md). Bu, kamera seçiminin en basit yoludur ve çoğu zaman bunu tercih etmelisiniz.

### Gelişmiş seçme

**Önemli**: Aşağıdaki seçme yöntemi yalnızca ileri düzey motor kullanıcıları içindir, matematiği bilmiyorsanız bunu kullanmamalısınız.


Matematiği biliyorsanız ve fiziksel varlıklar oluşturmak istemiyorsanız, bu ışını kullanarak manuel 
ışın kesişim kontrolü gerçekleştirebilirsiniz:

```rust,no_run
{{#include ../code/snippets/src/scene/camera.rs:precise_ray_test}}
```

`precise_ray_test` ihtiyacınız olan şeydir, bir mesh düğümünün geometrisiyle hassas kesişim kontrolü gerçekleştirir. En yakın mesafe ve en yakın kesişim noktasının bir tuple'ını döndürür. 


## Pozlama ve HDR

(WIP)