---
title: Modèles de pages de destination
feature: REST API, Landing Pages
description: Gérez les modèles de page de destination Marketo via les points d’entrée de l’API REST pour les types de formulaires gratuits et guidés, la requête par identifiant ou nom, la création, la mise à jour d’HTML, le clone et Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
TQID: https://experienceleague.adobe.com/U9K1MG-q2gIgJMgfM3lt1S4olETt8ln9seOIKZUncBY
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 499
ht-degree: 2%

---

# Modèles de pages de destination

[Référence du point d’entrée du modèle de page de destination](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates)

Les modèles de page de destination sont des ressources parentes pour les pages de destination Marketo. Chaque page de destination tire sa structure de contenu initiale de son modèle parent.

## Types de modèles

Marketo fournit des modèles de page de destination guidés et de forme libre. Les modèles à structure libre offrent une expérience de modification vaguement structurée. Les modèles guidés peuvent limiter les types d’éléments et les emplacements au niveau du modèle.

Pour une comparaison détaillée, consultez la section [Comprendre les pages de destination de forme libre par rapport aux pages de destination guidées](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Requête

Interroger les modèles de landing page [par ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageTemplateByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageTemplateByNameUsingGET) ou par [navigation](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageTemplatesUsingGET). Ces points d’entrée renvoient des métadonnées de modèle. Récupérez le contenu HTML séparément pour chaque modèle par ID.

## Créer et mettre à jour

Les modèles sont créés en tant que ressources vides avec des métadonnées. Les paramètres `name` et `folder` sont requis. Les paramètres `description`, `templateType` et `enableMunchkin` sont facultatifs.

La valeur `templateType` peut être `freeform` ou `guided` et est définie par défaut sur `freeForm`. La valeur `enableMunchkin` par défaut est `false`. Lorsqu’elle est activée, elle empêche le suivi Munchkin sur les pages de destination enfants du modèle.

```http
POST /rest/asset/v1/landingPageTemplates.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=New LPT - PHP&folder={"id":12,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "11b7#14dfe1e3bcf",
    "result": [
        {
            "id": 286,
            "name": "assetAPITest",
            "description": "test",
            "createdAt": "2015-06-16T20:45:03Z+0000",
            "updatedAt": "2015-06-16T20:45:03Z+0000",
            "url": "https://app-devlocal1.marketo.com/#LT286B2ZN12",
            "folder": {
                "type": "Folder",
                "value": 12,
                "folderName": "Templates"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

Ajoutez du contenu de modèle séparément avec le point d’entrée [ Mettre à jour le contenu du modèle de page de destination ](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageTemplateContentUsingPOST).

### Mettre à jour les métadonnées

Utilisez le point d’entrée [Mettre à jour les métadonnées du modèle de page de destination](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLpTemplateUsingPOST) pour modifier le nom, la description ou le paramètre de `enableMunchkin`.

### Mettre à jour le contenu

La mise à jour du contenu du modèle remplace tout le contenu HTML existant. Transmettez le remplacement comme `multipart/form-data` dans le paramètre `content` .

```http
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```html
content-type: multipart/form-data; boundary=--------------------------435851813185237176536801
----------------------------435851813185237176536801
Content-Disposition: form-data; name="content"; filename="content.txt"
Content-Type: text/plain

<html>
<head>
</head>
<body>
<div>Placeholder Content</div>
</body>
</html>
----------------------------435851813185237176536801--
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e0dc60bbc",
  "result": [
    {
      "id": 286
    }
  ]
}
```

## Cloner

Clonez un modèle de page de destination avec une requête POST `application/x-www-url-formencoded`.

Le paramètre de chemin d’accès `id` spécifie le modèle de page de destination source.

Le paramètre `name` spécifie le nom du nouveau modèle de page de destination.

Le paramètre `folder` spécifie le dossier parent du nouveau modèle. Transmettez-le en tant qu’objet JSON incorporé contenant `id` et `type`.

Le paramètre facultatif `description` décrit le nouveau modèle.

```http
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Standard Template Clone&folder={"type": "Folder", "id": 732}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "dee6#1683e9fd410",
    "warnings": [],
    "result": [
        {
            "id": 61,
            "name": "Standard Template Clone",
            "createdAt": "2019-01-11T20:34:48Z+0000",
            "updatedAt": "2019-01-11T20:34:48Z+0000",
            "url": "https://app-abm.marketo.com/#LT61B2ZN732",
            "folder": {
                "type": "Folder",
                "value": 732,
                "folderName": "Test LP Template Clone"
            },
            "status": "draft",
            "workspace": "Default",
            "templateType": "freeForm",
            "enableMunchkin": true
        }
    ]
}
```

## Validation

Les modèles de page de destination utilisent le modèle brouillon et approuvé standard. Les mises à jour s’appliquent d’abord au brouillon et ne deviennent actives qu’une fois le modèle approuvé.

Avant approbation, un modèle doit répondre aux exigences de son type guidé ou de forme libre. Consultez ces ressources :

- [Modèles de page de destination de formulaire libre](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Modèles de page de destination guidés](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Exemples de modèles guidés](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Supprimer

Pour supprimer un modèle, assurez-vous qu’il n’est pas approuvé et qu’aucune page de destination enfant ne le référence. Vous ne pouvez pas utiliser cette API pour supprimer des modèles de page de destination avec des boutons de réseaux sociaux incorporés.
