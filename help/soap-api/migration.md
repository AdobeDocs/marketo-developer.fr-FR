---
title: Migration vers l’API REST
feature: SOAP
description: Guide détaillé pour la migration de Marketo Engage de SOAP vers REST d’ici le 31 janvier 2026, avec mappages de points d’entrée, OAuth, méthodes de synchronisation des prospects et architectures de référence.
exl-id: c2956db3-defe-4163-99f3-58654ce8ee2b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 2%

---

# Migration vers l’API REST

L’API Marketo Engage SOAP sera supprimée après le 31 janvier 2026. Toutes les intégrations existantes utilisant l’API SOAP doivent être retirées ou migrées vers l’API REST [Marketo Engage](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/rest-api) d’ici cette date afin d’éviter des interruptions de service.

## Migration

L’API SOAP prend en charge un éventail limité de cas d’utilisation par rapport à l’API [REST](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/rest-api)I. Lorsque vous déterminez les points d’entrée à mapper à vos cas d’utilisation, vous devez suivre les [bonnes pratiques d’intégration de Marketo](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/marketo-integration-best-practices)

Les [architectures de référence](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/reference-architectures) sont disponibles pour les cas d’utilisation de [synchronisation CRM](https://experienceleague.adobe.com/docs/marketo-developer/assets/sync-architecture-whitepaper.pdf?lang=fr) et [exportation Data Warehouse](https://experienceleague.adobe.com/docs/marketo-developer/assets/reference_architecture.pdf?lang=fr).

## Authentification

[Documentation d’authentification](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/authentication)

L’API REST Marketo utilise l’authentification basée sur OAuth 2.0 avec le type d’octroi des informations d’identification client . Les jetons d’accès sont valides pendant une heure après leur création.

## Prospects

[&#x200B; Documentation de l’API de lead &#x200B;](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/lead-database/leads)

L’API SOAP prend en charge la synchronisation des données de prospect, l’association de cookies [Munchkin &#x200B;](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/javascriptapi/leadtracking/lead-tracking) et la fusion de prospects. Si votre application appelle la méthode syncLead de SOAP et définit le paramètre `marketoCookie`, vous pouvez effectuer la migration de l’une des manières suivantes :

1. Utilisation de la méthode REST [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST), suivie de [Associated Lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST)
2. Vous pouvez appeler [Envoyer le formulaire](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/lead-database/leads), bien que cela nécessite la configuration de certaines Assets marketing et une interaction avec l’API [Forms](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/assets/forms)

Les applications qui utilisent le type de clé `foreignSysPersonId` doivent migrer vers à l’aide d’un champ de prospect personnalisé pour représenter cet identifiant externe et utiliser des méthodes REST [Sync Leads](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/lead-database/leads#create-and-update) ou [Bulk Lead Import](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import).

| Méthode SOAP | Méthode(s) REST |
| --- | --- |
| [getLead](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/leads/getlead) | [Obtenir les leads par ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Obtenir les leads par type de filtre](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) |
| [getMultipleLeads](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/leads/getmultipleleads) | [Get Lead by ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), [Get Leads by Program ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByProgramIdUsingGET), [Get Leads by List ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByListIdUsingGET), [Bulk Lead Export](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads) |
| [mergeLeads](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/leads/mergeleads) | [Fusionner les prospects](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) |
| [syncLead](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/leads/synclead) | [Synchroniser les leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Envoyer le formulaire](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) [Associer le lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST) |
| [syncMultipleLeads](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/leads/syncmultipleleads) | [Synchroniser les leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Importer en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |

## Mes objets

M Objects était un concept fourre-tout destiné à prendre en charge l’exportation des données d’attribution d’opportunités pour une analyse externe. Il fonctionnait avec trois types d’objets : Opportunités, Rôles d’opportunités et Programmes.

Documentation REST :

- [Opportunité](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/lead-database/opportunities)
- [Rôles](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/lead-database/opportunity-roles)
- [&#x200B; Programmes &#x200B;](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/assets/programs)

| Méthode SOAP | Méthode(s) REST |
| --- | --- |
| [deleteMObjects](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/marketo-objects/deletemobjects) | [Supprimer des opportunités](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST), [Supprimer des rôles d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunityRolesUsingPOST) |
| [descriptionmobjects](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/marketo-objects/describemobject) | [Décrire l’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_4), [Décrire le rôle de l’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [getMObjects](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/marketo-objects/getmobjects) | [Obtenir des opportunités](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getOpportunitiesUsingGET), [Obtenir des rôles d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [listMObjects](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/marketo-objects/listmobjects) | S/O |
| [syncMObjects](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/marketo-objects/syncmobjects) | [Opportunités de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunitiesUsingPOST), [Rôles d’opportunité de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunityRolesUsingPOST) |
| [getChannels](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/programs/getchannels) | [Obtenir les canaux](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllChannelsUsingGET) |
| [getTags](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/programs/gettags) | [Get Tag Types](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagTypesUsingGET), [Get Tag by Name](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagByNameUsingGET) |

## Listes statiques

Liste statique Les cas d’utilisation dans l’API SOAP se limitent à l’ingestion des données d’abonnement et de prospect, ainsi qu’à la suppression de l’abonnement qui peut être effectuée à l’aide des méthodes REST [Ajouter à la liste](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST), [Importer des prospects en bloc](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) ou [Supprimer de la liste](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE).

| Méthode SOAP | Méthode(s) REST |
| --- | --- |
| [getImportToListStatus](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/static-lists/getimporttoliststatus) | [Importer des leads en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [importToList](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/static-lists/importtolist) | [Ajouter À La Liste](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) [Importer Des Leads En Bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [listOperation](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/static-lists/listoperation) | [Supprimer de la liste](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) |

## Activités

L’API SOAP ne prend en charge que la récupération des activités.

Documentation REST :

- [Activités synchrones](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/lead-database/activities)
- [Extraction d’activité en bloc](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/bulk-extract/bulk-activity-extract)

| Méthode SOAP | Méthode(s) REST |
| --- | --- |
| [getLeadActivity](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/activities/getleadactivity) | [Activités D’Exportation En Bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Obtenir Les Activités De Lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) |
| [getLeadChanges](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/activities/getleadchanges) | [Activités D’Exportation En Bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Obtenir Les Modifications De Lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadChangesUsingGET) |

## Campagnes

Documentation REST :

- [Campagnes intelligentes](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/assets/smart-campaigns)

L’API SOAP ne prend en charge que trois cas d’utilisation de campagnes intelligentes : [Déclenchement de la qualification des prospects pour une campagne intelligente pouvant faire l’objet d’une demande](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/assets/smart-campaigns#trigger), récupération de ces campagnes intelligentes pouvant faire l’objet d’une demande et [Planification d’une exécution future d’une campagne intelligente](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/assets/smart-campaigns#schedule).

| Méthode SOAP | Méthode(s) REST |
| --- | --- |
| [getCampaignsForSource](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/campaigns/getcampaignsforsource) | [Obtenir des campagnes intelligentes](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllSmartCampaignsGET) |
| [requestCampaign](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/campaigns/requestcampaign) | [Demander la campagne](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) |
| [scheduleCampaign](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/campaigns/schedulecampaign) | [Planifier la campagne](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) |

## Objets personnalisés

Documentation REST :

- [Objets personnalisés](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/lead-database/custom-objects)

L’API SOAP ne prenait en charge que les opérations CRUD pour les objets personnalisés.

| Méthode SOAP | Méthode(s) REST |
| --- | --- |
| [deleteCustomObjects](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/custom-objects/deletecustomobjects) | [Supprimer objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteCustomObjectsUsingPOST) |
| [getCustomObjects](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/custom-objects/getcustomobjects) | [Obtenir des objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCustomObjectsUsingGET) |
| [syncCustomObjects](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/custom-objects/synccustomobjects) | [Synchroniser les objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncCustomObjectsUsingPOST) [Importer en bloc un objet personnalisé](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import) |
