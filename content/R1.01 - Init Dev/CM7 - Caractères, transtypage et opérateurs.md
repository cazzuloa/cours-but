---
ressource: R1.01
type: cours
cm: 7
titre: Caractères, transtypage et opérateurs
auteur: Alain Casali
tags:
  - char
  - transtypage
  - operateurs
  - R1-01
  - Cpp
---

# CM7 — Caractères, transtypage et opérateurs

[[R1.01 - Index|← Index R1.01]] · Suite : [[CM8 - Structs et bibliothèque minGL]]

> [!abstract] En bref
> Les caractères sont des **entiers** : on peut les additionner, les comparer et les convertir. Ce CM explique le **codage ASCII**, le **transtypage** explicite et implicite, puis toute la panoplie d'**allègements d'écriture** du C++ : blocs sans accolades, `+=` / `-=` / `*=`, pré- et post-incrémentation, et opérateur ternaire `? :`.

---

## 1. Les types caractères

### 1.1 Deux familles de codage

| Codage | Taille | Valeurs possibles | Usage |
|--------|--------|-------------------|-------|
| **ASCII** | 1 octet = 8 bits | $2^8 = 256$ caractères max | 128 caractères de base + 128 caractères étendus |
| **Unicode** | 2 octets = 16 bits | $2^{16} = 65\,536$ caractères max | couvre beaucoup plus de langues et de symboles |

En C++, « caractère simple » ne veut pas dire « un seul type » : il existe **trois** types codés sur 1 octet.

| Type | Taille |
|------|--------|
| `char` | 1 octet de 8 bits |
| `signed char` | 1 octet de 8 bits |
| `unsigned char` | 1 octet de 8 bits |

```cpp
cout << sizeof (char);   // 1
```

> [!warning] `sizeof` ≠ `.size()`
> `sizeof` renvoie le **nombre d'octets** occupés par la structure de données en mémoire. `.size()` renvoie le **nombre d'éléments** d'une collection. Ne pas confondre les deux.

> [!note] Le type utilisé en cours
> « Nous n'utiliserons pour commencer que le type `char`. »

### 1.2 Le code ASCII de base

Le code ASCII de base contient **128 caractères** : les codes 0 à 127.

| Plage | Contenu |
|-------|---------|
| 0 – 31 | caractères de contrôle, **non éditables** (dont TAB, LF, CR) |
| 32 | l'espace |
| 48 – 57 | les chiffres `'0'` à `'9'` |
| 65 – 90 | les majuscules `'A'` à `'Z'` |
| 97 – 122 | les minuscules `'a'` à `'z'` |

Les autres codes de la plage 0–127 sont de la ponctuation ou des caractères de contrôle.

Quelques repères binaires/décimaux du support :

| Binaire | Décimal | Caractère |
|---------|---------|-----------|
| `0000 0000` | 0 | caractère de contrôle |
| `0000 1001` | 9 | TAB (`'\t'`) |
| `0000 1010` | 10 | LF / NL (`'\n'`) |
| `0001 1111` | 31 | dernier caractère de contrôle |
| `0010 0000` | 32 | espace |
| `0010 0001` | 33 | `'!'` |
| `0010 0010` | 34 | `'"'` |
| `0011 0000` | 48 | `'0'` |
| `0011 0001` | 49 | `'1'` |
| `0100 0001` | 65 | `'A'` |
| `0100 0010` | 66 | `'B'` |
| `0110 0001` | 97 | `'a'` |
| `0110 0010` | 98 | `'b'` |

> [!warning] Pas d'accent dans l'ASCII de base
> Le code ASCII de base ne contient **aucun caractère accentué**. Les accents relèvent du code ASCII étendu (ou d'Unicode).

Les caractères de contrôle utiles s'écrivent avec un **échappement** :

| Écriture | Nom | Effet |
|----------|-----|-------|
| `'\t'` | TAB | tabulation |
| `'\n'` | LF / NL | passage à la ligne |
| `'\r'` | CR | retour en début de ligne |

### 1.3 Un caractère est une valeur entière

> [!important] Règle fondamentale
> En C++, les caractères, **quel que soit leur type**, sont des **valeurs entières**. On peut donc faire des opérations arithmétiques dessus, et il existe une **relation d'ordre** entre eux.

```cpp
char carac = 'A';
cout << carac;        // affiche A
```

Le caractère `'A'` « vaut » **65** (son code ASCII). Mais l'injecteur `<<` « sait » que c'est le symbole `A` que l'utilisateur veut voir apparaître : il affiche donc `A`, pas `65`.

```cpp
cout << carac + 1;    // affiche 66 (et non 'B')
```

Pourquoi 66 ? Parce que `carac + 1` est une expression de type **entier** (`int`) : l'injecteur affiche la valeur numérique.

### 1.4 L'écart majuscule / minuscule

```cpp
const char kShift = 'a' - 'A';   // 97 - 65 = 32

char carac = 'C';
cout << carac + kShift;          // affiche 99
cout << char (carac + kShift);   // affiche c

carac = 65;
cout << carac;                   // affiche A

carac = 0x41;
cout << carac;                   // affiche A

carac = carac + 1;
cout << carac;                   // affiche B
```

| Expression | Valeur | Affichage |
|------------|--------|-----------|
| `'a' - 'A'` | 32 | — |
| `'A'` | 65 | `A` |
| `0x41` | 65 | `A` |
| `'A' + 1` | 66 | `66` (type entier) |
| `char ('A' + 1)` | 66 | `B` |
| `'C' + 32` | 99 | `99` |
| `char ('C' + 32)` | 99 | `c` |

---

## 2. Le transtypage

### 2.1 Définition

> [!important] Transtypage
> « Conversion » d'une valeur d'un **type source** dans un **type cible**.

Syntaxe :

```cpp
typeCible (valeurDuTypeSource);
```

```cpp
carac = 'C';
cout << carac + kShift        << endl   // 99
     << int  (carac + kShift) << endl   // 99
     << char (carac + kShift) << endl;  // c
```

Les trois lignes n'affichent pas la même chose : sans transtypage, l'expression est déjà un entier ; `int(...)` ne change rien ; `char(...)` force l'interprétation en caractère.

### 2.2 Transtypage explicite

On demande **explicitement** la conversion.

```cpp
string minusc;
for (unsigned i (0); i < 26; i = i + 1)
{
    minusc = minusc + char ('a' + i);
}
cout << minusc << endl;   // abcdefghijklmnopqrstuvwxyz
```

Ici `char ('a' + i)` est indispensable : sans lui, on concaténerait des **nombres** à la chaîne.

### 2.3 Transtypage implicite

Le compilateur convertit **tout seul**, sans qu'on l'écrive. Le parcours d'une `string` avec une référence `unsigned` en est l'exemple type :

```cpp
for (const unsigned & val : minusc)
{
    cout << val << '/';
}
// 97/98/99/100/101/102/103/…
```

Chaque `char` de la chaîne est **implicitement** converti en `unsigned` pour être affiché comme un code ASCII.

### 2.4 Transtypage implicite impossible

Comparer un `int` et un `size_t` (non signé) ne fonctionne pas proprement :

```cpp
for (int i (0); i < minusc.size (); i = i + 1)
    cout << minusc [i];
// warning: comparison between signed and unsigned integer expressions
```

Trois solutions :

```cpp
// 1. transtypage explicite de la taille
for (int i (0); i < int (minusc.size ()); i = i + 1)
    cout << minusc [i];

// 2. transtypage explicite de l'indice
for (int i (0); unsigned (i) < minusc.size (); i = i + 1)
    cout << minusc [i];

// 3. la meilleure : utiliser directement size_t
for (size_t i (0); i < minusc.size (); i = i + 1)
    cout << minusc [i];
```

> [!tip] La bonne habitude
> Pour parcourir une collection, utiliser **`size_t`** : c'est le type renvoyé par `.size()`. On évite ainsi le warning et tout risque de comparaison signé / non signé.

---

## 3. Bloc à instruction unique

### 3.1 Règle

> [!important] Accolades facultatives
> Lorsqu'un bloc contient une **unique instruction**, les accolades `{}` qui l'entourent sont **superflues**.

```cpp
int i;
cin >> i;
if ((i % 2) == 0)
    cout << "le chiffre est pair";
else
    cout << "le chiffre est impair";
```

est équivalent à :

```cpp
int i;
cin >> i;
if ((i % 2) == 0)
{
    cout << "le chiffre" << " est pair";
}
else
{
    cout << "le chiffre" << " est impair";
}
```

### 3.2 Dans un bloc répétitif

```cpp
for (unsigned i (0); i < minusc.size (); i = i + 1)
    cout << minusc [i];
```

### 3.3 Bloc alternatif dans un bloc répétitif

```cpp
for (unsigned i (0); i < 42; i = i + 1)
    if (rand (0, 2) == 0)
        cout << "le chiffre est pair";
    else
        cout << "le chiffre est impair";
```

Le `if … else …` est vu comme **une unique instruction** par le schéma répétitif ; il n'a donc pas besoin d'accolades.

> [!warning] Le `else` se rattache au `if` le plus proche
> Sans accolades, un `else` est associé au `if` **le plus proche**, pas à celui qu'on croit. En cas de doute (ou d'imbrication), remettre les accolades.

---

## 4. Allègement des opérateurs mathématiques

### 4.1 Principe

```cpp
i = i + (expression_entiere);
```

peut s'écrire :

```cpp
i += (expression_entiere);
```

ou plus simplement :

```cpp
i += expression_entiere;
```

```cpp
int i = 25;
int incr;
cin >> incr;
// ...
i += incr * 2;   // ou i = i + (incr * 2);
```

### 4.2 Tous les opérateurs binaires

| Forme longue | Forme courte |
|--------------|--------------|
| `i = i + expr` | `i += expr` |
| `i = i - expr` | `i -= expr` |
| `i = i * expr` | `i *= expr` |
| `i = i / expr` | `i /= expr` |
| `i = i % expr` | `i %= expr` |

### 4.3 Priorités : les parenthèses implicites

```cpp
i = i - (incr - 1);   //  i -= incr - 1;
```

Attention à la **priorité** : l'expression de droite est évaluée **avant** l'affectation, et `incr - 1` est bien entre parenthèses.

```cpp
i = i / (incr * 2);   //  i /= incr * 2;
i = i / incr / 2;     //  équivalent (division à gauche)
```

### 4.4 Le piège `*=` et `/`

```cpp
i = i * (incr / 2);   //  i *= incr / 2;
```

> [!warning] `i *= incr / 2` n'est pas `i = i * (incr / 2)`
> La division entière s'applique **dans l'expression de droite**, mais pas au même endroit selon l'écriture.

Démonstration du support avec `i = 10` et `incr = 5` :

| Écriture | Calcul | Résultat |
|----------|--------|----------|
| `i = i * incr / 2` | `10 * 5 / 2` = `50 / 2` | **25** |
| `i *= incr / 2` | identique à `i = i * incr / 2` | **25** |
| `i = i * (incr / 2)` | `10 * (5 / 2)` = `10 * 2` | **20** |

`i *= incr / 2` est donc **équivalent** à `i = i * incr / 2`, mais **pas** à `i = i * (incr / 2)`.

### 4.5 `+=` et les chaînes

```cpp
string s1 ("Coucou");
string s2 ("moi");
s1 = s1 + (" c'est " + s2);
```

peut s'écrire :

```cpp
s1 += " c'est " + s2;
```

| Élément | Détail |
|---------|--------|
| `+` sur des `string` | **surcharge** de l'opérateur : concaténation |
| `+=` sur des `string` | **surcharge** de l'opérateur arithmétique, même sémantique |

---

## 5. Opérateurs `++` et `--`

### 5.1 Pré-incrémentation / pré-décrémentation

```cpp
i = i + 1;   // incrémentation
i += 1;      // incrémentation
++i;         // incrémentation
```

> [!important] Double effet de `++i`
> 1. `i` est **incrémenté** de 1 ;
> 2. l'expression a pour valeur la **nouvelle** valeur de `i`.

`--i` est l'opérateur de **pré-décrémentation** (même principe, en diminuant).

```cpp
string str ("Bonjour");
unsigned i = 3;

cout << str [++i];   // i vaut 4 → affiche 'o'
cout << str [++i];   // i vaut 5 → affiche 'u'
```

### 5.2 Post-incrémentation / post-décrémentation

```cpp
i++;   // incrémentation
```

> [!important] Double effet de `i++`
> 1. l'expression a pour valeur l'**ancienne** valeur de `i` ;
> 2. **puis** `i` est incrémenté de 1.

`i--` est l'opérateur de **post-décrémentation**.

```cpp
string str ("Bonjour");
unsigned i = 3;

cout << str [i++];   // affiche str[3] = 'j', puis i vaut 4
cout << str [i++];   // affiche str[4] = 'o', puis i vaut 5
```

### 5.3 Effet de bord

> [!warning] Deux `++` dans un même `cout`
> L'ordre d'évaluation des **opérandes** d'un `<<` est de **droite à gauche** en C++ ; l'ordre d'**injection** est de **gauche à droite**. Avec deux `++`, le résultat n'est pas celui qu'on attend naïvement.

```cpp
i = 3;
cout << str [++i] << str [++i];
```

Le support donne l'exécution suivante :

1. évaluation de l'opérande de droite : `i` passe à 4, valeur `str[4]` = `'o'` ;
2. évaluation de l'opérande de gauche : `i` passe à 5, valeur `str[5]` = `'u'` ;
3. injection de gauche à droite : affichage de `'u'` puis de `'o'`.

Ce n'est donc **pas** équivalent à :

```cpp
i = 3;
cout << str [++i] << str [++i];   // 'u' puis 'o'
```

mais à :

```cpp
i = 3;
cout << str [5] << str [4];       // 'u' puis 'o'
```

C'est un **effet de bord** (*side effect*) : une expression modifie une variable pendant son évaluation. À éviter dans une même instruction.

### 5.4 Afficher un mot à l'envers

Version avec pré-décrémentation :

```cpp
string motLu;
cin >> motLu;

for (unsigned i = motLu.size (); i > 0; )
{
    cout << motLu [--i];
}
```

Version sans accolades (une seule instruction) :

```cpp
string motLu;
cin >> motLu;

for (unsigned i = motLu.size (); i > 0; )
    cout << motLu [--i];
```

Version avec post-décrémentation :

```cpp
string motLu;
cin >> motLu;

for (unsigned i = motLu.size (); i-- > 0; )
    cout << motLu [i];
```

> [!note] Pourquoi `size()` et pas `size() - 1` ?
> On part de `size()` (indice **hors** de la chaîne) et on décrémente **avant** d'accéder. Avec `--i`, le premier indice utilisé est `size() - 1`, le dernier caractère.

---

## 6. L'opérateur ternaire `? :`

### 6.1 Utilité : éviter les redondances

Code redondant :

```cpp
char C;
cin >> C;

if (' ' == C || '\t' == C)
    cout << C << " est un espace" << endl;
else
    cout << C << " n'est pas un espace" << endl;
```

On factorise ce qui est commun :

```cpp
char C;
cin >> C;
cout << C;
if (' ' == C || '\t' == C)
    cout << " est";
else
    cout << " n'est pas";
cout << " un espace" << endl;
```

### 6.2 Syntaxe et sémantique

```cpp
expression_1 ? expression_2 : expression_3
```

> [!important] Sémantique
> La valeur renvoyée est **`expression_2`** si `expression_1` est **vraie**, **`expression_3`** sinon.

Contraintes de type :

- `expression_1` doit être de type **booléen** ;
- `expression_2` et `expression_3` doivent être **du même type** ;
- ce type commun est aussi celui du **résultat**.

```cpp
cout << C
     << ((' ' == C || '\t' == C) ? " est" : " n'est pas")
     << " un espace" << endl;
```

### 6.3 Exemples

**Valeur absolue :**

```cpp
double X, AbsX;
cin >> X;

AbsX = (X < 0.0) ? -X : X;
```

**Validation d'un rang :**

```cpp
unsigned validRang (char codeOp, unsigned rang, unsigned max)
{
    return min (rang, ('I' == codeOp) ? max : max - 1);
}
```

Le support montre d'abord la version avec `if … else … return`, puis la version ternaire, et insiste : **ne pas perdre en lisibilité**.

### 6.4 Exemple complet : hexadécimal → décimal

```cpp
#include <cctype>   // toupper(), isxdigit(), isalpha()

short hexa2Dec (const char C)
{
    return (! isxdigit (C)) ? -1
                            : isalpha (C) ? toupper (C) - 'A' + 10
                                          : C - '0';
}
```

Décomposition :

| Cas | Calcul | Exemple |
|-----|--------|---------|
| caractère non hexadécimal | `-1` | `'z'` → `-1` |
| lettre hexadécimale | `toupper (C) - 'A' + 10` | `'b'` → `'B' - 'A' + 10` = 11 |
| chiffre | `C - '0'` | `'7'` → 7 |

### 6.5 Attention au type des deux branches

```cpp
cout << X;
if (X >= 0) cout << " non";
cout << " négatif";
```

Version ternaire **fausse** :

```cpp
cout << X << ((X >= 0) ? " non" : ) << " négatif";   // FAUX : branche vide
```

Version **fausse** aussi (types différents) :

```cpp
cout << X << ((X >= 0) ? " non " : ' ') << "négatif";   // FAUX : string vs char
```

Version **juste** (deux `string`) :

```cpp
cout << X << ((X >= 0) ? " non " : " ") << "négatif";
```

> [!warning] Même type obligatoire
> `" non "` est un **littéral chaîne** (type `const char[]`), `' '` est un **caractère** (`char`). Les deux branches du ternaire doivent avoir le **même type** : `" non "` et `" "` fonctionnent, `" non "` et `' '` non.

---

## 7. QT Creator

*(à venir)*

Le plan de l'amphi annonce une partie « QT Creator », mais le support ne contient qu'une **slide de titre**, sans contenu. Aucun détail n'est donné dans ce CM.

---

## ✅ À retenir

- Deux codages : **ASCII** (1 octet, 256 valeurs) et **Unicode** (2 octets, 65 536 valeurs) ; l'ASCII de base fait 128 caractères, **sans accents**.
- Trois types d'un octet : `char`, `signed char`, `unsigned char` ; `sizeof (char) == 1`.
- Un `char` est un **entier** : `'A'` vaut 65, `'a'` vaut 97, l'écart majuscule/minuscule est **32**.
- `cout << carac` affiche le **symbole** ; `cout << carac + 1` affiche le **nombre** (66).
- **Transtypage** : `typeCible (valeur)`. Explicite quand on l'écrit, implicite quand le compilateur le fait seul.
- Comparer `int` et `size_t` provoque un warning : utiliser **`size_t`** pour parcourir une collection.
- Une accolade est **superflue** pour un bloc à **une seule** instruction ; attention au rattachement du `else`.
- `+=`, `-=`, `*=`, `/=`, `%=` abrègent `i = i op expr`.
- **`i *= incr / 2`** vaut `i = i * incr / 2` (**25**), pas `i = i * (incr / 2)` (**20**).
- `++i` renvoie la **nouvelle** valeur ; `i++` renvoie l'**ancienne** puis incrémente.
- Deux `++` dans un même `cout` = **effet de bord** : évaluation des opérandes de droite à gauche.
- Ternaire : `condition ? valeur_si_vrai : valeur_si_faux` ; les deux valeurs doivent être **du même type**.

### Questions d'auto-évaluation

- [ ] Combien d'octets et de valeurs possibles pour l'ASCII et pour l'Unicode ?
- [ ] Quels sont les codes ASCII de `'0'`, `'A'`, `'a'` et de l'espace ?
- [ ] Pourquoi n'y a-t-il pas de caractère accentué dans l'ASCII de base ?
- [ ] Que vaut `'a' - 'A'` ? À quoi sert cette constante ?
- [ ] Pourquoi `cout << carac + 1` affiche-t-il `66` et non `B` ?
- [ ] Quelle est la différence entre transtypage explicite et implicite ? Donner un exemple de chaque.
- [ ] Pourquoi `i < minusc.size()` avec `int i` provoque-t-il un warning ? Trois solutions ?
- [ ] Quand peut-on enlever les accolades d'un bloc ?
- [ ] Pourquoi `i *= incr / 2` et `i = i * (incr / 2)` donnent-ils des résultats différents ?
- [ ] Quelle est la valeur de `++i` ? celle de `i++` ? Que vaut `i` après chacune ?
- [ ] Pourquoi `cout << str[++i] << str[++i]` n'affiche-t-il pas ce qu'on croit ?
- [ ] Écrire une boucle qui affiche un mot à l'envers avec `--i` puis avec `i--`.
- [ ] Donner la syntaxe et les contraintes de types de l'opérateur ternaire.
- [ ] Pourquoi `(X >= 0) ? " non " : ' '` ne compile-t-il pas ?
