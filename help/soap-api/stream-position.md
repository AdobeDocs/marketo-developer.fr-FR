---
title: Position de la diffusion
feature: SOAP
description: Présentation de la position de vapeur
exl-id: c3a3fc1e-086b-4822-b2c7-2a7959db557c
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Position de la diffusion

Les éléments de position de diffusion contiennent une référence de position pour un ou plusieurs flux logiques de données séquencées dans le temps. La référence de position peut être une spécification externe approximative comme un horodatage ou une spécification interne opaque de position renvoyée par un précédent appel API. Les positions de diffusion peuvent être définies sous la forme d’un type complexe à plusieurs éléments ou d’une chaîne.

La position du flux est utilisée pour récupérer des données par lots et permet à l’appelant de paginer le résultat. La position du flux transmise dans une requête API est la valeur de la position du flux renvoyée dans la réponse précédente. La modification de la position du flux renvoyée par l’appel API précédent n’est pas recommandée et peut entraîner un comportement API inattendu.

## API prenant en charge la position du flux

- [getCustomObjects](getcustomobjects.md)
- [getLeadChanges](getleadchanges.md)
- [getLeadActivity](getleadactivity.md)
- [getMObjects](getmobjects.md)
- [getMultipleLeads](getmultipleleads.md)

## Position simple du flux

```
<streamPosition>8UJZetaMb1V6uUZl+L7DcPP2jG+PMmtpF</streamPosition>
```

## Position complexe du flux

```xml
<startPosition>
  <latestCreatedAt  />
  <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
  <activityCreatedAt  />
  <offset>ID:1086173</offset>
</startPosition>
```
