---
title: Rediriger
description: Rediriger
feature: Javascript
exl-id: bbf91245-42e5-47ae-a561-e522cc65ff49
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 8%

---

# Rediriger

L’API de redirection HTTP permet de rediriger les audiences segmentées vers une URL cible.

- Vous devez devenir client Web Personalization et faire déployer la balise [RTP](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- Le protocole RTP ne prend pas en charge les listes de comptes nommés Marketing basé sur un compte. Les listes ABM et le code ne se rapportent qu’aux listes de comptes téléchargées (fichiers CSV) gérées dans RTP.

## Utilisation

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---------------------------|-------------------|---------|-----------------------------|
| &#39;send&#39; | Requis | Chaîne | Action de méthode. |
| &#39;redirect&#39; | Requis | Chaîne | Nom de la méthode. |
| field_name | Requis | Chaîne | Nom du champ à associer. Exemple : &#39;abm.name&#39; (voir ci-dessous). |
| values_array | Requis | Tableau | Liste des valeurs par rapport au champ (non sensible à la casse). |
| redirect_url | Requis | Chaîne | URL cible pour rediriger les visiteurs qui ont correspondu à la condition. |
| redirect_match_visitors | En option | Booléenne | Si la valeur est true, les visiteurs correspondant à la condition sont redirigés. Si la valeur est false, les conditions des visiteurs sans correspondance sont redirigées. Valeur par défaut : true. |

Organisation, secteur industriel, listes ABM, emplacement, FAI, segments mappés

| Condition | Hiérarchie des données | Exemple |
|-------------------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------|
| Segments correspondants (fonctionne uniquement après le premier clic) | matchedSegments.name | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;matchSegments.name&#39; , [&#39;Fortune 1000&#39; , &#39;Enterprise&#39;] , &#39;http://www.marketo.com&#39;); |
| Segments correspondants (fonctionne uniquement après le premier clic) | matchedSegments.id | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;matchSegments.id&#39; , [106, 107, 190] , &#39;http://www.marketo.com&#39;); |
| Listes ABM | abm.name | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;abm.name&#39;, [&#39;top_key_accounts&#39;, &#39;active_clients&#39;], &#39;http://www.marketo.com&#39;); |
| Listes ABM | abm.code | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;abm.code&#39; , [13, 15] , &#39;http://www.marketo.com&#39;); |
| Entreprises | org | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;org&#39;, [&#39;ebay&#39;], &#39;http://www.marketo.com&#39;); |
| Localisation | location.country | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;location.country&#39;, [&#39;United States&#39;], &#39;http://www.marketo.com&#39;); |
| Localisation | location.state | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;location.state&#39;, [&#39;ca&#39;], &#39;http://www.marketo.com&#39;); |
| Localisation | location.city | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;location.city&#39;, [&#39;San Mateo&#39;], &#39;http://www.marketo.com&#39;); |
| Secteurs | industries | rtp(&#39;send&#39;, &#39;redirect&#39;, &#39;industries&#39;, [&#39;Education&#39;], &#39;http://www.marketo.com&#39;); |
| FAI | isp | rtp(&#39;send&#39;, &#39;redirect&#39;, isp , [&#39;False&#39;], &#39;http://www.marketo.com&#39;); |


## Notes

- Si la règle/condition de redirection est basée sur le secteur (entreprise, secteur, emplacement), vous pouvez insérer le code de redirection avant le rtp(&#39;send&#39;, &#39;view&#39;) et le rtp(&#39;get&#39;,&#39;campaign&#39;) afin de réduire la latence.
- La redirection via JavaScript est une redirection côté navigateur qui dépend du chargement et de l’optimisation du site web pour atteindre une vitesse maximale.
- Il est recommandé de définir le code de redirection juste après la balise rtp et de le placer dans l’en-tête .
- Assurez-vous de ne pas exécuter de redirection automatique (il existe un filet de sécurité dans rtp pour bloquer les appels de redirection cyclique).

```html
<!DOCTYPE html>
<html lang="en-US">
<head>
<!-- RTP tag --> 
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//xyz.marketo.com/rtp-api/v1/rtp.js","xyz");
 
// START REDIRECT EXAMPLE 
//   - Using a helper redirect function
//   - Redirect based on named account
rtp('send','redirect','org', ['microsoft'],'http://www.marketo.com');
 
// Redirect based on named account list (ABM)
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm'] 
});
// END REDIRECT EXAMPLE
rtp('send','view');
rtp('get','campaign');
</script>
<!-- End of RTP tag -->
```

## Redirection des visiteurs suivis

1. Ajoutez un paramètre à la fin de l’URL cible : www.marketo.com?rtp=redirect
1. Créer un segment appelé - &quot;Redirigé par RTP&quot;
1. Utilisez le paramètre &quot;Pages spécifiques&quot; pour cibler les visiteurs qui consultent une page avec le paramètre illustré ci-dessous.

![tracking-redirect-vistors](assets/tracking-redirected-vistors.png)

## Comment définir plusieurs conditions avec différentes URL cibles

L’appel de redirection prend en charge plusieurs appels. Cela permet de rediriger avec plusieurs champs et de créer des conditions complexes avec différentes URL et valeurs.

### Utilisation

`rtp('send', 'redirect', field_name, url_values_map);`

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---|---|---|---|
| &#39;send&#39; | Requis | Chaîne | Action de méthode. |
| &#39;redirect&#39; | Requis | Chaîne | Nom de la méthode. |
| field_name | Requis | Chaîne | Nom du champ à associer. Exemple : &#39;abm.name&#39; (voir ci-dessus). |
| url_values_map | Requis | Objet | Mappage entre l’URL de redirection et la liste de valeurs. Exemple :{&#39;http://marketo.com&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |


#### Exemple

```javascript
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
rtp('send','redirect','org', {
    // Redirect visitors from 'Microsoft' to www.marketo.com/enterprise
    'http://www.marketo.com/enterprise' : ['microsoft']
});
```
