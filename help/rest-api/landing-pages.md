---
title: Pages de destination
feature: REST API, Landing Pages
description: Utilisez l’API REST Marketo pour interroger des métadonnées et du contenu, créer, mettre à jour, approuver, supprimer et cloner des pages de destination, y compris des types de formulaires guidés et libres.
exl-id: 2f986fb0-0a6b-469f-b199-1c526cd5a882
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1018'
ht-degree: 2%

---

# Pages de destination

[Référence du point d’entrée de la page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages)

Les landing pages sont des pages web hébergées par Marketo.

## Requête

Comme la plupart des autres ressources, les pages de destination peuvent être interrogées [par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByNameUsingGET), [par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByIdUsingGET) et par [navigation](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/browseLandingPagesUsingGET). Ces requêtes ne renvoient que des métadonnées et la liste des sections de contenu d’une page de destination doit être interrogée séparément à l’aide de l’identifiant de la page de destination.

L’interrogation du contenu de la page de destination renvoie une liste de sections de contenu disponibles dans la page de destination. Une section doit être présente dans la liste de contenu d’une page pour mettre à jour le contenu :

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

Les résultats diffèrent entre les modèles guidés et les modèles de formulaire libre, car les pages de destination guidées sont fournies avec un ensemble de sections définies par le modèle dont elles sont dérivées, tandis que les pages de formulaire libre ne sont pas fournies avec des sections prédéfinies et leur contenu doit être ajouté avant la modification.  Notez que le format de l’attribut « content » peut varier en fonction de l’attribut « type » et selon que le champ est statique ou dynamique.

## Créer et mettre à jour

[Les landing pages sont créées](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/createLandingPageUsingPOST) en se référant à un modèle. Les seuls champs obligatoires pour la création sont le nom, le modèle (l’identifiant du modèle) et le dossier dans lequel placer la page. Pour obtenir des métadonnées supplémentaires qui peuvent être renseignées, consultez la référence du point d’entrée .

Les types de contenu valides pour les points d’entrée [contenu de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content) sont les suivants : texte enrichi, HTML, formulaire, image, rectangle, extrait de code.

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

Les métadonnées de page de destination peuvent être mises à jour avec le point d’entrée [Mettre à jour les métadonnées de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/updateLandingPageUsingPOST).

## Validation

Les landing pages suivent le modèle standard approuvé pour le brouillon, où il peut y avoir une version brouillon et/ou une version approuvée. Chaque fois que des mises à jour sont appliquées à une page, elles le sont d’abord à la version préliminaire, et ne sont affichées en direct que lorsque la page a été approuvée.

## Supprimer

Pour supprimer une page de destination, celle-ci doit d’abord être obsolète et non référencée par d’autres ressources Marketo, ainsi que non approuvée. Les pages sont supprimées individuellement avec le point d’entrée [Supprimer la page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST). Les landing pages avec des boutons sociaux incorporés ne peuvent pas être supprimées via cette API.

## Cloner

Marketo offre une méthode simple pour cloner une page de destination. Il s’agit d’une requête POST codée en application/x-www-url-formencoded.

Le paramètre de chemin d’accès `id` spécifie l’identifiant de la page de destination source à cloner.

Le paramètre `name` est utilisé pour spécifier le nom de la nouvelle page de destination.

Le paramètre `folder` est utilisé pour spécifier le dossier parent dans lequel la nouvelle page de destination est créée. Il se présente sous la forme d’un objet JSON incorporé contenant `id` et `type`.

Le paramètre `template` est utilisé pour spécifier l’identifiant source du modèle de page de destination.

Le paramètre `description` facultatif est utilisé pour décrire la nouvelle page de destination.

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

## Section Gérer le contenu

Les sections de contenu sont classées selon leur propriété d’index et finalement mises en page en fonction des règles CSS appliquées lorsqu’elles sont affichées par le client. Les sections de contenu sont incluses et gérées avec les points d’entrée [Ajouter](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Mettre à jour](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) et [Supprimer](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) correspondants de la section de contenu de la page de destination et peuvent être interrogées à l’aide de [Obtenir le contenu de la page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Chaque section comporte un paramètre de type et de valeur. Le type détermine ce qui doit être placé dans la valeur .  Pour ces points d’entrée, les données sont transmises au format POST x-www-form-urlencoded, et non au format JSON.

**Types de section**

| Type | Valeur |
|--- |--- |
| Contenu dynamique | Identifiant de la segmentation. |
| Form | Identifiant du formulaire. |
| HTML | Envoyez du texte au contenu HTML. |
| Image | Identifiant de la ressource image. |
| Rectangle | Vide. |
| RichText | Envoyez du texte au contenu HTML.  Ne peut contenir que des éléments de texte enrichi. |
| Extrait | Identifiant du fragment de code. |
| SocialButton | L’identifiant de  le bouton social. |
| Vidéo | Identifiant de la vidéo. |

Pour les pages de forme libre, toutes les sections de contenu souhaitées doivent être ajoutées et seront incorporées dans l’élément div avec l’ID `mktoContent`. Pour les pages guidées, une liste d’éléments prédéfinis peut être présente dans la liste à partir du point d’entrée [Get Landing Page Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Vous pouvez en ajouter plus ou mettre à jour leur [contenu](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) via leurs points d’entrée respectifs.

### Contenu dynamique

Pour créer une section de contenu dynamique, elle doit déjà être présente dans la liste de contenu de la page de destination. Le point d’entrée [ Mettre à jour le contenu de la page de destination ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) doit ensuite être utilisé pour définir le type sur « DynamicContent ». Lorsqu’une section est définie sur du contenu dynamique, elle crée des sections dynamiques sous-jacentes dans la section de contenu qui héritent toutes du type de base de l’élément converti. Chaque section dynamique hérite également du contenu de la section convertie.

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

Les variables modifiables sont l’une des fonctionnalités introduites dans les pages de destination guidées.  Les variables contiennent des valeurs pour les éléments d’une page de destination.  Les variables peuvent facilement être modifiées à l’aide de l’éditeur de page de destination, comme illustré ci-dessous :

![Variables de page de destination](assets/landing-page-variables.png)

Les variables sont définies sous la forme de balises meta dans `<head>` élément d’un modèle de page de destination en mode guidé. Trois types de variables sont disponibles : chaîne, couleur et valeur booléenne.  Voici un exemple de trois définitions de variable :

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Pour plus d’informations, consultez la section « Variable modifiable » de la documentation [Création d’un modèle de page de destination guidé](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template).

### Requête

Récupérez les variables d’une page de destination guidée en transmettant l’ID de la page de destination au point d’entrée Obtenir les variables de page de destination.

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

Dans  Dans cet exemple, la page de destination guidée contient 3 variables : stringVar, colorVar, boolVar.

### Mise à jour 

Mettez à jour une variable pour une page de destination guidée en transmettant l’identifiant de page de destination, l’identifiant de variable et la valeur de variable au point d’entrée Mettre à jour les variables de page de destination .

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

## Aperçu de la page de destination

Marketo fournit le point d’entrée [Obtenir le contenu complet de la page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) pour récupérer un aperçu en direct d’une page de destination telle qu’elle serait rendue dans un navigateur. Il existe un paramètre obligatoire, le paramètre `id` path , qui est l’identifiant de la page de destination que vous souhaitez prévisualiser. Deux paramètres de requête facultatifs supplémentaires sont disponibles :

- segmentation : accepte un tableau d’objets JSON contenant les attributs segmentationId et segmentId. Lorsqu’elle est définie, prévisualise la page de destination comme si vous étiez un prospect correspondant à ces segments.
- leadId :  Accepte l’identifiant entier d’un prospect. Lorsqu’elle est définie, prévisualise la page de destination comme si elle avait été vue par le prospect désigné.

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
