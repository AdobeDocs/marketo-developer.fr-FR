---
title: Webhooks
feature: Webhooks
description: Présentation des webhooks
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Webhooks

Marketo permet l’utilisation de webhooks pour communiquer avec des services web tiers. Les webhooks prennent en charge l’utilisation de verbes HTTP GET ou POST pour pousser ou récupérer des données à partir d’une URL spécifique. Pour obtenir des instructions détaillées sur la création intégrée de webhooks et sur la manière de les ajouter aux campagnes dynamiques, reportez-vous aux articles suivants :

- [Créer un webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Appeler Webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Utilisation d’un webhook dans une campagne dynamique](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Chaque webhook individuel possède les propriétés suivantes :

- [!UICONTROL URL] - Entrez l’URL que vous utilisez pour envoyer votre demande au service Web.
- [!UICONTROL Type de requête] - Méthode HTTP.
- [!UICONTROL Modèle de payload] - Si vous souhaitez transmettre des informations dans le corps du POST, saisissez le modèle. Utilisez n’importe quel format de données prenant en charge le POST HTTP, y compris XML, JSON ou SOAP. Le format de sérialisation doit permettre des guillemets doubles autour des chaînes. Pour insérer un jeton dans votre modèle, cliquez sur **[!UICONTROL Insérer un jeton]**.  Les jetons de type chaîne sont automatiquement placés entre guillemets doubles.
- [!UICONTROL Encodage du jeton de demande] - Si les valeurs du jeton incluent des caractères spéciaux (tels qu’une esperluette, &#39;&amp;&#39;), indiquez le format de votre requête (JSON ou Formulaire/Url). Le codage correct doit être sélectionné pour le corps afin de garantir que le webhook communique correctement avec le service web.
- [!UICONTROL Type de réponse] - Sélectionnez le format de la réponse que vous recevez du service (JSON ou XML). Le type de réponse correct doit être sélectionné pour mapper les propriétés de la réponse sur les champs de piste dans Marketo.
- [!UICONTROL En-têtes personnalisés] - Accessibles par le biais des [!UICONTROL Actions Webhooks] -> [!UICONTROL Définir un en-tête personnalisé], ce menu permet d’ajouter un nombre indéfini de paires clé-valeur personnalisées en tant qu’en-têtes HTTP.

Les données peuvent être réécrites dans des pistes à partir de réponses de service Web à l’aide de [mappages de réponse](response-mappings.md)

## Jetons

Tous les champs sortants d’un webhook (URL, modèle et en-têtes personnalisés) renseignent le contenu des jetons dans le même contexte que l’étape de flux. Cela signifie que les jetons de piste et système sont toujours disponibles, tandis que les jetons Déclencheur, Campagne et Programme sont disponibles dans leur portée respective. Voir les articles relatifs aux jetons :

- [Présentation des jetons](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glossaire des jetons système](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Jetons pour les moments intéressants](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

C’est le cas le plus courant lorsqu’un programme ou une campagne est explicitement mappé à une ressource tierce. Un ID peut être défini au niveau du programme sous la forme `My Token`, puis transmis dans la requête Webhook sous la forme d’un jeton.

## Personnaliser les titres 

Les webhooks permettent d’utiliser un nombre indéfini de champs d’en-tête personnalisé à envoyer avec la requête sortante. Ils peuvent être ajoutés par l’intermédiaire des **[!UICONTROL actions Webhooks]** > **[!UICONTROL Définir l’en-tête personnalisé]**. Chaque en-tête est enregistré comme une simple paire clé-valeur. Les jetons peuvent être utilisés dans cette zone.

![En-têtes personnalisés](assets/custom-headers.png)

## Conseils

- L’étape Appeler le flux Webhook n’est valide que dans les campagnes Trigger.
- Les mises à jour via les mappages de réponse ne se produisent que si le service Web répond avec un code de réponse HTTP 2xx. D’autres types de codes n’entraîneront pas de mises à jour de l’enregistrement.
- Vous pouvez utiliser des services web pour effectuer un enrichissement, une validation ou une normalisation de données personnalisés à partir de services internes ou externes.
- Le temps d’exécution Webhook est à la merci du temps de réponse du service utilisé et peut entraîner de longs délais d’exécution de campagne. Même si l’exécution d’un service ne prend que 50 ms, c’est-à-dire 1,5 heure lorsqu’il est exécuté 100 000 fois.
- Marketo attend jusqu’à 30 secondes un appel de service donné avant d’interrompre l’appel (délai d’expiration de l’appel).
- Les caractères incorporés dans le champ URL sont transmis comme écrits ; par exemple, &quot;&amp;&quot; est envoyé comme &quot;&amp;&quot;, &quot;%26&quot; est envoyé comme &quot;%26&quot;
   - Si un caractère doit être codé en pourcentage lors de sa réception par le serveur destinataire, il doit être transmis explicitement comme chaîne représentant ce caractère.
