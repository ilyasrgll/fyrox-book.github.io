# Işın Atma

Işın atma, bir sahnedeki katı cisimler ile ışının kesişim noktalarını sorgulamanızı sağlar. Işın atmanın tipik kullanım alanları arasında
vuruş taramalı silahlar (yüksek hızlı mermiler ateşleyen silahlar), AI çarpışma önleme vb. bulunur. Kesişim noktalarını sorgulamak için
sahne grafiğinin fizik dünyası örneğini kullanın:

```rust,no_run
{{#include ../code/snippets/src/scene/ray.rs:do_ray_cast}}
```

Yukarıdaki işlev, kesişme mesafesine göre sıralanmış (ışının başlangıcından kesişme noktasına olan mesafe) kesişme noktalarının bir koleksiyonunu döndürür. Her kesişme noktası aşağıdaki yapı ile temsil edilir:


```rust,no_run
pub struct Intersection {
    pub collider: Handle<Node>,
    pub normal: Vector3<f32>,
    pub position: Point3<f32>,
    pub feature: FeatureId,
    pub toi: f32,
}
```

- `collider` - kesişmenin algılandığı çarpışmanın tanıtıcısı. Katı cismin tanıtıcısını elde etmek için, `collider`'ı ödünç alın ve `parent` alanını getirin: `graph[collider].parent()`.

- `normal` - dünya koordinatlarında kesişme konumundaki normal.

- `position` - dünya koordinatlarında kesişmenin konumu.
- `feature` - kesişmenin algılandığı özelliğin türünü ve indeksini içeren ek veriler.
FeatureId::Face, üçgen ağındaki üçgen sayısından daha büyük bir indekse sahip olabilir; bu, kesişmenin yüzün “arka” tarafından algılandığı anlamına gelir. Bu indeksi “düzeltmek” için, üçgen ağındaki üçgen sayısını değerden çıkarmanız yeterlidir.

- `toi` - (`time of impact`) ışının başlangıç noktasından `position` konumuna olan mesafe.
- `toi` - (`çarpışma zamanı`) ışının başlangıç noktasından `konum`a olan mesafe.

## Gereksiz tahsislerden kaçınmak

Fark etmiş olabileceğiniz gibi, yukarıdaki işlev `Vec<Intersection>` döndürür ve bu da kesişimleri yığın üzerinde tahsis eder. Bu,
nispeten yavaştır ve yığın üzerinde statik dizi kullanılarak çok daha hızlı hale getirilebilir:

```rust,no_run
{{#include ../code/snippets/src/scene/ray.rs:do_static_ray_cast}}
```

`usage_example`, `do_static_ray_cast` işlevinin nasıl kullanıldığını gösterir - tek yapmanız gereken, ilgilendiğiniz maksimum kesişme sayısını genel parametre olarak belirtmektir.
