při vytváření repozitáře se dává extends nějakej repozitář záleží co člověk používá já tady používám JpaRepository a pak <Název třídy, datovej typ (ovšem nedává se int ale Integer>
```java
public interface TeacherRepository extends JpaRepository<Teacher, Integer> {
}
```

rozdíl mezi integer a int:
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
