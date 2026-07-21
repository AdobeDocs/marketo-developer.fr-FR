---
title: Rediriger
description: Implémentez l’API de redirection RTP pour envoyer des visiteurs segmentés vers des URL ciblées à l’aide de champs tels que l’ABM, l’organisation, l’emplacement et les segments, avec des exemples et des conseils.
feature: Javascript
exl-id: bbf91245-42e5-47ae-a561-e522cc65ff49
TQID: https://experienceleague.adobe.com/frvGjN7DBJ1RJ3QFvWxo1qGiTNFmvyxi3H6FeynJHLU
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 473
ht-degree: 8%

---

# Rediriger

Utilisez l’API de redirection RTP pour envoyer des audiences segmentées à une URL cible.

- Vous devez être client de Web Personalization et avoir déployé la balise [RTP](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sur votre site avant d’utiliser l’API de contexte utilisateur.
- RTP ne prend pas en charge les listes de comptes nommés Marketing basées sur les comptes. Les listes et le code ABM ne concernent que les listes de comptes chargées (fichiers CSV) gérées dans RTP.

## Utilisation

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;envoi&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;rediriger&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| field_name | Obligatoire | Chaîne | Nom du champ à comparer. Exemple : &#39;abm.name&#39; (voir ci-dessous). |
| values_array | Obligatoire | Tableau | Liste des valeurs pour lesquelles comparer le champ (non sensible à la casse). |
| redirect_url | Obligatoire | Chaîne | URL cible pour rediriger les visiteurs correspondant à la condition. |
| redirect_match_visitor | Facultatif | Booléen | Si la condition est vraie, les visiteurs correspondants seront redirigés. Si la valeur est false, les visiteurs sans correspondance sont redirigés. Valeur par défaut : true. |

Les conditions de redirection peuvent utiliser l’organisation, le secteur, les listes ABM, l’emplacement, le FAI ou les segments correspondants.

| Condition | Hiérarchie des données | Exemple |
| --- | --- | --- |
| Segments correspondants (fonctionne uniquement après le premier clic) | matchSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchSegments.name&#39; , [&#39;Fortune 1 000&#39; , &#39;Entreprise&#39;] , &#39;<https://www.example.com>&#39;); |
| Segments correspondants (fonctionne uniquement après le premier clic) | matchSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchSegments.id&#39; , [106 , 107 , 190] , &#39;<https://www.example.com>&#39;); |
| Listes ABM | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.name&#39; , [&#39;top_key_accounts&#39;, &#39;active_customers&#39;] , &#39;<https://www.example.com>&#39;); |
| Listes ABM | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.code&#39; , [13 , 15] , &#39;<https://www.example.com>&#39;); |
| Organisations | org | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;org&#39;, [&#39;ebay&#39;], &#39;<https://www.example.com>&#39;); |
| Emplacement | location.country | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.country&#39; , [&#39;United States&#39;], &#39;<https://www.example.com>&#39;); |
| Emplacement | location.state | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.state&#39;, [&#39;ca&#39;], &#39;<https://www.example.com>&#39;); |
| Emplacement | location.city | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.city&#39;, [&#39;San Mateo&#39;], &#39;<https://www.example.com>&#39;); |
| Secteurs | industries | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;industries&#39; , [&#39;Education&#39;], &#39;<https://www.example.com>&#39;); |
| Fournisseur de services Internet | fai | rtp( &#39;send&#39;, &#39;redirect&#39; , isp , [&#39;False&#39;], &#39;<https://www.example.com>&#39;); |

## Notes

- Pour réduire la latence d’une redirection basée sur des micrographiques, tels que l’entreprise, le secteur ou l’emplacement, insérez le code de redirection avant rtp(’envoi’, ’affichage’) et rtp(’obtention’,’campagne’).
- Placez le code de redirection immédiatement après la balise rtp dans l’en-tête de la page.
- Optimisez le chargement du site web pour améliorer la vitesse de la redirection JavaScript côté navigateur.
- Évitez les redirections automatiques. rtp comprend une sauvegarde qui bloque les appels de redirection cycliques.

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

1. Ajoutez le paramètre à l’URL cible, par exemple &lt;www.marketo.com?rtp=redirect>.
1. Créez un segment nommé « Redirigé par RTP ».
1. Utilisez le paramètre « Pages spécifiques » pour cibler les visiteurs et visiteuses qui consultent une page contenant ce paramètre.

![tracking-redirect-vistors](assets/tracking-redirected-vistors.png)

## Définition de plusieurs conditions avec différentes URL Target

L’appel de redirection prend en charge plusieurs appels. Utilisez plusieurs appels pour combiner des champs et créer des conditions avec différentes URL et valeurs.

### Utilisation

`rtp('send', 'redirect', field_name, url_values_map);`

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;envoi&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;rediriger&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| field_name | Obligatoire | Chaîne | Nom du champ à comparer. Exemple : &#39;abm.name&#39; (voir ci-dessus). |
| url_values_map | Obligatoire | Objet | Mappage entre l’URL de redirection et la liste des valeurs. Exemple :{&#39;<https://www.example.com>&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |

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
