---
title: Champs standard
feature: REST API, Field Management
description: Parcourez la liste complète des champs de prospect standard Marketo avec des noms, libellés et descriptions REST et SOAP, et apprenez à les récupérer à l’aide de l’API Describe Lead.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 28%

---

# Champs standard

Voici une liste des champs standards disponibles dans Marketo et accessibles via l&#39;API.

Vous pouvez récupérer la liste de tous les noms de champ pris en charge disponibles dans vos enregistrements de prospect à l’aide du point d’entrée REST [Décrire le prospect](https://developer.adobe.com/marketo-apis/api/mapi/).

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
| doNotCallReason | DoNotCallReason | Raison Ne pas appeler | Explication de la préférence de refus d’appel du prospect |
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
| jigsawContactId | ID de contact Marketo Data.com | ID de Marketo Data.com | ID Data.com du lead si disponible |
| jigsawContactStatus | Statut de contact Marketo data.com | Statut Marketo Data.com | Statut Data.com du lead si disponible |
| facebookDisplayName | MarketoSocialFacebookDisplayName | Nom d’affichage Marketo pour Facebook | Nom d’affichage Facebook du prospect. Système renseigné lors de la connexion au réseau social |
| facebookId | MarketoSocialFacebookId | ID Marketo via Facebook | Identifiant Facebook du lead. Système renseigné lors de la connexion au réseau social |
| facebookPhotoURL | MarketoSocialFacebookPhotoURL | URL de photo Marketo pour Facebook | URL de la photo de profil Facebook du prospect. Système renseigné lors de la connexion au réseau social |
| facebookProfileURL | MarketoSocialFacebookProfileURL | URL du profil Marketo pour Facebook | URL du profil Facebook du prospect. Système renseigné lors de la connexion au réseau social |
| facebookReach | MarketoSocialFacebookReach | Portée sociale Marketo via Facebook | La portée Facebook du lead. Système renseigné lors de la connexion au réseau social |
| facebookReferredEnrollments | MarketoSocialFacebookReferredEnrollments | Inscriptions sur recommandation Marketo via Facebook | Nombre d’inscriptions référencées attribuées au lead via Facebook. Gestion du système |
| facebookReferredVisits | MarketoSocialFacebookReferredVisits | Visites sur recommandation Marketo via Facebook | Nombre de visites référées attribuées au prospect via Facebook. Gestion du système |
| sexe | MarketoSocialGender | Sexe Marketo pour réseaux sociaux | Sexe du lead. Système renseigné lors de la connexion au réseau social |
| lastReferredEnrollment | MarketoSocialLastReferredEnrollment | Dernière inscription sur recommandation Marketo via les réseaux sociaux | Date du dernier parrainage terminé. Gestion du système |
| lastReferredVisit | MarketoSocialLastReferredVisit | Dernière visite sur recommandation Marketo via les réseaux sociaux | Date de la dernière visite recommandée. Gestion du système |
| linkedInDisplayName | MarketoSocialLinkedInDisplayName | Nom d’affichage Marketo pour LinkedIn | Nom d’affichage LinkedIn du prospect. Système renseigné lors de la connexion au réseau social |
| linkedInId | MarketoSocialLinkedInId | ID Marketo via LinkedIn | ID LinkedIn du lead. Système renseigné lors de la connexion au réseau social |
| linkedInPhotoURL | MarketoSocialLinkedInPhotoURL | URL de la photo Marketo pour LinkedIn | URL de la photo LinkedIn du lead. Système renseigné lors de la connexion au réseau social |
| linkedInProfileURL | MarketoSocialLinkedInProfileURL | URL du profil Marketo pour LinkedIn | Profil LinkedIn du lead. Système renseigné lors de la connexion au réseau social |
| linkedInReach | MarketoSocialLinkedInReach | Portée sociale Marketo via LinkedIn | La portée de LinkedIn du lead. Système renseigné lors de la connexion au réseau social |
| linkedInReferredEnrollments | MarketoSocialLinkedInReferredEnrollments | Inscriptions sur recommandation Marketo via LinkedIn | Nombre d’inscriptions référencées attribuées au lead via LinkedIn. Gestion du système |
| linkedInReferredVisits | MarketoSocialLinkedInReferredVisits | Visites sur recommandation Marketo via LinkedIn | Nombre de visites référencées attribuées au lead via LinkedIn. Gestion du système |
| syndicationId |  - | ID association sociale Marketo | Identifiant social Marketo interne du prospect. Gestion du système |
| totalReferredEnrollments | MarketoSocialTotalReferredEnrollments | Total des inscriptions sur recommandation Marketo via les réseaux sociaux | Nombre total d’inscriptions pour recommandation terminées attribuées au prospect |
| totalReferredVisits | MarketoSocialTotalReferredVisits | Total des visites Marketo sur recommandation via les réseaux sociaux | Nombre total de visites référées attribuées au prospect |
| twitterDisplayName | MarketoSocialTwitterDisplayName | Nom d’affichage Marketo pour Twitter | Nom d’affichage Twitter du lead. Système renseigné lors de la connexion au réseau social |
| twitterId | MarketoSocialTwitterId | ID Marketo via Twitter | ID Twitter du lead. Système renseigné lors de la connexion au réseau social |
| twitterPhotoURL | MarketoSocialTwitterPhotoURL | URL de la photo Marketo pour Twitter | URL de la photo Twitter du lead. Système renseigné lors de la connexion au réseau social |
| twitterProfileURL | MarketoSocialTwitterProfileURL | URL du profil Marketo pour Twitter | URL du profil Twitter du lead. Système renseigné lors de la connexion au réseau social |
| twitterReach | MarketoSocialTwitterReach | Portée sociale Marketo via Twitter | Portée Twitter de Lead. Système renseigné lors de la connexion au réseau social |
| twitterReferredEnrollments | MarketoSocialTwitterReferredEnrollments | Inscriptions sur recommandation Marketo via Twitter | Nombre d’inscriptions référencées attribuées au prospect via Twitter. Gestion du système |
| twitterReferredVisits | MarketoSocialTwitterReferredVisits | Visites sur recommandation Marketo via Twitter | Nombre de visites référées attribuées au prospect via Twitter. Gestion du système |
| middleName | MiddleName | Deuxième prénom | Deuxième prénom du lead |
| mobilePhone | MobilePhone | Numéro de téléphone mobile | Numéro de téléphone mobile du lead |
| numberOfEmployees | NumberOfEmployees | Nombre d&#39;employés | Nombre d&#39;employés de la société du prospect |
| téléphone | Téléphone | Numéro de téléphone | Numéro de téléphone du lead |
| postalCode | PostalCode | Code postal | Code postal du lead |
| évaluation | Évaluation | Évaluation du lead | Note de marketing/vente du prospect |
| salutation | Titre | Titre | Formule de salutation préférée du lead, à savoir Monsieur, Échecs... etc. |
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
| inféréPhoneAreaCode |  - | Indicatif téléphonique local déduit | Indicatif régional du lead déduit par une recherche IP inversée de la première visite web enregistrée du lead. |
| inferredPostalCode |  - | Code postal déduit | Code postal du prospect déduit par une recherche IP inversée de la première visite web enregistrée du prospect. |
| inferredStateRegion |  - | Région déduite | Région d’état du prospect déduite par une recherche IP inversée de la première visite web enregistrée du prospect. |
| isAnonymous |  - | Est anonyme | Statut anonyme de l’enregistrement du prospect. Gestion du système. |
| priorité |  - | Priorité | Priorité d’Insight des ventes du prospect. Gestion du système. |
| relativeScore |  - | Évaluation relative | Score relatif Insight des ventes du prospect. Gestion du système. |
| urgence |  - | Urgence | Urgence Insight des ventes du prospect. Gestion du système. |
