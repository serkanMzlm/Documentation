# Make ve Makefile Kullanımı

Make, yazılım projelerinde kaynak kodların derlenmesi ve yapı adımlarının otomatikleştirilmesi için kullanılan güçlü bir araçtır. Özellikle büyük projelerde zamandan tasarruf sağlar ve yapı sürecini standardize eder.

!!! note "Not"
    Makefile’da girintiler **TAB karakteriyle** yapılmalıdır. Boşluk (space) kullanmak hata ile sonuçlanır!

## Temel Sözdizimi ve Semboller

| Sembol | Anlamı |
|--------|--------|
| `#`    | Yorum satırı. Satırın en başında kullanılmalıdır. |
| `\`    | Satırı bir sonrakine taşımak için kullanılır (satır devamı). |
| `@`    | Komutun çıktısı ekrana yazılmaz. `make -s` ile aynı etki yaratır. |
| `:`    | Bir hedef (target) tanımlar. |
| `::`   | Aynı isme sahip birden fazla hedef tanımına izin verir. |
| `$`    | Değişken referanslarında kullanılır. |
| `=`    | Normal değişken ataması. |
| `?=`   | Değişken önceden tanımlanmamışsa değer atar. |
| `:=`   |  Anında değerlendirme (immediate evaluation). |
| `$@`   |	Hedef (target) ismi |
| `$<`   |	İlk bağımlılık (prerequisite) |
| `$^`   |	Tüm bağımlılıklar |
| `$?`   |	Hedeften daha yeni bağımlılıklar |
| `make -s` |	Komutları bastırır (sessiz çalışır). |
| `make -k` |	Hata alınsa bile mümkün olduğunca devam eder. |
| `make -i` |	Hataları yoksayarak devam eder. |

***:** Shell tarzı tüm dosyalarla eşleşir. Örn: `*.c`

**%:** Pattern (desen) eşleştirme için kullanılır. Örn: `%.o: %.c`

```make
SRC := $(wildcard *.c)
OBJ := $(SRC:.c=.o)
```

Bu kullanım tüm .c dosyalarını .o uzantılı versiyonlara çevirir.

---


## Basit Makefile Örnekleri

### Örnek 1: Temel Hedef

```make
hello:
	echo "hello, world"

all: hello
	echo "hi"
```

### Örnek 2: Aynı Hedefe Sahip Birden Fazla Blok
!!! note "Not"
    blah hedefi ilk kez çalıştırıldığında komutlar çalışır ve dosya oluşturulursa, tekrar make çalıştırıldığında blah is up to date mesajı alınır. Bu durumda dosyada değişiklik yapılmadıkça hedef yeniden derlenmez.
```make
blah::
	echo "hello"

blah::
	echo "hello again"
```

### Örnek 3: Bağımlılık Farkı

```make title="1. Yöntem"
blah:
	cc blah.c -o blah
```
```make title="2. Yöntem"
blah: blah.c
	cc blah.c -o blah
```
!!! note "Not"
    2. yöntem tercih edilmelidir. Böylece sadece blah.c dosyası değiştiğinde hedef yeniden derlenir. Bu yöntem zamandan ve kaynaklardan tasarruf sağlar.


## Değişkenler ve Otomatik Değişkenler
```make title="Tanımlama"
CC = gcc
CFLAGS = -Wall -O2
TARGET = my_program
OBJ = main.o utils.o
```

## Kapsamlı Makefile Örneği

```make 
# Değişkenler
CC = gcc
CFLAGS = -Wall -O2
TARGET = my_program
OBJ = main.o utils.o

# Varsayılan hedef
all: $(TARGET)

# Hedef dosya
$(TARGET): $(OBJ)
	$(CC) $(CFLAGS) -o $@ $^

# .o dosyalarının nasıl üretileceği
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

# Temizlik
clean:
	rm -f $(TARGET) $(OBJ)

# Gerçek dosya olmayan hedefler
.PHONY: all clean

```

## wildcard ve pattern matching

`wildcard`, belirli dosya desenlerini eşleştirip listeleyen bir fonksiyondur. `wildcard` fonksiyonu mutlaka `:=` ile birlikte kullanılmalıdır. Aksi halde genişletilmez.

```make
thing_right := $(wildcard *.o)
```