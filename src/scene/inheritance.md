# Özellik Kalıtımı



Özellik kalıtımı, değiştirilmemiş özelliklerin bir prefabrikten örneklerine yayılması için kullanılır. Örneğin, bir prefabrikteki bir düğümün ölçeğini değiştirebilirsiniz ve ölçek bir örnekte açıkça belirtilmedikçe, örneklerin ölçeği de aynı olacaktır. Bu özellik, örnekleri ayarlamanıza, onlara bazı benzersiz ayrıntılar eklemenize, ancak genel özellikleri ana prefablardan almanızı sağlar.



Özellik mirası, herhangi bir derinlikteki prefab hiyerarşileri için çalışır. Bu, şunun gibi bir şey oluşturabileceğiniz anlamına gelir:
Bir oda prefabında çeşitli mobilya prefablarının birden fazla örneği bulunabilir, mobilya prefabları da diğer prefablardan oluşturulabilir ve bu şekilde devam edebilir. Bu durumda, zincirdeki prefablardan birinde bir özelliği değiştirirseniz, tüm örnekler değiştirilmemiş özelliklerini hemen senkronize eder. 



## Kalıtsal Özellikler Nasıl Oluşturulur



Özellik kalıtımı, komut dosyası değişkenleri için de kullanılabilir. Komut dosyanızdaki bir özelliği kalıtsal hale getirmek için, değerini `InheritableVariable` sarmalayıcısıyla sarmalamanız yeterlidir.



```rust,no_run

{{#include ../code/snippets/src/scene/inheritance.rs:my_script}}

```



Motor, komut dosyasının bulunduğu sahne yüklendiğinde özelliğin doğru değerini otomatik olarak çözer.

özelliğiniz değiştirilirse, değeri aynı kalır ve üst öğenin değeriyle üzerine yazılmaz. Unutmayın ki,

 miras alınabilir değişkenin türü klonlanabilir olmalı ve yansıtmayı desteklemelidir.



`InheritableVariable`, `Deref<Target = T> + DerefMut` özelliklerini uygular; bu, `DerefMut` özelliği aracılığıyla yapılan her erişimin

özelliği değiştirilmiş olarak işaretlenir. Bu, bazı durumlarda istenmeyen bir durum olabilir, bu nedenle `InheritableVariable`, iç değiştiricilere dokunmayan ve değeri “sessizce” başka bir değerle değiştirmenize olanak tanıyan özel `xxx_silent` 

yöntemlerini destekler -

değişkeni değiştirilmiş olarak işaretlemeden.



## Hangi Alanlar Kalıtsal Olmalıdır?



“Atomik” olması amaçlanan miras alınabilir değişkenler - bu, değişkenin bazı basit değişkenleri (`f32`, `String`,

`Handle<Node>`, vb.) depoladığı anlamına gelir. “Bileşik” değişkenleri (`InheritableVariable<YourStruct>`) depolamak mümkün olmakla birlikte,

 miras alma mekanizması nedeniyle tavsiye edilmez. 
Motor, miras alınabilir bir değişken gördüğünde, aynı değişkeni

üst varlıkta arar ve değerini alt varlığa kopyalar, böylece içeriğini tamamen değiştirir. Bu durumda, bileşik alan içinde miras alınabilir değişkenler olsa bile, bunlar doğru şekilde miras alınmaz.

Bunu aşağıdaki kod parçacığında gösterelim:



```rust,no_run

{{#include ../code/snippets/src/scene/inheritance.rs:complex_inheritance}}

```



Bu kod parçacığı, miras alınabilir alanların bazı “basit” veriler içermesi gerektiğini ve neredeyse hiçbir zaman karmaşık

yapılar içermemesi gerektiğini açıklığa kavuşturmalıdır.



## Editör



Editör, tüm miras alınabilir özellikleri, özellik geri alma özelliğini destekleyen özel bir widget ile sarar. Geri alma,

mevcut değişiklikleri silmenize ve ebeveynin özellik değerini almanıza olanak tanır. Bu, bir özelliğin ebeveyninin değerini miras almasını istediğinizde kullanışlıdır.

 Denetçide şöyle görünür:



![revert](./revert.png)



`<` düğmesine tıklandığında, ebeveyn prefab'dan değer alınır ve özellik artık değiştirilmiş olarak işaretlenmez.

Ebeveyn prefab yoksa, düğme sadece `modified` bayrağını kaldırır.
