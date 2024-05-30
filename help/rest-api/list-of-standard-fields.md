---
title: "Champs standard"
feature: REST API, Field Management
description: "Un tableau de champs Marketo standard."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 28%

---


# Champs standard

Voici une liste des champs standard disponibles dans Marketo et accessibles via l’API.

Vous pouvez récupérer la liste de tous les noms de champ pris en charge disponibles dans vos enregistrements de piste à l’aide du REST. [Description de la piste](https://developer.adobe.com/marketo-apis/api/mapi/) point de terminaison .

| REST API Nom | SOAP API Nom | Intitulé convivial | Description |
| --- | --- | --- | --- |
| address | Adresse | Adresse | Adresse du responsable |
| yearRevenue | AnnualRevenue | Chiffre d&#39;affaires annuel | Recettes annuelles de la société du prospect |
| anonymousIP | AnonymousIP | IP anonyme | Adresse IP de la première visite web enregistrée du prospect |
| billingCity | BillingCity | Ville de facturation | Ville de l’adresse de facturation du prospect |
| billingCountry | BillingCountry | Pays de facturation | Pays de l’adresse de facturation du prospect |
| billingPostalCode | BillingPostalCode | Code postal de facturation | Code postal de l’adresse de facturation du prospect |
| billingState | BillingState | État de facturation | Etat ou province de l&#39;adresse de facturation du prospect |
| billingStreet | BillingStreet | Adresse de facturation | Adresse postale de la société de prospect |
| city | Ville | Ville | La ville de Lead |
| société | Société | Nom de l’entreprise | Nom de la société du prospect |
| country | Pays | Pays | Pays du leader |
| dateOfBirth | DateofBirth | Date de naissance | Date de naissance de la piste |
| service | Service | Service | Responsable de service dans leur entreprise |
| doNotCall | DoNotCall | Ne pas appeler | Préférence du prospect à ne pas appeler |
| doNotCallReason | DoNotCallReason | Raison Ne pas appeler | Explication de la préférence de ne pas appeler de piste |
| E-mail | E-mail | Adresse e-mail | Adresse électronique du prospect. Champ de clé Marketo standard pour les enregistrements de piste |
| fax | Fax | Numéro de fax | Numéro de fax du prospect |
| firstName | FirstName | Prénom | Prénom du prospect |
| secteur | Industrie | Industrie | L&#39;industrie du leader |
| inferredCompany | InferredCompany | Société déduite | Nom de la société déduit par la recherche IP inversée de la première visite web enregistrée du prospect |
| inferredCountry | InferredCountry | Pays déduit | Pays déduit par la recherche IP inversée de la première visite web enregistrée du prospect |
| lastName | LastName | Nom | Nom du prospect |
| leadRole | LeadRole | Rôle | Le rôle du responsable dans sa société |
| leadScore | LeadScore | Évaluation des leads | Score entier attribué à l’prospect par les campagnes et programmes de notation |
| leadSource | LeadSource | Source du lead | Champ enregistrant la source de la piste |
| leadStatus | LeadStatus | Statut du lead | Champ enregistrant l’état actuel du marketing/des ventes du prospect |
| mainPhone | MainPhone | Téléphone principal | Numéro de téléphone Principal de la société du prospect |
| jigvisonContactId | ID de contact Marketo Data.com | ID de Marketo Data.com | Data.com ID de piste si disponible |
| jigvisonContactStatus | Statut de contact Marketo data.com | Statut Marketo Data.com | Statut Data.com du prospect si disponible |
| facebookDisplayName | MarketoSocialFacebookDisplayName | Nom d&#39;affichage Marketo pour Facebook | Nom d’affichage Facebook de la piste. Système renseigné lors de la connexion sociale |
| facebookId | MarketoSocialFacebookId | ID Marketo via Facebook | Identifiant Facebook du prospect. Système renseigné lors de la connexion sociale |
| facebookPhotoURL | MarketoSocialFacebookPhotoURL | URL de photo Marketo pour Facebook | URL de la photo de profil Facebook du prospect. Système renseigné lors de la connexion sociale |
| facebookProfileURL | MarketoSocialFacebookProfileURL | URL du profil Marketo pour Facebook | URL du profil Facebook du prospect. Système renseigné lors de la connexion sociale |
| facebookReach | MarketoSocialFacebookReach | Portée sociale Marketo via Facebook | Portée Facebook du prospect. Système renseigné lors de la connexion sociale |
| facebookReferredEnrollments | MarketoSocialFacebookReferredEnrollments | Inscriptions sur recommandation Marketo via Facebook | Nombre d’inscriptions référencées attribuées au prospect par le biais de Facebook. Gestion du système |
| facebookReferredVisits | MarketoSocialFacebookReferredVisits | Visites sur recommandation Marketo via Facebook | Nombre de visites référencées attribuées au prospect par le biais de Facebook. Gestion du système |
| gender | MarketoSocialGender | Sexe Marketo pour réseaux sociaux | Genre du prospect. Système renseigné lors de la connexion sociale |
| lastReferredEnrollment | MarketoSocialLastReferredEnrollment | Dernière inscription sur recommandation Marketo via les réseaux sociaux | Date de la dernière référence terminée. Gestion du système |
| lastReferredVisit | MarketoSocialLastReferredVisit | Dernière visite sur recommandation Marketo via les réseaux sociaux | Date de la dernière visite référencée. Gestion du système |
| linkedInDisplayName | MarketoSocialLinkedInDisplayName | Nom d&#39;affichage Marketo pour LinkedIn | Nom d’affichage LinkedIn de la piste. Système renseigné lors de la connexion sociale |
| linkedInId | MarketoSocialLinkedInId | ID Marketo via LinkedIn | Identifiant LinkedIn du prospect. Système renseigné lors de la connexion sociale |
| linkedInPhotoURL | MarketoSocialLinkedInPhotoURL | URL de la photo Marketo pour LinkedIn | URL de la photo LinkedIn de l’prospect. Système renseigné lors de la connexion sociale |
| linkedInProfileURL | MarketoSocialLinkedInProfileURL | URL du profil Marketo pour LinkedIn | Profil LinkedIn du prospect. Système renseigné lors de la connexion sociale |
| linkedInReach | MarketoSocialLinkedInReach | Portée sociale Marketo via LinkedIn | Portée LinkedIn de l’prospect. Système renseigné lors de la connexion sociale |
| linkedInReferredEnrollments | MarketoSocialLinkedInReferredEnrollments | Inscriptions sur recommandation Marketo via LinkedIn | Nombre d’inscriptions référencées attribuées au prospect par le biais de LinkedIn. Gestion du système |
| linkedInReferredVisits | MarketoSocialLinkedInReferredVisits | Visites sur recommandation Marketo via LinkedIn | Nombre de visites référencées attribuées au prospect par le biais de LinkedIn. Gestion du système |
| syndicationId |  - | ID association sociale Marketo | Identifiant social Marketo interne du prospect. Gestion du système |
| totalReferredEnrollments | MarketoSocialTotalReferredEnrollments | Total des inscriptions sur recommandation Marketo via les réseaux sociaux | Nombre total d&#39;inscriptions au parrainage terminées attribuées au prospect |
| totalReferredVisits | MarketoSocialTotalReferredVisits | Total des visites Marketo sur recommandation via les réseaux sociaux | Nombre total de visites référencées attribuées au prospect |
| twitterDisplayName | MarketoSocialTwitterDisplayName | Nom d&#39;affichage Marketo pour Twitter | Nom d’affichage du Twitter de la piste. Système renseigné lors de la connexion sociale |
| twitterId | MarketoSocialTwitterId | ID Marketo via Twitter | Identifiant de Twitter du prospect. Système renseigné lors de la connexion sociale |
| twitterPhotoURL | MarketoSocialTwitterPhotoURL | URL de la photo Marketo pour Twitter | URL de la photo de Twitter de l&#39;homme. Système renseigné lors de la connexion sociale |
| twitterProfileURL | MarketoSocialTwitterProfileURL | URL du profil Marketo pour Twitter | URL du profil de Twitter du prospect. Système renseigné lors de la connexion sociale |
| twitterReach | MarketoSocialTwitterReach | Portée sociale Marketo via Twitter | Portée du Twitter de la piste. Système renseigné lors de la connexion sociale |
| twitterReferredEnrollments | MarketoSocialTwitterReferredEnrollments | Inscriptions sur recommandation Marketo via Twitter | Nombre d’inscriptions référencées attribuées au prospect par Twitter. Gestion du système |
| twitterReferredVisits | MarketoSocialTwitterReferredVisits | Visites sur recommandation Marketo via Twitter | Nombre de visites référencées attribuées au prospect par Twitter. Gestion du système |
| middleName | MiddleName | Deuxième prénom | Nom intermédiaire du prospect |
| mobilePhone | MobilePhone | Numéro de téléphone mobile | Numéro de téléphone portable du prospect |
| numberOfEmployees | NumberOfEmployees | Nombre d&#39;employés | Nombre d’employés de la société de prospect |
| téléphone | Téléphone | Numéro de téléphone | Numéro de téléphone du prospect |
| postalCode | PostalCode | Code postal | Code postal du prospect |
| note | Classement | Évaluation du lead | Évaluation marketing/ventes du prospect |
| salutation | Titre | Titre | La formule de salutation préférée du prospect, à savoir Monsieur, Misses... etc. |
| sicCode | SICCode | Code SIC | Code de classification industrielle standard de la société du prospect |
| site | Site | Site |  |
| state | État | État | Etat du responsable |
| titre | Titre | Intitulé du poste | Fonction du responsable |
| désabonné | Désabonné | Désabonné | Statut de désinscription de l’email de l’prospect. Partiellement gérée par le système. Empêchera la réception d’emails non opérationnels s’ils sont définis sur true. |
| unsubscribedReason | UnsubscribedReason | Raison du désabonnement | Raison du statut de désabonnement du prospect. Partiellement gérée par le système. Renseigné avec les informations d’e-mail si le prospect est désabonné directement à partir d’un e-mail Marketo. |
| site web | Site web | Site web | URL du site web de la société de prospect |
| createdAt |  - | Créé à | Heure à laquelle l’enregistrement de piste a été initialement créé. Gestion du système |
| updatedAt |  - | Mis à jour à | Dernière mise à jour de l’enregistrement de piste. Gestion du système |
| emailInvalid |  - | E-mail non valide | Statut non valide du courrier électronique. Tous les emails vers l’adresse seront bloqués s’ils sont définis sur true. Les rebonds indiquant que l’email n’est pas valide définissent automatiquement ce champ sur true. |
| emailInvalidCause |  - | Cause de l&#39;e-mail non valide | Cause d’un état d’email non valide. Le message rebond à l’origine de l’envoi sera enregistré dans ce champ lorsque la valeur non valide de l’email est définie sur true. |
| inferredCity |  - | Ville déduite | Ville de piste déduite de la recherche IP inversée de la première visite web enregistrée de piste. |
| inferredMetropolitanArea |  - | Aire métropolitaine déduite | Zone métropolitaine du prospect déduite de la recherche IP inversée de la première visite web enregistrée du prospect. |
| inferredPhoneAreaCode |  - | Indicatif téléphonique local déduit | Code de zone de téléphone de l’piste déduit par la recherche IP inversée de la première visite web enregistrée de l’piste. |
| inferredPostalCode |  - | Code postal déduit | Code postal de l’prospect déduit par la recherche IP inversée de la première visite web enregistrée de l’prospect. |
| inferredStateRegion |  - | Région déduite | Région de l’état du prospect déduite de la recherche IP inversée de la première visite web enregistrée du prospect. |
| isAnonymous |  - | Est anonyme | État anonyme de l’enregistrement de piste. Gestion du système. |
| priorité |  - | Priorité | Priorité Insight sur les ventes du prospect. Gestion du système. |
| relativeScore |  - | Évaluation relative | Score relatif de l’insight commerciale du prospect. Gestion du système. |
| urgence |  - | Urgence | L&#39;urgence de l&#39;aperçu des ventes du prospect. Gestion du système. |
