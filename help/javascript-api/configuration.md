---
title: Configuration
description: Configurez Marketo Munchkin avec l’API JavaScript. Découvrez les paramètres Munchkin.init tels que altIds, anonymizeIP, asyncOnly, life des cookies, domainLevel, Beacon API.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
TQID: https://experienceleague.adobe.com/ip2cCGgoa83v8m9GYLYXe132veYxS1C6UWX1iLB6X5Q
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 541
ht-degree: 5%

---

# Configuration

Munchkin accepte les paramètres de configuration qui personnalisent son comportement. Transmettez les paramètres en tant que propriétés d’un objet JavaScript dans le deuxième paramètre de [Munchkin.init()](api-reference.md#munchkin_init).

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

L’objet des paramètres de configuration peut contenir un nombre indéfini de propriétés dans le tableau suivant.

## Propriétés

| Nom | Type de données | Description |
| --- | --- | --- |
| altIds | Tableau | Accepte un tableau de chaînes d’ID Munchkin. Lorsqu’elle est activée, cette option duplique toutes les activités web sur les abonnements identifiés par leurs identifiants Munchkin. |
| anonymizeIP | Booléen | Rend anonyme l’adresse IP enregistrée dans Marketo pour les nouveaux visiteurs. |
| apiOnly | Booléen | Si la valeur est définie sur true, `Munchkin.Init()` fonction n’appelle pas `visitsWebPage`. Cela s’avère utile pour les applications web monopages qui nécessitent un contrôle total sur chaque événement `visitsWebPage`. |
| asyncOnly | Booléen | Si la valeur est définie sur true, envoie XMLHttpRequests de manière asynchrone. La valeur par défaut est false. |
| clickTime | Nombre entier | Définit le temps, en millisecondes, à bloquer après un clic afin que la demande de suivi des clics puisse se terminer. La réduction de cette valeur réduit la précision du suivi des clics. La valeur par défaut est de 350 ms. |
| cookieAnon | Booléen | Si cette valeur est définie sur false, empêche le suivi et la création de cookies pour les nouveaux prospects anonymes. Les leads reçoivent des cookies et sont suivis après l’envoi d’un formulaire Marketo ou un clic sur à partir d’un e-mail Marketo. La valeur par défaut est « true ». |
| cookieLifeDays | Nombre entier | Définit la date d’expiration de tout nouveau cookie de suivi Munchkin sur ce nombre de jours à l’avenir. La valeur par défaut est de 730 jours (2 ans). |
| customName | Chaîne | Nom de page personnalisé. Utilisation du système uniquement. |
| <a name="domainlevel"></a>domainLevel | Nombre entier | Définit le nombre de parties du domaine de la page à utiliser pour l’attribut de domaine du cookie.<br><br> Pour « www.example.com », `domainLevel: 2` définit le domaine du cookie sur « .example.com » et `domainLevel: 3` le définit sur « .www.example.com ».<br><br>Par défaut, Munchkin utilise deux parties lorsque le domaine de niveau supérieur comporte trois lettres. Par exemple, « www.example.com » utilise « .example.com ».<br><br>Pour les codes de pays à deux lettres tels que « .jp », « .us », « .cn » et « .uk », Munchkin utilise trois parties. Par exemple, « www.example.co.jp » utilise « .example.co.jp ».<br><br>Utilisez le paramètre `domainLevel` lorsque le modèle de domaine nécessite un comportement différent. |
| domainSelectorV2 | Booléen | Si la valeur est définie sur « true », utilise une méthode améliorée pour déterminer comment définir l’attribut de domaine du cookie. |
| httpsOnly | Booléen | La valeur par défaut est false. Lorsque la valeur est définie sur true, définit le cookie sur le paramètre Sécurisé lorsque la page suivie a été diffusée via https. |
| useBeaconAPI | Booléen | La valeur par défaut est false. Lorsque la valeur est définie sur true, utilise l’[API de balise](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) pour envoyer des requêtes non bloquantes au lieu de [XMLHttpRequest](https://developer.mozilla.org/fr-FR/docs/Web/API/XMLHttpRequest). Si le navigateur ne prend pas en charge l’API de balise, Munchkin utilise XMLHttpRequest. |
| wsInfo | Chaîne | Cible un espace de travail. Obtenez l’identifiant de l’espace de travail en le sélectionnant dans le menu Admin > Intégration > Munchkin .<br><br>Ce paramètre s’applique uniquement lorsqu’un enregistrement de prospect anonyme est initialement créé. Une fois la valeur du cookie Munchkin établie pour cet enregistrement de prospect, le paramètre wsInfo ne peut plus modifier sa partition.<br><br>Ce paramètre affecte uniquement les prospects anonymes. Il n’est donc pertinent que pour les [visiteurs anonymes dans les rapports web](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports) spécifiques à une partition. |

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

Cet exemple force l&#39;envoi asynchrone de toutes les XMLHttpRequests à partir du thread principal.

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
