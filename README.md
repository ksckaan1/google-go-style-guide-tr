# Go Stili

https://google.github.io/styleguide/go

[Genel Bakış](README.md) | [Kılavuz](guide.md) | [Stil Kararları](decisions.md) |
[En İyi Uygulamalar](best-practices.md)

<!--

-->

{% raw %}

<a id="about"></a>

## Hakkında

Go Stil Kılavuzu ve ilişik dokümanlar, okunabilir ve idiomatik Go kodu yazmak için geçerli en iyi yaklaşımları belgelemektedir. Stil Kılavuzu'na mutlak olarak uyulması amaçlanmamakta ve bu dokümanlar asla kapsamlı olmayacaktır. Amacımız, dilin yeni kullanıcılarının yaygın hatalardan kaçınabilmesi için okunabilir Go yazma konusundaki tahminleri en aza indirmektir. Stil Kılavuzu ayrıca Google'da Go kodu gözden geçiren herkesin verdiği stil önerilerini birleştirmek için de kullanılır.

| Doküman                | Bağlantı                                              | Ana Kitle                | [Normatif] | [Kanonical] |
| ---------------------- | ----------------------------------------------------- | ------------------------ | ---------- | ----------- |
| **Stil Kılavuzu**      | guide          | Herkes                   | Evet       | Evet        |
| **Stil Kararları**     | decisions      | Okunabilirlik Mentörleri | Evet       | Hayır       |
| **En İyi Uygulamalar** | best-practices | İlgilenen herkes         | Hayır      | Hayır       |

[Normatif]: #normative
[Kanonical]: #canonical

<a id="docs"></a>

### Dokümanlar

1.  **[Stil Kılavuzu](guide.md)**, Google'da Go stilinin temelini çizer. Bu doküman kesin niteliktedir ve Stil Kararları ile En İyi Uygulamalardaki önerilerin temelini oluşturur.

1.  **[Stil Kararları](decisions.md)**, belirli stil noktalarına ilişkin kararları özetleyen ve gerektiğinde kararların arkasındaki mantığı tartışan daha ayrıntılı bir dokümandır.

    Bu kararlar yeni veriler, yeni dil özellikleri, yeni kütüphaneler veya ortaya çıkan kalıplara bağlı olarak zaman zaman değişebilir, ancak Google'daki bireysel Go programcılarının bu dokümanla güncel kalması beklenmez.

1.  **[En İyi Uygulamalar](best-practices.md)**, zaman içinde ortaya çıkan ve yaygın sorunları çözen, iyi okunan ve bakım ihtiyaçlarına karşı dayanıklı olan bazı kalıpları belgeler.

    Bu en iyi uygulamalar kanonical değildir, ancak Google'daki Go programcılarının kod tabanını tutarlı ve homojen kılmak için bunları mümkün olduğunca kullanmaları teşvik edilir.

Bu dokümanlar aşağıdakileri amaçlar:

- Alternatif stilleri değerlendirmek için bir dizi ilke üzerinde anlaşmak
- Go stilinde netleşmiş konuları kodlamak
- Go deyimleri için belge ve kanonical örnekler sunmak
- Çeşitli stil kararlarının artılarını ve eksilerini belgelemek
- Go okunabilirliği incelemelerinde sürprizleri en aza indirmeye yardımcı olmak
- Okunabilirlik mentörlerinin tutarlı terminoloji ve rehberlik kullanmasına yardımcı olmak

Bu dokümanlar aşağıdakileri **amaçlamaz**:

- Okunabilirlik incelemesinde verilebilecek yorumların kapsamlı bir listesi olmak
- Herkesin her zaman hatırlaması ve uyması beklenen tüm kuralları listelemek
- Dil özelliklerinin ve stilin kullanımında iyi yargıya alternatif olmak
- Stil farklılıklarından kurtulmak için yapılması gereken geniş çaplı değişiklikleri haklı çıkarmak

Bir Go programcısından diğerine ve bir takımın kod tabanından diğerine her zaman farklılıklar olacaktır. Ancak, kod tabanımızın mümkün olduğunca tutarlı olması Google ve Alphabet'in en iyi çıkarınadır. (Tutarlılık hakkında daha fazla bilgi için [kılavuza](guide.md#consistency) bakın.) Bu amaçla, gerekli gördüğünüz şekilde stil iyileştirmeleri yapmaktan çekinmeyin, ancak her bulduğunuz Stil Kılavuzu ihlali üzerinde fazla durmanıza gerek yoktur. Özellikle, bu dokümanlar zaman içinde değişebilir ve bu, mevcut kod tabanlarında fazladan değişiklik yapmak için bir neden değildir; yeni kodu en son en iyi uygulamalara göre yazmanız ve yakındaki sorunları zaman içinde ele almanız yeterlidir.

Stil konularının doğası gereği kişisel olduğunu ve her zaman doğal ödünler olduğunu kabul etmek önemlidir. Bu dokümanlardaki rehberliğin çoğu öznel olmakla birlikte, `gofmt`'de olduğu gibi sağladıkları homojenliğin önemli bir değeri vardır. Bu nedenle, stil önerileri uygun tartışma olmadan değiştirilmeyecek ve Google'daki Go programcılarının katılmasalar bile stil kılavuzuna uymaları teşvik edilecektir.

<a id="definitions"></a>

## Tanımlar

Stil dokümanlarında kullanılan aşağıdaki kelimeler aşağıda tanımlanmıştır:

- **Kanonical**: Kesin ve kalıcı kurallar belirler <a id="kanonical"></a>

  Bu dokümanlarda "kanonical", tüm kodun (eski ve yeni) uyması gereken bir standart olarak kabul edilen ve zaman içinde önemli ölçüde değişmesi beklenmeyen bir şeyi tanımlamak için kullanılır. Kanonical belgelerdeki ilkeler hem yazarlar hem de gözden geçirenler tarafından anlaşılmalıdır, bu nedenle kanonical bir belgeye dahil edilen her şeyin yüksek bir standardı karşılaması gerekir. Bu nedenle, kanonical belgeler genellikle daha kısadır ve kanonical olmayan belgelere göre stilin daha az unsurlarını belirler.

  https://google.github.io/styleguide/go#canonical

- **Normatif**: Tutarlılığı belirlemeyi amaçlar <a id="normative"></a>

  Bu dokümanlarda "normatif", Go kod inceleyicileri tarafından kullanılan, önerilerin, terminolojinin ve gerekçelerin tutarlı olmasını sağlamak amacıyla üzerinde anlaşılan bir stil unsurunu tanımlamak için kullanılır. Bu unsurlar zaman içinde değişebilir ve bu dokümanlar inceleyicilerin tutarlı ve güncel kalabilmesi için bu değişiklikleri yansıtacaktır. Go kodunun yazarlarının normatif belgelerle aşina olması beklenmez, ancak bu belgeler okunabilirlik incelemelerinde sıklıkla referans olarak kullanılacaktır.

  https://google.github.io/styleguide/go#normative

- **İdiomatik**: Yaygın ve tanıdık <a id="idiomatik"></a>

  Bu dokümanlarda "idiomatik", Go kodunda yaygın olan ve tanınması kolay bir kalıp haline gelmiş şeyleri ifade etmek için kullanılır. Genel olarak, her ikisi de bağlamda aynı amaca hizmet ediyorsa, bir idiomatik kalıp, idiomatik olmayan bir şeye tercih edilmelidir, çünkü okuyucular için en tanıdık olan budur.

  https://google.github.io/styleguide/go#idiomatic

<a id="references"></a>

## Ek referanslar

Bu kılavuz, okuyucunun [Effective Go] ile aşina olduğunu varsayar, çünkü bu tüm Go topluluğu genelinde Go kodu için ortak bir temel sağlar.

Aşağıda, Go stili hakkında kendi kendini eğitmek isteyenler ve incelemelerinde daha fazla bağlantı verilebilir bağlam sağlamak isteyen gözden geçirenler için bazı ek kaynaklar bulunmaktadır. Go okunabilirlik sürecine katılanların bu kaynaklarla aşina olması beklenmez, ancak okunabilirlik incelemelerinde bağlam olarak ortaya çıkabilir.

[Effective Go]: https://go.dev/doc/effective_go

**Dış Referanslar**

- [Go Dil Özellikleri](https://go.dev/ref/spec)
- [Go SSS](https://go.dev/doc/faq)
- [Go Bellek Modeli](https://go.dev/ref/mem)
- [Go Veri Yapıları](https://research.swtch.com/godata)
- [Go Arayüzleri](https://research.swtch.com/interfaces)
- [Go Atasözleri](https://go-proverbs.github.io/)

- <a id="gotip"></a Go İpuçları Bölümleri - takipte kalın.

- <a id="unit-testing-practices"></a> Birim Test Uygulamaları - takipte kalın.

**İlgili Testing-on-the-Toilet Makaleleri**

- [TotT: Tanımlayıcı İsimlendirme][tott-431]
- [TotT: Durum Testi ile Etkileşim Testi][tott-281]
- [TotT: Etkili Test][tott-324]
- [TotT: Risk Odaklı Test][tott-329]
- [TotT: Değişiklik Algılayıcı Testlerin Zararlı Olduğuna Dair][tott-350]

[tott-431]: https://testing.googleblog.com/2017/10/code-health-identifiernamingpostforworl.html
[tott-281]: https://testing.googleblog.com/2013/03/testing-on-toilet-testing-state-vs.html
[tott-324]: https://testing.googleblog.com/2014/05/testing-on-toilet-effective-testing.html
[tott-329]: https://testing.googleblog.com/2014/05/testing-on-toilet-risk-driven-testing.html
[tott-350]: https://testing.googleblog.com/2015/01/testing-on-toilet-change-detector-tests.html

**Ek Dış Yazılar**

- [Go ve Dogma](https://research.swtch.com/dogma)
- [Az olan üstel olarak daha fazladır](https://commandcenter.blogspot.com/2012/06/less-is-exponentially-more.html)
- [Esmerelda'nın Hayal Gücü](https://commandcenter.blogspot.com/2011/12/esmereldas-imagination.html)
- [Ayrıştırma için düzenli ifadeler](https://commandcenter.blogspot.com/2011/08/regular-expressions-in-lexing-and.html)
- [Gofmt'nin stili kimsenin favorisi değildir, ancak Gofmt herkesin favorisidir](https://www.youtube.com/watch?v=PAAkCSZUG1c&t=8m43s)
  (YouTube)

<!--

-->

{% endraw %}
