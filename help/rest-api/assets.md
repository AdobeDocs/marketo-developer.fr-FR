---
title: Ressources
feature: REST API
description: Une API pour travailler avec des ressources Marketo.
exl-id: 4273a5b1-1904-46e8-b583-fc6f46b388d2
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 2%

---

# Ressources

Marketo fournit des API pour interagir avec la plupart des ressources marketing et organisationnelles dans Marketo.

## Ressources

Les ressources Marketo incluent :

- Dossiers
- Programmes
- E-mails
- Modèles d&#39;e-mail
- Pages de destination
- Modèles de pages de destination
- Extraits
- Formulaires
- Jetons
- Fichiers

## API

Pour obtenir la liste complète des points d’entrée de l’API Asset, y compris les paramètres et les informations de modélisation, voir la [référence du point d’entrée de l’API Asset](endpoint-reference.md).

## Requête

Assets comporte généralement trois modèles par lesquels ils peuvent être récupérés : par identifiant, par nom et en naviguant.  Par identifiant et par nom, récupère une seule ressource pour un paramètre donné, tandis que la navigation renvoie et permet la pagination à travers toute la liste des ressources de ce type.  Les types de ressources individuels comportent des paramètres variables qui leur permettent de filtrer. Veillez donc à consulter leurs documents individuels pour plus de détails.

Dans certains cas, le point de terminaison de navigation de certains types de ressources ne renvoie pas de ressources enfants, telles que les valeurs autorisées pour une balise, et elles doivent être récupérées individuellement à l’aide du point de terminaison Par nom ou Par identifiant pour renvoyer l’ensemble complet des métadonnées.  D’autres peuvent avoir des points de fin distincts pour la récupération des objets dépendants tels que les champs de formulaire.

### Par identifiant

```
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

Pour des raisons techniques, les API Asset ne peuvent pas rechercher les noms de ressources contenant des virgules (,).  Il est recommandé que votre convention d’affectation des noms exclue les virgules pour tous les types de ressources.

```
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

L’exploration des ressources permet toujours deux paramètres de requête :

- offset : décalage entier duquel renvoyer les résultats.
- maxReturn : limite le nombre d’enregistrements renvoyés.  La valeur par défaut est de 20 si elle n’est pas définie et est de 200 au maximum.

```
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

Pour les types de ressources simples tels que les dossiers, les jetons et les fichiers, il n’existe généralement qu’un seul point de terminaison pour la création, puis un point de terminaison supplémentaire pour la mise à jour des enregistrements par identifiant.  Assets est créé avec un nom qui est toujours requis, puis les métadonnées et les identifiants sont renvoyés par la réponse de création ou de mise à jour.

Par exemple, voici comment créer un jeton :

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Pour mettre à jour un dossier, procédez comme suit :

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

D’autres ressources ont des structures plus complexes et nécessitent des mises à jour de sous-sections supplémentaires ou d’objets enfants, puis doivent finalement être approuvées avant de pouvoir être utilisées.  Ces types de ressources sont les suivants : Forms, Emails, modèles de courrier électronique, pages d’entrée et modèles de page d’entrée.  Chacun dispose d’un point de terminaison unique pour la création d’un enregistrement, puis de points de terminaison supplémentaires pour la mise à jour des sections de métadonnées, de contenu et de contenu.

Par exemple, pour créer une landing page, vous devez appeler son point de fin de création avec un identifiant de modèle, puis récupérer ses sections de contenu, et mettre à jour chacune d’elles individuellement pour ajouter du contenu, avant de le valider afin qu’il puisse être déployé en direct.

### Création complexe

Les landing pages nécessitent d’abord la création d’une ressource de landing page à l’aide d’un modèle parent.  Une nouvelle landing page contenant le contenu par défaut du modèle est ainsi créée pour chaque section de contenu.

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

#### Obtenir des sections

Pour renseigner le contenu d’une landing page, vous devez récupérer la liste des sections de contenu, puis effectuer des mises à jour individuelles pour toute section qui s’écarte du modèle.

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

#### Mettre à jour la section

```
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

De nombreux types de ressources sont associés à un système de brouillon et d’approbation, notamment les emails, les pages d’entrée, les fragments de code, Forms et leurs modèles correspondants.  Toute tentative d’approbation d’une ressource l’évalue par rapport à un ensemble spécifique de règles de validation, puis la définit sur un état approuvé ou renvoie une raison d’échec.  Pour ces types de ressources, chaque fois qu’une mise à jour est apportée au contenu d’une ressource spécifique, les modifications sont apportées à un brouillon de la ressource, ce qui n’affecte pas la version approuvée.  Cela permet d’apporter des modifications au contenu en toute sécurité sans affecter les versions actives de la ressource.  Les modifications peuvent ensuite être appliquées à la version en ligne à l’aide du point de terminaison de validation.  Cela efface également l’état de brouillon de la ressource jusqu’à ce que d’autres mises à jour soient appliquées.

```
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

L’abandon de brouillons est également disponible par le biais d’un point de terminaison pour chaque type de ressource valide.  Cette utilisation sur une ressource qui est à l’état approuvé avec brouillon annule le brouillon actuel et toutes les modifications en attente qu’elle a.  L’utilisation de cette méthode sur une ressource qui n’a actuellement pas de version approuvée ne fera rien et renverra une erreur.  Les ressources en version préliminaire uniquement peuvent être supprimées, mais elles ne peuvent pas être ignorées.

```
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

Assets peut également être non approuvé s’il est dans un état approuvé uniquement.  Cela supprime toutes les versions actives de la ressource et la renvoie à un état de brouillon uniquement, tout en ignorant le brouillon associé.  Cette action ne peut être effectuée que sur la plupart des ressources si elles ne sont utilisées nulle part dans Marketo, par exemple si un email est référencé à l’étape de flux Envoyer un courrier électronique ou si un fragment de code est incorporé dans un courrier électronique.

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

## Supprimer

Assets avec approbation et état de brouillon, à l’exception des formulaires, ne peut pas être supprimé lorsqu’il est approuvé et doit être non approuvé avant suppression.  En règle générale, les suppressions ne peuvent être effectuées que lorsqu’une ressource n’est pas validée et n’est pas utilisée et, dans le cas de dossiers, qu’elle est vide.  Une exception notable est les programmes, qui peuvent être supprimés avec l’ensemble de leurs contenus enfants, tant que le programme et son contenu ne sont pas utilisés en dehors des limites du programme.

```
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

## Délais d’expiration

Les API de ressources ont un délai d’expiration de 300 s.
