---
title: Objets personnalisés
feature: SOAP
description: Découvrez comment les objets personnalisés Marketo lient un prospect à de nombreux enregistrements, avec une structure, des limites et des appels d’API SOAP pour obtenir, synchroniser, supprimer, ainsi que pour l’utilisation de listes dynamiques et d’e-mails.
exl-id: 29d65841-4b44-4d94-b14e-c583d433d015
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# Objets personnalisés

Un objet personnalisé Marketo permet la création d’une relation un à plusieurs entre vos prospects Marketo et les enregistrements d’objet personnalisé. Vous pouvez, par exemple, effectuer le suivi de toutes les tournées auxquelles a assisté un prospect. Comme les prospects peuvent assister à un certain nombre de tournées de présentation (sur plusieurs années), les objets personnalisés sont mieux adaptés pour stocker ces informations.

Les objets personnalisés sont pris en charge sur toutes les éditions de Marketo, à l’exception de Spark. Lorsque l’objet personnalisé Marketo est correctement configuré dans votre compte, vous pouvez :

- Créer/Lire/Mettre à jour/Supprimer des enregistrements dans l’objet personnalisé via l’API Marketo SOAP
- Utiliser le déclencheur de liste dynamique lorsque de nouveaux enregistrements sont ajoutés à l’objet personnalisé
- Utilisation des données d’objet personnalisées comme filtre dans les listes dynamiques
- Utiliser les données d’objet personnalisées dans les e-mails à l’aide du script de messagerie Marketo

## Structure d’objets personnalisés

Les objets personnalisés se composent des éléments suivants :

- Un petit ensemble d’attributs fixes communs à tous les objets personnalisés :
   - Nom de l’objet (ou Nom du type d’objet)
   - Description de l&#39;objet
   - Nom de champ du lien de lead de l’objet personnalisé vers Marketo - Il s’agit d’un champ sur l’objet de lead (personne) auquel l’objet personnalisé fait référence
   - Nom du champ de clé de l’objet : la clé primaire utilisée par votre objet
- Un ou plusieurs champs spécifiques à un objet (nous prenons en charge un maximum de 50 champs de ce type)

## Opérations d’objet personnalisé

Les appels suivants peuvent être utilisés pour interagir avec des CO.

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
