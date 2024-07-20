---
title: Champs
feature: REST API, Field Management
description: Liste des noms de champ pris en charge.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 6%

---

# Champs

L’API REST et l’API SOAP utilisent différentes conventions de dénomination pour les champs de piste.

## Récupération de la liste des noms de champ

Récupérez la liste de tous les noms de champ pris en charge disponibles dans vos enregistrements de piste à l’aide du point de terminaison &quot;Décrire la piste&quot; REST.

## Où utiliser quel type de nom de champ ?

Il est parfois difficile de savoir quel type de nom de champ vous devez utiliser lors de l’utilisation d’une fonctionnalité spécifique liée à l’intégration. Vous trouverez ci-dessous une référence rapide pour laquelle les fonctionnalités utilisent des types de nom de champ REST ou SOAP.

| Fonctionnalité | Type de nom de champ à utiliser |
|--- |--- |
| API de suivi des pistes (Munchkin) | SOAP |
| API Forms 2.0 | SOAP |
| Import de liste (interface utilisateur) | SOAP |
| Import de liste (API REST) | REST |
| Mappages de réponse Webhook | SOAP |
| Script d’e-mail (Velocity) | SOAP |
| API SOAP | SOAP |
| API REST | REST |

### Pourquoi le champ d’API REST sfdcId renvoie-t-il toujours une valeur null ?

Le champ `sfdcId` est un champ de formule qui a été inclus par erreur dans la carte de champ d’origine de l’API REST. Les enregistrements récupérés via l’API REST ne calculent pas la valeur des champs de formule. Par conséquent, la valeur sera toujours nulle. Pour capturer l’identifiant SFDC réel, vous devez utiliser les champs appelés `sfdcLeadId` et `sfdcContactId`.
