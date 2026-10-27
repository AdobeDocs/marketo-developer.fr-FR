---
title: Serveur MCP Marketo Engage
description: Découvrez comment connecter un assistant d’IA à Marketo à l’aide du serveur MCP Marketo Engage. Configurez le bureau Claude, le curseur, le code Claude ou le code VS avec vos informations d’identification Marketo.
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
autotag-review: '2026-06-02T13:31:15.329Z'
TQID: 'https://experienceleague.adobe.com/PJJm7yv8HmbwMB2fsnfDCXs8zprDJK5Q5z2uiiCJRZI'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
    internal-label: Integrations
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
    internal-label: Configuration
  - id: c2dbad80-0f5c-4d96-a798-2a65f93b8721
    internal-label: Assets
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
    internal-label: Templates
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '2620'
ht-degree: 3%
---

# [!DNL Marketo Engage] MCP Server

Le protocole MCP (Model Context Protocol) est une norme ouverte qui connecte les outils d’IA aux services externes. Le serveur MCP [!DNL Marketo] connecte votre assistant AI à [!DNL Marketo]. Il prend en charge les opérations de gestion des ressources, les données de prospect et l’automatisation des campagnes. Consultez [Opérations MCP prises en charge](mcp-server-operations.md) pour connaître les exceptions et les exigences de disponibilité.

Lorsque votre outil d’IA appelle le serveur MCP, le serveur utilise les informations d’identification contenues dans cette requête pour exécuter l’appel API REST correspondant. Vous n’avez pas besoin d’installer, de déployer ou d’exécuter un logiciel côté serveur.

Pour plus d’informations sur la manière dont les données sont gérées avec l’IA dédiée au Marketo et le serveur MCP de Marketo Engage, consultez la page [Informations sur les données](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).

>[!IMPORTANT]
>
>Le protocole MCP (Model Context Protocol) est une norme open source émergente qui peut présenter des risques en matière de sécurité ou de fiabilité. Les intégrations du serveur Adobe MCP et la documentation associée sont fournies « en l’état », sans garantie d’aucune sorte.
>La connexion des clients ou serveurs MCP aux produits Adobe est une configuration choisie par le client. Ce dernier est chargé d’évaluer la sécurité et l’adéquation de toute intégration MCP. Adobe n’est pas responsable des problèmes résultant d’une mauvaise configuration, d’une utilisation abusive de MCP, de vulnérabilités dans les mises en œuvre tierces ou d’actions involontaires effectuées par le biais de workflows compatibles avec le serveur MCP.
>Pour réduire les risques, Adobe encourage à tester les intégrations dans un environnement Sandbox avant une utilisation productive et à examiner et valider soigneusement toutes les actions et réponses initiées par MCP avant de les confirmer ou de s’y fier.

## Principes de base de MCP

>Imaginez MCP comme un port USB-C pour les applications d’IA. USB-C fournit un moyen normalisé de connecter vos appareils à divers périphériques et accessoires, et MCP fournit un moyen normalisé de connecter les modèles d&#39;IA aux sources de données et aux outils. — [Modèle de protocole contextuel](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

MCP permet à un outil d’IA de se connecter à plusieurs services externes simultanément. Par exemple, un assistant d’IA peut :

* Connexion à un traitement de texte pour la génération de documents assistée par l’IA
* Connectez-vous aux outils d’animation, tels que Blender, pour créer des visualisations
* Connexion à Adobe After Effects pour le montage vidéo

Toute application peut implémenter MCP pour exposer les données et les actions aux outils d’IA.

## Ce que [!DNL Marketo Engage] MCP fait et ne fait pas

Comprendre la portée de MCP permet de définir les attentes avant de connecter votre outil d’IA.

**MCP effectue les opérations suivantes :**

* Fournir un accès aux données et fonctionnalités [!DNL Marketo] via les API REST standard
* Exécutez les appels API en votre nom à l’aide des informations d’identification fournies avec chaque requête
* Prise en charge de plusieurs utilisateurs simultanés, chacun connecté avec ses propres informations d’identification
* Gérer automatiquement l’actualisation du jeton OAuth. Vous n’avez pas besoin de gérer l’expiration des jetons
* Fonctionnent dans des environnements isolés du client afin que vos données n’interagissent jamais avec la session d’un autre utilisateur

**MCP ne :**

* Utilisez, hébergez ou exécutez des modèles d’IA ou de machine learning. Tout le traitement de l’IA s’effectue dans votre outil d’IA, et non dans le MCP
* Apprenez-en plus sur les données, y compris les données de vos clients, ou tirez des leçons de ces données.
* générer des prédictions, des recommandations ou des décisions ; La prise de décision est de la responsabilité de l’outil d’IA ou de l’utilisateur en aval
* Stocker ou conserver les informations d’identification, les données de requête ou l’état de session entre les requêtes
* Nécessite l’installation, le déploiement ou la gestion de tout logiciel côté serveur

MCP peut transmettre des données, y compris des champs potentiellement sensibles, en fonction de l’utilisation de l’API, mais les données B2B impliquent des données commerciales du client et n’impliquent pas de données PII.

## Conditions préalables

* Une instance [!DNL Marketo] avec l’accès API REST activé
* Accès administrateur à la création d’informations d’identification d’API dans [!DNL Marketo] LaunchPoint
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
1. Copiez l’ID de compte ****. Le format est `XXX-XXX-XXX` et correspond au préfixe de l’URL de votre instance.

## Configuration de votre outil d’IA

Les configurations varient légèrement en fonction de l’outil. Cette section fournit des exemples de connexion pour les outils courants.

>[!TIP]
>
>Pour vous connecter à plusieurs instances [!DNL Marketo], ajoutez des entrées distinctes dans votre configuration MCP avec des noms uniques : `marketo-prod` et `marketo-staging`, chacun avec les informations d’identification correspondantes.

>[!BEGINTABS]

>[!TAB Bureau Claude]

### Claude Desktop {#claude-desktop}

Pour vous connecter à Claude Desktop, téléchargez [marketo-mcp-bridge.zip](assets/marketo-mcp-bridge.zip) et décompressez-le. Placez `marketo-mcp-bridge.mjs` dans un emplacement connu et utilisez son chemin d’accès absolu dans la configuration ci-dessous.

Vous avez également besoin d’une version LTS prise en charge de Node.js installée sur votre ordinateur. Le pont ne nécessite pas de packages npm supplémentaires.

1. Ouvrez Claude Desktop.
1. Ouvrez `claude_desktop_config.json`. Sous macOS, le fichier est en `~/Library/Application Support/Claude/`. Sous Windows, il est en `%APPDATA%\Claude\`. Dans les versions avec le menu Paramètres du développeur, sélectionnez **[!UICONTROL Paramètres]** > **[!UICONTROL Développeur]** > **[!UICONTROL Modifier la configuration]** pour l’ouvrir.
1. Ajoutez l’entrée suivante sous `mcpServers`, en conservant les paramètres existants. Remplacez le chemin d’accès et les espaces réservés d’informations d’identification par vos valeurs :

```json
{
  "mcpServers": {
    "marketo-mcp": {
      "command": "node",
      "args": ["/path/to/marketo-bridge/marketo-mcp-bridge.mjs"],
      "env": {
        "MARKETO_MCP_PROD_CLIENT_ID": "<your-client-id>",
        "MARKETO_MCP_PROD_CLIENT_SECRET": "<your-client-secret>",
        "MARKETO_MCP_PROD_MUNCHKIN_ID": "<your-munchkin-id>"
      }
    }
  }
}
```

1. Redémarrez Claude Desktop.

Si Claude Desktop ne trouve pas Node.js, remplacez `node` dans `command` par le chemin d’accès absolu à votre fichier exécutable Node.js. Pour plus d’informations, voir [Connexion aux serveurs MCP locaux](https://modelcontextprotocol.io/docs/develop/connect-local-servers){target="_blank"}.

>[!TAB Curseur]

### Curseur {#cursor}

Ajoutez l’entrée `marketo` sous `mcpServers` dans `~/.cursor/mcp.json` pour le compte d’utilisateur ou le `.cursor/mcp.json` dans le répertoire du projet. Conservez les serveurs existants et remplacez les espaces réservés d’informations d’identification par vos valeurs :

```json
{
  "mcpServers": {
    "marketo": {
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

Ouvrez **[!UICONTROL Personnaliser]** dans le curseur et vérifiez que le serveur est sous tension. Pour plus d’informations, voir [Configuration du curseur MCP](https://cursor.com/docs/context/mcp){target="_blank"}.

>[!TAB Claude Code CLI]

### Claude Code (CLI) {#claude-code}

Exécutez la commande suivante dans votre terminal en remplaçant vos informations d’identification :

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

La commande ajoute le serveur pour le projet en cours et votre compte utilisateur. Pour le rendre disponible dans plusieurs projets, ajoutez des `--scope user`. Dans Claude Code, exécutez `/mcp` pour vérifier la connexion. Pour plus d’informations, voir [Configuration du MCP du code Claude](https://code.claude.com/docs/en/mcp){target="_blank"}.

>[!TAB  Codex OpenAI ]

### Codex OpenAI {#codex}

Ajoutez les éléments suivants à `~/.codex/config.toml`, en préservant toute configuration existante :

```toml
[mcp_servers.marketo]
url = "https://marketo-mcp.adobe.io/mcp"

[mcp_servers.marketo.env_http_headers]
X-Marketo-Client-Id = "MARKETO_CLIENT_ID"
X-Marketo-Client-Secret = "MARKETO_CLIENT_SECRET"
X-Marketo-Munchkin-Id = "MARKETO_MUNCHKIN_ID"
```

Définissez `MARKETO_CLIENT_ID`, `MARKETO_CLIENT_SECRET` et `MARKETO_MUNCHKIN_ID` dans l’environnement du processus qui exécute le Codex. Les valeurs dans `env_http_headers` sont des noms de variables d’environnement, et non des informations d’identification.

Démarrez une nouvelle session Codex et exécutez `/mcp` pour vérifier la connexion. Pour plus d’informations, voir [Configuration du MCP Codex](https://developers.openai.com/codex/mcp){target="_blank"}.

>[!TAB Gemini CLI]

### Interface de ligne de commande Gemini

Pour ajouter le serveur MCP [!DNL Marketo Engage] à l’interface de ligne de commande Gemini, ajoutez l’entrée de `mcpServers` suivante à `.gemini/settings.json` dans le répertoire du projet ou `~/.gemini/settings.json` de votre compte utilisateur. Conservez les paramètres existants et remplacez les espaces réservés d’informations d’identification par vos valeurs.

```json
{
  "mcpServers": {
    "marketo": {
      "httpUrl": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Vous pouvez également définir `MARKETO_CLIENT_ID`, `MARKETO_CLIENT_SECRET` et `MARKETO_MUNCHKIN_ID` dans votre environnement shell et exécuter la commande suivante. Le shell développe les variables avant que l’interface de ligne de commande ne stocke les valeurs d’en-tête dans votre configuration utilisateur :

```bash
gemini mcp add --transport http -s user marketo https://marketo-mcp.adobe.io/mcp \
  -H "X-Marketo-Client-Id: $MARKETO_CLIENT_ID" \
  -H "X-Marketo-Client-Secret: $MARKETO_CLIENT_SECRET" \
  -H "X-Marketo-Munchkin-Id: $MARKETO_MUNCHKIN_ID"
```

Redémarrez la session et exécutez `/mcp` pour vérifier la connexion. Pour plus d’informations, voir [Configuration MCP de l’interface de ligne de commande Gemini](https://geminicli.com/docs/tools/mcp-server/){target="_blank"}.

>[!TAB VS Code avec Copilote GitHub]

### VS Code avec pilote GitHub {#vscode}

Utilisez la configuration Copilot portable pour les dernières versions de VS Code. Ajoutez l’entrée suivante sous `mcpServers` dans `~/.copilot/mcp-config.json` ou `$COPILOT_HOME/mcp-config.json` si vous définissez `COPILOT_HOME`. Conserver les serveurs existants :

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "${MARKETO_CLIENT_ID}",
        "X-Marketo-Client-Secret": "${MARKETO_CLIENT_SECRET}",
        "X-Marketo-Munchkin-Id": "${MARKETO_MUNCHKIN_ID}"
      }
    }
  }
}
```

Définissez les trois variables d’environnement dans l’environnement qui exécute VS Code ou l’hôte distant qui se connecte au serveur. Démarrez une nouvelle session Copilot et examinez les invites d&#39;approbation de serveur.

Pour utiliser plutôt la configuration guidée, exécutez **[!UICONTROL MCP : Ajouter un serveur]**, sélectionnez **[!UICONTROL HTTP (HTTP ou Server-Sent Events)]**, saisissez l’URL du serveur, puis choisissez **[!UICONTROL Global]** > **[!UICONTROL Copilote global]**. Ajoutez ensuite les en-têtes à la configuration enregistrée.

Les versions plus anciennes prennent également en charge **[!UICONTROL MCP : Ouvrir la configuration utilisateur]** ou `.vscode/mcp.json`. Ces fichiers utilisent `servers` au lieu de la syntaxe `mcpServers` et `${env:MARKETO_CLIENT_ID}` pour les variables d’environnement. Ne mélangez pas les deux formats. Pour plus d’informations, voir [Configuration du MCP du code VS](https://code.visualstudio.com/docs/agents/reference/mcp-configuration){target="_blank"}.

>[!TAB  Copilot Studio ]

### Copilot Studio

Activez l’orchestration générative pour votre agent avant d’ajouter des outils MCP. Utilisez un connecteur personnalisé pour définir les trois en-têtes de [!DNL Marketo] requis.

Sur la page **[!UICONTROL Outils]** de votre agent, sélectionnez **[!UICONTROL Ajouter un outil]** > **[!UICONTROL Nouvel outil]** > **[!UICONTROL Connecteur personnalisé]**. Dans Power Apps, créez un connecteur personnalisé et utilisez l’**[!UICONTROL éditeur Swagger]** pour saisir la définition suivante :

```yaml
swagger: '2.0'
info:
  title: Marketo MCP Server
  description: Connect to the Marketo MCP server.
  version: 1.0.0
host: marketo-mcp.adobe.io
basePath: /
schemes:
  - https
paths:
  /mcp:
    post:
      summary: Marketo MCP Server
      description: Invoke the Marketo MCP server.
      operationId: InvokeServer
      x-ms-agentic-protocol: mcp-streamable-1.0
      parameters:
        - name: X-Marketo-Client-Id
          in: header
          description: Client ID.
          type: string
          required: true
        - name: X-Marketo-Client-Secret
          in: header
          description: Client secret.
          type: string
          required: true
        - name: X-Marketo-Munchkin-Id
          in: header
          description: Munchkin ID.
          type: string
          required: true
      responses:
        '200':
          description: Immediate Response
securityDefinitions: {}
security: []
```

Enregistrez ou mettez à jour le connecteur, revenez à Copilot Studio et ajoutez-le à votre agent. Créez une connexion et indiquez les trois valeurs d’en-tête lorsque vous y êtes invité. Conservez les informations d’identification en dehors de la définition OpenAPI et des instructions de l’agent. Pour plus d’informations, voir [Connexion d’un agent à un serveur MCP existant](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-add-existing-server-to-agent){target="_blank"}.

>[!TAB Glean]

### Glaner {#glean}

Pour connecter Glean au serveur MCP [!DNL Marketo Engage], suivez [les instructions de configuration à distance de Glean pour MCP](https://docs.glean.com/administration/tools/connect-remote-mcp-servers-to-glean){target="_blank"} afin d’ajouter des `https://marketo-mcp.adobe.io/mcp`. Consultez d’abord le catalogue **[!UICONTROL Connecteurs]**. Pour un serveur personnalisé ou non répertorié, utilisez **[!UICONTROL Admin Console]** > **[!UICONTROL Platform]** > **[!UICONTROL Tools]** > **[!UICONTROL Import tools from MCP server]**.

L’équipe d’assistance [ Glean doit configurer les en-têtes personnalisés ](https://docs.glean.com/administration/tools/mcp-custom-headers){target="_blank"} ci-dessous. Incluez le nom du serveur, l’URL et les noms d’en-tête requis dans votre demande d’assistance et coordonnez la gestion sécurisée des valeurs d’identification. Une connexion par clé API standard seule ne fournit pas les trois en-têtes.

| Header | Valeur |
| ------ | ----- |
| `X-Marketo-Client-Id` | Votre identifiant client |
| `X-Marketo-Client-Secret` | Votre Secret Client |
| `X-Marketo-Munchkin-Id` | Identifiant de votre compte Munchkin |

Une fois que Glean a confirmé que les en-têtes sont actifs, testez la connexion avec une requête en lecture seule avant de rendre les outils disponibles pour d’autres utilisateurs.

>[!TAB Autres outils]

### Autres outils {#other-tools}

Adobe héberge le serveur MCP [!DNL Marketo] et l’expose à une URL publique. Tout client MCP prenant en charge des serveurs distants via un transport HTTP en flux continu peut s’y connecter.
Les clients qui prennent en charge les connexions HTTP distantes directes n&#39;ont pas besoin d&#39;un pont ou d&#39;un logiciel serveur installé localement. Si votre outil n’est pas répertorié ci-dessus, utilisez les détails de connexion ci-dessous pour le configurer manuellement.

**Détails de la connexion :**

| Paramètre | Valeur |
| ------- | ----- |
| Transport | HTTP (flux HTTP) |
| URL du serveur | `https://marketo-mcp.adobe.io/mcp` |

**En-têtes d’authentification :**

Envoyez les trois en-têtes avec chaque requête. L’emplacement où vous saisissez l’URL du serveur et les en-têtes dépend de votre outil. Consultez donc sa documentation MCP.

| Header | Valeur |
| ------ | ----- |
| `X-Marketo-Client-Id` | Votre identifiant client |
| `X-Marketo-Client-Secret` | Votre Secret Client |
| `X-Marketo-Munchkin-Id` | Identifiant de votre compte Munchkin |

Si votre outil accepte une configuration JSON, commencez par les exemples [Cursor](#cursor) ou [VS Code](#vscode), puis ajustez les clés et la syntaxe de la variable d’environnement pour qu’elles correspondent au schéma de votre outil.

>[!ENDTABS]

Une fois la connexion établie, demandez à votre assistant AI d’utiliser le serveur MCP [!DNL Marketo] pour répertorier les canaux disponibles sans modifier les données. Inspectez l’appel d’outil et sa réponse pour confirmer que la requête utilise le serveur prévu et renvoie des données sans erreur de connexion ou d’authentification.

## Opérations disponibles

Une fois connecté, vous pouvez demander à votre assistant d’IA d’effectuer des opérations dans les catégories suivantes. Pour connaître les exceptions et les exigences de disponibilité, voir [Opérations MCP prises en charge](mcp-server-operations.md).

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

Rechercher des prospects par adresse e-mail, créer des enregistrements de prospect et gérer l’appartenance à une liste statique.

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

Pour comprendre la configuration de votre [!DNL Marketo], parcourez les dossiers, les canaux, les types de balises et les types d’activités.

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
| « Informations d’identification Marketo non fournies » | Un ou plusieurs parmi `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` ou `X-Marketo-Munchkin-Id` sont manquants. | Vérifiez que tous les en-têtes d’informations d’identification du client Marketo sont présents dans votre configuration. |
| « 401 Non Autorisé » | Vos informations d’identification sont manquantes, non valides ou expirées. Avec les informations d’identification du client Marketo, l’ID client ou le secret client est incorrect. Avec un jeton IMS, le jeton n’est pas valide ou a expiré. | Vérifiez les informations d’identification de votre méthode d’authentification. Pour les informations d’identification du client, revérifiez les **[!UICONTROL ID client]** et **[!UICONTROL Secret client]** dans **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. Pour un jeton IMS, générez un nouveau jeton et mettez à jour l’en-tête `Authorization`. |
| « 403 Interdit » | Vos informations d’identification sont valides, mais votre instance [!DNL Marketo] n’est pas activée pour l’accès au MCP. | Contactez votre administrateur MCP [!DNL Marketo] pour activer l’accès MCP à votre ID de compte Munchkin. |
| « Trop de requêtes » (limite de taux) | Vous avez envoyé trop de requêtes en une courte période, ou trop de requêtes au même moment, et atteint les limites d’API de votre instance [!DNL Marketo]. | Réduisez la fréquence et le nombre de requêtes que vous envoyez à la fois, et patientez quelques instants avant de réessayer. Utilisez un utilisateur d’API dédié pour suivre et gérer votre quota. |
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
* **Gardez les informations d’identification hors du contrôle de version.** Utilisez l’interpolation de variable d’environnement (`${MARKETO_CLIENT_SECRET}`) si votre outil d’IA le prend en charge, de sorte que les informations d’identification ne soient pas stockées en texte brut dans les fichiers de référentiel.

## Gouvernance et conservation des données

### Gestion des informations d’identification

* Les informations d’identification du client ne sont pas conservées côté serveur et sont fournies par le client par demande, ce qui permet de limiter l’exposition des informations d’identification dans le service.

### Modèle d’interaction API

* Utilisation des agents : les agents peuvent utiliser le serveur MCP pour appeler les API Marketo prises en charge.
* Alignement du modèle d’authentification : le service utilise le même modèle d’authentification API externe documenté pour les API Marketo.

### Authentification et autorisation

* Privilèges de moindre importance : les autorisations effectives sont héritées de l’utilisateur de l’API Marketo uniquement affecté au service LaunchPoint du client, ce qui permet l’administration avec les privilèges de moindre importance dans la configuration Marketo du client.
* Aucune persistance des jetons côté serveur : le service continue d’éviter le stockage côté serveur des informations d’identification du client ou des jetons.  

### Journalisation et surveillance

* Journalisation de sécurité : les journaux JSON structurés sont acheminés par le biais de Fluent Bit vers Splunk, avec le masquage des données sensibles et un filtrage supplémentaire pour prendre en charge les exigences de conformité.
* Support d’audit : ces contrôles prennent en charge la surveillance continue de la disponibilité du service, des événements liés à la sécurité et de la qualité opérationnelle.
* Aucun stockage secret côté serveur : les informations d’identification des clients ne sont pas stockées par le déploiement MCP et doivent être fournies par les clients par requête.
* Gestion des jetons : les jetons d’accès sont de courte durée, les réponses des jetons sont marquées comme étant hors magasin et les jetons sont acceptés par le biais de mécanismes d’autorisation standard plutôt que par la transmission de chaînes de requête.
* Accès opérationnel basé sur les rôles : l’accès au déploiement administratif est régi par les rôles de l’infrastructure Adobe et les contrôles basés sur les groupes, tandis que les autorisations relatives aux plans de données sont héritées de la configuration utilisateur de l’API Marketo du client.
* Audit et observabilité : la journalisation de la sécurité, le masquage, la surveillance et les alertes sont activés pour prendre en charge les enquêtes, le suivi de l’intégrité des services et la supervision opérationnelle.
