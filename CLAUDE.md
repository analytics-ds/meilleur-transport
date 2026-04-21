# Hugo Site Factory

Ce repo est un template pour créer des sites blogs statiques avec Hugo, optimisés SEO/GEO, hébergés gratuitement sur GitHub Pages.

## Comment ça marche

Ce repo ne contient pas de site. Il contient les **instructions et templates** pour que Claude Code génère un site complet automatiquement.

### Premier lancement

1. L'utilisateur connecte Claude Code à ce repo
2. L'utilisateur tape `/create-site`
3. Claude pose les questions nécessaires (nom du site, couleurs, catégories, etc.)
4. Claude génère tout le site Hugo, les fichiers SEO, et configure le déploiement
5. L'utilisateur push sur GitHub, active GitHub Pages, le site est en ligne

### Utilisation courante

- `/create-article` : créer un nouvel article de blog (choix parmi plusieurs types : article standard, comparatif). Push automatiquement sur GitHub si le repo est configuré
- `/seo-setup` : générer ou mettre à jour les fichiers SEO techniques de base (robots.txt, llms.txt, sitemap, structured data)
- `/seo` : mode interactif pour modifier/ajouter des éléments SEO (meta tags, JSON-LD, audit on-page, etc.)
- `/serve` : lancer le serveur Hugo en local (prévisualisation sur `http://localhost:1313/`)
- `/share` : lancer Hugo + ngrok pour partager le site via un lien public (accessible par n'importe qui)
- `/github-setup` : créer un repo GitHub, push le code et activer GitHub Pages (mise en ligne du site)
- `/github-deploy` : push les modifications vers GitHub et déclencher le déploiement

## Structure du repo

```
.claude/
├── skills/
│   ├── create-site.md           ← Workflow création de site complet
│   ├── create-article.md        ← Workflow création d'article (multi-types)
│   ├── seo-setup.md             ← Workflow fichiers SEO techniques (baseline)
│   ├── seo.md                   ← Mode interactif SEO (modifications ponctuelles)
│   ├── serve.md                 ← Lancer le serveur Hugo en local
│   ├── share.md                 ← Lancer Hugo + ngrok (partage public)
│   ├── github-setup.md          ← Créer un repo GitHub + activer GitHub Pages
│   └── github-deploy.md         ← Push et déployer sur GitHub Pages
└── templates/
    ├── hugo-workflow.yml         ← GitHub Actions CI/CD
    ├── main.css                  ← CSS avec variables de charte graphique
    ├── articles/                 ← Templates d'articles par type
    │   ├── article-standard.md   ← Article informatif SEO + GEO (type par défaut)
    │   └── geo-comparatif.md     ← Article comparatif avec mise en avant
    ├── seo/                      ← Fichiers SEO techniques (éditables)
    │   ├── robots.txt            ← Modèle robots.txt
    │   ├── llms.txt              ← Modèle llms.txt
    │   └── structured-data/      ← Schémas JSON-LD
    │       ├── article.json      ← BlogPosting
    │       ├── organization.json ← Organization
    │       ├── author.json       ← Person (auteur)
    │       ├── breadcrumb.json   ← BreadcrumbList
    │       ├── website.json      ← WebSite
    │       └── faq.json          ← FAQPage (à intégrer manuellement)
    ├── layouts/
    │   ├── baseof.html           ← Layout de base
    │   ├── home.html             ← Page d'accueil
    │   ├── list.html             ← Pages de liste
    │   ├── single.html           ← Page article (avec affichage auteur)
    │   └── sitemap-html.html    ← Page plan du site (liste toutes les pages)
    └── partials/
        ├── header.html           ← Header/navigation
        ├── footer.html           ← Footer
        └── seo-head.html         ← Meta tags SEO + JSON-LD (OG, Twitter, canonical, schemas)
```

## Contexte du site

- **Nom du site** : Meilleur Transport
- **Description** : Trouvez le meilleur transport pour tous vos besoins. Guides, comparatifs et conseils pour particuliers et professionnels.
- **URL** : https://meilleur-transport.com/
- **Repo GitHub** : analytics-ds/meilleur-transport
- **Couleurs** :
  - Primaire : `#FFC800` (jaune AMV)
  - Primaire claire : `#FFD94D`
  - Fond : `#FFFFFF`
  - Fond alt : `#F5F5F5`
  - Accent : `#1A1A1A`
  - CTA : `#FFC800` / hover `#E6B400`
  - Texte : `#1A1A1A` / texte light `#6B7280`
- **Polices** : Manrope (titres, 600/700/800) + Inter (corps et UI, 400/500/600/700)
- **Catégories** : Particuliers, Professionnels
- **Langues** : fr (principale) + en (content sous `content/en/`)
- **Auteur** : Julien Mercier
- **URL auteur** : (à définir)
- **Fonction auteur** : Expert mobilité et transports

## Suivi des publications (MEMORY.md)

Le fichier `MEMORY.md` à la racine trace tous les articles publiés, classés par semaine. Il est mis à jour automatiquement par `/create-article`.

**Limite de publication : 4 articles par semaine maximum.** Avant chaque création d'article, le système vérifie le quota. Si 4 articles sont déjà publiés dans la semaine en cours, l'utilisateur est averti.

Cette limite sert à éviter la publication en masse et à maintenir un rythme de publication régulier, ce qui est meilleur pour le SEO.

## Règle IMPÉRATIVE : accents français partout

**Tout texte en français doit utiliser les accents corrects** : é, è, ê, à, â, ù, û, ô, î, ï, ç.

Cela s'applique à :
- Le contenu des pages et articles (frontmatter + corps)
- Les textes hardcodés dans les templates Hugo (layouts, partials)
- Les descriptions, titres, meta tags, paramètres dans `hugo.toml`
- Les commentaires de code
- Ce fichier `CLAUDE.md` et tous les skills / templates du repo

**Exception unique** : les slugs d'URL restent sans accents (convention SEO). Voir règle slugs plus bas.

**À chaque édition de fichier** : si je repère des accents manquants sur le fichier en cours, je les corrige au passage (ex : "cree" → "créé", "deja" → "déjà", "ca marche" → "ça marche", "mobilite" → "mobilité", "categorie" → "catégorie", "Francais" → "Français").

## Règle IMPÉRATIVE : publication de contenu

**À chaque publication d'article (fichier dans `content/blog/`), les 5 points suivants doivent être mis à jour :**

### 1. Frontmatter complet obligatoire

Chaque article doit avoir :
- `title`, `description`, `date`, `lastmod`
- `author: "Nom Prénom"` (nom complet) + `authors: ["Nom Prénom"]` (pour la taxonomie)
- `categories: ["Particuliers"]` ou `["Professionnels"]` (1 seule catégorie)
- `tags: [...]` (3-5 tags)
- **`image: "https://..."`** (URL de l'image, affichée sur home, listings et article)
- **`imageAlt: "..."`** (texte alternatif, max 125 car)
- `translationKey: "slug-identique-fr-en"` (si traduction EN créée)
- `draft: false`

### 2. Sitemap XML (automatique)

Le sitemap `/sitemap.xml` et ses versions par langue `/fr/sitemap.xml` + `/en/sitemap.xml` sont régénérés automatiquement à chaque `hugo` build. Vérifier après build :

```bash
hugo
grep "<nouveau-slug>" public/fr/sitemap.xml public/en/sitemap.xml
```

### 3. Plan de site HTML (automatique)

La page `/plan-du-site/` utilise le layout `sitemap-html.html` qui liste toutes les pages par catégorie. Mise à jour automatique au build.

Vérifier :
```bash
grep "<nouveau-slug>" public/plan-du-site/index.html
```

### 4. Home : 3 derniers articles (automatique)

La section "Guides & analyses" de la home affiche automatiquement les 3 articles les plus récents (tri par date décroissante) via :
```go
{{ range first 3 (where .Site.RegularPages "Section" "blog") }}
```

L'image (`image` + `imageAlt` du frontmatter) est affichée en thumbnail à gauche de la card. Pour que le nouvel article remonte, il suffit de mettre `date:` au jour courant.

### 5. Page auteur (automatique via taxonomie)

La taxonomie `authors` est configurée dans `hugo.toml`. Chaque article avec `authors: ["Nom Prénom"]` génère automatiquement :
- Une page `/authors/<slug>/` listant tous les articles de cet auteur
- Un lien automatique sous chaque article sur la home

Slug auto-généré : "Julien Mercier" -> `/authors/julien-mercier/`

### 6. llms.txt (manuel)

**À chaque publication, ajouter la ligne manuellement** dans `static/llms.txt` :

```markdown
## Articles de référence (FR)

- Titre complet de l'article : https://meilleur-transport.com/blog/slug-de-larticle/
```

### Workflow post-publication checklist

```bash
# 1. Build
hugo

# 2. Vérifications
grep "<slug>" public/fr/sitemap.xml
grep "<slug>" public/plan-du-site/index.html
grep "<titre>" static/llms.txt

# 3. Commit + push
git add -A && git commit -m "content: <titre-article>" && git push
```

## Règles générales

- Toujours utiliser `relURL` dans les templates Hugo pour les liens (compatibilité GitHub Pages)
- Les articles vont dans `content/blog/`
- Les slugs sont en minuscules, **sans accents**, mots séparés par des tirets (seule exception à la règle des accents)
- Ne JAMAIS utiliser `&` dans les noms de catégories ou de tags — toujours remplacer par "et" (Hugo génère un double tiret `--` dans le slug, ce qui casse les URLs)
- Le ton des articles est impersonnel (pas de je/tu/nous/vous) sauf instruction contraire
- Les specs d'article (mots minimum, H2, blocs obligatoires) dépendent du type choisi — lire les `<!-- NOTES POUR CLAUDE -->` dans chaque template d'article
- Chaque article doit contenir au minimum 3 liens internes contextuels vers d'autres articles du blog. L'ancre de chaque lien doit contenir le mot-clé principal de l'article cible
- L'auteur est ajouté automatiquement dans le frontmatter et affiché sur la page (configuré dans `hugo.toml [params]`)
- Les templates SEO dans `.claude/templates/seo/` sont éditables par l'utilisateur — toujours lire la version en place avant de générer
- Pour ajouter un nouveau type d'article, créer un `.md` dans `.claude/templates/articles/` — il sera automatiquement proposé par `/create-article`
- Pour ajouter un schéma JSON-LD, créer un `.json` dans `.claude/templates/seo/structured-data/` et utiliser `/seo` pour l'intégrer
- Chaque article doit avoir un champ `lastmod` dans le frontmatter (= date de dernière modification). Il est utilisé par le sitemap XML, le sitemap HTML et le schéma JSON-LD
- Quand un article est modifié, toujours mettre à jour le champ `lastmod` avec la date du jour
- Le sitemap HTML (`/plan-du-site/`) se régénère automatiquement à chaque build Hugo
- Toujours build et vérifier (`hugo`) avant de commit

## SEO : pages tags en noindex

Les pages de tags (`/tags/` et `/tags/<slug>/`) sont configurées en **noindex permanent** dans `themes/meilleur-transport/layouts/partials/seo-head.html`. Raison : contenu maigre / duplication avec les listings catégories. Ne pas retirer cette règle.

## Comment répondre à l'utilisateur

- Tutoiement, ton décontracté
- Pas de jargon technique sans explication
- Réponses structurées avec listes à puces
- Pas d'emoji sauf demande explicite
