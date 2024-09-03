- @Entity značí tabulku v databázi
- @Id značí primární klíč
- @GeneratedValue slouží k automatickému generování idčka
  ```java
   @Id
   @GeneratedValue(strategy = GenerationType.IDENTITY)
   private int teacherid;
  ```
- @RestController na vytváření web service, mělo by to být oddělený tedy budu mít studentController a adresaController
- requesty se značí @GetMapping,@PostMapping etc vždy za tím jsou závorky a v tom se může definovat ten endpoint
pro vytváření relací:
- @OneToOne
- ##### @OneToMany + @JoinColumn(name ='nazev_fk') + vytvořím si prázdný objekt dané třídy kde chci mít ten cizí klíč
```java
 @OneToMany(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
 @JoinColumn(name = "teacher_id")
 private List<Student> students;
  ```
Db login se dává do application.properties nebo v případě hibernate se to dává do hibernate.cfg.xml
```
spring.datasource.url=jdbc:mysql://localhost:3306/dbname
spring.datasource.username=name
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
