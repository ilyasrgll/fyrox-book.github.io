# Tasarım Felsefesi ve Hedefleri

Motorun tasarım felsefesi ve hedefleri hakkında biraz konuşalım. Motorun geliştirilmesine

2019'un başında Rust öğrenmek için bir hobi projesi olarak başlandı ve kısa sürede Rust'un oyun geliştirme endüstrisinde bir devrim yaratabileceği ortaya çıktı

. Başlangıçta motor, C dilinde yazılmış [bir motorun](https://github.com/mrDIMAS/DmitrysEngine) bir portuydu. 

Başlangıçta, herhangi bir güvenlik garantisi olmadan bu kadar düşük seviyeli bir dilde oyun motoru gibi karmaşık bir şey oluşturmak çok ilginçti.

güvenlik garantisi olmadan böyle karmaşık bir şeyin yapımı çok ilginçti. Bir yıllık geliştirme sürecinin ardından, bellek ile ilgili sorunları (bellek bozulması,

sızıntılar vb.) düzeltmek can sıkıcı hale geldi. Neyse ki o sırada Rust'un popülaritesi arttı ve benim dikkatimi çekti. Ben ([@mrDIMAS](https://github.com/mrDIMAS)) 


, motoru bir yıldan kısa bir sürede Rust'a taşıyabildim. Kararlılık önemli ölçüde arttı, rastgele çökmeler sona erdi, 

performans aynı veya daha iyi seviyelere ulaştı - yeni dili öğrenmek için harcadığım zaman karşılığını verdi. Geliştirme hızı

C'de olduğu gibi zamanla düşmüyor, büyüyen projeleri yönetmek çok kolay.

## Güvenlik



Motorun geliştirilmesindeki ana hedeflerden biri, yüksek düzeyde güvenlik sağlamaktır. Bu ne anlama gelir?

Kısaca: bellek güvenliği ile ilgili hatalardan koruma. Bu, mantık hatalarını içermez, ancak oyununuz bellek güvenliği nedeniyle rastgele çökmelerden arındırılmışsa,

 mantık hatalarını düzeltmek çok daha kolaydır, çünkü
.



Güvenlik, oyununuzun mimari tasarım kararlarını da belirler. Diğer birçok dilde mümkün olan tipik geri arama cehennemi,

 Rust'ta uygulamak çok zahmetlidir. Mümkündür, ancak oldukça fazla manuel çalışma gerektirir

ve bu da hızlı bir şekilde yanlış yaptığınızı gösterir.



## Performans



Oyun motorları genellikle en yüksek performans seviyelerini sağlayan sistem düzeyinde programlama dilleri kullanılarak oluşturulur. Fyrox da

bir istisna değildir. Tasarım hedeflerinden biri, varsayılan olarak yüksek performans seviyeleri sağlamak ve performansın kritik olduğu yerler için özel çözümler ekleme olanağı sunmaktır.



## Kullanım kolaylığı



Bir diğer önemli nokta ise motorun yeni başlayanlar için kolay kullanımlı olmasıdır. Giriş eşiğini düşürmeli, zorlaştırmamalıdır.

 Fyrox, iyi bilinen ve savaşta test edilmiş kavramları kullanır, böylece oyun yapmayı kolaylaştırır. Öte yandan,

ihtiyacınız olan her şeyle genişletilebilir - oyun endüstrisinin deneyimli isimleri için olduğu kadar yeni başlayanlar için de iyi olmaya çalışır .



## Savaşta test edilmiş



Fyrox, genel amaçlı bir oyun motorunun gerçek ihtiyaçlarını anlamaya yardımcı olan büyük projeler üzerine inşa edilmiştir. Ayrıca

tasarımdaki zayıf noktaları ortaya çıkarmaya ve düzeltmeye
