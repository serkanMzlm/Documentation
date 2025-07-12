# C Programlama

- **LValue:** Adrese sahip, atama yapılabilen ifadelerdir.
- **RValue:** Atama yapılamayan, genellikle geçici değerlerdir.
- **Call by Reference:** C dilinde doğrudan yoktur, ancak pointerlar aracılığıyla benzer işlev sağlanabilir.
- **Call by Value:** Parametrelerin fonksiyona kopyalanarak gönderilmesidir.
- **Postfix (i++):** İşlemden sonra artırır.
- **Prefix (++i):** İşlemden önce artırır.
- **Recursion:** Bir fonksiyonun kendisini çağırma işlemidir.
- **Redeclaration:** Aynı kapsamda aynı isimle değişken ya da fonksiyon tekrar tanımlanamaz. Bu hata üretir.
- `,` operatörü aynı kapsamda aynı isimle değişken ya da fonksiyon tekrar tanımlanamaz. Bu hata üretir.
- **argc:** Argument count (argüman sayısı)
- **argv:** Argument vector (argüman dizisi)
- Diziler bellek adreslerini tutar.
- **dizi[index]** ile **index[dizi]** aynı anlama gelir (`*(dizi + index)`).
- **Temel Türler:** int, float, double, char, void
- **const:** Değişkeni sabit yapar.
- **sizeof:** Veri veya değişken boyutunu döner.
- **Struct:** Farklı veri türlerini gruplar.
- **Bitfield:** Hafızayı daha verimli kullanmak için bit seviyesinde alan tanımlar.
- **Enum:** Sabitlere anlamlı isimler verir.
- **Union:** Aynı bellek bölgesini farklı veri türleriyle kullanır.

```c
typedef int tamsayi;
tamsayi value = 0;

struct Person {
  char name[20];
  int age;
};

typedef struct {
  unsigned int month : 4;
  unsigned int year : 11;
  unsigned int day : 5;
} Date;

typedef struct {
  uint8_t x;
  uint16_t y;
} __attribute__((packed)) State_s;
```

- **Snake Case:** my_variable_name
- **Camel Case:** myVariableName
- **Pascal Case:** MyVariableName
- **__attribute__:** Yapıların bellekteki boşluklarını optimize eder.
- **volatile**: Optimize edilmemesi gereken değişkenlerde kullanılır.

```c
// constructor int main önce çalışmasını sağlar.
// destructor int mainden sonra çaılmasını sağlar.

void __attribute__((constructor)) calledFirst();
void __attribute__((destructor)) calledLast();
  
void main() { printf("main"); }
  
void calledFirst() {  printf("constructor\n"); }
void calledLast(){ printf("destructor\n"); }

// packed bellekte olan boşlukların doldurulmasını sağlar. normalde alta bulunan 
// struct  32 byte olur fakat packed  sayesinde 24 byte olur.
typedef struct {
  uint8_t x;
  uint16_t y;
} __attribute__((packed)) State_s;

State_s state_1
State_s state_2 = {1 ,2};
State_s state_3 = {0}; // Bütün verilere 0 atar.
State_s state_ptr;
state_ptr->x = 10; // ya da *(state_ptr).x = 10;

```

- **Implicit:** Otomatik tür dönüşüm.
- **Explicit:** Manuel tür dönüşüm.

```c
int x = (int)3.14; // C tarzı
int y = static_cast<int>(3.14); // C++ tarzı
```

- **Koşullar:** if, else if, else, switch
- **Döngüler:** for, while, do-while
- **Fonksiyonlar:** Void dışındaki fonksiyonlar mutlaka dönüş yapmalıdır.
- **Define ve Makrolar:** Derleme zamanında sembolik isimler yaratır. **`#define`** sembolik isimler atanması için kullanılır. Değişkenlerin önüne boş bir #define koyarak, o değişkenin kullanım amacı hakkında bilgi verilebilir. #define bellek kısmında alan tutmaz derleme sırasında otomatik olarak düzeltilir.

```c
#define MAX_SIZE 100
#define __MUTLAK_SONUC__    //boş define 
int __MUTLAK_SONUC__ degisken;
```

- **String (Karakter Dizileri):** C dilinde, string (karakter dizileri) aslında karakterlerden oluşan dizilerdir. Karakter dizileri her zaman özel bir sonlandırıcı karakter `\0` (null terminatör) ile biter. Bu sonlandırıcı, dizinin sonunu belirtmek için kullanılır.
    - Örneğin, 6 karakterlik bir metin için bellekte 7 byte'lık alan ayrılır (6 karakter + 1 null terminatör).
    - Çift tırnak " içinde yazılan stringlere otomatik olarak `\0` eklenir. Ancak tek tırnak ' ile belirtilen karakter dizilerinde son karakter olarak manuel `\0` eklenmelidir.

```c
char str1[] = "Hello"; // Otomatik olarak \0 eklenir.
char str2[] = {'H', 'i', '\0'}; // Manuel olarak \0 eklenm
```

```c
#include <stdarg.h>

if(printf("Hello World")) {}  // Bir kez çalışır.
while(printf("Hello World")) {} // Sonsuz döngü.

for(int a = 0, b = 10; a < 10 && b > 2; a++, b--);
unsigned log int = size_t;

int array[5] = {[0 ... 4] = 5}; // Derleyiciye göre kabul edilir.


printf("%2.3f", num);   // Sağ hizalı
printf("%-2.3f", num);  // Sol hizalı
printf("%+2.3f", num);  // Pozitif sayı önüne '+' koyar
printf("%05x", num);    // Boşluklara 0 koyar
printf("%*.*f", 2,3,number); // '*' verilerin dışardan doldurulmasını sağlar.
printf("%#x", number);       // '#' sayıların 0x şeklinde yazılmasını sağlar.

char buf[100];
sprintf(buf, "Sayı: %d", num);
sscanf(buf, "Sayı: %d", &num);
```

- `for(;;), while(true)` sonsuz döngüye örnektir.
- `continue` döngüyü başa alır. `break` döngüden çıkar. `return` fonksiyondan çıkar.
- Modern derleyiciler de `#pragma once` kullanılabilir. 

```c
// HEADER GUARDS
#ifndef SOME_UNIQUE_NAME_HERE
#define SOME_UNIQUE_NAME_HERE
#endif
```
```c
int add(int a, int b); // prototip (declaration)

int add(int a, int b) { return a + b; }  // Definition

int sum(int num, ...) { // değişken sayıda parametre alan fonksiyon
    int total = 0;
    va_list args;
    va_start(args, num);
    for (int i = 0; i < num; i++) {
        total += va_arg(args, int);
    }
    va_end(args);
    return total;
}
```

- **Direct Acces:** Bir değişkenin adını kullanarak erişmektedir.
- **Indirect Access(Dolaylı Erişim):** Pointer adresinden değişken erişimidir.
- Genel amaçlar için `void` veri tipi kullanılır. Bütün veri tipleri ataması yapılabilir.

## Pointer
- Bir bellek adresini tutar ve yönetir.
- Pointer adresini (ptr), işaret edilen değeri (*ptr) ifade eder.
```c
int val = 10;
int *ptr = &val;
```
## Dinamik Bellek Yönetimi

- **malloc:** Bellekte belirtilen boyut kadar yer ayırır.
- **calloc:** Bellekte yer ayırır ve temizler.
- **realloc:** Daha önce ayrılan bellek alanını yeniden boyutlandırır.
- **free:** Ayrılan belleği serbest bırakır.

```c
int *arr = (int*)malloc(sizeof(int)*10);
free(arr);
```
## Temel Veri Yapıları

- **Stack:** Program derlenirken sabit boyutludur, LIFO mantığı kullanır.
- **Heap:** Çalışma zamanı sırasında dinamik bellek yönetimine izin verir.
- **Linked List (Bağlı Liste):** Veri parçalarını birbirine bağlayan pointerlarla dinamik hafıza yönetimi sağlar.