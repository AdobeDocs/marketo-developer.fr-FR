---
title: Référence du point d’entrée
feature: REST API
description: Liste complète des points d’entrée de l’API REST Marketo avec les méthodes, les URI et les autorisations requises pour les activités, l’exportation en masse, l’identité, les prospects, les ressources et les utilisateurs.
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '4464'
ht-degree: 28%

---

# Référence du point d’entrée

Vous trouverez ci-dessous des liens vers les références de l’API REST Marketo.

- [Ressource](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identité](https://developer.adobe.com/marketo-apis/api/identity/)
- [Base de données de lead](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Gestion des utilisateurs](https://developer.adobe.com/marketo-apis/api/user/)

## Liste des points d’entrée {#endpoint_list}

Voici une liste complète des points d’entrée de l’API REST.

| Nom | Groupe | Méthode | URI | Autorisation requise |
|---|---|---|---|---|
| Ajouter des activités personnalisées | Activités | POST | /rest/v1/activities/external.json | Activité en lecture/écriture |
| Approuver le type d’activité personnalisé | Activités | POST | /rest/v1/activities/external/type/{apiName}/approve.json | Métadonnées d’activité en lecture/écriture |
| Créer Des Attributs De Type D’Activité Personnalisés | Activités | POST | /rest/v1/activities/external/type/{apiName}/attributes/create.json | Métadonnées d’activité en lecture/écriture |
| Création de types d’activités personnalisés | Activités | POST | /rest/v1/activities/external/type.json | Métadonnées d’activité en lecture/écriture |
| Supprimer le type d’activité personnalisé | Activités | POST | /rest/v1/activities/external/type/{apiName}/delete.json | Métadonnées d’activité en lecture/écriture |
| Supprimer les attributs de type d’activité personnalisés | Activités | POST | /rest/v1/activities/external/type/{apiName}/attributes/delete.json | Métadonnées d’activité en lecture/écriture |
| Décrire le type d’activité personnalisé | Activités | GET | /rest/v1/activities/external/type/{apiName}/describe.json | Métadonnées d’activité en lecture seule |
| Ignorer le brouillon de type d’activité personnalisé | Activités | POST | /rest/v1/activities/external/type/{apiName}/discardDraft.json | Métadonnées d’activité en lecture/écriture |
| Obtenir les types d’activités | Activités | GET | /rest/v1/activities/types.json | Activité en lecture seule |
| Obtenir des types d’activités personnalisés | Activités | GET | /rest/v1/activities/external/types.json | Métadonnées d’activité en lecture seule |
| Obtenir les leads supprimés | Activités | GET | /rest/v1/activities/deletedleads.json | Activité en lecture seule |
| Obtenir les activités du lead | Activités | GET | /rest/v1/activities.json | Activité en lecture seule |
| Obtenir les modifications du lead | Activités | GET | /rest/v1/activities/leadchanges.json | Activité en lecture seule |
| Obtenir le jeton de pagination | Activités | GET | /rest/v1/activities/pagingtoken.json | Activité en lecture seule |
| Mettre à jour le type d’activité personnalisé | Activités | POST | /rest/v1/activities/external/type/{apiName}.json | Métadonnées d’activité en lecture/écriture |
| Mettre à jour les attributs de type d’activité personnalisés | Activités | POST | /rest/v1/activities/external/type/{apiName}/attributes/update.json | Métadonnées d’activité en lecture/écriture |
| Identité | Authentification | GET ou POST | /identity/oauth/token | Aucune |
| Annuler le traitement de l’activité d’exportation | Activités d’exportation en bloc | POST | /bulk/v1/activities/export/{exportid}/cancel.json | Activité en lecture seule |
| Création D’Un Traitement D’Activité D’Exportation | Activités d’exportation en bloc | POST | /bulk/v1/activities/export/create.json | Activité en lecture seule |
| Mettre en file d’attente le travail de l’activité d’exportation | Activités d’exportation en bloc | POST | /bulk/v1/activities/export/{exportid}/enqueue.json | Activité en lecture seule |
| Obtenir le fichier d’activité d’exportation | Activités d’exportation en bloc | GET | /bulk/v1/activities/export/{exportid}/file.json | Activité en lecture seule |
| Obtention de l&#39;état du traitement de l&#39;activité d&#39;exportation | Activités d’exportation en bloc | GET | /bulk/v1/activities/export/{exportid}/status.json | Activité en lecture seule |
| Obtenir les tâches d’activité d’exportation | Activités d’exportation en bloc | GET | /bulk/v1/activities/export.json | Activité en lecture seule |
| Annuler l’exportation de la tâche d’objet personnalisé | Exporter en bloc des objets personnalisés | POST | /bulk/v1/customobjects/export/{exportid}/cancel.json | Objet personnalisé en lecture seule |
| Création d’une tâche d’exportation d’objet personnalisé | Exporter en bloc des objets personnalisés | POST | /bulk/v1/customobjects/export/create.json | Objet personnalisé en lecture seule |
| Mettre en file d&#39;attente la tâche d&#39;exportation d&#39;objet personnalisé | Exporter en bloc des objets personnalisés | POST | /bulk/v1/customobjects/export/{exportid}/enqueue.json | Objet personnalisé en lecture seule |
| Obtenir l&#39;exportation du fichier d&#39;objet personnalisé | Exporter en bloc des objets personnalisés | GET | /bulk/v1/customobjects/export/{exportid}/file.json | Objet personnalisé en lecture seule |
| Obtention de l&#39;état de la tâche d&#39;exportation d&#39;objet personnalisé | Exporter en bloc des objets personnalisés | GET | /bulk/v1/customobjects/export/{exportid}/status.json | Objet personnalisé en lecture seule |
| Obtenir les tâches d&#39;objets personnalisés d&#39;exportation | Exporter en bloc des objets personnalisés | GET | /bulk/v1/customobjects/export.json | Objet personnalisé en lecture seule |
| Annuler la tâche d’exportation du lead | Exporter des leads en bloc | POST | /bulk/v1/leads/export/{exportid}/cancel.json | Lead en lecture seule |
| Créer un traitement de lead d’exportation | Exporter des leads en bloc | POST | /bulk/v1/leads/export/create.json | Lead en lecture seule |
| Mettre en file d’attente le traitement du lead d’exportation | Exporter des leads en bloc | POST | /bulk/v1/leads/export/{exportid}/enqueue.json | Lead en lecture seule |
| Obtenir le fichier de lead d&#39;exportation | Exporter des leads en bloc | GET | /bulk/v1/leads/export/{exportid}/file.json | Lead en lecture seule |
| Obtenir le statut de la tâche de lead d’exportation | Exporter des leads en bloc | GET | /bulk/v1/leads/export/{exportid}/status.json | Lead en lecture seule |
| Obtenir les tâches de lead d’exportation | Exporter des leads en bloc | GET | /bulk/v1/leads/export.json | Lead en lecture seule |
| Annuler la tâche de membre du programme d’exportation | Membres du programme d’exportation en bloc | POST | /bulk/v1/program/members/export/{exportid}/cancel.json | Lead en lecture seule |
| Créer un traitement de membre de programme d&#39;exportation | Membres du programme d’exportation en bloc | POST | /bulk/v1/program/members/export/create.json | Lead en lecture seule |
| Mettre en file d&#39;attente le travail de membre du programme d&#39;exportation | Membres du programme d’exportation en bloc | POST | /bulk/v1/program/members/export/{exportid}/enqueue.json | Lead en lecture seule |
| Obtenir le fichier de membre du programme d&#39;exportation | Membres du programme d’exportation en bloc | GET | /bulk/v1/program/members/export/{exportid}/file.json | Lead en lecture seule |
| Obtenir le statut de la tâche du membre du programme d&#39;exportation | Membres du programme d’exportation en bloc | GET | /bulk/v1/program/members/export/{exportid}/status.json | Lead en lecture seule |
| Obtenir les emplois de membre du programme d&#39;exportation | Membres du programme d’exportation en bloc | GET | /bulk/v1/program/members/export.json | Lead en lecture seule |
| Échecs de l&#39;importation d&#39;objets personnalisés | Importer en bloc des objets personnalisés | GET | /bulk/v1/customobjects/import/{id}/failures.json | Objet personnalisé accessible en lecture/écriture |
| Obtenir le statut de l&#39;objet personnalisé importé | Importer en bloc des objets personnalisés | GET | /bulk/v1/customobjects/import/{id}/status.json | Objet personnalisé accessible en lecture/écriture |
| Obtenir des avertissements d&#39;importation d&#39;objet personnalisé | Importer en bloc des objets personnalisés | GET | /bulk/v1/customobjects/import/{id}/warnings.json | Objet personnalisé accessible en lecture/écriture |
| Importer des objets personnalisés | Importer en bloc des objets personnalisés | POST | /bulk/v1/customobjects/{apiName}/import.json | Objet personnalisé accessible en lecture/écriture |
| Obtenir les échecs du lead d&#39;importation | Importer des leads en bloc | GET | /bulk/v1/leads/batch/{id}/failures.json | Lead en lecture/écriture |
| Obtenir le statut du lead d’importation | Importer des leads en bloc | GET | /bulk/v1/leads/batch/{id}.json | Lead en lecture/écriture |
| Obtenir les avertissements du lead d&#39;importation | Importer des leads en bloc | GET | /bulk/v1/leads/batch/{id}/warnings.json | Lead en lecture/écriture |
| Importer les leads | Importer des leads en bloc | POST | /bulk/v1/leads.json | Lead en lecture/écriture |
| Échecs de l&#39;obtention du membre de programme d&#39;importation | Membres du programme d’importation en bloc | GET | /bulk/v1/program/members/import/{id}/failures.json | Lead en lecture/écriture |
| Obtenir le statut de membre du programme d&#39;importation | Membres du programme d’importation en bloc | GET | /bulk/v1/program/members/import/{id}/status.json | Lead en lecture/écriture |
| Obtenir les avertissements des membres du programme d&#39;importation | Membres du programme d’importation en bloc | GET | /bulk/v1/program/members/import/{id}/warnings.json | Lead en lecture/écriture |
| Importer des membres de programme | Membres du programme d’importation en bloc | POST | /bulk/v1/program/{programId}/members/import.json | Lead en lecture/écriture |
| Obtenir la campagne par ID | Campagnes | GET | /rest/v1/campaigns/{id}.json | Campagnes en lecture seule |
| Obtenir des campagnes | Campagnes | GET | /rest/v1/campaigns.json | Campagnes en lecture seule |
| Demander une campagne | Campagnes | POST | /rest/v1/campaigns/{id}/trigger.json | Campagnes en lecture-écriture |
| Planifier la campagne | Campagnes | POST | /rest/v1/campaigns/{id}/schedule.json | Campagnes en lecture-écriture |
| Obtenir le canal par nom | Canaux | GET | /rest/asset/v1/channel/byName.json | Ressource en lecture seule |
| Obtenir les canaux | Canaux | GET | /rest/asset/v1/channels.json | Ressource en lecture seule |
| Supprimer entreprises | Sociétés | POST | /rest/v1/companies/delete.json | Société accessible en lecture/écriture |
| Décrire les sociétés | Sociétés | GET | /rest/v1/companies/describe.json | Société en lecture seule |
| Obtenir les entreprises | Sociétés | GET | /rest/v1/companies.json | Société en lecture seule |
| Synchroniser les entreprises | Sociétés | POST | /rest/v1/companies.json | Société accessible en lecture/écriture |
| Obtenir le champ d’entreprise par nom | Sociétés | GET | /rest/v1/companies/schema/fields/{fieldApiName}.json | Champ personnalisé de schéma lecture-écriture |
| Obtenir les champs d’entreprise | Sociétés | GET | /rest/v1/companies/schema/fields.json | Champ personnalisé de schéma lecture-écriture |
| Ajouter des champs de type d’objet personnalisés | Objets personnalisés | POST | /rest/v1/customobjects/schema/{apiName}/addField.json | Type d’objet personnalisé accessible en lecture/écriture |
| Approuver le type d&#39;objet personnalisé | Objets personnalisés | POST | /rest/v1/customobjects/schema/{apiName}/approve.json | Type d’objet personnalisé accessible en lecture/écriture |
| Supprimer objets personnalisés | Objets personnalisés | POST | /rest/v1/customobjects/{name}/delete.json | Objet personnalisé accessible en lecture/écriture |
| Supprimer le type d&#39;objet personnalisé | Objets personnalisés | POST | /rest/v1/customobjects/schema/{apiName}/delete.json | Type d’objet personnalisé accessible en lecture/écriture |
| Supprimer les champs de type d&#39;objet personnalisés | Objets personnalisés | POST | /rest/v1/customobjects/schema/{apiName}/deleteField.json | Type d’objet personnalisé accessible en lecture/écriture |
| Décrire les objets personnalisés | Objets personnalisés | GET | /rest/v1/customobjects/{name}/describe.json | Objet personnalisé en lecture seule |
| Décrire le type d’objet personnalisé | Objets personnalisés | GET | /rest/v1/customobjects/schema/{apiName}/describe.json | Type d’objet personnalisé en lecture seule |
| Ignorer le brouillon de type d&#39;objet personnalisé | Objets personnalisés | POST | /rest/v1/customobjects/schema/{apiName}/discardDraft.json | Type d’objet personnalisé accessible en lecture/écriture |
| Obtention d’objets personnalisés | Objets personnalisés | GET | /rest/v1/customobjects/{name}.json | Objet personnalisé en lecture seule |
| Obtention d&#39;objets liés d&#39;objet personnalisés | Objets personnalisés | GET | /rest/v1/customobjects/schema/linkableObjects.json | Type d’objet personnalisé en lecture seule |
| Obtenir l’Assets dépendante de l’objet personnalisé | Objets personnalisés | GET | /rest/v1/customobjects/schema/{apiName}/dependentAssets.json | Type d’objet personnalisé en lecture seule |
| Obtenir les types de données de champ de type d&#39;objet personnalisé | Objets personnalisés | GET | /rest/v1/customobjects/schema/fieldDataTypes.json | Type d’objet personnalisé en lecture seule |
| Liste d’objets personnalisés | Objets personnalisés | GET | /rest/v1/customobjects.json | Objet personnalisé en lecture seule |
| Liste des types d’objets personnalisés | Objets personnalisés | GET | /rest/v1/customobjects/schema.json | Type d’objet personnalisé en lecture seule |
| Synchroniser les objets personnalisés | Objets personnalisés | POST | /rest/v1/customobjects/{name}.json | Objet personnalisé accessible en lecture/écriture |
| Synchroniser le type d&#39;objet personnalisé | Objets personnalisés | POST | /rest/v1/customobjects/schema.json | Type d’objet personnalisé accessible en lecture/écriture |
| Mettre à jour le champ de type d&#39;objet personnalisé | Objets personnalisés | POST | /rest/v1/customobjects/schema/{apiName}/updateField.json | Type d’objet personnalisé accessible en lecture/écriture |
| Approuver le brouillon du modèle d’e-mail | Modèles d’e-mail | POST | /rest/asset/v1/emailTemplate/{id}/approveDraft.json | Ressource en lecture-écriture |
| Cloner le modèle d&#39;e-mail | Modèles d’e-mail | POST | /rest/asset/v1/emailTemplate/{id}/clone.json | Ressource en lecture-écriture |
| Créer un modèle d’e-mail | Modèles d’e-mail | POST | /rest/asset/v1/emailTemplates.json | Ressource en lecture-écriture |
| Supprimer le modèle d&#39;e-mail | Modèles d’e-mail | POST | /rest/asset/v1/emailTemplate/{id}/delete.json | Ressource en lecture-écriture |
| Ignorer le brouillon de modèle d&#39;e-mail | Modèles d’e-mail | POST | /rest/asset/v1/emailTemplate/{id}/discardDraft.json | Ressource en lecture-écriture |
| Obtenir le modèle d’e-mail par ID | Modèles d’e-mail | GET | /rest/asset/v1/emailTemplate/{id}.json | Ressource en lecture seule |
| Obtenir le modèle d’e-mail par nom | Modèles d’e-mail | GET | /rest/asset/v1/emailTemplate/byName.json | Ressource en lecture seule |
| Obtenir le contenu du modèle d’e-mail par ID | Modèles d’e-mail | GET | /rest/asset/v1/emailTemplate/{id}/content.json | Ressource en lecture seule |
| Obtenir Le Modèle D’E-Mail Utilisé Par | Modèles d’e-mail | GET | /rest/asset/v1/emailTemplates/{id}/usedBy.json | Ressource en lecture seule |
| Obtenir les modèles d’e-mail | Modèles d’e-mail | GET | /rest/asset/v1/emailTemplates.json | Ressource en lecture seule |
| Désapprouver le brouillon de modèle d&#39;e-mail | Modèles d’e-mail | POST | /rest/asset/v1/emailTemplate/{id}/unapprove.json | Ressource en lecture-écriture |
| Mettre à jour le contenu du modèle d’e-mail | Modèles d’e-mail | POST | /rest/asset/v1/emailTemplate/{id}/content.json | Ressource en lecture-écriture |
| Mettre à jour les métadonnées du modèle d&#39;e-mail | Modèles d’e-mail | POST | /rest/asset/v1/emailTemplate/{id}.json | Ressource en lecture-écriture |
| Add Email Module | E-mails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/add.json | Ressource en lecture-écriture |
| Approuver le brouillon d’e-mail | E-mails | POST | /rest/asset/v1/email/{id}/approveDraft.json | Ressource en lecture-écriture |
| Cloner l&#39;e-mail | E-mails | POST | /rest/asset/v1/email/{id}/clone.json | Ressource en lecture-écriture |
| Créer un e-mail | E-mails | POST | /rest/asset/v1/emails.json | Ressource en lecture-écriture |
| Supprimer l&#39;e-mail | E-mails | POST | /rest/asset/v1/email/{id}/delete.json | Ressource en lecture-écriture |
| Supprimer le module | E-mails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/delete.json | Ressource en lecture-écriture |
| Ignorer le brouillon d&#39;e-mail | E-mails | POST | /rest/asset/v1/email/{id}/discardDraft.json | Ressource en lecture-écriture |
| Dupliquer le module d’e-mail | E-mails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json | Ressource en lecture-écriture |
| Obtenir l’e-mail par ID | E-mails | GET | /rest/asset/v1/email/{id}.json | Ressource en lecture seule |
| Obtenir l’e-mail par nom | E-mails | GET | /rest/asset/v1/email/byName.json | Ressource en lecture seule |
| Obtenir le contenu de l’e-mail | E-mails | GET | /rest/asset/v1/email/{id}/content.json | Ressource en lecture seule |
| Obtenir le contenu dynamique d’un e-mail | E-mails | GET | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Ressource en lecture seule |
| Obtenir le contenu complet de l’e-mail | E-mails | GET | /rest/asset/v1/email/{id}/fullContent.json | Ressource en lecture seule |
| Obtenir les variables d’e-mail | E-mails | GET | /rest/asset/v1/email/{id}/variables.json | Ressource en lecture seule |
| Obtenir les champs CC d’e-mail | E-mails | GET | /rest/asset/v1/email/ccFields.json | Ressource en lecture seule |
| Get Emails | E-mails | GET | /rest/asset/v1/emails.json | Ressource en lecture seule |
| Réorganiser les modules de messagerie | E-mails | POST | /rest/asset/v1/email/{id}/content/rearrange.json | Ressource en lecture-écriture |
| Renommer le module d’e-mail | E-mails | POST | /rest/asset/v1/email/{id}/content/{moduleId}/rename.json | Ressource en lecture-écriture |
| Envoyer un échantillon de l&#39;e-mail | E-mails | POST | /rest/asset/v1/email/{id}/sendSample.json | Ressource en lecture-écriture |
| Désapprouver l’e-mail | E-mails | POST | /rest/asset/v1/email/{id}/unapprove.json | Ressource en lecture-écriture |
| Mettre à jour le contenu de l’e-mail | E-mails | POST | /rest/asset/v1/email/{id}/content.json | Ressource en lecture-écriture |
| Mettre à jour la section de contenu d&#39;e-mail | E-mails | POST | /rest/asset/v1/email/{id}/content/{htmlId}.json | Ressource en lecture-écriture |
| Mettre à jour la section de contenu dynamique d’e-mail | E-mails | POST | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Ressource en lecture-écriture |
| Mettre à jour le contenu complet de l’e-mail | E-mails | POST | /rest/asset/v1/emails/{id}/fullContent.json | Ressource en lecture-écriture |
| Mettre à jour les métadonnées de courrier électronique | E-mails | POST | /rest/asset/v1/email/{id}.json | Ressource en lecture-écriture |
| Mettre à jour la variable d’e-mail | E-mails | POST | /rest/asset/v1/email/{id}/variable/{name}.json | Ressource en lecture-écriture |
| Créer un fichier | Fichiers | POST | /rest/asset/v1/files.json | Ressource en lecture-écriture |
| Obtenir le fichier par ID | Fichiers | GET | /rest/asset/v1/file/{id}.json | Ressource en lecture seule |
| Obtenir le fichier par nom | Fichiers | GET | /rest/asset/v1/file/byName.json | Ressource en lecture seule |
| Obtenir les fichiers | Fichiers | GET | /rest/asset/v1/files.json | Ressource en lecture seule |
| Mettre à jour le contenu du fichier | Fichiers | POST | /rest/asset/v1/file/{id}/content.json | Ressource en lecture-écriture |
| Créer un dossier | Dossiers | POST | /rest/asset/v1/folders.json | Ressource en lecture-écriture |
| Supprimer le dossier | Dossiers | POST | /rest/asset/v1/folder/{id}/delete.json | Ressource en lecture-écriture |
| Obtenir le dossier par ID | Dossiers | GET | /rest/asset/v1/folder/{id}.json | Ressource en lecture seule |
| Obtenir le dossier par nom | Dossiers | GET | /rest/asset/v1/folder/byName.json | Ressource en lecture seule |
| Obtenir le contenu du dossier | Dossiers | GET | /rest/asset/v1/folder/{id}/content.json | Ressource en lecture seule |
| Obtenir les dossiers | Dossiers | GET | /rest/asset/v1/folders.json | Ressource en lecture seule |
| Mettre à jour les métadonnées de dossier | Dossiers | POST | /rest/asset/v1/folder/{id}.json | Ressource en lecture-écriture |
| Ajouter un champ au formulaire | Champs de formulaire | POST | /rest/asset/v1/form/{id}/fields.json | Ressource en lecture-écriture |
| Ajouter un jeu de champs au formulaire | Champs de formulaire | POST | /rest/asset/v1/form/{id}/fieldSet.json | Ressource en lecture-écriture |
| Ajouter des règles de visibilité de champ de formulaire | Champs de formulaire | POST | /rest/asset/v1/form/{formId}/field/{fieldId}/visibility.json | Ressource en lecture-écriture |
| Ajouter un champ de texte enrichi | Champs de formulaire | POST | /rest/asset/v1/form/{id}/richText.json | Ressource en lecture-écriture |
| Supprimer le champ de l’ensemble de champs | Champs de formulaire | POST | /rest/asset/v1/form/{id}/fieldSet/{fieldSetId}/field/{fieldId}/delete.json | Ressource en lecture-écriture |
| Supprimer le champ de formulaire | Champs de formulaire | POST | /rest/asset/v1/form/{id}/field/{fieldId}/delete.json | Ressource en lecture-écriture |
| Obtenir les champs de formulaire disponibles | Champs de formulaire | GET | /rest/asset/v1/form/fields.json | Ressource en lecture seule |
| Obtenir les champs de membre de programme de formulaire disponibles | Champs de formulaire | GET | /rest/asset/v1/form/programMemberFields.json | Ressource en lecture seule |
| Obtenir des champs pour le formulaire | Champs de formulaire | GET | /rest/asset/v1/form/{id}/fields.json | Ressource en lecture seule |
| Mettre à jour les positions des champs | Champs de formulaire | POST | /rest/asset/v1/form/{id}/reArrange.json | Ressource en lecture-écriture |
| Mettre à jour le champ de formulaire | Champs de formulaire | POST | /rest/asset/v1/form/{id}/field/{fieldId}.json | Ressource en lecture-écriture |
| Approuver le brouillon de formulaire | Formulaires | POST | /rest/asset/v1/form/{id}/approveDraft.json | Ressource en lecture-écriture |
| Cloner le formulaire | Formulaires | POST | /rest/asset/v1/form/{id}/clone.json | Ressource en lecture-écriture |
| Créer un formulaire | Formulaires | POST | /rest/asset/v1/forms.json | Ressource en lecture-écriture |
| Obtenir le formulaire utilisé par | Formulaires | GET | /rest/asset/v1/form/{id}/usedBy.json | Ressource en lecture-écriture |
| Supprimer le formulaire | Formulaires | POST | /rest/asset/v1/form/{id}/delete.json | Ressource en lecture-écriture |
| Ignorer le brouillon de formulaire | Formulaires | POST | /rest/asset/v1/form/{id}/discardDraft.json | Ressource en lecture-écriture |
| Obtenir le formulaire par ID | Formulaires | GET | /rest/asset/v1/form/{id}.json | Ressource en lecture seule |
| Obtenir le formulaire par nom | Formulaires | GET | /rest/asset/v1/form/byName.json | Ressource en lecture seule |
| Obtenir Forms | Formulaires | GET | /rest/asset/v1/forms.json | Ressource en lecture seule |
| Obtenir une page de remerciement par ID de formulaire | Formulaires | GET | /rest/asset/v1/form/{id}/thankYouPage.json | Ressource en lecture seule |
| Mettre à jour les métadonnées de formulaire | Formulaires | POST | /rest/asset/v1/form/{id}.json | Ressource en lecture-écriture |
| Mettre à jour le bouton Envoyer | Formulaires | POST | /rest/asset/v1/{id}/submitButton.json | Ressource en lecture-écriture |
| Mettre à jour la page de remerciement | Formulaires | POST | /rest/asset/v1/form/{id}/thankYouPage.json | Ressource en lecture-écriture |
| Ajouter une section de contenu de page de destination | Contenu de la page de destination | POST | /rest/asset/v1/landingPage/{id}/content.json | Ressource en lecture-écriture |
| Supprimer la section de contenu de la page de destination | Contenu de la page de destination | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}/delete.json | Ressource en lecture-écriture |
| Obtenir le contenu de la page de destination | Contenu de la page de destination | GET | /rest/asset/v1/landingPage/{id}/content.json | Ressource en lecture seule |
| Obtenir le contenu dynamique de la page de destination | Contenu de la page de destination | GET | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Ressource en lecture seule |
| Mettre à jour la section de contenu de la page de destination | Contenu de la page de destination | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}.json | Ressource en lecture-écriture |
| Mettre À Jour La Section De Contenu Dynamique De La Page De Destination | Contenu de la page de destination | POST | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Ressource en lecture-écriture |
| Approuver le brouillon du modèle de page de destination | Modèles de pages de destination | POST | /rest/asset/v1/landingPageTemplate/{id}/approveDraft.json | Ressource en lecture-écriture |
| Cloner le modèle de page de destination | Modèles de pages de destination | POST | /rest/asset/v1/landingPageTemplate/{id}/clone.json | Ressource en lecture-écriture |
| Créer un modèle de page de destination | Modèles de pages de destination | POST | /rest/asset/v1/landingPageTemplate.json | Ressource en lecture-écriture |
| Supprimer le modèle de page de destination | Modèles de pages de destination | POST | /rest/asset/v1/landingPageTemplate/{id}/delete.json | Ressource en lecture-écriture |
| Ignorer le brouillon du modèle de page de destination | Modèles de pages de destination | POST | /rest/asset/v1/landingPageTemplate/{id}/discardDraft.json | Ressource en lecture-écriture |
| Obtenir le modèle de page de destination par ID | Modèles de pages de destination | GET | /rest/asset/v1/landingPageTemplate/{id}.json | Ressource en lecture seule |
| Obtenir le modèle de page de destination par nom | Modèles de pages de destination | GET | /rest/asset/v1/landingPageTemplates/byName.json | Ressource en lecture seule |
| Obtenir le contenu du modèle de page de destination | Modèles de pages de destination | GET | /rest/asset/v1/landingPageTemplate/{id}/content.json | Ressource en lecture seule |
| Obtenir les modèles de page de destination | Modèles de pages de destination | GET | /rest/asset/v1/landingPageTemplates.json | Ressource en lecture seule |
| Désapprouver le modèle de page de destination | Modèles de pages de destination | POST | /rest/asset/v1/landingPageTemplate/{id}/unapprove.json | Ressource en lecture-écriture |
| Mettre à jour le contenu du modèle de page de destination | Modèles de pages de destination | POST | /rest/asset/v1/landingPageTemplate/{id}/content.json | Ressource en lecture-écriture |
| Mettre à jour les métadonnées du modèle de page de destination | Modèles de pages de destination | POST | /rest/asset/v1/landingPageTemplate/{id}.json | Ressource en lecture-écriture |
| Approuver le brouillon de la page de destination | Pages de destination | POST | /rest/asset/v1/landingPage/{id}/approveDraft.json | Ressource en lecture-écriture |
| Cloner la page de destination | Pages de destination | POST | /rest/asset/v1/landingPage/{id}/clone.json | Ressource en lecture-écriture |
| Créer Des Pages De Destination | Pages de destination | POST | /rest/asset/v1/landingPages.json | Ressource en lecture-écriture |
| Supprimer la page de destination | Pages de destination | POST | /rest/asset/v1/landingPage/{id}/delete.json | Ressource en lecture-écriture |
| Ignorer le brouillon de la page de destination | Pages de destination | POST | /rest/asset/v1/landingPage/{id}/discardDraft.json | Ressource en lecture-écriture |
| Obtenir la page de destination par ID | Pages de destination | GET | /rest/asset/v1/landingPage/{id}.json | Ressource en lecture seule |
| Obtenir la page de destination par nom | Pages de destination | GET | /rest/asset/v1/landingPage/byName.json | Ressource en lecture seule |
| Obtenir les variables de page de destination | Pages de destination | GET | /rest/asset/v1/landingPage/{id}/variables.json | Ressource en lecture seule |
| Obtenir les pages de destination | Pages de destination | GET | /rest/asset/v1/landingPages.json | Ressource en lecture seule |
| Aperçu de la page de destination | Pages de destination | GET | /rest/asset/v1/landingPage/{id}/preview.json | Ressource en lecture seule |
| Désapprouver la page de destination | Pages de destination | POST | /rest/asset/v1/landingPage/{id}/unapprove.json | Ressource en lecture-écriture |
| Mettre à jour les métadonnées de la page de destination | Pages de destination | POST | /rest/asset/v1/{id}.json | Ressource en lecture-écriture |
| Mettre À Jour Les Variables De La Page De Destination | Pages de destination | POST | /rest/asset/v1/landingPage/{id}/variable/{variableId}.json | Ressource en lecture-écriture |
| Créer Des Règles De Redirection De Page De Destination | Pages de destination | POST | /rest/asset/v1/redirectRules.json | Règles Redirect en lecture et écriture |
| Supprimer la règle de redirection de la page de destination | Pages de destination | POST | /rest/asset/v1/redirectRule/{id}/delete.json | Règles Redirect en lecture et écriture |
| Obtenir les règles de redirection de la page de destination | Pages de destination | GET | /rest/asset/v1/redirectRules.json | Règles Redirect en lecture seule |
| Obtenir la règle de redirection de la page de destination par ID | Pages de destination | GET | /rest/asset/v1/redirectRule/{id}.json | Règles Redirect en lecture seule |
| Mettre À Jour La Règle De Redirection De La Page De Destination | Pages de destination | POST | /rest/asset/v1/redirectRule/{id}.json | Règles Redirect en lecture et écriture |
| Obtenir les domaines de la page de destination | Pages de destination | GET | /rest/asset/v1/landingPageDomains.json | Règles Redirect en lecture seule |
| Association au prospect | Prospects | POST | /rest/v1/leads/{id}/associate.json | Lead en lecture/écriture |
| Modifier le statut du programme de lead | Prospects | POST | /rest/v1/leads/programs/{programId}/status.json | Lead en lecture/écriture |
| Supprimer les leads | Prospects | POST | /rest/v1/leads.json | Lead en lecture/écriture |
| Décrire le lead | Prospects | GET | /rest/v1/leads/describe.json | Lead en lecture seule |
| Décrire le lead 2 | Prospects | GET | /rest/v1/leads/describe2.json | Lead en lecture seule |
| Décrire le membre du programme | Prospects | GET | /rest/v1/program/members/describe.json | Lead en lecture seule |
| Obtenir le lead par ID | Prospects | GET | /rest/v1/lead/{id}.json | Lead en lecture seule |
| Obtenir les partitions de lead | Prospects | GET | /rest/v1/leads/partitions.json | Lead en lecture seule |
| Obtenir les leads par type de filtre | Prospects | GET | /rest/v1/leads.json | Lead en lecture seule |
| Obtenir les leads par ID de programme | Prospects | GET | /rest/v1/leads/programs/{programId}.json | Lead en lecture seule |
| Fusionner des leads | Prospects | POST | /rest/v1/leads/{id}/merge.json | Lead en lecture/écriture |
| Obtenir des listes par ID de lead | Prospects | GET | /rest/v1/leads/{leadId}.json | Ressource en lecture seule |
| Obtenir des programmes par ID de lead | Prospects | GET | /rest/v1/leads/{leadId}programMembership.json | Ressource en lecture seule |
| Obtenir des campagnes intelligentes par ID de lead | Prospects | GET | /rest/v1/leads/{leadId}/smartCampaignMembership.json | Ressource en lecture seule |
| Transmettre le lead à Marketo | Prospects | POST | /rest/v1/leads/partitions.json | Lead en lecture/écriture |
| Envoyer le formulaire | Prospects | POST | /rest/v1/leads/submitForm.json | Lead en lecture/écriture |
| Synchroniser les leads | Prospects | POST | /rest/v1/leads.json | Lead en lecture/écriture |
| Mettre à jour la partition de lead | Prospects | POST | /rest/v1/leads/partitions.json | Lead en lecture/écriture |
| Obtenir le champ de lead par nom | Prospects | GET | /rest/v1/leads/schema/fields/{fieldApiName}.json | Champ personnalisé de schéma lecture-écriture |
| Obtenir les champs de lead | Prospects | GET | /rest/v1/leads/schema/fields.json | Champ personnalisé de schéma lecture-écriture |
| Créer des champs de lead | Prospects | POST | /rest/v1/leads/schema/fields.json | Champ personnalisé de schéma lecture-écriture |
| Mettre à jour le champ de lead | Prospects | POST | /rest/v1/leads/schema/fields/{fieldApiName}.json | Champ personnalisé de schéma lecture-écriture |
| Ajouter des membres de la liste des comptes nommés | Listes de comptes nommés | POST | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Compte nommé en lecture / écriture |
| Supprimer les listes de comptes nommés | Listes de comptes nommés | POST | /rest/v1/namedaccountlists/delete.json | Liste de comptes nommés en lecture/écriture |
| Obtenir les membres de la liste des comptes nommés | Listes de comptes nommés | GET | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Compte nommé en lecture seule |
| Obtenir les listes de comptes nommés | Listes de comptes nommés | GET | /rest/v1/namedaccountlists.json | Liste de comptes nommés en lecture seule |
| Supprimer les membres de la liste des comptes nommés | Listes de comptes nommés | POST | /rest/v1/namedaccountlist/{id}/namedaccounts/remove.json | Compte nommé en lecture / écriture |
| Synchroniser les listes de comptes nommés | Listes de comptes nommés | POST | /rest/v1/namedaccountlists.json | Liste de comptes nommés en lecture/écriture |
| Supprimer les comptes nommés | Comptes désignés | POST | /rest/v1/namedaccounts/delete.json | Compte nommé en lecture / écriture |
| Décrire les comptes nommés | Comptes désignés | GET | /rest/v1/namedaccounts/describe.json | Compte nommé en lecture seule |
| Obtenir les comptes nommés | Comptes désignés | GET | /rest/v1/namedaccounts.json | Compte nommé en lecture seule |
| Synchroniser les comptes nommés | Comptes désignés | POST | /rest/v1/namedaccounts.json | Compte nommé en lecture / écriture |
| Obtenir le champ de compte nommé par nom | Comptes désignés | GET | /rest/v1/namedaccounts/schema/fields/{fieldApiName}.json | Champ personnalisé de schéma lecture-écriture |
| Obtenir les champs de compte nommés | Comptes désignés | GET | /rest/v1/namedaccounts/schema/fields.json | Champ personnalisé de schéma lecture-écriture |
| Supprimer des opportunités | Opportunités | POST | /rest/v1/opportunities/delete.json | Opportunité accessible en lecture/écriture |
| Supprimer rôles d’opportunité | Opportunités | POST | /rest/v1/opportunities/roles/delete.json | Opportunité accessible en lecture/écriture |
| Décrire l’opportunité | Opportunités | GET | /rest/v1/opportunities/describe.json | Opportunité en lecture seule |
| Décrire le rôle de l’opportunité | Opportunités | GET | /rest/v1/opportunities/roles/describe.json | Opportunité en lecture seule |
| Obtenir des opportunités | Opportunités | GET | /rest/v1/opportunities.json | Opportunité en lecture seule |
| Obtenir les rôles d’opportunité | Opportunités | GET | /rest/v1/opportunities/roles.json | Opportunité en lecture seule |
| Opportunités de synchronisation | Opportunités | POST | /rest/v1/opportunities.json | Opportunité accessible en lecture/écriture |
| Rôles d’opportunité de synchronisation | Opportunités | POST | /rest/v1/opportunities/roles.json | Opportunité accessible en lecture/écriture |
| Obtenir le champ de l’opportunité par nom | Opportunités | GET | /rest/v1/opportunités/schema/fields/{fieldApiName}.json | Champ personnalisé de schéma lecture-écriture |
| Obtenir les champs d’opportunité | Opportunités | GET | /rest/v1/opportunities/schema/fields.json | Champ personnalisé de schéma lecture-écriture |
| Supprimer les membres du programme | Membres du programme | POST | /rest/v1/programs/{programId}/members/delete.json | Lead en lecture/écriture |
| Décrire le membre du programme | Membres du programme | GET | /rest/v1/programs/members/describe.json | Lead en lecture seule |
| Obtenir les membres du programme | Membres du programme | GET | /rest/v1/programs/{programId}/members.json | Lead en lecture seule |
| Synchroniser les données des membres du programme | Membres du programme | POST | /rest/v1/programs/{programId}/members.json | Lead en lecture/écriture |
| Synchroniser le statut du membre de programme | Membres du programme | POST | /rest/v1/programs/{programId}/members/status.json | Lead en lecture/écriture |
| Obtenir le champ de membre de programme par nom | Membres du programme | GET | /rest/v1/programs/members/schema/fields/{fieldApiName}.json | Champ personnalisé de schéma lecture-écriture |
| Obtenir les champs de membre de programme | Membres du programme | GET | /rest/v1/programs/members/schema/fields.json | Champ personnalisé de schéma lecture-écriture |
| Créer des champs de membre de programme | Membres du programme | POST | /rest/v1/programs/members/schema/fields.json | Champ personnalisé de schéma lecture-écriture |
| Mettre à jour le champ de membre de programme | Membres du programme | POST | /rest/v1/programs/members/schema/fields/{fieldApiName}.json | Champ personnalisé de schéma lecture-écriture |
| Approuver le programme | Programmes | POST | /rest/asset/v1/program/{id}/approve.json | Ressource en lecture-écriture |
| Cloner le programme | Programmes | POST | /rest/asset/v1/program/{id}/clone.json | Ressource en lecture-écriture |
| Créer des programmes | Programmes | POST | /rest/asset/v1/programs.json | Ressource en lecture-écriture |
| Supprimer le programme | Programmes | POST | /rest/asset/v1/program/{id}/delete.json | Ressource en lecture-écriture |
| Obtenir le programme par ID | Programmes | GET | /rest/asset/v1/program/{id}.json | Ressource en lecture seule |
| Obtenir le programme par nom | Programmes | GET | /rest/asset/v1/program/byName.json | Ressource en lecture seule |
| Obtenir les programmes | Programmes | GET | /rest/asset/v1/programs.json | Ressource en lecture seule |
| Obtenir les programmes par balise | Programmes | GET | /rest/asset/v1/program/byTag.json | Ressource en lecture seule |
| Obtenir la liste dynamique par ID de programme | Programmes | GET | /rest/asset/v1/program/{id}/smartList.json | Ressource en lecture seule |
| Désapprouver le programme | Programmes | POST | /rest/asset/v1/program/{id}/unapprove.json | Ressource en lecture-écriture |
| Mettre à jour les métadonnées du programme | Programmes | POST | /rest/asset/v1/program/{id}.json | Ressource en lecture-écriture |
| Mettre à jour la balise de programme | Programmes | POST | /rest/asset/v1/program/{id}/tag/{tagType}.json | Ressource en lecture-écriture |
| Supprimer la balise de programme | Programmes | POST | /rest/asset/v1/program/{id}/tag/{tagType}/delete.json | Ressource en lecture-écriture |
| Supprimer les vendeurs | Vendeurs | POST | /rest/v1/salespersons/delete.json | Commercial accessible en lecture/écriture |
| Description des commerciaux | Vendeurs | GET | /rest/v1/salespersons/describe.json | Commercial en lecture seule |
| Obtenir les commerciaux | Vendeurs | GET | /rest/v1/salespersons.json | Commercial en lecture seule |
| Synchroniser les commerciaux | Vendeurs | POST | /rest/v1/salespersons.json | Commercial accessible en lecture/écriture |
| Obtenir des segmentations | Segments | GET | /rest/asset/v1/segmentation.json | Ressource en lecture seule |
| Obtention De Segments Pour Les Segmentations | Segments | GET | /rest/asset/v1/segmentation/{id}/segments.json | Ressource en lecture seule |
| Activer campagne intelligente | Campagnes intelligentes | POST | /rest/asset/v1/smartCampaign/{id}/activate.json | Activer la campagne |
| Cloner la campagne intelligente | Campagnes intelligentes | POST | /rest/asset/v1/smartCampaign/{id}/clone.json | Ressource en lecture-écriture |
| Créer une campagne intelligente | Campagnes intelligentes | POST | /rest/asset/v1/smartCampaigns.json | Ressource en lecture-écriture |
| Désactiver la campagne intelligente | Campagnes intelligentes | POST | /rest/asset/v1/smartCampaign/{id}/deactivate.json | Désactiver la campagne |
| Supprimer la campagne intelligente | Campagnes intelligentes | POST | /rest/asset/v1/smartCampaign/{id}/delete.json | Ressource en lecture-écriture |
| Obtenir des campagnes intelligentes | Campagnes intelligentes | GET | /rest/asset/v1/smartCampaigns.json | Ressource en lecture seule |
| Obtenir une campagne intelligente par ID | Campagnes intelligentes | GET | /rest/asset/v1/smartCampaign/{id}.json | Ressource en lecture seule |
| Obtenir une campagne intelligente par nom | Campagnes intelligentes | GET | /rest/asset/v1/smartCampaign/byName.json | Ressource en lecture seule |
| Obtenir une liste dynamique par identifiant de campagne dynamique | Campagnes intelligentes | GET | /rest/asset/v1/smartCampaign/{id}/smartList.json | Ressource en lecture seule |
| Mise à jour de la campagne intelligente | Campagnes intelligentes | POST | /rest/asset/v1/smartCampaign/{id}.json | Ressource en lecture-écriture |
| Cloner la liste intelligente | Listes intelligentes | POST | /rest/asset/v1/smartList/{id}/clone.json | Ressource en lecture-écriture |
| Supprimer la liste intelligente | Listes intelligentes | POST | /rest/asset/v1/smartList/{id}/delete.json | Ressource en lecture-écriture |
| Obtenir la liste dynamique par ID | Listes intelligentes | GET | /rest/asset/v1/smartList/{id}.json | Ressource en lecture seule |
| Obtenir la liste dynamique par nom | Listes intelligentes | GET | /rest/asset/v1/smartList/byName.json | Ressource en lecture seule |
| Obtenir des listes dynamiques | Listes intelligentes | GET | /rest/asset/v1/smartLists.json | Ressource en lecture seule |
| Approuver le brouillon de fragment de code | Extraits | POST | /rest/asset/v1/snippet/{id}/approveDraft.json | Ressource en lecture-écriture |
| Cloner un extrait | Extraits | POST | /rest/asset/v1/snippet/{id}/clone.json | Ressource en lecture-écriture |
| Créer un fragment de code | Extraits | POST | /rest/asset/v1/snippets.json | Ressource en lecture-écriture |
| Supprimer l’extrait | Extraits | POST | /rest/asset/v1/snippet/{id}/delete.json | Ressource en lecture-écriture |
| Ignorer le brouillon de fragment de code | Extraits | POST | /rest/asset/v1/snippet/{id}/discardDraft.json | Ressource en lecture-écriture |
| Obtenir du contenu dynamique | Extraits | GET | /rest/asset/v1/snippet/{id}/dynamicContent.json | Ressource en lecture seule |
| Obtenir le fragment de code par ID | Extraits | GET | /rest/asset/v1/snippet/{id}.json | Ressource en lecture seule |
| Obtenir le contenu du fragment de code | Extraits | GET | /rest/asset/v1/snippet/{id}/content.json | Ressource en lecture seule |
| Obtenir des fragments de code | Extraits | GET | /rest/asset/v1/snippets.json | Ressource en lecture seule |
| Désapprouver extrait | Extraits | POST | /rest/asset/v1/snippet/{id}/unapprove.json | Ressource en lecture-écriture |
| Mettre à jour le contenu du fragment de code | Extraits | POST | /rest/asset/v1/snippet/{id}/content.json | Ressource en lecture-écriture |
| Mettre à jour le contenu dynamique d’un fragment de code | Extraits | POST | /rest/asset/v1/snippet/{id}/dynamicContent/{segmentId}.json | Ressource en lecture-écriture |
| Mettre à jour les métadonnées de fragment de code | Extraits | POST | /rest/asset/v1/snippet/{id}.json | Ressource en lecture-écriture |
| Ajouter à la liste | Listes statiques | POST | /rest/v1/lists/{listId}/leads.json | Lead en lecture/écriture |
| Créer une liste statique | Listes statiques | POST | /asset/v1/staticLists.json | Ressource en lecture-écriture |
| Supprimer la liste statique | Listes statiques | POST | /asset/v1/staticList/{id}/delete.json | Ressource en lecture-écriture |
| Obtenir les leads par ID de liste | Listes statiques | GET | /rest/v1/lists/{listId}/leads.json | Lead en lecture seule |
| Obtenir la liste par ID | Listes statiques | GET | /rest/v1/lists/{id}.json | Lead en lecture seule |
| Obtenir les listes | Listes statiques | GET | /rest/v1/lists.json | Lead en lecture seule |
| Obtenir la liste statique par ID | Listes statiques | GET | /asset/v1/staticList/{id}.json | Ressource en lecture seule |
| Obtenir la liste statique par nom | Listes statiques | GET | /asset/v1/staticList/byName.json | Ressource en lecture seule |
| Obtenir des listes statiques | Listes statiques | GET | /asset/v1/staticLists.json | Ressource en lecture seule |
| Membre de la liste | Listes statiques | GET | /rest/v1/lists/{listId}/leads/ismember.json | Lead en lecture seule |
| Supprimer de la liste | Listes statiques | DELETE | /rest/v1/lists/{listId}/leads.json | Lead en lecture/écriture |
| Mettre à jour les métadonnées de liste statique | Listes statiques | POST | /asset/v1/staticList/{id}.json | Ressource en lecture-écriture |
| Obtenir la balise par nom | Balises | GET | /rest/asset/v1/tagType/byName.json | Ressource en lecture seule |
| Obtenir les types de balises | Balises | GET | /rest/asset/v1/tagTypes.json | Ressource en lecture seule |
| Créer un jeton | Jetons | POST | /rest/asset/v1/folder/{id}/tokens.json | Ressource en lecture-écriture |
| Supprimer le jeton par nom | Jetons | POST | /rest/asset/v1/folder/{id}/tokens/delete.json | Ressource en lecture-écriture |
| Obtention des jetons par ID de dossier | Jetons | GET | /rest/asset/v1/folder/{id}/tokens.json | Ressource en lecture seule |
| Ajouter rôles | Gestion des utilisateurs | POST | /userservice/management/v1/users/{userid}/roles/create.json | Accéder à l’API de gestion utilisateur |
| Supprimer utilisateur invité | Gestion des utilisateurs | POST | /userservice/management/v1/users/{userId}/invite/delete.json | Accéder à l’API de gestion utilisateur |
| Supprimer rôles | Gestion des utilisateurs | POST | /userservice/management/v1/users/{userid}/roles/delete.json | Accéder à l’API de gestion utilisateur |
| Supprimer l’utilisateur | Gestion des utilisateurs | POST | /userservice/management/v1/users/{userId}/delete.json | Accéder à l’API de gestion utilisateur |
| Obtenir l’utilisateur invité par ID | Gestion des utilisateurs | GET | /userservice/management/v1/users/{userid}/invite.json | Accéder à l’API de gestion utilisateur |
| Obtenir des rôles | Gestion des utilisateurs | GET | /userservice/management/v1/users/roles.json | Accéder à l’API de gestion utilisateur |
| Obtenir les rôles et les espaces de travail par ID | Gestion des utilisateurs | GET | /userservice/management/v1/users/{userid}/roles.json | Accéder à l’API de gestion utilisateur |
| Obtenir des utilisateurs | Gestion des utilisateurs | GET | /userservice/management/v1/users/allusers.json | Accéder à l’API de gestion utilisateur |
| Obtenir l’utilisateur par ID | Gestion des utilisateurs | GET | /userservice/management/v1/users/{userid}/user.json | Accéder à l’API de gestion utilisateur |
| Obtenir les espaces de travail | Gestion des utilisateurs | GET | /userservice/management/v1/users/workspaces.json | Accéder à l’API de gestion utilisateur |
| Inviter un utilisateur | Gestion des utilisateurs | POST | /userservice/management/v1/users/invite.json | Accéder à l’API de gestion utilisateur |
| Mettre à jour les attributs utilisateur | Gestion des utilisateurs | POST | /userservice/management/v1/users/{userId}/update.json | Accéder à l’API de gestion utilisateur |
