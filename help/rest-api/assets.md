---
title: Ressources
feature: REST API
description: Présentation des API REST de ressources Marketo pour les requêtes par identifiant ou nom, la navigation dans les pages et la création ou la mise à jour de dossiers, d’e-mails, de formulaires, de modèles, de fichiers et de jetons.
exl-id: 4273a5b1-1904-46e8-b583-fc6f46b388d2
TQID: https://experienceleague.adobe.com/gRhXvFtG1FHtGJ4tFQxOyGMkEiOX0K1S0VpjcB6s6xM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: d65b4a73-87a3-4d56-b638-74e74d9939ce
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 631
ht-degree: 3%

---

# Ressources

Utilisez les API REST de ressources Marketo pour interroger et gérer des ressources marketing et organisationnelles.

## Ressources

Les ressources Marketo incluent :

- Dossiers
- Programmes
- E-mails
- Modèles d’e-mail
- Fragments
- Pages de destination
- Modèles de pages de destination
- Extraits
- Formulaires
- Jetons
- Fichiers

## API

Pour obtenir la liste complète des points d’entrée de l’API Asset, y compris les paramètres et les informations de modélisation, consultez la [&#x200B; Référence des points d’entrée de l’API Asset &#x200B;](endpoint-reference.md).

## Requête

Les API de ressources prennent généralement en charge trois modèles de récupération : par identifiant, par nom et par navigation. Les requêtes par ID ou nom récupèrent une ressource pour le paramètre spécifié. Les points d’entrée de navigation renvoient une liste paginée de ressources de ce type.

Les paramètres de filtrage varient selon le type de ressource. Consultez la documentation de chaque type de ressource pour connaître les filtres pris en charge.

Certains points d’entrée de navigation ne renvoient pas de ressources enfants, comme les valeurs autorisées pour une balise. Récupérez ces ressources individuellement par nom ou ID pour obtenir leurs métadonnées complètes. D’autres types de ressources fournissent des points d’entrée distincts pour les objets dépendants tels que les champs de formulaire.

### Par Id

```http
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

### Par nom

Les API de ressources ne peuvent pas rechercher des noms de ressources contenant des virgules. Excluez les virgules des noms de ressources.

```http
GET /rest/asset/v1/file/byName.json?name=My File
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":148,
         "size":270313,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/piKLbhVFvW",
         "folder":{
            "type":"Email",
            "id":10614
         },
         "name":"My File",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Parcourir

Les points d’entrée de l’exploration des ressources prennent en charge les paramètres de requête suivants :

- `offset` - Décalage entier auquel commencer à renvoyer les résultats.
- `maxReturn` - Nombre maximal d’enregistrements à renvoyer. La valeur par défaut est 20 et la valeur maximale est 200.

```http
GET /rest/asset/v1/emailTemplates.json?offset=10&maxReturn=50
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"33c4#14a1832b4a8",
   "result":[
      {
         "id":18,
         "name":"AAA0unit3CreateTestEmailTemplateName.2314673e-7bc2-47da-a1e8-66dfdd8a1f1d",
         "description":"AssetAPI: getTemplates test",
         "createdAt":"2014-11-03T19:52:58Z+0000",
         "updatedAt":"2014-11-03T19:52:58Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":177,
         "name":"ABfRGutnwN",
         "description":"HMmHkdTRrGaRpPakdgGKICxfMunCEWDUWiThgAbInfaBXxGxSFfjKQIwerngCHRlGTnAJhKPmwlXLcsjGPtWEiILGyeIJTNVHoHg",
         "createdAt":"2014-11-20T19:31:06Z+0000",
         "updatedAt":"2014-11-20T19:31:06Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":148,
         "name":"ADVHJBQLyw",
         "description":null,
         "createdAt":"2014-11-20T06:42:57Z+0000",
         "updatedAt":"2014-11-20T06:42:57Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

## Créer et mettre à jour

Les types de ressources simples, tels que les dossiers, les jetons et les fichiers, fournissent généralement un point d’entrée pour la création et un autre pour les mises à jour par identifiant. Un nom est requis lors de la création d’une ressource. La réponse de création ou de mise à jour renvoie les métadonnées et l’identifiant de la ressource.

La requête suivante crée un jeton :

```http
POST /rest/asset/v1/folder/{id}/tokens.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=April Fools&value=2015-04-01&type=date&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

La requête suivante met à jour un dossier :

```http
POST /rest/asset/v1/folder/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

Forms, les e-mails, les modèles d’e-mail, les pages de destination et les modèles de page de destination ont des structures plus complexes. Chaque type fournit un point d’entrée pour la création de la ressource et des points d’entrée supplémentaires pour la mise à jour de ses sections de métadonnées, de contenu et de contenu.

Ces ressources doivent être approuvées avant utilisation. Par exemple, créez une page de destination avec un ID de modèle, récupérez ses sections de contenu, mettez à jour chaque section requise, puis approuvez la page pour le déploiement.

### Création complexe

Créez une landing page à partir d’un modèle parent. La nouvelle page de destination contient le contenu par défaut du modèle pour chaque section.

```http
POST rest/asset/v1/landingPages.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

#### Obtenir les sections

Récupérez les sections de contenu de la page de destination. Mettez à jour chaque section qui doit être différente du modèle.

```http
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

#### Mettre à jour la section

```http
POST /rest/asset/v1/landingPage/{id}/content/{contentId}.json?type=Form&value=1
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#154ea32cf11",
    "result": [
        {
            "id": 174
        }
    ]
}
```

## Validation

Les e-mails, pages de destination, fragments de code, formulaires et leurs modèles utilisent un système de brouillon et d’approbation. Les mises à jour de contenu modifient le brouillon sans affecter la version active approuvée.

Le point d’entrée d’approbation valide le brouillon. Si la validation réussit, le brouillon remplace la version active et l’état de brouillon est effacé. Si la validation échoue, le point d’entrée renvoie la raison.

```http
POST /rest/asset/v1/emailTemplate/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"abe2#14a1832a97d",
   "result":[
      {
         "id":338,
         "name":"lvAVYMZqPS",
         "description":"fZLJQSJRvnYbjGTUpIHHqDOuQgQzXQcWIXoOUPwrVLdMHKcbRqwLoSLkWZTUmaMiCIJSfQiufnnrgITUIqjuAPBLpmliiKuIUFYG",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Approved",
         "workspace":"Default"
      }
   ]
}
```

L’approbation réussie remplace la version active précédente par la version mise à jour.

Chaque type de ressource pris en charge fournit un point d’entrée pour ignorer les brouillons. Pour une ressource approuvée avec un brouillon, ce point d’entrée ignore le brouillon et ses modifications en attente.

Le point d’entrée renvoie une erreur si la ressource n’a pas de version approuvée. Vous pouvez supprimer une ressource en mode brouillon uniquement, mais vous ne pouvez pas ignorer son brouillon.

```http
POST /rest/asset/v1/emailTemplate/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"17bfa#14a1832b3c4",
   "result":[
      {
         "id":344,
         "name":"LkilkvKrkp",
         "description":"yAyUEXuWMtdhpODUmnCkGjpBcyEKnYucxaSoTyYeQzyNbYanxCXWPOzwiIWmeXPUwjfGAUmgnxlhgOPluVqwNittuvxJmNTaHxYM",
         "createdAt":"2014-12-05T02:06:23Z+0000",
         "updatedAt":"2014-12-05T02:06:23Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

Vous pouvez annuler l’approbation d’une ressource dont le statut est défini sur Approuvé uniquement. L’annulation de l’approbation supprime la version active, ramène la ressource au statut brouillon uniquement et ignore tout brouillon associé.

Pour la plupart des types de ressources, la ressource ne doit pas être en cours d’utilisation. Par exemple, vous ne pouvez pas annuler l’approbation d’un e-mail référencé par une étape de flux Envoyer un e-mail ou un fragment de code incorporé dans un e-mail.

```http
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

## Supprimer

À l’exception des formulaires, les ressources aux états d’approbation et de brouillon doivent être désapprouvées avant suppression. En règle générale, une ressource doit également être inutilisée. Un dossier doit être vide.

Les programmes constituent une exception. Vous pouvez supprimer un programme et son contenu enfant si ni le programme ni son contenu ne sont utilisés en dehors du programme.

```http
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```

## Délais dépassés

Le délai d’expiration des API de ressources est de 300 secondes.
