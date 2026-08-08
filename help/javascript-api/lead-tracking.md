---
title: Suivi du lead
description: Découvrez comment intégrer Marketo Munchkin JavaScript, suivre les visites et les clics, gérer les prospects connus ou anonymes, les cookies interdomaines et refuser les campagnes intelligentes.
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
TQID: https://experienceleague.adobe.com/nGUcLLgL9X7PBKf2E5IzppDj8e-SyEtxmkQaESd90mE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 718
ht-degree: 0%

---

# API de suivi des leads

Marketo Munchkin JavaScript effectue le suivi des visites de page et des clics sur les liens des pages de destination Marketo et des pages web externes. Marketo enregistre ces interactions en tant qu’activités « Visiter la page web » et « Lien cliqué sur la page web ».

Utilisez les activités dans les déclencheurs et les filtres pour les campagnes et listes dynamiques.

## Incorporation du code

Votre instance Marketo fournit des fragments de code préconfigurés pour le suivi de l’activité à partir de pages externes. L’utilisation du code intégré est régie par le présent [contrat de licence](../munchkin-license.pdf).

Trois types de code de suivi sont disponibles :

1. Simple : charge de manière synchrone.
1. Asynchrone : charge de manière asynchrone.
1. Asynchrone : charge jQuery de manière asynchrone et nécessite que jQuery charge d&#39;abord.

Utilisez le code de suivi asynchrone pour incorporer Munchkin dans des pages externes. Pour obtenir le taux de réussite d’exécution le plus élevé possible, placez le code dans l’élément `<head>` de chaque page.

Certains systèmes de gestion de contenu peuvent avoir des méthodes spécifiques ou des restrictions lors de l’incorporation de scripts arbitraires.

Votre dernière page doit inclure un code similaire à l’exemple suivant dans l’élément `<head>` du document HTML :

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

Par défaut, Marketo Munchkin effectue les actions suivantes lors du chargement d’une page :

1. Vérifie si le navigateur actif possède un cookie Munchkin et en crée un si nécessaire.
1. Envoie un événement « Visiter la page web » à l’instance Marketo désignée en utilisant les informations de la page et du navigateur actifs. Cet événement enregistre une activité sur l’enregistrement Marketo correspondant.
1. Envoie un événement « Lien cliqué sur une page web » lorsque l’utilisateur sélectionne un lien.

Utilisez Munchkin [Paramètres de configuration](configuration.md) pour modifier ce comportement. Par exemple, utilisez `cookieAnon` pour contrôler si Munchkin crée un cookie pour tous les prospects qui visitent la page, ou utilisez `clickTime` pour modifier le délai de clic.

Pour désactiver l’activité Visite, définissez `apiOnly` sur true. Depuis la version 162 (août 2022), Munchkin suit les clics sur les liens `tel` et `mailto` en plus des liens `http/s`.

## Leads connus et anonymes

Lorsqu’un prospect visite pour la première fois une page de votre domaine, Marketo crée un enregistrement de prospect anonyme. La clé primaire de cet enregistrement est le cookie Munchkin (`_mkto_trk`) créé dans le navigateur de l’utilisateur.

Marketo enregistre l’activité web ultérieure de ce navigateur dans l’enregistrement anonyme. Pour associer l’activité à un enregistrement Marketo connu, l’un des événements suivants doit se produire :

- Le prospect doit se rendre sur une page suivie par Munchkin avec un paramètre `mkt_tok` dans la chaîne de requête à partir d’un lien e-mail Marketo suivi.
- Le prospect doit remplir un formulaire Marketo.
- Un appel REST [Associate Lead](https://developer.adobe.com/marketo-apis/api/mapi#operation/associateLeadUsingPOST) doit être envoyé.

Lorsque l’un de ces événements se produit, Marketo associe le cookie et toute activité web associée au prospect connu.

Marketo crée un enregistrement d’activité web anonyme pour chaque navigateur. Si un prospect visite votre domaine à partir d’un nouvel ordinateur ou d’un nouveau navigateur, l’association doit se reproduire.

## Domaines

Munchkin crée et suit des cookies par domaine. Pour effectuer le suivi d’un prospect connu sur plusieurs domaines, un événement d’association de prospect doit se produire sur chaque domaine.

Supposons, par exemple, que vous contrôliez `marketo.com` et `example.com`. Un prospect envoie un formulaire le `marketo.com` et passe ensuite à l’`example.com`. L’activité sur `marketo.com` est associée au prospect connu, mais l’activité sur `example.com` est anonyme.

Les prospects connus persistent sur plusieurs sous-domaines. Un prospect connu sur `www.example.com` est également un prospect connu sur `info.example.com`.

Si votre domaine de niveau supérieur comporte deux parties, telles que `.co.uk`, ajoutez un paramètre `domainLevel` à votre fragment de code Munchkin. Pour plus d’informations, voir [Configuration](configuration.md#domainlevel).

## Cookie

Le cookie Munchkin utilise la clé `_mkto_trk` et une valeur suivant l’un de ces modèles :

`id:561-HYG-937&token:_mch-marketo.com-1374552656411-90718`

Ou

`id:561-HYG-937&token:_mch-marketo.com-97bf4361ef4433921a6da262e8df45a`

Les cookies Munchkin sont spécifiques à chaque domaine de deuxième niveau, par exemple `example.com`. La durée de vie par défaut des cookies est de 2 ans (730 jours).

## Beta

Pour vous inscrire au canal bêta Munchkin pour vos pages de destination, accédez à [Admin -> Coffre au trésor](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) et activez le paramètre « Munchkin Beta sur les pages de destination ».

Ce paramètre ajoute des fragments de code au menu **[!UICONTROL Admin]** -> **[!UICONTROL Munchkin]**. Utilisez ces fragments de code pour exécuter la version bêta sur des sites externes.

## Opt-Out

Les visiteurs peuvent se désinscrire du suivi Munchkin en ajoutant le paramètre `querystring` « marketo_opt_out=true » à l’URL dans leur navigateur. Lorsque Munchkin JavaScript détecte ce paramètre, il tente de définir un nouveau cookie « mkto_opt_out » avec une valeur de `true`.

Munchkin supprime ensuite tous les autres cookies de suivi Marketo, ne définit pas de nouveaux cookies et n’effectue pas de requêtes HTTP.
