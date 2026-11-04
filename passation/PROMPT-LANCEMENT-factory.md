# Prompt de lancement : framework Factory DAJM

Copie le bloc suivant comme premier message de la nouvelle conversation. Les deux fichiers de passation doivent se trouver dans le dossier `passation/` du dépôt ouvert.

---

```text
Je lance la conception du framework Factory de l'agence DAJM. Deux conversations précédentes ont préparé le terrain. Leurs passations sont dans le dépôt :

- passation/PASSATION-tool-factory.md : conversation « TOOL FACTORY ». Chaîne de production générique (outil web, SaaS, logiciel), méthode BMAD, étapes obligatoires, sécurité imposée hors du modèle (hooks, permissions, CI), satellites activables, packs de stack, anti-slop pour l'UI.
- passation/PASSATION-web-factory.md : conversation « WEB STACK ». Compagnon de création web multi-agents pour les projets clients, cycle de vie en 9 phases, activation des agents par signaux, référentiel unique de contrôles (sécurité, RGAA, RGESN, performance, SEO, RGPD, AI Act), constats prouvés.

Lis les deux fichiers en entier avant de répondre.

## Comment me répondre

- Applique le skill style-google-devdocs : français, tutoiement, réponse d'abord, phrases courtes, voix active.
- Je ne suis pas développeur de formation. Explique les notions techniques quand elles apparaissent, en restant cohérent avec l'image du restaurant du guide des couches web.
- Valide les recherches avec le skill agent-trust-ai.

## Ce que je veux construire

Un seul framework, en deux niveaux :

1. Le socle Factory : l'orchestrateur Python (machine à états, gates, journal, reprise), l'intégration à Claude Code (hooks, permissions), la CI, les packs de stack et la veille de sécurité. Le socle vaut pour tout type de projet : site, SaaS, logiciel.
2. Le compagnon de création web : une configuration du socle dédiée aux projets clients web, avec ses agents, ses signaux et son référentiel de contrôles.

Le socle impose les étapes obligatoires. Les satellites (outils d'idéation, CIS, taste-skill, Figma, modules SEO…) s'activent à la demande ou par signal détecté.

## Ce qui est déjà décidé

- Python orchestre, l'IA ne décide pas seule. Aucune phase ne s'ouvre tant qu'une étape obligatoire reste ouverte.
- Je valide chaque jalon clé. L'achat de domaine, les clés et les paiements restent hors IA, et les secrets n'entrent jamais dans le contexte d'un agent.
- La sécurité ne repose pas sur la discipline du modèle. Les protections vivent dans les hooks, les permissions, Git et la CI.
- Les outils reconnus mesurent. L'IA juge, et le rapport le dit. Chaque constat porte une preuve.
- La factory reste agnostique sur la stack grâce aux packs.
- Parcours allégé pour les petits projets, parcours complet pour les SaaS.
- Scans actifs uniquement avec autorisation écrite du propriétaire du site.

## Points à réconcilier entre les deux passations

Traite ces écarts en priorité. Pour chacun, propose une solution et attends ma validation :

1. Modèle de phases : TOOL FACTORY propose A à H, avec une idéation obligatoire (brainstorming, recherche, pré-mortem, brief). WEB STACK propose 1 à 9, avec SEO, contenu et vie du site. Je veux un modèle unique qui garde l'idéation et le pré-mortem.
2. Activation des agents : satellites activables (TOOL FACTORY) et activation par signaux (WEB STACK). Propose un mécanisme commun.
3. Deux sens du mot « sécurité » : la sécurité du processus de construction (l'agent ne peut pas déborder, ni toucher à ses garde-fous) et la qualité du produit livré (audits, référentiel). Les deux doivent coexister dans l'architecture.
4. BMAD : TOOL FACTORY considère comme décidé de construire au-dessus de BMAD sans le modifier, avec une version figée. WEB STACK laisse la question ouverte. Confirme-moi l'option et ses conséquences.
5. Arborescence : fusionne la structure web-factory/ (orchestrator, referentiel, agents, checks, templates, stacks, projects, reports) avec les éléments de TOOL FACTORY (factory/state.json, .claude/hooks, security/, CODEOWNERS).
6. Feuilles de route : aligne les prochaines étapes de TOOL FACTORY sur la feuille de route en 7 étapes de WEB STACK.

## Faits à revérifier

- État de BMAD : la v6.12 était stable et gelée, la v7 était annoncée pour mi-novembre 2026. Vérifie la situation sur le dépôt bmad-code-org/BMAD-METHOD, ainsi que la licence et le format des agents.
- Noms exacts des workflows d'analyse et de brainstorming de BMAD, ainsi que du module CIS.
- Le skill taste-skill (dépôt Leonxlnx/taste-skill) : contenu, licence, version à figer. Lis-le en entier avant toute adoption.
- L'état de la proposition « Digital Omnibus » sur le calendrier de l'AI Act.

## Ce que j'attends de cette première réponse

1. Une synthèse des deux passations, en une page maximum.
2. Les contradictions et les doublons, avec ta proposition pour chacun.
3. Un schéma unique du parcours : tronc obligatoire, gates, satellites, signaux.
4. Les questions ouvertes des deux passations qui restent sans réponse, regroupées et dédoublonnées.

Ne lance aucun agent de recherche et n'écris aucun code avant ma validation. On avance brique par brique.
```
