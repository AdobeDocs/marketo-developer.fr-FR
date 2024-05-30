---
title: "Objets personnalisés"
feature: SOAP
description: '"Création d’objets personnalisés".'
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# Objets personnalisés

Un objet personnalisé Marketo permet la création d’une relation unique entre vos prospects Marketo et les enregistrements d’objets personnalisés. Vous pouvez, par exemple, effectuer le suivi de tous les défilés auxquels participe une piste. Comme les prospects peuvent assister à un certain nombre d’animations (sur plusieurs années), les objets personnalisés sont mieux adaptés pour stocker ces informations.

Les objets personnalisés sont pris en charge sur toutes les éditions Marketo, à l’exception de Spark. Lorsque l’objet personnalisé Marketo est correctement configuré dans votre compte, vous pouvez

- Créer/lire/mettre à jour/supprimer des enregistrements dans l’objet personnalisé via l’API Marketo SOAP
- Utiliser le déclencheur de liste dynamique lorsque de nouveaux enregistrements sont ajoutés à l’objet personnalisé
- Utiliser les données d’objet personnalisées comme filtre dans les listes dynamiques
- Utiliser les données d’objet personnalisées dans les emails à l’aide de Marketo Email Scripting

## Structure des objets personnalisés

Les objets personnalisés se composent comme suit :

- Un petit ensemble d’attributs fixes communs à tous les objets personnalisés :
   - Nom de l’objet (appelé Nom du type d’objet)
   - Description de l&#39;objet
   - Nom du champ Lien de piste personnalisé vers Marketo : il s’agit d’un champ de l’objet de piste (Personne) auquel l’objet personnalisé fait référence.
   - Nom du champ de clé d’objet : clé primaire utilisée par votre objet.
- Un ou plusieurs champs spécifiques à un objet (50 champs de ce type sont pris en charge)

## Opérations d’objet personnalisées

Les appels suivants peuvent être utilisés pour interagir avec les COs.

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
