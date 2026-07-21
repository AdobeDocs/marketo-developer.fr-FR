---
title: Événements de données personnalisés
description: Envoyez des événements personnalisés avec l’API JavaScript RTP pour Web Personalization, avec des paramètres, des données de chaîne ou de tableau allant jusqu’à quatre éléments, et des déclencheurs basés sur les clics.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
TQID: https://experienceleague.adobe.com/oWDmtMF94xG5HYXeTwkx5zF9PWo98bpwoVB6kAKLYDo
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 241
ht-degree: 3%

---

# Événements de données personnalisés

Utilisez cette méthode pour envoyer des événements personnalisés pour le suivi et la personnalisation en temps réel. Vous pouvez envoyer des données tierces ou déclencher un événement personnalisé en fonction du comportement des visiteurs.

Chaque événement de données personnalisé est comptabilisé une fois au cours de la session d’un visiteur.

Vous devez être client de Web Personalization et avoir déployé la balise [RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| `send` | Obligatoire | Chaîne | Action de méthode. |
| `event` | Obligatoire | Chaîne | Nom de la méthode. |
| `customData` | Obligatoire | Chaîne ou tableau | Données personnalisées. |

## Exemples

### Envoyer l’événement à l’aide de la chaîne pour les données personnalisées

```javascript
var customData = {value: 'MyEvent'};
rtp('send', 'event', customData);
```

### Envoyer l’événement à l’aide d’un tableau de chaînes pour les données personnalisées

Le tableau de données personnalisé peut contenir jusqu’à quatre éléments. Pour envoyer plus de quatre éléments, appelez plusieurs fois l’API d’événement d’envoi sans dépasser quatre éléments à chaque appel.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Envoyer l’événement en fonction du clic sur le bouton

Cet exemple montre comment envoyer un événement de données personnalisé lorsqu’un visiteur sélectionne le bouton permettant de télécharger un article technique spécifique. RTP peut utiliser l’événement pour segmenter ces visiteurs en temps réel.

Après deux clics supplémentaires, le site web peut afficher une campagne personnalisée. Par exemple, la campagne peut présenter un autre élément de contenu lié au livre blanc téléchargé.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
