---
title: Authentification
feature: REST API
description: Authentifiez les API REST Marketo avec 2 jetons OAuth 2.0, créez et utilisez des jetons d’accès, passez à l’en-tête d’autorisation, gérez l’expiration, gérez les erreurs 601 et 602.
exl-id: f89a8389-b50c-4e86-a9e4-6f6acfa98e7e
TQID: https://experienceleague.adobe.com/cIeI0m61CyIWq4HEosZ-QAsxzZb0WcrQRpCud2qysfY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 6d9408d07557d4b7426ad72d2a886220d622fb78
workflow-type: tm+mt
source-wordcount: 526
ht-degree: 0%

---

# Authentification

Les API REST Marketo utilisent OAuth 2.0 à 2 pattes pour l’authentification. Un service personnalisé fournit l’identifiant client et le secret client utilisés pour obtenir un jeton d’accès.

Chaque service personnalisé appartient à un utilisateur API uniquement. Les rôles et autorisations de l’utilisateur ou de l’utilisatrice autorisent le service à effectuer des actions spécifiques. Un jeton d’accès appartient à un seul service personnalisé et son expiration est indépendante des jetons des autres services personnalisés de l’instance.

## Création d’un jeton d’accès

Pour trouver les `Client ID` et les `Client Secret`, accédez à **[!UICONTROL Admin]** > **[!UICONTROL Intégration]** > **[!UICONTROL LaunchPoint]**. Sélectionnez le service personnalisé, puis sélectionnez **[!UICONTROL Afficher les détails]**.

![Obtenir les détails du service REST](assets/authentication-service-view-details.png)

![Informations d’identification du point de lancement](assets/admin-launchpoint-credentials.png)

Pour trouver le `Identity URL`, accédez à **[!UICONTROL Admin]** > **[!UICONTROL Intégration]** > **[!UICONTROL Services web]**. L’URL s’affiche dans la section API REST .

Créez un jeton d’accès avec une requête HTTP GET ou POST :

```http
GET <Identity URL>/oauth/token?grant_type=client_credentials&client_id=<Client Id>&client_secret=<Client Secret>
```

Si votre requête était valide, vous recevez une réponse JSON similaire à ce qui suit :

```json
{
    "access_token": "cdf01657-110d-4155-99a7-f986b2ff13a0:int",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "apis@acmeinc.com"
}
```

La réponse contient les champs suivants :

- `access_token` : jeton que vous transmettez avec les appels suivants pour l’authentification auprès de l’instance cible.
- `token_type` : méthode d’authentification OAuth.
- `expires_in` : durée de vie restante du jeton actuel, en secondes. Un nouveau jeton d’accès a une durée de vie de 3 600 secondes, soit une heure.
- `scope` : utilisateur propriétaire du service personnalisé utilisé pour l’authentification.

## Utilisation d’un jeton d’accès

Chaque appel de l’API REST doit inclure un jeton d’accès dans un en-tête HTTP.

>[!IMPORTANT]
>
>La prise en charge de l’authentification à l’aide du paramètre de requête `access_token` sera supprimée le 31 août 2026. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête [Authorization](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/authentication#using-an-access-token) dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête `Authorization` .

### Passage à l’en-tête d’autorisation

Pour remplacer le paramètre de requête `access_token` par un en-tête d’autorisation, mettez à jour la manière dont la requête envoie le jeton.

L’exemple de cURL suivant envoie la valeur `access_token` en tant que paramètre de formulaire avec l’indicateur `-F` :

```bash
curl ...  -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/apiCall.json
```

L’exemple suivant envoie la même valeur dans l’en-tête HTTP `Authorization: Bearer` avec l’indicateur `-H` :

```bash
curl ... -H 'Authorization: Bearer <Access Token>' <REST API Endpoint Base URL>/bulk/v1/apiCall.json
```

## Conseils et bonnes pratiques

Stockez le jeton d’accès et la période d’expiration à partir de la réponse Identité . La gestion de l’expiration de jeton permet d’éviter les erreurs d’authentification inattendues pendant le fonctionnement normal.

Avant d’effectuer un appel REST, vérifiez la durée de vie restante du jeton. Si le jeton a expiré, renouvelez-le en appelant le point d’entrée [Identity](https://developer.adobe.com/marketo-apis/api/identity#tag/Identity). Le renouvellement proactif empêche les échecs causés par des jetons expirés et rend la latence des appels REST plus prévisible, ce qui est important pour les applications destinées aux utilisateurs finaux.

Les erreurs d’authentification renvoient les codes suivants :

- `602` : le jeton d’accès a expiré.
- `601` : le jeton d’accès n’est pas valide.

Si le client reçoit l’un de ces codes, renouvelez le jeton en appelant le point d’entrée d’identité.

Si vous appelez le point d’entrée d’identité avant l’expiration du jeton, la réponse renvoie le même jeton et sa durée de vie restante.

Les jetons d’accès appartiennent aux services personnalisés, et non aux utilisateurs. Si les informations d’identification de deux services différents génèrent des réponses d’identité étendues au même utilisateur, leurs jetons d’accès et leurs périodes d’expiration restent indépendants.

Lorsqu’une application utilise plusieurs jeux d’informations d’identification, utilisez l’ID client comme clé pour gérer chaque jeton indépendamment.
