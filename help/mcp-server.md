---
title: Serveur MCP
description: Découvrez comment connecter un assistant AI à Marketo à l’aide du serveur MCP. Configurez le bureau Claude, le curseur, le code Claude ou le code VS avec vos informations d’identification Marketo.
badgeBeta: label="Beta" type="informative" tooltip="Cette fonctionnalité est actuellement en version bêta fermée"
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
hidefromtoc: true
hide: true
source-git-commit: a8bf6680a212dd665841896e4550a755dcdf745d
workflow-type: tm+mt
source-wordcount: '1478'
ht-degree: 1%

---

# [!DNL Marketo] MCP Server

>[!NOTE]
>
>Le serveur MCP est actuellement en version bêta fermée. Elle n’est pas disponible pour tous les utilisateurs et utilisatrices pour le moment.

Le protocole MCP (Model Context Protocol) est une norme ouverte qui permet aux outils d’IA de communiquer avec des services externes. Le serveur MCP [!DNL Marketo] fait office de pont entre votre assistant d’IA et [!DNL Marketo]. Il expose plus de 100 opérations sur des formulaires, des programmes, des campagnes intelligentes, des prospects, des e-mails, des fragments de code, des listes et des dossiers.

Lorsque votre outil d’IA appelle le serveur MCP, le serveur exécute l’appel API REST correspondant en votre nom, à l’aide des informations d’identification que vous fournissez dans chaque requête. Vous n’avez pas besoin d’installer, de déployer ou d’exécuter un logiciel côté serveur.

>[!IMPORTANT]
>
>Le protocole MCP (Model Context Protocol) est une norme open source émergente qui peut présenter des risques pour la sécurité ou la fiabilité. Les intégrations de serveurs Adobe MCP et la documentation associée sont fournies « en l’état », sans garantie d’aucune sorte.
>La connexion des clients ou serveurs MCP aux produits Adobe est une configuration choisie par le client. Ce dernier est chargé d’évaluer la sécurité et l’adéquation de toute intégration MCP. Adobe n’est pas responsable des problèmes résultant d’une mauvaise configuration, d’une utilisation abusive du MCP, de vulnérabilités dans les implémentations tierces ou d’actions involontaires effectuées par le biais de workflows prenant en charge MCP.
>Pour réduire les risques, Adobe encourage à tester les intégrations dans un environnement Sandbox avant une utilisation productive et à examiner et valider soigneusement toutes les actions et réponses initiées par MCP avant de les confirmer ou de s’y fier.

## Principes de base de MCP

>Imaginez MCP comme un port USB-C pour les applications d’IA. Tout comme USB-C offre un moyen normalisé de connecter vos appareils à divers périphériques et accessoires, MCP offre un moyen normalisé de connecter les modèles d&#39;IA à différentes sources de données et outils. — [Modèle de protocole contextuel](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

MCP permet à un outil d’IA de se connecter à plusieurs services externes en même temps. Par exemple, un assistant d’IA peut :

* Connexion à un traitement de texte pour la génération de documents assistée par l’IA
* Connectez-vous à des applications de modélisation 3D telles que Blender pour créer des animations
* Connexion à After Effects pour le montage vidéo

MCP est un protocole de communication, une norme ouverte que toute application peut implémenter pour exposer ses données et actions aux outils d’IA.

## Ce que [!DNL Marketo] MCP fait et ne fait pas

Comprendre la portée de MCP permet de définir les attentes avant de connecter votre outil d’IA.

**MCP effectue les opérations suivantes :**

* Fournir un accès aux données et fonctionnalités [!DNL Marketo] via les API REST standard
* Exécutez les appels API en votre nom à l’aide des informations d’identification fournies avec chaque requête
* Prise en charge de plusieurs utilisateurs simultanés, chacun connecté avec ses propres informations d’identification
* Gérer automatiquement l’actualisation du jeton OAuth - vous n’avez pas besoin de gérer l’expiration du jeton
* Fonctionnent dans des environnements isolés du client afin que vos données n’interagissent jamais avec la session d’un autre utilisateur

**MCP ne :**

* Utiliser, héberger ou exécuter n’importe quel modèle d’IA ou de machine learning : tout le traitement de l’IA s’effectue dans votre outil d’IA, et non dans MCP.
* Apprenez-en plus sur les données, y compris les données de vos clients, ou tirez des leçons de ces données.
* Générer des prédictions, des recommandations ou des décisions : la prise de décision est de la responsabilité de l’outil d’IA ou de l’utilisateur en aval
* Stocker ou conserver les informations d’identification, les données de requête ou l’état de session entre les requêtes
* Nécessite l’installation, le déploiement ou la gestion de tout logiciel côté serveur

## Conditions préalables

* Une instance [!DNL Marketo] avec l’accès API REST activé
* Accès administrateur à la création d’informations d’identification d’API dans [!DNL Marketo] LaunchPoint
* L’un des outils d’IA suivants : Claude Desktop, Cursor, Claude Code (CLI) ou VS Code avec Copilote GitHub
* Accès réseau à l’URL du serveur MCP : `https://marketo-mcp.adobe.io/mcp`

## Obtention des informations d’identification Marketo

Vous avez besoin des valeurs suivantes de votre instance [!DNL Marketo] :

* **Identifiant du client**
* **Secret du client**
* **Identifiant de compte**

Si vous les avez déjà, passez à [Configurer votre outil d’IA](#configure-your-ai-tool).

### ID client et secret client

1. Accédez à **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**.
1. Sélectionnez votre service d’API. Si vous n’en avez pas, sélectionnez **[!UICONTROL Nouveau]** > **[!UICONTROL Nouveau service]**, choisissez **[!UICONTROL Personnalisé]** comme type de service et affectez un utilisateur ou une utilisatrice d’API dédié(e).
1. Sélectionnez **[!UICONTROL Afficher les détails]** et copiez les valeurs **[!UICONTROL ID client]** et **[!UICONTROL Secret client]**.

### ID de compte Munchkin

1. Accédez à **[!UICONTROL Admin]** > **[!UICONTROL Munchkin]**.
1. Copiez l’ID de compte **&#x200B;**. Le format est `XXX-XXX-XXX` et correspond au préfixe de l’URL de votre instance.

## Configuration de votre outil d’IA

Chaque outil d’IA lit la configuration du serveur MCP à partir d’un emplacement différent. Recherchez votre outil ci-dessous et suivez les étapes pour ajouter le serveur MCP [!DNL Marketo].

>[!TIP]
>
>Pour vous connecter à plusieurs instances [!DNL Marketo], ajoutez des entrées distinctes dans votre configuration MCP avec des noms uniques (par exemple, `marketo-prod` et `marketo-staging`), chacun avec les informations d’identification correspondantes.

### Claude Desktop

Le fichier de configuration est `claude_desktop_config.json`. Ouvrez-le à partir de l’un des emplacements suivants :

* **macOS** : `~/Library/Application Support/Claude/claude_desktop_config.json`
* **Windows** : `%APPDATA%\Claude\claude_desktop_config.json`

Si le fichier contient déjà d’autres serveurs MCP, ajoutez l’entrée `marketo` sous `mcpServers`. L’exemple suivant illustre le bloc de `mcpServers` complet :

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Enregistrez le fichier, quittez Claude Desktop et rouvrez-le.

### Curseur

Si votre configuration MCP de curseur contient déjà d&#39;autres serveurs, ajoutez l&#39;entrée `marketo` sous `mcpServers`. L’exemple suivant montre le bloc de `mcpServers` complet dans **[!UICONTROL Paramètres]** > **[!UICONTROL MCP]** ou `.cursor/mcp.json` dans le répertoire du projet :

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Redémarrez le curseur.

### Claude Code (CLI)

Exécutez la commande suivante dans votre terminal en remplaçant vos informations d’identification :

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

### VS Code avec pilote GitHub

Appuyez sur **[!UICONTROL Ctrl+Maj+P]** (ou **[!UICONTROL Cmd+Maj+P]** sur macOS), saisissez **[!UICONTROL MCP: Open User Configuration]**, puis appuyez sur Entrée. Cette action ouvre `mcp.json`. Ajoutez l’entrée `marketo` dans l’objet `servers` :

```json
{
  "servers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

>[!NOTE]
>
>Pour des raisons de sécurité, utilisez l’interpolation des variables d’environnement dans les fichiers de configuration au lieu de coller directement les informations d’identification. Vous pouvez référencer des variables à l’aide d’une syntaxe telle que `${MARKETO_CLIENT_SECRET}` et les définir dans votre environnement. Cela empêche le stockage des informations d’identification en texte brut dans des fichiers qui peuvent être validés pour le contrôle de version.

## Opérations disponibles

Une fois connecté, vous pouvez demander à votre assistant d’IA d’effectuer des opérations dans les catégories suivantes.

### Formulaires

Parcourir, créer, cloner et approuver des formulaires. Ajouter ou supprimer des champs, configurer des règles de visibilité des champs et identifier l’emplacement d’incorporation des formulaires.

Exemples d’invites :

* « Afficher tous les formulaires approuvés »
* « Clonez le formulaire Nous contacter dans le dossier Campagne du 2e trimestre »
* « Ajouter un champ Société au formulaire de demande de démonstration »

### Campagnes intelligentes

Créez des campagnes intelligentes, configurez des filtres de liste dynamique, ajoutez des étapes de flux et activez ou désactivez des campagnes.

Exemples d’invites :

* « Quelles campagnes intelligentes sont actives en ce moment ? »
* « Créez une campagne intelligente appelée Mise à jour de la notation du lead dans le dossier Opérations »
* « Afficher les étapes de flux dans la campagne E-mail de bienvenue »

### Leads et listes

Rechercher des prospects par adresse e-mail, créer ou mettre à jour des enregistrements de prospect et gérer l’appartenance à une liste statique.

Exemples d’invites :

* « Trouver le prospect avec un e-mail jane@example.com »
* « Ajouter l’ID de lead 12345 à la liste MQL du 2e trimestre »
* « Création d’une liste statique appelée Participants à l’événement d’été »

### Programmes

Créer, cloner et baliser des programmes. Parcourir les programmes par type, canal ou période.

Exemples d’invites :

* « Clonez le programme de webinaire du 4e trimestre dans le dossier Événements 2026 »
* « Créez un programme de messagerie appelé Vente d’été dans le dossier Campagnes »
* « Afficher tous les programmes identifiés comme des webinaires »

### E-mails et fragments de code

Parcourez les e-mails, créez des e-mails à partir de modèles, mettez à jour les sections de contenu et gérez les fragments de code réutilisables.

Exemples d’invites :

* « Afficher tous les brouillons d’e-mails »
* « Mettre à jour la section d’en-tête de l’e-mail de bienvenue »
* « Quels éléments utilisent le fragment de code de promotion Holiday ? »

### Structure de l’instance

Parcourez les dossiers, les canaux, les types de balises et les types d’activités pour comprendre votre configuration [!DNL Marketo].

Exemples d’invites :

* « Répertorier tous les dossiers dans Marketo »
* « Afficher tous les canaux disponibles »
* « Quels types de balises sont configurés ? »

### Opérations en masse

Exporter les données de prospect en bloc et vérifier l’état de la tâche d’importation ou d’exportation.

Exemples d’invites :

* « Créer une exportation en bloc de prospects créés au cours des 30 derniers jours »
* « Vérifier le statut de la tâche d’exportation xx »

## Dépannage

| Erreur | Cause | Corriger |
| ------- | ------- | ----- |
| « Informations d’identification Marketo non fournies » | Un ou plusieurs parmi `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` ou `X-Marketo-Munchkin-Id` sont manquants. | Vérifiez que les quatre en-têtes sont présents dans votre configuration. |
| « Erreur d’authentification » | Vos informations d’identification ne sont pas valides ou ont expiré. | Vérifiez à nouveau votre ID client et votre secret client dans **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. |
| « 403 Interdit » | Votre Munchkin ID ne se trouve pas sur le serveur. | Contactez votre administrateur MCP [!DNL Marketo] pour ajouter votre Munchkin ID. |
| Timeout ou refus de la connexion | Le serveur MCP est inaccessible depuis votre réseau. | Vérifiez que vous pouvez accéder à l’URL du serveur à partir de votre environnement. Vérifiez les exigences VPN, le cas échéant. |
| Les appels d’outils renvoient des résultats vides | L’utilisateur de l’API ne dispose pas des autorisations pour le type de ressource demandé. | Demandez à votre administrateur [!DNL Marketo] de passer en revue le rôle et les autorisations de l’utilisateur API. |

## Considérations relatives à la sécurité

>[!IMPORTANT]
>
>Utilisez un utilisateur d’API dédié dans [!DNL Marketo] avec uniquement les autorisations requises pour votre travail. Ne réutilisez pas les informations d’identification d’administrateur pour l’accès à l’API.

* **Informations d’identification par demande.** L’ID client, le secret client, l’ID Munchkin et le point d’entrée de l’API REST sont transmis dans des en-têtes HTTP avec chaque requête. Le serveur ne les stocke pas et ne les met pas en cache.
* **Isolement à plusieurs clients.** Chaque requête utilise son propre jeu d’informations d’identification. Vos données n’interagissent avec aucune session d’un autre utilisateur.
* **Munchkin ID** Le serveur accepte uniquement les demandes d’instances [!DNL Marketo] approuvées. Les requêtes utilisant un ID Munchkin non autorisé sont rejetées avec une erreur 403.
* **Limites de débit d’API.** Le serveur MCP hérite des limites de débit d’API de votre instance [!DNL Marketo]. Utilisez un utilisateur d’API dédié pour suivre et gérer la consommation des quotas.
* **Gardez les informations d’identification hors du contrôle de version.** Utilisez l’interpolation de variable d’environnement (`${MARKETO_CLIENT_SECRET}`) si votre outil d’IA le prend en charge, de sorte que les informations d’identification ne soient pas stockées en texte brut dans les fichiers validés dans un référentiel.
