# Sistem Gereksinimleri



Diğer tüm yazılımlar gibi, Fyrox da en iyi kullanıcı deneyimini sağlamak için kendi sistem gereksinimlerine sahiptir.



- **CPU** - her bir çekirdek için en az 1,5 GHz hızında 2 çekirdekli CPU. Daha fazla çekirdek daha iyidir.

- **GPU** - OpenGL 3.3+ desteğine sahip nispeten modern herhangi bir GPU. Editör başlatılamazsa, büyük olasılıkla

video kartınız OpenGL 3.3+'yı desteklemiyor demektir. Editörü sanal makinelerde çalıştırmaya **çalışmayın**, çünkü bunların hemen hemen hepsi

editörü çalıştırmanıza izin vermeyecek temel grafik API desteğine sahiptir.

- **RAM** - en az 1 Gb RAM. Ne kadar fazla olursa o kadar iyi.

- **VRAM** - en az 256 Mb video belleği. Bu, oyununuzun özelliklerine bağlı olarak büyük ölçüde değişir.

## Supported Platforms

| Platform    | Engine | Editor |
|-------------|--------|--------|
| Windows     | ✅      | ✅      |
| Linux       | ✅      | ✅      |
| macOS       | ✅¹     | ✅      |
| WebAssembly | ✅      | ❌²     |
| Android     | ✅      | ❌²     |
| iOS         | ✅      | ❌²     |

- ✅ - birinci sınıf destek

- ❌ - desteklenmiyor

- ¹ - macOS, Intel yonga setlerinde kötü GPU performansından muzdariptir, M1+ iyi çalışır.

- ² - düzenleyici yalnızca PC'de çalışır, zengin dosya sistemi işlevselliğinin yanı sıra iyi iş parçacığı desteği gerektirir.