---
title: Prise en main
description: Prise en main des API du Marketo Engage
exl-id: 78c44c32-4e59-4d55-a45c-ef0d7dac814d
source-git-commit: 490411e411bed7b5b76fd9e5f41ccc9d156b2ba9
workflow-type: tm+mt
source-wordcount: '1340'
ht-degree: 1%

---

# Prise en main

Marketo Engage est une plateforme d’automatisation du marketing qui permet aux marketeurs de gérer des programmes et des campagnes multicanaux personnalisés vers les prospects et les clients. La plateforme du Marketo Engage peut être étendue à l’aide de points d’intégration. Vous trouverez ci-dessous les entités principales et leurs relations.

>[!NOTE]
>L’API SOAP est en cours d’obsolescence et ne sera plus disponible après le 31 octobre 2025. Tout nouveau développement doit être effectué avec l’API Marketo [REST](./rest-api/rest-api.md) et les services existants doivent être migrés avant cette date pour éviter toute interruption de service. Si vous disposez d’un service qui utilise l’API SOAP, consultez le [ Guide de migration de l’API SOAP ](./soap-api/migration.md) pour plus d’informations sur la migration.
>

Lorsque la connexion native SFDC ou MS Dynamics CRM est activée sur une instance de Marketo Engage, les objets suivants sont en lecture seule : Société, Opportunité, Rôle d’opportunité, Commercial

![ Modèle de données ](assets/data_model.png)

## Personne (Leads)

Les personnes sont la base de toute plateforme d’automatisation du marketing. Dans Marketo, tous les enregistrements autres que ceux des commerciaux sont appelés leads, qu’ils soient désignés comme leads, prospects, suspects, contacts, etc., du point de vue des ventes. L’objet de prospect est fourni avec un ensemble de champs standard, tels que l’adresse électronique, le prénom et le nom. Des champs supplémentaires peuvent être ajoutés au type d’objet de prospect pour étendre les types d’informations associés aux enregistrements du système. Les attributs personnalisés peuvent être lus et écrits de la même manière que les champs standard. Vous trouverez une liste complète de champs dans le menu Marketo **[!UICONTROL Admin]** > **[!UICONTROL Gestion des champs]**. Les prospects sont identifiés de manière unique dans Marketo par le champ d’identifiant . D’autres clés uniques doivent être appliquées en externe à partir du système.

API associées : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Activités

Les prospects interagissent avec votre entreprise de différentes manières. Un prospect peut consulter une page du site Web de votre entreprise, participer à un salon professionnel ou télécharger un livre blanc. Chacune de ces actions peut être capturée dans Marketo afin d’aider un professionnel du marketing à mieux comprendre les activités d’un prospect et le moment où elles ont été réalisées, de sorte qu’il puisse coordonner des communications opportunes et pertinentes. Les activités sont toujours liées aux prospects par ID de prospect.

Vous pouvez définir vos propres activités personnalisées. Une fois que vous avez créé et publié une activité personnalisée, vous pouvez ajouter des activités personnalisées via l’API Marketo. Vous trouverez plus d’informations sur les activités personnalisées [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

API associées : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programmes et campagnes

Un programme est le mécanisme par lequel un professionnel du marketing organise tous ses différents types d’efforts marketing à partir d’un emplacement central. Un exemple de programme est une explosion d’e-mail. Un prospect peut entreprendre plusieurs actions/activités liées à un programme donné qui lui sont associées. On parle alors de progression du prospect. Un exemple de progression d’un programme d’explosion d’e-mail enregistre le moment où un prospect reçoit un e-mail, le moment où la personne ouvre l’e-mail ou le fait qu’elle ait cliqué sur un lien dans l’e-mail.

Les campagnes sont créées pour atteindre un objectif spécifique au sein d’un programme. Par exemple, une campagne peut consister à limiter un groupe de prospects et à leur envoyer l’e-mail explosif, ou à informer un commercial pour le suivi si un prospect clique sur un lien dans le programme d’e-mail explosif.

API associées : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

## Balises

Les balises sont un moyen de regrouper des données à des fins de création de rapports. Ces identifiants permettent de classer les données et de définir la manière dont vous souhaitez créer des rapports sur votre programme pour comprendre l’efficacité et le retour sur investissement du programme.

En tant qu’administrateur Marketo, vous avez la possibilité de créer des types de balises obligatoires et facultatifs disponibles à la sélection lorsqu’un utilisateur Marketo crée un programme. Les valeurs possibles pour chacun de ces types de balises sont définies par vous et reflètent la manière dont votre entreprise souhaite utiliser les balises personnalisées à des fins de création de rapports.

Par exemple, vous pouvez créer un type de balise « Region » personnalisé avec plusieurs valeurs de balise (par exemple, Nord-Est, Sud-Est) afin d’analyser la région qui génère le plus de prospects. Vous pouvez également créer un type de balise « Propriétaire », ce qui vous permet d’évaluer et de comprendre quels propriétaires de programme (par exemple, Maria, David ou John) ont le plus d’impact sur la création de prospects et d’opportunités. Vous trouverez plus d’informations sur les balises [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

API associées : [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Listes

Les listes permettent à un spécialiste marketing d’organiser une collection de prospects. Il existe deux types de listes dans Marketo : statique et dynamique. Une liste statique est une liste fixe de prospects qu’un professionnel du marketing peut ajouter ou supprimer selon son choix. Une liste dynamique est une collection dynamique de prospects basée sur un ensemble de caractéristiques désignées. Voici un exemple de liste dynamique : « Tous les prospects qui ont visité la page de tarification de notre site Web ». Cette liste dynamique continue de s’allonger à mesure que de nouveaux prospects visitent la page de tarification. Vous trouverez plus d’informations sur les listes [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home).

API associées : [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

## Opportunités

Les marketeurs proposent des prospects aux ventes sous la forme d’une opportunité. Une opportunité représente une transaction commerciale potentielle et est associée à un prospect ou un contact et à une organisation dans Marketo. Un rôle d’opportunité est l’intersection entre un prospect donné et une organisation. Le rôle d’opportunité se rapporte à la fonction d’un prospect au sein de l’organisation.

API associées : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

## Sociétés

Une organisation, parfois appelée compte dans Marketo, fait référence à l’organisation à laquelle une personne appartient. Lors de l’utilisation de la création de rapports sur le retour sur investissement dans Marketo ou Revenue Cycle Analytics (RCA), il est important d’associer les personnes à leur organisation et aux opportunités afin de pouvoir déterminer l’attribution appropriée du retour sur investissement.

API associées : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

## Ressources

Assets se rapporte aux pages de destination, aux e-mails, aux formulaires et aux images utilisés dans un programme. Assets peut être local à un programme donné ou global. Les ressources globales sont disponibles dans n’importe quel programme.

API associées : [REST](https://developer.adobe.com/marketo-apis/api/asset/)

## Jetons

Les jetons permettent à un spécialiste marketing de personnaliser les messages avec des ressources et d’ajouter une logique dans les actions de flux. Il existe des jetons pour le système global, les programmes, les prospects et les entreprises. Un exemple de jeton de prospect est {{lead.First Name}}. Ce jeton peut être placé dans un e-mail pour afficher le prénom du prospect.

Les jetons définis au niveau du programme ou du dossier sont appelés « Mes jetons » dans Marketo. My Tokens peut être de l’un des trois types suivants : local, hérité ou remplacé.

Mes jetons créés localement dans un dossier ou un programme de campagne spécifique sont disponibles pour ce programme ou dossier de campagne spécifique (local). Mes jetons créés au niveau du dossier de campagne peuvent être utilisés dans tous les programmes contenus dans ce dossier de campagne (hérité). Mes jetons qui sont modifiés au niveau du programme avec des valeurs personnalisées ne modifient pas la valeur parent de Mon jeton au niveau du dossier du programme (remplacée).

Mes jetons utilisent la convention de nommage {{my.My Token}}, avec le mot « my » ajouté au début du nom du jeton. Par exemple, si vous créez un type de date « Mon jeton » avec le nom EventDate, le nom du jeton est {{my.EventDate}}. Vous trouverez plus d’informations sur Mes jetons [ici](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

API associées : [REST](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

## Objets personnalisés

Un objet personnalisé Marketo permet la création d’une relation un-à-plusieurs, ou plusieurs-à-plusieurs (Edge-Bridge-Edge) entre vos leads Marketo et les enregistrements d’objet personnalisé. Une fois que vous avez créé et publié un objet personnalisé Marketo, vous pouvez effectuer des opérations CRUD sur l’objet personnalisé via l’API Marketo. Vous trouverez plus d’informations sur la création d’objets personnalisés [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home). Lorsque de nouveaux enregistrements sont ajoutés à l’objet personnalisé, vous pouvez utiliser un déclencheur de liste dynamique pour répondre. Vous pouvez également utiliser les données d’objet personnalisées comme filtre dans les listes intelligentes (segmentation) ou dans les e-mails à l’aide de [script d’e-mail](email-scripting.md).

API associées : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects)

## Vendeurs

Les enregistrements de commercial et les relations de prospect peuvent être gérés dans Marketo si aucune intégration CRM native n’est activée. Ces enregistrements contiennent des informations de base sur le commercial, telles que le nom, l’adresse e-mail et le titre de la fonction, qui peuvent être utilisées pour le filtrage et les jetons dans Marketo lorsqu’un prospect appartient à un prospect. La relation avec un commercial est gérée au niveau du prospect par le biais du champ « externalSalesPersonId », qui doit être mis à jour via l’API [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST).

API associées : [REST](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)
