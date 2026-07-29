# TechWall

Site vitrine statique pour [TechWall](https://www.youtube.com/@TechWall), une chaîne YouTube qui propose des cours gratuits en français sur le développement web, le mobile, le Big Data et la cybersécurité.

> Depuis 2018, la chaîne est animée par trois enseignants-chercheurs de l'INSAT (Institut National des Sciences Appliquées et de Technologie, Tunis).

## Pages

| Fichier | Contenu |
|---|---|
| `index.html` | Accueil — hero, stats, catégories de cours, cours récents, équipe |
| `Cours.html` | Catalogue complet, filtrable par catégorie (Web, Mobile, Cybersécurité, Big Data, Algo) |
| `APropos.html` | Présentation de l'équipe, mission, chronologie de la chaîne |

## Stack technique

Le site est en HTML statique, sans build step côté contenu. Chaque page charge `support.js`, un runtime léger qui :

- repère la balise `<x-dc>` et le `<script type="text/x-dc" data-dc-script">` de la page,
- monte un composant React (chargé en UMD depuis un CDN) à partir de la classe `class Component extends DCLogic` définie dans ce script,
- interprète le template : interpolation `{{ expr }}`, boucles `<sc-for list="{{ ... }}" as="item">`, conditions `<sc-if value="{{ ... }}">`.

Les données de chaque page (stats, catégories, membres de l'équipe, timeline, filtres...) sont renvoyées par la méthode `renderVals()` de la classe `Component`, directement dans le HTML de la page.

**⚠️ Ne pas éditer `support.js` à la main** — le fichier est généré depuis `dc-runtime/src/*.ts` (voir l'en-tête du fichier). Toute modification du runtime doit passer par ce projet séparé, puis être rebuild (`cd dc-runtime && bun run build`).

## Structure du projet

```
.
├── index.html          # Page d'accueil
├── Cours.html          # Catalogue des cours
├── APropos.html        # Page À propos / équipe
├── support.js          # Runtime (généré, ne pas éditer)
├── CONTEXTE.md          # Notes de contexte du projet
└── assets/
    ├── logo.jpg         # Logo TechWall
    ├── team/            # Photos des 3 fondateurs
    ├── playlists/       # Miniatures des playlists YouTube
    └── videos/          # Miniatures des vidéos mises en avant
```

## Lancer en local

Le runtime va re-fetcher la page courante (`fetch(location.href)`) pour rafraîchir le template, ce qui ne fonctionne pas en ouvrant le fichier directement (`file://`). Servez le dossier avec un serveur statique, par exemple :

```bash
python3 -m http.server 8000
# ou
npx serve .
```

Puis ouvrez `http://localhost:8000/index.html`.

## Contenu

Les playlists, vidéos et statistiques affichées viennent d'exports JSON YouTube importés dans `context/import/` (`techwallPlaylistInfos.json`, `videosTechwall.json`), puis recopiés dans les composants. Les photos des trois fondateurs ont été fournies directement par Aymen — aucune image n'est scrapée automatiquement (notamment pas depuis LinkedIn, sans consentement explicite des personnes concernées).

La catégorie Cybersécurité n'a pas encore de miniatures de playlist disponibles ; les cartes correspondantes affichent un placeholder.

## Déploiement

Aucun déploiement en ligne pour l'instant. Le site étant 100 % statique (HTML + `support.js` + assets), il est prêt à être publié tel quel sur GitHub Pages, Netlify ou Vercel sans étape de build supplémentaire.

## Équipe

- **Aymen Sellaouti** — Co-fondateur, maître assistant à l'INSAT (Symfony, Angular, NestJs)
- **Lilia Sfaxi** — Co-fondatrice, maître assistante à l'INSAT (Big Data — Hadoop & Compagnie, Atelier Spark)
- **Souheib Yousfi** — Co-fondateur, enseignant-chercheur (sécurité, pentest, cybersécurité)

## Liens

- Dépôt : [github.com/aymensellaouti/techwall-site](https://github.com/aymensellaouti/techwall-site)
- Chaîne YouTube : [youtube.com/@TechWall](https://www.youtube.com/@TechWall)
