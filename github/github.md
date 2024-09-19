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
