---
title: API SOAP
feature: SOAP
description: Marketo SOAP - Aperçu
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 7a3df193e47e7ee363c156bf24f0941879c6bd13
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 2%

---

# API SOAP

L’API SOAP est en train d’être abandonnée et ne sera plus disponible après le 31 octobre 2025.  Tout nouveau développement doit être effectué avec l’API Marketo [REST](https://developer.adobe.com/marketo-apis/), et les services existants doivent être migrés à cette date afin d’éviter des interruptions de service.

## SOAP WSDL

Pour récupérer le document WSDL SOAP, obtenez votre point de terminaison de l’API SOAP dans le menu **[!UICONTROL Admin]** > **[!UICONTROL Intégration]** > **[!UICONTROL Services Web]** .

![SOAP Point d’entrée](assets/endpoint-soap.png)

Votre URL WSDL est :

`<SOAP API Endpoint> + ?WSDL`

N’utilisez pas le point de fin défini dans le fichier WSDL. Chaque instance Marketo possède un point de terminaison unique auquel effectuer des appels.

## Limites

- **Quota quotidien :** La plupart des abonnements reçoivent 10 000 appels d’API par jour (qui se réinitialise tous les jours à 00h00 heure du Pacifique). Vous pouvez augmenter votre quota quotidien par l’intermédiaire de votre gestionnaire de compte.
- **Limite de débit :** accès API par instance limité à 100 appels par 20 secondes.
- **Limite de simultanéité :**  Nombre maximum de dix appels API simultanés.

Nous vous recommandons de ne pas dépasser 300 lots. Les tailles plus volumineuses ne sont pas prises en charge et peuvent entraîner des délais d’expiration et, dans les cas extrêmes, un ralentissement.

## Paramètres de l’API SOAP dans Marketo

1. Accédez à la section **[!UICONTROL Admin]** et cliquez sur **[!UICONTROL Services web]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Définissez une [!UICONTROL clé de chiffrement] appropriée, cliquez sur **[!UICONTROL Enregistrer les modifications]** et utilisez l’API SOAP [!UICONTROL Endpoint], [!UICONTROL ID utilisateur] et les valeurs [!UICONTROL clé de chiffrement] pour générer la [signature d’authentification](authentication-signature.md) correcte pour chaque appel d’API SOAP.

![admin-web-services3](assets/admin-web-services3.png)
