---
title: URL de base
feature: REST API
description: Découvrez comment créer des requêtes d’API REST Marketo, comprendre les paramètres et la ressource du chemin d’URL de base et rechercher votre URL de base unique.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 2%

---

# URL de base

La documentation [Référence du point d’entrée](endpoint-reference.md) pour chaque appel API affiche la méthode REST, le chemin, la ressource et les paramètres qui doivent être ajoutés à l’URL de base pour former une requête.

Voici un exemple d’URL REST bien formée :

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

qui se compose des parties suivantes :

- URL de base : `https://284-RPR-133.mktorest.com/rest`
- Chemin : `/v1/lead/`
- Ressource : `318582.json`
- Paramètre de requête : `fields=email,firstName,lastName`

L’URL de base contient l’identifiant du compte (également appelé identifiant Munchkin) et est donc unique pour chaque abonnement Marketo. Pour trouver l’URL de base, connectez-vous à Marketo et accédez au menu **[!UICONTROL Admin]** > **[!UICONTROL Intégration]** > **[!UICONTROL Services Web]**. Il est étiqueté comme « Point d’entrée » sous la section « API REST », comme illustré dans les captures d’écran suivantes.

![Point d’entrée de l’URL de base des services web](assets/rest-api-base-url-web-services.png)

Une fois que vous avez trouvé l’URL de base, copiez-la et incluez-la dans les URL que vous utilisez lors de l’appel de l’une des API REST.
