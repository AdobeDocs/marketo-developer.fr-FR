---
title: E-mails
feature: REST API
description: API pour manipuler des ressources de courrier électronique.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1946'
ht-degree: 1%

---

# E-mails

[Référence du point de fin de courrier électronique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails) Un ensemble complet de points de fin REST est fourni pour manipuler les ressources de courrier électronique.

Remarque : Si vous utilisez [Marketo Predictive Content](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), les points de terminaison suivants échouent s’ils référencent un email contenant du contenu prédictif : [Obtenir le contenu de l’email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET), [Mettre à jour le contenu de l’email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) et [Approuver le brouillon du courrier électronique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/approveDraftUsingPOST). L’appel renvoie un code d’erreur 709 et le message d’erreur correspondant.

## Requête

Le modèle de requête des emails est identique à celui des modèles. Il permet aux requêtes [ by id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [ by name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET) et [browsing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET), ainsi qu’au filtrage basé sur un dossier avec les API de navigation et de nom.

Remarque : Si un email fait partie d’un programme de messagerie qui utilise [Test A/B](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test), cet email n’est pas disponible pour la requête à l’aide des points de terminaison suivants : [Obtenir un email par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [Obtenir un email par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), [Obtenir des emails](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET). L’appel indique la réussite, mais contient l’avertissement suivant : &quot;Aucune ressource trouvée pour les critères de recherche donnés&quot;.

### Par identifiant

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

Par nom, vous pouvez éventuellement transmettre un dossier à rechercher uniquement dans ce dossier.

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

La navigation des dossiers fonctionne comme d’autres points de terminaison de navigation de l’API Asset et permet un filtrage facultatif sur `status`, `folder`, `earliestUpdatedAt`/`latestUpdatedAt`, `maxReturn` et `offset`. `status` est Approuvé ou En création. `folder` est un objet JSON contenant `id` et `type`. `maxReturn` est un entier qui limite le nombre de résultats (la valeur par défaut est 20, la valeur maximale est 200) et `offset` est un entier qui peut être utilisé avec `maxReturn` pour lire les jeux de résultats volumineux (la valeur par défaut est 0).

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

Vous pouvez [récupérer les sections modifiables disponibles](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) pour un email en interrogeant son contenu et éventuellement appliquer un filtre sur l’état pour obtenir les sections pour les versions Approuvé ou Version préliminaire.

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

Les sections peuvent renvoyer comme ayant un type de contenu dynamique. Pour plus d’informations, consultez la section [Contenu dynamique](dynamic-content.md) .

## Champs de requête CC

Vous pouvez récupérer l’ensemble des champs activés pour Email CC dans l’instance cible en appelant le point de terminaison [Get Email CC Fields](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailCCFieldsUsingGET).

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

[Les emails sont créés](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailUsingPOST) à partir d’un modèle source et comportent une liste de sections modifiables dérivées de chaque élément d’HTML distinct dans ce modèle avec une classe &quot;mktEditable&quot; et une propriété d’identifiant unique. La création d’un courrier électronique avec l’API crée un enregistrement basé sur le modèle avec les métadonnées supplémentaires transmises. Les paramètres suivants sont requis pour un appel Create Email réussi : nom, modèle, dossier.

Les paramètres suivants sont facultatifs pour la création : `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational`, `isOpenTrackingDisabled`. Si non défini, `subject` sera vide, `fromName`, `fromEmail` et `replyEmail` seront définis sur les valeurs par défaut de l’instance, et `operational` et `isOpenTrackingDisabled` sur false. `isOpenTrackingDisabled` détermine si le pixel de suivi d’ouverture est inclus dans un email lorsqu’il est envoyé.

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

[La mise à jour d’un enregistrement d’email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) peut être effectuée par identifiant. Cela permet de mettre à jour la description ou le nom de l’email.

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

### Section, type et mise à jour de contenu

Le contenu de chaque section d&#39;un email doit être mis à jour individuellement, à l&#39;exception de l&#39;objet, fromName, fromEmail et responseEmail, qui sont mis à jour à l&#39;aide du point d&#39;entrée [Mettre à jour le contenu de l&#39;email](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST). Lors de l’utilisation de ce point de terminaison, ces valeurs peuvent également être définies pour utiliser du contenu dynamique au lieu du contenu statique. Chaque paramètre est un objet JSON de type/valeur, où le type est &quot;Texte&quot; ou &quot;Contenu dynamique&quot; et la valeur est la valeur de texte appropriée ou l’identifiant de la segmentation à utiliser pour le contenu dynamique. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.  isOpenTrackingDisabled peut être défini avec l’option Mettre à jour le contenu des emails

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

Si vous définissez une section pour utiliser du contenu dynamique, l’identifiant de section doit être récupéré via l’appel Get Email Content (Obtenir le contenu d’un email).

### Mettre à jour la section modifiable

Les sections modifiables sont mises à jour en fonction de leurs htmlIds individuels. Seuls l’identifiant de l’email et l’identifiant html de la section sont requis comme paramètres de chemin d’accès, tandis que type, value et textValue sont facultatifs. Le type peut être &quot;Texte&quot;, &quot;ContenuDynamique&quot; ou &quot;Extrait de code&quot; et aura une incidence sur ce qui est transmis dans la valeur. Si le type est Texte, la valeur est une chaîne contenant le contenu HTML de la section. S’il s’agit de contenu dynamique, il s’agit alors d’un bloc JSON, avec trois membres, type, qui sera &quot;Contenu dynamique&quot;, segmentation qui est l’identifiant de la segmentation à utiliser pour le contenu, et par défaut, une chaîne contenant le contenu par défaut de l’HTML de la section. Le paramètre textValue facultatif est une chaîne contenant la version texte de la section. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

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

Remarque : Si la copie automatique vers le texte est désactivée pour un fragment de code incorporé dans un email, la valeur d’HTML du fragment de code est mise à jour, puis la version texte d’une autre section de l’email est mise à jour. La version texte de l’email contiendra le texte reflétant la valeur mise à jour de l’HTML du fragment de code, et non la version précédente comme prévu avec la désactivation de la copie automatique.

## Modules

Dans Email Editor 1.0, un module est une section de votre email qui est définie dans le modèle. Les modules peuvent contenir toute combinaison d’éléments, de variables et d’autres contenus d’HTML, comme décrit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules). Marketo propose un ensemble d’API pour la gestion des modules dans un email. Pour les points de terminaison liés au module qui nécessitent la méthode de POST HTTP, le corps est formaté en tant que &quot;application/x-www-form-urlencoded&quot; (et non en tant que JSON).

La plupart des points de terminaison liés au module nécessitent un &quot;moduleId&quot; comme paramètre de chemin d’accès. Il s’agit d’une chaîne qui décrit le module. moduleIds est renvoyé par le point de terminaison [Get Email Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) en tant qu’attribut &quot;htmlId&quot; (voir la section [Query](#modules_query) ci-dessous).

### Requête

Pour utiliser des modules, vous devez spécifier un paramètre moduleId, qui identifie de manière unique le module. Vous pouvez également spécifier le paramètre d’index du module, qui est un entier qui décrit l’ordre du module dans l’email.

[Récupérez les ID de module et leurs index](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) en spécifiant l’ID de courrier électronique comme paramètre de chemin d’accès.

L’exemple suivant interroge un email version 1.0 basé sur le modèle &quot;Skeleton&quot; qui se trouve dans la section &quot;Modèles de démarrage&quot; de l’interface utilisateur du sélecteur de modèles.

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

Le tableau de résultat contient des éléments qui décrivent à la fois les modules et les éléments d’HTML mixtes. Les éléments du module contiennent un &quot;contentType&quot; : attribut &quot;Module&quot; et un attribut &quot;index&quot;. Le moduleId est stocké dans l’attribut &quot;htmlId&quot;.

En reprenant l’exemple &quot;Skeleton&quot; ci-dessus, le tableau suivant contient un résumé de moduleIds et les index correspondants contenus dans l’email.

| moduleId (alias htmlId) | Indice |
|---|---|
| spacer | 0 |
| free-image | 1 |
| video | 2 |
| free-text | 3 |
| Appel à l’action | 4 |
| hr | 5 |
| deux articles | 6 |
| pied de page | 7 |

#### Ajouter

[Ajoutez un module](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/addModuleUsingPOST) à un email en effectuant une sélection depuis l’un des modules existants contenus dans le modèle d’email en cours d’utilisation. Pour ce faire, spécifiez l’ID d’adresse électronique et le moduleId comme paramètres de chemin d’accès. Le paramètre de requête d’index est obligatoire et détermine l’ordre du module dans l’email. Si la valeur d’index dépasse la valeur d’index existante la plus grande, le module est ajouté à l’email.

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

[Supprimez un module](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/deleteModuleUsingPOST) en spécifiant l’ID de courrier électronique et le moduleId comme paramètres de chemin.

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

[Dupliquez un module](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/duplicateModuleUsingPOST) en spécifiant l’ID de courrier électronique et le moduleId comme paramètres de chemin. Cet appel duplique le module, le plaçant sous le module d’origine et poussant les autres modules vers le bas.

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

[Réorganiser les modules](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/rearrangeModulesUsingPOST)tableau qui contient tous les modules et la position souhaitée dans le courrier électronique de chacun. Chaque élément de tableau contient un objet JSON du formulaire suivant :  { &quot;index&quot;: &lt;_index_>, &quot;moduleId&quot;: &quot;&lt;_moduleId_>&quot; }, où &lt;_index_> est le numéro de commande du module à base zéro et &lt;_moduleId_> est l’ID du module.

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

[Renommez un module](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/renameUsingPOST) sur un email en transmettant le nouveau nom via le paramètre name . Indiquez l’ID d’adresse électronique et le moduleId (nom existant) comme paramètres de chemin d’accès.

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

Dans la version 1.0 de l’éditeur de messagerie, les variables sont utilisées pour stocker les valeurs des éléments de votre email. Chaque variable est définie en ajoutant une syntaxe spécifique à Marketo à votre HTML, comme décrit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Marketo propose un ensemble d’API pour gérer les variables dans un email.

### Requête

[Récupérez des variables](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailVariablesUsingGET) pour un email en spécifiant l’ID de l’email comme paramètre de chemin d’accès.

L’exemple suivant interroge un email version 1.0 basé sur le modèle &quot;Skeleton&quot; qui se trouve dans la section &quot;Modèles de démarrage&quot; de l’interface utilisateur du sélecteur de modèles.

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

Le tableau de résultat contient des éléments décrivant les variables, une variable par élément.

Les variables peuvent être incluses globalement dans l’ensemble de l’email ou localement dans un module spécifique. Les variables de l’une ou l’autre portée contiennent les attributs &quot;name&quot;, &quot;value&quot; et &quot;moduleScope&quot;. L’attribut &quot;moduleScope&quot; est booléen, où false indique global et true indique local. Les variables locales contiennent un attribut &quot;moduleId&quot; supplémentaire qui spécifie le module auquel la variable est associée.

#### Mise à jour

[Mettez à jour une variable](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateVariableUsingPOST) dans un email en transmettant la nouvelle valeur souhaitée via le paramètre de valeur. Indiquez l’ID de courrier électronique et le nom de la variable comme paramètres de chemin d’accès. Si vous mettez à jour une variable de module, vous devez également transmettre le paramètre moduleId pour spécifier le module auquel la variable est associée.

Dans l’exemple suivant, nous mettons à jour une variable globale nommée &quot;hrBorderSize&quot; avec la valeur 1.

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

Dans l’exemple suivant, nous mettons à jour une variable locale nommée &quot;ctaLinkText&quot; avec la valeur &quot;Cliquez sur ce bouton !&quot;. dans moduleId &quot;CTA&quot;.

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

Les courriers électroniques suivent le modèle standard d’approbation des enregistrements de ressources. Vous pouvez approuver un brouillon, annuler l’approbation d’une version approuvée et ignorer un brouillon existant d’un email via chacun de leurs propres points de terminaison.

### Approuver

Lors de l’appel du point de fin d’approbation, l’email est validé en fonction des règles des emails Marketo. Les `from name`, `from email`, `reply to email` et `subject` doivent être renseignés avant que l&#39;email puisse être approuvé.

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

L&#39;opération `unapprove` ne peut être effectuée que sur les emails approuvés.

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

L’email doit être à l’état de brouillon pour être ignoré. Un email approuvé ne peut pas être ignoré.

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

Marketo fournit une méthode simple pour cloner un email. Ce type de requête est effectué avec un POST application/x-www-url-urlencoded et prend deux paramètres requis, le nom et le dossier, un objet JSON incorporé avec l’identifiant et le type . description est également un paramètre facultatif. Si aucune version approuvée n’existe, la version préliminaire est clonée.

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

Vous pouvez déclencher un exemple d’email via l’api, à envoyer au paramètre de requête emailAddress . Vous pouvez également éventuellement ajouter un paramètre leadId pour emprunter l’identité d’une piste particulière à partir de votre base de données, ainsi qu’un paramètre textOnly pour envoyer uniquement la version texte de l’email.

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

Marketo fournit le point de terminaison [Get Email Full Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailFullContentUsingGET) pour récupérer un aperçu en direct d’un email tel qu’il serait envoyé à un destinataire. Ce point de terminaison ne peut être utilisé que pour les courriers électroniques de la version 1.0. Il existe un paramètre obligatoire, le paramètre de chemin d’accès à l’identifiant, qui est l’identifiant de la ressource de messagerie que vous souhaitez prévisualiser. Il existe trois autres paramètres de requête facultatifs :

- status : accepte les valeurs &quot;draft&quot; ou &quot;approved&quot; qui s’affichent par défaut sur la version approuvée, si elle est approuvée, version préliminaire si elle n’est pas approuvée.
- type : accepte &quot;Texte&quot; ou &quot;HTML&quot; et par défaut, HTML.
- leadId : . Accepte l’identifiant entier d’une piste. Une fois défini, prévisualise l’email comme s’il avait été reçu par le prospect désigné.

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

Marketo fournit le point de terminaison [Mettre à jour le contenu complet du courrier électronique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailFullContentUsingPOST) pour remplacer tout le contenu d’une ressource de courrier électronique. Ce point de terminaison ne peut être utilisé que pour les emails de la version 1.0 dans lesquels la fonction &quot;Modifier le code&quot; de l’interface utilisateur a été utilisée et où la relation avec leur modèle parent a été rompue. Cette API est principalement destinée aux ressources qui ont été clonées dans le cadre d’un programme et qui ne peuvent pas être modifiées à l’aide des points de terminaison de contenu standard. Les emails avec du contenu dynamique ne sont pas pris en charge. En outre, si vous essayez de remplacer un HTML sur un email dont la relation est intacte, une erreur est renvoyée.

Ce point de terminaison attend un type de contenu : multipart/form-data avec le paramètre id dans le chemin d’accès, l’identifiant de l’email et un paramètre dans le corps, le contenu comme document d’HTML complet avec le type de contenu &quot;text/html&quot;. Un document d’HTML mal formé émet un avertissement, mais peut ne pas autoriser l’approbation, tandis que l’inclusion de balises JavaScript et/ou `<script>` dans le document provoque l’échec de l’appel et génère une erreur.

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
