---
title: Champs
feature: REST API, Field Management
description: Découvrez le nommage des champs de prospect REST et SOAP, répertoriez les champs via REST, décrivez le prospect, le mappage des fonctionnalités, la raison pour laquelle sfdcId est nul et utilisez sfdcLeadId ou sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 6%

---

# Champs

L’API REST et l’API SOAP utilisent des conventions de nommage différentes pour les champs de prospect.

## Récupération de la liste des noms de champ

Récupérez la liste de tous les noms de champ pris en charge disponibles sur vos enregistrements de prospect à l’aide du point d’entrée REST « Décrire le prospect ».

## Où utiliser quel type de nom de champ ?

Il est parfois difficile de savoir quel type de nom de champ vous devez utiliser lors de l’utilisation d’une fonctionnalité spécifique liée à l’intégration. Voici une référence rapide pour les fonctionnalités qui utilisent des types de nom de champ REST ou SOAP.

| Fonctionnalité | Type de nom de champ à utiliser |
|--- |--- |
| API de suivi des leads (Munchkin) | SOAP |
| API Forms 2.0 | SOAP |
| Importation de liste (interface utilisateur) | SOAP |
| Import de liste (API REST) | REST |
| Mappages de réponse Webhook | SOAP |
| Script D’E-Mail (Velocity) | SOAP |
| API SOAP | SOAP |
| API REST | REST |

### Pourquoi le champ sfdcId de l’API REST renvoie-t-il toujours une valeur nulle ?

Le champ `sfdcId` est un champ de formule qui a été inclus par erreur dans le mappage de champs d’origine pour l’API REST. Les enregistrements récupérés via l’API REST ne calculent pas la valeur des champs de formule, la valeur sera donc toujours nulle. Pour capturer l’identifiant SFDC réel, vous devez utiliser les champs appelés `sfdcLeadId` et `sfdcContactId`.
