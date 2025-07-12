# Sahne ve Sahne Grafiği

Bir oyun oynarken, ekranın her yerinde çeşitli nesneler görürsünüz. Bunların hepsi bir
_sahne_ oluşturur. Bir sahne, diğer birçok oyun motorunda olduğu gibi, çeşitli nesnelerin bir kümesidir. Fyrox, sahneleri birden fazla amaç için oluşturmanıza olanak tanır.
sahne oluşturabilirsiniz. Örneğin, bir sahne menü için, bir dizi sahne oyun seviyeleri için
ve bir sahne de son ekran için kullanılabilir. Sahne, diğer sahneler için veri kaynağı oluşturmak için de kullanılabilir. Bu tür sahnelere
_prefabs_ denir. Sahne, diğer sahnelerde kullanılabilecek bir doku olarak da işlenebilir. Bu şekilde,
 diğer yerleri gösteren etkileşimli ekranlar oluşturabilirsiniz.


Oyun oynarken, bazı nesnelerin diğer nesnelere bağlıymış gibi davrandığını fark etmiş olabilirsiniz. Örneğin,
bir rol yapma oyunundaki karakter bir kılıç taşıyabilir. Karakter kılıcı tutarken, kılıç onun
koluna bağlıdır. Nesneler arasındaki bu tür ilişkiler bir grafik yapısı ile gösterilebilir.


Basitçe söylemek gerekirse, grafik, her nesne arasında hiyerarşik ilişkiler bulunan bir nesne kümesidir. Grafikteki her nesneye
düğüm denir. Kılıç ve karakter örneğinde, kılıç karakterin alt düğümüdür
ve karakter kılıcın üst düğümüdür (burada gerçekte karakter
modellerinin genellikle karmaşık iskeletler içerdiği ve kılıcın aslında karakterin değil, ellerin kemiklerinden birine bağlı olduğu gerçeğini göz ardı ediyoruz).


Düzenleyicide, `World Viewer`'da basit bir sürükle ve bırak işlevi kullanarak düğümlerin hiyerarşisini değiştirebilirsiniz - bir düğümü başka bir düğümün üzerine sürükleyin,
 düğüm kendini ona bağlayacaktır.

## Yapı Taşları veya Sahne Düğümleri

Motor, sahneniz için çeşitli türlerde “yapı taşları” sunar. Bu yapı taşlarının her birine _sahne düğümü_ adı verilir.

- [Base](../scene/base_node.md) - hiyerarşik bilgileri (üst düğüme ve alt düğümlere ait tanıtıcılar),
 yerel ve genel dönüşümleri, adı, etiketini, ömrünü vb. depolar. Kendini tanımlayan bir ada sahiptir - kompozisyon yoluyla diğer tüm sahne düğümleri için temel düğüm olarak kullanılır.


- [Mesh](../scene/mesh_node.md) - 3D modeli temsil eder. Bu, hemen hemen her oyunda en sık kullanılan düğümlerden biridir.

 Mesh'ler programlı olarak kolayca oluşturulabilir veya Blender gibi bazı 3D modelleme yazılımlarında oluşturulabilir

 ve ardından sahneye yüklenebilir.


- [Light](../scene/light_node.md) - represents a light source. There are three types of light sources:
    - **Point** - emits light in every direction. A real-world example would be a light bulb.
    - **Spot** - emits light in a particular direction, with a cone-like shape. A real-world example would be a flashlight.
    - **Directional** - emits light in a particular direction, but does not have position. The closest real-world example would be the Sun.


- [Camera](../scene/camera_node.md) - allows you to see the world. You must have at least one camera in your scene to be able to see anything.


- [Sprite](../scene/sprite_node.md) - represents a quad that always faces towards a camera. It can have a texture and size and can also can be rotated around the "look" axis.


- [Particle system](../scene/particle_system_node.md) - allows you to create visual effects using a huge set of small particles. It
  can be used to create smoke, sparks, blood splatters, etc.


- [Terrain](../scene/terrain_node.md) - allows you to create complex landscapes with minimal effort.


- [Decal](../scene/decal_node.md) - paints on other nodes using a texture. It is used to simulate cracks in concrete walls,
  damaged parts of the road, blood splatters, bullet holes, etc.


- [Rigid Body](../physics/rigid_body.md) - a physical entity that is responsible for the dynamic of the rigid. There is a special 
variant for 2D - `RigidBody2D`.


- [Collider](../physics/collider.md) - a physical shape for a rigid body. It is responsible for contact manifold generation, 
without it, any rigid body will not participate in simulation correctly, so every rigid body must have at least
one collider. There is a special variant for 2D - `Collider2D`.


- [Joint](../physics/joint.md) - a physical entity that restricts motion between two rigid bodies. It has various amounts
of degrees of freedom depending on the type of the joint. There is a special variant for 2D - `Joint2D`.


- [Rectangle](../scene/rectangle.md) - a simple rectangle mesh that can have a texture and a color. It is a very simple version of 
a Mesh node, yet it uses very optimized renderer, that allows you to render dozens of rectangles simultaneously.
This node is intended for use in **2D games** only.


- [Sound](../sound/sound.md) - a sound source universal for 2D and 3D. Spatial blend factor allows you to select
a proportion between 2D and 3D.
- Listener - an audio receiver that captures the sound at a particular point in your scene and sends it to an audio
context for processing and outputting to an audio playback device.
- Animation Player - a container for multiple animations. It can play animations made in the 


[animation editor](../animation/anim_editor.md) and apply animation poses to respective scene nodes.
- Animation Blending State Machine - a state machine that mixes multiple animations from multiple states into one; each
state is backed by one or more animation playing or blending nodes. See its [respective chapter](../animation/absm_editor.md) 
for more info.

Every node can be created either in the editor (through `Create` on the main menu, or through `Add Child` after right-clicking on 
a game entity) or programmatically via their respective node builder (see [API docs](https://docs.rs/fyrox/latest/fyrox/scene/index.html) 
for more info). These scene nodes allow you to build almost any kind of game. It is also possible to create your own 
types of nodes, but that is an advanced topic, which is covered in a [future chapter](../scene/custom_node.md).

## Yerel ve Küresel Koordinatlar

Bir grafik, sahnenizi çok doğal bir şekilde tanımlar ve _scene nodes_ ile çalışırken göreceli ve mutlak koordinatlar

cinsinden düşünmenizi sağlar.



Bir sahne düğümü iki tür dönüşüme sahiptir: yerel ve global. Yerel dönüşüm, düğümün kökenine göre konumunu,

 yüzdesel ölçeğini ve herhangi bir eksen etrafındaki dönüşünü tanımlar.

Global dönüşüm neredeyse aynıdır, ancak ebeveyn düğümlerin tüm dönüşüm zincirini de içerir. 

Karakter ve kılıç örneğine geri dönersek, karakter hareket ederse ve dolayısıyla kılıç da hareket ederse, kılıcın global 


dönüşümü karakterin konumunda yapılan değişiklikleri yansıtır, ancak yerel dönüşümü yansıtmaz, çünkü
 
kılıcın konumunu karakterin konumuna göre temsil eder ve karakterin konumu değişmemiştir.



Bu mekanizma çok basit, ancak çok güçlüdür. Bu mekanizmanın tüm güzelliği, iskeletli 3B modellerle çalışırken ortaya çıkar.

 İskeletin her kemiğinin bir üst öğesi ve bir dizi alt öğesi vardır, bu da bunları döndürmenize, kaydırmanıza veya ölçeklendirmenize olanak tanır
animate your entire character.
