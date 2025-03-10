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
od javy 8 se dá referencovat metody přes :: místo volání. Je to pro lambda výrazy, musí to být void metoda (nemá return)
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
#### Novinky ve verzi 22
1. nově jde používat "unnamed" proměnné

nejde je volat a nic s nima dělat

use case - třeba dát do catche pokud nepotřebuju vrátit tu danou chybu
```
Person _ = new Person(); //tohle je teď valid nebo
Person  = new Person();

catch(Exception _) {}

List<ječná> debilci = new List.of(new Zak(), new Zak());
debilci.forEach(_ -> System.out.println("debílek");
```

2. lze v command lině volat multiprogramový soubory

> basically týpek řekl že se dá spustit v commandlině soubor
```
java soubor.java

ovšem pokud v soubor.java referencuju jinej soubor tak by to nefungovalo
což teď ale funguje
```

3. String templating - tohle je jen preview
```
//example jak to funguje teď
String name = "Karel";
int age = 26;

String msg = "Ahoj já jsem " + name + " mám rád rajčata a je mi " + age;

//String templating
String msg = STR."Ahoj já jsem \{name} mám rád rajčata a je mi \{age}";
```
#### lambda výrazy
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
#### Multidimensionální pole
```
int [][] array = {{1,3},
		  {11,22},
		  {33,44}};
```
jedná se o matici která obsahuje hodnoty v řádcích a sloupcích

pole se taky dá inicializovat i bez definování hodnot
```
int[][] pole = new int[3][3];
```
![image](https://github.com/user-attachments/assets/f076015d-e4f9-4931-a527-0ab72c871562)


takhle pak vypadá matice o velikosti 3x3, nicméně není nutné definovat velikost vnořených polí

pro accessování hodnot se pak používá například
```
pole[0][1] = 5;
```
takže pro procházení multidimensionálního pole je pak potřeba používat dvojitý for loop
```
for(int o = 0;o<pole.length;o++){
for(int j = 0;j<pole.length;j++) {
system.out.println(pole[o][j] + " ");
}
}
```

tohle všechno platí samozřejmě i pro pole, které mají více dimenzí nemusí to nutně být jen 2 dimenze, jen se tam přidá další [] a je to
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
- @ControllerAdvice
slouží na handlování vyjímek pokud máš třeba několik controllerů který každej dělá něco jinýho, takže tohle prochází všechny controllery a hledá jakýkoliv chyby. </br>
tyto chyby pak předá metodám které mají anotaci ExceptionHandler

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
každá classa sama o sobě extendí speciální java classu. Ta se jmenuje Object a je v java.lang package

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

stack se používá na ukládání všech volání funkcí a lolálních proměnných </br>
ve stacku je něco čemu se říká stack-frame a to je nějaká alokovaná paměť (určuje OS) ve který jsou nějaký metody a lokální proměnný (třeba pokud mám metodu na sečtení 2 čísel tak mám obě čísla v tom stack-framu) </br>
nejdřív executuje stack-frame kterej byl zavolanej jako poslední </br>
pokud daná metoda dojede tak se to z toho stacku smaže a jde se na další stack-frame </br>
může se stát že nám ve stacku dojde paměť a pak dojde k přetečení stacku (stack overflow) nemůže se paměť alokovaná stacku zvětšit pokud program už běží</br>

oproti tomu heap nemá předem alokovanou paměť (size), takže se říká že to je free pool of memory

heap je taky zvanej jako dynamická paměť
jsou tam uložený všechny objekty a jejich instance proměnný 

heap je rozdělenej do několika oblastí podle typu garbage collectora. V C se tohle musí managovat zatímco v Javě se to managuje samo
##### je důležitý že stack a heap jsou rozdílný podle toho o jakým jazyce se bavíme (tj C a Java fungujou úplně jinak)

#### Způsoby injectování dependencies
dependency injection je v podstatě to, že třeba pro vytvoření objektu škola potřebuju objekt učitel apod, jinými slovy je škola závislá na učitelích

jsou 3 základní typy

- Constructor injection

```
    public Schedule(int id, String schoolYear, @NotNull List<Subject> subject, @NotNull Classroom classroom) {
        this.id = id;
        this.schoolYear = schoolYear;
        Subject = subject;
        this.classroom = classroom;
    }
```
jelikož v tom konstruktoru mám jinou třídu tak to tam do toho injectím dependency na třídu subject a třídu classroom</br>
takže v tenhle moment nemůžu vytvořit schedule bez třídy a bez předmětu
- Property injection

```
    public void setClassroom(Classroom classroom) {
        this.classroom = classroom;
    }
```
v podstatě injectuju dependency přes setter na tu danou classu, je potřeba použít @Autowired
- Method injection

```
    public void displaySchedule(@AutoWired List<Subject> subjects,@Autowired Classroom classroom) {
        System.out.println("Subjects: " + subjects);
        System.out.println("Classroom: " + classroom);
    }
```
používá se to v případě kdy je potřeba daná classa pouze v jednom místě


#### SOLID

- Single responsibility
to znamená že každá classa by měla mít pouze 1 zodpovědnost</br>
např v classe pro lidi nebudu mít zvířata

- Open for extension, closed for modification
to znamená že by třídy měly mít možnost něco do nich přidat ale ne je upravovat

- Liskov substitution
to znamená, že pokud třída A je typem třídy B tak by mělo bejt možný nahradit B s A aniž by se nějak rozbila funkcionalita</br>
třeba pokud mám automat na kopečkovou zmrzlinu a najednou budu chtít přidat kelímkovou tak to přestane fungovat protože nemá kelímky, takže to porušuje tohle pravidlo a musí se to přepsat

- Interface Segregation
to znamená, že by se nějaký velký interfacy měly rozdělit na několik menších. Tím se classy, který implementujou ty interfacy nemusí zajímat o metody, který nepoužívaj

- Dependency Inversion
to znamená, že by vyšší vrsty modulu neměly záviset na nižších vrstvách, třeba @Controller by neměl bejt rovnou závislej na repozitáři. </br>

#### Abstraktní třídy
je to třída ze který nejde dělat instance ale dá se z ní extendid

dá se taky udělat abstraktní metody který fungujou podobně a dělá se to aby to bylo uspořádaný

nicméně metody v abstraktní třídě nemusej bejt abstraktní

##### rozdíl oproti interface
rozdíl je v tom, že v interface se předpokládá že všechny metody jsou abstraktní a používá se implements a můžeš implementovat X interfaců ale jen 1 abstraktní třídu a jde dělat oboje

proměnný v interface jsou public static

#### The Record type
součástí javy od JDK 16, replacement pro POJO a je to speciální třída která obsahuje data, který se nikdy nebudou měnit <br/>
obsahuje pouze fundamentální metody jako konstruktory a gettery

```
public record student(String id, String name, String lastname, int age) {

}

```
pro každej parametr v hlavičce java generuje proměnnou s generovaným typem proměnný a každá proměnná je `private final` taky se tomu říká component field(?) <br/>
mimo jiné taky java generuje accessory pro každou proměnnou ale nemá to prefixy jako třeba get <br/>
takže je to prostě id() pokud chci getnout idčko objektu. Nedá se to setovat jelikož je to fixed hodnota

#### StringBuilder
na rozdíl od Stringu to nevytváří novej objekt v heapu jako String
![image](https://github.com/user-attachments/assets/be18f1e2-e58e-49e1-ab2f-8a3cc83e00a9) <br/>
StringBuilder pouze vrací referenci sám na sebe

má base kapacitu 16 a pokud to nadefinuju s nějakým stringem tak je to 16 + délka toho stringu
```
StringBuffer buffer = new StringBuffer(); //takže tohle je 16
StringBuffer sb = new StringBuffer("lubert");//tohle je 22
sb.append(" martin"); //přidá string
```

#### Spring Cloud

Spring Cloud je sada nástrojů a knihoven, které se používají ve vývoji distribuovaných systémů a mikroslužeb.

skládá se z:
- Spring Cloud Config - správa konfigurace pro mikroservices
- Spring Cloud Netflix - integrace s nástroji netflix OSS
- Spring CLoud Stream - zpracování zpráv a integrace s message brokery
- Spring Cloud Bus - propojení aplikací a šíření aktualizací konfigurace
- Spring Cloud Security - zabezepčení mikroslužeb a správa autentikace/authorizace

##### Výhody
- lze používat pouze potřebné komponenty
- snadná integrace s dalšími nástroji Spring
- optimalizováno pro provoz v cloudu

##### Nevýhody 
- nevhodný pro malé projekty jelikož je navržen na velké robustní projekty
- výkon: zvyšují režii aplikace
- složitost konfigurace
- závislost na externích službách
