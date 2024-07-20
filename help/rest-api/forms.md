---
title: Formulaires
feature: REST API, Forms
description: Créez et gérez des formulaires via l’API.
exl-id: 2e5dfa70-3163-4ab4-b269-3112417714c3
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 2%

---

# Formulaires

[Référence du point d’entrée Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms)

[Référence du point d’entrée des champs de formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields)

Les formulaires Marketo comportent un ensemble complexe de points de fin permettant un contrôle total de la gestion des formulaires à partir de systèmes distants. La structure des formulaires peut être complexe, car il existe de nombreux types d’objets différents qui doivent être gérés dans le cadre d’un formulaire : Forms, Champs, Jeux de champs, Règles de visibilité et Règles de page de suivi.

## Requête

Forms prend en charge les méthodes standard de récupération des ressources, [par id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET), [par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) et [par navigation](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET). Chaque réponse de formulaire contient toutes ses propriétés, à l’exception de sa liste de champs.

### Par identifiant

[Obtenir le formulaire par identifiant](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET) prend un formulaire `id` comme paramètre de chemin d’accès et renvoie un enregistrement de formulaire.

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

[Obtenir le formulaire par nom](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) prend un formulaire `name` comme paramètre de chemin d’accès et renvoie un enregistrement de formulaire.

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

[Get Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET) forms fonctionne comme d’autres points de terminaison de navigation de l’API Asset et permet un filtrage facultatif sur `status`, `maxReturn` et `offset`. L’état peut être : approuvé, approuvé avec brouillon ou brouillon.

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

La récupération de la liste des champs d’un formulaire s’effectue sur la base du formulaire.

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

Lors de la modification de champs ou de leur comportement dans un formulaire, la liste de champs doit toujours être récupérée avant d’effectuer des modifications. Cela vous garantit d’attribuer l’identifiant de champ approprié lors de la mise à jour ou de la suppression.

### Types de champ

| Type d’interface utilisateur | Nom de l&#39;API |
|--------------|-----------------|
| Cases à cocher | case à cocher |
| Bouton radio | radio |
| Zone de texte | zone de texte |
| Liste de sélection | liste de sélection |
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

Le point de terminaison [Get Form Used By](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getFormUsedByUsingGET) utilise un formulaire `id` comme paramètre de chemin d’accès et renvoie la liste des ressources qui dépendent du formulaire. Forms peut être utilisé par les types de ressources suivants : pages d’entrée, listes dynamiques, campagnes dynamiques, rapports, programmes de messagerie électronique.

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

Lors de la [création d’un formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/createLpFormsUsingPOST), il n’y a que deux champs obligatoires : le dossier parent du formulaire, le nom du formulaire. Tous les autres paramètres sont facultatifs avec la valeur par défaut. Lors de la création du formulaire, trois champs sont proposés par défaut : Prénom, Nom, Email.

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

Les Forms sont [mises à jour](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormsUsingPOST) avec un appel similaire via leur identifiant. Lors de la création ou de la mise à jour, les paramètres de style de base sont accessibles et modifiables, ce qui vous permet de modifier l’affichage du formulaire pour l’utilisateur final.

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

Les comportements de page de remerciement et de visiteur connus ne peuvent pas être modifiés par le biais des appels de formulaire de création ou de mise à jour et doivent être accessibles via leurs points de terminaison respectifs.

## Métadonnées de champ

Pour ajouter ou modifier correctement des champs appartenant à un formulaire, vous devez récupérer la liste des champs valides pour l&#39;instance cible. Les interactions de champ sont toujours effectuées en fonction de la propriété d’identifiant du champ qui s’affiche pour chaque élément du résultat.

Pour les champs de piste, cette opération s’effectue à l’aide du point de terminaison [Obtenir les champs de formulaire disponibles](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllFieldsUsingGET) et inclut le type de données et les métadonnées par défaut du champ lorsqu’il est ajouté à un formulaire.

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

Pour les champs personnalisés de membre de programme, appelez [Obtenir les champs de membre de programme de formulaire disponibles](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllProgramMemberFieldsUsingGET)  point de terminaison pour récupérer les types de données de champ personnalisés des membres du programme et les métadonnées par défaut. Pour utiliser ces champs dans un formulaire, le formulaire doit se trouver sous un programme (et non dans Design Studio). Les pages d’entrée contenant des formulaires utilisant ces champs doivent également se trouver sous un programme (elles ne peuvent pas résider dans Design Studio ou être clonées dans Design Studio).

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

Chaque formulaire contient une liste de champs modifiable qui s’affichera à l’utilisateur final lors du chargement. Chaque champ est ajouté, mis à jour ou supprimé de la liste des champs un par un via leurs points de terminaison respectifs.

[L&#39;ajout d&#39;un champ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldToAFormUsingPOST) nécessite uniquement l&#39;identifiant du formulaire parent et le fieldId du champ. Tous les autres champs seront vides ou auront des valeurs par défaut basées sur leur type de données et leurs métadonnées de champ. Les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

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

Les mises à jour peuvent modifier tous les mêmes champs que l’ajout d’un champ. De même, elles nécessitent l’identifiant de formulaire et l’identifiant de champ, sauf que le paramètre fieldId est un paramètre de chemin d’accès et non un paramètre de requête lors de l’exécution de mises à jour.

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

Dans l’exemple ci-dessus, nous mettons à jour le champ Nom qui est une chaîne simple. Certains champs de formulaire sont plus complexes. Par exemple, le champ Civilité est un type de champ &quot;select&quot; contenant la liste des éléments et une valeur par défaut. Si vous ajoutez ou mettez à jour un champ de type sélectionné, à moins que vous n’ayez défini l’un des choix pour avoir la valeur `isDefault` true, alors le premier choix n’a aucune valeur et être étiqueté &quot;Sélectionner...&quot;.

![Civilité](assets/form-field-salutation.png)

Pour mettre à jour les éléments de la liste, le format du paramètre &quot;values&quot; est le suivant :

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

 

Pour déterminer le format d’un champ de formulaire complexe, examinez la réponse de l’option Ajouter un champ au formulaire.

### Réorganisation du champ

Les champs d’un formulaire doivent être réorganisés en une seule unité par l’intermédiaire du point de terminaison [Modifier les positions des champs de formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Le point de terminaison requiert un paramètre appelé `positions`, qui est un tableau JSON d’objets comportant trois membres :

- columnNumber
- rowNumber
- fieldName (fait référence à l’identifiant du champ)

Les champs d’un formulaire sont organisés dans une interface de type tableau, avec trois colonnes au maximum et dix lignes au maximum. La ligne et la colonne sont indexées à partir de 0, de sorte que la première ligne et la première colonne sont toutes deux indiquées en transmettant un 0. Tous les champs doivent occuper une position unique

Si le champ cible est également un champ, son enregistrement dans le tableau positions doit également contenir un paramètre appelé fieldList, un tableau d’objets contenant les mêmes membres columnNumber, rowNumber et fieldName. Le champ lui-même est traité comme un champ unique pour sa position dans la liste parent, tandis que ses sous-champs sont positionnés en fonction des positions données dans le paramètre fieldList.

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

Les champs de texte enrichi sont ajoutés par l’intermédiaire d’un [point d’entrée distinct](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addRichTextFieldUsingPOST) à partir des champs de piste. Le contenu du champ est transmis en multipart/form-data. Il doit être structuré comme du contenu HTML qui ne contient aucun script, balise meta ou balise de lien.

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

Les formulaires Marketo comportent un composant facultatif appelé jeux de champs. Les jeux de champs sont des groupes de champs qui sont traités comme un champ unique dans la liste de champs de niveau supérieur à des fins de mouvement et de traitement par les règles de visibilité. Par exemple, s’il existe un champ pour les exigences de conformité et qu’un client sélectionne oui, il peut révéler un jeu de champs contenant des champs pour les exigences de conformité HIPAA et PCI.

Les champs des jeux de champs sont propres au formulaire dans son ensemble. Par conséquent, les champs en double peuvent ne pas se trouver dans la liste des champs parents du formulaire et dans un jeu de champs enfant. Les jeux de champs sont ajoutés via le point de terminaison [Ajouter un jeu de champs au formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldSetUsingPOST) et apparaîtront alors dans le résultat de [Obtenir des champs pour le formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getFormFieldByFormVidUsingGET). Les champs sont ajoutés à un jeu de champs en les déplaçant dans le fieldList du jeu de champs via [Mettre à jour les positions des champs](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Pour ces points de terminaison, les données sont transmises en tant que POST x-www-form-urlencoded, et non en tant que JSON.

## Règle de visibilité

Chaque champ peut comporter un ensemble de règles de visibilité qui déterminent si le champ peut être affiché par un visiteur selon les valeurs qu’il a entrées dans le formulaire. Les règles effectuent une comparaison entre la valeur d’un subjectField présent dans le formulaire et une liste de valeurs données dans la règle. Chaque champ peut avoir un type de règle de visibilité, afficher, masquer ou toujours afficher, puis une liste de règles à évaluer. Les règles sont évaluées de haut en bas, et la première règle qui s’avère vraie est celle qui sera appliquée.

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

Pour obtenir la liste complète des opérateurs disponibles, reportez-vous à la page de référence des points de fin de la fonction [Ajouter des règles de visibilité des champs de formulaire](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFormFieldVisibilityRuleUsingPOST).

## Abonnement

Les formulaires Marketo peuvent avoir un comportement de page de suivi dynamique dans lequel les règles de redirection vers une page donnée ou de maintien sur la page active peuvent être appliquées en fonction du contenu des champs désignés lors de l’envoi. Les règles peuvent être appelées de manière interchangeable Règles de page de remerciement ou Règles de page de suivi . Ces règles sont représentées sous la forme d’un tableau JSON avec les membres `followupType`, `followupValue`, `operator`, `subjectField`, `values` et `default`. `default` est une valeur booléenne pour laquelle un seul enregistrement du tableau peut être vrai. Lorsqu’un visiteur n’est admissible pour aucune autre règle, la règle désignée comme valeur par défaut est utilisée. `followupType` peut être lp ou url, où lp indique un identifiant de page d’entrée Marketo pour `followupValue` et l’url indique une URL vers une autre page. L&#39;opérateur est utilisé pour comparer la valeur du champ d&#39;objet à la liste des valeurs fournies.

## Bouton d&#39;envoi

Le style du bouton d’envoi du formulaire est géré avec le point de terminaison [Mettre à jour le bouton d’envoi](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormSubmitButtonUsingPOST). Les variables buttonPosition, buttonStyle, label et waitLabel (le libellé affiché pendant l’envoi est en attente) peuvent être modifiées.

Il s’agit d’une mise à jour destructrice.

## Validation

Comme la plupart des autres actifs, les formulaires suivent un modèle approuvé par brouillon, où il peut y avoir une version préliminaire et/ou une version approuvée. Chaque fois que des mises à jour sont appliquées à un formulaire, elles sont toujours appliquées en premier à la version préliminaire et ne sont visibles en direct que lorsque le formulaire a été approuvé. L’approbation d’un formulaire utilise la version préliminaire actuelle et remplace la version approuvée, le cas échéant, par le brouillon. Si le formulaire doit être retiré de la version en direct, il doit d’abord être non approuvé, ce qui supprime tous les brouillons en cours, puis rétrograder la version approuvée à un état de brouillon uniquement. Forms doit toujours être non approuvé avant de tenter la suppression.

## Profilage progressif

Lorsque le profilage progressif est activé pour un formulaire, un jeu de champs appelé &quot;Profilage&quot; est inclus dans sa liste de champs. Pour ajouter ou supprimer des champs de la liste de profilage progressif, vous devez utiliser le point de terminaison Mettre à jour les positions des champs . Ce point de terminaison effectue des mises à jour destructives. Par conséquent, tous les champs du formulaire doivent être inclus dans chaque requête. L’exemple ci-dessous ajoute le champ &quot;Téléphone&quot; à la liste de profilage progressif.

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
