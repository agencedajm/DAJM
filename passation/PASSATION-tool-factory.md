# Passation : framework de la factory DAJM

Ce document transmet à une nouvelle conversation Claude le résultat d'un échange de réflexion. Il sert de point de départ pour concevoir deux choses :

- **Le framework de la factory** : une chaîne de production qui mène une idée jusqu'à un outil web, un SaaS ou un logiciel en production, avec des étapes obligatoires et une sécurité contrôlée à chaque étape.
- **Le compagnon de création web** : l'outil qui accompagne Arnaud pendant la création. Son périmètre détaillé vient de la conversation « WEB STACK », à fournir en complément de ce document.

Date de la passation : 4 novembre 2026.

## Consignes pour la conversation qui reprend

- Réponds à Arnaud en français, au tutoiement, selon le skill `style-google-devdocs`.
- Ne lance aucune construction tant qu'Arnaud ne l'a pas décidé. La phase en cours reste une phase de réflexion et de cadrage.
- Avance petit à petit : une brique validée avant la suivante.
- Avant de concevoir le compagnon, demande à Arnaud le contenu ou la passation de la conversation « WEB STACK ».

## Objectif

Construire une factory qui respecte les principes suivants :

1. **Agnostique sur la stack.** Le client choisit WordPress, Symfony, Drupal, Next.js ou toute autre technologie. GitHub et Cloudflare n'étaient que des exemples.
2. **Étapes obligatoires.** Personne, ni Arnaud par erreur ni l'agent, ne peut sauter une étape du tronc commun.
3. **Satellites activables.** Des outils optionnels aident à faire aboutir une idée viable, pérenne et efficiente.
4. **Sécurité contrôlée à chaque instant.** Chaque langage, outil et dépendance passe par des contrôles automatiques et une veille continue des failles.
5. **Prise en main facile, profondeur en cybersécurité.** L'interface reste simple. La couche sécurité reste extensible.
6. **Interfaces sans « AI slop ».** La production d'UI intègre des skills qui évitent le rendu générique des interfaces générées par IA.

## Décisions prises

Arnaud a validé les points suivants :

- **BMAD comme colonne vertébrale méthodologique.** Son architecture epics et stories est jugée bien construite.
- **Parcours allégé pour les petits outils.** Un outil d'une semaine suit une version courte : brief, architecture courte, stories. Le parcours complet sert aux SaaS.
- **Infrastructure et argent hors IA.** L'achat de domaine, la création de clés et les paiements passent par des scripts que Arnaud valide. Les secrets n'entrent jamais dans le contexte d'un agent.
- **La sécurité ne repose pas sur la discipline du modèle.** Chaque protection existe en dehors du modèle, quel que soit le modèle utilisé.
- **Construire au-dessus de BMAD sans le modifier.** La factory est une couche qui pilote BMAD, avec une version figée.

## Architecture retenue : quatre couches

Chaque couche inférieure est plus déterministe que la précédente. Une étape obligatoire doit être vérifiée par du code dans les couches 2 à 4, jamais seulement par une consigne dans la couche 1.

```
┌─────────────────────────────────────────────────┐
│ 1. Méthode : BMAD + satellites                  │  ce qu'il faut faire
├─────────────────────────────────────────────────┤
│ 2. Orchestrateur Python (« factory »)           │  état du projet, gates,
│    machine à états, journal d'audit             │  ce qui est autorisé
├─────────────────────────────────────────────────┤
│ 3. Harness : Claude Code (hooks, permissions)   │  bloque l'action interdite
├─────────────────────────────────────────────────┤
│ 4. Plateforme : Git, CI, bac à sable            │  bloque la fusion, isole
└─────────────────────────────────────────────────┘
```

Précisions issues de l'échange :

- **BMAD n'est pas un harness.** C'est une méthode empaquetée en agents, workflows et modèles de documents. Elle tourne dans un harness comme Claude Code, Codex ou Cursor. Rien dans BMAD n'empêche physiquement de sauter une étape.
- **L'orchestrateur Python reste modeste.** Il stocke l'état dans `factory/state.json`. Pour chaque phase, il vérifie des critères de sortie objectifs : document présent, schéma respecté, CI verte, validation d'Arnaud enregistrée. Une phase reste verrouillée tant que la gate précédente n'est pas franchie.
- **Pas de harness maison.** Claude Code fait ce travail. Le SDK Claude Agent reste une option ultérieure.

## Parcours : tronc obligatoire et satellites

```
TRONC OBLIGATOIRE                              SATELLITES ACTIVABLES
═════════════════                              ═════════════════════
A. IDÉATION
   A1 Brainstorming guidé        ◄──────────── CIS : design thinking, innovation,
   A2 Recherche marché, domaine, technique      résolution de problèmes, storytelling
   A3 Pré-mortem : « pourquoi ça échouera » ◄── Veille concurrentielle, personas
   A4 Product brief                          ◄── Skills DAJM (strat, benchmark)
   ▼ GA · Arnaud : l'idée est-elle viable et pérenne ?
B. PLANIFICATION
   B1 PRD : exigences fonctionnelles + sécurité (OWASP ASVS)
   B2 Spécification UX           ◄──────────── taste-skill (anti-slop), Figma
   ▼ GB · Chaque exigence a un critère testable
C. SOLUTION
   C1 Choix de la stack → active un pack ◄───── Packs de stack
   C2 Architecture + modèle de menaces (STRIDE)
   C3 Epics et stories (périmètre de fichiers)
   C4 Contrôle de préparation (readiness)
   ▼ GC · Arnaud : architecture et menaces validées
D. FONDATIONS : dépôt, CI, secrets, infra ◄──── Packs hébergeur
   ▼ GD · Arnaud : achats, clés, accès
E. CONSTRUCTION, story par story (boucle) ◄──── Test Architect (TEA)
   ▼ GE · CI bloquante + relecture d'Arnaud
F. RECETTE EN PRÉPRODUCTION (E2E, DAST, en-têtes, CSP)
   ▼ GF · Checklist de mise en production
G. PRODUCTION ET EXPLOITATION
H. VEILLE CONTINUE ──────► nouvelle story en E
```

La phase A boucle autant que nécessaire avant la gate GA. Arnaud appelle cette étape le « malaxage » de l'idée. Le pré-mortem (A3) est une étape obligatoire ajoutée à BMAD.

## Packs de stack

Le cœur de la factory ne connaît aucune technologie. Le choix de stack active un pack qui apporte ses outils de contrôle et ses sources de veille :

| Pack | Audit des dépendances | Analyse du code | Sources de veille |
|---|---|---|---|
| WordPress | WPScan | PHPCS (règles WordPress et sécurité) | Wordfence Intelligence, WPScan DB |
| Symfony | `composer audit` | PHPStan, Psalm (analyse de flux) | Symfony Security Advisories |
| Drupal | `composer audit` | `drupal-check` | Drupal Security Advisories |
| JavaScript | `npm audit`, OSV | ESLint + règles sécurité | GitHub Advisories |
| Python | `pip-audit` | Bandit, Semgrep | PyPI Advisories, OSV |

Les outils transverses à tous les packs sont les suivants : Semgrep ou CodeQL, gitleaks, OSV, OWASP ZAP, Playwright, génération d'un SBOM. Les sources de veille transverses sont GitHub Advisories, OSV, CISA KEV et NVD.

## Mécanismes de sécurité hors modèle

| Mécanisme | Ce qu'il empêche | Couche |
|---|---|---|
| Hook de phase | Écrire du code tant que l'architecture n'est pas validée | Harness |
| Hook de périmètre | Modifier un fichier hors de la story en cours | Harness |
| Permissions `deny` sur `.claude/` et `factory/` | Laisser l'agent modifier ses propres garde-fous | Harness |
| Une branche par story | Mélanger deux stories dans un même changement | Git |
| Checks obligatoires | Fusionner si un scanner échoue | Plateforme |
| `CODEOWNERS` | Fusionner du code sensible sans relecture d'Arnaud | Plateforme |
| Bac à sable sans secrets | Exfiltrer une clé, même après une injection de prompt | Plateforme |

Règles associées :

- Les scanners bloquent. Toute faille de sévérité haute ou critique fait échouer la CI.
- La revue de l'agent QA est un commentaire de PR, sans pouvoir de blocage. Un LLM qui relit un LLM partage ses angles morts.
- Toute exception exige une justification écrite dans `security/exceptions.md`.

Exemple de hook de périmètre (hook `PreToolUse` de Claude Code sur `Edit` et `Write`, le code de sortie `2` bloque l'action) :

```python
# .claude/hooks/scope_guard.py
import json, sys, fnmatch
from factory.state import current_story   # lit factory/state.json

event = json.load(sys.stdin)
path = event["tool_input"].get("file_path", "")
story = current_story()

if not any(fnmatch.fnmatch(path, p) for p in story.allowed_paths):
    print(f"Hors périmètre de la story {story.id} : {path}", file=sys.stderr)
    sys.exit(2)
```

## Risques identifiés

- **Isolation documentaire des stories.** Une story dit quels fichiers toucher, elle ne l'impose pas. Seuls les hooks et la CI rendent l'isolation réelle.
- **Débordement de l'agent.** Les modèles récents respectent mieux un périmètre que Sonnet 4.6, sans mesure chiffrée disponible. Le mauvais cas compte : story ambiguë, contexte long, injection de prompt.
- **Chaîne d'approvisionnement.** Les dépendances et les skills tiers (dont taste-skill) s'exécutent avec les droits de l'agent. Il faut les lire avant adoption et figer une version.

## Faits vérifiés le 1ᵉʳ novembre 2026

- **BMAD v6.12** : version stable, gelée (corrections de bugs uniquement). Installation : `npx bmad-method install`. Prérequis : Node.js 20.12+ et Python 3.10+.
- **BMAD v7** : préversion alpha sur la branche `main`, sortie annoncée mi-novembre 2026. Installation sous forme de skills (`npx skills add bmad-code-org/BMAD-METHOD --skill '*'`, puis `bmad setup`). Prérequis : `uv`. Le README décrit une boucle « Clarify, Plan, Build and verify, Learn and adjust ».
- **Modules BMAD** : BMad Method, BMad Builder, Creative Intelligence Suite (CIS), Test Architect (TEA), BMad Loop (construit et vérifie un epic entier sans supervision), Game Dev Studio.
- **taste-skill** : bibliothèque open source de skills anti-slop, dépôt `Leonxlnx/taste-skill`, skill principal `design-taste-frontend`. Il n'est pas installé dans l'environnement d'origine. Les chiffres de popularité varient selon les sources et restent non fiables.

À vérifier : les noms exacts des workflows d'analyse de BMAD (brainstorming, recherche, product brief). Le site `docs.bmad-method.org` était inaccessible depuis l'environnement d'origine.

Recommandation sur la version : au lancement, prendre la v7 si elle est sortie en version stable. Sinon, prototyper sur la v6.12 figée.

## Décisions ouvertes

Arnaud doit trancher les points suivants avant le lancement :

1. Les satellites d'idéation activés par défaut, et ceux qui restent optionnels.
2. Les premiers packs de stack à livrer (suggestion : WordPress et Symfony).
3. Le support des validations de gate : fichier signé, commentaire de PR ou interface dédiée.
4. Le mode d'utilisation de l'orchestrateur : ligne de commande seule, ou avec une interface pour la prise en main.
5. L'articulation entre la factory et le compagnon de création web (voir la conversation « WEB STACK »).
6. La version de BMAD à figer.

## Prochaines étapes proposées

Pour la conversation qui reprend, procède dans cet ordre :

1. Récupère auprès d'Arnaud le contenu de la conversation « WEB STACK ».
2. Fais le point avec lui sur les décisions ouvertes.
3. Vérifie l'état de BMAD v7 sur le [dépôt BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD).
4. Propose l'arborescence du dépôt de la factory et le format de `factory/state.json`.
5. Décris le contrat d'un pack de stack : ce qu'il doit fournir pour être branché.
6. Attends la décision d'Arnaud avant d'écrire le moindre code.

## Prompt de démarrage suggéré

> Je reprends un travail de réflexion sur une factory de création d'outils web sécurisés, basée sur BMAD. La passation est dans `passation/PASSATION-tool-factory.md`. Lis-la en entier, puis lis la passation de la conversation « WEB STACK » que je te fournis. Ne construis rien pour l'instant : fais-moi d'abord le point sur les décisions ouvertes et sur la façon d'articuler la factory et le compagnon de création web.

## Sources

- [Dépôt BMAD-METHOD sur GitHub](https://github.com/bmad-code-org/BMAD-METHOD)
- [Taste Skill: The Anti-Slop Frontend Skill Library](https://developertoolkit.ai/en/ecosystem/skills/taste-skill/)
- [Stop the Slop with Taste Skill for Better AI Design](https://www.abduzeedo.com/stop-slop-taste-skill-better-ai-design)
