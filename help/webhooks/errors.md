---
title: Erreurs
feature: Webhooks
description: Codes d’erreur pour les webhooks
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---

# Erreurs

Cette page répertorie les codes de réponse d’erreur pour les webhooks dans Marketo.

Les versions 1000 et 1001 sont générées par Marketo et les versions 2x à 5xx sont des erreurs renvoyées par le système que le webhook de Marketo appelle.

Pour que Marketo puisse remapper les valeurs dans un champ, le code de réponse du webhook doit être de la variété 2xx. Si l’objectif du webhook est de modifier les valeurs de l’enregistrement de piste Marketo par le biais de la réponse, le service Web appelé doit renvoyer 2xx, tous les autres codes de réponse entraînent l’exclusion du webhook dans le but de mettre à jour les valeurs d’enregistrement de piste.

| Code de réponse | Description |
| --- | --- |
| 1000 | Cela indique que l’action de flux &quot;Call Webhook&quot; est hébergée dans une campagne par lots. Les webhooks ne peuvent être déclenchés qu’à partir des campagnes de déclenchement. |
| 1001 | Cela indique que le service Web a émis un corps de réponse vide. |

## Correction d’une erreur Webhook

Les erreurs des webhooks peuvent être capturées et gérées par le déclencheur [!UICONTROL Webhook est appelé] :

![Webhook est appelé](assets/webhook-called.png)

* Réponse : la réponse est la charge utile de réponse littérale reçue par la requête.
* Type d’erreur : correspond à la Reason-Phrase du message d’état HTTP.

Ils peuvent être utilisés pour gérer et réagir aux erreurs et aux exceptions prévisibles. Selon le service auquel vous effectuez une intégration, il peut être possible de récupérer automatiquement certaines classes d&#39;erreurs, tandis que des alertes peuvent être créées pour avertir les utilisateurs d&#39;erreurs inattendues.
