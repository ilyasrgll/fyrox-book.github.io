# Animasyon Karıştırma Durum Makinesi (ABSM) Düzenleyicisi

Animasyon karıştırma ve durumunu koddan manuel olarak oluşturmak ve yönetmek mümkün olsa da, bu işlem çok 
can sıkıcı ve yönetilemez hale gelir. Karıştırma makinelerini kolay bir şekilde oluşturmanıza ve yönetmenize yardımcı olmak için motor, 
bir ABSM Editör aracı sunar. Bu bölüm, editörün genel bir özetidir. Oldukça karmaşıktır, ancak kılavuz, hangi bölümün ne için olduğunu anlamanıza
yardımcı olacaktır. Bir sonraki bölüm, ilk animasyon karıştırma durum makinenizi oluşturmanıza yardımcı olacaktır.

![absm editor](./absm.png)

Editör dört ana bölümden (panelden) oluşur:

1. `Araç Çubuğu` - animasyon katmanlarını düzenlemek ve önizleme modunu etkinleştirmek/devre dışı bırakmak için bir dizi araç içerir. Daha fazla bilgi için [Araç Çubuğu](#toolbar)
bölümüne bakın.

2. `Parametreler` - geçişlerden, karıştırma için ağırlık parametrelerinden sorumlu çeşitli değişkenleri düzenlemenizi sağlar. Daha fazla bilgi için [Parametreler](./absm_parameters.png) bölümüne bakın.

3. `Durum Grafiği` - durumlar oluşturmanıza, silmenize, düzenlemenize ve bunlar arasında geçiş yapmanıza olanak tanır. Daha fazla bilgi için [Durum Grafiği](#state-graph) bölümüne bakın.

4. `Durum Görüntüleyici` - bir durumun poz kaynağını düzenlemenizi sağlar. Poz kaynağı, bir animasyonu oynatan tek bir
düğüm veya karıştırma düğümlerine bağlı bir dizi oynatma animasyonu düğümü (diğer karıştırma düğümlerine bağlanabilir
vb.) ile temsil edilebilir. Daha fazla bilgi için [Durum Görüntüleyici](#state-viewer) bölümüne bakın.


Editör iki şekilde açılabilir: `Utils -> ABSM Editor` kullanarak veya bir animasyon karıştırma durum makinesi
düğmesini seçip `Open ABSM Editor...` düğmesini tıklayarak:

![open1](./absm_open1.png)

![open1](./absm_open2.png)

Her iki durumda da, düzenleme için bir animasyon karıştırma durum makinesi düğümü seçmeniz gerekir.

## Toolbar

![toolbar](./absm_toolbar.png)

1. `Önizleme Anahtarı` - ABSM için önizleme modunu etkinleştirir veya devre dışı bırakır. Daha fazla bilgi için [Önizleme Modu](#preview-mode) bölümüne bakın.

2. `Katman Adı` - seçilen katmanın adı. Seçili katmanı yeniden adlandırmak için buraya yeni bir ad yazın (Enter tuşuna basın veya yeniden adlandırmak için başka bir yere tıklayın).

3. `Katman Ekle` - `Katman Adı` metin kutusuna yazılan adla ABSM'ye yeni bir katman ekler. ABSM'de aynı ada sahip birden fazla katman olabilir, ancak burada benzersiz adlar belirlemeniz şiddetle tavsiye edilir.

4. `Mevcut Katmanı Kaldır` - seçili katmanı kaldırır. Tüm katmanları silebilirsiniz, ancak bu durumda ABSM'niz herhangi bir etki yapmayacaktır.

5. `Katman Seçici` - düzenlemek için bir katman seçmenizi sağlar, varsayılan seçim yoktur.

6. `Katman Maskesi` - bir `Katman Maskesi Düzenleyici` açar ve mevcut katmanın katman maskesini düzenlemenize yardımcı olur. Daha fazla bilgi için
[Katman Maskesi](#layer-mask) bölümüne bakın.

## Parameters

Parametre, animasyon sisteminin çalışması için gerekli bazı verileri sağlayan, adlandırılmış ve türlenmiş bir değişkendir.

Sadece üç tür parametre vardır:



- `Rule` - geçişler için tetikleyici olarak kullanılan boole değeri. Geçiş bir kural kullanıyorsa, parametrenin değerini kontrol eder
ve `true` ise geçiş başlar.

- `Weight` - Birden fazla animasyonu tek bir animasyona karıştırırken ağırlık olarak kullanılan gerçek sayı (`f32`).

- `Index` - Animasyon seçici olarak kullanılan doğal sayı (`i32`).

![parameters](./absm_parameters.png)

1. `Parametre Ekle` - parametreler konteynerine yeni bir parametre ekler.

2. `Parametre Kaldır` - parametreler konteynerinden seçilen parametreyi kaldırır.

3. `Parametre Adı` - parametre adı belirlemenizi sağlar.

4. `Parametre Türü` - parametrenin türünü seçmenizi sağlar.

5. `Parametre Değeri` - parametre değerini ayarlamanızı sağlar.

## State Graph

Durum Grafiği, durumlar ve bunlar arasındaki geçişleri oluşturmanıza olanak tanır. 

![state graph](./absm_state_graph.png)

1. `State` - durum, bir dizi sahne düğümü için son animasyondur, bir seferde yalnızca bir durum etkin olabilir.

2. `Transition` - iki durum arasındaki _sıralı_ bağlantıdır, iki durumun 

karışması için gereken süreyi tanımlar.

3. `Root State` - geçerli katmanın giriş durumudur.

### State Context Menu

![state context menu](./absm_state_context_menu.png)

- `Geçiş Oluştur` - mevcut durumdan başka bir duruma geçiş oluşturmaya başlar.

- `Kaldır` - durumu kaldırır.

- `Giriş Durumu Olarak Ayarla` - durumu giriş durumu olarak işaretler (bu durum başlangıçta etkin olacaktır).

### Transition Context Menu

![transition context menu](./absm_transition_context_menu.png)

- `Remove Transition` - seçilen geçişi kaldırır.

### State Properties

Aşağıdaki özellikleri düzenlemek için bir `State` düğümü seçin:

![state properties](./absm_state_properties.png)

- `Position` - tuval üzerindeki durumun konumu.

- `Name` - durumun adı.

- `Root` - durum içindeki destek animasyon düğümünün tanıtıcısı.

### Transition Properties

Aşağıdaki özellikleri düzenlemek için bir `Transition` düğümü seçin:

![transition properties](./absm_transition_properties.png)

- `Name` - durumun adı.

- `Transition Time` - iki durum arasında geçiş süresi (saniye cinsinden).

- `Elapsed Time` - geçiş süresinin başlangıç değeri.

- `Source` - kaynak durumun tanıtıcısı.

- `Desc` - hedef durumun tanıtıcısı.

- `Rule` - geçişin etkinleştirilip etkinleştirilemeyeceğini tanımlayan `Rule` türü parametrenin adı.

- `Invert Rule` - `Rule` değerinin tersine çevrilip çevrilmeyeceğini tanımlar.

- `Blend Factor` - geçişin ne kadarının etkin olduğunu yüzde olarak (`0..1` aralığında) tanımlar.

## State Viewer

Durum Görüntüleyici, durumların içeriğini düzenlemenizi sağlar. Herhangi bir karmaşıklıkta animasyon karıştırma zincirleri oluşturabilirsiniz;

bir durumun en basit içeriği tek bir `Animasyonu Oynat` düğümüdür. Şu anda motor sadece üç animasyon

karıştırma düğümünü desteklemektedir:



- `Animasyonu Oynat` - animasyon pozunu doğrudan belirtilen animasyondan alır, üzerinde hiçbir işlem yapmaz.

- `Blend Animations` - ilgili animasyonlardan birden fazla animasyon pozunu alır ve bunları 

ilgili karıştırma ağırlıklarıyla birleştirir.

- `Blend Animations By Index` - ilgili animasyonlardan birden fazla animasyon pozunu alır ve bunları 

bir indeks parametresi kullanarak “yumuşak” bir geçişle aralarında değiştirir.

![state viewer](./absm_state_viewer.png)

1. `Düğüm` - karıştırma için animasyon kaynağıdır.

2. `Bağlantı` - düğümlerin birbirine nasıl bağlandığını tanımlar. Yeni bir bağlantı oluşturmak için, bir düğümdeki küçük noktaya tıklayın,

 düğmeyi basılı tutun ve başka bir düğümdeki noktaya sürüklemeye başlayın. 

3. `Kök Düğüm` - kök düğüm yeşil renkle işaretlenmiştir; kök düğüm, üst durum için nihai animasyon kaynağıdır. 
### `Animasyonu Oynat` Özellikleri



Aşağıdaki özellikleri düzenlemek için bir `Animasyonu Oynat` düğümü seçin:



![animasyonu oynat özellikleri](./absm_play_animation_properties.png)



- `Konum` - düğümün tuval üzerindeki konumu.

- `Animasyon` - pozun alınacağı animasyon.

### `Blend Animations` Properties

Aşağıdaki özellikleri düzenlemek için bir `Blend Animations` düğümü seçin:

![blend animations properties](./absm_blend_animations_properties.png)

- `Konum` - düğümün tuval üzerindeki konumu.

- `Poz Kaynakları` - bir dizi giriş pozu. Bir poz eklemek için düğümün üzerindeki `+` veya `+Giriş` düğmesine tıklayın. Bazı düğümleri yeni giriş pozlarına bağlamayı unutmayın

.

- `Ağırlık` - pozun ağırlığı; sabit bir değer veya bir parametre olabilir.

### `Dizinle Animasyonları Karıştır` Özellikleri

Aşağıdaki özellikleri düzenlemek için bir `Blend Animations By Index` düğümü seçin:

![blend animations by index properties](./absm_blend_animations_by_index_properties.png) 

- `Konum` - düğümün tuval üzerindeki konumu.

- `Dizin Parametresi` - bir dizin parametresinin adı (`Dizin` türü olmalıdır).

- `Girişler` - bir dizi giriş pozu. Bir poz eklemek için düğümün üzerinde `+` veya `+Giriş` seçeneğine tıklayın. Bazı düğümleri yeni giriş pozlarına bağlamayı unutmayın.

- `Poz` - düğümün kanvas üzerinde konumunu tanımlar.

- `Poz Süresi` - düğümün pozda kalacağı süreyi tanımlar.

### Bağlantı Bağlam Menüsü

Her bağlantının, bağlantıya sağ tıklayarak görüntülenebilen bir bağlam menüsü vardır.

![connection context menu](./absm_connection_context_menu.png)

- `Bağlantıyı Kaldır` - ana düğümler arasındaki bağlantıyı kaldırır.



### Düğüm Bağlam Menüsü



Her düğüm, bağlantıya sağ tıklayarak görüntülenebilen bir bağlam menüsüne sahiptir.

![node context menu](./absm_node_context_menu.png)

- `Kök Olarak Ayarla` - düğümü, üst durumun son poz kaynağı olarak ayarlar. 

- `Kaldır` - düğümü durumdan kaldırır.



## Katman Maskesi

![layer mask](./absm_layer_mask.png)

Katman maskesi düzenleyicisi, mevcut animasyon katmanı tarafından animasyon uygulanmayacak düğümleri seçmenizi sağlar. Seçilen düğümler

koyu renkle işaretlenir. Birden fazla düğümü aynı anda seçmek için `Ctrl` tuşunu basılı tutun ve öğelere tıklayın. Pencerenin üst kısmındaki metin kutusu

belirli bir sahne düğümünü aramanızı sağlar. Düzenlenen katman maskesini kaydetmek için `Tamam` düğmesine tıklayın.

## Preview Mode

Önizleme modu, animasyon karıştırma durum makinesini ve animasyon oynatıcısını etkinleştirir ve makinenin çalışmasının sonucunu görmenizi sağlar

. Sahne içindeki önemli değişiklikler önizleme modunu otomatik olarak devre dışı bırakır ve makine tarafından yapılan tüm

değişiklikler silinir. Önizleme modu etkinken, parametrelerin değerlerini serbestçe değiştirerek 

makinenin buna nasıl tepki vereceğini görebilirsiniz. Bu, durum makinenizi hata ayıklamanıza yardımcı olur ve özellikle 

çok sayıda katman içeren karmaşık durum makineleri için kullanışlıdır. Önizleme modu şu şekilde çalışır:

![absm](./absm.gif)