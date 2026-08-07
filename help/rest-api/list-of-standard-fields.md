---
title: Champs standard
feature: REST API, Field Management
description: Parcourez la liste complète des champs de prospect standard Marketo avec des noms, libellés et descriptions REST, et apprenez à les récupérer à l’aide de l’API Describe Lead.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
TQID: https://experienceleague.adobe.com/vu2wGk36XJ243vwavhfLE7Vc9vMIJKGx6vmVqMRgEDA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: bcf56d2102f2f60eac5ad3318d348fd020391e6b
workflow-type: tm+mt
source-wordcount: 688
ht-degree: 19%

---

# Champs standard

Le tableau suivant répertorie les champs Marketo standard disponibles via l’API. Il comprend le nom, le libellé et la description de l’API REST de chaque champ.

Utilisez le point d’entrée REST [Décrire le prospect](https://developer.adobe.com/marketo-apis/api/mapi) pour récupérer tous les noms de champ pris en charge par vos enregistrements de prospect.

| REST API Nom | Intitulé convivial | Description |
| --- | --- | --- |
| adresse | Adresse | Adresse du lead |
| annualRevenue | Revenus annuels | Chiffre d’affaires annuel de l’entreprise du prospect |
| anonymeIP | IP anonyme | Adresse IP de la première visite web enregistrée du prospect |
| billingCity | Ville de facturation | Ville de l’adresse de facturation du prospect |
| billingCountry | Pays de facturation | Pays de l’adresse de facturation du prospect |
| billingPostalCode | Code postal de facturation | Code postal de l’adresse de facturation du prospect |
| billingState | État de facturation | Département ou province de l&#39;adresse de facturation du prospect |
| billingStreet | Adresse de facturation | Adresse postale de facturation de la société du prospect |
| ville | Ville | Ville du lead |
| société | Nom de la société | Nom de la société du lead |
| pays | Pays | Pays du lead |
| dateOfBirth | Date de naissance | Date de naissance du lead |
| service | Service | Département du lead dans son entreprise |
| doNotCall | Ne pas appeler | Préférence de non-appel du lead |
| doNotCallReason | Raison pour « Ne pas appeler » | Explication de la préférence de refus d’appel du prospect |
| E-mail | Adresse e-mail | Adresse e-mail du lead. Champ de clé Marketo standard pour les enregistrements de prospect |
| télécopie | Numéro de fax | Numéro de fax du lead |
| Prénom | Prénom | Prénom du lead |
| secteur | Secteur | Industrie du lead |
| inféréCompany | Société déduite | Nom de société déduit par une recherche IP inversée de la première visite web enregistrée du prospect |
| Pays déduit | Pays déduit | Pays déduit par une recherche IP inversée de la première visite web enregistrée du prospect |
| Nom | Nom | Nom de famille du lead |
| leadRole | Rôle | Rôle du prospect dans son entreprise |
| leadScore | Évaluation des leads | Score entier attribué au prospect par la notation des campagnes et des programmes |
| leadSource | Source du lead | Champ enregistrant la source d’où provient le prospect |
| leadStatus | Statut du lead | Champ d’enregistrement du statut de vente/marketing actuel du prospect |
| mainPhone | Téléphone principal | Numéro de téléphone Principal de la société du prospect |
| jigsawContactId | ID de Marketo Data.com | ID Data.com du lead si disponible |
| jigsawContactStatus | Statut Marketo Data.com | Statut Data.com du lead si disponible |
| middleName | Deuxième prénom | Deuxième prénom du lead |
| mobilePhone | Numéro de téléphone mobile | Numéro de téléphone mobile du lead |
| numberOfEmployees | Nombre d&#39;employés | Nombre d&#39;employés de la société du prospect |
| téléphone | Numéro de téléphone | Numéro de téléphone du lead |
| postalCode | Code postal | Code postal du lead |
| évaluation | Évaluation du lead | Note de marketing/vente du prospect |
| salutation | Titre | La salutation préférée de Lead, c&#39;est-à-dire Monsieur, Mlle... et ainsi de suite |
| sicCode | Code SIC | Code de classification industrielle standard de la société du prospect |
| site | Site |  |
| state | État | État du lead |
| titre | Intitulé du poste | Fonction du lead |
| désabonné | Désabonné ou désabonnée | Statut de désabonnement de l’e-mail du lead. Partiellement géré par le système. Empêchera la réception d&#39;e-mails non opérationnels si défini sur true. |
| unsubscribedReason | Raison du désabonnement | Raison du statut de désabonnement du prospect. Partiellement géré par le système. Renseigné avec des informations de messagerie si le prospect s’est désabonné directement d’un e-mail Marketo. |
| site internet | Site web | URL du site web de la société du prospect |
| createdAt | Créé à | Heure à laquelle l’enregistrement de prospect a été initialement créé. Gestion du système |
| updatedAt | Mis à jour à | Dernière mise à jour de l’enregistrement du prospect. Gestion du système |
| emailInvalid | E-mail non valide | Statut de l’e-mail non valide. Tous les e-mails à l’adresse seront bloqués s’ils sont définis sur true. Les rebonds indiquant que l’e-mail n’est pas valide définiront automatiquement ce champ sur « true ». |
| emailInvalidCause | Cause de l&#39;e-mail non valide | Cause du statut d’e-mail non valide. Le message de rebond à l’origine est enregistré dans ce champ lorsque la valeur true est affectée à l’e-mail non valide. |
| inféréVille | Ville déduite | Ville du prospect déduite par une recherche IP inversée de la première visite web enregistrée du prospect. |
| inféréRégionMétropolitaine | Aire métropolitaine déduite | Région métropolitaine du prospect déduite par une recherche IP inversée de la première visite web enregistrée du prospect. |
| inféréPhoneAreaCode | Indicatif régional de téléphone déduit | Indicatif régional du lead déduit par une recherche IP inversée de la première visite web enregistrée du lead. |
| inferredPostalCode | Code postal déduit | Code postal du prospect déduit par une recherche IP inversée de la première visite web enregistrée du prospect. |
| inferredStateRegion | Région déduite | Région d’état du prospect déduite par une recherche IP inversée de la première visite web enregistrée du prospect. |
| isAnonymous | Est anonyme | Statut anonyme de l’enregistrement du prospect. Gestion du système. |
| priorité | Priorité | Priorité d’Insight des ventes du prospect. Gestion du système. |
| relativeScore | Évaluation relative | Score relatif Insight des ventes du prospect. Gestion du système. |
| urgence | Urgence | Urgence Insight des ventes du prospect. Gestion du système. |
