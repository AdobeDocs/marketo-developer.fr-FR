---
title: Erreurs
feature: Webhooks
description: Découvrez les codes d’erreur webhook de Marketo, pourquoi des réponses 2xx sont requises pour mettre à jour les champs de prospect et comment capturer et gérer les erreurs avec Webhook.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
TQID: https://experienceleague.adobe.com/N2jNA4EUMMTUFL9uJHZhOor6Tlz4-EXWciwoXrPml48
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 255
ht-degree: 2%

---

# Erreurs

Cette page répertorie les codes de réponse d’erreur pour les Webhooks dans Marketo.

Les versions 1000 et 1001 sont générées par Marketo et les versions 2xx à 5xx sont des erreurs renvoyées par le système appelé par le webhook Marketo.

Pour que Marketo mappe à nouveau les valeurs dans un champ, le code de réponse webhook doit être de la variété 2xx. Si l’intention du webhook est de modifier les valeurs de l’enregistrement du prospect Marketo via la réponse, alors le service Web appelé doit renvoyer la valeur 2xx, tous les autres codes de réponse entraîneront l’ignorance du webhook dans le but de mettre à jour les valeurs de l’enregistrement du prospect.

| Code de réponse | Description |
| --- | --- |
| 1 000 | Cela indique que l’action de flux « Appeler le Webhook » est hébergée dans une campagne par lots. Les Webhooks ne peuvent être déclenchés qu’à partir de campagnes de déclenchement. |
| 1001 | Cela indique que le service web a émis un corps de réponse vide. |

## Capturer une erreur Webhook

Les erreurs des Webhooks peuvent être interceptées et gérées par le déclencheur **[!UICONTROL Webhook est appelé]** :

![&#x200B; Webhook est appelé &#x200B;](assets/webhook-called.png)

* **Response** - La réponse est le payload de réponse littérale reçu par la requête.
* **Type d’erreur** - Correspond à la phrase de motif du message d’état HTTP.

Ils peuvent être utilisés pour gérer et réagir aux erreurs et exceptions prévisibles. Selon le service auquel vous intégrez, il peut être possible de récupérer automatiquement certaines classes d’erreurs, tandis que des alertes peuvent être créées pour informer les utilisateurs et utilisatrices d’erreurs inattendues.
