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