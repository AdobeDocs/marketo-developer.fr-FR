---
title: API JAVASCRIPT
description: Découvrez comment utiliser l’API Marketo JavaScript avec du code intégré pour le suivi des prospects Munchkin, Forms 2.0, Web Personalization et le contenu prédictif.
feature: Munchkin Tracking Code, Forms, Web Personalization, Predictive Content, Social, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
TQID: https://experienceleague.adobe.com/R9kIFBiH6jc64ay85QkumV7jCsFnj9J0t5G4IJKEsJM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 267
ht-degree: 2%

---

# API JavaScript

Les intégrations JavaScript côté client de Marketo fournissent des fonctionnalités de suivi des prospects, de formulaires, de personnalisation web et de contenu prédictif. Vous devez disposer d’un compte Marketo pour utiliser ces fonctionnalités.

L’implémentation implique généralement l’ajout de code incorporé à votre propriété web. Vous pouvez également appeler les fonctions JavaScript exposées par le code incorporé pour ajouter des fonctionnalités.

Le code incorporé est propre à votre instance Marketo, car il contient un identifiant de compte. Dans l’interface utilisateur de Marketo, accédez au panneau approprié, copiez le code intégré dans le presse-papiers, puis collez-le dans la page web.

## Suivi des leads (Munchkin)

Le code de suivi Marketo [Munchkin JavaScript](lead-tracking.md) génère des prospects à partir des visites sur votre site web. Il suit également les visiteurs qui n’ont pas fourni d’informations personnelles et crée des prospects anonymes qui incluent l’adresse IP et d’autres informations de l’utilisateur.

Configurez Munchkin sur la page Munchkin dans la zone Admin de Marketo.

## Formulaires 2.0

[Forms 2.0](forms-api-reference.md) permet aux spécialistes du marketing de créer des formulaires web sans connaissances en programmation. Forms peut résider sur des pages de destination Marketo ou être incorporé sur n’importe quelle page de votre site web.

Utilisez l’API JavaScript Forms 2.0 pour étendre les fonctionnalités principales d’un formulaire web Marketo.

## Web Personalization

[Marketo Web Personalization](web-personalization.md) vous aide à interagir en temps réel avec les prospects de votre site Web en fonction de leur identité et de leurs activités.

## Contenu prédictif

[Contenu prédictif ](predictive-content.md) utilise le machine learning et l’analyse prédictive pour présenter du contenu pertinent aux visiteurs et visiteuses web. Ajoutez des descriptions textuelles et des images à votre contenu, puis incorporez plusieurs recommandations de contenu sur votre site web.
