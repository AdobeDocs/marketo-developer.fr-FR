---
title: E-mails
feature: REST API
description: API pour manipuler des ressources d’e-mail.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1946'
ht-degree: 1%

---

# E-mails

[Référence de point d’entrée de courrier électronique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails) Un ensemble complet de points d’entrée REST est fourni pour manipuler des ressources de courrier électronique.

Remarque : si vous utilisez [du contenu prédictif Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), les points d’entrée suivants échoueront s’ils référencent un e-mail contenant du contenu prédictif : [Obtenir le contenu de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET), [Mettre à jour la section de contenu de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST), [Approuver le brouillon d’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/approveDraftUsingPOST). L’appel renvoie un code d’erreur 709 et le message d’erreur correspondant.

## Requête

Le modèle de requête pour les e-mails est identique à celui des modèles. Il autorise les requêtes [par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET) et [navigation](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET), ainsi que le filtrage en fonction du dossier avec les API de navigation et par nom.

Remarque : si un e-mail fait partie d’un programme de messagerie qui utilise des tests A/B [A/B Testing](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test), cet e-mail n’est pas disponible pour la requête à l’aide des points d’entrée suivants : [Get Email by Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [Get Email by Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), [Get Emails](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET). L’appel indique la réussite, mais contiendra l’avertissement suivant : « Aucune ressource trouvée pour les critères de recherche donnés ».

### Par ID

```
GET /rest/asset/v1/email/1351.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"9ad0#14a1832af8c",
   "result":[
      {
         "id":1356,
         "name":"sakZxhxkwV",
         "description":"sample description",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "subject":{
            "type":"Text",
            "value":"sample subject"
         },
         "fromName":{
            "type":"Text",
            "value":"RBxEtmdQZz"
         },
         "fromEmail":null,
         "replyEmail":{
            "type":"Text",
            "value":"Qlikf@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":10421
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":false,
         "template":338,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
         "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Par nom

Pour l’option par nom, vous pouvez éventuellement transmettre un dossier pour effectuer une recherche uniquement dans ce dossier.

```
GET /rest/asset/v1/email/byName.json?name=My Email&folder={"id":1056,"type"="Folder"}
```

```json
{
   "success":true,
   "warnings":[
   ],
   "errors":[
   ],
   "requestId":"3a7f#14c484de875",
   "result":[
      {
         "id":1032,
         "name":"My Email",
         "description":"eCjxjIHmYPLtecoSphkvIXlrygOBDLhgyQKnsKMpiKWgSCKhkPMUFvFPUvEylmFiLjQGnffXGaiNLxAwiFOmIDvxEINoaSYascJw",
         "createdAt":"2015-03-23T20:23:25Z+0000",
         "updatedAt":"2015-03-23T20:23:25Z+0000",
         "subject":{
            "type":"Text",
            "value":"ezyKBmDcyCcUIrXASrLSvRuWQgWpRZxQstJoStgMSLEBASGKMpAnVeWrgJsaVFoFJUEXhEIPpDAWpzajzingUruFpiMcRRwtoBzU"
         },
         "fromName":{
            "type":"Text",
            "value":"dAiqRNJOdY"
         },
         "fromEmail":{
            "type":"Text",
            "value":"ilZxG@testmail.com"
         },
         "replyEmail":{
            "type":"Text",
            "value":"VYsCS@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":1056
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":"draft",
         "template":32,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
        "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Parcourir

La navigation dans les dossiers fonctionne comme d’autres points d’entrée de navigation de l’API Assets et permet un filtrage facultatif sur les `status`, `folder`, `earliestUpdatedAt`/`latestUpdatedAt`, `maxReturn` et `offset`. `status` est Approuvé ou Brouillon. `folder` est un objet JSON contenant des `id` et des `type`. `maxReturn` est un entier qui limite le nombre de résultats (20 par défaut, 200 au maximum) et `offset` est un entier qui peut être utilisé avec `maxReturn` pour lire de grands ensembles de résultats (0 par défaut).

```
GET /rest/asset/v1/emails.json?maxReturn=3&folder={"id":341,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17576#14e22eb29cb",
    "result": [
        {
            "id": 2137,
            "name": "Social Sharing in Email",
            "description": "",
            "createdAt": "2011-03-04T17:12:42Z+0000",
            "updatedAt": "2011-03-04T19:04:36Z+0000",
            "url": null,
            "subject": {
                "type": "Text",
                "value": "Republish this content to your favorite social site!"
            },
            "fromName": {
                "type": "Text",
                "value": "Demo Master Marketo"
            },
            "fromEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 341,
                "folderName": "Social Media"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": true,
            "status": "approved",
            "template": null,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
               {
                 "attributeId": "157",
                 "objectName": "lead",
                 "displayName": "Lead Owner Email Address",
                 "apiName": null
               }
             ],
            "preHeader": "My awesome preheader!"
        }
    ]
}
```

## Contenu de la requête

Vous pouvez [récupérer les sections modifiables disponibles](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) d’un e-mail en interrogeant son contenu et éventuellement filtrer par statut pour obtenir les sections des versions Approuvées ou Brouillons.

```
GET /rest/asset/v1/email/1356/content.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"8a44#14c484de8c8",
   "result":[
      {
         "htmlId":"edit_text_3",
         "value":[
            {
               "type":"HTML",
               "value":"Content from testCreateEmailTemplate2"
            },
            {
               "type":"Text",
               "value":"Content from testCreateEmailTemplate2"
            }
         ],
         "contentType":"Text"
      }
   ]
}
```

Les sections peuvent renvoyer la valeur comme ayant un type de contenu dynamique. Voir la section [Contenu dynamique](dynamic-content.md) pour plus d’informations.

## Champs CC de requête

Vous pouvez récupérer l’ensemble des champs activés pour Email CC dans l’instance cible en appelant le point d’entrée [Obtenir les champs Email CC](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailCCFieldsUsingGET).

```
GET /rest/asset/v1/email/ccFields.json
```

```json
{
   "success":true,
   "errors":[ ],
   "requestId":"e54b#16796fdbd4e",
   "warnings":[ ],
   "result":[
      {
         "attributeId":"157",
         "objectName":"lead",
         "displayName":"Lead Owner Email Address",
         "apiName":"leadOwnerEmailAddress"
      },
      {
         "attributeId":"396",
         "objectName":"company",
         "displayName":"Account Owner Email Address",
         "apiName":"accountOwnderEmailAddress"
      }
   ]
}
```

## Créer et mettre à jour

[Les e-mails sont créés](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailUsingPOST) basés sur un modèle source, et comportent une liste de sections modifiables dérivées de chaque élément HTML distinct dans ce modèle avec une classe « mktEditable » et une propriété d’identifiant unique. La création d’un e-mail avec l’API crée un enregistrement basé sur le modèle avec toutes les métadonnées supplémentaires transmises. Les paramètres suivants sont requis pour réussir l’appel de création d’e-mail : nom, modèle, dossier.

Les paramètres suivants sont facultatifs pour la création : `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational`, `isOpenTrackingDisabled`. Si cette option n’est pas définie, `subject` sera vide, `fromName`, `fromEmail` et `replyEmail` seront définis sur les valeurs par défaut de l’instance, et `operational` et `isOpenTrackingDisabled` seront définis sur « false ». `isOpenTrackingDisabled` détermine si le pixel de suivi d’ouverture est inclus dans un e-mail lorsqu’il est envoyé.

```
POST /rest/asset/v1/emails.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=My New Email 02 - deverly&folder={"id":1017,"type":"Program"}&template=24&description=This is a test email&subject=Hey There&fromName=SomeBody&fromEmail=somebody@marketo.com&replyEmail=somebody@marketo.com
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "My New Email 02 - deverly",
            "description": "This is a test email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,
            "preHeader": null
        }
    ]
}
```

L’enregistrement [mise à jour d’un e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) peut être effectué par identifiant. Cela permet de mettre à jour la description ou le nom de l’e-mail.

```
POST /rest/asset/v1/email/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is an Email&name=Updated Email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "Updated Email",
            "description": "This is an Email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,
            "preHeader": null
        }
    ]
}
```

### Section, type et mise à jour du contenu

Le contenu de chaque section d’un e-mail doit être mis à jour individuellement, à l’exception des paramètres subject, fromName, fromEmail et responseEmail, qui sont mis à jour à l’aide du point d’entrée [Update Email Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST). Lors de l’utilisation de ce point d’entrée, ces valeurs peuvent également être définies pour utiliser du contenu dynamique au lieu du contenu statique. Chaque paramètre est un objet JSON de type/valeur, où le type est « Text » ou « DynamicContent » et où la valeur est la valeur de texte appropriée ou l’identifiant de la segmentation à utiliser pour le contenu dynamique. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.  isOpenTrackingDisabled peut être défini avec Mettre à jour le contenu de l’e-mail.

```
POST /rest/asset/v1/email/{id}/content.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
subject={"type":"Text","value":"Gettysburg Address"}&fromEmail={"type":"Text","value":"abe@testmail.com"}&fromName={"type":"Text","value":"Abe Lincoln"}&replyTO={"type":"Text","value":"replies@testmail.com"}
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"c865#14a1832afac",
   "result":[
      {
         "id":1356
      }
   ]
}
```

Si vous définissez une section pour utiliser du contenu dynamique, l’identifiant de section doit être récupéré via l’appel Get Email Content.

### Mettre à jour la section modifiable

Les sections modifiables sont mises à jour en fonction de leurs htmlId individuels. Seuls l’identifiant de l’e-mail et l’htmlId de la section sont requis en tant que paramètres de chemin, tandis que le type, la valeur et textValue sont facultatifs. Le type peut être « Texte », « Contenu dynamique » ou « Extrait de code » et affectera ce qui est transmis dans la valeur. Si le type est Texte, alors la valeur est une chaîne contenant le contenu HTML de la section. S’il s’agit de DynamicContent, il s’agit d’un bloc JSON, avec trois membres de type, qui sera « DynamicContent », segmentation qui est l’identifiant de la segmentation à utiliser pour le contenu et par défaut, qui est une chaîne contenant le contenu HTML par défaut de la section. Le paramètre facultatif textValue est une chaîne contenant la version texte de la section. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

```
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Text&value=<h1>Hello World!</h1>&textValue=Hello World!
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "155ac#14d58dfa9ad",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

Remarque : si la fonction de copie automatique au texte est désactivée pour un fragment de code incorporé dans un e-mail, la valeur HTML du fragment de code est mise à jour, puis la version texte d’une autre section de l’e-mail est mise à jour. Le texte de la version texte de l’e-mail reflète alors la valeur mise à jour du fragment de code HTML, et non la version précédente, comme cela aurait été prévu si la copie automatique était désactivée.

## Modules

Dans l’éditeur d’e-mail 1.0, un module est une section de votre e-mail définie dans le modèle. Les modules peuvent contenir toute combinaison d’éléments, de variables et d’autres contenus HTML comme décrit [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules). Marketo propose un ensemble d’API pour gérer les modules d’un e-mail. Pour les points d’entrée liés au module qui nécessitent la méthode HTTP POST, le corps est formaté comme « application/x-www-form-urlencoded » (et non comme JSON).

La plupart des points d’entrée liés au module nécessitent un « moduleId » en tant que paramètre de chemin d’accès. Il s’agit d’une chaîne qui décrit le module . Les moduleId sont renvoyés par le point d’entrée [Obtenir le contenu de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) en tant qu’attribut « htmlId » (voir la section [Requête](#modules_query) ci-dessous).

### Requête

Pour utiliser les modules, vous devez spécifier un paramètre moduleId qui identifie le module de manière unique. Vous pouvez également spécifier le paramètre d’index du module, qui est un entier qui décrit l’ordre du module dans l’e-mail.

[Récupérez les moduleIds et leurs index](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) en spécifiant l’ID d’e-mail comme paramètre de chemin.

L’exemple suivant interroge un e-mail 1.0 en fonction du modèle « Squelette » figurant dans la section « Modèles de démarrage » de l’interface utilisateur du sélecteur de modèles.

```
GET /rest/asset/v1/email/{moduleId}/content.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "3d79#158da6492bd",
  "result": [
    {
      "htmlId": "free-image",
      "contentType": "Module",
      "index": 1,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "single",
      "value": {
        "width": "600",
        "altText": "",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 600px"
      },
      "contentType": "Image",
      "parentHtmlId": "free-image",
      "isLocked": false
    },
    {
      "htmlId": "video",
      "contentType": "Module",
      "index": 2,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "video2",
      "value": {

      },
      "contentType": "Video",
      "parentHtmlId": "video",
      "isLocked": false
    },
    {
      "htmlId": "free-text",
      "contentType": "Module",
      "index": 3,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "text",
      "value": [
        {
          "type": "HTML",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        },
        {
          "type": "Text",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "free-text",
      "isLocked": false
    },
    {
      "htmlId": "two-articles",
      "contentType": "Module",
      "index": 6,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "article3",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text2",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "article4",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle2",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text3",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "footer",
      "contentType": "Module",
      "index": 7,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "footerText",
      "value": [
        {
          "type": "HTML",
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you've subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you've subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "footer",
      "isLocked": false
    },
    {
      "htmlId": "spacer",
      "contentType": "Module",
      "index": 0,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "CTA",
      "contentType": "Module",
      "index": 4,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "hr",
      "contentType": "Module",
      "index": 5,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    }
  ]
}
```

Le tableau result contient des éléments qui décrivent à la fois les modules et les éléments HTML mélangés. Les éléments de module contiennent un attribut « contentType » : attribut « Module » et un attribut « index ». ModuleId est stocké dans l&#39;attribut « htmlId ».

En reprenant l’exemple « Squelette » ci-dessus, le tableau suivant contient un résumé des moduleIds et de leurs index correspondants contenus dans l’e-mail.

| moduleId (ou htmlId) | Indice |
|---|---|
| entretoise | 0 |
| image libre | 1 |
| video | 2 |
| texte libre | 3 |
| Appel à l’action | 4 |
| h | 5 |
| deux articles | 6 |
| pied | 7 |

#### Ajouter

[Ajoutez un module](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/addModuleUsingPOST) à un e-mail en le sélectionnant dans l’un des modules existants contenus dans le modèle d’e-mail en cours d’utilisation. Pour ce faire, spécifiez l’ID d’e-mail et l’ID de module en tant que paramètres de chemin. Le paramètre de requête d’index est obligatoire et détermine l’ordre du module dans l’e-mail. Si la valeur de l’index dépasse la valeur d’index existante la plus grande, le module est ajouté à l’e-mail.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
index=10
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "1063e#158d6ad2c3f",
    "result": [
        {
            "id": 1028
        }
    ]
}
```

#### Supprimer

[Supprimez un module](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/deleteModuleUsingPOST) en spécifiant l’ID d’e-mail et l’ID de module en tant que paramètres de chemin.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "2356#158d6f6104a",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Dupliquer

[Dupliquez un module](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/duplicateModuleUsingPOST) en spécifiant l’ID d’e-mail et l’ID de module en tant que paramètres de chemin. Cet appel duplique le module, en le plaçant sous le module d’origine et en poussant les autres modules vers le bas.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "e740#158d705d967",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Réorganiser

[Réorganisez les modules](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/rearrangeModulesUsingPOST)tableau qui contient tous les modules et la position souhaitée dans l’e-mail pour chacun d’eux. Chaque élément de tableau contient un objet JSON de la forme suivante :  { « index »: &lt;_index_>, « moduleId »: « &lt;_moduleId_> » }, où &lt;_index_> est le numéro d&#39;ordre du module basé sur zéro, et &lt;_moduleId_> est le moduleId.

```
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "e67a#158d72d1cde",
    "result":[
        {
            "id": 1030
        }
    ]
}
```

#### Renommer

[Renommez un module](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/renameUsingPOST) sur un e-mail en transmettant le nouveau nom via le paramètre name. Spécifiez l’ID d’e-mail et le moduleId (nom existant) comme paramètres de chemin d’accès.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MarketoVideo
```

```
{
    "success": true,
    "warnings":[ ],
    "errors": [ ],
    "requestId":"11521#158d740abc0",
    "result": [
        {
            "id": 1030
        }
    ]
}
```

## Variables

Dans l’éditeur d’e-mail 1.0, les variables sont utilisées pour stocker les valeurs des éléments de votre e-mail. Chaque variable est définie en ajoutant une syntaxe spécifique à Marketo à votre HTML, comme décrit [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Marketo propose un ensemble d’API pour gérer les variables d’un e-mail.

### Requête

[Récupérez des variables](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailVariablesUsingGET) pour un e-mail en spécifiant l’ID d’e-mail comme paramètre de chemin.

L’exemple suivant interroge un e-mail 1.0 en fonction du modèle « Squelette » figurant dans la section « Modèles de démarrage » de l’interface utilisateur du sélecteur de modèles.

```
GET /rest/asset/v1/email/{id}/variables.json
```

```
{
  "success": true,
  "warnings": [ ],
  "errors": [  ],
  "requestId": "756#158dade55e8",
  "result": [
    {
      "name": "twoArticlesSpacer5",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer6",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer7",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText2",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer8",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer2",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "hrBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "freeTextBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink2",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "hrBorderColor",
      "value": "#e6e6e6",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "freeImageBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "footerSpacer",
      "value": "10",
      "moduleScope": false
    },
    {
      "name": "ctaLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "hrBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize2",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer4",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer3",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer",
      "value": "20",
      "moduleScope": false
    }
  ]
}
```

Le tableau de résultats contient des éléments décrivant des variables, une variable par élément.

Les variables peuvent être étendues globalement à l’ensemble de l’e-mail ou localement à un module spécifique. Les variables de l’une des portées contiennent des attributs « name », « value » et « moduleScope ». L’attribut « moduleScope » est booléen, où false indique global et true indique local. Les variables locales contiennent un attribut « moduleId » supplémentaire qui spécifie le module auquel la variable est associée.

#### Mise à jour 

[Mettez à jour une variable](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateVariableUsingPOST) dans un e-mail en transmettant la nouvelle valeur souhaitée via le paramètre de valeur. Spécifiez l’ID d’e-mail et le nom de variable en tant que paramètres de chemin. Si vous mettez à jour une variable de module, vous devez également transmettre le paramètre moduleId pour spécifier le module auquel la variable est associée.

Dans l’exemple suivant, nous mettons à jour une variable globale nommée « hrBorderSize » sur une valeur de 1.

```
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```
value=2
```

```
{
    "success":true,
    "warnings":[ ],
    "errors":[ ],
    "requestId":"feb5#158db4be57e",
    "result": [
        {
            "name":"hrBorderSize",
            "value":"2",
            "moduleScope":false
        }
    ]
}
```

Dans l’exemple suivant, nous mettons à jour une variable locale nommée « ctaLinkText » sur une valeur de « Cliquez sur ce bouton ! » dans moduleId « CTA ».

```
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
value=Click this button!&moduleId=CTA
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "7f34#158dc28d2f7",
    "result": [
        {
            "name":"ctaLinkText",
            "value":"Click this button!",
            "moduleScope":true,
            "moduleId":"CTA"
        }
    ]
}
```

## Validation

Les e-mails suivent le modèle standard pour les approbations des enregistrements de ressources. Vous pouvez approuver un brouillon, annuler l’approbation d’une version approuvée et ignorer un brouillon existant d’un e-mail via chacun de leurs points d’entrée.

### Approuver

Lors de l’appel du point d’entrée d’approbation, l’e-mail sera validé par rapport aux règles pour les e-mails Marketo. Les `from name`, `from email`, `reply to email` et `subject` doivent être renseignés pour que l’e-mail puisse être approuvé.

```
POST /rest/asset/v1/email/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"15dbf#14a1832ae86",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Désapprouver

L&#39;opération `unapprove` ne peut être effectuée que sur les e-mails approuvés.

```
POST /rest/asset/v1/email/{id}/unapprove.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"3514#14a1832b0fa",
   "result":[
      {
         "id":1364
      }
   ]
}
```

#### Rejeter

L’e-mail doit être à l’état de brouillon pour être ignoré. Un e-mail approuvé ne peut pas être ignoré.

```
POST /rest/asset/v1/email/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"182c0#14a1832af4f",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Supprimer

```
POST /rest/asset/v1/email/{id}/delete.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"169cd#14a1832adba",
   "result":[
      {
         "id":1361
      }
   ]
}
```

## Cloner

Marketo offre une méthode simple pour cloner un e-mail. Ce type de requête est effectué avec une requête POST application/x-www-url-urlencoded et utilise deux paramètres obligatoires, à savoir le nom et le dossier, ainsi qu’un objet JSON incorporé avec l’ID et le type. La description est également un paramètre facultatif. S’il n’existe aucune version approuvée, le brouillon est cloné.

```
POST /rest/asset/v1/email/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Clone of Social Sharing in Email&folder={"id":239,"type":"Folder"}&description=This is a test of clone email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bd49#15706f43d96",
    "result": [
        {
            "id": 2250,
            "name": "Clone of Social Sharing in Email",
            "description": "This is a test of clone email",
            "createdAt": "2016-09-07T23:20:52Z+0000",
            "updatedAt": "2016-09-07T23:20:52Z+0000",
            "url": "https://app-abm.marketo.com/#EM2250B2",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 239,
                "folderName": "Tradeshows and Events"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false
        }
    ]
}
```

## Envoyer un échantillon

Vous pouvez déclencher un exemple d’e-mail via l’api pour l’envoyer au paramètre de requête emailAddress. Vous pouvez également ajouter de manière facultative un paramètre leadId pour emprunter l’identité d’un prospect particulier de votre base de données et un paramètre textOnly pour envoyer uniquement la version texte de l’e-mail.

```
POST /rest/asset/v1/email/{id}/sendSample.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
emailAddress=abe@testmail.com&textOnly=true
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "360b#14cce7d2708",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

## Aperçu de l&#39;e-mail

Marketo fournit le point d’entrée [Obtenir le contenu complet de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailFullContentUsingGET) pour récupérer un aperçu en direct d’un e-mail tel qu’il serait envoyé à un destinataire. Ce point d’entrée ne peut être utilisé que sur les e-mails de la version 1.0. Il existe un paramètre obligatoire, le paramètre de chemin d’accès à l’identifiant, qui est l’identifiant de la ressource d’e-mail que vous souhaitez prévisualiser. Trois paramètres de requête facultatifs supplémentaires sont disponibles :

- Statut : accepte les valeurs « brouillon » ou « approuvé » qui correspondront par défaut à la version approuvée, si elle est approuvée, et à la version brouillon, si elle n’est pas approuvée
- type : accepte « Text » ou « HTML » et utilise par défaut HTML.
- leadId :. Accepte l’identifiant entier d’un prospect. Lorsqu’il est défini, prévisualise l’e-mail comme s’il avait été reçu par le prospect désigné

```
GET /rest/asset/v1/email/{id}/fullContent.json
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": null,
   "result": [
      {
         "id": 339,
         "status": "draft",
         "content": "<!DOCTYPE HTML PUBLIC \"-//W3C//DTD HTML 1.01 Transitional//EN\" \"http://www.w1.org/TR/html4/loose.dtd\">\n<html>\n  <head>\n    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\"/>\n    <title></title>\n  </head>\n  <body>\n      <div style=\"font: 14px tahoma; width: 100%\" class=\"mktEditable\" id=\"edit_text_3\">\n        Content from testCreateEmailTemplate2\n    </div>\n  </body>\n</html>"
      }
   ]
}
```

## Remplacer HTML

Marketo fournit le point d’entrée [Mettre à jour le contenu complet de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailFullContentUsingPOST) pour remplacer l’intégralité du contenu d’une ressource e-mail. Ce point d’entrée ne peut être utilisé que sur les e-mails de la version 1.0 auxquels la fonction « Modifier le code » de l’interface utilisateur a été utilisée et dont la relation avec le modèle parent a été rompue. Cette API est principalement destinée aux ressources qui ont été clonées dans le cadre d’un programme et ne peut pas être modifiée avec les points d’entrée de contenu standard. Les e-mails avec contenu dynamique ne sont pas pris en charge. En outre, si vous tentez de remplacer HTML sur un e-mail dont la relation est intacte, une erreur est renvoyée.

Ce point d’entrée exige un type de contenu : multipart/form-data avec le paramètre d’identifiant dans le chemin d’accès, l’identifiant de l’e-mail et un paramètre dans le corps, content as a complete HTML email document avec le type de contenu « text/html ». Un document HTML mal formé émet un avertissement, mais peut ne pas autoriser la validation, tandis que l’inclusion de JavaScript et/ou de `<script>` balises dans le document provoque l’échec de l’appel et l’émission d’une erreur.

```
POST /rest/asset/v1/email/{id}/fullContent.json
```

```
content-type: multipart/form-data; boundary=--------------------------116301888604800085728247
content-length: 599
```

```html
----------------------------116301888604800085728247
Content-Disposition: form-data; name="content"; filename="email_content.html"
Content-Type: text/html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 1.01 Transitional//EN" "http://www.w1.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
 <title></title>
 </head>
 <body>
 <div style="font: 14px tahoma; width: 100%" class="mktEditable" id="edit_text_3">
 EMAIL TEST CONTENT
 </div>
 </body>
 </html>
----------------------------116301888604800085728247--
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": "15dbf#14a1832ae86",
   "result": [
      {
         "id": 1001
      }
   ]
}
```
