Fragenkatalog Kurs 01853 - Moderne Programmiertechniken und -Methoden
=====================================================================

- Welche Art des Gebrauchs von Interfaces kommt bei Black-Box-Frameworks vor?
  >Ermöglichendes Interface (Server-Client-Interface, Server-Item-Interface)
- Welche Art des Gebrauchs von Interfaces kommt bei der Interface Injection vor?
  >Server-Client-Interface, Server-Item-Interface
- Welche Art des Gebrauchs von Interfaces kommt im Zusammenhang mit Factories vor?
  >Familieninterfaces
- Welche Art des Gebrauchs von Interfaces sollte hauptsächlich in Bibliotheken vorkommen?
  >Anbietendes Interface (Familien-Interface, Client-Server-Interface)
- Warum wird das idiosynkratische Interface in Sprachen wie Java oder C# kaum verwendet?
  >In diesen Sprachen erfüllt das Klasseninterface im Wesentlichen den gleichn Zweck.
- Um welche Art von Interface handelt es sich bei dem Interface `Connector` und bei dem Interface `Connectand`?
  ```java
  interface Connector {
      void connect(Connectand c);
  }

  interface Connectand {
      Address supplyAddress();
  }

  class Xxx implements Connector {
      ...
      public void connect(Connectand c) {
          c.supplyAddress();
      }
  }

  class Yyy implements Connectand {
      ...
      public Address supplyAddress() { ... }

      public static void main(String [] args) {
          Yyy y  = new Yyy();
          Xxx x = new Xxx();
          x.connect(y);
          ...
      }
  }
  ```
>Es handelt sich um ein paarig auftredendes Client/Server-Server/Client-Interface. In dieser Client/Server-Konstellation benötigt der Server Xxx für die Ausführung seiner Dienste eine Informatione, die ihm nicht beim Aufruf übergeben wurde. In solchen Fällen wird der Server zur gegebenen Zeit beim Client Yyy rückfragen, um diese Information zu erhalten. Dazu ist es allerdings notwendig, dass der Server den Client kennt.  

- Abstrakte Klassen erlauben gegenüber Interfaces die Implementierung von nichtstatischen Feldern und Default-Methoden. Welche Vorteile hat das?
>a) Eine gemeinsame Oberklasse kann bereits eine Basisfunktionalität bereitstellen, welche in allen erbenden Klassen gleich ist und ansonsten immer wieder neu implementiert werden müsste.

>b) Die Auswirkungen nachträglicher Erweiterungen des Interface können über Default-Implementierungen abgefedert werden.

>c) Abstrakte Klassen erlauben im Gegensatz zu Interfaces die Bereitstellung von Konstruktoren, über die die Mindestanforderungen an die Erstellung der Objektinstanzen gesteuert werden können. So kann die Initialisierung gesteuert werden.

>d) Die Bereitstellung einer Default-Implementierung befreit von der Implementierung einer Methode aus der Schnittstelle. Die Typkorrektheit wird so gewahrt.

- Abstrakte Klassen erlauben gegenüber Interfaces die Implementierung von nichtstatischen Feldern und Default-Methoden. Welchen Nachteil kann das haben?

>a) Bei Vorliegen einer Default-Methode muss der Nutzer der Schnittstelle keine eigene Implementierung erstellen, da durch die Default-Methode die Typkorrektheit gewahrt ist und es dadurch nicht während der Übersetzung zu Fehlern kommt. Allerdings kann es sein, dass das Verhalten der Default-Implementierung nicht dem entspricht, was in der abgeleiteten Klasse erforderlich ist. 

- Könnte man in C# oder Java auf Interfaces verzichten und stattdessen nur noch abstrakte Klassen verwenden, ohne sich in der Ausdrucksstärke einzuschränken?

> Nein, ein Verzicht ist nicht möglich. Weder C# noch Java kennen Mehrfachvererbung. Bei Interfaces ist die Implementierung mehrerer Interfaces durch eine Klasse möglich, es ist aber eben nicht möglich von mehreren (abstrakten) Klassen zu erben.

- Wozu benötigt man Dependency Injection bei der Interface-basierten Programmierung?
> Die konsequente Nutzung von Interfaces in Variablen- und Methodendeklarationen erlaubt die Anzahl der Referenzen auf andere Klassen und damit die Abhängigkeit von ihnen zu reduzieren. Es verbleibt aber eine Abhängigkeit an eine konkrete Klasse, die durch den Aufruf des Konstrukors zur Erzeugung einer Instanz dieser Klasse entsteht. Diese (letzte) Abhängigkeit lässt sich mit Dependency Injection eleminieren.

- Dependency Injection wird auch beim Testen verwendet. Was ist der Zusammenhang von Dependency Injection und Testen?
> Idealerweise wird jede einzelne selbst implementierte Klasse durch einen Unit-Test getestet. Dabei entstehen in der Regel auch Abhängigkeiten an weitere Klassen, z.B. aus Frameworks oder Bibliotheken. Deren Funktionalität muss in der Regel nicht getestet werden, allerdings kann deren Verwendung weitere Abhängigkeiten und Anforderungen stellen. Diese Abhängigkeit von den weiteren Klassen lassen sich durch s.g. Mock-Objekte vermeiden. Das Verhalten der genutzten Klassen wird durch Objekte nur soweit wie notwendig abgedeckt.

- Wie kann man einem zu testenden Objekt die Mock-Objekte unterschieben?
> Der Code kann so geschrieben werden, dass die benötigten Objekte nicht selbst erzeugt, sondern per Dependency Injection von außen injiziert werden. Im Sinne des Single-Responsibility-Principles sollte dies ohnehin geschehen.

- Wer führt die Injektion bei der Dependency Injection durch? Erläutern Sie bitte auch den verwendeten Begriff.
> Die Aufgabe übernimmt der Assembler. Bei dem Assembler handelt es sich um ein Programmfragment (häufig eine eigene Klasse), das auufgerufen wird, um die Objekte (oder Komponenten), die miteinander kooperieren sollen, zu verdrahten. Der Assembler stellt also zur Laufzeit eine Konfiguration aus voneinander unabhängigen (in dem Sinne, dass diese keine direkten Abhängigkeiten besitzen) Objekten her.

- Was sind die Grenzen der Dependency Injection?
> a) Dependency Injection lässt sich nicht für temporäre Variablen und durch formale Parameter hergestellte Abhängigkeiten nutzen.

> b) Die Abhängigkeit wird nicht zu einem genau definierten, von außen feststellbaren Zeitpunkt (wie beispielsweise dem Erzeugen eines Client-Objektes) eingerichtet.

> c) Die Erzeugung der Abhängigkeit (also die Zuweisung des Objektes, zu dem die Abhängigkeit besteht) ist von Bedingungen abhängig, die nur die abhängige Klasse selbst kennen kann.

- Java besitzt seit Version 1.4 das Schlüsselwort `assert`, mit dem beliebige boolesche Ausdrücke zu Zusicherungen gemacht werden können. Um welches Problem handelt es sich bei diesem Programmfragment?
```java
boolen bin_da() {
    assert bin_weg();
    return true;
} 

boolean bin_weg() {
    assert bin_da();
    return true;
}
```

> Wenn die Überprüfung eingeschaltet ist (`java -ea ...`), kommt es zu Seiteneffekten. Die Methoden `bin_weg()` und `bin_da()` rufen sich gegenseitig auf, ohne das `return` je zu erreichen, so dass es nach einer Zeit zu einem Stapelüberlauf kommt.

- Welche Unzulänglichkeiten gibt es bei Assertions mittels Javas `assert` zu beachten?

> a) Es ist nicht ersichtlich, ob es sich um eine Vor- oder Nachbedingung handelt

> b) Man hat keinen Zugriff auf zurückgegebene Elemente

> c) Der direkte Zugriff auf alte (also solche, die durch die Methode verändert wurden) Werte ist nicht möglich.

> d) Mangelnde Redudanz, da boole´sche Java-Ausdrücke zu Zusicherungen gemacht werden und damit keine unabhängige Spezifikation möglich ist (im Gegensatz zu JML oder Java Bean Validation).

- Substituierbarkeit ist nur dann gegeben, wenn das substituierende Objekt den Vertrag des substituierten einhält. Was bedeutet dies für Vor- und Nachbedingungen?

> Das zu substituierende Objekt darf nicht mehr an Voraussetzungen verlangen und nicht weniger an Leistungen liefern. Die Vorbedinungen der Methoden des Subtyps müssen gleich oder schwächer sein, die Nachbedingungen gleich oder stärker.

- Warum ist der Name JUnit eigentlich falsch?
  
> Beim Unit-Testing wird per Definition immer genau eine Unit getestet (die Unit under test). Allerdings wird dies durch JUnit nicht erzwungen. Es sollte eher JRegression heißen.

- Handelt es sich bei JUnit um eine Bibliothek, ein Blackbox- oder ein Whitebox-Framework? Begründen Sie Ihre Antwort?

> Es handelt sich um ein Framework. Dies erkennt man an der Umkehrung der Ausführungskontrolle: die Testfälle werden nicht direkt aufgerufen, sondern das Framework selbst ermittelt die aufzurufenden Klassen und Methoden. Da zusätzlich Vererbung zum Einsatz kommt (`extends TestCase`), handelt es sich um ein Whitebox-Framework.

- Wann spricht man im Zusammenhang von Unit-Tests von einem *falsch negativen Ergebnis*?

> Beide Fehler, sowohl in der Implementierung als auch im Test, passieren den Unit-Test unbemerkt.

- Was versteht der Kurs 01853 unter einem Entwurfsmuster?

> Unter einem Entwurfsmuster (engl. *design pattern*) versteht man eine Vorlage, anhand derer man eine konkrete Lösung für ein Problem implementieren kann. Entwurfsmuster müssen dabei nicht 1:1 umgesetzt werden, sondern erlauben gewisse Freiheitsgrade. Entwurfsmuster enthalten schemenhafte Lösungen für Standardprobleme, die so oder so ähnlich immer wieder auftreten.

- Beim Framework Junit kommen Entwurfsmuster zum Einsatz. Testsuiten bestehen aus Tests und Testsuiten. Welches Entwurfsmuster kommt zur Anwendung?
> Composite Pattern. 

- Man muss dem Framework mitteilen, welche Methoden es sind, die die Testfälle implementieren. Jeder Testfall wird in einer eigenen Klasse definiert, die eigentliche Testmethode heißt immer gleich, zum Beispiel `void test()`. Welches Pattern würde dann angewendet?
> Das Strategy Pattern.

- In der Klasse `TestResult` benachrichtigt die Methode `strTest(test)` die sog. Listener. Welches Pattern findet Anwendung?
> Das Observer Pattern.

- Beschreiben Sie anhand der abgebildeten Methode `runBare()` der Klasse `TestCase` das Template Method Pattern.
```java
public abstract class TestCase {
  ...
  public void runBare() throws Throwable {
    Throwable exception = null;
    setUp();
    try {
      runTest();
    } catch (Throwable running) {
      exception = running;
    } finally {
      try {
        tearDown();
      } catch (Throwable tearingDown) {
        if(exception == null)
          exception = tearingDown;
      }
    }
    if(exception != null)
      throw exception;
  }
}
```

> Das Template Method Pattern besteht meist aus einer Methode, die ein Schema für eine Funktionalität bereitstellt, so dass der Ablauf immer gleich ist. In diesem Beispiel hier sorgt `runBare()` dafür, dass `setUp()`, `runTest()` und `tearDown()` in dieser Reihenfolge aufgerufen werden. Dies sind die Hook-Methoden, mit denen in erbendenen Klassen eigene Zusatzfunktionalitäten implementiert werden können (hier das Herstellen der Vorbedingungen für den Test, die Testdurchführung selbst und den Rückbau der Testinfrastruktur). Primitive Methoden sind in der Template Methode nur abstrakt (also ohne Default-Implementierung) vertreten und werden in der erbendenden Klasse implementiert und werden hier nicht dargestellt.

- Warum ist das Refactoring `Replace inheritance with delegation` hier nicht anwendbar?
```java
class A {
  protected int i = 0;
  public void f() {}
  public int g() { return i;}
}

class B extends A {
  protected int j = 0;
  public void f() {}
  public int g() { return i + j;}
}

class C {
  public void f() {
    A a = new B();
    a.f();
    a.g();
  }
}
```
> In der Klasse `C` wird einer Variablen des Typs `A` eine Instanz von `B` zugewiesen, was faktisch bedeutet, dass die Vererbung und das damit einhergehende Liskov'sche Substitutionsprinzip (LSP) explizit vorausgesetzt/genutzt wird. Die Aufhebung der Vererbung von `A` durch `B` würde diese Zuweisung unmöglich machen. Zusätzlich wird in der Klasse `B` die Methode `public int g()` aus der Klasse `A` überschrieben und darin auf die Variable `i` zugegriffen, die allerdings `protected` deklariert wurde und damit mit der Aufhebung der Vererbung nicht mehr zur Verfügung stünde. Beides verhindert eine (einfache) Umsetzung des Musters sicher und würde mehr Aufwand erfordern, da dann sowohl `B` als auch `C` geändert werden müssen:
```java
class A {
  protected int i = 0;
  public void f() {}
  public int g() { return i;}
}

class B  {
  protected int j = 0;
  private A a = new A();
  public void f() {}
  public int g() { return a.g() + j;}
}

class C {
  public void f() {
    B b = new B();
    b.f();
    b.g();
  }
}
```
- Warum ist das Refactoring `Replace inheritance with delegation` hier nicht anwendbar?
```java
abstract class A {
    public int i = 0;
    public void f() {}
    public abstract int g();
}

class B extends A {
  public void f() {}
  public int g() { return i;}
}

class C {
  public static void f(A a) {
    a.f();
    a.g();
  }
}

class D {
  public void f() {
    B b = new B();
    C.f(b);
  }
}
```
> Da die Klasse `A` abstrakt ist, kann diese nicht instanziert und in der Folge auch nicht Teil einer Zuweisung sein, was die Umwandlung in eine Delegation sicher verhindert. In der Klasse `D` wird der Methode `C.f()` eine Instanz der Klasse `B` übergeben, auch das verhindert das Refactoring. Zuletzt greift `B.g()` auf `A.i` zu. Nach dem Refactoring wäre dieser Zugriff nicht mehr möglich.

- Warum ist das Refactoring `Replace inheritance with delegation` hier nicht anwendbar?
```java
class A {
  public void f() { g();}
  public void g() {}
}

class B extends A {
  public void f() {}
  public void g() {}
}
```

> Die Subklasse `B` wird durch `A` in offener Rekursion aufgerufen.

- Wenden Sie das Refactoring *Replace Conditional with Polymorphism* auf das folgende Codebeispiel an, so dass für die Vogelarten *European*, *African* und *Norweigian_Blue* entsprechende Klassen bestehen.
```java
double getSpeed() {
  switch(_type) {
    case EUROPEAN:
      return getBaseSpeed();
    case AFRICAN:
      return getBaseSpeed() - getLoadFactor() * _numberOfCoconuts;
    case NORWEIGIAN_BLUE:
      return (_isNailed) ? 0 : getBaseSpeed(_voltage);
  }
}
```
> Mit Interface

```java
interface Bird {
  double getSpeed();
}

class European implements Bird {
  double getSpeed() { return getBaseSpeed(); }
}

class African implements Bird {
  double getSpeed() { getBaseSpeed() - getLoadFactor() * _numberOfCoconuts; }
}

class Norweigian_Blue implements Bird {
  double getSpeed() { return (_isNailed) ? 0 : getBaseSpeed(_voltage); }
}
```

> Mit Vererbung / Klasseninterface

```java
abstract class Bird {
  abstract double getSpeed();
}

class European extends Bird {
  double getSpeed() { return getBaseSpeed(); }
}

class African extends Bird {
  double getSpeed() { getBaseSpeed() - getLoadFactor() * _numberOfCoconuts; }
}

class Norweigian_Blue extends Bird {
  double getSpeed() { return (_isNailed) ? 0 : getBaseSpeed(_voltage); }
}
```
- Führen Sie beim folgenden Codebeispiel das Refactoring `Introduce NULL object` durch.
```java
Betreuering betreuerin = kurs.betreuerin;
if(betreuerin == null)
  antwort = "leider keine da, die eine Antwort wüßte";
else
  antwort = betreuerin.getAntwort();
```
> Ergebnis:
```java
class NullBetreuerin {
  String getAntwort() {
    return "leider keine da, die eine Antwort wüßte";
  }
}

...

Betreuering betreuerin = kurs.betreuerin;
antwort = betreuerin.getAntwort();
```

- Was versteht der Kurs 01853 unter Metaprogrammierung?
> Unter Metaprogrammierung versteht man die Fähigkeit eines Programmes (das Metaprogramm) ein anderes (oder sich selbst) zu beobachten oder sogar zu verändern. Metaprogrammierung ist rekursiv anwendbar, so dass auch Metaprogramme anderen Metaprogrammen unterliegen können.

- Nenen Sie drei Beispiele für Metaprogramme aus unterschiedlichen Bereichen
> - Programmierwerkzeuge wie Compiler, Interpreter, Debugger, Optimierer
> - Programmanalysewerkzeuge wie Programme zur Erhebung von Metriken, Programme zur Findung von Fehlern und Sicherheitslücken
> - Refactoring-Werkzeuge wie IDE

- Beschreiben Sie ein *vollständig reflektives* System
> Ein vollständig reflexives System ist in der Lage sich selbst, seine Struktur zu erkennen und seine Ausführung zu beobachten (Introspektion). Weiter kann es seine Ausführung ändern (Interzession oder Interzension) und auch den eigenen Code austauschen (Modifikation).

- XP (e*X*treme *P*rogramming) ist ohne geeignete Werkzeuge nicht denkbar. Nennen Sie vier Werkzeuge und begründen Sie deren Wichtigkeit für XP.

> Editoren mit Syntaxhervorhebung und automatischer Codeformatierung und Vervollständigung: in XP gibt es das Konzept der gemeinsamen Verantwortung, der Quellcode "gehört" nicht einem Entwickler. Das macht es erforderlich, auf Änderungen durch andere Entwickler schnell reagieren zu können, wobei eine automatische Vervollständigung sehr hilfreich ist. Die automatische Formatierung von Code hilft dabei, automatisiert dafür Sorge zu tragen, dass der Code visual einem gemeinsamen Standard entspricht, was ebenfalls die Einarbeitung und Übernahme des Codes vereinfacht. 
> Refactoring-Werkzeuge: in XP sollen Änderungen "billig" sein, dass heißt schnell und einfach möglich sein. Durch den iterativen Ansatz ergeben sich regelmäßig Gelegenheiten den Code wieder lesbar/wartbar zu machen oder zu erhalten. Dazu benötigt man die Unterstützung von Werkzeugen, die das Refactoring mit guten Automatismen unterstützen.
> Versionsverwaltung: die gemeinsame Verantwortung bedingt, dass jeder Entwickler jederzeit Zugriff auf den aktuellen Quelltext hat. Da sich immer auch Fehler einschleichen können und Refactorings häufig und gewünscht sind, ist es erforderlich alle Änderungen nachvollziehen zu können. Dazu ist eine Versionsverwaltungssystem unabdingbar.
> Builder: Der Builder wird benötigt um regelmäßige Abläufe zu automatisieren und so die Entwickler zu entlasten. Häufige und schnelle Tests geben Rückmeldung zur Qualität der Umsetzung. Die Software kann automatisiert in Produktion gesetzt werden.

- Im Kurstext werden die folgenden Kategorien von Interfaces unterteilt:
  - anbietendes Interface
  - allgemeines Interface
  - kontextspezifisches Interface
  - ermöglichendes Interface
 
 - Zu welcher oder welchen dieser Kategorien gehören

  > - Idiosynkratische Interfaces (A: anbietend, allgemein)
  > - Familieninterfaces (A: anbietend, allgemein)
  > - Client/Server-Interfaces (A: anbietend, kontextspezifisch)
  > - Server/Client-Interfaces (A: ermöglichend, kontextspezifisch)
  > - Server/Item-Interfaces (A: einem dritten ermöglichend, kontextspezifisch)

- Beschreiben und vergleichen Sie idiosynkratisches und Familien-Interface
> Beim idiosynkratischen Interface gibt es exakt eine Implementierung. In Java würde dies einem `interface` entsprechen einer `class`, die alle vom Interface veröffentlichten Methoden implementiert, was unüblich ist, da das Klasseninterface diese Eigenschaft schon besitzt. Ein Familien-Interface wird von mehr als einer konkreten Klasse implementiert (Beispiel: JDBC Driver) und üblicherweise im Zusammenhang mit dem Factory-Pattern oder Dependency Injection genutzt.


- Beschreiben Sie die Rolle von Mock-Objekten beim Testen.
> Beim Testen sollte sinnvollerweise pro Unit-Test nur genau eine Klasse bzw. deren Methoden getestet werden. Dabei kann es allerdings passieren, dass Objekte benötigt werden, die außerhalb der Verantwortung der zu testenden Klasse liegen und den Test dadurch erschweren. Dann hilft es, diese Objekte durch Mock-Objekte zu ersetzen, also solche, die bei bestimmten Eingabedaten definierte Rückgabewerte liefern, so dass deren Verhalten zu jederzeit im Test kontrolliert werden und das Testergebnis nicht verfälschen können. Die Mock-Objekte können über geeignete Factories oder Dependency Injection erzeugt werden.

- Wie kann man dem zu testenden Object die Mock-Objekte unterschieben?
> a) Interzession: die Objekt-Erzeugung wird abgefangen und es wird stattdessen ein Mock-Objekt erzeugt, z.B. mit einem AOP Framework. b) Dependency Injection, das Mock-Objekt wird per Setter oder Konstruktor eingefügt c) Austausch der originalen Klasse durch eine Mock-Klasse im Quelltext und dann Übersetzung für den Test.

- Gegeben sind die folgenden Arten von Interfaces und die beiden Interfaces `Runnable` und `Comparable` aus der Java-API:
- idiosynkratisches Interface
- Familieninterface
- Client/Server-Interface
- Server/Client-Interface
- Server/Item-Interface 
```java
/******************************************/
public interface Runnable {
  void run();
}

class Client implements Runnable {
  ... 
  public void run() { }
  ...
}

Client client = new Client();
new Thread(client).start();
/******************************************/
class Server {
  ...
  void sort(List<? extends Comparable> items) {}
    ...
    if(a.compareTo(b) > 0 ) {
      ...
    }
  }  
}

class Client {
  Server server;
  List<Comparable> items;
  ...
  server.sort(items);
  ...
}
/******************************************/
```
- Welcher der genannten Kategorien kann `Runnable` zugeordnet werden und warum?
> Bei `Runnable` handelt es sich um ein kontextspezifisches Client/Server-Interface. Es implementiert als Client die Methode `void run()`, die es dem Server, in dem Fall der Klasse `Thread`, ermöglicht, eine Aufgabe auszuführen.

- Welcher der genannten Kategorien kann `Comparable` zugeordnet werden und warum?
> Bei `Comparable` handelt es sich um ein kontextspezifisches Server/Item-Interface. Die Items sind hier solche, die das Interface `Comparable` implementieren. Der Client *beauftragt* den Server mit der Durchführung der Operation, also der Sortierung.

- Wir betrachten anbietende und ermöglichende Interfaces. Welche kommen vermerhrt in Bibliotheken vor und welche in Black-Box-Frameworks und warum?
> Bibliotheken sind Sammlungen von Klassen und Methoden, die Aufgaben erfüllen, die nicht notwendigerweise in einem besonderen Zusammenhang stehen, wie beispielsweise eine Bibliohek zur Durchführung grundlegender mathematischer Berechnungen (`java.lang.Math`) wie `sin()`, `cos()`, `sqrt()` oder `exp()`. Dies sind typische anbietende Interfaces, da sie dem Aufrufer direkt eine Funktion erschließen, die Ausführungskontrolle aber beim eigenen Code bleibt und auch keine Vererbung eingesetzt wird (was für ein White-Box-Framework sprechen würde). Ermöglichende Interfaces kommen häufig in Black-Box-Frameworks vor, da diese eine Funktion ermöglichen und dazu die Ausführungskontrolle zumindest teilweise an das Framwork zur Durchführung der eigentlichen Funktion übergeben wird.

- Welche technischen Eigenschaften sollte eine Design-By-Contract Sprache besitzen?

> 1. Sie sollte die Formulierung von Vor- und Nachbedingungen sowie Invarianten ermöglichen.
> 2. Sie sollte Nebenwirkungsfrei sein
> 3. Sie sollte die Formulierung unabhängig von der Implementierungssprache ermöglichen.
> 4. Sie sollte Zugriff auf den Zustand vor und nach der Veränderung besitzen und diesen vergleichen können.
> 5. 

- Welche der im Kurs behandelten DbC-Sprachen eignet sich dazu, Interfaces mit Semantik zu versehen? Begründen Sie bitte.
> Mit der Java Modelling Language (JML). Mit JML ist es möglich durch sog. Modellvariablen- und Methoden eine vollständig abstrakte Definition zu schreiben und so auch Interfaces mit Semantik zu versehen.

- Java besitzt seit Version 1.4 das Schlüsselwort `assert`, mit dem beliebige boolesche Ausdrücke zu Zusicherungen gemacht werden können. Um welches Problem handelt es sich bei diesem Programmfragment? Gibt es eine andere DbC-Sprache, die für dieses Problem eine Lösung anbietet?
```java
boolen bin_da() {
    assert bin_weg();
    return true;
} 

boolean bin_weg() {
    assert bin_da();
    return true;
}
```
> Das Problem entsteht dadurch, dass die Prüfung der jeweiligen Assertion eine Nebenwirkung auftritt (ein Methodenaufruf), der aufgrund der Tatsache, dass diese Methoden immer `true` liefern, in einem endlosen rekursiven Aufruf mündet bis es zu einem Pufferüberlauf kommt. Ja, mit JML wäre es möglich, die Vorbedingungen ohne Nebenwirkungen zu formulieren.

- Welche Unzulänglichkeiten gibt es bei Java-Asserts zu beachten, die sich insbesondere bei der Formulierung von Nachbedingungen auswirken können?
> Einem `assert` sieht man nicht an, ob es sich um eine Vor- oder Nachbedingung handelt. Es ist nur verfügbar, wenn Java über Parameter angewiesen wurde, Assertions zu behandeln und es hat ohne zusätzliche Programmierung keinen Zugang zum Zustand des Objektes vor dem Beginn der Verarbeitung. Außerdem besteht keinerlei Redundanz, die Formulierung der Assertion unterscheidet sich nicht vom sonstigen Code.

- Kann die Programmiersprache EIFFEL das Problem der Seiteneffekte und das Problem der mangelnden Redundanz lösen?
> Nur sehr bedingt, da Verträge inhärenter Teil der Syntax der Sprache sind. Aufrufe mit Seiteneffekten sind zwar nicht erlaubt, werden durch den Compiler aber auch nicht verhindert. 

- Beim Composite Pattern in seiner klassischen Form folgen Blätter als auch Komposita demselben Protokoll. Was ist das Problem daran?
> Das selbe Protokoll bedeutet praktisch, das beide die gleichen Methoden anbieten, was allerdings nur bedingt sinnvoll ist: eine Methode wie `getBlätter()` macht auf einem Blatt keinen sinn, genauso wenig wie `getInhalt()` bei einem Kompositum. Damit werden bei sehr unterschiedliche Objekte gleich behandelt, was zu Fehlern führen kann.

- Beschreiben Sie das Refactoring `Extract Interface`.
> Aus den Deklarationen der Methoden einer Klasse wird ein Interface erstellt. Das bietet sich an, wenn eine Reihe Klassen von der Klasse erbt. Dadurch entsteht ein Family Interface und die Abhängigkeit von der Klasse wird reduziert (fragile base class problem).

- Der folgende Code enthält den Aufruf einer geerbten Methode und eine offene Rekursion. Identifizieren Sie beide.
```java
class Pferf {
  void wiehern() { fressen(); }
}

class Haflinger extends Pferd {
  void mitreden() { wiehern(); }

  void fressen() {
    System.out.println("Hafifutter");
  }
}
```

> In der Methode `mitreden()` der abgeleiteten Klasse `Haflinger` wird die geerbte Methode `wiehern()` aufgerufen. Diese wiederum ruft die Methode `fressen()` aufgerufen. Durch die Späte Bindung jedoch wird die in der erbenden Klasse `Haflinger` implementierte Version von `fressen()` aufgerufen, was der offenen Rekursion entspricht.

- Werden Annotationen nur in reflexiven Programmen eingesetzt? Begründen Sie Ihre Antwort.
> Annotationen können verschiedenen Zwecken dienen, aber eben nicht nur der Beeinflussung des Programmablaufs. So gibt es auch sog. Marker-Annotationen, die dazu dienen den Code mit Metainformationen zu versehen. Beispiele dafür sind `@API`, mit dem ein veröffentlichtes Interface ausgezeichnet werden kann, `@Deprecated`, um Klassen oder Methoden zu markieren, die in einer der nächsten Versionen nicht mehr vorhanden sein werden oder `@Override`, damit der Compiler die Re-Implementation geerberter Methoden prüfen kann.

- Ihre Firma schreibt ein Programm, um die Stellwerke der Deutschen Bundesbahn besser verwalten zu können. Ihre Programmierer wollen dabei für typische Cross-Cutting-Concerns AOP einsetzen. Halten Sie dies für eine gute oder eine schlechte Idee? Warum?
> Cross-Cutting-Concerns sind Anforderungen, die an vielen Stellen ähnlich bis identische umgesetzt werden müssen. Typische Beispiele sind Testing (insbesondere mit Mocking), Caching, Logging, Monitoring, Auditing, Profiling oder die PRüfung von Zugriffsrechten. Wollte man diese Dinge mit klassischer OOP umsetzen, würden unweigerlich eine Menge Redundanzen entstehen und der Programmtext würde durch die notwendigen Erweiterungen nicht besser lesbar. Bei der AOP hingegen können die Aspekte unabhängig vom eigentlichen Programmcode definiert werden. Dazu werden Point-Cuts definiert, also definierte Stellen, an denen die Ausführung unterbrochen und dann sog. Advices ausgeführt werden. Der eigentliche (fachliche) Programmtext wird zusammen mit dem Webeplan (also der Information wie der Programmtext mit den Aspekten zusammengehört) durch einen Weber (Weaver) verwoben und dann kompiliert. Dem fachlichen Code sieht man die Aspekte nicht an. Ja, ich halte das für eine gute Idee.

- Bennen Sie die fünf größten Risiken in der Softwareentwicklung und welche Lösung XP zu deren Reduktion anbietet.
> Zu wenig Personal: dem wird durch die gemeinsame Bearbeitung an Code, Pair-Programming und Reduktion der Workload begegnet.
> Unrealistische Zeitpläne und Budgets: diese entstehen vor allem durch zu lange und zu umfangreiche Planungen am Beginn des Projektes. Bei XP werden sog. Sprints mit relativ kurzer Dauer umgesetzt und nur die vorher explizit vereinbarte Funktionalität umgesetzt.
> Entwicklung der falschen Funktion: bei XP werden nur solche Funktionen umgesetzt, die explizit auf das vereinbarte Ziel einzahlen, YAGNI ist ein wesentliches Prinzip. Da Änderungen jederzeit möglich sind, kann fehlende Funktionalität in einem weiteren Sprint implementiert werden.
> Entwicklung der falschen Benutzerschnittstelle: bei XP ist ein Vertreter des Kunden vor Ort oder zumindest eng eingebunden um die Arbeitsergebnisse und insbesondere das UI sofort begutachten zu können.
> Vergoldung: Funktionen, die eventuell nur selten genutzte Anwendungsfälle bedienen oder gänzlich überflüssig sind, *vergolden* eine Software, machen ihre Entwicklung aber teuer und langwierig. Dem wird mit dem Kunden vor Ort und den kurzen Sprints engegengewirkt. Mit jedem Release soll ein Mehrwert ausgeliefert werden, es wird also zu Beginn der Sprints die Frage zu beantworten sein, ob das Feature dem Ziel zuträglich ist.

- Um welche Art von Metaprogrammierung (Introspektion, Interzension oder Modifikation) handelt es sich bei:
- - Java Reflection
- - AOP
- - LISP oder Prolog
- - JUNIT 3.8
- - Debugger
- - Compiler
- - Refactoring 
- - genetische Algorithmen
> - Java Reflection: Introspektion, Interzension
> - AOP: Interzension
> - LISP / Prolog: Introspektion, Interzension und Modifikation
> - JUNIT 3.8: Introspektion
> - Debugger: Introspektion, Interzension
> - Compiler: Introspektion, Modifikation
> - Refactoring: Introspektion, Modifikation
> - genetische Algorithmen: Introspektion, Modifikation

- Erläutern Sie den Unterschied zwischen Forwarding und Delegation, indem Sie auf die Bedeutung von `this` eingehen.
> Beim Forwarding bezeichnet `this` das Objekt, an das der Aufruf weitergeleitet wurde. Bei der Delegation bezeichnet `this` das Objekt, von dem weitergeleitet wurde (quasi eine Rückreferenz). In Java ist eine Delegation nur über einen Umweg möglich.

- Bei welchem Refaktoring ist es wichtig den Unterschied zwischen *Forwarding* und *Delegation* zu kennen?
> Dies ist bei `Replace inheritance with delegation` wichtig. Insbesondere, da es in Sprachen wie Java keine echte Delegation gibt und es beispielsweise nicht möglich ist per `this` auf Instanzvariablen des delegierten Objektes zuzugreifen.

- Wählen Sie für den folgenden Quellcode fünf Refactorings aus der Liste aus und begründen Sie kurz, wo und wie diese sinnvoll angewendet werden können:
- - *Replace nested conditions with guard clauses*
- - *Introduce parameter object*
- - *pull up method*
- - *replace conditions with polymorphism*
- - *decompose conditional*
- - *pull up field*
- - *extract class*
- - *encapsulate field*
```java
abstract class Figure {
    protected boolean isVisible;
    protected boolean isDrawable;
    protected boolean printWhenInvisible;
    private String d;

    public void setDescription(String description) {
        this.d = description;
    }

    public String toString() {
        String result = d;
        if (!isDrawable) {
            // nothing
        } else {
            if (isVisible) {
                result = d + " (will be drawn)";
            } else {
                if (printWhenInvisible) {
                    result = d + " (will be drawn but invisible)";
                }
            }
        }
        return result;
    }
}


class Rectangle extends Figure {
    public int x_position;
    public int y_position;
    public int width;
    public int length;

    public int getPerimeter()
        return 2 * width + 2 * length;
    }

    public void setPosition(int x, int y) {
        this.x_position = x;
        this.y_position = y;
    }

    public void draw(int foreground_red, int foreground_green, int foreground_blue, int background_red, int background_green, int background_blue) {
        if (isDrawable && (isVisible || printWhenInvisible)) {
            /* ... draws Rectangle with given fore- and background color ... */
        }
    }
}

class Square extends Figure {
    public int x_position;
    public int y_position;
    public int width;

    public int getPerimeter() {
        return 4 * width;
    }

    public void setPosition(int x, int y) {
        this.x_position = x;
        this.y_position = y;
    }

    public void draw(int foreground_red, int foreground_green, int foreground_blue, int background_red, int background_green, int background_blue) {
        if (isDrawable && (isVisible || printWhenInvisible)) {
        /* ... draws Square with given fore- and background color ... */
        }
    }
}
```
> 1. *Pull up method*: In den Klassen `Rectangle` und `Square` ist die Methode `draw` identisch, diese kann daher in die Klasse `Figure` hochgezogen werden
> 2. *Introduce parameter object*: dies bietet sich für die Methode `draw` an, denn diese wird mit sechs Paramtern versorgt. Aufgrund der Bennenung `foreground_xxx` und `background_yyy` bietet es sich zudem an, dies auf zwei Parameter aufzuteilen.
> 3. *encapsulate field*: bietet sich für alle public/protected Variablen an, damit kein direkter Zugriff auf den inneren Zustand notwendig ist. In dem Zusammenhang wende ich weiter *Rename* an, um die Bennung sinnvoller zu gestalten.
> 4. *pull up field*: bietet sich für die Felder der Klassen `Rectangle` und `Square` an, diese können in die Klasse `Figure` gezogen werden und werden danach mit *encapsulate field* versorgt.
> 5. *Replace nested conditions with guard clauses*: bietet sich bei der Methode `toString()` und bei der Methode `draw` an.

- Was sind Anzeichen Interfacebasierter Programmierung?
> Klassen implementieren Interfaces (anstatt Vererbung zu nutzen) und Variablen, Rückgabewerte und Methodenparamter werden mit Interfaces deklariert.

- In alten Java-Versionen hatten Interfaces keine Default-Implementierung. Welchen Nachteil hatte dies?
> Bei späteren Änderungen des Interfaces erzeugte dies immer Anpassungsbedarf bei implementierenden Klassen, da die Methode implementiert werden muss um kompilierbar zu sein. Die Default Implementierung reduziert solche Ereignisse.

- Welche Prinzipien sind im Zusammenhang mit Interface-basierter Programmierung wichtig?
> 1. Interface segregation principle: Abhängigkeiten sollten auf das kleinstmögliche Interface reduziert werden.
> 2. Dependency und insbesondere Interface injection: die Abhängigkeit von einer konkreten Klasse oder einem konkreten Objekt wird reduziert.
> 3. Information hiding: der innere Zustand bleibt durch das Interface verborgen 
> 4. Dependency inversion: konkrete Implementationen sollen von Abstraktion abhängen

- Wie können Familien-Interfaces eingesetzt werden?
> Mit Familieninterfaces ist es möglich, Objekte zur Laufzeit aus einer Wahl von spezialisierten Klassen wählen zu können, z.B. über Dependendy Injection oder das Factory Pattern.

- Wo finden ermöglichende Interfaces vornehmlich Verwendung?
> Sie finden vor allen Black-Box-Frameworks Verwendung. Diese sind durch Depencency Inversion gekennzeichnet, was bedeutet, dass die Ausführungskontrolle mindestens teilweise dem Framework übergeben wird um Aufgaben zu erledigen.

- Wie arbeiten Frameworks und wie kann man Whitebox- und Blackbox-Frameworks charakterisieren?
> Frameworks stellen Interfaces und Klassen zur Verfügung, die innerhalb einer Problemdomäne zueinander passende Klassen und Objekte mit einem Ordnungsrahmen Lösungen anbieten. Bei White-Box-Frameworks werden diese vor allem durch Vererbung und Spezialisierung genutzt, da der innere Aufbau bekannt ist. Bei Blackbox-Frameworks sind nur die Interfaces bekannt, die implementiert werden müssen. Die Ausführungskontrolle wird zumindest zeitweise dem Framework übergeben.

-  Wann sollten Klasseninvarianten überprüft werden?
> Mindestens nach jedem externen Methodenaufruf. Wegen des Problems des Alialising allerdings (mehrere Variablen zeigen auf dasselbe Objekt) kann es allerdings zu unbeabsichtigten Änderungen kommen. Besser sollten die Klasseninvarianten daher nach jedem Methodenaufruf geprüft werden.

- Substituierbarkeit ist nur dann gegeben, wenn das substituierende Objekt den Vertrag des substituierten einhält. Das tut das substituierende Objekt genau dann, wenn es nicht mehr an Voraussetzungen (Vorbedingungen) verlangt und nicht weniger an Leistung (Nachbedingungen) liefert. Mit anderen Worten: Die Vorbedingungen der Methoden eines Subtyps müssen gleich oder schwächer sein, die Nachbedingungen gleich oder stärker. Wie überprüfen das Eiffel und JML?
> Die Vor- und Nachbedingungen werden logisch miteinander verknüpft. Die Vorbedingungen werden disjunktiv, die Nachbedingungen konjunktiv verknüpft.

- Das Design by contract ist geeignet, die Schwachstelle der Interfacespezifikationen à la JAVA und C# zu beheben. Welche Schwachstelle ist hier gemeint? Wie kann sie behoben werden?
> Das Interface macht keine Aussagen über die Semantik, also das konkrete Verhalten. Durch die Formulierung der Nachbedingungen kann formuliert werden, welche Auswirkungen das Interface mit sich bringt, also welche Änderungen sich durch dessen Ausführung ergeben.

- Wie kann das Schreiben expliziter Testfälle die Möglichkeiten des Design by Contract wirkungsvoll ergänzen?
> Mit dem Schreiben expliziter Testfälle kann man erzwingen, dass bestimmte/seltene Code-Fragmente gezielt angesteuert werden.

- Gegeben sei eine Klasse oder Methode, die von einer Datenbank abhängt, auf die die Testenden keinen Zugriff haben. Wie kann trotzdem ein Unit-Test durchgeführt werden?
> Es ist möglich, diese Datenbank während des Tests durch eine Mock-Variante zu ersetzen.

- Wie schiebt man den zu testenden Objekten die Mock-Objekte unter?
> Durch Verändern des Quellcodes für den Test, durch Interzession (also durch das Abfangen der Objekterzeugung) oder durch Dependency Injection.

- Was sind die Grenzen des Einsatzes von Mock-Objekten?
> Statische Membervariablen lassen sich nicht mocken, ebensowenig temporäre Variablen und auch keine `final` markierten Variablen oder Klassen (laut Skript!).

- Wann liegt ein falsch positiver, wann ein falsch negativer Test vor?
> Ein *falsch negativer* Test liegt vor, wenn der Fehler sowohl im Code als auch im Test durchgeht. Ein *falsch positiver* Test liegt vor, wenn der Test durchgeht, das Testergebnis in Wahrheit aber falsch ist oder sein müsste.

- Wie kann man falsch positive, wie falsch negative Tests entlarven?
> Falsch-positive Tests können entlarvt werden, indem der Code eingehend gegen die implementierten Tests geprüft wird. Sollte der Test trotz korrekter Daten und korrekter Implementierung fehlschlagen, muss der Test fehlerhaft sein. Falsch-negative Tests können durch absichtliche Änderung des Programmcodes geprüft werden, z.B. mit Mutation-Testing (https://pitest.org/).

- Welche zwei Grundprinzipen der objektorientierten Programmierung sorgen für wiederverwendbare Entwürfe?
> - Program to an interface, not an implementation.
> - Favor object composition over class inheritance.

- Welche Gründe gibt es, die Vererbung infrage zu stellen?
> Die Vererbung koppelt eine Subklasse direkt an die Superklasse. Dadurch stehen aber alle Elemente der 
> Superklasse auch der Subklasse zur Verfügung. Wenn aus der Subklasse ein Objekt entsteht, basiert dieses Objekt
>  damit auf zwei Klassen. Das kann dann durch die späte Bindung (late binding) zu der Situation führen, dass
>  eine Methode der Superklasse eine Methode der Subklasse aufruft, wenn die Subklasse eine speziellere Version
>  davon bereitstellt und überschreibt; dies wird offene Rekursion genannt.
> Hinzu kommt das Problem der "fragile baseclass". Da in aller Regel die erbenden Klassen kontrolliert werden
> können, führen Änderungen an Basisklassen zu Problemen in allen erbenden Klassen, so dass dadurch auf beiden
> Seiten Abhängigkeiten und Aufwände entstehen. 

- Welche Vorteile haben Entwurfsmuster?
> Entwurfsmuster entlasten vom Finden einer idealen Lösung. Sie sind im besten Fall erprobt und haben sich in der
> Praxis bewährt. Für Entwurfsmuster bestehen nicht nur Hinweise oder Vorgaben zur Implementierung, sondern auch
> die Bedingungen für eine Implementierung nach diesem Muster sprechen und welche Vor- als auch Nachteile damit
> verbunden sind. Entwurfsmuster sind in aller Regel sprachunabhängig, wenngleich die allermeisten eher im OOP-Umfeld Verwendung finden.

- Wie kann die Umkehrung der Ausführungskontrolle vieler Whitebox-Frameworks umgesetzt werden?
> Das geschieht in  aller Regel über die offene Rekursion:  Dabei wird eine Subklasse einer meist abstrakten
> Basisklasse des Frameworks erstellt, eigene Funktionalität in einer definierten Methode implementiert und eine
> Instanz dieser Subklasse dem Framework übergeben. Das `Template method pattern` bietet eine strukturierte Schablone, die dies ausnutzt.

- Was ist eine besondere Nachbedingung für Refactorings und wie kann sie überprüft werden?
> Der Code muss nach dem Refactoring die gleiche Funktion bieten und darf sich nicht anders verhalten. Um das sicherzustellen, ist es notwendig Tests durchzuführen, deren Ergebnis sich vorher und nachher nicht unterscheiden darf. Ohne vorhandene Tests bleibt nur das Back-to-Back-Testing, bei dem die Ausgaben des Programms vor und nach dem Refactoring verglichen werden. 

- Was ist beim Automatisieren von Refactorings zu beachten?
> Für die wenigsten Refactorings ist ein einfaches Suchen und Ersetzen ausreichend. Damit diese umgesetzt werden können, ist die Kenntnis der Programmiersprache auf syntaktischer Ebene notwendig. Die dabei eingesetzten Werkzeuge übersetzen dazu den Quellcode in einen AST (abstract syntax tree) und nutzen diesen um die notwendigen Änderungen zu bestimmen. Die Undo-Funktion sollte sehr zuverlässig funktionieren und es sollten ausreichend Unit-Tests vorhanden sein, um das Ergebnis prüfen zu können.

- Welche acht Vorbedingungen müssen für die Refaktorisierung „Vererbung durch Delegation ersetzen“ erfüllt sein?
>dsf

- Mit welchen Refaktorisierungen kann man Bedingungen vereinfachen?
> - replace nested conditions with guard clauses
> - replace conditions with inheritance
> - decompose conditionals

- Macht es Sinn, das Refactoring „Superklasse extrahieren“ einzusetzen? Was muss man dabei beachten?
> Das macht dann Sinn, wenn die Überschneidungen der beteiligten Klassen hoch genug sind und die möglichen Spezialisierungen gut abgeleitet werden können. Dabei kommt es weniger auf die syntaktische Gleichheit an (gleiche Bennenung der Bezeichner etc.) sondern mehr auf die semantische Ähnlichkeit.

- Wann sollte man die Refaktorisierung „Interface extrahieren“ anwenden? Welche andere Rafoktorisierung hilft hier, die Methoden zu bestimmen, die ein extrahierendes Interface haben muss, damit es als Typ einer Variablen verwendbar ist? Wie geht man vor, um den Typ der Variablen konstruktiv zu bestimmen?
> - Wenn mehrere Clients die gleiche Untermenge eines Klasseninterfaces nutzen oder wenn mindestens zwei Klassen eine große Überlappung bei ihrem Klasseninterface nutzen. 
> - Dazu kann man das Refactoring *infer type* nutzen, dass man entweder durch Ausprobieren oder durch Ermittlung der Typinferenz findet. Dazu werden alle Zuweisungen verfolgt und das kleinstmögliche Netz, dass alle Zuweisungen befriedigt ist das gesuchte Interface.
  
- Was sind laut Kurstext unverzichtbare Metaprogramme?
> Compiler, Debugger, Linker, Refaktoringwerkzeuge, Builder

- Welchen Sprachen ist die Metaprogrammierung inhärent und warum ist das so?
> Das sind Sprachen, bei denen Programm und Daten mit der gleichen Struktur abgebildet werden (die sog. Homoikonizität). Dazu gehören Sprachen wie LISP (Listen), Prolog (Prädikate) oder aber TCL (Strings).

- Nennen Sie drei weitere Einsatzmöglichkeiten der Metaprogrammierung.
> - Erweiterung einer Sprache um zusätzliche Eigenschaften
> - Dynamische Konfiguration eines Programmes zur Laufzeit
> - Selbstverändernder Code, etwas genetische Algorithmen oder aber JIT-Compiler

- Welche Fähigkeiten besitzt ein vollständig reflexives System?
> Es ist in der Lage seine Struktur und seinen Zustand zu erfassen (Introspektion), es kann den Programmablauf verändern (Interzession) und den eigenen Code verändern (Modifikation).

- Beschreiben Sie die Möglichkeiten der Introspektion.
> Introspektion ist in der Lage das Programm in seiner Struktur (Klassen, Interfaces, Annotationen, Variablen) zu erfassen und seinen Zustand festzustellen (Inhalte von Variablen).

- Wie funktioniert die Introspektion in Java? Welche Probleme können auftreten? Nennen Sie zwei Anwendungen, die die introspektiven Eigenschaften von Java nutzen.
> Im Java JDK sind für alle möglichen Programmbestandteile Klassen vorhanden, die diese abbilden und die Möglichkeit eröffnen, dass vorliegende Programm zu untersuchen. Problematisch dabei ist, dass mögliche Fehler nicht durch den Compiler entdeckt werden können (da die Typkonformität erst zur Laufzeit bestimmt werden kann) und daher mit `try ... catch`-Blöcken abgesichert werden müssen. Auch steht keine Unterstützung durch Refactoring-Werkzeuge zur Verfügung.

- Was versteht man unter Interzession und was ist eine typische Anwendung?
> Interzession bezeichnet die Fähigkeit den Programmablauf zur Laufzeit zu beeinflussen. Eine typische Anwendung ist AOP.

- Was versteht man unter Modifikation und was ist ein typisches Beispiel?
> Modifikation ist die Fähigkeit das Programm zur Laufzeit verändern oder erweitern zu können. Typisch ist die Verwendung bei genetischen Algorithmen oder beim JIT Compiler in Java, der den Code zur Ausführungszeit beobachtet und gegen optimierten Code austauscht. In Sprachen wie Python oder Ruby wird dies z.B. für Monkey-Patching genutzt.

- Warum möchte man aspektorientierte Programmierung (AOP) anwenden? Was ist ein typisches einfaches Beispiel für einen Aspekt? Was ist ein größeres Beispiel?
> AOP bietet sich an, wenn sog. Cross-Cutting-Concerns versorgt werden müssen, also Anforderungen, die nicht primär fachlich gefordert werden. Typische Anwendungen sind Logging, Auditierung, Caching, Profiling. Ein größeres Beispiel ist das `Observer pattern`, dass sich mit AOP umsetzen lässt.
