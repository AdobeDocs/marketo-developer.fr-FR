---
title: Champs
feature: REST API, Field Management
description: Découvrez le nommage des champs de prospect REST et SOAP, répertoriez les champs via REST, décrivez le prospect, le mappage des fonctionnalités, la raison pour laquelle sfdcId est nul et utilisez sfdcLeadId ou sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
TQID: https://experienceleague.adobe.com/H2Bvhy-67U8JJ1V3JwYJ0O0vj4i11fwUCyYQtjxm8u0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 213
ht-degree: 6%

---

# Champs

L’API REST et l’API SOAP utilisent des conventions de nommage différentes pour les champs de prospect.

## Récupération de la liste des noms de champ

Récupérez la liste de tous les noms de champ pris en charge disponibles sur vos enregistrements de prospect à l’aide du point d’entrée REST « Décrire le prospect ».

## Où utiliser quel type de nom de champ ?

Il est parfois difficile de savoir quel type de nom de champ vous devez utiliser lors de l’utilisation d’une fonctionnalité spécifique liée à l’intégration. Voici une référence rapide pour les fonctionnalités qui utilisent des types de nom de champ REST ou SOAP.

| Fonctionnalité | Type de nom de champ à utiliser |
| --- | --- |
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
