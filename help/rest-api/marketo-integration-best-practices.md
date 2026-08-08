---
title: Bonnes pratiques d’intégration de Marketo
feature: REST API
description: Bonnes pratiques relatives aux intégrations d’API Marketo concernant les quotas, les limites de débit et de simultanéité, les traitements par lots, l’importation et l’exportation en masse, la mise en cache et la planification de la latence.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
TQID: https://experienceleague.adobe.com/Ld-rmFCwKSx-0W2-ceYICu0FQHK8BKAC1QgqtiOWDn4
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: df401a2a-327d-468c-a5e4-b7b7ccd071a0
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 866
ht-degree: 0%

---

# Bonnes pratiques d’intégration de Marketo

Concevez des intégrations autour des limites d’API partagées pour votre instance Marketo. Utilisez les taux de traitement par lots, de mise en cache et de requête prudents pour améliorer le débit et la fiabilité.

## Limites d’API

- **Quota quotidien :** la plupart des abonnements reçoivent 50 000 appels d’API par jour. Le quota est réinitialisé tous les jours à 00:00 CST. Contactez votre gestionnaire de compte pour augmenter le quota quotidien.
- **Limite de débit :** chaque instance est limitée à 100 appels API par période de 20 secondes.
- **Limite de simultanéité :** chaque instance autorise un maximum de dix appels d’API simultanés.
- **Taille du lot :** la base de données de lead prend en charge 300 enregistrements ; la requête de ressource prend en charge 200 enregistrements.
- **Taille de la payload de l’API REST :** 1 Mo.
- **Taille du fichier d’importation en bloc :** 10 Mo.
- **Tâches d’extraction en bloc :** deux en cours d’exécution et dix en file d’attente, inclus.

## Conseils rapides

- Définissez des limites d’utilisation conservatrices, car votre application partage des ressources de quota, de taux et de simultanéité avec d’autres applications.
- Utilisez les méthodes Marketo par lot et par lot si elles sont disponibles. Utilisez des appels à enregistrement unique ou à résultat unique uniquement lorsque cela est nécessaire.
- Utilisez l’[exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) pour réessayer les appels API qui échouent en raison de limites de taux ou de simultanéité.
- Évitez les appels API simultanés, sauf s’ils bénéficient à votre cas d’utilisation.

## Traitement par lots

Pour les insertions et mises à jour, regroupez les enregistrements en aussi peu de transactions que possible. Lors de la récupération d’enregistrements d’un magasin de données, agrégez-les avant l’envoi au lieu d’envoyer une seule demande pour chaque modification.

## Latence acceptable

Définissez la latence acceptable (durée maximale avant l’envoi d’un appel API) lorsque vous concevez une intégration. Ce choix détermine les méthodes Marketo et les options de configuration qui correspondent au cas d’utilisation.

Par exemple, une intégration en temps réel qui avertit un vendeur lorsqu’un utilisateur ou une utilisatrice commence une évaluation peut envoyer des lots d’un lot lorsqu’un suivi immédiat est requis. La plupart des cas d’utilisation tolèrent une latence plus élevée et fonctionnent plus efficacement en mettant les appels en file d’attente et par lots.

| Latence acceptable | Méthodes préférées | Notes |
| --- | --- | --- |
| Faible (&lt;10s) | API synchrones (par lot ou non) | Assurez-vous que votre cas d’utilisation le requiert. L’envoi d’appels immédiats et synchrones pour des cas d’utilisation à volume élevé peut rapidement consommer un quota d’API quotidien et potentiellement dépasser les limites de taux et de simultanéité. |
| Medium(10s - 60m) | API synchrones (par lots) | Pour les intégrations de données entrantes vers Marketo, il est vivement recommandé d’utiliser une file d’attente avec une limite d’âge et de taille. Lorsque l’une des limites est atteinte, videz la file d’attente et envoyez votre requête API avec les enregistrements accumulés. Il s’agit d’un compromis solide entre vitesse et efficacité. Vous pouvez ainsi vous assurer que vos requêtes se produisent à la cadence requise, tout en traitant autant d’enregistrements que le permet l’âge de la file d’attente. |
| Élevée (>60m) | Importation/exportation en bloc (si pris en charge) | Pour les intégrations de données entrantes, les enregistrements doivent être placés en file d’attente et envoyés via les API Bulk Marketo chaque fois qu’ils sont disponibles. |

## Limites quotidiennes

Chaque instance Marketo compatible avec les API dispose d’une allocation quotidienne d’au moins 10 000 appels API REST, bien que 50 000 appels ou plus soient courants. Chaque instance dispose également d’une capacité d’extraction en bloc de 500 Mo ou plus. Une capacité quotidienne supplémentaire peut être achetée dans le cadre d’un abonnement à Marketo, mais les conceptions d’application doivent tenir compte des limites d’abonnement courantes.

La capacité est partagée par tous les services d’API et les utilisateurs d’une instance. Éliminez les appels redondants et les enregistrements par lots en le réduisant au minimum.

La méthode d’importation la plus efficace pour les appels est l’API d’importation en bloc Marketo, disponible pour les [prospects/personnes](https://developer.adobe.com/marketo-apis/api/mapi#operation/importLeadUsingPOST) et [objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi#operation/importCustomObjectUsingPOST). Marketo fournit également l’extraction en bloc pour les [prospects](bulk-lead-extract.md) et [activités](bulk-activity-extract.md).

### Mise en cache

Les résultats des opérations suivantes peuvent généralement être mis en cache côté client pendant un jour ou plus, car ils changent rarement :

- Résultats des opérations de description
- [Types d’activité](https://developer.adobe.com/marketo-apis/api/mapi#operation/getAllActivityTypesUsingGET)
- [Partitions](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadPartitionsUsingGET)

Pour les cas d’utilisation tels que l’enrichissement des données de prospect ou d’activité, vous pouvez également mettre en cache des types de ressources tels que des programmes, des e-mails et des dossiers.

## Limite de débit

Chaque instance Marketo a une limite de débit partagé de 100 appels par 20 secondes sur tous les services d’API tiers. Si les appels dépassent cette limite, l’API renvoie un code d’erreur 606.

En règle générale, limitez chaque intégration tierce à 50 appels par 20 secondes ou moins afin que plusieurs intégrations d’API et utilisateurs puissent partager la capacité disponible. Certains cas d’utilisation peuvent nécessiter la limite complète. Toutefois, les applications qui utilisent le traitement par lots et ciblent un débit plus faible sont généralement plus réactives et cohérentes, avec une légère augmentation de la latence.

## Limite de simultanéité

Chaque instance Marketo a une limite partagée de dix appels d’API REST exécutés simultanément. Ne supposez pas que votre application est le seul consommateur de cette limite.

Marketo comptabilise les appels en cours de traitement qui n’ont pas encore été renvoyés. Lorsqu’un appel est renvoyé, il n’est plus comptabilisé dans la limite de simultanéité.

La plupart des intégrations ne bénéficient pas d’appels simultanés. Si vous implémentez la simultanéité, limitez d’abord l’application à cinq requêtes simultanées ou moins. Augmentez la limite uniquement après avoir déterminé que l’application en nécessite davantage.

## Erreurs

Sauf dans de rares cas, les requêtes API renvoient le code d’état HTTP 200. Les erreurs de logique commerciale renvoient également 200, mais incluent des détails dans le corps de la réponse. Voir [Codes d’erreur](error-codes.md) pour plus d’informations.

N’évaluez pas l’expression de raison HTTP, car elle est facultative et peut être modifiée.
