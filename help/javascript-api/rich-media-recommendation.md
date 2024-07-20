---
title: Recommandation de média intéractif
description: Recommandation de média intéractif
feature: Javascript
exl-id: ee92e46d-e529-40a2-a0d0-ee233916f004
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 5%

---

# Recommandation de média intéractif


Les balises et appels d’API suivants doivent être configurés sur la page que vous souhaitez afficher le modèle de recommandation pour les médias riches.

1. Dans l’en-tête de page
   1. Installation de la balise RTP
   1. Ajouter l’appel de GET à la page pour renseigner les recommandations
   1. Ajouter l’appel SET pour configurer le modèle
1. Dans le corps de page
   1. Placez la balise de modèle (classe div) à l’emplacement où vous souhaitez que le modèle s’affiche.

Plus d&#39;informations sont disponibles [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/enabling-predictive-content/enable-predictive-content-for-web-rich-media).

## Balise de modèle

| Attribut | Facultatif/Obligatoire | Description |
|---|---|---|
| une classe | Requis | Indiquez que cet élément d’HTML div est div de recommandation RTP. |
| data-rtp-template-id | Requis | Identifiant du modèle. Cela détermine l’alignement de votre recommandation. Utilisez &quot;template1&quot; pour l’alignement horizontal, &quot;template2&quot; pour l’alignement vertical ou &quot;template3&quot; pour l’alignement vertical qui ne comprend que le titre et la description. Le script injecte le modèle correspondant dans ces valeurs `div.Permissible` : template1, template2, template3. |

### Exemples

Pour afficher vos recommandations en alignement horizontal, utilisez &quot;template1&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
```

Pour afficher vos recommandations en alignement vertical, utilisez &quot;template2&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template2"></div>
```

Pour afficher vos recommandations en alignement vertical avec le titre et la description uniquement, utilisez &quot;template3&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template3"></div>
```

Voir les captures d&#39;écran des alignements de modèles [ici](#example_of_rich_media_recommendation_template_1).

## Renseigner la recommandation

Cette méthode renseigne tous les `<divs>` médias riches sur la page avec des recommandations.

### Utilisation

`rtp('get', 'rcmd', 'richmedia');`

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---|---|---|---|
| &#39;get&#39; | Requis | Chaîne | Action de méthode. |
| &#39;rcmd&#39; | Requis | Chaîne | Nom de la méthode. |
| &#39;richmedia&#39; | Requis | Chaîne | Nom de la sous-méthode. |


## Modifier la configuration des modèles

Cette méthode modifie la configuration par défaut du modèle.

Remarque : Lorsque vous utilisez cette méthode, elle doit être appelée avant d’appeler rtp(&#39;get&#39;,&#39;rcmd&#39;, &#39;richmedia&#39;);

### Utilisation

`rtp('set', 'rcmd', 'richmedia', 'template_id', conf_obj);`

| Paramètre | Facultatif/Obligatoire | Type | Description |
|---|---|---|---|
| &#39;set&#39; | Requis | Chaîne | Action de méthode. |
| &#39;rcmd&#39; | Requis | Chaîne | Nom de la méthode. |
| &#39;richmedia&#39; | Requis | Chaîne | Nom de la sous-méthode. |
| template_id | En option | Chaîne | ID de modèle pour les modifications de configuration. Utilisez pour spécifier la modification des paramètres d’un seul modèle. |
| conf_obj | Requis | Objet | Nouvelle configuration. L’objet contient toutes les configurations en tant que paire clé/valeur. |


### Exemples

Ce fragment de code modifie le texte du titre d’un modèle.

```javascript
rtp("set", "rcmd", "richmedia","template1",
    {
        "rcmd.title.text": "RECOMMENDED CONTENT"
    }
);
```

Ce fragment de code affiche la définition de catégories avec plusieurs configurations pour un modèle.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1": 
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial",
            "category":
            [
                "webinar",
                "blog posts",
                "pricing_page_category",
                "product_a_category"
            ]
        }
    }
);
```

REMARQUE : utilisez &quot;catégorie&quot; pour filtrer le contenu affiché dans le résultat des recommandations de contenu prédictif. Pour appliquer du contenu prédictif à tous les éléments de contenu activés, laissez la &quot;catégorie&quot; vide. Si vous souhaitez recommander uniquement du contenu spécifique pour la sortie dans le modèle Média enrichi, ajoutez une catégorie pour le contenu dans la page Définir le contenu et associez cette catégorie au code du modèle de recommandation. Catégorisation du contenu pertinent en fonction des sections de votre site web (produits ou solutions).

Ce fragment de code affiche la définition de plusieurs configurations de modèle pour un modèle.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1":
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial"
        }
    }
);
```

#### Propriétés de configuration

| Configuration | Exemple | Description |
|---|---|---|
| rcmd.general.font.family | &quot;rcmd.general.font.family&quot; : &quot;arial&quot; | Modifie la famille de polices pour tout le texte du modèle. Cette propriété prend en charge toutes les valeurs CSS par type de navigateur. Il est possible d’utiliser une famille de polices personnalisée si elle existe sur la page. |
| rcmd.content.background.color | &quot;rcmd.content.background.color&quot; : &quot;noir&quot; | Modifie la couleur d’arrière-plan des zones intérieures du modèle. Cette propriété prend en charge toutes les valeurs CSS par type de navigateur. |
| rcmd.title.text | &quot;rcmd.title.text&quot; : &quot;CONTENU RECOMMANDÉ&quot; | Modifie le titre du modèle. |
| rcmd.title.background.color | &quot;rcmd.title.background.color&quot; : &quot;blue&quot; | Modifie la couleur d’arrière-plan de la zone de titre. Cette propriété prend en charge toutes les valeurs de couleur CSS (nom de couleur, rgb, etc.). |
| rcmd.title.font.size | &quot;rcmd.title.font.size&quot; : &quot;26px&quot; | Modifie la taille de police du titre. La propriété prend en charge toutes les valeurs CSS de taille de police possibles (px, em, etc.). |
| rcmd.title.font.color | &quot;rcmd.title.font.color&quot; : &quot;blanc&quot; | Modifie la couleur de police du titre. Cette propriété prend en charge toutes les valeurs de couleur de police (rgb, hex, etc.). |
| rcmd.description.font.color | &quot;rcmd.description.font.color&quot; : &quot;blanc&quot; | Modifie la couleur de la police de description. Cette propriété prend en charge toutes les valeurs de couleur de police (rgb, hex, etc.). |
| rcmd.cta.background.color | &quot;rcmd.cta.background.color&quot; : &quot;vert&quot; | Modifie la couleur d’arrière-plan du bouton. Cette propriété prend en charge toutes les valeurs de couleur CSS (nom de couleur, rgb, etc.). |
| rcmd.cta.font.color | &quot;rcmd.cta.font.color&quot; : &quot;rgb(90, 84, 164)&quot; | Modifie la couleur de la police du bouton. Cette propriété prend en charge toutes les valeurs de couleur de police (rgb, hex, etc.). |
| rcmd.cta.text | &quot;rcmd.cta.text&quot; : &quot;Push&quot; | Modifie le texte du bouton. Le texte est le même pour tous les boutons. |
| Catégorie | &quot;category&quot; : [&quot;one category&quot;] | Modifie la catégorie de recommandations prise en charge par ce modèle. Le modèle affiche uniquement les recommandations avec l’une des catégories définies par cette configuration. |


Remarque : La prise en charge de la configuration peut changer par modèle.

#### Exemple de base

Cet exemple comporte un modèle avec trois recommandations. Copiez cet exemple dans une page d’HTML, puis remplacez la balise RTP par votre balise .

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag --> 
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Exemple avancé

Cet exemple comporte un modèle avec trois recommandations. Le titre du modèle est &quot;CONTENU RECOMMANDÉ&quot; et le texte du bouton est &quot;En savoir plus&quot;. Copiez cet exemple dans une page d’HTML, puis remplacez la balise RTP par votre balise .

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag --> 
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate the recommendation zone
rtp('get', 'campaign',true);
// Change template configuration
rtp('set', 'rcmd', 'richmedia',
    {
        template1 :
        {
            "rcmd.title.text" : "RECOMMENDED CONTENT",
            "rcmd.cta.text" : "Read More"
        }
    }
);
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Exemple de #1 de modèle de recommandation pour les médias riches

**Nom** : template1 **Description** : contenu horizontal comprenant l’image, le titre et la description et bouton d’appel à l’action.

![ Modèle de contenu multimédia enrichi](assets/rich-media-template1.png)

#### Exemple de #2 de modèle de recommandation pour les médias riches

**Nom** : template2 **Description** : contenu vertical comprenant image, titre et description et bouton d’appel à l’action.

![ Modèle de contenu multimédia enrichi](assets/rich-media-template2.png)

#### Exemple de #3 de modèle de recommandation pour les médias riches

**Nom** : template3 **Description** : contenu vertical comprenant uniquement le titre et la description. Lorsque vous pointez avec la souris, l’en-tête change de couleur et est lié à l’URL du contenu. La description est également liée au contenu sans changement de couleur. ![ Modèle de contenu multimédia enrichi](assets/rich-media-template3.png)
