---
title: Response Mappings
feature: Webhooks
description: Mappages de réponse des Webhooks Marketo pour JSON et XML, mappage des attributs aux champs de prospect avec les noms d’API SOAP, la notation par points et par tableau, et compatibilité des types.
exl-id: 95c6e33e-487c-464b-b920-3c67e248d84e
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 1%

---

# Response Mappings

Marketo peut traduire les données reçues par un Webhook de deux types de contenu et renvoyer ces valeurs dans un champ de prospect : JSON et XML. Le paramètre Champ Marketo utilisera toujours le [nom d&#39;API SOAP](../rest-api/fields.md) du champ. Chaque Webhook peut avoir un nombre illimité de mappages de réponse, qui sont ajoutés et modifiés en cliquant sur le bouton [!UICONTROL Modifier] dans le volet Mappages de réponse de votre Webhook :

![Mappage-Réponse](assets/response-mapping.png)

Les mappages de réponse sont créés au moyen d’une association d’« attribut de réponse », du chemin d’accès à la propriété souhaitée dans le document XML ou JSON et du « champ Marketo », qui spécifie le champ de prospect auquel la valeur a été écrite à partir de l’attribut de réponse.

Les clés des propriétés doivent être composées de caractères alphanumériques, d’un tiret (-), d’un trait de soulignement (_), de deux-points (:) et d’un espace pour être accessibles via les mappages de réponse Marketo.

## Mappages JSON

Les propriétés JSON sont accessibles avec la notation par points et la notation par tableau. Dans Marketo, la notation de tableau n’accepte pas les chaînes en entrée et n’accepte que les entiers. Pour récupérer les données d’un document JSON, le type de réponse doit être défini sur JSON :

```json
{ "foo":"bar"}
```

Pour accéder à la propriété `foo` dans un mappage de réponse, utilisez la `name` de la propriété , car elle se trouve au premier niveau de l’objet JSON, `foo`. Voici à quoi cela ressemble dans Marketo :

![Mappage de réponse](assets/json-resp.png)

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

Nous voulons accéder à orderDate à partir du premier élément du tableau de commandes. Pour accéder à cette propriété, utilisez ce qui suit : `orders[0].orderDate`

## Mappings XML

Les valeurs sont accessibles à partir d’éléments individuels dans les documents XML. Cette méthode utilise la notation par points similaire aux mappages JSON. Examinons cet exemple simple :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Pour accéder à la propriété foo ici, utilisez ce qui suit : `example.foo`

L’élément d’exemple doit d’abord être référencé avant d’accéder à `foo`. Pour accéder à une propriété, tous les éléments de la hiérarchie doivent être référencés dans le mappage. Les documents XML avec des tableaux sont un peu plus compliqués. Utilisez l’exemple suivant :

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

Le document se compose du tableau parent `elementList`, avec enfants, l’élément qui contient une propriété : `foo`. Pour les besoins des mappages de réponse Marketo, le tableau est référencé comme `elementList.element`, de sorte que les enfants de l’élément elementList sont accessibles via `elementList.element[i]`. Pour obtenir la valeur de foo à partir du premier enfant de elementList, nous utilisons cet attribut de réponse : `elementList.element[0].foo`. La valeur « baz » est renvoyée à notre champ désigné. Si vous essayez d’accéder aux propriétés dans des éléments qui contiennent des noms d’éléments à la fois uniques et non uniques, le comportement est indéfini. Chaque élément doit être une propriété unique ou un tableau, les types ne peuvent pas être mélangés.

## Types

Lors du mappage des attributs aux champs, vous devez vous assurer que le type de votre réponse webhook est compatible avec le champ cible. Par exemple, si la valeur de la réponse est une chaîne et que le champ sélectionné est de type entier, la valeur n’est pas écrite. En savoir plus sur les [types de champs](../rest-api/field-types.md).
