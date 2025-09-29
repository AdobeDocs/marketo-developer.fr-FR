---
title: Erreurs
feature: Webhooks
description: Découvrez les codes d’erreur webhook de Marketo, pourquoi des réponses 2xx sont requises pour mettre à jour les champs de prospect et comment capturer et gérer les erreurs avec Webhook.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# Erreurs

Cette page répertorie les codes de réponse d’erreur pour les Webhooks dans Marketo.

Les versions 1000 et 1001 sont générées par Marketo et les versions 2xx à 5xx sont des erreurs renvoyées par le système appelé par le webhook Marketo.

Pour que Marketo puisse mapper les valeurs dans un champ, le code de réponse webhook doit être de la variété 2xx. Si l’intention du webhook est de modifier les valeurs de l’enregistrement du prospect Marketo via la réponse, alors le service Web appelé doit renvoyer la valeur 2xx, tous les autres codes de réponse entraîneront l’ignorance du webhook dans le but de mettre à jour les valeurs de l’enregistrement du prospect.

| Code de réponse | Description |
| --- | --- |
| 1 000 | Cela indique que l’action de flux « Appeler le Webhook » est hébergée dans une campagne par lots. Les Webhooks ne peuvent être déclenchés qu’à partir de campagnes de déclenchement. |
| 1001 | Cela indique que le service web a émis un corps de réponse vide. |

## Capturer une erreur Webhook

Les erreurs des Webhooks peuvent être interceptées et gérées par le déclencheur [!UICONTROL Webhook est appelé] :

![ Webhook est appelé ](assets/webhook-called.png)

* Réponse : la réponse est la payload de réponse littérale reçue par la requête.
* Type d’erreur : il correspond à l’expression de raison du message d’état HTTP.

Ils peuvent être utilisés pour gérer et réagir aux erreurs et exceptions prévisibles. Selon le service auquel vous intégrez, il peut être possible de récupérer automatiquement certaines classes d’erreurs, tandis que des alertes peuvent être créées pour informer les utilisateurs et utilisatrices d’erreurs inattendues.
