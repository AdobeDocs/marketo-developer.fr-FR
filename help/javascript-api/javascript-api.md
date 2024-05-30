---
title: "API JavaScript"
description: "API JavaScript"
feature: Munchkin Tracking Code, Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 1%

---


# API JavaScript

Voici un aperçu des fonctionnalités d’intégration JavaScript côté client de Marketo. Vous devez disposer d’un compte Marketo pour utiliser ces fonctionnalités. En règle générale, l’implémentation implique simplement l’ajout d’un &quot;code incorporé&quot; à votre propriété web. Vous pouvez éventuellement utiliser des fonctionnalités supplémentaires en appelant les fonctions JavaScript exposées par le code incorporé. Ces fonctions sont décrites en détail ici.

Le code incorporé est unique à votre instance Marketo, car contient un identifiant de compte. Récupérez le code incorporé en accédant au panneau approprié dans l’interface utilisateur de Marketo, copiez-le dans le Presse-papiers, puis collez-le dans votre page web.

## Suivi des pistes (Munchkin)

Marketo [Code de suivi JavaScript Munchkin](lead-tracking.md) est la clé des fonctionnalités de Marketo. Il vous permet de générer des pistes à partir des visites sur votre site web. Il effectue même le suivi des visiteurs qui ne vous ont pas encore fourni leurs informations personnelles, créant ainsi des pistes anonymes comprenant l’adresse IP de l’utilisateur et d’autres informations. Vous configurez Munchkin sur la page Munchkin dans la zone Admin de Marketo.

## Formulaires 2.0

[Forms 2.0](forms-api-reference.md) Permet aux marketeurs de créer des formulaires web beaux, stables et flexibles sans connaissance de la programmation. Forms peut résider sur des landing pages Marketo et être incorporé sur n’importe quelle page de votre site web. Les fonctionnalités de base d’un formulaire web Marketo peuvent être étendues à l’aide de l’API JavaScript Forms 2.0.

## Personnalisation Web

[Personnalisation web Marketo](web-personalization.md) est une plateforme de ciblage et de personnalisation qui vous aide à interagir en temps réel avec des milliers de prospects sur votre site web en fonction de ce qu’ils sont et de ce qu’ils font.

## Contenu prédictif

[Contenu prédictif Marketo](predictive-content.md) vous permet d’interagir avec vos visiteurs web avec le contenu le plus pertinent, optimisé par l’apprentissage automatique et l’analyse prédictive. Améliorez votre contenu avec des descriptions textuelles et des images et incorporez plusieurs recommandations de contenu sur votre site web.

## Marketing social

[Marketo Social Marketing](social.md) permet aux marketeurs d’incorporer des widgets sociaux dans les sites web et les landing pages. Les widgets sociaux comprennent des sondages, des boutons de partage sur les réseaux sociaux, des vidéos, des tirages et des promotions comme des offres de parrainage. L’affichage de ces widgets peut être personnalisé, ainsi que les événements capturés, afin de créer des expériences utilisateur uniques.
