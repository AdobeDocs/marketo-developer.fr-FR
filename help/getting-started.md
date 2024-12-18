---
title: Prise en main
description: Prise en main des API de Marketo Engage
exl-id: 78c44c32-4e59-4d55-a45c-ef0d7dac814d
source-git-commit: 7a3df193e47e7ee363c156bf24f0941879c6bd13
workflow-type: tm+mt
source-wordcount: '1268'
ht-degree: 1%

---

# Prise en main

Marketo Engage est une plateforme d’automatisation marketing qui permet aux marketeurs de gérer des campagnes et des programmes multicanaux personnalisés pour les prospects et les clients. La plateforme du Marketo Engage peut être étendue à l’aide de points d’intégration. Vous trouverez ci-dessous les entités principales et leurs relations.

Les objets suivants ne sont pas disponibles via l’API REST lorsque la synchronisation native est activée : Société, Opportunité, Rôle d’opportunité, Personne commerciale.

![Modèle de données](assets/data_model.png)

## Personne (Leads)

Les personnes sont la base de toute plateforme d’automatisation du marketing. Dans Marketo, tous les enregistrements de personne qui ne sont pas vendeurs sont appelés prospects, prospects, contacts, etc., du point de vue des ventes. L’objet Lead est fourni avec un ensemble de champs standard, tels que l’adresse électronique, le prénom et le nom. Des champs supplémentaires peuvent être ajoutés au type d’objet de piste pour étendre les types d’informations associées aux enregistrements dans le système. Les attributs personnalisés peuvent être lus et écrits exactement comme les champs standard. Vous trouverez une liste complète de champs dans le menu Marketo **[!UICONTROL Admin]** > **[!UICONTROL Gestion des champs]** . Les pistes sont identifiées de manière unique dans Marketo par le champ Identifiant . D’autres clés uniques doivent être appliquées en externe à partir du système.

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Activités

Les prospects interagissent avec votre organisation de plusieurs façons. Un prospect peut consulter une page du site web de votre société, assister à un salon ou télécharger un livre blanc. Chacune de ces actions peut être capturée dans Marketo afin d’aider un marketeur à mieux comprendre les activités d’un prospect et le moment où il peut coordonner des communications opportunes et pertinentes. Les activités sont toujours liées aux pistes par leadId.

Vous pouvez définir vos propres activités personnalisées. Une fois que vous avez créé et publié une activité personnalisée, vous pouvez ajouter des activités personnalisées via l’API Marketo. Vous trouverez plus d&#39;informations sur les activités personnalisées [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programmes et campagnes

Un programme est le mécanisme par lequel un marketeur organise tous ses différents types d’efforts marketing à partir d’un seul emplacement central. Un exemple de programme est une explosion d&#39;email. Un prospect peut prendre plusieurs actions/activités liées à un programme donné qui sont associées au programme. On parle alors de progression de piste. Un exemple d’avancement d’un programme d’explosion d’un email permet d’enregistrer lorsqu’un prospect est envoyé, quand la personne a ouvert l’email ou si elle a cliqué sur un lien dans le courrier électronique.

Les campagnes sont créées dans le but de servir un objectif spécifique et un objectif spécifique au sein d’un programme. Une campagne peut, par exemple, réduire un groupe de pistes et leur envoyer l’explosion d’un email, ou notifier un représentant commercial pour le suivi si une piste clique sur un lien dans le programme d’explosion d’un email.

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

## Balises

Les balises permettent de regrouper des données à des fins de création de rapports. Ces identifiants permettent de classer les données et de définir la manière dont vous souhaitez générer des rapports sur votre programme afin de comprendre l’efficacité et le retour sur investissement du programme.

En tant qu’administrateur Marketo, vous pouvez créer les types de balises obligatoires et facultatifs pouvant être sélectionnés lorsqu’un utilisateur Marketo crée un programme. Les valeurs possibles pour chacun de ces types de balises sont définies par vous et reflètent la manière dont votre entreprise souhaite utiliser des balises personnalisées à des fins de création de rapports.

Par exemple, vous pouvez créer un type de balise &quot;Région&quot; personnalisé avec plusieurs valeurs de balise (par exemple, Nord-Est, Sud-Est) afin d’analyser la région qui génère le plus de pistes. Vous pouvez également créer un type de balise &quot;Propriétaire&quot;, qui vous permet d’évaluer et de comprendre quels propriétaires de programme (Maria, David ou John, par exemple) ont le plus d’impact sur la création de pistes et d’opportunités. Vous trouverez plus d’informations sur les balises [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Listes

Les listes permettent à un marketeur d’organiser une collection de pistes. Il existe deux types de listes dans Marketo : statique et dynamique. Une liste statique est une liste fixe de pistes qu’un marketeur peut ajouter ou supprimer au fur et à mesure de son choix. Une liste dynamique est un ensemble dynamique de pistes basé sur un ensemble de caractéristiques désignées. Un exemple de liste dynamique serait &quot;Tous les prospects qui ont consulté la page des prix de notre site web&quot;. Cette liste dynamique continue d’augmenter à mesure que d’autres pistes consultent la page de tarification. Vous trouverez plus d&#39;informations sur les listes [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home).

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

## Opportunités

Les marketeurs offrent des pistes de vente sous la forme d’une opportunité. Une opportunité représente une transaction commerciale potentielle et est associée à un prospect ou à un contact et à une organisation dans Marketo. Le rôle d&#39;opportunité est l&#39;intersection entre un prospect et une organisation. Le rôle d’opportunité se rapporte à la fonction d’un prospect au sein de l’organisation.

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

## Sociétés

Une organisation, parfois appelée compte dans Marketo, fait référence à l’organisation à laquelle appartient une personne. Lors de l’utilisation de la création de rapports de retour sur investissement dans Marketo ou dans l’analyse du cycle de revenus (RCA), il est important d’associer les personnes à leur organisation et à leurs opportunités afin de pouvoir déterminer l’attribution du retour sur investissement appropriée.

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

## Ressources

Assets se rapporte aux landing pages, aux emails, aux formulaires et aux images utilisés dans un programme. Assets peut être local pour un programme donné ou global. Les ressources globales sont disponibles dans n’importe quel programme.

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Jetons

Les jetons permettent à un marketeur de personnaliser les messages avec des ressources et d’ajouter une logique dans les actions de flux. Il existe des jetons pour l’ensemble du système, des programmes, des prospects et des entreprises. {{lead.First Name}} est un exemple de jeton de piste. Ce jeton peut être placé dans un email pour afficher le prénom de la piste.

Les jetons définis au niveau du programme ou du dossier sont appelés &quot;Mes jetons&quot; dans Marketo. Mes jetons peuvent être de trois types : local, hérité ou remplacé.

Les jetons créés localement dans un dossier ou un programme de campagne spécifique sont disponibles pour ce programme ou ce dossier de campagne spécifique (local). Les jetons créés au niveau du dossier de campagne peuvent être utilisés pour tous les programmes contenus dans ce dossier de campagne (hérité). Mes jetons qui sont modifiés au niveau du programme avec des valeurs personnalisées ne modifient pas la valeur parent Mon jeton du jeton au niveau du dossier du programme (remplacée).

Mes jetons utilisent la convention d’affectation des noms {{my.My Token}}, avec le mot &quot;my&quot; ajouté au début du nom du jeton. Par exemple, si vous créez un type de date Mon jeton avec le nom EventDate, le nom du jeton est {{my.EventDate}}. Vous trouverez plus d’informations sur My Tokens [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

## Objets personnalisés

Un objet personnalisé Marketo permet de créer une relation un-à-multiple ou plusieurs-à-multiple (Edge-Bridge-Edge) entre vos pistes Marketo et les enregistrements d’objet personnalisé. Une fois que vous avez créé et publié un objet personnalisé Marketo, vous pouvez effectuer des opérations CRUD sur l’objet personnalisé via l’API Marketo. Vous trouverez plus d&#39;informations sur la création d&#39;objets personnalisés [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home). Lorsque de nouveaux enregistrements sont ajoutés à l’objet personnalisé, vous pouvez utiliser un déclencheur de liste dynamique pour répondre. Vous pouvez également utiliser des données d’objet personnalisées comme filtre dans les listes intelligentes (segmentation) ou dans les emails à l’aide de [ scripts d’email ](email-scripting.md).

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects)

## Personnes de vente

Les enregistrements de personne commerciale et les relations de prospect peuvent être gérés dans Marketo lorsqu’aucune intégration CRM native n’est activée. Ces enregistrements contiennent des informations de base sur la personne commerciale, telles que le nom, l’adresse électronique et le titre de la tâche, qui peuvent être utilisées pour filtrer et jetons dans Marketo lorsqu’une piste appartient à une personne. La relation avec un vendeur est gérée au niveau de la piste via le champ &quot;externalSalesPersonId&quot;, qui doit être mis à jour via l’API [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST).

API connexes : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)
