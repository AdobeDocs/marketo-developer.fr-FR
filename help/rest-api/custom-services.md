---
title: Services personnalisés
feature: REST API
description: Créez des services personnalisés Marketo, définissez des rôles et des autorisations API uniquement, obtenez l’ID client et le secret client dans LaunchPoint, puis obtenez des jetons d’accès.
exl-id: 38b05c4c-4404-4c30-a7cb-d31b28a3a72e
TQID: https://experienceleague.adobe.com/lvT-8bYucf-K5LYxb5jQ7BHc137W71SvsGg7cWJlxEs
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 890
ht-degree: 9%

---

# Services personnalisés

Un service personnalisé fournit les informations d’identification utilisées pour s’authentifier auprès de Marketo et obtenir un jeton d’accès à partir du Marketo [Service d’identités](https://developer.adobe.com/marketo-apis/api/identity#operation/identityUsingGET). Chaque service personnalisé est limité à un utilisateur API uniquement et tire ses autorisations de cet utilisateur.

## Rôles

Avant de créer un service personnalisé, créez un rôle à affecter à l’utilisateur API uniquement approprié. Accédez à **[!UICONTROL Admin]** > **[!UICONTROL Utilisateurs et rôles]** > **[!UICONTROL Rôles]**.

Les rôles contiennent des autorisations individuelles qui permettent ou limitent l’accès à des fonctions spécifiques. Dans les abonnements pour lesquels les espaces de travail et les partitions sont activés, les autorisations sont attribuées par espace de travail. Un utilisateur ne peut effectuer des actions autorisées que dans les espaces de travail où il dispose de ces autorisations.

Pour créer un rôle, sélectionnez **[!UICONTROL Nouveau rôle]**.

![Utilisateurs et rôles](assets/admin-users-and-roles-roles.png)

Attribuez un nom explicite au rôle. Les utilisateurs disposant uniquement d’API disposent d’un ensemble spécifique d’autorisations distinctes des autorisations utilisateur standard. Les autorisations d’API apparaissent dans leur propre hiérarchie sous l’arborescence « API d’accès ».

![Nouvelles autorisations de rôle](assets/new-role-access-api-permissions.png)

### Autorisations des rôles

Seules les autorisations du groupe « API d’accès » s’appliquent aux utilisateurs de l’API . L’attribution de toutes les autorisations d’administrateur n’accorde pas d’autorisations API à un utilisateur.

Lorsque vous créez un rôle, identifiez les actions que l’application doit effectuer. Attribuez uniquement les autorisations minimales requises pour ces actions. Les autorisations inutiles peuvent permettre aux intégrations d’effectuer des actions indésirables dans votre abonnement.

Utilisez l’outil [autorisations](endpoint-reference.md) pour déterminer l’ensemble minimal d’autorisations. Consultez la liste complète des [autorisations](#permission_list).

## Utilisateurs et utilisatrices

Après avoir créé un rôle, créez un utilisateur « API uniquement ». Les autres utilisateurs administrent des utilisateurs API uniquement et les utilisateurs API uniquement ne peuvent pas se connecter à Marketo. Ils peuvent :

- Créer des services personnalisés
- Étendue des autorisations pour ces services
- Accès aux API REST

>[!MORELIKETHIS]
>
>Pour créer un utilisateur API uniquement, accédez au menu **[!UICONTROL Admin]** > **[!UICONTROL Utilisateurs et rôles]** > **[!UICONTROL Utilisateurs]** et sélectionnez **[!UICONTROL Inviter un nouvel utilisateur]**.

![Nouvelles informations utilisateur](assets/new-user-info.png)

Donnez à l’utilisateur un nom explicite et une adresse e-mail basée sur le service et l’application qui utiliseront le compte. Il n’est pas nécessaire que l’adresse e-mail soit valide. Renseignez les champs obligatoires, cochez la case **[!UICONTROL API uniquement]** et attribuez l’un de vos rôles d’API à l’utilisateur. Cette action affecte le jeu d’autorisations du rôle à l’utilisateur.

![Nouvelles autorisations d’utilisateurs](assets/new-user-permissions.png)

Sélectionnez **[!UICONTROL Envoyer]** pour créer l’utilisateur API uniquement.

Lorsque vous indiquez les informations d’identification d’une nouvelle application, pensez à créer un utilisateur distinct pour le service, même si une autre intégration utilise le même jeu d’autorisations. Les statistiques et erreurs d’utilisation des appels API sont suivies par utilisateur.

Un utilisateur pour chaque application permet d’isoler l’utilisation et les problèmes liés à des applications spécifiques. Cette séparation est utile lorsque les intégrations atteignent les limites d’appels API quotidiennes ou génèrent des erreurs API.

## Services personnalisés

Les services personnalisés fournissent l’ID client et le secret client requis pour s’authentifier avec une instance Marketo. Pour configurer un service, accédez à **[!UICONTROL Admin]** > **[!UICONTROL Intégrations]** > **[!UICONTROL LaunchPoint]**, puis sélectionnez **[!UICONTROL Nouveau service]**.

Attribuez un nom explicite au service. Dans la liste « Service », sélectionnez « Personnalisé ». Saisissez une description détaillée, sélectionnez un utilisateur approprié dans la liste Utilisateur API uniquement, puis sélectionnez **[!UICONTROL Créer]**.

![Nouveau service personnalisé](assets/admin-launchpoint-new-service.png)

Le service apparaît dans la liste des services LaunchPoint avec l’option « Afficher les détails ». Sélectionnez « Afficher les détails » pour accéder à l’ID client, au secret client, à l’utilisateur propriétaire et à l’option Obtenir le jeton .

Utilisez Get Token pour les tests à court terme. Le jeton a la même durée de vie que les jetons obtenus à partir du [Service d’identités](https://developer.adobe.com/marketo-apis/api/identity#operation/identityUsingGET) et est valide pendant 3 600 secondes après sa création.

![Obtenir un jeton](assets/get-token.png)

## Espaces de travail et partitions

Dans les abonnements aux espaces de travail et aux partitions, les autorisations de rôle d’un utilisateur dans un espace de travail déterminent l’accès aux enregistrements et aux ressources. Chaque espace de travail a accès à une ou plusieurs partitions, et chaque prospect appartient à une partition.

Si un utilisateur API uniquement peut lire ou écrire des enregistrements de prospect dans un espace de travail, il peut accéder à tous les enregistrements des partitions disponibles dans cet espace de travail.

Assets appartient aux espaces de travail. Un utilisateur peut lire ou écrire une ressource lorsqu’il dispose d’un rôle avec l’autorisation requise dans l’espace de travail de la ressource.

## Liste des autorisations

Le tableau suivant répertorie les autorisations disponibles pour les utilisateurs disposant uniquement d’API et l’accès accordé par chaque autorisation.

| Autorisation de rôle | Accorde l&#39;accès à... |
| --- | --- |
| Approuver les ressources | Approbation de ressources |
| Exécuter la campagne | Demander ou planifier une campagne |
| Activité en lecture seule | Récupération des activités de lead |
| Métadonnées d’activité en lecture seule | Récupération des métadonnées de l’activité du prospect |
| Ressources en lecture seule | Récupération des détails de la ressource |
| Campagne en lecture seule | Récupérer les détails de la campagne |
| Société en lecture seule | Récupérer les détails de l’entreprise |
| Objet personnalisé en lecture seule | Récupérer les détails de l’objet personnalisé |
| Lead en lecture seule | Récupérer les détails du lead |
| Compte nommé en lecture seule | Récupérer les détails du compte nommé |
| Liste de comptes nommés en lecture seule | Récupérer les détails de la liste des comptes nommés |
| Opportunité en lecture seule | Récupérer les détails de l’opportunité |
| Commercial en lecture seule | Récupérer les détails du vendeur |
| Activité en lecture/écriture | Récupération et création des activités de prospect |
| Métadonnées d’activité en lecture/écriture | Récupération et création des métadonnées de l’activité du prospect |
| Ressources accessibles en lecture/écriture | Récupération, création et mise à jour de ressources |
| Campagne accessible en lecture/écriture | Récupération, création et mise à jour de campagnes |
| Société accessible en lecture/écriture | Récupérer, créer et mettre à jour des sociétés |
| Objet personnalisé accessible en lecture/écriture | Récupération, création et mise à jour d’objets personnalisés |
| Lead en lecture/écriture | Récupération, création et mise à jour des détails du prospect |
| Compte nommé en lecture / écriture | Récupération, création et mise à jour des comptes nommés |
| Liste de comptes nommés en lecture/écriture | Récupération, création et mise à jour des listes de comptes nommés |
| Opportunité accessible en lecture/écriture | Récupération, création et mise à jour des opportunités |
| Commercial accessible en lecture/écriture | Récupérer, créer et mettre à jour des vendeurs |
