---
title: "Types de champ"
feature: REST API
description: "Liste des types de champ Marketo"
source-git-commit: fd75f60adbc4d38e4743db5447d15cf90f025e22
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 8%

---


# Types de champ

Voici une description des types de champ dans Marketo. Vous trouverez plus d’informations sur les types de champ [here](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Vous trouverez des informations supplémentaires sur les limites de type de champ. [here](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents).

| Type de champ | Description | Exemple |
| --- | --- | --- |
| Datetime | Utilisé pour saisir une date et une heure. Sujets [Format W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). La bonne pratique consiste à toujours inclure le décalage du fuseau horaire. Date complète plus heures et minutes : AAAA-MM-JJThh:mm:ssTZD où TZD est &quot;+hh:mm&quot; ou &quot;-hh:mm&quot; Remarque : Certaines API de ressources renvoient &quot;Z+000&quot; comme TZD pour updatedAt et createdAt. | 2010-05-07T15:41:32-05:00 |
| E-mail | Champ de chaîne qui accepte les adresses électroniques | example@example.com |
| Flottante | Champ numérique contenant des nombres réels et pouvant utiliser une décimale. | 10,4 |
| Entier | Nombre entier | 10 |
| Formule | Champs dont les valeurs sont générées par la manipulation de données à partir d’autres champs présents dans un enregistrement de piste. Elles ne sont pas exportées et ne peuvent pas être utilisées dans des campagnes dynamiques. | Voir [article](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Pourcentage | Pourcentage exprimé sous la forme d’un entier | 30 |
| URL | Champ de texte qui limite la saisie des URL, y compris le protocole de l’URL. | http://example.com/ |
| Téléphone | Numéro de téléphone | 111-111-1111 |
| Zone de texte | Texte plus long. | Prend en charge jusqu’à 30 000 octets. Les caractères ASCII standard utilisent 1 octet par caractère (ce qui permet d’utiliser jusqu’à 30 000 caractères). Les caractères Unicode peuvent utiliser jusqu’à 4 octets par caractère (ce qui réduit le nombre de caractères autorisé à moins de 30 000 caractères). |
| Chaîne | Texte plus court (jusqu’à 255 caractères) | Lorem ipsum dolor sit amet |
| Évaluation | Champ entier pouvant être manipulé à l’étape de flux Changer de score | 10 |
| Booléen (précédemment case à cocher) | Permet aux utilisateurs de sélectionner une valeur True (coché) ou False (non coché). | Vrai |
| Devise | Champ flottant représentant le type de devise par défaut sélectionné pour l’abonnement Marketo | 10,40 |
| Date | Utilisé pour la date. Suit le format W3C. | 2010-05-07 |
| Référence | Champ de chaîne contenant une clé d’un autre enregistrement (clé étrangère). | Contact de la société |
