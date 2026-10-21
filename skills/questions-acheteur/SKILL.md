---
name: questions-acheteur
description: Prépare les questions à poser à l'acheteur ou au maître d'œuvre pendant la consultation d'un appel d'offres BTP — pièces manquantes, contradictions entre pièces, clauses ambiguës, périmètre de lot flou — chaque question rattachée à sa pièce et à sa page, à partir d'un dossier BTP Copilot. À utiliser pour une demande de précisions, des questions à l'acheteur ou au MOE, ou une liste de points à clarifier avant la remise.
---

# Préparer les questions à l'acheteur

Une bonne question à l'acheteur lève un risque de prix ou de conformité sans révéler la stratégie de l'entreprise. Une mauvaise question demande ce qui est déjà écrit dans le DCE. Tu prépares la première sorte, à partir du dossier BTP Copilot. Réponds en français.

## 1. Rassembler les points ouverts

Identifie le dossier (`search_projects`), puis collecte :

- `get_go_no_go` — sa section « questions à poser à l'acheteur » ;
- `get_fiche_ao` — les informations marquées non trouvées, les pièces manquantes, les dérogations au CCAG ;
- `get_contract_risks` — diagnostics incomplets, réserves RICT, alertes géotechniques ;
- `get_dpgf` — alertes DELTA (écarts entre CCTP, plans et quantités) ;
- `list_documents` — pièces attendues mais absentes (plans d'un lot, diagnostic, DPGF vierge, annexes citées).

## 2. Vérifier chaque point avant de le poser

Pour chaque candidat, interroge `search_documents` avec deux ou trois formulations. Si la réponse figure dans une pièce, retire la question et indique à l'utilisateur où se trouve la réponse. Si deux pièces se contredisent, cite les deux.

## 3. Trouver la date limite et le canal

Cherche dans le RC (`search_documents`, `type_piece: "admin"`, « renseignements complémentaires », « questions des candidats ») :

- la date limite d'envoi des questions ;
- le canal imposé, en général la plateforme de dématérialisation de l'acheteur.

Rappelle-les à l'utilisateur. Si le RC n'en dit rien, signale-le plutôt que de supposer un délai.

## 4. Rédiger

Règles de rédaction :

- une question par point, factuelle et neutre ;
- la référence précise : pièce, article, page (« CCTP lot 03, art. 3.2.1, p. 14 ») ;
- quand c'est utile, l'hypothèse retenue à défaut de réponse (« À défaut de précision, nous chiffrerons sur la base de… ») ;
- rien sur les prix de l'entreprise, ses sous-traitants ou sa stratégie : les réponses sont souvent publiées à tous les candidats.

Familles de questions à passer en revue :

- **Pièces** : pièce citée mais non fournie, version ou indice incohérent entre pièces.
- **Périmètre** : postes « hors lot » ou « à la charge du lot X » sans lot désigné, interfaces entre lots, prestations dues au compte prorata.
- **Quantités** : écart entre DPGF et plans, quantité absente d'un poste forfaitaire.
- **Conditions d'exécution** : accès, horaires, site occupé, phasage, installations de chantier, stockage.
- **Diagnostics** : amiante ou plomb non couverts sur une zone touchée par les travaux, repérage avant travaux absent.
- **Administratif et financier** : variantes et prestations supplémentaires éventuelles, indices de révision, avance, visite de site.

## 5. Restituer

Un tableau trié par priorité :

| N° | Question | Référence (pièce, article, page) | Enjeu (prix, délai, conformité) | Priorité |
| --- | --- | --- | --- | --- |

Puis la date limite d'envoi et le canal. Propose ensuite, au choix :

- de mettre les questions en forme de courrier et de l'exporter en Word à la charte de l'entreprise (compétence `rediger-livrable-ao`, type `courrier`) ;
- de les garder en checklist de suivi (type `checklist`).
