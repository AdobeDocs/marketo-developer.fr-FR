---
title: Activation des liens profonds
feature: "Mobile Marketing"
description: "Instructions pour l’activation des liens profonds"
source-git-commit: cb000968c78e062b3c17be7d0faa6236c73e7358
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---


# Activation des liens profonds

Les liens profonds vous permettent de rediriger les personnes vers du contenu (ressources) spécifique au sein de votre application. Par exemple, lorsqu’une personne clique sur un message push mobile qui fait de la publicité pour un t-shirt violet, vous pouvez ouvrir l’application directement au contenu du t-shirt violet (plutôt que la page d’accueil).

Le processus fonctionne comme suit :

1. L’utilisateur Marketo place un URI personnalisé dans l’action Appuyez sur pour son message push.
1. Lorsqu’une personne appuie sur le message push sur son appareil, le SDK MME de Marketo déclenche un événement avec l’URI personnalisé.
1. Votre application traite ensuite l’événement et redirige la personne vers le contenu approprié de votre application.

Pour ce faire, vous devez définir une structure URI personnalisée pour votre application, enregistrer le schéma dans le manifeste de votre application, puis ajouter du code pour traiter les événements de lien profond et accéder à l’emplacement approprié dans votre application.

Pour iOS, reportez-vous à la documentation Apple sur [Définition d’un schéma d’URL personnalisé pour votre application](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Pour Android, reportez-vous à la documentation Google sur [Activation des liens profonds pour le contenu de l’application](https://developer.android.com/training/app-links/deep-linking).

Pour les applications PhoneGap, les liens profonds ne sont pas aussi directs qu’avec les applications iOS ou Android natives, mais il existe des modules externes qui permettent à votre application hybride de répondre à des schémas d’URL personnalisés de liens profonds et à des liens universels/d’application sur iOS et Android. Étudier [ces modules externes](https://cordova.apache.org/plugins/?q=deeplink).

Lorsque vous avez activé la création de liens profonds dans votre application, partagez vos URI personnalisés avec vos utilisateurs Marketo afin qu’ils puissent les insérer dans l’action Appuyez sur pour les messages push.

Marketo utilise une structure d’URI prédéfinie lors de la configuration des appareils de test. Reportez-vous à la section &quot;Périphériques de test&quot; du [Guide d’installation](installation.md) pour plus d’informations.

## Bonnes pratiques pour définir une structure d’URI

Si votre marque possède un site mobile existant, il est recommandé de suivre également sa structure d’URL pour l’URI de lien profond. Par exemple, si `https://myappname.com/products/purple-shirt` correspond à l’adresse de votre site web pour le produit en question, puis `myappname://products/purple-shirt` serait une bonne structure d’URI de lien profond à utiliser dans votre application.

En règle générale, vos schémas doivent être propres à votre marque. Bien qu’il n’existe actuellement aucune réglementation pour rendre les schémas uniques dans le monde, une façon de vous assurer que vos schémas sont uniques est d’inverser votre nom de domaine (par exemple, `org.companyname`).
