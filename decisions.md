<!--* toc_depth: 3 *-->

# Go Stil Kararları

decisions

[Genel Bakış](index) | [Kılavuz](guide) | [Kararlar](decisions) |
[En İyi Uygulamalar](best-practices)

<!--

-->

**Not:** Bu belge, Google'daki [Go Stili](index)'ni özetleyen belge serisinin bir
parçasıdır. Bu belge **[normatif](index#normative) ancak
[kanonik](index#canonical) değildir** ve [ana stil kılavuzu](guide)'na tabidir.
Daha fazla bilgi için [genel bakış](index#about) sayfasına bakın.

<a id="about"></a>

## Hakkında

Bu belge, Go okunabilirlik mentorlarının verdiği tavsiyeleri birleştirmek ve
standart yönlendirmeler, açıklamalar ve örnekler sunmak amacıyla hazırlanmış
stil kararlarını içerir.

Bu belge **kapsamlı değildir** ve zamanla büyüyecektir.
[Ana stil kılavuzu](guide) ile burada verilen tavsiyeler çeliştiğinde,
**stil kılavuzu önceliklidir** ve bu belge buna göre güncellenmelidir.

Tüm Go Stil belgeleri seti için
[Genel Bakış](https://google.github.io/styleguide/go#about) sayfasına bakın.

Aşağıdaki bölümler stil kararlarından kılavuzun başka bir bölümüne taşınmıştır:

- **MixedCaps**: [guide#mixed-caps](guide#mixed-caps) sayfasına bakın
  <a id="mixed-caps"></a>

- **Formatting**: [guide#formatting](guide#formatting) sayfasına bakın
  <a id="formatting"></a>

- **Line Length**: [guide#line-length](guide#line-length) sayfasına bakın
  <a id="line-length"></a>

<a id="naming"></a>

## Adlandırma

Genel adlandırma yönergeleri için [ana stil kılavuzu](guide#naming)'ndaki
adlandırma bölümüne bakın. Aşağıdaki bölümler adlandırma içindeki belirli
alanlara ilişkin ek açıklamalar sunmaktadır.

<a id="underscores"></a>

### Alt çizgiler

Go'da isimler genel olarak alt çizgi içermemelidir. Bu ilke için üç istisna
bulunmaktadır:

1.  Yalnızca üretilmiş kod tarafından import edilen paket isimleri alt çizgi
    içerebilir. Çok kelimeli paket isimlerini nasıl seçeceğinize ilişkin
    ayrıntılar için [paket isimleri](#package-names) bölümüne bakın.
1.  `*_test.go` dosyalarındaki test, benchmark ve example fonksiyon isimleri
    alt çizgi içerebilir.
1.  İşletim sistemi veya cgo ile etkileşime giren düşük seviyeli kütüphaneler,
    [`syscall`]'da yapıldığı gibi tanımlayıcıları yeniden kullanabilir. Bu,
    çoğu kod tabanında çok nadir görülen bir durumdur.

**Not:** Kaynak kod dosya isimleri Go tanımlayıcıları değildir ve bu
kurallara uymaları gerekmez. Alt çizgi içerebilirler.

[`syscall`]: https://pkg.go.dev/syscall#pkg-constants

<a id="package-names"></a>

### Paket isimleri

<a id="TOC-PackageNames"></a>

Go'da paket isimleri kısa olmalı ve yalnızca küçük harfler ve rakamlar
kullanmalıdır (ör. [`k8s`], [`oauth2`]). Çok kelimeli paket isimleri
kesintisiz ve tamamen küçük harflerle yazılmalıdır (ör. [`tabwriter`]
`tabWriter`, `TabWriter` veya `tab_writer` yerine).

Sık kullanılan yerel değişken isimleri tarafından [gölgede bırakılma]
ihtimali yüksek olan paket isimleri seçmekten kaçının. Örneğin, `count`
sık kullanılan bir değişken ismi olduğundan, `usercount` `count`'tan daha
iyi bir paket ismidir.

Go paket isimlerinde alt çizgi olmamalıdır. Adında alt çizgi olan bir
paketi import etmeniz gerekiyorsa (genellikle üretilmiş veya üçüncü parti
koddan), import sırasında Go kodunda kullanıma uygun bir isme
yeniden adlandırılmalıdır.

Buna bir istisna olarak, yalnızca üretilmiş kod tarafından import edilen
paket isimleri alt çizgi içerebilir. Belirli örnekler:

- Bir paketin yalnızca dışa açık API'sini test eden birim testleri için
  `_test` ekini kullanma (package `testing` bunları
  ["kutu dışı testler"](https://pkg.go.dev/testing) olarak adlandırır).
  Örneğin, `linkedlist` paketinin kutu dışı birim testleri `linkedlist_test`
  adlı bir pakette tanımlanmalıdır (`linked_list_test` değil)

- Fonksiyonel veya entegrasyon testlerini belirten paketler için alt
  çizgiler ve `_test` ekini kullanma. Örneğin, bir linked list servis
  entegrasyon testi `linked_list_service_test` olarak adlandırılabilir

- [Paket seviyesi dokümantasyon örnekleri](https://go.dev/blog/examples)
  için `_test` ekini kullanma

[`tabwriter`]: https://pkg.go.dev/text/tabwriter
[`k8s`]: https://pkg.go.dev/k8s.io/client-go/kubernetes
[`oauth2`]: https://pkg.go.dev/golang.org/x/oauth2
[shadowed]: best-practices#shadowing

`util`, `utility`, `common`, `helper`, `model`, `testhelper` gibi
paket kullanıcılarını [import sırasında yeniden adlandırmaya]
cesaretlendirecek anlamsız paket isimlerinden kaçının:

- [Sözde "yardımcı paketler" hakkındaki rehberlik](best-practices#util-packages)
- [Go İpucu #97: İsimde Ne Var](index#gotip)
- [Go İpucu #108: İyi Bir Paket Adının Gücü](index#gotip)

Bir import edilen paket yeniden adlandırıldığında (ör. `import foopb
"path/to/foo_go_proto"`), paketin yerel adı yukarıdaki kurallara uymalıdır,
çünkü yerel ad dosyadaki sembollerin nasıl referans verileceğini belirler.
Belirli bir import birden fazla dosyada, özellikle aynı veya yakın paketlerde
yeniden adlandırılıyorsa, tutarlılık için mümkünse aynı yerel ad kullanılmalıdır.

<!--#include file="/go/g3doc/style/includes/special-name-exception.md"-->

Ayrıca bakın: [Paket isimleri hakkında Go blog yazısı](https://go.dev/blog/package-names).

<a id="receiver-names"></a>

### Alıcı isimleri

<a id="TOC-ReceiverNames"></a>

[Alıcı] değişken isimleri şu kurallara uymalıdır:

- Kısa olmalı (genellikle bir veya iki harf uzunluğunda)
- Türün kendisi için kısaltmalar olmalı
- O tür için her alıcıya tutarlı olarak uygulanmalı
- Alt çizgi olmamalı; kullanılmıyorsa isim atlanmalıdır

| Uzun İsim                   | Daha İyi İsim             |
| --------------------------- | ------------------------- |
| `func (tray Tray)`          | `func (t Tray)`           |
| `func (info *ResearchInfo)` | `func (ri *ResearchInfo)` |
| `func (this *ReportWriter)` | `func (w *ReportWriter)`  |
| `func (self *Scanner)`      | `func (s *Scanner)`       |

[Receiver]: https://golang.org/ref/spec#Method_declarations

<a id="constant-names"></a>

### Sabit isimleri

Sabit isimleri, Go'daki diğer tüm isimler gibi [MixedCaps] kullanmalıdır.
([Dışa açık][Exported] sabitler büyük harfle başlar, dışa açık olmayan
sabitler küçük harfle başlar.) Bu, başka dillerdeki kuralları bozsa da
geçerlidir. Sabit isimleri değerlerinin türevi olmamalı, bunun yerine
değerin ne anlama geldiğini açıklamalıdır.

```go
// İyi:
const MaxPacketSize = 512

const (
    ExecuteBit = 1 << iota
    WriteBit
    ReadBit
)
```

[MixedCaps]: guide#mixed-caps
[Exported]: https://tour.golang.org/basics/3

MixedCaps olmayan sabit isimleri veya `K` önekli sabitler kullanmayın.

```go
// Kötü:
const MAX_PACKET_SIZE = 512
const kMaxBufferSize = 1024
const KMaxUsersPergroup = 500
```

Sabitleri değerlerine göre değil, rollerine göre adlandırın. Bir sabit
değerinden başka bir rolü yoksa, sabit olarak tanımlanması gereksizdir.

```go
// Kötü:
const Twelve = 12

const (
    UserNameColumn = "username"
    GroupColumn    = "group"
)
```

<!--#include file="/go/g3doc/style/includes/special-name-exception.md"-->

<a id="initialisms"></a>

### Kısaltmalar

<a id="TOC-Initialisms"></a>

İsimlerdeki kısaltma veya akronim kelimeler (ör. `URL` ve `NATO`) aynı
büyük/küçük harf durumuna sahip olmalıdır. `URL`, `URL` veya `url` olarak
görünmeli (`urlPony` veya `URLPony` gibi), asla `Url` olarak değil. Genel
kural olarak, tanımlayıcılar (ör. `ID` ve `DB`) İngilizce düz metindeki
kullanımlarına benzer şekilde büyük harfle yazılmalıdır.

- Birden fazla kısaltma içeren isimlerde (ör. `XMLAPI` çünkü `XML` ve `API`
  içerir), verilen bir kısaltma içindeki her harf aynı durumda olmalıdır, ancak
  isimdeki her kısaltmanın aynı durumda olması gerekmez.
- Küçük harf içeren bir kısaltma içeren isimlerde (ör. `DDoS`, `iOS`, `gRPC`),
  kısaltma standart metinde göründüğü gibi görünmelidir, [dışa açıklık] nedeniyle
  ilk harfi değiştirmeniz gerekmediği sürece. Bu durumlarda, tüm kısaltma aynı
  durumda olmalıdır (ör. `ddos`, `IOS`, `GRPC`).

[exportedness]: https://golang.org/ref/spec#Exported_identifiers

<!-- Keep this table narrow. If it must grow wider, replace with a list. -->

| İngilizce Kullanım | Kapsam      | Doğru    | Yanlış                                 |
| ------------------ | ----------- | -------- | -------------------------------------- |
| XML API            | Dışa açık   | `XMLAPI` | `XmlApi`, `XMLApi`, `XmlAPI`, `XMLapi` |
| XML API            | Dışa kapalı | `xmlAPI` | `xmlapi`, `xmlApi`                     |
| iOS                | Dışa açık   | `IOS`    | `Ios`, `IoS`                           |
| iOS                | Dışa kapalı | `iOS`    | `ios`                                  |
| gRPC               | Dışa açık   | `GRPC`   | `Grpc`                                 |
| gRPC               | Dışa kapalı | `gRPC`   | `grpc`                                 |
| DDoS               | Dışa açık   | `DDoS`   | `DDOS`, `Ddos`                         |
| DDoS               | Dışa kapalı | `ddos`   | `dDoS`, `dDOS`                         |
| ID                 | Dışa açık   | `ID`     | `Id`                                   |
| ID                 | Dışa kapalı | `id`     | `iD`                                   |
| DB                 | Dışa açık   | `DB`     | `Db`                                   |
| DB                 | Dışa kapalı | `db`     | `dB`                                   |
| Txn                | Dışa açık   | `Txn`    | `TXN`                                  |

<!--#include file="/go/g3doc/style/includes/special-name-exception.md"-->

<a id="getters"></a>

### Getter

<a id="TOC-Getters"></a>

Fonksiyon ve metot isimleri `Get` veya `get` öneki kullanmamalıdır, temel
kavram "get" kelimesini kullanmadığı sürece (ör. bir HTTP GET). İsmin
doğrudan isimle başlamasını tercih edin, örneğin `GetCounts` yerine
`Counts` kullanın.

Fonksiyon karmaşık bir hesaplama veya uzak bir çağrı yapmayı içeriyorsa,
`Get` yerine `Compute` veya `Fetch` gibi farklı bir kelime kullanılabilir,
böylece okuyucuya fonksiyon çağrısının zaman alabileceği ve
engellenebileceği veya başarısız olabileceği açıkça belirtilir.

<!--#include file="/go/g3doc/style/includes/special-name-exception.md"-->

<a id="variable-names"></a>

### Değişken isimleri

<a id="TOC-VariableNames"></a>

Genel kural olarak, bir ismin uzunluğu kapsamının büyüklüğü ile orantılı ve o
kapsamda kullanıldığı sayı ile ters orantılı olmalıdır. Dosya kapsaminda
oluşturulan bir değişken birden fazla kelime gerektirebilirken, tek bir iç blok
kapsamındaki bir değişken kodu net tutmak ve gereksiz bilgiden kaçınmak için tek
bir kelime hatta yalnızca bir veya iki karakter olabilir.

Bu yaklaşık bir başlangıçtır. Bu sayısal yönergeler katı kurallar
değildir. Bağlam, [açıklık] ve [kısalık]na dayanarak yargı
kullanın.

- Küçük kapsam, bir veya iki küçük işlemin yapıldığı kapsamdır, örneğin 1-7
  satır.
- Orta kapsam, birkaç küçük veya bir büyük işlemdir, örneğin 8-15 satır.
- Büyük kapsam, bir veya birkaç büyük işlemdir, örneğin 15-25 satır.
- Çok büyük kapsam, bir sayfadan fazla yayılan her şeydir (örneğin, 25 satırdan
  fazla).

[clarity]: guide#clarity
[concision]: guide#concision

Küçük bir kapsamda tamamen net olabilecek bir isim (ör. `c` bir sayaç için),
daha büyük bir kapsamda yetersiz kalabilir ve kodun ilerleyen kısımlarında
amacını okuyucuya hatırlatmak için netleştirme gerektirebilir. Birden fazla
değişkenin veya benzer değerleri ya da kavramları temsil eden değişkenlerin
bulunduğu bir kapsam, kapsamın önerdiğinden daha uzun değişken isimleri
gerektirebilir.

Kavramın özgüllüğü de bir değişkenin ismini kısa tutmaya yardımcı olabilir.
Örneğin, yalnızca tek bir veritabanının kullanıldığını varsayarsak, normalde
çok küçük kapsamlar için ayrılan kısa bir değişken ismi olan `db`, kapsam çok
büyük olsa bile tamamen net kalabilir. Bu durumda, kapsamın büyüklüğüne göre
tek bir kelime olan `database` kabul edilebilir, ancak `db` kelimenin çok yaygın
bir kısaltması ve az sayıda alternatif yorumu olduğundan zorunlu değildir.

Yerel bir değişkenin ismi, mevcut bağlamda nasıl kullanıldığına ve ne
içerdiğine yansımalıdır, değerin nereden geldiğine değil. Örneğin, en iyi yerel
değişken isminin çoğu zaman struct veya protocol buffer alan adıyla aynı
olmadığı durumlar sıkça görülür.

Genel olarak:

- `count` veya `options` gibi tek kelimeli isimler iyi bir başlangıç
  noktasıdır.
- Benzer isimleri ayırt etmek için ek kelimeler eklenebilir, örneğin
  `userCount` ve `projectCount`.
- Yazma kolaylığı için harfleri basitçe atmayın. Örneğin, dışa açık isimler
  için özellikle `Sandbox`, `Sbx`'den tercih edilir.
- Çoğu değişken isminden [tür ve tür benzeri kelimeleri] çıkarın.
  - Bir sayı için `userCount`, `numUsers` veya `usersInt`'den daha iyi bir
    isimdir.
  - Bir slice için `users`, `userSlice`'dan daha iyi bir isimdir.
  - Kapsamda bir değerin iki versiyonu varsa, tür benzeri bir nitelikleyici
    eklemek kabul edilebilir, örneğin girdiyi `ageString`'de saklayıp ayrıştırılmış
    değer için `age` kullanabilirsiniz.
- [Çevreleyen bağlamdan] net olan kelimeleri çıkarın. Örneğin, bir
  `UserCount` metotunun uygulamasında `userCount` adlı bir yerel değişken
  muhtemelen gereksizdir; `count`, `users` veya hatta `c` kadar okunabilir.

[types and type-like words]: #repetitive-with-type
[surrounding context]: #repetitive-in-context

<a id="v"></a>

#### Tek harfli değişken isimleri

Tek harfli değişken isimleri [tekrarı](#repetition) en aza indirmek için
yararlı bir araç olabilir, ancak kodu gereksiz şekilde belirsiz de
kılabilir. Kullanımlarını, tam kelimenin açık olduğu ve tek harfli değişken
yerine tekrarlayıcı olacağı durumlarla sınırlayın.

Genel olarak:

- Bir [metot alıcı değişkeni] için tek harfli veya iki harfli isim tercih
  edilir.
- Yaygın türler için tanıdık değişken isimleri kullanmak genellikle
  yararlıdır:
  - `io.Reader` veya `*http.Request` için `r`
  - `io.Writer` veya `http.ResponseWriter` için `w`
- Tek harfli tanımlayıcılar tamsayı döngü değişkenleri olarak kabul
  edilebilir, özellikle indeksler (ör. `i`) ve koordinatlar (ör. `x` ve `y`)
  için.
- Kısaltmalar, kapsam kısa olduğunda kabul edilebilir döngü tanımlayıcıları
  olabilir, örneğin `for _, n := range nodes { ... }`.

[method receiver variable]: #receiver-names

<a id="repetition"></a>

### Tekrar

<!--
Note to future editors:

Do not use the term "stutter" to refer to cases when a name is repetitive.
-->

Go kaynak kodunun parçası gereksiz tekrardan kaçınmalıdır. Bunun en yaygın
kaynağı, genellikle gereksiz kelimeleri içeren veya bağlamını ya da türünü
tekrarlayan isimlerdir. Kodun kendisi de, aynı veya benzer kod parçasının yakın
bir mesafede birden fazla kez görünmesi durumunda gereksiz şekilde tekrarlayıcı
olabilir.

Tekrarlayıcı isimlendirme birçok biçimde gelebilir, bunlar arasında:

<a id="repetitive-with-package"></a>

#### Paket vs. dışa açık sembol ismi

Dışa açık sembolleri isimlendirirken, paketin ismi her zaman paketinizin
dışında görünürdür, bu nedenle ikisi arasındaki gereksiz bilgi azaltılmalı veya
ortadan kaldırılmalıdır. Bir paket yalnızca bir tür dışa açıyorsa ve bu tür
paketin kendisiyle adlandırılmışsa, gerekirse oluşturucunun kanonik ismi `New`'dir.

> **Örnekler:** Tekrarlayıcı İsim -> Daha İyi İsim
>
> - `widget.NewWidget` -> `widget.New`
> - `widget.NewWidgetWithName` -> `widget.NewWithName`
> - `db.LoadFromDatabase` -> `db.Load`
> - `goatteleportutil.CountGoatsTeleported` -> `gtutil.CountGoatsTeleported`
>   veya `goatteleport.Count`
> - `myteampb.MyTeamMethodRequest` -> `mtpb.MyTeamMethodRequest` veya
>   `myteampb.MethodRequest`

<a id="repetitive-with-type"></a>

#### Değişken ismi vs. tür

Derleyici her zaman bir değişkenin türünü bilir ve çoğu durumda okuyucu için
de bir değişkenin türü onun nasıl kullanıldığından nettir. Yalnızca bir
değişkenin değeri aynı kapsamda iki kez göründüğünde türünü netleştirmek
gereklidir.

| Tekrarlayıcı İsim             | Daha İyi İsim          |
| ----------------------------- | ---------------------- |
| `var numUsers int`            | `var users int`        |
| `var nameString string`       | `var name string`      |
| `var primaryProject *Project` | `var primary *Project` |

Değer birden fazla formda görünüyorsa, bu durum `ham` ve `ayrıştırılmış` gibi
ek bir kelimeyle veya temel gösterimle netleştirilebilir:

```go
// İyi:
limitRaw := r.FormValue("limit")
limit, err := strconv.Atoi(limitRaw)
```

```go
// İyi:
limitStr := r.FormValue("limit")
limit, err := strconv.Atoi(limitStr)
```

<a id="repetitive-in-context"></a>

#### Dış bağlam vs. yerel isimler

Çevreleyen bağlamdan bilgi içeren isimler genellikle faydasız fazladan gürültü
oluşturur. Paket ismi, metot ismi, tür ismi, fonksiyon ismi, import yolu ve hatta
dosya adı, içindeki tüm isimleri otomatik olarak nitelendiren bağlam sağlayabilir.

```go
// Kötü:
// In package "ads/targeting/revenue/reporting"
type AdsTargetingRevenueReport struct{}

func (p *Project) ProjectName() string
```

```go
// İyi:
// In package "ads/targeting/revenue/reporting"
type Report struct{}

func (p *Project) Name() string
```

```go
// Kötü:
// In package "sqldb"
type DBConnection struct{}
```

```go
// İyi:
// In package "sqldb"
type Connection struct{}
```

```go
// Kötü:
// In package "ads/targeting"
func Process(in *pb.FooProto) *Report {
    adsTargetingID := in.GetAdsTargetingID()
}
```

```go
// İyi:
// In package "ads/targeting"
func Process(in *pb.FooProto) *Report {
    id := in.GetAdsTargetingID()
}
```

Tekrar, genellikle sembolün kullanıcısının bağlamında, izole bir şekilde
değerlendirilmelidir. Örneğin, aşağıdaki kodda bazı durumlarda kabul
edilebilecek ancak bağlamda gereksiz olan birçok isim bulunmaktadır:

```go
// Kötü:
func (db *DB) UserCount() (userCount int, err error) {
    var userCountInt64 int64
    if dbLoadError := db.LoadFromDatabase("count(distinct users)", &userCountInt64); dbLoadError != nil {
        return 0, fmt.Errorf("failed to load user count: %s", dbLoadError)
    }
    userCount = int(userCountInt64)
    return userCount, nil
}
```

Bunun yerine, bağlamdan veya kullanımdan net olan isimlerle ilgili bilgi
sıkça çıkarılabilir:

```go
// İyi:
func (db *DB) UserCount() (int, error) {
    var count int64
    if err := db.Load("count(distinct users)", &count); err != nil {
        return 0, fmt.Errorf("failed to load user count: %s", err)
    }
    return int(count), nil
}
```

<a id="commentary"></a>

## Yorumlama

Yorumlama ile ilgili standartlar (ne yorumlanacağı, hangi stilin kullanılacağı,
çalıştırılabilir örneklerin nasıl sunulacağı vb.) bir kamu API'sinin
dokümantasyonunu okuma deneyimini desteklemek amacıyla hazırlanmıştır. Daha
fazla bilgi için [Effective Go](http://golang.org/doc/effective_go.html#commentary)
sayfasına bakın.

[En iyi uygulamalar belgesi][documentation conventions] bu konuyu daha ayrıntılı
olarak ele almaktadır.

**En İyi Uygulama:** Dokümantasyonun ve çalıştırılabilir örneklerin
yararlı olup olmadığını ve beklediğiniz şekilde sunulup sunulmadığını
görmek için geliştirme ve kod incelemesi sırasında
[doküman önizlemesini][doc preview] kullanın.

**İpucu:** Godoc çok az özel biçimlendirme kullanır; listeler ve kod
parçacıkları genellikle satır kaymasını önlemek için girintili olmalıdır.
Girintilendirme dışında, süslemeden kaçınılmalıdır.

[doc preview]: best-practices#documentation-preview
[documentation conventions]: best-practices#documentation-conventions

<a id="comment-line-length"></a>

### Yorum satır uzunluğu

Go'da yorumlar için sabit bir [satır uzunluğu] yoktur.

[line length]: guide#line-length

Uzun yorum satırlarının, yorum satırlarını otomatik olarak sarmayan araçlarda
kaynağın okunabilirliğini sağlamak için sarılması gerekir. Sarmanın nerede
yapılacağından emin değilseniz, 80 veya 100 sütun yaygın seçimlerdir. Ancak bu
katı bir sınır değildir; uzun düz metni bölmekten kaçınılması gereken
durumlar vardır.
Sarılmanın gerçekleşeceği belirli sütun genişliği için bir gereklilik yoktur.
Bir dosya içinde [tutarlı](guide#consistency) olmaya çalışın.

Yorumlama hakkında daha fazla bilgi için
[Go Blog'undaki dokümantasyon yazısına][post from The Go Blog on documentation]
bakın.

[post from The Go Blog on documentation]: https://blog.golang.org/godoc-documenting-go-code

```text
# İyi:
// This is a comment paragraph.
// The length of individual lines doesn't matter in Godoc;
// but the choice of wrapping makes it easy to read on narrow screens.
//
// Don't worry too much about the long URL:
// https://supercalifragilisticexpialidocious.example.com:8080/Animalia/Chordata/Mammalia/Rodentia/Geomyoidea/Geomyidae/
//
// Similarly, if you have other information that is made awkward
// by too many line breaks, use your judgment and include a long line
// if it helps rather than hinders.
```

Tek satıra çok fazla metin sığdıran yorumlardan kaçının; bu kötü bir
okuyucu deneyimi yaratır.

```text
# Kötü:
// This is a comment paragraph. While some code editors and viewers will wrap the paragraph for the reader, others will display a very long line that will overflow most windows and require users to scroll horizontally. In addition, even on a screen capable of displaying the entire line, it is easier to read a narrower paragraph than very wide one.
//
// Don't worry too much about the long URL:
// https://supercalifragilisticexpialidocious.example.com:8080/Animalia/Chordata/Mammalia/Rodentia/Geomyoidea/Geomyidae/
```

<a id="doc-comments"></a>

### Doküman yorumları

<a id="TOC-DocComments"></a>

Tüm üst düzey dışa açık isimlerin doküman yorumları olmalıdır; ayrıca
açık olmayan davranış veya anlama sahip dışa açık olmayan tür veya
fonksiyon bildirimleri de olmalıdır. Bu yorumlar, açıklanan nesnenin
ismiyle başlayan [tam cümleler][full sentences] olmalıdır. Daha doğal
okunması için isimden önce bir artikıl ("bir", "the") gelebilir.

```go
// İyi:
// A Request represents a request to run a command.
type Request struct { ...

// Encode writes the JSON encoding of req to w.
func Encode(w io.Writer, req *Request) { ...
```

Doküman yorumları [Godoc](https://pkg.go.dev/) görünür ve IDE'ler
tarafından gösterilir, bu nedenle paketi kullanan herkes için
yazılmalıdır.

[full sentences]: #comment-sentences

Bir doküman yorumu, bir struct içinde görünüyorsa sonraki sembole veya
alan grubuna uygulanır.

```go
// İyi:
// Options, grup yönetim servisini yapılandırır.
type Options struct {
    // General setup:
    Name  string
    Group *FooGroup

    // Dependencies:
    DB *sql.DB

    // Customization:
    LargeGroupThreshold int // optional; default: 10
    MinimumMembers      int // optional; default: 2
}
```

**En İyi Uygulama:** Dışa açık olmayan kod için doküman yorumlarınız varsa,
dışa açıkmış gibi aynı geleneği izleyin (yani yorumu dışa açık olmayan isimle
başlatın). Bu, daha sonra yorumlarda ve kodda dışa açık olmayan ismi
yeni dışa açık isimle basitçe değiştirerek dışa açık hale getirmeyi kolaylaştırır.

<a id="comment-sentences"></a>

### Yorum cümleleri

<a id="TOC-CommentSentences"></a>

Cümle olan yorumlar, standart İngilizce cümleleri gibi büyük harfle
başlamalı ve noktalama işaretleriyle yazılmalıdır. (İstisna olarak, büyük harfle
başlamayan bir tanımlayıcı adıyla cümle başlatılabilir, eğer başka türlü
açıkça anlaşılıyorsa. Bu tür durumlar muhtemelen yalnızca paragrafın başında
yapılmalıdır.)

Cümle parçacığı olan yorumların noktalama veya büyük harf gibi bu tür
gereksinimleri yoktur.

[Doküman yorumları][Documentation comments] her zaman tam cümleler olmalıdır ve bu
nedenle her zaman büyük harfle başlamalı ve noktalama işaretleriyle yazılmalıdır.
Basit satır sonu yorumları (özellikle struct alanları için) alan adının
özne olduğunu varsayan basit ifadeler olabilir.

```go
// İyi:
// A Server handles serving quotes from the collected works of Shakespeare.
type Server struct {
    // BaseDir points to the base directory under which Shakespeare's works are stored.
    //
    // The directory structure is expected to be the following:
    //   {BaseDir}/manifest.json
    //   {BaseDir}/{name}/{name}-part{number}.txt
    BaseDir string

    WelcomeMessage  string // displayed when user logs in
    ProtocolVersion string // checked against incoming requests
    PageLength      int    // lines per page when printing (optional; default: 20)
}
```

[Documentation comments]: #doc-comments

<a id="examples"></a>

### Örnekler

<a id="TOC-Examples"></a>

Paketler, amaçlanan kullanımlarını açıkça belgelemelidir.
[Çalıştırılabilir bir örnek][runnable example] sunmaya çalışın; örnekler
Godoc'da görünür. Çalıştırılabilir örnekler test dosyasında olmalıdır,
üretim kaynak dosyasında değil. Bu örneğe ([Godoc], [source]) bakın.

[runnable example]: http://blog.golang.org/examples
[Godoc]: https://pkg.go.dev/time#example-Duration
[source]: https://cs.opensource.google/go/go/+/HEAD:src/time/example_test.go

Çalıştırılabilir bir örnek sunmak mümkün değilse, örnek kod yorumlar içinde
verilebilir. Yorumlardaki diğer kod ve komut satırı parçacıklarında olduğu gibi,
standart biçimlendirme kurallarına uymalıdır.

<a id="named-result-parameters"></a>

### İsimli sonuç parametreleri

<a id="TOC-NamedResultParameters"></a>

Parametreleri adlandırırken, fonksiyon imzalarının Godoc'da nasıl
göründüğünü düşünün. Fonksiyonun kendisinin ismi ve sonuç
parametrelerinin türü genellikle yeterince açıktır.

```go
// İyi:
func (n *Node) Parent1() *Node
func (n *Node) Parent2() (*Node, error)
```

Bir fonksiyon aynı türden iki veya daha fazla parametre döndürüyorsa, isim
eklemek faydalı olabilir.

```go
// İyi:
func (n *Node) Children() (left, right *Node, err error)
```

Çağıran belirli sonuç parametreleri üzerinde harekete geçmek zorundaysa,
isimlendirmek eylemin ne olduğunu belirtmeye yardımcı olabilir:

```go
// İyi:
// WithTimeout returns a context that will be canceled no later than d duration
// from now.
//
// The caller must arrange for the returned cancel function to be called when
// the context is no longer needed to prevent a resource leak.
func WithTimeout(parent Context, d time.Duration) (ctx Context, cancel func())
```

Yukarıdaki kodda iptal etme, bir çağırıcının yapması gereken belirli bir
eylemdir. Ancak sonuç parametreleri yalnızca `(Context, func())` olarak
yazılsaydı, "iptal fonksiyonu" ne anlama geldiği belirsiz olurdu.

İsimleri [gereksiz tekrara](#repetitive-with-type) neden olduğunda isimli sonuç
parametrelerini kullanmayın.

```go
// Kötü:
func (n *Node) Parent1() (node *Node)
func (n *Node) Parent2() (node *Node, err error)
```

Fonksiyonun içinde bir değişken bildirmekten kaçınmak için sonuç
parametrelerini isimlendirmeyin. Bu uygulama, uygulama kısalığının
küçük bir bedeli karşılığında gereksiz API açıklığına yol açar.

[Çıplak dönüşler][Naked returns] yalnızca küçük bir fonksiyonda kabul
edilebilir. Orta düzey bir fonksiyon haline geldiğinde, döndürülen değerlerinizi
açıkça belirtin. Benzer şekilde, yalnızca çıplak dönüşler kullanmanızı
sağladığı için sonuç parametrelerini isimlendirmeyin.
[Açıklık](guide#clarity), fonksiyonunuzda birkaç satır tasarruf etmekten her zaman
daha önemlidir.

Bir sonuç parametresinin değeri ertelenmiş bir kapanışta değiştirilmek
zorundaysa, isimlendirmek her zaman kabul edilebilir.

> **İpucu:** Fonksiyon imzalarında türler isimlerden daha açıklayıcı olabilir.
> [GoTip #38: Functions as Named Types] bunu göstermektedir.
>
> Yukarıdaki [`WithTimeout`] örneğinde, gerçek kod sonucu parametre listesinde
> ham `func()` yerine bir [`CancelFunc`] kullanır ve çok az dokümantasyon
> gerektirir.

[Naked returns]: https://tour.golang.org/basics/7
[GoTip #38: Functions as Named Types]: index#gotip
[`WithTimeout`]: https://pkg.go.dev/context#WithTimeout
[`CancelFunc`]: https://pkg.go.dev/context#CancelFunc

<a id="package-comments"></a>

### Paket yorumları

<a id="TOC-PackageComments"></a>

Paket yorumları, paket hükmünün hemen üstünde görünmeli, yorum ile paket
adı arasında boş satır olmamalıdır. Örnek:

```go
// İyi:
// Package math provides basic constants and mathematical functions.
//
// This package does not guarantee bit-identical results across architectures.
package math
```

Her pakette tek bir paket yorumu olmalıdır. Bir paket birden fazla dosyadan
oluşuyorsa, dosyalardan tam olarak birinde paket yorumu olmalıdır.

`main` paketleri için yorumlar biraz farklı bir formdadır; BUILD dosyasındaki
`go_binary` kuralının adı paket adının yerini alır.

```go
// İyi:
// The seed_generator command is a utility that generates a Finch seed file
// from a set of JSON study configs.
package main
```

Diğer yorum stilleri, binari adının BUILD dosyasında yazıldığı şekilde tam
olarak yazılıyor olmasında kabul edilebilir. Binari adı ilk kelime olduğunda,
komut satırı çağrısının yazımıyla sıkı eşleşmese bile büyük harfle yazılması
zorunludur.

```go
// İyi:
// Binary seed_generator ...
// Command seed_generator ...
// Program seed_generator ...
// The seed_generator command ...
// The seed_generator program ...
// Seed_generator ...
```

İpuçları:

- Örnek komut satırı çağrısı ve API kullanımı faydalı dokümantasyon olabilir.
  Godoc biçimlendirmesi için kod içeren yorum satırlarını girintileyin.

- Belirgin bir birincil dosya yoksa veya paket yorumu olağanüstü uzunsa,
  doc yorumunu yalnızca yorum ve paket hükmünü içeren `doc.go` adlı bir
  dosyaya koymak kabul edilebilir.

- Çok satırlı yorumlar, birden fazla tek satırlı yorum yerine kullanılabilir.
  Bu, özellikle dokümantasyonda kaynak dosyadan kopyala-yapıştır yapılabilecek
  bölümler içerdiğinde faydalıdır; örneğin örnek komut satırları (binariler için)
  ve şablon örnekleri.

  ```go
  // İyi:
  /*
  The seed_generator command is a utility that generates a Finch seed file
  from a set of JSON study configs.

      seed_generator *.json | base64 > finch-seed.base64
  */
  package template
  ```

- Bakımcılar için hazırlanan ve tüm dosyaya uygulanan yorumlar genellikle
  import bildirimlerinin ardından yerleştirilir. Bunlar Godoc'da gösterilmez
  ve paket yorumları ile ilgili yukarıdaki kurallara tabi değildir.

<a id="imports"></a>

## İçe Aktarımlar

<a id="TOC-Imports"></a>

<a id="import-renaming"></a>

### Import yeniden adlandırma

Paket import'ları normalde yeniden adlandırılmamalıdır, ancak
yeniden adlandırılmaları gereken veya yeniden adlandırmanın
okunabilirliği artırdığı durumlar vardır.

İçe aktarılan paketlerin yerel adları [paket adlandırma](#package-names) kurallarına uymalıdır, alt çizgi ve büyük harf kullanımının yasaklanması da buna dahildir. Her zaman aynı içe aktarılmış paket için aynı yerel adı kullanarak [tutarlılığı](guide#consistency) korumaya çalışın.

Bir içe aktarılan paket, diğer içe aktarımlarla ad çakışmasını önlemek için _yeniden adlandırılmalıdır_. (Bunun bir sonucu olarak [iyi paket adları](#package-names) yeniden adlandırma gerektirmemelidir.) Ad çakışması durumunda, en yerel veya projeye özel içe aktarımı yeniden adlandırmayı tercih edin.

Oluşturulan protocol buffer paketleri, adlarından alt çizgileri kaldırmak için yeniden adlandırılmalıdır ve yerel adlarının `pb` son ekine sahip olması zorundadır. Daha fazla bilgi için [proto ve stub en iyi uygulamalarına](best-practices#import-protos) bakın.

```go
// İyi:
import (
    foosvcpb "path/to/package/foo_service_go_proto"
)
```

Son olarak, içe aktarılan ve otomatik oluşturulmamış bir paket, bilgilendirici olmayan bir adı varsa yeniden adlandırılabilir (örneğin `util` veya `v1`). Bunu sınırlı yapın: Paketin kullanımını çevreleyen kod yeterli bağlam sağlıyorsa paketi yeniden adlandırmayın. Mümkünse, paketin kendisini daha uygun bir adla yeniden düzenlemeyi tercih edin.

```go
// İyi:
import (
    core "github.com/kubernetes/api/core/v1"
    meta "github.com/kubernetes/apimachinery/pkg/apis/meta/v1beta1"
)
```

Kullanmak istediğiniz yaygın bir yerel değişken adıyla çakışan bir adı olan bir paketi içe aktarmanız gerekiyorsa (örneğin `url`, `ssh`) ve paketi yeniden adlandırmak istiyorsanız, tercih edilen yol `pkg` son ekini kullanmaktır (örneğin `urlpkg`). Bir paketin yerel bir değişken ile gölgelenebileceğini unutmayın; bu yeniden adlandırma yalnızca böyle bir değişken kapsamdayken paketin hala kullanılması gereken durumlarda gereklidir.

<a id="import-grouping"></a>

### Import gruplama

İçe aktarımlar aşağıdaki gruplara sırayla düzenlenmelidir:

1.  Standart kütüphane paketleri

1.  Diğer (projeye ait ve vendor) paketler

1.  Protocol Buffer içe aktarımları (örneğin, `fpb "path/to/foo_go_proto"`)

1.  [Yan etkiler](https://go.dev/doc/effective_go#blank_import) için içe aktarım
    (örneğin, `_ "path/to/package"`)

```go
// İyi:
package main

import (
    "fmt"
    "hash/adler32"
    "os"

    "github.com/dsnet/compress/flate"
    "golang.org/x/text/encoding"
    "google.golang.org/protobuf/proto"

    foopb "myproj/foo/proto/proto"

    _ "myproj/rpc/protocols/dial"
    _ "myproj/security/auth/authhooks"
)
```

<a id="import-blank"></a>

### Import "boş" (`import _`)

<a id="TOC-ImportBlank"></a>

Yalnızca yan etkileri için içe aktarılan paketler (`import _ "package"` sözdizimi kullanılarak) yalnızca ana pakette veya bunları gerektiren testlerde içe aktarılabilir.

Bu tür paketlere bazı örnekler şunlardır:

- [time/tzdata](https://pkg.go.dev/time/tzdata)

- Görüntü işleme kodunda [image/jpeg](https://pkg.go.dev/image/jpeg)

Kütüphane paketlerinde boş içe aktarımlardan kaçının, kütüphane dolaylı olarak bunlara bağlı olsa bile. Yan etki içe aktarımlarını ana paketle sınırlamak bağımlılıkları kontrol etmeye yardımcı olur ve farklı bir içe aktarıma bağlı testleri çatışma veya boşa giden derleme maliyeti olmadan yazmayı mümkün kılar.

Aşağıdakiler bu kuralın tek istisnalarıdır:

- [nogo static checker]'da yasaklı içe aktarımlar için kontrolü atlatmak amacıyla boş bir içe aktarım kullanabilirsiniz.

- `//go:embed` derleyici yönergesini kullanan bir kaynak dosyada [embed](https://pkg.go.dev/embed) paketinin boş bir içe aktarımını kullanabilirsiniz.

**İpucu:** Üretim ortamında dolaylı olarak bir yan etki içe aktarımına bağlı olan bir kütüphane paketi oluşturuyorsanız, amaçlanan kullanımı belgeleyin.

[nogo static checker]: https://github.com/bazelbuild/rules_go/blob/master/go/nogo.rst

<a id="import-dot"></a>

### Import "nokta" (`import .`)

<a id="TOC-ImportDot"></a>

The `import .` form is a language feature that allows bringing identifiers
exported from another package to the current package without qualification. See
the [language spec](https://go.dev/ref/spec#Import_declarations) for more.

Bu özelliği Google kod tabanında **kullanmayın**; işlevselliğin
nereden geldiğini söylemeyi zorlaştırır.

```go
// Kötü:
package foo_test

import (
    "bar/testutil" // "foo" paketini de import eder
    . "foo"
)

var myThing = Bar() // Bar, foo paketinde tanımlıdır; nitelendirme gerekmez.
```

```go
// İyi:
package foo_test

import (
    "bar/testutil" // "foo" paketini de import eder
    "foo"
)

var myThing = foo.Bar()
```

<a id="errors"></a>

## Hatalar

<a id="returning-errors"></a>

### Hataları döndürmek

<a id="TOC-ReturningErrors"></a>

Bir fonksiyonun başarısız olabileceğini belirtmek için `error` kullanın. Söz dizimi gereği, `error` son
sonuç parametresidir.

```go
// İyi:
func Good() error { /* ... */ }
```

`nil` hatası döndürmek, aksi halde başarısız olabilecek bir başarılı işlemi belirtmenin
en doğal yoludur. Bir fonksiyon hata döndürüyorsa, çağrı yapanlar açıkça belirtilmedikçe
hata dışı tüm dönüş değerlerini belirsiz olarak ele almalıdır. Genellikle, hata dışı dönüş değerleri sıfır değerleridir, ancak bu
varsayılamaz.

```go
// İyi:
func GoodLookup() (*Result, error) {
    // ...
    if err != nil {
        return nil, err
    }
    return res, nil
}
```

Hata döndüren dışa açık fonksiyonlar bunları `error` türünü kullanarak döndürmelidir.
Somut hata türleri, ince hatalara yatkındır: somut bir `nil` gösterici
bir arayüze sarılabilir ve böylece nil olmayan bir değere dönüşebilir (bkz.
[konuyla ilgili SSS][nil error]).

```go
// Kötü:
func Bad() *os.PathError { /*...*/ }
```

**İpucu:** Bir [`context.Context`] argümanı alan bir fonksiyon genellikle
bir `error` döndürmelidir, böylece çağrı yapan kişi fonksiyon çalışırken
bağlamanın iptal edilip edilmediğini belirleyebilir.

[nil error]: https://golang.org/doc/faq#nil_error

<a id="error-strings"></a>

### Hata dizgileri

<a id="TOC-ErrorStrings"></a>

Hata dizgileri büyük harfle başlamamalıdır (bir dışa açık isim, özel isim veya kısaltma ile başlamadıkça) ve noktalama işaretiyle bitmemelidir. Bunun nedeni, hata dizgilerinin kullanıcıya yazdırılmadan önce genellikle başka bir bağlamda görünmesidir.

```go
// Kötü:
err := fmt.Errorf("Something bad happened.")
```

```go
// İyi:
err := fmt.Errorf("something bad happened")
```

Öte yandan, tam gösterilen mesajın (günlük, test
hatası, API yanıtı veya diğer UI) stili değişir, ancak tipik olarak büyük harfle
başlamalıdır.

```go
// İyi:
log.Infof("Operation aborted: %v", err)
log.Errorf("Operation aborted: %v", err)
t.Errorf("Op(%q) failed unexpectedly; err=%v", args, err)
```

<a id="handle-errors"></a>

### Hataları ele almak

<a id="TOC-HandleErrors"></a>

Hataya rastlayan kod, onu nasıl ele alacağına dair kasıtlı bir seçim yapmalıdır. Hataları `_` değişkenleri kullanarak atmak genellikle uygun değildir. Bir fonksiyon hata döndürüyorsa, aşağıdakilerden birini yapın:

- Hemen hatayı ele alın ve çözün.
- Hatayı çağıran kişiye geri döndürün.
- Olağanüstü durumlarda, [`log.Fatal`] veya (kesinlikle gerekliyse) `panic` çağırın.

**Not:** `log.Fatalf` standart kütüphane log'u değildir. [#logging]'e bakın.

Nadir durumlarda, bir hatayı görmezden gelmek veya atmak uygunsa (örneğin, asla başarısız olmayacağı belgelenmiş [`(*bytes.Buffer).Write`] çağrısı gibi), eşlik eden bir yorum neden güvenli olduğunu açıklamalıdır.

```go
// İyi:
var b *bytes.Buffer

n, _ := b.Write(p) // asla nil olmayan bir hata döndürmez
```

Hata ele alma hakkında daha fazla tartışma ve örnek için, bkz.
[Effective Go](http://golang.org/doc/effective_go.html#errors) ve
[en iyi uygulamalar](best-practices.md#error-handling).

[`(*bytes.Buffer).Write`]: https://pkg.go.dev/bytes#Buffer.Write

<a id="in-band-errors"></a>

### Bant içi hatalar

<a id="TOC-In-Band-Errors"></a>

C ve benzeri dillerde, fonksiyonların -1, null veya boş dize gibi değerler döndürerek hataları veya eksik sonuçları belirtmesi yaygın bir durumdur. Bu, bant içi hata ele alma olarak bilinir.

```go
// Kötü:
// Lookup returns the value for key or -1 if there is no mapping for key.
func Lookup(key string) int
```

Bant içi hata değerini kontrol etmemek, hatalara yol açabilir ve hataları yanlış fonksiyona atfedebilir.

```go
// Kötü:
// The following line returns an error that Parse failed for the input value,
// whereas the failure was that there is no mapping for missingKey.
return Parse(Lookup(missingKey))
```

Go'nun çoklu dönüş değerleri desteği daha iyi bir çözüm sunar (bkz.
[Çoklu Dönüşler Hakkında Effective Go Bölümü]). Müşterilerin bir bant içi hata
değeri kontrol etmesini sağlamak yerine, bir fonksiyon diğer dönüş değerlerinin geçerli
olup olmadığını belirten ek bir değer döndürmelidir. Bu dönüş değeri, açıklama
gerekmediğinde bir hata veya boolean olabilir ve son
dönüş değeri olmalıdır.

```go
// İyi:
// Lookup returns the value for key or ok=false if there is no mapping for key.
func Lookup(key string) (value string, ok bool)
```

Bu API, çağrı yapan kişinin `Parse(Lookup(key))` yazmasını engeller, bu derleme zamanı hatasına yol açar, çünkü `Lookup(key)` 2 çıktısı vardır.

Bu şekilde hata döndürmek, daha sağlam ve açık hata ele almayı teşvik eder:

```go
// İyi:
value, ok := Lookup(key)
if !ok {
    return fmt.Errorf("no value for %q", key)
}
return Parse(value)
```

Bazı standart kütüphane fonksiyonları, `strings` paketindeki fonksiyonlar gibi, bant içi hata değerleri döndürür. Bu, programcıdan daha fazla dikkat gerektirme pahasına dize işleme kodunu büyük ölçüde basitleştirir. Genel olarak, Google tabanında Go kodu hatalar için ek değerler döndürmelidir.

[Effective Go section on multiple returns]: http://golang.org/doc/effective_go.html#multiple-returns

<a id="indent-error-flow"></a>

### Girintili hata akışı

<a id="TOC-IndentErrorFlow"></a>

Kodunuzun geri kalanına geçmeden önce hataları ele alın. Bu, okuyucunun normal yolu hızlıca bulmasını sağlayarak kodun okunabilirliğini artırır. Aynı mantık, bir koşulu test eden ve sonunda terminal koşulu (örn., `return`, `panic`, `log.Fatal`) ile biten her blok için de geçerlidir.

Terminal koşulu karşılanmadığında çalışan kod, `if` bloğundan sonra gelmeli ve bir `else` clause'unda girintilenmemelidir.

```go
// İyi:
if err != nil {
    // error handling
    return // or continue, etc.
}
// normal code
```

```go
// Kötü:
if err != nil {
    // error handling
} else {
    // normal code that looks abnormal due to indentation
}
```

> **İpucu:** Bir değişkeni birkaç satırdan fazla kullanıyorsanız,
> genellikle `if`-başlatıcıyla stilini kullanmaya değmez. Bu durumlarda,
> genellikle bildirimi dışarı çıkarıp standart bir `if`
> ifadesi kullanmak daha iyidir:
>
> ```go
> // İyi:
> x, err := f()
> if err != nil {
>   // error handling
>   return
> }
> // lots of code that uses x
> // across multiple lines
> ```
>
> ```go
> // Kötü:
> if x, err := f(); err != nil {
>   // error handling
>   return
> } else {
>   // lots of code that uses x
>   // across multiple lines
> }
> ```

Daha fazla bilgi için [Go Tip #1: Line of Sight] ve
[TotT: Reduce Code Complexity by Reducing Nesting](https://testing.googleblog.com/2017/06/code-health-reduce-nesting-reduce.html)
sayfalarına bakın.

[Go Tip #1: Line of Sight]: index#gotip

<a id="language"></a>

## Dil

<a id="literal-formatting"></a>

### Literal biçimlendirme

Go, son derece güçlü bir [composite literal syntax] sunar; bu sayede tek bir
ifadeyle derin iç içe geçmiş, karmaşık değerler oluşturulabilir. Mümkün
olduğunda, değerleri alan alanı oluşturmak yerine bu literal sözdizimi
kullanılmalıdır. `gofmt` literal biçimlendirmesi genel olarak oldukça iyidir,
ancak bu literal'ların okunabilir ve sürdürülebilir olmasını sağlamak için
bazı ek kurallar mevcuttur.

[composite literal syntax]: https://golang.org/ref/spec#Composite_literals

<a id="literal-field-names"></a>

#### Alan isimleri

Struct literal'ları, geçerli paketin dışında tanımlanan türler için **alan
isimleri** belirtmek zorundadır.

- Diğer paketlerdeki türler için alan isimlerini dahil edin.

  ```go
  // İyi:
  // https://pkg.go.dev/encoding/csv#Reader
  r := csv.Reader{
    Comma: ',',
    Comment: '#',
    FieldsPerRecord: 4,
  }
  ```

  Struct'taki alanların konumu ve alanların tamamı (alan isimleri atlandığında
  her ikisinin de doğru olması gerekir) genellikle struct'ın herkese açık API'sinin
  bir parçası olarak değerlendirilmez; gereksiz bağımlılığı önlemek için alan
  ismini belirtmek gereklidir.

  ```go
  // Kötü:
  r := csv.Reader{',', '#', 4, false, false, false, false}
  ```

- Paket içi türler için alan isimleri isteğe bağlıdır.

  ```go
  // İyi:
  okay := Type{42}
  also := internalType{4, 2}
  ```

  Alan isimleri kodu daha net kılıyorsa yine de kullanılmalıdır ve bunu yapmak
  oldukça yaygındır. Örneğin, çok sayıda alanı olan bir struct neredeyse her
  zaman alan isimleriyle başlatılmalıdır.

  <!-- TODO: Maybe a better example here that doesn't have many fields. -->

  ```go
  // İyi:
  okay := StructWithLotsOfFields{
    field1: 1,
    field2: "two",
    field3: 3.14,
    field4: true,
  }
  ```

<a id="literal-matching-braces"></a>

#### Eşleşen süslü ayraçlar

Bir süslü ayraç çiftinin kapanan kısmı her zaman açılış süslü ayracıyla aynı
girinti miktarına sahip bir satırda görünmelidir. Tek satırlı literal'lar bu
özelliğe doğal olarak sahiptir. Literal birden fazla satıra yayıldığında, bu
özelliğin korunması literal'lar için süslü ayraç eşleşmesini, fonksiyonlar ve
`if` ifadeleri gibi yaygın Go sözdizimsel yapılarıyla aynı şekilde tutar.

Bu alandaki en yaygın hata, çok satırlı bir struct literal'da kapanış süslü
ayracını bir değerle aynı satıra koymaktır. Bu durumlarda, satır virgülle
bitmeli ve kapanış süslü ayracı bir sonraki satırda görünmelidir.

```go
// İyi:
good := []*Type{{Key: "value"}}
```

```go
// İyi:
good := []*Type{
    {Key: "multi"},
    {Key: "line"},
}
```

```go
// Kötü:
bad := []*Type{
    {Key: "multi"},
    {Key: "line"}}
```

```go
// Kötü:
bad := []*Type{
    {
        Key: "value"},
}
```

<a id="literal-cuddled-braces"></a>

#### Bitişik süslü ayraçlar

Dizi ve array literal'larında süslü ayraçlar arasında boşluk bırakmaya (yani
onları "bitiştirmeye") yalnızca aşağıdaki her iki koşul da sağlanıyorsa izin
verilir.

- [Girinti eşleşmesi](#literal-matching-braces)
- İç değerler de literal veya proto builder olmalıdır (yani bir değişken veya
  başka bir ifade değil)

```go
// İyi:
good := []*Type{
    { // Not cuddled
        Field: "value",
    },
    {
        Field: "value",
    },
}
```

```go
// İyi:
good := []*Type{{ // Cuddled correctly
    Field: "value",
}, {
    Field: "value",
}}
```

```go
// İyi:
good := []*Type{
    first, // Can't be cuddled
    {Field: "second"},
}
```

```go
// İyi:
okay := []*pb.Type{pb.Type_builder{
    Field: "first", // Proto Builders may be cuddled to save vertical space
}.Build(), pb.Type_builder{
    Field: "second",
}.Build()}
```

```go
// Kötü:
bad := []*Type{
    first,
    {
        Field: "second",
    }}
```

<a id="literal-repeated-type-names"></a>

#### Tekrarlanan tür isimleri

Tekrarlanan tür isimleri dizi ve harita literal'larından çıkarılabilir. Bu,
karmaşıklığı azaltmaya yardımcı olabilir. Tür isimlerini açıkça tekrarlamak
için makul bir durum, projenizde yaygın olmayan karmaşık bir türle
uğraşırken, tekrarlayan tür isimlerinin birbirinden uzak satırlarda olması ve
okuyucuya bağlamı hatırlatabilmesidir.

```go
// İyi:
good := []*Type{
    {A: 42},
    {A: 43},
}
```

```go
// Kötü:
repetitive := []*Type{
    &Type{A: 42},
    &Type{A: 43},
}
```

```go
// İyi:
good := map[Type1]*Type2{
    {A: 1}: {B: 2},
    {A: 3}: {B: 4},
}
```

```go
// Kötü:
repetitive := map[Type1]*Type2{
    Type1{A: 1}: &Type2{B: 2},
    Type1{A: 3}: &Type2{B: 4},
}
```

**İpucu:** Struct literal'larında tekrarlayan tür isimlerini kaldırmak
istiyorsanız, `gofmt -s` komutunu çalıştırabilirsiniz.

<a id="literal-zero-value-fields"></a>

#### Sıfır değerli alanlar

[Sıfır değerli] alanlar, netlik kaybına yol açmıyorsa struct literal'larından
çıkartılabilir.

İyi tasarlanmış API'ler genellikle okunabilirliği artırmak için sıfır değerli
oluşturmayı kullanır. Örneğin, aşağıdaki struct'tan üç sıfır değerli alanın
çıkartılması, belirtilen tek seçeneğe dikkat çekmesini sağlar.

[Zero-value]: https://golang.org/ref/spec#The_zero_value

```go
// Kötü:
import (
  "github.com/golang/leveldb"
  "github.com/golang/leveldb/db"
)

ldb := leveldb.Open("/my/table", &db.Options{
    BlockSize: 1<<16,
    ErrorIfDBExists: true,

    // These fields all have their zero values.
    BlockRestartInterval: 0,
    Comparer: nil,
    Compression: nil,
    FileSystem: nil,
    FilterPolicy: nil,
    MaxOpenFiles: 0,
    WriteBufferSize: 0,
    VerifyChecksums: false,
})
```

```go
// İyi:
import (
  "github.com/golang/leveldb"
  "github.com/golang/leveldb/db"
)

ldb := leveldb.Open("/my/table", &db.Options{
    BlockSize: 1<<16,
    ErrorIfDBExists: true,
})
```

Tablolara dayalı testlerdeki struct'lar genellikle [açık alan isimlerinden]
faydalanır, özellikle test struct'ı basit olmadığında. Bu, yazarın ilgili
alanlar test durumuyla ilişkili olmadığında sıfır değerli alanları tamamen
atlamasına olanak tanır. Örneğin, başarılı test durumları hatayla veya
hızla ilgili alanları atlamalıdır. Sıfır değerin test durumunu anlamak için
gerekli olduğu durumlarda, örneğin sıfır veya `nil` girdileri test edilirken,
alan isimleri belirtilmelidir.

[explicit field names]: #literal-field-names

**Özlü**

```go
tests := []struct {
    input      string
    wantPieces []string
    wantErr    error
}{
    {
        input:      "1.2.3.4",
        wantPieces: []string{"1", "2", "3", "4"},
    },
    {
        input:   "hostname",
        wantErr: ErrBadHostname,
    },
}
```

**Açık**

```go
tests := []struct {
    input    string
    wantIPv4 bool
    wantIPv6 bool
    wantErr  bool
}{
    {
        input:    "1.2.3.4",
        wantIPv4: true,
        wantIPv6: false,
    },
    {
        input:    "1:2::3:4",
        wantIPv4: false,
        wantIPv6: true,
    },
    {
        input:    "hostname",
        wantIPv4: false,
        wantIPv6: false,
        wantErr:  true,
    },
}
```

<a id="nil-slices"></a>

### Nil slice

Çoğu amaç için `nil` ile boş dizi arasında işlevsel bir fark yoktur. `len` ve
`cap` gibi yerleşik fonksiyonlar `nil` dizileri üzerinde beklendiği gibi
çalışır.

```go
// İyi:
import "fmt"

var s []int         // nil

fmt.Println(s)      // []
fmt.Println(len(s)) // 0
fmt.Println(cap(s)) // 0
for range s {...}   // no-op

s = append(s, 42)
fmt.Println(s)      // [42]
```

Boş bir diziyi yerel değişken olarak bildiriyorsanız (özellikle dönüş değeri
kaynağı olabiliyorsa), çağrı yapanlardan kaynaklanan hata riskini azaltmak için
nil başlatmayı tercih edin.

```go
// İyi:
var t []string
```

```go
// Kötü:
t := []string{}
```

nil ile boş dizi arasında ayrım yapmaya zorlayan API'ler oluşturmayın.

```go
// İyi:
// Ping pings its targets.
// Returns hosts that successfully responded.
func Ping(hosts []string) ([]string, error) { ... }
```

```go
// Kötü:
// Ping pings its targets and returns a list of hosts
// that successfully responded. Can be empty if the input was empty.
// nil signifies that a system error occurred.
func Ping(hosts []string) []string { ... }
```

Arayüz tasarlarken, `nil` dizi ile `nil` olmayan, sıfır uzunluklu dizi arasında
ayrım yapmaktan kaçının, çünkü bu incelikli programlama hatalarına yol açabilir.
Bu tipik olarak `== nil` yerine `len` kullanılarak boşluk kontrolü yapılarak
gerçekleştirilir.

Bu implementasyon hem `nil` hem de sıfır uzunluklu dizileri "boş" olarak
kabul eder:

```go
// İyi:
// describeInts describes s with the given prefix, unless s is empty.
func describeInts(prefix string, s []int) {
    if len(s) == 0 {
        return
    }
    fmt.Println(prefix, s)
}
```

API'nin bir parçası olarak bu ayrımın üzerine inşa etmek yerine:

```go
// Kötü:
func maybeInts() []int { /* ... */ }

// describeInts describes s with the given prefix; pass nil to skip completely.
func describeInts(prefix string, s []int) {
  // The behavior of this function unintentionally changes depending on what
  // maybeInts() returns in 'empty' cases (nil or []int{}).
  if s == nil {
    return
  }
  fmt.Println(prefix, s)
}

describeInts("Here are some ints:", maybeInts())
```

Daha fazla bilgi için [in-band errors] sayfasına bakın.

[in-band errors]: #in-band-errors

<a id="indentation-confusion"></a>

### Girinti karışıklığı

Bir satır kesintisi, satırın geri kalanını girintili bir kod bloğuyla
hizalayacaksa kaçının. Bu kaçınılmazsa, bloktaki kod ile sarılmış satır
arasında boşluk bırakın.

```go
// Kötü:
if longCondition1 && longCondition2 &&
    // Conditions 3 and 4 have the same indentation as the code within the if.
    longCondition3 && longCondition4 {
    log.Info("all conditions met")
}
```

Belirli kurallar ve örnekler için aşağıdaki bölümlere bakın:

- [Function formatting](#func-formatting)
- [Conditionals and loops](#conditional-formatting)
- [Literal formatting](#literal-formatting)

<a id="func-formatting"></a>

### Fonksiyon biçimlendirmesi

Fonksiyon veya metod imzası, [girinti karışıklığından](#indentation-confusion)
kaçınmak için tek bir satırda kalmalıdır.

Fonksiyon argüman listeleri bir Go kaynak dosyasındaki en uzun satırlardan
bazılarını oluşturabilir. Ancak, bunlar bir girinti değişikliği öncesinde
yer alır ve bu nedenle sonraki satırların fonksiyon gövdesinin bir parçası
gibi görünmesine neden olacak şekilde satırı kırmak zordur:

```go
// Kötü:
func (r *SomeType) SomeLongFunctionName(foo1, foo2, foo3 string,
    foo4, foo5, foo6 int) {
    foo7 := bar(foo1)
    // ...
}
```

See [best practices](best-practices#funcargs) for a few options for shortening
the call sites of functions that would otherwise have many arguments.

Lines can often be shortened by factoring out local variables.

```go
// İyi:
local := helper(some, parameters, here)
good := foo.Call(list, of, parameters, local)
```

Similarly, function and method calls should not be separated based solely on
line length.

```go
// İyi:
good := foo.Call(long, list, of, parameters, all, on, one, line)
```

```go
// Kötü:
bad := foo.Call(long, list, of, parameters,
    with, arbitrary, line, breaks)
```

Avoid adding inline comments to specific function arguments where possible.
Instead, use an [option struct](best-practices#option-structure) or add more
detail to the function documentation.

```go
// İyi:
good := server.New(ctx, server.Options{Port: 42})
```

```go
// Kötü:
bad := server.New(
    ctx,
    42, // Port
)
```

If the API cannot be changed or if the local call is unusual (whether or not the
call is too long), it is always permissible to add line breaks if it aids in
understanding the call.

```go
// İyi:
canvas.RenderHeptagon(fillColor,
    x0, y0, vertexColor0,
    x1, y1, vertexColor1,
    x2, y2, vertexColor2,
    x3, y3, vertexColor3,
    x4, y4, vertexColor4,
    x5, y5, vertexColor5,
    x6, y6, vertexColor6,
)
```

Note that the lines in the above example are not wrapped at a specific column
boundary but are grouped based on vertex coordinates and color.

Long string literals within functions should not be broken for the sake of line
length. For functions that include such strings, a line break can be added after
the string format, and the arguments can be provided on the next or subsequent
lines. The decision about where the line breaks should go is best made based on
semantic groupings of inputs, rather than based purely on line length.

```go
// İyi:
log.Warningf("Database key (%q, %d, %q) incompatible in transaction started by (%q, %d, %q)",
    currentCustomer, currentOffset, currentKey,
    txCustomer, txOffset, txKey)
```

```go
// Kötü:
log.Warningf("Database key (%q, %d, %q) incompatible in"+
    " transaction started by (%q, %d, %q)",
    currentCustomer, currentOffset, currentKey, txCustomer,
    txOffset, txKey)
```

<a id="conditional-formatting"></a>

### Koşullar ve döngüler

Bir `if` ifadesi satır kırılmalı değildir; çok satırlı `if` clause'ları
[girinti karışıklığına](#indentation-confusion) yol açabilir.

```go
// Kötü:
// The second if statement is aligned with the code within the if block, causing
// indentation confusion.
if db.CurrentStatusIs(db.InTransaction) &&
    db.ValuesEqual(db.TransactionKey(), row.Key()) {
    return db.Errorf(db.TransactionError, "query failed: row (%v): key does not match transaction key", row)
}
```

Kısa devre davranışı gerekli değilse, boolen operatörler doğrudan
çıkartılabilir:

```go
// İyi:
inTransaction := db.CurrentStatusIs(db.InTransaction)
keysMatch := db.ValuesEqual(db.TransactionKey(), row.Key())
if inTransaction && keysMatch {
    return db.Error(db.TransactionError, "query failed: row (%v): key does not match transaction key", row)
}
```

Özellikle koşul zaten tekrarlayıcıysa, çıkartılabilecek başka yerel
değişkenler de olabilir:

```go
// İyi:
uid := user.GetUniqueUserID()
if db.UserIsAdmin(uid) || db.UserHasPermission(uid, perms.ViewServerConfig) || db.UserHasPermission(uid, perms.CreateGroup) {
    // ...
}
```

```go
// Kötü:
if db.UserIsAdmin(user.GetUniqueUserID()) || db.UserHasPermission(user.GetUniqueUserID(), perms.ViewServerConfig) || db.UserHasPermission(user.GetUniqueUserID(), perms.CreateGroup) {
    // ...
}
```

`if` ifadeleri closure veya çok satırlı struct literal içerdiğinde,
[girinti karışıklığından](#indentation-confusion) kaçınmak için
[eşleşen süslü ayraçların](#literal-matching-braces) sağlandığından emin olunmalıdır.

```go
// İyi:
if err := db.RunInTransaction(func(tx *db.TX) error {
    return tx.Execute(userUpdate, x, y, z)
}); err != nil {
    return fmt.Errorf("user update failed: %s", err)
}
```

```go
// İyi:
if _, err := client.Update(ctx, &upb.UserUpdateRequest{
    ID:   userID,
    User: user,
}); err != nil {
    return fmt.Errorf("user update failed: %s", err)
}
```

Benzer şekilde, `for` ifadelerine yapay satır kırılmaları eklemeye
çalışmayın. Yeniden düzenlemenin şık bir yolu yoksa, satırın basitçe uzun
olmasına her zaman izin verebilirsiniz:

```go
// İyi:
for i, max := 0, collection.Size(); i < max && !collection.HasPendingWriters(); i++ {
    // ...
}
```

Sıkça, ancak vardır:

```go
// İyi:
for i, max := 0, collection.Size(); i < max; i++ {
    if collection.HasPendingWriters() {
        break
    }
    // ...
}
```

`switch` ve `case` ifadeleri de tek bir satırda kalmalıdır.

```go
// İyi:
switch good := db.TransactionStatus(); good {
case db.TransactionStarting, db.TransactionActive, db.TransactionWaiting:
    // ...
case db.TransactionCommitted, db.NoTransaction:
    // ...
default:
    // ...
}
```

```go
// Kötü:
switch bad := db.TransactionStatus(); bad {
case db.TransactionStarting,
    db.TransactionActive,
    db.TransactionWaiting:
    // ...
case db.TransactionCommitted,
    db.NoTransaction:
    // ...
default:
    // ...
}
```

Satır aşırı uzunsa, tüm case'leri girintileyin ve aralarına boş satır
koyarak [girinti karışıklığından](#indentation-confusion) kaçının:

```go
// İyi:
switch db.TransactionStatus() {
case
    db.TransactionStarting,
    db.TransactionActive,
    db.TransactionWaiting,
    db.TransactionCommitted:

    // ...
case db.NoTransaction:
    // ...
default:
    // ...
}
```

Bir değişkeni sabitle karşılaştıran koşullarda, değişken değerini eşitlik
operatörünün sol tarafına yerleştirin:

```go
// İyi:
if result == "foo" {
  // ...
}
```

Sabitin önce geldiği daha az açık ifadelemenin yerine
(["Yoda tarzı koşullar"](https://en.wikipedia.org/wiki/Yoda_conditions)):

```go
// Kötü:
if "foo" == result {
  // ...
}
```

<a id="copying"></a>

### Kopyalama

<a id="TOC-Copying"></a>

Beklenmeyen takma adlandırma ve benzeri hatalardan kaçınmak için, başka bir
paketten bir struct kopyalarken dikkatli olun. Örneğin, `sync.Mutex` gibi
eşzamanlılık nesneleri kopyalanmamalıdır.

`bytes.Buffer` türü bir `[]byte` slice içerir ve küçük diziler için bir
optimizasyon olarak, slice'ın referans verebileceği küçük bir byte dizisi
içerir. Bir `Buffer`'ı kopyalarsanız, kopyadaki slice orijinaldeki dizi ile
takma ad oluşturabilir ve bu da sonraki metod çağrılarının şaşırtıcı etkilere
sahip olmasına neden olabilir.

Genel olarak, `T` türünden bir değerin metotları işaretçi türü olan `*T`
ile ilişkiliyse bu değeri kopyalamayın.

```go
// Kötü:
b1 := bytes.Buffer{}
b2 := b1
```

Değer alan bir alıcıya sahip bir metodu çağırmak kopyayı gizleyebilir. Bir
API oluştururken, struct'larınız kopyalanmaması gereken alanlar içeriyorsa
genellikle işaretçi türleri almalı ve döndürmelisiniz.

Bunlar kabul edilebilir:

```go
// İyi:
type Record struct {
  buf bytes.Buffer
  // diğer alanlar atlandı
}

func New() *Record {...}

func (r *Record) Process(...) {...}

func Consumer(r *Record) {...}
```

Ancak bunlar genellikle yanlıştır:

```go
// Kötü:
type Record struct {
  buf bytes.Buffer
  // diğer alanlar atlandı
}


func (r Record) Process(...) {...} // Makes a copy of r.buf

func Consumer(r Record) {...} // Makes a copy of r.buf
```

Bu yönerge `sync.Mutex` kopyalama için de geçerlidir.

<a id="dont-panic"></a>

### Panic yapmayın

<a id="TOC-Don-t-Panic"></a>

Normal hata ele almak için `panic` kullanmayın. Bunun yerine `error` ve çoklu
dönüş değerlerini kullanın. [Hatalar hakkında Effective Go bölümüne][Effective Go section on errors] bakın.

`package main` ve başlatma kodunda, programı sonlandırması gereken hatalar için
(ör. geçersiz yapılandırma) [`log.Exit`] kullanmayı düşünün, çünkü bu
durumların çoğunda yığın izi okuyucuya yardımcı olmaz. [`log.Exit`]'in
[`os.Exit`] çağırdığını ve ertelenmiş fonksiyonların çalıştırılmayacağını lütfen
unutmayın.

"İmkansız" koşulları, yani kod incelemesi ve/veya test sırasında her zaman
yakalanması gereken hataları belirten hatalar için, bir fonksiyon makul bir
şekilde hata döndürebilir veya [`log.Fatal`] çağırabilir.

Ayrıca bakın: [panic ne zaman kabul edilebilir](best-practices.md#when-to-panic).

**Not:** `log.Fatalf` standart kütüphane log'u değildir. [#logging]'e bakın.

[Effective Go section on errors]: http://golang.org/doc/effective_go.html#errors
[`os.Exit`]: https://pkg.go.dev/os#Exit

<a id="must-functions"></a>

### Must fonksiyonları

Başarısızlıkta programı durduran kurulum yardımcı fonksiyonları `MustXYZ` (veya
`mustXYZ`) adlandırma kuralını izler. Genel olarak, yalnızca program
başlangıcında çağrılmalıdırlar, normal Go hata ele almanın tercih edildiği
kullanıcı girişi gibi durumlarda değil.

Bu durum sıkça, yalnızca [paket başlatma zamanında](https://golang.org/ref/spec#Package_initialization)
(paket düzeyindeki değişkenleri başlatmak için çağrılan fonksiyonlar için ortaya
çıkar (ör. [template.Must](https://golang.org/pkg/text/template/#Must) ve
[regexp.MustCompile](https://golang.org/pkg/regexp/#MustCompile)).

```go
// İyi:
func MustParse(version string) *Version {
    v, err := Parse(version)
    if err != nil {
        panic(fmt.Sprintf("MustParse(%q) = _, %v", version, err))
    }
    return v
}

// Package level "constant". If we wanted to use `Parse`, we would have had to
// set the value in `init`.
var DefaultVersion = MustParse("1.2.3")
```

Aynı kural, yalnızca geçerli testi durduran ( `t.Fatal` kullanarak) test
yardımcılarında da kullanılabilir. Bu tür yardımcılar, hata döndüren
fonksiyonların doğrudan bir alanına atanamadığı durumlarda, örneğin
[tablo tabanlı testlerdeki](#table-driven-tests) alanlarda test değerleri
oluştururken sıkça kullanışlıdır.

```go
// İyi:
func mustMarshalAny(t *testing.T, m proto.Message) *anypb.Any {
  t.Helper()
  any, err := anypb.New(m)
  if err != nil {
    t.Fatalf("mustMarshalAny(t, m) = %v; want %v", err, nil)
  }
  return any
}

func TestCreateObject(t *testing.T) {
  tests := []struct{
    desc string
    data *anypb.Any
  }{
    {
      desc: "my test case",
      // Creating values directly within table driven test cases.
      data: mustMarshalAny(t, mypb.Object{}),
    },
    // ...
  }
  // ...
}
```

Her iki durumda da, bu kalıbın değeri yardımcıların "değer" bağlamında
çağrılabilmesidir. Bu yardımcılar, bir hatanın yakalanmasının zor olduğu
yerlerde veya bir hatanın [kontrol edilmesi](#handle-errors) gereken bir bağlamda
(ör. birçok istek işleyicisinde) çağrılmamalıdır. Sabit girdiler için bu,
testlerin `Must` argümanlarının düzgün biçimlendirilmiş olmasını kolayca
sağlamasına izin verir ve sabit olmayan girdiler için testlerin hataların
[düzgün ele alındığını veya iletildiğini](best-practices#error-handling)
doğrulamasına olanak tanır.

`Must` fonksiyonları bir testte kullanıldığında, genellikle
[test yardımcısı olarak işaretlenmeli](#mark-test-helpers) ve hatada `t.Fatal`
çağırmalıdır (bu konuda daha fazla değerlendirme için
[test yardımcılarında hata ele alma](best-practices#test-helper-error-handling)
sayfasına bakın).

[Olağanüstü hata ele alma](best-practices#error-handling) mümkün olduğunda
(bazı yeniden düzenlemeler dahil) kullanılmamalıdır:

```go
// Kötü:
func Version(o *servicepb.Object) (*version.Version, error) {
    // Return error instead of using Must functions.
    v := version.MustParse(o.GetVersionString())
    return dealiasVersion(v)
}
```

<a id="goroutine-lifetimes"></a>

### Goroutine ömürleri

<a id="TOC-GoroutineLifetimes"></a>

Goroutine başlatırken, ne zaman veya ne sıklıkla sona erdiklerini açıkça
belirtin.

Goroutine'ler kanal gönderilerini veya alımlarını engelleyerek sızıntı
yapabilir. Çöp toplayıcı, kanala engellenmiş bir goroutine'i, başka bir
goroutine'in kanala referansı olmasa bile sonlandırmaz.

Goroutine'ler sızıntı yapmasa bile, artık gerekli olmadıklarında onları
devam ederken bırakmak, diğer ince ve teşhis edilmesi zor sorunlara yol açabilir.
Kapatılmış bir kanala gönderim yapmak paniğe neden olur.

```go
// Kötü:
ch := make(chan int)
ch <- 42
close(ch)
ch <- 13 // panic
```

"Sonuç artık gerekli olmadığında" hala kullanılan girdileri değiştirmek veri
yarışmalarına yol açabilir. Goroutine'leri keyfi olarak uzun süre devam ettirmek
tahmin edilemeyen bellek kullanımına yol açabilir.

Eşzamanlı kod, goroutine ömürlerinin belirgin olduğu şekilde yazılmalıdır. Bu
genellikle eşzamanlılıkla ilgili kodun bir fonksiyonun kapsamı içinde
sınırlı tutulması ve mantığın [eşzamanlı olmayan fonksiyonlara] ayrılması
anlamına gelir. Eşzamanlılık hala belirgin değilse, goroutine'lerin ne zaman ve
neden sona erdiğini belgelemek önemlidir.

Context kullanımıyla ilgili en iyi uygulamaları izleyen kod genellikle bunu
açık hale getirmeye yardımcı olur. Geleneksel olarak bir [`context.Context`]
ile yönetilir:

```go
// İyi:
func (w *Worker) Run(ctx context.Context) error {
    var wg sync.WaitGroup
    // ...
    for item := range w.q {
        // process returns at latest when the context is cancelled.
        wg.Add(1)
        go func() {
            defer wg.Done()
            process(ctx, item)
        }()
    }
    // ...
    wg.Wait()  // Prevent spawned goroutines from outliving this function.
}
```

Yukarıdakinin `chan struct{}` gibi ham sinyal channel'ları,
eşzamanlı değişkenler, [koşul değişkenleri][rethinking-slides] ve
daha fazlasını kullanan diğer varyasyonları vardır. Önemli olan,
goroutine'nin sonraki bakımcılar için belirgin olmasıdır.

Buna karşılık, başlatılmış goroutine'lerinin ne zaman bittiğine
dikkatsiz olan aşağıdaki kod:

```go
// Kötü:
func (w *Worker) Run() {
    // ...
    for item := range w.q {
        // process returns when it finishes, if ever, possibly not cleanly
        // handling a state transition or termination of the Go program itself.
        go process(item)
    }
    // ...
}
```

Bu kod düzgün görünse de, birkaç temel sorun vardır:

- Kod muhtemelen üretim ortamında tanımlanmamış davranış içerir ve program,
  işletim sistemi kaynakları serbest bile olsa düzgün sonlanmayabilir.

- Kod, belirsiz yaşam döngüsü nedeniyle anlamlı bir şekilde test edilmesi
  zordur.

- Kod, yukarıda açıklandığı gibi kaynakları sızdırabilir.

Ayrıca bakın:

- [Bir goroutine'i nasıl duracağını bilmeden başlamayın][cheney-stop]
- Klasik Eşzamanlılık Kalıplarını Yeniden Düşünme:
  [sunumlar][rethinking-slides], [video][rethinking-video]
- [When Go programs end]
- [Documentation Conventions: Contexts]

[synchronous functions]: #synchronous-functions
[cheney-stop]: https://dave.cheney.net/2016/12/22/never-start-a-goroutine-without-knowing-how-it-will-stop
[rethinking-slides]: https://drive.google.com/file/d/1nPdvhB0PutEJzdCq5ms6UI58dp50fcAN/view
[rethinking-video]: https://www.youtube.com/watch?v=5zXAHh5tJqQ
[When Go programs end]: https://changelog.com/gotime/165
[Documentation Conventions: Contexts]: best-practices.md#documentation-conventions-contexts

<a id="interfaces"></a>

### Interface

<a id="TOC-Interfaces"></a>

[Gerçek bir ihtiyaç](guide#simplicity) existene kadar interface oluşturmaktan
kaçının. "Servis" veya "depo" gibi soyut adlandırılmış kalıplar yerine gerekli
davranışa odaklanın.

- RPC istemcilerini soyutlama veya test amacıyla yeni manuel interface'ler
  içine sarmayın.
  Bunun yerine [gerçek taşıyıcıları](best-practices#use-real-transports) kullanın
  ([test RPC'si][testing RPC]).

- Arka kapılar tanımlamayın veya yalnızca test amacıyla bir interface'in
  [test ikamesi][test double] uygulamalarını dışa açmayın. Bunun yerine
  gerçek uygulamanın [herkese açık API'si][public API] üzerinden test etmeyi
  tercih edin.

Interface'leri, kolay uygulama ve bileşim için küçük tasarlayın
([GoTip #78: Minimal Viable Interfaces]). Interface'leri sözleşme, kenar
durumları ve beklenen hatalar dahil olmak üzere uygun şekilde belgeleyin.
Interface türlerini yalnızca paket içinde kullanılıyorsa dışa açılmamış
tutun.

Interface'in kullanıcısı onu tanımlamalıdır (interface'i uygulayan paket
değil), yalnızca gerçekten kullandıkları metotları içerdiğinden emin olun.
Üretici paket, interface bir ürün (ortak bir protokol) olduğunda interface'i
dışa açabilir, böylece interface yeniden tanımlama şişkinliği önlenir.

Bir deyiş vardır: Fonksiyonlar argüman olarak interface almalı ancak somut türler
döndürmelidir ([GoTip #49: Accept Interfaces, Return Concrete Types]).
Somut türler döndürmek, çağırıcının yalnızca önceden seçilmiş bir interface'de
tanımlanan alt küme değil, belirli uygulamanın her herkese açık metoduna ve
alanına erişmesine olanak tanır. Çağırıcı bu somut sonucu hala bir interface
bekleyen herhangi bir andere fonksiyona aktarabilir. Bazen bir interface döndürmek bir kapsülleme
için kabul edilebilir (ör. `error` interface'i) ve command, chaining,
factory ve [strategy](https://en.wikipedia.org/wiki/Strategy_pattern)
kalıpları gibi belirli yapılar için.

Interface'ler hakkında daha derin bir tartışma
[En İyi Uygulamalar'ın interface'ler bölümünde](best-practices#interfaces) bulunabilir.

[GoTip #78: Minimal Viable Interfaces]: index#gotip
[GoTip #49: Accept Interfaces, Return Concrete Types]: index#gotip
[testing RPC]: https://codelabs.developers.google.com/grpc/getting-started-grpc-go#3
[test double]: https://abseil.io/resources/swe-book/html/ch13.html
[public API]: https://abseil.io/resources/swe-book/html/ch12.html#test_via_public_apis

<a id="generics"></a>

### Generic

Generics (resmi olarak "[Type Parameters]" olarak adlandırılır) iş
gereksinimlerinizi karşıladıkları yerde izin verilir. Birçok uygulamada,
mevcut dil özelliklerini (slices, maps, interface'ler ve benzeri) kullanan
geleneksel bir yaklaşım, ek karmaşıklık olmadan aynı kadar iyi çalışır, bu
nedenle erken kullanımdan kaçının. [En az mekanizma][least mechanism]
hakkındaki tartışmaya bakın.

Generics kullanan dışa açık bir API tanıtırken, uygun şekilde
belgelendiğinden emin olun. Teşvik edici çalıştırılabilir [örnekler][examples]
eklemek şiddetle tavsiye edilir.

Üye elemanlarının türüyle ilgilenmeyen bir algoritma veya veri yapısı
uyguladığınız için generics kullanmayın. Pratikte yalnızca bir tür
somutlaştırılıyorsa, kodunuzu generics hiç kullanmadan o tür üzerinde
çalışacak şekilde başlatın. Daha sonra polymorphism eklemek, gereksiz olduğu
bulunan soyutlamayı kaldırmaya kıyasla daha kolay olacaktır.

Generics kullanarak alan-spesifik diller (DSLs) icat etmeyin. Özellikle,
okuyuculara önemli bir yük getirebilecek hata ele alma çerçeveleri
tanıtmaktan kaçının. Bunun yerine yerleşik [hata ele alma](#errors)
uygulamalarını tercih edin. Test için, daha az yararlı [test hatalarına][test failures]
yol açan [assertion kütüphanelerini][assertion libraries] veya çerçeveleri
tanıtmaktan özellikle kaçının.

Genel olarak:

- [Kod yazın, tür tasarlamayın][Write code, don't design types]. Robert
  Griesemer ve Ian Lance Taylor'ın GopherCon sunumundan.
- Birbirleriyle faydalı bir birleştiren interface'i paylaşan birkaç türünüz
  varsa, çözümü o interface'i kullanarak modellemeyi düşünün. Generics
  gerekli olmayabilir.
- Aksi takdirde, `any` türüne ve aşırı [tür değiştirmeye][type switching]
  güvenmek yerine, generics'i düşünün.

Ayrıca bakın:

- [Using Generics in Go], Ian Lance Taylor'ın sunumu

- Go web sayfasında [Generics eğitimi][Generics tutorial]

[Generics tutorial]: https://go.dev/doc/tutorial/generics
[Type Parameters]: https://go.dev/design/43651-type-parameters
[Using Generics in Go]: https://www.youtube.com/watch?v=nr8EpUO9jhw
[Write code, don't design types]: https://www.youtube.com/watch?v=Pa_e9EeCdy8&t=1250s

<a id="pass-values"></a>

### Değerleri geçirmek

<a id="TOC-PassValues"></a>

Birkaç byte kaydetmek için işaretçileri fonksiyon argümanları olarak geçirmeyin. Bir fonksiyon argümanını yalnızca `*x` olarak okuyorsa, bu argüman işaretçi olmamalıdır. Bu durumun yaygın örnekleri arasında bir dizeye (`*string`) veya bir arayüz değerine (`*io.Reader`) işaretçi geçirilmesi yer alır. Her iki durumda da değer kendisi sabit boyuttadır ve doğrudan geçirilebilir.

Bu öneri büyük struct'lara veya boyutu artabilecek küçük struct'lara uygulanmaz. Özellikle protocol buffer mesajları genellikle değer yerine işaretçi ile ele alınmalıdır. İşaretçi türü `proto.Message` arayüzünü karşılar (`proto.Marshal`, `protocmp.Transform` vb. tarafından kabul edilir) ve protocol buffer mesajları oldukça büyük olabilir ve zamanla daha da büyüyebilir.

<a id="receiver-type"></a>

### Alıcı türü

<a id="TOC-ReceiverType"></a>

Bir [metot alıcısı] normal bir fonksiyon parametresi gibi değer veya işaretçi olarak geçirilebilir. İkisi arasındaki seçim, metodun hangi [metot kümesi/kümeleri]ne ait olduğuna bağlıdır.

[metot alıcısı]: https://golang.org/ref/spec#Method_declarations
[metot kümesi/kümeleri]: https://golang.org/ref/spec#Method_sets

**Doğruluk hızdan veya basitlikten önce gelir.** İşaretçi değeri kullanmanız gereken durumlar vardır. Diğer durumlarda, büyük türler için işaretçiler tercih edin veya kodun nasıl büyüyeceğine dair iyi bir fikriniz yoksa geleceğe yönelik olarak işaretçi kullanın ve basit [eski usul veri] için değerleri kullanın.

Aşağıdaki liste her bir durumu daha ayrıntılı olarak açıklar:

- Alıcı bir slice ise ve metot slice'ı yeniden dilimlemiyorsa veya yeniden ayırmıyorsa, işaretçi yerine değer kullanın.

  ```go
  // İyi:
  type Buffer []byte

  func (b Buffer) Len() int { return len(b) }
  ```

- Metot alıcıyı değiştirmek zorundaysa, alıcı işaretçi olmalıdır.

  ```go
  // İyi:
  type Counter int

  func (c *Counter) Inc() { *c++ }

  // See https://pkg.go.dev/container/heap.
  type Queue []Item

  func (q *Queue) Push(x Item) { *q = append([]Item{x}, *q...) }
  ```

- Alıcı [güvenli bir şekilde kopyalanamayan](#copying) alanlar içeren bir struct ise, işaretçi alıcı kullanın. Yaygın örnekler [`sync.Mutex`] ve diğer eşzamanlama türleridir.

  ```go
  // İyi:
  type Counter struct {
      mu    sync.Mutex
      total int
  }

  func (c *Counter) Inc() {
      c.mu.Lock()
      defer c.mu.Unlock()
      c.total++
  }
  ```

  **İpucu:** Türün kopyalanmasının güvenli olup olmadığı hakkında bilgi için türün [Godoc]'una bakın.

- Alıcı "büyük" bir struct veya dizi ise, işaretçi alıcı daha verimli olabilir. Bir struct geçirmek, tüm alanlarını veya elemanlarını metodun argümanları olarak geçirmekle eşdeğerdir. Bu [değer ile geçirmek](#pass-values) için çok büyük görünüyorsa, işaretçi iyi bir seçimdir.

- Alıcıyı değiştiren diğer fonksiyonlarla eşzamanlı olarak çalışacak metotlar için, bu değişiklikler metodunuzdan görünmemeliyse değer kullanın; aksi takdirde işaretçi kullanın.

- Alıcı, herhangi bir elemanı değiştirilebilir bir şeye işaret eden bir struct veya dizi ise, mutabilitenin niyetini okuyucuya açıkça belirtmek için işaretçi alıcı tercih edin.

  ```go
  // İyi:
  type Counter struct {
      m *Metric
  }

  func (c *Counter) Inc() {
      c.m.Add(1)
  }
  ```

- Alıcı, değiştirilmesi gerekmeyen [yerleşik tür]den (ör. bir tamsayı veya dize) ise değer kullanın.

  ```go
  // İyi:
  type User string

  func (u User) String() { return string(u) }
  ```

- Alıcı bir harita, fonksiyon veya kanal ise, işaretçi yerine değer kullanın.

  ```go
  // İyi:
  // See https://pkg.go.dev/net/http#Header.
  type Header map[string][]string

  func (h Header) Add(key, value string) { /* omitted */ }
  ```

- Alıcı, değişken alanı olmayan ve işaretçisi olmayan doğal olarak değer türünde "küçük" bir dizi veya struct ise, değer alıcı genellikle doğru seçimdir.

  ```go
  // İyi:
  // See https://pkg.go.dev/time#Time.
  type Time struct { /* omitted */ }

  func (t Time) Add(d Duration) Time { /* omitted */ }
  ```

- Emin değilseniz, işaretçi alıcı kullanın.

Genel bir kural olarak, bir türün metotlarının ya tamamının işaretçi metodları ya da tamamının değer metodları olmasını tercih edin.

**Not:** Bir değerin veya işaretçinin bir fonksiyona geçirilmesinin performansı etkileyip etkilemediğine dair yanlış bilgiler vardır. Derleyici yığında hem değerlerin işaretçilerini geçirebilir hem de yığında değerleri kopyalayabilir, ancak bu hususların çoğu durumda kodun okunabilirliğinden ve doğruluğundan daha önemli olmaması gerekir. Performans gerçekten önemli olduğunda, bir yaklaşımın diğerinden daha iyi performans gösterdiğini belirlemeden önce her iki yaklaşımı da gerçekçi bir benchmark ile analiz etmek önemlidir.

[eski usul veri]: https://en.wikipedia.org/wiki/Passive_data_structure
[`sync.Mutex`]: https://pkg.go.dev/sync#Mutex
[yerleşik tür]: https://pkg.go.dev/builtin

<a id="switch-break"></a>

### `switch` ve `break`

<a id="TOC-SwitchBreak"></a>

`switch`ifadelerinin sonunda hedef etiketi olmayan `break` statements
kullanmayın; gereksizdirler. C ve Java'nın aksine, Go'daki `switch` blokları
otomatik olarak durur ve C tarzı davranışı elde etmek için `fallthrough`
ifadesine ihtiyaç vardır. Boş bir bloğun amacını netleştirmek istiyorsanız
`break` yerine yorum kullanın.

```go
// İyi:
switch x {
case "A", "B":
    buf.WriteString(x)
case "C":
    // switch statement dışında ele alınır
default:
    return fmt.Errorf("unknown value: %q", x)
}
```

```go
// Kötü:
switch x {
case "A", "B":
    buf.WriteString(x)
    break // bu break gereksizdir
case "C":
    break // bu break gereksizdir
default:
    return fmt.Errorf("unknown value: %q", x)
}
```

> **Not:** Eğer bir `switch` bloğu bir `for` döngüsünün içindeyse, `switch`
> içinde `break` kullanmak dıştaki `for` döngüsünden çıkmaz.
>
> ```go
> for {
>   switch x {
>   case "A":
>      break // switch'ten çıkar, döngüden değil
>   }
> }
> ```
>
> Döngüden çıkmak için `for` ifadesinde bir etiket kullanın:
>
> ```go
> loop:
>   for {
>     switch x {
>     case "A":
>        break loop // döngüden çıkar
>     }
>   }
> ```

<a id="synchronous-functions"></a>

### Eşzamanlı olmayan fonksiyonlar

<a id="TOC-SynchronousFunctions"></a>

Eşzamanlı olmayan fonksiyonlar sonuçlarını doğrudan döndürür ve geri dönmeden önce tüm geri çağırmaları veya kanal işlemlerini tamamlar. Eşzamanlı fonksiyonlar yerine eşzamanlı olmayan fonksiyonları tercih edin.

Eşzamanlı olmayan fonksiyonlar goroutine'leri bir çağrı içinde sınırlı tutar. Bu, ömürleri hakkında akıl yürütmeyi ve sızıntıları ile veri yarışlarını önlemeyi kolaylaştırır. Eşzamanlı olmayan fonksiyonlar ayrıca test etmesi daha kolaydır, çünkü çağrı yapan kişi girdi verebilir ve anket veya eşzamanlamaya ihtiyaç duymadan çıktıyı kontrol edebilir.

Gerekirse, çağrı yapan kişi fonksiyonu ayrı bir goroutine içinde çağırarak eşzamanlılık ekleyebilir. Ancak, çağrı yapan tarafta gereksiz eşzamanlılığı kaldırmak oldukça zordur (bazen imkansızdır).

Ayrıca bakın:

- "Rethinking Classical Concurrency Patterns", Bryan Mills tarafından sunum:
  [sunumlar][rethinking-slides], [video][rethinking-video]

<a id="type-aliases"></a>

### Tür takma adları

<a id="TOC-TypeAliases"></a>

Yeni bir tür tanımlamak için bir _tür tanımı_ `type T1 T2` kullanın.
Mevcut bir türe atıfta bulunmak için yeni bir tür tanımlamadan bir
[tür takma adı][type alias] `type T1 = T2` kullanın. Tür takma adları
nadir kullanılır; birincil kullanımları paketleri yeni kaynak kod
konumlarına taşımaya yardımcı olmaktır. Gerekli olmadığında tür
takma adı kullanmayın.

[*type alias*]: http://golang.org/ref/spec#Type_declarations

<a id="use-percent-q"></a>

### %q kullanma

<a id="TOC-UsePercentQ"></a>

Go'nun format fonksiyonları (`fmt.Printf` vb.) çift tırnak içinde yazdıran bir `%q` sözcüğü sunar.

```go
// İyi:
fmt.Printf("value %q looks like English text", someText)
```

`%s` kullanarak elle yapmaktan ziyade `%q` kullanmayı tercih edin:

```go
// Kötü:
fmt.Printf("value \"%s\" looks like English text", someText)
// Tek tırnaklarla elle sarmalamaktan da kaçının:
fmt.Printf("value '%s' looks like English text", someText)
```

İnsanlar için hazırlanan çıktılarda, girdi değeri boş olabileceğinde veya kontrol karakterleri içerebileceğinde `%q` kullanımı önerilir. Sessiz boş bir dizeyi fark etmek çok zor olabilirken, `""` bunu net bir şekilde gösterir.

<a id="use-any"></a>

### any kullanma

Go 1.18, `interface{}` için bir [takma ad] olarak `any` türünü tanıtır. Bir takma ad olduğu için `any`, birçok durumda `interface{}` ile eşdeğerdir ve diğerlerinde açık bir dönüşümle kolayca değiştirilebilir. Yeni kodda `any` kullanmayı tercih edin.

[takma ad]: https://go.googlesource.com/proposal/+/master/design/18130-type-alias.md

## Yaygın kütüphaneler

<a id="flags"></a>

### Bayraklar

<a id="TOC-Flags"></a>

Google kod tabanındaki Go programları [standart `flag` paketinin] bir iç
varyantını kullanır. Benzer bir arayüze sahiptir ancak Google iç
sistemleriyle iyi şekilde etkileşim kurar. Go binarilerindeki bayrak
isimleri kelimeleri ayırmak için alt çizgi kullanmalıdır, ancak bir
bayrağın değerini tutan değişkenler standart Go isim stilini ([karışık
harfler]) izlemelidir. Özellikle bayrak ismi snake case, değişken ismi
ise eşdeğer camel case olmalıdır.

```go
// İyi:
var (
    pollInterval = flag.Duration("poll_interval", time.Minute, "Interval to use for polling.")
)
```

```go
// Kötü:
var (
    poll_interval = flag.Int("pollIntervalSeconds", 60, "Interval to use for polling in seconds.")
)
```

Bayraklar yalnızca `package main` veya eşdeğerinde tanımlanmalıdır.

Genel amaçlı paketler, komut satırı arayüzüne geçmek yerine Go API'leri
kullanılarak yapılandırılmalıdır; bir kütüphane import etmenin yan etki
olarak yeni bayraklar dışa açmasına izin vermeyin. Yani, açık fonksiyon
argümanlarını veya struct alan atamalarını tercih edin; çok daha seyrek ve
en katı denetim altında dışa açık global değişkenleri kullanın. Bu kuralı
kırmak son derece nadir bir durum olmalıdır ve bayrak ismi, yapılandırdığı
paketi açıkça belirtmelidir.

Bayraklarınız global değişkenlerse, import bölümünden sonra kendi `var`
grubunuza yerleştirin.

Alt komutlar içeren [karmaşık CLI'lar] oluşturmak için en iyi uygulamalar
hakkında ek bir tartışma bulunmaktadır.

Ayrıca bakın:

- [Tip of the Week #45: Avoid Flags, Especially in Library Code][totw-45]
- [Go Tip #10: Configuration Structs and Flags](index#gotip)
- [Go Tip #80: Dependency Injection Principles](index#gotip)

[standard `flag` package]: https://golang.org/pkg/flag/
[mixed caps]: guide#mixed-caps
[complex CLIs]: best-practices#complex-clis
[totw-45]: https://abseil.io/tips/45

<a id="logging"></a>

### Günlükleme

Google kod tabanındaki Go programları standart [`log` paketinin] bir
varyantını kullanır. Benzer ama daha güçlü bir arayüze sahiptir ve Google
iç sistemleriyle iyi şekilde etkileşim kurar. Bu kütüphanenin açık kaynak
sürümü [paket `glog`] olarak mevcuttur ve açık kaynak Google projeleri bunu
kullanabilir, ancak bu kılavuz boyunca buna `log` olarak atıfta bulunur.

**Not:** Anormal program çıkışları için bu kütüphane, yığın iziyle birlikte
durmak için `log.Fatal` ve yığın izi olmadan durmak için `log.Exit` kullanır.
Standart kütüphanedeki gibi bir `log.Panic` fonksiyonu yoktur.

**İpucu:** `log.Info(v)`, `log.Infof("%v", v)` ile eşdeğerdir ve diğer
günlükleme seviyeleri için de aynıdır. Biçimlendirme yapmanız gerekmediğinde
biçimlendirme içermeyen sürümü tercih edin.

Ayrıca bakın:

- [Hataları günlükleme](best-practices#error-logging) ve
  [özel ayrıntı düzeyleri](best-practices#vlog) hakkında en iyi uygulamalar
- Log paketini ne zaman ve nasıl
  [programı durdurmak](best-practices#checks-and-panics) için kullanacağınız

[`log`]: https://pkg.go.dev/log
[`log/slog`]: https://pkg.go.dev/log/slog
[package `glog`]: https://pkg.go.dev/github.com/golang/glog
[`log.Exit`]: https://pkg.go.dev/github.com/golang/glog#Exit
[`log.Fatal`]: https://pkg.go.dev/github.com/golang/glog#Fatal

<a id="contexts"></a>

### Context

<a id="TOC-Contexts"></a>

[`context.Context`] türündeki değerler, güvenlik kimlik bilgileri, izleme
bilgileri, son tarihler ve iptal sinyallerini API ve süreç sınırları
boyunca taşır. Google kod tabanında iş parçacığı yerel deposu kullanan
C++ ve Java'nın aksine, Go programları context'leri gelen RPC'lerden ve
HTTP isteklerinden giden isteklere kadar tüm fonksiyon çağrısı zinciri
boyunca açıkça iletir.

[`context.Context`]: https://pkg.go.dev/context

Bir fonksiyona veya metoda geçirildiğinde, [`context.Context`] her zaman
birinci parametredir.

```go
func F(ctx context.Context /* other arguments */) {}
```

İstisnalar şunlardır:

- Bir HTTP işleyicide, context'in [`req.Context()`](https://pkg.go.dev/net/http#Request.Context) adresinden geldiği yerde.
- Akış RPC metodlarında, context'in akıştan geldiği yerde.

  gRPC akışını kullanan kod, `grpc.ServerStream` arayüzünü uygulayan
  oluşturulan sunucu türündeki `Context()` metodundan bir context'e erişir.
  [gRPC Oluşturulan Kod belgelerine](https://grpc.io/docs/languages/go/generated-code/) bakın.

- Test fonksiyonlarında (ör. `TestXXX`, `BenchmarkXXX`, `FuzzXXX`), context'in
  [`(testing.TB).Context()`](https://pkg.go.dev/testing#TB.Context) adresinden geldiği yerde.

- Diğer giriş noktaları fonksiyonlarında (böyle fonksiyonların örnekleri için
  aşağıya bakın), [`context.Background()`] kullanın.

  - Binary hedeflerde: `main`
  - Genel amaçlı kod ve kütüphanelerde: `init`

> **Not**: Bir çağrılma zincirinin ortasındaki kodun kendi temel
> context'ini [`context.Background()`] kullanarak oluşturması çok nadir
> görülür. Yanlış context olmadığı sürece her zaman çağırıcınızdan bir
> context almayı tercih edin.
>
> Sunucu kütüphaneleriyle (Google'ın Go için sunucu çerçevesindeki Stubby,
> gRPC veya HTTP uygulaması) karşılaşabilirsiniz; bunlar her istek için
> yeni bir context nesnesi oluşturur. Bu context'ler gelen istekten hemen
> doldurulur, böylece istek işleyicisine geçirildiğinde, context'e eklenen
> değerler müşteri çağrıcısı tarafından ağ sınırı boyunca iletilmiş olur.
> Ayrıca bu context'lerin ömrü isteğin ömrüyle sınırlıdır: istek
> tamamlandığında, context iptal edilir.
>
> Bir sunucu çerçevesi uygulamadığınız sürece, kütüphane kodunda
> [`context.Background()`] kullanarak context oluşturmamalısınız. Bunun
> yerine, mevcut bir context mevcutsa aşağıda bahsedilen context ayırmayı
> tercih edin. Giriş noktaları fonksiyonlarının dışında
> [`context.Background()`] gerektiğini düşünüyorsanız, uygulamaya geçmeden
> önce Google Go stil e-posta listesinde tartışın.

[`context.Context`] fonksiyonlarda birinci sırada gelme kuralı test
yardımcıları için de geçerlidir.

```go
// İyi:
func readTestFile(ctx context.Context, t *testing.T, path string) string {}
```

Struct türüne context üyesi eklemeyin. Bunun yerine, context'i iletmesi
gereken türün her metoduna bir context parametresi ekleyin. Tek istisna,
imzasının standart kütüphanedeki veya Google'ın kontrolü dışındaki üçüncü
parti bir kütüphanedeki bir arayüzle eşleşmesi gereken metotlardır. Böyle
durumlar çok nadirdir ve uygulamadan ve okunabilirlik incelemesinden önce
Google Go stil e-posta listesinde tartışılmalıdır.

**Not:** Go 1.24 bir [`(testing.TB).Context()`] metodu ekledi. Testlerde,
test tarafından kullanılan başlangıç [`context.Context`] değerini sağlamak
için [`context.Background()`] yerine [`(testing.TB).Context()`] kullanmayı
tercih edin. Yardımcı fonksiyonlar, ortam veya test sahte kurulumu ve test
fonksiyon gövdesinden çağrılan diğer fonksiyonlar bir context gerektiriyorsa,
bu açıkça geçirilmelidir.

[`(testing.TB).Context()`]: https://pkg.go.dev/testing#TB.Context

Google kod tabanında, ana context iptal edildikten sonra çalışabilen arka
plan işlemleri başlatması gereken kod, ayırmak için bir iç paket
kullanabilir. Açık kaynak alternatifi hakkındaki tartışmalar için
[issue #40221](https://github.com/golang/go/issues/40221) adresini takip edin.

Context'ler değişmez olduğundan, aynı son tarih, iptal sinyali, kimlik
bilgileri, ana iz ve benzerlerini paylaşan birden fazla çağrya aynı
context'i geçirmek sorun değildir.

Ayrıca bakın:

- [Context'ler ve struct'lar]

[`context.Background()`]: https://pkg.go.dev/context/#Background
[Contexts and structs]: https://go.dev/blog/context-and-structs

<a id="custom-contexts"></a>

### Özel context

Özel context türleri oluşturmayın veya fonksiyon imzalarında
[`context.Context`] dışında interface kullanmayın. Bu kuralın
istisnası yoktur.

Her takımın özel bir context'i olduğunu hayal edin. `p` paketinden `q`
paketine yapılan her fonksiyon çağrısı, `p` ve `q` paket çiftlerinin
tümü için `p.Context`'i nasıl dönüştüreceğini belirlemek zorunda kalır.
Bu, insanlar için uygulanamaz ve hataya yatkındır ve context parametreleri
ekleyen otomatik yeniden düzenlemeleri neredeyse imkansız hale getirir.

Etrafınıza aktarmanız gereken uygulama verileriniz varsa, bunu bir
parametreye, alıcıya, global değişkenlere veya gerçekten oraya aitse bir
`Context` değerine koyun. Kendi context türünüzü oluşturmak kabul
edilemez, çünkü bu Go ekibinin Go programlarının üretim ortamında düzgün
çalışmasını sağlama yeteneğini zayıflatır.

<a id="crypto-rand"></a>

### crypto/rand

<a id="TOC-CryptoRand"></a>

`math/rand` paketini anahtarlar oluşturmak için kullanmayın, hatta
atılabilir olanları bile. Tohumlanmamışsa, üreteç tamamen öngörülebilirdir.
`time.Nanoseconds()` ile tohumlanırsa, yalnızca birkaç bit entropi
bulunur. Bunun yerine `crypto/rand`'ın Reader'ını kullanın ve metin
gerekiyorsa hexadecimal veya base64'e yazdırın.

```go
// İyi:
import (
    "crypto/rand"
    // "encoding/base64"
    // "encoding/hex"
    "fmt"

    // ...
)

func Key() string {
    buf := make([]byte, 16)
    if _, err := rand.Read(buf); err != nil {
        log.Fatalf("Out of randomness, should never happen: %v", err)
    }
    return fmt.Sprintf("%x", buf)
    // or hex.EncodeToString(buf)
    // or base64.StdEncoding.EncodeToString(buf)
}
```

**Not:** `log.Fatalf` standart kütüphane log'u değildir. [#logging]'e bakın.

<a id="useful-test-failures"></a>

## Yararlı test hataları

<a id="TOC-UsefulTestFailures"></a>

Bir testin kaynağı okunmadan testin arızasının teşhis edilebilmesi
gerekir. Testler, aşağıdaki bilgileri ayrıntılandıran yararlı mesajlarla
başarısız olmalıdır:

- Arızaya neyin neden olduğu
- Hangi girdilerin hataya yol açtığı
- Gerçek sonuç
- Beklenen sonuç

Bu hedefe ulaşmak için belirli kurallar aşağıda açıklanmıştır.

<a id="assert"></a>

### Assertion kütüphaneleri

<a id="TOC-Assert"></a>

Testler için "assertion kütüphaneleri" oluşturmayın.

Assertion kütüphaneleri, bir test içinde doğrulama ve hata mesajı üretimini
birleştirmeye çalışan kütüphanelerdir (ancak aynı tuzaklar diğer test
yardımcıları için de geçerli olabilir). Test yardımcıları ile assertion
kütüphaneleri arasındaki fark hakkında daha fazla bilgi için
[en iyi uygulamalar](best-practices#test-functions) sayfasına bakın.

```go
// Kötü:
var obj BlogPost

assert.IsNotNil(t, "obj", obj)
assert.StringEq(t, "obj.Type", obj.Type, "blogPost")
assert.IntEq(t, "obj.Comments", obj.Comments, 2)
assert.StringNotEq(t, "obj.Body", obj.Body, "")
```

Assertion kütüphaneleri genellikle testi erken durdurur (`assert`
`t.Fatalf` veya `panic` çağrısı yaparsa) ya da testin doğru olan
taraflarıyla ilgili ilgili bilgileri atlar:

```go
// Kötü:
package assert

func IsNotNil(t *testing.T, name string, val any) {
    if val == nil {
        t.Fatalf("Data %s = nil, want not nil", name)
    }
}

func StringEq(t *testing.T, name, got, want string) {
    if got != want {
        t.Fatalf("Data %s = %q, want %q", name, got, want)
    }
}
```

Karmaşık assertion fonksiyonları genellikle [yararlı hata mesajları] ve test
fonksiyonundaki bağlamı sağlamaz. Çok fazla assertion fonksiyonu ve kütüphane
parçalanmış bir geliştirici deneyimine yol açar: hangi assertion kütüphanesini
kullanmalıyım, ne tür bir çıkış biçimi yaymalıdır, vb.? Parçalanma, özellikle
potansiyel downstream kırılmaları düzelmekten sorumlu olan kütüphane
sorumluları ve büyük ölçekli değişikliklerin yazarları için gereksiz kafa
karışıklığı yaratır. Test için bir alan-spesifik dil oluşturmak yerine, Go'nun
kendisini kullanın.

Assertion kütüphaneleri genellikle karşılaştırmaları ve eşitlik kontrollerini
ayırır. Bunun yerine [`cmp`] ve [`fmt`] gibi standart kütüphaneleri kullanmayı
tercih edin:

```go
// İyi:
var got BlogPost

want := BlogPost{
    Comments: 2,
    Body:     "Hello, world!",
}

if !cmp.Equal(got, want) {
    t.Errorf("Blog post = %v, want = %v", got, want)
}
```

Daha fazla alana özgü karşılaştırma yardımcıları için, `*testing.T` geçirip
hata raporlama metotlarını çağırmak yerine, testin hata mesajında
kullanılabilecek bir değer veya hata döndürmeyi tercih edin:

```go
// İyi:
func postLength(p BlogPost) int { return len(p.Body) }

func TestBlogPost_VeritableRant(t *testing.T) {
    post := BlogPost{Body: "I am Gunnery Sergeant Hartman, your senior drill instructor."}

    if got, want := postLength(post), 60; got != want {
        t.Errorf("Length of post = %v, want %v", got, want)
    }
}
```

**En İyi Uygulama:** `postLength` basit olmayan bir işlev olsaydı, onu doğrudan
test etmek mantıklı olurdu, onu kullanan testlerden bağımsız olarak.

Ayrıca bakın:

- [Eşitlik karşılaştırması ve farklar](#types-of-equality)
- [Farkları yazdırma](#print-diffs)
- Test yardımcıları ile assertion yardımcıları arasındaki fark hakkında daha fazla
  bilgi için [en iyi uygulamalar](best-practices#test-functions) sayfasına bakın
- [Go SSS]'de [test çerçeveleri] ve onların tercih edilmemesi hakkında bölüm

[useful failure messages]: #useful-test-failures
[`fmt`]: https://golang.org/pkg/fmt/
[marking test helpers]: #mark-test-helpers
[Go FAQ]: https://go.dev/doc/faq
[testing frameworks]: https://go.dev/doc/faq#testing_framework

<a id="identify-the-function"></a>

### Fonksiyonu tanımlama

Çoğu testte, hata mesajları başarısız olan fonksiyonun adını içermelidir, bu
test fonksiyonunun adından belliyse bile. Özellikle, hata mesajınız
`got %v, want %v` yerine `YourFunc(%v) = %v, want %v` olmalıdır.

<a id="identify-the-input"></a>

### Girdiyi tanımlama

Çoğu testte, hata mesajları kısa ise fonksiyon girdilerini içermelidir.
Girdilerin ilgili özellikleri açık değilse (örneğin, girdiler büyük veya
anlaşılmaz olduğu için), test durumlarınızı neyin test edildiğine ilişkin bir
açıklama ile adlandırmalı ve açıklamayı hata mesajınızın bir parçası olarak
yazdırmalısınız.

<a id="got-before-want"></a>

### Öncelikle got, sonra want

Test çıkışları, beklenen değeri yazdırmadan önce fonksiyonun döndürdüğü gerçek
değeri içermelidir. Test çıkışlarını yazdırmak için standart bir biçim
`YourFunc(%v) = %v, want %v` şeklindedir. "gerçek" ve "beklenen" yazdığınız
yerlerde, sırasıyla "got" ve "want" kelimelerini kullanmayı tercih edin.

Farklar için, yönlülük daha az açıktır ve bu nedenle hatanın yorumlanmasına
yardımcı olacak bir anahtar eklemek önemlidir.
[Farkları yazdırma bölümüne] bakın. Hata mesajlarınızda hangi fark sırasını
kullanırsanız kullanın, var olan kodun sıralama konusunda tutarsız olması
nedeniyle bunu hata mesajınızın bir parçası olarak açıkça belirtmelisiniz.

[section on printing diffs]: #print-diffs

<a id="compare-full-structures"></a>

### Tam yapı karşılaştırmaları

Fonksiyonunuz bir struct (veya slices, arrays ve maps gibi birden fazla alana
sahip herhangi bir veri türü) döndürüyorsa, struct'ın el yazması alan bazlı
karşılaştırmasını yapan test kodu yazmaktan kaçının. Bunun yerine, fonksiyonunuzun
döndürmesini beklediğiniz veriyi oluşturun ve doğrudan bir [derin karşılaştırma]
kullanarak karşılaştırın.

**Not:** Bu, veriniz testin niyetini karıştıran ilgisiz alanlar içeriyorsa
geçerli değildir.

Struct'ınızın yaklaşık (veya eşdeğer türde anlamsal) eşitlik için karşılaştırılması
gerekiyorsa veya eşitlik için karşılaştırılamayan alanlar içeriyorsa (örneğin,
alanlardan biri `io.Reader` ise), [`cmp.Diff`] veya [`cmp.Equal`] karşılaştırmasını
[`cmpopts`] seçenekleriyle (örn. [`cmpopts.IgnoreInterfaces`] ile ayarlamak
ihtiyaçlarınızı karşılayabilir
([örnek](https://play.golang.org/p/vrCUNVfxsvF)).

Fonksiyonunuz birden fazla dönüş değeri döndürüyorsa, bunları karşılaştırmadan
önce bir struct'a sarmalamanıza gerek yoktur. Yalnızca dönüş değerlerini tek tek
karşılaştırın ve yazdırın.

```go
// İyi:
val, multi, tail, err := strconv.UnquoteChar(`\"Fran & Freddie's Diner\"`, '"')
if err != nil {
  t.Fatalf(...)
}
if val != `"` {
  t.Errorf(...)
}
if multi {
  t.Errorf(...)
}
if tail != `Fran & Freddie's Diner"` {
  t.Errorf(...)
}
```

[derin karşılaştırma]: #types-of-equality
[`cmpopts`]: https://pkg.go.dev/github.com/google/go-cmp/cmp/cmpopts
[`cmpopts.IgnoreInterfaces`]: https://pkg.go.dev/github.com/google/go-cmp/cmp/cmpopts#IgnoreInterfaces

<a id="compare-stable-results"></a>

### Kararlı sonuçları karşılaştırma

Sahip olmadığınız bir paketin çıktı kararlılığına bağlı olabilecek sonuçları
karşılaştırmaktan kaçının. Bunun yerine, test bağımlılıklardaki değişikliklere
dirençli olan ve kararlı olan anlamsal olarak ilgili bilgileri karşılaştırmalıdır.
Biçimlendirilmiş bir dize veya serileştirilmiş baytlar döndüren işlevsellik için,
çıkışın kararlı olduğunu varsaymak genellikle güvenli değildir.

Örneğin, [`json.Marshal`] emits ettiği belirli baytları değiştirebilir (ve geçmişte
değiştirmiştir). JSON dizesi üzerinde dizgesel eşitlik yapan testler, `json`
paketinin baytları nasıl serileştirdiğini değiştirirse başarısız olabilir.
Bunun yerine, daha sağlam bir test JSON dizesinin içeriğini ayrıştırır ve onun
anlamsal olarak beklenen bazı veri yapılarına eşdeğer olduğunu garanti altına alır.

[`json.Marshal`]: https://golang.org/pkg/encoding/json/#Marshal

<a id="keep-going"></a>

### Devam edin

Testler, başarısızlık olsa bile mümkün olduğunca uzun süre devam etmelidir, böylece
tek bir çalıştırmada tüm başarısız kontroller yazdırılabilir. Bu şekilde,
başarısız testi düzeltmekte olan bir geliştirici, bir sonraki hatayı bulmak için
her hatayı düzelttikten sonra testi yeniden çalıştırması gerekmez.

Birden fazla farklı özelliği karşılaştırırken `t.Fatal` yerine `t.Error`
kullanmayı tercih edin. Fonksiyonun çıktısının birkaç farklı özelliğini
karşılaştırırken, bu karşılaştırmaların her biri için `t.Error` kullanın.

```go
// İyi:
gotMean, gotVariance, err := MyDistribution(input)
if err != nil {
  t.Fatalf("MyDistribution(%v) returned unexpected error: %v", input, err)
}
if diff := cmp.Diff(wantMean, gotMean); diff != "" {
  t.Errorf("MyDistribution(%v) returned unexpected difference in mean value (-want +got):\n%s", input, diff)
}
if diff := cmp.Diff(wantVariance, gotVariance); diff != "" {
  t.Errorf("MyDistribution(%v) returned unexpected difference in variance value (-want +got):\n%s", input, diff)
}
```

`t.Fatal` çağırmak, sonraki hatalar anlamsız olacaksa veya araştırmacıyı yanıltıyorsa,
bir hata veya çıktı uyumsuzluğu gibi beklenmeyen bir durumu raporlamak için
kullanışlıdır. Aşağıdaki kodun `t.Fatalf` ve _ardından_ `t.Errorf` çağırdığına
dikkat edin:

```go
// İyi:
gotEncoded := Encode(input)
if gotEncoded != wantEncoded {
  t.Fatalf("Encode(%q) = %q, want %q", input, gotEncoded, wantEncoded)
  // It doesn't make sense to decode from unexpected encoded input.
}
gotDecoded, err := Decode(gotEncoded)
if err != nil {
  t.Fatalf("Decode(%q) returned unexpected error: %v", gotEncoded, err)
}
if gotDecoded != input {
  t.Errorf("Decode(%q) = %q, want %q", gotEncoded, gotDecoded, input)
}
```

Tablo tabanlı testler için, alt testleri kullanmayı ve `t.Error` ve `continue`
yerine `t.Fatal` kullanmayı düşünün. Ayrıca
[GoTip #25: Subtests: Making Your Tests Lean](index#gotip)
sayfasına bakın.

**En iyi uygulama:** `t.Fatal`'ın ne zaman kullanılacağı hakkında daha fazla
tartışma için [en iyi uygulamalar](best-practices#t-fatal) sayfasına bakın.

<a id="types-of-equality"></a>

### Eşitlik karşılaştırması ve farklar

`==` operatörü eşitliği [dil tanımlı karşılaştırmalar] kullanarak değerlendirir.
Skaler değerler (sayılar, boole'lar vb.) değerlerine göre karşılaştırılır, ancak
yalnızca bazı struct'lar ve arayüzler bu şekilde karşılaştırılabilir. İşaretçiler,
değerlerin eşitliğine göre değil, gösterdikleri aynı değişkene göre
karşılaştırılır.

[`cmp`] paketi, `==` ile uygun şekilde işlenemeyen daha karmaşık veri yapılarını
karşılaştırabilir. Eşitlik karşılaştırması için [`cmp.Equal`] ve nesneler
arasında insan tarafından okunabilir bir fark elde etmek için [`cmp.Diff`] kullanın.

```go
// İyi:
want := &Doc{
    Type:     "blogPost",
    Comments: 2,
    Body:     "This is the post body.",
    Authors:  []string{"isaac", "albert", "emmy"},
}
if !cmp.Equal(got, want) {
    t.Errorf("AddPost() = %+v, want %+v", got, want)
}
```

Genel amaçlı bir karşılaştırma kütüphanesi olarak `cmp`, belirli türleri nasıl
karşılaştıracağını bilmeyebilir. Örneğin, protocol buffer mesajlarını yalnızca
[`protocmp.Transform`] seçeneği geçirilirse karşılaştırabilir.

<!-- The order of want and got here is deliberate. See comment in #print-diffs. -->

```go
// İyi:
if diff := cmp.Diff(want, got, protocmp.Transform()); diff != "" {
    t.Errorf("Foo() returned unexpected difference in protobuf messages (-want +got):\n%s", diff)
}
```

`cmp` paketi Go standart kütüphanesinin bir parçası olmasa da, Go ekibi tarafından
sürdürülmektedir ve zaman içinde kararlı eşitlik sonuçları üretmelidir.
Kullanıcı tarafından yapılandırılabilir ve çoğu karşılaştırma ihtiyacını
karşılamalıdır.

[dil tanımlı karşılaştırmalar]: http://golang.org/ref/spec#Comparison_operators
[`cmp`]: https://pkg.go.dev/github.com/google/go-cmp/cmp
[`cmp.Equal`]: https://pkg.go.dev/github.com/google/go-cmp/cmp#Equal
[`cmp.Diff`]: https://pkg.go.dev/github.com/google/go-cmp/cmp#Diff
[`protocmp.Transform`]: https://pkg.go.dev/google.golang.org/protobuf/testing/protocmp#Transform

Var olan kod aşağıdaki daha eski kütüphaneleri kullanabilir ve tutarlılık için
kullanmaya devam edebilir:

- [`pretty`] estetik olarak hoş fark raporları üretir. Ancak, aynı görsel
  temsile sahip değerleri kasıtlı olarak eşdeğer olarak değerlendirir.
  Özellikle `pretty`, nil diziler ile boş olanlar arasındaki farkları
  yakalamaz, aynı alanlara sahip farklı arayüz uygulamalarına duyarlı
  değildir ve bir struct değeriyle karşılaştırma temeli olarak iç içe
  geçmiş bir harita kullanmak mümkündür. Ayrıca bir fark üretmeden önce
  tüm değeri bir dizeye serileştirir ve bu nedenle büyük değerleri
  karşılaştırmak için iyi bir seçim değildir. Varsayılan olarak,
  dışa açılmamış alanları karşılaştırır, bu da bağımlılıklarınızdaki
  uygulama ayrıntılarındaki değişikliklere duyarlı olmasına neden olur.
  Bu nedenle, protobuf mesajlarında `pretty` kullanmak uygun değildir.

[`pretty`]: https://pkg.go.dev/github.com/kylelemons/godebug/pretty

Yeni kodlar için `cmp` kullanmayı tercih edin ve daha eski kodları uygulamak
mümkün olduğunda `cmp` kullanacak şekilde güncellemeyi düşünün.

Daha eski kodlar karmaşık yapıları karşılaştırmak için standart kütüphane
`reflect.DeepEqual` fonksiyonunu kullanabilir. `reflect.DeepEqual`, dışa
açılmamış alanlardaki ve diğer uygulama ayrıntılarındaki değişikliklere duyarlı
olduğundan eşitlik kontrolü için kullanılmamalıdır. `reflect.DeepEqual` kullanan
kod yukarıdaki kütüphanelerden birine güncellenmelidir.

**Not:** `cmp` paketi test için tasarlanmıştır, üretim kullanımı için değil.
Bu nedenle, bir karşılaştırmanın yanlış yapıldığını şüphelendiğinde, kullanıcıları
testi daha az kırılgan hale getirmeleri için yönlendirmek amacıyla panic yapabilir.
cmp'in panic eğilimi göz önüne alındığında, üretimde kullanılan kod için uygun
değildir, çünkü yanlış bir panic ölümcül olabilir.

<a id="level-of-detail"></a>

### Detay seviyesi

Geleneksel hata mesajı, çoğu Go testi için uygun olan `YourFunc(%v) = %v, want %v`
şeklindedir. Ancak daha fazla veya daha az ayrıntı gerektiren durumlar olabilir:

- Karmaşık etkileşimler yapan testler etkileşimleri de tanımlamalıdır. Örneğin,
  aynı `YourFunc` birkaç kez çağrılıyorsa, testi hangi çağrının başarısız
  olduğunu tanımlayın. Sistemin ek durumunu bilmek önemliyse, bunu hata
  çıktısına (veya en azından günlük dosyalarına) ekleyin.
- Veri, önemli miktarda tekrar içeren karmaşık bir struct ise, mesajda yalnızca
  önemli kısımları tanımlamak kabul edilebilir, ancak veriyi aşırı derecede
  gizlemeyin.
- Kurulum hataları aynı düzeyde ayrıntı gerektirmez. Bir test yardımcısı
  bir Spanner tablosunu dolduruyorsa ancak Spanner çökmüşse, veritabanına
  depolamayı planladığınız test girdisini dahil etmenize gerek yoktur.
  `t.Fatalf("Setup: Failed to set up test database: %s", err)` genellikle
  sorunu çözmek için yeterince yardımcıdır.

**İpucu:** Hata modunuzu geliştirme sırasında tetikleyin. Hata mesajının
nasıl göründüğünü ve bir sorumlunun hatayla etkili bir şekilde başa
çıkıp çıkamayacağını inceleyin.

Test girdilerini ve çıktılarını net bir şekilde çoğaltmak için bazı teknikler
vardır:

- Dize verisi yazdırırken, değerin önemli olduğunu ve kötü değerleri daha kolay
  tespit etmek için [`%q` genellikle yararlıdır](#use-percent-q).
- (Küçük) struct'ları yazdırırken `%+v`, `%v`'den daha yararlı olabilir.
- Daha büyük değerlerin doğrulaması başarısız olduğunda, [bir fark yazdırmak](#print-diffs)
  hatayı anlamayı kolaylaştırabilir.

<a id="print-diffs"></a>

### Farkları yazdırma

Fonksiyonunuz büyük bir çıktı döndürüyorsa, testiniz başarısız olduğunda hata
mesajını okuyan kişinin farkları bulması zor olabilir. Hem döndürülen değeri hem
de istenen değeri yazdırmak yerine bir fark oluşturun.

Böyle değerler için farkları hesaplamak için, özellikle yeni testler ve yeni kodlar
için [`cmp.Diff` tercih edilir`, ancak diğer araçlar da kullanılabilir. Her bir
fonksiyonun güçlü ve zayıf yönleri hakkında rehberlik için [eşitlik türlerine]
bakın.

- [`cmp.Diff`]

- [`pretty.Compare`]

Çok satırlı dize veya dize listelerini karşılaştırmak için [`diff`] paketini
kullanabilirsiniz. Bunu diğer tür farklar için bir yapı taşı olarak
kullanabilirsiniz.

[eşitlik türleri]: #types-of-equality
[`diff`]: https://pkg.go.dev/github.com/kylelemons/godebug/diff
[`pretty.Compare`]: https://pkg.go.dev/github.com/kylelemons/godebug/pretty#Compare

Hata mesajınıza farkın yönünü açıklayan bir metin ekleyin.

<!--
The reversed order of want and got in these examples is intentional, as this is
the prevailing order across the Google codebase. The lack of a stance on which
order to use is also intentional, as there is no consensus which is
"most readable."


-->

- `cmp`, `pretty` ve `diff` paketlerini kullanırken `diff (-want +got)` gibi bir
  şey iyidir (fonksiyona `(want, got)` geçirirseniz), çünkü format dizgenize
  eklediğiniz `-` ve `+` işaretleri, fark satırlarının başında görünen `-` ve
  `+` işaretleriyle eşleşecektir. Fonksiyona `(got, want)` geçirirseniz, doğru
  anahtar `(-got +want)` olacaktır.

- `messagediff` paketi farklı bir çıkış biçimi kullandığından, `diff (want -> got)`
  mesajı onu kullanırken uygundur (fonksiyona `(want, got)` geçirirseniz),
  çünkü ok yönü "değiştirilmiş" satırlardaki ok yönüyle eşleşecektir.

Fark birden fazla satıra yayılacağından, farkı yazdırmadan önce bir yeni satır
yazdırmalısınız.

<a id="test-error-semantics"></a>

### Test hata anlamları

Bir birim testi dizgi karşılaştırmaları yaptığında veya belirli girdiler için belirli hata türlerinin döndürülüp döndürülmediğini kontrol etmek için düz bir `cmp` kullandığında, bu hata mesajları gelecekte yeniden ifade edilirse testlerinizin kırılgan hale gelebileceğini görebilirsiniz. Bunun birim testinizi bir değişiklik dedektörüne dönüştürme potansiyeli olduğundan (bkz. [TotT: Değişiklik Dedektörü Testleri Zararlı Olarak Düşünülür][tott-350]), fonksiyonunuzun hangi tür hata döndürdüğünü kontrol etmek için dizgi karşılaştırması kullanmayın. Ancak, test edilen paketten gelen hata mesajlarının belirli özellikleri karşıladığını kontrol etmek için dizgi karşılaştırmaları kullanmak mümkündür, örneğin parametre adını içerdiğini.

Go'daki hata değerleri genellikle insan gözü için tasarlanmış bir bileşen ve anlamsal kontrol akışı için tasarlanmış bir bileşen içerir. Testler, güvenilir bir şekilde gözlemlenebilir anlamsal bilgiyi test etmeye çalışmalıdır, bunun yerine genellikle gelecekteki değişikliklere bağlı olan insan hata ayıklama bilgisini görüntülememelidir. Anlamsal anlama sahip hataların nasıl oluşturulacağına ilişkin yönerge için [hatalarla ilgili en iyi uygulamalara](best-practices#error-handling) bakın. Yeterli anlamsal bilgiye sahip bir hata kontrolünüz dışındaki bir bağımlılıktan geliyorsa, hata mesajını ayrıştırmaya güvenmek yerine, API'yi iyileştirmek için sahibine karşı bir hata raporu açmayı düşünün.

Birim testlerde, yalnızca bir hata oluşup oluşmadığıyla ilgilenmek yaygındır. Öyleyse, bir hata beklediğinizde hatanın nil olup olmadığını test etmek yeterlidir. Hatayı anlamsal olarak başka bir hata ile eşleştirmek istiyorsanız, [`errors.Is`] veya [`cmpopts.EquateErrors`] ile `cmp` kullanmayı düşünün.

> **Not:** Bir test [`cmpopts.EquateErrors`] kullanıyorsa ama tüm `wantErr`
> değerleri ya `nil` ya da `cmpopts.AnyError` ise, `cmp` kullanmak
> [gereksiz mekanizmadır](guide#least-mechanism). Want alanını `bool`
> yaparak kodu basitleştirin. Ardından `!=` ile basit bir karşılaştırma kullanabilirsiniz.
>
> ```go
> // İyi:
> err := f(test.input)
> if gotErr := err != nil; gotErr != test.wantErr {
>     t.Errorf("f(%q) = %v, want error presence = %v", test.input, err, test.wantErr)
> }
> ```

Ayrıca bakın
[Go İpucu #13: Kontrol İçin Hataları Tasarlama](index#gotip).

[tott-350]: https://testing.googleblog.com/2015/01/testing-on-toilet-change-detector-tests.html
[`cmpopts.EquateErrors`]: https://pkg.go.dev/github.com/google/go-cmp/cmp/cmpopts#EquateErrors
[`errors.Is`]: https://pkg.go.dev/errors#Is

<a id="test-structure"></a>

## Test yapısı

<a id="subtests"></a>

### Alt testler

Standart Go test kütüphanesi [alt testler tanımlama][define subtests] olanağı sunar. Bu, kurulum ve temizlikte esneklik, eşzamanlılığı kontrol etme ve test filtreleme sağlar. Alt testler faydalı olabilir (özellikle tablo tabanlı testler için), ancak bunları kullanmak zorunlu değildir. Ayrıca [alt testler hakkında Go blog yazısına](https://blog.golang.org/subtests) bakın.

Alt testler, başarıları veya başlangıç durumları için diğer durumların çalışmasına bağlı olmamalıdır, çünkü alt testlerin `go test -run` bayraklarıyla veya Bazel [test filtresi] ifadeleriyle ayrı ayrı çalıştırılabilmesi beklenir.

[define subtests]: https://pkg.go.dev/testing#hdr-Subtests_and_Sub_benchmarks
[test filter]: https://bazel.build/docs/user-manual#test-filter

<a id="subtest-names"></a>

#### Alt test isimleri

Alt testlerinizi test çıktısında okunabilir ve test filtreleme kullanan kullanıcılar için komut satırında faydalı olacak şekilde adlandırın. `t.Run` ile bir alt test oluşturduğunuzda, ilk argüman test için tanımlayıcı isim olarak kullanılır. Test sonuçlarının logları okuyan insanlar tarafından okunabilir olmasını sağlamak için, kaçınıldıktan sonra faydalı ve okunabilir kalacak alt test isimleri seçin. Alt test isimlerini düz metin açıklamasından ziyade bir fonksiyon tanımlayıcısı gibi düşünün.

Test çalıştırıcısı boşlukları alt çizgilerle değiştirir ve yazdırılamayan karakterleri escape eder. Test logları ile kaynak kod arasındaki doğru korelasyonu sağlamak için, alt test isimlerinde bu karakterleri kullanmaktan kaçınmanız önerilir.

Test veriniz daha uzun bir açıklamadan faydalanıyorsa, açıklamayı ayrı bir alana koymayı düşünün (belki `t.Log` kullanılarak veya hata mesajlarıyla birlikte yazdırılabilir).

Alt testler, [Go test çalıştırıcısına] veya Bazel [test filtresine] bayraklar kullanılarak ayrı ayrı çalıştırılabilir, bu yüzden hem tanımlayıcı hem de yazması kolay isimler seçin.

> **Uyarı:** Kesme işaretleri alt test isimlerinde özellikle sorunludur, çünkü [test filtreleri için özel anlamları] vardır.
>
> > ```sh
> > # Kötü:
> > # Assuming TestTime and t.Run("America/New_York", ...)
> > bazel test :mytest --test_filter="Time/New_York"    # Runs nothing!
> > bazel test :mytest --test_filter="Time//New_York"   # Correct, but awkward.
> > ```

Fonksiyonun [girdilerini tanımlamak](#identify-the-input) için, testin hata mesajlarına dahil edin, böylece test çalıştırıcısı tarafından escape edilmezler.

```go
// İyi:
func TestTranslate(t *testing.T) {
    data := []struct {
        name, desc, srcLang, dstLang, srcText, wantDstText string
    }{
        {
            name:        "hu=en_bug-1234",
            desc:        "regression test following bug 1234. contact: cleese",
            srcLang:     "hu",
            srcText:     "cigarettát és egy öngyújtót kérek",
            dstLang:     "en",
            wantDstText: "cigarettes and a lighter please",
        }, // ...
    }
    for _, d := range data {
        t.Run(d.name, func(t *testing.T) {
            got := Translate(d.srcLang, d.dstLang, d.srcText)
            if got != d.wantDstText {
                t.Errorf("%s\nTranslate(%q, %q, %q) = %q, want %q",
                    d.desc, d.srcLang, d.dstLang, d.srcText, got, d.wantDstText)
            }
        })
    }
}
```

Bunlardan kaçınmanız gereken birkaç örnek:

```go
// Kötü:
// Too wordy.
t.Run("check that there is no mention of scratched records or hovercrafts", ...)
// Slashes cause problems on the command line.
t.Run("AM/PM confusion", ...)
```

Ayrıca bakın
[Go İpucu #117: Alt Test İsimleri](index#gotip).

[Go test runner]: https://golang.org/cmd/go/#hdr-Testing_flags
[identify the inputs]: #identify-the-input
[special meaning for test filters]: https://blog.golang.org/subtests#:~:text=Perhaps%20a%20bit,match%20any%20tests

<a id="table-driven-tests"></a>

### Tablo tabanlı testler

Birçok farklı test durumu benzer test mantığıyla test edilebildiğinde
tablo tabanlı testler kullanın.

- Bir fonksiyonun fiili çıktısının beklenen çıktıya eşit olup olmadığını test ederken. Örneğin, [`fmt.Sprintf`'nin birçok testi] veya aşağıdaki minimum kod parçacığı.
- Bir fonksiyonun çıktılarının her zaman aynı değişmezler kümesine uyup uymadığını test ederken. Örneğin, [`net.Dial` için testler].

[tests of `fmt.Sprintf`]: https://cs.opensource.google/go/go/+/master:src/fmt/fmt_test.go
[tests for `net.Dial`]: https://cs.opensource.google/go/go/+/master:src/net/dial_test.go;l=318;drc=5b606a9d2b7649532fe25794fa6b99bd24e7697c

İşte tablo tabanlı testin minimal yapısı. Gerekirse farklı isimler kullanabilir veya alt testler ya da kurulum ve temizlik fonksiyonları gibi ek tesisler ekleyebilirsiniz. Her zaman [yararlı test hatalarını](#useful-test-failures) aklınızda tutun.

```go
// İyi:
func TestCompare(t *testing.T) {
    compareTests := []struct {
        a, b string
        want int
    }{
        {"", "", 0},
        {"a", "", 1},
        {"", "a", -1},
        {"abc", "abc", 0},
        {"ab", "abc", -1},
        {"abc", "ab", 1},
        {"x", "ab", 1},
        {"ab", "x", -1},
        {"x", "a", 1},
        {"b", "x", -1},
        // test runtime·memeq's chunked implementation
        {"abcdefgh", "abcdefgh", 0},
        {"abcdefghi", "abcdefghi", 0},
        {"abcdefghi", "abcdefghj", -1},
    }

    for _, test := range compareTests {
        got := Compare(test.a, test.b)
        if got != test.want {
            t.Errorf("Compare(%q, %q) = %v, want %v", test.a, test.b, got, test.want)
        }
    }
}
```

**Not**: Bu örnekteki hata mesajları, [fonksiyonu tanımlama](#identify-the-function) ve [girdiyi tanımlama](#identify-the-input) yönergelerini yerine getirir. Satırı [sayısal olarak tanımlamaya](#table-tests-identifying-the-row) gerek yoktur.

Bazı test durumlarının diğer test durumlarından farklı mantıkla kontrol edilmesi gerektiğinde, [GoTip #50: Ayrık Tablo Testleri]'nde açıklandığı gibi birden fazla test fonksiyonu yazmak uygundur.

Ek test durumları basit olduğunda (ör. temel hata kontrolü) ve tablo testinin döngü gövdesinde koşullu bir kod akışı oluşturmadığında, mevcut teste bu durumu dahil etmek mümkündür, ancak bu tür mantığı kullanırken dikkatli olun. Bugün basit başlayan bir şey, sürdürülemez bir şeye organik olarak büyüyebilir.

For example:

```go
func TestDivide(t *testing.T) {
    tests := []struct {
        dividend, divisor int
        want              int
        wantErr           bool
    }{
        {
            dividend: 4,
            divisor:  2,
            want:     2,
        },
        {
            dividend: 10,
            divisor:  2,
            want:     5,
        },
        {
            dividend: 1,
            divisor:  0,
            wantErr:  true,
        },
    }

    for _, test := range tests {
        got, err := Divide(test.dividend, test.divisor)
        if (err != nil) != test.wantErr {
            t.Errorf("Divide(%d, %d) error = %v, want error presence = %t", test.dividend, test.divisor, err, test.wantErr)
        }

        // In this example, we're only testing the value result when the tested function didn't fail.
        if err != nil {
            continue
        }

        if got != test.want {
            t.Errorf("Divide(%d, %d) = %d, want %d", test.dividend, test.divisor, got, test.want)
        }
    }
}
```

Test kodunuzdaki daha karmaşık mantık, tablo testi girdi parametrelerine dayanan koşullu farklılıklara dayalı karmaşık hata kontrolü gibi, tablodaki her bir girişin girdilere dayalı özel mantığa sahip olduğunda [anlaşılması zor](guide#maintainability) olabilir. Test durumlarının farklı mantığa sahip ama aynı kuruluma sahip olması durumunda, tek bir test fonksiyonu içindeki bir [alt testler](#subtests) dizisi daha okunabilir olabilir. Bir test yardımcısı da test gövdesinin okunabilirliğini korumak için test kurulumunu basitleştirmek amacıyla faydalı olabilir.

Tablo tabanlı testleri birden fazla test fonksiyonuyla birleştirebilirsiniz. Örneğin, bir fonksiyonun çıktısının beklenen çıktıyla tam olarak eşleştiğini ve fonksiyonun geçersiz bir girdi için nil olmayan bir hata döndürdüğünü test ederken, iki ayrı tablo tabanlı test fonksiyonu yazmak en iyi yaklaşımdır: biri normal hata dışı çıktılar için, diğeri hata çıktıları için.

[GoTip #50: Disjoint Table Tests]: index#gotip

<a id="table-tests-data-driven"></a>

#### Veri tabanlı test durumları

Tablo test satırları bazen karmaşık hale gelebilir ve satır değerleri test durumunun içindeki koşullu davranışı belirleyebilir. Test durumları arasındaki tekrarlamadan gelen ek açıklık, okunabilirlik için gereklidir.

```go
// İyi:
type decodeCase struct {
    name   string
    input  string
    output string
    err    error
}

func TestDecode(t *testing.T) {
    // setupCodex is slow as it creates a real Codex for the test.
    codex := setupCodex(t)

    var tests []decodeCase // rows omitted for brevity

    for _, test := range tests {
        t.Run(test.name, func(t *testing.T) {
            output, err := Decode(test.input, codex)
            if got, want := output, test.output; got != want {
                t.Errorf("Decode(%q) = %v, want %v", test.input, got, want)
            }
            if got, want := err, test.err; !cmp.Equal(got, want) {
                t.Errorf("Decode(%q) err %q, want %q", test.input, got, want)
            }
        })
    }
}

func TestDecodeWithFake(t *testing.T) {
    // A fakeCodex is a fast approximation of a real Codex.
    codex := newFakeCodex()

    var tests []decodeCase // rows omitted for brevity

    for _, test := range tests {
        t.Run(test.name, func(t *testing.T) {
            output, err := Decode(test.input, codex)
            if got, want := output, test.output; got != want {
                t.Errorf("Decode(%q) = %v, want %v", test.input, got, want)
            }
            if got, want := err, test.err; !cmp.Equal(got, want) {
                t.Errorf("Decode(%q) err %q, want %q", test.input, got, want)
            }
        })
    }
}
```

Aşağıdaki karşıt örnekte, her test durumunda hangi `Codex` türünün kullanıldığının ayırt edilmesinin ne kadar zor olduğuna dikkat edin. (Vurgulanan kısımlar, [TotT: Veri Tabanlı Tuzaklar!][tott-97] tavsiyesine aykırıdır.)

```go
// Kötü:
type decodeCase struct {
  name   string
  input  string
  codex  testCodex
  output string
  err    error
}

type testCodex int

const (
  fake testCodex = iota
  prod
)

func TestDecode(t *testing.T) {
  var tests []decodeCase // rows omitted for brevity

  for _, test := tests {
    t.Run(test.name, func(t *testing.T) {
      var codex Codex
      switch test.codex {
      case fake:
        codex = newFakeCodex()
      case prod:
        codex = setupCodex(t)
      default:
        t.Fatalf("Unknown codex type: %v", codex)
      }
      output, err := Decode(test.input, codex)
      if got, want := output, test.output; got != want {
        t.Errorf("Decode(%q) = %q, want %q", test.input, got, want)
      }
      if got, want := err, test.err; !cmp.Equal(got, want) {
        t.Errorf("Decode(%q) err %q, want %q", test.input, got, want)
      }
    })
  }
}
```

[tott-97]: https://testing.googleblog.com/2008/09/tott-data-driven-traps.html

<a id="table-tests-identifying-the-row"></a>

#### Satırı tanımlama

Testlerinizi adlandırmak veya girdileri yazdırmak yerine, test tablosundaki testin indeksini kullanmayın. Kimse hangi test durumunun başarısız olduğunu bulmak için test tablonuzu gidip girişleri saymak istemez.

```go
// Kötü:
tests := []struct {
    input, want string
}{
    {"hello", "HELLO"},
    {"wORld", "WORLD"},
}
for i, d := range tests {
    if strings.ToUpper(d.input) != d.want {
        t.Errorf("Failed on case #%d", i)
    }
}
```

Test struct'ınıza bir test açıklaması ekleyin ve hata mesajlarıyla birlikte yazdırın. Alt testler kullandığınızda, alt test isminiz satırı tanımlamada etkili olmalıdır.

**Önemli:** `t.Run` çıktıyı ve çalışmayı kapsam alsa bile, her zaman [girdiyi tanımlamanız](#identify-the-input) gerekir. Tablo test satır isimleri [alt test adlandırma](#subtest-names) yönergelerine uymalıdır.

[identify the input]: #identify-the-input
[subtest naming]: #subtest-names

<a id="mark-test-helpers"></a>

### Test yardımcıları

Bir test yardımcısı, kurulum veya temizlik görevi yerine getiren bir fonksiyondur. Test yardımcılarında meydana gelen tüm hataların ortam hataları olması beklenir (test edilen koddan değil) — örneğin, test veritabanının bu makinede boş port kalmadığı için başlatılamaması gibi.

Bir `*testing.T` geçiriyorsanız, test yardımcısındaki hataları yardımcının çağrıldığı satıra atfetmek için [`t.Helper`] çağırın. Bu parametre, varsa bir [bağlam](#contexts) parametresinden sonra ve kalan tüm parametrelerden önce gelmelidir.

```go
// İyi:
func TestSomeFunction(t *testing.T) {
    golden := readFile(t, "testdata/golden-result.txt")
    // ... tests against golden ...
}

// readFile returns the contents of a data file.
// It must only be called from the same goroutine as started the test.
func readFile(t *testing.T, filename string) string {
    t.Helper()
    contents, err := runfiles.ReadFile(filename)
    if err != nil {
        t.Fatal(err)
    }
    return string(contents)
}
```

Bu kalıbı, bir test hatasına ve ona yol açan koşullar arasındaki bağlantıyı gizlediğinde kullanmayın. Özellikle, [assertion kütüphaneleri](#assert) hakkındaki yönerge hala geçerlidir ve [`t.Helper`] bu tür kütüphaneleri uygulamak için kullanılmamalıdır.

**İpucu:** Test yardımcıları ile assertion yardımcıları arasındaki ayrım hakkında daha fazla bilgi için [en iyi uygulamalara](best-practices#test-functions) bakın.

Yukarıdaki öneri `*testing.T` ile ilgili olsa da, benchmark ve fuzz yardımcıları için de çoğu öneri aynıdır.

[`t.Helper`]: https://pkg.go.dev/testing#T.Helper

<a id="test-package"></a>

### Test paketi

<a id="TOC-TestPackage"></a>

<a id="test-same-package"></a>

#### Aynı pakette testler

Testler, test edilen kod ile aynı pakette tanımlanabilir.

Aynı pakette bir test yazmak için:

- Testleri bir `foo_test.go` dosyasına yerleştirin
- Test dosyası için `package foo` kullanın
- Test edilecek paketi açıkça import etmeyin

```build
# İyi:
go_library(
    name = "foo",
    srcs = ["foo.go"],
    deps = [
        ...
    ],
)

go_test(
    name = "foo_test",
    size = "small",
    srcs = ["foo_test.go"],
    library = ":foo",
    deps = [
        ...
    ],
)
```

Aynı paketteki bir test, paketteki dışa açık olmayan tanımlayıcılara erişebilir. Bu daha iyi test kapsamı ve daha özlü testler sağlayabilir. Testte bildirilen [örneklerin](#examples) kullanıcının kodunda kullanması gereken paket isimlerine sahip olmayacağını unutmayın.

[`library`]: https://github.com/bazelbuild/rules_go/blob/master/docs/go/core/rules.md#go_library
[examples]: #examples

<a id="test-different-package"></a>

#### Farklı bir pakette testler

Testlerin her zaman test edilen kod ile aynı pakette tanımlanması uygun veya mümkün değildir. Bu durumlarda, `_test` son ekine sahip bir paket adı kullanın. Bu, [paket isimleri](#package-names) kuralının "alt çizgi yok" kuralının bir istisnasıdır. Örneğin:

- Bir entegrasyon testinin ait olduğu belirgin bir kütüphanesi olmadığında

  ```go
  // İyi:
  package gmailintegration_test

  import "testing"
  ```

- Aynı pakette testleri tanımlamanın döngüsel bağımlılıklara yol açtığı durumlarda

  ```go
  // İyi:
  package fireworks_test

  import (
    "fireworks"
    "fireworkstestutil" // fireworkstestutil also imports fireworks
  )
  ```

<a id="use-package-testing"></a>

### `testing` paketini kullanma

Go standart kütüphanesi [`testing` paketini] sağlar. Bu, Google kod
tabanındaki Go kodu için izin verilen tek test çerçevesidir. Özellikle,
[assertion kütüphaneleri](#assert) ve üçüncü parti test çerçeveleri izin
verilmez.

`testing` paketi, iyi testler yazmak için minimal ama eksiksiz bir
işlevsellik seti sağlar:

- Üst düzey testler
- Benchmark'lar
- [Çalıştırılabilir örnekler](https://blog.golang.org/examples)
- Alt testler
- Loglama
- Hatalar ve kritik hatalar

Bunlar, [bileşik literal] ve [if-with-initializer] sözdizimi gibi temel dil
özellikleriyle uyumlu çalışacak şekilde tasarlanmıştır ve test yazarlarının
[açık, okunabilir ve sürdürülebilir testler] yazmasını sağlar.

[`testing` package]: https://pkg.go.dev/testing
[composite literal]: https://go.dev/ref/spec#Composite_literals
[if-with-initializer]: https://go.dev/ref/spec#If_statements

<a id="non-decisions"></a>

## Karar dışı konular

Bir stil kılavuzu tüm konular için olumlu reçeteleri listeleyemez,
tavsiye vermediği tüm konuları da listeleyemez. Bununla birlikte,
okunabilirlik topluluğunun daha önce tartıştığı ve uzlaşmaya
varamadığı birkaç konu vardır.

- **Sıfır değeriyle yerel değişken başlatma**. `var i int` ve `i := 0`
  eşdeğerdir. Ayrıca [başlatma en iyi uygulamaları] sayfasına bakın.
- **Boş bileşik literal vs. `new` veya `make`**. `&File{}` ve `new(File)`
  eşdeğerdir. `map[string]bool{}` ve `make(map[string]bool)` da eşdeğerdir.
  Ayrıca [bileşik bildirim en iyi uygulamaları] sayfasına bakın.
- **cmp.Diff çağrılarında got, want argüman sıralaması**. Yerel olarak tutarlı
  olun ve hata mesajınıza [bir gösterge ekleyin](#print-diffs).
- **Biçimlendirilmemiş dizgilerde `errors.New` vs `fmt.Errorf`**.
  `errors.New("foo")` ve `fmt.Errorf("foo")` birbirinin yerine kullanılabilir.

Özel durumlar nedeniyle tekrar ortaya çıkarsa, okunabilirlik mentörü isteğe
bağlı bir yorum yapabilir, ancak genel olarak yazar tercih ettiği stili seçmekte
serbesttir.

Doğal olarak, stil kılavuzunda ele alınmayan bir konunun daha fazla tartışmaya
ihtiyacı varsa, yazarlar sormaktan çekinmemelidir -- ister özel incelemede
isterse dahili mesaj panolarında.

[composite declaration best practices]: best-practices#vardeclcomposite
[initialization best practices]: best-practices#vardeclinitialization

<!--

-->
