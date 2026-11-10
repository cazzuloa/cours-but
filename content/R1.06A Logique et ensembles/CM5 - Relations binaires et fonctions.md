---
ressource: R1.06A
type: cours
cm: 5
titre: Relations binaires et fonctions
auteur: Samuele Anni
version: Version du 12 septembre 2026
tags:
  - R1.06A
  - logique
  - relations
  - fonctions
  - denombrement
---

# CM5 — Relations binaires et fonctions

[[R1.06A - Index|← Index R1.06A]] · Suite : *(fin du cours)*

> [!abstract] En bref
> On étudie les relations binaires (d'ordre, d'équivalence, classes et partitions), les fonctions (image, antécédent, composition, injectivité/surjectivité/bijectivité, réciproque), le dénombrement (permutations, combinaisons, binôme de Newton) et le pont vers la programmation.

---

## 1. Relations binaires

> [!important] Définition — Relation binaire
> Une **relation binaire** $R$ entre deux ensembles $E$ et $F$ est définie par une partie $G \subseteq E \times F$ appelée **graphe**. Si $(x,y)\in G$, on dit que $x$ est en relation avec $y$ et on note $x R y$.
>
> Si $E=F$, on parle de relation binaire définie dans $E$.

**Exemples :** sur les mots français, $M R N$ si $M$ écrit à l'envers donne $N$ (coloc, elle). Sur $\mathbb R$, $y R x$ si $y = x^2$.

---

## 2. Relations d'ordre

On se place dans le cas $E=F$. Soit $R$ une relation binaire dans $E$.

> [!important] Définitions
> - **Réflexive** : $\forall x \in E,\ x R x$.
> - **Antisymétrique** : $\forall x,y \in E,\ (x R y \wedge y R x) \Rightarrow x = y$.
> - **Transitive** : $\forall x,y,z \in E,\ (x R y \wedge y R z) \Rightarrow x R z$.
> - **Relation d'ordre** : réflexive, antisymétrique et transitive.

**Exemples d'ordres :** $(\mathbb R, \le)$ ; $(\mathcal P(E), \subseteq)$.

> [!important] Définition — Ordre total / partiel
> Un ordre est **total** si tous les éléments sont comparables : $\forall x,y \in E,\ x R y \vee y R x$. Sinon, il est **partiel**.

$\le$ est un ordre total sur $\mathbb R$.

---

## 3. Relations d'équivalence

> [!important] Définitions
> - **Symétrique** : $\forall x,y \in E,\ x R y \Rightarrow y R x$.
> - **Relation d'équivalence** : réflexive, symétrique et transitive.

**Exemple :** l'égalité est une relation d'équivalence.

> [!important] Définition — Classe d'équivalence
> Pour $x \in E$, la **classe d'équivalence** de $x$ est
> $$[x] = \{y \in E \mid y R x\}.$$

**Propriétés :**
- Les classes d'équivalence sont deux à deux disjointes : si $[x]\neq [y]$ alors $[x]\cap [y]=\emptyset$.
- Les classes d'équivalence réalisent une **partition** de $E$ (sous-ensembles disjoints dont l'union est $E$).

> [!important] Définition — Ensemble quotient
> L'**ensemble quotient** $E/R$ est l'ensemble des classes d'équivalence de tous les éléments de $E$.

---

## 4. Fonctions

> [!important] Définition — Fonction
> Une **fonction** $f : E \to F$ est une correspondance qui associe à chaque $x \in E$ **au plus un** $y \in F$, noté $y = f(x)$.
>
> - $E$ : ensemble source (départ) ; $F$ : ensemble but (arrivée).
> - $y = f(x)$ est l'**image** de $x$ ; $x$ est un **antécédent** de $y$.
> - $\operatorname{Im}(f) = \{y \in F \mid \exists x \in E,\ y=f(x)\} \subseteq F$ (image de $f$).
> - Pour $A \subseteq E$, $f(A) = \{y \in F \mid \exists x \in A,\ y=f(x)\}$.
> - Pour $B \subseteq F$, $f^{-1}(B) = \{x \in E \mid f(x) \in B\}$ (image réciproque).

---

## 5. Fonctions composées

> [!important] Définitions
> - **Domaine** $D_f$ : ensemble des $x \in E$ ayant une image.
> - **Composée** : si $f : E \to F$ et $g : F \to G$ avec $\operatorname{Im}(f) \subseteq D_g$, alors $g \circ f : E \to G$ définie par $(g\circ f)(x) = g(f(x))$.
>
> Si $f : E \to E$ avec $\operatorname{Im}(f)\subseteq D_f$, on peut composer $f$ avec elle-même : $f^n = f \circ \dots \circ f$ ($n$ fois).

---

## 6. Injectivité, surjectivité, bijectivité

> [!important] Définitions
> - **Injective** : $\forall x,y \in E,\ x \neq y \Rightarrow f(x) \neq f(y)$ (ou contraposée : $f(x)=f(y) \Rightarrow x=y$).
> - **Surjective** : $\forall t \in F,\ \exists x \in E,\ f(x)=t$.
> - **Bijective** : injective et surjective.
> - **Réciproque** : si $f$ est bijective, chaque $y \in F$ a un unique antécédent ; on définit $f^{-1} : F \to E$. On a $f \circ f^{-1} = \mathrm{id}_F$ et $f^{-1} \circ f = \mathrm{id}_E$.

**Exemples :**
- $f : \mathbb R \to \mathbb R,\ f(x)=\sqrt{x}$ (sur $x\ge 0$) est injective.
- $f : \mathbb R \to \mathbb R,\ f(x)=x^2$ n'est **pas** injective ($-1$ et $1$ ont même image) ni surjective (pas de préimage des négatifs).
- $f : \mathbb R \to [-1,1],\ f(x)=\sin x$ est surjective.

---

## 7. Dénombrement

> [!important] Définition — Cardinal
> Pour un ensemble fini $E$, son **cardinal** $|E|$ (ou $\#E$) est son nombre d'éléments. $|\emptyset|=0$.

**Propriétés (ensembles finis) :**
- Si $A \subset B$ alors $|A| \le |B|$ ; si de plus $|A|=|B|$ alors $A=B$.
- $|A \setminus B| \le |A|$.
- $|A \cup B| = |A| + |B| - |A \cap B|$.
- $|\mathcal P(E)| = 2^{|E|}$.

> [!important] Définition — Permutation
> Une **permutation** d'un ensemble de $n$ éléments est une disposition ordonnée de ses $n$ éléments. Il y en a $n!$.

> [!important] Définition — Combinaison sans répétition
> Le nombre de $p$-combinaisons d'un ensemble de $n$ éléments est
> $$\binom{n}{p} = C_n^p = \frac{n(n-1)\dots(n-p+1)}{p!} = \frac{n!}{p!(n-p)!}.$$

**Propriétés :**
- $\binom{n}{p} = \binom{n}{n-p}$.
- Relation de Pascal : $\binom{n}{p} = \binom{n-1}{p} + \binom{n-1}{p-1}$.

> [!important] Théorème — Binôme de Newton
> $$(a+b)^n = \sum_{k=0}^{n} \binom{n}{k} a^k b^{n-k}.$$

**Exemple :** $(a+b)^2 = b^2 + 2ab + a^2$.

### 7.1 Applications et cardinaux

Si $E,F$ finis : existence d'une injection $E\to F$ $\Rightarrow$ $|E|\le |F|$ ; d'une surjection $\Rightarrow$ $|E|\ge |F|$ ; d'une bijection $\Rightarrow$ $|E|=|F|$.

Pour les ensembles infinis, on compare par existence de bijections : $\mathbb N$ et $\mathbb Z$ ont le même cardinal ($\aleph_0$) ; $\mathbb N$ et $\mathbb R$ n'ont pas le même cardinal (Cantor) : il existe plusieurs tailles d'infinis $\aleph_0, \aleph_1, \dots$.

---

## 8. Pont vers la programmation

Les notions du cours se traduisent directement en Python :

- **Quantificateurs $\to$ boucles** : $\forall x \in A \mid P(x)$ $\leftrightarrow$ `all(P(x) for x in A)` ; $\exists$ $\leftrightarrow$ `any(...)`.
- **Lois de De Morgan $\to$ simplification de conditions** : `not (x>0 and y>0)` $\equiv$ `x<=0 or y<=0`.
- **Contraposée $\to$ débogage** : tester $\neg q \Rightarrow \neg p$ au lieu de $p \Rightarrow q$.
- **Fonctions mathématiques $\to$ fonctions Python** ; injectivité/surjectivité testables sur des ensembles finis.
- **Récurrence $\to$ récursion** : cas de base de l'induction = conditions `if n==0` ; pas d'induction = cas général.

**Exemple — Fibonacci :**
```python
def fibonacci(n):
    if n == 0: return 0
    if n == 1: return 1
    return fibonacci(n-1) + fibonacci(n-2)
```

---

## ✅ À retenir

- Relation d'ordre = réflexive + antisymétrique + transitive ; total si tous comparables.
- Relation d'équivalence = réflexive + symétrique + transitive ; classes = partition.
- Fonction : image, antécédent, composition ; injective/surjective/bijective.
- Dénombrement : $n!$ permutations ; $\binom{n}{p}$ combinaisons ; binôme de Newton.
- Pont programmation : boucles, conditions, récursion.

### Questions d'auto-évaluation

- [ ] Montrer que $(\mathbb R, \le)$ est un ordre total et $(\mathcal P(E), \subseteq)$ un ordre partiel.
- [ ] Donner les classes d'équivalence de $R$ sur $\mathbb R^2$ : $(x,y) R (x',y') \iff x=x'$.
- [ ] La fonction $f : \mathbb R^2 \to \mathbb R^2,\ f(x,y)=(x+y, x-y)$ est-elle bijective ?
- [ ] Combien de chaînes de 6 bits contiennent exactement deux 1 ?
- [ ] Écrire en Python un test d'injectivité sur un ensemble fini.
