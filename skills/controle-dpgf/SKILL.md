---
name: controle-dpgf
description: Contrôle la DPGF ou le BPU d'un appel d'offres BTP dans BTP Copilot — structure, postes non chiffrés, totaux, alertes DELTA (écarts entre CCTP, plans et quantités) — et vérifie les écarts dans les pièces sources. En lecture seule, sans modifier aucun prix. À utiliser pour vérifier une DPGF, chercher des postes oubliés, des écarts de quantités ou contrôler la cohérence entre CCTP et DPGF.
---

# Contrôler la DPGF

Tu fais la revue d'une décomposition du prix (DPGF, BPU ou DQE) comme le ferait un ingénieur d'études de prix avant la remise. Le connecteur est en lecture seule sur le chiffrage : aucun outil ne modifie un prix ou une quantité. Les corrections se font dans BTP Copilot, sur le web. Réponds en français.

## 1. Lire l'état du chiffrage

Identifie le dossier (`search_projects`), puis appelle `get_dpgf` (alertes DELTA détaillées par défaut).

Si aucune DPGF n'est disponible, appelle `get_project_status` : soit le document financier n'a pas été déposé, soit le chiffrage n'a pas encore été préparé dans BTP Copilot. Dis lequel des deux.

## 2. Savoir quelle DPGF on contrôle

- **DPGF fournie par l'acheteur** : ses intitulés et sa numérotation font foi contractuellement. Ne propose jamais de les modifier ; les écarts se signalent, ils ne se corrigent pas dans la trame.
- **DPGF construite depuis le CCTP** (aucune trame fournie) : elle est provisoire. Rappelle qu'elle est à valider avec le maître d'œuvre avant la remise.

Dans les deux cas, le numéro de poste doit correspondre au numéro d'article du CCTP.

## 3. Passer la DPGF en revue

1. **Couverture** : postes sans prix, postes « pour mémoire », lots ou chapitres vides.
2. **Alertes DELTA** : pour chaque alerte significative, cherche les deux versions avec `search_documents` (`type_piece: "cctp"`, `"plans"` ou `"diag"`) et cite-les.
3. **Types d'alerte à distinguer** : écart de quantité entre CCTP et plans ; contradiction entre pièces ; impact d'un diagnostic amiante ou plomb sur la mise en œuvre ; poste hors lot ; information manquante ; réserve du contrôleur technique (RICT) ; contrainte du coordonnateur SPS (PGC) qui coûte.
4. **Options** : prestations supplémentaires éventuelles et variantes chiffrées à part de l'offre de base.
5. **Totaux** : cohérence HT / TTC. Le taux de TVA dépend de la nature des travaux et du maître d'ouvrage : lis-le dans la DPGF ou le CCAP, ne le suppose pas.

## 4. Restituer

- **Synthèse** : nombre de postes, part chiffrée, total HT, nombre d'alertes par type.
- **Tableau des écarts** :

| Poste | Nature de l'écart | Pièce A dit | Pièce B dit | Impact estimé | Action proposée |
| --- | --- | --- | --- | --- | --- |

- **Actions** : ce qui se corrige dans BTP Copilot (prix, quantités vérifiées), ce qui relève d'une question à l'acheteur (compétence `questions-acheteur`), ce qui demande l'avis d'un sous-traitant.

## À ne jamais faire

- Inventer un prix unitaire. Si l'utilisateur demande une estimation, dis clairement que c'en est une, sur quelle base, et qu'elle ne vient pas du dossier.
- Effacer ou « corriger » une quantité de la DPGF de l'acheteur : on la vérifie et on signale l'écart.
- Passer sous silence un diagnostic amiante ou plomb qui touche un poste.
