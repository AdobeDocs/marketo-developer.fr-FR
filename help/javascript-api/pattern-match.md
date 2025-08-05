---
title: Correspondance de motifs
description: Correspondance de motifs
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 7%

---

# Correspondance de motifs

RTP expose une fonction utilitaire pour vérifier si le motif correspond à une certaine chaîne. L’utilitaire ne peut pas être utilisé en mode asynchrone, car il renvoie une indication s’il existe une correspondance ou non.

Vous devez devenir client de Web Personalization et la balise [RTP doit être déployée](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

## Utilisation

> rtp.checkPattern(check_against, pattern);

| Paramètre | Facultatif/obligatoire | Type | Description |
|---|---|---|---|
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
