---
title: Codes d’erreur
feature: SOAP
description: Guide de référence des codes d’erreur de l’API Marketo SOAP avec des messages et des notes, couvrant les échecs d’authentification, les limites de taux et de simultanéité et les problèmes de requête.
exl-id: 71796520-7bd6-4a37-94e7-b073d17df06f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 11%

---

# Codes d’erreur

Lors du développement pour Marketo, il est très important que les requêtes et les réponses soient consignées lorsqu’une exception inattendue est rencontrée.  Bien que certains types d’exceptions, telles que l’authentification expirée, puissent être gérés en toute sécurité par la réauthentification, d’autres peuvent nécessiter des interactions d’assistance. Les requêtes et les réponses seront toujours demandées dans ce scénario.

Vous trouverez ci-dessous une liste des codes d’erreur de l’API SOAP.

| Code | Message | Notes |
|--- |--- |--- |
| 10001 | Erreur interne | Défaillance grave du système |
| 20011 | Erreur interne | Échec du service d’API |
| 20012 | Demande Non Comprise | Message SOAP inattendu |
| 20013 | Accès refusé | Le client est bloqué de l’accès à l’API |
| 20014 | Échec de l’authentification | Le client n&#39;a pas fourni d&#39;informations d&#39;identification valides |
| 20015 | Limite de demande dépassée | Le nombre d&#39;appels aujourd&#39;hui a dépassé le quota d&#39;abonnement. Le quota d’abonnements par défaut est de 10 000 par jour. |
| 20016 | Demande expirée | La signature de la demande est trop ancienne. L’horodatage et la signature de la demande donnés sont dans le passé et ne sont plus valides. La demande peut être réessayée avec un horodatage et une signature nouvellement générés. |
| 20017 | Requête non valide | Il manque un paramètre attendu à la requête |
| 20019 | Opération non prise en charge. | L’opération appelée n’est pas définie dans le WSDL de l’API Marketo |
| 20022 | La période spécifiée dans le filtre de requête a dépassé la limite. | Le nombre de jours écoulés entre les champs « oldUpdatedAt » et « latestUpdatedAt » était supérieur à 30 |
| 20023 | Limite de taux dépassée | Le nombre d’appels au cours des 20 dernières secondes était supérieur à 100 |
| 20024 | Limite de simultanéité dépassée | Le nombre d’appels simultanés était supérieur à 10 |
| 20101 | Clé de lead requise | LeadKey est requis mais n’a pas été fourni |
| 20102 | Code de lead incorrect | LeadKeyType non valide |
| 20103 | Lead introuvable | La valeur de LeadKey ne correspond à aucun lead |
| 20104 | Détails du lead requis | LeadRecord requis mais non fourni |
| 20105 | Attribut de lead incorrect | LeadRecord contient un attribut dont le nom est incorrect |
| 20106 | Échec de la synchronisation des leads | Impossible de mettre à jour ou de créer l’enregistrement de lead |
| 20107 | Clé d’activité incorrecte | LeadActivityFilter contient un type d’activité incorrect |
| 20108 | Propriétaire du lead introuvable | LeadKey indique un propriétaire de lead qui n’existe pas |
| 20109 | Paramètre Obligatoire | La valeur du paramètre était nulle ou manquante |
| 20110 | Paramètre incorrect | Une valeur de paramètre est incorrecte |
| 20111 | Liste introuvable | ListKey spécifie une liste qui n&#39;existe pas |
| 20113 | Campagne Introuvable | La campagne n’existe pas |
| 20114 | Paramètre incorrect | Valeur de paramètre incorrecte |
| 20122 | Mauvaise Position Du Flux | La position du flux est mauvaise. |
| 20123 | Flux à la fin | La position du flux indique qu’aucun autre enregistrement n’est disponible |
