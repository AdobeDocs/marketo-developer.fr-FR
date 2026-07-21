---
title: Contenu dynamique
feature: REST API, Dynamic Content
description: Configurez du contenu dynamique Marketo au niveau de la section via des API REST à l'aide de segmentations pour personnaliser les e-mails, les landing pages et les fragments de code avec des points d'entrée et des exemples
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
TQID: https://experienceleague.adobe.com/MwfPxu74qk0bPZMr6yuxQi--e3gMvP1tXQZ5iMil02o
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 329
ht-degree: 3%

---

# Contenu dynamique

Utilisez les segmentations de leads pour fournir du contenu dynamique dans ces types de ressources :

- E-mails
- Pages de destination
- Extraits

## Vue d’ensemble

Le contenu dynamique fonctionne au niveau de la section. Chaque section peut fournir des variations pour les segments dans une segmentation sélectionnée.

Lorsqu’un prospect consulte la ressource, Marketo affiche la variation du segment du prospect. Si le prospect ne répond pas aux critères d’un segment, Marketo affiche le contenu par défaut.

## Exemple

Cet exemple utilise une segmentation Région (États-Unis) pour afficher une promotion d’événement aux prospects du segment Sud-Ouest. Le segment comprend des prospects de Californie, du Nevada, de l’Utah, du Colorado, de l’Arizona et du Nouveau-Mexique.

Utilisez le point d’entrée [Mettre à jour le contenu de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailComponentContentUsingPOST) pour transformer la section modifiable avec l’ID `Q1-promotion-banner` en section `DynamicContent`. Le paramètre `value` spécifie l’identifiant de segmentation.

Les e-mails et les landing pages suivent ce modèle. Les fragments de code utilisent le modèle différent décrit dans la documentation de l’API Fragments de code .

L’exemple suivant définit la section sur une section Contenu dynamique, segmentée par segmentation 1001.

```http
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```text
type=DynamicContent&value=1001
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1909
    }
  ]
}
```

Appelez le point d’entrée [Mettre à jour le contenu dynamique d’e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailDynamicContentUsingPOST) pour ajouter du contenu pour un segment dans une section spécifique.

La requête suivante affiche une bannière spéciale au lieu du contenu par défaut pour les prospects du segment Sud-Ouest. Pour créer d’autres variations, appelez le point d’entrée pour chaque segment et section.

```http
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```text
segment=Southwest&type=HTML&value=<img src='//www.example.com/SuperSpecialBannerForAmericanSouthwestLeads.jpg'/>
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1637
    }
  ]
}
```

## Segmentation

Une segmentation est une liste définie par l’utilisateur d’ensembles de règles que Marketo évalue de haut en bas par rapport à la base de données de prospect. Un prospect ne peut appartenir qu’à un seul segment dans chaque segmentation. Le prospect rejoint le premier segment pour lequel il remplit les conditions.

Si le prospect ne remplit pas les critères d’un autre segment, il rejoint le segment par défaut et reçoit le contenu par défaut de la segmentation.

### Liste

Utilisez le point d’entrée de liste pour récupérer les segmentations disponibles.

```http
GET /rest/asset/v1/segmentation.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "78eb#14e9de95868",
  "result": [
    {
      "id": 1001,
      "name": "My Industry Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:10Z+0000",
      "url": "https://app-abm.marketo.com/#SG1001A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    },
    {
      "id": 1002,
      "name": "My Country Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:28:23Z+0000",
      "updatedAt": "2015-04-06T18:37:18Z+0000",
      "url": "https://app-abm.marketo.com/#SG1002A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    }
  ]
}
```

Utilisez le point d’entrée des segments pour récupérer les segments dans une segmentation parent.

```http
GET /rest/asset/v1/segmentation/1001/segments.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "2031#14e9df08796",
  "result": [
    {
      "id": 1001,
      "name": "Manufacturing",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1002,
      "name": "Healthcare",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769688A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1003,
      "name": "Financial",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769690A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1004,
      "name": "Technology",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769692A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1005,
      "name": "Default",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769694A1",
      "status": "approved",
      "segmentationId": 1001
    }
  ]
}
```
