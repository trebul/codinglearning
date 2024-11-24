při vytváření repozitáře se dává extends nějakej repozitář záleží co člověk používá já tady používám JpaRepository a pak <Název třídy, datovej typ (ovšem nedává se int ale Integer>
```java
public interface TeacherRepository extends JpaRepository<Teacher, Integer> {
}
```

#### rozdíl mezi integer a int:
- integer je lepší pokud se jedná o hodnoty mezi -128 až 127
- metody který pracujou s objektama zásadně vyžadujou integer
- integer se nedá porovnávat pomocí `==` a `!=`
  ```java
  //použití integeru
      public Student updateStudent(int id, Student student) {
        student.setId(id);
        return studentRepository.save(student);
    }

  //použití Intu	
  Integer myIntegerObject1 = new Integer(10);
  ```
#### Čtení dat z inputu
- System.in
  stejný jako system.out  čte data z konzole nebo terminálu
- Sytem.console
  přečte to jeden řádek ale spousta IDE to nepodporuje, lze použít pokud se spouští z terminálu
  ```
  String text = System.console().readLine("Čus dej text");
  System.out.println(text + " tvůj text");
  ```
- Command line arguments
  při spouštění programu přes terminál
- Scanner
  klasika pro začátečníky
  ```
  Scanner scanner = new Scanner(System.in); //musí se vytvořit instance scanneru nejdřív
  System.out.println("jaký je tvoje jméno");
  String name = scanner.nextLine();
  System.out.println(name + " je tvoje jméno");

  //při použití scanner.NextInt(); metoda nejde na další řádek a musí se na další řádek manuálně
  int numbr = scanner.NextInt();
  scanner.nextLine();
  ```
#### :: syntaxe
od javy 8 se dá referencovat metody přes :: místo volání. Je to pro lambda výrazy
```
//představ si že máš metodu
public void print(String message) {
        System.out.println(message);
    }

//normální volání print při procházení listu
List<String> list = Arrays.asList("A", "B", "C");
list.forEach(message -> obj.print(message));//volám metodu nad každým prvkem v listu

//použití ::
List<String> list = Arrays.asList("A", "B", "C");
list.forEach(obj::print); //takhle to ví že nad každým prvkem to má použít metodu print aniž bych musel psát něco dalšího
```
### lambda výrazy
v podstatě 
parameter -> expression
výrazy jsou limitované = musí hned vracet hodnotu, nemůžou obsahovat ify/fory apod.
pokud je potřeba tam mít složitější operace pak je potřeba to dát do blocku

(param1) -> {
//do něco
}
```
public void print(String message) {
        System.out.println(message);
    }

//normální volání print při procházení listu
List<String> list = Arrays.asList("A", "B", "C");
list.forEach(message -> obj.print(message)); //takže tohle je lambda výraz
```
#### Optional<>
optional znamená že to může ale nemusí obsahovat hodnoty
```
//například u metody findById se nemusí nic vracet takže tam se pak dá použít optional
Optional<String> emptyString = Optional.empty();
//optional pak má metodu isPresent() na zjištění jestli je nebo není null
Optional<String> str = Optional.of("hello"); //pokud není hodnota null tak se dá použít of()
if(str.isPresent()){
 System.out.println("je tu");
}
//tohle printne je tu protože str = hello	
```

#### Switch
ve switchi nelze používat long, float, double nebo boolean
fall through:
  pokud case matchne tu hodnotu tak se provede všechno v tom casu až po break; - pokud tam break není tak to pokračuje dál skrz další casy (nevěděl jsem že tam jde nenapsat break tbh)
- enhanced switch
```java
//klasickej switch
case X:
 //do něco
 break;//nebo return

//enhanced
//taky jde
return switch (value) {} //pak tohle jde přiřadit nějaký proměnný
case X -> //do něco;
//pokud se používá funkce v enhanced switchi tak místo return se používá
yield value;
return switch (month) {
case "jan", "feb", "mar" -> "1st";
case "apr", "may", "jun" -> "2nd";
case "jul", "aug", "sep" -> "3rd";// 3rth haha
case "oct", "nov", "dec" -> "4th";
default -> {
 String badResponse = "wrong month";
 yield badResponse;
}
};
```
tohle je pro java > 14 btw

#### Interface
je abstraktní třída ve který se jen definuje název metody a pak se v další třídě implementuje(příklad uveden nahoře)

----
#### Stream() metody
takže v podstatě se to používá na různý pole a pak se na to řetězí další metody jako např .filter() nebo .map() který pak accessujou ty data co máš v tom poli.

- Map()
v podstatě stejný jako v javascriptu jen tady se musí ty data nejdřív streamovat než tam lze použít map().
```java
		List<Integer> cisla = Arrays.asList(1,2,3,4,5);

		List<Integer> cislaX2 = cisla.stream().map(number -> number*2).collect(Collectors.toList());
```
- collect()
collect slouží k tomu aby třeba v tom příkladu co tady mám vytvořil ten list, protože map tady vrací jen nějakej stream dat takže se to ještě musí hodit do listu.
Bere data a udělá z nich třeba list, pak to jde na použít na sety a mapy ale jde to upravit a použít vlastní implementaci.

----
#### Anotace
pokud má daná třída nebo interface anotaci tak se z ní stává Bean. Při startu Spring aplikace se pak provádí sken na tyhle Beany a vytváří se instance těch tříd.

- @Component
generic anotace pro jakýkoliv typ beanu.
- @Service
většinou pro třídy, které obsahují obchodní logiku
- @Repository
pro třídy, které komunikují s databází
- @Controller
na zpracování a vyřizování requestů
- @Autowired
slouží na injectnutí Beanu do classy

Tyhle anotace se používaj aby se to pak dalo injectit tam kam je potřeba, právě přes další anotaci.
#### Inject
existuje několik druhů injectů
zatím jen používám ten nejvíc basic
```java
@Autowired
private LubertRepository lubertRepository; 
```
používá se to pokud je potřeba používat metody z třídy A ve třídě B.

#### Mapstruct
vytváří se interface nad kterým se napíše anotace @Mapper
v něm se pak definují metody, které jsou automaticky generovány při mvn clean install
pokud se jedná o vnořené objekty které nejsou v DTO třídě je potřeba je specifikovat
je to ofc limited v tom kolik toho zná
```
@Mapper(componentModel = "spring")
public interface StudentMapper {

   @Mapping(source = "address", target = "addressDTO")
   StudentDTO studentToStudentDTO(Student student);//jelikož třída StudentDTO nemá objekt address ale addressDTO tak se to musí specifikovat přes @Mappping source je z původního objektu a target je v tom novém objektu
   Student toStudent(StudentDTO studentDTO);
}
```
#### Classy
classa může bejt buď public nebo přístupná jen v tom package kde je
```
public class Ishmoz {
}

class Ishmoz {
}
```

- Encapsulation

má dva významy

1. schovat atributy a nebo metody
2. spojit chování a atributy do jednoho objektu

#### Constructory
slouží k vytvoření objektu třídy
dá se je chainovat tzn zavolat konstruktor s parametrama a nastavit defaultní hodnoty v jiným konstruktoru

```
public Student() {
//chain musí být hned to první a slouží to k nastavení defaultních hodnot objektu
this(1,15,"Karel");
}

public Student(int id, int age, String name){
this.id = id;
this.age = age;
this.name = name;
}
```

#### Reference vs Object vs Instance vs Class
![image](https://github.com/user-attachments/assets/d1b2c85d-dd7b-45e3-a986-9ec1ac25b8a1)
když si vytvářím objekt nějaký classy tak se tomu dá říkat instance
a ta adresa kde ta instance je se nazývá reference
reference se daj předávat jako parametry do konstruktorů a metod

#### Static proměnný
```
static String name = "Karel;

//imagine je to třeba v classe User
//peší volat classu než volat instanci
User.name //vrátí Karel

//pokud ale třeba udělám
static String surname;

//zase classa User
User userA = new User("kulbac");
User userB = new User("Vopica");
System.out.println(userA.surname);
System.out.println(userB.surname);
//oboje printne vopica protože to je static takže to nevytváří instance
```
#### Static vs Instance metody
##### Static metody
- nejde v nich používat this()
- pokud jsou volaný ve stejný tříde tak se daj zavolat jen jako metoda(); místo třeba User.metoda();

##### Instance metody
- musí se nejdřív vytvořit instance
- instance můžou taky přistupovat k statickejm metodám a proměnejm

![image](https://github.com/user-attachments/assets/e3cdc6e2-0c36-4044-acaa-51bc76df7d4b)

#### POJO
plain old java object je třída která má většinou pouze instanční proměnný
používá se to pro předávání dat mezi třídama a má to většinou jen pár metod kromě setterů a getterů
taky se tomu dá říkat bean nebo JavaBean
POJO se taky říká entity
používá se to v DTO 

#### Hashmapy

```
HashMap<String, Integer> items = new HashMap<>(); //znamená že String bude key a value bude integer
items.put("Tatranka", 12345);
items.put("Karlův nůž", 42069);

items.get("Tatranka"); //vrátí 12345
items.containsKey(); //vrací true/false pokud to je v hashmapě nebo není
items.containsValue(); //stejný jen pro value místo klíče
//pokud chci vložit key kterej už je v mapě tak se to updatuje

items.replace("Karlův nůž", 0); //nahradí to

//pokud replace nenajde prvek v mapě tak to nic neudělá
//ale je tu
items.putIfAbsent("Kolo", 2587);

items.remove("Kolo");

items.clear(); //obvious

items.size();

for(String i : items.keySet()) //jak procházet hashmapu
//i obsahuje ten klíč takže na získání value se použije get
items.get(i);
```
hashmapy nemaj stejný pořadí ve kterým jsem to vkládal, u hashmap je mi jedno na který pozici to je <br />
pokud je tam toho moc v tom linkedlistu tak se vytvoří balance tree (defaultně je 8)

#### Hashtable
![image](https://github.com/user-attachments/assets/42718dc9-d919-4ba7-85b0-245dbd830500)
pokud jdou 2 páry na stejný index tak se z toho vytvoří z toho indexu vytvoří linkedlist <br />
![image](https://github.com/user-attachments/assets/082ecabf-59cc-4202-9b47-3ece61661cff)
<br /> a pak se prochází ten index, ale generally se preferuje aby každej měl vlastní index <br />
každýmu indexu se taky říká bucket

```
HashTable<Integer, String> food = new HashTable<>(); //do () se dá nastavit velikost a load factor

food.put(1, "Kebab");
//metody jsou skoro stejný jako u HashMap
//ale tohle má navíc metodu hashCode který vrací hash klíče  
```
jsou dobrý v tom, že se rychle vkládá, hledá a maže páry. <br />
není to vhodný pro malý data sety ale pro velký to je banger <br />
komplexita je O (1) v nejlepším případě a O (n) v nejhorším (pokud je tam hodně kolizí)


#### Shallow vs Deep Copy
![image](https://github.com/user-attachments/assets/40ad98bb-df33-453a-8287-fb3fbc941d91) <br />
v podstatě pokud u shallow copy budu měnit adresu tak se ta změna projeví u obou objektů
![image](https://github.com/user-attachments/assets/14193df7-98f3-47be-aa7d-6c108a462a94) <br />
pokud budu měnit adresu tak se ta změna projeví pouze u prvního objektu

#### Spring kontejnery

je to základní prvek springu a slouží k řízení životního cyklu objektů v aplikaci. Kontejner se stará o vytvoření, konfiguraci a správu instancí beanů.

jsou 2 typy kontejnerů

- BeanFactory

  Základní rozhraní pro Spring kontejnery. Vhodný pro jednoduché aplikace, kde není potřeba to nějak moc řešit
- ApplicationContext

  Upgraded verze BeanFactory, která má rozšíření a je nejvíc používanej kontejner ve Spring aplikacích

Spring kontejnery nevytváří objekty přímo ve třídách (nebo správu) ale poskytuje závislosti (beany) třídám
#### Bean scopes

je jich 5, ty 2 nejdůležitější jsou singleton a prototype

- Singleton

  Pouze jeden/ Spring kontejner, defaultní inicializuje se během inicializace projektu
  
  třeba @Service anotace nad třídou
- Prototype

  novej bean je vytvořenej pro každej request nebo referenci, čeká na get bean

  používá se když je potřeba aby každá komponenta měla vlastní instanci beanu

  ```
  @Scope("prototype")
  @Component
  public class myPrototype () {}
  ```
- Request

  novej bean pro každej servlet request ale pokud to je během toho requestu tak tam bude jen jeden

  Používá se pokud každý HTTP request potřebuje vlastní instanci beanu, třeba data specifický pro danej request

  označuje se obdoně jako prototype jen místo "prototype" se píše "request"
- Session

  novej bean pro každej session

  Používá se pro beany, který maj stav, který je potřeba uchovat během celý sessiony uživatele (login informace např.)

  @Scope("session")
- Global session

novej bean pro každej HTTP session

využívá se třeba v enterprise aplikacích

@Scope("globalSession")

#### Spring profily

v podstatě se jedná o nějakej config soubor a když vytvoříš novej například application-test.properties a pak nastavíš aby se tyhle properties použily tak se to při startu změní

test je název toho profilu
```
potřeba napsat
spring.profiles.active: test //v tomhle případě
```
lze mít několik aktivních profilů ty další přepisou ten původní (vždycky se používá ten poslední definovanej)

dá se přepsat aktivní profil v command line

u tříd se dá definovat profil u kterejch se to používá (např databáze pro produkci a testování)

@Profile("test") třeba (pokud to není specifikovaný tak se použije defaultní)

#### Garbage collector

Java naštěstí tohle dělá sama o sobě, v podstatě jde o to, že pokud by to Java nedělala tak by se nakupily objekty a došla by paměť </br>
V podstatě to hledá objekty, ke kterejm se už nedá se dostat protože např. už neexistuje pointer na to kde ten objekt je, takže garbage collector jde a smaže ten objekt </br>
Java má víc jak jeden garbage collector ale vždy se používá jen jeden </br>
při běhu programu se provádí tzv mark and sweep kde garbage collector projede paměť a hledá objekty, na který už není žádná reference a ty maže </br>
jsou 2 různý heapy (viz stack and heap) tzv young gen a old gen, v old genu jsou objekty který se používaj dlouhodobě zatímco v young gen jsou krátkodobý objekty

#### Stack and Heap

ve stacku jsou metody, proměnný atd a je to vždy lifo pořadí tzn že pokud tam mám 2 věci tak se musí vždy vzít ta druhá
