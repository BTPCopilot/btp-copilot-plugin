---
name: rediger-livrable-ao
description: Rédige un livrable d'appel d'offres BTP à partir d'un dossier BTP Copilot — note de synthèse ou note go / no-go, synthèse de lot, fiche de consultation de sous-traitants ou fournisseurs, PPSPS préliminaire, mode opératoire, courrier, checklist de remise — puis l'enregistre versionné et l'exporte en Word à la charte de l'entreprise. À utiliser quand l'utilisateur demande de rédiger, préparer, mettre à jour ou exporter un de ces documents.
---

# Rédiger un livrable sourcé

Tu rédiges dans la conversation ; BTP Copilot conserve et met en forme. Le livrable est enregistré, versionné et relié à la version du DCE dont il découle, puis rendu en Word avec la charte de l'entreprise. Réponds et rédige en français.

**Hors de ce périmètre** : le mémoire technique sur trame et l'acte d'engagement sont produits par la plateforme BTP Copilot sur le web, parce que leur forme est contractuelle. Oriente l'utilisateur vers l'application pour ceux-là.

## 1. Choisir la nature du livrable

| Demande de l'utilisateur | `type` pour `save_document` |
| --- | --- |
| Note de synthèse, note go / no-go, note pour la direction travaux | `note` |
| Synthèse d'un lot | `synthese_lot` |
| Consultation de sous-traitants ou de fournisseurs | `fiche_consultation` |
| PPSPS préliminaire | `ppsps` |
| Mode opératoire, méthodologie d'exécution | `mode_operatoire` |
| Courrier (questions à l'acheteur, demande de précisions…) | `courrier` |
| Checklist de remise, liste de pièces | `checklist` |
| Autre document de travail | `autre` |

## 2. Rassembler les faits

Identifie le dossier (`search_projects`), puis lis seulement ce dont le document a besoin : `get_fiche_ao`, `get_contract_risks`, `get_go_no_go`, `get_dpgf`, et `search_documents` pour chaque clause ou prescription citée. Note au passage la pièce, la page et l'article de chaque fait : ils iront dans `sources`.

## 3. Rédiger et faire itérer

Rédige le document dans la conversation et laisse l'utilisateur le faire évoluer (longueur, ton, sections). Structures de départ :

- **Note go / no-go** : objet et contexte ; données clés (date limite, durée, forme de prix, critères de jugement) ; risques majeurs ; opportunités ; recommandation et conditions ; questions ouvertes.
- **Synthèse de lot** : périmètre ; prescriptions clés du CCTP ; interfaces avec les autres lots et postes hors lot ; quantités et alertes DELTA ; contraintes de chantier ; points à chiffrer avec prudence.
- **Fiche de consultation** : opération ; lot et périmètre consulté ; extraits du CCTP applicables ; quantités ; délais et planning ; exigences (normes, DTU, certifications cités dans les pièces) ; date et modalités de retour des offres. N'y mets ni les prix de l'entreprise ni des pièces que l'acheteur a déclarées confidentielles.
- **PPSPS préliminaire** : renseignements généraux ; mesures issues du PGC ; risques propres à l'opération (amiante, plomb, travail en hauteur, coactivité, site occupé) ; installations de chantier. Indique que c'est une base à compléter et à valider par l'entreprise avant toute transmission au coordonnateur SPS.
- **Mode opératoire** : phasage ; moyens humains et matériels ; sécurité ; contrôles et autocontrôles.
- **Courrier** : références de la consultation ; objet ; corps ; formule de politesse.
- **Checklist de remise** : établie à partir du RC du dossier (pièces de candidature et pièces de l'offre exigées, format, mode de dépôt), pas d'une liste générique.

Règles :

- Chaque fait vient du dossier et porte sa source dans le texte (« CCAP art. 4.3, p. 12 »).
- Ce qui relève de l'entreprise et n'est pas dans le dossier (moyens, références, organisation, noms) est marqué `[À COMPLÉTER]`, jamais inventé.
- Aucun chiffre sans source.

## 4. Enregistrer

Quand l'utilisateur valide le texte, appelle `save_document` :

- `type` et `titre` (« Note go / no-go — Gymnase Marseille ») ;
- `sections` dans l'ordre de lecture, chacune avec un `title` et un `content` en Markdown (paragraphes, listes, tableaux, sous-titres `##` et `###`) ;
- `sources` : chaque pièce citée, avec `doc`, `page`, `article` et un court `extrait` quand c'est utile.

Pour une nouvelle version d'un livrable existant, passe son `document_id` : l'ancienne version reste dans l'historique. Pour retrouver un livrable d'une conversation précédente, appelle `list_deliverables`.

## 5. Exporter en Word

Appelle `render_document` avec le `document_id`, puis donne le lien de téléchargement. Précise que :

- le document est mis en forme avec la charte de l'entreprise si sa trame est chargée dans BTP Copilot ;
- il porte une mention « document de travail rédigé par un assistant IA, à vérifier et valider » : c'est voulu, il doit être relu avant tout usage externe ;
- il est aussi disponible dans l'onglet Livrables du dossier sur BTP Copilot.
