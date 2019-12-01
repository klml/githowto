## Git Schulung

Git ist eine *verteilte* __Versionsverwaltung__.

### Versionsverwaltung

* git ist ein __Programm__ (z.B. wie tar) das jede commitede __Version__ einer Datei in ein __repository__ abspeichert:
  * um mit einem __diff__ nur Änderungen zu sehen
  * einen __rollback__ ermöglichen
  * git speichert nur die Änderung, nicht die gesamte Version

* __Versionen__ sind nicht nummmeriert (123) sondern __hash__ (9e1f75....):
  * unabhängig da verteilt
  * abzweige branches
  * forks (sind streng genommen nur branches)
  * können getaged werden

Es ist kein Serverdienst, [kann aber](#verteilt) einer sein.

Alles (config, versionen, branches) wird in einem Directory im Unterordner `.git` gespeichert.


#### git init

Ein directory mit git versionieren:

```
mkdir test && cd test
git init
tree .git
```

Git braucht nur diesen Unterordner `.git`, sogar viel weniger:

```
rm -rf .git/branches/ .git/hooks/ .git/info/ .git/description .git/objects/info/ .git/objects/pack/ .git/refs/tags/ .git/refs/heads/ .git/config

git status

cat .git/*
ref: refs/heads/master
cat: .git/objects: Ist ein Verzeichnis
cat: .git/refs: Ist ein Verzeichnis

tree .git/
.git/
├── HEAD
├── objects
└── refs
```

und jetzt arbeiten wir

```
date > test.md
git status
git add test.md
git status
git commit -m "init"
git status
tree .git/
cat .git/COMMIT_EDITMSG
date >> test.md
git diff
git commit -a -m "zweiter"
git status
tree .git/
git log
```


### Verteilt

git muss nicht nur auf einem System sein, repositories können sich aufteilen (forken) und wieder vereinen (pull & push).
Das geht auch zwischen zwei plain hosts über z.B. ssh.

Oder eben ...

#### git{la|hu}b

Auch ein privates gitlab, gitlab.com oder github.com sind nur ein .git auf einem etwas zentraleren server.
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

### Merken, Aufschreiben, Machen

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

## Weblinks

Bei Problemen oder Ideen hilft die Suchmaschine Deiner Wahl, gute gitlivehacks zum drüberlesen und im Hinterkopf behalten:

* [ohshitgit.com](https://ohshitgit.com)
