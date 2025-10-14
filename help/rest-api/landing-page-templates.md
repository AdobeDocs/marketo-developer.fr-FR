---
title: Modèles de pages de destination
feature: REST API, Landing Pages
description: Gérez les modèles de page de destination Marketo via les points d’entrée de l’API REST pour les types de formulaires gratuits et guidés, la requête par identifiant ou nom, la création, la mise à jour d’HTML, le clone et Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 1%

---

# Modèles de pages de destination

[Référence du point d’entrée du modèle de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

Les modèles de page de destination sont une ressource parent et une dépendance pour les pages de destination Marketo individuelles. Les landing pages obtiennent le squelette de leur contenu à partir du modèle parent.

## Types de modèles

Marketo dispose de deux types de modèles de page de destination, à structure libre et guidés. Les modèles de page de destination à structure libre offrent une expérience de modification vaguement structurée pour les pages qui en sont dérivées. Les modèles guidés fournissent une expérience fortement structurée, où les types d’éléments et les emplacements peuvent être limités au niveau du modèle. Pour plus d’informations sur les différences, voir [ce document](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Requête

Les modèles de page de destination prennent en charge les types de requête standard pour les ressources [par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) et [navigation](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Ces points d’entrée renvoient des métadonnées pour les modèles. La récupération du contenu HTML des modèles doit être effectuée modèle par modèle via son identifiant .

## Créer et mettre à jour

Les modèles sont créés en tant que ressources vides avec les métadonnées associées. Lors de la création d’un modèle, un nom et un dossier doivent être inclus, ainsi qu’une description facultative, le paramètre templateType et enableMunchkin. templateType peut être à structure libre ou guidé et est défini par défaut sur freeForm. Pour connaître les différences entre les types, reportez-vous à la section Structure guidée ou Structure libre . enableMunchkin est défini par défaut sur false. Lorsqu’il est activé, il empêche l’exécution du suivi Munchkin sur les pages de destination enfants du modèle.

```
POST /rest/asset/v1/landingPageTemplates.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Le contenu du modèle doit être renseigné séparément via le point d’entrée [Mettre à jour le contenu du modèle de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST).

### Mettre à jour les métadonnées

Les métadonnées des modèles de page de destination peuvent être mises à jour via le point d’entrée [Mettre à jour les métadonnées du modèle de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST). Le nom, la description et le paramètre enableMunchkin peuvent être mis à jour de cette manière.

### Mettre à jour le contenu

Le contenu des modèles de page de destination est transformé en une mise à jour destructrice de l’intégralité du contenu HTML. Le contenu doit être transmis en tant que données multipart/form, le seul paramètre étant nommé content.

```
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```
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

```
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

Marketo offre une méthode simple pour cloner des modèles de page de destination. Il s’agit d’une requête POST codée en application/x-www-url-formencoded.

Le paramètre de chemin d’accès `id` spécifie l’identifiant du modèle de page de destination source à cloner.

Le paramètre `name` est utilisé pour spécifier le nom du nouveau modèle de page de destination.

Le paramètre `folder` est utilisé pour spécifier le dossier parent dans lequel le nouveau modèle de page de destination résidera. Il se présente sous la forme d’un objet JSON incorporé contenant  `id` et `type`.

Le paramètre `description` facultatif est utilisé pour décrire le nouveau modèle de page de destination.

```
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Les modèles de page de destination suivent le modèle standard approuvé pour le brouillon, où il peut y avoir un brouillon et/ou une version approuvée. Chaque fois que des mises à jour sont appliquées à un modèle, elles sont toujours appliquées en premier au brouillon et ne sont affichées en direct que lorsque le modèle a été approuvé.

Pour qu’un modèle soit approuvé, il doit être conforme aux règles de son type, guidé ou libre. Pour plus d’informations sur les conditions requises pour la création et la validation de modèles de leurs types respectifs, consultez leurs documents de création respectifs :

- [Modèles de page de destination de formulaire libre](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Modèles Guidés De Landing Page](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [&#x200B; Exemples de modèles guidés &#x200B;](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Supprimer

Pour supprimer un modèle, il doit être obsolète et non approuvé, ce qui signifie qu’aucune page de destination enfant ne peut y faire référence.  Les modèles de page de destination avec des boutons sociaux incorporés ne peuvent pas être supprimés avec cette API.
