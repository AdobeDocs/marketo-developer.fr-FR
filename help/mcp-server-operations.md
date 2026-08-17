---
title: Opérations Marketo Engage MCP
description: Découvrez les opérations Marketo Engage MCP disponibles à l’utilisation avec les assistants d’IA.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: dca84292-69e9-4116-a575-667d31fa060did: e64968b2-4ee5-47f9-8cae-0588f184b9eb
topic_v2: id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: c631b7c3d571f29083673f9b97d22230d109abfc
workflow-type: tm+mt
source-wordcount: 1228
ht-degree: 50%

---


# [!DNL Marketo Engage] des opérations MCP

Les opérations suivantes sont disponibles via le serveur MCP [!DNL Marketo Engage]. Le serveur fournit des points d’entrée en lecture seule ou non destructifs. Le système d’IA ne peut pas utiliser de `Delete` ni d’autres opérations destructives.

>[!NOTE]
>
>Les `create` et outils de `update` des listes intelligentes et des campagnes intelligentes sont prévus pour la version de septembre 2026.

Pour plus d’informations sur la façon dont les données sont gérées avec l’IA dédiée au Marketo et le serveur MCP de Marketo Engage, consultez la page [Informations sur les données](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).

## Exportation en bloc

[Référence de l’API d’exportation en bloc](https://developer.adobe.com/marketo-apis/api/mapi){target="_blank"}

- `bulk_export_create`
- `bulk_export_enqueue`
- `bulk_export_file`
- `bulk_export_status`
- `get_import_status`

## Canaux et balises

[Référence de l’API Channels ](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels){target="_blank"} | [Référence de l’API Tags](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags){target="_blank"}

- `browse_channels`
- `browse_tag_types`
- `get_channel_by_name`
- `get_tag_type_by_name`

## E-mails

[Référence de l’API Emails](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails){target="_blank"}

- `approve_email`
- `browse_emails`
- `create_email`
- `get_email_by_id`
- `get_email_by_name`
- `get_email_content`
- `update_email_content`

## Dossiers

[Référence de l’API Folders](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders){target="_blank"}

- `browse_folders`
- `create_folder`
- `delete_folder`
- `get_folder_by_id`
- `get_folder_by_name`
- `get_folder_content`
- `update_folder`

## Formulaires

[Référence de l’API Forms](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms){target="_blank"}

- `add_field_set`
- `add_field_to_form`
- `add_field_visibility_rule`
- `add_rich_text_field`
- `approve_form`
- `browse_forms`
- `clone_form`
- `create_form`
- `delete_field_from_fieldset`
- `delete_form`
- `delete_form_field`
- `discard_form_draft`
- `get_form_by_id`
- `get_form_by_name`
- `get_form_field_metadata`
- `get_form_fields`
- `get_forms_used_by`
- `get_program_member_fields`
- `get_thank_you_page`
- `set_field_autofill`
- `update_field_positions`
- `update_form`
- `update_form_field`

## Prospects

[Référence de l’API des leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads){target="_blank"}

- `add_leads_to_list`
- `describe_lead`
- `get_activity_types`
- `get_lead_activities`
- `get_leads_by_filter`
- `get_leads_by_smart_list`
- `get_paging_token`

## Programmes

[Référence de l’API de programmes](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs){target="_blank"}

- `approve_program`
- `browse_email_batch_programs`
- `browse_nurture_programs`
- `browse_program_details`
- `browse_program_events`
- `browse_programs`
- `browse_scheduled_programs`
- `clone_program`
- `create_program`
- `delete_program_tag`
- `get_program_by_id`
- `get_program_by_name`
- `get_program_creation_options`
- `get_program_smart_list`
- `get_programs_by_tag`
- `unapprove_program`
- `update_program`
- `update_program_tag`

## Campagnes intelligentes

[Référence de l’API de campagnes intelligentes](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns){target="_blank"}

- `activate_smart_campaign`
- `add_flow_step`
- `browse_smart_campaigns`
- `create_smart_campaign`
- `facet_smart_campaigns`
- `get_smart_campaign_auto_suggest`
- `get_smart_campaign_by_id`
- `get_smart_campaign_by_name`
- `get_smart_campaign_flow_step_by_name`
- `get_smart_campaign_flow_step_type_by_name`
- `get_smart_campaign_flow_step_types`
- `get_smart_campaign_flow_steps`
- `get_smart_campaign_rule_by_name`
- `get_smart_campaign_rules`
- `get_smart_campaign_scheduled_runs`
- `get_smart_campaign_used_by`
- `get_smart_list_by_campaign_id`
- `schedule_campaign`
- `trigger_campaign`
- `update_flow_step_choice`
- `update_smart_campaign`

## Listes intelligentes

[Référence de l’API de listes dynamiques](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Lists){target="_blank"}

- `add_smart_list_rule`
- `browse_smart_lists`
- `clone_smart_list`
- `create_smart_list`
- `delete_all_smart_list_rules`
- `get_smart_list_auto_suggest`
- `get_smart_list_by_id`
- `get_smart_list_by_name`
- `get_smart_list_rule_by_name`
- `get_smart_list_rules`
- `get_smart_list_used_by`
- `remove_smart_list_rule_constraint`
- `reorder_smart_list_rules`
- `update_smart_list_filter_logic`
- `update_smart_list_rule`

## Extraits

[Référence de l’API des fragments de code](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets){target="_blank"}

- `approve_snippet`
- `browse_snippets`
- `clone_snippet`
- `create_snippet`
- `delete_snippet`
- `discard_snippet_draft`
- `facet_snippets`
- `get_snippet_by_id`
- `get_snippet_content`
- `get_snippet_dynamic_content`
- `unapprove_snippet`
- `update_snippet`
- `update_snippet_content`
- `update_snippet_dynamic_content`

## Listes statiques

[Référence de l’API des listes statiques](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists){target="_blank"}

- `browse_lists`
- `create_list`
- `get_list_by_id`
- `get_list_by_name`
- `get_list_members`
- `remove_from_list`
- `update_list`

## Jetons

[Référence de l’API de jetons](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens){target="_blank"}

- `create_calendar_token`
- `create_token`
- `delete_token`
- `get_calendar_tokens`
- `get_tokens_by_folder`

## Outils d’étapes de flux MCP activés

<table style="table-layout:auto">
<tr>
<th>Étapes de flux</th>
<th>Déclencheurs</th>
<th>Filtres (activité)</th>
<th>Filtres (attribut)</th>
</tr>
<tr>
<td valign="top"><ul><li>Ajouter au jeu de champs</li><li>Ajouter à la liste</li><li>Ajouter à la campagne Microsoft</li><li>Ajouter à la fidélisation</li><li>Ajouter à la campagne SFDC</li><li>Appeler le Webhook</li><li>Modifier valeur des données</li><li>Modifier la partition de lead</li><li>Modifier le rythme de fidélisation</li><li>Modifier le suivi de fidélisation</li><li>Modifier l’entité propriétaire</li><li>Changer de propriétaire dans Microsoft​</li><li>Modifier les données du programme</li><li>Modifier les données du membre du programme​</li><li>Modifier l'étape dans le cycle de vente</li><li>Modifier évaluation</li><li>Modifier segment</li><li>Modifier le statut de progression</li><li>Modifier le statut dans une campagne SFDC</li><li>Lead converti</li><li>Créer une tâche</li><li>Créer une tâche dans Microsoft</li><li>Supprimer un lead</li><li>Supprimer le lead de Microsoft</li><li>Supprimer le lead de SFDC</li><li>Exécuter la campagne</li><li>Moment intéressant</li><li>Supprimer du jeu de champs</li><li>Supprimer des flux</li><li>Supprimer de la liste</li><li>Supprimer de la campagne Microsoft</li><li>Supprimer de la campagne SFDC</li><li>Demander une campagne</li><li>Envoyer une alerte</li><li>Envoyer un e-mail</li><li>Synchroniser le prospect avec Microsoft</li><li>Synchroniser le lead avec SFDC</li><li>Attendre</li></ul></td>
<td valign="top"><ul><li>Activité consignée</li><li>Activité mise à jour</li><li>Ajouté à la liste</li><li>Ajouté à Microsoft Campaign</li><li>Ajouté à la culture</li><li>Ajouté à l'opportunité</li><li>Ajouté à l’opportunité (compte)</li><li>Ajouté à l’opportunité (contact)</li><li>Ajouté à la campagne SFDC</li><li>Pose des questions pendant l’événement</li><li>Événement Attends</li><li>Campagne demandée</li><li>Clics sur le lien</li><li>Clique sur le lien dans l’e-mail​</li><li>Clics sur le lien dans l'e-mail de vente</li><li>Clics sur le lien dans un SMS</li><li>Clics sur un lien</li><li>Modifications valeur des données</li><li>Télécharge une ressource</li><li>L'e-mail est renvoyé</li><li>L'e-mail est renvoyé provisoirement</li><li>L'e-mail a été remis au destinataire</li><li>S’engage dans un flux de conversation</li><li>Engage une boîte de dialogue</li><li>Interagit avec un agent dans le flux de conversation</li><li>Interagit avec un agent dans la boîte de dialogue</li><li>Remplissage du formulaire</li><li>A un moment significatif</li><li>Interagit avec le document dans le flux de conversation</li><li>Interagit avec le document dans la boîte de dialogue</li><li>E-mail de vente envoyé</li><li>Lead converti</li><li>Le prospect est créé</li><li>Lead supprimé de Microsoft</li><li>Lead supprimé de SFDC</li><li>Le prospect est envoyé vers Marketo</li><li>Le prospect est synchronisé avec Microsoft</li><li>Le prospect est synchronisé avec SFDC</li><li>Modifications de la partition de lead</li><li>Changement manuel du niveau</li><li>Modifications du rythme de maturation</li><li>Suivi des modifications</li><li>Ouvre l'e-mail</li><li>Ouvre l'e-mail de vente</li><li>L’opportunité (compte) a été mise à jour</li><li>L’opportunité (contact) a été mise à jour</li><li>Mise à jour de l'opportunité</li><li>Modifications du détenteur</li><li>Modifications du propriétaire dans Microsoft</li><li>Les données des membres du programme ont été modifiées</li><li>Le statut de progression est modifié</li><li>Atteint L’Objectif De La Boîte De Dialogue</li><li>Atteint l’objectif dans le flux de conversation</li><li>E-mail de transfert à un ami reçu</li><li>Supprimé de la liste</li><li>Supprimé de la campagne Microsoft</li><li>Supprimé de l'opportunité</li><li>Supprimé de l’opportunité (compte)</li><li>Supprimé de l’opportunité (contact)</li><li>Supprimé de la campagne SFDC</li><li>Réponses aux e-mails relatifs aux ventes</li><li>Répond à un sondage</li><li>Répond à un questionnaire</li><li>Modification de l'étape dans le cycle de vente</li><li>Renvoi de l'e-mail de vente</li><li>E-mail de vente reçu</li><li>Planifie une réunion dans le flux de conversation</li><li>Plannifie une réunion dans une boîte de dialogue</li><li>Modification de l'évaluation</li><li>Modifications du segment</li><li>Alerte envoyée</li><li>E-mail de transfert à un ami envoyé</li><li>Rebonds de messages SMS</li><li>Le message SMS a été remis</li><li>Modification du statut dans la campagne SFDC</li><li>Désabonné de l'e-mail</li><li>Visite sur la page Web</li><li>Webhook appelé</li></ul></td>
<td valign="top"><ul><li>L'activité a été consignée</li><li>L'activité a été mise à jour</li><li>Alerte envoyée</li><li>La campagne a été exécutée</li><li>Une campagne a été demandée</li><li>Cliquer sur lien</li><li>Cliquer sur lien dans e-mail</li><li>Cliquer sur lien dans e-mail de vente</li><li>Lien cliqué dans un SMS</li><li>A cliqué sur un lien</li><li>Valeur des données modifiée</li><li>A téléchargé une ressource</li><li>E-mail retourné en erreur</li><li>E-mail retourné en erreur provisoirement</li><li>Engagé dans un flux conversationnel</li><li>A pris contact via une boîte de dialogue</li><li>Engagement avec un agent dans le flux de conversation</li><li>A interagi avec un agent dans le cadre d’un dialogue</li><li>Formulaire rempli</li><li>A eu un moment significatif</li><li>A posé des questions pendant l’événement</li><li>A participé à l’événement</li><li>Interaction avec le document dans le flux de conversation</li><li>A interagi avec un document dans la boîte de dialogue</li><li>Partition de lead modifiée</li><li>Lead converti</li><li>Lead créé</li><li>Le prospect a été supprimé de Microsoft.</li><li>Le prospect a été supprimé de SFDC.</li><li>Le prospect a été envoyé à Marketo</li><li>Le prospect a été synchronisé avec Microsoft</li><li>Le prospect a été synchronisé avec SFDC</li><li>Cadence de maturation modifiée</li><li>Suivi des maturations modifié</li><li>E-mail ouvert</li><li>E-mail de vente ouvert</li><li>L’opportunité (compte) a été mise à jour</li><li>L’opportunité (contact) a été mise à jour</li><li>Opportunité mise à jour</li><li>Détenteur modifié</li><li>Le propriétaire a été modifié dans Microsoft</li><li>Les données des membres du programme ont été modifiées</li><li>Le statut de progression a été modifié</li><li>A atteint l’objectif du dialogue</li><li>Objectif atteint dans le flux de conversation</li><li>E-mail de transfert à un ami reçu</li><li>A répondu à l'e-mail commercial</li><li>A répondu à un sondage</li><li>A répondu à un questionnaire</li><li>L'étape dans le cycle de vente modifiée</li><li>E-mail de vente renvoyé</li><li>E-mail de vente reçu</li><li>Réunion planifiée dans le flux de conversation</li><li>A planifié une réunion dans la boîte de dialogue</li><li>Évaluation modifiée</li><li>Segment modifié</li><li>E-mail de transfert à un ami envoyé</li><li>Message SMS rebond</li><li>Désinscription de l’e-mail</li><li>Page Internet visitée</li><li>A été ajouté à la liste</li><li>A été ajouté à la culture</li><li>A été ajouté à l'opportunité</li><li>A été ajouté à l’opportunité (compte)</li><li>A été ajouté à l’opportunité (contact)</li><li>A reçu un e-mail</li><li>A reçu un SMS</li><li>A été supprimé de la liste</li><li>A été supprimé de l'opportunité</li><li>A été supprimé de l’opportunité (compte)</li><li>A été supprimé de l’opportunité (contact)</li><li>Un e-mail lui a été envoyé</li><li>Un e-mail de vente lui a été envoyé</li><li>Webhook appelé</li></ul></td>
<td valign="top"><ul><li>Adresse e-mail du propriétaire du compte</li><li>Prénom du propriétaire du compte</li><li>Nom du propriétaire du compte</li><li>Date d’acquisition</li><li>Programme d’acquisition</li><li>Nom du programme d'acquisition</li><li>Adresse</li><li>Chiffre d'affaires annuel</li><li>IP anonyme</li><li>Adresse de facturation</li><li>Ville de facturation</li><li>Pays de facturation</li><li>Code postal de facturation</li><li>État de facturation</li><li>Sur liste noire</li><li>Ville</li><li>Type Microsoft de la société</li><li>Nom de la société</li><li>Pays</li><li>Créé à</li><li>Date de naissance</li><li>Service</li><li>Ne pas appeler</li><li>Motif de l’interdiction d’appel</li><li>Champs en double</li><li>Adresse e-mail</li><li>E-mail non valide</li><li>Cause e-mail non valide</li><li>E-mail interrompu</li><li>E-mail interrompu à</li><li>Cause e-mail interrompu</li><li>Numéro de fax</li><li>Prénom</li><li>Nom complet</li><li>A une opportunité</li><li>Secteur</li><li>Ville déduite</li><li>Société déduite</li><li>Pays déduit</li><li>Aire métropolitaine déduite</li><li>Indicatif téléphonique local déduit</li><li>Code postal déduit</li><li>Région déduite</li><li>Est client</li><li>Est partenaire</li><li>Intitulé du poste</li><li>Nom</li><li>Adresse e-mail du propriétaire du lead</li><li>Prénom du propriétaire du lead</li><li>Fonction du propriétaire du lead</li><li>Nom du propriétaire du lead</li><li>Numéro de téléphone du propriétaire du lead</li><li>Nom de la partition de lead</li><li>Évaluation du lead</li><li>Évaluation des leads</li><li>Source du lead</li><li>Statut du lead</li><li>Téléphone principal</li><li>Marketing interrompu</li><li>Membre du jeu de champs</li><li>Membre de la liste</li><li>Membre de l'éducation</li><li>Membre du programme</li><li>Personne membre du modèle de revenus</li><li>Étape du membre du chiffre d’affaires</li><li>Membre de la campagne SFDC</li><li>Membre de la campagne intelligente</li><li>Membre de la liste intelligente</li><li>Numéro de compte Microsoft</li><li>Date de création - Microsoft</li><li>Suppression de Microsoft</li><li>Type Microsoft</li><li>Deuxième prénom</li><li>Numéro téléphone mobile</li><li>Notes</li><li>Nombre d'employés</li><li>Nombre d'opportunités</li><li>Référent d'origine</li><li>Moteur de recherche d'origine</li><li>Phrase de recherche d'origine</li><li>Info source d'origine</li><li>Type source d'origine</li><li>Nom de la société mère</li><li>Fuseau horaire de la personne</li><li>Numéro de téléphone</li><li>Code postal</li><li>Modèle au hasard</li><li>Informations sur la source d'inscription</li><li>Type de source d'inscription</li><li>Rôle</li><li>Titre</li><li>Numéro de compte SFDC</li><li>Date de création SFDC</li><li>Suppression de SFDC</li><li>Type de SFDC</li><li>Code SIC</li><li>Site</li><li>État</li><li>Total du montant de l'opportunité</li><li>Total du chiffre d'affaires souhaité de l'opportunité</li><li>Désabonné ou désabonnée</li><li>Raison désabonnement</li><li>Mis à jour à</li><li>Site Web</li></ul></td>
</tr>
</table>
