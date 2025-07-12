# Mesh node

Mesh, bir 3D modeli temsil eden bir sahne düğümüdür. Bu, hemen hemen her oyunda en sık kullanılan düğümlerden biridir.

Mesh'ler programlı olarak kolayca oluşturulabilir veya bazı 3D modelleme yazılımlarında (Blender gibi)

ve sahnenize yüklenebilir.

## Surfaces

Yüzey, aynı [malzeme](../rendering/materials.md) kullanan bir dizi üçgendir. Mesh düğümü sıfır veya 

daha fazla yüzey içerebilir; her yüzey, köşeleri üçgenlerle bağlayan bir dizi köşe ve indeks içerir. Mesh düğümleri, modern GPU'lar tarafından etkili bir şekilde işlenebilmesi için yüzeylere bölünür.

## Nasıl oluşturulur

Temel olarak iki yol vardır, hangisini seçeceğiniz ihtiyaçlarınıza bağlıdır. Genel olarak, 3D modelleme yazılımı kullanmak
en iyi yoldur, özellikle çevrimiçi olarak tonlarca ücretsiz 3D model mevcut olduğundan.

> ⚠️ Motor, 3D modeller için _sadece_ FBX ve GLTF dosya formatlarını destekler!
> GLTF kullanmak için, kök Cargo.toml dosyanızda motorun `gltf` özelliğini belirtin.

### 3D modelleme yazılımı kullanarak

3D model oluşturmak için [Blender](https://www.blender.org/) kullanabilir ve ardından `FBX` dosya formatına Export edebilirsiniz.

3D modelinizi oyuna yüklemek için birkaç basit adım izlemelisiniz (3D model yüklemek prefab 
instantiation'dan farklı değildir):

```rust,no_run
{{#include ../code/snippets/src/scene/mesh.rs:load_model_to_scene}}
```

Bu kod parçacığı, uygun `async/await` kullanımını (bunun yerine, model yüklenene kadar mevcut iş parçacığını engeller) ve hata işlemeyi kasıtlı olarak atlamaktadır. Gerçek oyunda, tüm hataları dikkatlice ele almalı ve `async/await`
'i doğru şekilde kullanmalısınız.

### Prosedürel ağ oluşturma

Bir mesh örneği koddan oluşturulabilir, bu tür mesh'lere “prosedürel” denir. Bunlar,
3D modelleme yazılımında mesh oluşturamadığınız durumlar için uygundur.

```rust,no_run
{{#include ../code/snippets/src/scene/mesh.rs:create_procedural_mesh}}
```

Gördüğünüz gibi, prosedürel olarak bir ağ oluşturmak çok fazla manuel çalışma gerektirir ve o kadar kolay değildir.

## Animation

Mesh node, kemik tabanlı animasyon (skinning) ve blend shapes'i destekler. Daha fazla bilgi için [Animasyon bölümü](./../animation/animation.md) 

bölümüne bakın.

## Veri Tamponları



Bir mesh'in köşe tamponuna ve dizin tamponuna erişerek buradaki verileri okuyabilir veya yazabilirsiniz.

Örneğin, aşağıdaki kod animasyonlu bir mesh'in her köşesinin dünya uzayındaki konumlarını çıkarır:



```rust ,no_run

{{#include ../code/snippets/src/scene/mesh.rs:extract_world_space_vertices}}

```
