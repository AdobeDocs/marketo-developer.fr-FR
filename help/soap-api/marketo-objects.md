---
title: Objets Marketo
feature: SOAP
description: Présentation des MObjects Marketo, y compris les types, les attributs, le comportement des clés externes et les API SOAP prises en charge pour les enregistrements d’opportunités, de programmes et connexes.
exl-id: 99b9aed4-94e8-46e8-84d9-2cc5215b0c13
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Objets Marketo

Marketo utilise des objets Marketo (MObjects) pour représenter différentes classes telles que Program, Opportunity et OpportunityPersonRole.

## Structure des MObjects

MObjects peut être l&#39;un des trois types suivants : Standard, Personnalisé ou Virtuel. Les MObjects standard et personnalisés représentent des entités distinctes, telles que Lead ou Company, tandis que les objets virtuels, tels que LeadRecord, sont composés de champs provenant d’un ou de plusieurs objets. Les objets virtuels sont des objets pratiques utilisés dans l’API mais n’existent pas dans l’application Marketo.

Les MObjects se composent des éléments suivants :

- Un petit ensemble d’attributs fixes communs à tous les MObjects
   - Type obligatoire
   - Clé externe facultative
   - id en lecture seule, createdAt, updatedAt
- Liste d’un ou de plusieurs attributs spécifiques à un objet, sous la forme de paires nom/valeur, dont certains peuvent être obligatoires. Par exemple, le nom sur l’opportunité.
- Une liste des références d’objet associées, telles que object-name plus
   - Marketo ID ou
   - Clé externe en tant que paire nom-attribut/valeur-attribut.

### External-Keys

Les clés externes sont des champs personnalisés définis sur les objets Marketo, tels que les leads ou les opportunités. Le nom est le nom du champ et la valeur est la valeur du champ, générée dans un système externe. **Marketo n&#39;applique pas de contrainte unique sur ces valeurs.** Il est de la responsabilité de l’utilisateur de l’API de s’assurer que les valeurs sont uniques. En cas de doublon, Marketo utilise l’objet ajouté le plus récemment. Ce comportement est similaire à celui du champ standard Adresse e-mail .

### API disponibles

| API | Peut Fonctionner Sur |
|---|---|
| defineMObject | ActivityRecord, LeadRecord, Opportunité, OpportunityPersonRole |
| getMObjects | Opportunité, Rôle de personne d’opportunité, Programme |
| syncMObjects | Opportunité, Rôle de personne d’opportunité, Programme |
| deleteMObjects | Opportunité, OpportunitéRôlePersonne |
| listMObjects | ActivityRecord, LeadRecord, Opportunité, OpportunityPersonRole |
