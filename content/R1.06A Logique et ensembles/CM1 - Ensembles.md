---
ressource: R1.06A
type: cours
cm: 1
titre: Ensembles
auteur: Samuele Anni
version: Version du 12 septembre 2026
tags:
  - R1.06A
  - logique
  - ensembles
---

# CM1 — Ensembles

[[R1.06A - Index|← Index R1.06A]] · Suite : [[CM2 - Opérateurs booléens]]

> [!abstract] En bref
> Un **ensemble** est une collection d'objets deux à deux distincts appelés **éléments**. On présente ici la notion d'ensemble, ses sous-ensembles, l'ensemble des parties, puis les opérations fondamentales (union, intersection, différence, complémentaire, produit cartésien) et leurs propriétés.

---

## 1. Notion d'ensemble

> [!important] Définition — Ensemble
> Un ensemble est une collection d'objets deux à deux distincts appelés **éléments**. On peut définir un ensemble de deux manières :
> - **en extension** : on donne la liste exhaustive des éléments qui y figurent ;
> - **en compréhension** : on donne les propriétés que doivent posséder les éléments de l'ensemble.
>
> On symbolise un ensemble avec les accolades $\{...\}$. Dans un ensemble, **l'ordre n'importe pas** : $\{1,2,3\} = \{1,3,2\}$.

**Exemples d'ensembles :**
- $\{a,b,c,d,e\}$ ;
- $\{poire, pomme, banane\}$ ;
- $\{-1, 4, 5, \frac{1}{2}\}$ ;
- $\{\text{étudiants qui ont les yeux bleus}\}$ ;
- $\{\text{mots de la langue française}\}$.

**Ensembles classiques de nombres :**
- $\mathbb N = \{0,1,2,3,\dots\}$ : nombres naturels ;
- $\mathbb N^* = \{1,2,3,\dots\}$ : naturels non nuls ;
- $\mathbb Z = \{\dots,-3,-2,-1,0,1,2,3,\dots\}$ : entiers ;
- $\mathbb Q = \{\frac{p}{q} : p \in \mathbb Z,\ q \in \mathbb Z,\ q \neq 0\}$ : rationnels ;
- $\mathbb R$ : réels ;
- $\mathbb C$ : complexes.

> [!note] Types de données (Python)
> Un type de données est un ensemble d'objets associés à des opérations standards. En Python : `bool` = $\{\text{Vrai},\text{Faux}\}$ ; `int` = entiers ; `float` = flottants ; `str` = chaînes ; `list` = listes de longueur variable.

---

## 2. Règles de fonctionnement

On admet l'existence des ensembles. La notion d'ensemble satisfait notamment :

- **Relation d'appartenance** : on peut dire si un objet est dans l'ensemble. On note $x \in A$ (« $x$ est dans $A$ »).
- **Objets distincts** : on peut distinguer deux éléments ; un ensemble ne contient pas deux fois le même objet.
- **Ensemble vide** : il existe un ensemble qui ne contient aucun élément, noté $\emptyset$ ou $\{\}$.
- **Paradoxe de Russell** : un ensemble peut être élément d'un autre ensemble, mais pas de lui-même.

> [!warning] Paradoxe de Russell
> Si l'ensemble de tous les ensembles $E$ existait, on considérerait $A = \{x \in E : x \notin x\}$. Comme $E$ contient tous les ensembles, $A \in E$. Or $A \in A \iff A \notin A$ : contradiction. Illustration concrète : le **paradoxe du barbier** — le barbier rase tous ceux qui ne se rasent pas eux-mêmes ; qui rase le barbier ?

---

## 3. Représentation

On peut représenter les ensembles à l'aide d'un **diagramme de Venn** (les fameux diagrammes « patates »).

**Exemple :** $A = \{\text{kiwi, poire, pomme, fraise}\}$ se représente par une patate contenant ces quatre éléments.

---

## 4. Sous-ensembles

> [!important] Définition — Sous-ensemble
> L'ensemble $A$ est un **sous-ensemble** de $B$ si tous les éléments de $A$ sont des éléments de $B$ (autrement dit $x \in A \Rightarrow x \in B$). On dit aussi que $A$ est **inclus dans** $B$, et on note $A \subseteq B$.
>
> Pour tout ensemble $A$ : $\emptyset \subseteq A$ et $A \subseteq A$.

**Exemples :** $\{1,2\} \subseteq \{1,2,3\}$ ; $\mathbb N \subseteq \mathbb Z \subseteq \mathbb Q \subseteq \mathbb R \subseteq \mathbb C$.

> [!important] Définition — Égalité d'ensembles
> Deux ensembles sont **égaux** si et seulement s'ils ont les mêmes éléments, autrement dit si $A \subseteq B$ et $B \subseteq A$.

> [!important] Définition — Ensemble des parties
> Soit $A$ un ensemble. L'**ensemble des parties** de $A$, noté $\mathcal P(A)$, est l'ensemble des sous-ensembles de $A$.
>
> On a toujours $\emptyset \in \mathcal P(A)$ (car $\emptyset \subseteq A$) et $A \in \mathcal P(A)$ (car $A \subseteq A$).

**Exemple :** si $A = \{1,2,3\}$, alors
$$\mathcal P(A) = \{\emptyset, \{1\}, \{2\}, \{3\}, \{1,2\}, \{1,3\}, \{2,3\}, \{1,2,3\}\}.$$

> [!note] Piège à éviter
> $\mathcal P(\emptyset) = \{\emptyset\}$ et $\mathcal P(\mathcal P(\emptyset)) = \{\emptyset, \{\emptyset\}\}$. La notation $\emptyset$ décrit un ensemble qui ne contient rien, alors que $\{\emptyset\}$ décrit un ensemble contenant un élément (l'ensemble vide). Un tiroir contenant un sac vide ($\{\emptyset\}$) n'est pas vide.

---

## 5. Opérations sur les ensembles

### 5.1 Union

> [!important] Définition — Union
> L'**union** des ensembles $A$ et $B$ est l'ensemble des éléments qui sont dans $A$ **ou** dans $B$. On le note $A \cup B$.

**Propriétés de l'union :**
- Idempotence : $A \cup A = A$ ;
- Commutativité : $A \cup B = B \cup A$ ;
- Associativité : $A \cup (B \cup C) = (A \cup B) \cup C$ ;
- Élément neutre : $A \cup \emptyset = A$.

### 5.2 Intersection

> [!important] Définition — Intersection
> L'**intersection** des ensembles $A$ et $B$ est l'ensemble des éléments communs à $A$ et $B$. On le note $A \cap B$. Deux ensembles sont **disjoints** (ou incompatibles) si $A \cap B = \emptyset$.

**Propriétés de l'intersection :**
- Idempotence : $A \cap A = A$ ;
- Commutativité : $A \cap B = B \cap A$ ;
- Associativité : $A \cap (B \cap C) = (A \cap B) \cap C$ ;
- Élément neutre : si $A \subseteq \Omega$ (univers), alors $A \cap \Omega = A$.

**Propriétés de distributivité :**
- de $\cup$ sur $\cap$ : $A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$ ;
- de $\cap$ sur $\cup$ : $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$.

### 5.3 Différence

> [!important] Définition — Différence
> La **différence** de $A$ par $B$ est l'ensemble des éléments qui sont dans $A$ mais pas dans $B$. On le note $A \setminus B$.

### 5.4 Différence symétrique

> [!important] Définition — Différence symétrique
> La **différence symétrique** entre $A$ et $B$ est l'ensemble des éléments qui sont dans $A$ ou $B$ mais pas dans les deux :
> $$A \mathbin{\Delta} B = (A \cup B) \setminus (A \cap B).$$

### 5.5 Complémentaire

> [!important] Définition — Complémentaire
> On se fixe un ensemble $\Omega$ appelé **univers**. Pour $A \subseteq \Omega$, le **complémentaire** de $A$ (par rapport à $\Omega$) est l'ensemble des éléments de $\Omega$ qui ne sont pas dans $A$. On le note $\overline A = \Omega \setminus A$.
>
> Il faut obligatoirement se placer dans un ensemble de référence pour définir le complémentaire.

**Propriétés de la complémentation :**
- Involution : $\overline{\overline A} = A$ ;
- Lois de De Morgan : $\overline{A \cap B} = \overline A \cup \overline B$ et $\overline{A \cup B} = \overline A \cap \overline B$.

### 5.6 Produit cartésien

> [!important] Définition — Produit cartésien
> Le **produit cartésien** des ensembles $A$ et $B$ (dans cet ordre) est l'ensemble des couples $(a,b)$ où $a \in A$ et $b \in B$ :
> $$A \times B = \{(a,b) : a \in A,\ b \in B\}.$$
>
> Le produit cartésien de $A_1,\dots,A_k$ est l'ensemble des $k$-uplets $(a_1,\dots,a_k)$ avec $a_i \in A_i$. Si $A_1 = \dots = A_k = A$, on note $A^k$.
>
> Le couple $(a,b)$ n'est pas un ensemble ; si $a \neq b$, alors $(a,b) \neq (b,a)$.

**Exemple :** pour $A = \{a,b,c,d\}$ et $B = \{1,2,3\}$, $A \times B$ contient $12$ couples ; $A \times B \neq B \times A$ (l'ordre compte).

> [!example] Lecture — Couleurs RGB
> Une couleur RGB est un élément de $[0,255] \times [0,255] \times [0,255] = [0,255]^3$ (coordonnées entières) : chaque couleur est un 3-uplet de valeurs entre 0 et 255.

---

## ✅ À retenir

- Un ensemble = collection d'objets distincts ; définition en extension ou en compréhension ; l'ordre ne compte pas.
- $\emptyset \subseteq A$ et $A \subseteq A$ ; égalité $\iff$ inclusion mutuelle.
- $\mathcal P(A)$ = ensemble des sous-ensembles de $A$ ; $|\mathcal P(A)| = 2^{|A|}$ (pour $A$ fini).
- Opérations : $\cup$, $\cap$, $\setminus$, $\Delta$, complémentaire (relatif à $\Omega$), $\times$.
- Propriétés clés : idempotence, commutativité, associativité, distributivité, lois de De Morgan pour le complémentaire.
- $\mathcal P(\emptyset)=\{\emptyset\}$ (ne pas confondre avec $\emptyset$).

### Questions d'auto-évaluation

- [ ] Quelle est la différence entre $\emptyset$ et $\{\emptyset\}$ ?
- [ ] Donner $\mathcal P(\{a,b\})$.
- [ ] Énoncer les lois de De Morgan pour le complémentaire.
- [ ] Pourquoi le produit cartésien n'est-il pas commutatif ?
- [ ] Que se passe-t-il si on définit un complémentaire sans univers fixé ?
