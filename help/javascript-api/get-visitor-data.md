---
title: Obtenir les données du visiteur
description: Obtenez l’identification des visiteurs en temps réel à l’aide de l’API de contexte utilisateur RTP avec des paramètres, un exemple de rappel et des exemples de réponses pour les segments, ABM et l’emplacement.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
TQID: https://experienceleague.adobe.com/B-JMACtMs3aRVsb1eJKaRoQGgVKB6MTbd0KBoZ7Ay6k
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 214
ht-degree: 4%

---

# Obtenir les données du visiteur

Utilisez cette méthode pour obtenir des données d’identification des visiteurs en temps réel.

- Vous devez être client de Web Personalization et avoir déployé la balise [RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- RTP ne prend pas en charge les listes de comptes nommés Marketing basées sur les comptes. Les listes et le code ABM ne concernent que les listes de comptes chargées (fichiers CSV) gérées dans RTP.

Si une erreur se produit, le fichier JSON de réponse inclut un message d’erreur. Si l’API renvoie un code 500, contactez l’assistance et indiquez la requête qui a provoqué l’erreur.

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| `get` | Obligatoire | Chaîne | Action de méthode. |
| `visitor` | Obligatoire | Chaîne | Nom de la méthode. |
| `callback` | Obligatoire | Fonction | Fonction de rappel à déclencher pour chaque campagne renvoyée. |

## Exemples

L’exemple suivant récupère les données d’identification des visiteurs.

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Réponse avec correspondance de segments :

La réponse suivante inclut des `matchedSegments`, car le visiteur correspondait à des segments en temps réel avant l’appel de l’API Get Visitor Data.

```json
{
    "status": 200,
    "results": {
        "matchedSegments": [
            {
                "name": "first click",
                "id": 177
            }
        ],
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```

Réponse sans correspondance de segment :

La réponse suivante n’inclut pas `matchedSegments`, car le visiteur ne correspondait à aucun segment en temps réel avant l’appel de l’API Get Visitor Data.

```json
{
    "status": 200,
    "results": {
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```
