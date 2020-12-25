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




