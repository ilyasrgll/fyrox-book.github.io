# Sıkça Sorulan Sorular



Bu bölümde sıkça sorulan soruların yanıtları yer almaktadır.



## Motor hangi grafik API'sini kullanıyor?


Fyrox, PC'de OpenGL 3.3 ve WebAssembly'de OpenGL ES 3.0 kullanıyor. Neden? Esas olarak tarihsel nedenlerden dolayı. Eskiden


(2018'in 4. çeyreği), geniş bir platform yelpazesini destekleyen iyi bir alternatif yoktu. Örneğin, `wgpu`
 
[henüz mevcut bile değildi](https://crates.io/crates/wgpu/0.1.0), çünkü ilk sürümü Ocak 2019'da yayınlandı. Diğer kütüphaneler ise ilk adımlarını atıyor 

ve üretime hazır değildi.

### Neden şimdi alternatifleri kullanmıyorsunuz?

Buna gerek yoktur. Mevcut uygulama çalışıyor ve fazlasıyla yeterlidir. Bu nedenle, çok az fayda sağlayan bir şeyi değiştirmek yerine, mevcut odak noktası eksik özelliklerin eklenmesi ve gerektiğinde mevcut özelliklerin iyileştirilmesidir.

## Motor ECS tabanlı mı?



Hayır, motor, belirli bir göreve en uygun olan mesaj aktarımı ve diğer farklı yaklaşımları içeren karışık bileşim tabanlı, nesne yönelimli bir tasarım kullanır.

 Peki neden her şey için ECS kullanmıyorsunuz? Pragmatizm. İş için doğru aracı kullanın.

Çivi çakmak için mikroskop kullanmayın.

## Fyrox ile ne tür oyunlar yapabilirim?



Geniş açık dünyalı oyunlar hariç (çünkü yerleşik dünya akışı yoktur), hemen hemen her tür oyun yapabilirsiniz.

Genel olarak, oyun geliştirme deneyiminize bağlıdır.
