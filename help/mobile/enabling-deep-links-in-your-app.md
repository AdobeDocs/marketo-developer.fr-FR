---
title: Activation des liens profonds
feature: Mobile Marketing
description: Découvrez comment activer les liens profonds dans votre application pour les messages push Marketo à l’aide de schémas URI personnalisés, avec les conseils et les bonnes pratiques d’iOS, d’Android et de PhoneGap.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
TQID: https://experienceleague.adobe.com/UswOvHXGlfTrTUqr4Gsf3j2Z7Xpv2FF2luXeygT4qE0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 363
ht-degree: 5%

---

# Activation des liens profonds

Les liens profonds dirigent les personnes vers un contenu spécifique dans votre application. Par exemple, lorsqu’une personne sélectionne un message push mobile qui annonce un t-shirt violet, l’application peut ouvrir le contenu du t-shirt violet au lieu de la page d’accueil.

Le processus fonctionne comme suit :

1. Un utilisateur Marketo place un URI personnalisé dans l’action Appuyer pour un message push.
1. Lorsqu’une personne clique sur le message push sur son appareil, Marketo MME SDK déclenche un événement avec l’URI personnalisé.
1. Votre application traite l’événement et dirige la personne vers le contenu correspondant.

Pour activer ce processus :

1. Définissez une structure d’URI personnalisée pour votre application.
1. Enregistrez le schéma dans votre manifeste d’application.
1. Ajoutez du code qui traite les événements de lien profond et achemine les personnes vers le contenu correspondant.

Pour iOS, consultez la documentation d’Apple sur [Définition d’un schéma d’URL personnalisé pour votre application](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Pour Android, consultez la documentation de Google sur [Activation des liens profonds pour le contenu d’application](https://developer.android.com/training/app-links/deep-linking).

Pour les applications PhoneGap, utilisez un plug-in pour permettre à votre application hybride de répondre aux schémas d’URL personnalisés et aux liens universels/d’application sur iOS et Android. Consultez les [modules externes de lien profond](https://cordova.apache.org/plugins/?q=deeplink) disponibles.

Lorsque vous avez activé la liaison profonde dans votre application, partagez vos URI personnalisés avec vos utilisateurs Marketo afin qu’ils puissent les insérer dans l’action d’appui pour les messages push.

Marketo utilise une structure URI prédéfinie lors de la configuration des appareils de test. Pour plus d&#39;informations, consultez la section « Périphériques de test » du [ Guide d&#39;installation](installation.md).

## Bonnes pratiques relatives à la définition d’une structure URI

Si votre marque dispose d’un site pour appareils mobiles, suivez sa structure d’URL lorsque vous définissez l’URI de lien profond. Par exemple, si l’URL du produit est `https://myappname.com/products/purple-shirt`, utilisez `myappname://products/purple-shirt` comme URI de lien profond correspondant.

Utilisez un modèle propre à votre marque. Bien qu’aucune réglementation n’exige que les schémas soient globalement uniques, vous pouvez aider à créer un schéma unique en inversant votre nom de domaine, tel que `org.companyname`.
