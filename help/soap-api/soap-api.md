---
title: API SOAP
feature: SOAP
description: Marketo SOAP - Aperçu
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 1%

---

# API SOAP

L’API SOAP n’est plus en cours de développement. Les appels fonctionnent toujours, mais notre développement est axé sur [REST](https://developer.adobe.com/marketo-apis/) à venir.

L’API Marketo SOAP permet la création, la récupération et la suppression d’entités et de données stockées dans Marketo. Vous pouvez trouver la variable [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) sur GitHub. Il y a également [bibliothèques clientes](https://github.com/Marketo/Community-Supported-Client-Libraries) pour vous faire gagner du temps.

Dernière version de l’API : 3_1

## SOAP WSDL

Pour récupérer le document WSDL SOAP, procurez-vous votre point de terminaison d’API SOAP auprès de votre **[!UICONTROL Administration]** > **[!UICONTROL Intégration]** > **[!UICONTROL Services web]** .

![Point de terminaison SOAP](assets/endpoint-soap.png)

Votre URL WSDL est :

`<SOAP API Endpoint> + ?WSDL`

N’utilisez pas le point de fin défini dans le fichier WSDL. Chaque instance Marketo possède un point de terminaison unique auquel effectuer des appels.

## Limites

- **Quota quotidien :** La plupart des abonnements se voient attribuer 10 000 appels d’API par jour (qui se réinitialise tous les jours à 00h00 du matin de l’après-midi). Vous pouvez augmenter votre quota quotidien par l’intermédiaire de votre gestionnaire de compte.
- **Limite de taux :** Accès à l’API par instance limité à 100 appels par 20 secondes.
- **Limite de simultanéité :**  Nombre maximum de dix appels API simultanés.

Nous vous recommandons de ne pas dépasser 300 lots. Les tailles plus volumineuses ne sont pas prises en charge et peuvent entraîner des délais d’expiration et, dans les cas extrêmes, un ralentissement.

## Paramètres de l’API SOAP dans Marketo

1. Accédez au **[!UICONTROL Administration]** et cliquez sur **[!UICONTROL Services web]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Définissez les [!UICONTROL Clé de chiffrement], cliquez sur **[!UICONTROL Enregistrer les modifications]** et utiliser l’API SOAP [!UICONTROL Point d’entrée], [!UICONTROL Identifiant utilisateur], et [!UICONTROL Clé de chiffrement] pour générer les valeurs correctes [signature d’authentification](authentication-signature.md) pour chaque appel de l’API SOAP.

![admin-web-services3](assets/admin-web-services3.png)
