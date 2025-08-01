---
title: Personnalisation Web
description: Personnalisation Web
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 7%

---

# Personnalisation Web

L’API Web Personalization JavaScript étend la fonctionnalité de personnalisation automatisée de la plateforme. Il permet le suivi des événements et la personnalisation dynamique d’une page web. Fonctionnalités supplémentaires : [Événements de données personnalisés](custom-data-events.md), [Contenu dynamique](web-personalization.md), [Obtention des données du visiteur](get-visitor-data.md), [Exclure la balise pour des robots spécifiques](#exclude_tag_for_specific_bots).

- Vous devez devenir client de Web Personalization et la balise [RTP doit être déployée](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- RTP ne prend pas en charge les listes de comptes nommés Marketing basées sur les comptes. Les listes et le code ABM ne concernent que les listes de comptes chargées (fichiers CSV) gérées dans RTP.

## Configuration des balises

La balise RTP doit être insérée dans l’en-tête de la page personnalisée.

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

Cette méthode est appelée automatiquement au niveau de la balise pour définir l’identifiant de compte approprié. Vous pouvez définir l’identifiant de compte lorsque vous souhaitez le répartir entre différents domaines.

| Paramètre | Facultatif/obligatoire | Type | Description |
|--------------|-------------------|--------|--------------|
| &#39;setAccount&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| accountId | Obligatoire | Chaîne | ID de compte. |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Fonctions d’envoi d’événement

Cette méthode envoie un événement d’affichage, qui est utilisé pour le suivi des pages. Dans l’exemple ci-dessous, l’URL de la page active est suivie en tant que page visiteur vue.

En transmettant le paramètre facultatif « page » dans cette méthode, la page active peut être remplacée.

| Paramètre | Facultatif/obligatoire | Type | Description |
|-----------|-------------------|--------|---------------------------------|
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

Pour empêcher des navigateurs spécifiques d’envoyer des données à la plateforme Web Personalization (dans le cas de robots identifiés), ajoutez l’instruction IF suivante au script de balise.

Dans l’exemple de code ci-dessous, « Googlebot|msnbot » est utilisé comme exemple de robot pour exclure des activités de Web Personalization.

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

Description du JavaScript ajouté à un site web lors de l’utilisation de Web Personalization et de contenu prédictif.

### JavaScript principal/dépendant

| Nom | Description | Commande |
|---------------------------|-------------|--------------------------------------------------------|
| rtp.js | - | Contrôlé par Marketo |
| jquery.min.js | v1.8.3 | Peut être désactivé en contactant le service clientèle de Marketo |
| jquery-custom-ui-min.js | v1.9.2 | Peut être désactivé en contactant le service clientèle de Marketo |
| query-ui-1.8.17-dialog.js | v1.9.2* | Peut être désactivé en contactant le service clientèle de Marketo |


*Utilisé uniquement si la boîte de dialogue de l’interface utilisateur jQuery est manquante

### JavaScript On Demand

| Nom | Description | Commande |
|-------------------------|-----------------------------------------------------------------------|-----------------------|
| ga-integration-2.0.1.js | Utilisé si l’intégration Google Analytics/Facebook/SiteCatalyst est activée | Contrôlé par Marketo |
| insightera-bar-2.1.js | Utilisé si la barre de recommandation de contenu prédictif est activée | Contrôlé par Marketo |
| froogaloop2.min.js | Utilisé si le suivi de contenu est activé et que le lecteur Vimeo existe sur la page | - |
| iframe-api-v1.js | Utilisé si le suivi de contenu est activé et que le lecteur YouTube existe sur la page | - |
