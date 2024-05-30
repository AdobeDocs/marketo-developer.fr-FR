---
title: "Triggers"
description: "Triggers"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 11%

---


# Déclencheurs

Ajoute la possibilité de déclencher des fonctions sur certains états de l’objet rtp global.

Vous devez être un client de personnalisation Web et disposer de la variable [Balise RTP déployée](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

## Utilisation

`rtp('triggerName', function_to_trigger);`

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---------------------|-------------------|----------|----------------------|
| &#39;triggerName&#39; | Requis | Chaîne | Nom de la méthode. |
| function_to_trigger | Requis | Fonction | Fonction à déclencher. |


### Déclencheur prêt pour le contexte utilisateur

Définit une variable personnalisée en fonction de l’emplacement de l’utilisateur. Cette fonction est appelée lorsque l’objet global &quot;rtpUserContext&quot; est prêt.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
