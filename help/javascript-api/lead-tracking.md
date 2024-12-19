---
title: Suivi du lead
description: API de suivi des leads
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: 8ad3e3f0958ea705375651b1c8a75967d807ca80
workflow-type: tm+mt
source-wordcount: '766'
ht-degree: 0%

---

# API de suivi des leads

Marketo Munchkin JavaScript permet de suivre les visites de pages et les clics des utilisateurs finaux sur vos pages de destination Marketo et vos pages web externes. Elles sont enregistrées dans Marketo en tant qu’activités « Visiter la page web » et « Lien cliqué sur la page web », qui peuvent ensuite être utilisées dans des déclencheurs et des filtres pour les campagnes et listes dynamiques.

## Incorporation du code

Votre instance Marketo fournit automatiquement des fragments de code de suivi préconfigurés pour incorporer du code sur vos pages externes qui effectuent le suivi de l’activité de votre instance Marketo. L’utilisation du code intégré est régie par le présent [contrat de licence](../munchkin-license.pdf).

Trois types de code de suivi sont disponibles :

1. Simple - Charge de manière synchrone
1. Asynchrone - Charge de manière asynchrone
1. Asynchrone jQuery : se charge de manière asynchrone et nécessite que jQuery soit chargé au préalable

Il est vivement recommandé d’utiliser le code de suivi asynchrone pour incorporer Munchkin dans des pages externes. Pour garantir le taux de réussite d’exécution le plus élevé possible, incorporez le code de suivi asynchrone dans la `<head>` de chaque page.

Certains systèmes de gestion de contenu peuvent avoir des méthodes spécifiques ou des restrictions lors de l’incorporation de scripts arbitraires.

À titre de référence, votre dernière page doit inclure un code similaire à celui-ci dans la `<head>` de votre document d’HTML :

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

1. Vérifiez si le navigateur actuel possède un cookie Munchkin et créez-en un s’il n’existe pas.
1. Envoyez un événement « Visiter la page web » à l’instance Marketo désignée à l’aide des informations de la page et du navigateur actifs. Cette opération enregistre une activité sur l’enregistrement correspondant dans Marketo.
1. Envoyez l’événement « Lien cliqué sur une page web » pour tout clic de l’utilisateur ou de l’utilisatrice sur les liens.

Le comportement de Munchkin peut être modifié à l’aide des paramètres Munchkin [Configuration](configuration.md), par exemple en indiquant si un cookie est créé pour tous les prospects qui visitent la page avec le paramètre `cookieAnon` ou en modifiant le délai de clic avec le paramètre `clickTime`. L’envoi de l’activité Visite peut être désactivé en définissant le paramètre apiOnly sur « true ». Depuis la version 162 (août 2022), les clics `tel` et les liens `mailto` sont suivis en plus des liens `http/s`.

## Leads connus et anonymes

Lors de la première visite d’un prospect sur une page de votre domaine, un nouvel enregistrement de prospect anonyme est créé dans Marketo. La clé primaire de cet enregistrement est le cookie Munchkin (`_mkto_trk`) qui est créé dans le navigateur de l’utilisateur. Toutes les activités web suivantes sur ce navigateur sont enregistrées par rapport à cet enregistrement anonyme. Pour être associé à un enregistrement connu dans Marketo, l’une des choses suivantes doit se produire :

- Le prospect doit se rendre sur une page suivie par Munchkin avec un paramètre `mkt_tok` dans la chaîne de requête à partir d’un lien e-mail Marketo suivi.
- Le prospect doit remplir un formulaire Marketo.
- Un appel REST [Associate Lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) doit être envoyé.

Une fois l’une de ces conditions remplie, le cookie et toute l’activité web associée sont associés au prospect connu.

Un nouvel enregistrement d’activité web anonyme est créé pour chaque navigateur individuel. Par conséquent, si un prospect visite votre domaine pour la première fois à l’aide d’un nouvel ordinateur et/ou d’un nouveau navigateur, cette association doit à nouveau avoir lieu.

## Domaines

Munchkin crée et suit des cookies individuels sur une base par domaine. De ce fait, pour que le suivi connu des prospects se produise sur plusieurs domaines, un événement d’association de prospects doit se produire pour chaque domaine. Par exemple, si je contrôle deux domaines, `marketo.com` et `example.com`, et qu’un prospect remplit un formulaire sur `marketo.com`, puis accède à `example.com` ultérieurement, son activité sur `marketo.com` est suivie sur un enregistrement de prospect connu, mais son activité sur `example.com` est anonyme. Les prospects connus persistent sur plusieurs sous-domaines. Par conséquent, un prospect connu sur `www.example.com` est également un prospect connu sur `info.example.com`.

Si votre domaine de niveau supérieur se compose de deux parties, comme `.co.uk`, ajoutez un paramètre domainLevel à votre fragment de code Munchkin pour que le code puisse effectuer le suivi correctement. Voir [ici](configuration.md#domainlevel) pour plus d’informations.

## Cookie

Le cookie Munchkin utilise la clé `_mkto_trk` et possède une valeur suivant ce modèle :

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

ou

`id:561\-HYG\-937&token:_mch\-marketo.com\-97bf4361ef4433921a6da262e8df45a`

Les cookies Munchkin sont spécifiques à chaque domaine de deuxième niveau, c’est-à-dire `example.com`. La durée de vie par défaut du cookie est de 2 ans (730 jours).

## Version bêta

Pour vous inscrire au canal bêta Munchkin pour vos pages de destination, accédez au menu [Admin -> Coffre au trésor](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) et activez le paramètre « Munchkin Beta sur les pages de destination ». Vous obtenez ainsi de nouveaux fragments de code dans le **[!UICONTROL Admin]** ->  Menu **[!UICONTROL Munchkin]** permettant d&#39;utiliser la version bêta sur des sites externes.

## Opt-Out

Les visiteurs peuvent se désinscrire entièrement du suivi Munchkin en ajoutant le paramètre `querystring` « marketo_opt_out=true » à l’URL dans leur navigateur. Lorsque le JavaScript Munchkin détecte ce paramètre, il tente de définir un nouveau cookie « mkto_opt_out » avec une valeur de `true`. Tous les autres cookies de suivi Marketo sont supprimés, aucun nouveau cookie n’est défini et aucune requête HTTP n’est effectuée par Munchkin lorsque ce paramètre est détecté.
