---
title: Messages in-app
feature: Mobile Marketing
description: Configurez les messages in-app de Marketo avec Mobile SDK, configurez les déclencheurs d’événement personnalisés, suivez l’activité de clic et corrigez les problèmes d’initialisation de première ouverture d’application.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 1%

---

# Messages in-app

Pour utiliser les fonctionnalités de messagerie In-App de Marketo, procédez comme suit :

1. Installez Marketo Mobile SDK comme décrit dans la section [ Installation mobile ](installation.md).
1. Ajoutez votre application mobile à Marketo, comme décrit dans la section [Ajouter une application mobile](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Vous pouvez éventuellement ajouter du code à votre application mobile pour capturer [Actions personnalisées](custom-actions.md).

Une fois que vous avez installé Marketo Mobile SDK et que vous avez terminé d’ajouter votre application dans Marketo, vous êtes prêt à envoyer des messages In-App qui s’affichent lorsqu’un utilisateur ouvre votre application.

Par défaut, les messages in-app sont déclenchés à l’ouverture de votre application. Si vous souhaitez déclencher des messages in-app pour d’autres événements, tels que lorsqu’une page spécifique est consultée ou lorsqu’un bouton spécifique est enfoncé, vous devez ajouter des actions personnalisées à votre code. Voir la section [Actions personnalisées](custom-actions.md) pour obtenir des exemples de code à ce sujet.

## Dépannage

**Le message in-app ne s’affiche pas**

Marketo ne répond aux déclencheurs des applications qu’après l’initialisation de Marketo Mobile SDK avec Marketo Platform. Le processus d’initialisation se produit lorsque vous installez et ouvrez l’application pour la première fois. Comme l’initialisation se produit après la première ouverture de l’application, l’événement « App Open » n’est pas déclenché tant que l’application n’est pas ouverte une deuxième fois. Fermez l’application et ouvrez-la à nouveau. Un message déclenché par l’ouverture de l’application doit s’afficher sur votre appareil.

Les événements personnalisés sont déclenchés par une interaction de l’utilisateur une fois l’application ouverte. Les événements personnalisés sont reconnus par Marketo au cours de la première session.

**Suivi Des Activités D’Appui In-App**

Veillez à attribuer une action en plus de l’action « ignorer » à l’un des boutons principaux ou secondaires pour effectuer le suivi des activités d’appui et à utiliser les fréquences d’affichage de base en fonction du nombre d’appuis.

Pour plus d’informations, voir la section [Messages In-App](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) dans la documentation de notre produit.
