# Programme 12 semaines — v2

> **Synchronisé avec l'app le 2026-09-08, commit `e6345ac`.**
> Ce fichier est régénéré directement depuis `index.html` (objets `EX`, `SESS`,
> `WEEKS`, `VOL_TARGETS` et logique de progression) — il n'est plus édité à la
> main. En cas de doute, `index.html` fait foi ; si ce fichier est plus vieux
> que le dernier commit du dépôt, il est probablement périmé.
>
> Changement de format par rapport aux versions précédentes : plus de bloc
> JSON dupliqué en fin de fichier — une seule représentation lisible, pour
> éviter que les deux dérivent l'une de l'autre comme ça a été le cas.

## Contexte

Objectif : recomposition, priorité hypertrophie haut du corps
(bras > pecs > épaules > dos), bas du corps athlétique uniquement, objectif
secondaire cardio (marche + cardio sans impact, plus de plan de course).
Structure : 4 séances muscu par groupes musculaires (B, A, C, V) + cours
collectif jeudi + 2 séances cardio (mardi soir, samedi), sauf semaines Bali
(S6-8) qui basculent sur un format allégé 2×/semaine.

## Règles globales

- RIR : 1 à 3 en réserve sur tous les mouvements à la barre (`barre:true`),
  jamais l'échec. RIR 0 sur un exercice à la barre déclenche une alerte
  visuelle dans la séance.
- Échec autorisé uniquement sur la dernière série des exercices d'isolation
  (`isolation:true`, badge « échec ok »).
- RIR saisi à chaque série.
- **Double progression, telle que codée (tâche 6)** : suggestion de montée de
  charge (+2,5 kg haut du corps / +5 kg bas du corps, incrément propre à
  chaque exercice via son champ `inc`) affichée seulement si **les deux
  dernières séances loggées** de l'exercice ont **toutes leurs séries au
  sommet de la fourchette de reps ET un RIR renseigné sur chaque série**.
  Retour au bas de la fourchette après la montée.
  - Si le RIR n'est pas renseigné sur une série d'une des deux séances : pas
    de suggestion, message « RIR manquant en S… ».
  - Si une seule des deux dernières séances est au sommet : pas de
    suggestion, message « 1 séance sur 2 validée (S…) — encore une au top
    avant la montée ».
  - Le bandeau « Quand monter les charges » de l'onglet Calendrier a été mis
    à jour dans la foulée (il disait encore « la séance suivante », une
    seule séance) pour refléter cette règle à deux séances.
- Superset : 30 s entre les deux exercices, le repos indiqué s'applique après
  le second.
- Repos par exercice : lu depuis `repos_s` (pas de valeur globale). Les
  mouvements composés ont tous ≥ 90 s.

## Calendrier — semaine type (hors bloc Bali)

| Jour | Créneau | Contenu | Lieu | Durée cible |
|---|---|---|---|---|
| Lundi | midi | Séance B — pecs, triceps, épaules | salle boulot | ~47 min |
| Mardi | midi | Séance A — dos, biceps | salle boulot | ~47 min |
| Mardi | soir | Marche 30-45 min | extérieur | — |
| Mercredi | soir | Séance C — ischios, rappel dos, bras, rappel pecs/épaules | maison | ~70 min |
| Jeudi | midi | Cours collectif (renfo ou cardio) | — | — |
| Vendredi | libre | Séance V — dos, épaules, pecs, jambes | maison | ~88 min |
| Samedi | libre | Cardio sans impact 20-25 min (vélo, elliptique ou marche en côte) — effort où je peux parler mais pas chanter | extérieur | — |
| Dimanche | — | Repos complet | — | — |

Le log cardio (`ST.runs`) accepte un type (marche / vélo / élliptique / côte),
une durée et une distance optionnelle, par créneau.

## Séance B — Lundi midi — Pecs, triceps, épaules

| # | Exercice | Séries × reps | Repos | Groupe | Notes |
|---|---|---|---|---|---|
| 1 | Développé couché barre | 4×6-10 | 180 s | Pecs | Barre — 1-3 RIR, jamais l'échec |
| 2 | Développé incliné haltères | 3×8-12 | 120 s | Pecs | |
| 3 | Élévations latérales penchées, haltères | 6×12-20 | 75 s | Deltoïde latéral | Isolation, échec ok dernière série |
| 4 | Extension triceps à la machine | 3×10-15 | 90 s | Triceps | Isolation, échec ok dernière série |

## Séance A — Mardi midi — Dos, biceps

| # | Exercice | Séries × reps | Repos | Groupe | Notes |
|---|---|---|---|---|---|
| 1 | Rowing barre buste penché | 4×6-10 | 180 s | Dos | Barre — 1-3 RIR, jamais l'échec |
| 2 | Tirage vertical à genoux à la poulie, prise large ou neutre | 3×8-12 | 120 s | Dos | |
| 3 | Curl incliné haltères | 3×8-12 (**4×8-12 en S9-12**) | 90 s | Biceps | Isolation, échec ok dernière série |
| 4a | Curl marteau | 3×10-12 | 30 s → 4b | Biceps | Superset avec 4b |
| 4b | Face pull | 3×15-20 | 60 s | Deltoïde postérieur | Superset ; si poulie loin des haltères : 4a puis 4b en séries normales, 60 s de repos |

## Séance C — Mercredi soir — Ischios, rappel dos, bras, rappel pecs/épaules

| # | Exercice | Séries × reps | Repos | Groupe | Notes |
|---|---|---|---|---|---|
| 1 | Tirage vertical supination serré | 3×8-12 | 120 s | Dos | |
| 2 | Extension triceps poulie basse, allongé sur banc | 4×10-15 (**5×10-15 en S9-12**) | 90 s | Triceps | Isolation, échec ok dernière série. Allongé sur un banc, poulie basse derrière la tête, corde ou barre, bras dans l'axe du corps, coudes fixes |
| 3 | Curl poulie basse, bras derrière le corps | 3×10-12 | 90 s | Biceps | Isolation, échec ok dernière série |
| 4 | Écarté incliné haltères | 3×10-15 (**4×10-15 en S9-12**) | 90 s | Pecs | Isolation, échec ok dernière série |
| 5 | Élévations latérales poulie, bras derrière le corps | 6×12-15 | 75 s | Deltoïde latéral | Isolation, échec ok dernière série |
| 6 | Soulevé de terre roumain | 3×8-10 | 150 s | Ischios | Barre — 2-3 RIR, charge modérée les 3 premières semaines. **Substitution conditionnelle** : Curl ischios poulie basse, sangle de cheville, 3×10-15, 150 s, si lombaires fatiguées (bouton sur la carte, historique séparé) |
| 7 | Crunch enroulé + gainage planche | 3×12-15 | libre | Abdos | 10 min, format libre |

## Séance V — Vendredi — Dos, épaules, pecs, jambes

| # | Exercice | Séries × reps | Repos | Groupe | Notes |
|---|---|---|---|---|---|
| 1 | Rowing haltère un bras, appui sur banc | 3×8-12 /côté | 150 s | Dos | |
| 2 | Développé militaire barre debout | 3×6-8 | 180 s | Épaules | Barre — 1-3 RIR, jamais l'échec |
| 3 | Développé haltères prise serrée | 3×8-12 | 150 s | Pecs | Compte aussi comme volume triceps « bonus » non ciblé (voir tableau de volume) |
| 4a | Pull-over poulie bras tendus | 3×12-15 | 30 s → 4b | Dos | Superset avec 4b |
| 4b | Oiseau haltères poitrine appuyée sur banc incliné | 3×12-15 | 75 s | Deltoïde postérieur | Superset ; isolation, échec ok dernière série |
| 5 | Presse à cuisses | 3×8-12 | 150 s | Quadriceps | Avant-dernière position d'exercice chargé |
| 6 | Marche du fermier | 3×40 m | 120 s | Full body | Haltères les plus lourds tenables |
| 7 | Crunch enroulé + gainage planche | 3×12-15 | libre | Abdos | 10 min, format libre |

## Semaines Bali (bloc 2, S6-8) — Séance H, full body entretien

2 séances par semaine, ~45 min, tout à 2 RIR. Cardio libre facultatif
(pas de plan structuré, texte libre par semaine).

| # | Exercice | Séries × reps | Groupe |
|---|---|---|---|
| 1 | Presse à cuisses ou goblet squat | 2×10 | Jambes |
| 2 | Développé haltères | 2×8-12 | Pecs |
| 3 | Rowing haltères | 2×8-12 | Dos |
| 4 | Tirage vertical | 2×8-12 | Dos |
| 5 | Curl | 2×12 | Biceps |
| 6 | Extension triceps | 2×12 | Triceps |

Texte cardio par semaine : S6 « Course libre bord de mer, 20-30 min »,
S7 « Course libre — vise le 5 km continu », S8 « Course libre » — ces trois
semaines ne sont pas concernées par le remplacement marche/cardio sans impact
(elles gardent une vraie course, contexte voyage).

## Volume direct hebdomadaire — cibles (hors semaines Bali et bonus S9-12)

| Groupe | Cible v2 |
|---|---|
| Dos | 16 |
| Pecs | 13 |
| Deltoïde latéral | 12 |
| Deltoïde postérieur | 6 |
| Biceps | 9 |
| Triceps | 7 (+ développé serré, suivi séparément, non compté dans la cible) |
| Ischios | 3 |
| Quadriceps | 3 |

Alerte si déviation de plus de ±20 % sur un groupe une fois toutes les
séances de la semaine marquées faites (comparaison indicative sinon).

## Périodisation

- **Semaines 1-5** : programme complet ci-dessus. Pas de semaine allégée
  (S5 est un point de contrôle « Test », pas un deload — le programme reste
  complet cette semaine-là). S5 sert aussi de référence de charges pour le
  pré-remplissage de S9.
- **Semaines 6-8 (Bali)** : bloc entretien ci-dessus, 2 séances/semaine,
  calories à l'entretien.
- **Semaine 9** : reprise du programme complet. Pré-remplissage automatique
  des charges à 90 % de la charge de série 1 de S5, arrondi au 2,5 kg le plus
  proche (uniquement si une charge a été loggée en S5 pour cet exercice).
- **Semaines 9-12** : +1 série sur curl incliné (séance A), extension
  triceps poulie basse allongé (séance C), écarté incliné (séance C). Dos et
  épaules inchangés. S12 est un point de contrôle « Bilan » (tests, photos,
  mesures), pas un deload.

## Signaux d'alerte et ordre de coupe

Déclencheurs détectés automatiquement : charges qui reculent sur 3 séances
consécutives d'un même exercice, ou mention de sommeil dégradé dans les
notes de la semaine (mots-clés : mauvais, peu, dégrad…, fatigu…,
insuffisant, pas assez, manque). L'app **propose, n'applique jamais
automatiquement**.

Ordre de coupe proposé : (1) écarté incliné → 2 séries, (2) oiseau → 2
séries, (3) développé couché → 3 séries.

Si le sommeil reste sous 7 h : réduire le déficit à 600-750 kcal plutôt que
couper davantage de volume.

## Exercices retirés (historique — conservés en lecture, plus proposés en saisie)

| Exercice | Groupe | Remarque |
|---|---|---|
| Élévations latérales haltères (debout) | Deltoïde latéral | Remplacé par « Élévations latérales penchées » (tâche 2) |
| Extension triceps poulie, corde, face à la poulie | Triceps | Remplacé par « Extension triceps poulie basse, allongé sur banc » (tâche 1) |
| Extension triceps au-dessus de la tête, haltère à 2 mains | Triceps | Retiré avant cette session |
| Extension triceps poulie au-dessus de la tête, corde, dos à la poulie | Triceps | Retiré avant cette session |
| Extension triceps poulie (version basique) | Triceps | Retiré (v1) |
| Curl poulie ou haltères | Biceps | Retiré (v1) |
| Développé militaire haltères assis | Épaules | Retiré (v1 → v2) |
| Fentes bulgares | Jambes | Retiré (v1 → v2) |
| Squat barre | Quadriceps | Retiré avant cette session |
| Rowing barre lourd | Dos | Retiré (v1) |
| Élévations latérales lourdes | Épaules | Retiré (v1) |
| Shrugs haltères | Trapèzes | Retiré (v1) |
| Dips | Pecs | Retiré avant cette session |
| Relevé de jambes / gainage | Abdos | Retiré avant cette session |
| Rowing haltères poitrine appuyée sur banc incliné | Dos | Retiré avant cette session |
| Développé militaire haltères (variante entretien) | Épaules | Retiré, non utilisé dans la séance H actuelle |
| Élévations latérales (variante entretien) | Épaules | Retiré, non utilisé dans la séance H actuelle |
| Développé couché haltères | Pecs | Retiré cette session — orphelin trouvé en régénérant ce fichier (présent dans le code, inutilisé dans toute séance actuelle) |
| Rowing horizontal poulie | Dos | Retiré cette session — même situation |
| Step-up sur banc, haltères | Jambes | Retiré cette session — même situation |

Je n'ai une certitude de filiation directe (« remplacé par ») que pour les
deux premières lignes (mes propres changements cette session, tâches 1 et
2) — pour les autres, retirées avant cette session, je liste le fait sans
garantir quel exercice actuel les a remplacées : je n'ai pas l'historique
git détaillé de ces décisions sous la main. Dis-moi si tu veux que je
vérifie via `git log`/`git blame`.
