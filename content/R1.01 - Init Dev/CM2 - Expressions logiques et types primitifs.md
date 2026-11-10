---
ressource: R1.01
type: cours
cm: 2
titre: Expressions logiques et types primitifs
auteur: Alain Casali
tags:
  - algo
  - expressions-logiques
  - types
  - variables
  - R1-01
---

# CM2 — Expressions logiques et types primitifs

[[R1.01 - Index|← Index R1.01]] · Suite : [[CM3 - Chaînes, algorithmes et boucles]]

> [!abstract] En bref
> Ce CM complète les expressions logiques avec **`ET_ALORS`** et **`OU_SINON`**, qui **court-circuitent** l'évaluation, puis pose tout le socle des données : **variables**, **constantes**, **entrées/sorties**, et les types **booléen**, **entier**, **réel**, **caractère** et **tableau**.

---

## 1. Pourquoi des expressions logiques complexes ?

Deux schémas apparemment équivalents ne le sont pas **algorithmiquement** :

```text
si (Expr_log_1)
    si (Expr_log_2)
        Sequ_1;
    fsi
fsi

Sequ_n;
```

et

```text
si (Expr_log_1 ET Expr_log_2)
    Sequ_1;
fsi

Sequ_n;
```

> [!warning] « Schémas pas tout-à-fait équivalents »
> Mathématiquement, `Expr_log_1 ET Expr_log_2` = `Expr_log_2 ET Expr_log_1` : **pas de chronologie** en mathématiques, les deux expressions sont **toujours** évaluées. En algorithmique, l'ordre d'évaluation compte : si la première expression est fausse, la seconde **n'est jamais évaluée**.

---

## 2. Opérateur `ET_ALORS`

### 2.1 Évaluation

| Étape | Schéma imbriqué | `ET_ALORS` |
|-------|-----------------|------------|
| 1 | évaluer `Expr_log_1` d'abord | évaluer `Expr_log_1` d'abord |
| 2 | si vrai, évaluer `Expr_log_2` ensuite | si vrai, évaluer `Expr_log_2` ensuite |
| 3 | si `Res_1` est faux, `Expr_log_2` **n'est jamais évaluée** | idem |

`ET_ALORS` est donc une **sorte d'opérateur booléen** qui respecte le **court-circuit** : le deuxième opérande n'est évalué que si le premier est vrai.

### 2.2 Le problème concret : le fichier « ADetruire »

```text
// Version imbriquée : correcte
si (fichier "ADetruire" existe)
    si (date_de_creation_fichier "ADetruire" < 01/01/2022)
        Effacer fichier "ADetruire";
    fsi
fsi
```

Ici, la date n'est évaluée **que si le fichier existe**.

```text
// Version avec ET : DANGEREUSE
si (fichier "ADetruire" existe ET
    date_de_creation_fichier "ADetruire" < 01/01/2022)
    Effacer fichier "ADetruire";
fsi
```

Avec `ET`, la date est **toujours évaluée**, même si le fichier n'existe pas : on interroge la date d'un fichier inexistant → **erreur**.

**Solution** : utiliser `ET_ALORS`.

```text
si (fichier "ADetruire" existe ET_ALORS
    date_de_creation_fichier "ADetruire" < 01/01/2022)
    Effacer fichier "ADetruire";
fsi
```

> [!important] Règle
> `A ET_ALORS B` : si `A` est **faux**, on renvoie **faux** sans évaluer `B`. On protège ainsi les évaluations **dangereuses** ou **coûteuses**.

---

## 3. Opérateur `OU_SINON`

De même que `ET` avait un frère court-circuitant, `OU` a `OU_SINON` :

```text
si (Expr_log_1)
    Sequ_1;
sinon
    si (Expr_log_2)
        Sequ_1;
    fsi
fsi
```

se simplifie en :

```text
si (Expr_log_1 OU_SINON Expr_log_2)
    Sequ_1;
fsi
```

`Expr_log_2` n'est évaluée **que si `Expr_log_1` est fausse**.

### Exemple

```text
si (JeSuisUnGuerrier)
    Attaquer;
fsi

si (BeaucoupDeVie)
    Attaquer;
sinon
    …
fsi
```

devient :

```text
si (BeaucoupDeVie OU_SINON JeSuisUnGuerrier)
    Attaquer;
fsi
```

> [!note] Ordre des opérandes
> Dans l'exemple du cours, le premier opérande est `BeaucoupDeVie` : si le personnage a beaucoup de vie, on n'évalue même pas `JeSuisUnGuerrier`. L'ordre est donc un **choix de conception**, pas une simple commodité d'écriture.

---

## 4. Propagation de l'opérateur `NON`

### 4.1 `NON (A ET B)`

```text
si (NON (Expr_log_1 ET Expr_log_2))
```

est équivalent à :

```text
si ((NON Expr_log_1) OU (NON Expr_log_2))
```

Exemple :

```text
si (NON (JeSuisUnGuerrier ET BeaucoupDeVie))
    Avancer;
fsi
Attaquer;
```

se traduit par :

```text
si ((NON JeSuisUnGuerrier) OU (NON BeaucoupDeVie))
    Avancer;
fsi
Attaquer;
```

### 4.2 `NON (A ET_ALORS B)`

```text
NON (A ET_ALORS B)   ≡   (NON A) OU_SINON (NON B)
```

### 4.3 `NON (A OU B)`

```text
NON (A OU B)   ≡   (NON A) ET (NON B)
```

### 4.4 `NON (A OU_SINON B)`

```text
NON (A OU_SINON B)   ≡   (NON A) ET_ALORS (NON B)
```

> [!important] Récapitulatif — lois de De Morgan
> | Négation | Équivalent |
> |----------|------------|
> | `NON (A ET B)` | `(NON A) OU (NON B)` |
> | `NON (A ET_ALORS B)` | `(NON A) OU_SINON (NON B)` |
> | `NON (A OU B)` | `(NON A) ET (NON B)` |
> | `NON (A OU_SINON B)` | `(NON A) ET_ALORS (NON B)` |
>
> Le **ET devient OU**, le **OU devient ET**, et le **court-circuit se conserve** (le frère de l'opérateur reste le même).

---

## 5. Tables de vérité

### 5.1 `ET` / `ET_ALORS`

`Expr_log_3 = Expr_log_1 ET Expr_log_2`

| `ET` / `ET_ALORS` | `Expr_log_1` = V | `Expr_log_1` = F |
|-------------------|------------------|------------------|
| **`Expr_log_2` = V** | V | F |
| **`Expr_log_2` = F** | F | F |

### 5.2 `OU` / `OU_SINON`

`Expr_log_3 = Expr_log_1 OU Expr_log_2`

| `OU` / `OU_SINON` | `Expr_log_1` = V | `Expr_log_1` = F |
|-------------------|------------------|------------------|
| **`Expr_log_2` = V** | V | V |
| **`Expr_log_2` = F** | V | F |

> [!note] Mnémo
> `ET` = il faut **deux** V pour avoir V. `OU` = il suffit d'**un** V pour avoir V. La table ne dit **rien** sur l'ordre d'évaluation : c'est justement là que `ET_ALORS` / `OU_SINON` diffèrent de `ET` / `OU`.

---

## 6. Variables

### 6.1 Organisation de la mémoire

> **Une case de la mémoire ne peut contenir qu'une unique variable (du même type).**

```text
declarer jeu : jeuDeSociete;
```

Dans cette case mémoire (nommée `jeu`), on peut ranger **un seul** jeu de société.

### 6.2 Déclaration

```text
declarer nomDeVariable : Type;
```

Exemple :

```text
declarer jeu : jeuDeSociete;
```

`;` est le **caractère de terminaison d'instruction**.

### 6.3 Affectation

```text
nomDeVariable <- Valeur;
```

Exemple :

```text
jeu <- ColonDeCatane;
```

La case `jeu` contient maintenant `ColonDeCatane`.

### 6.4 Réaffectation

```text
jeu <- Carcassonne;
```

La case `jeu` contient **maintenant** `Carcassonne` : l'ancienne valeur est **écrasée**. Une variable ne garde pas d'historique.

### 6.5 Constantes

```text
declarer KnomVariable : constante Type <- Valeur;
```

Une **constante** est une variable qui :

- possède une **unique valeur** tout au long de son existence ;
- doit donc être **initialisée lors de sa création** ;
- **ne peut pas** être modifiée ensuite.

```text
declarer Kjeu : constante jeuDeSociete <- ColonDeCatane;
Kjeu <- Carcassonne;      // FAUX
```

### 6.6 Sémantique des noms

```text
declarer superPlombier : delivreurDePrincesse;
superPlombier <- Mario;    // juste
superPlombier <- Luigi;    // juste
superPlombier <- Link;     // juste… mais sémantiquement faux
```

Chaque nom de variable doit contenir la **sémantique** liée à son type. Le compilateur ne voit que le **type** ; le lecteur humain voit le **sens**.

### 6.7 Nomenclature (camelCase)

1. **Premier mot en minuscule** ;
2. **Première lettre du mot suivant en majuscule** ;
3. **Reste du mot en minuscule** ;
4. Revenir en 2 s'il y a un autre mot.

Exemples : `superPlombier`, `dateDeCreationFichier`.

> [!important] Exception
> Les **constantes** commencent par la lettre **`K`** : `Kjeu`, `KBlack`, `KPi`.

---

## 7. Entrées / sorties

### 7.1 But

Favoriser les interactions avec l'utilisateur :

- en demandant une **saisie clavier** ;
- en provoquant un **affichage écran**.

### 7.2 Saisie clavier

**Prérequis** : on ne peut demander la saisie d'une variable **que si elle a été déclarée auparavant**.

```text
declarer superPlombier : delivreurDePrincesse;
saisir (superPlombier);        // l'utilisateur tape Mario
```

La case `superPlombier` contient alors `Mario`.

> [!warning] Règle d'or de l'informatique #2 : NTUI
> **NTUI = Never Trust User Input** (« ne fais jamais confiance à l'entrée utilisateur »). L'utilisateur peut taper n'importe quoi : il faudra **valider** les saisies (ce sera fait en C++ avec `cin.fail()`, voir [[CM10 - Flux, tri et tests]]).

### 7.3 Affichage écran

**Prérequis** : on ne peut afficher une variable **que si elle a été initialisée, saisie ou affectée**.

```text
afficher (superPlombier);
```

### 7.4 Passage à la ligne

```text
ligne_suivante;
```

provoque un **retour à la ligne** lors d'un affichage.

```text
afficher (superPlombier);   // Mario
superPlombier <- Luigi;
ligne_suivante;
afficher (superPlombier);   // Luigi, à la ligne suivante
```

### 7.5 Compression d'affichage

```text
afficher ("mon héros est : ");
afficher (superPlombier);
```

se simplifie en :

```text
afficher ("mon héros est : ", superPlombier);
```

De même :

```text
afficher (superPlombier);
afficher (" : est mon héros");
```

devient :

```text
afficher (superPlombier, " : est mon héros");
```

> [!tip] En C++
> Cette compression correspond à l'enchaînement des `<<` : `cout << "mon héros est : " << superPlombier;`.

---

## 8. Type primitif `booleen`

| Élément | Contenu |
|---------|---------|
| **Identificateur** | `booleen` |
| **Valeurs** | `vrai`, `faux` |
| **Opérations booléennes** | `ET`, `OU`, `NON`, `ET_ALORS`, `OU_SINON` — produisent un `booleen` |
| **Opérations d'identité** | `vaut`, `ne_vaut_pas` — produisent un `booleen` |

```text
declarer toBe : booleen;
declarer notToBe : booleen;
declarer question : booleen;

toBe <- faux;
notToBe <- NON toBe;          // vrai
question <- toBe OU notToBe;  // toujours vrai !
```

### 8.1 Simplification des tests d'identité

> [!important] Règle
> Dans un test de comparaison, **on ne compare jamais un booléen avec les valeurs `vrai` ou `faux`**.

```text
// Sémantiquement juste mais NON conforme aux normes
si (jeSuisUnMage vaut vrai)
    …
fsi
```

```text
// Conforme : comparaison automatique à vrai
si (jeSuisUnMage)
    …
fsi
```

```text
// Conforme : comparaison automatique à faux
si (NON jeSuisUnMage)
    …
fsi
```

---

## 9. Types primitifs `entier` et `entier_naturel`

### 9.1 Identificateurs et valeurs

| Type | Valeurs |
|------|---------|
| `entier` | Sous-ensemble des entiers mathématiques (positifs et négatifs) |
| `entier_naturel` | Sous-ensemble des entiers **positifs ou nuls** |

### 9.2 Opérations arithmétiques

```text
+   -   *   /
```

Elles produisent un **entier**. La division `/` est une **division euclidienne (troncature)** : la partie décimale est **abandonnée**.

> [!warning] L'ordre des opérations change tout
> ```text
> declarer x : entier;
> x <- 1;
> x <- (x + 2) / 4 * 3;   // (1+2)/4 = 0, puis 0*3 = 0
> afficher (x);           // 0
>
> x <- 1;
> x <- (x + 2) * 3 / 4;   // (1+2)*3 = 9, puis 9/4 = 2
> afficher (x);           // 2
> ```
> Même valeurs de départ, **résultats différents** : la troncature s'applique **à chaque division**.

### 9.3 Opérations de comparaison et d'identité

| Opérateurs | Résultat | Remarque |
|------------|----------|----------|
| `<`, `<=`, `>`, `>=` | `booleen` | relation d'ordre |
| `vaut`, `ne_vaut_pas` | `booleen` | identité |

```text
declarer nb1 : entier_naturel;
declarer nb2 : entier_naturel;
declarer nb3 : entier_naturel;

nb1 <- 12;
nb2 <- 5;
nb3 <- nb1 / nb2;        // nb3 vaut 2

si (nb3 vaut 4)
    afficher ("nb3 vaut 4");
sinon
    si (nb3 >= 2)
        afficher ("nb3 >= 2");
    fsi
fsi
```

Résultat affiché : `nb3 >= 2`.

### 9.4 Fonctions mathématiques

#### `modulo (Ent_1, Ent_2)`

Retourne le **reste de la division entière**.

```text
declarer x : entier_naturel;
x <- 10;

declarer y : entier_naturel;
y <- modulo (2, 10);    // y <- 2
afficher (y);           // 2

afficher (modulo (2, x));   // 2, de manière plus compacte
```

> [!warning] Ordre des arguments
> Le cours écrit `modulo (2, 10)` et commente `2`. Le **premier** argument est donc le nombre dont on cherche le reste, le **second** le diviseur. Ne pas inverser : `modulo (10, 2)` vaut `0`.

#### `rand (entier1, entier2)`

Retourne un entier **aléatoire dans l'intervalle `[entier1, entier2]`** (bornes incluses).

```text
si (rand (1, 2) vaut 1)
    afficher ("j'ai gagné");
sinon
    afficher ("tu as perdu");
fsi
```

---

## 10. Type primitif `reel`

| Élément | Contenu |
|---------|---------|
| **Identificateur** | `reel` |
| **Valeurs** | Sous-ensemble des réels mathématiques |
| **Littéraux** | `3.5`, `-21.2e12` (notation scientifique) |
| **Opérations** | `+`, `-`, `*`, `/` → **division réelle** |
| **Comparaisons** | `<`, `<=`, `>`, `>=` → `booleen` |
| **Identité** | `vaut`, `ne_vaut_pas` → `booleen` |

> [!important] Différence clé avec les entiers
> Sur des `entier`, `/` **tronque**. Sur des `reel`, `/` fait une **division réelle**. `7 / 2` vaut `3` en entier, `3.5` en réel.

---

## 11. Type primitif `caractere`

| Élément | Contenu |
|---------|---------|
| **Identificateur** | `caractere` |
| **Littéral** | Un **symbole entre simples quotes** : `'a'`, `'9'` |
| **Comparaisons** | `<`, `<=`, `>`, `>=` → `booleen` |
| **Identité** | `vaut`, `ne_vaut_pas` → `booleen` |

### 11.1 Relation d'ordre

```text
chiffres < … < majuscules < … < minuscules < … < caractères accentués
```

> [!warning] Piège classique
> `'Z' < 'a'` est **vrai** : toutes les majuscules sont avant toutes les minuscules. Ne jamais supposer que `'a'` et `'A'` sont « côte à côte ».

### 11.2 Fonctions applicables

Toutes produisent un `caractere` :

| Fonction | Résultat |
|----------|----------|
| `tolower (unCaract)` | le caractère en **minuscule** (uniquement si c'est une majuscule) |
| `toupper (unCaract)` | le caractère en **majuscule** (uniquement si c'est une minuscule) |
| `succ (unCaract)` | le caractère **suivant** dans l'ordre prédéfini |
| `pred (unCaract)` | le caractère **précédent** dans l'ordre prédéfini |

```text
declarer caract : caractere;
caract <- nImporteQuelCaractere;

afficher (tolower (caract));   // minuscule
afficher (toupper (caract));   // majuscule
afficher (succ (caract));      // suivant
afficher (pred (caract));      // précédent
```

---

## 12. Type complexe `tableau_de`

### 12.1 Principe

`tableau_de` permet de **regrouper plusieurs valeurs sous un même identificateur**, valeurs **désignées par leur rang**.

```text
declarer tabReel : tableau_de 10 reel;
```

`tabReel` désigne l'ensemble des **10 valeurs réelles**. L'espace mémoire est **réservé mais pas initialisé**.

### 12.2 Modèle général

```text
declarer nomTab : tableau_de [Taille] typePrimitif;
```

### 12.3 Accès à un élément

On accède au **(i+1)ᵉ** élément avec `tab[i]`.

- La **première** case a pour indice **0** ;
- la **dernière** case a pour indice **taille − 1** ;
- **aucun contrôle** de validité de l'indice n'est effectué, ni en lecture ni en écriture.

```text
tabReel[0] <- 10;      // écriture
tabReel[1] <- 11;
…
afficher (tabReel[0]);   // 10 — lecture
afficher (tabReel[10]);  // oups, boulette : indice hors bornes
```

### 12.4 Taille

```text
taille (unTableau)
```

renvoie le **nombre d'éléments** (de cases).

```text
afficher (taille (tabReel));   // 10
```

**Conséquence** : tous les indices valides de `tab` sont dans `[0, taille (tab)[` (intervalle **fermé à gauche, ouvert à droite**).

### 12.5 Initialisation

On est **obligé d'initialiser le tableau case par case**. Il est **impossible** d'initialiser toutes les cases avec une unique instruction.

> **⇒ Pas de tableaux constants.**

### 12.6 Tableaux non dimensionnés

La plupart du temps, on **ne connaît pas la dimension à l'avance**. On veut pouvoir :

- **ajouter** de nouveaux éléments ;
- **supprimer** des éléments obsolètes ;
- tout en gardant une **taille convenable**.

```text
declarer nomTab : tableau_de typePrimitif;
```

#### `allonger (unTab, nbElem)`

Ajoute `nbElem` **nouvelles cases à la fin** du tableau.

> [!warning] Les nouvelles cases ne sont **pas initialisées** !
> ```text
> declarer tabInt : tableau_de entier;
> allonger (tabInt, 5);
> afficher (taille (tabInt));   // 5
> afficher (tabInt[0]);         // n'importe quoi
> ```

#### `redimensionner (unTab, nbElem)`

Fixe la taille du tableau à `nbElem`. On peut :

- **allonger** : les nouvelles cases ne sont pas initialisées ;
- **tronquer** (raccourcir).

```text
redimensionner (tabInt, 10);
afficher (taille (tabInt));   // 10

tabInt[3] <- 13;
afficher (tabInt[3]);         // 13

redimensionner (tabInt, 2);
afficher (taille (tabInt));   // 2
afficher (tabInt[3]);         // la case n'existe plus
```

---

## 13. Ce que le plan annonce mais que le deck ne contient pas

Le plan de l'amphi annonce deux parties supplémentaires :

- « **Cas particulier : les tableaux_de caractères** » ;
- « **Un Premier algorithme** ».

Elles **ne figurent pas** dans le diaporama de l'amphi 2 : elles sont traitées dans l'amphi 3, donc dans [[CM3 - Chaînes, algorithmes et boucles]] (type `string`, premier algorithme `Hello world`, second algorithme `y = ax + b`).

---

## ✅ À retenir

- `ET_ALORS` et `OU_SINON` **court-circuitent** : le second opérande n'est évalué que si nécessaire.
- `ET` / `OU` évaluent **toujours** les deux opérandes → danger avec un fichier inexistant, une division par zéro, etc.
- **De Morgan** : `NON (A ET B)` = `(NON A) OU (NON B)` ; `NON (A OU B)` = `(NON A) ET (NON B)` ; idem avec les versions court-circuitées.
- Une **variable** = une case mémoire, **un seul type**, **une seule valeur** à la fois.
- Une **constante** commence par **`K`**, est initialisée à la création et **ne change jamais**.
- **NTUI** : ne jamais faire confiance à la saisie utilisateur.
- Un **booléen** ne se compare pas à `vrai` / `faux` : `si (b)` et `si (NON b)`.
- Division entière = **troncature** ; `modulo` = **reste** ; `rand (a, b)` = **entier aléatoire dans `[a, b]`**.
- Un **tableau** va de l'indice `0` à `taille − 1`, **sans contrôle de bornes** ; il s'initialise **case par case** ; `allonger` / `redimensionner` ne remplissent pas les nouvelles cases.

### Questions d'auto-évaluation

- [ ] Pourquoi `A ET B` et `A ET_ALORS B` ne sont-ils pas équivalents en algorithmique ?
- [ ] Écrire l'exemple du fichier « ADetruire » avec `ET_ALORS` et expliquer ce qui se passerait avec `ET`.
- [ ] Énoncer les quatre lois de De Morgan du cours.
- [ ] Donner la table de vérité de `OU_SINON` et expliquer sa différence avec `OU`.
- [ ] Pourquoi ne compare-t-on jamais un booléen à `vrai` ou `faux` ?
- [ ] Que vaut `(x + 2) / 4 * 3` pour `x = 1` ? Et `(x + 2) * 3 / 4` ? Expliquer.
- [ ] Que renvoie `modulo (2, 10)` ? Et `modulo (10, 2)` ?
- [ ] Quelle est la différence entre `allonger` et `redimensionner` ?
- [ ] Quels sont les indices valides d'un tableau de taille `n` ? Que se passe-t-il si on sort de ces bornes ?
- [ ] Pourquoi n'existe-t-il pas de tableaux constants ?
- [ ] Quel est l'ordre des caractères : chiffres, majuscules, minuscules, accentués ?
- [ ] Que font `tolower`, `toupper`, `succ` et `pred` ? Que se passe-t-il si le caractère n'est pas dans le bon cas ?
