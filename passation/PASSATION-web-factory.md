# Passation : compagnon de création web multi-agents (« Web Factory »)

Ce document sert de point de départ à une nouvelle conversation. Il résume une conversation précédente et définit le projet à construire. Lis-le en entier avant de proposer quoi que ce soit.

- **Auteur du besoin :** Arnaud, agence DAJM (`agence.dajm@gmail.com`).
- **Date de passation :** 4 novembre 2026.
- **Conversation source :** session Claude Code « couches web et sécurité » sur le dépôt `agencedajm/DAJM`.
- **Conversation complémentaire à intégrer :** « TOOL FACTORY » (voir la section 11).

---

## 1. Qui est l'utilisateur et comment lui répondre

- Arnaud dirige une agence (DAJM). Il crée des sites et des web apps pour des clients, en grande partie en vibecoding avec l'IA.
- Il connaît de nom WordPress, Drupal, Symfony, Bootstrap et JavaScript. Il apprend à les situer dans une architecture. Il n'est pas développeur de formation.
- **Style de réponse imposé :** applique le skill `style-google-devdocs` à chaque réponse conversationnelle. Tutoiement, réponse d'abord, phrases courtes, voix active, pas de point d'exclamation.
- **Pédagogie :** il demande des explications pour « une personne jeune qui ne comprend rien ». Utilise l'image du restaurant (voir la section 2) pour rester cohérent avec ce qu'il connaît déjà.
- Il dispose aussi du skill `agent-trust-ai` pour vérifier les contenus produits par l'IA. Utilise-le pour valider les recherches.

## 2. Ce qui a déjà été produit

### Le guide « couches d'un site web »

Un guide interactif explique les couches d'un site à un débutant, avec l'image d'un restaurant :

| Zone | Image | Contenu |
|---|---|---|
| Front-end | La salle | HTML (contenu), CSS et Bootstrap (apparence), JavaScript (réactions) |
| Back-end | La cuisine | Serveur web (le passe), langage (langue des recettes), framework (cuisine équipée sans chef), CMS (restaurant clé en main), base de données (chambre froide) |
| Infrastructure | Le bâtiment | Hébergeur, nom de domaine, DNS |

- **Fichier :** `guides/couches-web.html`, sur la branche `claude/guide-couches-web` du dépôt `agencedajm/DAJM`.
- **Version en ligne (privée) :** https://claude.ai/artifact/AVnsPJ6e1h8zAf77zd29Ki
- **Correction à apporter :** le guide écrit « JavaScript (Node.js) » parmi les langages. Node.js n'est pas un langage : c'est l'environnement qui exécute JavaScript sur un serveur.
- **Ajout prévu :** un chapitre « SaaS, logiciels et applis mobiles ».

### Points pédagogiques déjà expliqués

- SaaS = modèle commercial (web app louée), mêmes couches qu'un site. Logiciel installé = tout tourne sur l'ordinateur. Hybrides : Electron (Slack, Notion), applis mobiles avec API.
- C et C++ servent à fabriquer les outils eux-mêmes (navigateurs, Nginx, MySQL, PHP). C# est le langage de Microsoft (.NET, Unity).
- Node.js exécute JavaScript côté serveur. `npm`, Vite et Webpack en dépendent.

### Analyse de la checklist sécurité d'Arnaud

Arnaud a fourni une checklist de sécurité et la transcription d'une vidéo sur les « 8 notions » d'un site créé avec l'IA. L'analyse a conclu que ces deux sources couvrent la plupart des sujets, avec ces manques :

- **Attaques courantes :** injection SQL (requêtes préparées), XSS (échappement des sorties), CSRF (jetons), en-têtes de sécurité (CSP, HSTS, X-Frame-Options), cookies `Secure`, `HttpOnly`, `SameSite`.
- **Compléments :** rate limiting au-delà du login, liens de réinitialisation à usage unique, uploads hors du dossier public et non exécutables, fichiers oubliés sur le serveur (`.zip`, `phpinfo.php`, `.git`), moindre privilège, audit automatique des dépendances.
- **Secrets :** un `.env` déjà commité reste dans l'historique Git. Il faut changer toutes les clés concernées.
- **Surveillance :** disponibilité, journaux, plan d'incident.
- **Accès de l'agence :** gestionnaire de mots de passe, double authentification sur registraire, hébergeur, GitHub, Stripe, Google. Document de passation client.
- **E-mail :** SPF, DKIM, DMARC.
- **Qualité :** accessibilité (European Accessibility Act applicable depuis le 28 juin 2025 à une partie de l'e-commerce), environnement de recette (staging), tests multi-navigateurs, redirections 301.
- **RGPD opérationnel :** aucun traceur avant consentement, minimisation, durées de conservation, localisation des sous-traitants.
- **Corrections de la vidéo :** le mode debug doit être coupé en production. Ne jamais désactiver le pare-feu applicatif (WAF) pour faire passer du code généré par l'IA : corriger le code.

## 3. Le projet : ce qu'Arnaud demande

### Formulation d'Arnaud

> Ce n'est pas que de l'audit, ce doit être un compagnon de construction. Un client me demande un site web. Je te demande (potentiellement via la méthode BMAD) de construire les wireframes, les fonctionnalités, l'infrastructure, le code, le code pour WordPress ou un autre CMS ou framework, et sur toutes les lignes l'outil devra m'aider. Avec tous les agents nécessaires qui s'activeront en fonction de l'avancement du projet et des demandes repérées.

### Reformulation

Le projet est un **compagnon de création web multi-agents** qui accompagne un projet client de la demande initiale à la maintenance. Il a deux faces :

- **Produire :** brief, stratégie SEO, arborescence, wireframes, spécifications, choix techniques, infrastructure, code (CMS ou framework), contenus, documentation.
- **Contrôler :** à chaque étape, des contrôles de qualité (« gates ») vérifient la sécurité, l'accessibilité, l'écoconception, la performance, le SEO, la conformité légale et la qualité du code.

Un **orchestrateur Python** pilote le tout. Il garantit qu'aucune étape obligatoire n'est oubliée. Il active les agents selon la phase du projet et selon des signaux détectés dans le projet.

### Ordre de grandeur

Ce projet représente plusieurs mois de travail s'il est construit en entier. La stratégie retenue : construire un noyau solide, puis livrer un premier parcours complet sur **un seul type de projet** avant d'élargir.

## 4. Principes non négociables

1. **Python orchestre, l'IA ne décide pas seule.** Le projet suit une machine à états : chaque phase a des étapes obligatoires, et chaque étape finit avec un statut (réussie, échouée, non applicable, à valider). L'orchestrateur refuse de passer à la phase suivante si une étape obligatoire reste ouverte.
2. **Validation humaine aux jalons.** Arnaud valide chaque livrable clé (brief, arborescence, wireframes, choix technique, mise en ligne) avant la suite.
3. **Les outils reconnus mesurent d'abord.** Les mesures objectives viennent d'outils open source éprouvés. L'IA traite les questions de jugement, et le rapport indique ce qu'elle a jugé.
4. **Chaque constat a une preuve :** outil source, élément concerné, règle de référence (par exemple « RGAA 1.1 », « OWASP ASVS V3 »), gravité, niveau de confiance.
5. **Un référentiel unique** relie chaque contrôle à sa norme, à l'outil qui le vérifie, à sa gravité et à la phase où il s'applique. Les agents de production et les agents de contrôle lisent le même référentiel.
6. **L'outil dit ce qu'il ne sait pas vérifier.** Un pré-audit automatique ne remplace pas un audit RGAA humain ni un test d'intrusion.
7. **Cadre légal :** scans actifs uniquement sur des sites dont le propriétaire a donné une autorisation écrite (article 323-1 du Code pénal). Sur un site tiers, vérifications passives seulement. Pas de scraping des résultats Google : utiliser des API autorisées.
8. **Reprise sur incident :** l'état du projet est sauvegardé après chaque étape. Une coupure ne fait rien perdre.

## 5. Le cycle de vie d'un projet et les agents

Les agents s'activent par phase. Le nom des rôles s'inspire de la méthode BMAD (voir la section 7).

| Phase | Agents de production | Contrôles (gates) en sortie de phase |
|---|---|---|
| 1. Cadrage | Analyste (brief client, objectifs, cibles), Stratège SEO (analyse d'activité, concurrence, mots-clés), Conformité (obligations applicables) | Brief complet, objectifs mesurables, obligations légales listées |
| 2. Stratégie et contenu | Architecte de l'information (arborescence, cocon sémantique), Rédacteur (copywriting, appels à l'action) | Chaque page a un mot-clé principal et un objectif, pas de cannibalisation |
| 3. Conception | UX (parcours, wireframes), Designer UI (design system), Accessibilité dès la conception, Écoconception | Wireframes conformes aux critères RGAA de conception et au RGESN |
| 4. Choix techniques | Architecte technique (CMS ou framework, hébergement), Sécurité (modélisation des menaces) | Choix justifié, risques identifiés |
| 5. Infrastructure | DevOps (domaine, DNS, e-mail, hébergement, CI, sauvegardes) | SPF, DKIM, DMARC, HTTPS, sauvegardes testées, accès documentés |
| 6. Développement | Développeur par stack (WordPress, Drupal, Symfony, Next.js…), Revue de code | Pas de secret dans le code, dépendances saines, tests présents |
| 7. Recette | QA fonctionnelle, Auditeurs (sécurité, accessibilité, écoconception, performance, SEO technique, RGPD) | Scores minimaux atteints, anomalies bloquantes corrigées |
| 8. Mise en ligne | Checklist de mise en production | Debug coupé, redirections 301, Search Console, surveillance active |
| 9. Vie du site | Maintenance, suivi SEO (positions), ré-audits périodiques | Évolution des scores, alertes |

Agents transversaux :

- **Chef de projet :** suit l'état du projet avec l'orchestrateur et prévient Arnaud des décisions attendues.
- **Vérificateur de confiance :** contrôle les affirmations et les sources (skill `agent-trust-ai`).
- **Documentaliste :** produit la documentation et le dossier de passation client.

### Activation par signaux

En plus des phases, l'orchestrateur détecte des signaux dans le brief ou le code et active les agents concernés :

| Signal détecté | Agents ou contrôles activés |
|---|---|
| Formulaire avec données personnelles | RGPD, sécurité des formulaires, anti-spam |
| Paiement en ligne | Sécurité du paiement, webhooks signés, CGV, droit de la consommation |
| Chatbot ou fonction d'IA | AI Act (article 50), sécurité des LLM (injection de prompt, clé API, plafond de coût) |
| E-commerce | Droit de la consommation, European Accessibility Act, rétractation |
| Espace membre | Authentification, sessions, réinitialisation du mot de passe, RLS si Supabase |
| Upload de fichiers | Taille, type, stockage hors du dossier public |
| Site multilingue | SEO international (`hreflang`), traductions |
| Refonte d'un site existant | Plan de redirections 301, conservation du référencement |

## 6. Les axes de contrôle et leurs référentiels

Les versions et les outils sont à confirmer en phase de recherche.

| Axe | Référentiels | Outils pressentis |
|---|---|---|
| Cybersécurité | OWASP Top 10, OWASP ASVS | OWASP ZAP (scan passif), Semgrep, Gitleaks ou TruffleHog, OSV-Scanner, Trivy, testssl.sh, Mozilla HTTP Observatory |
| Accessibilité | RGAA 4.1.2, WCAG 2.2, European Accessibility Act | axe-core, Pa11y, Lighthouse |
| Écoconception | RGESN 2024, EcoIndex | `ecoindex-cli`, GreenIT-Analysis |
| Performance | Core Web Vitals | Lighthouse, PageSpeed Insights API |
| Qualité du code IA | Bonnes pratiques par langage | Linters, détection de duplication et de code mort, vérification que chaque paquet existe (« slopsquatting ») |
| RGPD et légal | RGPD, recommandations CNIL, Code de la consommation | Détection des cookies et traceurs avant consentement (navigateur Playwright), contrôle des pages légales |
| AI Act | Règlement (UE) 2024/1689 | Questionnaire et détection des fonctions d'IA |
| SEO | Recommandations Google, données structurées schema.org | Voir la section 6.1 |
| DNS et e-mail | Bonnes pratiques SPF, DKIM, DMARC, DNSSEC, CAA | `checkdmarc`, requêtes DNS |
| Sécurité des fonctions IA | OWASP Top 10 for LLM Applications | Tests de prompts, revue de code |

### 6.1 Module SEO

Arnaud demande un module SEO complet :

- **Analyse d'activité :** offre, cibles, zone géographique, concurrents.
- **Recherche de mots-clés :** volumes, intention de recherche, difficulté.
- **Mot-clé principal par page** et mots-clés secondaires.
- **Cocon sémantique :** pages mères, pages filles, maillage interne.
- **Suivi des positions** sur les mots-clés cibles.
- **SEO technique :** balises, données structurées, sitemap, `robots.txt`, vitesse.
- **Visibilité dans les IA (GEO) :** fichier `llms.txt`, contenus citables.

Les volumes et les positions demandent une source de données. Options à comparer en phase de recherche : Google Search Console API (gratuite, données du site seulement), DataForSEO (API payante à l'usage), Semrush ou Ahrefs (abonnement et API). Pas de scraping direct de Google.

### 6.2 AI Act : réponse à la question d'Arnaud

Oui, il faut l'intégrer, de deux façons :

- **Pour les sites clients :** quand un site intègre un chatbot ou du contenu généré par l'IA, l'article 50 impose d'informer les utilisateurs qu'ils échangent avec une IA, et de signaler certains contenus générés. Ces obligations s'appliquent à partir du 2 août 2026. Un site vitrine classique relève rarement des systèmes « à haut risque », sauf cas particuliers (recrutement, crédit, éducation).
- **Pour l'agence elle-même :** DAJM utilise des systèmes d'IA dans son travail. L'article 4 (maîtrise de l'IA par le personnel) s'applique depuis le 2 février 2025.

**À vérifier en recherche :** l'état de la proposition « Digital Omnibus » de la Commission européenne (novembre 2025), qui prévoyait de décaler certaines échéances.

## 7. Méthode BMAD

Arnaud mentionne la méthode BMAD. C'est un framework open source de développement assisté par IA, organisé en agents spécialisés (analyste, chef de produit, architecte, UX, scrum master, développeur, QA) et en documents successifs (brief, PRD, architecture, stories).

À décider en phase de cadrage :

- Utiliser BMAD tel quel comme moteur de production, et ajouter par-dessus l'orchestrateur Python et les gates de contrôle.
- Ou reprendre ses rôles et ses modèles de documents dans une architecture propre.

À vérifier : la version actuelle, la licence, le format des agents et leur compatibilité avec Claude Code.

## 8. Architecture technique proposée (à valider)

```
web-factory/
├── orchestrator/          # Machine à états Python : phases, étapes, statuts, reprise
├── referentiel/           # Règles : un fichier YAML par axe, sourcé
├── agents/                # Définitions des agents (prompts, skills Claude Code)
├── checks/                # Contrôles automatiques : un module Python par outil
├── templates/             # Modèles de livrables : brief, PRD, wireframes, rapport
├── stacks/                # Règles et gabarits par stack : wordpress/, drupal/, symfony/, nextjs/
├── projects/              # Un dossier par projet client, avec son état
└── reports/               # Rapports HTML et JSON
```

Le format commun d'un constat contient au minimum : identifiant de règle, axe, gravité, outil source, preuve, statut, confiance, recommandation.

## 9. Feuille de route

| Étape | Contenu | Livrable |
|---|---|---|
| 0. Cadrage | Réponses aux questions de la section 10, décision sur BMAD, fusion avec la conversation TOOL FACTORY | Périmètre validé |
| 1. Recherche | Agents en parallèle, un par axe (voir la section 9.1), puis vérification avec `agent-trust-ai` | Référentiel sourcé |
| 2. Noyau | Orchestrateur Python, format des constats, sauvegarde de l'état, rapport vide, tests | Un projet fictif traverse toutes les phases |
| 3. Premier parcours | Un type de projet de bout en bout, par exemple un site vitrine WordPress | Premier projet réel accompagné |
| 4. Contrôles | Un axe à la fois, chacun testé avant le suivant | Gates opérationnels |
| 5. Production assistée | Agents de production branchés sur les modèles et les stacks | Livrables générés et contrôlés |
| 6. Mode création | Règles, skills et hooks Claude Code issus du référentiel | L'IA code conforme dès la génération |
| 7. Industrialisation | GitHub Actions, suivi des scores, rapport client en marque DAJM | Outil vendable en prestation |

### 9.1 Agents de recherche à lancer en phase 1

Chaque agent rend un fichier de référentiel avec : liste des critères, source officielle de chaque critère, part automatisable, outils open source recommandés (licence, maintenance, installation), limites connues.

1. Cybersécurité web et OWASP (ASVS, Top 10, en-têtes, secrets, dépendances).
2. Accessibilité (RGAA, WCAG 2.2, European Accessibility Act, outils).
3. Écoconception (RGESN 2024, EcoIndex, outils).
4. Performance (Core Web Vitals, outils, seuils).
5. RGPD, CNIL, droit de la consommation, pages légales obligatoires.
6. AI Act (obligations par rôle, calendrier, Digital Omnibus).
7. SEO (méthode du cocon sémantique, sources de données de mots-clés et de positions, GEO).
8. Qualité et sécurité du code généré par l'IA (slopsquatting, revue de code, OWASP LLM Top 10).
9. Méthode BMAD (version, licence, intégration avec Claude Code et Python).
10. DNS, e-mail, hébergement, sauvegardes, surveillance.

## 10. Questions ouvertes pour Arnaud

1. **Stacks :** quelles technologies utilises-tu pour tes sites (WordPress, Drupal, Symfony, Next.js, Supabase, HTML simple) ? Laquelle en premier ?
2. **Usage :** usage interne seulement, ou aussi prestation vendue aux clients ?
3. **Exécution :** sur ton ordinateur (Mac ou Windows), dans le cloud ou dans GitHub Actions ?
4. **Dépôt :** créer un dépôt dédié, par exemple `agencedajm/web-factory`.
5. **Budget :** budget mensuel acceptable pour les API d'IA et les données SEO.
6. **BMAD :** l'utiliser tel quel ou s'en inspirer ?
7. **Wireframes :** dans quel outil (Figma, HTML, autre) ?

## 11. Conversation TOOL FACTORY

Arnaud ajoute une seconde conversation, nommée « TOOL FACTORY ». Son contenu n'était pas disponible au moment de cette passation.

**Première action de la nouvelle conversation :** lire la conversation TOOL FACTORY, puis lister les points communs, les différences et les décisions déjà prises. Fusionner ensuite les deux visions avant de lancer la phase 1.

## 12. Message de démarrage suggéré

> Lis le fichier `PASSATION-web-factory.md` et la conversation TOOL FACTORY. Fais-moi une synthèse des deux, signale les contradictions, puis pose-moi les questions de la section 10 qui restent sans réponse. Ne lance aucun agent de recherche avant ma validation.
