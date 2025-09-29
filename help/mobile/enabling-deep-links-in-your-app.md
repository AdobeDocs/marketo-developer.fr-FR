---
title: Activation des liens profonds
feature: Mobile Marketing
description: Découvrez comment activer les liens profonds dans votre application pour les messages push Marketo à l’aide de schémas URI personnalisés, avec les conseils et les bonnes pratiques d’iOS, d’Android et de PhoneGap.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---

# Activation des liens profonds

Les liens profonds vous permettent de rediriger des personnes vers du contenu (ressources) spécifique dans votre application. Par exemple, lorsqu’une personne clique sur un message push mobile qui annonce un t-shirt violet, vous pouvez ouvrir l’application directement sur le contenu du t-shirt violet (plutôt que sur la page d’accueil).

Le processus fonctionne comme suit :

1. L’utilisateur de Marketo place un URI personnalisé dans l’action Appuyer pour son message push.
1. Lorsqu’une personne clique sur le message push sur son appareil, Marketo MME SDK déclenche un événement avec l’URI personnalisé.
1. Votre application traite ensuite l’événement et redirige la personne vers le contenu approprié dans votre application.

Pour ce faire, vous devez définir une structure d’URI personnalisée pour votre application, enregistrer le schéma dans le manifeste de votre application, puis ajouter du code pour traiter les événements de lien profond et acheminer vers l’emplacement approprié dans votre application.

Pour iOS, reportez-vous à la documentation d’Apple sur [Définition d’un schéma d’URL personnalisé pour votre application](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Pour Android, reportez-vous à la documentation de Google sur [Activation des liens profonds pour le contenu d’application](https://developer.android.com/training/app-links/deep-linking).

Pour les applications PhoneGap, la liaison profonde n’est pas aussi simple que pour les applications iOS ou Android natives, mais il existe des plug-ins qui permettent à votre application hybride de répondre aux schémas d’URL personnalisés de lien profond et aux liens universels/d’application sur iOS et Android. Tenez compte de [ ces modules externes ](https://cordova.apache.org/plugins/?q=deeplink).

Lorsque vous avez activé la liaison profonde dans votre application, partagez vos URI personnalisés avec vos utilisateurs Marketo afin qu’ils puissent les insérer dans l’action d’appui pour les messages push.

Marketo utilise une structure URI prédéfinie lors de la configuration des appareils de test. Pour plus d&#39;informations, reportez-vous à la section « Périphériques de test » du [ Guide d&#39;installation ](installation.md).

## Bonnes pratiques relatives à la définition d’une structure URI

Si votre marque dispose d’un site mobile existant, il est recommandé de suivre également sa structure d’URL pour l’URI de lien profond. Par exemple, si `https://myappname.com/products/purple-shirt` est l’adresse de votre site web pour le produit en question, `myappname://products/purple-shirt` serait une bonne structure d’URI de lien profond à utiliser dans votre application.

En règle générale, vos schémas doivent être propres à votre marque. Bien qu’il n’existe actuellement aucune réglementation pour rendre les schémas uniques dans le monde, une façon de vous assurer que vos schémas sont uniques est d’inverser votre nom de domaine (par exemple, `org.companyname`).
