<div align="center">

# HobbitZip
### Ölçülebilir performansa odaklanan kayıpsız sıkıştırma araştırması

<img src="./assets/hero.svg" alt="HobbitZip — kayıpsız sıkıştırma araştırması" width="100%" />

**Sıkıştırma Araştırması** · **Performans Mühendisliği** · **Tekrarlanabilir Değerlendirme**

[English](./README.md) · [Deutsch](./README.de.md) · [Türkçe](./README.tr.md)

</div>

---

## Genel Bakış

**HobbitZip**, kayıpsız veri sıkıştırmaya odaklanan deneysel bir mühendislik projesidir. Veri özelliklerinin, geri dönüşümlü dönüşümlerin, kodlama stratejilerinin ve hesaplama kısıtlarının sıkıştırma verimliliğini nasıl etkilediğini araştırır.

Temel amaç mevcut codec'lerden her durumda daha iyi olduğunu iddia etmek değil; **sıkıştırılmış boyut, kodlama hızı, çözme hızı, bellek tüketimi ve doğruluk** arasındaki ilişkiyi tekrarlanabilir ölçümlerle değerlendirmektir.

## Temel Teknik Sorular

- Hangi veri örüntüleri yararlanılabilir tekrar içerir?
- Geri dönüşümlü ön işleme hangi durumlarda sonucu iyileştirir?
- Daha iyi sıkıştırma oranının CPU ve bellek maliyeti nedir?
- Sıkıştırma ve açma hızları nasıl farklılaşır?
- Metin, ikili veri, önceden sıkıştırılmış dosyalar ve küçük girdiler sonuçları nasıl etkiler?
- Streaming ve sınırlı bellek altında hangi kısıtlar ortaya çıkar?

## Kavramsal Veri Akışı

```mermaid
flowchart LR
    INPUT[Girdi Baytları] --> ANALYZE[Veri Özellikleri]
    ANALYZE --> TRANSFORM[İsteğe Bağlı Geri Dönüşümlü Dönüşüm]
    TRANSFORM --> ENCODE[Kayıpsız Kodlayıcı]
    ENCODE --> FORMAT[Kodlanmış Akış]
    FORMAT --> DECODE[Kod Çözücü]
    DECODE --> REVERSE[Ters Dönüşüm]
    REVERSE --> OUTPUT[Orijinal Baytlar]
```

Diyagram olası bir sıkıştırma hattını gösterir; **her aşamanın uygulanmış olduğunu iddia etmez**. Kayıpsız akışa dahil edilen dönüşümler geri alınabilir olmalıdır. Kod çözücü uyumluluğu ve açık format sınırları da önemlidir.

## Doğruluk Değişmezi

Kayıpsız sıkıştırma, girdiyi bayt düzeyinde eksiksiz geri üretmelidir:

```text
decompress(compress(input)) == input
```

Bu yüzden testlerde boş girdiler, küçük dosyalar, ikili içerik, zor sıkıştırılan veriler, tekrarlayan örüntüler, kesilmiş akışlar ve bozuk girdiler ele alınmalıdır. Yalnızca sıkıştırma oranı doğruluğu kanıtlamaz.

## Benchmark Yöntemi

| Metrik | Tanım |
|---|---|
| **Sıkıştırma oranı** | Sıkıştırılmamış bayt / sıkıştırılmış bayt |
| **Alan tasarrufu** | (1 − sıkıştırılmış bayt / orijinal bayt) × %100 |
| **Sıkıştırma hızı** | Saniyede işlenen orijinal veri |
| **Açma hızı** | Saniyede geri elde edilen orijinal veri |
| **Tepe bellek kullanımı** | İşlem boyunca görülen en yüksek bellek tüketimi |
| **Round-trip bütünlüğü** | Açma işleminden sonra bayt düzeyinde tam eşitlik |

Anlamlı karşılaştırmalar için aynı veri kümeleri, donanım, iş parçacığı ayarları ve ölçüm yöntemleri kullanılmalıdır. Container formatı ve metadata yükü sıkıştırılmış boyuta dahil edilmelidir.

İş yüküne göre **zlib, Zstandard, LZ4 ve Brotli** gibi yerleşik codec'ler referans alınabilir. Sonuçlar kanıtsız genel sıralamalar yerine veri kümesi bazında raporlanmalıdır.

## Araştırma Alanları

**Tekrar analizi.** Kodlama stratejisi seçmeden önce girdi özelliklerini incelemek.

**Geri dönüşümlü dönüşümler.** Daha iyi kodlamanın ek süre ve bellek maliyetini karşılayıp karşılamadığını değerlendirmek.

**Entropi kodlama.** Sembol dağılımları, çıktı boyutu ve decoder karmaşıklığı arasındaki ilişkiyi incelemek.

**Akış formatı.** Çerçeveleme, sürümleme, bütünlük kontrolü ve bozuk veri yönetimini değerlendirmek.

**Performans ve güvenlik.** Hız ve bellek ölçümü yaparken geçersiz uzunluklara, bozuk akışlara ve aşırı kaynak tahsisine karşı koruma düşünmek.

---

<div align="center">

**HobbitZip** · `Lossless Compression` · `Algorithms` · `Benchmarking` · `Systems Engineering`

Geliştiren: [Taha Kubilay](https://github.com/tahakubilay)

</div>
