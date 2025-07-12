# Prefabrikler



Prefabrik, başka bir sahnede örneklenebilen ayrı bir sahnedir ve örneklerinin ve ana prefabriklerin özellikleri arasındaki bağlantıları korur.

Prefabrikler, bir sahnenin bir bölümünü oluşturmanıza ve diğer sahnelerde bunun birden fazla örneğini oluşturmanıza olanak tanır.



Bunun pratikte ne anlama geldiğini hızlıca kontrol edelim. Motor, 

hiyerarşik sahneler oluşturmanıza olanak tanıyan bir prefab sistemi içerir. Bu sahneler, alt sahne olarak istediğiniz sayıda başka sahne içerebilir. Alt sahneler de kendi alt

sahneleri içerebilir ve bu şekilde devam eder. Bu, sahnenin parçalarını ayrı 

sahnelere (prefabler) yerleştirip bağımsız olarak değiştirebilmenizi sağlayan çok verimli bir ayırma mekanizmasıdır. Alt sahnelerde yapılan değişiklikler, tüm ana

sahnelere otomatik olarak yansıtılır. Bunun neden önemli olduğunun çok basit bir örneği: Bir kasabayı 3D model arabalarla doldurmanız gerektiğini düşünün.

Her araba türünün kendi 3D modeli ve örneğin oyuncunun arabaların içinden geçmesini engelleyen bir çarpışma gövdesi vardır.
 Bunu nasıl yapardınız? En basit (ve en aptalca) çözüm, sahneye düzinelerce araba modelini kopyalamaktır ve
işiniz biter. Şimdi arabanızda bir şey değiştirmeniz gerektiğini düşünün, örneğin açılabilen bir bagaj eklemek.
Ne yaparsınız? Elbette, her araba modelini “tekrarlayıp” gerekli değişiklikleri yapmanız gerekir, başka

seçeneğiniz yoktur. Bu çok zaman alır ve genellikle çok verimsizdir.


İşte bu noktada prefabrikler size saatlerce sürecek işten kurtarır. Tek yapmanız gereken bir araba prefabriği oluşturmak ve bunu sahnenizde birden çok kez örneklendirmektir. Arabada bir şeyi değiştirmeniz gerektiğinde, prefabriğe gidip onu değiştirmeniz yeterlidir. Bundan sonra, her prefabrik örneği değişikliklerinizi yansıtacaktır!

Prefabrikler, oyununuzda bağımsız varlıklar oluşturmak için kullanılabilir. Buna örnek olarak görsel efektler, 
herhangi bir komut dosyası içeren oyun varlıkları (botlar, taretler, oyuncular, kapılar vb.) verilebilir. Bu tür prefabrikler,
editördeki bir sahnede doğrudan örneklenebilir veya gerektiğinde çalışma zamanında örneklenebilir.

## Prefabrik nasıl oluşturulur ve kullanılır?

Tek yapmanız gereken, editörde gerekli tüm nesneleri içeren bir sahne oluşturmak ve kaydetmek! Bundan sonra,


sahneyi diğer sahnelerde kullanabilir ve normal 3D modellerde olduğu gibi sadece örneklendirme işlemini yapabilirsiniz. Örneklemeyi,

editeden sahne önizleyicisine bir prefab sürükleyip bırakarak veya standart [model kaynağı örneklendirme](../resources/model.md#instantiation)

## Özellik mirası



Giriş bölümünde de belirtildiği gibi, örnekler özelliklerini üst prefab'lardan miras alır. Örneğin,

prefab'daki bir nesnenin konumunu değiştirdiğinizde, tüm örnekler bu değişikliği yansıtır ve nesnenin örnekleri de

hareket eder. Bu, örnekteki bir özelliğe manuel olarak değişiklik yapılmadığı sürece geçerlidir. Manuel değişiklik yaparsanız, sizin değişiklikleriniz 


daha yüksek öncelikli olarak değerlendirilir. Daha fazla bilgi için [bu bölüme](./inheritance.md) bakın.

## Hiyerarşik Prefabrikler




Prefabriklerin içinde başka prefabrik örnekleri olabilir. Bu, örneğin,

diğer prefabriklerin örnekleriyle (kitap rafları, sandalyeler, masalar vb.) dolu bir oda oluşturabilir ve ardından oda prefabriklerini kullanarak daha büyük bir sahne oluşturabilirsiniz.

Temel prefabriklerdeki değişiklikler, hiyerarşinin derinliği ne olursa olsun, örneklerine yansıtılacaktır.