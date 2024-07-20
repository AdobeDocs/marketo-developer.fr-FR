---
title: Social
description: Social
feature: Social, Javascript
exl-id: 82d2b86f-5efe-4434-b617-d27f76515a79
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 4%

---

# Social

[Marketo Social Marketing](https://business.adobe.com/products/marketo/social-marketing.html) permet aux marketeurs d’incorporer des widgets sociaux dans des sites web et des landing pages. Les widgets sociaux comprennent des sondages, des boutons de partage sur les réseaux sociaux, des vidéos, des tirages et des promotions comme des offres de parrainage.

## Exemple de widget de partage intégré

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

1. Utilisation de l’interface utilisateur normale du produit et association d’écouteurs d’événements à des informations lorsque certaines actions se sont produites dans l’interface utilisateur pour exécuter une logique commerciale supplémentaire.
1. Remplacement de l’IU normale du produit par une IU personnalisée et activation des &quot;étapes&quot; de la fenêtre contextuelle de l’IU si vous le souhaitez.

## Association d’événements à l’interface utilisateur normale

Il existe deux façons de s’abonner aux événements dans la bibliothèque JavaScript des Forces canadiennes, à l’échelle mondiale ou pour un seul widget. Les événements sont présentés ci-dessous dans le tableau des événements.

### Inscription à un événement global

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

### Abonnement d’événement par widget

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

Cet exemple montre un élément précédemment masqué avec l’identifiant &quot;signedUp&quot; après qu’un utilisateur a terminé l’inscription d’une offre pour un widget nommé &quot;refermented_SignUp&quot;.

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
| share_sent | Se déclenche chaque fois qu’une demande de partage est envoyée au serveur pour traitement. | Tous les widgets pouvant partager | 1.&quot;share_sent&quot; (chaîne)<br>2. Paramètres envoyés (objet) |
| share_success | Se déclenche lorsque la demande de partage est traitée avec succès. | Tous les widgets pouvant être partagés. | 1.&quot;share_success&quot; (chaîne)<br>2. Partager l’objet de réponse, contenant le message envoyé et l’URL raccourcie (objet) |
| voter_success | Se déclenche lorsqu’un utilisateur a voté avec succès dans un sondage. | Widgets Sondage, VS, Vote | 1. &quot;vote_success&quot; (Chaîne)<br>2. Élément voté pour, dont le titre, la description, l’identifiant d’entité (objet) |
| offer_registered | Se déclenche lorsqu’un utilisateur a réussi à s’inscrire à une offre | Tous les widgets d’offre | 1.&quot;offer_registered&quot; (chaîne)<br>2. Modification des propriétés de l’utilisateur (objet),<br>3. Attributs utilisateur modifiés (objet) |
| profile_saved | Se déclenche lorsqu’un utilisateur a mis à jour son profil à partir de la capture de profil. | Tous les widgets sans offre pour lesquels la capture de profil est activée | 1.&quot;profile_saved&quot; (Chaîne)<br>2. Modification des propriétés de l’utilisateur (objet)<br>3. Attributs utilisateur modifiés (objet) |
| video_loaded | Se déclenche lorsqu’une vidéo incorporée est entièrement chargée et initialisée. | Widget VideoShare | 1. &quot;video_loaded&quot; (chaîne) 2. &quot;.cf_videoshare_wrap&quot; Élément contenant la vidéo (objet jQuery) |

## Remplacement de l’IU par une IU personnalisée

Pour remplacer l’IU par une IU personnalisée, vous devez d’abord désactiver l’IU normale, en définissant l’option _popupUIOnly_ sur _true_. Avec cette option définie, l’interface utilisateur standard ne sera pas affichée au chargement de la page. Au lieu de cela, le widget récupère ses données et attend que vous commenciez l’une de ses étapes contextuelles en appelant la fonction _CF.widget.activate_ et en fournissant des options pour ce qu’il doit faire.

Voici un exemple de création d’un bouton personnalisé qui lance le flux d’inscription de l’offre de référence pour un widget d’offre de référence appelé _refermentez_SignUp_.

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

Comme l’ajout de gestionnaires de clics est courant, il existe une méthode de raccourci pour les ajouter. L’exemple suivant est fonctionnellement équivalent à l’exemple précédent.

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

## Obtention de données de l’interface utilisateur des widgets pour une insertion dans votre interface utilisateur de remplacement

Si vous avez besoin de données sur le widget pour dessiner votre interface utilisateur de remplacement, vous pouvez obtenir les données de l’événement spécial _ui_data_. Vous pouvez écouter cet événement avec la fonction `CF.widget.listen` normale, mais cela peut entraîner une condition de concurrence potentielle dans laquelle votre écouteur d’événement est ajouté après que le widget a déjà déclenché l’événement_ui_data_, de sorte que vous ne recevez jamais de données. Pour éviter cette concurrence, utilisez l’événement `CF.widget.uiData_ method instead, which will give you the most recent available _ui_data_, and listen for all future updates as well. The _ui_data` qui est déclenché chaque fois qu’une action a été entreprise, ce qui aurait entraîné la redéfinition de l’interface utilisateur standard du widget, même si vous avez désactivé cette interface utilisateur avec l’option `popupUIOnly`.

Exemple qui utilise la fonction `uiData` pour afficher le nombre d’entrées d’un utilisateur pour un tirage avec le nom de widget _sweeps_Sweepstakes_.

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

## Référence des données de l’interface utilisateur d’inscription aux offres de référent

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire &quot;aaaa-MM-jj&quot;. |
| Nombre | Nombre entier ou flottant |
| Texte complet | Chaîne d’HTML |
| Évaluation | Entier signé 32 bits |
| campagne sfdc | Utilisé dans l’intégration de la gestion de campagne Salesforce |
| Texte | Chaîne de texte |

## Référence des données de l’interface utilisateur TrackProgress de l’offre de référent

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire &quot;aaaa-MM-jj&quot;. |
| Nombre | Nombre entier ou flottant |
| Texte complet | Chaîne d’HTML |
| Évaluation | Entier signé 32 bits |
| campagne sfdc | Utilisé dans l’intégration de la gestion de campagne Salesforce |
| Texte | Chaîne de texte |

## Référence des données de l’interface utilisateur des tirages (pour les tirages de campagne sur les réseaux sociaux, pas pour les tirages au sort LM)

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire &quot;aaaa-MM-jj&quot;. |
| Nombre | Nombre entier ou flottant |
| Texte complet | Chaîne d’HTML |
| Évaluation | Entier signé 32 bits |
| campagne sfdc | Utilisé dans l’intégration de la gestion de campagne Salesforce |
| Texte | Chaîne de texte |

## Référence des données de connexion sociale (widget de remplissage du formulaire)

| Type | Description |
|---------------|----------------------------------------------------|
| Date | Valeur de date du formulaire &quot;aaaa-MM-jj&quot;. |
| Nombre | Nombre entier ou flottant |
| Texte complet | Chaîne d’HTML |
| Évaluation | Entier signé 32 bits |
| campagne sfdc | Utilisé dans l’intégration de la gestion de campagne Salesforce |
| Texte | Chaîne de texte |

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
