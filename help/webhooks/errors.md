---
title: Erreurs
feature: Webhooks
description: Découvrez les codes d’erreur webhook de Marketo, pourquoi des réponses 2xx sont requises pour mettre à jour les champs de prospect et comment capturer et gérer les erreurs avec Webhook.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
TQID: 'https://experienceleague.adobe.com/N2jNA4EUMMTUFL9uJHZhOor6Tlz4-EXWciwoXrPml48'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
    internal-label: Integrations
subfeature_v2:
  - id: fc9b09fe-b844-4544-887b-e420c3b82065
    internal-label: Webhooks
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 3%
---
# Erreurs

Cette page décrit les codes de réponse d’erreur pour les webhooks Marketo et explique comment gérer les erreurs webhook.

Marketo génère les codes d&#39;erreur 1000 et 1001. Le système appelé par le webhook Marketo renvoie des codes de réponse 2xx à 5xx.

Marketo mappe les valeurs de réponse à un champ uniquement lorsque le service web renvoie un code de réponse 2xx. Si une réponse webhook est destinée à modifier les valeurs dans un enregistrement de prospect Marketo, tous les autres codes de réponse font que Marketo ignore la réponse pour les mises à jour de champ.

| Code de réponse | Description |
| --- | --- |
| 1 000 | Cela indique que l’action de flux « Appeler le Webhook » est hébergée dans une campagne par lots. Les Webhooks ne peuvent être déclenchés qu’à partir de campagnes de déclenchement. |
| 1001 | Cela indique que le service web a émis un corps de réponse vide. |

## Capturer une erreur Webhook

Utilisez le déclencheur **[!UICONTROL Webhook est appelé]** pour capturer et gérer les erreurs webhook :

![ Webhook est appelé ](assets/webhook-called.png)

* **Response** - Payload de réponse littérale reçue par la requête.
* **Type d’erreur** - Expression de motif du message d’état HTTP.

Utilisez ces valeurs pour répondre aux erreurs et exceptions prévisibles. En fonction du service intégré, vous pouvez récupérer automatiquement de certaines classes d’erreurs et créer des alertes pour les erreurs inattendues.
