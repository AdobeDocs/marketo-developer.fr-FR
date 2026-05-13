---
title: Déclencheurs
description: Utilisez des déclencheurs RTP dans Web Personalization pour exécuter des fonctions à l’état rtp, y compris userContextReady, avec une syntaxe, des paramètres et un exemple d’emplacement.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
TQID: https://experienceleague.adobe.com/yTz9i4bnD4I0PDAmpnjdD1okYJzd40wriA-2ZzO5OMM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 116
ht-degree: 8%

---

# Déclencheurs

Ajoute la possibilité de déclencher des fonctions sur certains états de l’objet rtp global.

Vous devez être client de Web Personalization et avoir déployé la balise [RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

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
