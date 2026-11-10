---
ressource: R1.02
type: cours
cm: 1
partie: 2
titre: HTML
auteur: Mickaël Martin Nevot
version: V2.1.1
tags:
  - R1.02
  - HTML
  - Web
  - balises
---

# CM1-2 — HTML

[[CM1-1 - Web|← CM1-1 Web]] · [[R1.02 - Index|Index]] · Suite : [[CM2 - CSS]]

> [!abstract] En bref
> HTML est un **langage de description de données balisé** (pas un langage de programmation) : il structure le contenu des pages Web. On y trouve des balises simples ou doubles, des attributs, des commentaires, et une page minimale normalisée HTML5 avec `<!DOCTYPE html>`, `<head>` et `<body>`.

---

## 1. HTML, XHTML, HTML5

| Nom | Définition |
|-----|------------|
| **HTML** | Langage de **description de données** (≠ programmation), langage **balisé**, utilisé pour créer le contenu des pages Web. Extension `.html` (ou `.htm`). |
| **XHTML** | **Même langage** que HTML, mais avec le respect de certaines **règles syntaxiques**. |
| **HTML5 / XHTML5** | Dernière version de HTML/XHTML — normalisation **2014**. |

> [!note] Standard HTML
> <https://html.spec.whatwg.org>

## 2. Balises

| Élément | Exemple |
|---------|---------|
| **Balise simple** (orpheline, auto-fermante, unique) | `<br>` |
| **Balise double** (en paire) | `<title>Titre de la page</title>` |
| **Attribut** | `<html lang="fr"></html>`, `<meta charset="utf-8">` |
| **Contenu** | ensemble de données délimitées par une balise double |

> [!note] Types de balises
> *raw* (`script`, `style`), *escapable raw text* (`textarea`, `title`), *normal element*, *void element*.

## 3. Ordre d'imbrication

```html
<!-- Correct -->
<balise1>
    …
    <balise2>
        …
    </balise2>
    …
</balise1>

<!-- Incorrect : balises croisées -->
<balise1>
    …
    <balise2>
        …
</balise1>
    …
</balise2>
```

> [!tip] Une bonne **indentation** permet d'éviter la plupart des erreurs d'imbrication.

## 4. Commentaires HTML

Non interprétés par le navigateur, mais **visibles dans le code source**.

```html
<!-- Un commentaire HTML -->

<!--
    Un autre commentaire HTML
    sur plusieurs lignes
-->
```

## 5. Page HTML5 minimale

```html
<!DOCTYPE html>
<html lang="fr">
    <head>
        <title>Titre de la page</title>
        <meta charset="utf-8">
    </head>
    <body>
        <!-- Ici votre site Web -->
    </body>
</html>
```

| Élément | Rôle |
|---------|------|
| `<!DOCTYPE html>` | DTD (document type definition) : version de HTML utilisée |
| `<html>` | page HTML |
| `<head>` | informations générales sur la page (invisible) |
| `<meta charset="utf-8">` | encodage du fichier |
| `<title>` | titre de la page (visible dans l'onglet du navigateur) |
| `<body>` | partie visible de la page |

## 6. En-tête (`<head>`)

```html
<title>Titre de la page</title>

<!-- Favicon -->
<link rel="icon" href="favicon.ico">

<!-- Feuille de style CSS -->
<link rel="stylesheet" href="css/style.css">

<!-- JavaScript -->
<script src="js/myFile.js"></script>
```

### Métadonnées `<meta>`

```html
<meta charset="utf-8">
<meta name="author" content="Mickaël Martin Nevot">
<meta name="description" content="Description du document (160 car. max.)">
<meta name="keywords" content="HTML, CSS, javascript, php, flash">
<meta name="date" content="2012-02-14T05:00:00+01:00">
```

> [!warning] `keywords` n'est **plus utile** pour le référencement.

## 7. Corps de texte

| Balise | Rôle | Exemple |
|--------|------|---------|
| `<p></p>` | paragraphe | `<p>Un simple paragraphe.</p>` |
| `<br>` | retour de ligne | `Ce texte contient <br>un retour à la ligne.` |
| `<h1></h1>` … `<h6></h6>` | titres, du niveau 1 au niveau 6 | `<h1>Titre de la page</h1>` |
| `<strong></strong>` | force (gras sémantique) | `<strong>Attention !</strong>` |
| `<em></em>` | emphase (italique sémantique) | `<em>copyright</em>` |

> [!warning] `<h1>` ≠ `<title>`
> Il ne faut **pas** choisir une balise de titre en fonction de sa taille apparente, mais par **ordre hiérarchique** : il doit obligatoirement y avoir un `<h1>` s'il y a un `<h2>`, etc.

## 8. Textes évolués

| Balise | Rôle | Exemple |
|--------|------|---------|
| `<code></code>` | code source | `<code>++c;</code>` |
| `<pre></pre>` | texte préformaté : police non proportionnelle, espaces et sauts de ligne préservés | `<pre>…</pre>` |
| `<blockquote></blockquote>` | citation (indentation) | `<blockquote>Ceci est une citation longue.</blockquote>` |

## 9. Liens et ressources

### Hyperliens `<a>`

```html
<a href="http://www.domain.com" title="Explication du lien">Texte cliquable</a>

<!-- Ancrage -->
<a id="unique-id-1">Texte éventuellement vide</a>
<a id="unique-id-2"></a>
<a href="#unique-id-1">Lien interne vers l'ancre unique-id-1</a>
<a href="http://www.domain.com#unique-id-2">Lien vers l'ancre unique-id-2 du site</a>

<!-- E-mail et téléphone -->
<a href="mailto:mmartin.nevot@gmail.com">mmartin.nevot@gmail.com</a>
<a href="tel:0123456789">01 23 45 67 89</a>
```

### Images `<img>`

```html
<img src="img/photo.jpg" alt="Texte de l'infobulle">

<a href="img/photo.jpg">
    <img src="photo_mini.jpg"
         title="Image miniature cliquable : cliquez pour agrandir !"
         longdesc="Texte long de l'infobulle"
         loading="lazy">
</a>
```

> [!warning] Esperluette dans une URL
> Un lien vers une URL comportant des `&` nécessite de les remplacer par `&amp;`.

### Liens et ressources (slide illustrée)

La slide « Liens et ressources » est une **capture d'écran** (types de liens : absolu, relatif, ancre, mailto, tel). Retenir surtout la distinction **URL absolue** (avec protocole et domaine) / **URL relative** (par rapport au fichier courant).

## 10. Découpage et cadre

| Balise | Rôle |
|--------|------|
| `<div></div>` | division, section (bloc) |
| `<span></span>` | délimitation d'une partie du texte, dans une même ligne |
| `<iframe></iframe>` | cadre incorporé |

```html
<div class="myclass">
    <p>Ce paragraphe, </p>
    <p>et celui-là font partie de la même section.</p>
</div>

<p>Cette balise permet de regrouper
<span class="myclass">cet ensemble de mots uniquement</span>
à l'intérieur d'une même ligne d'un paragraphe.</p>

<iframe src="page.html">
    <p>Votre navigateur ne peut pas afficher de cadre incorporé.
    Vous pouvez appeler la page par ce lien :
    <a href="page.html">Titre de la page</a></p>
</iframe>
```

> [!important] Balises d'organisation
> `div`, `span` et `iframe` sont, avec les balises de **structuration HTML5**, les seules balises d'organisation autorisées.

## 11. Structuration sémantique HTML5

| Balise | Rôle |
|--------|------|
| `<header></header>` | en-tête |
| `<footer></footer>` | pied de page |
| `<nav></nav>` | navigation (menu) |
| `<article></article>` | partie indépendante (article de blog, commentaire, etc.) |
| `<aside></aside>` | zone secondaire non liée au contenu principal |

> [!tip] Ces balises sont **sémantiques** : elles décrivent le rôle du contenu, ce qui aide l'accessibilité et le référencement.

## 12. Tableaux

```html
<table border="1">
    <tr>
        <td>Cellule 1 de la ligne 1</td>
        <td>Cellule 2 de la ligne 1</td>
        <td>Cellule 3 de la ligne 1</td>
    </tr>
    <tr>
        <td colspan="2">Cellule 1 de la ligne 2</td>
        <td rowspan="2">Cellule 2 de la ligne 2</td>
    </tr>
    <tr>
        <td>Cellule 1 de la ligne 3</td>
        <td>Cellule 2 de la ligne 3</td>
    </tr>
</table>
```

| Balise | Rôle |
|--------|------|
| `<table>` | tableau |
| `<tr>` | ligne (*table row*) |
| `<td>` | cellule (*table data*) |
| `colspan` | fusion horizontale |
| `rowspan` | fusion verticale |

## 13. Listes

```html
<!-- Liste ordonnée -->
<ol>
    <li>Premier élément de la liste</li>
    <li>Deuxième élément de la liste</li>
</ol>

<!-- Liste non ordonnée (puces) -->
<ul>
    <li>Premier élément de la liste</li>
    <li>Deuxième élément de la liste</li>
</ul>

<!-- Liste de termes -->
<dl>
    <dt>Terme 1</dt>
    <dd>Définition du terme 1</dd>
    <dt>Terme 2</dt>
    <dd>Définition du terme 2</dd>
</dl>
```

## 14. Formulaires

| Balise | Rôle |
|--------|------|
| `<form></form>` | formulaire |
| `<label for="id"></label>` | étiquette liée à un identifiant |
| `<textarea></textarea>` | zone de texte |
| `<select></select>` / `<option></option>` | liste de choix |
| `<input>` | saisie d'information |
| `<button>` | bouton |
| `<output>` | résultat calculé |

### Attributs de `<form>`

| Attribut | Rôle |
|----------|------|
| `action` | script à exécuter au clic du bouton de type « submit » |
| `method` | méthode d'envoi |
| `enctype` | encodage des valeurs envoyées au serveur |

### Types d'`<input>`

| Classiques | HTML5 |
|------------|-------|
| `text` | `tel` |
| `password` | `email` |
| `checkbox` | `url` |
| `radio` | `search` |
| `file` | `color` |
| `submit` | `number` |
| `reset` | `range` |
| `button` | `datetime`, `month`, `week`, etc. |

> [!tip] Compatibilité des nouveaux types : <https://caniuse.com/>

### Autres attributs

| Attribut | Rôle |
|----------|------|
| `placeholder` | texte indicatif par défaut |
| `required="required"` | champ obligatoire |
| `pattern="[0-9][a-z]{3}"` | validation à la volée |
| `multiple="multiple"` | liste à choix multiple (pour `select`) |

> [!warning] `placeholder` n'est qu'une **aide supplémentaire** : il ne remplace pas `<label></label>`.

### Exemple complet

```html
<form id="form" action="" method="post" enctype="text/plain">
    <p>
        <label for="prenom">Prénom</label>
        <input id="prenom" type="text" name="prenom" value=" ">
        <label for="nom">Nom</label>
        <input id="nom" type="text" name="nom" value=" "><br>

        <select name="sexe">
            <option>homme</option>
            <option>femme</option>
        </select><br>

        <textarea name="impression" rows="2" cols="50">
            Donnez vos impressions sur le site...
        </textarea><br>

        <input type="submit" value="Enregistrer">
    </p>
</form>
```

```html
<button type="button">clic !</button>

<output for="t_ht t_tva" name="ttc" form="tva_form"></output> €
```

## 15. Balises multimédia

```html
<audio controls="controls">
    <source src="audio.ogg" type="video/ogg">
    <source src="audio.mp3" type="video/mp3">
    <source src="audio.wav" type="video/wav">
    Ici l'alternative : un lien de téléchargement, un message, etc.
</audio>

<video controls="controls">
    <source src="video.ogv" type="video/ogg">
    <source src="video.mp4" type="video/mp4">
    Ici l'alternative : un lien de téléchargement, un message, etc.
</video>
```

> [!note] Codec (code-decode)
> Un **codec** est un format de données. Les notions de codec, norme et conteneur sont souvent confondues. **Les navigateurs ne sont pas compatibles avec tous les codecs.**

## 16. Autres balises

| Balise | Rôle |
|--------|------|
| `<figure></figure>` | regrouper images et vidéos |
| `<figcaption></figcaption>` | légende d'une `<figure>` |
| `<mark></mark>` | texte marqué (surligné) |
| `<canvas></canvas>` | canevas (zone de dessin dynamique) |
| `<progress></progress>` | barre de progression |
| `<meter></meter>` | jauge |

```html
<figure>
    <img src="/images/chaton.jpg" alt="Oh le beau chaton">
    <figcaption>Un petit chat mignon tout plein</figcaption>
</figure>

<p>Le mot "<mark>HTML5</mark>" doit être mis en avant.</p>
```

## 17. Block et inline

| Block | Inline |
|-------|--------|
| `<p>` | `<br>` |
| `<hx>` (x : 1 à 6) | `<strong>` |
| `<pre>` | `<em>` |
| `<blockquote>` | `<code>` |
| `<div>` | `<a>` |
| `<iframe>` | `<img>` |
| `<header>`, `<footer>`, `<nav>`, `<article>`, `<aside>` | `<span>` |
| `<table>` | `<input>`, `<textarea>`, `<select>` |
| | `<mark>` |

> [!important] Block vs inline
> Une balise **block** occupe toute la largeur et provoque un retour à la ligne. Une balise **inline** reste à l'intérieur d'une balise block et ne provoque pas de retour à la ligne.

### Balises essentielles (récapitulatif des slides)

| Catégorie | Balises |
|-----------|---------|
| Structure du document | `<html>`, `<head>`, `<body>`, `<title>`, `<link>`, `<script>`, `<meta>` |
| Tableaux | `<tr>`, `<td>` |
| Listes | `<li>`, `<ol>`, `<ul>`, `<dl>` |
| Formulaires | `<form>`, `<option>` |
| Média | `<source>`, `<audio>`, `<video>` |
| Divers | `<figure>`, `<figcaption>`, `<canvas>`, `<progress>`, `<meter>` |

> [!note] Il existe bien d'autres balises que celles présentées dans ce CM.

## 18. Règles XHTML

- Utilisation d'un **DTD** (DOCTYPE)
- Les balises doivent être écrites en **minuscules**
- Les balises de fin sont **obligatoires**
- Les valeurs d'attributs sont **entre guillemets** et les attributs ne comportent ni espace ni retour chariot : `<html lang="fr">`
- Respecter l'**ordre d'imbrication** des balises
- Utilisation **recommandée** des feuilles de style CSS
- Plus d'informations : <http://www.w3.org/TR/xhtml1>

## 19. Liens

- HTML : <http://www.w3.org/>, <http://giminik.developpez.com/xhtml/>, <http://html5-css3.fr>
- Sébastien Mavromatis, *Langages de l'Internet*
- Antoine Cellerier, *Formation HTML/CSS*
- Jonathan Verrecchia, *Présentation complète de l'HTML5*
- Mathieu Nebra, *Apprenez à créer votre site web avec HTML5/CSS3* ; *Réalisez votre site web avec HTML5 et CSS3*

---

## ✅ À retenir

- HTML = **description de contenu**, pas de la programmation.
- Une balise peut être **simple** (`<br>`) ou **double** (`<p></p>`), avec des **attributs**.
- Toujours respecter l'**imbrication** et indenter le code.
- Page minimale : `DOCTYPE`, `html[lang]`, `head` (`title`, `meta charset`), `body`.
- `<h1>` à `<h6>` : hiérarchie **sémantique**, pas une question de taille.
- Distinguer **block** et **inline**.
- `<label>` est obligatoire pour l'accessibilité : `placeholder` ne le remplace pas.
- XHTML = HTML + règles syntaxiques strictes.

### Questions d'auto-évaluation

- [ ] Différencier HTML, XHTML et HTML5.
- [ ] Écrire de mémoire une page HTML5 minimale et expliquer chaque ligne.
- [ ] Quelle est la différence entre une balise simple et une balise double ?
- [ ] Pourquoi ne faut-il pas choisir un `<h3>` pour sa taille ?
- [ ] Citer cinq balises sémantiques HTML5 et leur rôle.
- [ ] Construire un tableau avec `colspan` et `rowspan`.
- [ ] Construire un formulaire complet avec `label`, `input`, `select`, `textarea`.
- [ ] Citer les différences entre balises block et inline.
- [ ] Énumérer les règles XHTML.
