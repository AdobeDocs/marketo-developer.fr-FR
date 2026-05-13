---
title: Correspondance de motifs
description: Utilisez l’utilitaire RTP rtp.checkPattern pour tester les modèles de chaîne avec des caractères génériques de pourcentage, voir les limites de synchronisation, des exemples d’utilisation et d’URL et la configuration requise des balises RTP.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
TQID: https://experienceleague.adobe.com/-HopUg6-2EchL9kJrPDbz62mRlrqYaXYdufILjkvP1Y
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
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 171
ht-degree: 5%

---

# Correspondance de motifs

RTP expose une fonction utilitaire pour vérifier si le motif correspond à une certaine chaîne. L’utilitaire ne peut pas être utilisé en mode asynchrone, car il renvoie une indication s’il existe une correspondance ou non.

Vous devez devenir client de Web Personalization et la balise [RTP doit être déployée](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

## Utilisation

> rtp.checkPattern(check_against, pattern);

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| check_against | Obligatoire | Chaîne | Chaîne avec laquelle comparer le motif. Par exemple : URL de la page actuelle, nom du produit. |
| pattern | Obligatoire | Chaîne | Ajoutez % pour le caractère générique. Le modèle peut être:start avec fin contenant une correspondance complète |

## Exemples

Définissez la variable personnalisée dans l’index 1 si l’URL de la page actuelle se termine par « productA ».

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Le chemin d’URL actuel est « /products/productB ». Cet exemple montre comment vérifier si le chemin d’accès contient « products » et définir une variable personnalisée.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
