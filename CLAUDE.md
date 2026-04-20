# Hugo Site Factory

Ce repo est un template pour creer des sites blogs statiques avec Hugo, optimises SEO/GEO, heberges gratuitement sur GitHub Pages.

## Comment ca marche

Ce repo ne contient pas de site. Il contient les **instructions et templates** pour que Claude Code genere un site complet automatiquement.

### Premier lancement

1. L'utilisateur connecte Claude Code a ce repo
2. L'utilisateur tape `/create-site`
3. Claude pose les questions necessaires (nom du site, couleurs, categories, etc.)
4. Claude genere tout le site Hugo, les fichiers SEO, et configure le deploiement
5. L'utilisateur push sur GitHub, active GitHub Pages, le site est en ligne

### Utilisation courante

- `/create-article` : creer un nouvel article de blog (choix parmi plusieurs types : article standard, comparatif). Push automatiquement sur GitHub si le repo est configure
- `/seo-setup` : generer ou mettre a jour les fichiers SEO techniques de base (robots.txt, llms.txt, sitemap, structured data)
- `/seo` : mode interactif pour modifier/ajouter des elements SEO (meta tags, JSON-LD, audit on-page, etc.)
- `/serve` : lancer le serveur Hugo en local (previsualisation sur `http://localhost:1313/`)
- `/share` : lancer Hugo + ngrok pour partager le site via un lien public (accessible par n'importe qui)
- `/github-setup` : creer un repo GitHub, push le code et activer GitHub Pages (mise en ligne du site)
- `/github-deploy` : push les modifications vers GitHub et declencher le deploiement

## Structure du repo

```
.claude/
├── skills/
│   ├── create-site.md           ← Workflow creation de site complet
│   ├── create-article.md        ← Workflow creation d'article (multi-types)
│   ├── seo-setup.md             ← Workflow fichiers SEO techniques (baseline)
│   ├── seo.md                   ← Mode interactif SEO (modifications ponctuelles)
│   ├── serve.md                 ← Lancer le serveur Hugo en local
│   ├── share.md                 ← Lancer Hugo + ngrok (partage public)
│   ├── github-setup.md          ← Creer un repo GitHub + activer GitHub Pages
│   └── github-deploy.md         ← Push et deployer sur GitHub Pages
└── templates/
    ├── hugo-workflow.yml         ← GitHub Actions CI/CD
    ├── main.css                  ← CSS avec variables de charte graphique
    ├── articles/                 ← Templates d'articles par type
    │   ├── article-standard.md   ← Article informatif SEO + GEO (type par defaut)
    │   └── geo-comparatif.md     ← Article comparatif avec mise en avant
    ├── seo/                      ← Fichiers SEO techniques (editables)
    │   ├── robots.txt            ← Modele robots.txt
    │   ├── llms.txt              ← Modele llms.txt
    │   └── structured-data/      ← Schemas JSON-LD
    │       ├── article.json      ← BlogPosting
    │       ├── organization.json ← Organization
    │       ├── author.json       ← Person (auteur)
    │       ├── breadcrumb.json   ← BreadcrumbList
    │       ├── website.json      ← WebSite
    │       └── faq.json          ← FAQPage (a integrer manuellement)
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
- **Categories** : Particuliers, Professionnels
- **Langues** : fr (principale) + en (content sous `content/en/`)
- **Auteur** : Julien Mercier
- **URL auteur** : (a definir)
- **Fonction auteur** : Expert mobilite et transports

## Suivi des publications (MEMORY.md)

Le fichier `MEMORY.md` a la racine trace tous les articles publies, classes par semaine. Il est mis a jour automatiquement par `/create-article`.

**Limite de publication : 4 articles par semaine maximum.** Avant chaque creation d'article, le systeme verifie le quota. Si 4 articles sont deja publies dans la semaine en cours, l'utilisateur est averti.

Cette limite sert a eviter la publication en masse et a maintenir un rythme de publication regulier, ce qui est meilleur pour le SEO.

## Regle IMPERATIVE : publication de contenu

**A chaque publication d'article (fichier dans `content/blog/`), les 5 points suivants doivent etre mis a jour :**

### 1. Frontmatter complet obligatoire

Chaque article doit avoir :
- `title`, `description`, `date`, `lastmod`
- `author: "Nom Prenom"` (nom complet) + `authors: ["Nom Prenom"]` (pour la taxonomie)
- `categories: ["Particuliers"]` ou `["Professionnels"]` (1 seule categorie)
- `tags: [...]` (3-5 tags)
- **`image: "https://..."`** (URL de l'image, affichee sur home, listings et article)
- **`imageAlt: "..."`** (texte alternatif, max 125 car)
- `translationKey: "slug-identique-fr-en"` (si traduction EN creee)
- `draft: false`

### 2. Sitemap XML (automatique)

Le sitemap `/sitemap.xml` et ses versions par langue `/fr/sitemap.xml` + `/en/sitemap.xml` sont regeneres automatiquement a chaque `hugo` build. Verifier apres build :

```bash
hugo
grep "<nouveau-slug>" public/fr/sitemap.xml public/en/sitemap.xml
```

### 3. Plan de site HTML (automatique)

La page `/plan-du-site/` utilise le layout `sitemap-html.html` qui liste toutes les pages par categorie. Mise a jour automatique au build.

Verifier :
```bash
grep "<nouveau-slug>" public/plan-du-site/index.html
```

### 4. Home : 3 derniers articles (automatique)

La section "Guides & analyses" de la home affiche automatiquement les 3 articles les plus recents (tri par date decroissante) via :
```go
{{ range first 3 (where .Site.RegularPages "Section" "blog") }}
```

L'image (`image` + `imageAlt` du frontmatter) est affichee en thumbnail a gauche de la card. Pour que le nouvel article remonte, il suffit de mettre `date:` au jour courant.

### 5. Page auteur (automatique via taxonomie)

La taxonomie `authors` est configuree dans `hugo.toml`. Chaque article avec `authors: ["Nom Prenom"]` genere automatiquement :
- Une page `/authors/<slug>/` listant tous les articles de cet auteur
- Un lien automatique sous chaque article sur la home

Slug auto-genere : "Julien Mercier" -> `/authors/julien-mercier/`

### 6. llms.txt (manuel)

**A chaque publication, ajouter la ligne manuellement** dans `static/llms.txt` :

```markdown
## Articles de reference (FR)

- Titre complet de l'article : https://meilleur-transport.com/blog/slug-de-larticle/
```

### Workflow post-publication checklist

```bash
# 1. Build
hugo

# 2. Verifications
grep "<slug>" public/fr/sitemap.xml
grep "<slug>" public/plan-du-site/index.html
grep "<titre>" static/llms.txt

# 3. Commit + push
git add -A && git commit -m "content: <titre-article>" && git push
```

## Regles generales

- Toujours utiliser `relURL` dans les templates Hugo pour les liens (compatibilite GitHub Pages)
- Les articles vont dans `content/blog/`
- Les slugs sont en minuscules, sans accents, mots separes par des tirets
- Ne JAMAIS utiliser `&` dans les noms de categories ou de tags — toujours remplacer par "et" (Hugo genere un double tiret `--` dans le slug, ce qui casse les URLs)
- Le ton des articles est impersonnel (pas de je/tu/nous/vous) sauf instruction contraire
- Les specs d'article (mots minimum, H2, blocs obligatoires) dependent du type choisi — lire les `<!-- NOTES POUR CLAUDE -->` dans chaque template d'article
- Chaque article doit contenir au minimum 3 liens internes contextuels vers d'autres articles du blog. L'ancre de chaque lien doit contenir le mot-cle principal de l'article cible
- L'auteur est ajoute automatiquement dans le frontmatter et affiche sur la page (configure dans `hugo.toml [params]`)
- Les templates SEO dans `.claude/templates/seo/` sont editables par l'utilisateur — toujours lire la version en place avant de generer
- Pour ajouter un nouveau type d'article, creer un `.md` dans `.claude/templates/articles/` — il sera automatiquement propose par `/create-article`
- Pour ajouter un schema JSON-LD, creer un `.json` dans `.claude/templates/seo/structured-data/` et utiliser `/seo` pour l'integrer
- Chaque article doit avoir un champ `lastmod` dans le frontmatter (= date de derniere modification). Il est utilise par le sitemap XML, le sitemap HTML et le schema JSON-LD
- Quand un article est modifie, toujours mettre a jour le champ `lastmod` avec la date du jour
- Le sitemap HTML (`/plan-du-site/`) se regenere automatiquement a chaque build Hugo
- Toujours build et verifier (`hugo`) avant de commit

## Comment repondre a l'utilisateur

- Tutoiement, ton decontracte
- Pas de jargon technique sans explication
- Reponses structurees avec listes a puces
- Pas d'emoji sauf demande explicite
