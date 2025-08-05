---
title: Référence de l’API Munchkin
description: Utilisez l’API JavaScript Munchkin pour personnaliser vos données Munchkin.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 9%

---

# Référence de l’API Munchkin

Munchkin fournit plusieurs fonctions qui peuvent être appelées manuellement via Javascript. Ils peuvent permettre un suivi personnalisé des événements de navigateur, tels que les lectures de vidéos, ou les clics sur des liens non pertinents.

## Fonctions

L’API Munchkin comprend les fonctions suivantes : `init`, `createTrackingCookie` et `munchkinFunction`.

<a name="munchkin_init"></a>

### Munchkin.init()

`Munchkin.init()` doit être appelé avant toute autre fonction. Il configure Munchkin sur la page active pour envoyer des activités à une instance spécifique et génère une activité « Page Web de visites » pour la page active.

| Nom du paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| ID Munchkin | Obligatoire | Chaîne | Identifiant de compte Munchkin sous le menu Admin > Intégration > Munchkin . Définit l’instance cible vers laquelle envoyer les activités. |
| [ Paramètres de configuration ](configuration.md) | Facultatif | Objet | Active d’autres paramètres de comportement pour Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

Lorsqu’elle est appelée, cette option vérifie qu’un cookie `_mkto_trk` existe dans le navigateur et, dans le cas contraire, en crée un. Cela s’avère utile pour le suivi des utilisateurs et utilisatrices lors d’actions spécifiques, telles que l’enregistrement ou le téléchargement d’une ressource, si `cookieAnon` est défini sur false.

| Nom du paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| forceCreate | Obligatoire | Booléen | Créez un cookie même si `cookieAnon` est défini sur false. |

```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Utilisé pour générer des comportements de suivi personnalisés, tels que les lectures et les pauses du lecteur vidéo, ou les visites de page pour une navigation non standard, comme les codes de hachage.

| Nom du paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| Type de fonction | Obligatoire | Chaîne | Détermine l’activité à enregistrer. Valeurs autorisées : `visitWebPage`, `clickLink`, `associateLead` |
| Données | Obligatoire | Objet | Contient les données de l’activité à enregistrer. |

#### visitWebPage

L’appel de `munchkinFunction()` avec `visitWebPage` envoie une activité de « visite » à Marketo pour l’utilisateur actuel. Vous pouvez personnaliser l’URL et les `querystring` qui sont envoyés avec l’objet de données dans le deuxième argument.

| Data Property Name | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| url | Obligatoire | Chaîne | Chemin d’accès au fichier URL utilisé pour enregistrer une visite de page.  Cette valeur est ajoutée au nom de domaine actuel pour créer un nom de page complet. Par exemple, si l’URL est `/index.html` et que le nom de domaine est `www.example.com`, la page visitée est enregistrée comme `www.example.com/index.html`. |
| Paramètres | Facultatif | Chaîne | Chaîne de requête des paramètres souhaités à enregistrer. |

Par exemple : `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

L’appel de `munchkinFunction()` avec `clickLink` envoie une activité de clic vers Marketo pour l’utilisateur actuel. Vous pouvez personnaliser l’URL de clic avec la propriété `href` dans l’objet de données.

| Data Property Name | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| href | Obligatoire | Chaîne | Chemin d’accès au fichier URL utilisé pour enregistrer un clic sur un lien. Cette valeur est ajoutée au nom de domaine actuel pour créer un lien complet. |

Par exemple, si href est `/index.html` et que le nom de domaine est `www.example.com`, le clic sur le lien est enregistré comme `www.example.com/index.html`.

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### associerLead

Cette méthode a été abandonnée et n’est plus disponible.
