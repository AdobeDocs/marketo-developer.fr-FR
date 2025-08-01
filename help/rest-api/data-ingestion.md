---
title: Ingestion des données
feature: REST API, Dynamic Content
description: Utiliser des données avec des API Marketo.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 13%

---

# API Data Ingestion

L’API Data Ingestion est un service à haut volume, à faible latence et à haute disponibilité conçu pour gérer l’ingestion de grandes quantités de données de personnes et de données relatives aux personnes de manière efficace et avec un délai minimal.

Les données sont ingérées en soumettant des requêtes qui s’exécutent de manière asynchrone. Le statut de la requête peut être récupéré en s’abonnant à des événements du flux de données d’observabilité [](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup) de Marketo. &#x200B;

Les interfaces sont proposées pour deux types d&#39;objets : Personnes, Objets personnalisés. L’opération d’enregistrement est « insert or update » uniquement.

L’API Data Ingestion est actuellement en version bêta privée.  Les invités doivent disposer d’un droit pour le package [Marketo Engage Performance Tier](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Authentification

L’API Data Ingestion utilise la même méthode d’authentification OAuth 2.0 que l’API REST Marketo pour générer un jeton d’accès, mais ce dernier doit être transmis via l’en-tête HTTP `X-Mkto-User-Token`. Vous ne pouvez pas transmettre le jeton d’accès via un paramètre de requête.

Exemple de jeton d’accès via l’en-tête :

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Autorisations

Data Ingestion utilise le même modèle d’autorisations que l’API REST Marketo et ne nécessite aucune autorisation spéciale supplémentaire à utiliser, bien que des autorisations spécifiques soient requises pour chaque point d’entrée.

| Point d’entrée | Autorisation |
|-|-|
| Personnes | Lead en lecture/écriture |
| Objets personnalisés | Objet personnalisé accessible en lecture/écriture |

## En-têtes

L’ingestion de données utilise les en-têtes HTTP personnalisés suivants.

### Requête

| Clé | Valeur | Obligatoire | Description |
| - | - | - | - |
| X-Correlation-Id | Chaîne arbitraire (longueur maximale de 255 caractères). | Non | Peut être utilisé pour suivre les requêtes à travers le système.  Voir Flux de données d’observabilité de Marketo |
| X-Request-Source | Chaîne arbitraire (longueur maximale de 50 caractères). | Non | Peut être utilisé pour suivre la source des requêtes à travers le système.  Voir Flux de données d’observabilité de Marketo |

### Réponse

| Clé | Valeur | Obligatoire |
| - | - | - |
| X-Request-Id | ID de requête unique. | Oui |

## Demandes

Utilisez la méthode HTTP POST pour envoyer des données au serveur.

La représentation des données est incluse dans le corps de la requête sous la forme application/json.

Le nom de domaine est : `mkto-ingestion-api.adobe.io`

Le chemin commence par `/subscriptions/MunchkinId` où MunchkinId est spécifique à votre instance Marketo. Votre Munchkin ID figure dans l’interface utilisateur de Marketo Engage sous **Admin** > **Mon compte** > **Informations d’assistance**.  Le reste du chemin d’accès est utilisé pour spécifier la ressource qui vous intéresse.

Exemple d’URL pour les personnes :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Exemple d’URL pour les objets personnalisés :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

### Réponses

Toutes les réponses renvoient un identifiant de requête unique via l’en-tête `X-Request-Id`.

Exemple d’identifiant de requête via l’en-tête :

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Réussite

Lorsqu’un appel réussit, un statut 202 est renvoyé.  Aucun corps de réponse n’est renvoyé.

Exemple de réponse de succès :

```
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Erreur

Lorsqu’un appel génère une erreur, un statut non 202 est renvoyé avec un corps de réponse contenant des détails d’erreur supplémentaires.  Le corps de la réponse est application/json et contient un seul objet avec les membres error_code et message.

Vous trouverez ci-dessous les codes d’erreur réutilisés de la passerelle Adobe Developer.

| Code d’état HTTP | code_erreur | message |
| - | - | - |
| 401 | 401013 | Jeton Oauth non valide |
| 403 | 403010 | Jeton Oauth manquant |
| 404 | 404040 | Ressource introuvable |
| 429 | 429001 | Limite d’utilisation du service atteinte |

Vous trouverez ci-dessous les codes d’erreur propres à l’API Data Ingestion et composés de 3 segments.  Les trois premiers chiffres correspondent au statut (renvoyé par la passerelle Adobe Developer), suivis d’un zéro « 0 », suivi de trois chiffres.

| Code d’état HTTP | code_erreur | message |
| - | - | - |
| 400 | 4000801 | Requête incorrecte |
| 400 | 4000802 | Données non valides |
| 403 | 4030801 | Non autorisé |
| 429 | 4290801 | Quota quotidien atteint |
| 500 | 5000801 | Erreur interne du serveur |

## Reprises

Lorsqu’une erreur transitoire est détectée, le service tente à nouveau l’opération trois fois.  La première reprise se produit après une période d’attente de 5 minutes, la seconde après 30 minutes supplémentaires et la troisième après 30 minutes supplémentaires.  Les reprises se produisent pour diverses raisons, principalement lorsqu’un service dépendant expire ou n’est temporairement pas disponible.

## Points d’entrée

Les points d’entrée d’ingestion sont disponibles pour les personnes et les objets personnalisés.

### Personnes

Point d’entrée utilisé pour mettre à jour les enregistrements de personne.

| Méthode | Chemin |
| - | - |
| POST | /subscriptions/{munchkinId}/person |

#### En-têtes

| Clé | Valeur |
| - | - |
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| - | - | - | - | - |
| priorité | Chaîne | Non | Priorité de la requête : normale ou élevée | normal |
| partitionName | Chaîne | Non | Nom de la partition de la personne | Par défaut |
| dedupeFields | Objet | Non | Attributs à dédupliquer. Un ou deux noms d’attribut sont autorisés. <br/> Deux attributs sont utilisés dans une opération AND. Par exemple, si les opérateurs `email` et `firstName` sont spécifiés, ils sont tous deux utilisés pour rechercher une personne à l’aide de l’opération AND. <br/>Les attributs pris en charge sont les suivants : `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, Attributs personnalisés (type « chaîne » et « entier » uniquement), `email` |
| personnes | Tableau d’objets | Oui | Liste des paires nom-valeur d’attribut pour la personne | - |

Les autorisations requises sont `Read-Write Lead`.

### Exemple de personnes

#### Requête

`POST /subscriptions/{munchkinId}/persons`

#### En-têtes

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corps

```json
{
   "priority": "high",
   "partitionName": "EMEA",
   "dedupeFields": {
      "field1": "email",
      "field2": "firstName"
   },
   "persons":[
      {
         "email": "brooklyn.parker@karnv.com",
         "firstName": "Brooklyn",
         "lastName": "Parker"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal"
      }
   ]
}
```

#### Réponse

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Objets personnalisés

Point d&#39;entrée pour l&#39;enregistrement d&#39;objets personnalisés

| Méthode | Chemin |
| - | - |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### En-têtes

| Clé | Valeur |
| - | - |
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| - |- | - | - | - |
| priorité | Chaîne | Non | Priorité de la requête : normale, élevée | normal |
| dedupeBy | Chaîne | Non | Attributs à dédupliquer : dedupeFields, marketoGUID | dedupeFields |
| customObjects | Tableau d’objets | Oui | Liste des paires nom-valeur d’attribut pour l’objet. | – |

Les autorisations requises sont `Read-Write Custom Object`.

Si un champ de lien vers une Personne est spécifié dans la requête et que cette Personne n’existe pas, plusieurs reprises se produisent. Si cette personne est ajoutée pendant la fenêtre de nouvelle tentative (65 minutes), la mise à jour est réussie. Par exemple, si le champ Lien est e-mail sur Personne et que Personne n’existe pas, une nouvelle tentative a lieu.

### Exemple d’objets personnalisés

#### Requête

`POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}`


#### En-têtes

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corps

```json
{
   "dedupeBy": "dedupeFields",
   "priority": "high",
   "customObjects": [
      {
         "email": "brooklyn.parker@karnv.com",
         "vin": "20UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 330i",
         "year": 2003
      },
      {
         "email": "johnny.neal@yvu30.com",
         "vin": "19UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 325i",
         "year": 1989
      }
   ]
}
```

#### Réponse

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

## Limites

Voici une liste des mécanismes de sécurisation utilisés :

* Taille maximale de la requête : 1 Mo
* Nombre maximal d&#39;objets par demande et par type d&#39;objet : 1 000
* Nombre maximal de requêtes par seconde par ID client : 5 000
* Nombre maximal d’objets par jour : 10 000 000

## API Data Ingestion et API REST

Voici une liste des différences entre l’API Data Ingestion et les autres API REST Marketo :

* Il ne s’agit pas d’une interface CRUD complète, elle prend uniquement en charge l’upsert
* Pour vous authentifier, vous devez transmettre le jeton d’accès en utilisant l’en-tête `X-Mkto-User-Token`
* Le nom de domaine de l’URL est `mkto-ingestion-api.adobe.io`
* Le chemin de l’URL commence par `/subscriptions/MunchkinId`
* Aucun paramètre de requête
* Si l’appel réussit, un statut 202 est renvoyé et le corps de la réponse est vide
* Si l’appel échoue, un statut non-202 est renvoyé et le corps de la réponse contient `{ "error_code" : "Error Code", "message" : "Message" }`
* L’identifiant de la requête est renvoyé via l’en-tête `X-Request-Id`
