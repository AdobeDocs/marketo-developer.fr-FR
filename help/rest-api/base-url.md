---
title: URL de base
feature: REST API
description: Découvrez comment créer des requêtes d’API REST Marketo, comprendre les paramètres et la ressource du chemin d’URL de base et rechercher votre URL de base unique.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
TQID: https://experienceleague.adobe.com/NZisV6V-FMPi0RHpdaFrc1kZc3nb15YomwRgohaQmEE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 146
ht-degree: 3%

---

# URL de base

Chaque appel API dans la [ Référence du point d’entrée ](endpoint-reference.md) spécifie la méthode REST, le chemin d’accès, la ressource et les paramètres. Ajoutez ces composants à l’URL de base pour former une requête.

Voici un exemple d’URL REST bien formée :

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

L’exemple contient les composants suivants :

- **URL de base :** `https://284-RPR-133.mktorest.com/rest`
- **Chemin:** `/v1/lead/`
- **Ressource:** `318582.json`
- **Paramètre de requête :** `fields=email,firstName,lastName`

L’URL de base contient l’identifiant du compte, également appelé Munchkin ID, et est propre à chaque abonnement Marketo.

Pour trouver l’URL de base, connectez-vous à Marketo et accédez à **[!UICONTROL Admin]** > **[!UICONTROL Intégration]** > **[!UICONTROL Services Web]**. L’URL de base est intitulée « Point d’entrée : » dans la section « API REST », comme illustré dans l’image suivante.

![Point d’entrée de l’URL de base des services web](assets/rest-api-base-url-web-services.png)

Copiez l’URL de base et incluez-la dans l’URL de chaque appel de l’API REST.
