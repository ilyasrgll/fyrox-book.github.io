# Model kaynakları



## Desteklenen formatlar



Fyrox, 3D modeller için şu dosya formatlarını destekler:



- FBX - standart oyun geliştirme endüstrisi 3D model değişim formatı

- RGS - Fyroxed (editör) tarafından üretilen yerel sahne formatı



Liste gelecekte genişletilebilir.



## Örnekleme



Model, sahnenizde örneklenmelidir, başka şekilde kullanılamaz. Bunu yapmak için, düzenleyicideki Varlık Tarayıcı'dan sürükle ve bırak

yöntemini kullanabilir veya modeli koddan dinamik olarak örneklendirebilirsiniz:

```rust,no_run,edition2018
{{#include ../code/snippets/src/resource/model.rs:instantiate_model}}
```

## Material import



Motor, malzemeleri modeldeki orijinallerine mümkün olduğunca yakın bir şekilde içe aktarmaya çalışır, ancak bu her zaman mümkün değildir

çünkü bazı 3D modelleme yazılımları farklı gölgelendirme modelleri kullanabilir. Varsayılan olarak, motor

her şeyi PBR malzemelerine dönüştürmeye çalışır, bu nedenle çizgi film gölgelendirme için özel olarak yapılmış bir malzemeye sahip bir 3D modeliniz varsa,

motor yine de PBR malzeme olarak içe aktaracaktır (tabii ki birçok doku eksik olacaktır). PBR malzemelerinden başka malzemelerle çalışırken bunu

dikkate almalısınız. 



3D modelinizde garip malzemeler varsa, uygun malzemeleri ve gölgelendiricileri _manuel olarak_ oluşturmalısınız.

Motor sihirli bir araç değildir, tüm olası durumları kapsamayan bazı varsayılan ayarları vardır.



3D modeli yüklerken dokuların nasıl çözüleceğini de belirlemek mümkündür. `Asset Browser`

'da modelinizi seçin, model önizlemesinin hemen altında içe aktarma seçenekleri olacaktır:



![model içe aktarma](model_import.png)



Bu seçenekleri manuel olarak da belirlemek mümkündür. Bunu yapmak için, 3D modelinizin yanına aşağıdaki içeriğe sahip bir içe aktarma seçenekleri dosyası oluşturmanız gerekir (bu, editörün sizin için yaptığı işlemdir):

```text
(
    material_search_options: RecursiveUp
)
```

Dosya, `.options` ek uzantısına sahip olmalıdır. Örneğin, `foo.fbx` modeliniz varsa, seçenekler

dosyasının adı `foo.fbx.options` olmalıdır. Elle değiştirilebilse bile, karışıklık olasılığını azaltmak için

dışa aktarma seçeneklerini düzenlemek için düzenleyiciyi kullanmanız şiddetle tavsiye edilir.



## Blender için ipuçları



Blender'ın FBX dışa aktarıcısı, dışa aktarma ölçek özelliklerini genellikle %100 olarak ayarlar, bu da modelinizin motor içinde yanlış ölçeklenmesine neden olabilir.

 Ölçek, `(100.0, 100.0, 100.0)` gibi çok büyük bir değer alır. Bunu düzeltmek için

dışa aktarıcıdaki ölçeği `0.01` olarak ayarlayın.



## 3Ds Max için ipuçları



3Ds max'in son sürümlerinde, malzeme içe aktarımını bozabilecek bazı “gereksiz” düğümler oluşturan düğüm tabanlı malzeme düzenleyici vardır.

 Bununla ilgili sorunları önlemek için, haritaları doğrudan kullanmak üzere malzeme yuvalarına yapılan tüm atamaları temizlemelisiniz.