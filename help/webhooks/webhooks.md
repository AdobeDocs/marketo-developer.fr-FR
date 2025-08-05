---
title: Webhooks
feature: Webhooks
description: Présentation des Webhooks
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Webhooks

Marketo permet d’utiliser des Webhooks pour communiquer avec des services web tiers. Les Webhooks prennent en charge l’utilisation des verbes HTTP GET ou POST pour transmettre ou récupérer des données à partir d’une URL spécifique. Pour obtenir des instructions détaillées sur la création de Webhooks dans l’application et sur la manière de les ajouter aux campagnes intelligentes, reportez-vous aux articles suivants :

- [Créer un Webhook](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Appeler Webhook](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Utilisation d’un Webhook dans une campagne dynamique](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Chaque webhook possède les propriétés suivantes :

- [!UICONTROL URL] - Saisissez l’URL que vous utilisez pour envoyer votre demande au service web.
- [!UICONTROL Type de requête] - Méthode HTTP.
- [!UICONTROL Modèle de payload] - Si vous souhaitez transmettre des informations dans le corps du POST, saisissez le modèle. Utilisez n’importe quel format de données prenant en charge HTTP POST, y compris XML, JSON ou SOAP. Le format de sérialisation doit autoriser les guillemets doubles autour des chaînes. Pour insérer un jeton dans votre modèle, cliquez sur **[!UICONTROL Insérer un jeton]**.  Les jetons de type chaîne sont automatiquement placés entre guillemets doubles.
- [!UICONTROL Encodage du jeton de requête] - Si les valeurs de jeton incluent des caractères spéciaux (par exemple une esperluette, «&amp; »), indiquez le format de votre requête (JSON ou Formulaire/Url). Le codage correct doit être sélectionné pour le corps afin de garantir que le Webhook communique correctement avec le service web.
- [!UICONTROL &#x200B; Type de réponse &#x200B;] - Sélectionnez le format de la réponse que vous recevez du service (JSON ou XML). Le type de réponse correct doit être sélectionné pour mapper les propriétés de la réponse aux champs de prospect dans Marketo
- [!UICONTROL En-têtes personnalisés] - Accessible via [!UICONTROL Actions Webhooks] -> [!UICONTROL Définir un en-tête personnalisé], ce menu permet d’ajouter un nombre illimité de paires clé-valeur personnalisées en tant qu’en-têtes HTTP.

Les données peuvent être réécrites dans les leads à partir des réponses de service web à l’aide de [Mappages de réponse](response-mappings.md)

## Jetons

Tous les champs sortants d’un Webhook (URL, modèle et en-têtes personnalisés) renseignent le contenu des jetons dans le même contexte de l’étape de flux. Cela signifie que les jetons de lead et de système sont toujours disponibles, tandis que les jetons de déclencheur, de campagne et de programme sont disponibles dans leurs portées respectives. Voir les articles relatifs aux jetons :

- [Présentation des jetons](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glossaire des jetons système](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [ Jetons pour les moments significatifs ](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Cela se produit généralement lorsqu’un programme ou une campagne est explicitement mappé à une ressource tierce. Un identifiant peut être défini au niveau du programme sous la forme d’un `My Token`, puis transmis à la requête Webhook sous la forme d’un jeton.

## Personnaliser les titres 

Les Webhooks permettent d’utiliser un nombre illimité de champs d’en-tête personnalisé à envoyer avec la requête sortante. Ils peuvent être ajoutés via **[!UICONTROL Actions Webhooks]** > **[!UICONTROL Définir un en-tête personnalisé]**. Chaque en-tête est enregistré en tant que simple paire clé-valeur. Des jetons peuvent être utilisés dans cette zone.

![En-têtes personnalisés](assets/custom-headers.png)

## Conseils

- L’étape de flux Appeler le Webhook n’est valide que dans les campagnes Trigger.
- Les mises à jour via les mappages de réponse ne se produisent que si le service web répond avec un code de réponse HTTP 2xx. Les autres types de codes n’entraînent pas de mises à jour de l’enregistrement.
- Vous pouvez utiliser les services web pour effectuer un enrichissement des données personnalisé, une validation ou une normalisation à partir de services internes ou externes.
- Le temps d’exécution d’un webhook est à la merci du temps de réponse du service en cours d’utilisation, ce qui peut entraîner de longs délais d’exécution des campagnes. Même si l’exécution d’un service ne prend que 50 ms, cela représente 1,5 heure lorsqu’il est exécuté 100 000 fois.
- Marketo attend jusqu’à 30 secondes pour un appel de service donné avant de mettre fin à l’appel (délai d’expiration).
- Les caractères incorporés dans le champ URL sont transmis tels qu&#39;écrits ; par exemple, &#39;&amp;&#39; est envoyé comme &#39;&amp;&#39;, &#39;%26&#39; est envoyé comme &#39;%26&#39;
   - Si un caractère doit être codé en pourcentage à la réception par le serveur destinataire, il doit être transmis explicitement en tant que chaîne représentant ce caractère
