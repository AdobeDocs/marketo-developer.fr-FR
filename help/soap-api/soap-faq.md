---
title: FAQ sur les SOAP
feature: SOAP
description: FAQ sur les SOAP
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# FAQ sur les SOAP

**Q :** Comment puis-je obtenir une liste de tous les programmes dans Marketo avec leurs métadonnées ?

**A :** Pour récupérer une liste de tous les programmes, vous pouvez utiliser [getMObjects](./getmobjects.md) en transmettant le type égal à &quot;Program&quot; et en définissant includeDetails sur true.

**Q :** Existe-t-il un moyen d’accélérer les performances de getMultipleLeads ?

**A :** Certaines options permettent d’accélérer les performances de l’appel getMultipleLeads. La première consiste à réduire la taille du lot que vous demandez pour chaque appel. 200 est la taille de lot recommandée. La deuxième option consiste à spécifier les champs qui vous intéressent à l’aide du filtre includeAttributes . Cela accélère la requête et réduit la charge utile de la réponse que vous recevez. L’approche finale consiste à utiliser le LastUpdateAtSelector et à spécifier les méthodes oldUpdatedAt et latestUpdatedAt. Vous pouvez spécifier différentes plages de dates, puis filtrer plusieurs requêtes simultanément. Si vous utilisez l’approche thread, assurez-vous que votre client SOAP/WSDL prend en charge les [connexions persistantes](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**Q :** Comment puis-je créer des opportunités via l’API SOAP si je ne suis pas intégré à un CRM comme SalesForce ou Microsoft Dynamics ?

**A :** Vous pouvez créer des opportunités à l’aide de l’API SOAP en utilisant les types d’appels [syncMObjects](syncmobjects.md) à OpportunityPersonRole et Opportunity [MObject](marketo-objects.md).

**Q :** Puis-je envoyer un courrier électronique par programmation à partir de Marketo ? Si tel est le cas, comment envoyer du contenu personnalisé pour chaque destinataire d&#39;email ?

**A :** Absolument. Vous pouvez demander l’envoi d’un email à partir de Marketo à l’aide de [requestCampaign](requestcampaign.md) ou d’une combinaison d’API [importToList](importtolist.md) et [scheduleCampaign](schedulecampaign.md) SOAP. Pour envoyer immédiatement un email à une ou plusieurs personnes, utilisez [requestCampaign](requestcampaign.md). Si vous souhaitez planifier l’envoi d’un email à une date et une heure spécifiées, utilisez [importToList](importtolist.md) pour spécifier les destinataires de l’email, et [scheduleCampaign](schedulecampaign.md) pour spécifier le moment où ces destinataires seront envoyés à cet email.

Si vous souhaitez personnaliser le contenu de chaque destinataire d’email, vous pouvez le faire en remplaçant les valeurs des [jetons de programme](../rest-api/tokens.md) qui sont définies dans le modèle de courrier électronique.
