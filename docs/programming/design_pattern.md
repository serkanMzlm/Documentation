# Design Pattern

## Creational Design Patterns

Mevcut kodun esnekliğini ve yeniden kullanımını artıran çeşitli nesne oluşturma mekanizmaları sağlar.

!!! note "Not"
    - **Factory Method** "hangi sınıf üretilecek",
    - **Abstract Factory** "hangi ürün ailesi üretilecek",
    - **Builder** "nasıl üretilecek",
    - **Prototype** "mevcut nesne nasıl kopyalanacak",
    - **Singleton** "yalnızca bir tane üretilecek" sorusuna cevap verir.

### **Factory Method** 
Nesne oluşturma sürecini merkezileştirip, alt sınıflara hangi sınıfın üretileceğine karar verme yetkisi vermek. **"Nesne oluşturma işini soyutla, hangi tür nesnenin üretileceğini alt sınıf belirlesin."**

```c++
class Transport {
public:
    virtual void deliver() = 0;
    virtual ~Transport() = default;
};

class Truck : public Transport {
public:
    void deliver() override { std::cout << "Deliver by land using a Truck." << std::endl; }
};

class Ship : public Transport {
public:
    void deliver() override { std::cout << "Deliver by sea using a Ship." << std::endl; }
};

class Logistics {
public:
    virtual ~Logistics() = default;
    virtual std::unique_ptr<Transport> createTransport() = 0;
    void planDelivery() {
        auto transport = createTransport();
        transport->deliver();
    }
};

class RoadLogistics : public Logistics {
public:
    std::unique_ptr<Transport> createTransport() override {
        return std::make_unique<Truck>();
    }
};

class SeaLogistics : public Logistics {
public:
    std::unique_ptr<Transport> createTransport() override {
        return std::make_unique<Ship>();
    }
};

int main() {
    std::unique_ptr<Logistics> logistics;

    logistics = std::make_unique<RoadLogistics>();
    logistics->planDelivery(); // Truck

    logistics = std::make_unique<SeaLogistics>();
    logistics->planDelivery(); // Ship
}
```

### **Abstract Factory**
Birbirleriyle ilişkili veya bağımlı nesne ailelerini, somut sınıf isimlerini belirtmeden oluşturmak.
Yani sadece tek bir nesneyi değil, uyumlu bir nesne grubunu üretir. **"Factory Method bir ürünü seçtirir, Abstract Factory bir ürün ailesini yönetir."**

```c++
class Button {
public:
    virtual void draw() = 0;
    virtual ~Button() = default;
};

class Checkbox {
public:
    virtual void draw() = 0;
    virtual ~Checkbox() = default;
};

class WinButton : public Button {
public:
    void draw() override { std::cout << "Draw Windows Button\n"; }
};

class WinCheckbox : public Checkbox {
public:
    void draw() override { std::cout << "Draw Windows Checkbox\n"; }
};

class MacButton : public Button {
public:
    void draw() override { std::cout << "Draw Mac Button\n"; }
};

class MacCheckbox : public Checkbox {
public:
    void draw() override { std::cout << "Draw Mac Checkbox\n"; }
};

class GUIFactory {
public:
    virtual std::unique_ptr<Button> createButton() = 0;
    virtual std::unique_ptr<Checkbox> createCheckbox() = 0;
    virtual ~GUIFactory() = default;
};

class WindowsFactory : public GUIFactory {
public:
    std::unique_ptr<Button> createButton() override {
        return std::make_unique<WinButton>();
    }
    std::unique_ptr<Checkbox> createCheckbox() override {
        return std::make_unique<WinCheckbox>();
    }
};

class MacFactory : public GUIFactory {
public:
    std::unique_ptr<Button> createButton() override {
        return std::make_unique<MacButton>();
    }
    std::unique_ptr<Checkbox> createCheckbox() override {
        return std::make_unique<MacCheckbox>();
    }
};

class Application {
    std::unique_ptr<Button> button;
    std::unique_ptr<Checkbox> checkbox;
public:
    Application(std::unique_ptr<GUIFactory> factory) {
        button = factory->createButton();
        checkbox = factory->createCheckbox();
    }
    void render() {
        button->draw();
        checkbox->draw();
    }
};

int main() {
    std::unique_ptr<GUIFactory> factory;

    factory = std::make_unique<WindowsFactory>();
    Application app1(std::move(factory));
    app1.render();

    factory = std::make_unique<MacFactory>();
    Application app2(std::move(factory));
    app2.render();
}
```

### **Builder**
karmaşık bir nesnenin oluşturulma sürecini adım adım kontrol edebilmek için kullanılan bir creational (oluşturucu) tasarım desenidir. Burada amaç, bir nesnenin nasıl oluşturulacağıyla neyin oluşturulacağının ayrıştırılmasıdır. **"Karmaşık bir nesnenin oluşturma sürecini, o nesnenin temsilinden ayır."**

Bu desen, çok fazla parametreye, isteğe bağlı bileşene veya oluşturma adımına sahip nesnelerde yapıcı **fonksiyonların karmaşık hale gelmesini engeller**.
`new` veya constructor parametrelerinin sayısı arttığında kodun okunabilirliği düşer; Builder bu problemi çözer.

```c++
class Drone {
    std::string frame;
    std::string controller;
    std::string camera;
    std::string communication;

public:
    void setFrame(const std::string& f) { frame = f; }
    void setController(const std::string& c) { controller = c; }
    void setCamera(const std::string& cam) { camera = cam; }
    void setCommunication(const std::string& comm) { communication = comm; }

    void showSpec() const {
        std::cout << "Frame: " << frame << "\n";
        std::cout << "Controller: " << controller << "\n";
        std::cout << "Camera: " << camera << "\n";
        std::cout << "Comm: " << communication << "\n\n";
    }
};

class DroneBuilder {
protected:
    std::unique_ptr<Drone> drone;

public:
    DroneBuilder() { drone = std::make_unique<Drone>(); }
    virtual ~DroneBuilder() = default;

    virtual void buildFrame() = 0;
    virtual void buildController() = 0;
    virtual void buildCamera() = 0;
    virtual void buildCommunication() = 0;

    std::unique_ptr<Drone> getDrone() { return std::move(drone); }
};

class RacingDroneBuilder : public DroneBuilder {
public:
    void buildFrame() override { drone->setFrame("Carbon Fiber"); }
    void buildController() override { drone->setController("Pixhawk 4"); }
    void buildCamera() override { drone->setCamera("Horus AI"); }
    void buildCommunication() override { drone->setCommunication("5.8GHz FPV"); }
};

class SurveyDroneBuilder : public DroneBuilder {
public:
    void buildFrame() override { drone->setFrame("Lightweight Aluminum"); }
    void buildController() override { drone->setController("Cube Orange"); }
    void buildCamera() override { drone->setCamera("RealSense D455"); }
    void buildCommunication() override { drone->setCommunication("LTE Module"); }
};

class Director {
public:
    std::unique_ptr<Drone> createDrone(DroneBuilder& builder) {
        builder.buildFrame();
        builder.buildController();
        builder.buildCamera();
        builder.buildCommunication();
        return builder.getDrone();
    }
};

int main() {
    Director director;

    RacingDroneBuilder racingBuilder;
    SurveyDroneBuilder surveyBuilder;

    auto racingDrone = director.createDrone(racingBuilder);
    auto surveyDrone = director.createDrone(surveyBuilder);

    racingDrone->showSpec();
    surveyDrone->showSpec();
}
```

### **Prototype**
Bir nesnenin oluşturulmasını, doğrudan sınıf kurucusunu çağırmak yerine, var olan bir örneğin kopyalanması (clone edilmesi) yoluyla yapan bir creational (oluşturucu) tasarım desenidir. Yani sıfırdan `new` ile inşa etmek yerine, mevcut bir “örnek nesne”yi klonlayarak yeni nesneler elde edersin. **"Yeni nesne üretmek için bir şablonu (prototype) kopyala."**

```C++
class Vehicle {
public:
    virtual std::unique_ptr<Vehicle> clone() const = 0;
    virtual void info() const = 0;
    virtual ~Vehicle() = default;
};

class Drone : public Vehicle {
public:
    std::unique_ptr<Vehicle> clone() const override {
        return std::make_unique<Drone>(*this);
    }
    void info() const override { std::cout << "Drone\n"; }
};

class Car : public Vehicle {
public:
    std::unique_ptr<Vehicle> clone() const override {
        return std::make_unique<Car>(*this);
    }
    void info() const override { std::cout << "Car\n"; }
};

int main() {
    std::unique_ptr<Vehicle> v = std::make_unique<Drone>();
    auto copy1 = v->clone();   // Prototype: doğru türe göre kopya (runtime)
    copy1->info();

    Vehicle* raw = v.get();
    Vehicle* copy2 = new Vehicle(*raw); // HATA: soyut sınıf kopyalanamaz!
}
```

!!! note "Not"
    `=` veya copy constructor, base sınıf üzerinden polymorphic kopyalama yapamaz.
    Yani elinde `Vehicle*` varsa ve onun aslında Drone olduğunu bilmiyorsan, C++’ın kopya işlemi doğru tipte nesne üretemez çünkü derleyici sadece Vehicle’ı görür.

### **Singleton**
Bir sınıfın sistem genelinde yalnızca tek bir örneğe sahip olmasını ve bu örneğe her yerden kontrollü erişim sağlanmasını garantileyen bir creational (oluşturucu) tasarım desenidi. Amaç, uygulama boyunca merkezi bir “tekil kontrol noktası” oluşturmaktır. Yani bu sınıfın örneği yalnız bir defa yaratılır; aynı nesneye tüm kod erişir. **"Bir sınıftan sadece bir nesne olmalı ve o nesneye global erişim sağlanmalı."**

```c++
class Logger {
private:
    static std::unique_ptr<Logger> instance;
    static std::mutex mtx;

    Logger() { std::cout << "Logger initialized\n"; }  

public:
    Logger(const Logger&) = delete;            
    Logger& operator=(const Logger&) = delete; 

    static Logger& getInstance() {
        std::lock_guard<std::mutex> lock(mtx);
        if (!instance)
            instance.reset(new Logger());
        return *instance;
    }

    void log(const std::string& msg) {
        std::cout << "[LOG] " << msg << "\n";
    }
};

std::unique_ptr<Logger> Logger::instance = nullptr;
std::mutex Logger::mtx;

int main() {
    auto& log1 = Logger::getInstance();
    log1.log("System started");

    auto& log2 = Logger::getInstance();
    log2.log("Connection established");

    if (&log1 == &log2)
        std::cout << "Both references point to the same instance.\n";
}
```

## Structural Design Patterns
Structural (Yapısal) Design Pattern’lar, yazılım sisteminde sınıfların ve nesnelerin birbiriyle nasıl ilişkilendirileceğini tanımlayan kalıplardır. Amaç, karmaşık sistemleri daha esnek, yeniden kullanılabilir ve sürdürülebilir hale getirmektir.

!!! note "Not"
    - **Adapter**	Uyumsuz arayüzleri birbirine dönüştürür (arayüz çevirmeni).
    - **Bridge**	Soyutlama ile implementasyonu ayırarak bağımsız gelişmelerini sağlar.
    - **Composite**	Nesneleri ağaç yapısında birleştirip tek bir nesne gibi işlem yapar.
    - **Decorator**	Nesnelere çalışma zamanında yeni davranışlar ekler.
    - **Facade**	Karmaşık sistemi basitleştiren tek bir arayüz sunar.
    - **Flyweight**	Çok sayıda küçük nesneyi paylaşım yoluyla hafifletir.
    - **Proxy**	Başka bir nesneye erişimi kontrol eden vekil sağlar.

### Adapter
Birbirleriyle uyumsuz arayüzlere sahip sınıfların birlikte çalışabilmesini sağlar. Amaç, mevcut bir sınıfın arayüzünü, onu kullanmak isteyen başka bir sınıfın beklediği formata dönüştürmektir. **"Var olan bir sınıfı değiştirmeden, onun arayüzünü başka bir arayüze uydur."**

```c++
class ITarget {
public:
    virtual void request() = 0;
    virtual ~ITarget() = default;
};

class LegacySystem {
public:
    void specificRequest() {
        std::cout << "Legacy system specific operation.\n";
    }
};

class Adapter : public ITarget {
private:
    LegacySystem* legacy;
public:
    Adapter(LegacySystem* l) : legacy(l) {}
    void request() override {
        legacy->specificRequest();
    }
};

int main() {
    LegacySystem oldSystem;
    Adapter adapter(&oldSystem);

    ITarget* client = &adapter;
    client->request();
}
```

### Bridge
Bir sınıfın soyutlamasını (abstraction) onun uygulamasından (implementation) ayırarak, her ikisinin de birbirinden bağımsız olarak geliştirilmesini sağlayan bir structural (yapısal) tasarım desenidir.

```c++
class Device {
public:
    virtual void turnOn() = 0;
    virtual void turnOff() = 0;
    virtual ~Device() = default;
};

// Somut implementasyonlar
class TV : public Device {
public:
    void turnOn() override { std::cout << "TV turned on.\n"; }
    void turnOff() override { std::cout << "TV turned off.\n"; }
};

class Radio : public Device {
public:
    void turnOn() override { std::cout << "Radio turned on.\n"; }
    void turnOff() override { std::cout << "Radio turned off.\n"; }
};

// Soyutlama (Abstraction)
class RemoteControl {
protected:
    std::shared_ptr<Device> device;
public:
    RemoteControl(std::shared_ptr<Device> dev) : device(dev) {}
    virtual void togglePower(bool on) {
        if (on) device->turnOn();
        else device->turnOff();
    }
    virtual ~RemoteControl() = default;
};

// Genişletilmiş soyutlama (Refined Abstraction)
class AdvancedRemote : public RemoteControl {
public:
    AdvancedRemote(std::shared_ptr<Device> dev) : RemoteControl(dev) {}
    void mute() {
        std::cout << "Device muted.\n";
    }
};

int main() {
    std::shared_ptr<Device> tv = std::make_shared<TV>();
    std::shared_ptr<Device> radio = std::make_shared<Radio>();

    AdvancedRemote remote1(tv);
    AdvancedRemote remote2(radio);

    remote1.togglePower(true);
    remote2.togglePower(true);
    remote2.mute();
}
```

### Composite
Nesneleri ağaç yapısı biçiminde organize edip, tek bir nesne gibi işlem yapmayı sağlayan yapısal (structural) bir tasarım desenidir.

```c++
class Component {
public:
    virtual void operation() = 0;
    virtual void add(std::shared_ptr<Component> comp) {}
    virtual void remove(std::shared_ptr<Component> comp) {}
    virtual ~Component() = default;
};

// Yaprak (Leaf)
class Sensor : public Component {
    std::string name;
public:
    Sensor(const std::string& n) : name(n) {}
    void operation() override {
        std::cout << "Reading data from " << name << " sensor.\n";
    }
};

// Bileşik (Composite)
class SensorGroup : public Component {
    std::vector<std::shared_ptr<Component>> children;
public:
    void add(std::shared_ptr<Component> comp) override {
        children.push_back(comp);
    }
    void remove(std::shared_ptr<Component> comp) override {
        // basit örnek için silme işlemi boş bırakıldı
    }
    void operation() override {
        std::cout << "Collecting data from sensor group...\n";
        for (auto& child : children)
            child->operation();
    }
};

int main() {
    auto imu = std::make_shared<Sensor>("IMU");
    auto gps = std::make_shared<Sensor>("GPS");
    auto baro = std::make_shared<Sensor>("Barometer");

    auto flightSensors = std::make_shared<SensorGroup>();
    flightSensors->add(imu);
    flightSensors->add(gps);
    flightSensors->add(baro);

    auto allSensors = std::make_shared<SensorGroup>();
    allSensors->add(flightSensors);
    allSensors->add(std::make_shared<Sensor>("Camera"));

    allSensors->operation();
}
```

### Decorator
Bir nesnenin davranışını çalışma zamanında (runtime) değiştirmeye veya genişletmeye olanak tanır ancak sınıfın kendisini değiştirmeden. **"Var olan bir sınıfa yeni özellikler ekle ama kalıtım (inheritance) kullanmadan."**

```c++
class Drink {
public:
    virtual void serve() = 0;
    virtual ~Drink() = default;
};

// Temel sınıf
class Coffee : public Drink {
public:
    void serve() override {
        std::cout << "Plain coffee";
    }
};

// Decorator (sarmalayıcı)
class DrinkDecorator : public Drink {
protected:
    std::shared_ptr<Drink> drink;
public:
    DrinkDecorator(std::shared_ptr<Drink> d) : drink(d) {}
    void serve() override {
        drink->serve();
    }
};

// Somut decorator — Süt ekle
class MilkDecorator : public DrinkDecorator {
public:
    MilkDecorator(std::shared_ptr<Drink> d) : DrinkDecorator(d) {}
    void serve() override {
        drink->serve();
        std::cout << " + milk";
    }
};

// Somut decorator — Şeker ekle
class SugarDecorator : public DrinkDecorator {
public:
    SugarDecorator(std::shared_ptr<Drink> d) : DrinkDecorator(d) {}
    void serve() override {
        drink->serve();
        std::cout << " + sugar";
    }
};

int main() {
    std::shared_ptr<Drink> coffee = std::make_shared<Coffee>();
    std::shared_ptr<Drink> sweetMilkCoffee =
        std::make_shared<SugarDecorator>(
            std::make_shared<MilkDecorator>(coffee));

    sweetMilkCoffee->serve();
}
```

### Facade
Karmaşık bir sistemi tek ve basit bir arayüz üzerinden kullanmanı sağlar. Yani sistemin içinde bir sürü alt bileşen, karmaşık işlem, modül, fonksiyon olabilir; ama sen bunları tek tek bilmek zorunda kalmazsın. Facade, tüm bu karmaşık yapıyı senin yerine yönetir.

```c++
class Engine {
public:
    void start() { std::cout << "Engine started.\n"; }
};

class Camera {
public:
    void calibrate() { std::cout << "Camera calibrated.\n"; }
};

class GPS {
public:
    void lock() { std::cout << "GPS signal locked.\n"; }
};

// Facade sınıfı
class DroneFacade {
private:
    Engine engine;
    Camera camera;
    GPS gps;

public:
    void startDrone() {
        std::cout << "Starting drone sequence...\n";
        gps.lock();
        camera.calibrate();
        engine.start();
        std::cout << "Drone is ready!\n";
    }
};

int main() {
    DroneFacade drone;
    drone.startDrone();  // sadece 1 basit çağrı
}
```

### Flyweight
Çok sayıda benzer nesne oluşturmak gerektiğinde,tekrarlanan verileri paylaştırarak bellek kullanımını azaltan bir yapısal (structural) tasarım desenidir.

```c++
#include <iostream>
#include <map>
#include <memory>

class Letter {
    char symbol; // paylaşılan veri
public:
    Letter(char s) : symbol(s) {}
    void display(int position) {
        std::cout << "Letter '" << symbol << "' at position " << position << "\n";
    }
};

class LetterFactory {
    std::map<char, std::shared_ptr<Letter>> letters;
public:
    std::shared_ptr<Letter> getLetter(char symbol) {
        if (letters.find(symbol) == letters.end()) {
            letters[symbol] = std::make_shared<Letter>(symbol);
            std::cout << "Creating new Letter: " << symbol << "\n";
        }
        return letters[symbol];
    }
};

int main() {
    LetterFactory factory;

    auto A1 = factory.getLetter('A');
    auto A2 = factory.getLetter('A');
    auto B1 = factory.getLetter('B');

    A1->display(1);
    A2->display(5);
    B1->display(3);
}
```

### Proxy
Bir nesneye erişimi kontrol etmek için kullanılan yapısal (structural) bir tasarım desenidir. Bu vekil, gerçek nesneye giden çağrıları yakalayabilir, öncesinde/sonrasında işlem yapabilir, ya da gerçek nesneyi gizleyebilir.

```c++
#include <iostream>
#include <memory>
#include <string>

// Ortak arayüz
class Video {
public:
    virtual void play() = 0;
    virtual ~Video() = default;
};

// Gerçek nesne
class RealVideo : public Video {
    std::string filename;
public:
    RealVideo(const std::string& f) : filename(f) {
        std::cout << "Loading video file: " << filename << "\n";
    }
    void play() override {
        std::cout << "Playing video: " << filename << "\n";
    }
};

// Proxy (vekil)
class VideoProxy : public Video {
    std::string filename;
    std::unique_ptr<RealVideo> realVideo;
public:
    VideoProxy(const std::string& f) : filename(f) {}

    void play() override {
        if (!realVideo) {  // ilk kez oynatıldığında dosyayı yükle
            realVideo = std::make_unique<RealVideo>(filename);
        }
        std::cout << "Proxy delegating play()...\n";
        realVideo->play();
    }
};

int main() {
    VideoProxy movie("mission_video.mp4");

    std::cout << "User presses play...\n";
    movie.play();  // ilk seferde yükler ve oynatır

    std::cout << "User presses play again...\n";
    movie.play();  // ikinci seferde yüklemez, direkt oynatır
}
```

## Behavioral Design Patterns
Nesnelerin nasıl iletişim kurduğunu ve birbirleriyle nasıl iş birliği yaptığını tanımlayan desenlerdir.

!!! note "Not"
    - **Chain of Responsibility**	Bir isteği sırayla farklı nesnelerin ele almasını sağlar.
    - **Command**	İstekleri nesne olarak paketler, geri alınabilir (undo) işlemler sağlar.
    - **Interpreter**	Basit bir dil veya kurallar kümesini çözümlemek için yorumlayıcı oluşturur.
    - **Iterator**	Koleksiyon elemanlarını tek tek gezmek için standart bir arayüz sunar.
    - **Mediator**	Nesneler arası iletişimi merkezi bir aracı üzerinden yönetir.
    - **Memento**	Nesnenin önceki durumunu saklar ve geri yüklemeyi sağlar.
    - **Observer**	Bir nesnedeki değişimi diğer bağlı nesnelere otomatik iletir.
    - **State**	Nesnenin davranışını mevcut durumuna göre değiştirir.
    - **Strategy**	Bir algoritmayı runtime’da seçilebilir hale getirir.
    - **Template** Method	Bir algoritmanın iskeletini tanımlar, alt sınıflar adımlarını özelleştirir.
    - **Visitor**	Nesnelere yeni operasyonlar eklerken sınıflarını değiştirmez.

### Chain of Responsibility
bir isteği bir dizi nesne üzerinden sırayla ileterek,isteği ilk uygun nesnenin işlemesine izin veren bir behavioral (davranışsal) tasarım desenidir

```C++
#include <iostream>
#include <memory>
#include <string>

// Zincirdeki ortak arayüz
class Handler {
protected:
    std::shared_ptr<Handler> next;
public:
    void setNext(std::shared_ptr<Handler> n) { next = n; }
    virtual void handleRequest(const std::string& req) {
        if (next) next->handleRequest(req);
    }
    virtual ~Handler() = default;
};

// Somut işlemciler
class SupportHandler : public Handler {
public:
    void handleRequest(const std::string& req) override {
        if (req == "basic") {
            std::cout << "Support: Basic request handled.\n";
        } else if (next) {
            std::cout << "Support: Passing to next...\n";
            next->handleRequest(req);
        }
    }
};

class TechHandler : public Handler {
public:
    void handleRequest(const std::string& req) override {
        if (req == "technical") {
            std::cout << "Tech: Technical request handled.\n";
        } else if (next) {
            std::cout << "Tech: Passing to next...\n";
            next->handleRequest(req);
        }
    }
};

class ManagerHandler : public Handler {
public:
    void handleRequest(const std::string& req) override {
        if (req == "urgent") {
            std::cout << "Manager: Urgent request handled.\n";
        } else {
            std::cout << "Manager: Request not recognized.\n";
        }
    }
};

int main() {
    auto support = std::make_shared<SupportHandler>();
    auto tech = std::make_shared<TechHandler>();
    auto manager = std::make_shared<ManagerHandler>();

    support->setNext(tech);
    tech->setNext(manager);

    support->handleRequest("basic");
    support->handleRequest("technical");
    support->handleRequest("urgent");
    support->handleRequest("unknown");
}
```

### Command
Bir işlemi veya isteği nesne olarak paketleyen davranışsal (behavioral) bir tasarım desenidir.
```c++
#include <iostream>
#include <memory>
#include <vector>

// Receiver (Komutu gerçekleştiren)
class Drone {
public:
    void takeOff() { std::cout << "Drone taking off.\n"; }
    void land() { std::cout << "Drone landing.\n"; }
};

// Command arayüzü
class Command {
public:
    virtual void execute() = 0;
    virtual void undo() = 0;
    virtual ~Command() = default;
};

// Concrete Commands
class TakeOffCommand : public Command {
    Drone& drone;
public:
    TakeOffCommand(Drone& d) : drone(d) {}
    void execute() override { drone.takeOff(); }
    void undo() override { drone.land(); }
};

class LandCommand : public Command {
    Drone& drone;
public:
    LandCommand(Drone& d) : drone(d) {}
    void execute() override { drone.land(); }
    void undo() override { drone.takeOff(); }
};

// Invoker (Komutu çağıran)
class RemoteControl {
    std::vector<std::shared_ptr<Command>> history;
public:
    void press(std::shared_ptr<Command> cmd) {
        cmd->execute();
        history.push_back(cmd);
    }

    void pressUndo() {
        if (!history.empty()) {
            history.back()->undo();
            history.pop_back();
        }
    }
};

int main() {
    Drone drone;
    RemoteControl remote;

    auto takeoff = std::make_shared<TakeOffCommand>(drone);
    auto land = std::make_shared<LandCommand>(drone);

    remote.press(takeoff);  // Drone taking off.
    remote.press(land);     // Drone landing.
    remote.pressUndo();     // Undo last → Drone taking off.
}
```

### Iterator
Bir koleksiyonun (örneğin liste, dizi, ağaç, vb.) iç yapısını bilmeden elemanlarına tek tek erişmeyi sağlayan bir behavioral (davranışsal) tasarım desenidir. Yani sen listeyi nasıl tuttuğunu (array mi, tree mi, map mi) bilmeden aynı şekilde next(), hasNext() diyerek üzerinde gezebilirsin.

```c++
#include <iostream>
#include <vector>
#include <memory>

// Iterator arayüzü
class Iterator {
public:
    virtual bool hasNext() = 0;
    virtual int next() = 0;
    virtual ~Iterator() = default;
};

// Koleksiyon arayüzü
class Collection {
public:
    virtual std::unique_ptr<Iterator> createIterator() = 0;
    virtual ~Collection() = default;
};

// Somut koleksiyon
class NumberCollection : public Collection {
    std::vector<int> numbers;
public:
    void add(int n) { numbers.push_back(n); }

    // İçeride özel bir iterator tanımlıyoruz
    class NumberIterator : public Iterator {
        const std::vector<int>& nums;
        size_t index = 0;
    public:
        NumberIterator(const std::vector<int>& n) : nums(n) {}
        bool hasNext() override { return index < nums.size(); }
        int next() override { return nums[index++]; }
    };

    std::unique_ptr<Iterator> createIterator() override {
        return std::make_unique<NumberIterator>(numbers);
    }
};

int main() {
    NumberCollection collection;
    collection.add(10);
    collection.add(20);
    collection.add(30);

    auto it = collection.createIterator();

    while (it->hasNext()) {
        std::cout << "Number: " << it->next() << "\n";
    }
}
```

### Mediator
Nesnelerin birbirleriyle doğrudan iletişim kurmak yerine,bu iletişimi merkezi bir aracı (mediator) üzerinden yapmasını sağlayan bir behavioral (davranışsal) tasarım desenidir.

```c++
#include <iostream>
#include <string>
#include <vector>
#include <memory>

// Arabulucu arayüzü
class Mediator {
public:
    virtual void notify(const std::string& sender, const std::string& event) = 0;
    virtual ~Mediator() = default;
};

// Uçak sınıfı (katılımcı)
class Airplane {
    std::string name;
    Mediator* tower;
public:
    Airplane(std::string n, Mediator* t) : name(std::move(n)), tower(t) {}
    void send(const std::string& msg) {
        std::cout << name << " sends: " << msg << "\n";
        tower->notify(name, msg);
    }
    void receive(const std::string& msg) {
        std::cout << "  " << name << " receives: " << msg << "\n";
    }
    std::string getName() const { return name; }
};

// Somut Mediator (Kule)
class ControlTower : public Mediator {
    std::vector<Airplane*> airplanes;
public:
    void registerPlane(Airplane* plane) { airplanes.push_back(plane); }

    void notify(const std::string& sender, const std::string& event) override {
        for (auto* plane : airplanes) {
            if (plane->getName() != sender) {
                plane->receive(sender + " says " + event);
            }
        }
    }
};

int main() {
    ControlTower tower;

    Airplane planeA("Drone-A", &tower);
    Airplane planeB("Drone-B", &tower);
    Airplane planeC("Drone-C", &tower);

    tower.registerPlane(&planeA);
    tower.registerPlane(&planeB);
    tower.registerPlane(&planeC);

    planeA.send("Requesting takeoff.");
    planeC.send("Acknowledged, holding position.");
}
```
### Memento
Bir nesnenin iç durumunu kaydedip, daha sonra aynı duruma geri dönmeyi sağlayan bir behavioral (davranışsal) tasarım desenidir.

```c++
// Memento: Nesnenin durumunu saklayan sınıf
class Memento {
    std::string state;
public:
    Memento(std::string s) : state(std::move(s)) {}
    std::string getState() const { return state; }
};

// Originator: Durumu değişen nesne
class TextEditor {
    std::string text;
public:
    void type(const std::string& newText) {
        text += newText;
    }

    std::shared_ptr<Memento> save() {
        return std::make_shared<Memento>(text);
    }

    void restore(std::shared_ptr<Memento> memento) {
        text = memento->getState();
    }

    void show() const {
        std::cout << "Current text: " << text << "\n";
    }
};

// Caretaker: Memento’ları yöneten (geçmiş kaydı tutan)
class History {
    std::vector<std::shared_ptr<Memento>> saves;
public:
    void add(std::shared_ptr<Memento> m) { saves.push_back(m); }
    std::shared_ptr<Memento> get(int index) { return saves[index]; }
};

int main() {
    TextEditor editor;
    History history;

    editor.type("Hello ");
    editor.show();

    history.add(editor.save());  // 1. durum kaydedildi

    editor.type("World!");
    editor.show();

    history.add(editor.save());  // 2. durum kaydedildi

    editor.type(" This will be undone.");
    editor.show();

    // Geri dön
    editor.restore(history.get(1));
    editor.show();
}
```

### Observer
Bir nesnede (subject) bir değişiklik olduğunda,bu değişikliği otomatik olarak bir veya daha fazla bağlı nesneye (observer) bildiren behavioral (davranışsal) bir tasarım desenidir.

```c++
class Observer {
public:
    virtual void update(const std::string& message) = 0;
    virtual ~Observer() = default;
};

// Subject (yayıncı)
class Subject {
    std::vector<std::shared_ptr<Observer>> observers;
public:
    void attach(std::shared_ptr<Observer> obs) {
        observers.push_back(obs);
    }

    void notify(const std::string& message) {
        for (auto& obs : observers)
            obs->update(message);
    }
};

// Somut observer'lar
class DroneDisplay : public Observer {
    std::string name;
public:
    DroneDisplay(std::string n) : name(std::move(n)) {}
    void update(const std::string& message) override {
        std::cout << name << " received update: " << message << "\n";
    }
};

int main() {
    Subject telemetrySystem;

    auto display1 = std::make_shared<DroneDisplay>("Ground Station");
    auto display2 = std::make_shared<DroneDisplay>("Control Tablet");
    auto display3 = std::make_shared<DroneDisplay>("Web Dashboard");

    telemetrySystem.attach(display1);
    telemetrySystem.attach(display2);
    telemetrySystem.attach(display3);

    telemetrySystem.notify("Altitude = 120m");
    telemetrySystem.notify("Battery = 85%");
}
```

### State
Bir nesnenin davranışının, içinde bulunduğu duruma (state) göre değişmesini sağlayan bir behavioral (davranışsal) tasarım desenidir.

```c++
class State {
public:
    virtual void handle() = 0;
    virtual ~State() = default;
};

// Somut durumlar
class IdleState : public State {
public:
    void handle() override {
        std::cout << "Drone is idle. Waiting for takeoff command.\n";
    }
};

class TakeoffState : public State {
public:
    void handle() override {
        std::cout << "Drone is taking off...\n";
    }
};

class FlyingState : public State {
public:
    void handle() override {
        std::cout << "Drone is flying.\n";
    }
};

class LandingState : public State {
public:
    void handle() override {
        std::cout << "Drone is landing.\n";
    }
};

// Context (durumu yöneten sınıf)
class Drone {
    std::shared_ptr<State> currentState;
public:
    Drone(std::shared_ptr<State> initState) : currentState(initState) {}
    void setState(std::shared_ptr<State> newState) {
        currentState = newState;
    }
    void execute() {
        currentState->handle();
    }
};

int main() {
    auto idle = std::make_shared<IdleState>();
    auto takeoff = std::make_shared<TakeoffState>();
    auto flying = std::make_shared<FlyingState>();
    auto landing = std::make_shared<LandingState>();

    Drone drone(idle);

    drone.execute();         // Idle
    drone.setState(takeoff); // Durum değişti
    drone.execute();         // Takeoff
    drone.setState(flying);
    drone.execute();         // Flying
    drone.setState(landing);
    drone.execute();         // Landing
}
```

### Strategy
Bir algoritma ailesini tanımlayıp, bu algoritmaları değiştirilebilir (interchangeable) hale getiren bir behavioral (davranışsal) tasarım desenidir.
```c++
class FlightStrategy {
public:
    virtual void navigate() = 0;
    virtual ~FlightStrategy() = default;
};

// Farklı stratejiler
class DirectFlight : public FlightStrategy {
public:
    void navigate() override {
        std::cout << "Flying directly to destination.\n";
    }
};

class SafeFlight : public FlightStrategy {
public:
    void navigate() override {
        std::cout << "Flying with obstacle avoidance enabled.\n";
    }
};

class EnergySavingFlight : public FlightStrategy {
public:
    void navigate() override {
        std::cout << "Flying with energy-saving path.\n";
    }
};

// Context (stratejiyi kullanan)
class DroneNavigator {
    std::shared_ptr<FlightStrategy> strategy;
public:
    void setStrategy(std::shared_ptr<FlightStrategy> s) {
        strategy = s;
    }

    void executeFlight() {
        if (strategy)
            strategy->navigate();
        else
            std::cout << "No strategy selected.\n";
    }
};

int main() {
    DroneNavigator navigator;

    auto direct = std::make_shared<DirectFlight>();
    auto safe = std::make_shared<SafeFlight>();
    auto eco = std::make_shared<EnergySavingFlight>();

    navigator.setStrategy(direct);
    navigator.executeFlight();  // direct route

    navigator.setStrategy(safe);
    navigator.executeFlight();  // obstacle avoidance

    navigator.setStrategy(eco);
    navigator.executeFlight();  // energy saving
}
```

### Template Method
Bir algoritmanın ana iskeletini tanımlar, ancak bazı adımların alt sınıflar tarafından özelleştirilmesine izin veren bir behavioral (davranışsal) tasarım desenidir.
```c++
class DroneMission {
public:
    void executeMission() {  // Template Method
        initialize();
        takeoff();
        performMission();
        land();
    }

protected:
    virtual void initialize() { std::cout << "Initializing drone systems...\n"; }
    virtual void takeoff() = 0;
    virtual void performMission() = 0;
    virtual void land() { std::cout << "Landing sequence complete.\n"; }
};

// Concrete sınıflar
class SurveyDroneMission : public DroneMission {
protected:
    void takeoff() override { std::cout << "Survey drone taking off smoothly.\n"; }
    void performMission() override { std::cout << "Performing area survey...\n"; }
};

class DeliveryDroneMission : public DroneMission {
protected:
    void takeoff() override { std::cout << "Delivery drone ascending vertically.\n"; }
    void performMission() override { std::cout << "Delivering package to destination...\n"; }
};

int main() {
    std::unique_ptr<DroneMission> mission1 = std::make_unique<SurveyDroneMission>();
    std::unique_ptr<DroneMission> mission2 = std::make_unique<DeliveryDroneMission>();

    std::cout << "--- Survey Mission ---\n";
    mission1->executeMission();

    std::cout << "\n--- Delivery Mission ---\n";
    mission2->executeMission();
}
```

### Visitor
Bir nesne yapısına yeni işlemler (operasyonlar) eklemeyi, nesnelerin sınıflarını değiştirmeden yapmamızı sağlayan bir behavioral (davranışsal) tasarım desenidir.

```c++
class SurveyDrone;
class DeliveryDrone;

// Visitor arayüzü
class DroneVisitor {
public:
    virtual void visit(SurveyDrone& drone) = 0;
    virtual void visit(DeliveryDrone& drone) = 0;
    virtual ~DroneVisitor() = default;
};

// Drone arayüzü
class Drone {
public:
    virtual void accept(DroneVisitor& visitor) = 0;
    virtual ~Drone() = default;
};

// Concrete drone sınıfları
class SurveyDrone : public Drone {
public:
    void accept(DroneVisitor& visitor) override {
        visitor.visit(*this);
    }
    void scanArea() { std::cout << "SurveyDrone scanning area...\n"; }
};

class DeliveryDrone : public Drone {
public:
    void accept(DroneVisitor& visitor) override {
        visitor.visit(*this);
    }
    void deliverPackage() { std::cout << "DeliveryDrone delivering package...\n"; }
};

// Somut Visitor: Bakım mühendisi
class MaintenanceVisitor : public DroneVisitor {
public:
    void visit(SurveyDrone& drone) override {
        std::cout << "Checking sensors of survey drone.\n";
        drone.scanArea();
    }
    void visit(DeliveryDrone& drone) override {
        std::cout << "Inspecting motors of delivery drone.\n";
        drone.deliverPackage();
    }
};

int main() {
    SurveyDrone s;
    DeliveryDrone d;
    MaintenanceVisitor engineer;

    std::vector<Drone*> fleet = { &s, &d };

    for (auto* drone : fleet) {
        drone->accept(engineer);
    }
}
```