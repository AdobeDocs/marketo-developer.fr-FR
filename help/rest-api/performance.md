---
title: Performance
feature: REST API
description: Boostez les performances de l’API REST Marketo avec la compression HTTP. Activez gzip pour réduire la bande passante ; les API en bloc ne sont pas prises en charge et moins de 1 024 octets ne sont pas compressés.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
TQID: https://experienceleague.adobe.com/foJCTd890HZtL-UzWx2cjRXwTxqgW56A79sB7FPEWis
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 129
ht-degree: 1%

---

# Performance

Utilisez les options de performances de cette page pour améliorer l’efficacité de votre intégration.

## Compression HTTP

L’API REST Marketo prend en charge la compression du corps de la réponse HTTP telle que définie par la spécification HTTP 1.1. Activez la compression pour réduire l’utilisation de la bande passante et le temps de récupération des données.

>[!NOTE]
>
>Les payloads inférieures à 1 024 octets ne sont pas compressées et les API en bloc ne prennent pas en charge la compression.

Pour activer la compression, incluez l’en-tête HTTP suivant dans la requête :

```html
Accept-Encoding: gzip
```

L’API REST Marketo compresse le corps de la réponse et inclut l’en-tête suivant :

```html
Content-Encoding: gzip
```

L’exemple cURL suivant appelle le point d’entrée [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByFilterUsingGET) pour récupérer cinq prospects :

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
