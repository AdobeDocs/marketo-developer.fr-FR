---
title: Codes d’erreur
feature: SOAP
description: Codes d’erreur pour les appels SOAP
exl-id: 71796520-7bd6-4a37-94e7-b073d17df06f
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 11%

---

# Codes d’erreur

Lors du développement pour Marketo, il est très important que les requêtes et les réponses soient consignées lorsqu’une exception inattendue est rencontrée.  Bien que certains types d’exceptions, telles que l’authentification expirée, puissent être gérés en toute sécurité par la réauthentification, d’autres peuvent nécessiter des interactions de prise en charge, et les demandes et réponses seront toujours demandées dans ce scénario.

Vous trouverez ci-dessous une liste des codes d’erreur de SOAP API.

| Code | Message | Notes |
|--- |--- |--- |
| 10001 | Erreur interne | Grave défaillance du système |
| 20011 | Erreur interne | Échec du service API |
| 20012 | Requête non comprise | Message SOAP inattendu |
| 20013 | Accès refusé | Le client est bloqué de l’accès aux API |
| 20014 | Échec de l&#39;identification | Le client n’a pas fourni d’informations d’identification valides |
| 20015 | Limite de requête dépassée | Le nombre d&#39;appels aujourd&#39;hui a dépassé le quota de l&#39;abonnement. Le quota d&#39;abonnements par défaut est de 10 000 par jour. |
| 20016 | Requête expirée | La signature de la demande est trop ancienne. L’horodatage et la signature de requête donnés se trouvent dans le passé et ne sont plus valides. La requête peut être relancée avec un horodatage et une signature nouvellement générés. |
| 20017 | Requête non valide | Un paramètre attendu est manquant dans la requête. |
| 20019 | Opération non prise en charge. | L’opération appelée n’est pas définie dans le WSDL de l’API Marketo |
| 20022 | La période spécifiée dans le filtre de requête a dépassé la limite | Le nombre de jours écoulés entre les champs &quot;plus ancienMisÀJour&quot; et &quot;dernierMisÀJourÀ&quot; est supérieur à 30. |
| 20023 | Limite de taux dépassée | Le nombre d’appels des 20 dernières secondes était supérieur à 100 |
| 20024 | Limite de simultanéité dépassée | Le nombre d’appels simultanés était supérieur à 10. |
| 20101 | Clé de piste requise | LeadKey est requis mais n’a pas été fourni |
| 20102 | Clé de piste incorrecte | LeadKeyType n’est pas valide |
| 20103 | Piste introuvable | La valeur LeadKey ne correspondait à aucun prospect |
| 20104 | Détails de piste requis | LeadRecord est requis mais n’a pas été fourni |
| 20105 | Attribut de piste incorrect | LeadRecord contient un attribut dont le nom est incorrect |
| 20106 | Échec de la synchronisation des pistes | LeadRecord n’a pas pu être mis à jour ou créé. |
| 20107 | Clé d’activité incorrecte | LeadActivityFilter contient un type d’activité incorrect. |
| 20108 | Propriétaire de piste introuvable | LeadKey spécifie un propriétaire de piste qui n’existe pas |
| 20109 | Paramètre requis | La valeur du paramètre était nulle ou manquante |
| 20110 | Mauvais paramètre | Une valeur de paramètre est incorrecte |
| 20111 | Liste introuvable | ListKey spécifie une liste qui n’existe pas |
| 20113 | Campagne introuvable | La campagne n’existe pas |
| 20114 | Mauvais paramètre | La valeur du paramètre est incorrecte |
| 20122 | Mauvaise position de diffusion | La position du flux est mauvaise. |
| 20123 | Diffusion en continu à la fin | La position du flux indique qu’aucun autre enregistrement n’est disponible |
