---
ressource: R1.01
type: cours
cm: 4
titre: Boucle pour, portée et sous-programmes
auteur: Alain Casali
tags:
  - algo
  - boucle-pour
  - portee
  - sous-programmes
  - R1-01
---

# CM4 — Boucle pour, portée et sous-programmes

[[R1.01 - Index|← Index R1.01]] · Suite : [[CM5 - Du pseudo-code au C++]]

> [!abstract] En bref
> La boucle **`pour`** regroupe en une ligne l'initialisation, le test et l'incrémentation d'un compteur — mais **interdit** toute rupture de séquence par `sortie`. On voit ensuite la **portée des variables**, puis les **sous-programmes** : procédures, fonctions, prédicats et passage de paramètres `in` / `out` / `in_out`.

---

## 1. La boucle à compteur

### 1.1 Afficher 100 fois « Bonjour »

```text
pour (i variant_de 1 a 100)
faire

    afficher ("Bonjour");

ffaire
```

Avantages par rapport à `tant_que` :

- **pas de déclaration** de `i` ;
- **pas d'initialisation** ;
- **pas d'incrémentation** ;
- **pas de test d'arrêt**.

Tout est **visible** dans l'en-tête de la boucle.

> [!important] Sémantique
> `i` prend **toutes les valeurs** entre `borneMin` et `borneMax`, **bornes incluses**, dans l'ordre croissant. La variable de parcours est **gérée par la boucle**.

### 1.2 Parcourir un tableau

```text
declarer tabInt : tableau_de entier;
// génération de tabInt

pour (i variant_de 0 a taille (tabInt) - 1)
faire
    afficher (tabInt[i]);
ffaire
```

> [!warning] Ne jamais atteindre `taille (tabInt)`
> La dernière case a pour indice `taille − 1`. Écrire `0 a taille (tabInt)` provoquerait un accès **hors bornes** à `tabInt[taille]`.

### 1.3 Parcourir à l'envers

#### Solution 1 : indice croissant, case calculée

```text
pour (i variant_de 0 a taille (tabInt) - 1)
faire
    afficher (tabInt[taille (tabInt) - 1 - i]);
ffaire
```

#### Solution 2 : indice **descendant**

```text
pour (i variant_de taille (tabInt) - 1 a 0 descendant)
faire
    afficher (tabInt[i]);
ffaire
```

> [!note] Le mot-clé `descendant`
> C'est lui qui indique que le compteur **décroît**. Sans lui, la boucle ne s'exécuterait pas (la borne de départ est supérieure à la borne d'arrivée).

#### Solution 3 : `tant_que` — et le piège de l'entier naturel

```text
declarer i : entier_naturel <- taille (tabInt) - 1;
tant_que (i >= 0)
faire
    afficher (tabInt[i]);
    i <- i - 1;
ffaire
```

> [!danger] « Surtout pas ça : il se passe quoi quand i vaut 0 ? »
> `i` est un **`entier_naturel`** : il ne peut pas être négatif. Quand `i` vaut `0`, `i <- i - 1` **déborde** (la valeur devient énorme au lieu de `−1`), donc `i >= 0` reste vrai → **boucle infinie** et accès hors bornes.
>
> Il faudrait un `entier` **signé**, ou tester `i > 0` en décrémentant **avant** l'accès. C'est un aperçu du problème `unsigned` / `int` du [[CM7 - Caractères, transtypage et opérateurs|CM7]].

### 1.4 Rupture de séquence interdite dans un `pour`

> [!important] Règles du cours
> Dans une boucle `pour`, la variable de parcours doit prendre **toutes** les valeurs entre les bornes minimale et maximale (incluses). **Aucune rupture de séquence utilisant `sortie` n'est permise.** Il faut changer de schéma de boucle.

Exemple : attaquer **(au plus)** 10 fois un monstre.

```text
// INTERDIT
pour (i variant_de 1 a 10)
faire
    Attaquer;
    si (Victoire) sortie;
ffaire
```

Traduction correcte :

```text
declarer i : entier_naturel;
i <- 1;
tant_que (i < 11)
faire
    si (victoire) sortie;
    attaquer;
    i <- i + 1;
ffaire
```

Ou mieux, en intégrant la condition d'arrêt dans la condition de continuation :

```text
declarer i : entier_naturel;
i <- 1;
tant_que (i < 11 ET_ALORS NON victoire)
faire
    attaquer;
    i <- i + 1;
ffaire
```

> [!tip] Pourquoi `ET_ALORS` et pas `ET` ?
> Si `victoire` est vrai, on veut sortir **sans** évaluer la suite. `ET_ALORS` respecte le court-circuit (voir [[CM2 - Expressions logiques et types primitifs]]).

### 1.5 `continue` est en revanche autorisé

```text
// Une lettre sur deux, avec deux blocs
pour (i variant_de 0 a taille (chaine) - 1)
faire
    si (0 vaut modulo (i, 2))
        afficher (chaine[i]);
    fsi
ffaire
```

```text
// Une lettre sur deux, avec continue
pour (i variant_de 0 a taille (chaine) - 1)
faire
    si (0 ne_vaut_pas modulo (i, 2)) continue;
    afficher (chaine[i]);
ffaire
```

> [!important] Nuance
> « En revanche, l'utilisation de `continue` dans une boucle `pour` est **légale**. » `continue` ne casse pas la séquence : la boucle **passe simplement** à la valeur suivante du compteur (l'incrémentation est gérée par le `pour`).

---

## 2. Portée des variables

### 2.1 Définition

> Une variable **n'existe que dans le bloc** dans lequel elle est déclarée.

```text
boucle
    declarer n : entier_naturel;
    afficher ("Saisir n");
    saisir (n);
    si (n > 10) sortie;
    afficher ("Plus grand que 10 svp");
    ligne_suivante;
fboucle

// n n'existe pas ici
```

### 2.2 Exemple : saisir `n > 10`

```text
declarer n : entier_naturel;
boucle
    afficher ("Saisir n");
    saisir (n);
    si (n > 10) sortie;
    afficher ("Plus grand que 10 svp");
    ligne_suivante;
fboucle

// n existe et a une valeur > 10
```

Ici `n` est déclaré **avant** la boucle : il est encore visible **après**. C'est souvent ce qu'on veut.

### 2.3 Pourquoi `i` n'existe pas après un `pour`

```text
declarer chaine : string;
declarer lettreATrouver : caractere;
saisir (chaine);
saisir (lettreATrouver);

pour (i variant_de 0 a taille (chaine) - 1)
faire
    si (lettreATrouver vaut chaine[i]) sortie;
ffaire

afficher (i);      // i n'a pas de valeur ici
```

La variable `i` est **déclarée par le `pour`**, donc sa portée est **le corps de la boucle**. Après le `ffaire`, elle n'existe plus.

> [!warning] « Premier patch » : mauvaise idée
> Déclarer `i` **avant** la boucle **puis** le redéclarer dans le `pour` :
> ```text
> declarer i : entier_naturel;
> …
> pour (i variant_de 0 a taille (chaine) - 1)
> ```
> redéfinit `i` : le compilateur peut refuser, et le résultat devient **ambigu**. C'est ce que le prof appelle « Modulo la (re)-définition de i ».

### 2.4 Le teaser « architecture de l'ordi en 2023 »

Les diapos suivantes montrent l'exécution de **4 algorithmes en parallèle** (`Thread1` à `Thread4`) sur la même boucle. Si chaque thread fait `i <- i + 2` sur une variable **partagée**, deux threads peuvent lire la même valeur et l'un écrase l'autre : le résultat affiché peut être faux.

> [!note] Niveau S5
> Le prof conclut : « Le résultat affiché sera `3` → fail, mais c'est du niveau **S5** ». À retenir pour l'instant : **une variable partagée entre threads est dangereuse**. Une boucle `pour` « parallélisable » n'est pas magiquement correcte.

---

## 3. Les sous-programmes

### 3.1 Motivation

```text
algorithme afficher100Bonjour
debut
    pour (i variant_de 1 a 100)
    faire
        afficher ("Bonjour");
    ffaire
fin
```

Et si on veut l'afficher **50 fois** ? **Tout réécrire ?** → **sous-programme**.

### 3.2 Procédure : exemple

```text
procedure afficherNFoisBonjour (n : in entier_naturel)
debut
    pour (i variant_de 1 a n)
    faire
        afficher ("Bonjour");
    ffaire
fin
```

Décomposition du profil :

| Élément | Rôle |
|---------|------|
| `afficherNFoisBonjour` | **identificateur** de la procédure |
| `n` | **identificateur du paramètre** |
| `in` | **marqueur** : paramètre « donnée » |
| `entier_naturel` | **type** du paramètre |
| `procedure` | indique que **ce n'est pas une fonction** (pas de valeur de retour) |

### 3.3 Qu'est-ce que c'est ?

Un sous-programme, c'est un algorithme qui :

- a toujours un **identificateur** (un nom) ;
- a des **specs** (spécifications = **conditions d'utilisation**) ;
- a éventuellement des **paramètres** — entre autres ses « ingrédients », les données sur lesquelles il travaille ;
- « produit » éventuellement un **résultat**.

> [!important] Image du guichet
> La liste des paramètres est un **guichet de communication** entre le **monde extérieur** et l'**intérieur** du sous-programme.

### 3.4 À quoi ça sert ?

#### Mettre en facteur du code

Une séquence d'instructions souvent utilisée est :

- **écrite une seule fois** ;
- mise sous forme de **sous-programme** ;
- toutes les occurrences de la séquence sont remplacées par un **appel** du sous-programme.

Exemple : la saisie de texte dans une zone de saisie. C'est gérer :

- le **déplacement du curseur** (caractère précédent / suivant, début / fin de zone) ;
- l'**affichage** ou la **suppression** des caractères ;
- l'**insertion** ou le **remplacement** ;
- l'**effacement** du caractère précédent.

Et cela intervient dans :

- les éditeurs de texte (IDE) ;
- les éditeurs de fichiers (`open`, `save`, `saveAs`…) ;
- les légendes graphiques Excel ;
- les zones de texte de l'explorateur ;
- « nommer un nouveau dossier », « renommer un dossier » ;
- les cellules d'Excel ;
- de nombreuses boîtes de dialogue.

#### Avantages

| Avantage | Explication |
|----------|-------------|
| **Taille du code** | on réduit le nombre de lignes |
| **Efficience** | économie des moyens (mémoire, disque) |
| **Réutilisabilité** | le sous-programme resservira dans du code à venir |
| **Maintenance** | mise au point faite **une seule fois** (modifs, améliorations, corrections) |
| **Maintenabilité** | évite le **copier/coller**, source d'erreurs |
| **Lisibilité** | on améliore la compréhension |

#### Écrire des algorithmes « de haut niveau »

Remplacer une suite d'actions élémentaires par une action « de haut niveau » :

```text
afficher l'écran d'accueil;
établir une connexion;
saisir login/password;
charger les images;
interroger une base de données;
…
```

### 3.5 Déclaration et utilisation

```text
procedure afficherNFoisBonjour (n : in entier_naturel);
```

C'est une **déclaration** (ou **profil**, ou **signature**) : elle précise le **mode d'emploi**.

```text
declarer nbFois : entier_naturel;
nbFois <- 12;

afficherNFoisBonjour (nbFois);
```

| Terme | Rôle |
|-------|------|
| `nbFois` dans l'appel | **paramètre effectif** (la valeur réellement passée) |
| `n` dans la déclaration | **paramètre formel** (le nom utilisé dans le sous-programme) |

Les deux doivent être **compatibles** en type.

### 3.6 Les paramètres : `in`, `out`, `in_out`

#### `in` — données

```text
procedure afficherNFoisBonjour (n : in entier_naturel);
```

- Données **nécessaires au fonctionnement** du sous-programme.
- Marqueur `in` : **pas de modification** du paramètre dans le sous-programme.

#### `out` — résultats

Exemple : un sous-programme qui reçoit une chaîne, **crée une nouvelle string sans espaces**, et **fournit le nombre de suppressions**.

```text
procedure supprSpaces (strInitiale : in  string,
                       strFinale   : out string,
                       nbSuppr     : out entier_naturel);
```

- Marqueur `out` : **modification obligatoire** du paramètre dans le sous-programme.

Corps (tel que donné sur la diapo) :

```text
procedure supprSpaces (strInitiale : in  string,
                       strFinale   : out string,
                       nbSuppr     : out entier_naturel)
debut
    nbSuppr <- 0;
    strFinale <- "";      // au cas où il y aurait déjà quelque chose dedans

    pour (i variant_de 0 a taille (strInitiale) - 1)
    faire
        si (strInitiale[i] vaut ' ') continue;
        strFinale <- strFinale + strInitiale[i];
        nbSuppr <- nbSuppr + 1;
    ffaire
fin
```

> [!warning] Incohérence du support
> L'énoncé annonce « le **nombre de suppressions** effectuées », mais le corps incrémente `nbSuppr` pour chaque caractère **conservé** (puisque `continue` saute les espaces **avant** l'incrément). Tel qu'écrit, `nbSuppr` compte donc les caractères **non-espace**. Pour compter les suppressions, il faudrait incrémenter **dans** le cas « c'est un espace ». Retenir l'**intention** (compter les espaces retirés) et savoir repérer l'erreur.

> [!important] Règle des `out`
> « Tout paramètre ayant un marqueur `out` doit être **initialisé dès que possible** dans un sous-programme afin d'éviter les **effets de bord**. »

```text
declarer ligneLue : string;       // par défaut chaîne vide
declarer nouvelleLigne : string;  // par défaut chaîne vide
declarer nbSuppr : entier_naturel; // non initialisé

saisir (ligneLue);
nbSuppr <- 10;                    // totalement inutile
supprSpaces (ligneLue, nouvelleLigne, nbSuppr);
// nouvelleLigne et nbSuppr sont utilisables en sortie
```

- `nbSuppr <- 10` avant l'appel est **inutile** : le paramètre `out` sera de toute façon réécrit.
- En revanche, `nouvelleLigne` et `nbSuppr` doivent être **déclarés** avant l'appel.
- `saisir (ligneLue)` donne sa **véritable taille** à `ligneLue`.

#### `in_out` — données-résultats

Exemple : transformer toutes les minuscules en majuscules **dans** la chaîne, et compter les transformations.

```text
procedure minToMaj (str      : in_out string,
                    nbTransf : out    entier_naturel);
```

- Marqueur `in_out` : **modification possible** du paramètre dans le sous-programme (il est lu **et** écrit).

```text
procedure minToMaj (str      : in_out string,
                    nbTransf : out    entier_naturel)
debut
    nbTransf <- 0;
    pour (i variant_de 0 a taille (str) - 1)
    faire
        // si on n'a pas une minuscule, on ne fait rien
        si (NON is_lower (str[i])) continue;
        // on sait qu'on a une minuscule
        str[i] <- to_upper (str[i]);
        nbTransf <- nbTransf + 1;
    ffaire
fin
```

> [!warning] Coquille du support
> La diapo écrit `is_lower (strInitiale[i])` alors que le paramètre s'appelle `str` et qu'il n'y a pas de `strInitiale` dans ce sous-programme. Lire `str[i]`.

### 3.7 Fonction

> Soit une procédure qui a **au moins un paramètre `out`** ; la **fonction** correspondante **renvoie** ce paramètre `out`.

Exemple : racine carrée.

```text
// En procédure
procedure sqrt (valInit : in  reel,
                result  : out reel);

// En fonction
fonction sqrt (valInit : in reel)
                  renvoie reel;
```

Utilisation :

```text
declarer rac12 : reel;
rac12 <- sqrt (12.0);
```

> [!important] Définition
> Une fonction est un sous-programme qui **renvoie une instance d'un type**. Le type de retour est déclaré après `renvoie`.

Implémentation du cours (approximation de `√a` par suites adjacentes) :

```text
fonction sqrt (X : in reel) renvoie reel
debut
    // la précision
    declarer eps : reel;
    eps <- 0.01;

    declarer u : reel;
    u <- 1;
    declarer v : reel;
    v <- X;

    // sert à mémoriser l'ancienne valeur de u
    declarer tmp : reel;

    tant_que (abs (u - v) > eps)
    faire
        tmp <- u;
        u <- 2 / ((1/u) + (1/v));
        v <- (tmp + v) / 2;
    ffaire

    renvoie u;
fin
```

> [!note] Appels imbriqués
> « On a le droit d'appeler un sous-programme dans un sous-programme, **à condition que ce dernier soit défini auparavant**. »

### 3.8 Prédicat

> Un **prédicat** est une **fonction qui renvoie un `booleen`**.

```text
fonction isMultiple (entier1 : in entier,
                     entier2 : in entier)
                     renvoie booleen;
```

- Choisir un identificateur qui commence par **`is`**, **`has`** ou **`are`**.
- `has` est parfois plus parlant : `hasValue (Objet)`.

```text
// Version 1
fonction isMultiple (entier1 : in entier,
                     entier2 : in entier)
                     renvoie booleen
debut
    si (modulo (entier1, entier2) vaut 0)
        renvoie vrai;
    sinon
        renvoie faux;
    fsi
fin

// Version 2, plus courte
fonction isMultiple (entier1 : in entier,
                     entier2 : in entier)
                     renvoie booleen
debut
    renvoie (modulo (entier1, entier2) vaut 0);
fin
```

Utilisation :

```text
si (isMultiple (Nombre, 10))
    …
fsi
```

---

## 4. Exercice : tracer la courbe `y = ax² + bx + c`

> **Supposition** : on dispose de la procédure `traceLigne ()` dont les **spécifications sont ??**

La diapo s'arrête sur cette question : les spécifications de `traceLigne` ne sont **pas données**. C'est un **exercice** : il faut les déduire (probablement les coordonnées des deux extrémités du segment et/ou la couleur), puis écrire l'algorithme qui, pour chaque `x` de l'intervalle, calcule `y` et trace le segment entre le point courant et le point suivant.

> [!warning] Ne pas inventer les specs
> Le support ne précise ni la signature ni le repère. À traiter en TD avec l'énoncé complet ; ne pas apprendre une signature inventée.

---

## ✅ À retenir

- `pour (i variant_de min a max)` : `i` prend **toutes** les valeurs, bornes **incluses** ; `descendant` pour décroître.
- **Interdit** : `sortie` dans un `pour`. **Autorisé** : `continue`.
- `i` déclaré par le `pour` **n'existe pas** après le `ffaire`.
- Piège **`entier_naturel`** : `i >= 0` est **toujours vrai** ; `i <- i - 1` sur un non-signé **déborde**.
- Un **sous-programme** = nom + specs + paramètres + éventuel résultat.
- Paramètres : **`in`** (donnée, non modifiée), **`out`** (résultat, à initialiser tôt), **`in_out`** (les deux).
- **Fonction** = sous-programme qui **renvoie** une valeur ; **prédicat** = fonction qui renvoie un **`booleen`**, nommée `is…`, `has…`, `are…`.
- Les sous-programmes évitent le **copier/coller** et améliorent **lisibilité, maintenance, réutilisabilité**.

### Questions d'auto-évaluation

- [ ] Écrire une boucle `pour` qui affiche les indices d'un tableau de taille `n`.
- [ ] Écrire trois versions du parcours **à l'envers** (indice calculé, `descendant`, `tant_que`).
- [ ] Pourquoi la version `tant_que (i >= 0)` avec `i : entier_naturel` boucle-t-elle ?
- [ ] Pourquoi `sortie` est-il interdit dans un `pour` ? Comment traduire l'exemple du monstre ?
- [ ] `continue` est-il autorisé dans un `pour` ? Pourquoi ?
- [ ] Qu'est-ce que la portée d'une variable ? Donner un exemple avec un bloc.
- [ ] Pourquoi `i` n'est-il pas utilisable après un `pour` ? Quel est le risque du « premier patch » ?
- [ ] Donner les avantages d'un sous-programme (au moins quatre).
- [ ] Différencier paramètre **formel** et **effectif**.
- [ ] Expliquer `in`, `out`, `in_out` et donner un exemple de chacun.
- [ ] Qu'est-ce qu'un prédicat ? Quelle convention de nommage ?
- [ ] Pourquoi faut-il initialiser un paramètre `out` dès que possible ?
