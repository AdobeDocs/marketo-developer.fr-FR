---
title: Téléchargement de définitions Swagger
feature: REST API, Programs
description: Téléchargez les fichiers de définition Swagger pour une consommation locale.
source-git-commit: 85062243d57a3fc6d15251163e926495858edf2a
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# Téléchargement de définitions Swagger

Les définitions [Swagger](https://swagger.io/) ou [OpenAPI](https://www.openapis.org/) sont des fichiers de données qui décrivent toutes les méthodes et tous les paramètres d’une API REST. Vous pouvez télécharger et utiliser ces fichiers de données localement comme référence d’API personnelle.

Pour utiliser les définitions Swagger/OpenAPI du Marketo Engage, le champ `host` de chaque document doit être mis à jour afin de correspondre au nom d’hôte de l’API REST de votre instance de Marketo Engage.

Tout d’abord, téléchargez la définition OpenAPI que vous souhaitez utiliser :

* [Ressource](assets/swagger-asset.json)
* [Lead](assets/swagger-mapi.json)
* [Identité        ](assets/swagger-identity.json)
* [Gestion des utilisateurs](assets/swagger-user.json)

Ensuite, procurez-vous votre nom d’hôte d’API REST auprès de l’administrateur Marketo. Accédez au menu _Admin_-> _Services web_ en Marketo Engage et copiez le nom d’hôte à partir du champ Point de terminaison . `hostname` est la chaîne entre le protocole `https://` et `/rest`, qui ressemble à `AAA-999-AAA.mktorest.com`.

Ouvrez votre fichier OpenAPI dans un éditeur de texte, recherchez le champ `host` dans le JSON et remplacez sa valeur par votre nom d’hôte d’API REST : `"host":"localhost:8080"` par `"host":"AAA-999-AAA.mktorest.com"`.

Une fois enregistré, vous pouvez exécuter le fichier de définition dans votre instance d’interface utilisateur Swagger ou tout autre logiciel OpenAPI.
