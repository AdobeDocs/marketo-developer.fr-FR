---
title: Opérations Marketo Engage MCP
description: Découvrez les opérations Marketo Engage MCP disponibles à l’utilisation avec les assistants d’IA.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%
---

# [!DNL Marketo Engage] des opérations MCP

Le serveur MCP [!DNL Marketo Engage] prend en charge 182 opérations relatives à la gestion des ressources, aux données de prospect et à l’automatisation des campagnes.



## Opérations prises en charge

Vous pouvez découvrir les opérations disponibles par l’intermédiaire de votre assistant d’IA connecté.

Si votre connexion expose les outils de détection, utilisez les invites suivantes :

* « Utilisez l’outil de `list_tool_categories` MCP de Marketo pour répertorier les catégories d’opérations disponibles. »
* « Utilisez l’outil de `search_tools` MCP de Marketo pour rechercher les opérations permettant de gérer les campagnes intelligentes. »

Remplacez la tâche de la deuxième invite par l’opération dont vous avez besoin. Si les outils de découverte ne sont pas exposés, demandez à votre assistant d’IA de répertorier les outils disponibles à partir du serveur MCP [!DNL Marketo].

Les résultats reflètent les autorisations et la disponibilité des fonctionnalités de votre connexion. Ils décrivent les outils MCP disponibles, et non l’API REST [!DNL Marketo] complète.

Pour les connexions qui utilisent des informations d’identification client [!DNL Marketo], les fonctionnalités suivantes ne sont pas prises en charge :

* Outils SMS
* Outils de notification push
* Message in-app et outils de programme in-app
* Planification de campagnes intelligentes avec `schedule_smart_campaign`
* Outils de gestion des utilisateurs et de service en bloc

L’opération `schedule_campaign` est prise en charge pour les connexions avec les informations d’identification du client.

Les opérations disponibles dépendent de vos autorisations d’utilisateur de l’API, des fonctionnalités activées pour votre instance [!DNL Marketo] et des restrictions de votre client MCP. Certaines opérations qui modifient ou suppriment des données nécessitent une confirmation et un accès supplémentaire. La prise en charge d’une catégorie n’implique pas l’accès à tous les points d’entrée REST.

Pour obtenir des instructions sur la connexion et des exemples d’invites, voir la page [Serveur MCP](mcp-server.md#available-operations). Pour plus d’informations sur la façon dont les données sont gérées avec [!DNL Marketo] AI et le serveur MCP [!DNL Marketo Engage], consultez la page [Informations sur les données](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).