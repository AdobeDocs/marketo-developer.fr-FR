---
title: API JAVASCRIPT
description: API JavaScript
feature: Munchkin Tracking Code, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 1%

---

# API JavaScript

Voici un aperçu des fonctionnalités d’intégration JavaScript côté client de Marketo. Vous devez disposer d’un compte Marketo pour utiliser ces fonctionnalités. En règle générale, l’implémentation implique simplement l’ajout d’un &quot;code incorporé&quot; à votre propriété web. Vous pouvez éventuellement utiliser des fonctionnalités supplémentaires en appelant les fonctions JavaScript exposées par le code incorporé. Ces fonctions sont décrites en détail ici.

Le code incorporé est unique à votre instance Marketo, car contient un identifiant de compte. Récupérez le code incorporé en accédant au panneau approprié dans l’interface utilisateur de Marketo, copiez-le dans le Presse-papiers, puis collez-le dans votre page web.

## Suivi des pistes (Munchkin)

Le Marketo [code de suivi Munchkin JavaScript](lead-tracking.md) est la clé des fonctionnalités de Marketo. Il vous permet de générer des pistes à partir des visites sur votre site web. Il effectue même le suivi des visiteurs qui ne vous ont pas encore fourni leurs informations personnelles, créant ainsi des pistes anonymes comprenant l’adresse IP de l’utilisateur et d’autres informations. Vous configurez Munchkin sur la page Munchkin dans la zone Admin de Marketo.

## Formulaires 2.0

[Forms 2.0](forms-api-reference.md) permet aux marketeurs de créer des formulaires web beaux, stables et flexibles sans connaissance de la programmation. Forms peut résider sur des landing pages Marketo et être incorporé sur n’importe quelle page de votre site web. Les fonctionnalités de base d’un formulaire web Marketo peuvent être étendues à l’aide de l’API JavaScript Forms 2.0.

## Personnalisation Web

[Marketo Web Personalization](web-personalization.md) est une plateforme de ciblage et de Personalization qui vous permet d’interagir en temps réel avec des milliers de prospects sur votre site web en fonction de leur identité et de ce qu’ils font.

## Prédictif  Contenu

[Contenu prédictif Marketo](predictive-content.md) vous permet d’interagir avec vos visiteurs web avec le contenu le plus pertinent, optimisé par l’apprentissage automatique et l’analyse prédictive. Améliorez votre contenu avec des descriptions textuelles et des images et incorporez plusieurs recommandations de contenu sur votre site web.

## Marketing social

[Marketo Social Marketing](social.md) permet aux marketeurs d’incorporer des widgets sociaux dans des sites web et des landing pages. Les widgets sociaux comprennent des sondages, des boutons de partage sur les réseaux sociaux, des vidéos, des tirages et des promotions comme des offres de parrainage. L’affichage de ces widgets peut être personnalisé, ainsi que les événements capturés, afin de créer des expériences utilisateur uniques.
