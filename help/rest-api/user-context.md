---
title: Contexte utilisateur
feature: REST API
description: Découvrez comment activer et utiliser l’API de contexte utilisateur de RTP Marketo pour définir des variables personnalisées, lire les données utilisateur lors des visites et suivre les campagnes consultées et ayant fait l’objet d’un clic.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 5%

---

# Contexte utilisateur

L’API User Context JavaScript expose les données au niveau de l’utilisateur et du visiteur sur plusieurs sessions afin d’activer une fonctionnalité de personnalisation avancée utilisant le comportement et les données historiques de l’utilisateur. L’API va au-delà de la lecture de données et expose des variables personnalisées qui vous permettent de transmettre des données et des événements significatifs au serveur principal RTP à des fins de segmentation avancée et de personnalisation. Fonctionnalités supplémentaires : [Triggers](../javascript-api/triggers.md), [Correspondance des motifs](../javascript-api/pattern-match.md).

- Vous devez devenir client de Web Personalization et la balise [RTP doit être déployée](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- L’API User Context est une fonctionnalité qui doit être activée sur demande par le support Marketo. Lorsque l’API est activée, un objet userContext sous l’objet global RTP est exposé.

## Attributs de contexte utilisateur

| Nom | Type | Description |
|------------------|-------------|------|
| customVar[1-5] | Chaîne | Données personnalisées enregistrées dans le contexte utilisateur. |
| viewsCampagnes | Identifiants de campagne sous forme de chaîne séparée par des virgules | Campagnes affichées au cours des visites actuelles ou précédentes. |
| clickedCampaign | Identifiants de campagne sous forme de chaîne séparée par des virgules | A cliqué dans des campagnes lors de visites actuelles ou précédentes. |

## Définition de variables personnalisées

Ajout de données personnalisées au contexte utilisateur.

### Utilisation

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Paramètre | Facultatif/obligatoire | Type | Description |
|-----------------|-------------------|--------|-----------------|
| &#39;set&#39; | Obligatoire | Chaîne | Action de méthode. |
| customVar | Obligatoire | Chaîne | Nom de variable personnalisé. |
| my_custom_value | Obligatoire | Chaîne | Valeur personnalisée à enregistrer sur la variable personnalisée dans l’index 1-5. |

Remarque : les variables personnalisées sont envoyées au RTP uniquement lors de l’appel de la vue. Il est donc recommandé de définir des variables personnalisées avant d’appeler la vue. Sinon, elle sera envoyée uniquement lors du prochain appel d’affichage.

Restrictions des variables personnalisées

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
