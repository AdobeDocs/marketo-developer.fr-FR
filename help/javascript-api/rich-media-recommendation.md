---
title: 'Recommandation de média intéractif '
description: 'Recommandation de média intéractif '
feature: Javascript
exl-id: ee92e46d-e529-40a2-a0d0-ee233916f004
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 5%

---

# Recommandation de média intéractif


Les balises et appels d’API suivants doivent être configurés sur la page sur laquelle vous souhaitez afficher le modèle de recommandation de média enrichi.

1. Dans l’en-tête de page
   1. Faire installer la balise RTP
   1. Ajoutez l’appel GET à la page pour renseigner les recommandations
   1. Ajoutez l’appel SET pour configurer le modèle
1. Dans le corps de la page
   1. Placez la balise du modèle (classe div) à l’emplacement où vous souhaitez que le modèle apparaisse

Vous trouverez plus d’informations [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/predictive-content/enabling-predictive-content/enable-predictive-content-for-web-rich-media).

## Balise de modèle

| Attribut | Facultatif/obligatoire | Description |
|---|---|---|
| une classe | Obligatoire | Spécifiez que cet élément div HTML est un div de recommandation RTP. |
| data-rtp-template-id | Obligatoire | Identifiant du modèle. Cela détermine l’alignement de votre recommandation. Utilisez « template1 » pour l’alignement horizontal, « template2 » pour l’alignement vertical ou « template3 » pour l’alignement vertical qui inclut uniquement le titre et la description. Le script injecte le modèle correspondant dans ces valeurs de `div.Permissible` : template1, template2, template3. |

### Exemples

Pour afficher vos recommandations en alignement horizontal, utilisez « template1 ».

```html
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
```

Pour afficher vos recommandations alignées verticalement, utilisez « template2 ».

```html
<div class="RTP_RCMD2" data-rtp-template-id="template2"></div>
```

Pour afficher vos recommandations alignées verticalement avec le titre et la description uniquement, utilisez « template3 ».

```html
<div class="RTP_RCMD2" data-rtp-template-id="template3"></div>
```

Voir les captures d’écran des alignements de modèles [ici](#example_of_rich_media_recommendation_template_1).

## Renseigner la recommandation

Cette méthode renseigne tous les `<divs>` de médias riches de la page avec des recommandations.

### Utilisation

`rtp('get', 'rcmd', 'richmedia');`

| Paramètre | Facultatif/obligatoire | Type | Description |
|---|---|---|---|
| &#39;get&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;rcmd&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| &#39;richmedia&#39; | Obligatoire | Chaîne | Nom de la sous-méthode. |

## Modifier la configuration du modèle

Cette méthode modifie la configuration par défaut du modèle.

Remarque : lors de l&#39;utilisation de cette méthode, elle doit être appelée avant d&#39;appeler rtp(&#39;get&#39;,&#39;rcmd&#39;, &#39;richmedia&#39;);

### Utilisation

`rtp('set', 'rcmd', 'richmedia', 'template_id', conf_obj);`

| Paramètre | Facultatif/obligatoire | Type | Description |
|---|---|---|---|
| &#39;set&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;rcmd&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| &#39;richmedia&#39; | Obligatoire | Chaîne | Nom de la sous-méthode. |
| template_id | Facultatif | Chaîne | Identifiant du modèle pour les modifications de configuration. Permet de spécifier la modification des paramètres pour un seul modèle. |
| conf_obj | Obligatoire | Objet | Nouvelle configuration. L’objet contient toutes les configurations en tant que paire clé/valeur. |

### Exemples

Ce fragment de code modifie le texte de titre d’un modèle.

```javascript
rtp("set", "rcmd", "richmedia","template1",
    {
        "rcmd.title.text": "RECOMMENDED CONTENT"
    }
);
```

Ce fragment de code présente la définition de catégories avec plusieurs configurations pour un modèle.

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

REMARQUE : utilisez « category » pour filtrer le contenu affiché dans le résultat des recommandations de contenu prédictif. Pour appliquer le contenu prédictif à tous les éléments de contenu activés, laissez le champ « catégorie » vide. Si vous souhaitez recommander uniquement du contenu spécifique pour la sortie dans le modèle de média enrichi, ajoutez une catégorie pour le contenu dans la page Définir le contenu et associez cette catégorie dans le code du modèle de recommandation. Catégorisation du contenu pertinent en fonction des sections de votre site web (produits ou solutions).

Ce fragment de code présente la définition de plusieurs configurations de modèle pour un modèle.

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
| rcmd.general.font.family | « rcmd.general.font.family » : « arial » | Modifie la famille de polices de tout le texte du modèle. Cette propriété prend en charge toutes les valeurs CSS par le type de navigateur. Il est possible d’utiliser une famille de polices personnalisée si elle existe sur la page. |
| rcmd.content.background.color | « rcmd.content.background.color » : « black » | Modifie la couleur d’arrière-plan des zones internes du modèle. Cette propriété prend en charge toutes les valeurs CSS par le type de navigateur. |
| rcmd.title.text | « rcmd.title.text » : « CONTENU RECOMMANDÉ » | Modifie le titre du modèle. |
| rcmd.title.background.color | « rcmd.title.background.color » : « bleu » | Modifie la couleur d’arrière-plan de la zone de titre. Cette propriété prend en charge toutes les valeurs de couleur css (nom de la couleur, rvb, etc.) |
| rcmd.title.font.size | « rcmd.title.font.size » : « 26px » | Modifie la taille de police du titre. La propriété prend en charge toutes les tailles de police possibles pour les valeurs CSS (px, em, etc.) |
| rcmd.title.font.color | « rcmd.title.font.color » : « blanc » | Modifie la couleur de la police du titre. Cette propriété prend en charge toutes les valeurs de couleur de la police (rvb, hex, etc.) |
| rcmd.description.font.color | « rcmd.description.font.color » : « blanc » | Modifie la couleur de la police de description. Cette propriété prend en charge toutes les valeurs de couleur de la police (rvb, hex, etc.) |
| rcmd.cta.background.color | « rcmd.cta.background.color » : « vert » | Modifie la couleur d’arrière-plan du bouton. Cette propriété prend en charge toutes les valeurs de couleur css (nom de la couleur, rvb, etc.) |
| rcmd.cta.font.color | « rcmd.cta.font.color » : « rgb(90, 84, 164) » | Modifie la couleur de police du bouton. Cette propriété prend en charge toutes les valeurs de couleur de la police (rvb, hex, etc.) |
| rcmd.cta.text | « rcmd.cta.text » : « Push » | Modifie le texte du bouton. Le texte est le même pour tous les boutons. |
| Catégorie | « category » : [ « one category »] | Modifie la catégorie de recommandation prise en charge par ce modèle. Le modèle affiche uniquement des recommandations avec l’une des catégories définies par cette configuration. |

Remarque : la prise en charge de la configuration peut changer pour chaque modèle.

#### Exemple de base

Cet exemple comporte un modèle avec trois recommandations. Copiez cet exemple dans une page HTML, puis remplacez la balise RTP par votre balise .

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

Cet exemple comporte un modèle avec trois recommandations. Le titre du modèle est « CONTENU RECOMMANDÉ » et le texte du bouton est « En savoir plus ». Copiez cet exemple dans une page HTML, puis remplacez la balise RTP par votre balise .

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

#### Exemple de modèle de recommandation de média enrichi #1

**Nom** : template1 **Description** : contenu horizontal comprenant une image, un titre et une description, ainsi qu’un bouton call to action.

![Modèle Rich Media](assets/rich-media-template1.png)

#### Exemple de modèle de recommandation de média enrichi #2

**Nom** : template2 **Description** : contenu vertical comprenant une image, un titre et une description, ainsi qu’un bouton call to action.

![Modèle Rich Media](assets/rich-media-template2.png)

#### Exemple de modèle de recommandation de média enrichi #3

**Nom** : template3 **Description** : contenu vertical incluant uniquement le titre et la description. Lorsque vous pointez dessus, l’en-tête change de couleur et contient un lien hypertexte vers l’URL du contenu. La description fournit également des liens vers le contenu sans changement de couleur. ![Modèle Rich Media](assets/rich-media-template3.png)
