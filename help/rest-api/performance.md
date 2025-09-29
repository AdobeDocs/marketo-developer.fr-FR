---
title: Performance
feature: REST API
description: Boostez les performances de l’API REST Marketo avec la compression HTTP. Activez gzip pour réduire la bande passante ; les API en bloc ne sont pas prises en charge et moins de 1 024 octets ne sont pas compressés.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 1%

---

# Performance

Cette page contient une liste de rubriques relatives aux performances que vous pouvez utiliser pour améliorer les performances de votre intégration.

## Compression HTTP

L’API REST Marketo prend en charge la compression HTTP des corps de réponse à l’aide des normes définies par la spécification HTTP 1.1. L’activation de la compression est recommandée, car elle réduit l’utilisation de la bande passante et le temps passé à récupérer les données.

>[!NOTE]
>
>Les payloads inférieures à 1 024 octets ne sont pas compressées et les API en bloc ne prennent pas en charge la compression.

Pour activer la compression, incluez l’en-tête HTTP suivant dans la requête :

```html
Accept-Encoding: gzip
```

L’API REST Marketo compresse le corps de la réponse et inclut cet en-tête :

```html
Content-Encoding: gzip
```

Voici un exemple utilisant Curl pour appeler le point d’entrée [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) afin de récupérer 5 prospects :

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
