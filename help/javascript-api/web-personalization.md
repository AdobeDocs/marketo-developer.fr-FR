---
title: Web Personalization
description: Guide de l’API JavaScript Web Personalization et de la balise RTP, couvrant les événements de page vue, la configuration du compte, les exclusions de robots et les scripts principaux et à la demande
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
TQID: https://experienceleague.adobe.com/yplunKmgjOJ7gJTA2TDc9cfJXyXbrVWuM-NdVbDMN4A
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2: id: cdd4e0f6-e87e-453f-88ee-2ee54a7de272
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 435
ht-degree: 6%

---

# Web Personalization

L’API Web Personalization JavaScript suit les événements et personnalise dynamiquement les pages web. Il étend les fonctionnalités de personnalisation automatisée de la plateforme.

Les fonctionnalités associées comprennent [Événements de données personnalisés](custom-data-events.md), [Contenu dynamique](web-personalization.md), [Obtenir les données du visiteur](get-visitor-data.md) et [Exclure la balise pour des robots spécifiques](#exclude_tag_for_specific_bots).

- Vous devez être client de Web Personalization et avoir déployé la balise [RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- RTP ne prend pas en charge les listes de comptes nommés Marketing basées sur les comptes. Les listes et le code ABM ne concernent que les listes de comptes chargées (fichiers CSV) gérées dans RTP.

## Configuration des balises

Insérez la balise RTP dans l’en-tête de chaque page personnalisée.

```javascript
<!-- RTP tag -->
<script type='text/javascript'>
(function(c,h,a,f,e,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].p=e;c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b)})
(window,document,"rtp","[rtp-js-cdn-url]","[pod-url]","[accountId]");
</script>
<!-- End of RTP tag -->
```

## Configuration du compte

La balise appelle automatiquement cette méthode pour définir l’identifiant de compte approprié. Définissez explicitement l’ID de compte lorsque vous souhaitez utiliser différents comptes pour différents domaines.

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;setAccount&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| accountId | Obligatoire | Chaîne | ID de compte. |

```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Fonctions d’envoi d’événement

Cette méthode envoie un événement d’affichage pour le suivi des pages. Le premier appel de l’exemple suivant effectue le suivi de l’URL de la page active en tant qu’affichage de la page visiteur.

Transmettez le paramètre facultatif « page » pour remplacer la page active, comme indiqué dans le deuxième appel.

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;envoi&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;vue&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| page | Facultatif | Chaîne | Chemin relatif ou URL de page complète. |

```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Exclure la balise de robots spécifiques (agents utilisateurs)

Pour empêcher les robots identifiés d’envoyer des données à la plateforme Web Personalization, ajoutez l’instruction `if` suivante au script de balise.

Cet exemple exclut les agents utilisateur « Googlebot|msnbot » des activités de Web Personalization.

```javascript
<!-- RTP tag -->
<script type='text/javascript'>
if(navigator.userAgent.match(/.(Googlebot|msnbot)./gi) == null){
    (function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
    c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
    g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//[cdn-pod-X-url]/rtp-api/v1/rtp.js","[accountId]");

    rtp('send','view');
    rtp('get', 'campaign', true);
}
</script>
<!-- End of RTP tag -->
```

## Présentation des appels JavaScript

Les tableaux ci-dessous décrivent le JavaScript ajouté à un site web qui utilise le Personalization web et le contenu prédictif.

### JavaScript principal/dépendant

| Nom | Description | Commande |
| --- | --- | --- |
| rtp.js | - | Contrôlé par Marketo |
| jquery.min.js | v1.8.3 | Peut être désactivé en contactant le service clientèle de Marketo |
| jquery-custom-ui-min.js | v1.9.2 | Peut être désactivé en contactant le service clientèle de Marketo |
| query-ui-1.8.17-dialog.js | v1.9.2* | Peut être désactivé en contactant le service clientèle de Marketo |

*Utilisé uniquement si la boîte de dialogue de l’interface utilisateur jQuery est manquante.

### JavaScript On Demand

| Nom | Description | Commande |
| --- | --- | --- |
| ga-integration-2.0.1.js | Utilisé si l’intégration Google Analytics/Facebook/SiteCatalyst est activée | Contrôlé par Marketo |
| insightera-bar-2.1.js | Utilisé si la barre de recommandation de contenu prédictif est activée | Contrôlé par Marketo |
| froogaloop2.min.js | Utilisé si le suivi de contenu est activé et que le lecteur Vimeo existe sur la page | - |
| iframe-api-v1.js | Utilisé si le suivi de contenu est activé et que le lecteur YouTube existe sur la page | - |
