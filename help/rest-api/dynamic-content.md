---
title: "Contenu dynamique"
feature: REST API, Dynamic Content
description: "Configuration de contenu dynamique avec les API Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 2%

---


# Contenu dynamique

Marketo facilite l’utilisation du contenu dynamique par le biais de la segmentation des pistes sur plusieurs types de ressources :

- E-mails
- Pages de destination
- Extraits

## Vue d’ensemble

Le contenu dynamique est implémenté au niveau de la section, en désignant des variantes spécifiques d’une section à diffuser dans une piste en fonction de leur qualification dans un segment au sein d’une segmentation donnée. Si un élément de contenu est configuré pour diffuser du contenu dynamique basé sur une certaine segmentation, un prospect qui voit que le contenu est traité avec la variation de contenu correspondant au segment dans lequel il appartient, ou au contenu par défaut, s’il ne remplit pas les critères d’un segment.

## Exemple

Pour le démontrer, prenons un exemple d’email dans lequel nous avons une segmentation Région (États-Unis) et voulons afficher une promotion d’événement uniquement pour les prospects appartenant au segment Sud-Ouest, qui inclut les prospects Californie, Nevada, Utah, Colorado, Arizona et Nouveau-Mexique. Pour ce faire, nous allons créer une section modifiable dans notre email avec l’identifiant &quot;Q1-promotion-banner&quot; dans une section DynamicContent . Pour ce faire, nous devons utiliser la variable [Mettre à jour la section Contenu des emails](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) point de terminaison de notre email. La variable `value` sert à spécifier l’identifiant de la segmentation.

Remarque : Les emails et les landing pages suivent ce modèle. Les fragments de code ont un modèle différent, détaillé dans la documentation de l’API Fragments de code.

L’exemple suivant définit la section comme une section Contenu dynamique, segmentée par segmentation 1001.

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

Pour ajouter du contenu pour des segments individuels, nous devons appeler la méthode [Mettre à jour la section Contenu dynamique des emails](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailDynamicContentUsingPOST) point de terminaison pour la section spécifique.

L’exemple suivant définit la section de manière à afficher l’image de bannière spéciale pour les pistes du segment Sud-Ouest au lieu de la section par défaut. Si nous voulions créer plus de variations pour plus de segments, nous appellerions à nouveau ce point de terminaison pour chaque segment et section.

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

La segmentation est au coeur du contenu dynamique de Marketo. Une segmentation est une liste définie par l’utilisateur de différents ensembles de règles qui sont évalués de haut en bas par rapport à l’ensemble de la base de données de pistes. Une piste ne peut être membre que d’un seul segment dans chaque segmentation et sera membre du premier segment auquel elle est admissible dans chaque segmentation. S’il ne répond pas aux critères d’un segment, il sera membre du segment par défaut et recevra le contenu par défaut pour tout élément de contenu dynamique donné utilisant cette segmentation.

### Liste

Les segments ont un point de terminaison list qui renvoie une réponse avec une liste de segments disponibles.

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

Les segments comportent également un point de terminaison qui renvoie une réponse avec une liste de segments d’une segmentation parente.

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
