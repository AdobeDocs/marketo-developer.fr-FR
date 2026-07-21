---
title: Messages in-app
feature: Mobile Marketing
description: Configurez les messages in-app de Marketo avec Mobile SDK, configurez les déclencheurs d’événement personnalisés, suivez l’activité de clic et corrigez les problèmes d’initialisation de première ouverture d’application.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
TQID: https://experienceleague.adobe.com/RVkEUBaFb-PHd0gE9ngzYc5zOojINwSI7ic2TmcU7-8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 321
ht-degree: 2%

---

# Messages in-app

Pour utiliser la messagerie in-app Marketo, procédez comme suit :

1. Installez Marketo Mobile SDK comme décrit dans la section [&#x200B; Installation mobile &#x200B;](installation.md).
1. Ajoutez votre application mobile à Marketo, comme décrit dans la section [Ajouter une application mobile](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Facultatif : ajoutez du code à votre application mobile pour capturer [actions personnalisées](custom-actions.md).

Après avoir installé Marketo Mobile SDK et ajouté votre application à Marketo, vous pouvez envoyer des messages in-app qui s’affichent lorsqu’un utilisateur ouvre votre application.

Par défaut, les messages in-app sont déclenchés à l’ouverture de l’application. Pour déclencher un message pour un autre événement, tel que l’affichage d’une page spécifique ou la sélection d’un bouton spécifique, ajoutez une action personnalisée à votre code. Voir [Actions personnalisées](custom-actions.md) pour obtenir des exemples de code.

## Dépannage

**Le message in-app ne s’affiche pas**

Marketo ne répond aux déclencheurs d’application qu’après l’initialisation de Marketo Mobile SDK avec Marketo Platform. L’initialisation se produit lorsque vous installez et ouvrez l’application pour la première fois.

Comme l’initialisation a lieu après la première ouverture de l’application, l’événement « App Open » n’est pas déclenché tant que vous n’avez pas ouvert l’application une deuxième fois. Fermez et rouvrez l’application. Un message déclenché par l’ouverture de l’application doit alors s’afficher sur votre appareil.

Les événements personnalisés sont déclenchés par une interaction de l’utilisateur une fois l’application ouverte. Les événements personnalisés sont reconnus par Marketo au cours de la première session.

**Suivi Des Activités D’Appui In-App**

Pour effectuer le suivi des activités d’appui et baser la fréquence d’affichage sur le nombre d’appuis, affectez une action autre que « ignorer » à un bouton principal ou secondaire.

Pour plus d’informations, voir [Messages In-App](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) dans la documentation du produit.
