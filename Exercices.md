# Expressions régulières : Exercices
## Exercice 1 : Cotes
Dans la liste de cotes fournies 01_corpus_auteur.txt, trouver toutes celles qui correspondent à un auteur dont le nom commence par L. 

Les cotes d’œuvre suivent la forme **84.xxx suivis des trois premières lettres du nom de l’auteur**.
### Chaîne à traiter
```
84.1 HUG
84.27 ZOL
84.302 CAM
84.5 DUM
84.78 GID
84.9 L
84.06 RIM
84.203 VIG
84.4 BEC
84.317 REN
84.81 DUA
84.02 MAU
84.903 COH
84.3 BER
84.79 ROU
84.63 FLA
84.11 GRA
84.100 PER
84.6 VAL
84.005 QUE
84.7 JON
84.83 NOI
84.321 LAC
84.08 TRE
84.602 ARD
84.12 GON
84.934 DEL
84.42 LEM
84.314 MON
84.023 FAB
84.53 LAU
84.107 ROC
84.015 NEY
84.64 BAR
84.91 MIC
84.71 BOI
84.007 GUI
84.808 LED
84.39 HOU
84.304 GAR
846.52 LUC
```
### Correction
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

- Expression : `^84\.\d\d\d L`
- Explication :
  - `^84` : la ligne doit commencer par la chaîne 84. `^` désigne le début de la ligne
  - `^84\.` : la chaîne '84' doit être suivie par un point. Le point est un métacaractère en regex. Il est donc nécessaire de "l'échapper" avec une barre oblique inversée (`\`). Par exemple, l'expression `84.` retrouve '84.' et '846'.
  - `^84\.\d\d\d` : le point doit être suivi par 3 chiffres. `\d`.
  - `^84\.\d\d\d L` : les 3 chiffres sont ensuite suivis par un espace (` `. On aurait aussi pu utiliser `\s`) puis par la lettre L majuscule `L`
</details>

## Exercice 2 : Extraction de mots dans un texte\b
### Chaîne à traiter
```
Le chat est un animal de compagnie très populaire. Les gens adorent les chats pour leur indépendance et leur affection. Saviez-vous que les chatouilles peuvent rendre un chat très joueur ? Hier, j'ai vu un chat sauter par la chatière pour entrer dans la maison.

En regardant un ballet, j'ai remarqué que les danseurs exécutaient un entrechat avec grâce. Un ami a un chat-siamois qui est particulièrement agile. Il adore jouer avec une balle de laine. Une autre amie a un doberman qui a mangé son chat.
Ce chat était vraiment unique.
Mes amis adorent les animaux. Je suis nul en amitié. 
Chat
```
### Identifier les mots 'danseurs' et 'maison' en une seule expression
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

- Expression : `(danseurs|maison)`
- Explication : 
    - `\$` Le dollar est un métacaractère pour les expressions régulière, il est donc nécessaire de *l'échapper* en utilisant la barre oblique inversée (`\`)
    - `[..]` représente une classe de caractère. Cela correspond à un caractère parmi ceux entre crochets.
</details>

### Identifier les mots 'ami', 'amis' et 'amie'. Ignorer 'amitié'
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

#### Première solution
- Expression : `ami.? `
- Explication : retourne "ami " "amie " "amis "
    - `.?` Indique que "ami' peut être suivi par n'importe quel caractère présent 0 ou 1 fois
    - ` ` On rajoute un espace pour être sûr de ne pas capturé "amit"
</details>

<details>

<summary>Solution plus complexe </summary>

#### Meilleure solution
- Expression : `ami.?(?= )`
- Explication : retourne "ami" "amie" "amis"
    - `(?= )` Est une assertion positive avant. Elle indique de ne pas prendre l'espace dans la capture.
</details>

### Rechercher le mot chat
On cherche le mot chat en excluant les mots pour lesquels chat n'est qu'une partie du mot (ex : chatouilles)
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

- Expression : `\b[Cc]hat\b`
- Explication : retourne "ami " "amie " "amis "
    - correspond au début ou à la fin d'un mot (inclus la fin d'une ligne)

</details>

## Exercice 3 : Cotes Dewey - Identifier les exemplaires classés dans la section  qui traite d'histoire et géographie
### Cas simples 
#### Chaînes à tester
```
022 ALE
900 FRA
989.506 7 ABDA B
891.992 TOT O
910.94 SCHO P
9 AVE
919.8 TRU
9ART
```

#### Correction
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

1. On recherche une chaîne qui commence par 9 suivie par 2 caractères :
    - Expression : `9..`
    - Explication : `.` désigne *n'importe quel caractère* sauf un retour à la ligne (`\b`)
    - Résultat : ❌ J'obtiens des correspondances sur des cotes qui commence par 9 et est suivie par des lettres ou des espaces  : *9ART*, *9 AV*
2. On recherche une chaîne qui commence par 9 suivie par 2 chiffres :
    - Expression: `9\d\d`
    - Explication : `\d` est un *token* ou raccourci de classe. Il correspond à tout chiffre compris en 0 et 9  
    - Résultat : ❌ J'obtiens une correspondance sur une cotes n'étant aps dans le domaine mais ayant une subdivision qui correspond au motif :  *891.992 TOT O*
3. On recherche une chaîne en début de ligne qui commence par 9 suivie par 2 chiffres
   - Expression : `^9\d\d`
   - Explication : `^` désigne le début d'une ligne
   - Résultat : ✔
   - Expressions alternatives :
     - `^9\d\d.*` --> Sélectionne la ligne
     - `^9\d{2}`  --> Utilise un qunatificateurs
     - `^9[0-9]{2}`  --> Utilise une classe de caractères
</details>

### Cas plus complexes : des cotes magasins sont présentes dans léchantillon 
#### Chaînes à tester
```
022 ALE
900 FRA
989.506 7 ABDA B
891.992 TOT O
910.94 SCHO P
9 AVE
919.8 TRU
9ART
958
998566
958156
```
Si on applique la syntaxe validée lors de l'exercice précédant, on obtient une correspondance sur les cotes *998566* et *958156* qui ne sont pas des indices dewey. Comment limiter la recherche sur les seules cotes en Dewey ?
#### Correction
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

Dans nos cotes libre accès, l'indice Dewey est toujours composé de 3 chiffres suivi d'un point s'il ya une subdivsion ou d'un espace :
1. Rechercher les indices immédiatement suivis par un espace :
    - Expression : `^9\d\d `
    - Explication : On peut simmplement désqigner un espace en tapant l'espace dans l'expression. ⚠ Il est aussi possible d'ultiliser le  token `\s` pour rechercher un espace. Dans notre cas ce n'est pertinent car ce token recherche aussi les tablulations (`\t`) et les retour à la ligne. Dans notre cas il retourne la chaîne *858*. 
2. Rechercher les indices immédiatement suivis par un point :
    - Expression : `^9\d\d\.`
    - Explication : le point est un métacarctères pour les expressions régulière, il est donc nécessaire de *l'échapper* en utilisant la barre oblique inversée (`\`).
3. Rechercher les indices immédiatement suivis par un espace :
    - Expression : `^9\d\d[ \.]`
    - Explication : `[..]` représente une classe de caractère. Cela correspond à un caractère parmi ceux entre crochets.
</details>

### Cas plus complexes : certaines cotes libres accès sont préfixées par des lettres 
#### Chaînes à tester
```
022 ALE
900 FRA
989.506 7 ABDA B
F901 TRU
891.992 TOT O
910.94 SCHO P
9 AVE
REC 989.12 TRI
919.8 TRU
9ART
958
998566
958156
```
Ma chaîne peux ou ne pas commencer par une ou plusieurs lettre suivi par un 9 lui même immédiatement suivi par 3 caractères numériques. Un espace séparées mon préfixe de mon indice Dewey.
#### Correction
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>


1. Rechercher les indices préfixés par une lettre :
    - Expression : `^\w*9\d\d[ \.]`
    - Explication : `\w` désigne *n'importe quel lettre accentuée* présent 0 ou pluiseurs fois (`*`)
    - Résultat : ❌ Ne trouve pas *REC 989.12 TRI* (Cas ou mon préfixe est séparé de mon indice par un espace)
2. Rechercher les indices préfixés par une lettre qui peut être suivies par un espace :
    - Expression : `^[\w\s]*9\d\d[ \.]`
    - Explication : `[..]` représente une classe de caractère. Cela correspond à un caractère parmi ceux entre crochets.
    - Résultat :white_check_mark:
</details>


## Exercice 4 : Sélectionner des ppn valides
Un PPN, contient 9 caractère 8 chiffres suivis par une clef qui peut un chiffre ou un x minuscule ou majuscule. Il peut être préfixé 
#### Chaînes à tester
```
(PPN)1887143088
(PPN)2a9032126
(PPN)254475183
(PPN)254475183 (valide)
(PPN)18625430X
ELC931030488922
(PPN)233849424
(PPN)23419852
(PPN)26474873z
(PPN)188428828
18625430X
(Oclc)178628921
```
#### Correction
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

1. Rechercher une séquence de 8 chiffres immédiatement suivie par un chiffre ou un x :
    - Expression : `\d{8}[\dxX]`
    - Explication : 
      - `\d` désigne *n'importe quel chiffre*
      - `\d{8}` désigne une séquence de 8 chiffres. `{}` permet de quantifier précisement le motif précédent
      - `[\dxX]` désigne un chiffre ou x minuscule ou majuscule
    - Résultat : ❌  J'obtiens une correspondance sur unidenfiant Préfixé par OCLC 
2. Ajouter un test sur le préfixe de l'identifiant  :
    - Expression : `\(PPN\)\d{8}[\dxX]`
    - Explication : `\(` On recherche la chaîne littérale *(PPN)*. Les prenthèses sont des éléments de synatxe des expressions régulières il est donc nécessaire de les échapper par une barre oblique inversée (`\`) 
    -  Résultat : ❌ Exclu *18625430X* qui est un PPN non préfixé
3. Modifier le test sur le préfixe pour prendre en compte l'absence de ce dernier : on recherche une chaine qui commence par PPN ou par un identifiant valide 
    - Expression : `(^|\(PPN\))\d{8}[\dxX]`
    - Explication : 
        - `()` permet de traiter plusieurs caractères comme une seule unité
        - `|` OU logique
        - `(^|\(PPN\))` signifie que la chaîne doit soit commencer au début de la ligne, soit être précédée par (PPN)
    - Alternative :`^(\(PPN\))?\d{8}[\dxX]` Le début de ligne (`^`) est suivi par 0 ou une fois (`?`) la chaîne *(PPN)*
    -  Résultat : ❌ Fait correspondre des PPN avec plus de 9 caractères *(PPN)1887143088* qui est un PPN non préfixé
3. Prendre en compte la fin de la chaine recherché
    - Expression : `^(\(PPN\))?\d{8}[\dxX]\b`
    - Explication : `\b`  correspond au début ou à la fin d'un mot (inclus la fin d'une ligne)
    - Résultat : ✔

</details>

## Exercice 5 : Valider une liste d'adresse emails
Une adresse mail valide est constituée de la manière suivante "login@nomdedomaine.extension". 
### Chaînes à traiter
```
mairieaullene@wanadoo.fr
mairieazilone@orange.fr
mairieazzana@orange.fr
mairiebalogna@orange.fr
mairiedebastelica@wanadoo.fr
mairiebastelicaccia@wanadoo.fr
mairiebelvederecampomoro@wanadoo.fr
mairiebilia@wanadoo.fr
bocognanomairie@orange.fr
mairiedecalcatoggio@wanadoo.fr
mairiecampowanadoofr
mairiecannelle@orange.fr
contactmairiecarbini@orange.fr
mairiedecarbuccia@orange.fr
cardotorgia@orange.fr
mairie@cargese.corsica
communedecargiaca@orange.fr
casaglione@orange.fr
contact@mairiecasalabriva.com
mairiecauro@wanadoo.fr
mairieciamannacce@wanadoo.fr
mairiedecoggia@orange.fr
mairiecognocoli@wanadoo.fr
mairieconca@wanadoofr
mairiecorrano@wanadoo.fr
mairiecotichiavari@orange.fr
@cozzano.corsica
mairiecristinacce@orange.fr
communecuttoli@wanadoo.fr
mairieeccicasuarella@wanadoo.fr
mairieevisa@orange.fr
accueil@mairiedefigari.corsica
communedefoce@orange.fr
communedeforciolo@orange.fr
mairiedefozzano@orange.fr
mairiedefrasseto@wanadoo.fr
mairiegiuncheto@orange.fr
mairiegranace@orange.fr
mairiegrossa@orange.fr
mairievillage@mairiegpp.corsica
communedeguagno@orange.fr
communedeguarguale@hotmail.fr
mairieguiteralesbains@orange.fr
mairielecci@wanadoo.fr
communedeletia@wanadoo.fr
mairie@levie.fr
mairielopigna@orange.fr
communeloretoditallano@orange.fr
mairiemarignana@orange.fr
mairiemela@wanadoo.fr
```
### Correction
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

- Correction : `^\w+@\w+\.\w+$`
- Explication : 
  - `^\w+@` Cherche depuis le début de la ligne (`^`), une chaîne de un ou plusieurs (`+`) carcatères alphabétique (`@`) jusqu' à l'arobase
  - `^\w+@\w+\.` l'arobase est suivie par une ou plusieurs lettres (`\w+`) jusqu'un point (`\.`)
  - `^\w+@\w+\.\w+$` le point est suivi par une ou plusieurs lettres (`\w+`) jusqu'à la fin de la ligne (`$`)

</details>

## Exercice 06 : Nettoyage de chaînes de caractères
Supprimer tous les préfixes « monsieur » / « madame » en début de ligne.
### Chaînes à traiter
```
Mr  Carlo SCHMID, Vice-Président du Bundestag, professeur de Sciences politiques de l'Université de Francfort
Mr LUO-DAGANG, Professeur de littérature française à l'Université de Pékin.
Mr Mario Vargas Llosa, écrivain.
Mr Christopher HILL, Professeur d'histoire moderne à Balliol College, Oxford
Monsieur Paul LECLERC, Président et directeur de la Public Library (New-York).
Mme Sandra di MAJO, Professeur à l'école Normale de Pise
Mme Rita SÜSSMUTH, Présidente du Bundestag
Mr Imre Kertész, écrivain, prix Nobel de littérature 2002
Mr William GOLDING, Homme de lettres britannique
Mr Randolph QUIRK, Professeur de langue et littérature anglaises à l'Université de Londres
Mr Umberto Eco, Professeur à l'Université de Bologne
Mr Candido Mendes, écrivain
Mr John Montague, poète.
Mr Elmar MITTLER, Directeur de la bibliothèque universitaire de Göttingen; Président de la Ligue des bibliothèques européennes de recherche (LIBER)
Mme Margaret Atwood, écrivain
Mr Peter Brook, metteur en scène
Mr Robert Wilson - metteur en scène et artiste visuel
Mr Siegfried LIENHARD, Professeur à l'Université de Stockholm
Mr Árpád GÖNCZ, Président de la République de Hongrie
Madame Margit FRENK, Professeur émérite de l'Université autonome de Mexico
Mr Gerardo MAROTTA, Président de L'istituto italiano per gli Studi Filosofici (Italie).
Mr Derek LAW, Directeur du domaine de l'informatique stratégique de la bibliothèque de l'Université de Strathclyde ; membre fondateur de l'Association européenne des bibliothèques médicales
Mr Giorgio Strehler, Homme de théâtre italien
Mr Mario SOARES, Président de la République du Portugal, conjoint de Madame Maria Barroso.
Mr Abbas Kiarostami, cinéaste
Mr Ravan MAC DAVID, Professeur de linguistique à l'Université de Chicago
Mr Seamus HEANEY, Poète irlandais, Prix Nobel de Littérature 1995, conjoint de Madame Marie Devlin
Mr Francesco GABRIELI, Professeur à l'Université de Rome (Institut d'Etudes Islamiques)
Mr David DAICHES, Professeur à l'Université de Sussex,
Mr Luigi de NARDIS, Professeur de l'Université de La Sapienza de Rome
Mr Bertil MALMBERG, Professeur de linguistique à l'Université de Lund, membre de l'Académie des Sciences Humaines de Suède.
Mr Istvàn SÖTER, Directeur de l'Institut d'Études Littéraires de l'Académie des Sciences de Hongrie.
Mr Tullio de Mauro, linguiste, ancien ministre
Mr Dario Fo, homme de théâtre, prix Nobel de littérature 1997
Mme Michelle Bachelet,  ancienne présidente du Chili, Secrétaire générale adjointe de "ONU - Femmes"
Mr Wim WENDERS, Réalisateur cinématographique et critique
Mr René RICHTERICH, Professeur à l'Université de Lausanne
Mr Bela KOPECZI, Secrétaire général de l'Académie des Sciences de Hongrie, directeur des Études françaises à l'Université de Budapest.
Mr Jorge Amado, Homme de lettres brésilien
```
### Correction :
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

- Expression : 
  - Rechercher : `^\w+`
  - Remplacer par : ``
- Explication : 
    - `^\w+\b` Capture le premier mot de la ligne
      - `^` Début de la ligne
      - `\w+` Sèrie de une plusieurs lettre. S'arrête au premier espace recncontré 
</details>


## Exercice 7 : Travail sur la langue du document zone 101
#### Chaînes à tester
```
=101  0\$afre$2639-2
=101  0\$aspa$2639-2
=101  0\$aeng$2639-2
=101  1\$afre$ceng$2639-2
=101  1\$afre$cspa$2639-2
=101  0\$afre$dfre$deng$2639-2
=101  0\$afre$dfre$deng$2639-2
=101  1\$afre$ceng$2639-2
```
### Trouver les documents en langue anglaise ou traduit de l'anglais: 101\$a = eng ou 101\$c = eng
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

Rechercher une chaîne qui commence par $ suivie de a ou c puis de la chaîne eng  :
- Expression : `\$[ac]eng`
- Explication : 
    - `\$` le dollars est un métacarctères pour les expressions régulière, il est donc nécessaire de *l'échapper* en utilisant la barre oblique inversée (`\`)
    - `[..]` représente une classe de caractère. Cela correspond à un caractère parmi ceux entre crochets.
</details>

### Trouver les documents écrits en langue anglaise ou espganole ou traduit de l'anglais ou de l'espagnol: 101\$a = eng, 101\$c = eng101\$a = spa ou 101\$c = spa
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

Rechercher une chaîne qui commence par $ suivie de a ou c puis de la chaîne eng ou de la chaine spa :
- Expression : `\$[ac](eng|spa)`
- Explication : 
        - `()` permet de traiter plusieurs caractères comme une seule unité
        - `|` OU logique
</details>

### Trouver les documents qui ne sont pas écrits en langue française
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

Rechercher une chaîne qui commence par $a qui n'est pas suivie par la chaîne fre  :
- Expression : `\$a(?!fre)`
- Explication : `(?!fre)` C'est une assertion négative (negative lookahead). Elle signifie que la chaîne fre ne doit pas suivre immédiatement la chaîne correspondante jusqu'à ce point.
</details>

## Exercice 8 : Extraire des données de localisations

Ces localisations contiennent de nombreuses informations dont nous voudrions extraire une liste de numéro de niveau et d’aile (nord ou sud)

### Chaînes à traiter
```
Niveau 2 - Aménagement du territoire, urbanisme, jardins - Salle Nord  (Mur G)
Niveau 2 - Géographie - Salle Nord  (Rayons 31-32)
Niveau 2 - Géographie pays - Salle Nord  (Rayons 33-34)
Niveau 2 - Géographie thématiques - Salle Nord (Mur G)
Niveau 2 - Grec moderne - Salle Sud (Rayon 5)
Niveau 2 - Histoire ancienne - Salle Sud (Rayons 11-15)
Niveau 2 - Histoire et géographie manuels - Salle Sud (Mur D)
Niveau 2 - Histoire généralités - Salle Sud (Mur D)
Niveau 2 - Revues histoire - Salle Nord
Niveau 2 - Revues langues anciennes - Salle Sud (Rayon 1)
Niveau 2 - Revues religions - Salle Sud (Rayon 6)
Niveau 2 - Sources historiques et religieuses - Salle Sud (Murs B et C)
Niveau 4 -  700  Histoire de l'art - Salle Nord (Rayons 26-27)
Niveau 4 -  730 Art Sculpture/Arts plastiques - Salle Nord (Mur F)
Niveau 4 -  740 Art Dessin/Design/Arts décoratifs - Salle Nord (Mur G)
Niveau 4 -  760 Arts graphiques/Gravure - Salle Nord (Mur G)
Niveau 4  - Théâtre, danse - Salle Sud (Rayons 15-16)
Niveau 4 - 709.04 - 709.05 Art contemporain - Salle Nord (Rayons 31-34)
Niveau 4 - 709.2-.9 Art par pays - Salle Nord (Rayons 34-35)
Niveau 4 - 720 Architecture - Salle Nord (Rayons 35-36)
Niveau 4 - 750 Peinture - Salle Nord (Mur G)
Niveau 4 - Cinéma - Salle Sud (Rayons 17-19)Niveau 4 - DVD documentaires CADIST - Salle Sud (espace central)
Niveau 4 - DVD fictions - Salle Sud (espace central)
Niveau 4 - DVD fictions Amérique latine et Afrique lusophone - Salle Sud (espace central)
Niveau 4 - Gestion des organisations - Salle Sud (Mur B)
Niveau 4 - Musique - Salle Sud (Rayons 12-14)
Niveau 4 - Philosophie (autre) - Salle Nord (Mur E)
Niveau 4 - Philosophie (généralités) - Salle Nord (Rayons 20-21)
Niveau 4 - Sciences de l'éducation - Salle Sud (Murs A et B)
Niveau 4 - Sciences de l'information - Salle Sud (Rayons 3-6)
Niveau 4 - Vie de l'étudiant, concours - Salle Sud (Rayons 1-2)
Niveau 6 - Bandes dessinées Bacs - Salle Nord (Espace BD bacs)
Niveau 6 - Français langue étrangère - Salle Sud (Rayons 22-23)
Niveau 6 - Illustrés jeunesse - Salle Nord (Espace BD)
Niveau 6 - Langue allemande - Salle Sud (Rayons 24-27)
Niveau 6 - Langue anglaise - Salle Sud (Rayons 24-27)
Niveau 6 - Langue arabe et autres langues orientales - Salle Sud (Rayons 24-27)
Niveau 6 - Langue espagnole - Salle Sud (Rayons 24-27)
Niveau 6 - Langue française - Salle Sud (Rayons 19-21)
Niveau 6 - Langue portugaise - Salle Sud (Rayons 24-27)
Niveau 6 - Littérature française - Salle Sud (Rayons 2-16)
Niveau 6 - Littérature francophone - Salle Sud (Murs A-C)
Niveau 6 - Littérature générale et comparée - Salle Nord (Rayons 52-57)
Niveau 6 - Littérature italienne et roumaine - Salle Nord (Rayons 69-70)
Niveau 6 - Littérature slave - Salle Nord (Rayons 74-75)
Niveau 6 - Littérature suédoise - Salle Nord (Rayon 68)
Niveau 6 - Méthodes de langues - Salle Sud (espace central)
Niveau 6 - Revues Amérique latine - Salle Nord (Rayon 51)
Niveau 6 - Revues Anglais - Salle Nord (Rayon 51)
Niveau 6 - Revues Espagnol et Portugais - Salle Nord (Rayon 51)
Niveau 6 - Revues langues slaves - Salle Nord (Rayon 51)
Niveau 6 - Revues linguistique française et romane - Salle Sud (Rayon 17)
Niveau 6 - Revues littérature générale - Salle Nord (Rayon 50-51)
Niveau 6 - Revues Moyen-Age - Salle Sud (Mur D)
```
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

- Correction : 
  - Rechercher : `(Niveau \d+).*(Salle \w+).*`
  - Remplacer par : `\$1\t$2`
- Explication :
  - `(Niveau \d+)`On capture `()` le mot niveau et le ou les chiffres qui suivent. Comme il s'agit du premier élément capturé on l'appelle avec `$1` dans la fonction remplacer
  - `.*` On consomme tous les cacractères qui suivent
  - `(Salle \w+)` Jusqu'au mot Salle que l'on capture avec le mot suivant ` \w+`. On utilise `$2` pour appeler la chaîne capturée
  -  `.*` On consomme tous les cacractères qui suivent
</details>

## Exercice 9 : Extraire et formater les dates de la zone 100
Nous voudrions extraire : 
 - la date 1 pour les monographies (100 pos.8 = d,e,f,g,h,i,j,k)
 - la date 1 suivie par un - pour les périodiques en cours (100 pos.8 = a)
 - la date 1 et la date 2 séparées par un - pour les ressources continue morte (100 pos.8 = b)
 - date 1 - ? pour les péridiques dont la situation est inconnue (100 pos.8 = c)  
<details>
<summary>Rappel : Type de date position 8 de la zone 100</summary>
  - a = ressource continue en cours
  - b = ressource continue morte
  - c = ressource continue dont la situation est inconnue
  - d = monographie complète à la publication ou publiée dans une année civile
  - e = reproduction
  - f = monographie dont la date de publication est incertaine
  - g = monographie dont la publication s’étend sur plus d’une année
  - h = monographie ayant à la fois une date de publication et une date de copyright ou de privilège
  - i = monographie ayant à la fois une date d’édition ou de diffusion et une date de production
  - j = monographie ayant une date de publication précise
  - k = monographie ayant à la fois une date de publication et une date d’impression
  - l = dates extrêmes d’un recueil1
  - u = date(s) de publication inconnue(s)
</details>

### Chaînes à traiter
```
=100  \\$a19870924b19811990u  y0frey50      ba
=100  \\$a19940705b19942005u  y0frey50      ba
=100  \\$a19880406b19761980u  y0frey50      ba
=100  \\$a19910816h19791973k  y0frey50      ba
=100  \\$a19991201d1682    k  y0frey50      ba
=100  \\$a19990313d1978    k  y0frey50      ba
=100  \\$a19980721c1977    e  y0frey50      ba
=100  \\$a19950403a19949999k  y0frey50      ba
=100  \\$a19830811g19821983k  y0frey50      ba
=100  \\$a20000407a19989999u  y0frey50      ba
=100  \\$a20000407b19992005u  y0frey50      ba
```

### Corrections :
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

#### 9.1 Extraire la date 1 pour les monographies (100 pos.8 = d,e,f,g,h,i,j,k)
- Expression :
    - Rechercher : `^.*?\$a.{8}[d-k](.{4}).*`
    - Remplacer par : `$1`
- Explication : 
    - `^.*?\$a` La chaîne de données codées commence à partir du $a. On part du début de la ligne (`^`) et on sélectionne n'importe quel caractère (`.`) présent 0 ou plusieurs fois (`*`). On arrète ce motif jusqu'à la premire occurence (`?`) du $a (\\$a). Le $ est échappé
    - `.{8}` On consomme ensuite les 8 caractères situés après le $a. `.` n'importe quel caractère, `{8}` répétés 8 fois.
    - `[d-k]` Le carcatère suivant doit une lettre comprise entre d et k
    - `(.{4})` On capture ensuite gra^ceau x parenthèses les caractères qui suivent. Comme il s'agit du premier groupe capturé. J'utilise ensuite le `$1` pour faire rféférence à ce groupe
    - `.*` Je consomme ensuite le reste de ma ligne. C'est essentiel pour pouvoir écraser cette partie de la chaîne de caractère lors de l'opération de remplacement.

#### 9.2 Extraire la date 1 et ajouter un - pour les périodiques en cours (100 pos.8 = a)
- Expression :
    - Rechercher : `^.*?\$a.{8}a(.{4}).*`
    - Remplacer par : `$1 - `

#### 9.3 Extraire la date 1 et ajouter un ? pour les péridiques dont la situation est inconnue (100 pos.8 = c)
- Expression :
    - Rechercher : `^.*?\$c.{8}a(.{4}).*`
    - Remplacer par : `$1 - ?`


#### 9.4 Extraire la date 1 et la date 2. Les séparer par un - pour les ressources continue morte (100 pos.8 = b)
- Expression :
    - Rechercher : `^.*?\$b.{8}b(.{4})(.{4}).*`
    - Remplacer par : `$1 - $2`

</details>

## Formater des champs de citations pour les exploiter dans un tableur
Voici une liste de document extraite d'une bibliographie formaté selon le style de citation (APA)7eme edition. Nous aimerions exploier ces données dans un tableur contenant :
  - Les auteurs de l'ouvrage ou de l'article
  - La date de publication
  - Le titre de l'ouvrage ou de l'article
  - Le titre de la revue ou l'éditeur de l'ouvrage

### Texte à traiter
```
Bazin, S. (2015). Trail running Préparez vos défis ! Editions Amphora.
Giovanelli, N., Biasutti, L., Salvadego, D., Alemayehu, H. K., Grassi, B., & Lazzer, S. (2020). Changes in Skeletal Muscle Oxidative Capacity After a Trail-Running Race. International Journal of Sports Physiology and Performance, 15(2), 278‑284. https://doi.org/10.1123/ijspp.2018-0882
Raul Zapata-Rodrigo, Antoine Bruneau, Bertrand Rabiers Du Villars, & Christophe Ramírez-Parenteau. (2024). Musculoeskeletal injuries and illnesses in the Spanish and French trail running teams during the WMTRC 2023 Musculoeskeletal injuries and illnesses in the Spanish and French trail running teams during the WMTRC 2023. Apunts Sports Medicine, 59(223), 100451-.
Ryan, C. (2024). Mud, sweat and cameras—Irish trail and mountain running vlogging. Sport in Society, 27(6), 965‑982. https://doi.org/10.1080/17430437.2024.2334600
Seguí Urbaneja, J., Inglés Yuba, E., Labrador Roca, V., & Faría Torbidoni, E. I. (2016). Carreras (de o por) montaña o trail running. El reconocimiento de la modalidad deportiva : Una visión jurídica. Retos (Madrid), 30(30), 162‑167. https://doi.org/10.47197/retos.v0i30.39868
Skroce, K., Bettega, S., D’Emanuele, S., Boccia, G., Schena, F., & Tarperi, C. (2023). Flat Versus Simulated Mountain Trail Running : A Multidisciplinary Comparison in Well-Trained Runners. International Journal of Environmental Research and Public Health, 20(6), 5189-. https://doi.org/10.3390/ijerph20065189
Sordello. (2015). La bible du running : Route et trail running. Amphora.
Suter, D., Sousa, C. V., Hill, L., Scheer, V., Nikolaidis, P. T., & Knechtle, B. (2020). Even Pacing Is Associated with Faster Finishing Times in Ultramarathon Distance Trail Running—The “Ultra-Trail du Mont Blanc” 2008–2019. International Journal of Environmental Research and Public Health, 17(19), 7074-. https://doi.org/10.3390/ijerph17197074
Thuany, M., Witthöft, A., Valero, D., Forte, P., Weiss, K., Scheer, V., Nikolaidis, P. T., Andrade, M. S., Cuk, I., & Knechtle, B. (2024). Road and trail running from 5 km to an ultra-marathon – trends in Switzerland from 1999 to 2019. Human Movement, 25(3), 96‑108. https://doi.org/10.5114/hm/193799
Trama, R., Blache, Y., Hintzy, F., Rossi, J., Millet, G. Y., & Hautier, C. (2023). Does neuromuscular fatigue generated by trail running modify foot-ground impact and soft tissue vibrations? European Journal of Sport Science, 23(7), 1155‑1163. https://doi.org/10.1080/17461391.2022.2093649


```
### Correction
<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

^(.*?)\((.*?)\)\. (.*?)[!\?\.] (.*?)[,\.].*
"$1"\t$2\t"$3"\t"$4"

#### Extraire les auteurs
Les auteurs sont le premier élément mentionné. L adéclaration des auteurs va jusqu'à la date de publication introduite par un espace et une parenthèse ouvrante
- Expression :
    - Rechercher : `^(.*?)\(`
    - Remplacer par : `"$1"`
- Explication : 
    - `^` La recherche comence au début de la ligne
    - `^(.*?)` Les parenthèses sont destinées à capturé le texte correspondant au motif. Nous utilisons $1 pour réutiliser dans le "Remplacer" le groupe capturé
    - `^(.*?)\(` Indique qu'on capture tous les caractères présents jusqu'à la premières parenthèses ouvrantes :
      - `.` Nimporte quel caractère
      - `.*` Présent 0 ou plusieurs fois
      - `.*?` Jusqu' à la première occurence du motif situé immédiatement après
      - `\()` La parenthèse est échappée

#### Extraire les  dates de publication
La date de publication est située entre les parenthèses situées après la mention des auteurs.
- Expression :
  - Rechercher : `^(.*?)\((.*?)\)`
  - Remplacer par : `"$1"\t$2`
- Explication : `\((.*?)\)` : on capture la chaîne depuis la parenthèse ouvrante jusqu' à la première parenthèse fermante rencontrée. Come il s'agit du deuxième groupe capturé, on utilise `$2` pour récupérer le contenu de la capture.

#### Extraire le titre
Un point et un espace sépare la date de publication entre parenthèses du titre. Le titre comprend tous les caractères depuis cette position jusqu'aux ponctuations ".", "!" ou "?"
- Expression :
  - Rechercher : `^(.*?)\((.*?)\)\. (.*?)[!\?\.]`
  - Remplacer par : `"$1"\t$2\t"$3"`
- Explication : `[!\?\.]` : !, ? ou .

#### Le titre de la revue ou l'éditeur de l'ouvrage
C'est la chaine suivante qui va jusqu'à un "." (pour une monographie ) ou la première virgule rencontrée
- Expression :
  - Rechercher : `^(.*?)\((.*?)\)\. (.*?)[!\?\.] (.*?)[,\.].*`
  - Remplacer par : `"$1"\t$2\t"$3"\t"$4"`
- Explication : `.*` : A la fin de l'expression consomme la fin de la chaîne de caractère pour pouvoir l'écraser via la fonction remplacer
</details>

### Et si on allait plus loin ? Récupérer les références de l'article (n° fascicule, pagination) et le doi
La difficulté de cet exercice vient du fait que toutes les lignes ne contiennent pas ces informations. On peut utiliser les quantificateurs pour contourner cette difficulté.

<details>
  <summary>🕵️‍♀️Avez-vous bien cherché avant de consulter la solution ?</summary>

  #### Extraire les informations sur le fascicule
- Expression :
  - Rechercher : `^(.*?)\((.*?)\)\. (.*?)[!\?\.] (.*?)[,\.] ?(.*?\.)?`
  - Remplacer par : `"$1"\t$2\t"$3"\t"$4\t"$5"`
- Explication : ` ?(.*?\.)?`
    - ` ?` On indique que l'espace est facultatif. ? = 0 ou 1 fois
    - `(.*?\.)` On capture tous les carctères jusqu'au premier "." rencontré . C'est le 5eme groupe capturé. On utilise $5 pour utiliser le texte capturé
    - `(.*?\.)?` Le point d'interrogation derrière le groupe permet d'indiquer que ce dernier est facultatif

#### Extraire le doi 
- Expression :
  - Rechercher : `^(.*?)\((.*?)\)\. (.*?)[!\?\.] (.*?)[,\.] ?(.*?\.)?(.*?$)?`
  - Remplacer par : `"$1"\t$2\t"$3"\t"$4\t"$5"\t"$6"`
- Explication : `(.*?$)?`
    - `(.*?$)` On capture tous les carctères jusqu'à la fin de laligne rencontré . C'est le 6eme groupe capturé. On utilise $6 pour utiliser le texte capturé
    - `(.*?\.)?` Le point d'interrogation derrière le groupe permet d'indiquer que ce dernier est facultatif
</details> 




