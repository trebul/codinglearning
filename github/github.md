- cherry-pick
```
git cherry-pick master
//applynutí změn všech předchozích commitů do master branche a vytvoření novýho commitu s touhle změnou
//pokud se tam dá ^před branch tak se excludne
git cherry-pick maint ^master //třeba takhle
//dá se taky zvolit kolikátej commit
git cherry-pick master~4 master~2 //tohle je 5 a 3 commit
//drop - commit se dropne
//keep - commit se nechává
```

- squash commit
znamená že se dá vzít několik commitů najednou a udělat z nich jeden
```
//třeba chci sloučit poslední 3 commity
git rebase -i HEAD~3
//použiju pick a pak squash
pick commit1
squash commit2
squash commit3
//když pak chci pushnout
git push --force
```

- amend
umožňuje upravit poslední commit bez potřeby vytvářet nový commit

```
//pokud chci jen upravit commit msg
git commit --amend //tím se otevře editor kde můžu změnit poslední zprávu

//pokud jsem zapomněl do commitu něco přidat
git add <file>
git commit --amend
```

- fetch
získá updaty z nějakýho branche/repo ale člověk pak musí provést spojení (merge)
```
git fetch feature
```

- merge
spojí soubory např pokud mám soubor student.js a při fetchi zjistím že byl student.js změněn tak provedu merge s cílovou větví(případně resolvnu konflikty) a soubor se mi aktualizuje
```
git merge feature
```
- pull
kombinuje fetch a merge dohromady
```
git pull feature
```
- rebase
  slouží k přesunu z jednoho branche na druhý a přepíše historii
  
```
git rebase -i //interaktivní rebase umožňuje upravit commity(třeba je spojit/upravit)
//lze rebasovat na nějaký jiný branch
git rebase feature
```
