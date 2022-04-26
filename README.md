# Algorithme de Cocke-Younger-Kasami 

## Implementation en Java

En informatique théorique et en théorie des langages, l'algorithme de Cocke-Younger-Kasami (CYK) est un algorithme d'analyse syntaxique pour les grammaires non contextuelles, publié par Itiroo Sakai en 19611,2. Il permet de déterminer si un mot est engendré par une grammaire, et si oui, d'en donner un arbre syntaxique. L'algorithme est nommé d'après les trois personnes qui l'ont redécouvert indépendamment, J. Cocke, dont l'article n'a jamais été publié3, D. H. Younger4 et T. Kasami qui a publié un rapport interne aux US-AirForce5.

L'algorithme opère par analyse ascendante et emploie la programmation dynamique. L'algorithme suppose que la grammaire est en forme normale de Chomsky. Cette restriction n'est pas gênante dans la mesure où toute grammaire non contextuelle admet une grammaire en forme normale de Chomsky équivalente6. Le temps de calcul de cet algorithme est en {\displaystyle O(|m|^{3}\cdot |G|)}{\displaystyle O(|m|^{3}\cdot |G|)}, où {\displaystyle |m|}{\displaystyle |m|} est la longueur du mot {\displaystyle m}m à analyser et {\displaystyle |G|}|G| est la taille de la grammaire.

Source : https://fr.wikipedia.org/wiki/Algorithme_de_Cocke-Younger-Kasami 

## Éxécution 

Il suffit d'éxécuter le CYK.java en lui passant en paramètre le fichier de grammaire ainsi que la phrase à analyser. 

```
java CYK.java <fichier> <phrase>
```

Exemple : 
```
java CYK.java java CYK.java grammaire.txt ab
```

Cette simple grammaire avec le mot ab génère en sortie :
```
$ java CYK.java grammaire.txt ab
Phrase: ab

G = ({a, b}, {S, X, Y}, P, S)

Production :
S -> XY
X -> a
Y -> b

CYK :

+-----+-----+
| a   | b   |
+-----+-----+
| X   | Y   |
+-----+-----+
| S   |
+-----+
```

## Structure du fichier de grammaire

Un fichier de grammaire est structuré ainsi : 

```
S                             -> Symbole de départ
a b                           -> Symboles terminaux
S X Y                         -> Symboles non-terminaux
S XY                          -> toutes les lignes suivante sont des règles de génération
X a
Y b
```

## Exemple issu des TDs

Dans cette parti nous allons utiliser l'algorithme avec des phrases en français et une grammaire plus complex. 

### Le petit garçon lit des livres.

```
$ java CYK.java exercice1.txt le petit garçon lit des livres
Phrase: le petit garçon lit des livres

G = ({le, petit, garçon, lit, des, livres, est, du}, {PHRASE, VERBE, DET, NOMC, ADJ, GADG, GV, GN, VERBETAT, DETPREP, GPREP}, P, PHRASE)

Production :
ADJ -> petit
DET -> le | des
DETPREP -> du
GADG -> ADJNOMC
GN -> DETGADG | DETNOMC | GNGPREP
GPREP -> DETPREPNOMC
GV -> VERBEGN | VERBETATADJ
NOMC -> chat | garçon | livres | lit
PHRASE -> GNGV
VERBE -> lit | livre
VERBETAT -> est

CYK :

+--------------+--------------+--------------+--------------+--------------+--------------+
| le           | petit        | garçon       | lit          | des          | livres       |
+--------------+--------------+--------------+--------------+--------------+--------------+
| DET          | ADJ          | NOMC         | NOMC,VERBE   | DET          | NOMC         |
+--------------+--------------+--------------+--------------+--------------+--------------+
| -            | GADG         | -            | -            | GN           |
+--------------+--------------+--------------+--------------+--------------+
| GN           | -            | -            | GV           |
+--------------+--------------+--------------+--------------+
| -            | -            | -            |
+--------------+--------------+--------------+
| -            | -            |
+--------------+--------------+
| PHRASE       |
+--------------+
```

### Le lit du garçon est petit.

```
$ java CYK.java exercice1.txt le lit du garçon est petit
Phrase: le lit du garçon est petit

G = ({le, petit, garçon, lit, des, livres, est, du}, {PHRASE, VERBE, DET, NOMC, ADJ, GADG, GV, GN, VERBETAT, DETPREP, GPREP}, P, PHRASE)

Production :
ADJ -> petit
DET -> le | des
DETPREP -> du
GADG -> ADJNOMC
GN -> DETGADG | DETNOMC | GNGPREP
GPREP -> DETPREPNOMC
GV -> VERBEGN | VERBETATADJ
NOMC -> chat | garçon | livres | lit
PHRASE -> GNGV
VERBE -> lit | livre
VERBETAT -> est

CYK :

+--------------+--------------+--------------+--------------+--------------+--------------+
| le           | lit          | du           | garçon       | est          | petit        |
+--------------+--------------+--------------+--------------+--------------+--------------+
| DET          | NOMC,VERBE   | DETPREP      | NOMC         | VERBETAT     | ADJ          |
+--------------+--------------+--------------+--------------+--------------+--------------+
| GN           | -            | GPREP        | -            | GV           |
+--------------+--------------+--------------+--------------+--------------+
| -            | -            | -            | -            |
+--------------+--------------+--------------+--------------+
| GN           | -            | -            |
+--------------+--------------+--------------+
| -            | -            |
+--------------+--------------+
| PHRASE       |
+--------------+
```
