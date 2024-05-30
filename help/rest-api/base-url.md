---
title: "URL de base"
feature: REST API
description: "Décrit comment accéder à des URL pour Marketo."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---


# URL de base

La variable [Référence du point d’entrée](endpoint-reference.md) La documentation de chaque appel API affiche la méthode, le chemin, la ressource et les paramètres REST qui doivent être ajoutés à l’URL de base pour former une requête.

Voici un exemple d’URL REST bien formée :

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

qui se compose des parties suivantes :

- URL de base : `https://284-RPR-133.mktorest.com/rest`
- Chemin : `/v1/lead/`
- Ressource : `318582.json`
- Paramètre de requête : `fields=email,firstName,lastName`

L’URL de base contient l’identifiant du compte (alias Munchkin id) et est donc unique pour chaque abonnement Marketo. L’URL de base se trouve en se connectant à Marketo et en accédant à la variable **[!UICONTROL Administration]** > **[!UICONTROL Intégration]** > **[!UICONTROL Services web]** . Il est intitulé &quot;Point de terminaison&quot; sous la section &quot;API REST&quot;, comme illustré dans les captures d’écran suivantes.

![Point d’entrée de l’URL de base des services Web](assets/rest-api-base-url-web-services.png)

Une fois que vous avez trouvé l’URL de base, copiez-la et incluez-la dans les URL que vous utilisez lors de l’appel de l’une des API REST.
