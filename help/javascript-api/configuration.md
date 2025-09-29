---
title: Configuration
description: Configurez Marketo Munchkin avec l’API JavaScript. Découvrez les paramètres Munchkin.init tels que altIds, anonymizeIP, asyncOnly, life des cookies, domainLevel, Beacon API.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 3%

---

# Configuration

Munchkin peut accepter différents paramètres de configuration pour personnaliser le comportement. Les paramètres de configuration sont les propriétés d&#39;un objet JavaScript transmis en tant que deuxième paramètre lors de l&#39;appel de [Munchkin.init()](api-reference.md#munchkin_init)

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

L’objet des paramètres de configuration peut contenir un nombre illimité de propriétés du tableau ci-dessous.

## Propriétés

| Nom | Type de données | Description |
|---|---|---|
| altIds | Tableau | Accepte un tableau de chaînes d’ID Munchkin. Lorsqu’elle est activée, cette option duplique toutes les activités web vers les abonnements ciblés, en fonction de leur identifiant Munchkin. |
| anonymizeIP | Booléen | Rend anonyme l’adresse IP enregistrée dans Marketo pour les nouveaux visiteurs. |
| apiOnly | Booléen | Si la valeur est définie sur true, `Munchkin.Init()` fonction n’appelle pas `visitsWebPage`. Cela s’avère utile pour les applications web monopages qui nécessitent un contrôle total sur chaque événement `visitsWebPage`. |
| asyncOnly | Booléen | Si la valeur est définie sur « true », envoie le de XMLHttpRequest de manière asynchrone. La valeur par défaut est false. |
| clickTime | Nombre entier | Définit la durée du blocage après un clic pour autoriser la requête de suivi des clics (en millisecondes). Cela réduit la précision du suivi des clics. La valeur par défaut est de 350 ms. |
| cookieAnon | Booléen | Si cette valeur est définie sur false, empêche le suivi et la création de cookies pour les nouveaux prospects anonymes. Les leads disposent de cookies et sont suivis après avoir rempli un formulaire Marketo ou en cliquant dessus à partir d’un e-mail Marketo. La valeur par défaut est « true ». |
| cookieLifeDays | Nombre entier | Définit la date d’expiration de tout nouveau cookie de suivi Munchkin sur ce nombre de jours à l’avenir. La valeur par défaut est de 730 jours (2 ans). |
| customName | Chaîne | Nom de page personnalisé. Utilisation du système uniquement. |
| <a name="domainlevel"></a>domainLevel | Nombre entier | Définit le nombre de parties du domaine de la page à utiliser lors de la définition de l’attribut de domaine du cookie. Par exemple, supposons que le domaine de la page active soit « www.example.com ».domainLevel : 2 définira l’attribut de domaine du cookie sur « .example.com »domainLevel : 3 définira l’attribut de domaine du cookie sur « .www.example.com »Background :Munchkin gérera automatiquement certains domaines de niveau supérieur à deux lettres. Il s’agit par défaut de deux parties dans les cas normaux où le domaine de niveau supérieur comporte trois lettres. Par exemple, « www.example.com », les deux parties les plus à droite sont utilisées pour définir le cookie « .example.com ». Pour deux codes de pays de lettre tels que « .jp », « .us », « .cn » et « .uk », le code se compose par défaut de trois parties. Par exemple, « www.example.co.jp » utilisera trois parties de domaine les plus à droite, « .example.co.jp ». Si le modèle de domaine nécessite un comportement différent, cela doit être spécifié à l’aide du paramètre `domainLevel`. |
| domainSelectorV2 | Booléen | Si la valeur est définie sur « true », utilise une méthode améliorée pour déterminer comment définir l’attribut de domaine du cookie. |
| httpsOnly | Booléen | La valeur par défaut est false. Lorsque la valeur est définie sur true, définit le cookie sur le paramètre Sécurisé lorsque la page suivie a été diffusée via https. |
| useBeaconAPI | Booléen | La valeur par défaut est false. Lorsque la valeur est définie sur true, utilise l’[API de balise](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) pour envoyer des requêtes non bloquantes au lieu de [XMLHttpRequest](https://developer.mozilla.org/fr-FR/docs/Web/API/XMLHttpRequest). Si le navigateur ne prend pas en charge cette API, le Munchkin revient à l’utilisation de XMLHttpRequest. |
| wsInfo | Chaîne | Prend une chaîne pour cibler un espace de travail. Cet identifiant d&#39;espace de travail est obtenu en sélectionnant le Workspace dans le menu Admin > Intégration > Munchkin . Ce paramètre s’applique uniquement à la création initiale d’un enregistrement de prospect anonyme. Une fois que la valeur du cookie Munchkin a été établie pour cet enregistrement de prospect, le paramètre wsInfo ne peut pas être utilisé pour modifier sa partition. Comme ce paramètre affecte uniquement les prospects anonymes, il ne concerne que les [visiteurs anonymes dans les rapports web](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports) spécifiques à une partition. |

## Exemples

### Envoyer l’activité à plusieurs abonnements

Cet exemple montre comment envoyer toute l’activité web aux instances dont les ID Munchkin sont « AAA-BBB-CCC » et « XXX-YYY-ZZZ ».

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCCC', { 'altIds': ['XXX-YYY-ZZZ'] });
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
```

### Définir le suivi sur Asynchrone

Cet exemple force l&#39;envoi asynchrone de tous les XMLHttpRequest à partir du thread principal.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCC', { 'asyncOnly': true });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin-beta.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```
