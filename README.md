# howto Git

Git ist eine *verteilte* __Versionsverwaltung__ für Datein.
Git wird vor allem in der Softwareentwicklung genutzt, kann aber auch Texte wie Wiki, Bücher oder Blogbeiträge versionieren.
Zur Not auch Bilder und andere Dateien.

> git - the stupid content tracker

aus `man git`


## Versionsverwaltung

* git ist ein __Programm__ (z.B. wie tar) das jede commitede __Version__ einer Datei in ein __repository__ abspeichert:
  * um mit einem __diff__ nur Änderungen zu sehen
  * einen __rollback__ ermöglichen

* die __Versionen__ sind nicht nummmeriert (123) sondern mit einem __hash__ (9e1f75....) identifiziert:
  * unabhängig da verteilt
  * [branches](#branch) können abzweigen
  * [forks](#fork) können unabhängig weiterentwickelt werden
  * oder können __getaged__ werden (`v1.2.3`, `latest`)

Git ist kein Serverdienst, [kann aber](#gitlahub) einer sein.

Alles (config, versionen, branches) wird in einer _lokale Kopie_ des gesamten Repositorys im Unterordner `.git` gespeichert.


### git init

Ein directory zu Demonstrationszwecken mit git versionieren:

```
mkdir test && cd test
git init
tree .git
```

Git braucht nur diesen Unterordner `.git`, sogar viel weniger.
Um das zu zeigen kann zu Demonstrationszwecken recht viel weg löschen:

```
rm -rf .git/branches/ .git/hooks/ .git/info/ .git/description .git/objects/info/ .git/objects/pack/ .git/refs/tags/ .git/refs/heads/ .git/config
```

Mit `git status` sieht man das dieser _lokale Kopie_ immer noch alles kann.
Mit `tree .git/` sieht man nur die Datei `HEAD` und zwei Verzeichnisse.

Das alles muss und sollte man in einem echten git checkout __nicht machen__!



und jetzt arbeiten wir in dieser minimalistischen _lokale Kopie_:

```
date > test.md
git status
git add test.md
git status
git commit -m "init"
git status
date >> test.md
git diff
git commit -a -m "zweiter"
git status
```

Diese nun zwei mal bearbeitete _lokale Kopie_ kann man untersuchen:

* `tree .git/` zeigt ganz viele binary Dateien die den Arbeitsverlauf jeder Verson speichern
* in `cat .git/COMMIT_EDITMSG` steht die letzte commitmessage
* `git log` zeigt die metadaten der letzten commits
* `git diff HEAD~1` den Unterschied von der aktuellen Version zur einer Version zurück


## branch, clone, fork, pull, push, spoon, merge?

### branch

Ein __branch__ (engl. _Zweig_) ist ein Entwicklungsstrang innerhalb eines repositories.

Branches werden nach unterschiedlichen Philosophien gehandhabt:

* *main* (ist default und haben die meisten Projekte)
* versions
* featurebranches
* dev, prod, live, nightly, bugfix

branchens können und sollen wieder __gemerged__ werden:

* features und dev in den main
* aber auch rückwärts main in dev und features!

Je mehr branches, desto mehr muss gemerged werden, was zu mergekonfliten führen kann!

### clone

Ein __clone__ ist eine Kopie eines repositorys, einschließlich aller Versionen und branches.
Im clone, z.B. lokaler Rechner, kann ganz normal weiter commited, gebranched, getagged werden.

Diese Änderungen können dann wieder in das *[remote "origin"]* zurück __gepushed__ werden.
Oder Änderungen anderer clones __gepulled__ werden.

Das geht auch zwischen zwei Verzeichnissen auf einem Rechner oder zwischen zwei plain hosts über ssh oder https.
Das passiert oft über ein "zentrales" repository...

### git{la|hu}b

Auch ein privates gitlab, [gitea](https://gitea.io), gitlab.com oder github.com sind nur ein .git auf einem etwas zentraleren Server.
Plus weboberfläche, plus benefits.

```
git clone https://gitlab.com/klml/lorem.git
cd lorem
cat .git/config
```

etwas codework, ein commit und dann ein __push__:
```
date > ich.md
git add ich.md
git commit -a -m "was anderes"
git log
git push
```

Tage später:
```
git pull # immer zuerst pullen!
date > ich.md
git commit -a -m "was anderes"
git push
```

### fork

Ein __fork__ ist das _unabhängige_ Weiterentwickeln eines clones.
Entweder weil man mit dem Ursprungsprojekt unzufrieden ist, oder weil man seine eigenen Änderungen ausprobieren will.
Forks können und sollen aber auch wieder mit einem pullrequest (github) oder mergerequest (gitlab) in das __upstream__ repository zurück.


## proTips

### stash
Wir arbeiten vor uns hin an:

```
date >> index.md
date >> 2016/kafka.md
sed -i '3 i\lora' index.md
```
Jetzt kommt ein dringender hotfix rein:
```
git stash
git status
```
und danach:
```
git stash pop
```
### git add --patch 
manchmal vergisst man aber den `git stash` oder macht mehrere unabhängige Aufgaben auf einmal in einem repository.
Dann hilft:
```
git add -p
```

## Merken, Aufschreiben, Machen

immer wenn man an einem bestehenden repository zu arbeiten beginnt:
```
git pull
```
sonst kommt man in die mergehölle!

und wenn man grad nicht weiter weiss:
```
git status
```

Und immer alles lesen was einem `git status` sagt!

## Weblinks und Buch

Bei Problemen oder Ideen hilft die Suchmaschine Deiner Wahl, gute gitlivehacks zum drüberlesen und im Hinterkopf behalten:

* häufige git Sackgassen und deren Lösung [ohshitgit.com](https://ohshitgit.com)
* Einführung von den Grundlagen bis zu den meisten Anwendungsfällen: [Sujeevan Vijayakumaran: Versionsverwaltung mit Git](https://www.bookzilla.de/shop/article/40280086/sujeevan_vijayakumaran_versionsverwaltung_mit_git.html) ISBN 9783747500422
