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

Interface je abstraktní třída ve který se jen definuje název metody a pak se v další třídě implementuje(příklad uveden nahoře)
