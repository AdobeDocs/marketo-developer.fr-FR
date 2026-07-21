---
title: Recommandation de média intéractif
description: Configurez la recommandation de média enrichi à l’aide de la balise RTP de contenu prédictif Marketo, template1 template2 template3 divs, GET pour remplir, SET pour configurer les catégories.
feature: Javascript
exl-id: ee92e46d-e529-40a2-a0d0-ee233916f004
TQID: https://experienceleague.adobe.com/ygm5h1FJZZW4mC318-fRR3VAcO6j1sitcAeqIUjDTbI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 814
ht-degree: 5%

---

# Recommandation de média intéractif

Pour afficher un modèle de recommandation de média enrichi, ajoutez les balises et les appels d’API requis à la page.

1. Dans l’en-tête de la page :
   1. Installez la balise RTP.
   1. Ajoutez l’appel GET qui renseigne les recommandations.
   1. Ajoutez l’appel SET qui configure le modèle.
1. Dans le corps de la page :
   1. Placez la balise de modèle (classe div) à l’endroit où vous souhaitez que le modèle apparaisse.

Pour plus d’informations, voir [Activer le contenu prédictif pour les médias riches en contenu web](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/predictive-content/enabling-predictive-content/enable-predictive-content-for-web-rich-media).

## Balise de modèle

| Attribut | Facultatif/obligatoire | Description |
| --- | --- | --- |
| une classe | Obligatoire | Identifie l’élément div HTML en tant que div de recommandation RTP. |
| data-rtp-template-id | Obligatoire | Détermine l’alignement des recommandations. Utilisez « template1 » pour l’alignement horizontal, « template2 » pour l’alignement vertical ou « template3 » pour l’alignement vertical avec uniquement un titre et une description. Le script injecte le modèle correspondant dans ce `div`. Valeurs autorisées : template1, template2, template3. |

### Exemples

Utilisez « template1 » pour afficher les recommandations horizontalement.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
```

Utilisez « template2 » pour afficher des recommandations verticalement.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template2"></div>
```

Utilisez « template3 » pour afficher des recommandations verticalement avec uniquement un titre et une description.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template3"></div>
```

Voir les [exemples d’alignement des modèles](#example_of_rich_media_recommendation_template_1).

## Renseigner la recommandation

Cette méthode renseigne toutes les `<divs>` de médias riches de la page avec des recommandations.

### Utilisation

`rtp('get', 'rcmd', 'richmedia');`

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;get&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;rcmd&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| &#39;richmedia&#39; | Obligatoire | Chaîne | Nom de la sous-méthode. |

## Modifier la configuration du modèle

Cette méthode modifie la configuration de modèle par défaut.

Appelez cette méthode avant d&#39;appeler rtp(&#39;get&#39;,&#39;rcmd&#39;, &#39;richmedia&#39;);

### Utilisation

`rtp('set', 'rcmd', 'richmedia', 'template_id', conf_obj);`

| Paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| &#39;set&#39; | Obligatoire | Chaîne | Action de méthode. |
| &#39;rcmd&#39; | Obligatoire | Chaîne | Nom de la méthode. |
| &#39;richmedia&#39; | Obligatoire | Chaîne | Nom de la sous-méthode. |
| template_id | Facultatif | Chaîne | Identifiant du modèle pour les modifications de configuration. Permet de spécifier la modification des paramètres pour un seul modèle. |
| conf_obj | Obligatoire | Objet | Nouvelle configuration. L’objet contient toutes les configurations en tant que paire clé/valeur. |

### Exemples

Cet exemple montre comment modifier le texte de titre d’un modèle.

```javascript
rtp("set", "rcmd", "richmedia","template1",
    {
        "rcmd.title.text": "RECOMMENDED CONTENT"
    }
);
```

Cet exemple montre comment définir des catégories et plusieurs propriétés de configuration pour un modèle.

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

Utilisez « category » pour filtrer le contenu affiché dans les recommandations de contenu prédictif. Pour utiliser le contenu prédictif pour tout le contenu activé, laissez le champ « catégorie » vide.

Pour ne recommander que du contenu spécifique dans le modèle de média enrichi, ajoutez une catégorie pour le contenu sur la page Définir le contenu . Associez ensuite cette catégorie au code du modèle de recommandation. Par exemple, catégorisez le contenu pertinent par sections de produit ou de solution de votre site web.

Cet exemple montre comment définir plusieurs propriétés de configuration pour un modèle.

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
| --- | --- | --- |
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

La prise en charge de la configuration peut varier en fonction du modèle.

#### Exemple de base

Cet exemple montre comment afficher trois recommandations dans un modèle. Copiez l’exemple dans une page HTML, puis remplacez la balise RTP par votre balise .

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

Cet exemple montre comment afficher trois recommandations dans un modèle. Le titre du modèle est « CONTENU RECOMMANDÉ » et le texte du bouton est « En savoir plus ». Copiez l’exemple dans une page HTML, puis remplacez la balise RTP par votre balise .

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

**Name** : template1

**Description** : contenu horizontal qui comprend une image, un titre, une description et un bouton call-to-action.

![Modèle Rich Media](assets/rich-media-template1.png)

#### Exemple de modèle de recommandation de média enrichi #2

**Nom** : template2

**Description** : contenu vertical qui comprend une image, un titre, une description et un bouton call-to-action.

![Modèle Rich Media](assets/rich-media-template2.png)

#### Exemple de modèle de recommandation de média enrichi #3

**Nom** : template3

**Description** : contenu vertical qui comprend uniquement un titre et une description. Lorsque vous pointez dessus, l’en-tête change de couleur et renvoie vers l’URL du contenu. La description renvoie également au contenu sans changer de couleur.

![Modèle Rich Media](assets/rich-media-template3.png)
