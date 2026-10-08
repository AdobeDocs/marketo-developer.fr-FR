---
title: Contexte utilisateur
feature: REST API
description: Découvrez comment activer et utiliser l’API de contexte utilisateur de RTP Marketo pour définir des variables personnalisées, lire les données utilisateur lors des visites et suivre les campagnes consultées et ayant fait l’objet d’un clic.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
TQID: 'https://experienceleague.adobe.com/Ph0Tw-C9jzWaR4bYyUIXyzzoa2yjHQk2gt6tNA8H2mA'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
    internal-label: Personalization
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
subfeature_v2:
  - id: a1d50dda-6d94-4e16-8c30-5eb7181c4650
    internal-label: Segmentation
  - id: cf1396d8-ab85-4e93-b35d-d9b573024abf
    internal-label: REST APIs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 5%
---
# Contexte utilisateur

L’API User Context JavaScript expose les données au niveau de l’utilisateur et du visiteur sur plusieurs sessions. Utilisez le comportement historique et les données pour créer une personnalisation avancée.

L’API fournit également des variables personnalisées pour envoyer des données et des événements au serveur principal RTP à des fins de segmentation et de personnalisation. Consultez les fonctionnalités [Triggers](../javascript-api/triggers.md) et [Correspondance des motifs](../javascript-api/pattern-match.md) associées.

- Vous devez être client de Web Personalization et la balise [RTP doit être déployée](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site.
- Vous devez demander à l’assistance Marketo d’activer l’API User Context. Après l’activation, un objet userContext est exposé sous l’objet global RTP.

## Attributs de contexte utilisateur

| Nom | Type | Description |
| --- | --- | --- |
| `customVar[1-5]` | Chaîne | Données personnalisées enregistrées dans le contexte utilisateur. |
| `viewedCampaigns` | Identifiants de campagne sous forme de chaîne séparée par des virgules | Campagnes affichées au cours des visites actuelles ou précédentes. |
| `clickedCampaigns` | Identifiants de campagne sous forme de chaîne séparée par des virgules | A cliqué dans des campagnes lors de visites actuelles ou précédentes. |

## Définition de variables personnalisées

Définissez des variables personnalisées pour ajouter des données au contexte utilisateur.

### Utilisation

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| `'set'` | Obligatoire | Chaîne | Action de méthode. |
| `customVar` | Obligatoire | Chaîne | Nom de variable personnalisé. |
| `my_custom_value` | Obligatoire | Chaîne | Valeur personnalisée à enregistrer sur la variable personnalisée dans l’index 1-5. |

Les variables personnalisées sont envoyées au RTP uniquement dans un appel de vue. Définissez des variables personnalisées avant l’appel d’affichage. Dans le cas contraire, les variables sont envoyées lors du prochain appel d’affichage.

Les variables personnalisées présentent les restrictions suivantes :

- Une variable personnalisée ne peut pas dépasser 100 caractères.
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
