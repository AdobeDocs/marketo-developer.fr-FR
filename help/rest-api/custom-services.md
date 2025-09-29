---
title: Services personnalisés
feature: REST API
description: Créez des services personnalisés Marketo, définissez des rôles et des autorisations API uniquement, obtenez l’ID client et le secret client dans LaunchPoint, puis obtenez des jetons d’accès.
exl-id: 38b05c4c-4404-4c30-a7cb-d31b28a3a72e
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 6%

---

# Services personnalisés

Un service personnalisé fournit des informations d’identification pour l’authentification auprès de Marketo. Les informations d’identification sont nécessaires pour obtenir un jeton d’accès du Marketo [Service d’identités](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Chaque service personnalisé est limité à un seul utilisateur API uniquement de qui il tire ses autorisations.

## Rôles

La première étape de la création d’un service personnalisé consiste à créer un rôle que vous pouvez appliquer à l’utilisateur API uniquement approprié. Cette opération s’effectue à partir du menu **[!UICONTROL Admin]** > **[!UICONTROL Utilisateurs et rôles]** > **[!UICONTROL Rôles]**.

Les rôles sont des conteneurs d’autorisations individuelles qui permettent ou limitent l’accès à certaines fonctions. Dans les abonnements pour lesquels les espaces de travail et les partitions sont activés, les autorisations sont accordées par espace de travail. Si un utilisateur dispose d’une autorisation dans un espace de travail, mais pas dans un autre, il ne pourra effectuer que les actions autorisées dans cet espace de travail. Pour créer un rôle, cliquez sur le bouton Nouveau rôle .

![Utilisateurs et rôles](assets/admin-users-and-roles-roles.png)

Veillez à donner à votre rôle un nom explicite. Les utilisateurs disposant uniquement d’API disposent d’un ensemble spécifique d’autorisations qui sont distinctes des autorisations utilisateur normales. Les autorisations d’API existent dans leur propre hiérarchie sous l’arborescence « API d’accès ».

![Nouvelles autorisations de rôle](assets/new-role-access-api-permissions.png)

### Autorisations des rôles

Seules les autorisations du groupe « API d’accès » sont appliquées aux utilisateurs de l’API. En d’autres termes, l’octroi de toutes les autorisations d’administrateur n’accordera aucune autorisation d’API à un utilisateur.

Lors de la création d’un rôle, réfléchissez soigneusement aux actions que vous devez autoriser l’application à effectuer. Attribuez uniquement l’ensemble minimal d’autorisations requises pour effectuer ces actions. L’autorisation d’un ensemble d’autorisations inutilement permissif peut permettre aux intégrations d’effectuer des actions indésirables dans votre abonnement. Vous pouvez utiliser l’outil [autorisations](endpoint-reference.md) pour déterminer votre jeu minimal d’autorisations. Consultez la liste complète des [autorisations](#permission_list).

## Utilisateurs et utilisatrices

Après avoir créé un rôle, vous devez créer un utilisateur « API uniquement ». Les utilisateurs uniquement API sont un type spécial d’utilisateur dans Marketo, car ils sont administrés par d’autres utilisateurs et ne peuvent pas être utilisés pour se connecter à Marketo. Les utilisateurs d’API uniquement peuvent :

- Créer des services personnalisés
- Étendue des autorisations pour ces services
- Accès aux API REST

>[!MORELIKETHIS]
>
>Pour créer un utilisateur API uniquement, accédez au menu **[!UICONTROL Admin]** > **[!UICONTROL Utilisateurs et rôles]** > **[!UICONTROL Utilisateurs]** et cliquez sur [!UICONTROL Inviter un nouvel utilisateur].

![Nouvelles informations utilisateur](assets/new-user-info.png)

Donnez à votre utilisateur un nom et une adresse e-mail descriptifs (ils ne doivent pas être valides), en fonction du service et de l’application pour lesquels ils seront utilisés. Renseignez les champs obligatoires dans le menu de la boîte de dialogue, cliquez sur la case à cocher « API uniquement », puis attribuez l’un de vos rôles d’API à l’utilisateur. Cette action affecte les autorisations définies pour ce rôle à l’utilisateur.

![Nouvelles autorisations d’utilisateurs](assets/new-user-permissions.png)

Enfin, cliquez sur « Envoyer » pour créer l’utilisateur API uniquement.

Lors de la mise en service d’une nouvelle application avec des informations d’identification, envisagez sérieusement de créer un nouvel utilisateur pour le service même s’il dispose du même jeu d’autorisations qu’une autre intégration existante. Les statistiques et les erreurs d’utilisation des appels API sont suivies par utilisateur. Ainsi, la mise en service d’un utilisateur pour chaque application peut vous aider à isoler les problèmes d’utilisation et d’erreur pour des applications spécifiques. Cela s’avère pratique si vous rencontrez des problèmes pour atteindre les limites de vos appels API quotidiens ou si vous rencontrez des erreurs résultant d’appels API effectués par les intégrations.

## Services personnalisés

Les services personnalisés fournissent les informations d’identification réelles, l’ID client et le secret client, nécessaires pour effectuer l’authentification avec une instance Marketo. Pour en configurer un, accédez au menu **[!UICONTROL Admin]** > **[!UICONTROL Intégrations]** > **[!UICONTROL LaunchPoint]** et sélectionnez **[!UICONTROL Nouveau service]**.

Donnez un nom explicite à votre service et, dans la liste « Service », sélectionnez « Personnalisé ». Donnez à votre service une description détaillée et sélectionnez un utilisateur approprié dans la liste Utilisateur API uniquement , puis cliquez sur [!UICONTROL Créer].

![Nouveau service personnalisé](assets/admin-launchpoint-new-service.png)

Vous ajoutez ainsi un nouveau service à votre liste de services LaunchPoint et l’option « Afficher les détails ». Cliquez sur « Afficher les détails » et vous recevez l’ID client et le secret client requis pour l’authentification, l’utilisateur propriétaire et une option permettant d’obtenir un jeton à des fins de test à court terme. Le jeton obtenu à partir de cette boîte de dialogue a la même durée de vie que les jetons obtenus normalement à partir du [Service d’identités](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET) et est valide pendant 3 600 secondes à compter de la création.

![Obtenir un jeton](assets/get-token.png)

## Espaces de travail et partitions

Dans les abonnements aux espaces de travail et aux partitions, la possibilité d’accéder à un enregistrement ou à une ressource donnée est accordée en fonction des autorisations dont dispose le rôle d’un utilisateur dans un espace de travail donné. Chaque espace de travail a accès à une ou plusieurs partitions dans le menu Espaces de travail et partitions, et un prospect appartient à une seule partition. Si l’utilisateur API uniquement a accès à la lecture ou à l’écriture des enregistrements de prospect dans un espace de travail, il peut accéder à tous les enregistrements des partitions auxquelles cet espace de travail a accès.

Assets appartient aux espaces de travail. La possibilité de lire ou d’écrire une ressource est donc déterminée par le fait que l’utilisateur ou l’utilisatrice dispose ou non d’un rôle dans l’espace de travail approprié qui dispose de l’autorisation de lire ou d’écrire ce type d’enregistrement de ressource dans l’espace de travail.

## Liste des autorisations

Vous trouverez ci-dessous une liste de toutes les autorisations disponibles pour les utilisateurs disposant uniquement d’API et ce qu’ils permettent à un utilisateur disposant de cette autorisation de faire.

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
