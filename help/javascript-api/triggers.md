---
title: Déclencheurs
description: Utilisez des déclencheurs RTP dans Web Personalization pour exécuter des fonctions à l’état rtp, y compris userContextReady, avec une syntaxe, des paramètres et un exemple d’emplacement.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 6145067629ce78175af3b7464807a0fa100c7b57
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 8%

---

# Déclencheurs

Ajoute la possibilité de déclencher des fonctions sur certains états de l’objet rtp global.

Vous devez être client de Web Personalization et avoir déployé la balise [RTP](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

## Utilisation

`rtp('triggerName', function_to_trigger);`

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;triggerName&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| function_to_trigger | Obligatoire | Fonction | Fonction à déclencher. |

### Déclencheur prêt pour le contexte utilisateur

Définit une variable personnalisée en fonction de l’emplacement de l’utilisateur. Cette fonction est appelée lorsque l’objet global « rtpUserContext » est prêt.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
