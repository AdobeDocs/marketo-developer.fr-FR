---
title: Référence du point d’entrée
feature: REST API
description: Références de point d’entrée de l’API Marketo
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 1%

---

# Référence du point d’entrée

- [Ressource](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identité](https://developer.adobe.com/marketo-apis/api/identity/)
- [Base de données de piste](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Gestion utilisateur](https://developer.adobe.com/marketo-apis/api/user/)

## Référence du point d’entrée

Marketo utilise Swagger pour fournir une définition formelle de l’interface publique de ses API REST. Swagger fournit un modèle de définition riche pour les structures d’URL, les modèles de requête et les modèles de réponse et dispose d’un écosystème développé d’outils à utiliser avec l’interaction, le test et la génération de client de l’API.

La référence de point de terminaison utilise le package JavaScript [Swagger-UI](https://swagger.io/tools/swagger-ui/) pour générer les pages de référence côté client. Chaque point de terminaison public est répertorié et donne la structure du modèle de réponse, les paramètres de requête requis et le modèle de requête si nécessaire.

## Utilisation des définitions Swagger de Marketo

La norme Swagger exige qu’un hôte soit fourni ou que l’hôte soit généré par l’hôte qui diffuse le fichier. Il est important de corriger l’hôte dans la définition avant d’utiliser les outils existants, car Marketo fournit un paramètre d’hôte vide avec la définition. L’hôte de l’API REST pour chaque instance Marketo est unique et suit le modèle suivant :

`{Munchkin ID}.mktorest.com`

## API d’actifs

Les API Marketo Asset utilisent des paramètres de style `application/x-www-url-formencoded` dans les requêtes de points de terminaison qui nécessitent une méthode de POST. Cependant, dans certains cas, comme les paramètres de dossier, la valeur du paramètre peut être un tableau ou un objet JSON. Ces paramètres sont indiqués dans la référence du point de terminaison .
