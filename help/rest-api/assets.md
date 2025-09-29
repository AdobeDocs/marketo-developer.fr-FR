---
title: Ressources
feature: REST API
description: Présentation des API REST de ressources Marketo pour les requêtes par identifiant ou nom, la navigation dans les pages et la création ou la mise à jour de dossiers, d’e-mails, de formulaires, de modèles, de fichiers et de jetons.
exl-id: 4273a5b1-1904-46e8-b583-fc6f46b388d2
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 2%

---

# Ressources

Marketo fournit des API pour interagir avec la plupart des ressources marketing et organisationnelles dans Marketo.

## Ressources

Les ressources Marketo incluent :

- Dossiers
- Programmes
- E-mails
- Modèles d’e-mail
- Pages de destination
- Modèles de pages de destination
- Extraits
- Formulaires
- Jetons
- Fichiers

## API

Pour obtenir la liste complète des points d’entrée de l’API Asset, y compris les paramètres et les informations de modélisation, consultez la [ Référence des points d’entrée de l’API Asset ](endpoint-reference.md).

## Requête

Assets comporte généralement trois modèles permettant de les récupérer : par identifiant, par nom et par navigation.  Les options Par id et Par nom récupèrent toutes deux une seule ressource pour un paramètre donné, tandis que l’exploration renvoie et permet la pagination de l’ensemble de la liste des ressources de ce type.  Les différents types de ressources sont dotés de paramètres variables selon lesquels ils peuvent être filtrés. Veillez donc à consulter individuellement leurs documents pour obtenir des informations spécifiques.

Dans certains cas, le point d’entrée de navigation de certains types de ressources ne renvoie pas les ressources enfants, telles que les valeurs autorisées pour une balise, et elles doivent être récupérées individuellement à l’aide du point d’entrée Par nom ou Par ID pour renvoyer l’ensemble complet des métadonnées.  D’autres peuvent avoir des points d’entrée distincts entièrement pour récupérer des objets dépendants tels que des champs de formulaire.

### Par Id

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

Pour des raisons techniques, les API de ressources ne peuvent pas rechercher des noms de ressources contenant des virgules (,).  Il est recommandé d’exclure les virgules de votre convention de nommage pour tous les types de ressources.

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

La navigation dans les ressources autorise toujours deux paramètres de requête :

- offset : décalage entier à partir duquel renvoyer les résultats.
- maxReturn - Limite le nombre d&#39;enregistrements retournés.  La valeur par défaut est 20 s’il n’est pas défini, et est de 200 au maximum.

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

Pour les types de ressources simples tels que les dossiers, les jetons et les fichiers, il n’existe généralement qu’un seul point d’entrée pour la création, puis un point d’entrée supplémentaire pour la mise à jour des enregistrements par identifiant.  Les Assets sont créées avec un nom qui est toujours obligatoire, puis toutes les métadonnées et tous les identifiants sont renvoyés par la réponse create ou update.

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

Les autres ressources ont des structures plus complexes et nécessitent des mises à jour de sous-sections ou d’objets enfants supplémentaires. Elles doivent ensuite être approuvées avant de pouvoir être utilisées.  Ces types de ressources comprennent Forms, les e-mails, les modèles d’e-mail, les pages de destination et les modèles de page de destination.  Ils disposeront chacun d’un point d’entrée unique pour créer un enregistrement, puis de points d’entrée supplémentaires pour mettre à jour les sections de métadonnées, de contenu et de contenu.

Par exemple, pour créer une page de destination, vous devez appeler son point d’entrée de création avec un identifiant de modèle, puis récupérer ses sections de contenu et les mettre à jour individuellement pour ajouter du contenu, avant de les approuver afin qu’elles puissent être déployées en direct.

### Création complexe

Les pages de destination nécessitent d’abord de créer une ressource de page de destination à l’aide d’un modèle parent.  Cela crée une page de destination contenant le contenu par défaut du modèle pour chaque section de contenu.

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

#### Obtenir les sections

Pour renseigner le contenu d&#39;une landing page, vous devez récupérer la liste des sections de contenu, puis effectuer des mises à jour individuelles pour toute section qui s&#39;écarte du modèle.

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

De nombreux types de ressources sont associés à un système de brouillon et d’approbation, notamment les e-mails, les pages de destination, les fragments de code, le Forms et les modèles correspondants.  Toute tentative d’approbation d’une ressource l’évalue par rapport à un ensemble spécifique de règles de validation, puis la définit sur un état approuvé ou renvoie un motif d’échec.  Pour ces types de ressources, chaque fois qu’une mise à jour est apportée au contenu d’une ressource particulière, les modifications sont apportées à un brouillon de la ressource, ce qui n’affecte pas la version approuvée.  Cela permet d’apporter des modifications au contenu en toute sécurité sans affecter les versions actives de la ressource.  Les modifications peuvent ensuite être appliquées à la version active à l’aide du point d’entrée d’approbation.  Cela efface également l’état de brouillon de la ressource jusqu’à ce que d’autres mises à jour soient appliquées.

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

L’abandon des brouillons est également disponible via un point d’entrée pour chaque type de ressource valide.  L’utilisation de cette option sur une ressource dont le statut est Approuvé avec le statut de brouillon supprime le brouillon actuel et toutes les modifications en attente.  L’utilisation de cette option sur une ressource qui n’a actuellement aucune version approuvée n’aura aucun effet et renverra une erreur.  Les ressources en mode Brouillon uniquement peuvent être supprimées, mais elles ne peuvent pas être ignorées.

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

L’approbation d’Assets peut également être annulée si le statut de ces applications est uniquement approuvé.  Cette opération supprimera toutes les versions actives de la ressource et rétablira l’état de la ressource en mode brouillon uniquement, tout en ignorant les brouillons associés.  Cette action ne peut être effectuée sur la plupart des ressources que si elle n’est utilisée nulle part dans Marketo, par exemple lorsqu’un e-mail est référencé dans une étape de flux Envoyer un e-mail ou qu’un fragment de code est incorporé dans un e-mail.

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

Assets avec les états approbation et brouillon, à l’exception des formulaires, ne peut pas être supprimé lors de l’approbation et doit être non approuvé avant suppression.  Les suppressions ne peuvent généralement être effectuées que lorsqu’une ressource n’est pas approuvée et n’est pas utilisée et, dans le cas des dossiers, lorsqu’elle est vide.  Les programmes constituent une exception notable : ils peuvent être supprimés ainsi que l’ensemble de leurs contenus enfants, à condition que le programme et son contenu ne soient pas utilisés en dehors des limites du programme.

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

## Délais dépassés

Le délai d’expiration des API de ressources est de 300
