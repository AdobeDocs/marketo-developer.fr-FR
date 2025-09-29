---
title: Fichiers
feature: REST API
description: Guide de requête des fichiers d’API REST Marketo par identifiant ou nom, navigation avec dossier et décalage, création ou mise à jour par chargement multipartie, insertOnly, types MIME, pas de diffusion en continu
exl-id: 17361cdc-2309-442c-803c-34ce187aee1a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 1%

---

# Fichiers

[Référence des points d’entrée de fichiers](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files)

Les abonnements Marketo permettent de stocker des fichiers arbitraires tels que des images, des scripts, des documents et des feuilles de style. Toutes ces fonctionnalités peuvent être exploitées à distance via l’API REST. Le stockage disponible dans les abonnements Marketo n’est pas optimisé pour les applications gourmandes en bande passante. Par conséquent, d’autres solutions doivent être utilisées pour les applications de diffusion en continu audio et vidéo appropriées.

## Requête

La requête de fichiers est simple et suit les types de requête standard pour les ressources des catégories [par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files/operation/getFileByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files/operation/getFileByNameUsingGET) et [navigation](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files/operation/getFilesUsingGET).

### Par Id

```
GET /rest/asset/v1/file/{id}.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":147,
         "size":61346,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/rYXNeQFFVu",
         "folder":{
            "type":"Email",
            "id":10613
         },
         "name":"rYXNeQFFVu",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Par nom

Indiquez le nom du fichier à l’aide du paramètre `name` requis.

```
GET /rest/asset/v1/file/byName.json?name=foo.png
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9049#15918a76619",
    "result": [
        {
            "id": 46488,
            "size": 13987,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/foo.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images"
            },
            "name": "foo.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T22:16:58Z+0000",
            "updatedAt": "2015-05-06T22:19:29Z+0000"
        }
    ]
}
```

### Parcourir

Il existe trois paramètres facultatifs :

- folder - dossier parent spécifié comme bloc JSON contenant les attributs « id » et « type »
- offset - entier qui spécifie où commencer à récupérer les entrées (la valeur par défaut est 0) ; peut être utilisé avec le paramètre maxReturn
- maxReturn - entier spécifiant le nombre maximal d’entrées à renvoyer (20 par défaut, 200 au maximum)

```
GET /rest/asset/v1/files.json?folder={"id":436, "type": "Folder"}&maxReturn=3
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17e4e#14e23372d80",
    "result": [
        {
            "id": 46484,
            "size": 1454,
            "mimeType": "text/plain",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/websites.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "websites.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T20:15:58Z+0000",
            "updatedAt": "2015-06-22T02:12:36Z+0000"
        },
        {
            "id": 46486,
            "size": 4169,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/mobile.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "mobile.png",
            "description": null,
            "createdAt": "2015-05-06T22:13:33Z+0000",
            "updatedAt": "2015-05-06T22:13:33Z+0000"
        },
        {
            "id": 46488,
            "size": 13987,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/foo.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "foo.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T22:16:58Z+0000",
            "updatedAt": "2015-05-06T22:19:29Z+0000"
        }
    ]
}
```

## Créer et mettre à jour

[La création d’un fichier](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files/operation/createFileUsingPOST) s’effectue avec une requête de type multipart/form-data . Le nom, le dossier et le fichier sont obligatoires dans la requête, avec une description facultative et un indicateur insertOnly, ce qui empêche un appel de création de mettre à jour un fichier existant portant le même nom. Pour le paramètre de fichier, un « filename » est requis dans l’en-tête Content-Disposition, en plus du paramètre name. Vous devez également transmettre un en-tête Type de contenu pour le fichier , qui sera le type MIME que Marketo utilisera pour diffuser le fichier.

```
POST /rest/asset/v1/files.json
```

```
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="file"; filename="marketo.html"
Content-Type: text/html
<html>
<body>
<h1>Test Page - marketo.html</h1>
</body>
</html>
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="name"
marketo.html
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="folder"
{"id":436,"type":"Folder"}
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="description"
This is a test file
------WebKitFormBoundary2VyWOacQSupl4gUL—
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "278d#14e23316f63",
    "result": [
        {
            "id": 46960,
            "size": 69,
            "mimeType": "text/html",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/marketo.html",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "marketo.html",
            "description": "This is a test file",
            "createdAt": "2015-06-24T01:31:59Z+0000",
            "updatedAt": "2015-06-24T01:31:59Z+0000"
        }
    ]
}
```

[La mise à jour d’un fichier](https://developer.adobe.com/marketo-apis/api/asset/#tag/File-Contents/operation/updateContentUsingPOST) peut être effectuée en fonction de son identifiant. Le seul paramètre est un paramètre de fichier qui a les mêmes exigences que la création.

```
POST /rest/asset/v1/file/{id}/content.json
```

```
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="file"; filename="marketo.html"
Content-Type: text/html
<html>
<body>
<h1>Test Page - marketo.html</h1>
</body>
</html>
------WebKitFormBoundary2VyWOacQSupl4gUL--
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": null,
    "result": [
        {
            "id": 67,
            "size": 512000,
            "mimeType": "image/png",
            "url": "http://pages.devlocal.marketo.com/rs/test/assets/aLZiwCkXor",
            "folder": {
                "type": "Email",
                "id": 10391
            },
            "name": "aLZiwCkXor",
            "description": null,
            "createdAt": "2014-12-18T09:03:43Z+0000",
            "updatedAt": "2015-01-07T04:40:20Z+0000"
        }
    ]
}
```
