# Python Programlama

- `#` Satır içi yorum başlatır.
- `\` Satır sonu karakter kaçışında kullanılır (örneğin uzun satırı bölmek için).
- `\\` Tek bir ters eğik çizgi (\) üretir.
- **Type casting** `int(), float(), bool(), str(), complex()`
- Değişken atamaları ve temel operatörler
```python
a = b = 10            # Birden fazla değişkene aynı anda değer atama
a, b, c = 10, 20, 30  # Çoklu atama
a, b = b, a           # İki değişkenin yerini değiştirme
a ** 2                # Kuvvet alma (aynı zamanda pow(a, 2)
```
- `:=` (walrus operatörü) Koşul içindeyken atama yapmanızı sağlar
```python
if (n := len(input("deger: "))) < 10:
print(f"Girdi {n} karakterden kısa")
```

### Yerleşik Fonksiyonlar

- Sayısal ve Matematiksel Fonksiyonlar

| Fonksiyon | Açıklama |
|--------|--------|
| `abs(x)` |  Mutlak değer |
| `pow(x, y[, z])` |  x**y veya modlu: (x**y) % z   |
| `round(x[, n])` |  x’i n ondalığa yuvarlar (default n=0). |
| `divmod(a, b)` |  `(a // b, a % b)` döner.   |
| `max(iterable, …)` | En büyük eleman; çoklu arg ile de kullanılabilir  |
| `min(iterable, …)` |  En küçük eleman |
| `sum(iterable, start=0)` | Toplam. start başlangıç değeri.  |
| `range(start, stop[, step])` | Sayı dizisi üretir (start varsayılan 0, step varsayılan 1)  |


- Dönüşüm Fonksiyonları

| Fonksiyon | Açıklama |
|--------|--------|
| `int(x[, base])` |  	x’i tam sayıya çevirir (base taban).  |
| `float(x)` |  x’i ondalıklı sayıya çevirir.   |
| `complex(re, im=0)` |  Karmaşık sayı oluşturur.   |
| `str(x)` |  x’i string’e dönüştürür.   |
| `bytes(x, encoding)` |   x’i bytes yapar.  |
| `bytearray(x, …)` |  Değiştirilebilir bytes.   |
| `bin(x), oct(x), hex(x)` |   x’i sırasıyla ikili, sekizli, onaltılıya çevirir.  |
| `chr(i), ord(c)` |   Kod noktası ↔ karakter.  |

- Koleksiyon İşlemleri

| Fonksiyon | Açıklama |
|--------|--------|
| `len(x)` |   Eleman sayısı / uzunluk.  |
| `type(obj)` |  Nesnenin türü  |
| `sorted(iterable, key, reverse)` |  Sıralı liste döner.   |
| `reversed(seq)` |   Ters yönde iterator döner.  |
| `sum(iter, start)` |  	Toplam (sayısal iter)   |
| `min, max` |   en küçük / en büyük  |
| `all(iter)` |  Tüm öğeler True ise True   |
| `any(iter)` |  En az bir öğe True ise True   |
| `enumerate(iter, start=0)` |  (index, value) çiftlerinden iter oluşturur   |
| `zip(*iterables)` |  Paralel iterasyon için tuple iterator  |
| `filter(func, iterable)` |  func(item) True ise öğeyi geçirir  |
| `map(func, iterable)` |  func(item) dönen yeni öğe iteratoru  |
| `slice(start, stop, step)` |  Dilim objesi (obj[slice])  |

```python linenums="1"
# filter & map örneği
l = [1,2,3,4,5,6]
print(list(filter(lambda x: x%2, l)))       # [1,3,5]
print(list(map(lambda x: x*x, l)))          # [1,4,9,16,25,36]

# Direkt liste halinde görmek için:
print(*enumerate("istihza"))
# veya for döngüsünde:
for idx, harf in enumerate("istihza", start=1):
    print(idx, harf)
```

- Fonksiyonel ve Dinamik Fonksiyonlar

| Fonksiyon | Açıklama |
|--------|--------|
| `eval(expr)` |  expr string’ini değerlendirir, sonuç döner.  |
| `exec(code)` |  code string’ini çalıştırır, değer döndürmez.  |
| `compile(...)` |  String’i çalıştırılabilir koda çevirir.  |
| `globals()` |  Global isim alanı sözlüğünü döner.  |
| `locals()` |  Mevcut local isim alanı sözlüğünü döner.  |
| `callable(obj)` |  obj() çağrılabilir mi? (True/False)  |
| `hash(obj)` |  obj hash değerini döner.  |
| `isinstance(obj, type)` | Tür denetimi yapar.   |
| `issubclass(cls, classinfo)` |  Alt sınıf sorgusu.  |
| `dir(obj)` |  obj’in metot/özellik adlarını listeler.  |
| `vars([obj])` |  obj’in __dict__ öğesini döner (varsayılan local/global).  |
| `help(obj)` |  Yardım sistemini açar, dokümantasyonu gösterir.  |
| `id(obj)` | obj’in benzersiz kimlik numarası (bellek adresi).   |
| `input(prompt)` | Kullanıcıdan girdi alır (string).  |
| `exit(), quit()` |  Çıkış işlevi (REPL için)  |


```python linenums="1"
# globals/locals örneği
globals()['z'] = 23
def fonk(a,b):
    x = a+b
    print(locals())
fonk(10,20)
# {'a':10, 'b':20, 'x':30}

exec("print('Merhaba')")
result = eval("10 + 20")  # 30

print(dir(str))   # str sınıfının metotlarını gösterir
print(dir(10))    # int nesnesinin metotlarını gösterir
print(dir(""))    # yine str metotlarını listeler
```

- `print(*objs, sep=' ', end='\n', file=sys.stdout, flush=False)`
    - `sep` Öğeler arası ayırıcı.
    - `end` Sonuna eklenen karakter.
    - `file` Yazma hedefi (dosya veya başka bir çıktı akışı).
    - `flush` Anında yazmayı zorlar.
- `input(prompt)` Kullanıcıdan girdi alır (her zaman string döner).
```python
with open("deneme.txt", "w") as dosya:
    ad = input("adiniz: ")
    yas = int(input("yas: "))
    print(ad, yas, sep="-", file=dosya, flush=True)
```
- `*` operatörü
    - Fonksiyon çağrısında `*iterable`: elemanları ayrı argüman olarak geçirir.
    - String çarpımında `"deneme" * 3 ‑> "denemedenedeme"`.

- `replace(old, new, count=-1)` **String** içinde **old** karakter(leri)ni **new** ile değiştirir. count parametresi, en fazla kaç değişiklik yapılacağını sınırlar **(default: tümü)**.

```python
metin = "denemedeneme"
print(metin.replace("e", "E"))      # "dEnEmEdEnEmE"
print(metin.replace("d", "", 1))    # ilk 'd' silinir: "enemedeneme"
```

- `split(sep=None, maxsplit=-1) / rsplit(sep=None, maxsplit=-1) / splitlines(keepends=False)`
    - `split` Varsayılan boşluk/kaydırma karakterlerine göre böler.
    - `rsplit` Sağdan başlayarak böler (özellikle maxsplit kullanırken fark eder).
    - `splitlines` Satır sonu karakterlerine göre böler; keepends=True satır sonlarını korur.
```python
metin = "a, b, c, d"
print(metin.split())          # ['a,', 'b,', 'c,', 'd']
print(metin.split(", "))      # ['a', 'b', 'c', 'd']
print(metin.rsplit(", ", 2))  # ['a, b', 'c', 'd']
print("line1\nline2".splitlines())        # ['line1', 'line2']
print("line1\nline2".splitlines(True))    # ['line1\n', 'line2']
```
- `strip(chars=None) / lstrip(chars=None) / rstrip(chars=None)` Başlangıç ve/veya sondaki boşluk ya da chars parametresindeki karakterleri siler.
```python
s = "  abc  "
print(s.strip())    # "abc"
print(s.lstrip())   # "abc  "
print(s.rstrip())   # "  abc"
```
- `join(iterable)` Iterable içindeki tüm öğeleri birleştirip tek bir string döner; self araya konur.
```python
parts = ["Py", "thon", "3"]
print("".join(parts))      # "Python3"
print("-".join(parts))     # "Py-thon-3"
```
- `count(sub[, start[, end]])` Bir alt string’in kaç kez geçtiğini sayar.
```python
s = "banana"
print(s.count("a"))          # 3
print(s.count("na", 2, 5))   # 1
```
- `index(sub[, start[, end]]) / rindex(sub[, start[, end]])` 
    - `index` İlk eşleşmenin başlangıç indeksini döner, bulunamazsa ValueError.
    - `rindex` Sağdan aramaya başlar.
```python
s = "abracadabra"
print(s.index("ra"))      # 2
print(s.rindex("ra"))     # 9
```
- `find(sub[, start[, end]]) / rfind(sub[, start[, end]])`
    - `find` İlk eşleşmenin indeksini döner; bulunamazsa -1.
    - `rfind` Sağdan arar.
```python
print(s.find("cad"))   # 4
print(s.find("xyz"))   # -1
```
- `center(width[, fillchar]) / ljust(width[, fillchar]) / rjust(width[, fillchar])` Metni belirtilen genişliğe ortalar veya sola/sağa yaslar; boşluk dışı karakter için fillchar verebilirsiniz.
```python
print("hi".center(6))           # '  hi  '
print("hi".ljust(6, "_"))       # 'hi____'
print("hi".rjust(6, "-"))       # '----hi'
```
- `zfill(width)` Sağa yaslayarak sol tarafı sıfır ('0') ile doldurur.
```python
print("42".zfill(5))    # '00042'
print("-42".zfill(5))   # '-0042'
```
- `partition(sep) / rpartition(sep)` String’i üç parçaya ayırır: (önce, sep, sonra). rpartition sağdan başlar.
```python
print("istanbul".partition("an"))   # ('ist', 'an', 'bul')
print("istanbul".rpartition("an"))  # ('ist', 'an', 'bul')
```
- `encode(encoding="utf-8", errors="strict")` String’i belirtilen kodlamaya göre byte dizisine çevirir.
```python
b = "çay".encode("utf-8")   # b'\xc3\xa7ay'
b2 = "abc".encode("ascii")  # b'abc'
```
- `str.maketrans(x, y=None, z=None)` & `translate(table)`
    - `maketrans(x, y)` İki string’in karakterlerini eşleştiren bir dönüşüm tablosu (sözlük) oluşturur.
    - `maketrans('', '', z)` z içindeki karakterleri kaldırmak için tablo üretir.
    - `translate(table)` String’i tabloya göre dönüştürür veya siler.
```python
# Türkçe karakter dönüşümü örneği
kaynak = "şçöğüıŞÇÖĞÜİ"
hedef  = "sco guiSCOGUI"
tablo  = str.maketrans(kaynak, hedef)
print("çağ".translate(tablo))   # "cag"

# Belirli karakterleri silme
sil = "aeıioöuüAEIİOÖUÜ"
tablo2 = str.maketrans('', '', sil)
print("Merhaba Dünya".translate(tablo2))  # "Mrhb Dny"
```

### % Biçimlendirme (Eski Stil) 
- `%` işaretinden sonra gelen format karakterleri (%d, %s, %f, %x vb.) C tarzı yer tutuculardır.
```python
print("Adınız: %s, Soyadınız: %s" % ("Serkan", "Mazlum"))
print("%%")                # Yüzde işareti yazdırır: %

print("%d" % 13.5)         # 13
print("%05d" % 20)         # '00020' — genişlik 5, soldan sıfırla doldur
print("%.5d" % 23)         # '00023' — tam sayı için 5 basamaklı gösterim

print("%.5f" % 3.1415926)  # '3.14159' — virgülden sonra 5 basamak
print("%15s" % "serkan")   # '         serkan' — genişlik 15, sağa yasla
print("%-15s" % "serkan")  # 'serkan         ' — sola yasla

# İç içe metodlarla:
print("|%s|" % "deneme".center(15))
print("|%s|" % "istihza".rjust(15))
```

### str.format() Metodu
- `{}` yer tutucularını, format() içindeki argümanlarla doldurur.
- `{0}, {1}…` sayılarla pozisyon belirtebilir; `{name}` ile isimli argüman kullanabilirsiniz.
- İsteğe bağlı format belirtimleri için `:` kullanılır:
    - Hizalama: `{:>10} (sağa)`, `{:^10} (ortala)`, `{:<10} (sola)`.
    - Tip ve hassasiyet: `{:d}, {:.2f}` vb.
    - Binlik ayırıcı: `{:,}` (her 3 basamakta virgül).
```python
template = """
Adınız : {}
Soyadınız: {}
"""
print(template.format("Serkan", "Mazlum"))

# Pozisyonlu parametre
template2 = "Merhaba {1}, ben {0}"
print(template2.format("Serkan", "Mazlum"))

# İsimli parametre
print("Kullanıcı: {user}".format(user="serkan"))

# Binlik ayırıcı
print("{:,}".format(1234567890))   # '1,234,567,890'

# Hizalama ve precision
print("{:>8}".format("hi"))        # '      hi'
print("{:.3f}".format(2/3))        # '0.667'
```

### F-String (Python 3.6+)
- Dizge başına `f` eklenir, `{}` içinde doğrudan değişken veya ifadeler yazabilirsiniz.
- Esnek: aritmetik, metod çağrısı, format belirtimi doğrudan kullanılabilir.
```python
isim, soyad = "Serkan", "Mazlum"
sayi1, sayi2 = 10, 20

# Basit kullanım
print(f"Selam {isim} {soyad}, toplam = {sayi1 + sayi2}")

# Format ile precision ve hizalama
pi = 3.14159
print(f"Pi ≈ {pi:.2f}")           # 'Pi ≈ 3.14'
print(f"|{'deneme':^10}|")        # '|  deneme  |'

# Direkt ifadeler ve input
print(f"Sayıların toplamı = {int(input('1. sayı: ')) + int(input('2. sayı: '))}")
```

### Temel Veri Türleri

- **Immutable:** Değiştirilemez veri tipleri
    - Sayılar (int, float, complex)
    - String (str)
    - Demet (tuple)

- **Mutable:** Değiştirilebilir veri tipleri
    - Liste (list)
    - Sözlük (dict)
    - Küme (set)

#### Sayı Sistemleri
- Python’da sayıları farklı tabanlarda görüntülemek ve dönüştürmek için yerleşik fonksiyonlar ve biçimlendirme özellikleri vardır.
- abs(x) → |x|
- `max(iterable, key=…) / min(…)` → En büyük/küçük öğe (veya key fonksiyonuna göre).
- `sum(iterable, start=0)` → Toplam.
- `divmod(a, b) → (a // b, a % b)` bölüm ve kalan.

```python title="Tabanda Yazdırma" linenums="1"
bases = ["Onlu", "Sekizli", "Onaltılı", "İkili"]
print((" {:^9} " * len(bases)).format(*bases))

for i in range(17):
    # {:^9}    → Onlu
    # {:^9o}   → Sekizli (oct)
    # {:^9x}   → Onaltılı (hex, küçük harf)
    # {:^9b}   → İkili (bin)
    print("{0:^9} {0:^9o} {0:^9x} {0:^9b}".format(i))

   Onlu     Sekizli   Onaltılı    İkili   
    0          0          0          0    
    1          1          1          1    
    2          2          2         10    
   … 
   16         20         10       10000  
``` 
- `bin(n) → "0b..."` (ikili)
- `oct(n) → "0o..."` (sekizli)
- `hex(n) → "0x..."` (onaltılı)
- `int(s, base) → string` belirtilen base tabanından onluyla döndürür.

```python linenums="1"
>>> bin(14)               # '0b1110'
>>> oct(239)              # '0o357'
>>> hex(1234)             # '0x4d2'
>>> int('a12b', 16)       # 0xA12B ondalık: 41291
```

- `int, float, complex` temel sayısal türler.
- `float.as_integer_ratio()` → Kesir olarak (`pay, payda`).
- `float.is_integer()` → Tam sayı mı? (ör. `12.0.is_integer() == True`)
- `int.bit_length()` → İkili gösterimde kaç bit kullanılıyor?

```python  linenums="1"
a = 12.0
c = 12 + 4j
n = 11
print(a.as_integer_ratio())   # (12, 1)
print(a.is_integer())         # True
print(c.real, c.imag)         # 12.0 4.0
print(n.bit_length())         # 4  (çünkü 11 → '1011')

nums = [2, -5, 7, 3]
print(abs(-5))               # 5
print(max(nums))             # 7
print(min(nums, key=abs))    # 2
print(sum(nums))             # 7
print(divmod(20, 6))         # (3, 2)
```

#### Karakter Dizileri (Strings)
- Tek ('…'), çift ("…") veya üçlü tırnak ('''…''' / """…""") kullanılabilir.
- Üçlü tırnak çok satırlı metin tanımı içindir.
- İç içe tırnak gerekirse diğer tip tırnak kullanılır veya kaçış karakteri (`\", \'`) kullanılır.

```python linenums="1"
multiline = """
Hello
World
"""
print('He said "Hello"')
print("It's OK")
```

- `str` (Immutable)
- `expandtabs(tabsize)` Sekme (`\t`) karakterlerini belirtilen genişlikte boşluklara çevirir. `"a\tb".expandtabs(4)  # 'a   b'`
- `help(str)` str sınıfının metotlarını gösterir

```python title="İndeksleme ve Dilimleme" linenums="1"
kelime = "ali"
kelime[0]       # 'a'
kelime[-1]      # 'i' (son eleman)
kelime[:2]      # 'al' (başlangıçtan 2. karaktere kadar, 2 dahil değil)
kelime[1:]      # 'li' (1. karakterden sona kadar)
kelime[::-1]    # 'ila' (ters çevirme)
kelime[::2]     # 'ai' (iki karakterde bir atlayarak)
"A" + kelime[1:]# 'Ali' (ilk harfi değiştirerek)
```
```python title="Büyük / Küçük Harf Dönüşümleri" linenums="1"
s = "python Programlama"
s.capitalize()   # 'Python programlama' – sadece ilk harf
s.title()        # 'Python Programlama' – her kelimenin ilk harfi
s.upper()        # 'PYTHON PROGRAMLAMA'
s.lower()        # 'python programlama'
s.casefold()     # 'python programlama' – dil bağımsız, daha agresif küçültme
s.swapcase()     # 'PYTHON pROGRAMLAMA'
```
```python title="Sorgulamalar" linenums="1"
s.islower()     # tümü küçük mü?
s.isupper()     # tümü büyük mü?
s.startswith("py")  # başlıyor mu?
s.endswith("ma")    # bitiyor mu?
s.isalpha()     # yalnızca harflerden mi?
s.isdigit()     # yalnızca rakamlardan mı?
s.isalnum()     # harf veya rakam mı?
s.isdecimal()   # ondalık rakam mı?
s.isnumeric()   # sayısal karakter mi?
s.isidentifier()# geçerli Python tanımlayıcı mı?
s.isspace()     # yalnızca boşluk karakterlerinden mi?
s.isprintable() # yazdırılabilir karakterler mi?
```
```python title="Ters Çevirme ve Sıralama" linenums="1"
a = "ali"
# reversed: iterator döner
print("".join(reversed(a)))  # 'ila'

# sorted: liste döner
print(sorted("deneme"))      # ['a','d','e','e','m','n']
```
```python linenums="1" title="Türkçe karakter sıralama için locale kullanın"
import locale
locale.setlocale(locale.LC_ALL, "tr_TR.UTF-8")  # sistemde yüklü olmalı
print(sorted("çiçek", key=locale.strxfrm))
```

#### Liste (list)
- Köşeli parantez içinde tanımlanır, öğeler , ile ayrılır `liste = [1, "iki", 3.0, True]`
- **Mutable (değiştirilebilir):** Aynı liste üzerinde ekleme, silme, değiştirme yapabilirsiniz.
- Tekrar eden elemanlar olabilir.
- Eleman sayısını len(liste) ile alırsınız.
- İç içe listeler desteklenir
- `list()` yapıcıyla da oluşturabilirsiniz
- `append(x)` Sonuna x ekler.
- `extend(iterable)` Tüm öğeleri ekler (tek tek).
- `insert(i, x)` İndeks i konumuna x ekler.
- `remove(x)` İlk eşleşen öğeyi siler; bulunmazsa ValueError.
- `pop([i])` İndeks ideki öğeyi döner ve siler (varsayılan son öğe).
- `clear()` Tüm öğeleri siler ([] olur).
- `index(x[, start[, end]])` İlk bulduğu x’in indeksini döner; bulunmazsa ValueError.
- `count(x)` x’in listede kaç kez geçtiğini döner.
- `sort(key=None, reverse=False)` Yerinde sıralar.
- `reverse()` Yerinde ters çevirir.
- `copy()` Yüzeysel kopya döner (orijinal listeden bağımsız).
- `b = a → referans` aynı listeyi işaret eder; birinde değişiklik diğerini etkiler.
- `b = a[:]` veya `b = list(a)` veya `b = a.copy() → yeni liste` bağımsız kopya.

```python linenums="1"
my_list = [1, 2, 2, 3, 'a', 'b', 'c']

matris = [[1,2,3], [4,5,6], [7,8,9]]
print(matris[1][2])  # 6

bos = list()                 # []
chars = list("python")       # ['p','y','t','h','o','n']
nums  = list(range(5))       # [0,1,2,3,4]

a = [3,1,4]
a.append(2)           # [3,1,4,2]
a.extend([5,6])       # [3,1,4,2,5,6]
a.insert(1, 99)       # [3,99,1,4,2,5,6]
a.remove(99)          # [3,1,4,2,5,6]
x = a.pop()           # x=6, a=[3,1,4,2,5]
a.sort()              # [1,2,3,4,5]
a.reverse()           # [5,4,3,2,1]
b = a.copy()          # b bağımsız kopya
```

```python title="Öğelere Erişim ve Dilimleme" linenums="1"
meyveler = ["elma","armut","çilek","kiraz"]
print(meyveler[0])     # 'elma'
print(meyveler[-1])    # 'kiraz'
print(meyveler[1:3])   # ['armut','çilek']
print(meyveler[:2])    # ['elma','armut']
print(meyveler[::2])   # ['elma','çilek']
```

```python title="List Comprehensions" linenums="1"
# 0–999 arası sayılar
liste = [i for i in range(1000)]
# çift sayılar
even = [i for i in range(1000) if i % 2 == 0]
# iç içe açma
matris = [[1,2],[3,4]]
duz = [x for row in matris for x in row]  # [1,2,3,4]
```

#### Demet (tuple) 
- Parantez içinde veya virgülle oluşturulur
- **Immutable:** İçerik değiştirilemez; yeni bir atama eski nesneden farklı kimlik üretir.
- Erişim, dilimleme listelerle aynı şekilde
- Tekrar eden elemanlar olabilir.
- Tek öğeli demet: sonuna virgül koyun.
- Yüzeysel kopya gerekmez; atama hep referans davranışı verir.
- `count(x)` x’in kaç kez geçtiğini sayar.
- `index(x)` İlk x’in indeksini döner; bulunmazsa `ValueError`.

```python linenums="1"
t = (1, 2, 3)
t2 = 4, 5, 6

print(t[0], t[-1], t[1:3])
single = (42,) # Tek elemanlı Demet

t = (1,2,1,3)
print(t.count(1))   # 2
print(t.index(3))   # 3
```

#### Sözlük (dict)
- Anahtarlar immutable ve benzersiz olmalı; değerler mutable olabilir.
- Süslü parantezle oluşturulur: `{}`.
- Key – Value çiftlerinden oluşur
- Değişik veri tipleri kullanılabilir:
    - Key olarak immutable tipler (str, int, tuple vs.).
    - Value olarak her tür (list, dict, başka dict vs.).
- Yeni öğe eklemek/güncellemek:
- `keys()` Tüm anahtarları döner.
- `values()` Tüm değerleri döner.
- `items()` (key, value) çiftlerini döner.
- `get(key, default=None)` Key yoksa default döner, hata vermez.
- `pop(key, default=…)` Key’i silip değerini döner; yoksa default veya KeyError.
- `popitem()` Son eklenen çifti silip döner.
- `setdefault(key, default=None)` Key yoksa ekler ve default döner; varsa mevcut değeri döner.
- `update(other_dict)` Başka bir sözlüğün içeriğini ekler/günceller.
- `clear()` Tüm öğeleri siler ({}).
- `copy()` Yüzeysel kopya oluşturur.
- `fromkeys()` Belirli anahtarlarla hepsi aynı değere sahip yeni bir sözlük oluşturur:

```python linenums="1"
sozluk = {
    "kitap"      : "book",
    "bilgisayar" : "computer",
    "dil"        : "language"
}

sozluk["Ahmet"] = "Adana"

sozluk = {"a": 1, "b": 2}
print(list(sozluk.keys()))    # ['a', 'b']
print(sozluk.get("c", 0))     # 0
sozluk.update({"b": 3, "c": 4})
print(sozluk)                 # {'a':1, 'b':3, 'c':4}
```

```python linenums="1" title="İç içe sözlüklere erişim"
kisiler = {
    "Ahmet": {"Sehir": "Istanbul", "Meslek": "Ogretmen"},
    "Mehmet": {"Sehir": "Adana", "Meslek": "Muhendis"}
}
print(kisiler["Ahmet"]["Meslek"])  # Ögretmen

keys = ['a', 'b', 'c']
sozluk = dict.fromkeys(keys, 0)
# {'a':0, 'b':0, 'c':0}
```


#### Küme (set)
- `set()` ile veya `{}` içinde virgülle oluşturulur.
- **Mutable** (değiştirilebilir), sırasız, benzersiz elemanlar.
- **Immutable** set: `frozenset()` oluşturulduktan sonra değiştirilemez.
- `add(x)` Eleman ekler.
- `update(iterable)` Birden fazla elemanı ekler.
- `remove(x)` Eleman siler; yoksa KeyError.
- `discard(x)` Eleman siler; yoksa hata vermez.
- `pop()` Rastgele bir elemanı siler ve döner.
- `clear()` Tüm elemanları siler.
- `copy()` Yüzeysel kopya döner.
- `union(other)` / `|`: Birleşim.
- `intersection(other)` / `&`: Kesişim.
- `difference(other)` / `-`: Fark.
- `symmetric_difference(other)` / `^`: Karşılıklı fark.
- `_update` versiyonları (`intersection_update`, vb.) sonucu aynı kümeye uygular.
- Sorgular:
    - `isdisjoint(other)` Ortak eleman yok mu?
    - `issubset(other)` Alt küme mi?
    - `issuperset(other)` Üst küme mi?

```python linenums="1"
my_set = {1, 2, 3, 'a', 'b', 'c'}
s = set([1,2,3])
t = {"elma", "armut", "kiraz"}
f = frozenset([1,2,3])

s = {1,2,3}
s.add(4)           # {1,2,3,4}
s.update([5,6])    # {1,2,3,4,5,6}
s.discard(2)       # {1,3,4,5,6}
s.remove(3)        # {1,4,5,6}

a = {1,2,3,4}
b = {3,4,5,6}

print(a | b)   # {1,2,3,4,5,6}
print(a & b)   # {3,4}
print(a - b)   # {1,2}
print(a ^ b)   # {1,2,5,6}
print(a.issubset({1,2,3,4,5}))  # True
```

### Döngüler ve Koşullar

- **if – elif – else**: Koşulları `and` ve `or` ile birleştirebilir, `not` ile tersleyebilirsiniz. `in` öğenin içinde olup olmadığını kontrol eder. `is` kimlik (aynı nesneye işaret etme) kontrolü yapar.

```python
n = 42

if (n > 10) and (n < 100):
    print("10 ile 100 arasında")
elif (n > 100) or (n < 0):
    print("0’dan küçük veya 100’den büyük")
elif not (n == 42):
    print("Yanlış sayı")
else:
    print("Diğer durumlar")

# 'in' örneği
if 'a' in "ali":
    print("‘a’ var")

# 'is' örneği (özellikle tekil nesneler için, örn. None)
a = None
if a is None:
    print("a hâlâ None")
```

- **for – while** Döngü oluşturmak için kullanılır. `pass – break – continue` 
    - **pass:** Gövdesi boş bir blok bırakmak için (ileride doldurmak üzere).
    - **break:** Döngüyü tamamen sonlandırır.
    - **continue:** Döngünün bir sonraki yinelemesine atla

```python
# for döngüsü
for harf in "deneme":
    print(harf)

# enumerate ile indeksli yineleme
for idx, harf in enumerate("deneme"):
    print(idx, harf)

# while döngüsü
i = 0
while i < 3:
    print(i)
    i += 1
```

- `for - while` döngüsüne bir `else` bloğu ekleyebilirsiniz. Döngü normal sonlanırsa (yani `break` ile kesilmezse) else çalışır:


```python
metin = "Bu yAzıdA küçük a yok."
for harf in metin:
    if harf == 'a':
        print("‘a’ bulundu.")
        break
else:
    print("‘a’ bulunmadı.")
```

### Hata Yakalama ve İstisnalar
- **try – except** `try` bloğu içindeki kod çalıştırılır; eğer tanımlı bir istisna (Exception) fırlatılırsa uygun `except` bloğu devreye girer. Birden fazla `except` ile farklı hata türlerini ayrı işleyebilirsiniz. Hiçbir tür belirtilmeden yazılan `except` tüm istisnaları yakalar **(genellikle önerilmez)**.

```python
try:
    deger = input("Sayı: ")
    deger_int = int(deger)
except ValueError:
    print("Geçerli bir sayı değil.")
except ZeroDivisionError:
    print("Sıfıra bölme hatası.")
except (TypeError, NameError) as hata:
    print("Başka bir hata:", hata)
else:
    print("Başarılı dönüşüm:", deger_int)
finally:
    print("Bu blok her zaman çalışır.")

```

- **raise** Kendi hata durumlarınızı fırlatmak için:

```python
deger = int(input("Değer: "))
if deger == 10:
    raise Exception("10 sayısı yasaktır!")
```

- **assert** Bir koşulun doğru olduğunu doğrulamak için. Yanlışsa `AssertionError` fırlatır:

```python
giris = input("Adınız: ")
assert giris, "İsim boş olamaz!"
print("Hoş geldiniz,", giris)
```

### Temel Dosya İşlemleri

- open(path, mode, encoding=None) ile dosya açılır.
- mode:
    - `'r'` – okuma (varsayılan)
    - `'w'` – yazma (varsa üzerine yazar, yoksa oluşturur)
    - `'a'` – ekleme (varsa sona ekler, yoksa oluşturur)
    - b eklenirse ikili mod (örn. `'rb'`, `'wb'`)
    - + eklenirse hem okuma hem yazma (örn. `'r+'`, `'w+'`)

- Windows yollarında ters eğik çizgi (\) kaçış karakteri olduğundan:

```python linenums="1"
# Ya çift ters eğik kullanın:
f = open("C:\\klasor\\dosya.txt", "r")
# Ya ham string (raw) ile:
f = open(r"C:\klasor\dosya.txt", "r")
```

```python title="Manuel açma-kapatma" linenums="1"
try:
    f = open("veri.txt", "r", encoding="utf-8")
    içerik = f.read()
    # ... işlemler ...
except IOError:
    print("Dosya açılamadı veya okuma hatası!")
finally:
    f.close()
```

- **Context manager (with)** kullandığınızda `close()` otomatik çağrılır:

```python
with open("veri.txt", "r", encoding="utf-8") as f:
    for satır in f:
        print(satır.rstrip("\n"))
# with bloğu bitince f.close() otomatik yapılır
```

- `f.read(size=-1)` Tüm içeriği (size=-1) veya en fazla size byte/karakter okur.
- `f.readline()` Bir satır okur (satır sonunu da içerir).
- `f.readlines()` Tüm satırları liste olarak döner.
- `f.write(text) / f.writelines(iterable)` Metni yazar. write tek string, writelines birden çok satır içeren iterable alır.
- `print(..., file=f, flush=True)` print fonksiyonu ile doğrudan dosyaya yazabilirsiniz.

```python
with open("cikti.txt", "w", encoding="utf-8") as f:
    f.write("Merhaba\n")
    print("Dosyaya ek satır", file=f, flush=True)
```

- `f.seek(offset, whence=0)` 
    - İmleci offset byte ileri/geri taşır.
        - whence=0: baştan (seek(0) başa dön).
        - whence=1: mevcut pozisyondan.
        - whence=2: sondan.
- `f.tell()` Geçerli imleç pozisyonunu byte cinsinden döner.
- `f.readable(), f.writable(), f.seekable()` İzin sorguları (bool).
- `f.truncate(size=None)` Dosyayı `size` byte’a kadar kısaltır (default: mevcut pozisyona kadar).
- `f.mode` → açılış modu ('r', 'wb', vb.)
- `f.name` → dosya adı veya yol
- `f.encoding` → metin modunda kullanılan kodlama

```python
# Metin modu (satır sonu çevirisi, encoding)
with open("notlar.txt", "w+", encoding="utf-8") as f:
    f.write("Türkçe karakter: ç\n")
    f.seek(0)
    print(f.read())

# İkili mod (raw byte)
with open("resim.png", "rb") as f:
    header = f.read(8)
    print(header)  # b'\x89PNG\r\n\x1a\n'

```

#### PDF Dosyaları Üzerinde Basit İşlemler
- PDF’ler genellikle ikili dosyalardır; içinde `/Producer`, `/CreationDate` gibi metaveri etiketleri bulunur. Örneğin üretici bilgisini okumak için:

```python
with open("belge.pdf", "rb") as f:
    data = f.read()
    idx = data.find(b"/Producer")
    if idx != -1:
        print(data[idx:idx+50])
```

!!! note "Not"

    Gerçek PDF işleme için PyPDF2, pdfminer.six gibi kütüphaneleri kullanmak çok daha güvenli ve pratiktir.

| Etiket | Anlamı |
|--------|--------|
| `/Creator`              | Belgeyi oluşturan uygulama veya yazılım (ör. Adobe InDesign) |
| `/Producer`        | PDF’yi üreten kütüphane veya araç (ör. “PDFLib” vb.) |
| `/Author`             | Belge yazarı |
| `/Title`           | Belge başlığı |
| `/Subject`          | Belge konusu veya özeti |
| `/Keywords`  | Anahtar kelimeler (arama ve sınıflandırma için) |
| `/CreationDate`        | Oluşturulma tarihi (D:YYYYMMDDHHmmSSZ formatında) |
| `/ModDate`          | Son değiştirilme tarihi |
| `/Trapped`      | Baskı iş akışı notu (“True”, “False” veya “Unknown”) |	
	
```python
with open("belge.pdf", "rb") as f:
    data = f.read()
    for tag in [b"/Creator", b"/Producer", b"/Author", b"/Title", b"/Subject", b"/Keywords", b"/CreationDate", b"/ModDate"]:
        idx = data.find(tag)
        if idx != -1:
            snippet = data[idx:idx+100].split(b"\n",1)[0]
            print(snippet)
```

#### Dosya Türü Algılama (Magic Numbers)

- Bazı dosyalar, içerdikleri “magic number” (sihrî sayı) adı verilen sabit bayt dizileri sayesinde türleri kolayca tespit edilebilir:
- **JPEG** – Başlangıç baytları: `FF D8 FF E0 ?? ?? 4A 46 49 46` (JFIF) Veya `FF D8 FF E0 ?? ?? 45 78 69 66` (Exif)
- **PNG** – İlk 8 bayt: 
    - Onluk: `137 80 78 71 13 10 26 10` 
    - Onaltılık: `89 50 4E 47 0D 0A 1A 0A`
    - Karakter karşılığı: `\x89 P N G \r \n \x1A \n`
- **GIF** – İlk 3 bayt: `47 49 46` (“GIF”)
- **TIFF** – İlk 2 bayt: `49 49` (“II”) veya `4D 4D` (“MM”)
- **BMP** – İlk 2 bayt: `42 4D` (“BM”)

```python
for path in dosyalar:
    header = open(path, 'rb').read(10)
    if header[6:10] in (b'JFIF', b'Exif'):
        print(f"{path} → JPEG")
    elif header[:8] == b"\x89PNG\r\n\x1a\n":
        print(f"{path} → PNG")
    elif header[:3] == b"GIF":
        print(f"{path} → GIF")
    elif header[:2] in (b"II", b"MM"):
        print(f"{path} → TIFF")
    elif header[:2] == b"BM":
        print(f"{path} → BMP")
    else:
        print(f"{path} → Bilinmeyen tür")
```

#### Karakter, Byte ve Bytearray

- Kodlama Hataları (`errors`): 
    - Dosya açarken errors parametresi, çözümlenemeyen karakterlerle ne yapılacağını belirler:
        - `'strict'` → Hata fırlatır.
        - `'ignore'` → Hatayı atlatarak karakteri atlar.
        - `'replace'` → Geçersiz karakter yerine ? koyar.
        - `'xmlcharrefreplace'` → XML karakter referansı (&#1234;) ekler.

```python
# 'ç' ASCII’e çevrilmez:
"çalış".encode("ascii", errors="ignore")   # b"al s"
"çalış".encode("ascii", errors="replace")  # b"?al?s"
```

- `repr(obj)` Nesnenin resmi (parse edilebilir) gösterimini döner; kaçış dizilerini gösterir.
- `ascii(obj)` repr() benzeri, ama ASCII olmayan karakterleri \x../\u.. biçiminde kodlar.
- `ord(c)` Karakter c’nin Unicode kod noktasını döner (int).
- `chr(n)` Kod noktası n’e karşılık gelen karakteri döner.
- `bytes` Değiştirilemez (immutable) bayt dizisi.
- `bytearray` Değiştirilebilir (mutable) bayt dizi
- `s.encode(encoding, errors)` → bytes
- `b.decode(encoding, errors)` → str
- `fromhex()` Onaltılık dizeden bytes oluşturur

```python
print(repr("Line\nNew"))    # "'Line\\nNew'"
print(ascii("Türkçe"))      # "'T\\xfcrk\\xe7e'"

print(ord('A'))  # 65
print(chr(65))   # 'A'

b = b"hello"                         # literal
b2 = bytes([65,66,67])               # b"ABC"
ba = bytearray(b"mutable")           # bytearray(b"mutable")

s = "İstanbul"
b = s.encode("utf-8")            # b'\xc4\xb0stanbul'
print(b.decode("utf-8"))         # 'İstanbul'

bytes.fromhex("deadbeef")      # b'\xde\xad\xbe\xef'

# Bir karakterin byte uzunluğu, kullandığınız kodlamaya göre değişir:
print(len("ş".encode("utf-8")))    # 2 byte
print(len("ş".encode("cp1254")))   # 1 byte
```

### Fonksiyonlar (def)

- `def` anahtar kelimesiyle fonksiyon tanımlanır.
- İlk satırdaki string (docstring), fonksiyonun belgelendirilmesi içindir (`help()` ile erişilebilir).

```python title="Tanım ve Çağrı" linenums="1"
def kayit_olustur(isim, soyisim, sehir):
    """
    Üç zorunlu parametre alan kayıt oluşturma fonksiyonu.
    """
    print(f"İsim   : {isim}")
    print(f"Soyisim: {soyisim}")
    print(f"Şehir  : {sehir}")

# Pozisyonel argümanlarla
kayit_olustur("Fırat", "Özgül", "İstanbul")

# Keyword argümanlarla (sıra farketmez)
kayit_olustur(soyisim="Özgül", isim="Fırat", sehir="İstanbul")
```

- `Default` değerler **tanım sırasında** belirlenir.
- Default’lar yalnızca sondaki parametrelerde kullanılabilir.

```python title="Varsayılan (Default) Parametreler" linenums="1"
def kur(kurulum_dizini="/usr/bin/"):
    """
    Kurulum dizini verilmezse varsayılan "/usr/bin/" kullanılır.
    """
    print(f"Program {kurulum_dizini} dizinine kuruldu!")

kur()                    # Program /usr/bin/ dizinine kuruldu!
kur("/opt/myapp/")       # Program /opt/myapp/ dizinine kuruldu!
```


- **Değişken Sayıda Parametre:** `*args` → pozisyonel argümanları tuple olarak alır. `**kwargs` → keyword argümanları dict olarak alır.

```python linenums="1"
def carp(*sayilar):
    """İstediğiniz kadar sayı alır, hepsini çarpar."""
    sonuc = 1
    for s in sayilar:
        sonuc *= s
    return sonuc

print(carp(2, 3, 5))  # 30

def kayit(**bilgiler):
    """Anahtar=değer çiftlerini sözlük olarak yazdırır."""
    for k, v in bilgiler.items():
        print(f"{k:10}: {v}")

kayit(ad="Ahmet", soyad="Öz", sehir="Ankara")
```

- `*args` ve `**kwargs` aynı fonksiyonda birlikte kullanılabilir.
- Sıralama:
    - Pozisyonel zorunlu parametreler
    - Default parametreler
    - `*args`
    - Keyword-only parametreler (sadece * sonrası gelenler)
    - `**kwargs`

- `*` işaretiyle, sonrasında gelen parametrelerin yalnızca keyword ile atanmasını zorunlu kılabilirsiniz

```python linenums="1" title="Keyword‑only Parametreler"
def islem(a, b, *, verbose=False):
    """verbose ancak key=value ile geçilebilir."""
    result = a + b
    if verbose:
        print(f"{a} + {b} = {result}")
    return result

islem(2, 3, verbose=True)  # 5 satırını yazdırır
# islem(2,3,True)          # TypeError!
```

#### Fonksiyondan Değer Döndürme

- `return` ile fonksiyonu sonlandırıp değer döndürür.
- `return` yoksa `None` döner.

```python linenums="1"
import random

def sayi_uret(bas=0, bit=50, adet=5):
    """Tekrarsız rastgele sayılar seti döner."""
    sonuc = set()
    while len(sonuc) < adet:
        sonuc.add(random.randint(bas, bit))
    return sonuc

rastgele = sayi_uret(1, 100, adet=3)
print(rastgele)  # örn. {42, 7, 99}
```

#### Lambda (Anonim Fonksiyonlar)
- Tek satırlık, isimlendirilmemiş fonksiyon tanımı için:

```python linenums="1"
carp = lambda *s, **k: __import__("functools").reduce(lambda x,y: x*y, s, 1)
print(carp(2,3,4))  # 24

# Basit örnek
kare = lambda x: x*x
print(kare(5))      # 25
```

```python linenums="1"
def fonk(param1, param2):
    return param1 + param2

#lambda 
fonk = lambda param1, param2: param1 + param2

harfler = "abcçdefgğhıijklmnoöprsştuüvyz"
isimler = ["ahmet", "ışık", "ismail", "can", "iskender"]

çevrim = {i: harfler.index(i) for i in harfler}           
print(sorted(isimler, key=lambda x: çevrim.get(x[0])))

l = [2, 5, 10, 23, 3, 6]
print(*map(lambda sayı: sayı ** 2, l))
```

- **Local** değişkenler fonksiyon içinde tanımlıdır.
- `global` ile fonksiyon içinde `global` scope’daki bir değişkene atama yapabilirsiniz:

```python linenums="1"
isim = "Fırat"

def degistir():
    global isim
    isim += " Özgül"

degistir()
print(isim)  # Fırat Özgül
```

- `nonlocal` ile dış (ancak global olmayan) scope’daki değişkene erişebilirsiniz:

```python linenums="1"
def dis_fonksiyon():
    x = 5
    def ic_fonksiyon():
        nonlocal x
        x += 3
    ic_fonksiyon()
    return x

print(dis_fonksiyon())  # 8
```
#### Öz yinelemeli fonksiyonlar 

```python linenums="1"
def azalt(s):
    if len(s) < 1:
        return s
    else:
        print(s)
        return azalt(s[1:])

print(azalt('istihza'))
```

#### İç İçe Fonksiyonlar

```python
def yazıcı():
    def yaz(mesaj):
        print(mesaj)
    return yaz

def fonksiyon_sayıcı():
    sayı = 0
    def say():
        nonlocal sayı   #localde bulunan değişkene erişmeyi sağlar
        sayı += 1
        return sayı
    return say
```

#### Fonksiyon İmzası ve Anotasyonlar
- Parametre ve dönüş türü açıklamaları için **type hints**

```python linenums="1"
from typing import List, Dict

def ortalama(sayilar: List[int]) -> float:
    return sum(sayilar) / len(sayilar)

print(ortalama([10, 20, 30]))  # 20.0
```

- Bu anotasyonlar çalışma zamanı değil, statik araçlar (mypy, IDE) içindir.

#### Üreteçler (Generators)

- Python üreteçleri, `yield` anahtar kelimesiyle tanımlanan, duraklatılabilir ve devam ettirilebilir fonksiyonlardır. Büyük veri setlerini adım adım üretmek ve bellek tasarrufu sağlamak için idealdir.

```python linenums="1"
def uretec():
    sayi = 0
    while True:
        sayi += 1
        yield sayi
```

- yield: Fonksiyonu duraklatır ve bir değer döndürür.
- Bir sonraki değeri almak için next() kullanılır:

```python linenums="1"
g = uretec()
print(next(g))  # 1
print(next(g))  # 2
print(next(g))  # 3
```

- Üreteç sonsuz döngüde çalışabilir; ihtiyaç duyulana kadar bellek kullanmaz.

```python linenums="1" title="Sınırlı Üreteç Örneği"
def iki_yazi():
    yield "Merhaba"
    yield "Dünya"

g = iki_yazi()
print(next(g))  # "Merhaba"
print(next(g))  # "Dünya"
# print(next(g))  # StopIteration hatası (artık değer kalmadı)
```

```python linenums="1" title="yield from ile İç Üreteç"
# Başka bir üreteci doğrudan çağırmak ve alt değerleri yeniden üretmek için
def uretec1():
    yield "A başladı"
    yield "A bitti"

def uretec2():
    yield "B başladı"
    yield from uretec1()
    yield "B bitti"

for v in uretec2():
    print(v)

# Çıktı:
# B başladı
# A başladı
# A bitti
# B bitti
```

```python linenums="1" title="Üreteç İfadeleri (Generator Expressions)"
# Kısa söz dizimiyle üreteç oluşturma
# Liste üretmek yerine üreteç:
liste = [i*i for i in range(5)]         # normal liste
uretec = (i*i for i in range(5))         # üreteç ifadesi

print(type(uretec))  # <class 'generator'>
print(next(uretec))  # 0
print(list(uretec))  # [1, 4, 9, 16]
```

- `()` içinde, `[]` listesinden tek farkı bellek yerine değerleri teker teker üretir.
- `Send / Throw / Close` Üreteç ile çift yönlü iletişim sağlar.
- `StopIteration` Üreteç tükendiğinde (ek `yield` yoksa), `next()` StopIteration fırlatır.

```python linenums="1"
def echo():
    while True:
        value = (yield)
        print("Gelen:", value)

g = echo()
next(g)            # başlat (ilk yield’e kadar)
g.send("Merhaba")  # "Gelen: Merhaba"
g.close()          # üreteci kapat
```

#### Üreteçlerin Avantajları
- **Bellek Verimliliği:** Tüm veriyi aynı anda saklamaz, ihtiyaç duyulana kadar hesaplar.
- **Akış Tabanlı İşlemler:** Dosya satır satır okuma, sonsuz veri akışları, büyük diziler vb.


### Modüller ve Paketler
- `os.name, os.listdir()` gibi tam yolu kullanırsınız.
- `subprocess` yerine `sp` yazarak erişebilirsiniz.

```python linenums="1" title="Tüm modülü içe aktarma"
import os
import subprocess as sp   # alias ile kısaltma
```

- Yalnızca gerekli nitelikleri (fonksiyon, değişken, sınıf) getirir.
- `p.join(), p.exists()` gibi path içindekileri kısaca çağırabilirsiniz.

```python linenums="1" title="Modülden belirli üyeleri içe aktarma"
from os import name, listdir, getcwd
from os import path as p   # alias ile kısaltma
```

```python linenums="1" title="Alt modülleri veya alt paketleri içe aktarma"
import urllib.request        # modül içindeki alt modülü tam yoluyla kullan
from urllib import request   # doğrudan `request` adıyla erişim
from urllib.request import urlopen, Request
```

```python linenums="1" title="Tüm nitelikleri içe aktarma (genellikle önerilmez)"
from urllib.request import *
from sys import *
```

!!! note "Not"

    İsim çatışmalarına ve okunabilirlik sorunlarına yol açabilir.


#### Paket Yapısı

- Bir klasörün paket olarak tanınması için içinde bir **__init__.py** dosyası bulunmalıdır (Python 3.3+’ta bu zorunlu değil, ama iyi pratiktir).
- Paket içindeki modüller şöyle içe aktarılır:


```python linenums="1" title=""
import mypkg.module1
from mypkg.module2 import func
```

```python linenums="1" title="Göreceli içe aktarma (paket içi modülleri import ederken):"
# same package altında
from . import helper       # mypkg/helper.py
from .sub import utils     # mypkg/sub/utils.py
# bir üst pakete çıkmak
from ..core import config
```

- `sys.path` ile Özel Yol Ekleme: Python, modülleri sys.path listesindeki klasörlerde arar. Dinamik olarak ekleme yapabilirsiniz:

```python linenums="1" title=""
import os, sys

home = os.environ.get('HOME') or os.environ.get('HOMEPATH')
desktop = os.path.join(home, 'Desktop')
sys.path.append(desktop)  # artık Desktop’taki modülleri import edebilirsiniz
```

- `sys.path` değişkeni, `import` sırasında bakılan dizin listesini tutar.
- Yolu ekledikten sonra, o klasördeki `.py` dosyalarını doğrudan `import` edebilirsiniz.

## Nesne Yönelimli Programlama (OOP)
Python’da her şey bir nesne olduğundan, sınıflar (class) veri yapılarınızı ve davranışlarınızı (metotlar) bir arada tanımlamak için temel yapıdır.

- `class` anahtar kelimesiyle başlar, sınıf adı **PascalCase** (büyük harfle başlayan her kelime) olur.
- `pass` boş bir gövde için kullanılır.

```python linenums="1" title="Sınıf Tanımı ve Nesne Oluşturma"
class Kisi:
    pass

# Sınıftan nesne oluşturma (örnek/instance)
k = Kisi()
```

- `__init__` metodu, nesne oluşturulurken otomatik çağrılır.
- `self`, o örneğe (nesneye) referans verir; her metot ilk parametresi `self` olmalıdır.

```python linenums="1" title="__init__  Yapıcı (Constructor)"
class Kisi:
    def __init__(self, isim, yas):
        self.isim = isim     # Örnek niteliği
        self.yas   = yas

k = Kisi("Ali", 30)
print(k.isim, k.yas)       # Ali 30
```

- Sınıf nitelikleri (toplam) tüm örnekler arasında paylaşılır.
- Örnek nitelikleri (self.id) yalnızca o nesneye özeldir.

```python linenums="1" title="Örnek ve Sınıf Nitelikleri"
class Sayaç:
    toplam  = 0        # Sınıf niteliği

    def __init__(self):
        Sayaç.toplam += 1
        self.id = Sayaç.toplam   # Örnek niteliği

a = Sayaç()
b = Sayaç()
print(a.id, b.id)      # 1 2
print(Sayaç.toplam)    # 2
```

- Sınıf içindeki her işlev, metot olarak adlandırılır.
- İlk parametresi `self` olduğu için örneğe erişebilir.

```python linenums="1" title="Metotlar"
class Matematik:
    def kare(self, x):
        return x*x

m = Matematik()
print(m.kare(5))       # 25
```

- `@classmethod` ile işaretlenen metotlara ilk parametre olarak sınıf `(cls)` gelir.
- Sınıf niteliği üzerinde işlem yapmak için kullanılır.

```python linenums="1" title="Sınıf Metotları ve @classmethod"
class Sayaç:
    toplam = 0

    def __init__(self):
        Sayaç.toplam += 1

    @classmethod
    def toplam_sayac(cls):
        return cls.toplam

print(Sayaç.toplam_sayac())  # 0
a, b = Sayaç(), Sayaç()
print(Sayaç.toplam_sayac())  # 2
```

- `@staticmethod` ile tanımlanan metotlar ne self ne de cls alır; sınıfa bağlı değil, bağımsız işlev gibidir.

```python linenums="1" title="Statik Metotlar ve @staticmethod"
class Yardimci:
    @staticmethod
    def topla(a, b):
        return a + b

print(Yardimci.topla(3, 4))  # 7
```

- `@property` ile metodu öznitelik gibi (d.alan) kullanabilirsiniz.
- Okuma (`@<isim>.setter`) ve silme (`@<isim>.deleter`) işlemleri de eklenebilir.

```python linenums="1" title=" Özellikler ve @property"
class Daire:
    def __init__(self, yaricap):
        self._r = yaricap

    @property
    def alan(self):
        return 3.1416 * self._r ** 2

d = Daire(5)
print(d.alan)  # 78.54
```

- **Public** (her yerden erişilebilir): isim
- **Protected** (alt sınıflarda kullanılabilir): _isim
- **Private** (sınıf içinde saklı): __isim → arkasında isim eşleme (`name mangling`)

```python linenums="1" title="Bilgi Gizleme (Encapsulation)"
class A:
    def __init__(self):
        self.__gizli = 42

a = A()
# print(a.__gizli)       # AttributeError
print(a._A__gizli)        # 42 (name mangling)
```

- **Bir sınıf class AltSinif(UstSinif):** ile miras alır.
- `super()` ile üst sınıfın metotlarını çağırabilirsiniz.
- Çoklu kalıtım: `class C(A, B)`

```python linenums="1" title="Kalıtım (Inheritance)"
    def __init__(self, isim):
        self.isim = isim

    def calis(self):
        print(f"{self.isim} çalışıyor")

class Yonetici(Calisan):
    def calis(self):
        super().calis()
        print(f"{self.isim} yönetiyor")

y = Yonetici("Ayşe")
y.calis()
# Ayşe çalışıyor
# Ayşe yönetiyor
```

- Dunder (double underscore) metotları, nesnelere özel davranışlarla Python’un yerleşik işlemlerini yeniden tanımlar:
- Böylece +, -, len(), str(), == gibi işlemleri nesnenize özgü hale getirebilirsiniz.

```python linenums="1" title="Polimorfizm ve Dunder Metotlar"
class Nokta:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __add__(self, other):
        return Nokta(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Nokta({self.x}, {self.y})"

p1, p2 = Nokta(1,2), Nokta(3,4)
print(p1 + p2)  # Nokta(4, 6)
```
## Pip 

```bash linenums="1"
pip show numpy
pip3 show numpy

pip install numpy
pip3 install numpy

pip3 install [package_name]==[verion] # Belirli bir version indirilir.
pip install numpy==1.25.0
pip3 install numpy==1.25.0

pip install --upgrade scikit-learn
pip3 install --upgrade scikit-learn

pip list
pip3 list # Yüklü olan python paketlerini listeler
```


## Standart Kütüphanaler

### Düzenli İfadeler (re Modülü)
Python’un `re` modülü, metin içinde karmaşık desen aramaları ve dönüşümleri için kullanılır. Gereksiz yere değil, gerçekten ihtiyaç duyduğunuzda tercih edin.

- Temel Fonksiyonlar

|**Fonksiyon** |	**Açıklama** |
|-------------|------------------|
| `re.match(pattern, text)` | Metnin başında pattern ile eşleşme arar. |
| `re.search(pattern, text)` | Metnin her yerinde ilk eşleşmeyi bulur. |
| `re.findall(pattern, text)` | Tüm eşleşmeleri liste olarak döner. |
| `re.finditer(pattern, text)`	| Tüm eşleşmeleri iterator olarak döner (match objeleri). |
| `re.sub(pattern, repl, text)`	| Deseni repl ile değiştirir (tüm eşleşmelerde). |
| `re.split(pattern, text) ` | Desene göre bölerek liste döner. |

```python linenums="1" title=""
text = "Foo123 Bar456 Baz789"

# Başta “Foo” olup olmadığını kontrol
print(bool(re.match(r"Foo", text)))          # True

# Metinde ilk sayısal bloku bul
m = re.search(r"\d+", text)
print(m.group(), m.start())                  # '123', 3

# Tüm sayı bloklarını listele
print(re.findall(r"\d+", text))              # ['123', '456', '789']

# Harf ve rakam arasına tire ekle
print(re.sub(r"([A-Za-z]+)(\d+)", r"\1-\2", text))
# 'Foo-123 Bar-456 Baz-789'
```

- Karakter Sınıfları ve Özel Diziler

|**Desen**   | **Anlamı** |
|------------|--------|
| `[abc]`     | a veya b veya c |
| `[0-9]`     | 0–9 arası |
| `[^abc]`	     | a, b, c dışındaki karakterler |
| `.`	         | Yeni satır (\n) hariç her karakter |
| `\d`	         | Ondalık rakam ([0-9]) |
| `\D`	         | Ondalık olmayan ([^0-9]) |
| `\w`	         | Alfanümerik ve alt çizgi ([A-Za-z0-9_]) |
| `\W`	         | Alfanümerik olmayan ([^A-Za-z0-9_]) |
| `\s`	         | Boşluk karakterleri ([ \t\n\r\f\v]) |
| `\S`	         | Boşluk olmayan karakterler |

- Tekrarlama Miktarları

| Metakarakter |	Anlamı |
|------------|-------------|
| `*`	    | 0 veya daha fazla kez |
| `+`	    | 1 veya daha fazla kez |
| `?`	    | 0 veya 1 kez |
| `{m}	`    | Tam olarak m kez |
| `{m,}`	| En az m kez |
| `{m,n}`	| m ile n kez arası |

- Bağlam Belirleyiciler
    - `^` → Metnin başlangıcı
    - `$` → Metnin sonu
    - `\b` → Kelime sınırı
    - `\B` → Kelime sınırı değil

```python
# Büyük harfle başlayıp rakamla biten satır
pattern = r"^[A-Z].*\d$"
print(bool(re.match(pattern, "Hello123")))  # True
print(bool(re.match(pattern, "hello123")))  # False
```

- Gruplama ve Yakalama: Parantez içinde oluşturulur:
    - `(...)` → Yakalama grubu (daha sonra `.group(n)` ile erişilir).
    - `(?:...)` → Yakalamadan sadece grup oluşturur.

```python
m = re.search(r"(\d+)-(\d+)-(\d+)", "Tarih: 2021-12-31")
print(m.groups())    # ('2021', '12', '31')
```

- Alternation ve Seçimler: `|` ile “veya” ilişkisi kurulur:

```python
# 'cat' veya 'dog'
re.findall(r"cat|dog", "cat dog mouse")  # ['cat', 'dog']
```

- Bayraklar (Flags): Regex’i satır satır (`re.MULTILINE`), büyük/küçük harf duyarsız (`re.IGNORECASE`) veya noktanın yeni satırı da yakalaması için (`re.DOTALL`) yapılandırın:

```python
pattern = r"^foo"
text = "foo\nbar\nfoo"

print(re.findall(pattern, text))  # ['foo']
print(re.findall(pattern, text, flags=re.MULTILINE))  # ['foo', 'foo']
```

- İpuçları ve İyi Uygulamalar: 
    - Ham string (raw): `r"..."` kullanın, böylece \ kaçış sorunlarıyla uğraşmazsınız.
    - **Derlenmiş desen:** Çok sık kullanılan deseni re.compile() ile derleyip tekrar kullanın.
    - **Yorumlayın:** Karmaşık desenler için (?x) flag’i ile boşluk ve yorum yazabilirsiniz

```python
pattern = re.compile(r"""
    ^               # satır başı
    [A-Za-z]+       # harflerden oluşan bir veya daha fazla karakter
    \d{2,4}         # 2 ila 4 rakam
    $               # satır sonu
""", re.VERBOSE)
```