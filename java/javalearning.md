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
