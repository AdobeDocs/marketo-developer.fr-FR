---
title: Correspondance de motifs
description: Utilisez l’utilitaire RTP rtp.checkPattern pour tester les modèles de chaîne avec des caractères génériques de pourcentage, voir les limites de synchronisation, des exemples d’utilisation et d’URL et la configuration requise des balises RTP.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
TQID: https://experienceleague.adobe.com/-HopUg6-2EchL9kJrPDbz62mRlrqYaXYdufILjkvP1Y
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 188
ht-degree: 5%

---

# Correspondance de motifs

RTP fournit une fonction utilitaire qui vérifie si un modèle correspond à une chaîne. L’utilitaire renvoie un résultat de correspondance de manière synchrone et ne peut pas être utilisé de manière asynchrone.

Vous devez être client de Web Personalization et avoir déployé la balise [RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

## Utilisation

> rtp.checkPattern(check_against, pattern);

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| check_against | Obligatoire | Chaîne | Chaîne en fonction de laquelle comparer le modèle, par exemple l’URL de la page active ou un nom de produit. |
| pattern | Obligatoire | Chaîne | Motif à faire correspondre. Ajoutez `%` comme caractère générique pour correspondre au début, à la fin ou au contenu d’une chaîne. Omettez `%` pour une correspondance complète. |

## Exemples

Cet exemple définit une variable personnalisée à l’index 1 lorsque l’URL de la page active se termine par « productA ».

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Dans l’exemple suivant, le chemin d’URL actuel est « /products/productB ». L’exemple vérifie si le chemin contient « products », puis définit une variable personnalisée.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
