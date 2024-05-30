---
title: "Bonnes pratiques d’intégration Marketo"
feature: REST API
description: '"Bonnes pratiques pour l’utilisation des API Marketo".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---


# Bonnes pratiques d’intégration Marketo

## Limites de l’API

- **Quota quotidien :** La plupart des abonnements se voient attribuer 50 000 appels d’API par jour (qui se réinitialise tous les jours à 00h00 du matin de l’heure du Pacifique). Vous pouvez augmenter votre quota quotidien par l’intermédiaire de votre gestionnaire de compte.
- **Limite de taux :** Accès à l’API par instance limité à 100 appels par 20 secondes.
- **Limite de simultanéité :**  Nombre maximum de dix appels API simultanés.
- **Taille du lot :** Lead DB - 300 enregistrements ; Asset Query - 200 enregistrements
- **Taille de la payload de l’API REST :** 1 Mo
- **Taille de fichier d’importation en bloc :** 10 Mo
- **Taille maximale du lot SOAP :** 300 enregistrements
- **Tâches d’extraction en bloc :** 2 en cours d’exécution ; 10 en file d’attente (inclus)

## Conseils rapides

- Supposons que votre application soit en concurrence avec d’autres applications pour les ressources de quota, de taux et d’accès simultané, et fixez des limites d’utilisation prudentes.
- Lorsque cela est possible et approprié, utilisez les méthodes Marketo en bloc et par lots. N’utilisez qu’un seul enregistrement ou des appels de résultat uniques si nécessaire.
- Utilisation [backoff exponentiel](https://en.wikipedia.org/wiki/Exponential_backoff) pour réessayer les appels API qui échouent en raison de limites de taux ou de simultanéité.
- Évitez d’effectuer des appels API simultanés si votre cas d’utilisation n’en bénéficie pas.

## Traitement par lot

Pour optimiser les performances de vos intégrations, lors d’insertions ou de mises à jour, les enregistrements doivent être regroupés dans le moins de transactions possible. Lors de la récupération d’enregistrements d’un entrepôt de données pour envoi, les enregistrements doivent toujours être agrégés avant envoi, plutôt que d’envoyer une requête pour chaque modification individuelle.

## Latence acceptable

La détermination de vos tolérances de latence, ou du temps maximal qui peut s’écouler avant l’envoi d’un appel API, informera un grand nombre, voire la plupart, des décisions que vous prenez lors de la conception de votre intégration à Marketo. Marketo fournit de nombreuses méthodes et options de configuration différentes adaptées à différents cas d’utilisation, ainsi qu’à différentes classes de latence. Par exemple, une intégration en temps réel permettant d’informer un vendeur d’un utilisateur qui s’abonne à un test peut uniquement envoyer des lots d’un si un suivi immédiat est requis. Cependant, la plupart des cas ne le nécessitent pas et peuvent tolérer une latence supplémentaire et peuvent être gérés plus efficacement par le biais d’appels de mise en file d’attente et de traitement par lot.

| Latence acceptable | Méthodes préférées | Notes |
|---|---|---|
| Faible (&lt;10 s) | API synchrones (mises en lot ou non par lot) | Assurez-vous que votre cas d’utilisation le requiert. L’envoi d’appels immédiats et synchrones pour des cas d’utilisation à volume élevé peut rapidement utiliser un quota d’API quotidien et potentiellement dépasser les limites de taux et de simultanéité. |
| Medium (10 à 60 m) | API synchrones (mise en cache) | Pour les intégrations de données entrantes vers Marketo, il est vivement recommandé d’utiliser une file d’attente avec une limite d’âge et de taille. Lorsque l’une des limites est atteinte, videz la file d’attente et envoyez votre demande d’API avec les enregistrements cumulés. Il s’agit d’un compromis important entre la vitesse et l’efficacité, en veillant à ce que vos demandes se produisent à la cadence requise, tout en battant autant d’enregistrements que l’âge de la file d’attente le permet. |
| High(>60m) | Importation/exportation en bloc (si prise en charge) | Pour les intégrations de données entrantes, les enregistrements doivent être placés en file d’attente et envoyés par le biais des API Marketo Bulk chaque fois que cela est possible. |

## Limites quotidiennes

Chaque instance de Marketo compatible avec les API a une allocation quotidienne d’au moins 10 000 appels d’API REST par jour, mais plus généralement 50 000 ou plus, et 500 Mo ou plus de capacité d’extraction en bloc. Bien qu’une capacité quotidienne supplémentaire puisse être achetée dans le cadre d’un abonnement Marketo, votre conception d’application doit tenir compte des limites courantes des abonnements Marketo.

Comme la capacité est partagée entre tous les services d’API et utilisateurs d’une instance, la bonne pratique consiste à éliminer les appels redondants et à créer par lots le moins d’appels possibles. Le moyen le plus efficace d’importer des enregistrements consiste à utiliser les API d’importation en bloc de Marketo, disponibles pour [Prospects/personnes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) et [Objets personnalisés](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Snippets/operation/createSnippetUsingPOST). Marketo fournit également un extraction en bloc pour [Pistes](bulk-lead-extract.md) et [Activités](bulk-activity-extract.md).

### Mise en cache

Les résultats des opérations suivantes peuvent généralement être mis en cache côté client pendant un jour ou plus, car ils changent rarement :

- Résultats des opérations de description
- [Types d’activité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partitions](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadPartitionsUsingGET)

La mise en cache de certains types de ressources, tels que les programmes, les emails et les dossiers, est également appropriée pour certains cas d’utilisation, tels que l’enrichissement des données pour les enregistrements de piste ou d’activité.

## Limite de taux

Chaque instance Marketo a une limite de débit de 100 appels par 20 secondes, qui est partagée entre tous les services d’API tiers. Si cette limite est dépassée, l’API répond avec un code d’erreur 606 indiquant que la limite de taux a été dépassée. En règle générale, les intégrations tierces doivent limiter leur utilisation à 50 appels par 20 secondes ou moins pour permettre une utilisation correcte des limites de taux par plusieurs intégrations d’API et utilisateurs. Bien qu’il puisse être approprié de saturer cette limite dans certains cas, en général, les applications qui utilisent le traitement par lot et ciblent leur débit à une limite inférieure à cette limite sont plus réactives et cohérentes dans leur fonctionnement, à un faible coût de latence accrue.

## Limite de simultanéité

Chaque instance Marketo a une limite partagée de dix appels d’API REST en cours d’exécution simultanée. Comme les limites de taux et de quota quotidiennes qu’elle est partagée, vous ne devez pas supposer que votre application sera le consommateur exclusif de cette limite. Marketo comptabilise le nombre d’appels simultanés comme étant en cours de traitement et n’ayant pas encore renvoyé, de sorte que lorsqu’un appel est renvoyé, il n’est plus comptabilisé à la limite des appels simultanés.

La plupart des cas d’utilisation de l’intégration ne bénéficient pas d’appels simultanés. Vous devez donc déterminer si votre application bénéficie de ces appels avant de décider d’envoyer des requêtes simultanées à Marketo. Si vous souhaitez implémenter la simultanéité, vous devez limiter le nombre de requêtes simultanées à cinq ou moins dans votre conception initiale, et ne l’augmenter qu’après avoir déterminé que votre application nécessite plus.

## Erreurs

Sauf quelques rares cas, les demandes d’API renvoient un code d’état HTTP de 200. Les erreurs de logique métier renvoient également une valeur 200, mais contiennent des informations détaillées dans le corps de la réponse. Voir [Codes d’erreur](error-codes.md) pour une explication détaillée. L’expression de raison HTTP ne doit pas être évaluée, car elle est facultative et susceptible d’être modifiée.
