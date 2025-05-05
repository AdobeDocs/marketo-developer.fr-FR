---
title: Codes d’erreur
feature: REST API
description: Descriptions du code d’erreur Marketo.
exl-id: a923c4d6-2bbc-4cb7-be87-452f39b464b6
source-git-commit: d0750eab0a37df0b7f80c6252f46c95068975000
workflow-type: tm+mt
source-wordcount: '2273'
ht-degree: 3%

---

# Codes d’erreur

Vous trouverez ci-dessous des listes de codes d’erreur de l’API REST, ainsi qu’une explication de la manière dont les erreurs sont renvoyées aux applications.

## Gestion et journalisation des exceptions

Lors du développement pour Marketo, il est important que les requêtes et les réponses soient consignées lorsqu’une exception inattendue se produit. Bien que certains types d’exceptions, telles que l’authentification expirée, puissent être gérés en toute sécurité par la réauthentification, d’autres peuvent nécessiter des interactions d’assistance, et les demandes et réponses seront toujours demandées dans ce scénario.

## Types d’erreur

L’API REST Marketo peut renvoyer trois types d’erreurs différents en cas de fonctionnement normal :

* Au niveau HTTP : ces erreurs sont indiquées par un code `4xx`.
* Niveau de réponse : ces erreurs sont incluses dans le tableau &quot;errors&quot; de la réponse JSON.
* Record-Level : ces erreurs sont incluses dans le tableau &quot;result&quot; de la réponse JSON et sont indiquées sur une base d’enregistrement individuelle avec le champ &quot;status&quot; et le tableau &quot;raisons&quot;.

Pour les types d’erreur Response-Level et Record-Level, un code d’état HTTP 200 est renvoyé. Pour tous les types d’erreur, l’expression de raison HTTP ne doit pas être évaluée, car elle est facultative et susceptible d’être modifiée.

### Erreurs au niveau HTTP

Dans des conditions d’exploitation normales, Marketo ne doit renvoyer que deux erreurs de code d’état HTTP, `413 Request Entity Too Large` et `414 Request URI Too Long`. Ils peuvent tous deux être récupérés en capturant l’erreur, en modifiant la requête et en réessayant, mais avec des pratiques de codage intelligent, vous ne devriez jamais les rencontrer dans la nature.

Marketo renvoie 413 si la charge utile de la requête dépasse 1 Mo ou 10 Mo dans le cas d’une piste d’importation. Dans la plupart des scénarios, il est peu probable que ces limites soient atteintes, mais l’ajout d’une vérification de la taille de la requête et le déplacement d’enregistrements, ce qui entraîne le dépassement de la limite par une nouvelle requête, devrait empêcher tout contexte, ce qui entraîne le renvoi de cette erreur par les points de terminaison.

414 est renvoyé lorsque l’URI d’une demande de GET dépasse 8 Ko. Pour l’éviter, vérifiez la longueur de votre chaîne de requête pour voir si elle dépasse cette limite. S’il transforme votre requête en méthode de POST, entrez votre chaîne de requête en tant que corps de requête avec le paramètre supplémentaire `_method=GET`. Cela annule la limitation des URI. Il est rare d’atteindre cette limite dans la plupart des cas, mais cela est quelque peu courant lors de la récupération de lots volumineux d’enregistrements avec de longues valeurs de filtre individuelles, telles qu’un GUID.
Le point d’entrée [Identity](https://developer.adobe.com/marketo-apis/api/identity/) peut renvoyer une erreur 401 Unauthorized . Cela est généralement dû à un ID de client non valide ou à un secret de client non valide. Codes d’erreur au niveau HTTP

<table>
  <thead>
    <tr>
      <th> Code de réponse </th>
      <th> Description </th>
      <th colspan="1"> Commentaire </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="413"></a>413</td>
      <td>Entité de requête trop volumineuse</td>
      <td>La payload a dépassé la limite de 1 Mo.</td>
    </tr>
    <tr>
      <td><a name="414"></a>414</td>
      <td>Request-URI Trop long</td>
      <td>L’URI de la requête dépassait 8 Ko. La requête doit être relancée en tant que POST avec le paramètre `_method=GET` dans l’URL et le reste de la chaîne de requête dans le corps de la requête.</td>
    </tr>
  </tbody>
</table>


#### Erreurs au niveau de la réponse

Des erreurs de niveau de réponse sont présentes lorsque le paramètre `success` de la réponse est défini sur false et est structuré comme suit :

```json
{
    "requestId": "e42b#14272d07d78",
    "success": false,
    "errors": [
        {
            "code": "601",
            "message": "Unauthorized"
        }
    ]
}
```

Chaque objet du tableau &quot;errors&quot; comporte deux membres, `code`, qui est un entier entre 601 et 799 cité et un `message` qui indique la raison en texte brut de l’erreur. Les codes 6xx indiquent toujours qu’une requête a complètement échoué et n’a pas été exécutée. Par exemple, 601, &quot;Jeton d’accès non valide&quot;, qui peut être récupéré en réauthentifiant et en transmettant le nouveau jeton d’accès avec la requête. Les erreurs 7xx indiquent que la requête a échoué, soit parce qu’aucune donnée n’a été renvoyée, soit parce que la requête a été mal paramétrée, par exemple en incluant une date non valide, ou parce qu’un paramètre requis a été absent.

#### Codes d’erreur de niveau réponse

Un appel API qui renvoie ce code de réponse n’est pas comptabilisé dans votre quota quotidien ou votre limite de taux.

<table>
  <thead>
    <tr>
      <th> Code de réponse </th>
      <th> Description </th>
      <th> Commentaire </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="502"></a>502</td>
      <td>Passerelle incorrecte</td>
      <td>Le serveur distant a renvoyé une erreur. Probablement un dépassement de délai. La requête doit être relancée avec un backoff exponentiel.</td>
    </tr>
    <tr>
      <td><a name="601"></a>601*</td>
      <td>Jeton d’accès non valide</td>
      <td>Un paramètre Access Token a été inclus dans la requête, mais la valeur n’était pas un jeton d’accès valide.</td>
    </tr>
    <tr>
      <td><a name="602"></a>602*</td>
      <td>Jeton d’accès expiré</td>
      <td>Le jeton d’accès inclus dans l’appel n’est plus valide en raison de l’expiration.</td>
    </tr>
    <tr>
      <td><a name="603"></a>603</td>
      <td>Accès refusé</td>
      <td>L’authentification a réussi, mais l’utilisateur ne dispose pas des autorisations suffisantes pour appeler cette API. [Autorisations supplémentaires](custom-services.md) peut avoir besoin d’être affecté au rôle d’utilisateur, ou la <a href="https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/additional-integrations/create-an-allowlist-for-ip-based-api-access">Liste autorisée pour l’accès API basé sur IP</a> peut être activée.</td>
    </tr>
    <tr>
      <td><a name="604"></a>604*</td>
      <td>Délai d’expiration de la requête</td>
      <td>La requête s’exécutait trop longtemps (par exemple, a rencontré une contention de base de données) ou a dépassé le délai d’expiration spécifié dans l’en-tête de l’appel.</td>
    </tr>
    <tr>
      <td><a name="605"></a>605*</td>
      <td>Méthode HTTP non prise en charge</td>
      <td>GET n’est pas pris en charge pour le point de terminaison Leads de synchronisation. POST doit être utilisé.</td>
    </tr>
    <tr>
      <td><a name="606"></a>606</td>
      <td>Limite de taux maximale `%s`; dépassée par dans `%s` secondes</td>
      <td>Le nombre d’appels des 20 dernières secondes était supérieur à 100</td>
    </tr>
    <tr>
      <td><a name="607"></a>607</td>
      <td>Quota quotidien atteint</td>
      <td>Le nombre d’appels aujourd’hui a dépassé le quota d’abonnement (réinitialisations quotidiennes à 12h00 heure du Pacifique).&gt;Votre quota se trouve dans le menu Admin-&gt;Services web . Vous pouvez augmenter votre quota par l’intermédiaire de votre gestionnaire de compte.</td>
    </tr>
    <tr>
      <td><a name="608"></a>608*</td>
      <td>API temporairement indisponible</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="609"></a>609</td>
      <td>JSON non valide</td>
      <td>Le corps inclus dans la requête n’est pas un JSON valide.</td>
    </tr>
    <tr>
      <td><a name="610"></a>610</td>
      <td>Ressource demandée introuvable</td>
      <td>L’URI de l’appel ne correspondait pas à un type de ressource API REST. Cela est souvent dû à un URI de requête mal orthographié ou mal formaté.</td>
    </tr>
    <tr>
      <td><a name="611"></a>611*</td>
      <td>Erreur système</td>
      <td>Toutes les exceptions non traitées</td>
    </tr>
    <tr>
      <td><a name="612"></a>612</td>
      <td>Type de contenu non valide</td>
      <td>Si cette erreur s’affiche, ajoutez un en-tête de type de contenu spécifiant le format JSON à votre requête. Par exemple, essayez d’utiliser `content type: application/json`. <a href="https://stackoverflow.com/questions/28181325/why-invalid-content-type">Pour plus d’informations, consultez cette question StackOverflow</a> .</td>
    </tr>
    <tr>
      <td><a name="613"></a>613</td>
      <td>Requête multipartie non valide</td>
      <td>Le contenu en plusieurs parties du POST n’a pas été correctement formaté.</td>
    </tr>
    <tr>
      <td><a name="614"></a>614</td>
      <td>Abonnement non valide</td>
      <td>L’abonnement à la destination est introuvable ou inaccessible. Cela indique généralement une inaccessibilité temporaire.</td>
    </tr>
    <tr>
      <td><a name="615"></a>615</td>
      <td>Limite d’accès simultanée atteinte</td>
      <td>Au maximum, les demandes sont traitées par n’importe quel abonnement 10 à la fois. Cette valeur est renvoyée s’il existe déjà 10 requêtes en cours.</td>
    </tr>
    <tr>
      <td><a name="616"></a>616</td>
      <td>Type d'abonnement non valide</td>
      <td>Le type d’abonnement Marketo approprié est requis pour accéder à l’API de métadonnées d’objet personnalisé. Pour plus d’informations, consultez votre CSM .</td>
    </tr>
    <tr>
      <td><a name="701"></a>701</td>
      <td>%s ne peut pas être vide</td>
      <td>Le champ signalé ne doit pas être vide dans la requête.</td>
    </tr>
    <tr>
      <td><a name="702"></a>702</td>
      <td>Aucune donnée trouvée pour un scénario de recherche donné</td>
      <td>Aucun enregistrement ne correspondait aux paramètres de recherche donnés.
        Remarque : De nombreuses opérations de recherche ayant échoué renvoient `success = true` et aucune erreur et définissent une chaîne d’information d’avertissement.</td>
    </tr>
    <tr>
      <td><a name="703"></a>703</td>
      <td>La fonctionnalité n’est pas activée pour l’abonnement.</td>
      <td>Fonction bêta qui n’a pas été activée dans l’abonnement d’un utilisateur</td>
    </tr>
    <tr>
      <td><a name="704"></a>704</td>
      <td>Format de date invalide</td>
      <td><ul>
          <li>Une date spécifiée dans un format incorrect a été spécifiée.</li>
          <li>Un identifiant de contenu dynamique non valide a été spécifié.</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="709"></a>709</td>
      <td>Violation des règles commerciales</td>
      <td>L’appel ne peut pas être rempli, car il enfreint une obligation de création ou de mise à jour d’une ressource, par exemple en essayant de créer un email sans modèle. Il est également possible d’obtenir cette erreur lorsque vous essayez d’effectuer les opérations suivantes :
        <ul>
          <li>Récupérez le contenu des landing pages qui contiennent du contenu social.</li>
          <li>Cloner un programme qui contient certains types de ressources (voir <a href="programs.md#clone">Clone de programme</a> pour plus d’informations).</li>
          <li>Approuvez une ressource sans version préliminaire (c’est-à-dire qu’elle a déjà été approuvée).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="710"></a>710</td>
      <td>Dossier parent introuvable</td>
      <td>Le dossier parent spécifié est introuvable</td>
    </tr>
    <tr>
      <td><a name="711"></a>711</td>
      <td>Type de dossier incompatible</td>
      <td>Le dossier spécifié n’était pas du type correct pour répondre à la requête.</td>
    </tr>
    <tr>
      <td><a name="712"></a>712</td>
      <td>L’opération de fusion de compte de personne n’est pas valide</td>
      <td>Un appel Fusionner les pistes a échoué en raison d’une tentative de fusion de pistes qui sont des comptes de personne Salesforce.  Les comptes de personne Salesforce doivent être fusionnés dans Salesforce.</td>
    </tr>
    <tr>
      <td><a name="713"></a>713</td>
      <td>Erreur transitoire</td>
      <td>Une ressource système était temporairement indisponible au moment de l’appel API. Lorsque cette erreur est rencontrée, il est conseillé d’attendre longtemps, puis de relancer la requête.</td>
    </tr>
    <tr>
      <td><a name="714"></a>714</td>
      <td>Impossible de trouver le type d’enregistrement par défaut</td>
      <td>Un appel Fusionner les pistes a échoué car il n’a pas pu trouver un type d’enregistrement par défaut.</td>
    </tr>
    <tr>
      <td><a name="718"></a>718</td>
      <td>ExternalSalesPersonID introuvable</td>
      <td>Un appel de synchronisation des opportunités a été effectué avec une valeur "ExternalSalesPersonID" inexistante.</td>
    </tr>
    <tr>
      <td>719</td>
      <td>Verrouillage de l’exception de délai d’attente</td>
      <td>Un appel au programme de clonage a été effectué et a expiré en attendant un verrou.</td>
    </tr>
  </tbody>
</table>

### Record Level {#record_level_errors}

Les erreurs de niveau enregistrement indiquent qu’une opération n’a pas pu être effectuée pour un enregistrement individuel, mais la requête elle-même était valide. Une réponse avec des erreurs de niveau enregistrement suit ce modèle :


#### Réponse

```json
{  
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[  
      {  
         "id":50,
         "status":"created"
      },
      {  
         "id":51,
         "status":"created"
      },
      {  
         "status":"skipped",
         "reasons":[  
            {  
               "code":"1005",
               "message":"Lead already exists"
            }
         ]
      }
   ]
}
```

Les enregistrements inclus dans le tableau de résultats des appels sont classés de la même manière que le tableau d’entrée d’une requête.
Chaque enregistrement d’une requête réussie peut réussir ou échouer individuellement, ce qui est indiqué par le champ d’état de chaque enregistrement inclus dans le tableau de résultat d’une réponse. Le champ &quot;status&quot; de ces enregistrements sera &quot;sauté&quot; et un tableau &quot;raisons&quot; est présent. Chaque raison contient un membre &quot;code&quot; et un membre &quot;message&quot;. Le code est toujours 1xxx et le message indique pourquoi l’enregistrement a été ignoré. Par exemple, une requête de pistes de synchronisation avec &quot;action&quot; définie sur &quot;createOnly&quot;, mais une piste existe déjà pour l’une des clés dans les enregistrements envoyés. Ce cas renvoie un code de 1005 et un message indiquant &quot;Le prospect existe déjà&quot; comme indiqué ci-dessus.

#### Codes d’erreur au niveau de l’enregistrement

<table>
  <tbody>
    <tr>
      <td>Code de réponse</td>
      <td>Description</td>
      <td>Commentaire</td>
    </tr>
    <tr>
      <td><a name="1001"></a>1001</td>
      <td>Valeur non valide ‘%s’. Requis de type ‘%s’</td>
      <td>Une erreur est générée chaque fois qu’une valeur de paramètre ne correspond pas au type . Par exemple, la valeur string spécifiée pour un paramètre entier.</td>
    </tr>
    <tr>
      <td><a name="1002"></a>1002</td>
      <td>Valeur manquante pour le paramètre requis %s</td>
      <td>Une erreur est générée lorsqu’un paramètre requis est absent de la requête.</td>
    </tr>
    <tr>
      <td><a name="1003"></a>1003</td>
      <td>Données non valides</td>
      <td>Lorsque les données envoyées ne sont pas un type valide pour le point de terminaison ou le mode donné, par exemple lorsque l’identifiant est envoyé pour une piste avec l’action désignée comme createOnly ou lors de l’utilisation de l’option Demander la campagne sur une campagne par lots.</td>
    </tr>
    <tr>
      <td><a name="1004"></a>1004</td>
      <td>Lead introuvable</td>
      <td>Pour syncLead, lorsque l’action est "updateOnly" et si la piste est introuvable</td>
    </tr>
    <tr>
      <td><a name="1005"></a>1005</td>
      <td>La piste existe déjà</td>
      <td>Pour syncLead, lorsque l’action est "createOnly" et si une piste existe déjà</td>
    </tr>
    <tr>
      <td><a name="1006"></a>1006</td>
      <td>Champ ‘%s’ introuvable</td>
      <td>Un champ inclus dans l’appel n’est pas un champ valide.</td>
    </tr>
    <tr>
      <td><a name="1007"></a>1007</td>
      <td>Plusieurs pistes correspondent aux critères de recherche.</td>
      <td>Plusieurs pistes correspondent aux critères de recherche. Les mises à jour ne peuvent être effectuées que lorsque la clé correspond à un seul enregistrement.</td>
    </tr>
    <tr>
      <td><a name="1008"></a>1008</td>
      <td>Accès refusé à la partition ‘%s’</td>
      <td>L’utilisateur du service personnalisé n’a pas accès à un espace de travail avec la partition dans laquelle l’enregistrement existe.</td>
    </tr>
    <tr>
      <td><a name="1009"></a>1009</td>
      <td>Le nom de la partition doit être spécifié</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="1010"></a>1010</td>
      <td>Mise à jour de la partition non autorisée</td>
      <td>L’enregistrement spécifié existe déjà dans une partition de piste distincte.</td>
    </tr>
    <tr>
      <td><a name="1011"></a>1011</td>
      <td>Champ ‘%s’ non pris en charge</td>
      <td>Lorsque le champ de recherche ou `filterType` est spécifié avec des champs standard non pris en charge (ex : firstName, lastName)</td>
    </tr>
    <tr>
      <td><a name="1012"></a>1012</td>
      <td>Valeur de cookie non valide ‘%s’</td>
      <td>Cela peut se produire lors de l’appel de <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST">Associer le prospect</a> avec une valeur non valide pour le paramètre `cookie`.
        Cela se produit également lors de l’appel de <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET">Get Leads by Filter Type</a> avec `filterType=cookies` et une valeur non valide pour le paramètre `filterValues`.</td>
    </tr>
    <tr>
      <td><a name="1013"></a>1013</td>
      <td>Objet introuvable</td>
      <td>Get object (list, campaign) by id renvoie ce code d’erreur.</td>
    </tr>
    <tr>
      <td><a name="1014"></a>1014</td>
      <td>Échec de la création de l’objet</td>
      <td>Échec de la création de l’objet (liste)</td>
    </tr>
    <tr>
      <td><a name="1015"></a>1015</td>
      <td>Plomb non dans la liste</td>
      <td>Le prospect désigné n’est pas membre de la liste cible</td>
    </tr>
    <tr>
      <td><a name="1016"></a>1016</td>
      <td>Trop d'imports</td>
      <td>Trop d'importations sont en file d'attente. Un maximum de 10 est autorisé</td>
    </tr>
    <tr>
      <td><a name="1017"></a>1017</td>
      <td>L'objet existe déjà</td>
      <td>La création a échoué car l’enregistrement existe déjà</td>
    </tr>
    <tr>
      <td><a name="1018"></a>1018</td>
      <td>CRM activé</td>
      <td>L’action n’a pas pu être effectuée, car l’intégration CRM native de l’instance est activée.</td>
    </tr>
    <tr>
      <td><a name="1019"></a>1019</td>
      <td>Importation en cours</td>
      <td>La liste cible est déjà en cours d'import vers</td>
    </tr>
    <tr>
      <td><a name="1020"></a>1020</td>
      <td>Trop de clones à programmer</td>
      <td>L’abonnement a atteint l’utilisation autorisée de "cloneToProgramName" dans le programme de planification pour la journée.</td>
    </tr>
    <tr>
      <td><a name="1021"></a>1021</td>
      <td>Mise à jour de société non autorisée</td>
      <td>Mise à jour de la société non autorisée pendant syncLead</td>
    </tr>
    <tr>
      <td><a name="1022"></a>1022</td>
      <td>Objet utilisé</td>
      <td>La suppression n’est pas autorisée lorsqu’un objet est utilisé par un autre objet.</td>
    </tr>
    <tr>
      <td><a name="1025"></a>1025</td>
      <td>État du programme introuvable</td>
      <td>Un état a été spécifié pour Modifier l’état du programme de piste qui ne correspondait pas à un état disponible pour le canal du programme.</td>
    </tr>
    <tr>
      <td><a name="1026"></a>1026</td>
      <td>Objet personnalisé non activé</td>
      <td>L’action n’a pas pu être exécutée, car l’intégration des objets personnalisés n’est pas activée pour l’instance.</td>
    </tr>
    <tr>
      <td><a name="1027"></a>1027</td>
      <td>Limite max. de type d’activité atteinte</td>
      <td>L’abonnement a atteint le nombre maximal de types d’activité personnalisés disponibles.</td>
    </tr>
    <tr>
      <td><a name="1028"></a>1028</td>
      <td>Limite de champ maximale atteinte</td>
      <td>Les activités personnalisées possèdent au maximum 20 attributs secondaires.</td>
    </tr>
    <tr>
      <td><a name="1029"></a>1029</td>
      <td><ul>
          <li>Trop de tâches dans la file d’attente</li>
          <li>Exporter le quota quotidien dépassé</li>
        </ul></td>
      <td><ul>
          <li>Les abonnements peuvent, à tout moment, contenir, au maximum, 10 tâches d’extraction en masse dans la file d’attente.</li>
          <li>Par défaut, les tâches d’extraction sont limitées à 500 Mo par jour (réinitialisations quotidiennes à 00h00 du matin (heure de la côte Est).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="1035"></a>1035</td>
      <td>Type de filtre non pris en charge</td>
      <td>Dans certains abonnements, les types de filtres d’extraction de pistes en bloc suivants ne sont pas pris en charge : updatedAt, smartListId, smartListName.</td>
    </tr>
    <tr>
      <td><a name="1036"></a>1036</td>
      <td>Objet dupliqué trouvé dans l’entrée</td>
      <td>Un appel a été lancé pour mettre à jour deux enregistrements ou plus utilisant la même clé étrangère. Par exemple, un appel de sociétés de synchronisation utilisant le même externalCompanyId pour plusieurs sociétés.</td>
    </tr>
    <tr>
      <td><a name="1037"></a>1037</td>
      <td>Le plomb a été sauté.</td>
      <td>La piste a été ignorée car elle se trouve déjà dans ce statut ou en est dépassée.</td>
    </tr>
    <tr>
      <td><a name="1042"></a>1042</td>
      <td>Date d’exécution non valide</td>
      <td>La date runAt spécifiée pour la planification de la campagne était trop éloignée dans le futur (le maximum est de 2 ans).</td>
    </tr>
    <tr>
      <td><a name="1048"></a>1048</td>
      <td>Échec de l’abandon du brouillon d’objet personnalisé</td>
      <td>Un appel a été effectué pour ignorer la version préliminaire d’un objet personnalisé.</td>
    </tr>
    <tr>
      <td><a name="1049"></a>1049</td>
      <td>Échec de la création de l’activité</td>
      <td>Tableau d’attributs trop long.
        Le tableau des attributs transmis à l’enregistrement a dépassé la longueur maximale de 6 536 octets.</td>
    </tr>
    <tr>
      <td><a name="1076"></a>1076</td>
      <td>L’appel <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Fusionner les pistes</a> avec l’indicateur mergeInCRM est 4.</td>
      <td>Vous créez un enregistrement en double. Il est recommandé d’utiliser un enregistrement existant à la place.
        Il s’agit du message d’erreur que Marketo reçoit lors de la fusion dans Salesforce.</td>
    </tr>
    <tr>
      <td><a name="1077"></a>1077</td>
      <td>L’appel <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Fusionner les pistes</a> a échoué en raison de la longueur du "champ SFDC"</td>
      <td>Un appel Fusionner les pistes avec mergeInCRM défini sur true a échoué en raison d’un "champ SFDC" dépassant la limite des caractères autorisés. Pour corriger ce problème, réduisez la longueur de `SFDC Field` ou définissez mergeInCRM sur false.</td>
    </tr>
    <tr>
      <td><a name="1078"></a>1078</td>
      <td>L’appel <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Fusionner les pistes</a> a échoué en raison d’une entité supprimée, pas un critère de piste/contact, ou de filtre de champ ne correspond pas.</td>
      <td>Échec de la fusion, impossible d’effectuer une opération de fusion dans un CRM synchronisé nativement
        Il s’agit du message d’erreur que Marketo reçoit lors de la fusion dans Salesforce.</td>
    </tr>
  </tbody>
</table>
