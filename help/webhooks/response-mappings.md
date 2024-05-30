---
title: "Mappages des réponses"
feature: Webhooks
description: "Mappages des réponses pour Marketo"
source-git-commit: bcc0c0c8e8209cf9fb962a85c8e7da354d95a8fe
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---


# Response Mappings

Marketo peut traduire les données reçues par un webhook à partir de deux types de contenu et renvoyer ces valeurs dans un champ de piste : JSON et XML. Le paramètre de champ Marketo utilisera toujours la variable [Nom de l’API SOAP](../rest-api/fields.md) du champ . Chaque webhook peut comporter un nombre illimité de mappages de réponse, qui sont ajoutés et modifiés en cliquant sur le [!UICONTROL Modifier] dans le volet Correspondances de réponses de votre webhook :

![Correspondance des réponses](assets/response-mapping.png)

Les mappages de réponse sont créés via une association d’un &quot;attribut de réponse&quot;, le chemin d’accès à la propriété souhaitée dans le document XML ou JSON et le &quot;champ Marketo&quot;, qui spécifie le champ de piste dont la valeur est écrite à partir de l’attribut de réponse.

Les clés des propriétés doivent être constituées de caractères alphanumériques, de tirets (-), de traits de soulignement (_), de deux-points (:) et d’espaces blancs accessibles via les mappages de réponse Marketo.

## Mappages JSON

Les propriétés JSON sont accessibles avec notation par points et notation par tableaux. La notation de tableau dans Marketo n’accepte pas les chaînes en tant qu’entrée et accepte uniquement les entiers. Pour récupérer les données d’un document JSON, le type de réponse doit être défini sur JSON :

```json
{ "foo":"bar"}
```

Pour accéder au `foo` dans un mappage de réponse, utilisez la propriété `name` de la propriété, car elle se trouve au premier niveau de l’objet JSON, `foo`. Voici à quoi cela ressemble dans Marketo :

![Mappage des réponses](assets/json-resp.png)

Voici un exemple plus complexe avec un tableau :

```json
{
    "profileId" : 1234,
    "firstName" : "Jane",
    "lastName" : "Doe",
    "orders" : [
        {
            "orderId" : 5678,
            "orderDate" : "2015-01-01",
            "orderProductId" : "4982"
        },
        {
            "orderId" : 5678,
            "orderDate" : "2014-05-07",
            "orderProductId" : "4982"
        }
    ]
}
```

Nous voulons accéder à orderDate à partir du premier élément du tableau des commandes. Pour accéder à cette propriété, utilisez les méthodes suivantes : `orders[0].orderDate`

## Mappages XML

Les valeurs sont accessibles à partir d’éléments individuels dans des documents XML. La notation par points est similaire aux mappages JSON. Examinons cet exemple simple :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Pour accéder à la propriété foo ici, utilisez ce qui suit : `example.foo`

L’élément d’exemple doit d’abord être référencé avant d’accéder `foo`. Pour accéder à une propriété, tous les éléments de la hiérarchie doivent être référencés dans le mappage. Les documents XML avec des tableaux sont un peu plus complexes. Utilisez l’exemple suivant :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<elementList>
    <element>
        <foo>baz</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
</elementList>
```

Le document se compose du tableau parent. `elementList`, avec enfants, élément contenant une propriété : `foo`. Pour les mappages de réponse Marketo, le tableau est référencé comme `elementList.element`, de sorte que les enfants de elementList sont accessibles via `elementList.element[i]`. Pour obtenir la valeur de foo à partir du premier enfant d’elementList, nous utilisons cet attribut de réponse : `elementList.element[0].foo` Cela renvoie la valeur &quot;baz&quot; à notre champ désigné. Toute tentative d’accès aux propriétés à l’intérieur d’éléments qui contiennent des noms d’éléments uniques et non uniques entraîne un comportement indéfini. Chaque élément doit être une propriété unique ou un tableau, les types ne peuvent pas être mixtes.

## Types

Lors du mappage des attributs aux champs, vous devez vous assurer que le type de votre réponse webhook est compatible avec le champ cible. Par exemple, si la valeur de la réponse est une chaîne et que le champ sélectionné est de type entier, la valeur n’est pas écrite. En savoir plus [Types de champ](../rest-api/field-types.md).
