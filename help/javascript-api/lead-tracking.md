---
title: Suivi du lead
description: API de suivi des pistes
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: 1ad2d793832d882bb32ebf7ef1ecd4148a6ef8d5
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 0%

---

# API de suivi des pistes

Marketo Munchkin JavaScript permet le suivi des visites et des clics des pages des utilisateurs finaux vers vos pages Web externes et vos pages de destination Marketo. Elles sont enregistrées dans Marketo en tant qu’activités &quot;Page web de visite&quot; et &quot;Lien cliqué sur la page web&quot;, qui peuvent ensuite être utilisées dans les déclencheurs et les filtres pour les campagnes dynamiques et les listes dynamiques.

## Incorporation du code

Votre instance Marketo fournit automatiquement des fragments de code de suivi préconfigurés pour incorporer du code sur vos pages externes qui effectuent le suivi de l’activité vers votre instance Marketo. L’utilisation du code incorporé est régie par ce [contrat de licence](../munchkin-license.pdf).

Trois types de code de suivi sont disponibles :

1. Simple : charge de manière synchrone
1. Asynchrone - Chargements asynchrones
1. jQuery asynchrone : charge de manière asynchrone et nécessite que jQuery soit chargé au préalable.

Il est vivement recommandé d’utiliser le code de suivi asynchrone pour incorporer Munchkin sur des pages externes. Pour garantir le taux de réussite le plus élevé possible pour l’exécution, incorporez le code de suivi asynchrone dans `<head>` de chaque page.

Certains systèmes de gestion de contenu peuvent avoir des méthodes ou des restrictions spécifiques lors de l’incorporation de scripts arbitraires.

À titre de référence, votre dernière page doit inclure un code similaire à celui-ci dans `<head>` de votre document d’HTML :

```html
<head>
    <script type="text/javascript">
    (function() {
        var didInit = false;
        function initMunchkin() {
            if(didInit === false) {
                didInit = true;
                Munchkin.init('CHANGE-ME');
            }
        }
        var s = document.createElement('script');
        s.type = 'text/javascript';
        s.async = true;
        s.src = '//munchkin.marketo.net/munchkin.js';
        s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
                initMunchkin();
            }
        };
        s.onload = initMunchkin;
        document.getElementsByTagName('head')[0].appendChild(s);
        })();
    </script>
    ...
</head>
```

## Comportement de Munchkin

Le comportement par défaut de Marketo Munchkin consiste à effectuer les opérations suivantes au chargement de la page :

1. Vérifiez si le navigateur actuel comporte un cookie Munchkin et créez-en un s’il n’y figure pas.
1. Envoyez un événement &quot;Visite de page web&quot; à l’instance Marketo désignée à l’aide des informations de la page et du navigateur actifs. Cette opération enregistre une activité sur l’enregistrement correspondant dans Marketo.
1. Envoyez l’événement &quot;Lien cliqué sur la page web&quot; pour tout clic de l’utilisateur sur les liens.

Le comportement de Munchkin peut être modifié en utilisant les [paramètres de configuration](configuration.md) de Munchkin, par exemple si un cookie est créé pour tous les prospects lors de leur visite sur la page avec le paramètre `cookieAnon` ou en modifiant le délai de clics avec le paramètre `clickTime`. L’envoi de l’activité Visite peut être désactivé en définissant le paramètre apiOnly sur true. Depuis la version 162 (août 2022), les clics sur les liens `tel` et `mailto` sont suivis en plus des liens `http/s`.

## Pistes connues et anonymes

Lors de la première visite d’une piste sur une page de votre domaine, un nouvel enregistrement de piste anonyme est créé dans Marketo. La clé primaire de cet enregistrement est le cookie Munchkin (`_mkto_trk`) créé dans le navigateur de l’utilisateur. Toutes les activités web suivantes sur ce navigateur sont enregistrées pour cet enregistrement anonyme. Pour être associé à un enregistrement connu dans Marketo, l’un des événements suivants doit se produire :

- Le prospect doit consulter une page trackée par Munchkin avec un paramètre `mkt_tok` dans la chaîne de requête à partir d’un lien de courrier électronique Marketo suivi.
- Le prospect doit remplir un formulaire Marketo.
- Un appel SOAP [syncLead](../soap-api/leads.md) ou REST [Associate Lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) doit être envoyé.

Une fois l’une de ces conditions remplie, le cookie et toutes les activités Web associées sont associés au prospect connu.

Un nouvel enregistrement anonyme d’activité web est créé pour chaque navigateur. Ainsi, si une piste se rend pour la première fois sur votre domaine à l’aide d’un nouvel ordinateur et/ou d’un nouveau navigateur, cette association doit se reproduire.

## Domaines

Munchkin crée et effectue le suivi des cookies individuels sur une base par domaine. Pour que le suivi des pistes connues se produise sur plusieurs domaines, un événement d’association de pistes doit se produire pour chaque domaine. Par exemple, si je contrôle deux domaines, `marketo.com` et `example.com`, et qu’une piste remplit un formulaire sur `marketo.com`, puis accède à `example.com` plus tard, son activité sur `marketo.com` est suivie sur un enregistrement de piste connu, mais son activité sur `example.com` est anonyme. Cependant, les pistes connues persistent dans les sous-domaines. Par conséquent, une piste connue sur `www.example.com` est également une piste connue sur `info.example.com`.

Dans le cas où votre domaine de niveau supérieur est composé de deux parties, telles que `.co.uk`, ajoutez un paramètre domainLevel à votre fragment de code Munchkin pour que le code soit correctement suivi. Voir [ici](configuration.md#domainlevel) pour plus de détails.

## Cookie

Le cookie Munchkin utilise la clé `_mkto_trk` et a une valeur suivant ce modèle :

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

ou

`id:561\-HYG\-937&token:_mch\-marketo.com\-97bf4361ef4433921a6da262e8df45a`

Les cookies Munchkin sont spécifiques à chaque domaine de second niveau, c’est-à-dire `example.com`. La durée de vie par défaut du cookie est de 2 ans (730 jours).

## Version bêta

Pour activer le canal bêta Munchkin pour vos landing pages, accédez au menu [Admin -> Treasure Chest](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) et activez le paramètre &quot;Munchkin Beta on Landing Pages&quot;. Ceci fournit de nouveaux fragments de code dans le **[!UICONTROL Admin]** ->  Le menu **[!UICONTROL Munchkin]** vous permet d’utiliser la version bêta sur des sites externes.

## Exclusion

Les visiteurs peuvent se désabonner du suivi Munchkin entièrement en ajoutant le paramètre `querystring` &quot;marketo_opt_out=true&quot; à l’URL dans leur navigateur. Lorsque Munchkin JavaScript détecte ce paramètre, il tente de définir un nouveau cookie &quot;mkto_opt_out&quot; avec la valeur `true`. Tous les autres cookies de suivi Marketo sont supprimés, aucun nouveau cookie n’est défini et aucune requête HTTP n’est effectuée par Munchkin lorsque ce paramètre est détecté.
