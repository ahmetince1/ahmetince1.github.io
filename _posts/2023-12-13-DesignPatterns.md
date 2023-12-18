---
layout: post
title: Design Patterns 
subtitle: Design Patterns ( Tasarım Kalıpları )
cover-img: /assets/img/designPatternsCover.png
thumbnail-img: /assets/img/designPatterns.png
share-img: /assets/img/designPatternsCover.png
author: Ahmet Zeki İnce
---

## Design Patterns

***Nedir ?***

<p>Tasarım kalıpları, genellikle yazılım tasarımı bağlamında karşılaşılan yaygın sorunlara yönelik tekrar kullanılabilir çözümlerdir. Bu kalıplar, farklı bağlamlarda ortaya çıkan benzer sorunlara uygulanan en iyi uygulamalar olarak bilinir. Gang of Four olarak bilinen yazarların “Elements of Reusable Object-Oriented Software”kitabında 1994 yılında resmi olarak tanıtıldıktan sonra tasarım kalıpları büyük popülerlik kazandı.</p>
<p>İlk olarak C++ ve Smalltalk kod örnekleriyle yayınlanan bu kalıplar, özellikle Java ve C# gibi dillerde oldukça popülerdir ve genellikle tüm nesne yönelimli dillerde uygulanabilir. Ancak, bazı fonksiyonel dillerde, örneğin Scala gibi, bazı kalıplar artık gerekli değildir.</p>
Bu kalıplar 3 ana başlık altında toplanmıştır. Bunlar:

-   Creational Design Patterns ( Yaratımsal Tasarım Kalıpları )
-   Behavioral Design Patterns ( Davranışsal Tasarım Kalıpları )
-   Structural Design Patterns ( Yapısal Tasarım Kalıpları )
<p></p>

##### ***Creational Design Patterns***

<p>Yaratımsal tasarım kalıpları, yazılım nesnelerini esnek bir yapı kullanarak genel öneriler sunar ve önceden belirlenmiş durumlara bağlı olarak gerekli nesneleri yaratır. Bu tasarım kalıpları, sistem içinde hangi nesnenin çağrılması gerektiğini izlemeden, sistemin uygun nesneyi çağırmasını sağlar. Nesnelerin yaratılmasıyla ilgili esnek bir yaklaşım sunarak, uygulamaya belirli bir durumda nasıl davranılacağı konusunda farkedilebilir bir esneklik kazandırır. Temel amaç, yazılımın içindeki nesnelerin nasıl yaratıldığından bağımsız olarak iyi bir tasarım sağlamaktır.</p>
Yaratımsal tasarım kalıpları şunlardır:

***Singleton ( Yegane )***
<p>Nesnenin sadece bir defa oluşturulmasını öngören bir mekanizma kurulmak istenildiğinde etkin bir biçimde kullanılabilen bir tasarım kalıbıdır. Oluşturulan sınıftan sadece bir nesne yaratılacak şekilde bir kısıtlama yapabilme olanağı sağlar ve nesneye ilk kez ihtiyaç duyulana kadar yaratılmayabilir. Örneğin:</p>

```dart
class Singleton {
  static Singleton? _instance;
  // Constructor özel olarak tanımlanır ve dışarıdan erişime kapatılır.
  Singleton._();
  // Singleton örneğine erişimi sağlayan metod.
  static Singleton getInstance() {
    _instance ??= Singleton._(); // null ise yeni bir örnek oluştur
    return _instance!;
  }
  static const isim='Ahmet';
  // Singleton sınıfının işlevselliği buraya eklenir.
  void printMessage() {
    print("Singleton instance is created.");
  }
}

void main() {
  // Singleton sınıfından bir örnek elde edilir.
  Singleton singleton1 = Singleton.getInstance();
  singleton1.printMessage();
  Singleton singleton2 = Singleton.getInstance();
  singleton2.printMessage();
  // 2 nesne yaratılmış gibi görünse de tek nesne yaratılmıştır.
}
```

***Factory ( Fabrika )***
<p>Fabrika Metodu, yaratımsal tasarım kalıplarından biridir ve nesne yaratım sürecini alt sınıflara bırakarak, nesne oluşturma işlevselliğini genel bir arayüzle birbirinden ayırmayı sağlayan bir tasarım desenidir. Bu desen, bir arayüz veya soyut bir sınıf içinde tanımlanan bir yöntemi, alt sınıflara bırakarak, alt sınıfların bu yöntemi uygulayarak kendi özel nesnelerini oluşturmalarına izin verir. Oluşturulan fabrika yapısı, belirli bir bağlamda doğru sınıfın seçilmesini ve gereken nesnenin yaratılmasını sağlar. Bu sayede, benzer sınıfların yönetimini kolaylaştırır ve esnek bir nesne yaratım süreci sunar. Örneğin:</p>

```dart
// Product (Ürün)
abstract class Shape {
  void draw();
}

// ConcreteProduct (Somut Ürün)
class Circle implements Shape {
  @override
  void draw() {
    print("Circle is drawn.");
  }
}

// ConcreteProduct (Somut Ürün)
class Rectangle implements Shape {
  @override
  void draw() {
    print("Rectangle is drawn.");
  }
}

// Creator (Yaratıcı)
abstract class ShapeFactory {
  Shape createShape();
}

// ConcreteCreator (Somut Yaratıcı)
class CircleFactory implements ShapeFactory {
  @override
  Shape createShape() {
    return Circle();
  }
}

// ConcreteCreator (Somut Yaratıcı)
class RectangleFactory implements ShapeFactory {
  @override
  Shape createShape() {
    return Rectangle();
  }
}

void main() {
  // Kullanıcı (client) sınıf
  ShapeFactory circleFactory = CircleFactory();
  Shape circle = circleFactory.createShape();
  circle.draw(); // Çıktı: Circle is drawn.
  ShapeFactory rectangleFactory = RectangleFactory();
  Shape rectangle = rectangleFactory.createShape();
  rectangle.draw(); // Çıktı: Rectangle is drawn.
}
```

***Abstract Factory ( Soyut Fabrika )***
<p>Factory design pattern’de tek bir ürün ailesine ait tek bir arayüz mevcutken, abstract factory’de farklı ürün aileleri için farklı arayüzler mevcuttur. Fabrika olarak düşünürsek, Factory DP sadece tek bir ürünün üretildiği fabrika, Abstract Factory DP ise farklı farklı ürünlerin üretildiği fabrika olarak düşünebiliriz. Birden fazla ürün ailesi ile çalışmak durumunda kaldığımızda , ürün ailesi ile istemci tarafını soyutlamak için abstract factory kullanılır. Örneğin:</p>

```dart
// interfaceler
abstract class IPlayer{
  String GetTopScorer();
}
abstract class ITeam{
  String GetTeamColor();
}
abstract class IFootballFactory{
  ITeam CreateTeam();
  IPlayer CreatePlayer();
}
// IFootballFactory interface'ini somutlayan classlar
class BundesligaFactory implements IFootballFactory{
  @override
  IPlayer CreatePlayer() {
    return BundesligaPlayer();
  }
  @override
  ITeam CreateTeam() {
    return BorussiaDortmund();
  }
}
class LaLigaFactory implements IFootballFactory{
  @override
  IPlayer CreatePlayer() {
    return LaLigaPlayer();
  }
  @override
  ITeam CreateTeam() {
    return RealMadrid();
  }
}
class SerieAFactory implements IFootballFactory{
  @override
  IPlayer CreatePlayer() {
    return SerieAPlayer();
  }
  @override
  ITeam CreateTeam() {
    return Juventus();
  }
}
//ITeam interface'ini somutlayan classlar
class BorussiaDortmund implements ITeam{
  @override
  String GetTeamColor() {
    return 'Black and Yellow';
  }
}
class Juventus implements ITeam{
  @override
  String GetTeamColor() {
    return 'Black and White';
  }
}
class RealMadrid implements ITeam{
  @override
  String GetTeamColor() {
    return 'Blue and White';
  }
}
// IPlayer interface'ini somutlaştıran classlar
class BundesligaPlayer implements IPlayer{
  @override
  String GetTopScorer() {
    return 'Robert Lewandowski';
  }
}
class LaLigaPlayer implements IPlayer{
  @override
  String GetTopScorer() {
    return 'Lionel Messi';
  }
}
class SerieAPlayer implements IPlayer{
  @override
  String GetTopScorer() {
    return 'Cristiano Ronaldo';
  }
}
//client class
class FootballWorld{
  late ITeam _team;
  late IPlayer _player;
  FootballWorld(IFootballFactory factory){
    _team = factory.CreateTeam();
    _player = factory.CreatePlayer();
  }
  String getFootballTeamColor(){
    return _team.GetTeamColor();
  }
  String getTopScorer(){
    return _player.GetTopScorer();
  }
}

void main() {
  // nesnelerin yaratılışı
  IFootballFactory germany = BundesligaFactory();
  IFootballFactory spain = LaLigaFactory();
  IFootballFactory italy = SerieAFactory();
  FootballWorld footballWorld =FootballWorld(spain);
  print(footballWorld.getFootballTeamColor());
  print(footballWorld.getTopScorer());
}
```

***Builder ( Yapıcı )***
<p>Tek ara yüz kullanarak karmaşık bir nesne grubundan gerektiğince parça yaratılmasını sağlar. Nesne grubu kullanıldıkça istenilen şekilde yapılanır ve bu sayede kullanılmayan parçaların gereksiz yere yaratılarak kaynak harcama durumu ortadan kaldırılmış olur. Örneğin:</p>

```dart
// Pizza sınıfı, builder tasarım deseni kullanılarak oluşturulacak nesneyi temsil eder.
class Pizza{
  int? _diameter;
  String? _name;
  String? _cheese;
  String? _crust;
  Set<String>? _topping;

// Pizza sınıfının constructor'ı, PizzaBuilder'dan gelen bilgilerle nesneyi oluşturur.
Pizza(PizzaBuilder builder){
  _diameter = builder._diameter;
  _name = builder._name;
  _cheese = builder._cheese;
  _crust = builder._crust;
  _topping = builder._topping;
}

@override
String toString() => 'Pizza $_diameter $_name $_cheese $_crust $_topping';

}
// PizzaBuilder sınıfı, Pizza sınıfının nesnesini adım adım oluşturmak için kullanılır.
class PizzaBuilder{
  int? _diameter;
  String? _name;
  String? _cheese;
  String? _crust;
  Set<String>? _topping;
// PizzaBuilder'ın constructor'ı, gerekli olan minimum bilgileri alır ve nesnenin temelini oluşturur.
  PizzaBuilder(this._diameter,this._name);
// Cheese özelliğine getter ve setter eklenmiş.
  String? get cheese => _cheese;
  void set cheese(String? value){
    _cheese = value;
  }
  // Crust özelliğine getter ve setter eklenmiş.
  String? get crust => _crust;
  void set crust(String? value){
    _crust = value;
  }
  // Topping özelliğine getter ve setter eklenmiş.
  Set<String>? get topping => _topping;
  void set topping (Set<String>? value){
    _topping = value;
  }
// PizzaBuilder'ın build metodu, nesneyi oluşturur ve geri döndürür.
  Pizza build(){
    return Pizza(this);
  }
}
```

***Prototype ( Örnek )***
<p>Kendi üzerinden yaratılacak nesneler için prototip görevi üstlenen bir yapı sunmaktadır. Diğer bir deyişle, sınıflardan nesne yaratırken yeni nesnelerin baştan yaratılmayıp, mevcutlarını örnek kabul ederek yaratılmasını sağlar. Bu desen sayesinde nesneler, kaynaklar gereksiz yere meşgul edilmeden yaratılırlar. Örneğin:</p>

```dart
class Person {
  String name;
  String lastName;
  int age;

  // Person sınıfının constructor'ı, zorunlu parametrelerle bir nesne oluşturur.
  Person({required this.name, required this.lastName, required this.age});

  // Clone metodu, mevcut nesnenin bir kopyasını oluşturur ve geri döndürür.
  Person clone() => Person(name: name, lastName: lastName, age: age);
}

void main() {
  // İlk olarak bir Person nesnesi oluşturuluyor.
  Person person = Person(name: 'Murat', lastName: 'Kiliç', age: 34);

  // Clone metodu kullanılarak mevcut nesnenin bir kopyası oluşturuluyor.
  Person person1 = person.clone();

  // Orijinal ve klonlanmış nesnelerin isimleri ekrana yazdırılıyor.
  print(person.name);
  print(person1.name);
}
```

<p></p>

##### ***Behavioral Design Patterns***
<p>Belirli görevlerin bu işleri yapmak üzere tasarlanmış nesnelere gönderilmesi ve geri dönen sonuçların işlenmesi davranışsal tasarım kalıpları içerisinde yapılan bir işlemdir. Nesnelerin çalışma zamanında sergiledikleri davranışları değiştirmek için kullanılan tasarım kalıplarından oluşur. Nesneler arasındaki iletişime ve birlikte çalışma biçimlerine odaklanan tasarım kalıplarıdır. Nesneler arasındaki ilişkileri daha dinamik hale getirerek yazılım sistemlerinin esnekliğini ve yeniden kullanımını geliştirmeyi amaçlarlar.</p>
Davranışsal tasarım kalıpları içerisinde bilinen ve sıklıkla kullanılan kalıplar şu şekildedir:

***Command Pattern ( Komut Kalıbı )***
<p>Bir nesne üzerindeki işlemlerin yapılmasındaki süreci bilmediğimizde ya da kullanılmak istenen nesnenin tanınmadığı durumlarda, komut tasarım kalıbı ile yapılmak istenen işlem bir nesneye dönüştürülerek, alıcı nesne tarafından işlemin yerine getirilmesi amaçlanmaktadır. Örneğin:</p>

```dart
// Receiver: İsteği gerçekleştiren nesne
class Light {
  void turnOn() {
    print("Light is ON");
  }
  void turnOff() {
    print("Light is OFF");
  }
}

// Command arayüzü: İsteği temsil eden arayüz
abstract class Command {
  void execute();
}

// ConcreteCommand sınıfları: Belirli bir isteği temsil eden sınıflar
class TurnOnCommand implements Command {
  final Light light;
  TurnOnCommand(this.light);
  @override
  void execute() {
    light.turnOn();
  }
}

class TurnOffCommand implements Command {
  final Light light;
  TurnOffCommand(this.light);
  @override
  void execute() {
    light.turnOff();
  }
}

// Invoker: Komutları çağıran nesne
class RemoteControl {
  Command ?_command;
  void setCommand(Command command) {
    _command = command;
  }
  void pressButton() {
    _command!.execute();
  }
}

void main() {
  // Client kodu
  final Light light = Light();
  final TurnOnCommand turnOnCommand = TurnOnCommand(light);
  final TurnOffCommand turnOffCommand = TurnOffCommand(light);
  final RemoteControl remote = RemoteControl();
  remote.setCommand(turnOnCommand);
  remote.pressButton(); // Light is ON
  remote.setCommand(turnOffCommand);
  remote.pressButton(); // Light is OFF
}
```

***Memento Pattern ( Hatıra Kalıbı )***
<p>Bir nesnenin, daha önce sahip olduğu durumlardan birine tekrar dönüştürülebilmesi için hatıra tasarım kalıbı kullanılır. Bu sayede belirli durumlardan sonra nesnelerin aldıkları çeşitli durumların kolaylıkla değiştirilebilmesi ve bu durumların farklı sınıflarda tanımlanarak kullanılabilirliğinin arttırılması amaçlanmaktadır. Örneğin:</p>

```dart
// Originator sınıfı: Durumu değiştiren ve kaydeden sınıf
class Originator {
  late String _state;
  void setState(String state) {
    print("Originator: Setting state to $state");
    _state = state;
  }
  String getState() {
    return _state;
  }
  // Memento oluştur
  Memento createMemento() {
    print("Originator: Creating Memento");
    return Memento(_state);
  }
  // Memento'yu geri yükle
  void restoreMemento(Memento memento) {
    print("Originator: Restoring state from Memento");
    _state = memento.getState();
  }
}

// Memento sınıfı: Originator'un durumunu içeren sınıf
class Memento {
  String _state;
  Memento(this._state);
  String getState() {
    return _state;
  }
}

// Caretaker sınıfı: Memento'ları saklayan ve geri yükleyen sınıf
class Caretaker {
  late Memento _memento;
  Memento getMemento() {
    return _memento;
  }
  void setMemento(Memento memento) {
    print("Caretaker: Saving Memento");
    _memento = memento;
  }
}

void main() {
  // Originator ve Caretaker oluştur
  Originator originator = Originator();
  Caretaker caretaker = Caretaker();
  // Originator durumunu değiştir
  originator.setState("State 1");
  // Memento oluştur ve sakla
  caretaker.setMemento(originator.createMemento());
  // Originator durumunu değiştir
  originator.setState("State 2");
  // Durumu geri yükle
  originator.restoreMemento(caretaker.getMemento());
  // Son durumu yazdır
  print("Current State: ${originator.getState()}");
}
```

***Strategy Pattern ( Strateji Kalıbı )***
<p>Bir işlemin yerine getirilmesi için birden fazla metot kullanılıyor olabilir. İhtiyaçlara göre bir metodun seçilip, uygulaması için strateji tasarım kalıbı kullanılmaktadır. Her metot bir sınıf içinde tanımlanabilir. Örneğin:
</p>

```dart
// Strateji arayüzü
abstract class OdemeStratejisi {
  void odemeYap(int tutar);
}

// Somut stratejiler
class KrediKartiOdeme implements OdemeStratejisi {
  late String kartNumarasi;
  KrediKartiOdeme(this.kartNumarasi);
  @override
  void odemeYap(int tutar) {
    print('$tutar TL tutarında ödeme, kredi kartı ile yapıldı: $kartNumarasi');
  }
}

class PayPalOdeme implements OdemeStratejisi {
  late String email;
  PayPalOdeme(this.email);
  @override
  void odemeYap(int tutar) {
    print('$tutar TL tutarında ödeme, PayPal hesabı ile yapıldı: $email');
  }
}

// Bağlam sınıfı
class AlisverisSepeti {
  late OdemeStratejisi _odemeStratejisi;
  void odemeStratejisiBelirle(OdemeStratejisi odemeStratejisi) {
    _odemeStratejisi = odemeStratejisi;
  }
  void odemeYap(int tutar) {
    if (_odemeStratejisi == null) {
      print('Lütfen ödeme stratejisini belirleyin.');
    } else {
      _odemeStratejisi.odemeYap(tutar);
    }
  }
}

void main() {
  // İstemci kodu
  AlisverisSepeti sepet = AlisverisSepeti();
  // Ödeme stratejisi seç ve belirle
  OdemeStratejisi krediKartiOdeme = KrediKartiOdeme('1234-5678-9012-3456');
  sepet.odemeStratejisiBelirle(krediKartiOdeme);
  // Ödeme yap
  sepet.odemeYap(100);
  // Ödeme stratejisini değiştir
  OdemeStratejisi paypalOdeme = PayPalOdeme('john.doe@example.com');
  sepet.odemeStratejisiBelirle(paypalOdeme);
  // Yeni strateji ile ödeme yap
  sepet.odemeYap(50);
}
```

***Iterator Pattern ( Tekrarlayıcı Kalıbı )***
<p>Tekrarlayıcı tasarım kalıbı ile bir listede yer almakta olan nesnelerin sırasıyla, listenin yapısını ve çalışma tarzının uygulamanın diğer kısımları ile olan bağlantılarını en aza indirmek için uygulamadan soyutlama amaçlı kullanılabilmektedir. Örneğin:</p>

```dart
// Iterable sınıfını uygulayan bir sınıf
class MyIterable {
  List<String> _data;
  MyIterable(this._data);
  // Iterator döndüren bir metot
  MyIterator get iterator => MyIterator(_data);
}

// Iterator sınıfı
class MyIterator {
  List<String> _data;
  int _index;
  MyIterator(this._data) : _index = 0;
  // Bir sonraki elemana geçen ve durumu kontrol eden metot
  bool moveNext() {
    if (_index < _data.length) {
      _index++;
      return true;
    } else {
      return false;
    }
  }
  // Şu anki elemanı döndüren getter metot
  String get current => _data[_index - 1];
}

void main() {
  // MyIterable sınıfını kullanarak bir örnek oluşturuluyor
  MyIterable myIterable = MyIterable(["Merhaba", "Dart", "Iterator", "Pattern"]);
  // Iterator alınıyor
  MyIterator iterator = myIterable.iterator;
  // Iterator kullanılarak liste üzerinde dolaşma
  while (iterator.moveNext()) {
    print(iterator.current);
  }
}
```

***State Pattern ( Durum Kalıbı )***
<p>State, bir nesnenin iç durumu değiştiğinde davranışını değiştirmesini sağlayan bir davranışsal tasarım modelidir. Nesne sınıfını değiştirmiş gibi görünür. Örneğin:</p>

```dart
// State interface
abstract class LampState {
  void turnOn();
  void turnOff();
}

// Concrete State - Off State
class OffState implements LampState {
  @override
  void turnOn() {
    print("Turning the lamp on.");
  }
  @override
  void turnOff() {
    print("The lamp is already off.");
  }
}

// Concrete State - On State
class OnState implements LampState {
  @override
  void turnOn() {
    print("The lamp is already on.");
  }
  @override
  void turnOff() {
    print("Turning the lamp off.");
  }
}

// Context - Lamp
class Lamp {
  late LampState _currentState;
  Lamp() {
    // Başlangıç durumu olarak Off State'i belirle
    _currentState = OffState();
  }
  void setState(LampState state) {
    _currentState = state;
  }
  void turnOn() {
    _currentState.turnOn();
    setState(OnState()); // Durumu On State olarak güncelle
  }
  void turnOff() {
    _currentState.turnOff();
    setState(OffState()); // Durumu Off State olarak güncelle
  }
}

void main() {
  Lamp lamp = Lamp();
  // Lambayı aç
  lamp.turnOn();
  // Lambayı kapat (tekrar açmaya çalışabiliriz, ancak durum kontrol edilir)
  lamp.turnOff();
  // Lambayı tekrar aç
  lamp.turnOn();
  lamp.turnOn();
}
```

***Chain of Responsibility Pattern ( Sorumluluk Zinciri Kalıbı )***
<p>Sorumluluk Zinciri, istekleri bir işleyici zinciri boyunca aktarmanıza olanak tanıyan davranışsal bir tasarım modelidir. İstek alan her işleyici, isteği işleme koymaya ya da zincirdeki bir sonraki işleyiciye aktarmaya karar verir. Örneğin:</p>

```dart
// İstek sınıfı
class Request {
  String message;
  Request(this.message);
}

// İşleyici arayüzü
abstract class Handler {
  Handler? _nextHandler;
  void setNextHandler(Handler nextHandler) {
    _nextHandler = nextHandler;
  }
  void handleRequest(Request request);
}

// İşleyici uygulaması
class ConcreteHandler1 extends Handler {
  @override
  void handleRequest(Request request) {
    if (request.message.contains('Handler1')) {
      print('ConcreteHandler1 işledi: ${request.message}');
    } else if (_nextHandler != null) {
      _nextHandler!.handleRequest(request);
    } else {
      print('İşleyici bulunamadı: ${request.message}');
    }
  }
}

class ConcreteHandler2 extends Handler {
  @override
  void handleRequest(Request request) {
    if (request.message.contains('Handler2')) {
      print('ConcreteHandler2 işledi: ${request.message}');
    } else if (_nextHandler != null) {
      _nextHandler!.handleRequest(request);
    } else {
      print('İşleyici bulunamadı: ${request.message}');
    }
  }
}

class ConcreteHandler3 extends Handler {
  @override
  void handleRequest(Request request) {
    if (request.message.contains('Handler3')) {
      print('ConcreteHandler3 işledi: ${request.message}');
    } else if (_nextHandler != null) {
      _nextHandler!.handleRequest(request);
    } else {
      print('İşleyici bulunamadı: ${request.message}');
    }
  }
}

void main() {
  // İşleyicileri oluştur
  Handler handler1 = ConcreteHandler1();
  Handler handler2 = ConcreteHandler2();
  Handler handler3 = ConcreteHandler3();
  // İşleyici zincirini kur
  handler1.setNextHandler(handler2);
  handler2.setNextHandler(handler3);
  // İstekleri işle
  Request request1 = Request('Handler1 için istek');
  Request request2 = Request('Handler2 için istek');
  Request request3 = Request('Handler3 için istek');
  handler1.handleRequest(request1);
  handler1.handleRequest(request2);
  handler1.handleRequest(request3);
}
```

***Mediator Pattern ( Aracı Kalıbı )***
<p>Aracı tasarım kalıbı nesnelerin yönetimi ve aralarındaki iletişimin merkezi bir noktadan sağlanması ve yönetilmesi için kullanılmaktadır. Bu nesneler arasındaki bağı azaltmakta ve sadece bir sınıfı, yönetici sınıf olarak diğer sınıfların koordine edilmesinden sorumlu kılar. Örneğin:</p>

```dart
// Mediator arayüzü
abstract class ChatMediator {
  void sendMessage(String message, Participant participant);
}

// Katılımcı sınıfı
class Participant {
  String name;
  ChatMediator mediator;
  Participant(this.name, this.mediator);
  void sendMessage(String message) {
    print('$name: Mesaj gönderiliyor - $message');
    mediator.sendMessage(message, this);
  }
  void receiveMessage(String message) {
    print('$name: Mesaj alındı - $message');
  }
}

// Grup sohbeti sağlayan Mediator sınıfı
class GroupChatMediator implements ChatMediator {
  List<Participant> participants = [];
  void addParticipant(Participant participant) {
    participants.add(participant);
  }
  @override
  void sendMessage(String message, Participant sender) {
    for (var participant in participants) {
      if (participant != sender) {
        participant.receiveMessage(message);
      }
    }
  }
}

void main() {
  // Mediator oluştur
  var chatMediator = GroupChatMediator();
  // Katılımcıları oluştur
  var participant1 = Participant('Ali', chatMediator);
  var participant2 = Participant('Ayşe', chatMediator);
  var participant3 = Participant('Mehmet', chatMediator);
  // Katılımcıları gruba ekle
  chatMediator.addParticipant(participant1);
  chatMediator.addParticipant(participant2);
  chatMediator.addParticipant(participant3);
  // Mesaj gönderme
  participant1.sendMessage('Selam herkese!');
  participant2.sendMessage('Merhaba, nasılsınız?');
}
```

***Observer Pattern ( Gözlemci Kalıbı )***
<p>Uygulama içerisinde bir nesnede meydana gelen değişikliklerden haberdar olup üzerinde belli metotları çalıştırıp değişlikler yapmak isteyen diğer nesneler bulunabilmektedir. Bu durumda haberdar olmak isteyen nesneler diğer nesne ile ilişkilendirilerek, ilişkili oldukları nesnede meydana gelen değişikliklerden haberdar edilebilmektedirler. İlişki içerisinde olan nesne ile bağlantı iptal edilerek, ilişkili olduğu nesne ile arasındaki bağ sonlandırılabilir. Örneğin:</p>

```dart
// Observer arayüzü
abstract class Observer {
  void update(String news);
}

// Observable (Gözlemlenebilir) sınıfı
class NewsAgency {
  List<Observer> _observers = [];
  String _latestNews = '';
  void addObserver(Observer observer) {
    _observers.add(observer);
  }
  void removeObserver(Observer observer) {
    _observers.remove(observer);
  }
  void setNews(String news) {
    _latestNews = news;
    notifyObservers();
  }
  void notifyObservers() {
    for (var observer in _observers) {
      observer.update(_latestNews);
    }
  }
}

// Concrete Observer sınıfı
class NewsChannel implements Observer {
  String channelName;
  NewsChannel(this.channelName);
  @override
  void update(String news) {
    print('$channelName: Yeni haber - $news');
  }
}

void main() {
  // Observable nesne oluştur
  NewsAgency newsAgency = NewsAgency();
  // Observer'ları oluştur
  Observer channel1 = NewsChannel('Kanal 1');
  Observer channel2 = NewsChannel('Kanal 2');
  Observer channel3 = NewsChannel('Kanal 3');
  // Observer'ları Observable nesneye ekle
  newsAgency.addObserver(channel1);
  newsAgency.addObserver(channel2);
  newsAgency.addObserver(channel3);
  // Haber güncellemesi ve bildirim
  newsAgency.setNews('Dünya genelinde yeni bir keşif yapıldı!');
}
```

***Template Pattern ( Şablon Kalıbı )***
<p>Template tasarım deseni, bir algoritmanın genel yapısını belirleyen bir şablon sağlar, ancak bu algoritmanın belirli adımları, alt sınıflar tarafından özelleştirilecek şekilde bırakır. Bu sayede, temel algoritma aynı kalırken, alt sınıflar kendilerine özgü implementasyonları ekleyebilir ve böylece genel algoritmanın davranışını değiştirebilir. Örneğin:</p>

```dart
// Template sınıfı
abstract class OrderTemplate {
  void selectProducts();
  void makePayment();
  void ship();
  // Template method
  void processOrder() {
    selectProducts();
    makePayment();
    ship();
  }
}

// Concrete Template sınıfı
class OnlineOrder extends OrderTemplate {
  @override
  void selectProducts() {
    print("Online: Ürünleri seç");
  }
  @override
  void makePayment() {
    print("Online: Ödemeyi yap");
  }
  @override
  void ship() {
    print("Online: Ürünleri gönder");
  }
}

// Concrete Template sınıfı
class InStoreOrder extends OrderTemplate {
  @override
  void selectProducts() {
    print("Mağazada: Ürünleri seç");
  }
  @override
  void makePayment() {
    print("Mağazada: Ödemeyi yap");
  }
  @override
  void ship() {
    print("Mağazada: Ürünleri teslim al");
  }
}

void main() {
  // Online sipariş oluştur ve işle
  OrderTemplate onlineOrder = OnlineOrder();
  print("Online Sipariş:");
  onlineOrder.processOrder();
  print("\n---------------------\n");
  // Mağazada sipariş oluştur ve işle
  OrderTemplate inStoreOrder = InStoreOrder();
  print("Mağazada Sipariş:");
  inStoreOrder.processOrder();
}
```

***Visitor Pattern ( Ziyaretçi Kalıbı )***
<p>Hiyerarşik yapıda ve farklı tipte olan nesnelerin mevcut yapılarını değiştirmeden işlem yapabilmek amacıyla kullanılır. İşlemi ziyaretçi (visitor) nesneler yapar. Ziyaretçi nesneler sayesinde yeni metotlar tanımlanabilir ve ilgili nesneler bu ziyaretçi nesneye parametre olarak aktarılıp işlem yapılır. Örneğin:</p>

```dart
// Element arayüzü
abstract class Expression {
  void accept(ExpressionVisitor visitor);
}

// Concrete Element sınıfı
class NumberExpression implements Expression {
  int value;
  NumberExpression(this.value);
  @override
  void accept(ExpressionVisitor visitor) {
    visitor.visitNumber(this);
  }
}

// Concrete Element sınıfı
class AdditionExpression implements Expression {
  Expression left;
  Expression right;
  AdditionExpression(this.left, this.right);
  @override
  void accept(ExpressionVisitor visitor) {
    visitor.visitAddition(this);
  }
}

// Concrete Element sınıfı
class MultiplicationExpression implements Expression {
  Expression left;
  Expression right;
  MultiplicationExpression(this.left, this.right);
  @override
  void accept(ExpressionVisitor visitor) {
    visitor.visitMultiplication(this);
  }
}

// Visitor arayüzü
abstract class ExpressionVisitor {
  get result => null;
  void visitNumber(NumberExpression number);
  void visitAddition(AdditionExpression addition);
  void visitMultiplication(MultiplicationExpression multiplication);
}

// Concrete Visitor sınıfı
class EvaluateVisitor implements ExpressionVisitor {
  int result = 0;
  @override
  void visitNumber(NumberExpression number) {
    result = number.value;
  }
  @override
  void visitAddition(AdditionExpression addition) {
    addition.left.accept(this);
    int leftResult = result;
    addition.right.accept(this);
    int rightResult = result;
    result = leftResult + rightResult;
  }
  @override
  void visitMultiplication(MultiplicationExpression multiplication) {
    multiplication.left.accept(this);
    int leftResult = result;
    multiplication.right.accept(this);
    int rightResult = result;
    result = leftResult * rightResult;
  }
}

void main() {
  // Ağaç oluştur
  Expression expression = AdditionExpression(
    NumberExpression(5),
    MultiplicationExpression(
      NumberExpression(3),
      NumberExpression(2),
    ),
  );
  // EvaluateVisitor kullanarak ağacı gezmek
  ExpressionVisitor evaluateVisitor = EvaluateVisitor();
  expression.accept(evaluateVisitor);

  print('Sonuç: ${evaluateVisitor.result}');
}
```

***Interpreter Pattern ( Yorumlayıcı Kalıbı )***
<p>Sıklıkla karşılaşılan belli mantıksal kalıpların bir bütün içerisinde yer almasını sağlamak amacıyla kullanılmaktadır. Dil bilgisi kuralları gibi kalıplar içerisinde yer alan ifadelerin yorumlanması amacıyla kullanılması tercih edilmektedir. Örneğin:</p>

```dart
// Abstract Expression sınıfı
abstract class Expression {
  int interpret();
}

// Terminal Expression sınıfı
class NumberExpression implements Expression {
  late int _number;
  NumberExpression(int number) {
    _number = number;
  }
  @override
  int interpret() {
    return _number;
  }
}

// Nonterminal Expression sınıfı
class AdditionExpression implements Expression {
  late Expression _left;
  late Expression _right;
  AdditionExpression(Expression left, Expression right) {
    _left = left;
    _right = right;
  }
  @override
  int interpret() {
    return _left.interpret() + _right.interpret();
  }
}

// Nonterminal Expression sınıfı
class SubtractionExpression implements Expression {
  late Expression _left;
  late Expression _right;
  SubtractionExpression(Expression left, Expression right) {
    _left = left;
    _right = right;
  }
  @override
  int interpret() {
    return _left.interpret() - _right.interpret();
  }
}

// Context sınıfı
class Context {
  late String _input;
  List<Expression> _expressions = [];
  Context(String input) {
    _input = input;
    List<String> tokens = input.split(' ');
    for (int i = 0; i < tokens.length; i++) {
      String token = tokens[i];
      if (token == '+') {
        Expression left = _expressions.removeLast();
        Expression right = NumberExpression(int.parse(tokens[++i]));
        _expressions.add(AdditionExpression(left, right));
      } else if (token == '-') {
        Expression left = _expressions.removeLast();
        Expression right = NumberExpression(int.parse(tokens[++i]));
        _expressions.add(SubtractionExpression(left, right));
      } else {
        _expressions.add(NumberExpression(int.parse(token)));
      }
    }
  }
  int interpret() {
    for (Expression expression in _expressions) {
      expression.interpret();
    }
    return _expressions.last.interpret();
  }
}

void main() {
  // İfade oluştur
  var context = Context('3 + 5 - 2');
  // İfadeyi yorumla (interpret)
  var result = context.interpret();
  print('Sonuç: $result'); // Sonuç: 6
}
```