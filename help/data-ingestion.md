---
title: "Data Ingestion"
description: "Présentation de l’API Data Ingestion"
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 11%

---


# d’ingestion de données

L’API Data Ingestion est un service à volume élevé, à faible latence et hautement disponible, conçu pour gérer efficacement l’ingestion de grandes quantités de données liées aux personnes et aux personnes, avec un minimum de délais. 

Les données sont ingérées en envoyant des requêtes qui s’exécutent de manière asynchrone. L’état de la requête peut être récupéré en s’abonnant aux événements à partir du [Flux de données d’observabilité Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup/)&#x200B;

Les interfaces sont proposées pour deux types d&#39;objets : Personnes, Objets personnalisés. L&#39;opération d&#39;enregistrement est &quot;insert or update&quot; uniquement.

L’API Data Ingestion est en version bêta privée. Les invités doivent disposer d’un droit pour [Package de niveau de performance Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Authentification

L’API Data Ingestion utilise la même méthode d’authentification OAuth 2.0 que l’API REST Marketo pour générer un jeton d’accès, mais le jeton d’accès doit être transmis via l’en-tête HTTP. `X-Mkto-User-Token`. Vous ne pouvez pas transmettre le jeton d’accès via un paramètre de requête.

Exemple de jeton d’accès via l’en-tête :

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Permissions

Data Ingestion utilise le même modèle d’autorisations que l’API REST Marketo et ne nécessite aucune autorisation spéciale supplémentaire à utiliser, bien que des autorisations spécifiques soient requises pour chaque point de terminaison.

| Point de terminaison | Autorisation |
|---|---|
| Personnes | Lead en lecture/écriture |
| Objets personnalisés | Objet personnalisé accessible en lecture/écriture |

## En-têtes

Data Ingestion utilise les en-têtes HTTP personnalisés suivants.

### Demande

| Code  | Valeur | Requis | Description |
|---|---|---|---|
| X-Correlation-Id | Chaîne arbitraire (longueur maximale : 255 caractères). | Non | Peut être utilisé pour suivre les requêtes par le biais du système. Voir Flux de données d’observabilité Marketo |
| X-Request-Source | Chaîne arbitraire (longueur maximale de 50 caractères). | Non | Peut être utilisé pour tracer la source des requêtes par le biais du système. Voir Flux de données d’observabilité Marketo |

### Réponse

| Code  | Valeur | Requis | Description |
|---|---|---|---|
| X-Request-Id | ID de requête unique. | Oui | |

## Demandes

Utilisez la méthode de POST HTTP pour envoyer des données au serveur.

La représentation des données est incluse dans le corps de la requête sous la forme application/json.

Le nom de domaine est : `mkto-ingestion-api.adobe.io`

Le chemin commence par `/subscriptions/_MunchkinId_` where `_MunchkinId_` est spécifique à votre instance Marketo. Votre Munchkin ID se trouve dans l’interface utilisateur du Marketo Engage sous **Administration** >**Mon compte** > **Informations sur l’assistance**. Le reste du chemin est utilisé pour spécifier la ressource intéressante.

Exemple d’URL pour les personnes :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Exemple d’URL pour les objets personnalisés :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

## Réponses

Toutes les réponses renvoient un identifiant de requête unique via la variable `X-Request-Id` en-tête .

Exemple d’identifiant de requête via l’en-tête :

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Succès

Lorsqu’un appel aboutit, un état 202 est renvoyé. Aucun corps de réponse n’est renvoyé.

Exemple de réponse de succès :

`HTTP/1.1 202 Accepted` `X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598` `Content-Length: 0` `Date: Wed, 18 Oct 2023 18:56:49 GMT`

### Erreur

Lorsqu’un appel génère une erreur, un état non-202 est renvoyé avec un corps de réponse avec des détails d’erreur supplémentaires. Le corps de la réponse est application/json et contient un seul objet avec des membres. `error_code` et `message`.

Vous trouverez ci-dessous les codes d’erreur réutilisés de la passerelle Adobe Developer.

| Code d’état HTTP | error_code | message |
|--- |--- |--- |
| 401 | 401013 | Jeton Oauth non valide |
| 403 | 403010 | Jeton Oauth manquant |
| 404 | 404040 | Ressource introuvable |
| 429 | 429001 | Limite d’utilisation du service atteinte |

Vous trouverez ci-dessous des codes d’erreur propres à l’API Data Ingestion et composés de trois segments. Les trois premiers chiffres sont l’état (renvoyé par la passerelle Adobe IO), suivi d’un zéro &quot;0&quot;, suivi de trois chiffres.

| Code d’état HTTP | error_code | message |
|--- |--- |--- |
| 400 | 4000801 | Requête incorrecte |
| 400 | 4000802 | Données non valides |
| 403 | 4030801 | Non autorisé |
| 429 | 4290801 | Quota quotidien atteint |
| 500 | 5000801 | Erreur interne du serveur |

Exemple de réponse d’erreur :

`HTTP/1.1 403 Forbidden` `Content-Type: application/json` `Content-Length: 54` `Date: Wed, 18 Oct 2023 19:10:07 GMT` `X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw` `{"error_code":"403010","message":"Oauth token is missing"}`

## Reprises

Lorsqu’une erreur transitoire est détectée, le service effectue une nouvelle tentative de l’opération trois fois. La première reprise se produit après une période d’attente de 5 minutes, la seconde après 30 minutes supplémentaires, et enfin la troisième après 30 minutes supplémentaires. Les reprises se produisent pour diverses raisons, principalement lorsqu’un service dépendant expire ou n’est temporairement pas disponible.

## Points de fin

Les points de fin d’ingestion sont disponibles pour les personnes et les objets personnalisés.

### Personnes

Point de terminaison utilisé pour insérer des enregistrements de personne.

| Méthode |
|---|
| POST |

| Chemin d’accès |
|---|
| `/subscriptions/{munchkinId}/persons` |

| HeadersKey | Valeur |
|---|---|
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

Corps de requête

| Code  | Type de données | Requis | Valeur | Valeur par défaut |
|---|---|---|---|---|
| priorité | Chaîne | Non | Priorité de la requête : normalhigh | normal |
| partitionName | Chaîne | Non | Nom de la partition de la personne | Par défaut |
| dedupeFields | Objet | Non | Attributs à dédupliquer. Un ou deux noms d’attribut sont autorisés. Deux attributs sont utilisés dans une opération ET. Par exemple, si les deux `email` et `firstName` sont spécifiées. Elles peuvent toutes deux être utilisées pour rechercher une personne à l’aide de l’opération ET. Les attributs pris en charge sont les suivants :`idemail`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId`, `sfdcLeadOwnerIdCustom` Attributs (type &quot;string&quot; et &quot;integer&quot; uniquement) | E-mail |
| personnes | Tableau d’objets | Oui | Liste des paires nom-valeur d’attribut pour la personne | - |

| Autorisation |
|---|
| Lead en lecture/écriture |

#### Exemple de personnes

```
POST /subscriptions/{munchkinId}/persons
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

### Objets personnalisés

Point de terminaison utilisé pour insérer des enregistrements d’objets personnalisés.

| Méthode |
|---|
| POST |

| Chemin d’accès |
|---|
| `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

En-têtes

| Code  | Valeur |
|---|---|
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

Corps de la requête

| Code  | Type de données | Requis | Valeur | Valeur par défaut |
|---|---|---|---|---|
| priorité | Chaîne | Non | Priorité de la requête : normalhigh | normal |
| dedupeBy | Chaîne | Non | Attributs à dédupliquer sur:dedupeFieldsmarketoGUID | dedupeFields |
| customObjects | Tableau d’objets | Oui | Liste des paires nom-valeur d’attribut pour l’objet. | - |

| Autorisation |
|---|
| Objet personnalisé accessible en lecture/écriture |

#### Personne non présente

Si un champ de lien vers une Personne est spécifié dans la requête et que cette Personne n’existe pas, plusieurs reprises se produisent. Si cette Personne est ajoutée pendant la fenêtre de reprise (65 minutes), la mise à jour est réussie. Par exemple, si le champ de lien est `email` sur Personne et Personne n’existe pas, des reprises se produisent.

#### Exemple d’objets personnalisés

```
POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

## Limites

Voici une liste de l’utilisation des barrières de sécurité :

- Taille maximale de la requête : 1 Mo
- Nombre maximal d’objets par requête par type d’objet : 1 000
- Nombre maximal de demandes par seconde par ID client : 5 000
- Objets maximum par jour : 10 000 000

## API Data Ingestion et API REST

Voici une liste des différences entre l’API Data Ingestion et d’autres API REST Marketo :

- Il ne s’agit pas d’une interface CRUD complète, elle prend uniquement en charge &quot;upsert&quot;.
- Pour vous authentifier, vous devez transmettre le jeton d’accès à l’aide de la variable `X-Mkto-User-Token` header
- Le nom de domaine de l’URL est `mkto-ingestion-api.adobe.io`
- Le chemin de l’URL commence par `/subscriptions/_MunchkinId_`
- Il n’existe aucun paramètre de requête
- Si l’appel aboutit, un état 202 est renvoyé et le corps de la réponse est vide.
- Si l’appel échoue, un état non-202 est renvoyé et le corps de la réponse contient `{ "error_code" : "_Error Code_", "message" : "_Message_" }`
- L’ID de demande est renvoyé par `X-Request-Id` header
