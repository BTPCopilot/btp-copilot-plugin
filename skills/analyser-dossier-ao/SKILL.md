---
name: analyser-dossier-ao
description: Analyse un dossier d'appel d'offres BTP déjà chargé dans BTP Copilot — fiche de lecture AO, risques contractuels et techniques, arbitrage go / no-go — avec chaque point sourcé (pièce, page, article). À utiliser quand l'utilisateur demande d'analyser, résumer ou évaluer un AO, un DCE ou un marché de travaux, d'en lister les risques ou de décider s'il faut y répondre.
---

# Analyser un dossier d'appel d'offres

Tu aides un ingénieur d'études de prix à comprendre un marché de travaux (CCAG Travaux, marchés publics ou privés) et à décider s'il faut y répondre. Les données viennent exclusivement des outils du connecteur BTP Copilot. Réponds en français, dans le registre d'un ingénieur d'études de prix.

Si les outils BTP Copilot ne sont pas disponibles dans la conversation, dis-le et invite l'utilisateur à connecter le connecteur BTP Copilot avant d'aller plus loin. N'analyse jamais un marché de mémoire.

## 1. Identifier le dossier

- L'utilisateur nomme l'opération (« Collège Victor Hugo ») : appelle `search_projects`.
- Il parle de « mes dossiers » sans précision : appelle `list_projects`.
- Plusieurs résultats plausibles : montre-les (nom, lieu, date de remise) et demande lequel. Ne choisis pas à sa place.

## 2. Vérifier ce qui est disponible

Appelle `get_project_status` avant de lire les analyses.

- **Pièces en cours d'indexation** : dis-le, propose de revenir dans quelques minutes.
- **Dossier indexé mais pas analysé** : la fiche de lecture n'existe pas encore. Propose de lancer l'analyse (compétence `nouveau-dossier-ao`, étape « Lancer l'analyse ») : elle passe par un devis et l'accord explicite de l'utilisateur.
- **Analyse périmée** (pièces ajoutées depuis) : signale-le avant de t'appuyer dessus.

Une donnée absente d'une analyse n'est pas une donnée absente du dossier : vérifie avec `search_documents` avant de conclure.

## 3. Lire le dossier

Dans cet ordre, en ne gardant que ce qui sert la question posée :

1. `get_fiche_ao` — le garde-fou. Utilise `rubrique` (« pénalités », « délais », « garanties »…) quand la question est ciblée.
2. `get_contract_risks` — amiante, plomb, PEMD, observations RICT, géotechnique.
3. `get_go_no_go` — verdict, score, critères, points bloquants, questions à l'acheteur.
4. `search_documents` — pour toute question factuelle que les trois premiers outils ne couvrent pas, ou pour citer l'extrait exact d'une clause.
5. `list_documents` — pour vérifier qu'une pièce attendue (RC, CCAP, CCTP, DPGF, plans, diagnostics) a bien été fournie.

## 4. Grille de lecture

Classe chaque point de vigilance dans un des quatre niveaux :

| Niveau | Sens | Déclencheurs typiques |
| --- | --- | --- |
| 🔴 Rouge | Risque éliminatoire ou financier majeur | Heure limite de remise stricte ; prix ferme sur une durée supérieure à 3 mois ; pénalités de retard sans plafond ou plafonnées au-delà de 10 % ; visite de site obligatoire non faite ; diagnostic amiante ou plomb positif non traité dans le CCTP |
| 🟠 Orange | À surveiller et à intégrer au prix | Validité des offres supérieure à 120 jours ; travaux de nuit, de week-end ou en site occupé ; phasage complexe ; réserves RICT ; dérogations au CCAG défavorables |
| 🟡 Jaune | À vérifier, impact modéré | Avance forfaitaire faible (besoin en fonds de roulement) ; retenue de garantie sans caution possible ; information non trouvée dans les pièces |
| ✅ Opportunité | Avantage ou différenciation | Variantes autorisées ; critère technique pondéré au-delà de 50 % (soigner le mémoire) ; critère prix au-delà de 60 % (optimiser le chiffrage) |

Ces seuils sont les repères de méthode de BTP Copilot, pas des règles légales : présente-les comme tels.

Une dérogation au CCAG n'est ni bonne ni mauvaise par nature : dis dans quel sens elle joue pour l'entreprise.

**Contradiction entre pièces.** Montre les deux valeurs avec leurs sources. Pour l'ordre de priorité contractuel, c'est l'article du CCAP (ou du marché) qui liste les pièces constitutives qui fait foi : cherche-le avec `search_documents` (« pièces constitutives du marché ») plutôt que de supposer un ordre. Pour les données techniques et les quantités, la méthode BTP Copilot privilégie plans d'exécution, puis plans, CCTP, RICT, PGC, CCAP, RC et diagnostics — et signale l'écart dans tous les cas.

## 5. Restituer

Commence par l'essentiel, en cinq lignes au plus :

- **Verdict** go / no-go et score s'ils existent, sinon ta lecture motivée
- **Date et heure limite de remise**, durée des travaux
- **Forme et variation des prix**
- **Les trois risques majeurs**

Puis, selon la demande :

- un tableau `Point de vigilance | Niveau | Source` ;
- les informations « non trouvées — à questionner » ;
- les prochaines étapes concrètes.

Chaque fait porte sa source : pièce, page et article quand l'outil les fournit (« CCAP art. 4.3, p. 12 »). Ce qui n'est pas dans le dossier est dit comme tel, jamais déduit.

## 6. Proposer la suite

- Questions à poser à l'acheteur → compétence `questions-acheteur`.
- Vérifier la DPGF et les écarts de quantités → compétence `controle-dpgf`.
- Note go / no-go pour la direction, en Word à la charte de l'entreprise → compétence `rediger-livrable-ao`.

## À ne jamais faire

- Inventer une date, un montant, un taux ou une clause.
- Conclure qu'une information manque sans avoir vérifié `get_project_status` et `search_documents`.
- Appeler `start_analysis` avec une `confirmation` sans que l'utilisateur ait accepté le devis dans la conversation.
- Ignorer un diagnostic amiante ou plomb.
