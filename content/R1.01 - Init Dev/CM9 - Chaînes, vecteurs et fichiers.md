---
ressource: R1.01
type: cours
cm: 9
titre: Chaînes, vecteurs et fichiers
auteur: Alain Casali
tags:
  - string
  - vector
  - fichiers
  - R1-01
  - Cpp
---

# CM9 — Chaînes, vecteurs et fichiers

[[R1.01 - Index|← Index R1.01]] · Suite : [[CM10 - Flux, tri et tests]]

> [!abstract] En bref
> Rappel des `struct`, puis **initialisation** de `string` et de `vector`. Le cœur du CM : les **flux sur fichiers** — `ofstream` en sortie, `ifstream` en entrée, les modes d'ouverture, `is_open`, `eof`, `getline`. Et trois petits programmes à la `wc` : compter les mots, les caractères et les lignes d'un fichier.

---

## 1. Rappel : les `struct`

Le CM reprend le bloc `struct` de l'amphi précédent (voir [[CM8 - Structs et bibliothèque minGL]] pour le détail).

```cpp
struct pos {
  unsigned abs;
  unsigned ord;
}; //pos
```

| Point | Règle |
|-------|-------|
| Définition | `struct nom { type champ; … };` — **`;` final obligatoire** |
| Déclaration | `pos aPos;` |
| Accès | notation pointée : `aPos.abs = 10;` |
| Initialisation | à la volée : `pos aPos {10, 10};` |
| Tri | `sort (v.begin(), v.end(), critere);` avec un prédicat `bool (const pos &, const pos &)` |

> [!note] Différence avec le CM8
> Les slides « structs » sont **identiques** dans les amphis 08 et 09. Elles servent ici de rappel avant d'attaquer `string`, `vector` et les fichiers.

---

## 2. Le type `string` : initialisation

```cpp
string s1;
string s2 (10);
string s2 (10, 'c');
```

| Déclaration | Contenu |
|-------------|---------|
| `string s1;` | chaîne **vide** (taille 0) |
| `string s2 (10);` | chaîne de **taille 10** (10 caractères nuls) |
| `string s2 (10, 'c');` | chaîne contenant **10 fois** le caractère `'c'` |

> [!warning] Deux coquilles du support
> 1. La même variable `s2` est déclarée **deux fois** : le code tel qu'il est écrit ne compile pas.
> 2. La slide ne donne que **deux** légendes pour **trois** déclarations : celle de `string s2 (10)` manque.
>
> Le fond à retenir reste la forme `string (taille, caractère)`.

---

## 3. Le type `vector` : initialisation

```cpp
vector<int> v1;
vector<int> v2 (10);
vector<int> v3 (10, -1);
vector <string> v4 (10, string (100));
vector <string> v5 (10, string (100, 'c'));
```

| Déclaration | Contenu |
|-------------|---------|
| `vector<int> v1;` | tableau d'entiers de **taille 0** |
| `vector<int> v2 (10);` | taille **10** (cases initialisées à 0) |
| `vector<int> v3 (10, -1);` | taille **10**, chaque case vaut **−1** |
| `vector<string> v4 (10, string (100));` | 10 cases, chacune une `string` de **taille 100** |
| `vector<string> v5 (10, string (100, 'c'));` | 10 cases, chacune une `string` de 100 fois `'c'` |

> [!warning] Légendes mélangées
> Dans le support, les légendes ne sont pas dans l'ordre des déclarations (la description de `v2` et celle de `v3` sont inversées). Le tableau ci-dessus rétablit la correspondance logique.

### 3.1 Vecteurs de `struct`

```cpp
vector <pos> v6 (10, pos);
vector <pos> v7 (10, pos (10,10));

vector <pos> v8;
for (size_t i (0); i < 10; ++i)
    v8.push_back (pos (i, 10));
```

| Déclaration | Contenu |
|-------------|---------|
| `v6 (10, pos)` | 10 positions valant **(0, 0)** |
| `v7 (10, pos (10, 10))` | 10 positions valant **(10, 10)** |
| `v8` + `push_back` | 10 positions valant **(i, 10)** |

> [!warning] `pos` sans parenthèses
> Le support écrit `vector <pos> v6 (10, pos);`. En C++ il faut construire l'objet : `pos ()` (ou `pos {}`). Tel quel, `pos` seul ne compile pas.

> [!tip] `push_back` avec une position calculée
> `v8.push_back (pos (i, 10));` construit une `pos` à la volée à chaque tour : c'est la façon idiomatique de remplir un vecteur dont la taille n'est pas connue à l'avance.

---

## 4. Flux sur un fichier de sortie

### 4.1 Pourquoi ne pas rediriger la sortie standard ?

**Solution 1 :** rediriger la sortie standard vers un fichier.

```bash
a.out > OutFile
```

Problèmes :

- comment écrire dans un fichier depuis une **interface graphique** (pas d'accès à la console) ?
- comment écrire dans **plusieurs fichiers en même temps** ? On ne peut faire que **2 redirections** : la sortie standard et le flux d'erreur.

**Solution 2 :** utiliser les **flux de sortie**.

### 4.2 Déclaration

```cpp
#include <fstream>
using namespace std;

ofstream ofs;
```

`ofstream` = **O**utput **F**ile **STREAM**.

### 4.3 Ouverture

```cpp
ofs.open (const string & fileName [, Mode XXX]);
```

On **lie** le flux de sortie `ofs` au fichier `fileName` selon le mode spécifié.

> [!important] Chemin relatif
> `fileName` est le chemin **relatif** au fichier souhaité. Le « point de départ » du chemin relatif est **l'exécutable**.

**Mode par défaut** (aucun mode spécifié) :

- le fichier texte est accessible en **écriture** ;
- si le fichier **n'existe pas**, il est **créé** ;
- s'il existe, les informations sont **ajoutées avant** celles qui existaient déjà.

```cpp
ofs.open ("test.txt");
```

> [!warning] Incohérence du support sur le mode par défaut
> Un `ofstream` ouvert **sans mode** utilise en réalité `ios_base::out | ios_base::trunc` : le fichier existant est **écrasé** (tronqué), pas complété. La phrase « les informations sont ajoutées avant celles qui existaient déjà » décrit un comportement de type `app`, pas le défaut.
>
> Pour **ajouter** réellement à la fin d'un fichier existant, il faut ouvrir en mode `app` :
>
> ```cpp
> ofstream ofs ("test.txt", ios_base::app);
> ```

### 4.4 Déclaration et ouverture en une fois

```cpp
ofstream ofs (const string & filename [, Mode XXX]);

ofstream ofs ("test.txt");
```

### 4.5 Les modes d'ouverture

Tous les modes sont des **constantes de la classe `ios_base`**. On y accède par `ios_base::modeName`.

| Mode | Signification | Accès |
|------|---------------|-------|
| `out` | output (sortie) | accès au flux en **écriture** |
| `in` | input (entrée) | accès au flux en **lecture** |
| `binary` | binary (binaire) | opérations d'E/S en **binaire** et non en texte |
| `ate` | at end (à la fin) | positionne la tête de lecture **à la fin** du fichier |
| `app` | append (ajout) | les ajouts se font **à la fin** et non au début |
| `trunc` | truncate (troncature) | **supprime** le contenu du fichier |

```cpp
ofstream ofs ("test.txt", ios_base::out | ios_base::binary);
```

> [!note] Le `|`
> Le caractère `|` permet d'**enchaîner** les options (OU binaire).

### 4.6 Fermeture

```cpp
ofs.close ();
```

> [!warning] `trunc` et `app` sont incompatibles
> Un flux de sortie ne peut pas avoir **en même temps** les modes `trunc` et `app` : l'un vide le fichier, l'autre veut écrire à la fin.

### 4.7 Écrire dans un flux de sortie

On utilise l'**injecteur** `<<`, exactement comme pour la console (qui est un cas particulier de flux de sortie).

L'injecteur fonctionne :

- pour **tous les types** (`int`, `char`, `bool`, …) ;
- pour **certaines classes** (`string`).

```cpp
ofs << 10 << endl;
ofs << true << endl;
ofs << 'a' << endl;
ofs << "coucou" << endl;
```

Contenu de `test.txt` :

```text
10
1
a
coucou
```

> [!note] `true` s'écrit `1`
> Sans `boolalpha`, un `bool` est injecté sous forme numérique : `true` → `1`, `false` → `0`.

### 4.8 On ne peut pas injecter un `vector`

```cpp
vector <int> v (10);
ofs << v;
```

```text
error: cannot bind 'std::basic_ostream<char>' lvalue to
       'std::basic_ostream<char>&&'
```

> [!important] La seule solution
> Injecter **chaque élément** du vecteur les uns après les autres, avec une **boucle**.

```cpp
for (const int & x : v)
    ofs << setw (3) << x;

for (size_t i (0); i < v.size (); ++i)
    ofs << setw (3) << v[i];
```

Contenu de `test.txt` :

```text
  0  0  0  0  0  0  0  0  0  0
```

---

## 5. Flux sur un fichier d'entrée

### 5.1 Pourquoi ne pas rediriger l'entrée standard ?

**Solution 1 :**

```bash
a.out < InFile
```

Problèmes :

- comment lire des données depuis le **Net** (voir S3) ?
- comment lire depuis **plusieurs fichiers** en même temps ? On ne peut faire qu'**une** redirection : l'entrée standard.

**Solution 2 :** les **flux d'entrée**.

### 5.2 Déclaration

```cpp
#include <fstream>
using namespace std;

ifstream ifs;
```

`ifstream` = **I**nput **F**ile **STREAM**.

### 5.3 Ouverture

```cpp
ifs.open (const string & fileName [, Mode XXX]);
```

**Mode par défaut** :

- le fichier texte est accessible en **lecture** ;
- si le fichier **n'existe pas**, il **n'est pas créé** (aucun contrôle de validité n'est effectué) ;
- s'il existe, la **tête de lecture** est positionnée au **début** du fichier.

```cpp
ifs.open ("test.txt");
```

### 5.4 Déclaration et ouverture en une fois

```cpp
ifstream ifs (const string & filename [, Mode XXX]);

ifstream ifs ("test.txt");
```

### 5.5 Modes et fermeture

Les modes sont **les mêmes** que pour les flux de sortie.

> [!warning] Phrase ambiguë du support
> Le support écrit : « On ne peut toujours pas associer les modes `trunc` et `ate`. De même l'ouverture échoue si le mode est `trunc`, mais pas `out`. »
>
> Les deux phrases se contredisent partiellement et la seconde est obscure. Ce qu'il faut retenir de solide :
> - `trunc` **détruit** le contenu : incompatible avec une ouverture en lecture seule ;
> - `trunc` et `app` sont antagonistes ;
> - en entrée, on utilise `in` (défaut) et éventuellement `ate` pour se placer à la fin.

```cpp
ifs.close ();
```

### 5.6 Vérifier que le fichier existe : `is_open`

```cpp
if (!ifs.is_open ())
{
    cerr << "File not found" << endl;
    return;
}
// le fichier existe et est accessible
```

> [!important] `is_open()`
> Renvoie **vrai** si le flux existe, **faux** sinon. À appeler **systématiquement** après une ouverture en lecture.

### 5.7 Lire depuis un flux : `>>`

L'**extracteur** `>>` fonctionne comme pour le clavier (cas particulier de flux d'entrée) :

- pour **tous les types** (`int`, `char`, `bool`, …) ;
- pour **certaines classes** (`string`).

```cpp
string str;
ifs >> str;
cout << str << endl;

int x;
ifs >> x;
cout << x << endl;

bool b;
ifs >> b;
cout << b << endl;

char c;
ifs >> c;
cout << c << endl;
```

Avec `test.txt` :

```text
coucou
10
1
a
```

Sortie :

```text
coucou
10
1
a
```

> [!warning] `>>` saute les blancs
> L'extracteur **ignore** espaces, tabulations et retours à la ligne avant la valeur. Il lit **un mot** (ou un nombre), puis **s'arrête** au blanc suivant. Le reste du fichier n'est pas exploité.

Illustration avec `test.txt` contenant :

```text
 0  0  0  0  0  0  0  0  0  0
```

```cpp
int x;    ifs >> x;   cout << x << endl;   // 0
bool b;   ifs >> b;   cout << b << endl;   // 0
char c;   ifs >> c;   cout << c << endl;   // 0
string str; ifs >> str; cout << str << endl; // 0
```

Chaque lecture consomme **un** `0` et laisse la **tête de lecture** juste après. Les autres `0` restent « données non exploitées ».

### 5.8 Lire tout un fichier : le problème de `eof`

Algorithme souhaité :

```text
tant_que (NON fin_de_fichier)
faire
    unType resultat <- lireQuelqueChose;
    manipuler (resultat);
ffaire
```

> [!warning] On ne détecte la fin qu'**après** une tentative de lecture
> D'où la structure correcte :
>
> ```text
> boucle
>     unType resultat <- lireQuelqueChose;
>     si (finDeFichier) sortie;
>     manipuler (resultat);
> fboucle
> ```

En C++ :

```cpp
while (true)
{
    aType varIdent = readSomething ();
    if (ifs.eof ()) break;
    manipulate (varIdent);
}
```

> [!important] `eof()`
> Renvoie **vrai** si la fin du fichier a été atteinte, **faux** sinon. **Toujours tester après la lecture**, jamais avant.

### 5.9 Compter les mots (`wc -w`)

```cpp
unsigned wc (0);
string str;
while (true)
{
    ifs >> str;
    if (ifs.eof ()) break;
    ++wc;
}
cout << setw (8) << wc << ' ' << __FILE__ << endl;
```

> [!note] `__FILE__`
> Macro du C permettant de connaître le **nom du fichier courant** (le fichier source, ici).

Équivalent de la commande Linux :

```bash
wc -w fichier
```

### 5.10 Compter les caractères (`wc -c`)

**Solution 1 — avec `>>` :**

```cpp
unsigned wc (0);
char c;
while (true)
{
    ifs >> c;
    if (ifs.eof ()) break;
    ++wc;
}
cout << setw (8) << wc << ' ' << __FILE__ << endl;
```

> [!warning] Faux avec `>>`
> `>>` **saute les blancs** : les espaces, tabulations et retours à la ligne ne sont **pas comptés**. Le total est donc inférieur au vrai nombre de caractères.
>
> Le support illustre aussi le problème de l'**encodage** :
>
> ```text
> wc: hello.cxx: Illegal byte sequence
>      8230 hello.cxx
> ./a.out
>      6414 hello.cxx
> ```

**Solution 2 — avec `get (char &)` :**

```cpp
unsigned wc = 0;
char c;
while (true)
{
    ifs.get (c);
    if (ifs.eof ()) break;
    ++wc;
}
cout << setw (8) << wc << ' ' << __FILE__ << endl;
```

> [!important] `get` lit **tout**
> `ifs.get (c)` consomme **chaque caractère**, y compris les blancs et les retours à la ligne. C'est la bonne méthode pour compter les caractères.

### 5.11 Compter les lignes (`wc -l`)

Rappel : pour lire une ligne complète depuis la console :

```cpp
void getline (cin, string &);
```

Pour un flux d'entrée :

```cpp
void getline (ifstream &, string &);
```

> [!note] Pourquoi le flux par référence ?
> « Le flux est passé par référence car il y a **consommation de l'information** (déplacement de la tête de lecture). »

```cpp
unsigned wc = 0;
string str;
while (true)
{
    getline (ifs, str);
    if (ifs.eof ()) break;
    ++wc;
}
cout << setw (8) << wc << ' ' << __FILE__ << endl;
```

**Version avec une boucle `for` :**

```cpp
unsigned wc = 0;
string str;
for (getline (ifs, str); !ifs.eof (); getline (ifs, str))
    ++wc;

cout << setw (8) << wc << ' ' << __FILE__ << endl;
```

> [!warning] Dernière ligne sans `\n`
> Si le fichier ne se termine pas par un retour à la ligne, la dernière ligne est comptée différemment selon la forme de la boucle. `wc -l` sous Linux compte les **`\n`**, pas les lignes « logiques » : les deux comptages peuvent différer d'une unité.

### 5.12 Démo annoncée

Le plan annonce une **« Demo : copie d'un fichier et lien avec R1.02 »**. Les slides fournies s'arrêtent au plan : aucune trace de code pour cette démo. À traiter en TP.

---

## ✅ À retenir

- `string s (n, c)` : `n` fois le caractère `c` ; `vector<T> v (n, val)` : `n` cases valant `val`.
- `vector<pos> v8;` + `push_back (pos (i, 10))` pour une taille non connue à l'avance.
- `ofstream` = sortie, `ifstream` = entrée ; `#include <fstream>`.
- Ouverture : `ofs.open ("f.txt" [, mode]);` ou directement `ofstream ofs ("f.txt");`.
- Modes : `out`, `in`, `binary`, `ate`, `app`, `trunc` ; combinaison avec `|`.
- **Par défaut, un `ofstream` tronque** le fichier existant (le support dit l'inverse : se fier au mode `app` pour ajouter).
- `close()` ferme ; `is_open()` vérifie l'existence en lecture.
- `<<` écrit, `>>` lit ; **`>>` saute les blancs**.
- ❌ `ofs << v` ne compile pas : **boucler** sur les éléments.
- Fin de fichier : `while (true) { lire; if (ifs.eof ()) break; traiter; }`.
- `wc -w` : `>>` sur des `string` ; `wc -c` : `ifs.get (c)` ; `wc -l` : `getline (ifs, str)`.
- `getline` prend le flux **par référence** (consommation) et une `string` **obligatoirement**.

### Questions d'auto-évaluation

- [ ] Que valent les cases de `vector<int> v (10);` ? Et de `vector<int> v (10, -1);` ?
- [ ] Comment créer 10 `string` de 100 caractères `'c'` chacune ?
- [ ] Pourquoi rediriger la sortie standard ne suffit-il pas pour une application graphique ?
- [ ] Quels sont les trois effets du mode par défaut d'un `ofstream` ? Lequel est faux dans le support ?
- [ ] Quels modes sont incompatibles ? Pourquoi ?
- [ ] Comment ajouter à la fin d'un fichier existant sans l'écraser ?
- [ ] Pourquoi `ofs << v` ne compile-t-il pas ? Écrire la boucle correcte.
- [ ] Que se passe-t-il si on ouvre en lecture un fichier inexistant ?
- [ ] À quoi sert `is_open()` et quand l'appeler ?
- [ ] Différence entre `ifs >> c` et `ifs.get (c)` pour compter les caractères ?
- [ ] Pourquoi `while (!ifs.eof ()) { ifs >> x; … }` est-il un mauvais schéma ?
- [ ] Écrire les trois programmes `wc -w`, `wc -c`, `wc -l`.
- [ ] Pourquoi `getline` prend-il le flux par référence ?
