#### VM
installing the entire OS to work on it

#### Docker
installing containers that run 1 thing and can be bundled together

##### What's the difference
- VM: Complete system with OS
- Container: Shares part of your current OS

Hypervisor: Manages access to the HW by several OS at once
OS arent aware of it.
- Type 1 Hypervisor: Runs directly on the real HW
- Type 2 Hypervisor: Runs as an application on top of an existing OS

Hypervisor vs Emulator
Hypervisor makes your code run on the real CPU meanwhile Emulator makes it run on a simulated CPU

VM makes a snapshot of your environment
meanwhile in Docker to containerize a project for example, user has to define all the necessary information such as installing all the necessary application, dependencies etc. in order to make the app run

containers run faster than VM

VM is full isolation of the OS important when u needd simulation on the OS level
Container is more efficient in terms of resource usage and is about setting up the application side where the changes on the OS level are minimal

VM needs to have RAM allocated for each image.
![image](https://github.com/user-attachments/assets/3bd2dc1f-eb18-48ba-bc98-73429a47ffd8)


##### Spring vs Spring Boot
Spring boot builds on top of Spring framework to simplify development process, especially for standalone applications.

##### Spring
tldr robust framework for big projects
- framework that provides a wide range of tools to build java applications.
- users have to manually add dependencies via maven or gradle in pom.xml
- does not provide embedded servers. Typically needs to deploy application to external servers.
- does not have a main method by default as applications are typically deployed to an external server and user has to rely on containers to run/stop the application
- user has to add all the necessary libraries and modules themselves
- does not provide command line interface

#### Spring Boot
tldr simplified Spring with default configs for small apps
- compared to Spring has auto configuration and opinionated setups and is easier to configurate. Embeds some application servers that allows for standalone application development without server setup.
- provides pre configured dependencies that are commonly needed for building applications.
- supports embedded servers == can run application in standalone jar with built in server
- has main method with the @SpringBootApplication annotation that can be run
- provides starter modules and dependencies called Spring Boot Starters that are necessary libraries
- provides command line interface

##### Conclusion
Spring is better for large applications where control is needed over the architecture, configuration and components

Spring boot is better for standalone production-ready applications quickly espeically for smaller services/microservices.


#### AOP (Aspect Oriented Programming)
![image](https://github.com/user-attachments/assets/b851d48a-6c9b-4701-b543-12923775e0fc)
rozdělený do layerů
přes ty layery jsou pak "cross cutting concerns" který se skládaj z transaction managementu, logging, profiling a security (jsou tam i další ale tyhle jsoui nejdůležitější)

Místo třídy se používá Aspect, kterej se zabývá jednou z těch věcí (transaction managentu, loggingu, profilingu a nebo security)

důvod proč to dělat je protože se to rychle dá napsat a snadno se to reusuje + je tam vždycky focus na jeden aspect

dá se je enablovat nebo disablovat

nejpopulárnější knihovny jsou spring a aspectJ s tím že aspectJ funguje se springem

#### OOP principy

jsou 4 základní pilíře
- Abstrakce

hlavním konceptem je ukazovat uživateli jen to co je nezbytné

příklad - pokud mám třeba dálkový ovladač od tv tak uživatel nepotřebuje vědět jak přesně to funguje uvnitř

stačí jen přepínání kanálů a ovládání hlasitosti

- Dědičnost

hlavním konceptem je možnost znovu používat kód, díky tomu se redukuje duplicita a vytváření specializovaných tříd

příklad - pracuju na videohře a chci přidat nepřátele a všichni mají například funkci talk

tak můžu vytvořit třídu nepřítel a ta bude mít funkci talk a pak jen dědím tuhle třídu pro všechny typy nepřátel a případně upravuju co ta funkce vrací

- Polymorfismus

hlavní koncept je možnost určit průběh funkce za běhu programu

lze použít rodičovskou třídu na volání děděných metod

```
//příklad z nepřítele u dědičnosti
Enemy enemy = new Vampire();
```

- Zapouzdření

hlavním konceptem je skyrtí dat a řízený přístup k vlastnostem objektu

používaj se modifikátory přístupu (private)

implementace getterů a setterů - tím se zajistí ochrana vnitřního stavu objektu

#### Microservisy
je to styl, kde aplikace je rozdělena na malé, nezávislé služby na rozdíl od monolitického stylu kde je všechno součástí jednoho projektu

jedná se například o autentikaci apod, kde spolu tyto služby komunikují přes api většinou REST api. Každá mikroslužba je samostatná jednotka, která má svou vlastní databázi.

oproti monolitického stylu, kde je všechno součástí jednoho celku (a tím pádem těžší na úpravu) je jednodušší každou servisu spravovat zvlášť ovšem je pak zase těžší debugovat pokud se něco rozbije.

každá servisa musí jít vyvíjet,testovat a používat samostatně

celkem jsou 3 typy komunikace mezi microservisami

- api
![image](https://github.com/user-attachments/assets/59cdbcb1-1f5d-4940-bc65-6b4daef17ba9)
- message broker
![image](https://github.com/user-attachments/assets/fdb50323-e43f-41f1-820e-6e54e0b56dfe)
- service mesh
![image](https://github.com/user-attachments/assets/296be7cf-eaef-4ac8-8851-5edc3a685688)

jelikož jsou microservisy nezávislé na sobě tak je lze vyvíjet v různých jazycích

##### CI/CD pipeline
jsou 2 možnosti

- monorepo

1 složka pro každou servisu v 1 repozitáři

je to jednodušší pro code management a development

jelikož jsou v 1 repozitáři tak jednodušší rozbít strukturu kde všechno má být nezávislé na sobě takže tam musí být další úpravy aby pipelina detekovala změny které by tohle narušily

- polyrepo

vytvoří se skupina ve které jsou repozitáře pro všechny repa pro tu aplikaci


#### Message Broker
software komponenta, která umožňuje aplikacím vzájemně komunikovat pomocí výměny zpráv. Je to prostředník mezi odesílatelem a příjemcem zpráv, což usnadňuje asynchronní komunikaci a oddělení jednotlivých částí systému.

Hlavní funkce:
- Asynchronní komunikace odesílatel nemusí čekat na odpověď pro příjemce
- ukládání zpráv, lze je dočasně uložit dokud příjemce nepříjme zprávu

Message brokeři jsou například Apache Kafka, RabbitMQ, Amazon SQS apod.

Používají se k asynchronní komunikaci mezi mikroservisy

##### Výhody
- příjemce a odesílatel se nemusí znát
- zprávy jsou uloženy v případě selhání
- dá se jednoduše scalovat
- asynchronní

##### Nevýhody
- složitost
- přidává latenci a zvyšuje režii
- vyžadují správu a údržbu
