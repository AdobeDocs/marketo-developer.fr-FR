---
title: Obtenir les données du visiteur
description: Obtenez l’identification des visiteurs en temps réel à l’aide de l’API de contexte utilisateur RTP avec des paramètres, un exemple de rappel et des exemples de réponses pour les segments, ABM et l’emplacement.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 4%

---

# Obtenir les données du visiteur

Cette méthode est utilisée pour obtenir des données d’identification des visiteurs en temps réel.

- Vous devez devenir client de Web Personalization et la balise [RTP doit être déployée](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- RTP ne prend pas en charge les listes de comptes nommés Marketing basées sur les comptes. Les listes et le code ABM ne concernent que les listes de comptes chargées (fichiers CSV) gérées dans RTP.

Si une erreur se produit, un message d’erreur s’affiche dans le cadre de la réponse JSON. Si un code 500 est renvoyé, contactez l’assistance pour la requête que vous avez effectuée.

| Paramètre | Facultatif/obligatoire | Type | Description |
|---|---|---|---|
| `get` | Obligatoire | Chaîne | Action de méthode. |
| `visitor` | Obligatoire | Chaîne | Nom de la méthode. |
| `callback` | Obligatoire | Fonction | Fonction de rappel à déclencher pour chaque campagne renvoyée. |

## Exemples

Obtention des données d’identification des visiteurs :

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Réponse avec correspondance de segments :

Vous trouverez ci-dessous un exemple de réponse qui est renvoyé si le visiteur correspondait à des segments en temps réel avant l’appel de l’API Get Visitor Data .

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

Vous trouverez ci-dessous un exemple de réponse renvoyée au cas où le visiteur ne correspondait à aucun segment en temps réel avant l’appel de l’API Get Visitor Data .

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
