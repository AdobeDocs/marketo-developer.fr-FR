---
title: Obtention des données du visiteur
description: Obtention des données du visiteur
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 5%

---

# Obtention des données du visiteur

Cette méthode est utilisée pour obtenir des données d’identification des visiteurs en temps réel.

- Vous devez devenir client Web Personalization et faire déployer la balise [RTP](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- Le protocole RTP ne prend pas en charge les listes de comptes nommés Marketing basé sur un compte. Les listes ABM et le code ne se rapportent qu’aux listes de comptes téléchargées (fichiers CSV) gérées dans RTP.

Si une erreur se produit, un message d’erreur s’affiche dans le cadre de la réponse JSON. Si un code 500 est renvoyé, contactez l’assistance avec la demande que vous avez effectuée.

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---|---|---|---|
| `get` | Requis | Chaîne | Action de méthode. |
| `visitor` | Requis | Chaîne | Nom de la méthode. |
| `callback` | Requis | Fonction | Fonction de rappel à déclencher pour chaque campagne renvoyée. |

## Exemples

Obtention des données d’identification des visiteurs :

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Réponse avec la correspondance de segment :

Vous trouverez ci-dessous un exemple de réponse qui est renvoyée si le visiteur correspondait à des segments en temps réel avant l’appel de l’API Get Visitor Data.

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

Vous trouverez ci-dessous un exemple de réponse qui est renvoyée si le visiteur ne correspond à aucun segment en temps réel avant l’appel de l’API Get Visitor Data.

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
