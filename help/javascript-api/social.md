---
title: Social
description: Social
feature: Social, Javascript
exl-id: 82d2b86f-5efe-4434-b617-d27f76515a79
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 4%

---

# Social

[Marketo Social Marketing](https://business.adobe.com/products/marketo/social-marketing.html) permet aux spécialistes du marketing d’incorporer des widgets de réseaux sociaux dans des sites web et des pages de destination. Les widgets sociaux incluent les sondages, les boutons de partage sur les réseaux sociaux, les vidéos, les tirages au sort et les promotions comme les offres de référence.

## Exemple de widget de partage incorporé

```html
<!-- Marketo Widget Loader Script -->

<script type="text/javascript" src="//b2c-mlm.marketo.com/jsloader/271d8232-1500-4305-b7ed-05d451b9ee0c/loader.php.js">
</script>

 <!-- The Location of the Social Widget -->

<divclass='cf_widgetloader cf_w_245d8f3c0955454cbd26abc39d0d874c'="" options="{&quot;outerHeight&quot;:400, &quot;outerWidth&quot;:600}">
</divclass='cf_widgetloader'>
```

![social-share-widget](assets/social-share-widget.png)

Il existe deux méthodes de base pour personnaliser un widget social :

1. Utilisez l’interface utilisateur normale du produit et joignez des écouteurs d’événement pour être informés lorsque certaines actions se sont produites dans l’interface utilisateur afin d’exécuter une logique commerciale supplémentaire.
1. Remplacement de l’interface utilisateur normale du produit par une interface utilisateur personnalisée et activation des « étapes » contextuelles de l’interface utilisateur, le cas échéant.

## Association d’événements à l’interface utilisateur normale

Il existe deux manières de s’abonner à des événements dans la bibliothèque JavaScript de CF, globalement ou pour un seul widget. Les événements sont documentés ci-dessous dans le tableau des événements.

### Abonnement à un événement global

```html
<script>
cf_scripts.afterload(function(){
    CF.events.listen("event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever ANY widget fires the event "event_name_here".
        }
    );
});
</script>
```

### Abonnement À Un Événement Par Widget

```html
<script>
cf_scripts.afterload(function(){
    CF.widget.listen("widget_name_here", "event_name_here",
        function(event, arg1){
            //Your code to do something on the event goes here.
            //It will be fired whenever the widget named "widget_name_here" fires the event "event_name_here".
        }
    );
});
</script>
```

## Exemple

Cet exemple montre un élément précédemment masqué avec l’ID « signedUp » une fois qu’un utilisateur a terminé une inscription d’offre pour un widget nommé « referral_SignUp ».

```html
<div id='signedUp'style='display:none; color:green;'>This is a custom message to let you know that you signed up!</div>
<div class='cf_widgetLoader cf_w_referral_SignUp'></div>

<script>
    cf_scripts.afterload(function(){
        CF.widget.listen("referral_SignUp", "offer_enrolled", function(){
        cf_jq("#signedUp").show();
    });
});
</script>
```

## Tableau des événements de base

| Nom de l’événement | Description | Widgets qui utilisent cet événement | Arguments pris en charge (transmis à la fonction de rappel d’événement) |
| --- | --- | --- | --- |
| share_sent | Se déclenche chaque fois qu’une demande de partage est envoyée au serveur pour traitement | Tous les widgets qui peuvent être partagés | &#x200B;1. »share_sent » (chaîne)<br>2. Paramètres envoyés (objet) |
| share_success | Se déclenche lorsque la demande de partage est traitée avec succès. | Tous les widgets qui peuvent être partagés. | &#x200B;1. »share_success » (chaîne)<br>2. Partager l&#39;objet de réponse contenant le message envoyé et l&#39;URL raccourcie (objet) |
| vote_success | Se déclenche lorsqu’un utilisateur a voté avec succès dans un sondage. | Widgets Sondage, VS, Vote | &#x200B;1. « vote_success » (chaîne)<br>2. Élément voté, y compris le titre, la description, l’identifiant d’entité (objet) |
| offer_enrolled | Se déclenche lorsqu’un utilisateur s’est inscrit avec succès à une offre | Tous les widgets d’offre | &#x200B;1. »offer_enrolled » (chaîne)<br>2. Propriétés utilisateur modifiées (objet),<br>3. Attributs utilisateur modifiés (objet) |
| profile_saved | Se déclenche lorsqu’un utilisateur a mis à jour son profil à partir de la capture de profil | Tous les widgets hors offre dont la capture de profil est activée | &#x200B;1. »profile_saved » (chaîne)<br>2. Modification des propriétés utilisateur (objet)<br>3. Attributs utilisateur modifiés (objet) |
| video_loaded | Se déclenche lorsqu’une vidéo intégrée est complètement chargée et initialisée. | Widget VideoShare | &#x200B;1. « video_loaded » (chaîne) 2. Élément « .cf_videoshare_wrap » contenant la vidéo (objet jQuery) |

## Remplacement de l’interface utilisateur par une interface utilisateur personnalisée

Pour remplacer l’interface utilisateur par une interface utilisateur personnalisée, vous devez d’abord désactiver l’interface utilisateur normale en définissant l’option _popupUIOnly_ sur _true_. Lorsque cette option est définie, l’interface utilisateur standard n’effectue pas le rendu au chargement de la page. Au lieu de cela, le widget récupère ses données et attend que vous commenciez l’une de ses phases contextuelles en appelant la fonction _CF.widget.activate_ et en fournissant des options sur ce qu’elle doit faire.

Voici un exemple de création d’un bouton personnalisé qui lance le flux d’inscription à une offre de référence pour un widget d’offre de référence nommé _referral_SignUp_.

```html
<button id="myNewSignUpButton">My newSign Up button</button>

<!-- Turn off the defaultreferral offer UI by setting popupUIOnly to true-->
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // After the cf script library has loaded, find the button with
    // id="myNewSignUpButton", and attach a click listener to it.
    $("#myNewSignUpButton").click(function(){
        // When it is clicked, activate the popup widget flow for the referral,
        // asking it to point to the clicked button.
        CF.widget.activate("referral_SignUp", {pointTo:$(this)});
    });
});
</script>
```

Comme l’ajout de gestionnaires de clics est courant, il existe une méthode de raccourci pour les ajouter. Ce qui suit est fonctionnellement équivalent à l&#39;exemple précédent.

```html
<button id="myNewSignUpButton">My newSign Up button</button>
<div class="cf_widgetLoader cf_w_referral_SignUp" options="{popupUIOnly:true}"></div>

<script>
cf_scripts.afterload(function($, CF){
    // Use the addClickActivate convenience method, which will
    // automatically make the popup point at the clicked item with id myNewSignUpButton.
    CF.widget.addClickActivate("#myNewSignUpButton", "referral_SignUp", {});
});
</script>
```

## Obtention des données de l’interface utilisateur du widget à placer dans l’interface utilisateur de remplacement

Si vous avez besoin de données sur le widget pour dessiner votre interface utilisateur de remplacement, vous pouvez obtenir les données de l’événement spécial _ui_data_. Vous pouvez écouter cet événement avec la fonction `CF.widget.listen` normale, mais cela peut entraîner une condition de concurrence potentielle dans laquelle l’écouteur d’événement est ajouté une fois que le widget a déjà déclenché l’événement _ui_data_ , vous ne recevez donc jamais de données. Pour éviter cette concurrence, utilisez l’événement `CF.widget.uiData_ method instead, which will give you the most recent available _ui_data_, and listen for all future updates as well. The _ui_data` qui est déclenché chaque fois qu’une action effectuée aurait entraîné le redimensionnement de l’interface utilisateur standard du widget, même si vous avez désactivé cette interface utilisateur avec `popupUIOnly` option.

Exemple qui utilise `uiData` fonction pour afficher le nombre d’entrées dont dispose un utilisateur pour un tirage au sort avec le nom de widget _sweeps_Sweepstakes_.

```html
<span>You have <span id="entryCount">?</span> entries.</span>

<div class="cf_widgetLoader cf_w_sweeps_Sweepstakes"></div>

<button id='myNewSweepsButton'>New Sweeps Up Button!</button>

<script>
cf_scripts.afterload(function($, CF){
    CF.widget.uiData("sweeps_Sweepstakes", function(uiData){
        if(uiData.user && uiData.userStatus && uiData.userEntries){
            $("entryCount").html(""+ uiData.userEntries);
        }
        else{
            $("entryCount").html("0");
        }
    });
});
</script>
```

## Référence des données de l’interface utilisateur d’inscription à une offre de référence

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire « aaaa-MM-jj » |
| Nombre | Nombre entier ou à virgule flottante |
| Texte complet | Une chaîne HTML |
| Évaluation | Nombre entier 32 bits signé |
| sfdc campaign | Utilisé dans l’intégration de la gestion de campagnes Salesforce |
| texte | Chaîne de texte |

## Référence des données de l’interface utilisateur du suivi de la progression des offres de référence

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire « aaaa-MM-jj » |
| Nombre | Nombre entier ou à virgule flottante |
| Texte complet | Une chaîne HTML |
| Évaluation | Nombre entier 32 bits signé |
| sfdc campaign | Utilisé dans l’intégration de la gestion de campagnes Salesforce |
| texte | Chaîne de texte |

## Référence des données de l’interface utilisateur des tirages au sort (pour les tirages au sort des campagnes sociales, et non les tirages au sort LM)

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire « aaaa-MM-jj » |
| Nombre | Nombre entier ou à virgule flottante |
| Texte complet | Une chaîne HTML |
| Évaluation | Nombre entier 32 bits signé |
| sfdc campaign | Utilisé dans l’intégration de la gestion de campagnes Salesforce |
| texte | Chaîne de texte |

## Référence Des Données De Connexion Sociale (Widget De Remplissage De Formulaire)

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire « aaaa-MM-jj » |
| Nombre | Nombre entier ou à virgule flottante |
| Texte complet | Une chaîne HTML |
| Évaluation | Nombre entier 32 bits signé |
| sfdc campaign | Utilisé dans l’intégration de la gestion de campagnes Salesforce |
| texte | Chaîne de texte |

```javascript
{
"alt_id": "http://www.facebook.com/profile.php?id=1526228678",
"provider_name": "facebook",
"default_photo_url": "https://graph.facebook.com/1526228678/picture?type=large",
"email": "ian.b.taylor@gmail.com",
"verified_email": "ian.b.taylor@gmail.com",
"gender": "male",
"preferred_user_name": "IanTaylor",
"display_name": "Ian Taylor",
"birth_date": 343954800000,
"first_name": "Ian",
"last_name": "Taylor",
"city": null,
"state": null,
"region": null,
"postal_code": null,
"country": null,
"time_zone": null,
"connection_count": 0,
"credentials": {
"uid": "1526228678",
"scopes": "publish_actions",
"expires": "1371994082",
"accessToken": "BAAGFJ0KUFpcBABuNMptmYY...",
"type": "Facebook"
},
"about_me": null,
"cur_pos_title": "Senior Staff Engineer",
"phone_number": null,
"company": "Marketo",
"cur_pos_start_date": 1333231200000,
"cur_pos_summary": null
}
```
