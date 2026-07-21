---
title: Objets Marketo
feature: Email Programs
description: Guide d’utilisation de Marketo Velocity avec les leads, les opportunités et les objets personnalisés, les champs de chargement, l’accès à la liste des 10 premiers, les relations SFDC et $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
TQID: https://experienceleague.adobe.com/PvLJb-AOk6DKaNINycpzk5ojZiL8UNcanRg3vXmsGCI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 452
ht-degree: 1%

---

# Objets Marketo

L’implémentation de Marketo Velocity peut utiliser les données provenant des sources Marketo suivantes :

- Prospects
- Opportunités
- Objets personnalisés
- Application mobile
- Installation de l’application mobile

## Champs en cours de chargement

Pour utiliser un champ dans un script, sélectionnez-le dans la liste correspondante de l’éditeur de jeton de script.

Si un script fait référence à un champ qui n’est pas chargé, le script échoue au moment de l’exécution. Faites glisser un champ du menu de champ vers le script pour le charger et ajouter une référence au niveau du curseur.

## Listes d’opportunités et d’objets personnalisés

Pour Opportunités et Objets personnalisés, Marketo ne charge que les 10 objets les plus récemment mis à jour de chaque type. Vous pouvez augmenter le nombre d’objets personnalisés disponibles en suivant les étapes décrites ici.

Marketo fournit les objets dans une liste nommée `<objectName>List`, classée de l’enregistrement le plus récemment mis à jour à l’enregistrement le moins récemment mis à jour. Pour accéder au champ Montant à partir de l’opportunité mise à jour le plus récemment, utilisez :

`${OpportunityList.get(0).Amount}`

Cet exemple fait référence à l&#39;objet OpportunityList, utilise la méthode get pour accéder à l&#39;enregistrement à l&#39;index 0 et récupère la propriété Amount à partir de cet enregistrement.

Lorsque vous faites glisser un champ Opportunité ou Objet personnalisé dans l’éditeur, Marketo récupère automatiquement le champ de l’enregistrement à l’index 0.

## Relations d’objet personnalisé SFDC

Pour utiliser un objet personnalisé SFDC, l’objet ne doit avoir qu’une seule relation avec le prospect Marketo. Les objets sont souvent liés à la fois par le biais du contact et du compte. Synchronisez uniquement les objets pour lesquels la relation lead/contact est activée.

## Déclencher des objets

Lorsqu’une campagne utilise la variable Ajoutée à l’opportunité, l’opportunité est mise à jour ou ajoutée à `<Custom Object Name>` déclencheur, la variable `$TriggerObject` est disponible pour les jetons de script qui s’exécutent dans la campagne de déclenchement. Cette variable n’est pas prise en charge pour le déclencheur `<Custom Object Name>` est mis à jour .

Cette variable fait référence à l’objet qui a déclenché la campagne. Il contient les mêmes données d’enregistrement que celles disponibles lorsque vous accédez à l’objet via un autre nom de variable.

N’utilisez pas de jeton qui fait référence à `$TriggerObject` dans une campagne par lots. L’objet n’est pas disponible dans les campagnes par lots et l’envoi de l’e-mail échoue.

Par exemple, si un objet personnalisé pour une commande de produit déclenche une campagne, la variable `$TriggerObject` expose la commande à laquelle le prospect a été ajouté.

L’exemple suivant illustre un script pour un e-mail de suivi de commande :

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

L’action de déclenchement détermine l’objet . Vous n’avez pas besoin de code supplémentaire pour déterminer quel objet disponible contient les données locales. Utilisez `$TriggerObject` lorsqu’il est disponible et approprié, car il identifie explicitement l’objet à référencer.

Remarque : lorsque vous utilisez `$TriggerObject`, sélectionnez les champs de l&#39;objet dans le volet d&#39;édition pour les rendre disponibles dans le script.

Remarque 2 : `$TriggerObject` fonctionne uniquement pour les déclencheurs « ajoutés », et non pour les déclencheurs « mis à jour ».
