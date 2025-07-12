# Temel kavramlar



Motorun bazı temel kavramlarını kısaca gözden geçirelim. Çok fazla değil, ancak hepsi motorun tasarım kararlarını anlamak için çok önemlidir.

## Klasik OOP



Motor, miras yerine bileşim kullanarak biraz klasik OOP kullanır. Motordaki karmaşık nesneler, daha basit nesneler kullanılarak oluşturulabilir.



## Scene



Fyrox'ta, oyununuzu bir dizi yeniden kullanılabilir sahneye ayırırsınız. Neredeyse her şey sahne olabilir: bir oyuncu, bir silah, bir bot, seviye parçaları vb. Sahneler birbirinin içine yerleştirilebilir.


Fyrox'ta, oyununuzu bir dizi yeniden kullanılabilir sahneye böler. Hemen hemen her şey bir sahne olabilir: bir oyuncu, bir silah,

bir bot, seviye parçaları vb. Sahneleri birbirinin içine yerleştirebilirsiniz, bu da karmaşık sahneleri yeniden kullanılabilir

parçalara ayırmanıza yardımcı olur. Fyrox'ta bir sahne aynı zamanda prefab rolünü de oynar, aralarında neredeyse hiç fark yoktur.

## Düğümler ve Scene Graph



Bir sahne, bir veya daha fazla düğümden oluşur (her sahnenin, diğer her şeyin bağlı olduğu en az bir kök düğümü olmalıdır).

Bir sahne düğümü, belirli bir özellik kümesinin yanı sıra, özel

oyun mantığından sorumlu olan _bir_ isteğe bağlı komut dosyası örneği içerir.



Bir sahne düğümünün tipik yapısı aşağıdaki örnekle gösterilebilir. Her sahne düğümünün temel nesnesi 

bir `Base` düğümüdür ve bir dönüşüm, bir alt öğe listesi vb. içerir. `Base` düğümünün işlevselliğini _genişleten_ daha karmaşık bir düğüm
 

içinde bir `Base` örneği, yani bir bileşim saklar. Örneğin, bir `Mesh` düğümü, bir `Base` düğümü _artı_ bazı belirli bilgiler 

(yüzeylerin listesi, malzeme vb.). “Hiyerarşi” derinliği sınırsızdır, örneğin motor içindeki bir `Light` düğümü, üç olası ışık kaynağı türünün bir numaralandırmasıdır

: `Directional`, `Point` ve `Spot`. Bu üç ışık kaynağının tümü bir `BaseLight` düğümü içerir ve

bu düğüm de bir `Base` düğümü içerir. Grafiksel olarak şöyle gösterilebilir:

```text
`Point`
|__ Point Light Properties (radius, etc.)
|__`BaseLight`
   |__ Base Light Properties (color, etc.)
   |__`Base`
      |__ Base Node Properties (transform, children nodes, etc.)
```

Gördüğünüz gibi, bu, nesnenin içeriğini gösteren güzel bir ağaç (grafik) oluşturur. Bu, sahne düğümlerini tanımlamanın çok doğal bir yoludur

ve size herhangi bir karmaşıklıkta nesne oluşturma konusunda tam güç sağlar.



## Plugin



Eklenti, “global” oyun verileri ve mantığı için bir konteynerdir, ana kullanımı, komut dosyalarına bazı veriler sağlamak ve 

global oyun durumunu yönetmektir.


## Komut Dosyaları (Script)



Komut dosyası, sahne düğümlerine eklenebilen ayrı bir veri ve mantık parçasıdır. Bu, özel oyun mantığı eklemenin birincil (ancak tek değil)

yoludur.
