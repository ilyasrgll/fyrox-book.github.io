# Sinyaller



Bazı durumlarda, animasyonunuzun belirli bir anında bir eylem gerçekleştirmeniz gerekebilir. Bu, ayak sesi,

ayak yere değdiğinde, el bombası atma vb. olabilir. Bu, animasyon sinyalleri aracılığıyla yapılabilir. Animasyon sinyali, 

animasyon zaman çizelgesinde zaman konumu olan, adlandırılmış bir işaretçidir. Animasyonun oynatılma süresi geçtiğinde

(animasyonun gerçek hızına bağlı olarak soldan sağa veya sağdan sola). Tek yapmanız gereken,

oyun kodunuzda bu sinyalleri yakalamak ve istenen eylemleri gerçekleştirmektir.



## Nasıl eklenir



Her zamanki gibi, animasyon sinyallerini eklemenin iki yolu vardır: animasyon düzenleyicisinden ve koddan.

### Animasyon düzenleyiciden



Bir animasyona sinyal eklemek için, bir animasyon oynatıcı seçin, animasyon düzenleyiciyi açın ve içinde bir animasyon seçin.

 Şimdi tek yapmanız gereken zaman çizelgesine sağ tıklayıp `Sinyal Ekle`'ye basmak.

![Add Signal](signal_add.png)

Sinyal eklendikten sonra, onu seçip denetçide özelliklerini düzenleyebilirsiniz. Ayrıca, konumunu ayarlamak için zaman çizelgesine sürükleyebilirsiniz. 

![Edit Signal](signal_edit.png)

Sinyale anlamlı bir isim verin, hepsi bu kadar - geriye kalan tek şey, oyununuzda sinyal işleme

kodunu yazmak. Bunun nasıl yapılacağını öğrenmek için [sonraki bölüme](#reacting-to-signal-events) bakın.

### Koddan



Bir sinyal koddan da eklenebilir, bunun için animasyon oynatıcınızın tanıtıcısını ve animasyonunuzun adını/tanıtıcısını bilmeniz gerekir.

 Aşağıdaki kodda sinyalin uuid'si ile ilgili açıklamaya dikkat edin.

```rust,no_run
{{#include ../code/snippets/src/animation/signal.rs:add_signal}}
```

## Sinyal olaylarına tepki verme



Sinyalleriniz kullanıma hazır olduğunda, tek yapmanız gereken sinyallere bir şekilde tepki vermektir. Bu çok basittir:

animasyon oynatıcısından animasyonunuzu ödünç alın ve dahili kuyruktan animasyon olaylarını tek tek açın:

```rust,no_run
{{#include ../code/snippets/src/animation/signal.rs:react_to_signal_events}}
```

Sinyallere tepki verirken hemen hemen her şeyi yapabilirsiniz. Örneğin, bu, ayakların altında duman efekti oluşturmak, ayak sesi çalmak vb. için önceden hazırlanmış bir örnektir.

## ABSM'den olaylar



Animasyon karıştırma durum makineleri, 

farklı stratejiler kullanarak o anda oynatılan animasyonlardan olayları toplayabilir. Bu özellik, bir dizi animasyondan sıkıcı manuel animasyon olayları toplama işleminden sizi kurtarır

.

```rust ,no_run
{{#include ../code/snippets/src/animation/signal.rs:collect_events_from_absm}}
```

Bu işlev, belirtilen ABSM'deki (ilk

katmanında) tüm aktif animasyonlardan tüm animasyon olaylarını toplar. İşlevin argümanları şunlardır:



- `absm` - bir animasyon karıştırma durum makinesi düğümüne bir tanıtıcı.

- `strategy` - tüm olayların toplanmasını, maksimum ve minimum ağırlığı içeren olay toplama stratejisi. 

Son ikisi, çok sayıda olay alıyorsanız ve sırasıyla maksimum veya minimum ağırlıklı animasyonlardan olay almak istiyorsanız kullanılabilir.

- `ctx` - mevcut komut dosyası bağlamı, hemen hemen tüm komut dosyası yöntemlerinde kullanılabilir.