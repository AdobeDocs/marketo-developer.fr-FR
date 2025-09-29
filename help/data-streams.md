---
title: Flux de données
description: Présentation des flux de données Marketo Engage permettant une activité de prospect et des événements de contrôle des utilisateurs en temps quasi réel, ce qui allège les limites d’API pour les clients de niveau de performance
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 2%

---

# Flux de données

>[!NOTE]
> Vous trouverez désormais les informations actuelles sur les flux de données à l’adresse [Utilisation des flux de données](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/).
>

Les organisations marketing de nos clients comptent sur des campagnes marketing opportunes et ciblées pour rester à la pointe de leur activité et être compétitives. Pour prendre des décisions rapides et permettre un changement stratégique rapide, il est important de disposer de données pour prendre en charge et orienter ces décisions clés qui génèrent des campagnes ciblées et ciblées. Certains clients et clientes effectuent également des efforts marketing à des niveaux de leurs segments de clients et clientes à l’intérieur et à l’extérieur de Marketo Engage. Pour prendre en charge ces différents efforts, Marketo a créé la possibilité d’acquérir d’importants volumes de données en temps quasi réel au moyen de flux de données.

Outre l’avantage des données en temps quasi réel, il existe des avantages liés aux produits :

- Soulage le goulot d’étranglement des limites d’API, car le streaming est utilisé à la place.
- Réduit le scénario des limites d’API, générant moins de messages d’alerte.
- Aucun ne doit effectuer d’exportations en bloc pour extraire les données en raison de la fonctionnalité de diffusion de données en continu.

Les flux de données sont disponibles pour ceux qui ont acheté un package [Marketo Engage Performance Tier](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Flux de données d’activité du lead - Aperçu

Le flux de données des activités de lead fournit une diffusion en continu en temps quasi réel du suivi d’audit des activités de lead, où de grands volumes d’activités de lead peuvent être envoyés au système externe d’un client. Les flux permettent aux clients d’auditer efficacement les événements liés aux leads, les modèles d’utilisation, de fournir des vues sur les modifications des leads et de déclencher des processus et des workflows en fonction des différents types d’événements de leads. Il existe plus de 144 types d’activités auxquels vous pouvez vous abonner et qui peuvent être envoyés par le biais du flux.

Types de données de leads diffusées en continu :

1. Modifications de leads : toutes les modifications sur tous les champs et nouveaux leads
1. Activités de lead - tous les types d’activités de lead décrits dans le document
1. Leads supprimés
1. Tous les objets personnalisés du prospect (si demandé). C&#39;est tout ou rien pour le moment.

En offrant des vues sur les modifications apportées aux leads, cela permet aux clients de prendre des décisions plus rapides sur leurs stratégies marketing globales et de créer des campagnes ciblées plus ciblées. Voici quelques cas d’utilisation courants :

- Alertes personnalisées : lorsque certains prospects présentent des conditions incohérentes, ils peuvent être ajoutés à la liste. Les flux d’activités peuvent les sélectionner et pousser l’activité « Ajouter à la liste » pour que les clients puissent effectuer une action de suivi.
- Optimiser les modèles ML : certains clients prévoient de créer des modèles de notation qui utilisent des informations sur les activités et les renvoient à Marketo, ou de les utiliser dans leurs propres modèles de notation internes selon leurs besoins. En notant un prospect, les clients peuvent ensuite informer Marketo d’ajouter des clients aux campagnes d’entretien afin d’augmenter leur note.

Liste des activités diffusées en continu :

| AchieveGoalInReferral | ClickPredictiveContent | ReceivedForwardToFriendEmail |
|--- |--- |--- |
| AddToList | ClickRTPCallToAction | ReceiveSalesEmail |
| AddToNurture | ClickSalesEmail | ReferToSocialApp |
| Ajouter à l’opportunité | ClickSharedLink | RemoveFromList |
| AddToSalesCampaign | ConvertLead | RemoveFromOpportunity |
| CallWebhook | Supprimer le lead | Campagne de demande |
| ChangeDataValue | Disqualifier les tirages au sort | SalesEmailBounce |
| ChangeLeadPartition | EarnEntryInSocialApp | SendAlert |
| ChangeCultureCadence | E-mail non remis | SendEmail |
| ChangeNurtureTrack | EmailBounceSoft | SendSalesEmail |
| Modifier le propriétaire | E-mail diffusé | SentForwardToFriendEmail |
| ModifierDonnéesProgramme | EnrichWithDataDotCom | Activité SFDCA |
| ModifierDonnéesMembreProgramme | EnterSweepstakes | ShareContent |
| ChangeRevenueStage | FillOutFacebookLeadAdsForm | SignUpForReferralOffer |
| ChangeRevenueStageManually | FillOutForm | SyncLeadVersMicrosoft |
| ChangeScore | Moment intéressant | SyncLeadVersSFDC |
| ModifierSegment | MergeLeads | UnsubscribeEmail |
| ChangeStatusInProgression | NewLead | UpdateOpportunity |
| ChangeStatusInSalesCampaign | OpenEmail | VisitWebPage |
| ClickEmail | OpenSalesEmail | VoteInPoll |
| ClickLink | PushLeadToMarketo | WinSweepstakes |

Notez que si des objets personnalisés doivent être diffusés en continu, il doit s’agir de tous les objets personnalisés liés au lead. Il n&#39;y a actuellement aucun moyen de sélectionner ceux qui sont souhaités.

## Flux de données d’audit de l’utilisateur - Aperçu

Le flux de données d’audit d’utilisateur fournit un suivi d’audit en temps quasi réel des modifications de ressources par les utilisateurs&#x200B;. Le client peut ainsi auditer efficacement les événements de ressources, afficher les modifications apportées aux utilisateurs et déclencher des processus ou des workflows en fonction de différents types d’événements d’audit. Les modifications de ressources en temps quasi réel sont envoyées via des événements Adobe I/O à un point d’entrée configurable. Les événements d’audit sont répartis par type de ressource et peuvent s’abonner aux événements d’audit qui leur sont importants.

Un bon cas d’utilisation pour l’abonnement à ce flux serait :

- Suivi des modifications lors de l’utilisation de plusieurs systèmes marketing : certains clients effectuent également un certain niveau d’activités marketing dans un autre système, tel qu’un CRM comme Salesforce, puis transmettent le prospect à Marketo. Le prospect est parfois mis à jour et synchronisé dans les deux sens. Il est donc important de savoir quel système a récemment apporté des modifications.

Liste des événements d’audit d’utilisateur diffusés en continu :

| COMPOSANT | LISTE DES TYPES D’ÉVÉNEMENTS |
|--- |--- |
| Programme par défaut | cloner, créer, supprimer, modifier le canal, exporter, modifier la configuration du programme, modifier le jeton de programme, renommer |
| E-mail | approuver, cloner, créer, supprimer, modifier, déplacer, renommer, annuler l’approbation |
| Programme d&#39;e-mail en masse | approuver, childUpdate, cloner, créer, supprimer, modifier, modifier le canal, modifier le planning du programme, modifier la configuration du programme, modifier le jeton du programme, renommer, annuler l’approbation |
| Modèle d’e-mail | approuver, cloner, créer, supprimer, draftCreate, draftDiscard, modifier, renommer, annuler l&#39;approbation |
| Programme d’engagement | cloner, créer, supprimer, modifier le canal, modifier la configuration du programme, modifier le flux du programme, modifier le jeton du programme, renommer |
| Programme d’événement | cloner, créer, supprimer, modifier le canal, modifier le planning du programme, modifier la configuration du programme, modifier le jeton du programme, renommer |
| Dossier | créer, supprimer, modifier, renommer |
| Form | approuver, cloner, créer, supprimer, brouillonCréer, modifier, déplacer, renommer |
| Formulaire -> Formulaire de la page de destination | créer, cloner, modifier, supprimer, approuver, renommer |
| Page de destination | approuver, cloner, créer, supprimer, brouillonIgnorer, modifier, renommer, annuler l&#39;approbation |
| Modèle de page de destination | approuver, cloner, créer, supprimer, draftCreate, draftDiscard, modifier, renommer, annuler l&#39;approbation |
| Liste intelligente | cloner, créer, supprimer, modifier, exporter, modifier la configuration de la liste dynamique, renommer |
| Dossier marketing | créer, modifier, supprimer |
| Programme de fidélisation | cloner, créer, supprimer, modifier le canal, modifier la configuration du programme, modifier le flux du programme, modifier le jeton du programme, renommer |
| Segment | créer, supprimer, modifier, renommer |
| Segmentation | approuver, créer, supprimer, draftCreated, draftDiscarded, renommer, annuler l&#39;approbation |
| Campagne intelligente | abandonner, activer, cloner, créer, désactiver, supprimer, modifier, modifier le planning de campagne, modifier l’action d’étape de flux, modifier la configuration de la liste dynamique, déplacer, renommer |
| Extrait | approuver, approuver sans brouillon, cloner, créer, supprimer, modifier, renommer, annuler l’approbation |
| Admin UI -> Launchpoint -> Intégration | créer, supprimer, modifier |
| Interface utilisateur d’administration -> Utilisateur | créer, modifier, supprimer (identique pour l’utilisateur de l’API uniquement) |
| Connexion administrateur -> utilisateur | connexion réussie, échec de connexion |
| Programme -> Programme par lots d’e-mails | Modifier l’API de ressources (pour modifier l’adresse e-mail sélectionnée) |
| Programme -> Programme marketing | créer, cloner |

Exemple d’événement d’audit d’utilisateur :

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

## Présentation Du Flux De Données De Notification

Le flux de données de notification est disponible dans le cadre des offres de niveau de performances de Marketo Engage.

Actuellement, le centre de notifications de Marketo peut être configuré pour envoyer des notifications à une adresse e-mail. Le flux de données de notification permet d’envoyer les notifications directement à un point d’entrée configurable via des événements Adobe I/O. Les notifications sont fournies via l’interface utilisateur dès aujourd’hui et peuvent être référencées par la cloche orange en haut à droite de l’écran. Ce flux prend ces notifications et les envoie via un flux.

Liste des événements de notification :

| COMPOSANT | LISTE DES TYPES D’ÉVÉNEMENTS |
|--- |--- |
| Notification | abandon de campagne, échec de campagne, maturation (programme épuisé), échec de synchronisation de Salesforce, groupe de test (résultat du test A/B), services web (quota quotidien) |

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

Cette section fournit des instructions sur ce qui est nécessaire, comment se connecter et recevoir des données en continu pour chacun des flux. Un certain niveau de codage et de configuration est nécessaire pour chacun d’eux.

### Flux de données d’activité du lead

Le flux d’activité de lead fournit une diffusion en continu en temps quasi réel des événements d’activité de lead de Marketo et envoie les modifications de type d’activité avec abonnement et attributs configurables :

- Fréquence des envois de données toutes les 2 secondes par défaut.
- Lots de 100 à 500 par abonnement.
- Le délai d’expiration du service REST client est de 20 secondes avec 3 reprises toutes les 3 minutes et l’activation automatique en cas de succès. Sinon, elles sont mises en pause. Une fois suspendu, le service tente, toutes les 3 minutes, de le réactiver, sauf s’il a été désactivé manuellement.
- Conservation des données dans une file d’attente pendant 7 jours au maximum.

Pour mettre en œuvre le flux de données d’activité du prospect, les clients doivent suivre les étapes suivantes :

1. Exposez un point d’entrée HTTP qui peut recevoir des requêtes POST avec un corps JSON d’Internet public. Le flux de données des notifications push d’activité envoie des requêtes à :
1. Fournissez les éléments suivants à Adobe :
   1. Marketo Munchkin ID pour son abonnement
   1. URL du point d’entrée à l’étape 1
   1. Les types d&#39;activités qu&#39;ils souhaitent recevoir (liste complète ci-dessus)
   1. Moyen d’authentification permettant au client de vérifier que les demandes sont légitimes. Soit :
      1. Une URL de fournisseur d’identité, un ID client et un secret client pour OAuth [authentification par informations d’identification client](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Jeton API pouvant être inclus dans les requêtes envoyées par le flux de données de l’activité du prospect dans un en-tête HTTP d’autorisation

Adobe active ensuite le flux de données, à partir duquel les clients commencent à recevoir des données.

Diagramme UML d’un appel de flux de données d’activité de lead type :

![Diagramme Flux de données d’activité du lead](assets/lead-activity-data-stream.png)

Exemple de création de point d’entrée d’URL :

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

Vous trouverez [ici](https://github.com/ihgrant/activity-stream-consumer-example) un exemple de code pour une application qui utilise le flux de données de l’activité du prospect Marketo.

### Flux de données d’audit d’utilisateur et flux de données de notification

Les événements d’audit de l’utilisateur sont envoyés à Adobe IO et peuvent être consommés en se connectant avec un Adobe ID. Procédez comme suit :

1. Les clients fournissent à Adobe les éléments suivants :
   1. Adobe ID
   1. Marketo Munchkin ID pour son abonnement
1. Le client expose un point d’entrée REST pour consommer des événements normalement sous la forme d’un webhook.
1. Une fois cette information fournie, Adobe active le flux pour l’abonnement du client.
1. Le client configure ensuite le flux dans Adobe IO (instructions à fournir)
   1. Cette étape nécessite une organisation Adobe
   1. Nécessite que l’utilisateur de l’organisation Adobe ait un rôle de développeur ou d’administrateur système

Pour configurer Adobe IO, reportez-vous à la section [Configuration de flux de données d’audit des utilisateurs Marketo avec Adobe IO](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup/) de la section Documentation publique.

### Configuration du flux de données d’audit des utilisateurs dans Marketo

Le flux de données de contrôle de l’utilisateur est actuellement disponible dans le cadre des packages de performances avec les 3 autres flux de données. Pour plus d’informations sur les packages, reportez-vous à la [page de description du produit](https://helpx.adobe.com/legal/product-descriptions/adobe-marketo-engage---product-description.html) pour connaître les limites et fonctionnalités du produit.

### Configuration d’Adobe I/O

[Voir Prise en main de Adobe I/O Events](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Pour obtenir des instructions de base sur ce cas d’utilisation, à partir de [console.adobe.io](https://developer.adobe.com/console) :

Lorsque vous y êtes invité, sélectionnez **[!UICONTROL Créer un projet]** ou **[!UICONTROL Ajouter un événement]**.

### Prise en main de votre nouveau projet

Pour commencer à utiliser les services Adobe, ajoutez une API, des événements ou un runtime, consultez notre [documentation](https://developer.adobe.com/runtime/docs/).

## Documentation publique

- [Flux De Données Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Présentation des événements et Webhooks Adobe IO](https://developer.adobe.com/events/docs/guides/)
- [Blog sur les flux de données](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
