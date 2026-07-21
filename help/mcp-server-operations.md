---
title: Opérations Marketo Engage MCP
description: Découvrez les opérations Marketo Engage MCP disponibles à l’utilisation avec les assistants d’IA.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: dca84292-69e9-4116-a575-667d31fa060did: e64968b2-4ee5-47f9-8cae-0588f184b9eb
topic_v2: id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 260
ht-degree: 5%

---


# [!DNL Marketo Engage] des opérations MCP

Les opérations suivantes sont disponibles via le serveur MCP [!DNL Marketo Engage]. Le serveur fournit généralement des points d’entrée en lecture seule ou non destructifs. Le système d’IA ne peut pas utiliser de `Delete` ni d’autres opérations destructives.

>[!NOTE]
>
>Cette liste va continuer à s’allonger à mesure que nous ajouterons d’autres outils.

Pour plus d’informations sur la façon dont les données sont gérées avec l’IA dédiée au Marketo et le serveur MCP de Marketo Engage, consultez la page [Informations sur les données](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).

## Exportation en bloc

[Référence de l’API d’exportation en bloc](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export){target="_blank"}

- `bulk_export_create`
- `bulk_export_enqueue`
- `bulk_export_file`
- `bulk_export_status`
- `get_import_status`

## Canaux et balises

[Référence de l’API Channels ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels){target="_blank"} | [Référence de l’API Tags](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags){target="_blank"}

- `browse_channels`
- `browse_tag_types`
- `get_channel_by_name`
- `get_tag_type_by_name`

## E-mails

[Référence de l’API Emails](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails){target="_blank"}

- `approve_email`
- `browse_emails`
- `create_email`
- `get_email_by_id`
- `get_email_by_name`
- `get_email_content`
- `update_email_content`

## Dossiers

[Référence de l’API Folders](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders){target="_blank"}

- `browse_folders`
- `create_folder`
- `delete_folder`
- `get_folder_by_id`
- `get_folder_by_name`
- `get_folder_content`
- `update_folder`

## Formulaires

[Référence de l’API Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms){target="_blank"}

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

[Référence de l’API des leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads){target="_blank"}

- `add_leads_to_list`
- `describe_lead`
- `get_activity_types`
- `get_lead_activities`
- `get_leads_by_filter`
- `get_leads_by_smart_list`
- `get_paging_token`

## Programmes

[Référence de l’API de programmes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs){target="_blank"}

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

[Référence de l’API de campagnes intelligentes](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns){target="_blank"}

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

[Référence de l’API de listes dynamiques](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists){target="_blank"}

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

[Référence de l’API des fragments de code](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets){target="_blank"}

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

[Référence de l’API des listes statiques](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists){target="_blank"}

- `browse_lists`
- `create_list`
- `get_list_by_id`
- `get_list_by_name`
- `get_list_members`
- `remove_from_list`
- `update_list`

## Jetons

[Référence de l’API de jetons](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens){target="_blank"}

- `create_calendar_token`
- `create_token`
- `delete_token`
- `get_calendar_tokens`
- `get_tokens_by_folder`
