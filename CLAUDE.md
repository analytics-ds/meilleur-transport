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

- `/create-article-geo` : créer un nouvel article de blog (choix parmi plusieurs types : article standard, comparatif). Push automatiquement sur GitHub si le repo est configuré
- `/create-article-auto` : **publication automatique** d'un article evergreen SEO bilingue FR+EN depuis la roadmap `roadmap.yaml`. Full auto, aucun input humain. Conçue pour être déclenchée par une routine planifiée (ex: 2x/semaine via `/schedule`). C'est la **méthode 1** (CCR cloud). Voir section "Publications evergreen automatiques" plus bas
- `/create-article-seo` : **production polyvalente locale** d'articles evergreen SEO bilingues FR+EN. Tourne sur le Mac de Damien (Opus 4.7, fetch concurrents réel, maillage cross-batch). 3 modes : (A) suivre la roadmap.yaml du blog, (B) roadmap externe fournie, (C) KW à la demande. C'est la **méthode 2** (batch local + GitHub Actions cron). Voir section "Publications evergreen automatiques" plus bas
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
│   ├── create-article-geo/      ← Workflow création d'article GEO (multi-types, interactif)
│   ├── create-article-auto/     ← Méthode 1 : publication auto d'article evergreen SEO depuis la roadmap (CCR cloud)
│   ├── create-article-seo/      ← Méthode 2 : production locale polyvalente d'articles evergreen SEO (Mac, Opus 4.7)
│   ├── seo-setup.md             ← Workflow fichiers SEO techniques (baseline)
│   ├── seo.md                   ← Mode interactif SEO (modifications ponctuelles)
│   ├── serve.md                 ← Lancer le serveur Hugo en local
│   ├── share.md                 ← Lancer Hugo + ngrok (partage public)
│   ├── github-setup.md          ← Créer un repo GitHub + activer GitHub Pages
│   └── github-deploy.md         ← Push et déployer sur GitHub Pages
└── templates/
    ├── hugo-workflow.yml         ← GitHub Actions CI/CD
    ├── roadmap-template.yaml     ← Squelette commenté de la roadmap éditoriale evergreen (pour /create-article-auto)
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
- **Google Analytics 4** : propriété "meilleur-transport.com" sur le compte Google analytics@datashake.fr (compte GA "Meilleur Transport" 397659088), ID de mesure `G-TNV9WBJCZ6`, tag gtag.js dans `themes/meilleur-transport/layouts/_default/baseof.html` (installé le 2026-06-11)
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

Le fichier `MEMORY.md` a la racine trace tous les articles publies, classes par semaine. Il est mis a jour automatiquement par `/create-article-geo` et `/create-article-seo`.

**Repere indicatif : 4 articles par semaine.** C'est un rythme cible pour eviter la publication en masse. **Ce n'est pas un blocage.**

Comportement par skill :
- `/create-article-geo` (creation manuelle interactive) : si 4 articles ou plus deja publies cette semaine, **simple warning** affiche a l'utilisateur. L'utilisateur peut continuer en validant. Ne JAMAIS bloquer.
- `/create-article-seo` (batch local methode 2) : meme principe, warning soft en pre-validation, jamais de blocage.
- `/create-article-auto` (routine cron methode 1, mardi/vendredi) : **la regle ne s'applique pas**. La routine publie systematiquement l'entree eligible de la roadmap, sans lire le quota hebdo. Aucun check, aucun warning.

## Garde-fou cannibalisation a l'ajout dans la roadmap

Quand l'utilisateur (ou Claude pour son compte) ajoute un nouveau mot-cle dans `roadmap.yaml` (ou dans une roadmap externe en mode B/C de `/create-article-seo`), executer un check de cannibalisation **soft** avant insertion :

1. Lire toutes les entrees existantes de `roadmap.yaml` (tous statuts confondus : todo, queued, done, failed).
2. Pour chaque entree existante, comparer le `kw` propose au `kw` existant :
   - Tokeniser les deux KW (lowercase, retirer stop words FR/EN courants : le/la/les/de/du/des/un/une/a/au/aux/the/of/and/or/in/on/for...).
   - Calculer l'overlap : nombre de tokens communs / nombre de tokens du KW le plus court.
   - Drapeau "risque" si overlap >= 50% OU si l'un des KW est sous-string complet de l'autre.
3. Si au moins un drapeau "risque" est leve : afficher un warning a l'utilisateur listant les KW suspects et demander confirmation explicite ("Cannibalisation potentielle avec : [liste]. Ajouter quand meme ? oui/non").
4. Si aucun risque : ajouter directement, pas de warning.

Ce garde-fou est purement informatif. L'utilisateur peut toujours forcer l'ajout. L'objectif est juste de l'avertir avant qu'il ne commande deux articles qui se cannibaliseraient en SERP.

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
- Pour ajouter un nouveau type d'article, créer un `.md` dans `.claude/templates/articles/` — il sera automatiquement proposé par `/create-article-geo`
- Pour ajouter un schéma JSON-LD, créer un `.json` dans `.claude/templates/seo/structured-data/` et utiliser `/seo` pour l'intégrer
- Chaque article doit avoir un champ `lastmod` dans le frontmatter (= date de dernière modification). Il est utilisé par le sitemap XML, le sitemap HTML et le schéma JSON-LD
- Quand un article est modifié, toujours mettre à jour le champ `lastmod` avec la date du jour
- Le sitemap HTML (`/plan-du-site/`) se régénère automatiquement à chaque build Hugo
- Toujours build et vérifier (`hugo`) avant de commit

## Règle IMPÉRATIVE : le tableau de valeurs se place juste après le chapô

Dès qu'un article contient un tableau comparatif ou un tableau de valeurs (prix, critères, notes, délais, verdicts), ce tableau doit être placé **immédiatement après le chapô "En bref" et le paragraphe d'introduction, en tout premier H2 de l'article**. Jamais au milieu, jamais en fin d'article.

Pourquoi : c'est une règle GEO, pas de confort de lecture. Les LLMs (ChatGPT Search, Perplexity, AI Overviews) découpent la page en passages et pondèrent plus fortement ceux du début du document. Un tableau de données structurées placé haut est le passage le plus facilement citable de l'article (données denses, comparables, extractibles en une seule fois). Placé à 60 % de la page, il perd cette pondération et il sort souvent hors de la fenêtre de contenu réellement lue par le crawler du moteur génératif. Même logique côté SEO classique pour les featured snippets de type tableau.

Comment faire :
- Le tableau constitue sa propre section H2, ancrée `{#tableau}` (FR) / `{#table}` (EN)
- Ordre imposé : frontmatter → chapô `> **En bref**` → paragraphe d'introduction → H2 tableau → reste de l'article
- Les sections détaillées site par site / produit par produit viennent APRÈS le tableau, jamais avant
- Le paragraphe de transition qui commente le tableau reste collé au tableau, il monte avec lui
- Appliquer strictement la même position dans la version EN de l'article (parité FR/EN obligatoire)
- Sur un article existant qu'on remonte, mettre à jour `lastmod`

## SEO : pages tags en noindex

Les pages de tags (`/tags/` et `/tags/<slug>/`) sont configurées en **noindex permanent** dans `themes/meilleur-transport/layouts/partials/seo-head.html`. Raison : contenu maigre / duplication avec les listings catégories. Ne pas retirer cette règle.

## Publications evergreen automatiques

En plus des articles GEO (geo-comparatif, rédigés à la main via `/create-article-geo`), chaque blog peut publier automatiquement des articles evergreen SEO. Deux méthodes coexistent dans le réseau, le choix se fait par blog en fonction du contexte (modèle, fréquence, fetch concurrents, maillage).

### Méthode 1 : CCR cloud auto (`/create-article-auto`)

- **Skill** : `/create-article-auto`
- **Exécution** : sandbox cloud Anthropic (CCR), déclenchée par une routine `/schedule` (cron 2x/semaine, mardi + vendredi 3h du matin)
- **Modèle** : Sonnet 4.6 forcé (Opus 4.7 a un bug Stream idle timeout en CCR)
- **Fetch concurrents** : bloqué par le sandbox (aucun accès aux domaines commerciaux), analyse limitée aux métadonnées SerpAPI (titles + snippets + PAA)
- **Maillage cross-batch** : non (1 article à la fois)
- **Publication** : push immédiat -> en ligne tout de suite
- **Cas d'usage** : tient la cadence sans intervention humaine, idéal pour les blogs avec roadmap stable
- **Exemple en prod dans le réseau** : `como-blog-ai`

### Méthode 2 : batch local + GitHub Actions cron (`/create-article-seo`)

- **Skill** : `/create-article-seo` polyvalente
- **Exécution** : Mac de Damien (local), Opus 4.7 sans contrainte
- **Modèle** : Opus 4.7 (qualité max, pas de bug timeout)
- **Fetch concurrents** : marche normalement, analyse SERP avec lecture des 3-5 pages concurrentes
- **Maillage cross-batch** : oui (les articles produits dans une même batch se citent entre eux)
- **3 modes au choix** :
  - **(A) Roadmap blog** : N premières entrées `todo` triées par scheduled_date
  - **(B) Roadmap externe** : roadmap fournie par l'utilisateur (Sheet, KW client)
  - **(C) KW à la demande** : 1 ou plusieurs KW dans le chat
- **3 stratégies de scheduling** :
  - Garder les `scheduled_date` source (défaut)
  - Cascade remapping à partir d'une date X (décale en avant)
  - Prochain slot dispo dans la cadence (mardi/vendredi non occupé)
- **Publication** : article écrit avec `publishDate` futur. Hugo (`buildFuture: false`) le masque jusqu'à la date. GitHub Actions cron mardi/vendredi 3h Paris rebuild le site, l'article apparaît automatiquement quand sa date est arrivée.
- **Cas d'usage** : production en lot mensuelle, qualité max, maillage interne propre
- **Exemple en prod dans le réseau** : `ma-bonne-sante`

### Principe commun aux 2 méthodes

- **SEO pur**, pas GEO : pas de "prompt GEO", pas de "En bref numéroté". Juste un mot-clé SEO ciblé, analyse SERP, structure Hn basée sur les concurrents, rédaction optimisée.
- **Bilingue FR + EN** comme tous les articles du réseau (trad directe de la version FR).
- **Human in the loop** uniquement sur la roadmap : c'est l'humain qui décide des mots-clés à cibler et de leur date de publication.

### Roadmap éditoriale

Fichier : `roadmap.yaml` à la racine du blog. Format documenté dans `.claude/templates/roadmap-template.yaml`.

Chaque entrée = 1 article à publier. Champs éditables par l'humain :
- `kw` (obligatoire) : mot-clé SEO principal dans la langue principale du blog
- `category` (obligatoire) : doit matcher une catégorie définie dans `hugo.toml`
- `scheduled_date` (obligatoire) : date à partir de laquelle l'agent peut publier (YYYY-MM-DD)
- `status` : `todo` | `done` | `failed`

Champs remplis par l'agent (ne pas toucher sauf pour réactiver un `failed`) :
- `published_date`, `published_url_fr`, `published_url_en`, `error`

### Comment l'humain modifie la roadmap

- **Ajouter une entrée** : copier un bloc existant, remplir `kw` + `category` + `scheduled_date`, laisser les autres champs tels quels, garder `status: todo`.
- **Reporter une entrée** : modifier `scheduled_date`.
- **Annuler une entrée non encore traitée** : supprimer le bloc, ou passer `status` à `done` manuellement (l'agent l'ignorera).
- **Débloquer un `failed`** : corriger la cause (ex: `kw` trop concurrentiel, category invalide), repasser `status: todo`, vider `error`.

Demander à Claude "ajoute telle entrée à la roadmap du blog X" ou "passe la roadmap de X ça" fonctionne aussi, tant que le format YAML reste respecté.

### Exécution

- **Manuelle (test méthode 1)** : se placer dans le dossier du blog, taper `/create-article-auto`. L'agent prend la prochaine entrée éligible et déroule.
- **Planifiée (production méthode 1)** : routine `/schedule` qui lance `/create-article-auto` dans le contexte du blog, 2x/semaine (mardi + vendredi, 3h du mat recommandé pour minimiser les conflits avec les autres consultants).
- **Batch (méthode 2)** : se placer dans le dossier du blog, taper `/create-article-seo`. La skill propose les 3 modes (A/B/C) puis les 3 stratégies de scheduling. Articles produits avec `publishDate` futur, Hugo les masque, le cron GitHub Actions du blog (mardi/vendredi 3h Paris) les rend visibles automatiquement quand leur date arrive.

### Échecs

Une entrée qui échoue passe en `status: failed` avec `error: "[étape] [message]"`. Elle n'est **pas retentée automatiquement**. L'humain corrige, repasse en `todo`, l'agent la reprendra au lancement suivant.

Le suivi des articles publiés en auto se fait via :
- Le champ `published_date` / URLs de chaque entrée de la roadmap
- Le `MEMORY.md` à la racine du blog (suffixe ` | auto` sur les lignes générées par cette skill)

## Comment répondre à l'utilisateur

- Tutoiement, ton décontracté
- Pas de jargon technique sans explication
- Réponses structurées avec listes à puces
- Pas d'emoji sauf demande explicite

## Images d'article

- **Image hero OBLIGATOIRE, jamais optionnelle.** Elle est recuperee par `.claude/scripts/fetch-image.sh`, qui suit la **cascade Pexels -> Unsplash -> Openverse -> visuel de charte genere** (`.claude/scripts/make-placeholder.py`) et ne rend jamais la main sans visuel. Revue du 2026-09-04 : Openverse n'est plus la source nominale, il ne produisait que 15 photos pertinentes sur 45 heros mesures. Les cles `PEXELS_API_KEY` et `UNSPLASH_ACCESS_KEY` viennent du **prompt de la routine** ou du `.env` local, **jamais du repo** qui est public ; sans cle la cascade demarre a Openverse et l'article sort quand meme. Le registre `.claude/hero-sources.json` empeche deux articles de porter la meme photo. Si `image` est renseigne, `imageAlt` est obligatoire, et `imageCredit` des que le script renvoie un credit. **Controle visuel obligatoire avant publication, quelle que soit la banque.**
