---
title: "Messages In-App"
feature: "Mobile Marketing"
description: "Présentation des messages In-App"
source-git-commit: e8bb45a7b3bee71c3d0ab6117296a75c8959d72e
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 1%

---


# Messages internes à l’application

Pour utiliser les fonctionnalités de messagerie In-App de Marketo, vous devez effectuer les étapes suivantes :

1. Installez le SDK Mobile Marketo comme décrit dans la section [Installation mobile](installation.md).
1. Ajoutez votre application mobile à Marketo comme décrit dans la section [Ajout d’une application mobile](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Vous pouvez éventuellement ajouter du code à votre application mobile pour la capture. [Actions personnalisées](custom-actions.md).

Une fois que vous avez installé le SDK Mobile Marketo et que vous avez terminé l’ajout de votre application dans Marketo, vous êtes prêt à envoyer des messages In-App qui s’affichent lorsqu’un utilisateur ouvre votre application.

Par défaut, les messages in-app sont déclenchés à l’ouverture de votre application. Si vous souhaitez déclencher des messages in-app pour d’autres événements, par exemple lorsqu’une page spécifique est consultée ou lorsqu’un bouton spécifique est appuyé, vous devez ajouter des actions personnalisées à votre code. Voir [Actions personnalisées](custom-actions.md) pour des exemples de code.

## Dépannage

**Le message in-app ne s’affiche pas**

Marketo répond aux déclencheurs à partir des applications uniquement une fois que le SDK Marketo Mobile est initialisé avec la plateforme Marketo. Le processus d’initialisation se produit lorsque vous installez et ouvrez l’application pour la première fois. L’initialisation ayant lieu après la première ouverture de l’application, l’événement &quot;Ouverture de l’application&quot; n’est pas déclenché tant que l’application n’est pas ouverte une seconde fois. Fermez l’application et rouvrez-la. Un message déclenché par l’Ouverture de l’application doit s’afficher sur votre appareil.

Les événements personnalisés sont déclenchés par l’interaction de l’utilisateur une fois l’application ouverte. Les événements personnalisés sont reconnus par Marketo lors de la première session.

**Suivi de l’activité de l’utilisateur dans l’application**

Veillez à attribuer une action en plus de &quot;Ignorer&quot; à l’un des boutons primaires ou secondaires pour effectuer le suivi des activités d’appui et à utiliser les fréquences d’affichage de base en fonction du nombre de clics.

Pour plus d’informations, voir [Messages In-App](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) dans la documentation du produit.
