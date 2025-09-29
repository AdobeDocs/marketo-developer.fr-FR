---
title: FAQ sur SOAP
feature: SOAP
description: Découvrez comment répertorier les programmes avec getMObjects, optimiser getMultipleLeads, créer des opportunités et envoyer ou planifier des e-mails personnalisés via l’API Marketo SOAP.
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 0%

---

# FAQ sur SOAP

**Q :** Comment puis-je obtenir une liste de tous les programmes dans Marketo avec leurs métadonnées ?

**A:** Pour récupérer une liste de tous les programmes, vous pouvez utiliser [getMObjects](./getmobjects.md) en transmettant le type égal à « Program » et en définissant includeDetails sur true.

**Q:** Existe-t-il un moyen d’accélérer les performances de getMultipleLeads ?

**A :** Il existe quelques options pour accélérer les performances de l’appel getMultipleLeads. La première consiste à réduire la taille du lot que vous demandez pour chaque appel. 200 est la taille de lot recommandée. La deuxième option consiste à spécifier les champs qui vous intéressent à l’aide du filtre includeAttributes . Cela permet d’accélérer la requête et de réduire la payload de la réponse que vous recevez. L’approche finale consiste à utiliser LastUpdateAtSelector et à spécifier les propriétés oldUpdatedAt et latestUpdatedAt. Vous pouvez spécifier différentes périodes, puis traiter plusieurs requêtes simultanément. Si vous utilisez l’approche par threads, assurez-vous que votre client SOAP/WSDL prend en charge les [connexions persistantes](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**Q :** Comment créer des opportunités via l’API SOAP si elles ne sont pas intégrées à un CRM comme Salesforce ou Microsoft Dynamics ?

**A :** vous pouvez créer des opportunités à l’aide de l’API SOAP en écrivant l’appel [syncMObjects](syncmobjects.md) sur les types OpportunityPersonRole et Opportunity [MObject](marketo-objects.md).

**Q:** Puis-je envoyer un e-mail par programmation à partir de Marketo ? Si tel est le cas, comment envoyer du contenu personnalisé pour chaque destinataire d’e-mail ?

**R:** Tout à fait. Vous pouvez demander l’envoi d’un e-mail à partir de Marketo à l’aide de l’une des API SOAP [requestCampaign](requestcampaign.md) ou d’une combinaison des API [importToList](importtolist.md) et [scheduleCampaign](schedulecampaign.md). Pour envoyer immédiatement un e-mail à une ou plusieurs personnes, utilisez [requestCampaign](requestcampaign.md). Si vous souhaitez planifier l’envoi d’un e-mail à une date et une heure spécifiées, vous devez utiliser [importToList](importtolist.md) pour spécifier les destinataires de l’e-mail, et [scheduleCampaign](schedulecampaign.md) pour spécifier le moment où ces destinataires recevront cet e-mail.

Si vous souhaitez personnaliser le contenu pour chaque destinataire d’e-mail, vous pouvez le faire en remplaçant les valeurs des [ jetons de programme ](../rest-api/tokens.md) définies dans le modèle d’e-mail.
