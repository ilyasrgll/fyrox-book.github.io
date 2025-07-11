# Editör, Eklentiler ve Komut Dosyaları

Her Fyrox oyunu, hem motor hem de editör için bir eklentidir. Bu yaklaşım, oyunun editöründen çalıştırılmasını ve oyun öğelerinin içinden düzenlenmesini sağlar. Bir oyun, sahne nesnelerine atanarak üzerlerinde özel oyun mantığı çalıştırmak için herhangi bir sayıda komut dosyası tanımlayabilir. Bu bölümde, motorun platformuna özgü bağımlılıklarıyla birlikte nasıl kurulacağı, eklentilerin ve komut dosyası sisteminin nasıl kullanılacağı ve editörün nasıl çalıştırılacağı anlatılacaktır.

## Platforma özgü bağımlılıklar

Motoru kullanmaya başlamadan önce, gerekli tüm platforma özgü geliştirme bağımlılıklarının yüklü olduğundan emin olun. 

Windows veya macOS kullanıyorsanız, platformunuz için uygun araç zinciri ile [en son Rust sürümü](https://rustup.rs)

dışında ek bir bağımlılık gerekmez.

### Linux

Linux'ta, Fyrox'un geliştirilmesi için aşağıdaki kütüphaneler gereklidir: `libxcb-shape0`, `libxcb-xfixes0`, `libxcb1`, `libxkbcommon`, `libasound2`, `libegl-mesa0` ve `build-essential` paket grubu.

Ubuntu gibi Debian tabanlı dağıtımlarda, aşağıdaki gibi kurulabilir:

```shell
sudo apt install libxcb-shape0-dev libxcb-xfixes0-dev libxcb1-dev libxkbcommon-dev libasound2-dev libegl-mesa0 build-essential
```

NixOS için, deponuzun kök dizinine `flake.nix` adlı bir dosya ekleyin ve içine aşağıdaki içeriği yazın, dosyayı git dizinine ekleyin (örneğin, `git add flake.nix` komutuyla) ve ardından `nix develop` komutunu çalıştırarak gerekli tüm bağımlılıkların bulunduğu bir kabuk açın.

```nix
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    rust-overlay = {
      url = "github:oxalica/rust-overlay";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = {
    nixpkgs,
    rust-overlay,
    ...
  }:
  let
    overlays = [
      (import rust-overlay)
    ];

    systems = [
      "x86_64-linux"
      "aarch64-linux"
    ];

    forAllSystems = f:
      nixpkgs.lib.genAttrs systems
      (system: f { pkgs = import nixpkgs { inherit system overlays; }; });
  in
  {
    devShells = forAllSystems ({ pkgs }: with pkgs; {
      default = mkShell rec {
        buildInputs = [
          rust-bin.stable.latest.default

          pkg-config
          xorg.libxcb
          alsa-lib
          wayland
          libxkbcommon
          libGL
        ];
        LD_LIBRARY_PATH = "${lib.makeLibraryPath buildInputs}";
      };
    });
  };
}
```

## Proje Yöneticisi

![proje yöneticisi](https://fyrox.rs/assets/0.36/project_manager.png)

Proje yöneticisi, motorla oluşturulan birden fazla projeyi aynı anda yönetmenizi sağlayan motorun bir parçasıdır.
Yeni bir proje oluşturmanıza veya mevcut bir projeyi içe aktarmanıza, projeyi çalıştırmanıza veya düzenleyicide düzenlemenize, projeyi seçilen motor sürümüne yükseltmenize ve daha birçok seçeneğe sahip olursunuz.

[Proje yöneticisini indirin](https://fyrox.rs/download.html) işletim sisteminiz için indirin ve çalıştırın. Ardından `+create` düğmesine tıklayın, projenin yer almasını istediğiniz yolu seçin ve `create` düğmesine tıklayın. Listeden yeni projeyi seçin ve düzenleyiciyi çalıştırmak için `edit` düğmesine tıklayın.

## Konsol Komutlarını Kullanarak Hızlı Başlangıç

Editörü mümkün olan en kısa sürede kullanmaya başlamak için aşağıdaki komutları çalıştırın.

```sh
cargo install fyrox-template
fyrox-template init --name fyrox_test --style 2d
cd fyrox_test
cargo run --package editor --release
```

## Proje Oluşturucu

> ⚠️ Bu bölüm çoğunlukla konsol kullanıcıları ve yazılımlarını kaynak kodundan derlemeyi sevenler içindir.
> Project Manager'ı kullanmayı tercih ederseniz, `fyrox-template` ile aynı işlevi görür, ancak GUI'nin avantajlarından da yararlanabilirsiniz.

Fyrox eklentileri Rust ile yazılmıştır, bu da oyunun kaynak kodu değişirse yeniden derlemeniz gerekeceği anlamına gelir.
Mimarisi bazı standart kodlar gerektirir. Fyrox, özel bir küçük komut satırı aracı sunar: `fyrox-template`. Bu, tek bir komutla tüm standart kodları oluşturmanıza yardımcı olur. Aşağıdaki komutu çalıştırarak yükleyin:

```shell
cargo install fyrox-template
```

_Linux için not:_ Bu, programı `$user/.cargo/bin` dizinine yükler. `fyrox-template` komutunun bulunamadığına dair hata mesajları alırsanız  ,
bu gizli cargo bin klasörünü işletim sisteminin `$PATH` ortam değişkenine ekleyin.



Şimdi, istediğiniz proje klasörüne gidin ve aşağıdaki komutu çalıştırın:

```shell
fyrox-template init --name my_game --style 3d
```

`cargo init` komutundan farklı olarak, bu komut verilen adla yeni bir klasör oluşturur.



Araç iki argüman kabul eder: bir proje adı (`--name`) ve bir stil (`--style`), bu stil varsayılan sahnenin içeriğini tanımlar. Projeyi başlattıktan sonra, `game/src/lib.rs` dosyasına gidin. Burası oyun mantığının bulunduğu yerdir. 

Gördüğünüz gibi, `fyrox-template` sizin için oldukça fazla kod üretir. Kod, her yerin ne işe yaradığını açıklayan yorumlarla süslenmiştir. 

Her yöntem hakkında daha fazla bilgi için lütfen [belgelere](https://docs.rs/fyrox/latest/fyrox/plugin/trait.Plugin.html) bakın..

Proje oluşturulduktan sonra, oyununuzu farklı modlarda çalıştırmak için iki komut kullanılabilir:



- `cargo run --package editor --release` - oyununuzun eklendiği düzenleyiciyi başlatır. Düzenleyici, oyununuzu

  buradan çalıştırmanıza ve oyun öğelerini düzenlemenize olanak tanır. Bu komut yalnızca geliştirme amaçlı kullanılmak üzere tasarlanmıştır.

- `cargo run --package executor --release` - oyununuzun üretim ikili dosyasını oluşturur ve çalıştırır. Bu komut, gönderilebilecek (örneğin bir mağazaya) yürütülebilir dosyalar oluşturur.



Projenizin dizinine gidin ve `cargo run --package editor --release` komutunu çalıştırın. Kısa bir süre sonra 

![editor](editor.png)

Editörde oyun sahnenizi oluşturmaya başlayabilirsiniz. 

**Önemli not:** sahnenizde en az bir kamera olmalıdır,

aksi takdirde hiçbir şey göremezsiniz. Editörün nasıl kullanıldığını öğrenmek için bir sonraki bölümü okuyun.

## En Son Motor Sürümünü Kullanma

Yazılım geliştirmenin doğası gereği, büyük sürümlerde kaçınılmaz olarak hatalar ortaya çıkacaktır. Bu nedenle, 

GitHub'daki depodan her zaman en son motor sürümünü kullanmanız önerilir. Bu sürümlerde hatalar büyük olasılıkla düzeltilmiş olacaktır

(bulduğunuz hataları düzelterek veya en azından [bir sorun bildirerek](https://github.com/FyroxEngine/Fyrox/issues) katkıda bulunabilirsiniz).

### Otomatik

> ⚠️ `fyrox-template`, istenen motor sürümüne hızlı bir şekilde yükseltmek için özel bir alt komut olan `upgrade` komutuna sahiptir. 
> En son sürüme (`nightly`) yükseltmek için, oyununuzun 
> dizininde `fyrox-template upgrade --version nightly` komutunu çalıştırmalısınız.

`--version` anahtarının üç ana varyantı vardır:

- `nightly` - GitHub'dan doğrudan motorun en son gece sürümünü kullanır. En son değişiklikleri ve hata düzeltmelerini yayınlandıkları anda kullanmak istiyorsanız bu sürümü tercih etmelisiniz
.

- `latest` - motorun en son kararlı sürümünü kullanır. Bu seçenek, motorun yolunu `../Fyrox/fyrox` ve editörün yolunu `../Fyrox/editor` olarak ayarlayan `--local` anahtarını da destekler. Açıkçası, bu yol motorun projenizin üst dizininde bulunmasını gerektirir. Bu seçenek, motorun projenizin üst dizininde bulunmasını gere

`../Fyrox/fyrox` ve editörün yolunu `../Fyrox/editor` olarak ayarlar. Açıkçası, bu yolun motorun projenizin ana dizininde bulunmasını gerektirir.

 Bu seçenek, motorun özel bir sürümünü kullanmak istiyorsanız (örneğin, motor için bir yama geliştiriyorsanız) yararlı olabilir.

- `major.minor.patch` - crates.io'dan belirli bir kararlı sürümü kullanır (örneğin `0.30.0`).

### Manual

Motor sürümü manuel olarak da güncellenebilir. İlk adım, en son `fyrox-template` sürümünü yüklemektir. Bu,
tek bir `cargo` komutuyla yapılabilir:

```shell
cargo install fyrox-template --force --git https://github.com/FyroxEngine/Fyrox
```

Bu, en son proje/komut dosyası şablon oluşturucuyu kullandığınızdan emin olmanızı sağlar, bu çok önemlidir; eski sürümler
şablon oluşturucunun büyük olasılıkla artık motorla uyumlu olmayan eski kodlar oluşturacaktır.
Mevcut projeleri motorun en son sürümüne geçirmek için, `fyrox` ve`fyroxed_base` bağımlılıkları için uzak depoya işaret eden yollar belirtmeniz gerekecektir. 
Tek yapmanız gereken, kök `Cargo.toml` dosyasındaki bu bağımlılıkların yollarını değiştirmektir:

```toml
[workspace.dependencies.fyrox]
version = { git = "https://github.com/FyroxEngine/Fyrox" }
default-features = false
[workspace.dependencies.fyroxed_base]
version = { git = "https://github.com/FyroxEngine/Fyrox" }
```

Artık oyununuz en son motoru ve editörü kullanacak, ancak dikkatli olun - yeni taahhütler bazı API uyumsuzlukları ortaya çıkarabilir. Bunları, belirli bir taahhüt belirleyerek önleyebilirsiniz. Bunun için, her bağımlılığa şu şekilde `rev = "desired_commit_hash` ekleyin:

```toml
[dependencies]
[workspace.dependencies.fyrox]
version = { git = "https://github.com/FyroxEngine/Fyrox", rev = "0195666b30562c1961a9808be38b5e5715da43af" }
default-features = false
[workspace.dependencies.fyroxed_base]
version = { git = "https://github.com/FyroxEngine/Fyrox", rev = "0195666b30562c1961a9808be38b5e5715da43af" }
```

Motorun yerel git deposunu en son sürüme getirmek için, projenin

çalışma alanının kök dizininde `cargo update` komutunu çalıştırın. Bu, `rev` belirtilmediği sürece, uzak sunucudan en son değişiklikleri çekecektir.



Bağımlılık yolları hakkında daha fazla bilgiyi resmi `cargo` belgelerinde [burada](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#specifying-dependencies-from-git-repositories) bulabilirsiniz.

## Oyun Mantığı Ekleme

Nesneye özgü oyun mantığı, komut dosyaları kullanılarak eklenmelidir. Komut dosyası, veriler ve kodlar için bir “kap”tır ve

motor tarafından yürütülür. Oyununuzda komut dosyalarını nasıl oluşturacağınızı, düzenleyeceğinizi ve kullanacağınızı öğrenmek için [Komut Dosyaları](../scripting/script.md) bölümünü okuyun

.
