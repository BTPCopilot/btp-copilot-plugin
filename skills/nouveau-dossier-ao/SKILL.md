---
name: nouveau-dossier-ao
description: Crée un dossier d'appel d'offres BTP dans BTP Copilot, fournit le lien de dépôt du DCE, suit l'indexation des pièces, puis lance l'analyse IA après accord explicite de l'utilisateur sur le devis. À utiliser quand l'utilisateur veut ouvrir, créer ou démarrer un dossier AO, déposer un DCE, ou lancer l'analyse d'un marché de travaux.
---

# Ouvrir un dossier et lancer l'analyse

Tu accompagnes l'utilisateur du nouvel appel d'offres jusqu'à la fiche de lecture, avec les outils du connecteur BTP Copilot. Réponds en français.

Principe à expliquer si besoin : **les pièces du DCE ne transitent jamais par la conversation.** Elles sont déposées dans BTP Copilot par un lien de dépôt, où elles sont versionnées et indexées. Si l'utilisateur joint des PDF ou un ZIP au chat, explique que le connecteur ne les reçoit pas et donne-lui le lien de dépôt.

## 1. Éviter les doublons

Avant de créer quoi que ce soit, appelle `search_projects` avec le nom de l'opération. Si un dossier proche existe, demande s'il faut le reprendre plutôt qu'en ouvrir un second.

## 2. Créer le dossier

Il faut au minimum l'intitulé de l'opération, tel qu'il figure dans l'avis. Le lieu et la date limite de remise sont utiles s'ils sont connus ; ne les invente pas, ils pourront être corrigés après lecture du RC.

Appelle `create_project`, puis enchaîne directement avec `create_upload_session`.

## 3. Transmettre le lien de dépôt

Donne le lien tel quel, sans le raccourcir ni le reformuler, avec :

- ce qu'on peut déposer : ZIP du DCE, PDF, Excel (DPGF, BPU, DQE), Word, plans ;
- sa durée de validité (24 h par défaut ; `duree_minutes` va de 15 minutes à 7 jours si l'utilisateur en a besoin) ;
- la consigne de revenir dans la conversation une fois le dépôt terminé.

Le lien permet de déposer, pas de lire : on peut le transmettre à un collègue qui détient le DCE.

## 4. Suivre l'indexation

Quand l'utilisateur annonce que c'est déposé, appelle `get_project_status`.

- Indexation en cours : donne l'avancement, puis vérifie à nouveau une ou deux fois au plus. Si ça dure, propose à l'utilisateur de revenir plus tard plutôt que d'interroger l'outil en boucle.
- Pièces manquantes ou illisibles signalées : liste-les, l'utilisateur peut rouvrir un lien de dépôt avec `create_upload_session`.

## 5. Lancer l'analyse — toujours avec un devis

`start_analysis` engage le forfait du client. Le serveur l'impose en deux temps :

1. **Appel sans `confirmation`** : rien n'est lancé, l'outil renvoie un devis — livrables produits (fiche de lecture, carte du DCE, synthèse des risques), coût exact, usage du compte.
2. **Présente ce devis** à l'utilisateur en reprenant le coût et ce qu'il restera sur son forfait, puis pose la question : « Je lance l'analyse ? »
3. **Seulement après un « oui » explicite**, rappelle `start_analysis` avec la `confirmation` reçue. Un accord général donné plus tôt (« fais tout ») ne remplace pas l'accord sur le devis affiché.

Cas particuliers renvoyés par l'outil :

- **Analyse impossible** (abonnement inactif, quota épuisé sans pack, module non activé) : explique la cause et l'action de déblocage indiquée.
- **Déjà analysé** pour cette version des pièces : passe à la lecture, sans relancer. Ne propose `relancer: true` que si l'utilisateur le demande (par exemple après un additif mal pris en compte) — cela produit un nouveau devis.
- **Confirmation expirée ou périmée** (plus de 10 minutes, ou pièces ajoutées entre-temps) : redemande un devis et un nouvel accord.

## 6. Suivre le traitement

Appelle `get_job_status` avec le `job_id` renvoyé. L'analyse prend quelques minutes : donne l'étape en cours, sans promettre de délai précis. Une fois terminée, enchaîne avec la compétence `analyser-dossier-ao`.

## 7. Corriger les métadonnées

Après lecture de la fiche, si la date limite de remise, l'intitulé ou le lieu enregistrés diffèrent du RC, propose la correction et attends l'accord de l'utilisateur avant d'appeler `update_project` : l'outil remplace la valeur existante.
