---
title: Bonnes pratiques d’intégration de Marketo
feature: REST API
description: Bonnes pratiques relatives aux intégrations d’API Marketo concernant les quotas, les limites de débit et de simultanéité, les traitements par lots, l’importation et l’exportation en masse, la mise en cache et la planification de la latence.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 0%

---

# Bonnes pratiques d’intégration de Marketo

## Limites d’API

- **Quota quotidien :** la plupart des abonnements reçoivent 50 000 appels API par jour (qui sont réinitialisés tous les jours à 12 :00AM CST). Vous pouvez augmenter votre quota quotidien par l&#39;intermédiaire de votre gestionnaire de compte.
- **Limite de débit :** accès à l’API par instance limité à 100 appels par 20 secondes.
- **Limite de simultanéité :**  Maximum de dix appels API simultanés.
- **Taille du lot :** base de données du lead - 300 enregistrements ; requête de ressource - 200 enregistrements.
- **Taille de la payload de l’API REST :** 1 Mo
- **Taille du fichier d’importation en bloc :** 10 Mo
- **Taille de lot SOAP max. :** 300 enregistrements
- **Tâches d’extraction en bloc :** 2 en cours d’exécution ; 10 placées en file d’attente (incluse)

## Conseils rapides

- Supposons que votre application soit en concurrence avec d’autres applications pour les ressources de quota, de taux et de simultanéité, et définissez des limites d’utilisation prudentes.
- Utilisez les méthodes de Marketo par lot et par lots si elles sont disponibles et appropriées. N’utilisez qu’un seul enregistrement ou qu’un seul appel de résultat, si nécessaire.
- Utilisez l’[exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) pour réessayer les appels API qui échouent en raison de limites de taux ou de simultanéité.
- Évitez d’effectuer des appels API simultanés si votre cas d’utilisation n’en tire pas parti.

## Traitement par lots

Pour optimiser les performances de vos intégrations, lors des insertions ou des mises à jour, les enregistrements doivent être regroupés en le moins de transactions possible. Lors de la récupération d’enregistrements d’un magasin de données pour envoi, les enregistrements doivent toujours être agrégés avant envoi, plutôt que d’envoyer une demande pour chaque modification individuelle.

## Latence acceptable

La détermination de vos tolérances de latence, ou de la durée maximale qui peut s’écouler avant l’envoi d’un appel API, informera de nombreuses décisions, voire de la plupart, que vous prenez lors de la conception de votre intégration à Marketo. Marketo fournit de nombreuses méthodes différentes et options de configuration qui conviennent à différents cas d’utilisation et différentes classes de latence. Par exemple, une intégration en temps réel pour informer un vendeur de l’inscription d’un utilisateur à une version d’essai peut uniquement envoyer des lots d’un seul si un suivi immédiat est requis. Cependant, la plupart des cas ne le nécessitent pas et peuvent tolérer une latence supplémentaire. Ils peuvent également être gérés plus efficacement par le biais d’appels en file d’attente et par lots.

| Latence acceptable | Méthodes préférées | Notes |
|---|---|---|
| Faible (&lt;10s) | API synchrones (par lot ou non) | Assurez-vous que votre cas d’utilisation le requiert. L’envoi d’appels immédiats et synchrones pour des cas d’utilisation à volume élevé peut rapidement consommer un quota d’API quotidien et potentiellement dépasser les limites de taux et de simultanéité. |
| Medium(10s - 60m) | API synchrones (par lots) | Pour les intégrations de données entrantes vers Marketo, il est vivement recommandé d’utiliser une file d’attente avec une limite d’âge et de taille. Lorsque l’une des limites est atteinte, videz la file d’attente et envoyez votre requête API avec les enregistrements accumulés. Il s’agit d’un compromis solide entre vitesse et efficacité. Vous pouvez ainsi vous assurer que vos requêtes se produisent à la cadence requise, tout en traitant autant d’enregistrements que le permet l’âge de la file d’attente. |
| Élevée (>60m) | Importation/exportation en bloc (si pris en charge) | Pour les intégrations de données entrantes, les enregistrements doivent être placés en file d’attente et envoyés via les API Bulk Marketo chaque fois qu’ils sont disponibles. |

## Limites quotidiennes

Chaque instance de Marketo compatible avec les API dispose d’une allocation quotidienne d’au moins 10 000 appels d’API REST par jour, mais le plus souvent de 50 000 appels ou plus, et d’une capacité d’extraction en bloc de 500MB ou plus. Bien qu’il soit possible d’acheter de la capacité quotidienne supplémentaire dans le cadre d’un abonnement à Marketo, votre conception d’application doit tenir compte des limites courantes des abonnements à Marketo.

Comme la capacité est partagée entre tous les services d’API et les utilisateurs d’une instance, il est recommandé d’éliminer les appels redondants et d’utiliser les enregistrements par lots en le moins d’appels possible. Le moyen le plus efficace d’importer des enregistrements lors des appels est d’utiliser les API d’importation en bloc de Marketo, disponibles pour les [prospects/personnes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) et [objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Snippets/operation/createSnippetUsingPOST). Marketo fournit également l’extraction en bloc pour les [prospects](bulk-lead-extract.md) et [activités](bulk-activity-extract.md).

### Mise en cache

Les résultats des opérations suivantes peuvent généralement être mis en cache côté client pendant un jour ou plus, car ils changent rarement :

- Résultats des opérations de description
- [Types d’activités](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partitions ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadPartitionsUsingGET)

La mise en cache de certains types de ressources, tels que les programmes, les e-mails et les dossiers, est également appropriée pour certains cas d’utilisation, tels que l’enrichissement des données pour les enregistrements de prospect ou d’activité.

## Limite de débit

Chaque instance de Marketo est limitée à 100 appels par 20 secondes, ce qui est partagé entre tous les services d’API tiers. Si cette limite est dépassée, l’API répond avec un code d’erreur 606 indiquant que la limite de taux a été dépassée. En règle générale, les intégrations tierces doivent limiter leur utilisation à 50 appels par 20 secondes ou moins afin de permettre une utilisation équitable des limites de débit par plusieurs intégrations d’API et utilisateurs. Bien qu&#39;il puisse être approprié de saturer cette limite dans certains cas, en général, les applications qui utilisent le traitement par lots et ciblent leur débit en dessous de cette limite sont plus réactives et cohérentes dans leur fonctionnement, à un faible coût d&#39;augmentation de la latence.

## Limite de simultanéité

Chaque instance Marketo a une limite partagée de dix appels d’API REST exécutés simultanément. Comme les limites de quota et de taux journaliers, il est partagé, vous ne devez donc pas supposer que votre application sera le consommateur exclusif de cette limite. Marketo compte le nombre d’appels simultanés comme ceux qui sont en cours de traitement et qui n’ont pas encore été renvoyés. Lorsqu’un appel est renvoyé, il n’est donc plus comptabilisé par rapport à la limite d’appels simultanés.

La plupart des cas d’utilisation d’intégration ne bénéficient pas des appels simultanés. Déterminez donc si votre application en bénéficie avant de décider d’envoyer des requêtes simultanées à Marketo. Si vous souhaitez implémenter la simultanéité, vous devez limiter le nombre de requêtes simultanées à cinq ou moins dans votre conception initiale, et augmenter ce nombre uniquement après avoir déterminé que votre application en nécessite davantage.

## Erreurs

Sauf dans de rares cas, les requêtes d’API renvoient un code d’état HTTP de 200. Les erreurs de logique commerciale renvoient également un 200, mais contiennent des informations détaillées dans le corps de la réponse. Voir [Codes d’erreur](error-codes.md) pour une explication détaillée. L’expression de raison HTTP ne doit pas être évaluée, car elle est facultative et peut être modifiée.
