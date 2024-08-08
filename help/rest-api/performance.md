---
title: Performance
feature: REST API
description: Conseils de performance pour l’utilisation de l’API Marketo.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
source-git-commit: 4e64b8a801e443471f52090b7f008b11e628012d
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 1%

---

# Performance

Cette page contient une liste de rubriques relatives aux performances que vous pouvez utiliser pour améliorer les performances de votre intégration.

## Compression HTTP

L’API REST Marketo prend en charge la compression HTTP des corps de réponse à l’aide de normes définies par la spécification HTTP 1.1. L’activation de la compression est recommandée, car elle réduit l’utilisation de la bande passante et le temps passé à récupérer les données.

>[!NOTE]
>
>Les charges utiles inférieures à 1 024 octets ne sont pas compressées et les API en masse ne prennent pas en charge la compression.

Pour activer la compression, insérez l’en-tête HTTP suivant dans la requête :

```html
Accept-Encoding: gzip
```

L’API REST Marketo compresse le corps de la réponse et inclut cet en-tête :

```html
Content-Encoding: gzip
```

Voici un exemple utilisant Curl pour appeler le point de terminaison [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) pour récupérer 5 pistes :

```bash
$ curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
