---
title: Modèles de pages de destination
feature: REST API, Landing Pages
description: Créez et modifiez des modèles de landing page.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 1%

---

# Modèles de pages de destination

[Référence du point de terminaison du modèle de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

Les modèles de page d’entrée sont une ressource parent et une dépendance pour des pages d’entrée Marketo individuelles. Les landing pages obtiennent le squelette de leur contenu du modèle parent.

## Types de modèle

Marketo dispose de deux types de modèles de page d’entrée : libre et guidé. Les modèles de page d’entrée de formulaire libre offrent une expérience de modification structurée pour les pages qui en sont dérivées. Les modèles guidés offrent une expérience fortement structurée, où les types et emplacements d’éléments peuvent être restreints au niveau du modèle. Pour plus d&#39;informations sur les différences, consultez [ce document](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Requête

Les modèles de page d’entrée prennent en charge les types de requête standard pour les ressources [ by id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [ by name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) et [browsing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Ces points de fin renvoient des métadonnées pour les modèles. La récupération du contenu HTML des modèles doit être effectuée par modèle à partir de son identifiant.

## Créer et mettre à jour

Les modèles sont créés en tant que ressources vides avec les métadonnées associées. Lors de la création d’un modèle, un nom et un dossier doivent être inclus, ainsi qu’une description, un templateType et un paramètre enableMunchkin facultatifs. templateType peut être à structure libre ou guidé et la valeur par défaut freeForm est . Pour connaître les différences entre les types, reportez-vous à la section Formulaire guidé et formulaire libre . La valeur par défaut de enableMunchkin est false. Lorsqu’elle est activée, elle empêche l’exécution du suivi Munchkin sur n’importe quelle page d’entrée enfant du modèle.

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

Le contenu du modèle doit être renseigné séparément via le point de terminaison [Mettre à jour le contenu du modèle de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST) .

### Mettre à jour les métadonnées

Les métadonnées des modèles de landing page peuvent être mises à jour via le point de terminaison [ Mettre à jour les métadonnées des modèles de landing page](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST) . Le nom, la description et le paramètre enableMunchkin peuvent être mis à jour de cette manière.

### Mettre à jour le contenu

Le contenu des modèles de page d’entrée est une mise à jour destructrice de l’ensemble du contenu de l’HTML. Le contenu doit être transmis en multipart/form-data, le seul paramètre étant contenu nommé.

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

Marketo fournit une méthode simple pour le clonage de modèles de page d’entrée. Il s’agit d’une demande de POST codée au format application/x-www-url.

Le paramètre de chemin `id` spécifie l’identifiant du modèle de page d’entrée source à cloner.

Le paramètre `name` est utilisé pour spécifier le nom du nouveau modèle de page d’entrée.

Le paramètre `folder` est utilisé pour spécifier le dossier parent où réside le nouveau modèle de page d’entrée. Il s’agit d’un objet JSON incorporé contenant  `id` et `type`.

Le paramètre facultatif `description` est utilisé pour décrire le nouveau modèle de page d’entrée.

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

Les modèles de page d’entrée suivent le modèle approuvé par le brouillon standard, où il peut y avoir une version préliminaire et/ou une version approuvée. Chaque fois que des mises à jour sont appliquées à un modèle, elles sont toujours appliquées en premier à la version préliminaire et ne sont visibles en direct que lorsque le modèle a été validé.

Pour qu’un modèle soit validé, il doit être conforme aux règles de son type, soit guidé en forme libre. Pour plus d’informations sur les exigences relatives à la création et à la validation de modèles de leurs types respectifs, voir leurs documents de création respectifs :

- [Modèles de page d’entrée de formulaire gratuit](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Modèles de page d’entrée guidée](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Exemples de modèles guidés](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Supprimer

Pour supprimer un modèle, il doit être inutilisable et non approuvé, ce qui signifie qu’aucune page d’entrée enfant ne peut le référencer.  Il est possible que les modèles de page d’entrée contenant des boutons sociaux incorporés ne soient pas supprimés avec cette API.
