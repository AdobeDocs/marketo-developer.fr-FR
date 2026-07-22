---
title: Déclencheurs
description: Utilisez des déclencheurs RTP dans Web Personalization pour exécuter des fonctions à l’état rtp, y compris userContextReady, avec une syntaxe, des paramètres et un exemple d’emplacement.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
TQID: https://experienceleague.adobe.com/yTz9i4bnD4I0PDAmpnjdD1okYJzd40wriA-2ZzO5OMM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 117
ht-degree: 8%

---

# Déclencheurs

Les déclencheurs exécutent des fonctions lorsque l’objet `rtp` global atteint un état spécifié.

Vous devez être client de Web Personalization et avoir déployé la balise [RTP](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

## Utilisation

`rtp('triggerName', function_to_trigger);`

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;triggerName&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| function_to_trigger | Obligatoire | Fonction | Fonction à déclencher. |

### Déclencheur prêt pour le contexte utilisateur

Le déclencheur `userContextReady` appelle une fonction lorsque l’objet `rtpUserContext` global est prêt. L’exemple suivant définit une variable personnalisée en fonction de l’emplacement de l’utilisateur.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
