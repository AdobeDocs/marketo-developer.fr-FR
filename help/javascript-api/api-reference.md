---
title: Référence de l’API Munchkin
description: Utilisez l’API JavaScript Munchkin pour personnaliser vos données Munchkin.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
source-git-commit: 1ad2d793832d882bb32ebf7ef1ecd4148a6ef8d5
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 9%

---

# Référence de l’API Munchkin

Munchkin fournit plusieurs fonctions qui peuvent être appelées manuellement via JavaScript. Ils peuvent permettre un suivi personnalisé des événements du navigateur, tels que les lectures de vidéo ou les clics sur des liens autres que les liens.

## Fonctions

L’API Munchkin comprend les fonctions suivantes : `init`, `createTrackingCookie`, `munchkinFunction`.

<a name="munchkin_init"></a>

### Munchkin.init()

`Munchkin.init()` doit être appelé avant toute autre fonction. Il configure Munchkin sur la page en cours pour envoyer les activités vers une instance spécifique et génère une activité &quot;Page Web des visites&quot; pour la page en cours.

| Nom du paramètre | Facultatif/Obligatoire | Type | Description |
| --- | --- | --- | --- |
| ID Munchkin | Obligatoire | Chaîne | Identifiant de compte Munchkin situé sous Admin > Intégration > Menu Munchkin. Définit l’instance cible à laquelle envoyer les activités. |
| [Paramètres de configuration](configuration.md) | Facultatif | Objet | Permet d’activer d’autres paramètres de comportement pour Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

Lorsqu’elle est appelée, cette vérification vérifie qu’un cookie `_mkto_trk` existe dans le navigateur et, dans le cas contraire, en crée un. Cela s’avère utile pour effectuer le suivi des utilisateurs lors d’actions spécifiques, telles que l’enregistrement ou le téléchargement d’une ressource, si `cookieAnon` est défini sur false.

| Nom du paramètre | Facultatif/Obligatoire | Type | Description |
| --- | --- | --- | --- |
| forceCreate | Obligatoire | Booléen | Créez un cookie même si `cookieAnon` est défini sur false. |


```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Utilisé pour générer des comportements de suivi personnalisés, tels que les lectures et pauses du lecteur vidéo, ou les visites de pages pour une navigation non standard, comme les codes de hachage.

| Nom du paramètre | Facultatif/Obligatoire | Type | Description |
| --- | --- | --- | --- |
| Type de fonction | Obligatoire | Chaîne | Détermine l’activité à enregistrer. Valeurs valides : `visitWebPage`, `clickLink`, `associateLead` |
| Données | Obligatoire | Objet | Contient les données de l’activité à enregistrer. |

#### visitWebPage

L’appel de `munchkinFunction()` avec `visitWebPage` envoie une activité &quot;visite&quot; à Marketo pour l’utilisateur actuel. Vous pouvez personnaliser l’URL et les `querystring` qui sont envoyés avec l’objet de données dans le deuxième argument.

| Nom de la propriété de données | Facultatif/Obligatoire | Type | Description |
| --- | --- | --- | --- |
| url | Obligatoire | Chaîne | Chemin d’accès au fichier URL utilisé pour enregistrer la visite d’une page.  Cette valeur est ajoutée au nom de domaine actuel pour créer un nom de page complet. Par exemple, si l’URL est `/index.html` et que le nom de domaine est `www.example.com`, la page visitée est enregistrée comme `www.example.com/index.html`. |
| params | Facultatif | Chaîne | Chaîne de requête des paramètres à enregistrer. |

Par exemple : `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

L’appel de `munchkinFunction()` avec `clickLink` envoie une activité de clic à Marketo pour l’utilisateur actuel. Vous pouvez personnaliser l’URL de clic avec la propriété `href` dans l’objet de données.

| Nom de la propriété de données | Facultatif/Obligatoire | Type | Description |
| --- | --- | --- | --- |
| href | Obligatoire | Chaîne | Chemin d’accès au fichier URL utilisé pour enregistrer un clic sur les liens. Cette valeur est ajoutée au nom de domaine actuel pour créer un lien complet. |

Par exemple, si href est `/index.html` et que nom de domaine est `www.example.com`, le clic sur le lien est enregistré comme `www.example.com/index.html`.

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### associerLead

Cette méthode est obsolète et n’est plus disponible.
