---
title: Types de champs
feature: REST API
description: Une liste des types de champs Marketo
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 8%

---

# Types de champs

Voici une description des types de champs dans Marketo. Vous trouverez des informations supplémentaires sur les types de champs [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Vous trouverez des informations supplémentaires sur les limites de type de champ [ici](https://nation.marketo.com/t5/knowledgebase/marketo-field-limits-by-field-type/ta-p/251613).

| Type de champ | Description | Exemple |
| --- | --- | --- |
| Datetime | Utilisé pour saisir une date et une heure. Suit le format [W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). Il est recommandé d’inclure le décalage du fuseau horaire. Date d’achèvement plus heures et minutes : AAAA-MM-JJThh:mm:ssTZD où TZD est « +hh:mm » ou « -hh:mm » Remarque : certaines API de ressources renvoient « Z+0000 » en tant que TZD pour `updatedAt` et `createdAt`. | 2010-05-07T15:41:32-05:00 |
| E-mail | Un champ de chaîne qui accepte les adresses e-mail | <example@example.com> |
| Flottante | Champ numérique contenant des nombres réels et pouvant utiliser une décimale. | 10,4 |
| Nombre entier | Nombre entier | 10 |
| Formule | Champs dont les valeurs sont générées en manipulant les données d’autres champs présents sur un enregistrement de prospect. Elles ne sont pas exportées et ne peuvent pas être utilisées dans des campagnes intelligentes. | Voir cet [article](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Pourcentage | Un pourcentage exprimé sous la forme d’un entier | 30 |
| URL | Champ de texte qui limite la saisie aux URL, y compris le protocole de l’URL. | <http://example.com/> |
| Téléphone | Numéro de téléphone | 111-111-1111 |
| Zone de texte | Texte plus long. | Prend en charge jusqu’à 30 000 octets. Les caractères ASCII standard utilisent 1 octet par caractère (jusqu’à 30 000 caractères). Les caractères Unicode peuvent utiliser jusqu’à 4 octets par caractère (ce qui réduit  nombre de caractères autorisés à être inférieur à 30 000 caractères). |
| Chaîne | Texte plus court | Texte jusqu’à 255 caractères |
| Score | Champ entier qui peut être manipulé avec l’étape Modifier le flux de score | 10 |
| Booléen (précédemment Case à cocher) | Permet aux utilisateurs de sélectionner une valeur True (cochée) ou False (non cochée). | True |
| Devise | Champ flottant représentant le type de devise par défaut sélectionné pour l’abonnement Marketo | 10,40 |
| Date | Utilisé pour la date. Suit le format W3C. | 2010-05-07 |
| Référence | Champ de chaîne contenant une clé vers un autre enregistrement (clé étrangère). | Contact de la société |
