---
title: Codes d’erreur
feature: REST API
description: Descriptions du code d’erreur Marketo.
exl-id: a923c4d6-2bbc-4cb7-be87-452f39b464b6
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '2320'
ht-degree: 3%

---

# Codes d’erreur

Vous trouverez ci-dessous des listes de codes d’erreur de l’API REST et une explication de la manière dont les erreurs sont renvoyées aux applications.

## Gestion et journalisation des exceptions

Lors du développement pour Marketo, il est important que les requêtes et les réponses soient consignées lorsqu’une exception inattendue est rencontrée. Bien que certains types d’exceptions, comme l’authentification expirée, puissent être gérés en toute sécurité par la réauthentification, d’autres peuvent nécessiter des interactions d’assistance. Les demandes et réponses seront toujours demandées dans ce scénario.

## Types d’erreurs

L’API REST Marketo peut renvoyer trois types d’erreurs différents en fonctionnement normal :

* HTTP-Level : ces erreurs sont indiquées par un code `4xx`.
* Niveau de réponse : ces erreurs sont incluses dans le tableau « erreurs » de la réponse JSON.
* Record-Level : ces erreurs sont incluses dans le tableau « résultat » de la réponse JSON et sont indiquées sur une base d’enregistrement individuel avec le champ « statut » et le tableau « raisons ».

Pour les types d’erreur de niveau réponse et de niveau enregistrement, un code d’état HTTP de 200 est renvoyé. Pour tous les types d’erreur, l’expression de raison HTTP ne doit pas être évaluée, car elle est facultative et peut être modifiée.

### Erreurs au niveau du HTTP

Dans des circonstances de fonctionnement normales, Marketo ne doit renvoyer que deux erreurs de code d’état HTTP, `413 Request Entity Too Large` et `414 Request URI Too Long`. Vous pouvez les récupérer en interceptant l’erreur, en modifiant la requête et en réessayant. Toutefois, avec les pratiques de codage intelligent, vous ne devriez jamais les rencontrer en mode sauvage.

Marketo renvoie la valeur 413 si la payload de la requête dépasse 1 Mo, ou 10 Mo dans le cas du lead d’importation. Dans la plupart des scénarios, il est peu probable que ces limites soient atteintes, mais l’ajout d’une vérification à la taille de la requête et le déplacement de tous les enregistrements, qui entraînent le dépassement de la limite dans une nouvelle requête, doivent éviter toute circonstance qui entraîne le renvoi de cette erreur par n’importe quel point d’entrée.

L’erreur 414 est renvoyée lorsque l’URI d’une requête GET dépasse 8 Ko. Pour l’éviter, vérifiez la longueur de votre chaîne de requête pour voir si elle dépasse cette limite. S’il remplace votre requête par une méthode POST, saisissez votre chaîne de requête comme corps de la requête avec le paramètre supplémentaire `_method=GET`. Cela annule la limitation des URI. Il est rare d&#39;atteindre cette limite dans la plupart des cas, mais cela est assez courant lors de la récupération de lots volumineux d&#39;enregistrements avec de longues valeurs de filtre individuel, comme un GUID.
Le point d’entrée [Identity](https://developer.adobe.com/marketo-apis/api/identity/) peut renvoyer une erreur 401 Non autorisé. Cela est généralement dû à un ID client non valide ou à un secret client non valide. Codes d’erreur au niveau HTTP

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
      <td>Entité De Requête Trop Grande</td>
      <td>La payload a dépassé la limite de 1 Mo.</td>
    </tr>
    <tr>
      <td><a name="414"></a>414</td>
      <td>Request-URI trop long</td>
      <td>L’URI de la requête a dépassé 8 000. La requête doit être retentée en tant que POST avec le paramètre `_method=GET` dans l’URL et le reste de la chaîne de requête dans le corps de la requête.</td>
    </tr>
  </tbody>
</table>


#### Erreurs de niveau de réponse

Les erreurs de niveau de réponse sont présentes lorsque le paramètre `success` de la réponse est défini sur false et sont structurées comme suit :

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

Chaque objet du tableau « errors » comporte deux membres, `code`, qui est un entier entre guillemets compris entre 601 et 799 et un `message` donnant la raison en texte brut de l’erreur. Les codes 6xx indiquent toujours qu’une requête a complètement échoué et n’a pas été exécutée. Exemple : un 601, « Jeton d’accès non valide », qui peut être récupéré en s’authentifiant à nouveau et en transmettant le nouveau jeton d’accès avec la requête. Les erreurs 7xx indiquent que la requête a échoué, soit parce qu’aucune donnée n’a été renvoyée, soit parce que la requête a été paramétrée de manière incorrecte, par exemple en incluant une date non valide, ou en raison d’un paramètre obligatoire manquant.

#### Codes d’erreur au niveau de la réponse

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
      <td>Le serveur distant a renvoyé une erreur. Probablement une temporisation. La requête doit être reprise avec une interruption exponentielle.</td>
    </tr>
    <tr>
      <td><a name="601"></a>601*</td>
      <td>Jeton d’accès non valide</td>
      <td>Un paramètre de jeton d’accès a été inclus dans la requête, mais la valeur n’était pas un jeton d’accès valide.</td>
    </tr>
    <tr>
      <td><a name="602"></a>602*</td>
      <td>Jeton d’accès expiré</td>
      <td>Le jeton d’accès inclus dans l’appel n’est plus valide en raison de son expiration.</td>
    </tr>
    <tr>
      <td><a name="603"></a>603</td>
      <td>Accès refusé</td>
      <td>L’authentification est réussie, mais l’utilisateur ne dispose pas des autorisations suffisantes pour appeler cette API. [Autorisations supplémentaires](custom-services.md) devront peut-être être attribuées au rôle utilisateur, ou <a href="https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/additional-integrations/create-an-allowlist-for-ip-based-api-access">la Liste autorisée pour l’accès à l’API IP</a> pourra être activée.</td>
    </tr>
    <tr>
      <td><a name="604"></a>604*</td>
      <td>Délai d’expiration de la demande</td>
      <td>La requête était en cours d’exécution depuis trop longtemps (par exemple, une contention de base de données s’est produite) ou a dépassé le délai d’expiration spécifié dans l’en-tête de l’appel.</td>
    </tr>
    <tr>
      <td><a name="605"></a>605*</td>
      <td>Méthode HTTP non prise en charge</td>
      <td>GET n’est pas pris en charge pour le point d’entrée des prospects de synchronisation. POST doit être utilisé.</td>
    </tr>
    <tr>
      <td><a name="606"></a>606</td>
      <td>Limite de débit maximale de `%s'; dépassée avec dans `%s's</td>
      <td>Le nombre d’appels au cours des 20 dernières secondes était supérieur à 100</td>
    </tr>
    <tr>
      <td><a name="607"></a>607</td>
      <td>Quota quotidien atteint</td>
      <td>Le nombre d’appels aujourd’hui a dépassé le quota d’abonnements (réinitialisé tous les jours à 00:00 CST).&gt;Votre quota se trouve dans le menu Admin-&gt;Services Web. Vous pouvez augmenter votre quota par l'intermédiaire de votre gestionnaire de compte.</td>
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
      <td>La ressource demandée est introuvable</td>
      <td>L’URI de l’appel ne correspondait pas à un type de ressource API REST. Cela est souvent dû à un URI de requête mal orthographié ou mal formaté</td>
    </tr>
    <tr>
      <td><a name="611"></a>611*</td>
      <td>Erreur système</td>
      <td>Toutes les exceptions non gérées</td>
    </tr>
    <tr>
      <td><a name="612"></a>612</td>
      <td>Type de contenu non valide</td>
      <td>Si cette erreur s’affiche, ajoutez un en-tête de type de contenu spécifiant le format JSON à votre requête. Par exemple, essayez d’utiliser « type de contenu : application/json ». <a href="https://stackoverflow.com/questions/28181325/why-invalid-content-type">Voir cette question StackOverflow</a> pour plus d’informations.</td>
    </tr>
    <tr>
      <td><a name="613"></a>613</td>
      <td>Requête Multipart Non Valide</td>
      <td>Le contenu en plusieurs parties de l’instruction POST n’a pas été formaté correctement</td>
    </tr>
    <tr>
      <td><a name="614"></a>614</td>
      <td>Abonnement non valide</td>
      <td>L’abonnement de destination est introuvable ou inatteignable. Cela indique généralement une inaccessibilité temporaire.</td>
    </tr>
    <tr>
      <td><a name="615"></a>615</td>
      <td>Limite d’accès simultané atteinte</td>
      <td>Au maximum, les demandes sont traitées par un abonnement 10 à la fois. Elle est renvoyée s’il existe déjà 10 requêtes en cours.</td>
    </tr>
    <tr>
      <td><a name="616"></a>616</td>
      <td>Type d’abonnement non valide</td>
      <td>Le type d’abonnement Marketo approprié est requis pour accéder à l’API de métadonnées d’objet personnalisé. Consultez votre CSM pour plus de détails.</td>
    </tr>
    <tr>
      <td><a name="701"></a>701</td>
      <td>%s ne peut pas être vide</td>
      <td>Le champ signalé ne doit pas être vide dans la requête</td>
    </tr>
    <tr>
      <td><a name="702"></a>702</td>
      <td>Aucune donnée trouvée pour un scénario de recherche donné</td>
      <td>Aucun enregistrement ne correspond aux paramètres de recherche donnés.
        Remarque : de nombreuses opérations de recherche ayant échoué renvoient « success = true » et aucune erreur, et définissent une chaîne d’informations d’avertissement.</td>
    </tr>
    <tr>
      <td><a name="703"></a>703</td>
      <td>La fonctionnalité n’est pas activée pour l’abonnement</td>
      <td>Fonctionnalité bêta qui n’a pas été activée dans l’abonnement d’un utilisateur</td>
    </tr>
    <tr>
      <td><a name="704"></a>704</td>
      <td>Format de date invalide</td>
      <td><ul>
          <li>Une date spécifiée n'était pas au bon format</li>
          <li>Un ID de contenu dynamique non valide a été spécifié</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="709"></a>709</td>
      <td>Violation de règle métier</td>
      <td>L’appel ne peut pas être effectué, car il enfreint une exigence de création ou de mise à jour d’une ressource, par exemple, la tentative de création d’un e-mail sans modèle. Il est également possible d’obtenir cette erreur lors de la tentative de :
        <ul>
          <li>Récupérez le contenu des pages de destination qui contiennent du contenu social.</li>
          <li>Clonez un programme qui contient certains types de ressources (voir <a href="programs.md#clone">Clonage de programme</a> pour plus d’informations).</li>
          <li>Approuver une ressource qui n’a pas de brouillon (c’est-à-dire qui a déjà été approuvée).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="710"></a>710</td>
      <td>Dossier Parent Introuvable</td>
      <td>Le dossier parent spécifié est introuvable</td>
    </tr>
    <tr>
      <td><a name="711"></a>711</td>
      <td>Type de dossier incompatible</td>
      <td>Le type du dossier spécifié n’était pas correct pour répondre à la demande</td>
    </tr>
    <tr>
      <td><a name="712"></a>712</td>
      <td>Opération de fusion vers le compte de personne non valide</td>
      <td>Un appel de fusion de leads a échoué en raison d’une tentative de fusion de leads qui sont des comptes de personne Salesforce.  Les comptes de personne Salesforce doivent être fusionnés dans Salesforce.</td>
    </tr>
    <tr>
      <td><a name="713"></a>713</td>
      <td>Erreur transitoire</td>
      <td>Une ressource système était temporairement indisponible au moment de l’appel API. Lorsque cette erreur se produit, il est conseillé d’attendre un certain temps, puis de relancer la requête.</td>
    </tr>
    <tr>
      <td><a name="714"></a>714</td>
      <td>Impossible de trouver le type d’enregistrement par défaut</td>
      <td>Un appel de fusion de leads a échoué, car aucun type d’enregistrement par défaut n’a pu être trouvé.</td>
    </tr>
    <tr>
      <td><a name="718"></a>718</td>
      <td>ExternalSalesPersonID introuvable</td>
      <td>Un appel d’opportunités de synchronisation a été effectué avec une valeur « ExternalSalesPersonID » inexistante.</td>
    </tr>
    <tr>
      <td>719</td>
      <td>Exception de délai d’attente de verrouillage</td>
      <td>Un appel de programme de clonage a été effectué et a expiré en attendant un verrouillage.</td>
    </tr>
  </tbody>
</table>

### Record-Level {#record_level_errors}

Les erreurs de niveau enregistrement indiquent qu&#39;une opération n&#39;a pas pu être effectuée pour un enregistrement individuel, mais que la demande elle-même était valide. Une réponse contenant des erreurs au niveau des enregistrements suit ce modèle :


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

Les enregistrements inclus dans le tableau de résultats des appels sont triés de la même manière que le tableau d’entrée d’une requête.
Chaque enregistrement d’une requête réussie peut réussir ou échouer sur une base individuelle, ce qui est indiqué par le champ de statut de chaque enregistrement inclus dans le tableau de résultats d’une réponse. Le champ « statut » de ces enregistrements sera « ignoré » et un tableau « raisons » sera présent. Chaque raison contient un membre « code » et un membre « message ». Le code est toujours 1xxx et le message indique pourquoi l’enregistrement a été ignoré. Par exemple, si une demande de leads de synchronisation a « action » défini sur « createOnly », mais qu’un lead existe déjà pour l’une des clés dans les enregistrements envoyés. Ce cas renvoie un code de 1005 et un message indiquant « Le prospect existe déjà », comme indiqué ci-dessus.

#### Codes D’Erreur Au Niveau De L’Enregistrement

>[!NOTE]
>
><table>
><tbody>
>    <tr>
>      <td>Code de réponse</td>
>      <td>Description</td>
>      <td>Commentaire</td>
>    </tr>
>    <tr>
>      <td><a name="1001"></a>1001</td>
>      <td>Valeur « %s » non valide. Obligatoire de type « %s »</td>
>      <td>Une erreur est générée chaque fois qu’une valeur de paramètre présente une incohérence de type. Par exemple, la valeur de chaîne spécifiée pour un paramètre entier.</td>
>    </tr>
>    <tr>
>      <td><a name="1002"></a>1002</td>
>      <td>Valeur manquante pour le paramètre obligatoire « %s »</td>
>      <td>Une erreur est générée lorsqu’un paramètre obligatoire est manquant dans la requête</td>
>    </tr>
>    <tr>
>      <td><a name="1003"></a>1003</td>
>      <td>Données non valides</td>
>      <td>Lorsque le type des données envoyées n’est pas valide pour le point d’entrée ou le mode donné, par exemple lorsque l’ID est envoyé pour un prospect avec une action désignée comme createOnly ou lors de l’utilisation de la campagne Demander sur une campagne par lots.</td>
>    </tr>
>    <tr>
>      <td><a name="1004"></a>1004</td>
>      <td>Lead introuvable</td>
>      <td>Pour syncLead, lorsque l’action est « updateOnly » et si le prospect est introuvable</td>
>    </tr>
>    <tr>
>      <td><a name="1005"></a>1005</td>
>      <td>Le lead existe déjà</td>
>      <td>Pour syncLead, lorsque l’action est « createOnly » et si un prospect existe déjà</td>
>    </tr>
>    <tr>
>      <td><a name="1006"></a>1006</td>
>      <td>Champ « %s » introuvable</td>
>      <td>Un champ inclus dans l’appel n’est pas un champ valide.</td>
>    </tr>
>    <tr>
>      <td><a name="1007"></a>1007</td>
>      <td>Plusieurs prospects correspondent aux critères de recherche</td>
>      <td>Plusieurs prospects correspondent aux critères de recherche. Les mises à jour ne peuvent être effectuées que lorsque la clé correspond à un seul enregistrement</td>
>    </tr>
>    <tr>
>      <td><a name="1008"></a>1008</td>
>      <td>Accès refusé à la partition « %s »</td>
>      <td>L’utilisateur du service personnalisé n’a pas accès à un espace de travail avec la partition où se trouve l’enregistrement.</td>
>    </tr>
>    <tr>
>      <td><a name="1009"></a>1009</td>
>      <td>Le nom de la partition doit être spécifié.</td>
>      <td></td>
>    </tr>
>    <tr>
>      <td><a name="1010"></a>1010</td>
>      <td>Mise à jour de partition non autorisée</td>
>      <td>L'enregistrement spécifié existe déjà dans une partition de lead distincte.</td>
>    </tr>
>    <tr>
>      <td><a name="1011"></a>1011</td>
>      <td>Champ « %s » non pris en charge</td>
>      <td>Lorsque le champ de recherche ou « filterType » est spécifié avec des champs standard non pris en charge (par exemple : firstName, lastName)</td>
>    </tr>
>    <tr>
>      <td><a name="1012"></a>1012</td>
>      <td>Valeur de cookie non valide « %s »</td>
>      <td>Peut se produire lors de l’appel de la <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST">Associer le prospect</a> avec une valeur non valide pour le paramètre « cookie ».
>        Cela se produit également lors de l’appel de <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET">Get Leads by Filter Type</a> avec « filterType=cookies » et une valeur non valide pour le paramètre « filterValues ».</td>
>    </tr>
>    <tr>
>      <td><a name="1013"></a>1013</td>
>      <td>Objet introuvable</td>
>      <td>Obtenir l'objet (liste, campagne) par id renvoie ce code d'erreur</td>
>    </tr>
>    <tr>
>      <td><a name="1014"></a>1014</td>
>      <td>Échec de la création de l’objet</td>
>      <td>Échec de la création de l’objet (liste)</td>
>    </tr>
>    <tr>
>      <td><a name="1015"></a>1015</td>
>      <td>Lead absent de la liste</td>
>      <td>Le prospect désigné n’est pas membre de la liste cible</td>
>    </tr>
>    <tr>
>      <td><a name="1016"></a>1016</td>
>      <td>Trop d’importations</td>
>      <td>Il y a trop d’importations en file d’attente. 10 maximum sont autorisés</td>
>    </tr>
>    <tr>
>      <td><a name="1017"></a>1017</td>
>      <td>L'objet existe déjà</td>
>      <td>Échec de la création car l'enregistrement existe déjà</td>
>    </tr>
>    <tr>
>      <td><a name="1018"></a>1018</td>
>      <td>CRM activé</td>
>      <td>L’action n’a pas pu être effectuée, car l’instance a une intégration CRM native activée.</td>
>    </tr>
>    <tr>
>      <td><a name="1019"></a>1019</td>
>      <td>Importation en cours</td>
>      <td>La liste cible est déjà importée dans</td>
>    </tr>
>    <tr>
>      <td><a name="1020"></a>1020</td>
>      <td>Trop de clones à programmer</td>
>      <td>L’abonnement a atteint l’utilisation allouée de « cloneToProgramName » dans le programme planifié pour la journée</td>
>    </tr>
>    <tr>
>      <td><a name="1021"></a>1021</td>
>      <td>Mise à jour d’entreprise non autorisée</td>
>      <td>Mise à jour d’entreprise non autorisée pendant syncLead</td>
>    </tr>
>    <tr>
>      <td><a name="1022"></a>1022</td>
>      <td>Objet utilisé</td>
>      <td>La suppression n'est pas autorisée lorsqu'un objet est utilisé par un autre objet</td>
>    </tr>
>    <tr>
>      <td><a name="1025"></a>1025</td>
>      <td>Statut de programme introuvable</td>
>      <td>Un statut a été spécifié pour Modifier le statut du programme de lead qui ne correspondait pas à un statut disponible pour le canal du programme.</td>
>    </tr>
>    <tr>
>      <td><a name="1026"></a>1026</td>
>      <td>Objet personnalisé non activé</td>
>      <td>L'action n'a pas pu être exécutée, car l'intégration d'objets personnalisés n'est pas activée pour l'instance.</td>
>    </tr>
>    <tr>
>      <td><a name="1027"></a>1027</td>
>      <td>Limite de type d’activité max. atteinte</td>
>      <td>L’abonnement a atteint le nombre maximum de types d’activités personnalisées disponibles.</td>
>    </tr>
>    <tr>
>      <td><a name="1028"></a>1028</td>
>      <td>Limite de champ maximale atteinte</td>
>      <td>Les activités personnalisées ont un maximum de 20 attributs secondaires.</td>
>    </tr>
>    <tr>
>      <td><a name="1029"></a>1029</td>
>      <td><ul>
>          <li>Trop de tâches dans la file d’attente</li>
>          <li>Quota quotidien d'exportation dépassé</li>
>          <li>Traitement déjà mis en file d’attente</li>
>        </ul></td>
>      <td><ul>
>          <li>Les abonnements peuvent contenir jusqu’à 10 tâches d’extraction en bloc dans la file d’attente à tout moment donné.</li>
>          <li>Par défaut, les tâches d’extraction sont limitées à 500MB par jour (réinitialisé tous les jours à 00:00 CST).</li>
>          <li>L’ID d’exportation a déjà été mis en file d’attente.</li>
>        </ul></td>
>    </tr>
>    <tr>
>      <td><a name="1035"></a>1035</td>
>      <td>Type de filtre non pris en charge</td>
>      <td>Dans certains abonnements, les types de filtre d’extraction de leads en bloc suivants ne sont pas pris en charge : updatedAt, smartListId, smartListName.</td>
>    </tr>
>    <tr>
>      <td><a name="1036"></a>1036</td>
>      <td>Objet en double trouvé dans l’entrée</td>
>      <td>Un appel a été effectué pour mettre à jour deux enregistrements ou plus à l'aide de la même clé étrangère. Par exemple, un appel Sync Companies utilisant le même externalCompanyId pour plusieurs sociétés.</td>
>    </tr>
>    <tr>
>      <td><a name="1037"></a>1037</td>
>      <td>Lead ignoré</td>
>      <td>Le prospect a été ignoré, car il a déjà ce statut ou l’a dépassé.</td>
>    </tr>
>    <tr>
>      <td><a name="1042"></a>1042</td>
>      <td>Date d’exécution non valide</td>
>      <td>La date runAt spécifiée pour Planifier la campagne était trop éloignée dans le futur (la durée maximale est de 2 ans).</td>
>    </tr>
>    <tr>
>      <td><a name="1048"></a>1048</td>
>      <td>Échec de l’abandon du brouillon d’objet personnalisé</td>
>      <td>Un appel a été effectué pour ignorer le brouillon d'un objet personnalisé.</td>
>    </tr>
>    <tr>
>      <td><a name="1049"></a>1049</td>
>      <td>Échec de la création de l’activité</td>
>      <td>Tableau d’attributs trop long.
>        Le tableau d’attributs transmis à l’enregistrement a dépassé la longueur maximale de 65536 octets</td>
>    </tr>
>    <tr>
>      <td><a name="1076"></a>1076</td>
>      <td>L’appel <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Fusionner les prospects</a> avec l’indicateur mergeInCRM est le 4.</td>
>      <td>Vous êtes en train de créer un enregistrement en double. Il est recommandé d’utiliser plutôt un enregistrement existant.
>        Il s’agit du message d’erreur que Marketo reçoit lors de la fusion dans Salesforce.</td>
>    </tr>
>    <tr>
>      <td><a name="1077"></a>1077</td>
>      <td>Échec de l’appel <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Merge Leads</a> en raison de la longueur du champ « SFDC »</td>
>      <td>Un appel de fusion de leads avec mergeInCRM défini sur true a échoué, car « SFDC Field » a dépassé la limite de caractères autorisés. Pour corriger ce problème, réduisez la longueur du « champ SFDC » ou définissez mergeInCRM sur false.</td>
>    </tr>
>    <tr>
>      <td><a name="1078"></a>1078</td>
>      <td>L’appel <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Fusionner les leads</a> a échoué en raison d’une entité supprimée, d’un lead/contact ou d’un critère de filtre de champ ne correspond pas.</td>
>      <td>Échec de la fusion, impossible d’effectuer l’opération de fusion dans un CRM synchronisé en mode natif
>        Il s’agit du message d’erreur que Marketo reçoit lors de la fusion dans Salesforce.</td>
>    </tr>
>    <tr>
>      <td><a name="1079"></a>1079</td>
>      <td>L’appel <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Fusionner les leads</a> a échoué en raison d’un conflit d’URL personnalisée dans les enregistrements en double</td>
>      <td>Un appel de fusion de leads a spécifié de nombreux leads avec la même URL personnalisée. Pour résoudre ce problème, utilisez l’interface utilisateur de Marketo Engage pour fusionner ces enregistrements.</td>
>    </tr>
>  </tbody>
></table>
