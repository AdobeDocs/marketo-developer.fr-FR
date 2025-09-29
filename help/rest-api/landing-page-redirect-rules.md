---
title: Règles de redirection de page de destination
feature: REST API, Landing Pages
description: Utilisez les API REST de ressources Marketo pour créer, interroger, mettre à jour et supprimer des règles de redirection de page de destination avec des filtres, une pagination, des options de nom d’hôte et des cibles autres que Marketo.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 3%

---

# Règles de redirection de page de destination

[Référence du point d’entrée des règles de redirection de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo propose un ensemble d’API REST pour effectuer des opérations CRUD sur des URL de redirection de page de destination. Ces API suivent le modèle d’interface standard des API de ressources en fournissant les options Requête, Créer, Mettre à jour et Supprimer .

Les règles de redirection de page de destination permettent de rediriger une URL de page de destination vers une autre URL de page. Vous pouvez rediriger des pages de destination Marketo, des pages de destination autres que Marketo ou des combinaisons de celles-ci. Des informations supplémentaires sur les règles de page de destination de redirection sont disponibles [ici](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=fr).

## Requête

La requête aux règles de redirection de page de destination suit les types de requête standard pour les ressources de [par ID](#by_id) et [navigation](#browse).

### Par Id

Le point d’entrée [Obtenir les règles de redirection de la page de destination par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) prend un paramètre de chemin d’`id` de redirection de règle de page de destination unique et renvoie un enregistrement de règle de redirection de page de destination unique.

```
GET /rest/asset/v1/redirectRule/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d0#1707b2521e4",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

### Parcourir

Le point d’entrée [Get Landing Page Redirect Rules](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) renvoie une liste d’enregistrements de règle de redirection de page de destination.

Plusieurs paramètres de requête facultatifs peuvent être transmis pour filtrer les résultats.

Le paramètre `offset` est un entier qui spécifie le nombre maximal d’entrées à renvoyer (la valeur par défaut est 20). La valeur maximale est 200. Le paramètre `maxReturn` est un entier qui spécifie où commencer à récupérer les entrées. Peut être utilisé conjointement avec le décalage (la valeur par défaut est 0).

Le paramètre `hostname` peut être utilisé pour filtrer selon le nom d’hôte des pages de destination.

Le `redirectToLandingPageId` est un entier qui peut être utilisé pour filtrer selon l’identifiant de la page de destination vers laquelle vous effectuez une redirection. Le `redirectToPath` peut être utilisé pour filtrer selon le chemin d’accès des pages de destination vers lesquelles vous effectuez une redirection.

Les paramètres `earliestUpdatedAt` et `latestUpdatedAt` vous permettent de définir des filigranes de date/heure bas et élevés pour le renvoi des règles de redirection de page de destination qui ont été mises à jour ou créées initialement dans la plage donnée.

```
GET /rest/asset/v1/redirectRules.json&maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "12213#1707b27efb5",
    "warnings": [],
    "result": [
        {
            "id": 5,
            "redirectFromUrl": "https://www.kirtideep.contact/LandingPage2.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5406
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:26:29Z+0000",
            "updatedAt": "2019-11-14T06:26:29Z+0000"
        },
        {
            "id": 6,
            "redirectFromUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectTo": {
                "type": "url",
                "value": "www.contactLogs.com"
            },
            "redirectToUrl": "www.contactLogs.com",
            "createdAt": "2019-11-14T06:27:10Z+0000",
            "updatedAt": "2019-11-14T06:27:10Z+0000"
        },
        {
            "id": 7,
            "redirectFromUrl": "https://www.kirtideep.contact/contact/log/check",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "path",
                "value": "/contact/log/check"
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:27:49Z+0000",
            "updatedAt": "2019-11-14T06:27:49Z+0000"
        }
    ]
}
```

## Créer

Le point d’entrée [Créer une règle de redirection de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) est exécuté avec une requête POST application/x-www-form-urlencoded qui présente les trois paramètres obligatoires suivants.

Le paramètre `hostname` spécifie le nom d’hôte de la page de destination. Il doit appartenir à un domaine ou à un alias de marque. La longueur maximale est de 255 caractères.

Le paramètre `redirectFrom` spécifie la page de destination source. Il s’agit d’un objet JSON qui contient une paire type/valeur qui détermine si la source est une page de destination Marketo ou une page de destination autre que Marketo. L’attribut `type` peut être « landingPageId » ou « path ».

| Paramètre | Facultatif/obligatoire | Type | Description |
|---|---|---|---|
| &#39;get&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;visiteur&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| rappel | Obligatoire | Fonction | Fonction de rappel à déclencher pour chaque campagne renvoyée. |

Le paramètre `redirectTo` spécifie la page de destination cible. Il s’agit d’un objet JSON qui contient une paire type/valeur qui détermine si la source est une page de destination Marketo ou une page de destination autre que Marketo. L’attribut `type` peut être « landingPageId » ou « url ».

| Type de page de destination | type redirectTo | Exemple |
|---|---|---|
| Marketo | landingPageId | {« type »:« landingPageId »,« value »:« 1774 »} |
| Non Marketo | url | {« type »:« url »,« value »: »www.contactLogs.com« } |

Vous trouverez plus d’informations sur la création de règles de redirection de page de destination [ici](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html?lang=fr).

```
POST /rest/asset/v1/redirectRules.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
hostname=calqeauto.com&redirectFrom={"type":"landingPageId", "value":"5483"}&redirectTo={"type":"landingPageId", "value":"5559"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d7c6#1707b223522",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

## Mise à jour 

Le point d’entrée [Mettre à jour les règles de redirection de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) prend une règle de redirection de page de destination unique `id` un paramètre de chemin d’accès. Ce point d’entrée est exécuté avec une requête POST application/x-www-form-urlencoded.

Comme pour l’appel de création décrit ci-dessus, un ou plusieurs des paramètres de requête suivants sont transmis pour spécifier l’attribut de la règle à mettre à jour : `hostname`, `redirectFrom`, `redirectTo`.

L’enregistrement de règle de redirection de page de destination mis à jour est renvoyé dans la réponse.

```
POST /rest/asset/v1/redirectRule/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
redirectTo={"type":"landingPageId", "value":"5561"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "57b2#1707b3852d7",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5561
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage3.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T07:20:53Z+0000"
        }
    ]
}
```

## Supprimer

Le point d’entrée [Supprimer la règle de redirection de la page de destination par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) prend un seul paramètre de chemin d’`id` de redirection de règle de page de destination.

```
POST /rest/asset/v1/redirectRule/{id}/delete.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "d505#154d01c8364",
  "result": [
    {
      "id": 2
    }
  ]
}
```

## Parcourir Les Domaines Des Pages De Destination

Le point d’entrée [Obtenir les domaines de la page de destination](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) renvoie une liste d’enregistrements de domaine de la page de destination.

Deux paramètres de requête facultatifs peuvent être transmis pour filtrer les résultats.

Le paramètre `offset` est un entier qui spécifie le nombre maximal d’entrées à renvoyer (20 par défaut, 200 au maximum).

Le paramètre `maxReturn` est un entier qui spécifie où commencer à récupérer les entrées. Peut être utilisé conjointement avec `offset` (0 par défaut).

```
POST /rest/asset/v1/landingPageDomains.json?maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6eb8#1707b43d3cb",
    "warnings": [],
    "result": [
        {
            "hostname": "calqeauto.com",
            "type": "domain"
        },
        {
            "hostname": "www.google.com",
            "type": "domain-alias"
        },
        {
            "hostname": "www.kirti.com",
            "type": "domain-alias"
        }
    ]
}
```
