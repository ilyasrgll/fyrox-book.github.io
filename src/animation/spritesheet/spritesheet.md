# Sprite Animasyonu



Sprite'lar, önceden hazırlanmış bir dizi görüntü kullanılarak canlandırılabilir. Performans nedenleriyle, genellikle

dikdörtgen bir dokuya paketlenir ve her bir görüntü, bir ızgaranın kendi hücresinde bulunur. Bu tür dokuya 

sprite sayfası denir ve şuna benzer:



![sprite sayfası örneği](sprite_sheet_example.png)



Gördüğünüz gibi, her animasyon için (boşta, koşma, kılıç sallama vb.) birden fazla kare tek bir 

görüntüye paketlenmiştir. Bir animasyonu oynatmak için tek yapmamız gereken, kareleri istediğimiz sıklıkta değiştirmek ve...

hepsi bu kadar. Bu, hayal edilebilecek en basit animasyon tekniğidir.



Sprite sheet'ler genellikle programcılar tarafından değil, sanatçılar tarafından yapılır, bu yüzden internette sprite sheet arayın veya 

bir sanatçıdan yeni bir tane sipariş edin. Programcıların sanatları neredeyse her zaman kötüdür.



## Nasıl kullanılır



![sprite animation editor](sprite_animation_editor.png)



Fyrox, kendi editörüne sahip yerleşik bir sprite animasyon sistemi sunar. Sprite animasyonunu kullanabilmek için

tek yapmanız gereken, komut dosyanıza bir (veya birkaç) `SpriteSheetAnimation` alanı eklemek ve 

`on_update` alanına aşağıdaki kodu yazmaktır:



```rust

{{#include ../../code/snippets/src/animation/mod.rs:animation}}

```