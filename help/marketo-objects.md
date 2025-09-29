---
title: Objets Marketo
feature: Email Programs
description: Guide d’utilisation de Marketo Velocity avec les leads, les opportunités et les objets personnalisés, les champs de chargement, l’accès à la liste des 10 premiers, les relations SFDC et $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Objets Marketo

L’implémentation de Marketo Velocity peut fonctionner sur des données provenant de plusieurs sources dans Marketo : Leads, Opportunités, Objets personnalisés, Application mobile, Installation d’application mobile.

## Champs en cours de chargement


Pour charger un champ à utiliser dans un script, ce champ doit être vérifié sous la liste correspondante dans l’éditeur de jeton de script.

Si vous ne chargez pas de champ et qu’il est référencé dans le script, l’exécution du script échoue au moment de l’exécution. Vous pouvez faire glisser des champs du menu de champs vers le script. Cela permet de les charger et ajoute une référence au champ au niveau du curseur.

## Listes d’opportunités et d’objets personnalisés

Lors de la récupération à partir d’Opportunités ou d’Objets personnalisés, seuls les 10 objets d’un type mis à jour le plus récemment sont chargés. Vous pouvez augmenter le nombre d’objets personnalisés disponibles en suivant les étapes décrites ici. Elles sont données sous forme de liste, avec le nom `<objectName>List` et sont classées de l’enregistrement le plus récemment mis à jour à l’enregistrement le moins récemment mis à jour. Ainsi, pour accéder au champ Montant à partir de l’opportunité qui a été mise à jour le plus récemment, vous devez utiliser ce qui suit :

`${OpportunityList.get(0).Amount}`

Dans cet exemple, vous référencez l’objet OpportunityList, utilisez la méthode get pour accéder à l’enregistrement indexé à 0, puis récupérez la propriété Amount de l’objet renvoyé. Si vous faites glisser un champ d’un objet Opportunité ou Personnalisé vers l’éditeur, il récupère automatiquement le champ de l’enregistrement indexé à 0.

## Relations d’objet personnalisé SFDC

Pour pouvoir être utilisé, un objet personnalisé SFDC ne doit avoir qu’une seule relation avec le prospect Marketo. Les objets sont souvent liés à la fois par le biais du contact et du compte. Il est donc important de synchroniser les objets uniquement avec Marketo lorsque la relation lead/contact est activée.

## Déclencher des objets

Lorsqu’une campagne est déclenchée via le déclencheur Ajouté à l’opportunité, l’opportunité est mise à jour ou ajoutée à des déclencheurs de `<Custom Object Name>`, une variable spéciale est disponible dans les Jetons de script exécutés dans le contexte de la campagne de déclenchement : `$TriggerObject ` (non pris en charge pour `<Custom Object Name>` le déclencheur est Mise à jour).  Si un jeton utilisant une référence de `$TriggerObject` est utilisé dans une campagne par lots, l’envoi de l’e-mail échoue, car cet objet n’est disponible dans aucune campagne par lots.  Il s’agit d’une référence à l’objet qui a déclenché la campagne. L’objet contient toutes les données dont dispose l’enregistrement lorsqu’il est accessible via un autre nom de variable.

Par exemple, si une campagne a été déclenchée via un objet personnalisé pour une commande de produit, la commande à laquelle le prospect a été ajouté est exposée dans la variable `$TriggerObject` .

Voici un exemple de script pour un e-mail de suivi de commande :

```html
<div>
<strong>Your order information:</strong>
##pull information from the Triggering Order and format it in a list
<ul>
<li>Product Ordered: $!{TriggerObject.ProductName}</li>
<li>Product Quantity: $!{TriggerObject.Quanitity}</li>
<li>Shipping Address: $!{TriggerObject.ShippingAddress}</li>
<li>Billing Address: $!{TriggerObject.BillingAddress}</li>
<li>Order Total: $!{TriggerObject.Amount}</li>
</ul>
<p><a href="$!{TriggerObject.OrderURL}">View Your Order Online</a></p>
</div>
```

L’avantage de l’utilisation de la variable `$TriggerObject` est que vous n’avez pas besoin de dédier de code pour déterminer lequel des objets disponibles vous souhaitez extraire vos données locales.  L’objet est déterminé par l’action de déclenchement. Il s’agit de la méthode la plus explicite pour choisir un objet à référencer. Elle doit être utilisée lorsqu’elle est disponible et appropriée.

Remarque : lors de l&#39;utilisation du `$TriggerObject`, les champs doivent être cochés dans le volet d&#39;édition pour que l&#39;objet soit disponible pour le script.

Remarque 2 : le `$TriggerObject` fonctionne uniquement pour les déclencheurs « ajoutés » et non pour les déclencheurs « mis à jour ».
