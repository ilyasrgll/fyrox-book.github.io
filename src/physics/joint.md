# Joint

Eklem, iki rijit cisim arasındaki yapılandırılabilir bir bağlantıdır ve iki cismin göreceli hareketini kısıtlar. Fyrox, 
çeşitli uygulamalar için uygun sabit bir eklem seti sunar.

- Sabit Eklem - iki cisim arasındaki sert bağlantı, iki rijit cismin metal bir çubukla birbirine “kaynaklanmış” olmasıyla aynıdır.

- Döner Eklem - tüm öteleme hareketlerini ve Y ve Z eksenleri etrafındaki tüm dönüşleri kısıtlar, ancak yerel X ekseni etrafındaki dönüşleri serbest bırakır.
 Gerçek dünyadan bir eklem örneği, kapı menteşesidir; kapının tek bir eksen etrafında dönmesine izin verir, ancak hareket etmesine izin vermez
.


- Prizmatik Eklem - tüm dönüşleri kısıtlar, hareket tek bir eksen boyunca (eklemin yerel X ekseni) izin verilir. 
Gerçek hayatta bu eklemin bir örneği, masadaki çekmeceleri destekleyen bir sürgü olabilir.

- Bilyalı Eklem - tüm hareketleri kısıtlar, ancak dönmeleri kısıtlamaz. Gerçek hayatta bilyalı eklemin bir örneği 
insan omuzu olabilir.



2D eklemlerde dönme eklemleri yoktur, çünkü bunlar bilyalı eklemlere dönüşür.

## Bedenleri Bağlama



Eklem oluşturulduğunda ve tüm bedenler ona ayarlandığında, kendi küresel dönüşümünü ve bedenlerin küresel dönüşümlerini kullanarak

bedenler için yerel çerçeveleri hesaplar. Bu işleme _bağlama_ denir ve eklem oluşturulduğunda bir kez gerçekleşir, ancak

eklemin yerel dönüşümünü değiştirerek eklemi başka bir konuma taşıyarak başlatılabilir.

## Nasıl oluşturulur

Koddan bir eklem oluşturmak için `JointBuilder` kullanın:

```rust,no_run

{{#include ../code/snippets/src/scene/joint.rs:create_joint}}

```

Eklem oluşturulduktan sonra, yukarıdaki bölümde açıklanan işlemleri kullanarak verilen gövdeleri birbirine bağlar.

Editörden bir eklem oluşturmak için  `MainMenu -> Create -> Physics -> Joint` seçin, yeni eklemi seçin ve `Body1` ve
`Body2` özelliklerini bulun. `Alt` tuşunu basılı tutarak alanları atayın ve bir rigid body'yi bir alana sürükleyip bırakın. Bağlanmanın istenildiği gibi gerçekleşmesini sağlamak için eklemi 
doğru konuma taşıyın.

## Sınırlar



İstenilen eksene bir sınır ayarlayarak birincil eklem eksenindeki hareketi (dönme ve öteleme) kısıtlayabilirsiniz.



- Bilyeli Eklem, eksen etrafındaki her dönüş için bir tane olmak üzere üç açısal sınıra sahiptir. Açı aralığı radyan cinsinden verilir.

- Prizmatik Eklem, birincil eklem ekseni boyunca iki cisim arasındaki maksimum doğrusal mesafe olmak üzere tek bir sınıra sahiptir.

- Döner Eklem, birincil eksen etrafında tek bir açısal limite sahiptir. Açı aralığı radyan cinsinden verilir.

- Sabit Eklem, tüm serbestlik derecelerini kilitlediği için herhangi bir limit ayarı yoktur.



## Kullanım




Eklemler, kapılar, zincirler ve bez bebekler gibi birçok oyun nesnesi oluşturmak için kullanılabilir. Burada en ilginç olanı 

bez bebek. Oyunlarda insanlar ve yaratıklar için gerçekçi davranışlar oluşturmak için kullanılır. Genel olarak, bir dizi 

rigid body, çarpıştırıcı ve eklemden oluşur. Her eklem, bir yaratığın eklemlerine uyacak şekilde yapılandırılır; örneğin, top eklem

omuzlar için, revolute eklemler dizler ve dirsekler için kullanılabilir.