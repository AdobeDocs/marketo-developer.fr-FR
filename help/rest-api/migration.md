---
title: Obtenir les mises à jour de l’API du lead
feature: REST API
description: Découvrez les modifications apportées aux limites des points d’entrée Obtenir les activités de lead et Obtenir les modifications de lead .
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Obtenir les mises à jour de l’API du lead

À compter du 30 septembre 2026, les appels aux points d’entrée [Obtenir les activités de lead](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) ou [Obtenir les modifications de lead](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) qui incluent le paramètre `listId` échoueront si les listes cibles contiennent 10 000 leads ou plus. Les points d’entrée renvoient un code d’erreur 1003 indiquant que la liste statique cible contient trop d’enregistrements.

Un ou plusieurs appels API récents seraient affectés par cette modification. Pour éviter toute interruption de service, vous devrez peut-être mettre à jour la façon dont vos applications s’intègrent à Marketo d’ici le 30 septembre 2026.

Ces requêtes créent souvent des recherches qui n’ont aucun résultat potentiel ou qui expirent avant d’obtenir des résultats. Limiter la taille définie améliore la réactivité des requêtes et permet aux recherches de se terminer en temps voulu.

## Comment puis-je savoir si je suis affecté ?

Cette modification n’affecte qu’un petit nombre d’instances Marketo Engage. Les administrateurs des abonnements concernés recevront une notification dans l’application avant l’application de la modification.

## Que dois-je faire ?

Partagez ce document avec les personnes ou l’équipe responsables de vos intégrations Marketo Engage.

Selon votre cas d’utilisation, utilisez l’une des options de migration suivantes :

* Limitez à 10 000 membres les listes statiques utilisées pour l’extraction d’activité. Divisez les listes existantes en listes plus petites pour continuer à interroger la même audience pour les activités .
* Extrayez des activités ou des modifications de valeur de données à l’aide d’une extraction d’activité en bloc ou de flux de données. Joignez les résultats à l’appartenance à une liste statique avec [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) ou [Extraction de lead en bloc](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract).

## Que Se Passera-T-Il Si Je Ne Fais Rien ?

Vos intégrations d&#39;API peuvent être interrompues par des erreurs non prises en charge lors de l&#39;interrogation d&#39;activités à partir de listes statiques comportant un grand nombre de membres.
