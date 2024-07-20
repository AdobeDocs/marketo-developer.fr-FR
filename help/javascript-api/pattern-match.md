---
title: Correspondance de motif
description: Correspondance de motif
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 6%

---

# Correspondance de motif

RTP expose une fonction d’utilitaire pour vérifier si le modèle correspond à certaines chaînes. L’utilitaire ne peut pas être utilisé en mode asynchrone, car il renvoie une indication s’il existe une correspondance ou non.

Vous devez devenir client Web Personalization et faire déployer la balise [RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

## Utilisation

> rtp.checkPattern(check_against, pattern);

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---|---|---|---|
| check_against | Requis | Chaîne | Chaîne correspondant au modèle. Par exemple : URL de la page active, nom du produit. |
| pattern | Requis | Chaîne | Ajoutez % pour le caractère générique. Le modèle peut être le suivant : commencer par avec des conteneurs avec une correspondance complète |


## Exemples

Définissez la variable personnalisée dans l’index 1 si l’URL de la page active se termine par &quot;productA&quot;.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Le chemin d’accès à l’URL actuelle est &quot;/products/productB&quot;. Cet exemple vérifie si le chemin contient &quot;products&quot; et définit une variable personnalisée.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
