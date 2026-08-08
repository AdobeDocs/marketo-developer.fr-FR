---
title: E-mail transactionnel
feature: REST API
description: Découvrez comment configurer Marketo pour les e-mails transactionnels et les déclencher via la campagne de requête de l’API REST, avec les étapes de configuration et des exemples de code Java.
exl-id: 057bc342-53f3-4624-a3c0-ae619e0c81a5
TQID: https://experienceleague.adobe.com/eUw2THnwDdIuEO3MsuG4cSaoPnKVvdZ0ZTV-gxP-pJQ
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: aeb0d5a176ffdd0910ee533353593bba95f91d08
workflow-type: tm+mt
source-wordcount: 891
ht-degree: 1%

---

# E-mail transactionnel

Utilisez l’API [Request Campaign](https://developer.adobe.com/marketo-apis/api/mapi#operation/triggerCampaignUsingPOST) pour envoyer des e-mails transactionnels à des enregistrements Marketo spécifiques. Configurez l’e-mail et déclenchez la campagne avant d’effectuer la requête.

- Assurez-vous que le destinataire dispose d’un enregistrement Marketo.
- Créez et approuvez un e-mail transactionnel dans l’instance Marketo.
- Activez une campagne de déclenchement qui utilise « La campagne est demandée », 1. Source : API de service web » et envoie l’e-mail.

Commencez par [créer et approuver l’e-mail](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=fr). Si l’e-mail est juridiquement qualifié d’opérationnel, configurez-le comme étant opérationnel sous Actions d’e-mail > Paramètres d’e-mail :

![Request-Campaign-Email-Settings](assets/request-campaign-email-settings.png)

![Demande-Campagne-Opérationnelle](assets/request-campaign-operational.png)

Valider l&#39;email avant de créer la campagne :

![RequestCampaign-Approve-Draft](assets/request-campaign-approve-draft.png)

Si nécessaire, consultez [Création d’une campagne dynamique](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign.html?lang=fr). Configurez la liste dynamique de la campagne avec le déclencheur La campagne est demandée :

![Requête-Campagne-Liste Dynamique](assets/request-campaign-smart-list.png)

Configurez une étape de flux Envoyer un e-mail qui référence l’e-mail transactionnel :

![Request-Campaign-Flow](assets/request-campaign-flow.png)

Avant l’activation, configurez les paramètres de qualification dans l’onglet Planning . Conservez le paramètre par défaut si chaque enregistrement ne doit recevoir l’e-mail qu’une seule fois. Sinon, autorisez les destinataires à se qualifier à chaque fois ou à une cadence disponible.

Activer la campagne :

![Request-Campaign-Schedule](assets/request-campaign-schedule.png)

## Envoi des appels API

Les exemples Java utilisent le package [minimal-json](https://github.com/ralfstx/minimal-json) pour gérer les représentations JSON.

Avant d’envoyer l’e-mail, vérifiez qu’il existe un enregistrement Marketo pour l’adresse e-mail et récupérez son ID de prospect. Cet exemple suppose que l’adresse e-mail existe déjà.

Utilisez [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByFilterUsingGET) pour récupérer l’identifiant. La méthode principale suivante demande ensuite la campagne :

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
        //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");

        //Create and parameterize an instance of Leads
        //Set your email filterValue appropriately
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("test.requestCamapign@example.com");

        //Get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //Get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of your campaign from Marketo
        int campaignId = 0;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Send the request to Marketo
        rc.postData();
    }
}
```

Extrayez le tableau de résultats de la réponse `JsonObject` et récupérez l’objet à l’index 0 :

```java
JsonArray leadsResult = leadsRequest.getData().get("result").asArray();
int leadId = leadsResult.get(0).asObject().get("id").asInt();
```

Appelez la campagne de demande avec l’identifiant de campagne dans l’URL de demande. Le corps de la requête contient un tableau d’objets JSON avec un membre `id` :

```java
package dev.marketo.blog_request_campaign;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class RequestCampaign {
    private String endpoint;
    private Auth auth;
    public ArrayList leads = new ArrayList();
    public ArrayList tokens = new ArrayList();

    public RequestCampaign(Auth auth, int campaignId) {
        this.auth = auth;
        this.endpoint = this.auth.marketoInstance + "/rest/v1/campaigns/" + campaignId + "/trigger.json";
    }
    public RequestCampaign setLeads(ArrayList leads) {
        this.leads = leads;
        return this;
    }
    public RequestCampaign addLead(int lead){
        leads.add(lead);
        return this;
    }
    public RequestCampaign setTokens(ArrayList tokens) {
        this.tokens = tokens;
        return this;
    }
    public RequestCampaign addToken(String tokenKey, String val){
        JsonObject jo = new JsonObject().add("name", tokenKey);
        jo.add("value", val);
        tokens.add(jo);
        return this;
    }
    public JsonObject postData(){
        JsonObject result = null;
        try {
            JsonObject requestBody = buildRequest(); //builds the Json Request Body
            System.out.println("Executing RequestCampaign call\n" + "Endpoint: " + endpoint + "\nRequest Body:\n"  + requestBody);
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
            urlConn.setRequestMethod("POST");
            urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
            OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
            wr.write(requestBody.toString());
            wr.flush();
            InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
            Reader reader = new InputStreamReader(inStream);
            result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
            System.out.println("Result:\n" + result);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return result;
    }

    private JsonObject buildRequest(){
        JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
        JsonObject input = new JsonObject();
        JsonArray leadsArray = new JsonArray();
        for (int lead : leads) {
            JsonObject jo = new JsonObject().add("id", lead);
            leadsArray.add(jo);
        }
        input.add("leads", leadsArray);
        JsonArray tokensArray = new JsonArray();
        for (JsonObject jo : tokens) {
            tokensArray.add(jo);
        }
        input.add("tokens", tokensArray);
        requestBody.add("input", input);
        return requestBody;
    }

}
```

Cette classe a un constructeur qui prend une authentification et l’identifiant de la campagne. Les leads sont ajoutés à l’objet soit en transmettant un `ArrayList<Integer>` contenant les identifiants des enregistrements à setLeads, soit en utilisant addLead, qui prend un entier et l’ajoute à ArrayList existant dans la propriété leads. Pour déclencher l’appel API afin de transmettre les enregistrements de prospect à la campagne, postData doit être appelé, qui renvoie un objet JsonObject contenant les données de réponse de la requête. Lorsque la campagne de requête est appelée, chaque prospect transmis à l’appel est traité par la campagne de déclenchement cible dans Marketo et l’e-mail précédemment créé lui est envoyé. Félicitations, vous avez déclenché un e-mail via l’API REST Marketo. Gardez un œil sur la Partie 2, où nous examinons la personnalisation dynamique du contenu d’un e-mail par le biais de Request Campaign.

### Création de votre e-mail

Pour personnaliser notre contenu, nous devons d’abord configurer un [programme](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program.html?lang=fr) et un [e-mail](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=fr) dans Marketo. Pour générer notre contenu personnalisé, nous devons créer des jetons dans le programme, puis les placer dans l’e-mail que nous allons envoyer. Pour plus de simplicité, nous n’utilisons qu’un seul jeton dans cet exemple, mais vous pouvez remplacer un nombre illimité de jetons dans un e-mail, dans l’e-mail de l’expéditeur, le nom de l’expéditeur, le destinataire de la réponse ou tout autre élément de contenu de l’e-mail. Créons donc un jeton de texte enrichi à remplacer et appelons-le « bodyReplacement ». Le texte enrichi nous permet de remplacer n’importe quel contenu du jeton par HTML arbitraire que nous voulons saisir.

![Nouveau-jeton](assets/New-Token.png)

Les jetons ne peuvent pas être enregistrés s’ils sont vides. Allez-y, insérez du texte d’espace réservé ici. Nous devons maintenant insérer notre jeton dans l’e-mail :

![Add-Token](assets/Add-Token.png)

Ce jeton sera désormais accessible pour remplacement par le biais d’un appel de campagne de demande. Ce jeton peut être aussi simple qu’une seule ligne de texte qui doit être remplacée par e-mail, ou peut inclure presque toute la disposition de l’e-mail.

### Le code

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
        //Create an instance of Auth so that we can authenticate with our Marketo instance
        Auth auth = new Auth("Client ID - CHANGE ME", "Client Secret - CHANGE ME", "Host - CHANGE ME");

        //Create and parameterize an instance of Leads
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");

        //get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of our campaign from Marketo
        int campaignId = 1578;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Create the content of the token here, and add it to the request
        String bodyReplacement = "<div class=\"replacedContent\"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Si le code vous est familier, c’est qu’il ne comporte que deux lignes supplémentaires issues de la méthode principale ci-dessus. Cette fois, nous créons le contenu de notre jeton dans la variable bodyReplacement , puis nous utilisons la méthode addToken pour l’ajouter à la requête. addToken prend une clé et une valeur, puis crée une représentation JsonObject et l’ajoute au tableau de jetons interne. Elle est ensuite sérialisée pendant la méthode postData et crée un corps qui ressemble à ceci :

```json
{
    "input":
    {
        "leads": [
            {
                "id": 1
            }
        ],
        "tokens": [
            {
                "name": "{{my.bodyReplacement}}",
                "value": "<div class=\"replacedContent\"><p>This content has been replaced</p></div>"
            }
        ]
    }
}
```

Combinée, notre sortie console ressemble à ceci :

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with ...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"apiuser@mktosupport.com"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class=\"replacedContent\"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

## Conclusion

Cette méthode peut être étendue de différentes manières, en modifiant le contenu des e-mails dans des sections de disposition individuelles ou en dehors des e-mails, ce qui permet de transmettre des valeurs personnalisées dans des tâches ou des moments intéressants. Chaque fois qu’un jeton peut être utilisé à partir d’un programme, il peut être personnalisé à l’aide de cette méthode. Une fonctionnalité similaire est également disponible avec l’appel [Planifier la campagne](https://developer.adobe.com/marketo-apis/api/mapi#operation/scheduleCampaignUsingPOST) qui vous permet de traiter les jetons dans une campagne par lots entière. Elles ne peuvent pas être personnalisées par prospect, mais sont utiles pour personnaliser le contenu d’un large éventail de prospects.
