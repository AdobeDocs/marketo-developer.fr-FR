---
title: Contenu dynamique
feature: REST API, Dynamic Content
description: Configurez du contenu dynamique Marketo au niveau de la section via des API REST à l'aide de segmentations pour personnaliser les e-mails, les landing pages et les fragments de code avec des points d'entrée et des exemples
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 2%

---

# Contenu dynamique

Marketo facilite l’utilisation du contenu dynamique par le biais d’une segmentation de piste sur plusieurs types de ressources :

- E-mails
- Pages de destination
- Extraits

## Vue d’ensemble

Le contenu dynamique est implémenté au niveau de la section, en désignant des variations spécifiques d’une section à diffuser à un prospect en fonction de leur qualification dans un segment au sein d’une segmentation choisie. Si un élément de contenu est configuré pour diffuser du contenu dynamique en fonction d’une certaine segmentation, un prospect constatant que ce contenu est diffusé présente la variation de contenu correspondant au segment dans lequel il se trouve, ou le contenu par défaut, s’il n’est pas éligible à un segment.

## Exemple

Prenons un exemple d’e-mail dans lequel nous avons une segmentation Région (États-Unis) et voulons afficher une promotion d’événement uniquement pour les prospects du segment Sud-Ouest, qui inclut les prospects de Californie, du Nevada, de l’Utah, du Colorado, de l’Arizona et du Nouveau-Mexique. Pour ce faire, nous transformons une section modifiable dans notre e-mail avec l’identifiant « Q1-promotion-banner » en section Contenu dynamique. Pour ce faire, nous devons utiliser le point d’entrée [Mettre à jour le contenu de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) pour notre e-mail. Le paramètre `value` est utilisé pour spécifier l’identifiant de la segmentation.

Remarque : les e-mails et les pages de destination suivent ce modèle. Les fragments de code ont un modèle différent, détaillé dans la documentation de l’API Fragments de code .

L’exemple suivant définit la section sur une section Contenu dynamique, segmentée par segmentation 1001.

```
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```
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

Pour ajouter du contenu pour des segments individuels, nous devons appeler le point d’entrée [Mettre à jour le contenu dynamique d’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailDynamicContentUsingPOST) pour la section spécifique.

L’exemple suivant montre comment définir la section afin d’afficher notre image de bannière spéciale pour les prospects du segment Sud-Ouest au lieu de la valeur par défaut. Si nous voulions créer d’autres variations pour d’autres segments, nous appellerions de nouveau ce point d’entrée pour chaque segment et section.

```
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```
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

La segmentation est le cœur du contenu dynamique de Marketo. Une segmentation est une liste définie par l’utilisateur d’ensembles individuels de règles qui sont évaluées de haut en bas par rapport à l’ensemble de la base de données de prospects. Un prospect ne peut être membre que d’un segment dans chaque segmentation et sera membre du premier qu’il qualifie pour dans chaque segmentation. S’il n’est pas qualifié pour un segment, il sera membre du segment par défaut et recevra le contenu par défaut pour tout élément de contenu dynamique donné utilisant cette segmentation.

### Liste

Les segmentations ont un point d’entrée de liste qui renvoie une réponse avec une liste de segmentations disponibles.

```
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

Les segmentations ont également un point d’entrée qui renvoie une réponse avec une liste de segments d’une segmentation parente.

```
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
