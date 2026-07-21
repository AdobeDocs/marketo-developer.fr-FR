---
title: Champs standard
feature: REST API, Field Management
description: Parcourez la liste complète des champs de prospect standard Marketo avec des noms, libellés et descriptions REST et SOAP, et apprenez à les récupérer à l’aide de l’API Describe Lead.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
TQID: https://experienceleague.adobe.com/vu2wGk36XJ243vwavhfLE7Vc9vMIJKGx6vmVqMRgEDA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 745
ht-degree: 24%

---

# Champs standard

Le tableau suivant répertorie les champs Marketo standard disponibles via l’API. Il comprend le nom de l’API REST de chaque champ, le nom, le libellé et la description de l’API SOAP.

Utilisez le point d’entrée REST [Décrire le prospect](https://developer.adobe.com/marketo-apis/api/mapi) pour récupérer tous les noms de champ pris en charge par vos enregistrements de prospect.

| REST API Nom | SOAP API Nom | Intitulé convivial | Description |
| --- | --- | --- | --- |
| adresse | Adresse | Adresse | Adresse du lead |
| annualRevenue | AnnualRevenue | Revenus annuels | Chiffre d’affaires annuel de l’entreprise du prospect |
| anonymeIP | AnonymousIP | IP anonyme | Adresse IP de la première visite web enregistrée du prospect |
| billingCity | BillingCity | Ville de facturation | Ville de l’adresse de facturation du prospect |
| billingCountry | BillingCountry | Pays de facturation | Pays de l’adresse de facturation du prospect |
| billingPostalCode | BillingPostalCode | Code postal de facturation | Code postal de l’adresse de facturation du prospect |
| billingState | BillingState | État de facturation | Département ou province de l&#39;adresse de facturation du prospect |
| billingStreet | BillingStreet | Adresse de facturation | Adresse postale de facturation de la société du prospect |
| ville | Ville | Ville | Ville du lead |
| société | Société | Nom de la société | Nom de la société du lead |
| pays | Pays | Pays | Pays du lead |
| dateOfBirth | DateofBirth | Date de naissance | Date de naissance du lead |
| service | Service | Service | Département du lead dans son entreprise |
| doNotCall | DoNotCall | Ne pas appeler | Préférence de non-appel du lead |
| doNotCallReason | DoNotCallReason | Raison pour « Ne pas appeler » | Explication de la préférence de refus d’appel du prospect |
| E-mail | E-mail | Adresse e-mail | Adresse e-mail du lead. Champ de clé Marketo standard pour les enregistrements de prospect |
| télécopie | Fax | Numéro de fax | Numéro de fax du lead |
| Prénom | FirstName | Prénom | Prénom du lead |
| secteur | Secteur industriel | Secteur industriel | Industrie du lead |
| inféréCompany | InferredCompany | Société déduite | Nom de société déduit par une recherche IP inversée de la première visite web enregistrée du prospect |
| Pays déduit | InferredCountry | Pays déduit | Pays déduit par une recherche IP inversée de la première visite web enregistrée du prospect |
| Nom | LastName | Nom | Nom de famille du lead |
| leadRole | LeadRole | Rôle | Rôle du prospect dans son entreprise |
| leadScore | LeadScore | Évaluation des leads | Score entier attribué au prospect par la notation des campagnes et des programmes |
| leadSource | LeadSource | Source du lead | Champ enregistrant la source d’où provient le prospect |
| leadStatus | LeadStatus | Statut du lead | Champ d’enregistrement du statut de vente/marketing actuel du prospect |
| mainPhone | MainPhone | Téléphone principal | Numéro de téléphone Principal de la société du prospect |
| jigsawContactId | ID de contact Marketo Jigsaw | ID de Marketo Data.com | ID Data.com du lead si disponible |
| jigsawContactStatus | Statut de contact Marketo Jigsaw | Statut Marketo Data.com | Statut Data.com du lead si disponible |
| middleName | MiddleName | Deuxième prénom | Deuxième prénom du lead |
| mobilePhone | MobilePhone | Numéro de téléphone mobile | Numéro de téléphone mobile du lead |
| numberOfEmployees | NumberOfEmployees | Nombre d&#39;employés | Nombre d&#39;employés de la société du prospect |
| téléphone | Téléphone | Numéro de téléphone | Numéro de téléphone du lead |
| postalCode | PostalCode | Code postal | Code postal du lead |
| évaluation | Évaluation | Évaluation du lead | Note de marketing/vente du prospect |
| salutation | Titre | Titre | La salutation préférée de Lead, c&#39;est-à-dire Monsieur, Mlle... et ainsi de suite |
| sicCode | SICCode | Code SIC | Code de classification industrielle standard de la société du prospect |
| site | Site | Site |  |
| state | État | État | État du lead |
| titre | Titre | Intitulé du poste | Fonction du lead |
| désabonné | Désabonné ou désabonnée | Désabonné ou désabonnée | Statut de désabonnement de l’e-mail du lead. Partiellement géré par le système. Empêchera la réception d&#39;e-mails non opérationnels si défini sur true. |
| unsubscribedReason | UnsubscribedReason | Raison du désabonnement | Raison du statut de désabonnement du prospect. Partiellement géré par le système. Renseigné avec des informations de messagerie si le prospect s’est désabonné directement d’un e-mail Marketo. |
| site internet | Site web | Site web | URL du site web de la société du prospect |
| createdAt |  - | Créé à | Heure à laquelle l’enregistrement de prospect a été initialement créé. Gestion du système |
| updatedAt |  - | Mis à jour à | Dernière mise à jour de l’enregistrement du prospect. Gestion du système |
| emailInvalid |  - | E-mail non valide | Statut de l’e-mail non valide. Tous les e-mails à l’adresse seront bloqués s’ils sont définis sur true. Les rebonds indiquant que l’e-mail n’est pas valide définiront automatiquement ce champ sur « true ». |
| emailInvalidCause |  - | Cause de l&#39;e-mail non valide | Cause du statut d’e-mail non valide. Le message de rebond à l’origine est enregistré dans ce champ lorsque la valeur true est affectée à l’e-mail non valide. |
| inféréVille |  - | Ville déduite | Ville du prospect déduite par une recherche IP inversée de la première visite web enregistrée du prospect. |
| inféréRégionMétropolitaine |  - | Aire métropolitaine déduite | Région métropolitaine du prospect déduite par une recherche IP inversée de la première visite web enregistrée du prospect. |
| inféréPhoneAreaCode |  - | Indicatif régional de téléphone déduit | Indicatif régional du lead déduit par une recherche IP inversée de la première visite web enregistrée du lead. |
| inferredPostalCode |  - | Code postal déduit | Code postal du prospect déduit par une recherche IP inversée de la première visite web enregistrée du prospect. |
| inferredStateRegion |  - | Région déduite | Région d’état du prospect déduite par une recherche IP inversée de la première visite web enregistrée du prospect. |
| isAnonymous |  - | Est anonyme | Statut anonyme de l’enregistrement du prospect. Gestion du système. |
| priorité |  - | Priorité | Priorité d’Insight des ventes du prospect. Gestion du système. |
| relativeScore |  - | Évaluation relative | Score relatif Insight des ventes du prospect. Gestion du système. |
| urgence |  - | Urgence | Urgence Insight des ventes du prospect. Gestion du système. |
