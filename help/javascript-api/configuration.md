---
title: Configuration
description: API de configuration
feature: Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 3%

---

# Configuration

Munchkin peut accepter divers paramètres de configuration pour personnaliser le comportement. Les paramètres de configuration sont des propriétés d’un objet JavaScript transmis en tant que deuxième paramètre lors de l’appel de [Munchkin.init()](lead-tracking.md#munchkin-behavior)

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

L’objet settings de configuration peut contenir un nombre indéfini de propriétés dans le tableau ci-dessous.

## Propriétés

| Nom | Type de données | Description |
|---|---|---|
| altIds | Tableau | Accepte un tableau de chaînes Munchkin ID. Lorsqu’elle est activée, cette option duplique toutes les activités web aux abonnements ciblés, en fonction de leur Munchkin ID. |
| anonymizeIP | Booléenne | Anonyme de l’adresse IP enregistrée dans Marketo pour les nouveaux visiteurs. Vous pouvez déterminer si votre abonnement est configuré avec Munchkin V2 en vérifiant si votre domaine `{Munchkin-Id}.mktoresp.com` possède l’une des adresses suivantes : `192.28.144.124` `134.213.193.62` `192.28.147.68` `103.237.104.82`. Vous pouvez également exécuter le script ci-dessous à partir d’un shell unix : nslookup {munchkin-id}.mktoresp.com | grep -E -c -e &quot;(192.28.144.124,134.213.193.62,192.28.147.68,103.237.104.82)&quot; Si la commande génère &quot;0&quot;, votre abonnement n’est pas fourni avec Munchkin V2 ; si elle génère 1 ou plus, elle est configurée. |
| apiOnly | Booléenne | Si la valeur est définie sur true, la fonction `Munchkin.Init()` n’appellera pas `visitsWebPage`. Ceci est utile pour les applications web d’une seule page qui ont besoin d’un contrôle total sur chaque événement `visitsWebPage`. |
| asyncOnly | Booléenne | S’il est défini sur true, envoie le fichier XMLHttpRequest de manière asynchrone. La valeur par défaut est false. |
| clickTime | Entier | Définit le délai à bloquer après un clic pour autoriser la demande de suivi des clics (en millisecondes). Cette réduction réduit la précision du suivi des clics. La valeur par défaut est de 350 ms. |
| cookieAnon | Booléenne | S’il est défini sur false, empêche le suivi et la création de cookies de nouvelles pistes anonymes. Les pistes comportent des cookies et sont suivies après le remplissage d’un formulaire Marketo ou en cliquant dans un message électronique Marketo. La valeur par défaut est true. |
| cookieLifeDays | Entier | Définit la date d’expiration de tout nouveau cookie de suivi Munchkin sur ce nombre de jours dans le futur. La valeur par défaut est de 730 jours (2 ans). |
| customName | Chaîne | Nom de page personnalisé. Utilisation du système uniquement. |
| domainLevel | Entier | Définit le nombre de parties du domaine de la page à utiliser lors de la définition de l’attribut domain du cookie. Supposons, par exemple, que le domaine de la page active soit &quot;www.example.com&quot;.domainLevel: 2 définisse l’attribut domain du cookie sur &quot;.example.com&quot;domainLevel: 3 définira l’attribut domain du cookie sur &quot;.www.example.com&quot;Background:Munchkin gèrera automatiquement certains domaines de niveau supérieur à deux lettres. Par défaut, il s’agit de deux parties dans les cas normaux où le domaine de niveau supérieur comporte trois lettres. Par exemple &quot;www.example.com&quot;, les deux parties les plus à droite sont utilisées pour définir le cookie &quot;.example.com&quot;. Pour les codes de pays à deux lettres tels que &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot; et &quot;.uk&quot;, le code est défini par défaut sur trois parties. Par exemple, &quot;www.example.co.jp&quot; utilisera trois parties de domaine les plus à droite, &quot;.example.co.jp&quot;. Si le modèle de domaine nécessite un comportement différent, cela doit être spécifié à l’aide du paramètre `domainLevel` . |
| domainSelectorV2 | Booléenne | S’il est défini sur true, utilise une méthode améliorée pour déterminer comment définir l’attribut de domaine du cookie. |
| httpsOnly | Booléenne | La valeur par défaut est false. Lorsqu’elle est définie sur true, définit le cookie pour utiliser le paramètre Sécurisé lorsque la page trackée a été diffusée via https. |
| useBeaconAPI | Booléenne | La valeur par défaut est false. Lorsqu’elle est définie sur true, utilise l’API Beacon pour envoyer des requêtes non bloquantes au lieu de XMLHttpRequest. Si le navigateur ne prend pas en charge cette API, le Munchkin revient à l’utilisation de XMLHttpRequest. |
| wsInfo | Chaîne | Prend une chaîne pour cibler un espace de travail. Pour obtenir cet identifiant d’espace de travail, sélectionnez Workspace dans le menu Admin > Intégration > Munchkin . Ce paramètre s’applique uniquement à la création initiale d’un enregistrement de piste anonyme. Une fois la valeur du cookie Munchkin établie pour cet enregistrement de piste, le paramètre wsInfo ne peut pas être utilisé pour modifier sa partition. Puisque ce paramètre n’affecte que les pistes anonymes, il s’applique uniquement aux visiteurs anonymes spécifiques à la partition dans les rapports Web. |

## Exemples

### Envoyer l’activité à plusieurs abonnements

Cet exemple envoie toutes les activités web aux instances avec les identifiants Munchkin &quot;AAA-BBB-CC&quot; et &quot;XXX-YYY-ZZ&quot;.

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

Cet exemple force l’envoi asynchrone de tous les XMLHttpRequest à partir du thread principal.

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
