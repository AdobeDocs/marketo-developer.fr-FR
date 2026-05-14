---
title: Obtenir les mises à jour de l’API du lead
feature: REST API
description: Découvrez les modifications apportées aux limites des points d’entrée Obtenir les activités de lead et Obtenir les modifications de lead .
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 0%

---

# Obtenir les mises à jour de l’API du lead

À compter du 30 septembre 2026, les appels aux points d’entrée [Obtenir les activités de lead](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) ou [Obtenir les modifications de lead](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) qui incluent le paramètre `listId` échoueront si les listes cibles contiennent 10 000 leads ou plus avec un code d’erreur 1003 indiquant que la liste statique cible contient trop d’enregistrements. Un ou plusieurs appels API ont récemment été effectués, qui pourraient être affectés par cette modification. Pour éviter toute interruption de service, vous devrez peut-être mettre à jour la façon dont vos applications s’intègrent à Marketo d’ici le 30 septembre 2026.

Ces types de requêtes créent souvent des recherches qui n’ont aucun résultat potentiel ou expirent avant d’obtenir un résultat. Limiter la taille de l’ensemble améliore la réactivité de ces types de requêtes et garantit qu’une recherche du jeu de données peut être effectuée en temps voulu.

## Comment puis-je savoir si je suis affecté ?

Cette modification n’affectera qu’un petit nombre d’instances Marketo Engage. Les administrateurs des abonnements concernés seront avertis dans l’application avant l’application de la modification.

## Que dois-je faire ?

Vous devez partager ce document avec les personnes ou l’équipe responsables de vos intégrations Marketo Engage.

Selon votre cas d’utilisation, il existe deux options de base pour migrer votre application vers :

* Limitez le nombre de listes statiques à partir desquelles vous extrayez des activités à un maximum de 10 000 membres. Vous pouvez diviser n’importe laquelle de vos listes existantes en listes plus petites afin de continuer à interroger la même audience pour les activités .
* Extrayez vos activités ou modifications de valeur de données à l’aide de l’extraction d’activité en bloc ou de flux de données et joignez ces résultats à l’appartenance à une liste statique avec [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) ou [Extraction de lead en bloc](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract)

## Que Se Passera-T-Il Si Je Ne Fais Rien ?

Il se peut que le fonctionnement de vos intégrations d’API soit interrompu en raison d’erreurs non prises en charge lors de l’interrogation d’activités à partir de listes statiques comportant un grand nombre de membres.
