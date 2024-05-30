---
title: "Performances"
feature: REST API
description: "Conseils de performance pour l’utilisation de l’API Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---


# Performance

Cette page contient une liste de rubriques relatives aux performances que vous pouvez utiliser pour améliorer les performances de votre intégration.

## Compression HTTP

L’API REST Marketo prend en charge la compression HTTP des corps de réponse à l’aide de normes définies par la spécification HTTP 1.1.  L’activation de la compression est recommandée, car elle réduit l’utilisation de la bande passante et le temps passé à récupérer les données.

**Remarque :**  Les charges utiles inférieures à 1 024 octets ne seront pas compressées.

Pour activer la compression, insérez l’en-tête HTTP suivant dans la requête :

```html
Accept-Encoding: gzip
```

L’API REST Marketo compresse le corps de la réponse et inclut cet en-tête :

```html
Content-Encoding: gzip
```

Voici un exemple utilisant Curl pour appeler la fonction [Obtenir des pistes par type de filtre](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) point d’entrée pour récupérer 5 pistes :

```bash
$ curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
