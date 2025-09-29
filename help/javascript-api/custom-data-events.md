---
title: Événements de données personnalisés
description: Envoyez des événements personnalisés avec l’API JavaScript RTP pour Web Personalization, avec des paramètres, des données de chaîne ou de tableau allant jusqu’à quatre éléments, et des déclencheurs basés sur les clics.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 3%

---

# Événements de données personnalisés

Cette méthode envoie des événements personnalisés pour le suivi et la personnalisation en temps réel. Il peut être utilisé pour envoyer des données tierces ou pour déclencher votre propre événement personnalisé en fonction du comportement du visiteur. Les événements de données personnalisées sont comptabilisés une fois dans la session d’un visiteur.

Vous devez devenir client de Web Personalization et la balise [RTP doit être déployée](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

| Paramètre | Facultatif/obligatoire | Type | Description |
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

Le tableau de données personnalisé peut contenir un maximum de quatre éléments.  Si vous devez envoyer plus de quatre éléments, appelez plusieurs fois l’API d’événement d’envoi (avec un maximum de quatre éléments) jusqu’à ce que tous les éléments soient envoyés.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Envoyer l’événement en fonction du clic sur le bouton

Marketo personnalise le contenu de son site web pour les visiteurs et visiteuses web qui téléchargent un article technique spécifique. Pour ce faire, ils capturent le clic du visiteur sur le bouton de téléchargement du livre blanc, qui envoie un événement de données personnalisé. Segments RTP en temps réel de tous les visiteurs qui ont cliqué sur le bouton Télécharger le livre blanc, présentant à chaque visiteur une campagne personnalisée offrant 2 clics plus tard. Pour ce faire, affichez un autre élément de contenu lié au livre blanc téléchargé.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
