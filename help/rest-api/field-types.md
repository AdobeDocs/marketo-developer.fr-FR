---
title: Types de champ
feature: REST API
description: Liste des types de champ Marketo
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
source-git-commit: 2e1a3991c99ec4563e6532253ac1ad81a5f4c465
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 8%

---

# Types de champ

Voici une description des types de champ dans Marketo. Vous trouverez des informations supplémentaires sur les types de champ [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Vous trouverez des informations supplémentaires sur les limites de type de champ [ici](https://nation.marketo.com/t5/knowledgebase/marketo-field-limits-by-field-type/ta-p/251613).

| Type de champ | Description | Exemple |
| --- | --- | --- |
| Datetime | Utilisé pour saisir une date et une heure. Suit [Format W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). La bonne pratique consiste à toujours inclure le décalage du fuseau horaire. Date complète plus heures et minutes : AAAA-MM-DDThh:mm:ssTZD où TZD est &quot;+hh:mm&quot; ou &quot;-hh:mm&quot; Remarque : Certaines API de ressources renvoient &quot;Z+000&quot; comme TZD pour `updatedAt` et `createdAt`. | 2010-05-07T15:41:32-05:00 |
| E-mail | Champ de chaîne qui accepte les adresses électroniques | exemple@exemple.com |
| Flottante | Champ numérique contenant des nombres réels et pouvant utiliser une décimale. | 10,4 |
| Nombre entier | Nombre entier | 10 |
| Formule | Champs dont les valeurs sont générées par la manipulation de données à partir d’autres champs présents dans un enregistrement de piste. Elles ne sont pas exportées et ne peuvent pas être utilisées dans des campagnes dynamiques. | Voir cet [article](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Pourcentage | Pourcentage exprimé sous la forme d’un entier | 30 |
| URL | Champ de texte qui limite la saisie des URL, y compris le protocole de l’URL. | http://example.com/ |
| Téléphone | Numéro de téléphone | 111-111-1111 |
| Zone de texte | Texte plus long. | Prend en charge jusqu’à 30 000 octets. Les caractères ASCII standard utilisent 1 octet par caractère (ce qui permet d’utiliser jusqu’à 30 000 caractères). Les caractères Unicode peuvent utiliser jusqu’à 4 octets par caractère (ce qui réduit la capacité d’utilisation des caractères  Nombre de caractères autorisé à moins de 30 000 caractères). |
| Chaîne | Texte plus court (jusqu’à 255 caractères) | Lorem ipsum dolor sit amet |
| Score | Champ entier pouvant être manipulé à l’étape de flux Changer de score | 10 |
| Booléen (précédemment case à cocher) | Permet aux utilisateurs de sélectionner une valeur True (coché) ou False (non coché). | True |
| Devise | Champ flottant représentant le type de devise par défaut sélectionné pour l’abonnement Marketo | 10,40 |
| Date | Utilisé pour la date. Suit le format W3C. | 2010-05-07 |
| Référence | Champ de chaîne contenant une clé d’un autre enregistrement (clé étrangère). | Contact de la société |
