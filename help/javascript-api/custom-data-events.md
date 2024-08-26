---
title: Événements de données personnalisés
description: Utilisez l’API JavaScript des événements de données personnalisés pour effectuer le suivi de vos événements uniques.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
source-git-commit: e609f9d5d58f656298412acef5e2106a19765396
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 3%

---

# Événements de données personnalisés

Cette méthode envoie des événements personnalisés pour le suivi et la personnalisation en temps réel. Il peut être utilisé pour envoyer des données tierces ou pour déclencher votre propre événement personnalisé en fonction du comportement du visiteur. Les événements de données personnalisés sont comptabilisés une fois dans la session d’un visiteur.

Vous devez devenir client Web Personalization et faire déployer la balise [RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---|---|---|---|
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

Le tableau de données personnalisé peut contenir, au maximum, quatre éléments.  Si vous devez envoyer plus de quatre éléments, appelez l’API Envoyer l’événement de manière répétée (avec un maximum de quatre éléments) jusqu’à ce que tous les éléments soient envoyés.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Envoyer l’événement en fonction du clic sur le bouton

Marketo personnalise le contenu de son site web pour les visiteurs web qui téléchargent un livre blanc spécifique. Pour ce faire, il capture le clic du visiteur sur le bouton de téléchargement du livre blanc, qui envoie un événement de données personnalisé. Segmente en temps réel tous les visiteurs qui ont cliqué sur le bouton de téléchargement du livre blanc, affichant à chaque visiteur une campagne personnalisée offrant 2 clics plus tard. Pour ce faire, affichez un autre élément de contenu lié au livre blanc téléchargé.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
