---
title: "Correspondance de motif"
description: "Correspondance de motif"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 6%

---


# Correspondance de motif

RTP expose une fonction d’utilitaire pour vérifier si le modèle correspond à certaines chaînes. L’utilitaire ne peut pas être utilisé en mode asynchrone, car il renvoie une indication s’il existe une correspondance ou non.

Vous devez devenir un client de personnalisation Web et disposer de la variable [Balise RTP déployée](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.

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
