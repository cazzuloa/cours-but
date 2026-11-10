---
ressource: R1.02
type: cours
cm: 3
titre: Accessibilité, SEO, écoconception et outils Web
auteur: Mickaël Martin Nevot
version: V2.2.0
tags:
  - R1.02
  - accessibilité
  - SEO
  - écoconception
---

# CM3 — Accessibilité, SEO, écoconception et outils Web

[[CM2 - CSS|← CM2 CSS]] · [[R1.02 - Index|Index]]

> [!abstract] En bref
> Un site Web de qualité ne se limite pas à « s'afficher » : il doit être **accessible** (4 principes : perceptible, utilisable, compréhensible, robuste), **bien référencé** (SEO interne/externe), **sobre** (écoconception) et **testé** avec les bons outils.

---

## 1. Accessibilité : les quatre principes directeurs

| Principe | Contenu |
|----------|---------|
| **Être perceptible** | multilinguisme ; présentation visuelle avec équivalents textuels au contenu non textuel ; redimensionnement du texte ; mise en place simplifiée ; contraste des couleurs |
| **Être utilisable** | accessible par le clavier ; navigabilité (liens soignés, fil d'Ariane, plan de site) ; délai suffisant de lecture, pas de défilement automatique ; pas plus de trois flashs à la suite |
| **Être compréhensible** | lisibilité ; prévisibilité ; aide à la saisie ; rétroactions, messages d'erreurs, aides contextuelles ; abréviations et jargon explicités |
| **Être robuste** | interprétable de manière fiable par les navigateurs, les lecteurs d'écran et l'ensemble des technologies d'assistance |

### 1.1 Référentiel WCAG

> [!important] WCAG
> **WCAG** : *Web Content Accessibility Guidelines* — référentiel international d'accessibilité du **W3C**. La slide présente les **4 principes**, les **règles** qui les déclinent et les **3 niveaux** de conformité.

| Principe | Règles associées (slide) |
|----------|--------------------------|
| **Perceptible** | Texte alternatif · Média temporel · Adaptable · Distinguable |
| **Utilisable** | Accessible au clavier · Assez de temps · Crise d'épilepsie · Navigable |
| **Compréhensible** | Lisible · Prévisible · Aide à la saisie (*Input*) |
| **Robuste** | Compatible |

| Niveau | Signification (complément) |
|--------|----------------------------|
| **A** | niveau **minimal** |
| **AA** | niveau **recommandé**, le plus souvent exigé |
| **AAA** | niveau **le plus exigeant** |

> [!note] Lecture de la slide
> Les **règles** sont les critères concrets à respecter (alternative textuelle, sous-titres, contraste, navigation au clavier, etc.) ; chaque critère est classé **A**, **AA** ou **AAA** selon son degré d'exigence.

## 2. WAI-ARIA

Attributs ARIA qui enrichissent la sémantique pour les technologies d'assistance.

```html
<!-- aria-label : nom accessible -->
<button aria-label="accéder au code Hypertext markup language">html</button>

<!-- aria-labelledby : référence l'id d'un autre élément comme nom -->
<h2 id="titre">Code de la page HTML</h2>
<button aria-label="accéder au code Hypertext markup language"
        aria-labelledby="titre">html</button>

<!-- aria-describedby : référence une description -->
<h3 id="titre">Code de la page de formulaire de connexion</h3>
<button id="code" aria-describedby="code titre">Accéder au code HTML</button>
```

| Attribut | Ce que le lecteur d'écran annonce |
|-----------|-----------------------------------|
| `aria-label` | « accéder au code Hypertext markup language » |
| `aria-labelledby` | le texte de l'élément référencé (« Code de la page HTML ») |
| `aria-describedby` | « Accéder au code HTML code de la page de formulaire de connexion » |

## 3. Outils d'accessibilité

| Outil | Particularité |
|-------|---------------|
| **WAVE** | simple mais intrusif |
| **axe** | intégré à DevTools |
| **HeadingsMap** | audit de la hiérarchie des titres |
| **NVDA** | lecteur d'écran |

## 4. Ergonomie

- **Règle des trois clics** : un internaute doit pouvoir accéder à n'importe quelle information d'un site Web en suivant **au plus trois liens**.
- Meilleur référencement par les moteurs de recherche.
- **Landing page** (page d'atterrissage) :
  - visibilité importante sur la première page d'un site Web ;
  - communication ciblée sur l'acte direct (inscription à un bulletin d'information, achat rapide, etc.).

## 5. Référencement naturel (SEO)

Le SEO (*Search Engine Optimization*) se joue sur deux tableaux.

| Interne | Externe |
|---------|---------|
| Balisage sémantique | Annuaires |
| Contenu | Forums |
| Rapidité du site Web | Communiqués de presse |
| Design et ergonomie | Digg-like |
| Publicité | Réseaux sociaux |
| Liens internes | Commentaires de blogs |
| Plan de site | |
| Version mobile | |
| URL canonique | |
| Mots clefs | |

> [!important] Objectif du SEO externe
> **Obtenir des liens depuis d'autres sites** (backlinks).

## 6. Référencement local et réseaux sociaux

- **Micro-données** : standard de Google, Bing et Yahoo — <https://schema.org>
  - outil d'aide au balisage : <https://www.google.com/webmasters/markup-helper>
  - test des résultats enrichis : <https://search.google.com/test/rich-results>
- **Protocole Open Graph** : <https://ogp.me>
  - prévisualisation : <https://opengraph.dev>

## 7. Écoconception

- **115 bonnes pratiques** : <https://github.com/cnumr/best-practices>
- **Obésiciels** : sites Web **155 fois plus lourds qu'en 1995**, source d'**obsolescence programmée**.

> [!tip] Écoconcevoir, c'est alléger : images optimisées, code minimal, cache, moins de requêtes, pas de fonctionnalités inutiles.

## 8. Technologies liées à HTML5

| Technologie | Rôle |
|-------------|------|
| **Canevas** (`<canvas>`) | zone de dessin (dynamique) |
| **Web Storage** | alternative puissante aux cookies |
| **Drag and drop** | glisser/déposer des objets dans la page Web |
| **File API** | accès aux fichiers stockés chez le client (avec autorisation), souvent avec drag and drop |
| **Géolocalisation** | géolocalisation (nécessite autorisation) |
| **SVG** | format d'images vectorielles |
| **Cache manifest** | applications Web en mode « hors ligne » |
| **WebSockets** | communication client-serveur bidirectionnelle |
| **WebGL** | OpenGL dans les applications Web |

## 9. HTML5/CSS3 vs plugins

| Critère | HTML5 | Plugin |
|---------|-------|--------|
| Contenu référencé | ✔ | — |
| Chargement progressif | ✔ | — |
| Historique de navigation | ✔ | — |
| Changement d'URL | ✔ | — |
| Accessibilité | ✔ | — |
| Compatibilité iOS | ✔ | — |
| Jeux vidéo sur navigateur | ✔ | ✔ |
| Bandeaux publicitaires | ✔ | ✔ |
| Animations | ✔ | ✔ |
| Vidéos | ✔ | ✔ |
| Applications 3D | ✔ | ✔ |
| Applications multimédia hors Web | — | ✔ |

> [!important] Plugins et HTML5 ne sont pas concurrents
> Les plugins sont des **compléments** d'HTML5.
>
> - **Avantages** : pas de questionnement sur la normalisation, accès à des fonctionnalités avancées.
> - **Inconvénients** : pas forcément multiplateformes, pas ouverts (pérennité), taux de pénétration.

## 10. Aller plus loin

- Préprocesseur CSS (Sass, Less, etc.)
- Fonctionnement d'un navigateur et AMP
- Grid layout
- `<embed>` et `<object>`
- CMS
- Attribut `ContentEditable`
- File API, SVG, cache manifest, Web Sockets, WebGL
- BDD Web SQL

## 11. Liens

- Accessibilité : <https://www.un.org/fr/webaccessibility/guidelines/masthead.html>
- Référencement : <http://www.whiteref.com/blog/guide-du-referencement-naturel.html>
- Thomas Dimnet, *Concevez un contenu web accessible*

---

## ✅ À retenir

- Accessibilité = **perceptible, utilisable, compréhensible, robuste**.
- **WCAG** : 4 principes, des règles par principe, et **3 niveaux** (A, AA, AAA).
- ARIA complète la sémantique : `aria-label`, `aria-labelledby`, `aria-describedby`.
- Tester avec **WAVE**, **axe**, **HeadingsMap**, **NVDA**.
- SEO **interne** (contenu, sémantique, rapidité, mobile) + **externe** (backlinks).
- **Règle des trois clics** et *landing page* relèvent de l'ergonomie.
- Écoconception : réduire le poids, lutter contre les **obésiciels**.
- HTML5 couvre nativement canvas, stockage, géolocalisation, SVG, WebSockets, WebGL…

### Questions d'auto-évaluation

- [ ] Citer et expliquer les quatre principes de l'accessibilité.
- [ ] Donner le rôle de `aria-label`, `aria-labelledby`, `aria-describedby`.
- [ ] Citer deux outils d'audit d'accessibilité et un lecteur d'écran.
- [ ] Qu'est-ce que la règle des trois clics ?
- [ ] Distinguer SEO interne et SEO externe.
- [ ] Qu'est-ce qu'un obésiciel ?
- [ ] Citer cinq technologies HTML5 et leur usage.
- [ ] Comparer HTML5 et les plugins (avantages / inconvénients).
