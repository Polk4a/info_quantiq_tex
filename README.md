# Informatique quantique — notes de cours (INSA CVL, 4A FISE SF)

Cours de M. Adell, rédigé en LaTeX par Niels LOPEZ. De l'onde de Schrödinger au circuit quantique :
mécanique ondulatoire, notation de Dirac, opérateurs et matrice de densité, portes quantiques,
intrication, téléportation, exercices corrigés.

Le `main.tex` sert aussi de **template INSA CVL** : charte rouge, en-tête avec logo, page de titre,
sections optionnelles en commentaire.

## Compilation

```bash
pdflatex main.tex
biber main            # bibliographie (biblatex + biber, style APA)
pdflatex main.tex
pdflatex main.tex     # 2 passes : sommaire, renvois, glossaire, figures TikZ
```

Le glossaire utilise `\makenoidxglossaries` : pas de `makeindex` à lancer.
Dépendances : une distribution TeX Live/MiKTeX complète (tcolorbox, TikZ, biblatex, glossaries…) et `biber`.

## Arborescence

```
main.tex            Préambule, informations du document, ordre des fichiers importés
pagetitre.tex       Page de titre (page 1 du PDF, sans en-tête ni numéro)
introduction.tex    Introduction (section non numérotée, présente au sommaire)
chapitre1.tex       1. Histoire et mécanique quantique
chapitre2.tex       2. Notation de Dirac et espace de Hilbert
chapitre3.tex       3. Opérateurs, matrice de densité, sphère de Bloch, produit tensoriel
chapitre4.tex       4. Portes quantiques et systèmes multi-qubits
chapitre5.tex       5. États quantiques, intrication, états de Bell
chapitre6.tex       6. Exemples et exercices corrigés
conclusion.tex      Conclusion (non numérotée, au sommaire)
glossaire.tex       Entrées du glossaire (~47 termes)
sources.bib         Bibliographie (16 références)
images/logo_insa.png  Logo INSA (seul fichier image : tous les schémas sont en TikZ)
```

## Organisation de `main.tex`

1. **Informations du document** : `\doctitre`, `\docsoustitre`, `\doctagline`, `\docauteur`,
   `\docprof`, `\docecole`, `\docdate`. Pour réutiliser le template, modifier ce bloc.
2. **`pagesdegarde`** : nombre de pages de garde au début (page de titre ou pages importées avec
   `\includepdf`). Elles sont comptées dans la numérotation (numéro affiché = page du PDF) mais sans
   en-tête ni numéro. Valeur actuelle : 1.
3. **Packages** : maths, tcolorbox, TikZ (+ bibliothèques), titlesec, fancyhdr, eso-pic, glossaries,
   biblatex, hyperref, pdfpages, etc. Les packages inutilisés sont volontairement conservés.
4. **Couleurs** : `insarouge` (#E42618, couleur principale) et `insagris` (#5F5F5F), relevées sur le logo.
5. **Macros** et **environnements** (voir ci-dessous).
6. **Corps du document**, dans l'ordre : page de titre → sommaire → introduction → chapitres 1 à 6 →
   conclusion → bibliographie → glossaire. Les sections optionnelles (résumés, table des illustrations,
   annexes, `chapitre7`) sont présentes **en commentaire** : il suffit de les décommenter.

Chaque fichier est importé par `\include{...}` (saut de page automatique, compilation partielle possible
avec `\includeonly{chapitre2}` ; la numérotation est conservée).
Un chapitre = une `\section` ; les sous-parties sont des `\subsection`.

## Structure type d'un chapitre

1. Sections de cours (`\subsection`), avec définitions, résultats, remarques, figures TikZ.
2. `\clearpage`, puis **`Récapitulatif du chapitre`** (dernière sous-section) :
   - schéma de synthèse (TikZ),
   - encadré `aretenir` (points à retenir),
   - tableau de formules avec renvois aux sections,
   - encadré `savoirfaire` (liste de compétences, avec renvois aux exercices du chapitre 6).

Le chapitre 6 regroupe 18 exercices (numérotés 6.1 à 6.18), rangés selon l'ordre des chapitres 1 à 5.

## Environnements (encadrés colorés)

| Environnement | Usage |
|---|---|
| `\begin{definition}{Titre}` | définition |
| `\begin{resultat}{Titre}` | théorème, propriété, postulat |
| `\begin{remarque}[Titre]` | remarque |
| `\begin{aretenir}` / `\begin{savoirfaire}` | encadrés de récapitulatif |
| `\begin{exercice}{Titre}` | énoncé ; numéroté `section.n`, étiquetable par `\label{ex:...}` |
| `\begin{correction}` | correction (encadré gris, coupable entre deux pages) |

## Macros principales

| Macro | Rendu |
|---|---|
| `\ket{a}`, `\bra{a}`, `\braket{a}{b}` | \|a⟩, ⟨a\|, ⟨a\|b⟩ |
| `\ii`, `\ee`, `\dd` | i, e, d droits (unité imaginaire, exponentielle, différentielle) |
| `\pd`, `\pdd` | dérivées partielles première et seconde |
| `\Hop` | opérateur hamiltonien |

Circuits quantiques (TikZ) : styles/pics `ctl` (contrôle), `cible` (⊕), `croix` (SWAP),
`mesure` (compteur) et style de nœud `porte` (boîte de porte).

## Conventions

- **Circuits** : le fil du haut correspond au dernier chiffre du ket ; `|ab⟩_AB = |a⟩_A ⊗ |b⟩_B`.
  CNOT du cours : contrôle B, cible A, `CNOT|a,b⟩ = |a⊕b,b⟩`. Générateur de Bell
  `G = CNOT·(I⊗H)`, décodage `G†`.
- **Postulats numérotés** : 1 état (ch. 2), 2 mesure/Born (ch. 2), 3 systèmes composites (ch. 3),
  4 évolution unitaire (ch. 4).
- **Renvois** : `\label`/`\ref` préfixés (`sec:`, `chap:`, `fig:`, `ex:`). Ne pas écrire « section
  suivante » en dur.
- **TikZ** : dimensions en cm explicites, coordonnées nommées absolues ; ne pas nommer `\c` ou `\l`
  une variable de `\foreach`. Figures : `\begin{figure}` ou, dans une correction, `minipage` +
  `\captionof{figure}`.
- **Titres** contenant des kets : `\texorpdfstring{$\ket{0}$}{|0>}`.
- **Décimales** en mode math : `0{,}81`.

## Fichiers à ne pas versionner

```gitignore
*.aux
*.log
*.toc
*.out
*.bbl
*.bcf
*.blg
*.run.xml
*.synctex.gz
```

Le `main.pdf` peut être versionné ou non, selon l'usage du dépôt.
