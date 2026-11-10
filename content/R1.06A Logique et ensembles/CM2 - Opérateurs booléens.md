---
ressource: R1.06A
type: cours
cm: 2
titre: Opérateurs booléens
auteur: Samuele Anni
version: Version du 12 septembre 2026
tags:
  - R1.06A
  - logique
  - booleen
---

# CM2 — Opérateurs booléens

[[R1.06A - Index|← Index R1.06A]] · Suite : [[CM3 - SAT, algèbre de Boole et quantificateurs]]

> [!abstract] En bref
> La logique booléenne fournit un langage pour combiner des affirmations vraies/fausses à l'aide d'opérateurs ($\neg,\wedge,\vee,\Rightarrow,\Leftrightarrow$). On présente leurs tables de vérité, leurs propriétés (dont De Morgan), et deux techniques de démonstration : par table de vérité et par succession d'équivalences. On introduit aussi le problème SAT.

---

## 1. Expressions booléennes

> [!important] Définition — Expression booléenne
> Une **expression booléenne** correspond à un regroupement d'affirmations. Une expression booléenne **atomique** contient une seule affirmation. Une affirmation est une phrase à laquelle on peut attribuer une **valeur de vérité** (vrai/faux).

**Exemples atomiques :** « Manille est la capitale des Philippines » (vrai) ; « Cinq est un nombre impair » (vrai) ; « Dix est plus petit que sept » (faux).

Certaines expressions changent de valeur selon le contexte : « $x < 7$ » dépend de $x$. Si aucune valeur n'est attribuée à $x$, c'est une **variable libre**.

On combine des expressions atomiques avec des opérateurs (« et », « ou », « si… alors »), comme dans le langage courant.

---

## 2. Opérateurs de base

Les valeurs de vérité se notent V/F ou 1/0.

### 2.1 Négation, conjonction, disjonction

| $p$ | $\neg p$ |
|-----|----------|
| V   | F        |
| F   | V        |

| $p$ | $q$ | $p \wedge q$ |
|-----|-----|--------------|
| V   | V   | V            |
| V   | F   | F            |
| F   | V   | F            |
| F   | F   | F            |

| $p$ | $q$ | $p \vee q$ |
|-----|-----|------------|
| V   | V   | V          |
| V   | F   | V          |
| F   | V   | V          |
| F   | F   | F          |

- $\neg$ : « non » ;
- $\wedge$ : « et » ;
- $\vee$ : « ou » (inclusif).

### 2.2 Implication et « si et seulement si »

> [!important] Définition — Implication
> Soient $p,q$ des expressions booléennes :
> $$p \Rightarrow q \stackrel{\text{def}}{=} \neg p \vee q.$$
> On dit « $p$ implique $q$ » ou « si $p$ alors $q$ ».

Table de l'implication :

| $p$ | $q$ | $p \Rightarrow q$ |
|-----|-----|-------------------|
| V   | V   | V                 |
| V   | F   | F                 |
| F   | V   | V                 |
| F   | F   | V                 |

> [!warning] Piège
> $p \Rightarrow q$ est **faux seulement** lorsque $p$ est vrai et $q$ est faux. Le fait que $p$ soit faux n'exclut pas que $q$ soit vrai.

> [!important] Définition — « Si et seulement si »
> $$p \Leftrightarrow q \stackrel{\text{def}}{=} (p \Rightarrow q) \wedge (q \Rightarrow p).$$
> $p \Leftrightarrow q$ est vrai lorsque $p$ et $q$ ont la **même** valeur de vérité. C'est l'opérateur d'**équivalence**.

Table :

| $p$ | $q$ | $p \Leftrightarrow q$ |
|-----|-----|------------------------|
| V   | V   | V                      |
| V   | F   | F                      |
| F   | V   | F                      |
| F   | F   | V                      |

---

## 3. Priorités et évaluation

Ordre de priorité (du plus fort au plus faible) :

| Symbole | Nom | Priorité |
|---------|-----|----------|
| $\neg$ | Négation | élevée |
| $\wedge,\ \vee$ | Conjonction, disjonction | |
| $\Rightarrow$ | Implication | |
| $\Leftrightarrow$ | Si et seulement si | faible |

On évalue d'abord les parenthèses. Exemple : $p \wedge \neg q \Leftrightarrow r$ se lit $(p \wedge (\neg q)) \Leftrightarrow r$.

> [!note] Notation
> On utilise $:=$ pour attribuer une valeur à une variable (comme en programmation) et $=$ pour une affirmation (qui peut être vraie ou fausse).

---

## 4. De la logique booléenne à l'informatique

La logique booléenne est à la base de l'informatique : les circuits électroniques sont des agencements de **portes logiques** matérialisant $\wedge,\vee,\neg$ ; le binaire n'est qu'une succession de 0 et de 1.

---

## 5. Propriétés des opérateurs

### 5.1 Lois de De Morgan

> [!important] Proposition — Lois de De Morgan
> Soient $p,q$ des expressions booléennes :
> $$\neg(p \wedge q) \Leftrightarrow \neg p \vee \neg q \quad\text{(1}^{re}\text{ loi)}$$
> $$\neg(p \vee q) \Leftrightarrow \neg p \wedge \neg q \quad\text{(2}^{e}\text{ loi)}$$

Intuition : « il n'est pas vrai que Paul a payé son loyer **et** sa facture » $\Leftrightarrow$ « Paul n'a pas payé son loyer **ou** il n'a pas payé sa facture ».

### 5.2 Propriétés de la négation, de la conjonction et de la disjonction

**Négation :**
- Double négation : $\neg(\neg p) \Leftrightarrow p$ ;
- Tiers exclu : $p \vee \neg p \Leftrightarrow \text{vrai}$ ;
- Contradiction : $p \wedge \neg p \Leftrightarrow \text{faux}$.

**Conjonction :**
- Élément neutre : $p \wedge \text{vrai} \Leftrightarrow p$ ;
- Élément absorbant : $p \wedge \text{faux} \Leftrightarrow \text{faux}$ ;
- Idempotence : $p \wedge p \Leftrightarrow p$ ;
- Commutativité : $p \wedge q \Leftrightarrow q \wedge p$ ;
- Associativité : $(p \wedge q) \wedge r \Leftrightarrow p \wedge (q \wedge r)$ ;
- Distributivité : $(p \vee q) \wedge r \Leftrightarrow (p \wedge r) \vee (q \wedge r)$.

**Disjonction :**
- Élément neutre : $p \vee \text{faux} \Leftrightarrow p$ ;
- Élément absorbant : $p \vee \text{vrai} \Leftrightarrow \text{vrai}$ ;
- Idempotence : $p \vee p \Leftrightarrow p$ ;
- Commutativité : $p \vee q \Leftrightarrow q \vee p$ ;
- Associativité : $(p \vee q) \vee r \Leftrightarrow p \vee (q \vee r)$ ;
- Distributivité : $(p \wedge q) \vee r \Leftrightarrow (p \vee r) \wedge (q \vee r)$.

### 5.3 Transitivité du « si et seulement si »

> [!important] Proposition
> $$(p \Leftrightarrow q) \wedge (q \Leftrightarrow r) \Rightarrow (p \Leftrightarrow r).$$

Analogie avec l'égalité arithmétique : si $x=y$ et $y=z$ alors $x=z$.

### 5.4 Contraposition

> [!important] Proposition — Contraposition
> $$p \Rightarrow q \Leftrightarrow \neg q \Rightarrow \neg p.$$

Exemple : « Si le Père Noël existe, alors je recevrai une PS5 » $\Leftrightarrow$ « Si je ne reçois pas de PS5, alors le Père Noël n'existe pas ».

### 5.5 Affaiblissement et renforcement

> [!important] Proposition
> $$p \wedge q \Rightarrow p \quad\text{(affaiblissement de la conjonction)}$$
> $$p \Rightarrow p \vee q \quad\text{(renforcement de la disjonction)}$$

---

## 6. Démonstration par table de vérité

Une démonstration par table de vérité consiste à vérifier une expression pour **toutes** les combinaisons de valeurs de vérité possibles. C'est une démonstration par cas structurée.

**Exemple — 1$^{re}$ loi de De Morgan** $\neg(p \wedge q) \Leftrightarrow \neg p \vee \neg q$ :

| $p$ | $q$ | $p\wedge q$ | $\neg(p\wedge q)$ | $\neg p$ | $\neg q$ | $\neg p \vee \neg q$ | $\Leftrightarrow$ |
|-----|-----|-------------|-------------------|----------|----------|----------------------|-------------------|
| V   | V   | V           | F                 | F        | F        | F                    | V                 |
| V   | F   | F           | V                 | F        | V        | V                    | V                 |
| F   | V   | F           | V                 | V        | F        | V                    | V                 |
| F   | F   | F           | V                 | V        | V        | V                    | V                 |

Pour toutes les combinaisons, l'expression est vraie. C.Q.F.D.

---

## 7. Démonstration par succession d'équivalences

Lorsque l'expression contient plusieurs variables, la table de vérité devient longue. On peut alors démontrer en transformant l'expression ligne par ligne, chaque passage justifié par une définition ou une propriété déjà démontrée, avec transitivité de $\Leftrightarrow$.

**Exemple — contraposition :**
$$\begin{aligned}
p \Rightarrow q &\Leftrightarrow \neg p \vee q &&\text{(définition de l'implication)}\\
&\Leftrightarrow \neg p \vee \neg(\neg q) &&\text{(double négation)}\\
&\Leftrightarrow \neg(\neg q) \vee \neg p &&\text{(commutativité de } \vee\text{)}\\
&\Leftrightarrow \neg q \Rightarrow \neg p &&\text{(définition de l'implication)}
\end{aligned}$$

---

## 8. Le problème SAT

> [!important] Définition — SAT
> Une **instance du problème SAT** consiste en une équation booléenne contenant des variables libres. Elle est **satisfiable** s'il existe une assignation de variables rendant l'équation vraie ; **insatisfiable** sinon.

**Exemple satisfiable :** $\neg[(x_1 \vee x_2 \vee x_3) \Rightarrow (x_1 \wedge x_3) \vee (x_2 \wedge x_3)]$ est satisfiable (prendre $x_1=x_2=\text{V}$, $x_3=\text{F}$).

**Exemple insatisfiable :** $\neg x_1 \wedge (x_2 \vee x_3) \wedge (x_2 \vee \neg x_3) \wedge (x_1 \vee \neg x_2)$ est insatisfiable (vérifiable par table de vérité, 8 lignes).

Pour montrer satisfiable : exhiber une assignation. Pour montrer insatisfiable : montrer que pour toute assignation, l'expression est fausse (souvent par table de vérité).

> [!note] Complexité
> La table de vérité nécessite $2^n$ lignes pour $n$ variables : impraticable pour de grandes instances. SAT est le premier problème identifié comme **NP-complet** (Cook, 1971) : vérifier une solution est facile, en trouver une est difficile.

---

## ✅ À retenir

- Opérateurs : $\neg$ (non), $\wedge$ (et), $\vee$ (ou), $\Rightarrow$ (si…alors), $\Leftrightarrow$ (ssi).
- $p \Rightarrow q$ faux seulement si $p$ V et $q$ F.
- Lois de De Morgan : $\neg(p\wedge q)\Leftrightarrow \neg p \vee \neg q$ et $\neg(p\vee q)\Leftrightarrow \neg p \wedge \neg q$.
- Contraposition : $p\Rightarrow q \Leftrightarrow \neg q \Rightarrow \neg p$.
- Deux techniques de démonstration : table de vérité (tous les cas) et succession d'équivalences.
- SAT : satisfiable si une assignation rend l'expression vraie.

### Questions d'auto-évaluation

- [ ] Construire la table de vérité de $(p \Rightarrow q) \wedge (q \Rightarrow p)$.
- [ ] Montrer par table de vérité la 2$^e$ loi de De Morgan.
- [ ] Quelle est la contraposée de « si $n$ est pair alors $n^2$ est pair » ?
- [ ] Pourquoi la table de vérité ne résout-elle pas efficacement SAT pour 100 variables ?
- [ ] Réécrire $\neg(p \wedge q)$ en utilisant uniquement $\neg$ et $\vee$.
