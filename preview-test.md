---
title: Test d’aperçu EXL
description: Exemples de syntaxe Markdown EXL d’Adobe pour tester l’aperçu de l’extension.
source-git-commit: 87d2584ed0ef2c1fa219f2a3ad120c91dc5491e0
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 13%

---


# Test d’aperçu EXL

## Blocs d’alerte

>[!NOTE]
>
>Ceci est une note. Utilisez des notes pour obtenir des renseignements supplémentaires dont le lecteur doit être au courant.

>[!TIP]
>
>C&#39;est un pourboire. Utilisez des conseils pour obtenir des informations facultatives mais utiles.

>[!IMPORTANT]
>
>Il s’agit d’une alerte importante. Utiliser pour l&#39;information que le lecteur ne doit pas négliger.

>[!WARNING]
>
>Ceci est un avertissement. À utiliser pour plus d’informations sur les problèmes potentiels.

>[!CAUTION]
>
>C&#39;est une mise en garde. À utiliser pour obtenir des informations sur les risques potentiels.

>[!ADMIN]
>
>Il s’agit d’une alerte d’administrateur. Pour le contenu destiné aux administrateurs uniquement.

>[!AVAILABILITY]
>
>Ceci est une note de disponibilité. Pour plus d’informations sur la disponibilité des fonctionnalités.

>[!PREREQUISITES]
>
>Il s’agit d’un bloc de conditions préalables. Énumérez ce dont le lecteur a besoin avant de commencer.

## Boîtes de dialogue

>[!BEGINSHADEBOX « Titre facultatif »]

Ce contenu s’affiche sur un arrière-plan gris. Utilisez des zones ombrées pour regrouper visuellement le contenu associé.

Vous pouvez inclure des listes :

- Point 1
- Point 2
- Point 3

>[!ENDSHADEBOX]

>[!BEGINSHADEBOX]

Zone de nuance sans titre.

>[!ENDSHADEBOX]

## Sections réductibles

+++Cliquez pour développer — Exemple de base

Ce contenu est masqué jusqu’à ce que l’utilisateur sélectionne le titre.

Vous pouvez inclure n’importe quel contenu ici, y compris les blocs de code :

```javascript
const example = 'hello world';
console.log(example);
```

+++

+++Configuration avancée

Utilisez des sections réductibles pour le contenu facultatif ou avancé qui, autrement, encombrerait le flux principal.

| Paramètre | Valeur | Description |
| --- | --- | --- |
| délai d’expiration | 30 | Secondes avant l’expiration du délai de requête |
| reprises | 3 | Nombre de tentatives de reprise |

{style="table-layout:auto"}

+++

## Aide contextuelle

L’aide contextuelle est masquée dans l’aperçu. Regarde !
>[!CONTEXTUALHELP]
>id="models_insights_undefinedchannels"
>title="Canaux non définis"
>abstract="Les canaux non définis sont inclus, mais n’ont aucune conversion attribuée."

## Vidéo incorporée

>[!VIDEO](https://video.tv.adobe.com/v/3427028/?quality=12&learn=on)

## Macros de localisation

Utilisez [!DNL Marketo] pour encapsuler les noms de produits afin qu’ils ne soient pas localisés.

Utilisez **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]** pour les libellés des éléments de l’interface utilisateur.

Exemple combiné : dans [!DNL Adobe Analytics], sélectionnez **[!UICONTROL Workspace]** > **[!UICONTROL Créer un projet]**.

## Badges

[!BADGE Beta]{type=Informative}

[!BADGE Généralement disponible]{type=Positive}

[!BADGE Obsolète]{type=Negative}

[!BADGE Expérimental]{type=Caution}

## Onglets

>[!BEGINTABS]

>[!TAB Conditions]

>[!IMPORTANT]
>
>Vous devez disposer des autorisations d’administrateur pour effectuer cette tâche.

Champs obligatoires :

| Champ | Type | Obligatoire |
| --- | --- | --- |
| Nom | Chaîne | Oui |
| E-mail | Chaîne | Oui |
| Rôle | Enum | Non |

>[!TAB Étapes]

1. Ouvrez l’Admin Console.
1. Sélectionnez **[!UICONTROL Utilisateurs]** > **[!UICONTROL Ajouter un utilisateur]**.
1. Renseignez les champs obligatoires.
1. Sélectionnez **[!UICONTROL Enregistrer]**.

>[!NOTE]
>
>Les modifications prennent effet immédiatement.

>[!TAB Résultat]

Le nouvel utilisateur reçoit un e-mail de bienvenue contenant un lien lui permettant de définir son mot de passe.

- Le lien expire après 24 heures.
- Les utilisateurs peuvent demander un nouveau lien à partir de la page de connexion.

>[!ENDTABS]

## Blocs de code

```json
{
  "name": "example",
  "version": "1.0.0",
  "enabled": true
}
```

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
```

## Tableaux

| Colonne 1 | Colonne 2 | Colonne 3 |
| --- | --- | --- |
| [!UICONTROL Ligne 1], cellule 1 | Ligne 1, cellule 2 | [!DNL Row 1, cell 3] |
| Ligne 2, cellule 1 | Ligne 2, cellule 2 | Ligne 2, cellule 3 |

