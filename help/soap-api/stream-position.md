---
title: Position du flux
feature: SOAP
description: Explique la position du flux pour la pagination des données à séquence temporelle dans SOAP, les formats simples et complexes, et leur utilisation dans getLeadChanges, getLeadActivity, etc
exl-id: c3a3fc1e-086b-4822-b2c7-2a7959db557c
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Position du flux

Les éléments de position de flux contiennent une référence de position pour un ou plusieurs flux logiques de données à séquence temporelle. La référence de position peut être une spécification externe approximative telle qu’un horodatage ou une spécification interne opaque de position renvoyée par un appel API précédent. Les positions de flux peuvent être définies sous la forme d’un type complexe à plusieurs éléments ou d’une chaîne.

La position du flux est utilisée pour récupérer les données par lots et permet à l’appelant de paginer à travers le résultat. La position du flux transmise dans une requête API est la valeur de la position du flux renvoyée dans la réponse précédente. La modification de la position du flux renvoyée par l’appel API précédent n’est pas recommandée et peut entraîner un comportement d’API inattendu.

## API prenant en charge la position des flux

- [getCustomObjects](getcustomobjects.md)
- [getLeadChanges](getleadchanges.md)
- [getLeadActivity](getleadactivity.md)
- [getMObjects](getmobjects.md)
- [getMultipleLeads](getmultipleleads.md)

## Position du flux simple

```
<streamPosition>8UJZetaMb1V6uUZl+L7DcPP2jG+PMmtpF</streamPosition>
```

## Position Complexe Du Flux

```xml
<startPosition>
  <latestCreatedAt  />
  <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
  <activityCreatedAt  />
  <offset>ID:1086173</offset>
</startPosition>
```
