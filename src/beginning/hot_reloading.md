# Code Hot Reloading (CHR)

Fyrox, oyun çalışırken oyun kodunu yeniden derlemenizi sağlayan CHR (kısaca kodun sıcak yeniden yüklenmesi) özelliğini destekler.

Bu işlevsellik, yineleme sürelerini önemli ölçüde azaltır ve hızlı prototip oluşturmaya olanak tanır. Bu sayede Rust, bir tür

“komut dosyası” dili haline gelir, ancak tüm Rust güvenlik ve performans garantileriyle birlikte. CHR'nin çalışması şöyle görünür:

<iframe width="560" height="315" src="https://www.youtube.com/embed/vq6P3Npydmw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Nasıl Kullanılır

> ⚠️ Motorun önceki sürümlerinden birinde mevcut bir projeniz varsa,
> CHR desteğini eklemenin en iyi yolu, tüm projeyi yeniden oluşturmak ve tüm varlıkları ve oyun kodunu yeni projeye kopyalamaktır. CHR, çok
> özel bir proje yapısı gerektirir ve bu yapıda küçük bir hata bile hatalı davranışlara yol açabilir.

CHR'nin kullanımı oldukça basittir - proje yöneticisi veya `fyrox-template` tarafından oluşturulan bir proje, 

sıcak yeniden yükleme için gereken her şeye zaten sahiptir. Sıcak yeniden yükleme desteğini etkinleştirmenin iki yolu vardır: proje yöneticisini kullanmak ve 

aynı işlemi konsol komutlarını kullanarak manuel olarak yapmak.

### Proje Yöneticisi

Sıcak yeniden yükleme desteğini etkinleştirmenin en kolay yolu, proje yöneticisinde `hot reloading` onay kutusunu tıklayıp

Edit` veya `Run`'ı tıklayın:

![project manager hot reloading](project_manager_hot_reloading.png)



Küçük “ateş” simgesine dikkat edin, bu simge projenin bu özelliğin etkin olduğunu gösterir. Bu özelliği istediğiniz zaman etkinleştirebilir veya devre dışı bırakabilirsiniz.

 
### Konsol Komutları

Aynı işlemi konsol komutlarıyla yapmak için bazı ön hazırlıklar gerekir. İlk olarak, aşağıdaki komutu kullanarak oyun eklentinizi derlemeniz gerekir :

```shell
RUSTFLAGS="-C prefer-dynamic=yes" cargo build --package game_dylib --no-default-features --features="dylib-engine" --profile dev-hot-reload
```

Bu komut, motor DLL'sini (`fyrox_dylib.dll/so`) ve eklenti DLL'sini (`game_dylib.dll/so`) derleyecektir. 
zorunlu ortam değişkeni `RUSTFLAGS=“-C prefer-dynamic=yes”`'e dikkat edin. Bu, derleyiciyi standart kütüphaneyi 
dinamik olarak bağlamaya zorlar. Bu çok önemlidir, çünkü ayarlanmazsa standart kütüphane oyun eklentisi ve motorunda çoğaltılır ve
bu da ince hatalara yol açar.

> ⚠️ Çevre değişkenleri, işletim sisteminize bağlı olarak farklı şekillerde ayarlanabilir. Linux'ta, gerçek
> komutun önüne eklenir, Windows'ta ise [ayrı bir komut](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/set_1#examples) gerektirir. 
> Diğer işletim sistemlerinde çevre değişkenlerini ayarlamanın kendi yöntemleri olabilir.

Bir sonraki adım, editörü CHR modunda derlemektir. Bunu yapmak için aşağıdaki komutu çalıştırın:

```shell
RUSTFLAGS="-C prefer-dynamic=yes" cargo run --package editor --no-default-features --features="dylib" --profile dev-hot-reload
```

Bu komut, editörü CHR modunda derleyecek ve çalıştıracaktır. Bundan sonra, yapmanız gereken tek şey editörde derleme profilini
`Debug (HR)` olarak seçmektir:

![img.png](build_profile.png)

Bu işlem tamamlandıktan sonra yeşil renkli `Oynat` düğmesine tıklayarak oyununuzu çalıştırabilirsiniz. CHR ve normal mod
(statik bağlantı) arasında istediğiniz zaman geçiş yapabilirsiniz. Editörü CHR modunda çalıştırırsanız, değiştirilen tüm eklentilerin de yeniden yükleneceğini unutmayın.

## Profiller Oluşturun

CHR ayrı derleme profilleri kullanır: `dev-hot-reload` (optimizasyon yok) ve `release-hot-reload` (optimizasyonlu).

Ayrı derleme profilleri, statik olarak bağlanmış Plugin ve code hot reloading arasında hızlıca geçiş yapmanızı sağlar. Bu, sıcak yeniden yükleme ile ilgili bazı sorunlar yaşıyorsanız yararlı olabilir (daha fazla bilgi için sonraki bölüme bakın).

## Kararlılık

CHR, motorun çok yeni ve deneysel bir özelliğidir. Bu özellik, bellek bozulmasına, ince hatalara vb. neden olabilecek son derece güvenli olmayan işlevselliğe dayanmaktadır.
 Sıcak yeniden yükleme sonrasında oyununuzda garip davranışlar gözlemlerseniz, oyunu normal (statik bağlantı) modunda çalıştırın.
 Herhangi bir hatayı [sorun izleyici](https://github.com/FyroxEngine/Fyrox/issues) 
'na bildirin. CHR, nispeten büyük iki oyunda test edildi: [Fish Folly](https://github.com/mrDIMAS/FishFolly) ve 
[Station Iapetus](https://github.com/mrDIMAS/StationIapetus). Bu projeleri indirip CHR'yi kendiniz deneyebilirsiniz.

## Teknik Ayrıntılar ve Sınırlamalar



CHR, paylaşımlı kütüphanelerin (kısaca DLL) standart işletim sistemi (OS) mekanizmasını kullanır. Hemen hemen tüm işletim sistemleri,
 çalışan bir sürece DLL'den dinamik olarak yerel kodu yükleyebilir. Dinamik olarak yüklenen herhangi bir kütüphane,
süreç belleğinden kaldırılabilir. Bu, oyun kodunu çalışma sırasında yeniden yüklemek için mükemmel bir fırsat sunar. Kulağa oldukça kolay gelebilir, ancak pratikte 
birçok sorun vardır.

### Plugin Varlıkları ve Yeniden Yükleme

cEklentiler, motora önceden tanımlanmış bir dizi varlık (komut dosyaları vb.) sağlayabilir. Bu varlıklar, 
eklenti kendiliğinden kaldırılmadan önce bir bellek blobuna serileştirilir. Tüm eklentiler yeniden yüklendiğinde, bu bellek blobu eklenti varlıklarının durumunu geri yüklemek için kullanılır.
 Bununla birlikte, hemen hemen tüm eklenti varlıkları serileştirilebilir olmalıdır (`Visit` özelliğini uygulayın).

### Özellik Nesneleri

Özellik nesneleri, sıcak yeniden yükleme ile çok sorunludur, çünkü içlerinde işlev işaretçileri içeren vtable bulunur.
 Bu işaretçiler, eklenti kaldırıldığında kolayca geçersiz hale gelebilir. Bu, doğrudan eklenti tarafından oluşturulmuşsa
motor özellik nesneleri için de geçerlidir. Bu sorunu aşmanın tek yolu, özellik nesnelerini oluşturmak için
motorunun özel yöntemlerini kullanmaktır. Bu tür durumları kontrol etmek için clippy'ye bir lint eklemek mümkündür (ilgili 
[sorun](https://github.com/rust-lang/rust-clippy/issues/12819) bölümüne bakın).

### Asılı Nesneler


Mevcut eklenti sistemi, eklentileri yeniden yüklemeden önce motorun içinden tüm eklenti varlıklarını kaldırmak için elinden geleni yapar.
Ancak, bazı nesneler bu sistem tarafından gözden kaçabilir ve bu da çökmeye veya bellek bozulmasına neden olabilir. Mevcut
sarkan nesnelerin oluşmasını önleme yaklaşımı, yerleşik yansıma sistemine dayanmaktadır — eklenti sistemi, 
her nesnenin tüm alanlarını yineler ve derleme adını kontrol eder. Derleme adı eklentinin derleme adıyla eşleşirse, 
bu nesne eklenti kaldırılmadan önce silinmelidir.
 

### Seri hale getirilemeyen varlıklar


Her nesne seri hale getirilemez ve bu durumda, mevcut eklenti sistemi, sıcak yeniden yükleme sonrasında bu tür
seri hale getirilemeyen varlıkları geri yüklemek için özel bir yöntem çağırır. Bu tür varlıklar arasında sunucu bağlantıları, iş kuyrukları vb. bulunabilir.