---
title: API JAVASCRIPT
description: Découvrez comment utiliser l’API Marketo JavaScript avec du code intégré pour le suivi des prospects Munchkin, Forms 2.0, Web Personalization et le contenu prédictif.
feature: Munchkin Tracking Code, Forms, Web Personalization, Predictive Content, Social, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
TQID: https://experienceleague.adobe.com/R9kIFBiH6jc64ay85QkumV7jCsFnj9J0t5G4IJKEsJM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 1%

---

# API JavaScript

Voici un aperçu des fonctionnalités d’intégration de JavaScript côté client de Marketo. Vous devez disposer d’un compte Marketo pour utiliser ces fonctionnalités. En règle générale, l’implémentation implique simplement l’ajout d’un « code intégré » à votre propriété web. Vous pouvez éventuellement utiliser des fonctionnalités supplémentaires en appelant les fonctions JavaScript exposées par le code incorporé. Ces fonctions sont entièrement documentées ici.

Le code incorporé est propre à votre instance Marketo, car contient un identifiant de compte. Obtenez le code incorporé en accédant au panneau approprié dans l’interface utilisateur de Marketo, copiez-le dans le presse-papiers, puis collez-le dans votre page web.

## Suivi des leads (Munchkin)

Le code de suivi Marketo [Munchkin JavaScript](lead-tracking.md) est essentiel aux fonctionnalités de Marketo. Il vous permet de générer des pistes à partir des visites sur votre site web. Il effectue même le suivi des visiteurs qui ne vous ont pas encore communiqué leurs informations personnelles, créant des pistes anonymes qui incluent l’adresse IP et d’autres informations de l’utilisateur. Configurez Munchkin dans la page Munchkin de la zone Admin de Marketo.

## Formulaires 2.0

[Forms 2.0](forms-api-reference.md) permet aux marketeurs de créer des formulaires web beaux, stables et flexibles sans connaissances en programmation. Forms peut résider sur des pages de destination Marketo et être incorporé sur n’importe quelle page de votre site web. Les principales fonctionnalités d’un formulaire web Marketo peuvent être étendues à l’aide de l’API JavaScript Forms 2.0.

## Web Personalization

[Marketo Web Personalization](web-personalization.md) est une plateforme de ciblage et de Personalization qui vous aide à interagir en temps réel avec des milliers de prospects sur votre site web en fonction de leur identité et de leurs activités.

## Contenu prédictif

[Contenu prédictif &#x200B;](predictive-content.md) vous permet d’impliquer vos visiteurs et visiteuses web avec le contenu le plus pertinent, optimisé par le machine learning et l’analyse prédictive. Améliorez votre contenu avec des descriptions textuelles et des images, et incorporez plusieurs recommandations de contenu sur votre site web.

