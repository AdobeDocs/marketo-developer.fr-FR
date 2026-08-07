---
title: Règles de redirection des pages de destination
feature: REST API, Landing Pages
description: Utilisez les API REST de ressources Marketo pour créer, interroger, mettre à jour et supprimer des règles de redirection de page de destination avec des filtres, une pagination, des options de nom d’hôte et des cibles autres que Marketo.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
TQID: https://experienceleague.adobe.com/2gePbKA3xeoRdnL8mNnObN-GPTX00Ii4-zcM0lBjs-o
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 596
ht-degree: 5%

---

# Règles de redirection de page de destination

[Référence Du Point D’Entrée Des Règles De Redirection De Page De Destination](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules)

Utilisez les API REST de règles de redirection de page de destination pour interroger, créer, mettre à jour et supprimer des URL de redirection de page de destination.

Les règles de redirection envoient une URL de page de destination à une autre URL de page. La source et la destination peuvent être des pages Marketo ou autres que Marketo. Pour consulter la documentation du produit connexe, voir la documentation de [](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=fr).

## Requête

Requête sur les règles de redirection des landing pages [par identifiant](#by_id) ou par [navigation](#browse).

### Par Id

Le point d’entrée [Obtenir les règles de redirection de la page de destination par ID](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageRedirectRuleByIdUsingGET) prend un paramètre de chemin d’`id` de la règle de redirection et renvoie l’enregistrement correspondant.

```http
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

Le point d’entrée [Get Landing Page Redirect Rules](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageRedirectRulesUsingGET) renvoie des enregistrements de règle de redirection de page de destination.

Utilisez des paramètres de requête facultatifs pour filtrer les résultats.

Le paramètre `offset` est un entier qui spécifie le nombre maximal d’entrées à renvoyer (la valeur par défaut est 20). La valeur maximale est 200. Le paramètre `maxReturn` est un entier qui spécifie où commencer à récupérer les entrées. Peut être utilisé conjointement avec le décalage (la valeur par défaut est 0).

Le paramètre `hostname` filtre par nom d’hôte de page de destination.

L’entier `redirectToLandingPageId` filtre par l’identifiant de page de destination. Le paramètre `redirectToPath` filtre le chemin d’accès à la page de destination.

Les paramètres `earliestUpdatedAt` et `latestUpdatedAt` définissent les limites de date et d’heure basse et haute. Le point d’entrée renvoie les règles créées ou mises à jour dans la plage.

```http
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

Appelez le point d’entrée [Créer une règle de redirection de page de destination](https://developer.adobe.com/marketo-apis/api/asset#operation/createLandingPageRedirectRuleUsingPOST) avec une requête POST `application/x-www-form-urlencoded`. La requête présente trois paramètres obligatoires.

Le paramètre `hostname` spécifie le nom d’hôte de la page de destination. Il doit appartenir à un domaine ou à un alias de marque et ne peut pas dépasser 255 caractères.

Le paramètre `redirectFrom` spécifie la page de destination source sous la forme d’un objet JSON avec une paire type/valeur. L’attribut `type` peut être `landingPageId` pour une page de destination Marketo ou `path` pour une page autre que Marketo.

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;get&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;visiteur&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| rappel | Obligatoire | Fonction | Fonction de rappel à déclencher pour chaque campagne renvoyée. |

Le paramètre `redirectTo` spécifie la destination sous la forme d’un objet JSON avec une paire type/valeur. L’attribut `type` peut être `landingPageId` pour une page de destination Marketo ou `url` pour une page autre que Marketo.

| Type de page de destination | type redirectTo | Exemple |
| --- | --- | --- |
| Marketo | landingPageId | {« type »:« landingPageId »,« value »:« 1774 »} |
| Non Marketo | url | {« type »:« url »,« value »:« www.contactLogs.com« } |

Pour plus d’informations, voir [Rediriger une page de destination Marketo vers une autre page](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

```http
POST /rest/asset/v1/redirectRules.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Le point d’entrée [Mettre à jour les règles de redirection de page de destination](https://developer.adobe.com/marketo-apis/api/asset#operation/updateLandingPageRedirectRuleUsingPOST) accepte un paramètre de chemin d’accès de `id` de règle de redirection. Envoyez la mise à jour sous la forme d’une requête POST `application/x-www-form-urlencoded`.

Transmettez un ou plusieurs de ces paramètres pour sélectionner les attributs à mettre à jour : `hostname`, `redirectFrom` ou `redirectTo`.

La réponse renvoie l’enregistrement de règle de redirection mis à jour.

```http
POST /rest/asset/v1/redirectRule/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Le point d’entrée [Supprimer la règle de redirection de la page de destination par ID](https://developer.adobe.com/marketo-apis/api/asset#operation/deleteLandingPageRedirectRuleUsingPOST) prend un paramètre de chemin d’`id` de la règle de redirection.

```http
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

Le point d’entrée [Get Landing Page Domains](https://developer.adobe.com/marketo-apis/api/asset#operation/getLandingPageDomainsUsingGET) renvoie des enregistrements de domaine de page de destination.

Utilisez deux paramètres de requête facultatifs pour filtrer les résultats.

Le paramètre `offset` est un entier qui spécifie le nombre maximal d’entrées à renvoyer (20 par défaut, 200 au maximum).

Le paramètre `maxReturn` est un entier qui spécifie où commencer à récupérer les entrées. Peut être utilisé conjointement avec `offset` (0 par défaut).

```http
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
