---
title: Pages de destination
feature: REST API, Landing Pages
description: Requête sur les landing pages dans Marketo.
exl-id: 2f986fb0-0a6b-469f-b199-1c526cd5a882
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 2%

---

# Pages de destination

[Référence du point de terminaison de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages)

Les landing pages sont des pages web hébergées par Marketo.

## Requête

Comme la plupart des autres ressources, les pages d’entrée peuvent être interrogées [ par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByNameUsingGET), [ par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByIdUsingGET) et par [navigation](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/browseLandingPagesUsingGET). Ces requêtes renvoient uniquement des métadonnées. La liste des sections de contenu d’une landing page doit être interrogée séparément par l’identifiant de la landing page.

La requête sur le contenu de la landing page renvoie une liste des sections de contenu disponibles dans la landing page. Pour mettre à jour le contenu, une section doit être présente dans la liste de contenu d’une page :

```
GET /rest/asset/v1/landingPage/{id}/content.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154ea1689d7",
    "result": [
        {
            "id": "67",
            "type": "Form",
            "index": 1,
            "content": {
                "content": "189",
                "contentType": "Form",
                "contentUrl": "https://app-devlocal1.marketo.com/#FO189A1ZN13LA1"
            },
            "formattingOptions": {
                "zIndex": 15,
                "left": "359px",
                "top": "122px"
            }
        }
    ]
}
```

Les résultats diffèrent entre les modèles de formulaire guidé et les modèles de formulaire libre, car les landing pages guidées contiennent un ensemble de sections définies par le modèle à partir duquel elles sont dérivées, tandis que les pages de formulaire libre ne contiennent pas de sections prédéfinies et que leur contenu doit être ajouté avant modification.  Notez que le format de l’attribut &quot;content&quot; peut varier en fonction de l’attribut &quot;type&quot; et que le champ est statique ou dynamique.

## Créer et mettre à jour

[Les landing pages sont créées](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/createLandingPageUsingPOST) en renvoyant à un modèle. Les seuls champs obligatoires pour la création sont le nom, le modèle (l’identifiant du modèle) et le dossier dans lequel placer la page. Pour obtenir des métadonnées supplémentaires qui peuvent être renseignées, reportez-vous à la référence du point de fin .

Les types de contenu valides pour les points de terminaison [contenu de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content) sont les suivants : richText, HTML, Form, Image, Rectangle, Extrait de code.

```
POST rest/asset/v1/landingPages.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=createLandingPage&folder={"type": "Folder", "id": 11}&template=1&description=this is a test&workspace=default&title=test create&keywords=awesome&formPrefill=false
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#154cf7922c6",
    "result": [
        {
            "id": 27,
            "name": "createLandingPage",
            "description": "this is a test",
            "createdAt": "2016-05-20T18:41:43Z+0000",
            "updatedAt": "2016-05-20T18:41:43Z+0000",
            "folder": {
                "type": "Folder",
                "value": 11,
                "folderName": "Landing Pages"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 1,
            "title": "test create",
            "keywords": "awesome",
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "https://app-devlocal1.marketo.com/lp/622-LME-718/createLandingPage.html",
            "computedUrl": "https://app-devlocal1.marketo.com/#LP27B2"
        }
    ]
}
```

Les métadonnées de page d’entrée peuvent être mises à jour avec le point d’entrée [Mettre à jour les métadonnées de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/updateLandingPageUsingPOST).

## Validation

Les pages d’entrée suivent le modèle standard approuvé par les brouillons, où il peut y avoir une version préliminaire et/ou une version approuvée. Chaque fois que des mises à jour sont appliquées à une page, elles sont toujours appliquées en premier à la version préliminaire et ne sont visibles en direct que lorsque la page a été approuvée.

## Supprimer

Pour supprimer une landing page, elle doit d’abord être inutilisable et ne pas être référencée par d’autres ressources Marketo, et ne pas être approuvée. Les pages sont supprimées individuellement avec le point d’entrée [Supprimer la page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST). Il n’est pas possible de supprimer des landing pages avec des boutons sociaux incorporés via cette API. 

## Cloner

Marketo fournit une méthode simple pour le clonage d’une page d’entrée. Il s’agit d’une demande de POST codée au format application/x-www-url.

Le paramètre de chemin `id` spécifie l’identifiant de la page d’entrée source à cloner.

Le paramètre `name` est utilisé pour spécifier le nom de la nouvelle Landing Page.

Le paramètre `folder` est utilisé pour spécifier le dossier parent dans lequel la nouvelle page d’entrée est créée. Il s’agit d’un objet JSON incorporé contenant `id` et `type`.

Le paramètre `template` est utilisé pour spécifier l’identifiant du modèle de page d’entrée source.

Le paramètre facultatif `description` est utilisé pour décrire la nouvelle page d’entrée.

```
POST /rest/asset/v1/landingPage/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MyNewLandingPage&folder={"type":"Program","id":1119}&template=57
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1078d#1683e4881c6",
    "warnings": [],
    "result": [
        {
            "id": 3291,
            "name": "MyNewLandingPage",
            "createdAt": "2019-01-11T18:59:25Z+0000",
            "updatedAt": "2019-01-11T18:59:25Z+0000",
            "folder": {
                "type": "Program",
                "value": 1119,
                "folderName": "DefaultProgramWithGuidedLP"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 57,
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "http://na-abm.marketo.com/lp/284-RPR-133/DefaultProgramWithGuidedLPPerkutoTestLP-Clone-1.html",
            "computedUrl": "https://app-abm.marketo.com/#LP3291A1LA1"
        }
    ]
}
```

## Gestion de la section

Les sections de contenu sont classées selon leur propriété d’index et mises en page selon les règles CSS appliquées lors de l’affichage par le client. Les sections de contenu sont incluses et gérées avec les points de terminaison de section de contenu [Add](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Update](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) et [Delete](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) correspondants, et peuvent être interrogées à l’aide de [Get Landing Page Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Chaque section comporte un type et un paramètre de valeur. Le type détermine ce qui doit être placé dans la valeur.  Pour ces points de terminaison, les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

**Types de section**

| Type | Valeur |
|--- |--- |
| DynamicContent | ID de la segmentation. |
| Formulaire | ID du formulaire. |
| HTML | Contenu de l’HTML texte. |
| Image | ID de la ressource image. |
| Rectangle | Vide. |
| RichText | Contenu de l’HTML texte.  Ne peut contenir que des éléments de texte enrichi. |
| Extrait | Identifiant du fragment de code. |
| SocialButton | L’identifiant de  le bouton social . |
| Vidéo | Identifiant de la vidéo. |

Pour les pages de formulaire libre, toutes les sections de contenu souhaitées doivent être ajoutées et seront incorporées dans l’élément div avec l’identifiant `mktoContent`. Pour les pages guidées, une liste d’éléments prédéfinis peut être présente dans la liste à partir du point d’entrée [Obtenir le contenu de la page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). D’autres éléments peuvent être ajoutés ou leur [contenu mis à jour](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) via leurs points de terminaison respectifs.

### Contenu dynamique

Pour créer une section Contenu dynamique , elle doit déjà être présente dans la liste de contenu de la landing page. Le point d’entrée [Mettre à jour le contenu de la page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) doit ensuite être utilisé pour définir le type sur &quot;Contenu dynamique&quot;. Lorsqu’une section est définie sur du contenu dynamique, elle crée des sections dynamiques sous-jacentes dans la section de contenu qui héritent toutes du type de base de l’élément converti. Chaque section dynamique hérite également du contenu de la section convertie.

```
GET /rest/asset/v1/landingPage/{id}/dynamicContent/RVMtNDg=.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "46e#1560fa169d9",
  "result": [
    {
      "createdAt": "2016-07-21",
      "updatedAt": "2016-07-21",
      "segmentation": 1007,
      "segments": [
        {
          "segmentId": 1018,
          "segmentName": "Default",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        },
        {
          "segmentId": 1017,
          "segmentName": "New Segment",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        }
      ]
    }
  ]
}
```

[La mise à jour du contenu](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageDynamicContentUsingPOST) pour chaque segment individuel est effectuée sur la base de l’identifiant du segment.

```
POST /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
segment=New Segment&value=New Content
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e08fe7cbbc",
  "result": [
    {
      "id": 1012
    }
  ]
}
```

## Variables

L’une des fonctionnalités introduites dans les landing pages guidées est les variables modifiables.  Les variables contiennent des valeurs pour les éléments d’une landing page.  Les variables peuvent facilement être modifiées à l’aide de l’éditeur de page d’entrée, comme illustré ci-dessous :

![Variables de page d’entrée](assets/landing-page-variables.png)

Les variables sont définies comme des balises META dans l’élément `<head>` d’un modèle de page d’entrée en mode guidé. Trois types de variables sont disponibles : chaîne, couleur et booléen.  Voici un exemple de trois définitions de variable :

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Pour plus d’informations, voir la section &quot;Variable modifiable&quot; de la documentation [Création d’un modèle de page d’entrée guidée](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template) .

### Requête

Récupérez les variables d’une page d’entrée guidée en transmettant l’identifiant de page d’entrée au point de terminaison Obtenir les variables de page d’entrée .

```
GET /rest/asset/v1/landingPage/{id}/variables.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "10843#15a6d7e5fa1",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello World!",
            "type": "string"
        },
        {
            "id": "colorVar",
            "value": "#FFFFFF",
            "type": "color"
        },
        {
            "id": "boolVar",
            "value": "true",
            "type": "boolean"
        }
    ]
}
```

Dans  dans cet exemple, la page d’entrée guidée contient 3 variables : stringVar, colorVar, boolVar.

### Mise à jour

Mettez à jour une variable pour une page d’entrée guidée en transmettant l’identifiant de page d’entrée, l’identifiant de variable et la valeur de variable au point de terminaison Mettre à jour les variables de page d’entrée .

```
POST /rest/asset/v1/landingPage/{id}/variable/{variableId}.json?value={newValue}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2b07#15a6db77da3",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello Brave New World!",
            "type": "String"
        }
    ]
}
```

## Aperçu de la page d’entrée

Marketo fournit le point de terminaison [Obtenir le contenu complet de la page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) pour récupérer un aperçu en direct d’une page d’entrée telle qu’elle serait rendue dans un navigateur. Il existe un paramètre obligatoire, le paramètre de chemin `id` qui est l’identifiant de la page d’entrée que vous souhaitez prévisualiser. Il existe deux autres paramètres de requête facultatifs :

- segmentation : accepte un tableau d’objets JSON contenant des attributs segmentationId et segmentId . Une fois défini, prévisualise la page d’entrée comme si vous étiez une piste correspondant à ces segments.
- leadId :  Accepte l’identifiant entier d’une piste. Lorsqu’elle est définie, la landing page est prévisualisée comme si elle avait été vue par la piste désignée.

```
GET /rest/asset/v1/landingPage/{id}/fullContent.json?leadId=1001&segmentation=[{"segmentationId":1030,"segmentId":1103}]
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "119ab#17692849f1e",
  "warnings": [],
  "result": [
    {
      "id": 1023,
      "content": "<!DOCTYPE html>\n<html>\n <head>\n <meta charset=\"utf-8\">\n \n \n <meta name=\"robots\" content=\"index, nofollow\">\n <title></title>\n <style>\n body {background:#FFFFFF} \n #myConditionalDisplayArea {\n display: true;\n }\n </style>\n <link rel=\"shortcut icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n<link rel=\"icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n\n\n<style>.mktoGen.mktoImg {display:inline-block; line-height:0;}</style>\n </head>\n <body id=\"bodyId\">\n \n Hello Brave New World!\n <div class=\"mktoText\" id=\"exampleText\"><div>This is an example editable text area.</div>\n<div>Lead Full Name = Hanna Crawford</div>\n<div><br /></div>\n <script type=\"text/javascript\" src=\"//munchkin.marketo.net//munchkin.js\"></script><script>Munchkin.init('123-ABC-456', {customName: 'Test-Landing-Page-APIs_Guided-Landing-Page---deverly', PURL_VISIT_TOKEN, wsInfo: 'j1RR'});</script>\n<div id=\"mktoClickBlockingDiv\"></div>\n </body>\n</html>\n"
    }
  ]
}
```
