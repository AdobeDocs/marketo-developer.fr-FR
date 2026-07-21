---
title: Ingestion de données
feature: REST API, Dynamic Content, Static Lists
description: Utilisez l’API Marketo Data Ingestion pour une ingestion de volume élevé et à faible latence de personnes, d’objets personnalisés, d’entreprises, de membres de programme et de listes.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
TQID: https://experienceleague.adobe.com/xby7hs-CSLrVzy-FXEBi1FeU1-ca7vI4kB85BYJ9snk
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2151
ht-degree: 17%

---

# API Data Ingestion

L’API Data Ingestion est un service à volume élevé, à faible latence et à haute disponibilité. Utilisez-le pour ingérer de grandes quantités de données relatives aux personnes et aux personnes dans un délai minimal.

Les requêtes d’ingestion de données s’exécutent de manière asynchrone. Pour récupérer le statut de la requête, abonnez-vous aux événements du flux de données d’observabilité [&#128279;](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup).

L’API fournit des interfaces pour cinq types d’objets :

- Les personnes, les objets personnalisés et les sociétés prennent en charge les opérations « insertion ou mise à jour ».
- Les membres de programme prennent en charge les opérations d’insertion ou de mise à jour et de suppression.
- Les listes (listes statiques) prennent en charge les opérations d’ajout et de suppression.

Lisez la [Documentation de l’API Data Ingestion](https://developer.adobe.com/marketo-apis/api/data-ingestion).

>[!NOTE]
>
>L’accès à l’API Data Ingestion nécessite l’octroi du droit d’accès au package [Marketo Engage Performance Tier](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Authentification

L’API Data Ingestion utilise la même méthode d’authentification OAuth 2.0 que l’API REST Marketo pour générer un jeton d’accès. Transmettez le jeton d’accès dans l’en-tête HTTP `X-Mkto-User-Token`. Vous ne pouvez pas le transmettre en tant que paramètre de requête.

L’exemple suivant transmet un jeton d’accès dans l’en-tête :

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Autorisations

L’ingestion de données utilise le modèle d’autorisations de l’API REST Marketo et ne nécessite pas d’autorisations supplémentaires. Chaque point d’entrée nécessite une autorisation existante spécifique, comme illustré dans le tableau suivant.

| Point d’entrée | Autorisation |
| --- | --- |
| Personnes | Lead en lecture/écriture |
| Objets personnalisés | Objet personnalisé accessible en lecture/écriture |
| Sociétés | Société accessible en lecture/écriture |
| Membres du programme | Lead en lecture/écriture |
| Listes | Lead en lecture/écriture |

## Types d’objet pris en charge

| Type d&#39;objet | Opérations prises en charge |
| --- | --- |
| Personnes | Upsert (insertion ou mise à jour) |
| Objets personnalisés | Upsert (insertion ou mise à jour) |
| Sociétés | Synchronisation (`createOnly`, `updateOnly`, `createOrUpdate`) |
| Membres du programme | Synchroniser (upsert status), Supprimer (supprimer du programme) |
| Listes | Ajouter à la liste, Supprimer de la liste |

## En-têtes

L’ingestion de données prend en charge les en-têtes HTTP personnalisés suivants.

### Requête

| Clé | Valeur | Obligatoire | Description |
| --- | --- | --- | --- |
| `X-Correlation-Id` | Chaîne arbitraire (longueur maximale de 255 caractères). | Non | Peut être utilisé pour suivre les requêtes à travers le système. Voir Flux de données d’observabilité de Marketo |
| `X-Request-Source` | Chaîne arbitraire (longueur maximale de 50 caractères). | Non | Peut être utilisé pour suivre la source des requêtes à travers le système. Voir Flux de données d’observabilité de Marketo |

### Réponse

| Clé | Valeur | Obligatoire |
| --- | --- | --- |
| `X-Request-Id` | ID de requête unique. | Oui |

## Demandes

Envoyez des données au serveur à l’aide de la méthode HTTP POST.

Incluez les données dans le corps de la requête en tant qu’application/json.

Utilisez l’`mkto-ingestion-api.adobe.io` de domaine .

Le chemin commence par `/subscriptions/MunchkinId`, où MunchkinId est spécifique à votre instance Marketo. Recherchez votre Munchkin ID dans l’interface utilisateur de Marketo Engage sous **Admin** > **Mon compte** > **Informations d’assistance**. Le reste du chemin d’accès spécifie la ressource.

Exemple d’URL pour les personnes :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

Exemple d’URL pour les objets personnalisés :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

Exemple d’URL pour les sociétés :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/companies`

Exemple d’URL pour les membres du programme :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/programmembers`

Exemple d’URL pour les listes :

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/lists`

### Réponses

Chaque réponse renvoie un identifiant de requête unique dans l’en-tête `X-Request-Id`.

Exemple d’identifiant de requête via l’en-tête :

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Réussite

Un appel réussi renvoie le statut 202 et aucun corps de réponse.

Exemple de réponse de succès :

```http
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Erreur

Lorsqu’un appel échoue, il renvoie un statut non-202 et un corps de réponse avec des détails d’erreur. Le corps de la réponse `application/json` contient un objet avec des membres `error_code` et `message`.

Les codes d’erreur suivants sont réutilisés à partir de la passerelle Adobe Developer.

| Code d’état HTTP | code_erreur | message |
| --- | --- | --- |
| 401 | 401013 | Jeton Oauth non valide |
| 403 | 403010 | Jeton Oauth manquant |
| 404 | 404040 | Ressource introuvable |
| 429 | 429001 | Limite d’utilisation du service atteinte |

Les codes d’erreur spécifiques à l’API Data Ingestion contiennent trois segments : le statut à trois chiffres renvoyé par la passerelle Adobe Developer, un zéro « 0 » et trois chiffres supplémentaires.

| Code d’état HTTP | code_erreur | message |
| --- | --- | --- |
| 400 | 4000801 | Requête incorrecte |
| 400 | 4000802 | Données non valides |
| 403 | 4030801 | Non autorisé |
| 429 | 4290801 | Quota quotidien atteint |
| 500 | 5000801 | Erreur interne du serveur |

## Reprises

Lorsque le service détecte une erreur transitoire, il tente à nouveau l’opération. Une nouvelle tentative se produit principalement lorsqu’un service dépendant expire ou est temporairement indisponible.

Le service utilise les intervalles de reprise suivants :

- Opération initiale à effectuer pour la première fois : 5 minutes
- Première reprise et deuxième reprise : 15 minutes
- Deuxième reprise à la troisième reprise : 20 minutes
- Troisième reprise à la quatrième reprise : 20 minutes
- Quatrième reprise à la cinquième reprise : 2 heures
- Après la cinquième reprise : 3 heures

## Points d’entrée

Les points d’entrée d’ingestion sont disponibles pour les personnes, les objets personnalisés, les entreprises, les membres de programme et les listes. Chaque section de point d’entrée définit la requête et fournit un exemple.

### Personnes

Utilisez ce point d’entrée pour ajouter des enregistrements de personne.

| Méthode | Chemin |
| --- | --- |
| POST | /subscriptions/{munchkinId}/person |

#### En-têtes

| Clé | Valeur |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| --- | --- | --- | --- | --- |
| `priority` | Chaîne | Non | Priorité de la requête : normale ou élevée | normal |
| `partitionName` | Chaîne | Non | Nom de la partition de la personne | Par défaut |
| `dedupeFields` | Objet | Non | Attributs à dédupliquer. Un ou deux noms d’attribut sont autorisés. <br/> Deux attributs sont utilisés dans une opération AND. Par exemple, si les opérateurs `email` et `firstName` sont spécifiés, ils sont tous deux utilisés pour rechercher une personne à l’aide de l’opération AND. <br/>Les attributs pris en charge sont les suivants : `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, Attributs personnalisés (type « chaîne » et « entier » uniquement), `email` |  |
| `persons` | Tableau d’objets | Oui | Liste des paires nom-valeur d’attribut pour la personne | – |

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
         "lastName": "Parker",
         "company": "Karnv"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal",
         "company": "Acme Inc"
      }
   ]
}
```

#### Réponse

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Objets personnalisés

Utilisez ce point d’entrée pour mettre à jour les enregistrements d’objet personnalisés.

| Méthode | Chemin |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### En-têtes

| Clé | Valeur |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| --- | --- | --- | --- | --- |
| `priority` | Chaîne | Non | Priorité de la requête : normale, élevée | normal |
| `dedupeBy` | Chaîne | Non | Attributs à dédupliquer : dedupeFields, marketoGUID | dedupeFields |
| `customObjects` | Tableau d’objets | Oui | Liste des paires nom-valeur d’attribut pour l’objet. | – |

Les autorisations requises sont `Read-Write Custom Object`.

Si un champ de lien vers une Personne est spécifié dans la requête et que cette Personne n’existe pas, plusieurs reprises se produisent. Si cette personne est ajoutée pendant la fenêtre de nouvelle tentative (65 minutes), la mise à jour est réussie. Par exemple, si le champ Lien est `email` sur Personne et que Personne n’existe pas, des reprises ont lieu.

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

### Sociétés

Utilisez ce point d’entrée pour synchroniser les enregistrements d’entreprise. Il prend en charge les opérations de création, de mise à jour et d’upsert avec déduplication par un ID de société externe ou un ID interne Marketo.

| Méthode | Chemin |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/companies` |

#### En-têtes

| Clé | Valeur | Obligatoire |
| --- | --- | --- |
| `Content-Type` | application/json | Oui |
| `X-Mkto-User-Token` | {accessToken} | Oui |
| `X-Correlation-Id` | Chaîne arbitraire (longueur maximale de 255 caractères) | Non |
| `X-Request-Source` | Chaîne arbitraire (longueur maximale de 50 caractères) | Non |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| --- | --- | --- | --- | --- |
| `action` | Chaîne | Non | Action de synchronisation : `createOnly`, `updateOnly` ou `createOrUpdate` | `createOrUpdate` |
| `dedupeBy` | Chaîne | Non | Champ à dédupliquer sur : `dedupeFields` ou `idField` (non sensible à la casse). Pour `createOnly` et `createOrUpdate`, seul le `dedupeFields` est autorisé. Par `updateOnly`, les deux sont autorisés. | `dedupeFields` |
| `input` | Tableau d’objets | Oui | Liste des paires nom-valeur d’attributs de société. Accepte les `input` ou les `companies` de clé JSON. | – |

Chaque objet société du tableau `input` prend en charge les champs suivants :

| Clé | Type de données | Obligatoire | Description |
| --- | --- | --- | --- |
| `externalCompanyId` | Chaîne | Conditionnel | Identifiant externe de la société. Obligatoire lorsque la `dedupeBy` est `dedupeFields`. Non autorisé lorsque la `dedupeBy` est `idField`. |
| `id` | Long | Conditionnel | Identifiant de la société interne Marketo. Obligatoire lorsque `dedupeBy` est `idField` et `action` est `updateOnly`. Non autorisé lorsque la `dedupeBy` est `dedupeFields`. |
| `company` | Chaîne | Non | Nom de la société. |
| (n’importe quel champ) | Tous | Non | Champs d’entreprise standard ou personnalisés supplémentaires, tels que définis par [Décrire les entreprises](companies.md). Les noms de champ ne respectent pas la casse. |

Les autorisations requises sont `Read-Write Company`.

### Exemple de sociétés

#### Requête

`POST /subscriptions/{munchkinId}/companies`

#### En-têtes

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corps

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalCompanyId": "ext-company-001",
         "company": "Acme Corporation",
         "industry": "Technology",
         "numberOfEmployees": 5000,
         "annualRevenue": 100000000
      },
      {
         "externalCompanyId": "ext-company-002",
         "company": "Globex Industries",
         "industry": "Manufacturing",
         "numberOfEmployees": 1200
      }
   ]
}
```

#### Réponse

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Exemple de mise à jour des sociétés par ID

```json
{
   "action": "updateOnly",
   "dedupeBy": "idField",
   "input": [
      {
         "id": 12345,
         "company": "Acme Corporation (Renamed)",
         "numberOfEmployees": 5500
      }
   ]
}
```

### Règles de validation des sociétés

| Règle | Détail |
| --- | --- |
| action | Doit être l’un des suivants : `createOnly`, `updateOnly`, `createOrUpdate`. Respect de la casse. |
| dedupeBy | Doit être `dedupeFields` ou `idField` (non-respect de la casse). La valeur par défaut est `dedupeFields`. |
| dedupeBy + action | `createOnly` et `createOrUpdate` n’autorisent que les `dedupeFields`. `updateOnly` permet à la fois les `dedupeFields` et les `idField`. |
| Lorsqu’`dedupeBy=dedupeFields` | Chaque entreprise doit avoir des `externalCompanyId`. Le champ `id` doit être absent. |
| Lorsqu’`dedupeBy=idField` | Chaque entreprise doit avoir des `id`. Le champ `externalCompanyId` doit être absent. |
| `input` / `companies` | Ne doit pas être nul ou vide. |
| Nbre max. d’objets par requête | 1,000 |

### Membres du programme (synchronisation)

Point d&#39;entrée utilisé pour synchroniser le statut des membres du programme, ajouter des leads aux programmes ou mettre à jour leur statut de programme.

| Méthode | Chemin |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers` |

#### En-têtes

| Clé | Valeur | Obligatoire |
| --- | --- | --- |
| Content-Type | application/json | Oui |
| X-Mkto-User-Token | {accessToken} | Oui |
| X-Correlation-Id | Chaîne arbitraire (longueur maximale de 255 caractères) | Non |
| X-Request-Source | Chaîne arbitraire (longueur maximale de 50 caractères) | Non |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| --- | --- | --- | --- | --- |
| Programmes | Tableau d’objets | Oui | Liste des opérations du programme. Chaque spécifie un programme, un statut de cible et les prospects à synchroniser. | – |

Chaque objet du tableau de `programs` contient :

| Clé | Type de données | Obligatoire | Description |
| --- | --- | --- | --- |
| programId | Long | Oui | Identifiant du programme Marketo. Doit être un entier positif. |
| statut | Chaîne | Oui | Statut du membre du programme à définir ; `"Member"` ou `"Influenced"`, par exemple. Accepte les `statusName` ou les `status` de clé JSON. La valeur ne doit pas être `"Not in Program"` ; utilisez plutôt le point d’entrée de suppression. |
| membres | Tableau d’objets | Oui | Liste des références de lead à ajouter ou à mettre à jour dans le programme. Accepte les `input` ou les `members` de clé JSON. |

Chaque objet du tableau de `members` contient :

| Clé | Type de données | Obligatoire | Description |
| --- | --- | --- | --- |
| leadId | Long | Oui | ID de lead Marketo. |
| (n’importe quel champ) | Tous | Non | Champs de membre de programme personnalisés supplémentaires. Les noms de champ ne respectent pas la casse. |

Les autorisations requises sont `Read-Write Lead`.

### Exemple de synchronisation des membres du programme

#### Requête

`POST /subscriptions/{munchkinId}/programmembers`

#### En-têtes

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corps

```json
{
   "programs": [
      {
         "programId": 1001,
         "status": "Member",
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "status": "Influenced",
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Réponse

`HTTP/1.1 202`
`X-Request-ID: e3d92152-0fb1-444a-8f8f-29d5a2338598`

### Règles de validation de la synchronisation des membres du programme

| Règle | Détail |
| --- | --- |
| Programmes | Ne doit pas être nul ou vide. |
| programId | Obligatoire. Doit être un entier positif. |
| statut | Obligatoire. Ne doit pas être vide. Ne doit pas être `"Not in Program"` (non-respect de la casse). Utilisez plutôt le point d’entrée de suppression . |
| membres | Ne doit pas être nul ou vide. |
| leadId | Obligatoire pour chaque membre du tableau d’entrée. |
| Nb max de leads par requête | 1 000 membres au total pour tous les programmes. |

### Membres du programme (supprimer)

Point d’entrée utilisé pour supprimer les prospects des programmes. Cela définit le statut d’abonnement du prospect sur `"Not in Program"` et supprime le membre de ce programme.

>[!NOTE]
>
>Ce point d’entrée utilise la technique POST plutôt que DELETE, car la requête nécessite un corps JSON avec des données structurées.

| Méthode | Chemin |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers/delete` |

#### En-têtes

| Clé | Valeur | Obligatoire |
| --- | --- | --- |
| Content-Type | application/json | Oui |
| X-Mkto-User-Token | {accessToken} | Oui |
| X-Correlation-Id | Chaîne arbitraire (longueur maximale de 255 caractères) | Non |
| X-Request-Source | Chaîne arbitraire (longueur maximale de 50 caractères) | Non |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| --- | --- | --- | --- | --- |
| Programmes | Tableau d’objets | Oui | Liste des opérations de suppression de programme. Chaque spécifie un programme et les prospects à supprimer. | – |

Chaque objet du tableau de `programs` contient :

| Clé | Type de données | Obligatoire | Description |
| --- | --- | --- | --- |
| programId | Long | Oui | Identifiant du programme Marketo. Doit être un entier positif. |
| membres | Tableau d’objets | Oui | Liste des références de lead à supprimer du programme. Accepte les `input` ou les `members` de clé JSON. |

Chaque objet du tableau de `members` contient :

| Clé | Type de données | Obligatoire | Description |
| --- | --- | --- | --- |
| leadId | Long | Oui | ID de lead Marketo. |

Les autorisations requises sont `Read-Write Lead`.

### Exemple de suppression de membres de programme

#### Requête

`POST /subscriptions/{munchkinId}/programmembers/delete`

#### En-têtes

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corps

```json
{
   "programs": [
      {
         "programId": 1001,
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Réponse

`HTTP/1.1 202`
`X-Request-ID: a1b2c3d4-e5f6-7890-abcd-ef1234567890`

### Les membres du programme suppriment les règles de validation

| Règle | Détail |
| --- | --- |
| Programmes | Ne doit pas être nul ou vide. |
| programId | Obligatoire. Doit être un entier positif. |
| membres | Ne doit pas être nul ou vide. |
| leadId | Obligatoire pour chaque membre du tableau d’entrée. |
| Nb max de leads par requête | 1 000 membres au total pour tous les programmes. |

### Listes (Ajouter à la liste)

Point d&#39;entrée utilisé pour ajouter des leads à une liste statique. Les prospects sont identifiés par leur ID de prospect Marketo.

| Méthode | Chemin |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/lists` |

#### En-têtes

| Clé | Valeur | Obligatoire |
| --- | --- | --- |
| `Content-Type` | application/json | Oui |
| `X-Mkto-User-Token` | {accessToken} | Oui |
| `X-Correlation-Id` | Chaîne arbitraire (longueur maximale de 255 caractères) | Non |
| `X-Request-Source` | Chaîne arbitraire (longueur maximale de 50 caractères) | Non |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| --- | --- | --- | --- | --- |
| `listId` | Long | Oui | Identifiant de liste statique Marketo. Doit être un entier positif. | – |
| `leads` | Tableau d’objets | Oui | Liste des références de leads à ajouter à la liste. Accepte le `input` ou le `leads` de la clé JSON. | – |

Chaque objet du tableau d’entrée contient :

| Clé | Type de données | Obligatoire | Description |
| --- | --- | --- | --- |
| `leadId` | Long | Oui | ID de lead Marketo. Accepte le `leadId` ou le `id` de la clé JSON. |

Les autorisations requises sont `Read-Write Lead`.

### Liste ajoutée à l’exemple de liste

#### Requête

`POST /subscriptions/{munchkinId}/lists`

#### En-têtes

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corps

```json
{
   "listId": 1001,
   "leads": [
      {
         "leadId": 10001
      },
      {
         "leadId": 10002
      },
      {
         "leadId": 10003
      }
   ]
}
```

#### Réponse

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Listes ajoutées aux règles de validation de liste

| Règle | Détail |
| --- | --- |
| listId | Obligatoire. Doit être un entier positif (> 0). |
| leads | Obligatoire. Ne doit pas être nul ou vide. |
| leadId | Obligatoire pour chaque prospect dans le tableau d’entrée. |
| Nb max de leads par requête | 1 000 prospects au total dans le tableau d’entrée. |

### Listes (supprimer de la liste)

Point d’entrée utilisé pour supprimer les prospects d’une liste statique. Les prospects sont identifiés par leur ID de prospect Marketo.

>[!NOTE]
>
>Ce point d’entrée utilise la technique POST plutôt que DELETE, car la requête nécessite un corps JSON avec des données structurées.

| Méthode | Chemin |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/lists/remove` |

#### En-têtes

| Clé | Valeur | Obligatoire |
| --- | --- | --- |
| `Content-Type` | application/json | Oui |
| `X-Mkto-User-Token` | {accessToken} | Oui |
| `X-Correlation-Id` | Chaîne arbitraire (longueur maximale de 255 caractères) | Non |
| `X-Request-Source` | Chaîne arbitraire (longueur maximale de 50 caractères) | Non |

#### Corps de la requête

| Clé | Type de données | Obligatoire | Valeur | Valeur par défaut |
| --- | --- | --- | --- | --- |
| `listId` | Long | Oui | Identifiant de liste statique Marketo. Doit être un entier positif. | – |
| `leads` | Tableau d’objets | Oui | Liste des références de leads à supprimer de la liste. Accepte le `input` ou le `leads` de la clé JSON. | – |

Chaque objet du tableau d’entrée contient :

| Clé | Type de données | Obligatoire | Description |
| --- | --- | --- | --- |
| `leadId` | Long | Oui | ID de lead Marketo. Accepte le `leadId` ou le `id` de la clé JSON. |

Les autorisations requises sont `Read-Write Lead`.

### Exemples de listes supprimées

#### Requête

`POST /subscriptions/{munchkinId}/lists/remove`

#### En-têtes

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corps

```json
{
   "listId": 1001,
   "leads": [
      {
         "leadId": 10001
      },
      {
         "leadId": 10002
      }
   ]
}
```

#### Réponse

`HTTP/1.1 202`
`X-Request-ID: e3d92152-0fb1-444a-8f8f-29d5a2338598`

### Listes supprimées des règles de validation de liste

| Règle | Détail |
| --- | --- |
| listId | Obligatoire. Doit être un entier positif (> 0). |
| leads | Obligatoire. Ne doit pas être nul ou vide. |
| leadId | Obligatoire pour chaque prospect dans le tableau d’entrée. |
| Nb max de leads par requête | 1 000 prospects au total dans le tableau d’entrée. |

## Limites

L’API Data Ingestion comporte les mécanismes de sécurisation suivants :

- Taille maximale de la requête : 1 Mo
- Nombre maximal d’objets par demande pour chaque type d’objet : 1 000
- Nombre maximal de requêtes par seconde pour chaque ID client : 5 000
- Nombre maximal d’objets par jour : 10 000 000

Ces limites s’appliquent uniformément aux personnes, aux objets personnalisés, aux sociétés, aux membres de programme et aux listes. Pour les membres de programme, « objets par demande » correspond au nombre total de références de prospect dans tous les programmes dans une seule demande. Pour les listes, « objets par requête » correspond au nombre de références de prospect dans le tableau d’entrée.

## API Data Ingestion et API REST

L’API Data Ingestion diffère des autres API REST Marketo des manières suivantes :

- Transmettez le jeton d’accès dans l’en-tête `X-Mkto-User-Token`.
- Utilisez le domaine `mkto-ingestion-api.adobe.io`.
- Commencez le chemin de l’URL par `/subscriptions/MunchkinId`.
- N’utilisez pas de paramètres de requête.
- Un appel réussi renvoie le statut 202 et un corps de réponse vide.
- Un appel en échec renvoie un statut non-202 et un corps de réponse contenant des `{ "error_code" : "Error Code", "message" : "Message" }`.
- L’en-tête `X-Request-Id` renvoie l’identifiant de requête.
