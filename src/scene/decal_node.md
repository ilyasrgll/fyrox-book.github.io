# Decal node

Decal düğümleri, belirli sınırlar içinde bir dokuyu sahnenize “yansıtmanızı” sağlar. Genellikle

kurşun delikleri, kan sıçramaları, kir, çatlaklar vb. için kullanılır. Aşağıda sahneye uygulanan decal örneği gösterilmektedir:

![Decal](./decal.PNG)

Pas izleri, belirli bir yönde pas dokusu yansıtılarak sahnenin mevcut geometrisine uygulanır.

## Nasıl oluşturulur

Bir çıkartma örneği DecalBuilder kullanılarak oluşturulabilir:

```rust,no_run
{{#include ../code/snippets/src/scene/decal.rs:create_decal}}
```

## Textures

Çıkartmanın hangi dokuları yansıtacağını belirleyebilirsiniz, şu anda yalnızca diffüz ve normal haritalar

desteklenmektedir.

## Rendering

Şu anda motor yalnızca _ertelenmiş çıkartmaları_ desteklemektedir, bu da çıkartmaların

G-Buffer'da depolanan bilgileri değiştirdiği anlamına gelir. Bu durum, çıkartmaların sahnedeki diğer geometrilerle birlikte doğru şekilde aydınlatılacağı anlamına gelir. Ancak, sahnenizde ileri işleme yolunu kullanan bazı nesneler varsa, çıkartmalarınız bunlara uygulanmayacaktır.

## Bounds

Decal, decal dokularının yansıtılacağı pikselleri belirlemek için Nesneye Yönelik Sınır Kutusu (OOB) kullanır.
OOB'ye giren her şey kaplanacaktır. Kesin sınırlar, decal'ın yerel dönüşümünü ayarlayarak belirlenebilir.
Decal'ınızın daha büyük olmasını istiyorsanız, ölçeğini büyük bir değere ayarlayın. Decal'ı konumlandırmak için yerel konumu,
döndürmek için yerel dönüşü kullanın.



Decal, küpün içine giren sahnenin her pikseline bir doku yansıtan bir küp tanımlar. Kesin küp boyutu
decal'ın yerel ölçeği ile tanımlanır. Örneğin, ölçeği (1.0, 2.0, 0.1) olan bir decal varsa,
küpün boyutu (yerel koordinatlarda) genişlik = 1.0, yükseklik = 2.0 ve derinlik = 0.1 olacaktır. Decal, diğer
sahne düğümleri gibi döndürülebilir. Nihai boyutu ve yönü, üst düğümlerin dönüşüm zinciri tarafından tanımlanır.

## Layers

Bazı geometrilerin çıkartma ile kaplanmasını önlemek istediğiniz durumlar olabilir. Bunu yapmak için motor

katman kavramını sunar. Bir çıkartma, yalnızca eşleşen katman indeksine sahipse bir geometriye uygulanır. Bu,

 ortam hasarı çıkartmaları oluşturmanıza olanak tanır ve bunlar farklı katmanlarda bulundukları için dinamik nesneleri etkilemezler.

## Performans



Çıkartmaların mevcut uygulaması nispeten ucuzdur, bu da sahnede birçok çıkartma oluşturmanıza olanak tanır. Ancak,
çıkartma sayısını makul bir seviyede tutmalısınız.
