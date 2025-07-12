# Animasyon Karıştırma



Animasyon karıştırma, birden fazla animasyonu tek bir animasyona karıştırmanıza olanak tanıyan güçlü bir özelliktir. Her animasyon,

 toplamı 1,0 (100%) olacak şekilde çeşitli ağırlıklarla karıştırılır. Zıt katsayılara (k1 = 0 -> 1, k2 = 1 -> 0)


zaman içinde değişerek geçiş efekti oluşturmak mümkündür.
 


Tüm katsayılarla geçişleri yönetmek rutin bir iştir, motor bunu sizin için hallederek size bazı güzel

özellikler sunar:



- Aralarında yumuşak geçişler olan birden fazla durum

- Birden fazla animasyonu tek bir animasyonda karıştırma ve karıştırma için poz kaynağı olarak kullanma

- Karıştırma katsayıları ve geçiş kuralları olarak kullanılacak bir dizi değişken belirleme



Tüm bu özellikler, animasyon karıştırma durum makinesi (ABSM) olarak adlandırılan bir sistemde birleştirilmiştir. Makine, birden fazla 

animasyonu karıştırmak ve durumlar arasında otomatik “pürüzsüz” geçişler gerçekleştirmek için kullanılır. Genel olarak, ABSM şu şekilde temsil edilebilir

:

![ABSM Structure](absm_structure.png)

İlk bakışta çok karmaşık görünebilir, ancak aslında oldukça basit teknikler kullanılır.


resmin sol tarafından başlayıp sağa doğru ilerleyelim. Soldaki sarı rectangle,

karıştırma için kullanılacak bir dizi animasyon içeren bir animasyon oynatıcı düğümünü temsil eder. Ortadaki iki blok (katman 0 ve katman 1)

ayrı katmanları temsil eder (ABSM içinde herhangi bir sayıda katman olabilir). Her katman, rastgele düğümler (yeşil


şekiller), durumlar (mavi şekiller) ve geçişler (kalın sarı oklar) içerebilir.
 


Düğümler, istenen şekilde karıştırılabilen pozların kaynağı olarak işlev görür. Durumlar, iç durum 


makinesinin bir parçasıdır ve aynı anda yalnızca bir durum etkin olabilir. Geçişler, durum geçiş kurallarını belirtmek için kullanılır.
 


Her katmanın “çıkışında” bir katman filtresi bulunur. Bu filtre, belirli

sahne düğümleri için değerleri filtrelemekten sorumludur ve bazı sahne düğümlerinin belirli bir katman tarafından canlandırılmasını önlemek için kullanılabilir.

Görünüşüne rağmen, katman filtresi tüm animasyonlar ve durumlar karıştırıldıktan sonra uygulanması gerekmez -

bu işlem herhangi bir anda yapılabilir ve sadece basitlik amacıyla bu şekilde çizilmiştir.



Resimde son olarak, ancak en az değil, önemli olan şey, resmin sağ tarafındaki parametre konteyneridir.

 Parametre, bir geçiş kuralı, karıştırma ağırlığı veya örnekleme noktasıdır. Geçişlere veya animasyon karıştırma düğümlerine yakından bakarsanız,

 küçük metin işaretleri göreceksiniz. Bunlar, ilgili parametrelerin adlarıdır.



Genel olarak, tüm durum makineleri bu şekilde çalışır - ABSM düğümleri animasyonları karıştırmak veya almak için kullanılır ve bunların sonuçta ortaya çıkan

pozları ABSM durumları tarafından kullanılır. Etkin durum, son pozu sağlar, bu poz daha sonra filtrelemeden geçer ve size geri döndürülür.

 Son aşamadan sonra, pozu bir sahne grafiğine uygulayarak sonuçta ortaya çıkan animasyonun etkili olmasını sağlayabilirsiniz.

## Nasıl oluşturulur



Her zaman olduğu gibi, Fyrox'ta bir şeyler oluşturmanın iki ana yolu vardır: düzenleyiciden veya koddan. Seçim sizin.

## Editörden



Animasyon karıştırma durum makineleri oluşturmak için [ABSM Editor](absm_editor.md) kullanın.

## Koddan



Her zaman koddan bir ABSM oluşturabilirsiniz. Basit bir ABSM şu şekilde oluşturulabilir:

```rust,no_run
{{#include ../code/snippets/src/animation/blending.rs:create_absm}}
```

Burada, farklı poz kaynakları kullanan Yürüme, Boşta ve Koşma durumları vardır:

- Yürüme - burada en karmaşık olanıdır - farklı ağırlıklarla `Aim` ve `Walk` animasyonlarının karıştırılmasının sonucunu kullanır.

 Bu, karakterinizin sadece yürüyebilmesi veya aynı anda yürüyüp *ve* nişan alabilmesi durumunda kullanışlıdır. İstenen poz,

 Yürüme Ağırlığı ve Nişan Ağırlığı parametrelerinin kombinasyonu ile belirlenir.

- Run ve idle, poz kaynağı olarak doğrudan animasyonu kullanır.



Üç durum arasında, her birinin kendi kuralı olan dört geçiş vardır. Kural, geçişin etkinleştirilmesi gerektiğini belirten bir boole parametresidir.

 Yukarıdaki durum grafiğinin kod örneğine bakalım:



Gördüğünüz gibi, her şey oldukça basit. Bu kadar basit bir durum makinesi bile oldukça fazla kod gerektirir, ancak

bu kod ABSM editörü kullanılarak kaldırılabilir. Bununla ilgili bilgi için bir sonraki bölüme geçin.
