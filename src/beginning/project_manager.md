# Proje Yöneticisi


Bu bölümde, proje yöneticisinin çeşitli bölümlerinin nasıl kullanıldığı, genel olarak neler yapabildiği ve neden eski konsol komutlarına tercih edilmesi gerektiği açıklanmaktadır.


## Genel Bakış


![proje yöneticisi](project_manager.png)


Proje yöneticisi, projeler oluşturmak, mevcut projeleri içe aktarmak, yapılandırmak ve derlemek gibi işlemleri gerçekleştirebilen bir araçtır. 

Ana amacı, proje yönetiminin karmaşıklığını en aza indirmektir. Örneğin, birkaç tıklama ile yeni bir proje oluşturulabilir ve derlenebilir. Aynı işlem manuel olarak da yapılabilir, ancak bu durumda konsol komutlarıyla uğraşmak gerekir ve bazı durumlarda (kodun yeniden yüklenmesi) bu komutlar birçok beklenmedik bölüm içerebilir.

## Yeni Proje Oluşturma

Yeni bir proje oluşturmak için `+create` düğmesine tıklayın, ardından aşağıdaki pencere açılacaktır:

![project manager wizard](pm_create_project.png)

Bu pencere, projeniz için dört ana seçenek içerir:



- `Yol` - projenin dizinlerinin oluşturulacağı ve daha sonra projenin dosyalarıyla doldurulacağı üst dizini belirtir.

- `Ad` - proje adı, belirli kurallara uymalıdır. Ad, bir harf veya alt çizgi (`_`) ile başlamalıdır, geri kalan karakterler harf, rakam, tire (`-`)

- `Ad` - proje adı, belirli kurallara uymalıdır. Ad, bir harf veya alt çizgi (`_`) ile başlamalıdır,

geri kalan karakterler harf, rakam, tire (`-`) veya alt çizgi (`_`) olmalıdır. Proje yöneticisi 

adınızı sizin için doğrular:

![project manager name validation](pm_validation.png)

- `Stil` - varsayılan sahnenin ilk içeriğini tanımlar. Genel olarak, sizi belirli bir boyut sayısıyla sınırlamaz

; 2D ve 3D'yi birlikte kullanabilir veya karıştırabilirsiniz.

- `Sürüm Kontrolü` - projeniz için istediğiniz sürüm kontrol sistemini (VCS) seçmenizi sağlar. Varsayılan olarak Git'tir,

ancak istediğiniz herhangi bir VCS'yi seçebilir veya `Yok` seçeneğini seçerek tamamen devre dışı bırakabilirsiniz.



Her projenin proje listesinde kendi öğesi vardır, proje hakkında önemli bilgileri gösterir:

![project info](pm_project_info.png)

1. Proje adı.

2. Projenin tam yolu.

3. Projenin kullandığı motorun sürümü.

4. Kodun sıcak yeniden yükleme işaretçisi.

## Proje Yönetimi

![project management](pm_management.png)

Bir proje seçildiğinde, sağ taraftaki araç çubuğunu kullanarak projeyi yönetebilirsiniz. Kullanılabilir seçenekler 

şunlardır:



- `Hot Reloading` - kodun sıcak yeniden yüklenmesini etkinleştirir veya devre dışı bırakır. Kodun sıcak yeniden yüklenmesi, hızlı prototip oluşturma için kullanışlı bir özelliktir.

 Daha fazla bilgi için [ilgili bölüme](./hot_reloading.md) bakın.

- `Edit` - editörü oluşturur ve çalıştırır.

- `Run` - oyunu oluşturur ve çalıştırır. Oyunun son sürümleri,


editörünün proje dışa aktarma aracı kullanılarak üretilmelidir. Daha fazla bilgi için [ilgili bölüme](../shipping/shipping.md) bakın.
 
- `IDE'yi aç` - projenin kaynak kodunu düzenlemek için belirtilen IDE'yi açar.

- `Yükselt` - istediğiniz motor sürümünü seçmenize olanak tanıyan ayrı bir araç açar. Daha fazla bilgi için

aşağıdaki bölüme bakın.

- `Temizle` - projeden tüm derleme kalıntılarını kaldırır. Esasen, projeniz için `cargo clean` komutunu çalıştırır.

- `Bul` - dosya sistemi gezgininde proje dizinini açar.

- `Sil` - projeyi siler. Bu, geri dönüşü olmayan bir işlemdir ve ayrı bir onay iletişim kutusu ile “kilitlenir”.

- `Hariç tut` - projeyi proje listesinden kaldırır.

## Proje Güncellemesi

![project manager upgrade](pm_upgrade.png)

Bu küçük araç, birkaç tıklama ile istediğiniz motor sürümünü seçmenizi sağlar. Kullanılabilir seçenekler şunlardır:



- `Specific` - motorun belirli bir sürümü. Sürüm, semver kurallarına uygun olmalıdır (örneğin - `0.36.0`).


- `Nightly` - geliştirme dalından doğrudan alınan, mümkün olan en son _potansiyel olarak kararsız_ motor sürümü (`master`)

[GitHub deposundan](https://github.com/FyroxEngine/Fyrox). En son özelliklere ve hata düzeltmelerine ihtiyacınız varsa kullanın.

- `Local` - motoru motor deposunun yerel kopyasına geçirmeyi sağlayan özel seçenek. Motor

projenizin dizinindeki üst klasörde bulunmalıdır.

## Ayarlar

![project manager settings](pm_settings.png)

Proje yöneticisinin kendi ayarları vardır, şimdilik çok fazla değildir, ancak zamanla gelişecektir. Şu anda tek bir

seçenek vardır: projenizin kaynak kodunu düzenlemek için kullanılabilen bir IDE.



Tek yapmanız gereken, IDE'nizin yürütülebilir dosyasının adını belirtmektir. Yukarıdaki görüntüde `RustRover` IDE kullanılmıştır.

Belirtilen yürütülebilir dosyaya tam yolun eklenmesi için `PATH` ortam değişkenini değiştirmeniz gerektiğini unutmayın, aksi takdirde

bu seçenek düzgün çalışmayacaktır!