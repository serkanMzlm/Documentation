# XML Programlama

XML (Extensible Markup Language), veri depolama ve taşınmasını kolaylaştıran, hem insan hem de makine tarafından okunabilir bir işaretleme dilidir. Aşağıda XML’in temel kavramlarını ve ek bilgileri bulabilirsiniz:

## Prolog ve Temel Kurallar
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
    … içerik …
</root>
```

- Prolog (`<?xml …?>`):
    - **version:** XML sürümü (çoğunlukla `"1.0"`).
    - **encoding:** Karakter kodlaması (örn. `"UTF-8"`, `"ISO-8859-1"`).
- **Tek bir root (kök)** eleman zorunludur; tüm diğer etiketler bu kökün içinde tanımlanır.
- **Etiket adları** rakamla veya `xml` ile **başlayamaz**; büyük/küçük harf duyarlıdır.

##  Etiketler ve İçerik
```xml
<parent>
    <child>Metin içeriği</child>
    <emptyTag/>           <!-- Kendini kapatan boş etiket -->
</parent>
```

- **Açılış/kapama:** `<tag> … </tag>`
- **Boş eleman:** `<tag/>`
- **İçerik:** Metin, başka etiketler veya her ikisi birden olabilir.
- **Sır:** XML’de iç içe etiketler, doğru açılış/kapama sırasıyla tanımlanmalıdır.

## Öğelerin (Elements) Adlandırma Stilleri
| Stil          |   Örnek    |
|---------------|------------|
| Lower case    | firstname  |
| Upper case    | FIRSTNAME  |
| Underscore    | first_name |
| PascalCase    | FirstName  |
| camelCase     | firstName  |

## Öznitelikler (Attributes)
```xml
<person id="123" role="admin">Ali</person>
```

- Etiket içinde ad="değer" biçiminde tanımlanır.
- Aynı öğe içinde birden fazla öznitelik kullanılabilir.
- Öznitelik değerleri her zaman tırnak ("…" veya '…') içinde olmalıdır.

## Özel Karakterler ve Escaping
Bazı karakterler doğrudan yazılamaz, yerlerine entity referansları kullanılır:

| Entity          |   Karakter    |    Açıklama    |
|-----------------|---------------|----------------|
| `&lt;`          | `<`     |  küçüktür     |
| `&gt;`          | `>`     |  büyüktür     |
| `&amp;`         | `&`     |  ampersand    |
| `&quot;`        | `"`     |  çift tırnak     |
| `&apos;`        | `'`     |  tek tırnak     |

```xml
<note>Use &lt;tag&gt; to define elements &amp; entities.</note>
```

## Yorumlar ve CDATA
- Yorum Satırı `<!-- Bu bir yorum satırıdır -->`
- **CDATA bölgesi:** Özel karakterlerin kaçış gerektirmeden yer aldığı ham metin. 
```xml
<script><![CDATA[
    if (a < b && b > c) { /* kod */ }
]]></script>
```

## Beyaz Alan (Whitespace) ve Biçimlendirme

- XML boşlukları korur; `<a> </a>` içinde iki boşluk gerçek içerik olur.
- Dosyayı küçültmek için tüm satır sonu ve gereksiz boşlukları kaldırabilirsiniz (“minify”).

## Namespace (Ad Alanı)
- İsim çakışmalarını engellemek için kullanılır. Kök öğede tanımlanır:
```xml
<root xmlns:h="http://www.w3.org/TR/html4"
      xmlns:f="https://www.w3schools.com/furniture">
  <h:table>
    <h:tr><h:td>Apples</h:td></h:tr>
  </h:table>
  <f:table>
    <f:name>African Coffee Table</f:name>
  </f:table>
</root>
```
- `xmlns:prefix="URI": prefix:` ile başlayan öğeler bu URI kapsamında tanımlanır.
- URI gerçek bir web adresi olmak zorunda değil; tanımlama amacıdır.

## Şema ve DTD (Doctype)
- XML belgelerinin yapısını tanımlayan mekanizmalardır:
    - DTD (Document Type Definition):
```xml
<!DOCTYPE note [
  <!ELEMENT note (to,from,heading,body)>
  <!ELEMENT to (#PCDATA)>
  …
]>
```
    - XSD (XML Schema Definition):
``` xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="note">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="to" type="xs:string"/>
        …
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

## İpuçları ve En İyi Uygulamalar
- Tek bir root öğe kullanın.
- Gereksiz derin iç içe geçişlerden kaçının; okunabilirliği bozar.
- Boyutsal verileri (tarih, saat) parçalara ayırmak yerine kısa string formatı tercih edin.
- Belgeleri minify ederek küçültün, aktarımı hızlandırın.
- Karmaşık veri yapıları için `JSON` veya `YAML` değerlendirilebilir.
