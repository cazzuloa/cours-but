---
type: methode
titre: Méthode — Transformer des CM PDF en notes Obsidian
tags:
  - methode
  - obsidian
  - prise-de-notes
  - workflow
---

# Méthode — Transformer des CM PDF en notes Obsidian

> [!abstract] But de ce document
> Décrire **exactement** la procédure à suivre pour transformer un dossier de PDF de cours (`CM PDF/<année>/<ressource>/`) en notes de cours structurées dans le vault Obsidian, avec les mêmes conventions de style, de nommage et de vérification à chaque fois.

**Déclencheur** : « dans *CM PDF* il y a les PDF de la ressource *R1-0X*, rédige-moi mes notes de cours ».

---

## 0. Prompt type à réutiliser

```
Dans le dossier "CM PDF" il y a les PDF de la ressource <R1-0X>.
Rédige mes notes de cours dans le vault Obsidian en suivant la méthode
_Méthode - Notes de cours depuis CM PDF.md (structure, nommage, style).
```

---

## 1. Repérage (ne rien supposer)

1. Localiser **tous** les PDF de la ressource :
   `**/CM PDF/**/<RESSOURCE>/*.pdf`
2. Localiser le vault : chercher un dossier `.obsidian` (attention aux fautes de frappe, ici `Obsidiant Vault`).
3. **Lister le contenu du vault avant d'écrire** : s'il contient déjà des notes, ne jamais écraser ; s'aligner sur la structure existante.
4. Regarder `core-plugins.json` pour savoir ce qui est activé (callouts, propriétés YAML, Mermaid, canvas…).

> [!warning] Règle d'or
> **Ne jamais supprimer ni écraser un fichier existant** sans l'avoir lu. Si un fichier proche existe déjà, compléter ou proposer une fusion.

---

## 2. Extraction du texte

### 2.1 Installer l'outil (une seule fois)

```powershell
python -m pip install pymupdf
```

### 2.2 Script d'extraction

Toujours **écrire un fichier `.py`** puis l'exécuter : les heredocs PowerShell (`python -c @"..."@`) cassent les échappements (`\"`, `%`, parenthèses).

```python
import pymupdf, glob, os

for f in sorted(glob.glob(r'D:\IUT\CM PDF\A1\R1-05\*.pdf')):
    d = pymupdf.open(f)
    print('=====', os.path.basename(f), 'pages:', d.page_count)
    for i, p in enumerate(d):
        print(f'--- page {i+1} ---')
        print(p.get_text())
```

Toujours définir l'encodage avant :

```powershell
$env:PYTHONIOENCODING="utf-8"
```

### 2.3 Détecter les slides « image »

Une page dont `len(p.get_text().strip())` est **petit** (< ~200 caractères) alors qu'elle a des images = slide image (tableau, schéma, capture). Il faut l'OCR ou la décrire.

```python
print(len(p.get_text().strip()), p.get_images(full=True), len(p.get_drawings()))
```

---

## 3. OCR des slides image

> [!danger] Piège constaté
> **Tesseract 3.02** (installé sur cette machine) renvoie une sortie **vide** sur les PNG rendus par PyMuPDF, même avec `-l eng`, `-psm 6` et un prétraitement ImageMagick. Ne pas perdre de temps : passer directement à l'OCR Windows.

### 3.1 Découper la slide en zones lisibles

```python
import pymupdf, os

out = r'C:\Users\antho\AppData\Local\Temp\opencode\pdfimg'
os.makedirs(out, exist_ok=True)

d = pymupdf.open(r'D:\IUT\CM PDF\A1\R1-05\<fichier>.pdf')
p = d[17]                      # page 18
for xref in [i[0] for i in p.get_images(full=True)]:
    for rect in p.get_image_rects(xref):
        if rect.width < 100 or rect.height < 50:
            continue
        pix = p.get_pixmap(dpi=300, clip=rect)
        pix.save(os.path.join(out, f'p18_{xref}.png'))
```

Et pour une page entière en 2×2 (avec léger recouvrement) :

```python
r = p.rect
for i in range(2):
    for j in range(2):
        clip = pymupdf.Rect(
            r.x0 + r.width * i / 2 - (0.03 * r.width if i else 0),
            r.y0 + r.height * j / 2 - (0.03 * r.height if j else 0),
            r.x0 + r.width * (i + 1) / 2 + (0.03 * r.width if i < 1 else 0),
            r.y0 + r.height * (j + 1) / 2 + (0.03 * r.height if j < 1 else 0),
        )
        p.get_pixmap(dpi=260, clip=clip).save(os.path.join(out, f'q{i}{j}.png'))
```

### 3.2 OCR avec Windows.Media.Ocr

Fichier `ocr_win.ps1` (à réutiliser tel quel) :

```powershell
Add-Type -AssemblyName System.Runtime.WindowsRuntime
$null = [Windows.Media.Ocr.OcrEngine, Windows.Foundation, ContentType = WindowsRuntime]
$null = [Windows.Graphics.Imaging.BitmapDecoder, Windows.Foundation, ContentType = WindowsRuntime]
$null = [Windows.Storage.StorageFile, Windows.Foundation, ContentType = WindowsRuntime]

$asTaskGeneric = ([System.WindowsRuntimeSystemExtensions].GetMethods() | ? { $_.Name -eq 'AsTask' -and $_.GetParameters().Count -eq 1 -and $_.GetParameters()[0].ParameterType.Name -eq 'IAsyncOperation`1' })[0]
function Await($WinRtTask, $ResultType) {
  $asTask = $asTaskGeneric.MakeGenericMethod($ResultType)
  $netTask = $asTask.Invoke($null, @($WinRtTask))
  $netTask.Wait(-1) | Out-Null
  $netTask.Result
}

$engine = [Windows.Media.Ocr.OcrEngine]::TryCreateFromUserProfileLanguages()
Get-ChildItem '<DOSSIER_PNG>\*.png' | ForEach-Object {
  Write-Output ("===== " + $_.Name)
  $file = Await ([Windows.Storage.StorageFile]::GetFileFromPathAsync($_.FullName)) ([Windows.Storage.StorageFile])
  $stream = Await ($file.OpenAsync([Windows.Storage.FileAccessMode]::Read)) ([Windows.Storage.Streams.IRandomAccessStream])
  $decoder = Await ([Windows.Graphics.Imaging.BitmapDecoder]::CreateAsync($stream)) ([Windows.Graphics.Imaging.BitmapDecoder])
  $bitmap = Await ($decoder.GetSoftwareBitmapAsync()) ([Windows.Graphics.Imaging.SoftwareBitmap])
  $result = Await ($engine.RecognizeAsync($bitmap)) ([Windows.Media.Ocr.OcrResult])
  $result.Lines | ForEach-Object { $_.Text }
}
```

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File ocr_win.ps1
```

### 3.3 Fallback si l'OCR est illisible

- Décrire la slide **honnêtement** (« logos des SGBD du marché », « schéma conceptuel → logique → implémentation »).
- **Ne jamais inventer** un contenu précis (chiffres, noms de colonnes) à partir d'une image floue.
- Le signaler à l'utilisateur à la fin.

---

## 4. Planification des notes

1. Lire **tout** le texte extrait avant d'écrire (pas de rédaction au fil de l'eau).
2. Découper en **une note par CM** + **une note d'index** par ressource.
3. La note d'index reprend le CM0 (présentation) : objectifs, prérequis, évaluation, plan, sources.
4. Écrire les notes **dans l'ordre du cours**, en gardant la numérotation des diapos comme fil logique.
5. Pour les parties non encore fournies, mettre `*(à venir)*` avec le titre exact de la partie.

---

## 5. Conventions de nommage

```
Obsidiant Vault/
└── <RESSOURCE> - <Titre lisible>/
    ├── <RESSOURCE> - Index.md
    ├── CM1 - <Titre>.md
    ├── CM2 - <Titre>.md
    └── ...
```

- Dossier : `<code ressource> - <titre>` → `R1-05 - Introduction aux BD et SQL`
- Fichiers : `CM<n> - <Titre du PDF sans le préfixe technique>` → `CM2 - Algèbre relationnelle.md`
- Index : `<RESSOURCE> - Index.md`
- **Pas de tiret cadratin (—) dans les noms de fichiers**, seulement ` - ` (tiret simple entouré d'espaces).
- Les accents sont autorisés dans les noms de fichiers (Windows/OneDrive OK).
- Les liens internes doivent **reprendre exactement** le nom du fichier sans `.md` : `[[CM2 - Algèbre relationnelle]]`.

---

## 6. Règles de rédaction

### 6.1 Frontmatter YAML (propriétés Obsidian)

```yaml
---
ressource: R1-05
type: cours          # cours | index | methode | td | fiche
cm: 2                # seulement pour les CM
titre: Algèbre relationnelle
auteur: <auteur du PDF>
version: V1.7.0      # version indiquée sur la slide de titre
tags:
  - R1-05
  - BD
  - <theme>
---
```

### 6.2 Structure d'une note de CM

1. Titre `#`
2. Barre de navigation : `[[... - Index|← Index]] · Suite : [[CM suivant]]`
3. Callout `> [!abstract] En bref` : 2–4 lignes de synthèse
4. Sections `## n. Titre` dans l'ordre du cours
5. Tableaux pour les énumérations, définitions, avantages/inconvénients
6. Mermaid pour les architectures, cycles, schémas
7. `## ✅ À retenir` : liste de puces de synthèse
8. `### Questions d'auto-évaluation` : checkboxes `- [ ]`

### 6.3 Callouts à utiliser (avec parcimonie)

| Callout | Usage |
|---------|-------|
| `> [!abstract]` | résumé d'ouverture |
| `> [!important]` | définition ou règle centrale |
| `> [!warning]` | piège, contrainte, restriction |
| `> [!tip]` | astuce, « en SQL », pour aller plus loin |
| `> [!note]` | précision, abréviations, vocabulaire |
| `> [!example]` | lecture d'un exemple |
| `> [!quote]` | citation exacte du cours |

### 6.4 Fidélité au cours — règles non négociables

- **Recopier les définitions du prof presque mot pour mot** (elles sont souvent piégeuses et c'est ce qui est attendu à l'examen).
- **Conserver les nuances du cours même si elles contredisent la théorie générale** — et le signaler dans un `> [!warning]`.
  *Exemple réel : dans ce cours la projection **ne déduplique pas**, seuls les opérateurs ensemblistes dédupliquent.*
- Conserver les **exemples chiffrés** et les **jeux de données** des slides (les recopier en tableaux markdown : c'est ce qui sert à s'entraîner).
- Ne pas ajouter de contenu hors-programme sans le marquer comme tel (`> [!tip] Pour aller plus loin`).
- Les **ajouts pédagogiques** (équivalents SQL, mémos) sont bienvenus mais **identifiés** comme compléments.
- Ne pas « corriger » silencieusement une faute du support : garder le fond, éventuellement signaler.

### 6.5 Mathématiques

- Obsidian rend **MathJax** : `$\pi_{nom}(Etudiant)$`, `$\sigma_{annee<3}(Etudiant)$`.
- Bloc : `$$R_1 = R_2 \div R_1$$`.
- **Jamais de `|` dans une formule placée dans un tableau markdown** (ça casse la colonne) : utiliser `\mid` ou sortir la formule du tableau.

### 6.6 Mermaid

- `flowchart LR/TD` pour les architectures et cycles.
- `sequenceDiagram` pour les échanges client/serveur.
- `erDiagram` pour les schémas relationnels (`int numD PK`, `int numD FK`, `int numP PK, FK`).
- `mindmap` pour les classifications d'opérateurs.
- Éviter les libellés trop longs : utiliser `<br/>` pour couper.

---

## 7. Vérification finale (obligatoire)

- [ ] Tous les PDF du dossier ont été traités (compter les fichiers).
- [ ] Chaque note a un frontmatter valide et des tags cohérents.
- [ ] **Tous les liens `[[...]]` pointent vers un fichier existant** (nom exact, casse exacte).
- [ ] L'index liste toutes les notes créées.
- [ ] Les tableaux markdown ont le bon nombre de colonnes (vérifier les `|` dans les cellules).
- [ ] Les blocs Mermaid sont fermés (```).
- [ ] Aucun fichier existant n'a été écrasé sans raison.
- [ ] Les slides images non exploitables sont signalées à l'utilisateur.
- [ ] Résumé final à l'utilisateur : arborescence créée + contenu de chaque note + limites (OCR, slides vides) + proposition de suite.

---

## 8. Pièges techniques déjà rencontrés

| Piège | Solution |
|-------|----------|
| `python -c @"..."@` casse les échappements | écrire un `.py` et l'exécuter |
| `SyntaxError` sur `f'...{img[\"ext\"]}'` | idem : fichier `.py`, ou concaténation `%` |
| Accents mal affichés en console | `$env:PYTHONIOENCODING="utf-8"` |
| Tesseract 3.02 → sortie vide | utiliser **Windows.Media.Ocr** (PowerShell) |
| `--psm` refusé par Tesseract 3.02 | `-psm` (mais OCR vide quand même → Windows OCR) |
| Vault nommé `Obsidiant Vault` | ne pas « corriger » le nom du dossier |
| Images `Indexed`/`DCTDecode` non OCR-isables | rendre la **zone** de la page en 300 dpi via `get_image_rects` |
| Formule avec `\|` dans un tableau | `\mid` ou sortir du tableau |

---

## 9. Squelette prêt à copier

```markdown
---
ressource: R1-0X
type: cours
cm: N
titre: <Titre>
auteur: <Auteur>
version: Vx.y.z
tags:
  - R1-0X
  - <theme>
---

# CM<N> — <Titre>

[[R1-0X - Index|← Index R1-0X]] · Suite : [[CM<N+1> - <Titre>]]

> [!abstract] En bref
> <2 à 4 lignes de synthèse.>

---

## 1. <Section du cours>

<définitions, tableau, schéma Mermaid>

> [!important] <Règle centrale>
> <contenu>

---

## ✅ À retenir

- <point clé 1>
- <point clé 2>

### Questions d'auto-évaluation

- [ ] <question 1>
- [ ] <question 2>
```
