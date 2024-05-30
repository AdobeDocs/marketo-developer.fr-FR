---
title: "Listes intelligentes"
feature: REST API
description: '"Créer et modifier des listes dynamiques".'
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 1%

---


# Listes intelligentes

[Référence du point de terminaison des listes dynamiques](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists)

Marketo propose un ensemble d’API REST pour effectuer des opérations sur des listes dynamiques. Ces API suivent le modèle d’interface standard pour les API de ressources qui offrent des options de requête, de suppression et de clonage.

Remarque : Ces API sont prises en charge uniquement pour les listes dynamiques créées par l’utilisateur. Ils ne peuvent pas être utilisés pour [Listes dynamiques système/intégrées](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/using-smart-lists/use-built-in-system-smart-lists).

## Requête

La requête de listes dynamiques suit les types de requête standard pour les ressources de [par id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET), et [parcourir](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListsUsingGET).

### Par identifiant

[Requête par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByIdUsingGET) prend une seule liste dynamique ; `id` comme paramètre de chemin d’accès et renvoie un seul enregistrement de liste dynamique. Vous pouvez éventuellement transmettre la variable `includeRules` paramètre booléen permettant d’inclure des règles de liste dynamique dans la réponse.

![Règles de liste intelligente](assets/smartlist-rules.png)

```
GET /rest/asset/v1/smartList/{id}.json?includeRules=true
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default",
            "rules": {
                "filterMatchType": "all",
                "triggers": [],
                "filters": [
                    {
                        "id": 459,
                        "name": "Visited Web Page",
                        "ruleTypeId": 1,
                        "ruleType": "Activity",
                        "operator": "occurs",
                        "conditions": [
                            {
                                "activityAttributeId": 1,
                                "activityAttributeName": "Web Page",
                                "operator": "is",
                                "values": [
                                    "Program Test.Landing Page Test 01"
                                ],
                                "isPrimary": true
                            },
                            {
                                "activityAttributeId": 6,
                                "activityAttributeName": "Browser",
                                "operator": "is",
                                "values": [
                                    "Chrome"
                                ],
                                "isPrimary": false
                            },
                            {
                                "activityAttributeId": -101,
                                "activityAttributeName": "Date of Activity",
                                "operator": "in past",
                                "values": [
                                    "30 days"
                                ],
                                "isPrimary": false
                            }
                        ]
                    }
                ]
            }
        }
    ]
}
```

### Par identifiant de campagne dynamique

[Requête par identifiant de campagne dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartListBySmartCampaignIdUsingGET) prend une seule campagne dynamique ; `id` comme paramètre de chemin d’accès et renvoie un seul enregistrement de liste dynamique. Vous pouvez éventuellement transmettre la variable `includeRules` paramètre booléen permettant d’inclure des règles de liste dynamique dans la réponse.

```
GET /rest/asset/v1/smartCampaign/{smartCampaignId}/smartList.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default"
         }
    ]
}
```

### Par ID de programme

[Requête par identifiant de programme](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getSmartListByProgramIdUsingGET) prend un seul programme de messagerie ; `id` comme paramètre de chemin d’accès et renvoie un seul enregistrement de liste dynamique. Vous pouvez éventuellement transmettre la variable `includeRules` paramètre booléen permettant d’inclure des règles de liste dynamique dans la réponse.

```
GET /rest/asset/v1/program/{programId}/smartList.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6efc#16c8967a21f",
    "warnings": [],
    "result": [
        {
            "id": 4363,
            "name": "Smart List Test 01",
            "createdAt": "2019-06-03T23:01:13Z+0000",
            "updatedAt": "2019-06-04T17:37:45Z+0000",
            "url": "https://app-sjqe.marketo.com/#SL4363A1LA1",
            "folder": {
                "id": 1041,
                "type": "Program"
            },
            "workspace": "Default"
         }
    ]
}
```

### Par nom

[Requête par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) prend une liste dynamique ; `name` comme paramètre et renvoie un seul enregistrement de liste dynamique.  Une correspondance de chaîne exacte est exécutée par rapport à tous les noms de liste dynamique de l’instance et renvoie un résultat pour la liste dynamique correspondant à ce nom.

```
GET /rest/asset/v1/smartList/byName.json?name=2018 Leads
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "115d7#16423bc13b4",
    "result": [
        {
            "id": 283988,
            "name": "2018 Leads",
            "createdAt": "2008-10-07T15:20:39Z+0000",
            "updatedAt": "2010-04-13T15:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL283988A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

### Parcourir

Les listes dynamiques peuvent également [récupéré dans les lots](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListsUsingGET). La variable `folder` sert à spécifier le dossier parent sous lequel la requête est exécutée. Il est formaté en tant qu’objet JSON contenant `id` et `type`. Comme les autres points de fin de récupération de ressources en masse, `offset` et `maxReturn` sont des paramètres facultatifs qui peuvent être utilisés pour la pagination. Le paramètre facultatif `earliestUpdatedAt` et `latestUpdatedAt` Les paramètres datetime peuvent être utilisés pour filtrer les résultats par période UpdatedAt .

```
GET /rest/asset/v1/smartLists.json?folder={"id":31,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "9aa4#16423c0e969",
    "result": [
        {
            "id": 283988,
            "name": "2018 Leads",
            "createdAt": "2008-10-07T15:20:39Z+0000",
            "updatedAt": "2010-04-13T15:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL283988A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        },
        {
            "id": 299697,
            "name": "Active Prospects",
            "createdAt": "2008-10-17T02:09:49Z+0000",
            "updatedAt": "2010-03-27T18:27:46Z+0000",
            "url": "https://app-abm.marketo.com/#SL299697A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        },
        {
            "id": 400517,
            "name": "Leads by Score",
            "createdAt": "2009-01-07T18:52:52Z+0000",
            "updatedAt": "2010-04-13T15:36:09Z+0000",
            "url": "https://app-abm.marketo.com/#SL400517A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

## Cloner

[Clonage d’une liste dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/cloneSmartListUsingPOST) est exécuté avec un POST encodé en application/x-www-form-urlencoded. La liste dynamique à cloner est spécifiée dans la variable `id` paramètre path . La variable `folder` sert à spécifier le dossier parent sous lequel la liste dynamique sera créée et est formatée en tant qu’objet JSON contenant l’identifiant et le type . Le dossier parent doit être un dossier Program ou Smart List. La variable `name` est utilisé pour nommer la nouvelle liste dynamique et doit être unique. Si vous le souhaitez, la variable `description` peut être utilisé pour décrire la liste dynamique.

```
POST /rest/asset/v1/smartList/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
folder={"id":31,"type":"Folder"}&name=2018 Leads Qualified
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a672#16423d755ed",
    "result": [
        {
            "id": 788645,
            "name": "2018 Leads Qualified",
            "createdAt": "2018-06-21T19:34:32Z+0000",
            "updatedAt": "2018-06-21T19:34:32Z+0000",
            "url": "https://app-abm.marketo.com/#SL788645A1",
            "folder": {
                "id": 31,
                "type": "Folder"
            },
            "workspace": "Default"
        }
    ]
}
```

## Supprimer

[Suppression d’une liste dynamique](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/deleteSmartListByIdUsingPOST) prend une seule liste dynamique ; `id` comme paramètre de chemin d’accès.

```
POST /rest/asset/v1/smartList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "8f5#16423dd0fbe",
    "result": [
        {
            "id": 788645
        }
    ]
}
```
