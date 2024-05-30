---
title: "User Context"
feature: REST API
description: "Présentation du contexte utilisateur et descriptions des API"
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 5%

---


# Contexte de l’utilisateur

L’API JavaScript de contexte utilisateur expose les données au niveau des utilisateurs et des visiteurs sur plusieurs sessions afin d’activer la fonctionnalité de personnalisation avancée à l’aide de données et de comportements utilisateur historiques. L’API va au-delà de la lecture de données et expose des variables personnalisées qui vous permettent de transmettre des données et des événements significatifs au serveur principal RTP à des fins de segmentation et de personnalisation avancées. Fonctionnalités supplémentaires : [Triggers](../javascript-api/triggers.md), [Correspondance de motif](../javascript-api/pattern-match.md).

- Vous devez devenir un client de personnalisation Web et disposer de la variable [Balise RTP déployée](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- L’API de contexte utilisateur est une fonctionnalité qui doit être activée par le support Marketo sur demande. Lorsque l’API est activée, un objet userContext sous l’objet global RTP est exposé.

## Attributs de contexte utilisateur

| Nom | Type | Description |
|------------------|-------------|------|
| customVar[1-5] | Chaîne | Données personnalisées enregistrées dans le contexte utilisateur. |
| viewCampaigns | ID de campagne sous la forme d’une chaîne séparée par des virgules | Campagnes consultées au cours de visites précédentes ou en cours. |
| clickedCampaigns | ID de campagne sous la forme d’une chaîne séparée par des virgules | Clics sur les campagnes au cours de visites précédentes ou en cours. |

## Définition de variables personnalisées

Ajout de données personnalisées à User Context.

### Utilisation

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Paramètre | Facultatif/Obligatoire | Type | Description |
|-----------------|-------------------|--------|-----------------|
| &#39;set&#39; | Requis | Chaîne | Action de méthode. |
| customVar | Requis | Chaîne | Nom de variable personnalisée. |
| my_custom_value | Requis | Chaîne | Valeur personnalisée à enregistrer sur la variable personnalisée dans l’index 1-5. |

Remarque : Les variables personnalisées sont envoyées à la méthode RTP uniquement lors de l’appel de vue. Il est donc recommandé de définir des variables personnalisées avant l’appel de la vue. Sinon, elle sera envoyée uniquement lors de l’appel de vue suivant.

Restrictions de variable personnalisée

- La longueur de la variable personnalisée ne peut pas dépasser 100 caractères.
- Les données de campagne sont limitées aux dix dernières visites avec dix campagnes par visite.

### Utilisation

`rtp('set', 'customVar', 'A');`

```javascript
// Set and get customVars
rtp('set', 'customVar1', 'foo');
 
// Read location 
if (rtp.userContext.location.state == 'CA')  {
    // Do something
}
 
// Check if user viewed campaign id 45:
// The campaign id is exposed in the RTP UI when hovering over a campaign name.
if (rtp.userContext.viewedCampaign('45')) {
    // Do something
}
```
