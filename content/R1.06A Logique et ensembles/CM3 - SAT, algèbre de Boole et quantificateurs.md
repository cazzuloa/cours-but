---
ressource: R1.06A
type: cours
cm: 3
titre: SAT, algèbre de Boole et quantificateurs
auteur: Samuele Anni
version: Version du 12 septembre 2026
tags:
  - R1.06A
  - logique
  - booleen
  - quantificateurs
---

# CM3 — SAT, algèbre de Boole et quantificateurs

[[R1.06A - Index|← Index R1.06A]] · Suite : [[CM4 - Démonstrations et récurrence]]

> [!abstract] En bref
> On précise le problème SAT sous forme normale conjonctive (CNF), on introduit l'algèbre de Boole et ses axiomes, les fonctions booléennes et leurs formes normales (FND/FNC) via la table de vérité, la décomposition de Shannon, puis les quantificateurs $\forall$ et $\exists$ et la négation des quantifiés.

---

## 1. Forme normale conjonctive (CNF)

> [!important] Définitions
> - Un **littéral** est une variable booléenne ou sa négation.
> - Une **clause** est une disjonction de littéraux.
> - Une **forme normale conjonctive** (CNF) est une conjonction de clauses.

**Exemple SAT-CNF :** $(x_1 \vee x_2) \wedge (\neg x_1 \vee x_3) \wedge (\neg x_2 \vee \neg x_3) \wedge (x_1 \vee x_2 \vee x_3)$.

Transformer une expression en CNF se fait en appliquant les lois de De Morgan et les propriétés des opérateurs (développement).

> [!example] Transformation en CNF
> $\neg[(x_1\vee x_2\vee x_3)\Rightarrow (x_1\wedge x_3)\vee (x_2\wedge x_3)]$
> $\Leftrightarrow \neg[\neg(x_1\vee x_2\vee x_3)\vee (x_1\wedge x_3)\vee (x_2\wedge x_3)]$ (déf. implication)
> $\Leftrightarrow (x_1\vee x_2\vee x_3)\wedge \neg(x_1\wedge x_3)\wedge \neg(x_2\wedge x_3)$ (De Morgan + double nég.)
> $\Leftrightarrow (x_1\vee x_2\vee x_3)\wedge (\neg x_1\vee \neg x_3)\wedge (\neg x_2\vee \neg x_3)$.

---

## 2. L'algèbre de Boole

L'algèbre de Boole formalise les opérations sur les variables logiques à l'aide d'un ensemble $B=\{0,1\}$ et de deux opérations $+$ (disjonction $\vee$) et $\times$ (conjonction $\wedge$), plus le complément $\bar a$ (négation).

> [!important] Définition — Algèbre de Boole
> Une algèbre de Boole est la donnée d'un ensemble $B$, de deux éléments distincts $0,1$, de deux opérations binaires $+,\times$ et d'une opération unaire $\bar{\phantom{a}}$ telles que :

| Nom | Axiome pour $\times$ | Axiome pour $+$ |
|-----|----------------------|------------------|
| Neutre | $a \times 1 = a$ | $a + 0 = a$ |
| Inverse | $a \times \bar a = 0$ | $a + \bar a = 1$ |
| Commutativité | $a \times b = b \times a$ | $a + b = b + a$ |
| Distributivité | $a \times (b + c) = a\times b + a \times c$ | $a + b \times c = (a+b)\times(a+c)$ |

À partir des axiomes on déduit (on omet $\times$) :

| Nom | Pour $\times$ | Pour $+$ |
|-----|---------------|----------|
| Associativité | $a(bc)=(ab)c$ | $a+(b+c)=(a+b)+c$ |
| Involution | $\bar{\bar a}=a$ | |
| Idempotence | $a\times a = a$ | $a+a=a$ |
| Élément absorbant | $a\times 0=0$ | $a+1=1$ |
| De Morgan | $\overline{ab}=\bar a+\bar b$ | $\overline{a+b}=\bar a \times \bar b$ |
| Absorption | $a(a+b)=a$ | $a+a b=a$ |
| Allègement | $a(\bar a+b)=ab$ | $a+\bar a b=a+b$ |
| Consensus | $ab+ac+bc=ab+ac$ | $(a+b)(a+c)(b+c)=(a+b)(a+c)$ |

> [!note] Dualité
> Si on définit une règle pour un opérateur, on obtient la règle pour l'autre en interchangeant $0\leftrightarrow 1$ et $+\leftrightarrow \times$.

---

## 3. Fonctions booléennes

> [!important] Définition — Fonction booléenne
> Une **fonction booléenne** est une série d'atomes liés par des opérations, par exemple $f(a,b,c)=a\times(b+c)$.

Une fonction booléenne se réécrit sous deux formes :

- **Forme normale conjonctive (FNC)** : conjonction de disjonctions (clauses) : $f = m_1 \times \dots \times m_n$, $m_i = q_1+\dots+q_r$.
- **Forme normale disjonctive (FND)** : disjonction de conjonctions : $f = m_1 + \dots + m_n$, $m_i = q_1\times \dots \times q_r$.

### 3.1 Obtention des formes normales par table de vérité

**Méthode FND :**
1. Dresser la table de vérité.
2. Pour chaque ligne où $f=1$, écrire la conjonction des littéraux (variable telle quelle si 1, négation si 0).
3. La FND est la disjonction de ces conjonctions.

**Méthode FNC (duale) :**
1. Dresser la table de vérité.
2. Pour chaque ligne où $f=0$, écrire la disjonction des littéraux (variable telle quelle si 0, négation si 1).
3. La FNC est la conjonction de ces disjonctions.

> [!example] FND
> Pour $f(a,b)=\bar a + b$, la table donne trois lignes à 1, d'où
> $$f(a,b)=a\times b + \bar a \times b + \bar a \times \bar b.$$
> On vérifie par simplification : $= b(a+\bar a)+\bar a\bar b = b + \bar a\bar b = (\bar a+b)(\bar a+\bar b) = \bar a + b$.

> [!example] FNC
> Pour $f(a,b,c)=a\times b + \bar a \times c$, les lignes à 0 donnent
> $$f(a,b,c)=(\bar a+b+\bar c)\times(\bar a+b+c)\times(a+\bar b+c)\times(a+b+c).$$

> [!note]
> La méthode par table de vérité est systématique mais longue : $2^n$ lignes pour $n$ variables.

---

## 4. Décomposition de Shannon

> [!important] Théorème — Décomposition de Shannon
> Soit $f$ une fonction booléenne de paramètres $x_1,\dots,x_n$ :
> $$f(x_1,\dots,x_n) = \bar{x}_1 \times f(0,x_2,\dots,x_n) + x_1 \times f(1,x_2,\dots,x_n).$$
> (la variable $x_1$ peut être remplacée par n'importe quelle autre variable.)

**Démonstration :** si $x_1=0$, les deux côtés valent $f(0,\dots)$ ; si $x_1=1$, les deux côtés valent $f(1,\dots)$. C.Q.F.D.

> [!example] Application — démontrer une loi de De Morgan
> Montrons $\overline{a+b} = \bar a \times \bar b$ par Shannon.
> Soit $f(a,b)=\overline{a+b}$. Alors $f(0,b)=\overline{0+b}=\bar b$ et $f(1,b)=\overline{1+b}=0$.
> Par Shannon :
> $$f(a,b)=\bar a\cdot f(0,b)+a\cdot f(1,b)=\bar a\cdot\bar b+a\cdot 0=\bar a\cdot\bar b.$$
> C'est bien $\bar a \times \bar b$. C.Q.F.D.

---

## 5. Quantificateurs

La logique booléenne n'est pas assez expressive ; on passe à la logique du premier ordre avec les quantificateurs.

> [!important] Définition — Quantificateur universel $\forall$
> $$\forall x \in X \mid P(x)$$
> est vrai si $P(x)$ est vrai pour **tous** les $x \in X$. Traduction : « pour tout $x \in X$, $x$ satisfait $P$ ».
>
> Il y a un lien fort entre $\forall$ et $\wedge$ :
> $\forall x \in X \mid P(x) \iff \bigwedge_{x\in X} P(x)$.

> [!important] Définition — Quantificateur existentiel $\exists$
> $$\exists x \in X \mid P(x)$$
> est vrai si $P(x)$ est vrai pour **au moins un** $x \in X$. Traduction : « il existe $x \in X$ tel que $P(x)$ ».
>
> Lien fort avec $\vee$ :
> $\exists x \in X \mid P(x) \iff \bigvee_{x\in X} P(x)$.

**Exemples :** pour $A=\{1,2,3,4,5\}$ :
- $(\forall x \in A \mid x < 10)$ se vérifie par $(1<10)\wedge\dots\wedge(5<10)$ ;
- $(\exists x \in A \mid x\text{ est pair})$ se vérifie par $(1\text{ pair})\vee\dots\vee(5\text{ pair})$.

On peut combiner plusieurs quantificateurs (logique du premier ordre).

### 5.1 Lois de De Morgan généralisées

> [!important] Proposition
> Pour $X$ un ensemble et $P(x)$ une expression booléenne dépendant de $x\in X$ :
> $$\neg(\forall x \in X \mid P(x)) \Leftrightarrow \exists x \in X \mid \neg P(x)$$
> $$\neg(\exists x \in X \mid P(x)) \Leftrightarrow \forall x \in X \mid \neg P(x)$$

> [!warning] Point clé
> Bien comprendre ces lois en français : « il n'est pas vrai que tous… » $\Leftrightarrow$ « il existe au moins un qui ne… ».

### 5.2 Évaluation d'un quantificateur universel sur domaine vide
> [!note]
> Si le domaine est vide, $(\forall x \in \emptyset \mid P(x))$ est **toujours vrai** (vacuité). Exemple : « tous les chiens qui parlent sont noirs » est vrai car aucun chien ne parle.

---

## ✅ À retenir

- CNF = conjonction de clauses (disjonctions de littéraux) ; forme standard du SAT.
- Algèbre de Boole : axiomes (neutre, inverse, commutativité, distributivité) ; dualité.
- FND (par table de vérité) : disjonction des conjonctions pour les lignes à 1 ; FNC duale.
- Shannon : $f = x_1 f(0,\dots) + \bar{x}_1 f(1,\dots)$.
- $\forall$ lié à $\wedge$, $\exists$ lié à $\vee$.
- Lois de De Morgan pour les quantificateurs : $\neg\forall \Leftrightarrow \exists\neg$ et $\neg\exists \Leftrightarrow \forall\neg$.

### Questions d'auto-évaluation

- [ ] Donner la CNF de $(x_1 \vee x_2) \Rightarrow x_3$.
- [ ] Calculer la FND de $f(a,b)=a \oplus b$ (XOR) par table de vérité.
- [ ] Énoncer la décomposition de Shannon pour $f(a,b,c)$ selon $b$.
- [ ] Nier : $\forall x \in \mathbb R,\ x^2 \ge 0$.
- [ ] Nier : $\exists x \in \mathbb N,\ x^2 = 2$.

---


