# BRIEF — Refonte de l'application d'entraînement

Tu travailles sur `index.html`, une application web mono-fichier hébergée sur GitHub Pages
(dépôt **public** `BiggieSu0/programme`). C'est un carnet d'entraînement personnel sur 12 semaines.

Travaille **priorité par priorité**, en commitant après chaque bloc terminé. Montre-moi le diff
avant de pousser. Ne casse rien : l'app est utilisée quotidiennement depuis le 17 août 2026.

---

## Contexte technique actuel

- **Un seul fichier** `index.html` : HTML + CSS + JS vanilla, aucune dépendance externe
- **GIF d'exercices** : encodés en base64 directement dans le JS (objet `GIFS`), ~2 Mo au total
- **Données** : `localStorage`, clé `mp-raouf-v2`, structure
  `{logs:{}, done:{}, valid:{}, runs:{}, weight:[], notes:{}}`
  où `logs` a des clés de la forme `w<semaine>:<idExercice>` → `{kg, rir, sets:[bool]}`
- **4 onglets** : Calendrier · Séance · Progression · Bibliothèque
- Thème sombre, mobile-first, ajouté à l'écran d'accueil comme PWA

⚠️ **Le dépôt est public.** Aucune donnée personnelle (poids, mesures, photos) ne doit jamais
être committée. Toute sauvegarde distante doit viser un dépôt privé ou un Gist secret.

⚠️ **Migration obligatoire** : des données réelles existent déjà sous la clé `mp-raouf-v2`.
Toute évolution du schéma doit les migrer automatiquement, jamais les écraser.

---

## PRIORITÉ 1 — Corriger le contenu du programme

Le programme actuel dans le JS est à remplacer par la version ci-dessous. L'objectif de
l'utilisateur : **hypertrophie du haut du corps** (bras, pecs, épaules, dos) et **bas du corps
athlétique, zéro hypertrophie**. La version actuelle sur-sert le dos et les jambes.

### Séance A — Lundi midi (55 min) · Dos + biceps
| Exercice | Séries × reps |
|---|---|
| Rowing barre buste penché | 4 × 8-12 |
| Tirage vertical à genoux | 4 × 8-12 |
| Curl incliné haltères | 3 × 10-12 |
| Curl marteau | 3 × 10-12 |
| Face pull | 3 × 15 |

### Séance B — Mardi midi (55 min) · Pecs + épaules + triceps
| Exercice | Séries × reps |
|---|---|
| Développé couché barre | 4 × 6-10 |
| Développé incliné haltères | 3 × 10-12 |
| Élévations latérales | 4 × 12-20 |
| Extension triceps au-dessus de la tête | 3 × 10-12 |

*Contrainte forte : cette séance doit tenir en 55 min (créneau 12h-13h). Ne pas rallonger.*

### Séance C — Mercredi soir, maison (75 min) · Épaules, bras, rappel
| Exercice | Séries × reps |
|---|---|
| Développé militaire haltères assis | 3 × 8-12 |
| Élévations latérales poulie | 4 × 12-15 |
| Écarté incliné haltères ou poulie | 3 × 12-15 |
| Extension triceps poulie | 4 × 10-15 |
| Curl poulie ou haltères | 3 × 10-12 |
| Tirage vertical supination serré | 3 × 8-12 |
| Fentes bulgares | 3 × 8-10 /jambe |
| + 10 min abdos | |

### Séance V — Vendredi, maison (90 min) · Séance longue
| Exercice | Séries × reps |
|---|---|
| Rowing barre lourd | 3 × 6-10 |
| Développé militaire barre debout | 3 × 6-8 |
| Dips | 3 × 8-12 |
| Pull-over poulie bras tendus | 3 × 12-15 |
| Élévations latérales lourdes | 4 × 10-12 |
| Shrugs haltères | 3 × 10-12 |
| Squat barre au rack | 3 × 5-8 |
| Marche du fermier | 3 × 40 m |
| + 10 min abdos | |

**Le squat doit rester en avant-dernière position**, pas en début de séance : c'est l'exercice le
plus coûteux en fatigue et il ne doit pas dégrader le travail du haut du corps qui le précède.

### Exercices à retirer du programme
Soulevé de terre roumain · Step-up sur banc · Développé couché haltères · Rowing horizontal poulie
(les garder dans la bibliothèque, mais plus dans les séances)

### Courses : passer de 3 à 2 par semaine
Mardi soir + samedi uniquement. Adapter les libellés dans le calendrier.

---

## PRIORITÉ 2 — Persistance réelle (fin du localStorage seul)

Remplacer le stockage local par une vraie base, avec le localStorage conservé en cache hors ligne.

- **Option recommandée** : Supabase (gratuit, Postgres + auth). Table `entries` simple,
  auth par magic link ou compte unique.
- **Alternative** : Cloudflare Worker + D1.
- Les clés d'API publiques peuvent figurer dans le code (clé `anon` Supabase prévue pour ça),
  **mais aucune donnée personnelle ne doit être committée.**
- Comportement attendu : l'app marche hors ligne, se synchronise dès qu'il y a du réseau,
  et retrouve les données sur un nouvel appareil après connexion.

## PRIORITÉ 3 — Sauvegarde automatique vers Git (dépôt PRIVÉ)

- Export JSON poussé automatiquement une fois par semaine vers un **Gist secret** ou un
  **second dépôt privé**, via l'API GitHub avec un token à portée minimale (`gist` uniquement).
- Le token est saisi par l'utilisateur dans l'app et stocké localement — jamais en dur dans le code,
  jamais committé.
- Garder l'export/import JSON manuel existant comme filet de sécurité.

## PRIORITÉ 4 — Saisie par série (régression à corriger)

Aujourd'hui, un seul champ « charge » par exercice. Rétablir **kg / reps / RIR pour chaque série**,
comme dans la première version. Nécessaire pour repérer la série qui lâche et calculer le tonnage.
Migrer les données existantes : la charge unique actuelle devient la valeur de toutes les séries.

## PRIORITÉ 5 — Timer de repos

Déclenché automatiquement à la validation d'une série. Durées par défaut :
- Mouvements composés à la barre : 2 min 30
- Autres composés : 2 min
- Isolation : 60-90 s

Affichage en overlay avec bouton « Passer », son ou vibration en fin de repos.

## PRIORITÉ 6 — Compteur de volume hebdomadaire par groupe musculaire

Calcul automatique des séries effectuées par muscle sur la semaine, avec comparaison aux cibles :

| Muscle | Cible séries/semaine |
|---|---|
| Dos | 16-17 |
| Pecs | 14 |
| Deltoïde latéral | 12-13 |
| Triceps | 10-11 |
| Biceps | 9-10 |
| Deltoïde postérieur | 4-5 |
| Jambes | 7-9 |

Affichage sous forme de barres dans l'onglet Progression, avec alerte visuelle si un groupe
sort de sa fourchette. Chaque exercice porte déjà un champ `grp` exploitable.

## PRIORITÉ 7 — Historique et courbe par exercice

Vue par exercice : évolution de la charge sur les 12 semaines, 1RM estimé (formule d'Epley :
`1RM = kg × (1 + reps/30)`), et tonnage total par séance.

## PRIORITÉ 8 — Substitutions en un tap

Chaque exercice propose ses équivalences (salle du boulot ↔ maison, machine occupée).
La substitution choisie est enregistrée pour la séance sans casser l'historique de l'exercice.

## PRIORITÉ 9 — PWA complète et hors ligne

`manifest.json` + service worker : icône propre, plein écran, fonctionnement **hors ligne total**
(l'app doit marcher sans réseau à la salle et pendant un voyage à l'étranger en septembre).
Attention au cache des ~2 Mo de GIF.

## PRIORITÉ 10 — Détection de stagnation

Si un exercice ne progresse pas sur 2 semaines consécutives avec un RIR ≤ 1, l'afficher
dans Progression avec une suggestion : baisser la charge de 10 % et remonter, ou vérifier
récupération et sommeil.

---

## Règles de progression déjà en place (à conserver)

- Arrêt à **1-3 reps de l'échec** sur les mouvements à la barre — jamais l'échec
- **Échec autorisé sur la dernière série** des exercices d'isolation uniquement
- **Double progression** : toutes les séries en haut de la fourchette → +2,5 kg (haut du corps)
  ou +5 kg (bas du corps), puis retour au bas de la fourchette
- Semaine 5 : allégée, −1 série par exercice
- Semaines 6-8 : bloc voyage, séances full body d'entretien
- Bloc 3 (S9-12) : +1 série sur dos et épaules

## Ce qu'il ne faut pas toucher

- Le thème sombre et la structure des 4 onglets
- L'export texte « Bilan pour Claude » (format à conserver tel quel)
- Les GIF embarqués en base64
- Aucun onglet nutrition : géré ailleurs
