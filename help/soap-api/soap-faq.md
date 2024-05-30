---
title: "FAQ SOAP"
feature: SOAP
description: "FAQ SOAP"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# FAQ SOAP

**Q :** Comment obtenir une liste de tous les programmes dans Marketo avec leurs métadonnées ?

**A :** Pour récupérer une liste de tous les programmes, vous pouvez utiliser [getMObjects](./getmobjects.md) transmission du type sur &quot;Program&quot; et définition includeDetails sur true.

**Q :** Existe-t-il un moyen d’accélérer les performances de getMultipleLeads ?

**A :** Il existe quelques options pour accélérer les performances de l’appel getMultipleLeads. La première consiste à réduire la taille du lot que vous demandez pour chaque appel. 200 est la taille de lot recommandée. La deuxième option consiste à spécifier les champs qui vous intéressent à l’aide du filtre includeAttributes . Cela accélère la requête et réduit la charge utile de la réponse que vous recevez. L’approche finale consiste à utiliser le LastUpdateAtSelector et à spécifier les méthodes oldUpdatedAt et latestUpdatedAt. Vous pouvez spécifier différentes plages de dates, puis filtrer plusieurs requêtes simultanément. Si vous utilisez l’approche thread, assurez-vous que votre client SOAP/WSDL prend en charge [connexions persistantes](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**Q :** Comment créer des opportunités via l’API SOAP lorsqu’elle n’est pas intégrée à un CRM comme SalesForce ou Microsoft Dynamics ?

**A :** Vous pouvez créer des opportunités à l’aide de l’API SOAP à l’aide de l’ [syncMObjects](syncmobjects.md) l’écriture d’appels à OpportunityPersonRole et Opportunity [MObject](marketo-objects.md) types.

**Q :** Puis-je envoyer un courrier électronique par programmation à partir de Marketo ? Si tel est le cas, comment envoyer du contenu personnalisé pour chaque destinataire d&#39;email ?

**A :** Absolument. Vous pouvez demander l’envoi d’un courrier électronique à partir de Marketo à l’aide de l’option [requestCampaign](requestcampaign.md) ou combinaison de [importToList](importtolist.md) et [scheduleCampaign](schedulecampaign.md) API SOAP. Pour envoyer immédiatement un e-mail à une ou plusieurs personnes, utilisez [requestCampaign](requestcampaign.md). Si vous souhaitez planifier l’envoi d’un email à une date et une heure spécifiées, utilisez [importToList](importtolist.md) pour spécifier les destinataires de l&#39;email, et [scheduleCampaign](schedulecampaign.md) pour indiquer quand ces destinataires seront envoyés à cet email.

Si vous souhaitez personnaliser le contenu de chaque destinataire d&#39;email, vous pouvez le faire en remplaçant les valeurs de [jetons de programme](../rest-api/tokens.md) qui sont définis dans le modèle de courrier électronique.
