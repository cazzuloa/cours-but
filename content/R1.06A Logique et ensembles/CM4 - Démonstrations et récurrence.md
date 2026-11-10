---
ressource: R1.06A
type: cours
cm: 4
titre: Démonstrations et récurrence
auteur: Samuele Anni
version: Version du 12 septembre 2026
tags:
  - R1.06A
  - logique
  - demonstrations
  - recurrence
---

# CM4 — Démonstrations et récurrence

[[R1.06A - Index|← Index R1.06A]] · Suite : [[CM5 - Relations binaires et fonctions]]

> [!abstract] En bref
> On présente les techniques de démonstration classiques en mathématiques : quantificateurs universel/existentiel, négation de quantificateurs, implication et ssi, distinction de cas, contradiction, contraposée, puis les suites et l'induction mathématique (faible).

---

## 1. Démonstrations avec $\forall$ et $\exists$

### 1.1 Quantificateur universel $\forall$

Pour démontrer $(\forall x \in X \mid P(x))$ : on présente une entité **inconnue fixée** $x \in X$, puis on déduit $P(x)$.

**Structure :**
> Soit $x \in X$, montrons que $P(x)$ est vrai. … Donc $P(x)$ est vrai. C.Q.F.D.

> [!warning] Domaine vide
> Si le domaine est vide, $(\forall x \in \emptyset \mid P(x))$ est **toujours vrai** (vacuité).

**Exemple :** démontrer $\forall x \in ]0,1[ \mid x^2 < x$.
> Soit $x \in ]0,1[$. On a $x < 1$. Comme $x>0$, on multiplie : $x\cdot x < x$, donc $x^2 < x$. C.Q.F.D.

### 1.2 Quantificateur existentiel $\exists$

Pour démontrer $(\exists x \in X \mid P(x))$ : on **exhibe** un $x \in X$ dont on est sûr de l'existence, puis on vérifie $P(x)$.

**Structure :**
> Prenons $x = \dots$. Ce $x$ existe et $x \in X$ car … Montrons que $P(x)$ est vrai. … C.Q.F.D.

**Exemple :** démontrer $\exists x \in \mathbb N \mid x^2 = 2x+8$.
> Prenons $x=4 \in \mathbb N$. On a $4^2=16$ et $2\cdot 4+8=16$. C.Q.F.D.

> [!note]
> Une variable présentée reste inconnue mais ne varie plus jusqu'à la fin de la démonstration.

---

## 2. Démonstrations utilisant l'arithmétique

Les propriétés de base de l'arithmétique sont tenues pour acquises (addition, soustraction, multiplication, division). Un simple commentaire « propriété de l'arithmétique » suffit, mais il faut être rigoureux.

**Exemple :** démontrer $(\forall n \in \mathbb N \mid (\exists m \in \mathbb N \mid m > n))$.
> Soit $n \in \mathbb N$. Prenons $m = n+1$. Par les propriétés de l'arithmétique, $m \in \mathbb N$ et $m = n+1 > n$. C.Q.F.D.

> [!warning] Piège
> Ne pas supposer l'existence d'un « nombre magique » plus grand que tout naturel. Il faut **exhiber** $m$ en fonction de $n$.

Remarque : l'ordre des quantificateurs compte : $(\exists m \mid \forall n \mid m>n)$ est **faux**.

---

## 3. Démonstrations avec $\neg\forall$ ou $\neg\exists$

On transforme la négation en énoncé positif grâce aux lois de De Morgan généralisées (voir CM3), puis on démontre.

**Exemple 1 — réfuter un $\forall$ :** montrer que $(\forall x \in \mathbb N \mid x^2 = 1)$ est faux.
> On veut montrer $\neg(\forall x\in\mathbb N \mid x^2=1) \Leftrightarrow \exists x\in\mathbb N \mid x^2 \neq 1$. Prenons $x=2 \in \mathbb N$ ; $2^2=4 \neq 1$. C.Q.F.D.

**Exemple 2 — nier un $\exists$ :** montrer $\neg(\exists m \in \mathbb N^* \mid m > m^2)$.
> Par De Morgan : $\forall m \in \mathbb N^* \mid m \le m^2$. Soit $m \in \mathbb N^*$, on a $m \ge 1$, donc en multipliant par $m$ : $m^2 \ge m$, i.e. $m \le m^2$. C.Q.F.D.

---

## 4. Démonstration avec une implication

Pour démontrer $P \Rightarrow Q$, on suppose $P$ vrai et on en déduit $Q$.

**Exemple :** pour tout ensemble $S,T$, on a $S \subseteq T \cup S$.
> Soient $S,T$ des ensembles. On veut montrer $\forall e,\ e\in S \Rightarrow e\in T\cup S$. Soit $e$, supposons $e\in S$. Alors « $e\in T$ ou $e\in S$ » est vrai, donc $e \in T\cup S$. C.Q.F.D.

Pour une implication inverse $P \Leftarrow Q$, on suppose $Q$ vrai et on en déduit $P$.

---

## 5. Démonstration avec un « si et seulement si »

On démontre séparément les deux directions $P \Rightarrow Q$ et $P \Leftarrow Q$.

**Exemple — antisymétrie de l'inclusion :** $S\subseteq T \wedge T\subseteq S \Leftrightarrow S=T$.

- ($\Rightarrow$) Supposons $S\subseteq T \wedge T\subseteq S$. Soit $e$. Si $e\in S$ alors $e\in T$ ; si $e\in T$ alors $e\in S$. Donc $e\in S \Leftrightarrow e\in T$, i.e. $S=T$.
- ($\Leftarrow$) Supposons $S=T$. Alors $S\subseteq T$ et $T\subseteq S$.

C.Q.F.D.

---

## 6. Démonstration par cas

Lorsqu'on ne réussit pas à démontrer $(\forall x \in X \mid P(x))$ d'un coup, on partitionne $X$ en $X_1, X_2, \dots$ (dont l'union est $X$) et on vérifie $P$ sur chaque partie.

**Exemple :** montrer $\forall m \in \mathbb N \mid m \le m^2$.
> Soit $m \in \mathbb N$.
> - Cas 1 : $m=0$ : $0 \le 0^2$. OK.
> - Cas 2 : $m \in \mathbb N^*$ : $m \ge 1$, donc en multipliant par $m$ : $m \le m^2$.
>
> Comme $\mathbb N = \{0\} \cup \mathbb N^*$, c'est démontré. C.Q.F.D.

**Exemple — distributivité de l'union :** $S \cup (T\cap U) = (S\cup T)\cap(S\cup U)$.
On montre les deux inclusions par cas (voir cours).

---

## 7. Démonstrations par contradiction

On suppose que ce qu'on veut démontrer est **faux**, et on montre que cela implique une contradiction (basé sur $(\neg p \Rightarrow \text{faux}) \Leftrightarrow p$).

**Exemple :** montrer $\forall x \in \mathbb N^* \mid x^2 \ge 1$ par contradiction.
> Supposons $\neg(\forall x\in\mathbb N^* \mid x^2\ge 1)$, i.e. $\exists x\in\mathbb N^* \mid x^2 < 1$. Soit $y$ tel que $y^2 < 1$. Comme $y\in\mathbb N^*$, $y\ge 1$, donc $y^2 \ge 1$ : contradiction. Donc l'énoncé est vrai. C.Q.F.D.

---

## 8. Démonstrations par contraposée

Basé sur $p \Rightarrow q \Leftrightarrow \neg q \Rightarrow \neg p$. Au lieu de démontrer « $p$ vrai implique $q$ vrai », on démontre « $q$ faux implique $p$ faux ».

**Exemple :** montrer $\forall n \in \mathbb N \mid (n^2 \text{ pair} \Rightarrow n \text{ pair})$.
> On démontre la contraposée : $\forall n \in \mathbb N \mid (n \text{ impair} \Rightarrow n^2 \text{ impair})$. Soit $n$ impair. Il existe $i\in\mathbb N$ tel que $n=2i+1$. Alors $n^2=(2i+1)^2=4i^2+4i+1=2(2i^2+2i)+1$, impair. C.Q.F.D.

---

## 9. Suites

> [!important] Définition — Suite
> Une **suite** est une séquence d'éléments d'un ensemble ; un élément est un **terme**. On note $\{a_n\}_{n\in\mathbb N}$.
>
> Une suite peut être définie en extension, en compréhension, ou **par récurrence** : on donne le(s) premier(s) terme(s) et une méthode pour calculer un terme à partir de son (ses) prédécesseur(s).

**Exemples :**
- Entiers pairs : $b_0=0,\ b_n=b_{n-1}+2$.
- Fibonacci : $f_0=0,\ f_1=1,\ f_n=f_{n-1}+f_{n-2}$.

Somme : $\sum_{i=1}^{5} i = 1+2+3+4+5$ ; en général $\sum_i a_i$.

---

## 10. Induction mathématique (faible)

> [!important] Théorème — Induction faible
> Soit $P$ un prédicat. Alors
> $$[P(0) \wedge (\forall n \in \mathbb N^* \mid P(n-1) \Rightarrow P(n))] \Rightarrow (\forall n \in \mathbb N \mid P(n)).$$

**Structure d'une démonstration par induction :**
1. **Cas de base** : démontrer $P(0)$.
2. **Pas d'induction** : démontrer $\forall n \in \mathbb N^* \mid P(n-1) \Rightarrow P(n)$ (hypothèse d'induction : supposer $P(n-1)$ vrai, montrer $P(n)$).
3. **Conclusion** : par le principe d'induction, $\forall n \in \mathbb N \mid P(n)$.

**Exemple — suite des carrés parfaits :** $c_0=0,\ c_n=c_{n-1}+2n-1$. Montrer $c_n = n^2$.
> $P(n) : c_n = n^2$.
> - Cas de base : $c_0 = 0 = 0^2$. OK.
> - Pas d'induction : supposons $c_{n-1}=(n-1)^2$. Alors $c_n = c_{n-1}+2n-1 = (n-1)^2+2n-1 = n^2-2n+1+2n-1 = n^2$.
> - Conclusion : $c_n=n^2\ \forall n\in\mathbb N$. C.Q.F.D.

### 10.1 Induction avec base quelconque

Si l'énoncé n'est vrai qu'à partir de $n_0 \in \mathbb N$ :
$$[P(n_0) \wedge (\forall n \in \{n_0,n_0+1,\dots\} \mid P(n-1)\Rightarrow P(n))] \Rightarrow (\forall n \ge n_0 \mid P(n)).$$

Si le calcul de $a_n$ repose sur plusieurs prédécesseurs, il faut démontrer $P(0)$ et $P(1)$ (cas de base multiples).

---

## ✅ À retenir

- $\forall$ : présenter un $x$ inconnu fixé ; $\exists$ : exhiber un $x$.
- Négation d'un quantificateur $\to$ quantificateur de l'autre avec négation du prédicat.
- Implication : supposer $P$, déduire $Q$ ; ssi : deux directions.
- Cas, contradiction, contraposée : outils de démonstration.
- Induction faible : cas de base + pas d'induction $\Rightarrow$ conclusion.

### Questions d'auto-évaluation

- [ ] Démontrer par induction que $1^3+\dots+n^3 = \frac{n^2(n+1)^2}{4}$.
- [ ] Quelle est la contraposée de « si $n$ est carré alors $2n$ n'est pas carré » ?
- [ ] Démontrer par contradiction que $\forall x\in\mathbb N^*,\ x^2 \ge 1$.
- [ ] Pourquoi l'ordre des quantificateurs $\forall\exists$ vs $\exists\forall$ change-t-il la vérité ?
- [ ] Énoncer le principe d'induction avec base $n_0$.
