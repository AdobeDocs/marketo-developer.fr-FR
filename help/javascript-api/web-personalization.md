---
title: Personnalisation Web
description: Personnalisation Web
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 6%

---

# Personnalisation Web

L’API JavaScript de Web Personalization étend la fonctionnalité de personnalisation automatisée de la plateforme. Il permet le suivi des événements et la personnalisation dynamique d’une page web. Fonctionnalités supplémentaires : [Événements de données personnalisés](custom-data-events.md), [Contenu dynamique](web-personalization.md), [Obtenir les données du visiteur](get-visitor-data.md), [Exclure la balise pour les robots spécifiques](#exclude_tag_for_specific_bots).

- Vous devez devenir client Web Personalization et faire déployer la balise [RTP](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- Le protocole RTP ne prend pas en charge les listes de comptes nommés Marketing basé sur un compte. Les listes ABM et le code ne se rapportent qu’aux listes de comptes téléchargées (fichiers CSV) gérées dans RTP.

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

Cette méthode est appelée automatiquement au niveau de la balise pour définir l’ID de compte approprié. Vous pouvez définir l’ID de compte lorsque vous souhaitez le fractionner entre différents domaines.

| Paramètre | Facultatif/Obligatoire | Type | Description |
|--------------|-------------------|--------|--------------|
| &#39;setAccount&#39; | Requis | Chaîne | Nom de la méthode. |
| accountId | Requis | Chaîne | Identifiant de compte. |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Fonctions d’envoi d’événement

Cette méthode envoie un événement view utilisé pour le suivi des pages. Dans l’exemple ci-dessous, l’URL de la page active est suivie en tant que page vue du visiteur.

En transmettant le paramètre facultatif &quot;page&quot; dans cette méthode, la page active peut être remplacée.

| Paramètre | Facultatif/Obligatoire | Type | Description |
|-----------|-------------------|--------|---------------------------------|
| &#39;send&#39; | Requis | Chaîne | Action de méthode. |
| &#39;view&#39; | Requis | Chaîne | Nom de la méthode. |
| page | En option | Chaîne | Chemin relatif ou URL de la page entière. |


```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Exclure la balise pour des robots spécifiques (agents utilisateur)

Pour exclure des navigateurs spécifiques de l’envoi de données à la plateforme Web Personalization (dans le cas de robots identifiés), ajoutez l’instruction IF suivante au script de balise.

Dans l’exemple de code ci-dessous, &quot;Googlebot|msnbot&quot; est utilisé comme exemples de robots à exclure des activités Web Personalization.

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

## Explication des appels JavaScript

Description de JavaScript qui est ajouté à un site web lors de l’utilisation de Web Personalization et de contenu prédictif.

### JavaScript principal/dépendant

| Nom | Description | Contrôle |
|---------------------------|-------------|--------------------------------------------------------|
| rtp.js | - | Contrôlé par Marketo |
| jquery.min.js | v1.8.3 | Peut être désactivé en contactant le service clientèle de Marketo |
| jquery-custom-ui-min.js | v1.9.2 | Peut être désactivé en contactant le service clientèle de Marketo |
| query-ui-1.8.17-dialog.js | v1.9.2* | Peut être désactivé en contactant le service clientèle de Marketo |


*Utilisé uniquement si la boîte de dialogue de l’interface utilisateur jQuery est manquante

### JavaScript à la demande

| Nom | Description | Contrôle |
|-------------------------|-----------------------------------------------------------------------|-----------------------|
| ga-integration-2.0.1.js | Utilisé si l’intégration de Google Analytics/Facebook/SiteCatalyst est activée | Contrôlé par Marketo |
| insightera-bar-2.1.js | Utilisé si la barre de recommandation de contenu prédictif est activée | Contrôlé par Marketo |
| froogaloop2.min.js | Utilisé si le suivi du contenu est activé et que le lecteur Vimeo existe sur la page | - |
| iframe-api-v1.js | Utilisé si le suivi du contenu est activé et que le lecteur YouTube existe sur la page | - |
