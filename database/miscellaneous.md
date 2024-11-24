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
