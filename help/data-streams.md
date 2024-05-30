---
title: "Flux de données"
description: "Présentation des Data Steams"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1594'
ht-degree: 2%

---


# Flux de données

Les organisations marketing de nos clients comptent sur des campagnes marketing ciblées et opportunes pour rester au sommet de leur activité et être compétitives. Afin de prendre en charge des décisions rapides et de permettre des changements stratégiques à la vitesse voulue, il est important de disposer de données pour prendre en charge et piloter ces décisions clés qui fournissent des campagnes ciblées et ciblées. Certains clients effectuent des efforts marketing à des niveaux de leurs segments de clients à l’intérieur et à l’extérieur du Marketo Engage. Pour prendre en charge ces différents efforts, Marketo a créé la possibilité d’acquérir de gros volumes de données en temps quasi réel via les flux de données.

Outre les avantages des données en temps quasi réel, il existe des avantages liés aux produits :

- Détermine le goulot d’étranglement des limites de l’API, car la diffusion en continu est utilisée à la place.
- Réduit le scénario des limites de l’API, générant moins de messages d’alerte.
- Aucun ne doit effectuer d’exportations en bloc pour extraire des données en raison de la fonctionnalité de diffusion en continu des données.

Les flux de données sont disponibles pour ceux qui ont acheté une [Package de niveau de performance Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Présentation du flux de données des activités de piste

Le flux de données des activités de piste fournit une diffusion en continu en temps quasi réel du suivi des audits des activités de piste où de grands volumes d’activités de piste peuvent être envoyés au système externe d’un client. Les flux permettent aux clients de contrôler de manière efficace les événements liés aux pistes, les schémas d’utilisation, de fournir des vues sur les modifications des pistes et de déclencher des processus et des workflows en fonction des différents types d’événements de piste. Il existe plus de 144 types d’activité qui peuvent être abonnés et envoyés par le biais du flux.

Types de données de piste diffusées :

1. Changements de piste - toutes les modifications sur tous les champs et les nouvelles pistes
1. Activités de piste : tous les types d’activité de piste décrits dans le document.
1. Pistes supprimées
1. Tous les objets personnalisés sur la piste (si nécessaire). C&#39;est tout ou rien pour le moment.

En fournissant des vues sur les changements de piste, cela permet aux clients de prendre des décisions plus rapides sur leurs stratégies marketing globales et de créer des campagnes ciblées plus ciblées. Voici quelques cas d’utilisation courants :

- Alertes personnalisées : lorsque certaines pistes présentent des conditions incohérentes, elles peuvent être ajoutées à la liste. Les flux d’activités peuvent les sélectionner et pousser l’activité &quot;Ajouter à la liste&quot; pour que les clients puissent suivre l’action.
- Alimentation de modèles ML : certains clients prévoient de créer des modèles de notation qui utilisent les informations d’activité et les ramènent à Marketo ou les utilisent dans leurs propres modèles de notation internes selon leurs besoins. En évaluant une piste, les clients peuvent alors informer Marketo d’ajouter des clients aux campagnes de formation, ce qui augmente leur score.

Liste des activités en continu :

| SuccessGoalInReferral | ClickPredictiveContent | ReceivedForwardToFriendEmail |
|--- |--- |--- |
| AddToList | ClickRTPCallToAction | ReceiveSalesEmail |
| AddToNurture | ClickSalesEmail | Reportez-vous à SocialApp |
| AddToOpportunity | ClickSharedLink | RemoveFromList |
| AddToSalesCampaign | ConvertLead | RemoveFromOpportunity |
| CallWebhook | DeleteLead | RequestCampaign |
| ChangeDataValue | DisqualifSweepstakes | SalesEmailBounce |
| ChangeLeadPartition | EarnEntryInSocialApp | SendAlert |
| ChangeNurtureCadence | EmailBounce | SendEmail |
| ChangeNurtureTrack | EmailBounceSoft | SendSalesEmail |
| ChangeOwner | EmailDelivered | SentForwardToFriendEmail |
| ChangeProgramData | EnrichWithDataDotCom | SFDCAactivity |
| ChangeProgramMemberData | EnterSweepstakes | ShareContent |
| ChangeRevenueStage | FillOutFacebookLeadAdsForm | SignUpForReferralOffer |
| ChangeRevenueStageManually | FillOutForm | SyncLeadToMicrosoft |
| ChangeScore | IntéressantMoment | SyncLeadToSFDC |
| ChangeSegment | MergeLeads | UnsubscribeEmail |
| ChangeStatusInProgression | NewLead | UpdateOpportunity |
| ChangeStatusInSalesCampaign | OpenEmail | VisitWebPage |
| ClickEmail | OpenSalesEmail | VoteInPoll |
| ClickLink | PushLeadToMarketo | WinSweepstakes |

Notez que si les objets personnalisés doivent être diffusés en continu, il doit s’agir de tous les objets personnalisés liés au prospect. Actuellement, il n’est pas possible de sélectionner les options souhaitées.

## Présentation du flux de données d’audit utilisateur

Le flux de données d’audit utilisateur fournit un suivi en temps quasi réel des modifications de ressources par les utilisateurs &#x200B;. Cela permet à un client de contrôler efficacement les événements de ressources, de fournir une vue des modifications de l’utilisateur et de déclencher des processus ou des workflows en fonction de différents types d’événements de contrôle. Les modifications de ressources en temps quasi réel sont envoyées via des événements d’Adobe I/O à un point de terminaison configurable. Les événements d’audit sont ventilés par type de ressource et peuvent s’abonner à des événements d’audit importants pour eux.

Voici un bon cas d’utilisation pour s’abonner à ce flux :

- Suivi des modifications lors de l’utilisation de plusieurs systèmes marketing : certains clients effectuent également un certain niveau d’activités marketing dans un autre système, tel qu’un CRM comme Salesforce, puis transmettent le prospect à Marketo. Il arrive que le prospect soit mis à jour et synchronisé. Il est donc important de savoir quel système a apporté des modifications récentes.

Liste des événements d’audit utilisateur diffusés :

| COMPOSANT | LISTE DE TYPE D’ÉVÉNEMENT |
|--- |--- |
| Programme par défaut | clone, créer, supprimer, modifier le canal, exporter, modifier la configuration du programme, modifier le jeton du programme, renommer |
| E-mail | approuver, cloner, créer, supprimer, modifier, déplacer, renommer, annuler l’approbation |
| Programme d&#39;e-mail en masse | approuver, childUpdate, cloner, créer, supprimer, modifier, modifier le canal, modifier le planning du programme, modifier la configuration du programme, modifier le jeton du programme, renommer, annuler l’approbation |
| Modèle d&#39;e-mail | approuver, cloner, créer, supprimer, draftCreate, draftDiscard, edit, rename, unapprove |
| Programme d’engagement | clone, créer, supprimer, modifier le canal, modifier la configuration du programme, modifier le flux du programme, modifier le jeton du programme, renommer |
| Programme d’événement | cloner, créer, supprimer, modifier le canal, modifier le planning du programme, modifier la configuration du programme, modifier le jeton du programme, renommer |
| Dossier | créer, supprimer, modifier, renommer |
| Formulaire | approuver, cloner, créer, supprimer, draftCreate, modifier, déplacer, renommer |
| Formulaire -> Formulaire de page d’entrée | créer, cloner, modifier, supprimer, approuver, renommer |
| Page de destination | approuver, cloner, créer, supprimer, draftDiscard, modifier, renommer, annuler l’approbation |
| Modèle de page de destination | approuver, cloner, créer, supprimer, draftCreate, draftDiscard, edit, rename, unapprove |
| Liste intelligente | clone, créer, supprimer, modifier, exporter, modifier la configuration de liste dynamique, renommer |
| Dossier marketing | créer, modifier et supprimer |
| Programme de fidélisation | clone, créer, supprimer, modifier le canal, modifier la configuration du programme, modifier le flux du programme, modifier le jeton du programme, renommer |
| Segment | créer, supprimer, modifier, renommer |
| Segmentation | approuver, créer, supprimer, draftCreated, draftDiscarded, renommer, annuler l’approbation |
| Campagne intelligente | abandonner, activer, cloner, créer, désactiver, supprimer, modifier, modifier le planning de campagne, modifier l’action de l’étape de flux, modifier la configuration de la liste dynamique, déplacer, renommer |
| Extrait | approuver, approuver sans brouillon, cloner, créer, supprimer, modifier, renommer, annuler l’approbation |
| Interface utilisateur d’administration -> Point de lancement -> Intégration | créer, supprimer, modifier |
| Interface utilisateur d’administration -> Utilisateur | créer, modifier et supprimer (Identique pour l’utilisateur API uniquement) |
| Admin Login -> User | succès de connexion, échec de connexion |
| Programme -> Programme par lots d’emails | Modifier (pour modifier l’adresse électronique sélectionnée) API de ressource |
| Programme -> Programme marketing | créer, cloner |

Exemple d’événement d’audit utilisateur :

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "id": "b77c743a-8e28-40f2-8aab-9541bbc85e68",
        "type": "com.adobe.platform.marketo.audit.user.email",
        "source": "https://www.marketo.com",
        "time": "2020-05-28T19:20:47.28Z",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentId": 232459,
            "componentType": "Email",
            "eventAction": "approve",
            "munchkinId": "123-ABC-456",
            "imsOrgId": "ADOBEORGID@AdobeOrg",
            "user": 253,
            "userId": "example@marketo.com"          
        }
    }
}
```

## Présentation du flux de données de notification

Le flux de données de notification est disponible dans le cadre des offres de niveau de performance de Marketo Engage.

Actuellement, le centre de notification de Marketo peut être configuré pour envoyer des notifications à une adresse email. Le flux de données de notification permet d’envoyer directement les notifications à un point de terminaison configurable via des événements d’Adobe I/O. Les notifications sont fournies via l’interface utilisateur actuelle et peuvent être référencées par la cloche orange en haut à droite de l’écran. Ce flux prend ces notifications et les envoie par un flux.

Liste des événements de notification :

| COMPOSANT | LISTE DE TYPE D’ÉVÉNEMENT |
|--- |--- |
| Notification | abandon de campagne, échec de campagne, pépinière (programme épuisé), échec de synchronisation Salesforce, groupe test (résultat de test A/B), services web (quota quotidien) |

Exemple d’événement de notification :

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.notification.campaign_abort",
        "source": "https://www.marketo.com",
        "time": "2021-05-27T10:22:37.489-5:00",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentType": "campaign_abort",
            "subType": "user_campaign_abort",
            "eventAction": {
                "campaignId":1234,
                "userId":"example@marketo.com",
            }
            "imsOrgId":"ADOBEORGID@AdobeOrg",
            "munchkinId":"123-ABC-456"
        }
    }
}
```

## Détails techniques

Cette section fournit des instructions sur ce qui est nécessaire, comment connecter et recevoir des données en continu pour chaque flux. Il y a un certain niveau de codage et de configuration impliqué pour chacun d’eux.

### Flux de données d’activité de piste

Le flux d’activité de piste fournit une diffusion en continu en temps quasi réel des événements d’activité de piste Marketo et envoie des modifications de type d’activité abonné avec des attributs configurables :

- Par défaut, la fréquence des données est transmise toutes les 2 secondes.
- Lots de 100 à 500 par abonnement.
- Le délai d’attente du service REST client est de 20 secondes avec 3 reprises toutes les 3 minutes et automatiquement activé en cas de succès. Sinon, elles sont mises en pause. Une fois qu’il est en pause, le service tente de réactiver toutes les 3 minutes, à moins qu’il ne soit déconfiguré manuellement.
- Conservation des données dans une file d’attente pendant 7 jours au maximum.

Pour mettre en oeuvre le flux de données d’activité de piste, les clients doivent suivre les étapes suivantes :

1. Exposez un point de terminaison HTTP qui peut recevoir des requêtes de POST avec un corps JSON de l’Internet public. Le flux de données Push d’activité envoie des demandes à :
1. Fournissez un Adobe avec les éléments suivants :
   1. Marketo Munchkin ID pour son abonnement
   1. URL du point de terminaison de l’étape 1
   1. Types d’activité qu’ils souhaitent recevoir (liste complète ci-dessus)
   1. Un moyen d’authentification, de sorte que le client puisse vérifier que les demandes sont légitimes. Soit :
      1. Une URL de fournisseur d’identité, un identifiant client et un secret client pour OAuth [Authentification des informations d’identification du client](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Jeton API pouvant être inclus dans les requêtes envoyées par la banque de données des activités de piste dans les paramètres de requête ou dans un en-tête d’autorisation (choix du client)

Adobe active ensuite le flux de données, à ce stade, les clients commencent à recevoir des données.

Schéma UML d’un appel de flux de données d’activité de piste standard :

![Diagramme de flux de données d’activité de piste](assets/lead-activity-data-stream.png)

Exemple de création de point de terminaison d’URL :

```javascript
/*
Copyright 2022 Adobe
All Rights Reserved.

NOTICE: Adobe permits you to use, modify, and distribute this file in
accordance with the terms of the Adobe license agreement accompanying
it.
*/
constexpress=require('express')
constwinston=require('winston');
constport=3000

constapp=express().use(express.json())

constlogger=winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: {service: 'activity-stream-consumer-example'},
  transports: [
    // - Write all logs with level `error` and below to `error.log`
    newwinston.transports.File({filename: 'error.log',level: 'error'}),
    // - Write all logs with level `info` and below to `combined.log`
    newwinston.transports.File({filename: 'combined.log'}),
    newwinston.transports.Console({format: winston.format.simple()})
  ],
});

app.get('/',(req,res)=>{
  logger.info(JSON.stringify(req.query))
  res.sendStatus(200)
})

app.post('/',(req,res)=>{
  logger.info(JSON.stringify(req.body))
  res.sendStatus(200)
})

app.listen(port,()=>{
  logger.info(`app listening on port ${port}`)
})
```

Vous trouverez un exemple de code pour une application qui utilise le flux de données d’activité de piste Marketo. [here](https://github.com/ihgrant/activity-stream-consumer-example).

### Flux de données d’audit utilisateur et flux de données de notification

Les événements d’audit utilisateur sont envoyés aux E/S d’Adobe et peuvent être utilisés en se connectant à l’aide d’Adobe ID. Voici les étapes à suivre :

1. Les clients fournissent un Adobe avec les éléments suivants :
   1. ID Adobe
   1. Marketo Munchkin ID pour son abonnement
1. Le client expose un point de terminaison REST pour consommer les événements normalement sous la forme d’un webhook.
1. Une fois fourni, Adobe active le flux pour l’abonnement du client.
1. Le client configure ensuite le flux dans Adobe IO (instructions à fournir).
   1. Cette étape nécessite une organisation d’Adobe
   1. Nécessite que l’utilisateur de l’organisation Adobe ait un rôle de développeur ou d’administrateur système

Pour configurer les E/S d’Adobe, voir [Configuration des flux de données d’audit utilisateur de Marketo avec des E/S d’Adobe](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup/) dans la section Documentation publique .

### Configuration du flux de données d’audit utilisateur dans Marketo

Le flux de données d’audit utilisateur est actuellement disponible dans le cadre des packages de performances avec les 3 autres flux de données. Pour plus d’informations sur les packages, reportez-vous au [Page Description du produit](https://helpx.adobe.com/legal/product-descriptions/adobe-marketo-engage---product-description.html) pour les limites de produit et les fonctionnalités.

### Configuration de l’Adobe I/O

[Voir Prise en main des événements d’Adobe I/O](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Pour obtenir des instructions de base sur ce cas pratique, commencez par [console.adobe.io](https://developer.adobe.com/console):

Lorsque vous y êtes invité, sélectionnez **[!UICONTROL Créer un projet]** ou **[!UICONTROL Ajouter un événement]**.

### Prise en main de votre nouveau projet

Pour commencer à utiliser les services Adobe, ajoutez une API, des événements ou un runtime, consultez notre [documentation](https://developer.adobe.com/runtime/docs/).

## Documentation publique

- [Flux de données Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Présentation des événements et des webhooks d’Adobe IO](https://developer.adobe.com/events/docs/guides/)
- [Blog de flux de données](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
