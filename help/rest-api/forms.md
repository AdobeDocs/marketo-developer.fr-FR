---
title: Formulaires
feature: REST API, Forms
description: Guide de l’API REST Marketo Forms pour la création et la gestion des formulaires, la récupération par identifiant ou nom, la navigation avec des filtres de statut et la gestion des champs, des ensembles de champs et des règles.
exl-id: 2e5dfa70-3163-4ab4-b269-3112417714c3
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1616'
ht-degree: 2%

---

# Formulaires

[Référence de point d’entrée Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms)

[Référence du point d’entrée des champs de formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields)

Les formulaires Marketo comportent un ensemble complexe de points d’entrée permettant un contrôle complet de la gestion des formulaires à partir de systèmes distants. La structure des formulaires peut être complexe, car de nombreux types d’objets différents doivent être gérés dans le cadre d’un formulaire : Forms, Champs, Ensembles de champs, Règles de visibilité et Règles de page de suivi.

## Requête

Forms prend en charge les méthodes standard de récupération des ressources [par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) et [par navigation](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET). Chaque réponse de formulaire contient toutes ses propriétés, à l’exception de sa liste de champs.

### Par ID

[Obtenir le formulaire par ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET) prend un `id` de formulaire en tant que paramètre de chemin d’accès et renvoie un enregistrement de formulaire.

```
GET /rest/asset/v1/form/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Par nom

[Obtenir le formulaire par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) prend un `name` de formulaire en tant que paramètre de chemin d’accès et renvoie un enregistrement de formulaire.

```
GET /rest/asset/v1/form/byName.json?name=newForm
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Parcourir

[Obtenir Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET) Forms fonctionne comme les autres points d’entrée de navigation de l’API de ressources et permet un filtrage facultatif sur les `status`, les `maxReturn` et les `offset`. Le statut peut être : approuvé, approuvé avec un brouillon ou brouillon.

```
GET /rest/asset/v1/forms.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "645d#154e3d499ac",
    "result": [
        {
            "id": 227,
            "name": "aKAUVDfbsX",
            "description": "",
            "createdAt": "2016-05-18T20:36:20Z+0000",
            "updatedAt": "2016-05-18T20:36:20Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO227B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        },
        {
            "id": 695,
            "name": "AoMXgfFbma",
            "description": "",
            "createdAt": "2016-05-19T18:50:40Z+0000",
            "updatedAt": "2016-05-19T18:50:40Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO695B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 565,
                "folderName": "WfUvYmlcyT"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

### Liste de champs

La récupération de la liste des champs d’un formulaire s’effectue par formulaire.

```
GET /rest/asset/v1/form/{id}/fields.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2165#154eee00d01",
    "result": [
        {
            "id": "FirstName",
            "label": "First Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "LastName",
            "label": "Last Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 1,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Email",
            "label": "Email Address:",
            "dataType": "email",
            "validationMessage": "Must be valid email. <span class='mktoErrorDetail'>example@yourdomain.com</span>",
            "rowNumber": 2,
            "columnNumber": 0,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Profiling",
            "dataType": "profiling",
            "rowNumber": 3,
            "columnNumber": 0
        }
    ]
}
```

Lors de la modification de champs ou de leur comportement dans un formulaire, la liste de champs doit toujours être récupérée avant toute tentative de modification. Cela vous permet de donner l’identifiant de champ approprié lors de la mise à jour ou de la suppression.

### Types de champs

| Type d’interface utilisateur | Nom de l&#39;API |
|--------------|-----------------|
| Cases à cocher | case à cocher |
| Bouton Radio | radio |
| Zone de texte | zone de texte |
| Liste de sélection | liste à sélection |
| Chaîne | Chaîne |
| E-mail | E-mail |
| Date | Date |
| Nombre | Nombre |
| Double | double |
| Téléphone | téléphone |
| URL | url |
| Devise | currency |
| Case à cocher | single_checkbox |
| Curseur | gamme |

### Dépendances

Le point d’entrée [Obtenir le formulaire utilisé par](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getFormUsedByUsingGET) prend un formulaire `id` comme paramètre de chemin d’accès et renvoie la liste des ressources qui dépendent du formulaire. Forms peut être utilisé par les types de ressources suivants : Pages de destination, Listes dynamiques, Campagnes intelligentes, Rapports, Programmes de messagerie électronique.

```
GET /rest/asset/v1/form/{id}/usedBy.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fdf4#17285b25038",
    "warnings": [],
    "result": [
        {
            "id": 1038,
            "name": "LP Redirect Rules Program.LP Test 01",
            "type": "Landing Page",
            "status": "approved",
            "updatedAt": "2020-02-23T01:31:21Z+0000"
        }
    ]
}
```

## Créer et mettre à jour

Lors de la [création d’un formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/createLpFormsUsingPOST) il n’y a que deux champs obligatoires : le dossier parent du formulaire, le nom du formulaire. Tous les autres paramètres sont facultatifs avec la valeur par défaut. Lorsque le formulaire est créé, il est fourni avec trois champs par défaut : Prénom, Nom, E-mail.

```
POST /rest/asset/v1/forms.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=newForm&description=test&folder={"type": "Folder","id": 293}&language=French
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

Les Forms sont [mises à jour](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormsUsingPOST) avec un appel similaire via leur identifiant. Lors de la création ou de la mise à jour, les paramètres de style de base sont accessibles et modifiables, ce qui vous permet de modifier la manière dont le formulaire est affiché pour l’utilisateur final.

```
POST /rest/asset/v1/form/736.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=updated name&description=This is a test for updateapi&language=English&progressiveProfiling=true&locale=en_US
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154e3cf6efe",
    "result": [
        {
            "id": 736,
            "name": "updated name",
            "description": "This is a test for update api",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:28:23Z+0000",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

Les comportements connus du visiteur et de la page de remerciement ne peuvent pas être modifiés par le biais des appels de création ou de mise à jour de formulaire et doivent être accessibles via leurs points d’entrée respectifs.

## Métadonnées de champ

Pour ajouter ou modifier correctement des champs appartenant à un formulaire, vous devez récupérer la liste des champs valides pour l’instance cible. Les interactions avec les champs sont toujours effectuées en fonction de la propriété d’identifiant du champ qui est affichée pour chaque élément dans le résultat.

Pour les champs de prospect, cette opération s’effectue à l’aide du point d’entrée [Obtenir les champs de formulaire disponibles](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllFieldsUsingGET) et inclut le type de données et les métadonnées par défaut du champ lorsqu’il est ajouté à un formulaire.

```
GET /rest/asset/v1/form/fields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "176ca#167a9808f4c",
    "warnings": [],
    "result": [
        {
            "id": "AnnualRevenue",
            "isRequired": false,
            "dataType": "currency"
        },
        {
            "id": "City",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Company",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Country",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Description",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 32000,
            "visibleRows": 2
        },
        {
            "id": "Email",
            "isRequired": false,
            "dataType": "email"
        },
        {
            "id": "Fax",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "FirstName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Industry",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LastName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LeadSource",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "MobilePhone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "NumberOfEmployees",
            "isRequired": false,
            "dataType": "int"
        },
        {
            "id": "Phone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "PostalCode",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Rating",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Salutation",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "Mr.,Ms.,Mrs.,Dr.,Prof."
        },
        {
            "id": "State",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "AK::AK,AL::AL,AR::AR,AZ::AZ,CA::CA,CO::CO,CT::CT,DE::DE,FL::FL,GA::GA,HI::HI,IA::IA,ID::ID,IL::IL,IN::IN,KS::KS,KY::KY,LA::LA,MA::MA,MD::MD,ME::ME,MI::MI,MN::MN,MO::MO,MS::MS,MT::MT,NC::NC,ND::ND,NE::NE,NH::NH,NJ::NJ,NM::NM,NV::NV,NY::NY,OH::OH,OK::OK,OR::OR,PA::PA,RI::RI,SC::SC,SD::SD,TN::TN,TX::TX,UT::UT,VA::VA,VT::VT,WA::WA,WI::WI,WV::WV,WY::WY"
        },
        {
            "id": "Street",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 2000,
            "visibleRows": 2
        },
        {
            "id": "Title",
            "isRequired": false,
            "dataType": "picklist"
        }
    ]
}
```

Pour les champs personnalisés de membre de programme, appelez [Obtenir les champs de membre de programme disponibles](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllProgramMemberFieldsUsingGET)  Point d’entrée pour récupérer les types de données de champ personnalisé du membre de programme et les métadonnées par défaut. Pour utiliser ces champs dans un formulaire, le formulaire doit résider sous un programme (et non dans Design Studio). Les landing pages contenant des formulaires utilisant ces champs doivent également résider sous un programme (ne peuvent pas résider dans Design Studio ni être clonées dans Design Studio).

```
GET /rest/asset/v1/form/programMemberFields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "109c6#16fa0b9c51a",
    "warnings": [],
    "result": [
        {
            "id": "pMCFCustomField01",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "pMCFCustomField02",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "myPMCF",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        }
    ]
}
```

### Modifier le champ

Chaque formulaire contient une liste modifiable de champs qui sera affichée à l’utilisateur final au chargement. Chaque champ est ajouté, mis à jour ou supprimé de la liste de champs un par un via leurs points d’entrée respectifs.

[L’ajout d’un champ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldToAFormUsingPOST) ne nécessite que l’identifiant du formulaire parent et l’fieldId du champ. Tous les autres champs seront vides ou auront des valeurs par défaut en fonction de leur type de données et de leurs métadonnées de champ. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

```
POST /rest/asset/v1/form/{id}/fields.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
fieldId=NumberOfEmployees&maxLength=125&defaultValue=this is default&required=true&fieldWidth=100&validationMessage=hey, you there?&label=employee count&hintText=Hint me&minValue=10
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1826e#154f41b214c",
    "result": [
        {
            "id": "NumberOfEmployees",
            "label": "employee count",
            "fieldWidth": 100,
            "dataType": "number",
            "defaultValue": "this is default",
            "validationMessage": "hey, you there?",
            "rowNumber": 5,
            "columnNumber": 0,
            "required": true,
            "formPrefill": true,
            "fieldMetaData": {
                "minValue": 10,
                "maxValue": null
            },
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "hintText": "Hint me"
        }
    ]
}
```

Les mises à jour peuvent modifier tous les mêmes champs que l’ajout d’un champ et nécessiter de la même manière l’ID de formulaire et l’ID de champ, sauf que l’ID de champ est un paramètre de chemin et non de requête lors de l’exécution de mises à jour.

```
POST /rest/asset/v1/form/{id}/field/LastName.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
label=enter the last name here
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5634#15508303abb",
    "result": [
        {
            "id": "LastName",
            "label": "enter the last name here",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        }
    ]
}
```

Dans l’exemple ci-dessus, nous mettons à jour le champ LastName qui est une chaîne simple. Certains champs de formulaire sont plus complexes. Par exemple, le champ Salutation est un type de champ « select » contenant la liste d’éléments et une valeur par défaut. Si vous ajoutez ou mettez à jour un champ de type Sélection, à moins que vous ne définissiez l’un des choix pour qu’il ait une valeur `isDefault` true, le premier choix n’a aucune valeur et est intitulé « Sélectionner... »

![&#x200B; Salutation &#x200B;](assets/form-field-salutation.png)

Pour mettre à jour les éléments de la liste, le format du paramètre « values » est le suivant :

```
POST /rest/asset/v1/form/{id}/field/Salutation.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
values=[{"label":"Select...","value":"","isDefault":true,"selected":true}, {"label":"MR","value":"MR"}, {"label":"MS","value":"MS"}, {"label":"MRS","value":"MRS"}, {"label":"DR","value":"DR"}, {"label":"PROF","value":"PROF"}]
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "71fd#1588d9d1b0c",
  "result": [
    {
      "id": "Salutation",
      "label": "Salutation:",
      "dataType": "select",
      "validationMessage": "This field is required.",
      "rowNumber": 3,
      "columnNumber": 0,
      "required": false,
      "formPrefill": true,
      "fieldMetaData": {
        "multiSelect": false,
        "values": [
          {
            "label": "Select...",
            "value": "",
            "isDefault": true,
            "selected": true
          },
          {
            "label": "MR",
            "value": "MR"
          },
          {
            "label": "MS",
            "value": "MS"
          },
          {
            "label": "MRS",
            "value": "MRS"
          },
          {
            "label": "DR",
            "value": "DR"
          },
          {
            "label": "PROF",
            "value": "PROF"
          }
        ],
        "visibleLines": 1
      },
      "visibilityRules": {
        "ruleType": "alwaysShow"
      }
    }
  ]
}
```

Pour déterminer comment formater un champ de formulaire complexe, examinez la réponse de l’onglet Ajouter un champ à un formulaire .

### Réorganisation du champ

Les champs d’un formulaire doivent être réorganisés tous en une seule unité via le point d’entrée [Modifier la position des champs de formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Le point d’entrée nécessite un paramètre appelé `positions`, qui est un tableau JSON d’objets avec trois membres :

- columnNumber
- rowNumber
- fieldName (fait référence à l’identifiant du champ)

Les champs d’un formulaire sont organisés en une interface de type tableau, avec jusqu’à trois colonnes et dix lignes. La ligne et la colonne sont indexées à partir de 0, de sorte que la première ligne et la première colonne sont toutes deux indiquées en transmettant un 0. Tous les champs doivent occuper une position unique

Si le champ cible est également un jeu de champs, son enregistrement dans le tableau des positions doit également contenir un paramètre appelé fieldList, un tableau d’objets contenant les mêmes membres columnNumber, rowNumber et fieldName. L’ensemble de champs lui-même est traité comme un champ unique pour sa position dans la liste parente, tandis que ses sous-champs sont positionnés en fonction des positions données dans le paramètre fieldList.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"FirstName"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"}, {"columnNumber":0,"rowNumber":2, "fieldName":"Email"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bb18#15508ef9c04",
    "result": [
        {
            "id": 764
        }
    ]
}
```

### Texte complet

Les champs de texte enrichi sont ajoutés via un [point d’entrée distinct](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addRichTextFieldUsingPOST) à partir des champs de prospect. Le contenu du champ est transmis en tant que données multipartie/formulaire. Il doit être structuré en tant que contenu HTML qui ne contient pas de script, de balises méta ou de balises de lien.

```
POST /rest/asset/v1/form/{id}/richText.json
```

```
Content-Type: multipart/form-data; boundary=---------------------------9051914041544843365972754266
-----------------------------9051914041544843365972754266
Content-Disposition: form-data; name="text"
Content-Type: text/html
<div>Fancy Rich Text Component</div>
-----------------------------9051914041544843365972754266--
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "82c8#154f423bf5c",
    "result": [
        {
            "id": "SHRtbFRleHRfMjAxNi0wNS0yN1QxNDozNDoyNC4xMTVa",
            "labelWidth": 260,
            "dataType": "htmltext",
            "rowNumber": 8,
            "columnNumber": 0,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "text": "<div>Fancy Rich Text Component</div>"
        }
    ]
}
```

### Jeu de champs

Les formulaires Marketo comportent un composant facultatif appelé fieldsets. Les jeux de champs sont des groupes de champs qui sont traités comme un seul champ dans la liste de champs de niveau supérieur à des fins de déplacement et de traitement par les règles de visibilité. Par exemple, s’il existe un champ pour les exigences de conformité et qu’un client sélectionne oui, il peut afficher un jeu de champs contenant des champs pour les exigences de conformité HIPAA et PCI.

Les champs dans les jeux de champs sont propres au formulaire dans son ensemble. Par conséquent, les champs en double peuvent ne pas être dans la liste des champs parents du formulaire et dans un jeu de champs enfant. Les jeux de champs sont ajoutés via le point d’entrée [Ajouter un jeu de champs au formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldSetUsingPOST) et apparaissent ensuite dans le résultat de [Obtenir les champs du formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getFormFieldByFormVidUsingGET). Les champs sont ajoutés à un jeu de champs en les déplaçant dans la fieldList du jeu de champs via [Mettre à jour la position des champs](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Pour ces points d’entrée, les données sont transmises au format POST x-www-form-urlencoded, et non au format JSON.

## Règle De Visibilité

Chaque champ peut comporter un ensemble de règles de visibilité qui déterminent si le champ peut être affiché par un visiteur ou une visiteuse en fonction des valeurs qu’il ou elle a saisies dans le formulaire. Les règles effectuent une comparaison entre la valeur d’un subjectField présent dans le formulaire et une liste de valeurs donnée dans la règle. Chaque champ peut avoir un type de règle de visibilité, afficher, masquer ou alwaysShow, puis une liste de règles à évaluer. Les règles sont évaluées de haut en bas, et la première règle qui est évaluée comme vraie est celle qui sera appliquée.

La modification des règles de visibilité est une mise à jour destructrice.

```
POST /rest/asset/v1/form/{id}/field/Email/visibility.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
visibilityRule={"ruleType":"show", "rules":[{"subjectField": "LastName", "operator": "isNotEmpty", "values": [], "altLabel": "Email:"}]}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "ab4a#15509030601",
    "result": [
        {
            "formFieldId": "Email",
            "ruleType": "show",
            "rules": [
                {
                    "subjectField": "LastName",
                    "operator": "isNotEmpty",
                    "values": [],
                    "altLabel": "Email:"
                }
            ]
        }
    ]
}
```

Pour obtenir la liste complète des opérateurs disponibles, consultez la page de référence des points d’entrée pour [Ajouter des règles de visibilité des champs de formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFormFieldVisibilityRuleUsingPOST).

## Suivi

Les formulaires Marketo peuvent avoir un comportement de page de suivi dynamique dans lequel les règles de redirection vers une page donnée ou de maintien sur la page active peuvent être appliquées en fonction du contenu des champs désignés lors de l’envoi. Les règles peuvent être appelées règles de page de remerciement ou règles de page de suivi de manière interchangeable. Ces règles sont représentées sous la forme d’un tableau JSON avec les membres `followupType`, `followupValue`, `operator`, `subjectField`, `values` et `default`. `default` est une valeur booléenne pour laquelle un seul enregistrement du tableau peut être vrai. Lorsqu’un visiteur se qualifie pour aucune autre règle, la règle désignée comme par défaut est utilisée. `followupType` peut être lp ou url, où lp indique un ID de page de destination Marketo pour `followupValue`, et url indique une URL vers une autre page. L’opérateur est utilisé pour comparer la valeur du champ d’objet à la liste de valeurs fournies.

## Bouton d&#39;envoi

Le style du bouton Envoyer du formulaire est géré à l’aide du point d’entrée [Mettre à jour le bouton Envoyer](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormSubmitButtonUsingPOST). Les éléments buttonPosition, buttonStyle, label et waitLabel (le libellé affiché lorsque l’envoi est en attente) peuvent être modifiés.

Il s’agit d’une mise à jour destructrice.

## Validation

Comme la plupart des autres ressources, les formulaires suivent un modèle approuvé pour le brouillon, dans lequel il peut y avoir un brouillon et/ou une version approuvée. Chaque fois que des mises à jour sont appliquées à un formulaire, elles sont toujours appliquées en premier au brouillon et ne sont affichées en direct que lorsque le formulaire a été approuvé. L’approbation d’un formulaire utilise le brouillon actuel et remplace la version approuvée, le cas échéant, par le brouillon. Si le formulaire doit être retiré de la mise en ligne, il doit d’abord être désapprouvé, ce qui supprime tous les brouillons actuels, et rétrograder la version approuvée au statut brouillon uniquement. L’approbation de Forms doit toujours être annulée avant toute tentative de suppression.

## Profilage progressif

Lorsque le profilage progressif est activé pour un formulaire, un jeu de champs appelé « Profilage » est inclus dans sa liste de champs. Pour ajouter ou supprimer des champs de la liste de profilage progressif, vous devez utiliser le point d’entrée Mettre à jour les positions de champ . Ce point d’entrée effectue des mises à jour destructives. Par conséquent, tous les champs du formulaire doivent être inclus dans chaque requête. L’exemple ci-dessous ajoute le champ « Téléphone » à la liste de profilage progressif.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"Email"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"},{"columnNumber":0,"rowNumber":2,"fieldName":"Company"},{"columnNumber":0,"rowNumber":3,"fieldName":"Website"},{"columnNumber":0,"rowNumber":4,"fieldName":"Profiling","fieldList":[{"columnNumber":0,"rowNumber":0,"fieldName":"Phone"}]}]
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d6a#164190dbdf2",
    "result": [
        {
            "id": 1031
        }
    ]
}
```
