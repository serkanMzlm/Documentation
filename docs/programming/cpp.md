# C++ Programlama

- Bir değişkeni başlatmanın (initialization) üç yolu vardır:
    - Copy initialization (örneğin `int a = 5;`)
    - Direct initialization (örneğin `int b(5);`)
    - List initialization (örneğin `int c{5};`)

!!! note "Not"
    En güvenilir olanı list initialization’dır; çünkü daraltıcı (narrowing) dönüşümlere örneğin bir float değeri doğrudan int’e izin vermez.

- Tanımlanıp hiç kullanılmayan değişkenler için derleyici uyarı verir. Bu uyarıyı bastırmak için C++17’den itibaren `[[maybe_unused]]` özniteliğini kullanabiliriz.

- `::` operatörü, önünde bir isim (örneğin bir namespace veya sınıf adı) yoksa “global” ad alanını ifade eder.

```c++
#include <iostream>   // Standart giriş-çıkış kütüphanesi

using namespace std; // Tüm std isimlerini doğrudan kullanmamıza izin verir
// Sadece belli bir ögeyi dahil etmek istersek:
// using std::cout;

int main() {
    [[maybe_unused]] int a = 5; // copy initialization
    int b(5);                   // direct initialization
    int c{5};                   // list initialization — narrowing’a izin vermez
}
```

- `std::cin >>` operatörü boşluk karakterine kadar okurken,

- `std::getline(cin, str)` satır sonuna (veya belirlediğiniz başka bir sınırlayıcıya) kadar tüm girdi satırını alır. Bu sayede kullanıcıdan boşluk içeren satırlar veya tüm satırı almamız gerektiğinde **getline** tercih edilir.

- `namespace`, aynı isimli tanımlamaların çakışmasını önlemek için kullanılır.
    - Hem değişken, hem fonksiyon, hem de sınıf ve diğer tanımları ayrı gruplarda toplayabiliriz.
    - İç içe namespace’ler tanımlamak da mümkündür.

```c++
namespace A {
    namespace B {
        void foo();
    }
}
```

- `constexpr`, derleme zamanında hesaplanabilen ve kesinlikle sabit (compile-time constant) olan değerler için;
- `const` ise derlendikten sonra değiştirilemeyen, ama derleme zamanında mutlaka hesaplanamayabilecek değerler için kullanılır
- C++20 ile aggregate tipler için “designated initializer” desteği eklendi. Böylece yapıdaki sadece istediğiniz üyeyi atayabilirsiniz:
```c++
struct Foo { int a, b, c; };

Foo f0 {1, 2, 3};                   // Tüm üyeleri sırayla atar
Foo f1 {.a = 1, .c = 3};            // Sadece a ve c’yi atar; b = 0
// C++20’de üye sırası önemli değil:
Foo f2 {.c = 3, .a = 1};            // Geçerli
```
- `typedef` Eski C/C++ tarzı tip takma adı (alias) oluşturma.
- `using` C++11 ile gelen, daha okunur ve şablonlarla da çalışabilen modern alias tanımı.
```
// Eski stil
typedef unsigned long long ULLI1;

// Yeni, daha okunur stil
using ULLI2 = unsigned long long;
```

- **Öne sıfır koymak:** oktalık (ör. 012); ekrana oktal gösterimi için std::oct.
- **Öne 0x koymak:** onaltılık (ör. 0x12); ekrana onaltılık için std::hex.
- **Öne 0b koymak:** ikilik (ör. 0b10).
- C++14+’te büyük sayılarda okuryazılığını artırmak için tırnak: `100'000'000`.

## Döngüler ve Kontrol Yapıları
C++’ta klasik kontrol yapılarının yanı sıra range-based for (C++11) döngüsü de bulunur.

- `for (init; cond; inc)`, `while (cond)`, `do { … } while (cond);`
- `if (cond) { … } else { … }`
- `switch (expr) { case …: …; break; … }`
- `goto label; ` **(genellikle önerilmez; kod akışını karmaşıklaştırır)**

```c++ linenums="1" hl_lines="8-10"
#include <iostream>
#include <vector>
#include <string>
using namespace std::literals;  

int main() {
    std::vector words{ "peter"s, "likes"s, "frozen"s };
    for (auto& word : words) {
        std::cout << word << ' ';
    }
    // Çıktı: peter likes frozen 
}
```

## Programı Sonlandırma ve Hata Denetimi
- `std::exit(int status):` Programı “normal” olarak sonlandırır.
- `std::atexit(func):` Program kapanırken func()’u çağırır.
- `std::abort():` Programı aniden, “abnormal” biçimde sonlandırır.
- `assert(cond):` cond sağlanmazsa programı durdurur (yalnızca NDEBUG tanımlı değilken).

## Fonksiyonlar ve Inline Namespace
- **inline fonksiyon:** Derleme aşamasında çağrıldığı yere yerleştirilir (inlining), fonksiyon çağrısı yükünü azaltır.
- **inline namespace:** Birden fazla sürüm içeren kütüphanelerde, öntanımlı sürümü belirtmek için:

```c++
#include <iostream>

namespace V1 {
    void doSomething() { std::cout << "V1\n"; }
}

inline namespace V2 {
    void doSomething() { std::cout << "V2\n"; }
}

int main() {
    V1::doSomething(); // V1
    V2::doSomething(); // V2
    doSomething();     // inline namespace → V2
}
```

- constexpr vs. consteval vs. constexpr
    - **constexpr:** Derleme zamanında hesaplanabilir, fakat ihtiyaç duyulduğunda çalışma zamanında da çağrılabilir.
    - **consteval:** Her çağrısı derleme zamanında kesinlikle değerlendirilir (C++20).

## Lambda İfadeleri
- Anonim fonksiyon tanımı yapar:
```c++
auto f = []() { std::cout << "Hello\n"; };
f();
```
- Parametre ve dönüş:
```c++
auto div = [](int x, int y, bool intDiv) -> double {
    if (intDiv)
        return x / y;                   // int bölme, sonra double’a dönüştürülür
    else
        return static_cast<double>(x) / y;
};
```
- Yakalama listesi
    - `[=]` tüm yerel değişkenleri değer ile kopyalar
    - `[&]` tüm yerel değişkenlere referans verir
    - `[a, &b]` belli değişkeni değer, diğerini referans alır
    - mutable ile değer yakalansa bile içerde değiştirme izinli olur ama dışarı etkilemez

```
int a = 1, b = 2;
auto lv = [=]() mutable { a = 5; return a; }; // dışarıdaki a değişmez
auto lf = [&]()         { b = 5; return b; }; // dışarıdaki b değişir
```

## Şablonlar (Templates)
- C++’ta türden bağımsız kod yazmak için kullanılır.
- Eğer tüm argümanlar aynı türden ise < > yazmaya gerek yoktur.
- Farklı türler için genellikle türleri kendiniz belirtirsiniz.
```c++
#include <iostream>
template<typename T, typename U>
auto add(T a, U b) {
    return a + b;
}

int main() {
    std::cout << add(10, 3.5) << '\n';    // 13.5
    std::cout << add<int, double>(10, 3.5) << '\n'; // açık belirtilmiş
}
```
- Sınıf Şablonları ve Varsayılan Argüman
```c++
#include <iostream>
#include <typeinfo>

template<typename T, typename U = int>
class MyClass {
public:
    MyClass() {
        T value{};
        std::cout << typeid(value).name() << '\n';
    }
    template<typename X>
    void add(X a, X b) {
        std::cout << (a + b) << '\n';
    }
};

int main() {
    MyClass<double>    m1;    // U = int
    MyClass<char, long> m2;   // açık belirtildi
    m1.add(1.2, 3.4);
    m1.add<double>(5.5, 6.6);
}
```


## Bitset
```c++
#include <bitset>
bitset<5> bits;    // 5 bitlik bir dizi
cout << bits << endl;  // 00000
bits = 10;
cout << bits << endl;  // 01010
```

## Dinamik Bellek

```c++
char* buffer = new char[8];   // Bellekten dizi ayırma
delete[] buffer;              // Dizi silme

const int* ptr1; // İşaret ettiği değer sabit, adres değişebilir
int* const ptr2; // Adres sabit, işaret ettiği değer değişebilir

int (*array)[5] = new int[x][5];  // x×5’lik matris
```

## Smart Pointers

C++’ta ham işaretçiler (new/delete) yerine akıllı işaretçiler (smart pointers) kullanmak, bellek sızıntılarını ve çifte silme hatalarını büyük oranda ortadan kaldırır. Üç ana türü vardır:
1. `std::unique_ptr<T>` Tek sahipli akıllı işaretçidir. Sahip olduğu nesnenin bellekten silinmesini otomatikleştirir: unique_ptr kapsam (scope) sonlandığında delete çağrılır. Kopyalanamaz, ancak taşınabilir (std::move ile). Bu sayede çifte silme riskini ve bellek sızıntılarını önler. İsterseniz özel bir silici (deleter) de tanımlayabilirsiniz:

```c++
auto fileCloser = [](FILE* f){ if(f) fclose(f); };
std::unique_ptr<FILE, decltype(fileCloser)> fptr(fopen("log.txt","r"), fileCloser);

```

2. `std::shared_ptr<T>` Paylaşımlı sahiplik sağlayan işaretçidir. Aynı nesneye birden fazla shared_ptr işaret edebilir ve referans sayısı (use_count) sıfırlanana dek nesne silinmez. Döngüsel bağımlılık riskini kırmak için std::weak_ptr kullanarak “zayıf” işaretçi oluşturabilirsiniz. Referans sayısını yönetmek için atomik işlemler kullandığından, unique_ptr’a göre biraz daha maliyetlidir.

3. `std::make_unique<T>(...)` ve `std::make_shared<T>(...)` C++14/17 ile gelen fabrikalar (factory functions). Ham işaretçi yerine doğrudan akıllı işaretçi oluşturur ve exception safety (istisna güvenliği) sağlar:

```c++
auto up = std::make_unique<MyClass>(ctorArg1, ctorArg2);
auto sp = std::make_shared<MyClass>(ctorArg1, ctorArg2);
```

- **make_unique:** yalnızca bir sahipli unique_ptr döndürür.
- **make_shared:** nesneyi ve referans sayacı tek bir bellek bloğunda tutar, daha verimli bellek kullanımı sunar.


## Casting Türleri
1. **static_cast<T>(x):** Derleme zamanında güvenli dönüşümler (örneğin sayı türleri arası).
2. **dynamic_cast<T*>(p):** Çalışma zamanında polimorfik sınıflar arasında güvenli downcast/upcast.
3. **const_cast<T&>(x):** const niteliğini kaldırmak için.
4. **reinterpret_cast<T*>(p):** İki iş parçacığı arasındaki bellek gösterimini yeniden yorumlamak için (çok dikkatli kullanılmalı).

```c++
float  f   = 3.14f;
int    i1  = static_cast<int>(f);

class Base { virtual void foo() {} };
class Derived : public Base {};

Base*       bp = new Derived();
Derived*    dp = dynamic_cast<Derived*>(bp);

const int   x  = 5;
int&        y  = const_cast<int&>(x);

int*        p  = reinterpret_cast<int*>(0x1234);
```

## `std::vector` vs. `std::array`

C++’ta dinamik ve sabit boyutlu diziler için iki temel sınıf şablonu vardır:

- `std::vector<T>`
    - Dinamik boyutlu dizi. Eleman ekledikçe otomatik büyür.
    - Rastgele erişim için `operator[]` ve sınır denetimli `at()` sağlar.
    - `vec[i]` hızlı ama sınır denetimsiz.
    - `vec.at(i)` sınır dışı erişimde std::out_of_range fırlatır.

```c++
#include <vector>
#include <iostream>

int main() {
    std::vector<int> primes{2, 3, 5, 7};  // Başlangıç listesiyle 4 eleman
    std::vector<int> data(10);            // 10 elemanlı, tüm değerler 0

    // Hatalı: doğrudan tek sayıyla vektör oluşturulamaz
    // std::vector<int> v1 = 10;  

    // Doğru: tek elemanlı vektör
    std::vector<int> v1{10};

    // Erişim
    std::cout << primes[1] << "\n";       // 3
    std::cout << v1.at(0) << "\n";        // 10

    // Boyut öğrenme
    std::cout << "Size: "     << v1.size()     << "\n";
    std::cout << "Capacity: " << v1.capacity() << "\n";
}
```

- `std::array<T, N>`
    - Sabit boyutlu dizi; derleme zamanında N belirlenir.
    - İçinde C stili dizilere benzer şekilde çalışır, ama STL ile uyumludur.
    - `size()`, `at()`, `operator[]`, `begin()`, `end()` gibi üyelere sahiptir.
```c++
#include <array>
#include <iostream>

int main() {
    std::array<int, 5> arr = {10, 20, 30, 40, 50};
    for (std::size_t i = 0; i < arr.size(); ++i) {
        std::cout << arr[i] << ' ';
    }
    // Çıktı: 10 20 30 40 50 
}
```

- `empty():` Boşsa true, değilse false döner.
- `size():` Geçerli eleman sayısını döner.
- `capacity():` Ayrılmış bellek kapasitesini döner; size() ≤ capacity().
- `reserve(n):` Kapasiteyi en az n olacak şekilde önceden ayırır; yeniden tahsisi azaltır.
- `resize(n):` Boyutu n olarak ayarlar; yeni elemanlar varsayılan T ctor ile oluşturulur.
- `push_back(val):` Sonuna bir kopya ekler.
- `emplace_back(args…):` Nesneyi yerinde (in-place) oluşturur; kopya maliyeti yok.
- `pop_back():` Son elemanı siler.
- `front() / back():` İlk ve son elemanı referans olarak döner.

```c++
#include <vector>
#include <iostream>

int main() {
    std::vector<int> v;
    std::cout << std::boolalpha << "Empty? " << v.empty() << "\n";

    v.reserve(5);              // Kapasite ≥ 5
    for (int i = 1; i <= 5; ++i) {
        v.emplace_back(i * 10); // 10, 20, 30, 40, 50
    }

    std::cout << "Size: "     << v.size()     << "\n"; // 5
    std::cout << "Capacity: " << v.capacity() << "\n";

    v.resize(3);               // artık {10,20,30}
    std::cout << "After resize: ";
    for (auto x : v) std::cout << x << ' ';
    std::cout << "\n";

    v.push_back(99);           // {10,20,30,99}
    std::cout << "Last element: " << v.back() << "\n";

    v.pop_back();              // son elemanı kaldırır
    std::cout << "After pop: ";
    for (auto x : v) std::cout << x << ' ';
    std::cout << "\n";
}
```

## Nesne Yönelimli Programlama (OOP)

1. Erişim Belirleyicileri (Access Specifiers)
    - `public`: Her yerden erişilebilir.
    - `protected`: Sadece o sınıf içinden ve ondan türeyen alt sınıflardan erişilebilir.
    - `private`: Yalnızca tanımlandığı sınıfın içinden erişilebilir. (Varsayılan erişim seviyesi sınıf (class) içinde private’dır.)
```c++
class A {
public:
    void pub();      // her yerden
protected:
    void prot();     // A ve alt sınıflar
private:
    void priv();     // sadece A içinde
};
```

2. Özel Üye Fonksiyonlar: `Default`, `Delete`, `Special Members`
    - C++ derleyici, sınıfınız için eğer belirtmezseniz aşağıdakileri “default” olarak oluşturur:
    - Varsayılan yapıcı (default constructor)
    - Yıkıcı (destructor)
    - Kopya yapıcı (copy constructor)
    - Atama operatörü (copy assignment operator)
```c++
class MyClass {
public:
    MyClass(int x) : value(x) {}     // Özel yapıcı
    MyClass() = default;             // Derleyici yapıcıyı oluşturursun
    MyClass(const MyClass&) = delete;           // Kopyalamayı yasakla
    MyClass& operator=(const MyClass&) = delete; // Atamayı yasakla
    ~MyClass() = default;            // Varsayılan yıkıcı
private:
    int value;
};
```

3. `this` İşaretçisi ve Başlatıcı Listesi
    - `this`, içinde bulunduğunuz nesnenin adresini gösteren işaretçidir.
    - Üye başlatma listesi (`initializer list`) ile, yapıcı gövdesine girmeden önce üye değişkenlerinizi atayabilirsiniz; böylece çakışan isimlerde de netlik sağlanır:
```c++
class Point {
int x, y;
public:
Point(int x, int y) : x(x), y(y) {}  // soldaki x: üye, sağdaki: parametre
Point() : x(0), y(0) {}              // default yapıcı
};
```

4. Kapsülleme (Encapsulation)
    - Sınıfın iç verilerini private tutup, public getter/ setter metotlarıyla korumalı erişim sağlarsınız:
```c++
class Rectangle {
private:
    double width, height;
public:
    void setWidth(double w) { width = w; }
    double getWidth() const { return width; }
    // Benzer setter/getter’lar height için de...
};
```

5. Static Üyeler
    - static üye değişken ve fonksiyonlar sınıfa ait, nesneye değil.
    - static üye fonksiyonlar sadece başka static üyelere erişebilir.
    - Tanımı sınıf dışına bir kez yapılmalıdır:
```c++
class Counter {
public:
    static int count;
    Counter() { ++count; }
};
int Counter::count = 0;  // sınıf dışı tanım

// Kullanım:
Counter a, b;
std::cout << Counter::count;  // 2
```

6. Friend (Arkadaş) Bildirimi
    - `friend` fonksiyon veya sınıflar, private/protected üyelere erişebilir:
```c++
class A {
    int x = 42;
    friend void printA(const A&);
};

void printA(const A& a) {
    std::cout << a.x << "\n";  // private x’e erişim
}
```

7. Operatör Aşırı Yükleme (Operator Overloading)
    - Sınıfınızda var olan operatörleri özelleştirebilirsiniz; ancak tüm operatörler yüklenemez (sizeof, ::, .* vb. yasaktır).
```c++
class Vec {
    int x, y;
public:
    Vec(int x, int y) : x(x), y(y) {}
    Vec operator+(const Vec& r) const { return Vec{x + r.x, y + r.y}; }
    Vec& operator+=(const Vec& r) { x += r.x; y += r.y; return *this; }
    friend std::ostream& operator<<(std::ostream& os, const Vec& v) {
        return os << "(" << v.x << ", " << v.y << ")";
    }
};
```

8. `const` Üyeler ve mutable
    - `const` üye fonksiyonlar (void f() const) sınıf verilerini değiştirmez.
    - `mutable` ile işaretlenmiş üye değişkenler, const fonksiyonlar içinde bile değiştirilebilir:
```cpp
class Example {
    mutable int cache;
public:
    Example() : cache(0) {}
    int getCache() const { return cache++; } // derleme hatası yok
};
```

9. `explicit` Yapıcılar
    - Tek parametreli yapıcılar için explicit koyarak istenmeyen otomat dönüşümleri önlersiniz:
```cpp
class Foo {
public:
    explicit Foo(int x);
};

void bar(Foo f);

bar(10);       // HATA: otomatik dönüşüm yasak
bar(Foo(10));  // DOĞRU
```

10. Kalıtım (Inheritance)

    - Tekli kalıtım: class Derived : access Base { … };
    - Erişim: public, protected veya private (default private).
```cpp
class Base { public: void foo(); };
class Derived : public Base { /* Base::foo() kullanabilir */ };

class A { public: void f(); };
class B { public: void g(); };
class C : public A, public B { /* hem f() hem g() */ };
```
    - Virtüel kalıtım (diamond problemini çözer):
```cpp
class Device { /*…*/ };
class Scanner : virtual public Device { /*…*/ };
class Printer : virtual public Device { /*…*/ };
class Copier  : public Scanner, public Printer { /*…*/ };
```

11. Polimorfizm (Çok Biçimlilik)
    - virtual üye fonksiyonlar, çağrının gerçek nesne türüne göre çözülmesini sağlar.
```cpp
class Base {
public:
    virtual void work() { std::cout<<"Base\n"; }
    virtual ~Base() = default;  // sanal yıkıcı
};

class Derived : public Base {
public:
    void work() override { std::cout<<"Derived\n"; }
};

int main() {
    Base* p = new Derived();
    p->work();   // “Derived” yazdırır
    delete p;
}
```
    - **Saf sanal fonksiyon** (pure virtual) ile soyut sınıf (interface) oluşturabilirsiniz:
```c++
class IShape {
public:
    virtual double area() const = 0;  // saf sanal
    virtual ~IShape() = default;
};
```
