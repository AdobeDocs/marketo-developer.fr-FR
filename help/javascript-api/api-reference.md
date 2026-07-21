---
title: Référence de l’API Munchkin
description: Utilisez l’API JavaScript Munchkin pour effectuer le suivi des visites de page, des clics sur les liens et des événements personnalisés à l’aide des méthodes init, createTrackingCookie et munchkinFunction.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
TQID: https://experienceleague.adobe.com/s97x6wVZijnnxZwS7HMIkQAKlxXkcfPXuSZG4KjXGoc
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 414
ht-degree: 9%

---

# Référence de l’API Munchkin

Munchkin fournit des fonctions JavaScript pour le suivi personnalisé des événements de navigateur. Vous pouvez, par exemple, effectuer le suivi des lectures vidéo ou des clics sur des éléments qui ne sont pas des liens.

## Fonctions

L’API Munchkin offre les fonctions suivantes :

- `init`
- `createTrackingCookie`
- `munchkinFunction`

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

`Munchkin.createTrackingCookie()` vérifie si un cookie `_mkto_trk` existe dans le navigateur. Si le cookie n’existe pas, la fonction en crée un.

Lorsque `cookieAnon` est défini sur false, utilisez cette fonction pour suivre les utilisateurs lors d’actions spécifiques, telles que l’enregistrement ou le téléchargement d’une ressource.

| Nom du paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| forceCreate | Obligatoire | Booléen | Créez un cookie même si `cookieAnon` est défini sur false. |

```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Utilisez des `Munchkin.munchkinFunction()` pour créer des comportements de suivi personnalisés. Par exemple, suivez l’activité du lecteur vidéo ou les visites de page à partir d’une navigation non standard telle que des modifications de hachage.

| Nom du paramètre | Facultatif/obligatoire | Type | Description |
| --- | --- | --- | --- |
| Type de fonction | Obligatoire | Chaîne | Détermine l’activité à enregistrer. Valeurs autorisées : `visitWebPage`, `clickLink`, `associateLead` |
| Données | Obligatoire | Objet | Contient les données de l’activité à enregistrer. |

#### visitWebPage

L’appel de `munchkinFunction()` avec `visitWebPage` envoie une activité de « visite » à Marketo pour l’utilisateur actuel. Utilisez l’objet de données dans le deuxième argument pour personnaliser l’URL et le `querystring`.

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

L’appel de `munchkinFunction()` avec `clickLink` envoie une activité de clic vers Marketo pour l’utilisateur actuel. Utilisez la propriété `href` dans l’objet de données pour personnaliser l’URL de clic.

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
