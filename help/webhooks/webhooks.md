---
title: Webhooks
feature: Webhooks
description: Découvrez comment configurer des Webhooks Marketo pour appeler des services tiers, définir des modèles de payload, un codage, des mappages de réponse, des jetons, des en-têtes personnalisés et des conseils.
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
TQID: https://experienceleague.adobe.com/r-GpAqhYPKvlDtMw5l23jeJWzlSqycP65eYJPA3m9EM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: fc9b09fe-b844-4544-887b-e420c3b82065
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: bcf56d2102f2f60eac5ad3318d348fd020391e6b
workflow-type: tm+mt
source-wordcount: 613
ht-degree: 4%

---

# Webhooks

Les webhooks Marketo communiquent avec des services web tiers. Un webhook utilise le verbe HTTP GET ou POST pour envoyer des données à ou récupérer des données à partir d’une URL spécifique.

Pour obtenir des instructions sur la création d’un webhook et son ajout à une campagne dynamique, voir :

- [Créer un webhook](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Appeler le Webhook](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Utiliser un webhook dans une campagne intelligente](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Configurez chaque webhook avec les propriétés suivantes :

- **[!UICONTROL URL]** - URL vers laquelle vous envoyez la demande de service web.
- **[!UICONTROL Type de requête]** - Méthode HTTP.
- **[!UICONTROL Modèle de payload]** - Modèle pour les informations envoyées dans le corps POST. Utilisez n’importe quel format de données prenant en charge HTTP POST, y compris XML et JSON. Le format de sérialisation doit autoriser les guillemets doubles autour des chaînes. Pour insérer un jeton, sélectionnez **[!UICONTROL Insérer un jeton]**. Marketo met automatiquement les jetons de type chaîne entre guillemets doubles.
- **[!UICONTROL Encodage du jeton de requête]** - Format de requête, JSON ou Formulaire/Url, utilisé pour coder les valeurs de jeton qui incluent des caractères spéciaux tels qu’une esperluette, «&amp; ». Sélectionnez l’encodage du corps correct afin que le webhook communique correctement avec le service web.
- **[!UICONTROL Type de réponse]** - Format de réponse, JSON ou XML. Sélectionnez le type approprié pour mapper les propriétés de réponse aux champs de prospect dans Marketo.
- **[!UICONTROL En-têtes personnalisés]** - Paires clé-valeur ajoutées en tant qu’en-têtes HTTP via **[!UICONTROL Actions Webhooks]** > **[!UICONTROL Définir l’en-tête personnalisé]**. Vous pouvez ajouter un nombre illimité d’en-têtes personnalisés.

Utilisez [Mappages de réponse](response-mappings.md) pour écrire des données à partir des réponses de service web dans les prospects.

## Jetons

Tous les champs webhook sortants, y compris l’URL, le modèle et les en-têtes personnalisés, renseignent le contenu du jeton dans le même contexte que l’étape de flux.

Les jetons de lead et système sont toujours disponibles. Les jetons Trigger, Campaign et Program sont disponibles dans leurs portées respectives. Pour plus d’informations, consultez les éléments suivants :

- [Vue d’ensemble des jetons](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glossaire des jetons système](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Jetons pour les moments significatifs](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Par exemple, lorsqu’un programme ou une campagne est mappé à une ressource tierce, définissez un identifiant au niveau du programme sous la forme d’un `My Token`. Transmettez ensuite l’identifiant dans la requête webhook sous la forme d’un jeton.

## Personnaliser les titres

Les Webhooks peuvent envoyer un nombre illimité de champs d’en-tête personnalisé avec une requête sortante. Ajoutez des en-têtes via **[!UICONTROL Actions Webhooks]** > **[!UICONTROL Définir un en-tête personnalisé]**.

Chaque en-tête est une paire clé-valeur et peut contenir des jetons.

![En-têtes personnalisés](assets/custom-headers.png)

## Conseils

- Utilisez l’étape Appeler le flux Webhook uniquement dans les campagnes Trigger.
- Les mappages de réponse ne mettent à jour un enregistrement que lorsque le service web renvoie un code de réponse HTTP 2xx.
- Vous pouvez utiliser les services web pour effectuer un enrichissement des données personnalisé, une validation ou une normalisation à partir de services internes ou externes.
- Le temps d’exécution du webhook dépend du temps de réponse du service et peut entraîner de longs délais d’exécution des campagnes. Même si l’exécution d’un service ne prend que 50 ms, 100 000 exécutions prennent 1,5 heure.
- Marketo attend jusqu’à 30 secondes pour un appel de service donné avant de mettre fin à l’appel (également appelé délai d’expiration).
- Marketo transmet les caractères dans le champ URL tel qu’il a été écrit. Par exemple, &#39;&amp;&#39; est envoyé en tant que &#39;&amp;&#39;, et &#39;%26&#39; est envoyé en tant que &#39;%26&#39;.
  - Pour envoyer un caractère codé en pourcentage au serveur de destinataires, transmettez explicitement la chaîne qui représente ce caractère.
