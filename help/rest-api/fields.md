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
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 194
ht-degree: 6%

---

# Champs

L’API REST et l’API SOAP utilisent des conventions de nommage différentes pour les champs de prospect. Utilisez la convention de nom de champ requise par chaque fonction d’intégration.

## Récupération de la liste des noms de champ

Utilisez le point d’entrée REST « Décrire le lead » pour récupérer tous les noms de champ pris en charge pour les enregistrements de lead.

## Où utiliser quel type de nom de champ ?

Le type de nom de champ requis dépend de la fonction d’intégration. Le tableau suivant indique si chaque fonctionnalité utilise des noms de champ REST ou SOAP.

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

Le champ `sfdcId` est un champ de formule qui a été inclus dans le mappage de champs d’origine pour l’API REST. Les enregistrements récupérés via l’API REST ne calculent pas les valeurs des champs de formule. Par conséquent, `sfdcId` renvoie toujours la valeur null.

Pour récupérer l’identifiant SFDC, utilisez les champs `sfdcLeadId` et `sfdcContactId` .
