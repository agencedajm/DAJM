# Passation : Vibe, le framework de création web de DAJM

Ce document lance une nouvelle conversation de conception. Il fusionne deux conversations de réflexion complémentaires :

- **WEB STACK :** les directions expertes (technique, cybersécurité, écoconception, accessibilité, AI Act, SEO), le diagnostic d'entrée, le travail sur des sites existants, la posture de conseil.
- **TOOL FACTORY :** l'architecture en quatre couches, le tronc d'étapes obligatoires, les packs de stack, la sécurité hors modèle. Son texte d'origine est dans `passation/PASSATION-tool-factory.md`. En cas de doute, le présent document fait foi.

Date de passation : 4 novembre 2026.

---

## 0. Consignes pour la conversation qui reprend

- Réponds à Arnaud en français, au tutoiement, selon le skill `style-google-devdocs`.
- Arnaud n'est pas développeur de formation. Explique simplement, sans jargon inutile. Il connaît l'image du restaurant : salle = front-end, cuisine = back-end, bâtiment = infrastructure.
- **Ne construis rien tant qu'Arnaud ne l'a pas décidé.** La phase en cours reste une phase de cadrage.
- Avance pas à pas : une brique validée avant la suivante.
- Recommande toujours une option principale, argumentée. Arnaud décide.
- Pour vérifier une affirmation ou une recherche, utilise le skill `agent-trust-ai`.

## 1. La vision en une phrase

**Vibe est un directeur de projet et un directeur technique virtuel.** Il commence par demander où en est le projet. Il lit ce qu'Arnaud lui fournit. Il propose la suite étape par étape, avec ses mises en garde. Un orchestrateur Python vérifie qu'aucune étape nécessaire n'est sautée.

Objectif d'Arnaud : se faciliter la vie, recevoir les bonnes solutions techniques et livrer des outils viables, sécurisés et évolutifs, en vibecoding.

« Vibe » est le nom de travail du framework. Il est personnalisé pour DAJM et construit au-dessus de BMAD.

## 2. Les trois briques

| Brique | Rôle | Image |
|---|---|---|
| BMAD | La méthode : de l'idée à la livraison, avec ses agents (analyste, PM, UX, architecte, développeur, QA) et sa structure epics et stories | La brigade qui sait cuisiner |
| Les directions DAJM | Ce qui manque à BMAD : direction technique, cybersécurité, écoconception, accessibilité, AI Act, SEO, conformité légale | Les chefs experts qui disent comment bien faire |
| L'orchestrateur Python | Repère quoi activer et quand, vérifie les gates, tient l'état du projet | Le chef de cuisine qui coche la liste avant chaque envoi |

### Posture : un directeur technique proactif

Vibe propose avant même que le travail commence :

- Dès la demande, il propose une direction : type de projet, stack, hébergement, risques, obligations légales probables.
- À chaque étape, il annonce la suite, ce qu'il recommande et pourquoi, avec les options écartées.
- Quand il repère un signal (paiement, chatbot, données personnelles), il alerte et propose les mesures avant le problème.
- Il travaille en mode pas à pas avec validation, ou en mode automatique sur un périmètre défini.

## 3. Comment une séance commence : le diagnostic d'entrée

Vibe ne suppose rien. Il pose d'abord trois questions :

1. Que veux-tu faire : créer un projet, améliorer un existant, ajouter une fonctionnalité, ou auditer ?
2. Où en est le projet ?
3. Quels éléments as-tu : comptes rendus, brief, accès, maquettes, code, URL ?

Il lit ensuite le corpus fourni, situe le projet dans le tronc (section 5) et propose un plan. Exemple de proposition attendue :

> « Le site est sous WordPress et tu as les comptes rendus. Je te propose de commencer par l'UX, puis l'UI, puis l'infrastructure. Attention : le site utilise tel builder, donc on procède ainsi. Telle extension n'est plus maintenue : je recommande de la remplacer. Ça te convient ? »

### Les quatre modes

| Mode | Cas d'usage | Parcours |
|---|---|---|
| Création | Nouvelle idée, nouvel outil, nouveau site | Tronc complet A à H, ou parcours allégé pour un petit outil |
| Amélioration d'un existant | Refonte, amélioration des parcours (cas Rigby) | Analyse de l'existant, puis tronc à partir de la phase utile |
| Fonctionnalité | Ajouter une fonction à un site en place | Analyse ciblée de l'existant, puis C3 à G sur un périmètre réduit |
| Audit | Contrôler un site ou un dépôt | Directions en mode contrôle, rapport, plan d'action |

### Règle d'entrée en cours de projet

Entrer à n'importe quel stade ne veut pas dire sauter des étapes. Quand Vibe démarre en cours de route, chaque phase amont reçoit l'un de ces statuts :

- **Reconstituée :** Vibe produit une version courte du livrable à partir de l'existant. Par exemple, un document d'architecture du site WordPress actuel.
- **Hors périmètre :** Arnaud déclare la phase non concernée, avec une justification écrite enregistrée dans l'état du projet.

Une gate ne s'ouvre jamais sur une phase sans statut. C'est ce qui réconcilie la souplesse demandée par WEB STACK et le tronc obligatoire de TOOL FACTORY.

## 4. Architecture : quatre couches

Chaque couche inférieure est plus déterministe que la précédente. Une étape obligatoire est vérifiée par du code dans les couches 2 à 4, jamais seulement par une consigne dans la couche 1.

```
┌─────────────────────────────────────────────────┐
│ 1. Méthode : BMAD + directions + satellites     │  ce qu'il faut faire
├─────────────────────────────────────────────────┤
│ 2. Orchestrateur Python (« factory »)           │  état du projet, gates,
│    machine à états, journal d'audit             │  ce qui est autorisé
├─────────────────────────────────────────────────┤
│ 3. Harness : Claude Code (hooks, permissions)   │  bloque l'action interdite
├─────────────────────────────────────────────────┤
│ 4. Plateforme : Git, CI, bac à sable            │  bloque la fusion, isole
└─────────────────────────────────────────────────┘
```

- **BMAD n'est pas un harness.** C'est une méthode empaquetée en agents, workflows et modèles. Rien dans BMAD n'empêche physiquement de sauter une étape.
- **L'orchestrateur reste modeste.** Il stocke l'état dans `factory/state.json` : mode, phase, statut de chaque phase, signaux détectés, directions actives, validations d'Arnaud. Il vérifie des critères de sortie objectifs : document présent, schéma respecté, CI verte, validation enregistrée.
- **Pas de harness maison.** Claude Code fait ce travail. Le SDK Claude Agent reste une option ultérieure.
- **Les directions ont deux faces.** En couche 1, elles conseillent et produisent des recommandations. En couches 2 à 4, leurs contrôles sont du code : scanners, tests, règles de CI.

## 5. Le tronc obligatoire, les directions et les satellites

```
TRONC OBLIGATOIRE                              SATELLITES ACTIVABLES
═════════════════                              ═════════════════════
0. DIAGNOSTIC D'ENTRÉE : mode, état, corpus, phases reconstituées
A. IDÉATION
   A1 Brainstorming guidé        ◄──────────── CIS : design thinking, innovation,
   A2 Recherche marché, domaine, technique      résolution de problèmes, storytelling
   A3 Pré-mortem : « pourquoi ça échouera » ◄── Veille concurrentielle, personas
   A4 Product brief                          ◄── Skills DAJM (strat, benchmark)
   ▼ GA · Arnaud : l'idée est-elle viable et pérenne ?
B. PLANIFICATION
   B1 PRD : exigences fonctionnelles + sécurité (OWASP ASVS)
   B2 Stratégie SEO : mots-clés, cocon sémantique, arborescence
   B3 Spécification UX et wireframes ◄────────── taste-skill (anti-slop), Figma
   ▼ GB · Chaque exigence a un critère testable
C. SOLUTION
   C1 Choix de la stack → active un pack ◄───── Packs de stack
   C2 Architecture + modèle de menaces (STRIDE)
   C3 Epics et stories (périmètre de fichiers)
   C4 Contrôle de préparation (readiness)
   ▼ GC · Arnaud : architecture et menaces validées
D. FONDATIONS : dépôt, CI, secrets, infra, DNS, e-mail ◄── Packs hébergeur
   ▼ GD · Arnaud : achats, clés, accès
E. CONSTRUCTION, story par story (boucle) ◄──── Test Architect (TEA)
   ▼ GE · CI bloquante + relecture d'Arnaud
F. RECETTE EN PRÉPRODUCTION (E2E, DAST, en-têtes, CSP, accessibilité,
   écoconception, performance, RGPD)
   ▼ GF · Checklist de mise en production
G. PRODUCTION ET EXPLOITATION
H. VEILLE CONTINUE ──────► nouvelle story en E
```

- La phase A boucle autant que nécessaire avant GA. Arnaud appelle cette étape le « malaxage » de l'idée. Le pré-mortem (A3) est une étape ajoutée à BMAD.
- **Parcours allégé** pour un outil d'une semaine : brief, architecture courte, stories. Le parcours complet sert aux SaaS et aux sites conséquents.
- B2 (stratégie SEO) est un ajout de WEB STACK, placé avant l'UX car l'arborescence en dépend.

### Les directions DAJM

Chaque direction intervient à plusieurs phases : elle conseille en amont et contrôle en aval.

| Direction | Conseille en | Contrôle en | Référentiels | Outils pressentis |
|---|---|---|---|---|
| Technique | A2, C1, C2 | C4, E | Bonnes pratiques par stack | Packs de stack |
| Cybersécurité | B1, C2 | E, F, H | OWASP ASVS, OWASP Top 10, OWASP Top 10 for LLM | Semgrep, gitleaks, OSV, ZAP, testssl.sh, Mozilla HTTP Observatory |
| Accessibilité | B3 | E, F | RGAA 4.1.2, WCAG 2.2, European Accessibility Act | axe-core, Pa11y, Lighthouse |
| Écoconception | B3, C1 | F | RGESN 2024, EcoIndex | `ecoindex-cli`, GreenIT-Analysis |
| Performance | C1 | F | Core Web Vitals | Lighthouse, PageSpeed Insights API |
| SEO | A2, B2 | F, G, H | Recommandations Google, schema.org | Search Console API, source de mots-clés à choisir |
| Conformité légale | A4, B1 | F | RGPD, CNIL, Code de la consommation | Détection des traceurs avant consentement (Playwright) |
| AI Act | A4, B1 | F | Règlement (UE) 2024/1689 | Questionnaire, détection des fonctions d'IA |
| Infrastructure | C1, D | D, G | SPF, DKIM, DMARC, DNSSEC, sauvegardes | `checkdmarc`, scripts validés par Arnaud |
| Qualité du code IA | C3 | E | Bonnes pratiques par langage | Linters, détection des paquets inventés (« slopsquatting ») |

Ce qu'aucun outil ne vérifie automatiquement (sauvegardes réellement restaurables, double authentification des comptes, contrats de maintenance) passe par un questionnaire guidé intégré à la phase concernée.

### Précisions sur deux directions

**AI Act.** Deux angles :

- Sites clients : un site avec chatbot ou contenu généré par IA relève de l'article 50 (informer l'utilisateur qu'il échange avec une IA, signaler certains contenus générés). Application à partir du 2 août 2026. Un site vitrine relève rarement du « haut risque », sauf cas particuliers (recrutement, crédit, éducation).
- Agence : l'article 4 (maîtrise de l'IA par les personnes qui l'utilisent) s'applique depuis le 2 février 2025.
- À vérifier : l'état de la proposition « Digital Omnibus » (novembre 2025), qui prévoyait de décaler certaines échéances.

**SEO.** Arnaud demande : analyse d'activité, recherche de mots-clés, mot-clé principal par page, cocon sémantique, suivi des positions, SEO technique, visibilité dans les IA (fichier `llms.txt`, contenus citables). Les volumes et les positions demandent une source de données : Search Console API (gratuite, données du site), DataForSEO (payante à l'usage), Semrush ou Ahrefs. Pas de scraping de Google.

### Activation par signaux

L'orchestrateur détecte des signaux dans le corpus, le brief ou le code et active les directions concernées :

| Signal | Activation |
|---|---|
| Formulaire avec données personnelles | Conformité légale, sécurité des formulaires, anti-spam |
| Paiement en ligne | Sécurité du paiement, webhooks signés, CGV, droit de la consommation |
| Chatbot ou fonction d'IA | AI Act, OWASP Top 10 for LLM, plafond de coût des API |
| E-commerce | Droit de la consommation, European Accessibility Act |
| Espace membre | Authentification, sessions, réinitialisation du mot de passe, RLS si Supabase |
| Upload de fichiers | Taille, type, stockage hors du dossier public |
| Site multilingue | SEO international (`hreflang`) |
| Refonte d'un site existant | Plan de redirections 301, conservation du référencement |

## 6. Packs de stack

Le cœur ne connaît aucune technologie. Le choix de stack (C1) ou la détection de l'existant (diagnostic) active un pack :

| Pack | Audit des dépendances | Analyse du code | Sources de veille |
|---|---|---|---|
| WordPress | WPScan | PHPCS (règles WordPress et sécurité) | Wordfence Intelligence, WPScan DB |
| Symfony | `composer audit` | PHPStan, Psalm | Symfony Security Advisories |
| Drupal | `composer audit` | `drupal-check` | Drupal Security Advisories |
| JavaScript | `npm audit`, OSV | ESLint + règles sécurité | GitHub Advisories |
| Python | `pip-audit` | Bandit, Semgrep | PyPI Advisories, OSV |

Outils transverses : Semgrep ou CodeQL, gitleaks, OSV, OWASP ZAP, Playwright, SBOM. Veille transverse : GitHub Advisories, OSV, CISA KEV, NVD.

### Exigence propre aux sites existants (pack WordPress en premier)

Un pack doit savoir **analyser un existant** avant de le modifier :

- Détecter l'éditeur (Gutenberg ou page builder : Elementor, Divi, WPBakery…), le thème (parent, enfant), les extensions et leurs versions, l'hébergement.
- Produire un état des lieux de départ : sécurité, accessibilité, performance, écoconception, SEO.
- Accéder au site par des moyens contrôlés : WP-CLI, API REST, SSH, selon ce que l'hébergement permet.
- **Règle stricte :** travailler sur une copie (staging) après une sauvegarde. Le site en ligne n'est modifié qu'après validation d'Arnaud.
- Respecter la façon dont le site est construit : produire des blocs Gutenberg si le site est en Gutenberg, des sections du builder si le site utilise un builder.

## 7. Sécurité hors modèle

| Mécanisme | Ce qu'il empêche | Couche |
|---|---|---|
| Hook de phase | Écrire du code tant que l'architecture n'est pas validée | Harness |
| Hook de périmètre | Modifier un fichier hors de la story en cours | Harness |
| Permissions `deny` sur `.claude/` et `factory/` | Laisser l'agent modifier ses propres garde-fous | Harness |
| Une branche par story | Mélanger deux stories dans un même changement | Git |
| Checks obligatoires | Fusionner si un scanner échoue | Plateforme |
| `CODEOWNERS` | Fusionner du code sensible sans relecture d'Arnaud | Plateforme |
| Bac à sable sans secrets | Exfiltrer une clé, même après une injection de prompt | Plateforme |

Règles :

- **Infrastructure et argent hors IA.** Achat de domaine, création de clés et paiements passent par des scripts qu'Arnaud valide. Les secrets n'entrent jamais dans le contexte d'un agent.
- Les scanners bloquent : une faille haute ou critique fait échouer la CI.
- La revue de l'agent QA est un commentaire, sans pouvoir de blocage. Un LLM qui relit un LLM partage ses angles morts.
- Toute exception exige une justification écrite dans `security/exceptions.md`.
- Les skills et dépendances tiers (dont taste-skill) sont lus avant adoption, et leur version est figée.

L'exemple de hook de périmètre en Python figure dans `passation/PASSATION-tool-factory.md`.

## 8. BMAD

### Décisions prises

- BMAD est la colonne vertébrale méthodologique. Sa structure epics et stories est jugée solide.
- Vibe se construit **au-dessus de BMAD sans le modifier**, avec une version figée. Les ajouts DAJM (directions, pré-mortem, stratégie SEO) vivent dans un module séparé, à créer avec BMad Builder si possible.

### Faits vérifiés (1ᵉʳ et 4 novembre 2026)

- Licence MIT.
- **v6.12 :** stable, gelée (corrections de bugs uniquement). Installation : `npx bmad-method install`. Prérequis : Node.js 20.12+ et Python 3.10+.
- **v7 :** alpha sur la branche `main`, sortie annoncée mi-novembre 2026. Livrée en skills, avec un plugin Claude Code (`/plugin marketplace add bmad-code-org/bmad-plugins`). Prérequis : `uv`.
- Modules : BMad Method, BMad Builder, Creative Intelligence Suite (CIS), Test Architect (TEA), BMad Loop (construit et vérifie un epic sans supervision), Game Dev Studio.
- À vérifier : les noms exacts des workflows d'analyse, et ce que la v7 propose pour les projets existants (« brownfield »).

Recommandation : prendre la v7 si elle est sortie en stable au lancement. Sinon, prototyper sur la v6.12 figée.

### Tâche : faire le point sur le BMAD local d'Arnaud

Arnaud possède une installation BMAD ancienne, qu'il a beaucoup modifiée. Avant toute décision de version, fais-en l'inventaire :

1. **Ne modifie rien dans ce dossier.** Lecture seule.
2. Identifie la version installée (manifeste d'installation, `package.json`, fichiers de configuration comme `core-config.yaml`).
3. Compare avec la version d'origine correspondante sur GitHub pour isoler les modifications d'Arnaud.
4. Classe chaque modification :
   - **À reprendre :** idée ou contenu utile à porter dans le module DAJM.
   - **Couvert par la version récente :** BMAD fait déjà mieux.
   - **À abandonner :** obsolète ou incompatible.
5. Signale tout secret, clé ou donnée client trouvé, sans en recopier la valeur.
6. Rends un fichier `bmad-local-audit.md` avec ce classement et une recommandation de migration.

## 9. Cas de test : Rigby

Le projet de refonte du site Rigby sert de premier cas réel :

- Site existant sous WordPress. Construction exacte (Gutenberg ou builder) à déterminer.
- Comptes rendus disponibles.
- Objectif : une passe d'amélioration du site et des parcours utilisateurs.
- Parcours attendu : diagnostic d'entrée, analyse de l'existant, phases amont reconstituées, puis UX, UI, infrastructure, construction sur staging, recette, mise en ligne.

Demande à Arnaud la conversation « Rigby refonte site » ou ses documents quand le cadrage de Vibe sera validé.

## 10. Acquis de la conversation WEB STACK

À garder en tête, sans y passer du temps :

- Un guide pédagogique « couches d'un site web » existe : `guides/couches-web.html` (dépôt `agencedajm/DAJM`, branche `claude/guide-couches-web`). Correction à faire : Node.js est un environnement d'exécution, pas un langage.
- La checklist sécurité d'Arnaud a été analysée. Ses manques sont repris dans les directions : injection SQL, XSS, CSRF, en-têtes, cookies, secrets déjà commités, surveillance, plan d'incident, gestion des accès de l'agence, SPF/DKIM/DMARC, accessibilité, RGPD opérationnel. Le mode debug est coupé en production. On ne désactive jamais un WAF pour faire passer du code généré.

## 11. Décisions ouvertes

Arnaud doit trancher ces points, dans l'ordre que tu lui proposeras :

1. Version de BMAD à figer, après l'audit du BMAD local.
2. Premiers packs de stack (suggestion : WordPress, pour Rigby, puis Symfony).
3. Satellites d'idéation activés par défaut.
4. Support des validations de gate : fichier signé, commentaire de PR ou interface.
5. Mode d'utilisation : ligne de commande seule, ou avec une interface simple.
6. Où tourne Vibe : ordinateur d'Arnaud (système ?), cloud, GitHub Actions.
7. Usage interne seulement, ou aussi prestation vendue aux clients.
8. Budget mensuel pour les API d'IA et les données SEO.
9. Outil de wireframes : Figma, HTML ou autre.

## 12. Prochaines étapes

1. Fais l'audit du BMAD local (section 8).
2. Vérifie l'état de BMAD v7 sur le [dépôt BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD).
3. Fais le point avec Arnaud sur les décisions ouvertes.
4. Propose l'arborescence du dépôt Vibe et le format de `factory/state.json`, incluant le mode, les statuts « reconstituée » et « hors périmètre », les signaux et les directions actives.
5. Décris le contrat d'une direction et celui d'un pack de stack : ce que chacun doit fournir pour être branché.
6. Attends la décision d'Arnaud avant d'écrire le moindre code.

## 13. Prompt de démarrage suggéré

> Je reprends la conception de Vibe, mon framework de création web basé sur BMAD. Lis `passation/PASSATION-vibe.md` en entier. Commence par l'audit de mon BMAD local, en lecture seule, comme décrit en section 8. Ne construis rien : rends-moi l'audit, puis fais le point sur les décisions ouvertes.

## Sources

- [Dépôt BMAD-METHOD sur GitHub](https://github.com/bmad-code-org/BMAD-METHOD)
- [Taste Skill: The Anti-Slop Frontend Skill Library](https://developertoolkit.ai/en/ecosystem/skills/taste-skill/)
- [Règlement (UE) 2024/1689 sur l'intelligence artificielle (EUR-Lex)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)
