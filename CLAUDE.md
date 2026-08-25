# CLAUDE.md — Mémoire du projet « programme »

## Ce qu'est ce projet

Carnet d'entraînement personnel (12 semaines, recomposition corporelle), utilisé
quotidiennement sur téléphone à la salle de sport, parfois sur PC. Un seul utilisateur.
**Des données réelles sont en production depuis le 17 août 2026** : ne jamais casser ni
écraser l'existant. Interface entièrement en français.

## Sources de vérité

- `PROGRAMME-V2.md` → le programme d'entraînement (séances, reps, repos, règles,
  périodisation). **Le JSON en fin de fichier fait foi.** Ne jamais modifier le contenu du
  programme de sa propre initiative : c'est une décision de coaching, pas de code.
- `BRIEF.md` → la liste des tâches en cours, par priorité.
- Ce fichier → l'architecture et les conventions.

## Architecture

- **Un seul fichier applicatif : `index.html`** — HTML + CSS + JS vanilla, aucun framework,
  aucun build. Garder ce modèle sauf décision explicite contraire.
- Hébergement : **GitHub Pages**, dépôt **PUBLIC** `BiggieSu0/programme`, branche `main`.
- **Supabase** : auth par lien magique ; table `state` (une ligne par utilisateur,
  colonne `data` = blob JSON de l'état complet `ST`) ; écriture debounce 1,5 s après la
  dernière action. Sans connexion, tout vit dans `localStorage` uniquement.
- État local : `localStorage`, structure
  `{logs:{"w<semaine>:<idExo>": {...}}, done:{}, valid:{}, runs:{}, weight:[], notes:{}, subs:{}}`.
  - `subs` : substitutions d'exercice actives par semaine, ex. `{"w3:rdl":true}` — active le
    remplacement défini dans `EX[id].remplacement` pour cette semaine-là. Chaque id garde son
    propre historique dans `logs` (aucun log partagé entre un exercice et son remplaçant).
  - Une entrée de `logs` a la forme `{sets:[{d,kg,reps,r}, ...]}` — par série : `d` faite,
    `kg` charge, `reps` répétitions, `r` = RIR (`"0"|"1"|"2"|"3+"|null`). Aucun champ `kg`/`rir`
    global depuis la tâche « poids/reps par série » : `repKg()`/`repReps()` (première série
    faite) et `lastRir()` (dernière série faite) donnent les valeurs représentatives utilisées
    par les vues agrégées (table de charges, export, alertes).
  - Trois générations de format coexistent en lecture, toutes normalisées par `normSets()` —
    ne jamais réécrire l'historique en masse, seule l'édition d'un log précis le fait
    basculer au format courant :
    1. `{kg, rir, sets:[bool, ...]}` (avant RIR par série) — un seul poids et un seul RIR pour
       toute la séance, le RIR réinjecté sur la dernière série cochée ;
    2. `{kg, sets:[{d,r}, ...]}` (RIR par série, avant poids/reps par série) — un seul poids
       pour toute la séance, RIR par série ;
    3. `{sets:[{d,kg,reps,r}, ...]}` (actuel) — tout par série.
  - **Pré-remplissage anti-friction** : à l'affichage, une série sans `kg`/`reps` explicite
    hérite en direct de la série 1 (ou de la série 1 hérite du pré-remplissage 90 % de S5 en
    semaine 9, cf. `prefillKg()`) — recalculé à chaque rendu, jamais écrit en storage tant que
    l'utilisateur n'a pas modifié cette série précise. Une fois modifiée, une série garde sa
    propre valeur même si la série 1 change ensuite.
- GIF d'exercices : **base64 embarqués dans le JS (~2 Mo)**. Ne pas les dupliquer, ne pas
  les recompresser sans demande, attention à la taille du fichier.

## Règles de sécurité (non négociables)

- Le dépôt est **public** : jamais de secret, de token, de clé `service_role`, ni de donnée
  personnelle (poids, mesures, notes, exports JSON) dans un commit.
- La clé `anon` Supabase peut être dans le code **uniquement** parce que le RLS restreint
  chaque ligne à son propriétaire (`auth.uid() = user_id`). Ne jamais élargir les policies.
- Toute nouvelle table → RLS activé + policies par utilisateur avant la première écriture.

## Règles métier à faire respecter par l'app (jamais à modifier)

- RIR 1-3 sur les mouvements `barre: true` — l'échec (RIR 0) y déclenche une alerte.
- Échec autorisé uniquement sur la dernière série des exercices d'isolation.
- Double progression : toutes les séries au haut de la fourchette → +2,5 kg (haut du corps)
  / +5 kg (bas), retour au bas de la fourchette.
- Supersets : 30 s entre les deux exercices, repos indiqué après le second.
- Repos par exercice : lire `repos_s` du programme, pas de valeur globale.

## Pièges connus

- **Synchro = dernier écrit gagne** (blob unique + debounce) : risque d'écrasement entre
  téléphone et PC tant que la gestion de conflits (BRIEF, chantier 2) n'est pas faite.
- Réseau instable à la salle : toute écriture doit survivre à une coupure (file d'attente).
- Migration : les logs historiques référencent d'anciens id d'exercices — les conserver en
  lecture même quand un exercice sort du programme.
- Le format de l'export texte « Bilan pour Claude » est consommé tel quel dans une autre
  conversation : ne jamais en changer la structure.
- iOS/Safari : l'app est utilisée en PWA depuis l'écran d'accueil — tester les
  changements de stockage dans ce contexte.

## Interdits

- Pas d'onglet nutrition (géré ailleurs).
- Ne pas changer le thème sombre ni la structure des 4 onglets
  (Calendrier · Séance · Progression · Bibliothèque) sans demande explicite.
- Aucune image d'exercice scrapée d'un site tiers ; uniquement des sources dont la licence
  l'autorise, sinon carte texte.

## Méthode de travail

1. Une tâche à la fois, dans l'ordre du `BRIEF.md`.
2. Annoncer le plan avant de coder ; montrer le diff avant de committer.
3. Avant tout push : ouvrir l'app, vérifier que les données existantes s'affichent, que la
   saisie fonctionne et que la synchro Supabase répond.
4. Commits atomiques, messages clairs, un commit par tâche.
5. Si l'architecture évolue, mettre ce fichier à jour dans le même commit.
