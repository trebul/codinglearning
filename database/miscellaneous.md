#### block vs deadlock
block je pokud je resource potřebována 2 procesy a druhá musí počkat na tu první

deadlock je pokud máš 2 procesy a každej drží zdroje toho druhýho

#### ACID

4 vlastnosti každý databáze, týká se to transakcí
- a tomicity

  každá transakce se musí dokončit nebo vrátit do původního stavu (commit/rollback)
- c onistency

databáze by měla zajistit, že jsou data správně za předpokladu že je dodržena konzistence dat
- I solation

zabezpečení ongoing transakcí, tak aby se pustily v pořadí v jakým uživatel spustil
- D urability

pokud nastala chyba během transakce tak tam ty změny musí být (za předpokladu že bylo commitnuto)


#### Typy Izolací
jsou 4 fenomény, které mohou nastat a neměly by když běží více transakcí najednou

- Dirty read

example
mám 2 transakce

A

update product set price = 1000 where productID=1

B

select * from product where productID=1

změny se projeví v transakci B ovšem pokud je transakce A rollbacknuta, transakce B pořád bude mít data, který nakonec zmizely

- Lost update

Nastává když se 2 procesy snaží updatovat stejnej záznam s jinejma hodnotama tak jeden z těch updatů zmizí. </br> Například když A a B budou chtít updatovat productID=1, jeden nastaví price na 100 a druhej na 200 a oba commitnou. Po selectu tam je pouze vidět hodnota podle toho která transakce byla commitnuta později.
- Non-repeatable read

stává se když se transakce snaží číst stejnej záznam 2x ale pokaždý dostane jinou hodnotu

tldr imagine máš zase transakci A a B a transakce a čte data z tabulky product kde ID=1. B pak updatuje tento záznam a commitne. </br> Když pak budu selectit ten samej záznam tak tam budou nový hodnoty
- Phantoms

Nastává, když během transakce jsou přidány nebo odebrány záznamy ze stejné tabulky kterou transakce používají </br>
Například u transakce A čtu z tabulky objednávek a v transakci B insertnu to tabulky objednávek </br>
pokud transakci B commitnu tak pak uvidím nový záznam v transakci A (to je ten phantom)
- Serialization anomaly
mám například transakci A a B </br>
transakce A čte data z objednávek, které mají status X </br>
transakce B čte data z objednávek, které mají status Y </br>
v transakci A pak provedu insert objednávky se statusem Y zatímco v transakci B provedu insert objednávky se statusem X </br>
problém je, že pak zase záleží na tom v jakém pořadí byly transakce commitnuty což porušuje serializaci databáze </br>
pokud bych commitnul A dřív než B tak uvidím novej insert v B a obráceně

###### Jak tyto problémy řešit pomocí isolace

![image](https://github.com/user-attachments/assets/df9692be-776b-4e39-a72f-1367b66bb7c3) </br>
takhle to generally funguje pro všechny databáze ale každá ne každá to implementuje stejně </br>
u MySQL je defaultní repeatable read </br>
u PostreSQL je defaultní read commited

- Repeatable read


- 
