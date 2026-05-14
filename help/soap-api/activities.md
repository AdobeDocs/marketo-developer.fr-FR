---
title: Activités
feature: SOAP
description: Découvrez comment interagir avec les activités à l’aide de SOAP, récupérer les activités de lead et suivre les modifications de lead avec getLeadActivities et getLeadChanges
exl-id: fd695ab6-e7be-4ced-89c9-c4cd2d4c2ab0
TQID: https://experienceleague.adobe.com/6zUkvoDCqlRmblFDPWzLjdwITsyWxcXrJBKbLux76WI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: 79
ht-degree: 2%

---

# Activités

Les appels SOAP suivants peuvent être utilisés pour interagir avec les activités .

- [getLeadActivities](getleadactivity.md)
- [getLeadChanges](getleadchanges.md)

>[!CAUTION]
>
>À compter du 30/12/2026, les appels aux points d’entrée `Get Lead Activities` et `Get Lead Changes` qui incluent le paramètre `listId` échoueront (code d’erreur 1003) si les listes cibles contiennent 10 000 prospects ou plus. Pour éviter toute interruption de service, assurez-vous que la portée des appels est correctement définie pour éviter cette limite. Voir le [ Guide de migration ](migration.md).
