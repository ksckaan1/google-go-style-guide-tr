<!--* toc_depth: 3 *-->

# Go Stil Kılavuzu

https://google.github.io/styleguide/go/guide

[Genel Bakış](index) | [Kılavuz](guide) | [Kararlar](decisions) |
[En İyi Uygulamalar](best-practices)

<!--

-->

**Not:** Bu, Google'daki [Go Stili](index)'ni özetleyen belgeler dizisinin bir
parçasıdır. Bu belge **[normatif](index#normative) ve
[kanonik](index#canonical)**'dir. Daha fazla bilgi için [genel bakışa](index#about) bakınız.

<a id="principles"></a>

## Stil ilkeleri

Okunabilir Go kodu yazma konusunda nasıl düşünüleceğini özetleyen birkaç
temel ilke bulunmaktadır. Aşağıda, önem sırasına göre okunabilir kodun
nitelikleri yer almaktadır:

1.  **[Netlik]**: Kodun amacı ve gerekçesi okuyucu için açıktır.
1.  **[Basitlik]**: Kod hedefine mümkün olan en basit şekilde ulaşır.
1.  **[Kısalık]**: Kodun sinyal-gürültü oranı yüksektir.
1.  **[Sürdürülebilirlik]**: Kod kolayca sürdürülebilecek şekilde yazılmıştır.
1.  **[Tutarlılık]**: Kod, genel Google kod tabanıyla tutarlıdır.

[Clarity]: #clarity
[Simplicity]: #simplicity
[Concision]: #concision
[Maintainability]: #maintainability
[Consistency]: #consistency

<a id="clarity"></a>

### Netlik

Okunabilirliğin temel amacı, okuyucu için net olan kod üretmektir.

Netlik, temel olarak etkili isimlendirme, faydalı yorumlar ve verimli kod
organizasyonuyla sağlanır.

Netlik, kodun yazarı değil, okuyucunun perspektifinden değerlendirilmelidir.
Kodun okunması yazılmasından daha önemlidir. Kodda netliğin iki farklı
boyutu vardır:

- [Kod aslında ne yapıyor?](#clarity-purpose)
- [Kod neden bunu yapıyor?](#clarity-rationale)

<a id="clarity-purpose"></a>

#### Kod aslında ne yapıyor?

Go, kodun ne yaptığını görmeyi nispeten basit kılacak şekilde tasarlanmıştır.
Belirsiz durumlarda veya bir okuyucunun kodu anlamak için önceden bilgiye
ihtiyacı olduğu durumlarda, gelecekteki okuyucular için kodun amacını daha net
hale getirmek zaman ayırmaya değerdir. Örneğin, aşağıdakiler faydalı olabilir:

- Daha açıklayıcı değişken isimleri kullanmak
- Ek yorumlar eklemek
- Kodu boşluklar ve yorumlarla ayırmak
- Kodu daha modüler hale getirmek için ayrı fonksiyonlara/yöntemlere bölmek

Burada her duruma uyan tek bir yaklaşım olmasa da, Go kodu geliştirirken
netliği önceliklendirmek önemlidir.

<a id="clarity-rationale"></a>

#### Kod neden bunu yapıyor?

Kodun gerekçesi genellikle değişkenlerin, fonksiyonların, yöntemlerin veya
paketlerin isimleriyle yeterince aktarılır. Bu olmadığı durumlarda, yorum
eklemek önemlidir. "Neden?" özellikle kod, okuyucunun aşina olmayabileceği
nüanslar içerdiğinde önemlidir, örneğin:

- Dildeki bir nüans, örneğin bir closure bir döngü değişkenini yakalayacak,
  ama closure birçok satır uzakta olabilir
- İş mantığındaki bir nüans, örneğin gerçek kullanıcı ile bir kullanıcıyı
  taklit eden kişi arasındaki ayrımı yapması gereken bir erişim kontrolü

Bir API, doğru kullanılmak için dikkat gerektirebilir. Örneğin, bir kod parçası
performans nedenleriyle karmaşık ve takip etmesi zor olabilir, veya karmaşık bir
matematiksel işlemler dizisi type dönüşümlerini beklenmedik bir şekilde
kullanabilir. Bu durumlarda ve daha birçoklarında, eşlik eden yorumların ve
belgelerin bu yönleri açıklaması önemlidir; böylece gelecekteki sürdürücüler
hata yapmaz ve okuyucular kodu tersine mühendislik yapmadan anlayabilir.

Ayrıca, netlik sağlamaya yönelik bazı girişimlerin (ekstra yorum eklemek gibi)
aslında kodun amacını gizleyebileceğini unutmamak gerekir; bu, gereksiz yığılma
yaratır, kodun zaten söylediklerini tekrar eder, kodla çelişir veya yorumları
güncel tutma bakım yükü ekler. Kodun kendi konuşmasına izin verin (örneğin,
sembol isimlerini kendinden açıklayıcı yaparak) yerine gereksiz yorumlar
eklemekten kaçının. Yorumlar genellikle neden bir şeyin yapıldığını açıklamalı,
kodun ne yaptığını değil.

Google kod tabanı büyük ölçüde tutarlıdır. Genellikle, öne çıkan kod (örneğin,
tanınmayan bir kalıp kullanarak) iyi bir nedenle, genellikle performans için
bunu yapar. Bu özelliği sürdürmek, okuyucuların yeni bir kod parçasını okurken
dikkatlerini nereye yoğunlaştıracaklarını netleştirmek için önemlidir.

Standart kütüphane, bu ilkenin pratikte birçok örneğini içerir. Bunlar
arasında:

- [`package sort`](https://cs.opensource.google/go/go/+/refs/tags/go1.19.2:src/sort/sort.go)
  içindeki sürdürücü yorumları.
- Aynı paketteki [iyi çalıştırılabilir örnekler](https://cs.opensource.google/go/go/+/refs/tags/go1.19.2:src/sort/example_search_test.go),
  hem kullanıcılara (bunlar [godoc'ta görünür](https://pkg.go.dev/sort#pkg-examples))
  hem de sürdürücülere (bunlar [testlerin parçası olarak çalışır](decisions#examples))
  fayda sağlar.
- [`strings.Cut`](https://pkg.go.dev/strings#Cut) sadece dört satır koddan
  oluşur, ama [çağrı sitelerinin netliğini ve doğruluğunu artırır](https://github.com/golang/go/issues/46336).

<a id="simplicity"></a>

### Basitlik

Go kodunuz, kullanan, okuyan ve sürdüren kişiler için basit olmalıdır.

Go kodu, hem davranış hem de performans açısından hedeflerine ulaşan en basit
şekilde yazılmalıdır. Google Go kod tabanında, basit kod:

- Yukarıdan aşağıya okunması kolaydır
- Zaten ne yaptığınızı bildiğinizi varsaymaz
- Tüm önceki kodu ezberleyebileceğinizi varsaymaz
- Gereksiz soyutlama katmanları içermez
- Sıradan bir şeye dikkat çeken isimlere sahip değildir
- Değerlerin ve kararların yayılmasını okuyucu için netleştirir
- Gelecekte sapmayı önlemek için kodun ne yaptığını değil neden yaptığını
  açıklayan yorumlara sahiptir
- Kendi başına duran belgelendirmeye sahiptir
- Faydalı hatalara ve faydalı test hatalarına sahiptir
- Genellikle "zeki" kodla karşılıklı olarak dışlayıcı olabilir

Kod basitliği ile API kullanım basitliği arasında dengelemeler ortaya
çıkabilir. Örneğin, son kullanıcının API'yi daha kolay doğru çağırabilmesi
için kodun daha karmaşık olmasına değer olabilir. Buna karşılık, kodun basit ve
anlaşılması kolay kalması için son kullanıcıya biraz ek iş bırakmaya da değer
olabilir.

Kod karmaşıklık gerektirdiğinde, karmaşıklık bilinçli olarak eklenmelidir. Bu
genellikle ekstra performans gerektiğinde veya belirli bir kütüphane veya
servisin birden fazla farklı kullanıcısı olduğunda gereklidir. Karmaşıklık
haklı görülebilir, ancak müşterilerin ve gelecekteki sürdürücülerin karmaşıklığı
anlayabilmesi ve üzerinde gezinebilmesi için eşlik eden belgelerle birlikte
gelmelidir. Bu, kodun hem "basit" hem de "karmaşık" bir kullanım yolu varsa,
özellikle doğru kullanımını gösteren testler ve örneklerle desteklenmelidir.

Bu ilke, karmaşık kodun Go'da yazılamayacağı veya yazılmaması gerektiği veya Go
kodunun karmaşık olamayacağı anlamına gelmez. Gereksiz karmaşıklıktan kaçınan,
böylece karmaşıklık ortaya çıktığında ilgili kodun anlaşılması ve
sürdürülmesi için özen gerektirdiğini gösteren bir kod tabanı hedefliyoruz.
İdeali, gerekçeyi açıklayan ve alınması gereken özeni belirten eşlik eden
yorumların olmasıdır. Bu genellikle kodu performans için optimize ederken
ortaya çıkar; bunu yapmak genellikle bir goroutine ömrü boyunca bir bufferı
önceden ayırma ve yeniden kullanma gibi daha karmaşık bir yaklaşım gerektirir.
Bir sürdürücü bunu gördüğünde, bu bir ipucu olmalıdır: ilgili kod performans
kritiktir ve bu, gelecekteki değişiklikler yaparken alınan özeni
etkilemelidir. Öte yandan, gereksiz kullanılırsa bu karmaşıklık, kodu
gelecekte okumak veya değiştirmek isteyenler için bir yüktür.

Kodu çok karmaşık çıktığında, amacı basit olmalıysa, bu genellikle aynı şeyi
başarmanın daha basit bir yolu olup olmadığını görmek için uygulamayı tekrar
gözden geçirmenin bir sinyalidir.

<a id="least-mechanism"></a>

#### En az mekanizma

Aynı fikri ifade etmenin birkaç yolu varsa, en standart araçları kullananı
tercih edin. Gelişmiş mekanizmalar genellikle mevcuttur, ancak neden olmadan
kullanılmamalıdır. Koda ihtiyaç duyulduğunda karmaşıklık eklemek kolaydır,
oysa mevcut karmaşıklığın gereksiz bulunduktan sonra kaldırılması çok daha
zordur.

1.  Durumunuz için yeterliyse temel bir dil yapısı (örneğin bir channel, slice, map,
    döngü veya struct) kullanmayı hedefleyin.
2.  Yoksa, standart kütüphane içinde bir araç arayın (bir HTTP client veya
    bir şablon motoru gibi).
3.  Son olarak, yeni bir bağımlılık getirmeden veya kendi başınıza oluşturmadan
    önce Google kod tabanında yeterli olan temel bir kütüphane olup olmadığını
    düşünün.

Örnek olarak, testlerde geçersiz kılınması gereken varsayılan değere sahip bir
değişkene bağlı bir flag içeren üretim kodunu düşünün. Programın komut satırı
arayüzünün kendisini test etme niyetinde değilseniz (örneğin `os/exec` ile),
bağlı değeri `flag.Set` kullanmak yerine doğrudan geçersiz kılmak daha basit
ve bu nedenle tercih edilir.

Benzer şekilde, bir kod parçası küme üyelik kontrolü gerektiriyorsa, boolean
değerli bir map (örneğin `map[string]bool`) genellikle yeterlidir. Küme benzeri
tipler ve işlevsellik sağlayan kütüphaneler, sadece map ile mümkün olmayan veya
aşırı karmaşık olan daha karmaşık işlemler gerektiriyorsa kullanılmalıdır.

<a id="concision"></a>

### Kısalık

Kısa Go kodunun sinyal-gürültü oranı yüksektir. İlgili ayrıntıları ayırt etmek
kolaydır ve isimlendirme ve yapı, okuyucuyu bu ayrıntılarda yönlendirir.

Herhangi bir anda en önemli ayrıntıları ortaya çıkarmanın önünde durabilecek
birçok şey vardır:

- Tekrarlayan kod
- Gereksiz sözdizimi
- [Belirsiz isimler](#naming)
- Gereksiz soyutlama
- Boşluk

Tekrarlayan kod, her neredeyse aynı bölüm arasındaki farkları özellikle
gizler ve okuyucunun değişimleri bulmak için benzer kod satırlarını görsel olarak
karşılaştırmasını gerektirir. [Tablo tabanlı testler], her tekrarın önemli
ayrıntılarından ortak kodu verimli bir şekilde ayırt edebilen bir mekanizmanın
iyi bir örneğidir, ancak tabloda hangi parçaların dahil edileceği seçimi,
tablonun anlaşılmasını ne kadar kolaylaştırdığı üzerinde etkisi olacaktır.

Kodu yapılandırmanın birden fazla yolunu değerlendirirken, hangi yolun önemli
ayrıntıları en belirgin hale getirdiğini düşünmeye değer.

Ortak kod yapılarını ve deyimleri anlamak ve kullanmak da yüksek sinyal-gürültü
oranını korumak için önemlidir. Örneğin, aşağıdaki kod bloğu [hata işlemede]
çok yaygındır ve okuyucu bu bloğun amacını hızla anlayabilir.

```go
// İyi:
if err := doSomething(); err != nil {
    // ...
}
```

Kod buna çok benziyor ama ince bir şekilde farklıysa, okuyucu farkı fark etmeyebilir.
Bu tür durumlarda, hata kontrolüne dikkat çekmek için yorum ekleyerek hata kontrolünün
sinyalini kasıtlı olarak "artırmaya" değer.

```go
// İyi:
if err := doSomething(); err == nil { // hata YOKSA
    // ...
}
```

[Table-driven testing]: https://go.dev/wiki/TableDrivenTests
[error handling]: https://go.dev/blog/errors-are-values
["boosting"]: best-practices#signal-boost

<a id="maintainability"></a>

### Sürdürülebilirlik

Kod, yazıldığından çok daha fazla kez düzenlenir. Okunabilir kod, sadece nasıl
çalıştığını anlamaya çalışan bir okuyucuya değil, aynı zamanda kodu değiştirmesi
gereken programcıya da mantıklı gelir. Netlik anahtardır.

Sürdürülebilir kod:

- Gelecekteki bir programcının doğru bir şekilde değiştirmesi kolaydır
- Nazikçe büyümeye devam edebilecek şekilde yapılandırılmış API'lara sahiptir
- Varsayımları konusunda nettir ve kodun yapısına değil, problemin yapısına
  karşılık gelen soyutlamaları seçer
- Gereksiz bağımlılıklardan kaçınır ve kullanılmayan özellikler içermez
- Vaat edilen davranışların korunduğunu ve önemli mantığın doğru olduğunu
  sağlamak için kapsamlı bir test paketine sahiptir ve testler, hata durumunda
  net, uygulanabilir tanılamalar sağlar

Tanımları gereği kullanıldıkları bağlamdan bilgiyi kaldıran interface ve type
gibi soyutlamalar kullanırken, yeterli fayda sağladıklarından emin olmak
önemlidir. Düzenleyiciler ve IDE'ler, somut bir type kullanıldığında doğrudan
bir yöntem tanımına bağlanabilir ve karşılık gelen belgeleri gösterebilir,
ancak yalnızca bir interface tanımına atıfta bulunabilirler. Interface'ler
güçlü bir araçtır, ancak bir bedeli vardır; sürdürücünün interface'i doğru
kullanabilmesi için temel uygulamanın ayrıntılarını anlaması gerekebilir ve bu,
interface belgelerinde veya çağrı noktasında açıklanmalıdır.

Sürdürülebilir kod, ayrıca kolayca göz ardı edilebilecek yerlerde önemli
ayrıntıları gizlemekten kaçınır. Örneğin, aşağıdaki kod satırlarının her
birinde, tek bir karakterin varlığı veya yokluğu satırı anlamak için kritiktir:

```go
// Kötü:
// = yerine := kullanmak bu satırı tamamen değiştirebilir.
if user, err = db.UserByID(userID); err != nil {
    // ...
}
```

```go
// Kötü:
// Bu satırın ortasındaki ! karakterini kaçırmak çok kolaydır.
leap := (year%4 == 0) && (!(year%100 == 0) || (year%400 == 0))
```

Bunların ikisi de incorrect değildir, ancak ikisi de daha açık bir şekilde
yazılabilir veya önemli davranışı dikkat çeken bir eşlik eden yoruma sahip
olabilir:

```go
// İyi:
u, err := db.UserByID(userID)
if err != nil {
    return fmt.Errorf("invalid origin user: %s", err)
}
user = u
```

```go
// İyi:
// Miladi takvimdeki artık yıllar sadece year%4 == 0 değildir.
// Bkz. https://en.wikipedia.org/wiki/Leap_year#Algorithm.
var (
    leap4   = year%4 == 0
    leap100 = year%100 == 0
    leap400 = year%400 == 0
)
leap := leap4 && (!leap100 || leap400)
```

Aynı şekilde, kritik bir mantığı veya önemli bir kenar durumunu gizleyen bir
yardımcı fonksiyon, gelecekteki bir değişikliğin bunu düzgün bir şekilde
hesaba katmasını kolayca zorlaştırabilir.

Öngörülebilir isimler, sürdürülebilir kodun bir başka özelliğidir. Bir
paketin kullanıcısı veya bir kod parçasının sürdürücüsü, belirli bir bağlamda
bir değişkenin, yöntemin veya fonksiyonun ismini tahmin edebilmelidir. Aynı
kavramlar için fonksiyon parametreleri ve alıcı isimleri genellikle aynı ismi
paylaşmalıdır; hem belgeleri anlaşılır tutmak hem de kodu yeniden düzenlemeyi
en az yük ile kolaylaştırmak için.

Sürdürülebilir kod, bağımlılıklarını (hem zorunlu hem de açıkça belirtilen)
en aza indirir. Daha az pakete bağımlı olmak, davranışı etkileyebilecek daha
az kod satırı anlamına gelir. Dahili veya belgelenmemiş davranışlara
bağımlılıklardan kaçınmak, bu davranışlar gelecekte değiştiğinde bakım yükü
oluşturma olasılığını azaltır.

Kodu nasıl yapılandıracağınızı veya yazacağınızı düşünürken, kodun zamanla
nasıl gelişebileceğini düşünmeye zaman ayırmaya değer. Belirli bir yaklaşım,
daha kolay ve güvenli gelecekteki değişikliklere daha elverişliyse, bu genellikle
iyi bir dengelemedir, hatta biraz daha karmaşık bir tasarım anlamına gelse bile.

<a id="consistency"></a>

### Tutarlılık

Tutarlı kod, geniş kod tabanı genelinde, bir takım veya paket bağlamında ve
hatta tek bir dosya içinde benzer kodla aynı görünüşe, hisse ve davranışa
sahiptir.

Tutarlılık endişeleri yukarıdaki ilkelerin hiçbirini override etmez, ancak
beraberlik bozulması gerekiyorsa, genellikle tutarlılığın lehine bozmak
faydalıdır.

Bir paket içindeki tutarlılık, genellikle en yakın zamanda önemli olan
tutarlılık seviyesidir. Aynı probleme bir paket içinde birden fazla yaklaşımla
yaklaşılması veya aynı kavramın bir dosya içinde birçok isme sahip olması çok
rahatsız edici olabilir. Ancak, bu bile belgelenmiş stil ilkelerini veya
küresel tutarlılığı override etmemelidir.

<a id="core"></a>

## Temel kurallar

Bu kurallar, tüm Go kodunun uyması beklenen en önemli Go stili yönlerini
toplar. Bu ilkelerin, okunabilirlik kazanıldığı zamana kadar öğrenilmesi ve
takip edilmesi beklenmektedir. Bunların sık sık değişmesi beklenmemektedir ve
yeni eklemelerin yüksek bir barı aşması gerekecektir.

Aşağıdaki kurallar, tüm topluluk genelinde Go kodu için ortak bir temel
sağlayan [Effective Go]'daki önerileri genişletmektedir.

[Effective Go]: https://go.dev/doc/effective_go

<a id="formatting"></a>

### Biçimlendirme

Tüm Go kaynak dosyaları, `gofmt` aracı tarafından üretilen biçime uymalıdır.
Bu biçim, Google kodtabanında bir presubmit kontrolü ile zorunlu kılınmaktadır.
[Üretilen kod] genellikle biçimsel olarak da biçimlendirilmelidir (örneğin
[`format.Source`]'ı kullanarak), çünkü Code Search'te de taranabilir.

[Generated code]: https://docs.bazel.build/versions/main/be/general.html#genrule
[`format.Source`]: https://pkg.go.dev/go/format#Source

<a id="mixed-caps"></a>

### MixedCaps

Go kaynak kodu, çok kelimeli isimler yazarken alt çizgiler (snake case) yerine
`MixedCaps` veya `mixedCaps` (camel case) kullanır.

Bu, diğer dillerdeki gelenekleri kırdığında bile geçerlidir. Örneğin, bir sabit
export edilmişse `MaxLength` (not `MAX_LENGTH`), export edilmemişse `maxLength`
(not `max_length`) olmalıdır.

Yerel değişkenler, ilk büyük harf seçimi söz konusu olduğunda [export edilmemiş]
sayılır.

<!--#include file="/go/g3doc/style/includes/special-name-exception.md"-->

[unexported]: https://go.dev/ref/spec#Exported_identifiers

<a id="line-length"></a>

### Satır uzunluğu

Go kaynak kodu için sabit bir satır uzunluğu yoktur. Bir satır çok uzun
görünüyorsa, onu ayırmak yerine yeniden düzenlemeyi tercih edin. Zaten pratik
olabildiğince kısaysa, satırın uzun kalmasına izin verilmelidir.

Satırı bölmeyin:

- Bir [girinti değişikliğinden](decisions#indentation-confusion) önce
  (örneğin, fonksiyon bildirimi, koşul)
- Uzun bir dizeyi (örneğin bir URL) daha kısa birkaç satıra sığdırmak için

<a id="naming"></a>

### İsimlendirme

İsimlendirme, bilimden ziyade bir sanattır. Go'da isimler birçok diğer dile
göre biraz daha kısadır, ancak aynı [genel kurallar] geçerlidir. İsimler:

- Kullanıldığında [tekrarlayan](decisions#repetition) hissettirmemelidir
- Bağlamı göz önünde bulundurmalıdır
- Zaten açık olan kavramları tekrarlamamalıdır

İsimlendirme hakkında daha spesifik yönergeleri [kararlar](decisions#naming)
bölümünde bulabilirsiniz.

[general guidelines]: https://testing.googleblog.com/2017/10/code-health-identifiernamingpostforworl.html

<a id="local-consistency"></a>

### Yerel tutarlılık

Stil kılavuzu belirli bir stil noktasında hiçbir şey söylemediğinde, yazarlar
tercih ettikleri stili seçmekte özgürdür, ancak yakın çevredeki kod (genellikle
aynı dosya veya paket içinde, ancak bazen bir takım veya proje dizini içinde)
konuda tutarlı bir duruş almışsa bu geçerli değildir.

**Geçerli** yerel stil değerlendirmelerinin örnekleri:

- Hataların biçimlendirilmiş baskısı için `%s` veya `%v` kullanımı
- Mutexler yerine bufferlı channel'ların kullanımı

**Geçersiz** yerel stil değerlendirmelerinin örnekleri:

- Kod için satır uzunluğu kısıtlamaları
- Assertion tabanlı test kütüphanelerinin kullanımı

Yerel stil stil kılavuzuyla uyuşmuyorsa ama okunabilirlik etkisi tek bir
dosyayla sınırlıysa, genellikle tutarlı bir düzeltmenin ilgili CL'nin kapsamı
dışında olacağı bir kod incelemesinde ortaya çıkacaktır. Bu noktada, düzeltmeyi
izlemek için bir hata kaydı açmak uygundur.

Bir değişiklik mevcut bir stil sapmasını daha da kötüleştirecekse, onu daha
fazla API yüzeyinde ortaya çıkaracaksa, sapmanın bulunduğu dosya sayısını
artıracaksa veya gerçek bir hata tanıtacaksa, yerel tutarlılık artık yeni kod
için stil kılavuzunu ihlal etmek için geçerli bir gerekçe değildir. Bu
durumlarda, yazarın aynı CL'de mevcut kod tabanını temizlemesi, mevcut CL'den
önce bir yeniden düzenleme yapması veya en azından yerel sorunu daha da
kötüleştirmeyen bir alternatif bulması uygundur.

<!--

-->
