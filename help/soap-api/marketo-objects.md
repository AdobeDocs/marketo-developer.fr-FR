---
title: "Objets Marketo"
feature: SOAP
description: "Présentation des objets Marketo"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---


# Objets Marketo

Marketo utilise des objets Marketo (MObjects) pour représenter différentes classes telles que Program, Opportunity et OpportunityPersonRole.

## Structure des objets MO

Les MObjects peuvent être de trois types : Standard, Personnalisé ou Virtuel. Les MObjects standard et personnalisés représentent des entités distinctes, telles que &quot;prospect&quot; ou &quot;société&quot;, tandis que les objets virtuels, tels que &quot;enregistrement de piste&quot;, sont composés de champs provenant d’un ou de plusieurs objets. Les objets virtuels sont des objets pratiques utilisés dans l’API, mais n’existent pas dans l’application Marketo.

Les MObjects se composent des éléments suivants :

- Un petit ensemble d’attributs fixes communs à tous les MObjects
   - Type requis
   - ExternalKey facultatif
   - id en lecture seule, createdAt, updatedAt
- Une liste d’un ou plusieurs attributs spécifiques à l’objet, sous forme de paires nom/valeur, dont certains peuvent être obligatoires. Par exemple, nommez sur Opportunity.
- Une liste de références d’objet associées, sous la forme object-name plus
   - Marketo ID ou
   - Clé externe en tant que paire nom-attribut/valeur-attribut.

### External-Keys

Les clés externes sont des champs personnalisés définis sur des objets Marketo, tels que &quot;prospect&quot; ou &quot;opportunité&quot;. Le nom est le nom du champ et la valeur est la valeur du champ, générée dans un système externe. **Marketo n’impose pas de contrainte unique sur ces valeurs.** Il est de la responsabilité de l’utilisateur de l’API de s’assurer que les valeurs sont uniques. Si un doublon se produit, Marketo utilise l’objet ajouté le plus récemment. Ceci est similaire au comportement du champ standard Adresse électronique .

### API disponibles

| API | Peut fonctionner sur |
|---|---|
| descriptionMObject | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
| getMObjects | Opportunity, OpportunityPersonRole, Program |
| syncMObjects | Opportunity, OpportunityPersonRole, Program |
| deleteMObjects | Opportunity, OpportunityPersonRole |
| listMObjects | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
