---
ressource: R1.01
type: cours
cm: 6
titre: Caractères, chaînes, boucles et vector
auteur: Alain Casali
tags:
  - char
  - string
  - boucles
  - vector
  - R1-01
  - Cpp
---

# CM6 — Caractères, chaînes, boucles et vector

[[R1.01 - Index|← Index R1.01]] · Suite : [[CM7 - Caractères, transtypage et opérateurs]]

> [!abstract] En bref
> Le passage complet du pseudo-code au C++ : `char` et `string`, les boucles `while`, `do while` et `for`, les ruptures de séquence (`break`, `continue`), le `switch/case`, les sous-programmes, puis le `vector` — avec **le bug du débutant** (`v[0] = 0` sur un vecteur vide).

---

## 1. Le type caractère

### 1.1 Identificateur et valeurs

| Élément | `char` |
|---------|--------|
| Valeurs | n'importe quel caractère **ASCII** entre simples quotes `'…'` |
| Comparaisons | `<`, `<=`, `>`, `>=` → `bool` |
| Identité | `==`, `!=` → `bool` |

**ASCII** = *American Standard Code for Information Interchange*.

### 1.2 Élément suivant / précédent

> [!warning] `succ` et `pred` n'existent pas en C++
> En algorithmique on dispose de `succ` et `pred`. En C++, on applique **`++`** ou **`--`** à gauche de la variable.

```cpp
char c = 'a';
char next = ++c;      // pré-incrémentation
cout << next;         // b
```

```cpp
char c = 'b';
char prev = --c;
cout << prev;         // a
```

```cpp
char next = ++'a';    // ERREUR de compilation
```

> [!important] Pourquoi `++'a'` est illégal
> On ne peut pas incrémenter un **littéral** : `++` modifie une **variable**. `'a'` est une constante.

### 1.3 Fonctions applicables

Prérequis :

```cpp
#include <cctype>
using namespace std;
```

| Fonction | Résultat |
|----------|----------|
| `tolower (aCharacter)` | le caractère en **minuscule** si c'est une majuscule, sinon le caractère inchangé |
| `toupper (aCharacter)` | le caractère en **majuscule** si c'est une minuscule, sinon le caractère inchangé |

```cpp
cout << toupper (next);    // B
cout << tolower ('C');     // c
```

### 1.4 Prédicats applicables

| Prédicat | Résultat |
|----------|----------|
| `islower (aCharacter)` | vrai si le caractère est une **minuscule** |
| `isalpha (aCharacter)` | vrai si le caractère est **alphabétique** |

```cpp
if (islower (next))
{
    cout << "caractere minuscule";
}
```

> [!note] Nuance sur `isalpha`
> Le support écrit « vrai si le caractère est **alphanumérique** ». En réalité, `isalpha` teste **alphabétique** (lettre) ; c'est **`isalnum`** qui teste alphabétique **ou** chiffre. Retenir `isalpha` = lettre.
>
> D'autres prédicats : <http://www.cplusplus.com/reference/cctype/?kw=cctype>.

### 1.5 Caractères spéciaux

| Caractère | Signification |
|-----------|---------------|
| `'\t'` | tabulation |
| `'\n'` | *Line Feed* (saut de ligne) |
| `'\r'` | *Carriage Return* (retour chariot) |
| `'\''` | le caractère `'` |

> [!note] Fin de ligne Unix vs Windows
> Sous Unix, le retour à la ligne dans un fichier est **`'\n'`**. Sous Windows, c'est la **combinaison `'\r' '\n'`**. Cela explique certains bugs de comptage de caractères (voir [[CM10 - Flux, tri et tests|CM10]]).

### 1.6 Encodage des caractères

- `char` : **1 octet** → `2⁸ = 256` valeurs possibles.
- `wchar_t` : **4 octets** → `2³²` valeurs possibles.

Encodages populaires en France :

- **Unicode** (compatible avec UTF-8, UTF-16 et UTF-32) ;
- **ISO 8859-15**.

> **UTF** = *Universal Transformation Format*.

---

## 2. Le type `string`

### 2.1 Identificateur et valeurs

| Élément | `string` |
|---------|----------|
| Valeurs | n'importe quelle suite de caractères ASCII entre **doubles quotes** `"…"` |
| Comparaisons | `<`, `<=`, `>`, `>=` → `bool` (**ordre lexicographique**) |
| Identité | `==`, `!=` → `bool` |

### 2.2 Taille d'une string

`size ()` produit un **`size_t`**, qui correspond à un **`unsigned long long`**.

```cpp
string str;
cin >> str;              // Mario
cout << str.size ();     // 5
```

```cpp
// C++17 : fonction libre size()
cout << size (str);      // 5
```

> [!warning] `.size()` n'est pas `sizeof`
> `sizeof` donne le nombre d'**octets** occupés par la **structure de données** ; `.size()` donne le nombre d'**éléments** de la chaîne. Ne pas confondre (voir [[CM7 - Caractères, transtypage et opérateurs|CM7]]).

### 2.3 Accès à un élément

On accède au **(i+1)ᵉ** élément avec `str[i]`.

- première case : indice **0** ;
- dernière case : indice **`str.size() - 1`** ;
- **aucun contrôle** de validité de l'indice, ni en lecture ni en écriture.

```cpp
// Afficher chaque caractère sur une ligne
for (size_t i = 0; i < str.size (); i = i + 1)
{
    cout << str[i] << endl;
}
```

### 2.4 Concaténation `+`

L'opérateur `+` est **binaire**. Pour produire une `string`, il faut :

- à gauche un **littéral ou une variable** (`lhs`) ;
- à droite un **littéral ou une variable** (`rhs`).

| `lhs` | `rhs` | Disponibilité |
|-------|-------|---------------|
| `char` | `string` | C++11 |
| `string` | `char` | C++11 |
| `string` | `string` | C++ |

```cpp
str = "0123456789";
cout << str.size ();              // 10
str = str + "aeiouy\"";
cout << str.size ();              // 17
```

---

## 3. Boucle `while`

### 3.1 `while` = `tant_que`

```text
// Algorithmique
tant_que (condition)
faire
    ...
ffaire
```

```cpp
// C++
while (condition)
{
    ...
}
```

> [!warning] Pas de `;` après `while (condition)`
> `while (c);` signifie « boucle vide tant que `c` est vrai » → souvent une **boucle infinie**.

```cpp
unsigned long long i;      // ou size_t i
i = 0;
while (i < str.size ())
{
    cout << str[i] << endl;
    i = i + 1;
}
```

### 3.2 `do … while` = `repeter … tant_que`

```text
// Algorithmique
repeter
    ...
tant_que (condition)
```

```cpp
// C++
do
{
    ...
}
while (condition);
```

> [!important] Le `;` est obligatoire après `while (condition);`

```cpp
unsigned long long i;
i = 0;
do
{
    cout << str[i] << endl;
    i = i + 1;
}
while (i < str.size ());
```

> [!danger] Ce code est faux si `str` est vide
> Le `do … while` s'exécute **au moins une fois** : si `str` est de taille 0, on accède à `str[0]`, qui n'existe pas.

---

## 4. Boucle `for`

### 4.1 Le `for` algorithmique n'existe pas tel quel

```text
pour (nomVar variant_de borneMin a borneMax)
faire
    ...
ffaire
```

> « Cette boucle **n'existe pas telle quelle** en C++, mais la boucle `for` existe. »

### 4.2 Profil de la boucle `for`

```cpp
for ([expression1]; [expression2]; [expression3])
{
    instruction(s);
}
```

| Élément | Rôle |
|---------|------|
| `expression1` | instruction **unique**, exécutée **avant** l'entrée dans l'itération : **initialisation** |
| `expression2` | instruction **unique et booléenne** : **condition de continuation** |
| `expression3` | instruction **unique**, exécutée **avant** la prochaine itération |

> [!warning] Danger
> Éviter de mettre un `;` après `for ( ; ;)`. `for (;;);` est une boucle infinie **vide**.

### 4.3 Exemple et équivalences

```cpp
for (size_t i = 0; i < str.size (); i = i + 1)
{
    cout << str[i] << endl;
}
```

Équivalent à :

```cpp
for (size_t i = 0; i < str.size (); )
{
    cout << str[i] << endl;
    i = i + 1;
}
```

**Mais pas** à :

```cpp
size_t i = 0;
for ( ; i < str.size (); i = i + 1)
{
    cout << str[i] << endl;
}
```

> [!important] « Non : portée de variable »
> Dans la première version, `i` est déclaré **dans** le `for` : il n'existe pas après. Dans la dernière, `i` existe **avant** et **après**. Ce n'est donc pas équivalent.

### 4.4 `size_t` plutôt que `int`

```cpp
// À éviter
for (int i = 0; i < str.size (); i = i + 1)
```

> [!warning] « Non »
> Si la chaîne est de taille **≥ `max (int) + 1`**, un `int` ne peut pas atteindre ces indices. Utiliser **`size_t`** (ou `unsigned long long`).

### 4.5 Effet de bord

```cpp
int i (0);
...
i = -1;
for ( ; i < str.size (); i = i + 1)
{
    cout << str[i] << endl;
}
```

> [!danger] Accès à la case d'indice `−1`
> `i` est signé et vaut `−1`. La condition `i < str.size()` est **vraie** (car `size()` est non signé, et `−1` est converti en un très grand nombre non signé… ou comparé selon les règles de conversion). On accède alors à `str[-1]` : **comportement indéfini**.

---

## 5. Boucle infinie

```text
// Algorithmique
boucle
    instruction(s);
fboucle
```

```cpp
// C++
for ( ; ; )
{
    instruction(s);
}
```

ou

```cpp
while (true)
{
    instruction(s);
}
```

> **Pas de condition de sortie explicite** : il faut un `break` (ou un `return`) quelque part.

---

## 6. Rupture de schéma répétitif

### 6.1 `break`

```text
// Algorithmique
boucle
    instruction(s);
    si (condition) sortie;
    instruction(s);
fboucle
```

```cpp
// C++
for ([e1]; [e2]; [e3])
{
    instruction(s);
    if (condition) break;
    instruction(s);
}
```

> On sort de la boucle **dès que `condition` est vraie**.

Exemple : générer un entier naturel `> 10`.

```text
boucle
    afficher ("Entrer une valeur > 10 : ");
    saisir (N);
    si (N > 10) sortie;
    afficher (" >10 svp!");
fboucle
```

```cpp
unsigned N;
while (true)
{
    cout << "Entrer une valeur > 10 : ";
    cin >> N;
    if (N > 10) break;
    cout << " >10 svp!";
}
```

### 6.2 `continue`

```text
// Algorithmique
boucle
    instruction(s);
    si (condition) continue;
    instruction(s);
fboucle
```

```cpp
for ([e1]; [e2]; [e3])
{
    instruction(s);
    if (condition) continue;
    instruction(s);
}
```

> On exécute `e3` (si présent) **puis** `e2` dès que `condition` est vraie. Autrement dit, on **saute le reste du corps** et on passe à l'itération suivante.

Exemple : afficher une lettre sur deux.

```cpp
// Deux blocs
for (size_t i = 0; i < str.size (); i = i + 1)
{
    if (0 == i % 2)
    {
        cout << str[i];
    }
}
```

```cpp
// Un bloc, avec continue
for (size_t i = 0; i < str.size (); i = i + 1)
{
    if (0 != i % 2) continue;
    cout << str[i];
}
```

> [!tip] Lisibilité
> **2 blocs ⇒ 2 indentations ⇒ 2 fois moins lisible.** **1 bloc ⇒ 1 indentation ⇒ plus lisible.** C'est l'argument du cours en faveur de `continue`.

### 6.3 Combinaison `break` / `continue`

```cpp
while (ICanFrag ())
{
    if (kill ()) continue;
    if (someoneNearMe ()) break;
}
findAnotherSpot ();
```

---

## 7. Schéma séquentiel `switch / case`

### 7.1 Correspondance

```text
choix_sur Variable_de_choix entre
  cas Ens_1 :
    Sequ_1;
  cas Ens_2 :
    Sequ_2;
  cas Ens_3 :
    Sequ_3;
  autre :
    Sequ_4;
fchoix
```

```cpp
switch (varIdent)
{
    case literal1:
        instruction1;
        break;
    case literal2:
        instruction2;
        break;
    ...
    default:
        instruction4;
}
```

- `break` permet la **sortie du bloc courant** (donc du `switch`).
- `varIdent` doit être de type **entier** ou **caractère**.
- **Pas de `break` après la dernière instruction** (`default`).

### 7.2 Exemple : déplacement (touches uniques)

```cpp
char c;
cin >> c;

switch (c)
{
    case 'a':
        moveLeftward ();
        break;
    case 'z':
        moveUpward ();
        break;
    case 'e':
        moveRightward ();
        break;
    case 's':
        moveDownward ();
        // break;
}
```

> [!note] Pas d'accolades pour les `case`
> On écrit `case 'a':` puis les instructions, **sans `{ }`**.

### 7.3 Touches multiples

```cpp
switch (c)
{
    case 'A':
    case 'a':
        moveLeftward ();
        break;
    case 'Z':
    case 'z':
        moveUpward ();
        break;
    ...
}
```

Ou plus élégant :

```cpp
switch (tolower (c))
{
    case 'a': moveLeftward ();  break;
    case 'z': moveUpward ();    break;
    case 'e': moveRightward (); break;
    case 's': moveDownward ();
}
```

> [!danger] L'oubli de `break` « tombe » dans le `case` suivant
> Si le `case 'a'` n'a pas de `break`, l'exécution **continue** dans le `case 'z'` : on appelle `moveLeftward()` **puis** `moveUpward()`. La diapo le montre : taper `a` provoque « déplacement gauche » **et** « déplacement haut ». C'est le **fallthrough**, volontaire ici pour regrouper des `case`, accidentel sinon.

### 7.4 `break` dans un `switch` **dans** une boucle

```cpp
char c;
cin >> c;
for ( ; 'q' != tolower (c); cin >> c)
{
    switch (tolower (c))
    {
        case 'a': moveLeftward ();  break;
        case 'z': moveUpward ();    break;
        case 'e': moveRightward (); break;
        case 's': moveDownward ();  break;
    }
    ...
}
```

> [!important] `break` sort de la structure **la plus interne**
> Ici, `break` sort du `switch`, **pas** de la boucle. Ce n'est **pas du tout équivalent** à une série de `if … break` dans la boucle : dans ce cas, `break` sortirait de la **boucle**.

---

## 8. Les sous-programmes

### 8.1 Procédure

```text
// Algorithmique
procedure nomProc (variable1 : [in | out | in_out] type1,
                   variable2 : [in | out | in_out] type2,
                   ...);
```

```cpp
// C++
void procName (type1 var1, type2 var2, ...)
{
    ...
}
```

Exemple :

```text
procedure affichBonjour ()
debut
    afficher ("bonjour");
    ligne_suivante;
fin
```

```cpp
void showHello ()
{
    cout << "bonjour" << endl;
} // showHello()
```

### 8.2 Fonction

```text
// Algorithmique
fonction nomFonct (variable1 : [in | out | in_out] type1, ...)
                  renvoie TypeDeRetour;
```

```cpp
// C++
returnType fctName (type1 var1, ...)
{
    ...
    return ...;
}
```

> À l'intérieur d'une fonction C++, l'instruction algorithmique **`renvoie`** se traduit par **`return`**.

```cpp
string showHello ()
{
    cout << "bonjour" << endl;
    return "bonjour";
} // showHello()
```

### 8.3 Passage de paramètres « données » (`in`)

```text
// Algorithmique
SP nomSP (variable1 : in type1,
          variable2 : in type2, ...)
         [renvoie typeDeRetour];
```

```cpp
// C++
[returnType | void] fctName (const type1 & var1,
                             const type2 & var2, ...);
```

> [!important] `const type &`
> En C++, un paramètre **`in`** se traduit par **`const T &`** :
> - `const` interdit la modification ;
> - `&` évite la **copie** (passage par référence), donc gagne en performance sur les grosses structures comme `string` ou `vector`.

```cpp
unsigned spaceCount (const string & str)
{
    unsigned cpt = 0;
    for (size_t i = 0; i < str.size (); i = i + 1)
    {
        if (isspace (str[i]))
            cpt = cpt + 1;
    }
    return cpt;
} // spaceCount()
```

### 8.4 Passage de paramètres « donnés-résultats » (`out` / `in_out`)

```cpp
// C++
[returnType | void] fctName (type1 & var1,
                             type2 & var2, ...);
```

> Un paramètre **`out`** ou **`in_out`** se traduit par une **référence non `const`** : `T &`.

```cpp
void rand (const int & min, const int & max, int & res)
{
    srand (time (NULL));
    res = min + rand () % (max - min);
} // rand()
```

> [!note] `res` est un paramètre `out`
> `min` et `max` sont `const &` (données), `res` est `int &` (résultat). L'appelant doit déclarer `res` avant l'appel ; le sous-programme l'écrit.

---

## 9. Cosmétique

### 9.1 `setw ()` — cadrage à droite

Prérequis :

```cpp
#include <iomanip>
using namespace std;
```

```cpp
/* undefined */ setw (int n);
```

> L'injection de `setw (Nb)` provoque l'affichage de la variable ou du littéral qui le suit sur **`Nb` caractères**, **cadrés à droite**. Les caractères manquants sont remplacés par des **espaces**.

```cpp
int a (10), b (20), c (30), d (40);
cout << setw (4) << a << setw (4) << b << endl;
cout << setw (4) << c << setw (4) << d << endl;
```

```text
  10  20
  30  40
```

### 9.2 `setfill ()`

```cpp
/* undefined */ setfill (char c);
```

> `setfill ()` s'utilise **avant** `setw ()`. Il remplace les espaces par le caractère passé en paramètre. **`setfill ()` a un effet permanent.**

```cpp
cout << setfill ('x') << setw (4) << a
     << setfill ('?') << setw (4) << b
     << endl
     << setw (4) << c
     << setfill (' ') << setw (4) << d
     << endl;
```

```text
xx10??20
??30  40
```

> [!warning] Effet permanent
> Après `setfill ('?')`, **tous** les `setw` suivants utilisent `?` jusqu'au prochain `setfill`. C'est pourquoi il faut parfois **remettre l'espace** : `setfill (' ')`.

---

## 10. Le type `vector` (tableau de taille variable)

### 10.1 Prérequis et identificateur

```cpp
#include <vector>
using namespace std;
```

```text
// Algorithmique
tableau_de unType;
```

```cpp
// C++
vector <aType>
```

```cpp
vector <int> vInt;
vector <float> vFloat;
```

> **La taille du vecteur est nulle lors de sa création.**

### 10.2 Taille

```cpp
cout << vInt.size ();    // 0
```

```cpp
// C++17
cout << size (v);        // 0
```

### 10.3 Insertion en fin de vecteur

```cpp
vInt.push_back (0);
vInt.push_back (1);
...
vInt.push_back (9);
cout << vInt.size ();    // 10
```

> `push_back` insère un élément **compatible** en fin de vecteur et **redimensionne automatiquement**. La méthode existe aussi pour les `string`.

```cpp
for (int insVal (0); insVal < 10; insVal = insVal + 1)
{
    vInt.push_back (insVal);
}
cout << vInt.size ();    // 10
```

```cpp
// 10 valeurs saisies au clavier
for (size_t nbVal = 0; nbVal < 10; nbVal = nbVal + 1)
{
    int insVal;
    cin >> insVal;
    vInt.push_back (insVal);
}
```

### 10.4 Redimensionnement

```cpp
vInt.resize (42);
```

Si l'ancienne taille est **inférieure** à la nouvelle (pas de troncature), les nouvelles valeurs sont les **valeurs par défaut** :

| Type | Valeur par défaut |
|------|-------------------|
| Entier | `0` |
| Réel | `0.0` |
| `string` | chaîne vide |
| `char` | `'\0'` |

> [!note] Troncature
> Si la nouvelle taille est **inférieure**, les cases en trop sont **supprimées** (comme `redimensionner` en algo).

### 10.5 Accès à un élément

- première case : indice **0** ;
- dernière case : indice **`size() - 1`** ;
- **aucun contrôle** de validité.

```cpp
for (size_t i = 0; i < vInt.size (); i = i + 1)
{
    cout << vInt[i];
}
```

> [!danger] « Et surtout pas `cout << vInt;` »
> Un `vector` **ne s'affiche pas** d'un coup : il faut une boucle (ou la boucle de collection, section 11).

### 10.6 La boulette du débutant

```cpp
vector <int> vInt;
vInt[0] = 0;
```

> [!danger] Segmentation fault
> **Compilation** : pas d'erreur. **Exécution** : `Segmentation fault : 11` (anciennement « écran bleu »).
>
> `vInt` est déclaré mais **non dimensionné** : la case d'indice `0` **n'existe pas**. Il faut `vInt.push_back (0);` ou `vInt.resize (1);` **avant** d'écrire `vInt[0]`.

### 10.7 Recopier un tableau dans un autre

```cpp
vector <int> vInt2;
```

**Solution 1** — boucle :

```cpp
for (size_t i = 0; i < vInt.size (); i = i + 1)
{
    vInt2.push_back (vInt[i]);
}
```

**Solution 2** — affectation :

```cpp
vector <int> vInt2;
vInt2 = vInt;
// ou : vector <int> vInt2 = vInt;
```

**Solution 3** — construction par copie :

```cpp
vector <int> vInt2 (vInt);
```

---

## 11. Boucle `for` pour les collections

En algorithmique, on aimerait :

```text
pour_chaque (valeur dans idTableau)
faire
    ...
ffaire
```

En C++ :

```cpp
for ([const] Type & varIdent : collectionVarIdent)
{
    ...
}
```

- **`const` DOIT être présent** si aucune modification d'une des cases de la collection n'est faite ;
- `varIdent` et `collectionVarIdent` doivent être de **type compatible**.

```cpp
// Afficher
for (const int & val : vInt)
{
    cout << val << '\t';
}
```

```cpp
// Saisir
for (int & val : vInt)
{
    cin >> val;
}
```

> [!warning] Taille définie avant l'entrée
> `vInt` doit avoir une **taille définie avant** la boucle. Un `vector` vide ne produit **aucune** itération : le `cin >> val` ne sera jamais exécuté.

---

## ✅ À retenir

- `char` est un **entier** codé sur 1 octet ; `++` / `--` remplacent `succ` / `pred`.
- `tolower`, `toupper` (fonctions) ; `islower`, `isalpha` (prédicats) ; `#include <cctype>`.
- `string` : `size()` renvoie un `size_t`, accès `[i]` **sans contrôle**, concaténation `+`.
- `while` = `tant_que` (test **avant**) ; `do … while` = `repeter … tant_que` (test **après**, **`;` obligatoire**).
- `for (init ; condition ; incrément)` : les trois expressions sont **optionnelles**.
- `size_t` pour parcourir une collection ; `int` peut **déborder**.
- `for ( ; ; )` et `while (true)` = **boucle infinie**.
- `break` sort de la structure **la plus interne** ; `continue` passe à l'itération suivante.
- `switch` : `break` obligatoire entre les `case`, `default` pour le reste, types **entier ou caractère**.
- Paramètre `in` → **`const T &`** ; `out` / `in_out` → **`T &`**.
- `setw(n)` cadre à droite ; `setfill(c)` change le caractère de remplissage (**effet permanent**).
- `vector` : taille **0** à la création, `push_back`, `resize`, `[]` sans contrôle, **copie** par `=` ou construction.
- `v[0] = 0` sur un `vector` vide **compile** mais **plante**.

### Questions d'auto-évaluation

- [ ] Pourquoi `++'a'` ne compile-t-il pas ?
- [ ] Quelle est la différence entre `islower` et `isalpha` ? Que teste réellement `isalpha` ?
- [ ] Combien d'octets pour `char` et `wchar_t` ? Combien de valeurs possibles ?
- [ ] Que renvoie `str.size()` comme type ? Pourquoi est-ce important dans une boucle `for` ?
- [ ] Écrire une boucle `while` puis un `do … while` qui parcourt une `string`.
- [ ] Pourquoi le `do … while` est-il faux sur une chaîne vide ?
- [ ] Quelles sont les trois parties du `for` ? Lesquelles sont optionnelles ?
- [ ] Différencier la portée de `i` dans `for (size_t i = 0; …)` et dans `size_t i = 0; for (; …)`.
- [ ] Que fait `break` dans un `switch` imbriqué dans une boucle ? Et dans un `if` ?
- [ ] Pourquoi un `case` sans `break` peut-il exécuter le `case` suivant ?
- [ ] Traduire `in`, `out`, `in_out` en signatures C++.
- [ ] Que fait `setfill` après la fin de la ligne ? Pourquoi faut-il parfois le réinitialiser ?
- [ ] Pourquoi `vInt[0] = 0` sur un `vector` vide est-il dangereux ?
- [ ] Donner trois façons de copier un `vector`.
- [ ] Écrire une boucle de collection pour afficher puis pour saisir un `vector`.
