---
title: Rediriger
description: Rediriger
feature: Javascript
exl-id: bbf91245-42e5-47ae-a561-e522cc65ff49
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 8%

---

# Rediriger

L’API de redirection RTP vous permet de rediriger des audiences segmentées vers une URL cible.

- Vous devez devenir client de Web Personalization et la balise [RTP doit être déployée](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- RTP ne prend pas en charge les listes de comptes nommés Marketing basées sur les comptes. Les listes et le code ABM ne concernent que les listes de comptes chargées (fichiers CSV) gérées dans RTP.

## Utilisation

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Paramètre | Facultatif/obligatoire | Type | Description |
|---------------------------|-------------------|---------|-----------------------------|
| &#39;envoi&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;rediriger&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| field_name | Obligatoire | Chaîne | Nom du champ à comparer. Exemple : &#39;abm.name&#39; (voir ci-dessous). |
| values_array | Obligatoire | Tableau | Liste des valeurs pour lesquelles comparer le champ (non sensible à la casse). |
| redirect_url | Obligatoire | Chaîne | URL cible pour rediriger les visiteurs correspondant à la condition. |
| redirect_match_visitor | Facultatif | Booléen | Si la condition est vraie, les visiteurs correspondants seront redirigés. Si la valeur est false, les visiteurs sans correspondance sont redirigés. Valeur par défaut : true. |

Organisation, secteur, listes ABM, emplacement, FAI, segments correspondants

| Condition | Hiérarchie des données | Exemple |
|-------------------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------|
| Segments correspondants (fonctionne uniquement après le premier clic) | matchedSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchSegments.name&#39; , [&#39;Fortune 1 000&#39; , &#39;Entreprise&#39;] , &#39;<http://www.marketo.com>&#39;); |
| Segments correspondants (fonctionne uniquement après le premier clic) | matchedSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchSegments.id&#39; , [106 , 107 , 190] , &#39;<http://www.marketo.com>&#39;); |
| Listes ABM | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.name&#39; , [&#39;top_key_accounts&#39;, &#39;active_customers&#39;] , &#39;<http://www.marketo.com>&#39;); |
| Listes ABM | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.code&#39; , [13 , 15] , &#39;<http://www.marketo.com>&#39;); |
| Organisations | org | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;org&#39;, [&#39;ebay&#39;], &#39;<http://www.marketo.com>&#39;); |
| Emplacement | location.country | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.country&#39; , [&#39;United States&#39;], &#39;<http://www.marketo.com>&#39;); |
| Emplacement | location.state | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.state&#39;, [&#39;ca&#39;], &#39;<http://www.marketo.com>&#39;); |
| Emplacement | location.city | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.city&#39;, [&#39;San Mateo&#39;], &#39;<http://www.marketo.com>&#39;); |
| Secteurs | industries | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;industries&#39; , [&#39;Education&#39;], &#39;<http://www.marketo.com>&#39;); |
| Fournisseur de services Internet | fai | rtp( &#39;send&#39;, &#39;redirect&#39; , isp , [&#39;False&#39;], &#39;<http://www.marketo.com>&#39;); |

## Notes

- Si la règle/condition de redirection est basée sur Firmographics (société, secteur, emplacement) vous pouvez insérer le code de redirection avant le rtp(&#39;send&#39;, &#39;view&#39;) et le rtp(&#39;get&#39;,&#39;campaign&#39;) pour réduire la latence.
- La redirection via JavaScript est une redirection côté navigateur et dépend du chargement et de l’optimisation du site web pour atteindre la vitesse maximale.
- La bonne pratique consiste à définir le code de redirection juste après la balise rtp et à le placer dans l’en-tête.
- Assurez-vous de ne pas exécuter une redirection automatique (il existe un filet de sécurité dans rtp pour bloquer les appels de redirection cycliques).

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

## Comment rediriger des visiteurs suivis

1. Ajoutez un paramètre à la fin de l’URL cible : &lt;www.marketo.com?rtp=redirect>.
1. Créer un segment appelé « Redirigé par RTP »
1. Utilisez le paramètre « Pages spécifiques » pour cibler les visiteurs qui consultent n’importe quelle page avec le paramètre illustré ci-dessous.

![tracking-redirect-vistors](assets/tracking-redirected-vistors.png)

## Définition de plusieurs conditions avec différentes URL Target

L’appel de redirection prend en charge plusieurs appels. Cela permet de rediriger avec plusieurs champs et de créer des conditions complexes avec différentes URL et valeurs.

### Utilisation

`rtp('send', 'redirect', field_name, url_values_map);`

| Paramètre | Facultatif/obligatoire | Type | Description |
|---|---|---|---|
| &#39;envoi&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;rediriger&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| field_name | Obligatoire | Chaîne | Nom du champ à comparer. Exemple : &#39;abm.name&#39; (voir ci-dessus). |
| url_values_map | Obligatoire | Objet | Mappage entre l’URL de redirection et la liste des valeurs. Exemple :{&#39;<http://marketo.com>&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |

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
