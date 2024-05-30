---
title: "Règles de redirection de page d’entrée"
feature: REST API, Landing Pages
description: "Configurez les règles de redirection de page d’entrée via l’API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 3%

---


# Règles de redirection de page de destination

[Référence du point de terminaison des règles de redirection de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo propose un ensemble d’API REST pour effectuer des opérations CRUD sur les URL de redirection de page d’entrée. Ces API suivent le modèle d’interface standard pour les API de ressources qui offrent les options Requête, Créer, Mettre à jour et Supprimer.

Les règles de redirection de page d’entrée permettent de rediriger une URL de page d’entrée vers une autre URL de page. Vous pouvez rediriger les landing pages Marketo, les landing pages non Marketo ou leurs combinaisons. Vous trouverez des informations supplémentaires sur les règles de page d’entrée de redirection. [here](https://experienceleague.adobe.com/docs/marketo/using/home.html).

## Requête

Les règles de redirection de page d’entrée interrogent les types de requête standard pour les ressources de [par id](#by_id), et [navigation](#browse).

### Par identifiant

La variable [Obtention des règles de redirection de page d’entrée par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) Le point de terminaison effectue une redirection de règle de page d’entrée unique. `id` paramètre path et renvoie un seul enregistrement de règle de redirection de page d’entrée.

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

La variable [Obtention des règles de redirection de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) endpoint renvoie une liste d’enregistrements de règle de redirection de page d’entrée.

Plusieurs paramètres de requête facultatifs peuvent être transmis pour filtrer les résultats.

La variable `offset` est un entier qui spécifie le nombre maximal d’entrées à renvoyer (la valeur par défaut est 20). 200 au maximum. La variable `maxReturn` est un entier qui spécifie où commencer la récupération des entrées. Peut être utilisé conjointement avec le décalage (la valeur par défaut est 0).

La variable `hostname` peut être utilisé pour filtrer selon le nom d’hôte des landing pages.

La variable `redirectToLandingPageId` est un entier qui peut être utilisé pour filtrer selon l’identifiant de la page d’entrée vers laquelle vous redirigez. La variable `redirectToPath` peut être utilisé pour filtrer selon le chemin des landing pages vers lesquelles vous redirigez.

La variable `earliestUpdatedAt` et `latestUpdatedAt` Les paramètres vous permettent de définir des filigranes datetime bas et élevé pour renvoyer les règles de redirection de page d’entrée qui ont été mises à jour ou créées initialement dans la plage donnée.

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

La variable [Créer une règle de redirection de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) endpoint est exécuté avec un POST application/x-www-form-urlencoded qui possède les trois paramètres requis suivants.

La variable `hostname` spécifie le nom d’hôte de la landing page. Cela doit appartenir à un domaine de marque ou à un alias. La longueur maximale est de 255 caractères.

La variable `redirectFrom` spécifie la landing page source. Il s’agit d’un objet JSON qui contient une paire type/valeur qui détermine si la source est une page d’entrée Marketo ou une page d’entrée non Marketo. La variable `type` peut être &quot;landingPageId&quot; ou &quot;path&quot;.

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---|---|---|---|
| &#39;get&#39; | Requis | Chaîne | Action de méthode. |
| &quot;visitor&quot; | Requis | Chaîne | Nom de la méthode. |
| callback | Requis | Fonction | Fonction de rappel à déclencher pour chaque campagne renvoyée. |


La variable `redirectTo` spécifie la landing page cible. Il s’agit d’un objet JSON qui contient une paire type/valeur qui détermine si la source est une page d’entrée Marketo ou une page d’entrée non Marketo. La variable `type` peut être &quot;landingPageId&quot; ou &quot;url&quot;.

| Type de page d’entrée | redirectTo type | Exemple |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Non Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Vous trouverez plus d’informations sur la création de règles de redirection de landing page [here](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

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

La variable [Mettre à jour les règles de redirection de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) Le point de terminaison utilise une règle de redirection de page d’entrée unique. `id` paramètre path . Ce point de terminaison est exécuté avec un POST encodé application/x-www-form-urlencoded.

Comme pour l’appel de création décrit ci-dessus, un ou plusieurs des paramètres de requête suivants sont transmis pour spécifier l’attribut de la règle à mettre à jour : `hostname`, `redirectFrom`, `redirectTo`.

L’enregistrement de règle de redirection de page d’entrée mis à jour est renvoyé dans la réponse.

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

La variable [Supprimer la règle de redirection de page d’entrée par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) Le point de terminaison effectue une redirection de règle de page d’entrée unique. `id` paramètre path .

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

## Parcourir les domaines de la page d’entrée

La variable [Obtention des domaines de page d’entrée](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) endpoint renvoie une liste d’enregistrements de domaine de page d’entrée.

Deux paramètres de requête facultatifs peuvent être transmis pour filtrer les résultats.

La variable `offset` est un entier qui spécifie le nombre maximal d’entrées à renvoyer (la valeur par défaut est 20, la valeur maximale est 200).

La variable `maxReturn` est un entier qui spécifie où commencer la récupération des entrées. Peut être utilisé conjointement avec `offset` (la valeur par défaut est 0).

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
