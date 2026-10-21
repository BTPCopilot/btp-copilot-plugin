# BTP Copilot pour Claude

![BTP Copilot](./assets/icon.png)

**BTP Copilot** est l'assistant des entreprises du bâtiment qui répondent aux appels d'offres de travaux (marchés publics et privés, CCAG Travaux). Ce plugin relie Claude à vos dossiers BTP Copilot et lui apprend les méthodes d'un ingénieur d'études de prix : lire un DCE, arbitrer un go / no-go, contrôler une DPGF, préparer les questions à l'acheteur et rédiger des livrables sourcés, exportés en Word à la charte de votre entreprise.

Chaque réponse s'appuie sur les pièces réelles du dossier, avec la pièce, la page et l'article cités. Ce qui ne figure pas dans le dossier est signalé comme tel, jamais deviné.

> **In English.** BTP Copilot helps French construction companies answer public and private works tenders (CCAG Travaux). This plugin connects Claude to the user's BTP Copilot workspace and adds five French-language skills for tender analysis, go / no-go decisions, bill-of-quantities (DPGF) review, clarification questions to the buyer, and sourced deliverables exported to Word. It requires a BTP Copilot account. It runs no local code: Claude calls the remote BTP Copilot MCP server at `https://app.btpcopilot.fr/api/mcp` after the user signs in with OAuth.

## Contenu du plugin

### Connecteur

Le serveur MCP distant BTP Copilot, `https://app.btpcopilot.fr/api/mcp`, déclaré dans `.mcp.json`. Il donne accès à vos dossiers : recherche de dossiers, inventaire et recherche dans les pièces, fiche de lecture AO, risques, go / no-go, état de la DPGF, livrables et usage du forfait.

### Compétences

| Compétence | À quoi elle sert |
| --- | --- |
| `analyser-dossier-ao` | Synthèse d'un dossier : fiche de lecture, risques classés par niveau, verdict go / no-go, informations à questionner |
| `nouveau-dossier-ao` | Créer un dossier, obtenir le lien de dépôt du DCE, suivre l'indexation et lancer l'analyse après accord sur le devis |
| `questions-acheteur` | Liste de questions à l'acheteur ou au maître d'œuvre, chacune vérifiée dans le DCE et rattachée à sa pièce |
| `controle-dpgf` | Revue de la DPGF : postes non chiffrés, totaux, écarts entre CCTP, plans et quantités |
| `rediger-livrable-ao` | Note go / no-go, synthèse de lot, fiche de consultation, PPSPS préliminaire, mode opératoire, courrier, checklist — enregistrés, versionnés et exportés en Word |

## Prérequis

- Un compte **BTP Copilot** avec un abonnement actif. Créez-le sur [btpcopilot.fr](https://btpcopilot.fr).
- L'analyse IA d'un dossier consomme votre forfait BTP Copilot. Elle n'est jamais lancée sans votre accord : Claude vous présente d'abord un devis (coût exact, quota restant) et attend votre confirmation.

## Connexion

1. Ajoutez le plugin BTP Copilot depuis l'annuaire de Claude.
2. À la première utilisation, Claude ouvre l'écran de connexion BTP Copilot : connectez-vous et choisissez les autorisations accordées.
3. Révoquez l'accès à tout moment dans BTP Copilot, **Paramètres › Assistants connectés**.

Autorisations proposées :

| Portée | Ce qu'elle permet |
| --- | --- |
| `projects:read` | Lire vos dossiers, leur état et leurs analyses |
| `documents:read` | Lire l'inventaire des pièces et rechercher des extraits |
| `projects:write` | Créer et mettre à jour un dossier, ouvrir un lien de dépôt, lancer une analyse, enregistrer et exporter des livrables |

L'accès de Claude ne dépasse jamais vos propres droits dans BTP Copilot : il est limité à votre organisation et à votre rôle.

## Exemples de demandes

- « Analyse le dossier Collège Victor Hugo et donne-moi les cinq risques principaux. »
- « Crée un dossier pour l'appel d'offres Gymnase Marseille. »
- « Quelles questions poser à l'acheteur sur le lot gros œuvre ? »
- « Vérifie la DPGF et liste les écarts avec le CCTP. »
- « Rédige une note go / no-go de deux pages pour la direction travaux et exporte-la en Word. »

## Ce que le plugin exécute, envoie et stocke

- **Rien ne s'exécute sur votre poste.** Le plugin ne contient ni script, ni hook, ni serveur local : uniquement des compétences en Markdown et l'adresse du serveur MCP distant.
- **Appels au serveur BTP Copilot.** Quand Claude utilise un outil, il envoie à `app.btpcopilot.fr` le nom de l'outil et ses paramètres (identifiant de dossier, termes de recherche, texte d'un livrable à enregistrer). BTP Copilot répond avec les données du dossier.
- **Vos pièces ne passent pas par la conversation.** Le DCE est déposé directement dans BTP Copilot par un lien de dépôt à durée limitée.
- **Ce que BTP Copilot conserve.** Les dossiers créés, les livrables enregistrés (avec leurs versions) et les exports Word sont stockés dans votre espace BTP Copilot. La recherche dans les pièces et l'analyse de dossier sont traitées par BTP Copilot et ses sous-traitants, dans les conditions de sa politique de confidentialité.
- **Aucun autre destinataire.** Le plugin ne contacte aucun autre service que BTP Copilot.

## Confidentialité et conditions

- Politique de confidentialité : [btpcopilot.fr/politique-de-confidentialite](https://btpcopilot.fr/politique-de-confidentialite)
- Conditions générales : [btpcopilot.fr/conditions-generales](https://btpcopilot.fr/conditions-generales)

## Assistance

Écrivez à [contact@btpcopilot.fr](mailto:contact@btpcopilot.fr) ou passez par [btpcopilot.fr/contact](https://btpcopilot.fr/contact).

## Licence

Les fichiers de ce plugin sont distribués sous licence MIT (voir `LICENSE`). Le service BTP Copilot reste soumis à ses conditions générales.
