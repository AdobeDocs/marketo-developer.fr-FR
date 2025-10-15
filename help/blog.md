---
title: Archive de blog
description: Archive du blog du développeur de Marketo 2014-2023 offrant des publications historiques sur Forms 2.0, Zapier, les mises à jour des API, l’obsolescence de SOAP et la migration vers REST.
exl-id: d7ae88dd-9938-4957-9798-db43090dab4e
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '61723'
ht-degree: 0%

---

# Archive de blog

>[!INFO]
>
>Il s’agit d’une archive du blog Marketo couvrant la période 2014-2023. Il est fourni ici uniquement à titre de référence historique.
>&#x200B;>Certaines informations peuvent être obsolètes.  Consultez toujours la documentation actuelle pour connaître les dernières fonctionnalités.
>

>[!IMPORTANT]
>L’API SOAP sera abandonnée et ne sera plus disponible après le 31 janvier 2026. Tout nouveau développement doit être effectué avec l’API REST Marketo et les services existants doivent être migrés à cette date pour éviter toute interruption de service. Si un service utilise l’API SOAP, consultez le [Guide de migration de l’API SOAP](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/migration) pour plus d’informations sur la migration.
>

>[!IMPORTANT]
>La prise en charge de l’authentification à l’aide du paramètre de requête `access_token` sera supprimée le 31 janvier 2026. Si votre projet utilise un paramètre de requête pour transmettre le jeton d’accès, il doit être mis à jour afin d’utiliser l’en-tête [Authorization](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/rest/authentication#using-an-access-token) dès que possible. Le nouveau développement doit utiliser exclusivement l’en-tête d’autorisation .
>

## Bienvenue sur le blog du développeur de Marketo

Bienvenue aux développeurs et développeuses Marketo ! Chez Marketo, nous avons été très occupés à publier de nouvelles fonctionnalités pour aider les professionnels du marketing à réussir plus que jamais. Les professionnels du marketing bénéficient aujourd’hui du soutien d’une communauté de développeurs phénoménale. Ce blog est dédié aux développeurs web et aux ingénieurs en logiciels qui prennent en charge l’évolution rapide des besoins des professionnels du marketing modernes. À mesure que la plateforme Marketo évolue, nous annonçons de nouvelles options d’intégration et des mises à jour de la version des API au fur et à mesure de leur lancement. Nous présenterons également une nouvelle série d’articles pratiques dans lesquels nous partagerons des exemples de code et des bonnes pratiques sur l’intégration à la plateforme Marketo. Le premier article de cette série vous explique comment récupérer efficacement des informations sur les personnes (clients/contacts/prospects) stockées dans Marketo à l’aide de l’API . Veuillez vous abonner en utilisant le formulaire ci-dessus pour rester à jour. Nous vous envoyons des mises à jour par e-mail à mesure que de nouveaux articles sont publiés.

Publié le _2014-03-06_ par _David_

## Mises À Jour De Janvier 2014

### Formulaires 2.0

Forms 2.0 permet aux marketeurs de créer des formulaires web beaux, stables et flexibles sans connaissances en matière de programmation. Outre l’éditeur considérablement amélioré, la logique conditionnelle et la conception robuste, il est désormais plus facile que jamais d’incorporer Marketo forms dans n’importe quelle page de votre site web. L’équipe de développement peut utiliser JavaScript pour étendre les fonctionnalités de base. Les cas d’utilisation incluent :

* Masquage d’un formulaire après un envoi réussi afin que le visiteur reste sur la page après avoir rempli le formulaire
* Affichage d’un message d’erreur personnalisé lors de l’envoi en fonction de la logique commerciale personnalisée
* Affichage du formulaire dans une boîte de dialogue de style Lightbox

La documentation destinée aux développeurs est disponible [ici](/help/javascript-api/forms-api-reference.md).

### API SOAP version 2_3 désormais disponible

* [getLeadChanges:](/help/soap-api/getleadchanges.md) Ajout d’un champ de requête `activityNameFilter`
* [ListOperation:](/help/soap-api/listoperation.md) champ de demande supprimé `skipActivityLog`

**Remarque :** les révisions de l’API SOAP sont rétrocompatibles

Publié le _2014-01-26_ par _Travis Kaufman_

## Zapier Partie II : annonce de l’intégration de Marketo

Dans un précédent article, nous avons expliqué comment utiliser Zapier pour intégrer des sources de données externes à Marketo. L&#39;article présentait une approche pratique pour créer votre propre workflow Zapier (ou « Zap ») à partir de zéro pour intégrer Marketo et d&#39;autres applications. Alors, vous aimez l&#39;idée d&#39;utiliser Zapier avec Marketo, mais vous avez besoin d&#39;aide pour commencer ? Bonne nouvelle ! Zapier vient de publier un certain nombre d&#39;exemples de Zaps pour Marketo qui vous permettent de démarrer rapidement :

* Capturer de nouveaux leads
* Développez vos clients
* Distribuer les coordonnées
* Réagir aux nouveaux prospects

Outre ces exemples, vous pouvez parcourir la page des [intégrations de Marketo](https://zapier.com/apps/marketo/integrations) avec des centaines d&#39;autres applications sur Zapier et créer vos propres workflows automatisés en quelques minutes. Aucun codage n’est requis. Gagnez du temps et laissez l&#39;automatisation faire votre travail manuel. Soyez créatif ! Le ciel est la limite !

Publié le _2016-06-01_ par _David_

## Récupérer des informations sur les clients et les prospects dans Marketo à l’aide de l’API

Vous pouvez récupérer des informations sur les clients et les prospects stockées dans Marketo à l’aide des API `getLead` et [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) de SOAP. Il est souvent préférable d’extraire ces informations de manière récurrente afin de maintenir un autre système à jour au fur et à mesure que les informations des clients et des prospects sont mises à jour ou que de nouveaux enregistrements sont créés dans Marketo. Nous vous montrons l’exemple de code qui serait exécuté de manière récurrente pour interroger Marketo concernant les mises à jour. Le diagramme ci-dessous illustre les appels API effectués selon un minuteur périodique défini. Selon le cas d’utilisation, le minuteur périodique peut être défini pour exécuter le code ci-dessous toutes les 10 minutes.

Le premier appel à [`getMultipleLeads`](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) définit la période, la taille du lot et les champs à renvoyer. Tous les prospects de Marketo qui ont été mis à jour au cours de la période spécifiée sont renvoyés avec un streamPosition lorsque davantage d’enregistrements sont disponibles que le batchSize spécifié.

**Demande SOAP du premier appel à getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector>
  <batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

**Réponse SOAP du premier appel à getMultipleLeads :**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
 <returnCount>2</returnCount><remainingCount>24</remainingCount><newStreamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</newStreamPosition><leadRecordList>
      <leadRecord>
        <Id>84105</Id>
        <Email>eimang@marketo.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>French</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>Lead</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <leadRecord>
        <Id>1089965</Id>
        <Email>t@t.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
    </leadRecordList>
  </result>
</ns2:successGetMultipleLeads>
```

Tant que la valeur `<remainingCount/>` est supérieure à 0, vous effectuez des appels suivants à `getMultipleLeads` pour paginer le reste en transmettant la valeur `<newStreamPosition/>` renvoyée lors de l’appel précédent dans le paramètre `<streamPosition/>` . **Demande SOAP pour l&#39;appel suivant à getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector><streamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</streamPosition><batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

Cette logique se poursuit tant que `<remainingCount/>` est supérieur à zéro. **Consultez ci-dessous un exemple de programme Java qui exécute le scénario décrit ci-dessus.**

```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.GregorianCalendar;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import javax.xml.datatype.DatatypeFactory;
import javax.xml.datatype.XMLGregorianCalendar;

public class GetMultipleLeads {
  public static void main(String[] args) {
    System.out.println("Executing GetMultipleLeads");
        try {
        URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
        String marketoUserId = "CHANGE ME";
        String marketoSecretKey = "CHANGE ME";
        QName serviceName = new QName("<http://www.marketo.com/mktows/>",
        "MktMktowsApiService");
        MktMktowsApiService service = new
        MktMktowsApiService(marketoSoapEndPoint, serviceName);
        MktowsPort port = service.getMktowsApiSoapPort();

        // Create Signature
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String text = df.format(new Date());
        String requestTimestamp = text.substring(0, 22) + ":" +         text.substring(22);
        String encryptString = requestTimestamp + marketoUserId ;

        SecretKeySpec secretKey = new         SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");

        Mac mac = Mac.getInstance("HmacSHA1");
        mac.init(secretKey);
        byte[] rawHmac = mac.doFinal(encryptString.getBytes());
        char[] hexChars = Hex.encodeHex(rawHmac);
        String signature = new String(hexChars);

        // Set Authentication Header
        AuthenticationHeader header = new AuthenticationHeader();
        header.setMktowsUserId(marketoUserId);
        header.setRequestTimestamp(requestTimestamp);
        header.setRequestSignature(signature);

        // Create Request
        ParamsGetMultipleLeads request = new ParamsGetMultipleLeads();

        // Build Request Using LastUpdateAtSelector
        LastUpdateAtSelector leadSelector = new LastUpdateAtSelector();
        GregorianCalendar gc = new GregorianCalendar();
        gc.setTimeInMillis(new Date().getTime());
        gc.add( GregorianCalendar.DAY_OF_YEAR, -20);
        DatatypeFactory factory = DatatypeFactory.newInstance();
        ObjectFactory objectFactory = new ObjectFactory();
        JAXBElement<XMLGregorianCalendar> until =         objectFactory.createLastUpdateAtSelectorLatestUpdatedAt(factory.newXMLGregorianCalendar(gc));

        GregorianCalendar since = new GregorianCalendar();
        since.setTimeInMillis(new Date().getTime());
        since.add( GregorianCalendar.DAY_OF_YEAR, -21);
        leadSelector.setOldestUpdatedAt(factory.newXMLGregorianCalendar(since));
        leadSelector.setLatestUpdatedAt(until);
        request.setLeadSelector(leadSelector);

        ArrayOfString attributes = new ArrayOfString();
        attributes.getStringItems().add("FirstName");
        attributes.getStringItems().add("LastName");
        attributes.getStringItems().add("Email");
        request.setIncludeAttributes(attributes);

        JAXBElement<Integer> batchSize = new
        ObjectFactory().createParamsGetMultipleLeadsBatchSize(2);
        request.setBatchSize(batchSize);

        SuccessGetMultipleLeads result = null;

        int count = 0;

        do {
        if (count > 0) {
        // Set the streamPosition on subsequent calls to paginate
        through large result sets
        String pos = result.getResult().getNewStreamPosition();
        JAXBElement<String> streamPos = new
        ObjectFactory().createParamsGetMultipleLeadsStreamPosition(pos);
        request.setStreamPosition(streamPos);
        }

        JAXBContext context =
        JAXBContext.newInstance(ParamsGetMultipleLeads.class);
        Marshaller m = context.createMarshaller();
        m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        System.out.println("REQUEST:");
        m.marshal(request, System.out);
        result = port.getMultipleLeads(request, header);
        System.out.println("RESPONSE:");
        m.marshal(result, System.out);
        count = result.getResult().getReturnCount();
        } while (result.getResult().getRemainingCount() > 0);
        }

        catch(Exception e) {
        // Update to include appropriate retry/error handling
        e.printStackTrace();
        }

  }

}
```

### Conseils et astuces

Lors de l’extraction d’un grand nombre de contacts hors de Marketo, il est recommandé d’ajuster la requête d’API avec les paramètres suivants :

* `<includeAttributes/>` : il est recommandé de ne demander que les champs que vous souhaitez conserver synchronisés avec votre système. Cela réduit la payload de la réponse et augmente les performances des requêtes.
* `<batchSize/>` : l’API prend en charge jusqu’à 1 000 enregistrements à renvoyer dans un seul appel. Le réglage de cette valeur sur 500 enregistrements réduit également la charge utile de la réponse.
* `<LastUpdatedAtSelector/>` : il est recommandé de définir à la fois la `<oldestUpdatedAt/>` et le paramètre `<latestUpdatedAt/>` pour limiter la période. Par exemple, au lieu d’effectuer une seule demande pour l’équivalent d’un an de données. Divisez les appels d’API pour demander des périodes plus petites.

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-03-05_ par _Travis Kaufman_

## Mises À Jour De Février 2014

### Mise à jour de l’API SOAP

* [syncMObjects](/help/soap-api/syncmobjects.md) : vous pouvez désormais ajouter et mettre à jour des balises et des canaux pour les programmes existants.

Les mises à jour sont intégrées dans le WSDL [2_3](http://app.marketo.com/soap/mktows/2_3?WSDL).

Publié le _2014-02-26_ par _Travis Kaufman_

## Mises À Jour De Mars 2014

### Mise à jour de l’API SOAP

* Amélioration des performances de [syncLead](/help/soap-api/synclead.md) et [syncMultipleLeads](/help/soap-api/syncmultipleleads.md)

Les mises à jour sont intégrées dans le WSDL [2_3](http://app.marketo.com/soap/mktows/2_3?WSDL).

Publié le _2014-03-20_ par _Travis Kaufman_

## Fusion de l’activité Visiteur anonyme lorsque le visiteur remplit un formulaire

Dans l’article de blog intitulé « Capturer l’activité des visiteurs anonymes en fonction de la logique commerciale », nous avons expliqué comment créer des enregistrements de prospects anonymes dans Marketo en fonction d’événements personnalisés. Dans cet article de blog, nous nous appuierons sur cet article et associerons un enregistrement de prospect anonyme à un utilisateur connu une fois que vous aurez reçu les coordonnées de l’utilisateur. Le code de suivi Marketo [Munchkin](/help/javascript-api/lead-tracking.md) vous aide à suivre les visites sur votre site web. La première fois qu’une personne visite une page de votre site web comportant le code de suivi Munchkin, Marketo crée un prospect anonyme et utilise un cookie de navigateur pour le suivre. Une fois identifiés, ils deviennent un prospect connu et l’historique associé à leur cookie de navigateur est fusionné dans leur enregistrement de prospect Marketo. **Des prospects anonymes sont créés lorsque quelqu’un :**

1. Première visite de votre page de destination Marketo
1. Visite une page qui comporte un code de suivi Munchkin
1. Cliquez sur le lien Afficher en tant que page web dans un email Marketo

**Un prospect anonyme est fusionné dans un nouveau prospect connu ou un prospect existant lorsque quelqu’un :**

1. Clic sur un lien vers un e-mail Marketo
1. Remplit un formulaire Marketo
1. Utilise l’une des techniques ci-dessous pour associer un prospect anonyme à un enregistrement connu.

Pour envoyer des données de personne ou associer un cookie de tracking web de navigateur à l’enregistrement de personne correspondant dans Marketo, utilisez l’une des méthodes suivantes : **Envoi côté navigateur** Si votre cas d’utilisation nécessite l’envoi de données de personne à partir du navigateur, vous devez utiliser l’envoi du formulaire en arrière-plan. **Envoi côté serveur** Si vous n’avez pas besoin d’un envoi côté navigateur, l’API REST offre de nombreuses méthodes d’envoi de données de personne, ainsi qu’une méthode spécifique pour associer des cookies aux enregistrements de personne.

Publié le _2014-04-22_ par _Murta_

## Mises À Jour De La Version D’Avril 2014

### Mise à jour de sécurité de Marketo Forms

Nous avons introduit une limite au nombre et à la fréquence des envois de publications de formulaires à partir d’une seule adresse IP. Cette limite est désormais appliquée à 30 publications par minute pour protéger nos clients contre l’utilisation malveillante des envois de formulaires programmatiques. L’API [syncLead](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/leads/synclead) est le véhicule d’intégration recommandé pour l’envoi programmatique de nouveaux contacts dans Marketo.

Publié le _2014-04-29_ par _Travis Kaufman_

## Remplacer votre post-intégration de formulaire par l’API Marketo

Il est courant pour les marketeurs qui utilisent Marketo d’associer le succès d’une campagne marketing donnée en fonction du nombre de contacts/prospects qui ont rempli un formulaire web spécifique. Le marketeur peut simplement créer un nouveau formulaire Marketo, le placer sur une page de destination, puis configurer une campagne de déclenchement dans Marketo pour associer tous les contacts qui ont rempli ce formulaire spécifique en tant que succès pour ce programme marketing. (voir la capture d’écran ci-dessous). Il est naturel d’utiliser cette même approche lors de l’enregistrement de la réussite d’un programme, même si la réussite de la campagne réside en dehors du fait que quelqu’un a rempli un formulaire. Par exemple, lorsqu’un spécialiste marketing exécute une offre promotionnelle et que la conversion consiste à appeler et à planifier un rendez-vous. Le client potentiel ne remplit pas de formulaire à un moment ou à un autre du processus. Dans l’article ci-dessous, nous vous montrons comment utiliser l’API syncLead de Marketo pour créer un nouveau contact s’il s’agit de nouveaux contacts, puis demander à l’API Campaign d’enregistrer le succès d’un programme marketing donné.

Publié le _1970-01-01_ par _Travis Kaufman_

## Supprimer un prospect avec l’API Marketo

Supposons que vous souhaitiez supprimer un prospect via l’API Marketo. Pour ce faire, appelez l’API requestCampaign dans une campagne qui comporte une action de flux prédéfinie permettant de supprimer un prospect. Nous vous montrons d’abord comment créer une campagne intelligente, ensuite comment configurer un déclencheur pour exécuter une campagne via l’API, enfin comment définir la suppression d’un prospect dans le cadre d’une action de flux et, enfin, un exemple de code qui serait utilisé pour exécuter cette campagne. **Création d’une campagne intelligente dans Marketo** les campagnes intelligentes dans Marketo exécutent toutes vos activités marketing. Dans une campagne dynamique, vous pouvez configurer une série d’actions automatisées pour prendre en charge une liste dynamique de contacts. En cas de suppression d’un prospect, vous devez configurer un déclencheur dans la campagne, comme illustré ci-dessous. Commençons par configurer la campagne intelligente.

1. Dans Activités marketing, choisissez un programme puis, dans la liste déroulante Nouveau , cliquez sur Nouvelle ressource locale 1. Clic sur la campagne intelligente
1. Saisissez le nom de la campagne intelligente et cliquez sur Créer Ajouter des déclencheurs à une campagne intelligente** L’ajout de déclencheurs à une campagne intelligente vous permet de lancer une campagne intelligente sur une personne à la fois en fonction d’un événement en direct, qui est dans ce cas une requête via l’API requestCampaign.
1. Recherchez le déclencheur « La campagne est demandée », puis faites-le glisser et déposez-le sur la zone de travail.
1. Dans le déclencheur, sélectionnez « est » et « API de service web ».

**Comment créer une action Supprimer un flux de leads sur une campagne** Cliquez sur Flux dans le menu supérieur. Dans le menu de droite, recherchez « Supprimer le prospect », puis faites-le glisser vers le centre pour l’ajouter en tant que déclencheur à la campagne. Remarque : si vous supprimez un prospect de Marketo uniquement et que vous le laissez dans votre CRM, lorsque l’une des données de ce prospect est mise à jour, le prospect est recréé dans Marketo.  **Exemple de code pour appeler l’API requestCampaign** Après avoir configuré la campagne et les triggers dans l’interface Marketo, nous vous montrons comment utiliser l’API pour exécuter cette campagne. Le premier exemple est une requête XML, le second est une réponse XML et le dernier est un exemple de code Java qui peut être utilisé pour générer la requête XML. Nous vous montrons également comment trouver l’identifiant de campagne utilisé lors d’un appel à l’API requestCampaign. L’appel API nécessite également que vous connaissiez à l’avance l’identifiant de la campagne Marketo. Vous pouvez déterminer l&#39;identifiant de la campagne à l&#39;aide de l&#39;une des méthodes suivantes :

1. Utiliser l’API [getCampaignsForSource](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET)
1. Ouvrez la campagne Marketo dans un navigateur et regardez la barre d’adresse URL. L’identifiant de campagne (représenté sous la forme d’un entier de 4 chiffres) se trouve immédiatement après « SC ». Par exemple : `https://app-stage.marketo.com/#SC**1025**A1`. La partie en gras est l’identifiant de campagne « 1025 ». Requête SOAP pour requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Réponse de SOAP pour requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Vous trouverez ci-dessous un exemple de programme Java qui exécute le scénario décrit ci-dessus.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-05-16_ par _Murta_

## Suivi des activités personnalisées avec l’API Munchkin -

Souhaitons effectuer le suivi de l’activité personnalisée dans Marketo. Par exemple, vous avez une vidéo sur une page web et vous souhaitez effectuer le suivi des visiteurs qui regardent plus de 50 % d’une vidéo. Pour ce faire, utilisez la fonction de suivi d’activité personnalisée de Munchkin. Pour ce faire, il suffit d’écouter un événement sur la page, à savoir la vidéo qui atteint 50 %, puis d’appeler l’API Munchkin. Pour ce faire, nous devons configurer une activité personnalisée dans Marketo à appeler en fonction de cet événement sur la page. Nous utilisons YouTube pour le lecteur vidéo et leur API Iframe [YouTube](https://developers.google.com/youtube/iframe_api_reference) pour appeler la méthode sur l’API Munchkin.

Nous vous montrons tout d’abord comment générer le code de suivi Munchkin dans Marketo, ensuite comment modifier votre exemple de code Munchkin pour le déclencher en fonction des événements de page, enfin comment configurer une campagne avec une liste dynamique définie par des actions sur la page avec des étapes de flux et enfin comment vérifier qu’une visite de page effectuée par un utilisateur anonyme a été enregistrée dans Marketo. ==== Cet article de blog est un exemple concret du code expliqué. Veuillez remplir ce formulaire afin d’être un utilisateur connu de Marketo. Ainsi, lorsque vous regardez 50 % de la vidéo, vous recevez le reste de la vidéo, et si vous regardez 100 % de la vidéo, vous recevez un lien vers un autre article de blog. === <https://developers.google.com/youtube/iframe_api_reference>

**Comment générer le code de suivi Munchkin** le code de suivi Munchkin vous permet de suivre les visites sur votre site web. Il existe trois types de code Munchkin décrits ci-dessous, mais dans cet exemple, nous utilisons le code de suivi Munchkin asynchrone . A) Simple : contient le moins de lignes de code, mais ne s’optimise pas pour le temps de chargement des pages web. Ce code charge la bibliothèque jQuery chaque fois qu’une page web est chargée. B) Asynchrone : réduit le temps de chargement des pages web. Ce code vérifie si la bibliothèque jQuery existe déjà, la charge si elle est manquante et l’utilise pour exécuter le code de suivi une fois le reste de la page web chargé. C) Requête asynchrone : réduit le temps de chargement des pages web et améliore également les performances du système. Ce code suppose que vous disposez déjà de jQuery et ne vérifie pas pour le charger.

1. Cliquez sur Admin en haut à droite de l’application.
1. Cliquez sur Munchkin dans l’arborescence de gauche.
1. Sélectionnez Asynchrone pour le type de code de suivi.
1. Cliquez sur le code de suivi JavaScript à placer sur votre site web, puis copiez-le. **Code YouTube** <https://developers.google.com/youtube/js_api_reference#EventHandlers> <https://developers.google.com/youtube/iframe_api_reference> lecteur.

`getCurrentTime()` Renvoie le temps écoulé en secondes depuis le début de la lecture de la vidéo. `player.getDuration()` Renvoie la durée, en secondes, de la vidéo en cours de lecture. Notez que `getDuration()` renvoie 0 jusqu’à ce que les métadonnées de la vidéo soient chargées, ce qui se produit normalement juste après le début de la lecture de la vidéo. Lorsqu’un utilisateur non-cookie accède à une page contenant le code de suivi Munchkin, un nouveau cookie est créé dans le navigateur de l’utilisateur et un nouveau prospect anonyme est créé dans Marketo. Si l’utilisateur est déjà cookie et qu’il existe déjà un prospect dans Marketo, la visite de la page est enregistrée dans le journal d’activité de l’utilisateur dans Marketo. **Exemple de code pour l’utilisateur de cookie et l’événement de suivi** Placez le code de suivi sur vos pages web juste avant la balise `</body>`. Les landing pages créées dans Marketo contiennent automatiquement le code de suivi. Vous n’avez donc pas besoin de placer ce code dessus. Cet exemple de code appelle l’API Munchkin une fois le script chargé :

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Cet exemple de code appelle l’API Munchkin une fois que l’utilisateur a séjourné sur la page pendant 5 secondes et qu’il a également fait défiler la page de 500 pixels vers le bas :

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Comment vérifier que la visite d’une page par un utilisateur anonyme a été enregistrée dans Marketo**

1. Cliquez sur Analytics dans le menu supérieur, puis sur Nouveau rapport. Choisissez Activité de page web comme type de rapport, puis nommez le rapport.
1. Après avoir créé un rapport, cliquez sur Liste dynamique. Sélectionnez ensuite le filtre Page web visitée dans la zone de droite. Saisissez la page web dans laquelle vous avez placé le code de suivi Munchkin.
1. Cliquez sur Configuration. Sélectionnez Visiteurs anonymes à partir de FAI et définissez l’option sur Affiché .
1. Cliquez sur Rapport. L’activité est désormais suivie sur la page web que vous avez sélectionnée.
1. Double-cliquez sur l’enregistrement du prospect, qui affiche alors le journal d’activité sur lequel vous pouvez voir la page spécifique visitée par l’utilisateur anonyme.

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Pour les pages comportant du contenu multimédia, par exemple, vous pouvez effectuer un suivi personnalisé. Un exemple courant consiste à ajouter le code de suivi Munchkin à la page et à utiliser l’API Munchkin pour générer des événements dans votre instance Marketo pour des activités telles que la lecture d’une vidéo ou l’écoute d’un clip audio. Nous vous recommandons de placer le code de suivi Munchkin sur la plupart ou la totalité de vos pages web. Le code de suivi Munchkin est automatiquement inclus dans les pages de destination que vous créez à l’aide de Marketo. Utilisez cet appel pour enregistrer que l’utilisateur a fait quelque chose, comme visiter une page dans Ajax, Flash ou un autre environnement RIA. L’URL ne doit pas contenir « » ni aucun domaine, mais elle peut pointer vers n’importe quelle page, même des pages qui n’existent pas. Si vous souhaitez ajouter des paramètres d’URL, utilisez l’argument params .
L’événement s’affiche en tant qu’événement de page web de visite dans le journal d’activité de l’utilisateur ou de l’utilisatrice sous le domaine de la page web appelante. Remarque Votre premier appel à `mktoMunchkin()` crée toujours un événement Visiter la page Web pour la page active. Vous n’avez pas besoin d’appeler `visitWebPage` , sauf si vous souhaitez effectuer le suivi d’une visite supplémentaire de la page web. `mktoMunchkinFunction('visitWebPage', { url: '/MyFlashMovie/Story1', params: 'x=y&2=3' });` Remarque Assurez-vous d’avoir accès à un développeur JavaScript expérimenté. L’assistance technique de Marketo n’est pas configurée pour vous aider à résoudre les problèmes liés au JavaScript personnalisé. L’API Munchkin JavaScript vous permet d’intégrer un système web tiers à votre compte Marketo. Grâce à certains développements web, vous pouvez capturer de nouveaux prospects ou mettre à jour les prospects actuels avec des applications existantes sur votre site web. Supposons que vous ayez une application web pour l’enregistrement des clients qui recueille de nouvelles informations sur les clients. Avec juste un peu de programmation, vous pouvez également avoir des informations de prospect pour les utilisateurs capturés dans Marketo, ainsi qu’un cookie Marketo défini pour le suivi web futur.

En outre, une autre fonctionnalité permet à vos développeurs web de capturer et de suivre les informations d’activité web à partir d’environnements web riches, tels que Flash ou Ajax. Remarque : si vous disposez des ressources de développement appropriées, vous devez envisager d’utiliser notre API SOAP pour l’intégration au lieu de cette API. L’API SOAP est plus robuste et dispose de davantage de fonctionnalités que l’API Munchkin. Exigences en matière d’API Marketo SOAP Pour que cela fonctionne, vous devez inclure le code Munchkin JavaScript sur votre page web. Vous trouverez les balises de script nécessaires dans le tutoriel Munchkin. Activez également l’API Munchkin, qui est également décrite dans le tutoriel.
Sous le capot Après avoir effectué un appel API Munchkin, l’utilisateur reçoit automatiquement un cookie s’il n’en a pas. Dans Marketo, il consigne l’événement (clic sur un lien, visite une page web ou un nouveau prospect) dans le journal d’activité de la personne. Si vous utilisez le lien de clic ou consultez un appel de page web, l’événement est ajouté au journal d’activité de ce prospect (connu ou anonyme). S’il s’agit d’un nouveau prospect et que vous utilisez l’appel de prospect associé, ce prospect devient un prospect connu et son historique d’activité est conservé. S’il s’agit d’un prospect existant (en fonction de la correspondance d’adresse e-mail), toute valeur modifiée ou nouvelle sera mise à jour dans l’enregistrement de ce prospect.

Voici la forme générale d&#39;un appel `munchkinFunction`. Incluez-le en tant que balises dans votre page web, quel que soit l’endroit où vous souhaitez l’appeler. Vous pouvez l’appeler comme n’importe quelle autre fonction JavaScript. Cependant, vous devez appeler la fonction de tracking Munchkin `mktoMunchkin()` avant d&#39;effectuer tout appel `mktoMunchkinFunction()` :

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"> mktoMunchkin("###-###-###"); mktoMunchkinFunction('function', { key: 'value', key2: 'value'}, 'hash');
```

Où : `###-###-###` est l’identifiant de compte Munchkin pour votre fonction de compte est l’appel que vous souhaitez faire de params un tableau de paramètres nécessaires pour que le hachage de l’appel ne soit nécessaire que pour `associateLead`

Publié le _1970-01-01_ par _Murta_

## Obtention de données dans Marketo

La présentation ci-dessous vous montre les différentes manières d’importer des données dans Marketo. Il se concentre sur les formulaires, les objets personnalisés et les intégrations.

[Obtention de données dans Marketo](https://www.slideshare.net/MurtzaManzur/getting-data-into-marketo-35662408) depuis [Murtza Manzur](https://www.slideshare.net/MurtzaManzur)

Publié le _2014-06-06_ par _Murta_

## Création de leads dans un Workspace

Supposons que votre entreprise ait deux divisions : l’Amérique du Nord et l’Europe. Vous souhaitez segmenter vos prospects en fonction de la division de la société dans Marketo. Pour ce faire, utilisez les espaces de travail, qui sont une fonctionnalité de Marketo permettant de restreindre l’accès aux prospects. Pour ce faire, vous créeriez un espace de travail pour l’Amérique du Nord et un autre pour l’Europe. Vous pouvez ensuite créer un prospect dans un espace de travail particulier à l’aide de l’API [syncLead](/help/soap-api/synclead.md). Vous devez envisager d’utiliser des espaces de travail et des partitions de prospect si votre entreprise :

1. Des équipes marketing distinctes pour plusieurs lignes de produits
1. Équipes marketing distinctes pour différents territoires ou pays
1. Une société mère et des filiales
1. Société mère et revendeurs

Lorsque vous utilisez des partitions et des espaces de travail de prospect, vous pouvez :

1. Limiter l’accès aux prospects de votre organisation
1. Limiter l’accès aux ressources de votre organisation
1. Partage de ressources entre les équipes marketing

Nous vous montrons d’abord comment créer un espace de travail dans Marketo via l’interface utilisateur, puis comment écrire une piste dans cet espace de travail à l’aide de l’API [syncLead](/help/soap-api/synclead.md). **Création d’un Workspace** Un espace de travail est un ensemble de prospects et de ressources Marketo. Dans un espace de travail, vous ne pouvez afficher que les prospects et les ressources (e-mails, campagnes, listes, etc.) de cet espace de travail. Les campagnes intelligentes dans cet espace de travail n’affectent que les prospects de cet espace de travail. Pour afficher les espaces de travail dans votre compte :

1. Accédez à la page Espaces de travail et partitions de leads de la section Admin . Vos espaces de travail apparaissent dans l’onglet Espaces de travail . 1. Pour créer un espace de travail, cliquez sur le bouton Nouveau Workspace dans la barre de menus de l&#39;onglet Espaces de travail.
1. Dans la boîte de dialogue, vous devez ajouter des informations sur le nouvel espace de travail :

* **Nom de Workspace** - nom de cet espace de travail tel qu&#39;il apparaît dans l&#39;interface
* **Description** - description textuelle facultative de l’espace de travail
* **Partitions de leads** - les leads sont visibles dans cette partition.
* **Toutes les partitions de lead** - verra les leads de toutes les partitions actuelles et futures
* **Sélectionnez des partitions individuelles** - seuls les prospects de ces partitions (et aucune partition future) sont visibles
* **Partition de lead Principal** - Les leads qui visitent vos pages de destination sont ajoutés à cette partition par défaut
* **Langue** - Langue professionnelle de l’espace de travail

Nous vous montrons comment utiliser les prospects d’écriture d’API dans un espace de travail spécifique. Le premier exemple est une requête XML, le second est une réponse XML et le dernier est un exemple de code Ruby qui peut être utilisé pour générer la requête XML. 1. Après la création du prospect, la partition de prospect est un champ des informations sur le prospect. Demande de `requestCampaign` SOAP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Réponse de SOAP pour requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Vous trouverez ci-dessous un exemple de programme Java qui exécute le scénario décrit ci-dessus.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

**Comment puis-je m’inscrire aux espaces de travail et aux partitions de lead ?** Pour les clients Marketo Enterprise, vous disposez d’un accès gratuit aux espaces de travail et aux partitions de prospect. Contactez votre responsable de l’activation des clients pour plus d’informations sur leur activation et leur implémentation. Pour les autres clients, veuillez contacter votre responsable des ventes Marketo ou envoyer un e-mail à notre équipe des ventes pour plus d’informations sur ce produit.

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _1970-01-01_ par _Murta_

## Mises À Jour De Juin 2014

### API Marketo Real-Time Personalization

L’API JavaScript Marketo Real-Time Personalization (RTP) étend la fonctionnalité de personnalisation automatisée de la plateforme. Il permet le suivi des événements et la personnalisation dynamique d’une page web. Voir la documentation complète [ici](/help/javascript-api/web-personalization.md).

Publié le _2014-06-20_ par _Travis Kaufman_

## Stockage d’une clé étrangère dans Marketo

Lors de la synchronisation des enregistrements de contact et de prospect entre des systèmes tels qu&#39;un CRM ou un entrepôt de données propriétaire, il est courant d&#39;associer un enregistrement de prospect à un identifiant système unique. Dans Marketo, vous pouvez créer ou mettre à jour un enregistrement de prospect par le biais d’un appel [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) à l’aide de votre identifiant système unique. Pour ce faire, vous devez stocker votre identifiant système unique (clé primaire) en tant que clé étrangère dans Marketo. Le nom de ce champ dans Marketo pour stocker une clé étrangère est foreignSysPersonId. Voici trois points importants à noter :

1. foreignSysPersonId n’est pas visible dans l’interface utilisateur de Marketo. Il est donc recommandé de renseigner également un champ d’attribut personnalisé avec cette valeur.
1. foreignSysPersonId est propre à un prospect, mais un prospect peut avoir plusieurs foreignSysPersonId.
1. foreignSysPersonId ne peut pas être mis à jour ni supprimé, mais peut être réaffecté à un autre enregistrement.

Nous montrons comment effectuer un appel à l’API [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) pour écrire une valeur foreignSysPersonId dans deux enregistrements de leads existants dans Marketo. **Comment écrire un foreignSysPersonId à l’aide de l’API syncMultipleLeads** Vous pouvez insérer un nouvel enregistrement de lead et spécifier le foreignSysPersonId. Vous pouvez également l’ajouter à un prospect existant en spécifiant à la fois Marketo ID et foreignSysPersonId. Nous vous expliquons ce dernier cas. **Demande XML pour l’appel API SOAP syncMultipleLeads**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>b5e21953ae9f1b263e644da5eccce9ff33802513</requestSignature>
      <requestTimestamp>2013-08-01T18:22:30-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncMultipleLeads>
      <leadRecordList>
        <leadRecord>
          <leadId>1090240</leadId>
          <foreignSysPersonId>1224191</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1000</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1000</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
        <leadRecord>
          <leadId>1090239</leadId>
          <foreignSysPersonId>1224192</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1001</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1001</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
      </leadRecordList>
      <dedupEnabled>true</dedupEnabled>
    </ns1:paramsSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Réponse XML pour l’appel API SOAP syncMultipleLeads**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncMultipleLeads>
      <result>
        <syncStatusList>
          <syncStatus>
            <leadId>1090240</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
          <syncStatus>
            <leadId>1090239</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
        </syncStatusList>
      </result>
    </ns1:successSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Consultez ci-dessous un exemple de programme Ruby qui génère la requête XML ci-dessus.**

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
        :lead_record_list => {
                :lead_record => {
                        :lead_id => "1090240",
                        :foreign_sys_person_id => "1224191",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1000" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1000" }
                }
        },
        :lead_record! => {
                        :lead_id => "1090239",
                        :foreign_sys_person_id => "1224192",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1001" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1001" }
                }
        }
        },
    :dedup_enabled => "True"
}

response = client.call(:sync_multiple_leads, message: request)

puts response
```

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-06-27_ par _Murta_

## Mise à jour de l’adresse e-mail d’un lead

Supposons qu’un utilisateur remplisse un formulaire Marketo sur votre site. Que se passe-t-il ? Marketo crée un cookie pour l’utilisateur et l’associe à l’e-mail fourni. Que se passera-t-il si la prochaine fois que l’utilisateur consulte votre site web et qu’il remplit à nouveau le même formulaire avec un autre e-mail ? Que va-t-il se passer ? Marketo crée un nouvel enregistrement de prospect et remplace le premier cookie sur le navigateur de l’utilisateur. L’utilisateur est désormais un prospect nouveau/différent dans Marketo. Nous vous montrons quatre façons de mettre à jour l’adresse e-mail d’un prospect dans Marketo, notamment la [méthode d’API syncLead](/help/soap-api/synclead.md), le champ personnalisé dans une méthode de formulaire, l’interface utilisateur de Marketo et en important une liste. **Via l’API syncLead** vous pouvez utiliser l’API [syncLead](/help/soap-api/synclead.md) pour mettre à jour un enregistrement de prospect à l’aide de son identifiant Marketo et de sa nouvelle adresse e-mail. Demander le XML pour `syncMultipleLeads` appel API SOAP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>bigcorp1_461839624B16E06BA2D663</mktowsUserId>
      <requestSignature>92f05a7be4838ae1c0e5aafe814891ee72968a08</requestSignature>
      <requestTimestamp>2013-07-31T12:38:47-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncLead>
      <leadRecord>
        <leadId>1090240</leadId>
        <Email>t@t.com</Email>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Réponse XML pour l’appel API SOAP syncMultipleLeads

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1090240</leadId>
        <syncStatus>
          <leadId>1090240</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Vous trouverez ci-dessous un exemple de programme Ruby qui génère la requête XML ci-dessus.

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = {
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
 "requestTimestamp"  => requestTimestamp
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
 :lead_record => {
  :Email => "<t@t.com>",
  :lead_id => "1090240",
 :return_lead => "false"
}

response = client.call(:sync_lead, message: request)

puts response
```

**via un champ personnalisé dans un formulaire** vous pouvez créer un champ personnalisé pour la « Nouvelle adresse e-mail » dans Marketo. Demandez ensuite à l’utilisateur de remplir un formulaire contenant ce nouveau champ. Créez ensuite un programme dans Marketo qui modifierait la valeur des données du champ d’adresse e-mail système avec le jeton `{{lead.newEmailAddress}}` en cas de modification du nouveau champ personnalisé « Nouvelle adresse e-mail ». **Via l’interface utilisateur de Marketo** vous pouvez mettre à jour manuellement l’adresse e-mail d’un prospect via l’interface utilisateur de Marketo. Voici un [article d’aide](https://nation.marketo.com/) qui décrit comment procéder (connexion à Marketo requise pour voir l’article). **Via l’importation d’une liste** vous pouvez mettre à jour l’adresse e-mail d’un prospect à l’aide de la méthode d’importation d’une liste dans Marketo décrite [ici](https://nation.marketo.com/) (connexion Marketo requise pour consulter l’article).  

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _1970-01-01_ par _Murta_

## Stocker une deuxième adresse e-mail pour un lead

Supposons que vous souhaitiez modifier le score d’un prospect dans Marketo à l’aide des API. Cela est possible avec l’API REST à l’aide du point d’entrée Créer/Mettre à jour un prospect. Si vous souhaitez stocker plusieurs e-mails dans un enregistrement de prospect, vous devez créer un champ personnalisé et y stocker le second e-mail. Voici un article d’aide contenant plus d’informations : Voici un exemple de code dans Ruby qui montre comment effectuer cet appel.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Publié le _2015-02-20_ par _Murta_

## Création d’un champ personnalisé dans Marketo et mise à jour de ce champ via AP

Supposons que vous disposiez de données supplémentaires sur vos prospects qui ne s’intègrent pas aux champs Marketo standard. Par exemple, ce champ personnalisé peut être un score tiers. Vous pouvez créer un champ personnalisé dans Marketo pour votre score tiers, puis mettre à jour la valeur de ce champ via les API Marketo [REST](https://developer.adobe.com/marketo-apis/) ou [SOAP](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/soap/activity-type-filters). Nous vous montrons d’abord comment créer un champ personnalisé dans Marketo, puis comment mettre à jour ce champ à l’aide de l’API REST.

### Création d’un champ personnalisé dans Marketo

1. Sous Admin, cliquez sur Gestion des champs.
1. Cliquez sur le bouton Nouveau champ personnalisé .
1. Choisissez le type de champ. Cela modifie son rendu dans les listes dynamiques et dans Forms dans Marketo.
1. Saisissez le Nom tel que vous souhaitez qu’il apparaisse dans Marketo. Sélectionnez soigneusement le nom et le nom de l’API, car le changement de nom des champs peut être difficile et dans certains cas impossible.
1. Le champ personnalisé que vous avez créé est désormais accessible via les API.

### Comment mettre à jour le champ personnalisé via l’API REST

Dans la section précédente, nous avons créé un champ personnalisé appelé `myCustomField` avec la chaîne de type de données . Pour mettre à jour la valeur de ce champ, nous utilisons le point d’entrée de l’API REST appelé Créer/Mettre à jour des prospects. Avant de pouvoir envoyer une requête à l’API REST, vous devez vous authentifier. Bien que cela ne relève pas de cet article, des informations détaillées [sont disponibles sur le site des développeurs de Marketo](/help/rest-api/authentication.md).

**Point d’entrée**

`/rest/v1/leads.json`

**Corps de la requête**

```json
{
   "action":"createOrUpdate",
   "input":[
      {
         "email":"<example@example.com>",
         "myCustomField":"examplestring"
      }
   ]
}
```

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-08-19_ par _Murta_

## Intégration d’Unbounce et de Marketo

**NOTE : Ceci est un billet de blog invité par Fab Capodicasa. Il est consultant certifié Marketo chez [Hoosh Marketing](http://hooshmarketing.com.au/), une agence partenaire de Marketo LaunchPoint spécialisée dans le B2C. Il a travaillé dans le domaine du SaaS et du marketing au cours des 13 dernières années. Il a une formation mixte en informatique, en marketing direct et en ventes d&#39;entreprise. Fab est également un ancien employé de Marketo.**

**Présentation** dans cet article, nous montrons comment intégrer Unbounce, un outil de page de destination populaire, à Marketo. Nous vous montrons d’abord comment insérer le suivi Marketo dans Unbounce, puis comment modifier les formulaires Unbounce pour insérer des données directement dans Marketo. Le défi de l’intégration d’Unbounce à Marketo est que l’Unbounce ne permet pas de renommer les champs par défaut (par exemple, first_name ne peut pas être remplacé par FirstName). En outre, les libellés des champs ne peuvent pas être différents des noms de champ. Cette intégration implique JavaScript qui ajuste les formulaires existants pour les rendre compatibles avec Marketo. Je vous recommande de disposer d’au moins un niveau débutant en JavaScript et d’un niveau intermédiaire en Marketo pour accomplir les tâches de cet article.
**Partie 1 : ajouter le code de suivi Marketo à Unbounce** l’ajout du script de suivi Marketo Munchkin aux pages Unbounce est nécessaire pour que les analyses et l’intégration des formulaires fonctionnent. Procédez comme suit : Copiez votre code Munchkin à partir de Marketo : Accédez à Admin -> Munchkin et copiez la version « Simple » de JavaScript. Ouvrez la page de destination Rebond et cliquez sur JavaScript-> Ajouter un nouveau JavaScript.  Cliquez sur Ajouter, appelez le script « Munchkin », sélectionnez « Before Body End Tag », puis collez le code Munchkin. Cliquez sur le bouton Terminé . Pour les prochaines pages Annuler le rebond, accédez à JavaScript et activez le script Munchkin que nous avons créé. Il n’est pas nécessaire de la recréer.
**Partie 2 : Convertissez le formulaire Unbounce en formulaire Marketo** Nous devons maintenant modifier le formulaire Unbounce en ajoutant de nouveaux champs masqués et un nouveau JavaScript pour permettre à vos pages de destination Unbounce d&#39;envoyer des informations de prospect directement dans Marketo. Nous allons d’abord créer un formulaire d’espace réservé Marketo. Dans Marketo, créez un formulaire vierge et approuvez-le.

Il s’agit du formulaire de proxy dans Marketo qui représente le formulaire d’annulation de rebond. Ajouter des champs masqués au formulaire d’annulation du rebond. Ces champs masqués sont requis par Marketo pour déterminer à quelle instance Marketo, à quel formulaire et à quelle session utilisateur cet envoi de formulaire s’appliquera. Dans Unbounce, ouvrez votre formulaire en double-cliquant. Ajoutez un champ masqué appelé `_mkt_trk`. Ajoutez un second champ masqué appelé `formid`.  233 doit être remplacé par l’identifiant de votre formulaire, qui se trouve dans le code incorporé du formulaire Marketo dans Marketo. Dans Marketo, ouvrez votre formulaire, puis sélectionnez Actions du formulaire ->Code incorporé. Ajoutez un champ masqué appelé `returnurl`. `http://hooshmarketing.com.au/thank-you` doit être remplacé par l&#39;URL de relance, il s&#39;agit de l&#39;URL vers laquelle vous souhaitez que les utilisateurs soient redirigés après l&#39;envoi du formulaire. Par exemple, il peut s’agir de votre page de remerciement.
**Partie 3 : formulaire de rebond direct vers Marketo** l’URL de suivi est la page vers laquelle votre prospect sera redirigé une fois qu’il aura été envoyé à Marketo. Dans Unbounce, procédez comme suit : cliquez sur votre formulaire. Modifiez la section Confirmation du formulaire . Modifiez la Confirmation pour Publier les données de formulaire dans une URL. Définissez l’URL sur la page de relance de votre choix. `fpmarkets` doit être remplacé par la chaîne de votre compte Marketo, qui se trouve dans Marketo sous Admin->Pages de destination.
**Partie 4 : Ajouter JavaScript à la page Annuler le rebond** Ce JavaScript convertira le formulaire pour qu’il soit compatible avec Marketo et l’enverra à Marketo. Dans Unbounce, procédez comme suit : ouvrez la page de destination Unbounce et cliquez sur JavaScript-> Ajouter un nouveau JavaScript. Cliquez sur Ajouter, appelez le script « Marketo Form Convert », puis sélectionnez « Before Body End Tag ». Collez le code JavaScript ci-dessous :

```javascript
var MARKETO_MUNCHKIN_ID='614-CGT-700';
var MARKETO_ACCOUNT_STRING = 'fpmarkets';

var UNBOUNCE_MARKETO_FIELD_MAP = new Object();

//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
UNBOUNCE_MARKETO_FIELD_MAP['last_name'] = 'LastName';
UNBOUNCE_MARKETO_FIELD_MAP['phone_number'] = 'Phone';

function getMarketoField(k) {
    return UNBOUNCE_MARKETO_FIELD_MAP[k];
}


var formFields = [];
var hiddenClonedFields = [];
var firstForm = document.forms[0];

//Convert Unbounce form names to Marketo field names
for(i=0; i<firstForm.elements.length; i++){
 var formField = firstForm.elements[i];
 var newFieldName = getMarketoField(formField.name);

  if(newFieldName != undefined) {


    //save original field as hidden field
    var hiddenField = document.createElement("input");
    hiddenField.setAttribute("type", "hidden");
    hiddenField.setAttribute("name", formField.name);
    hiddenField.setAttribute("id", formField.id);
    hiddenClonedFields.push(hiddenField);

    //change original field
    console.log ( 'Changed form field name: ' + formField.name + '=>' + newFieldName );
    formField.name = newFieldName;
    formField.id = newFieldName;
    formFields.push(formField);


  } else {
    console.log ( 'Couldn't map:' + formField.name );
  }
}

//Add hidden cloned Unbounce fields to form
//for Unbounce validation
for(i=0; i<hiddenClonedFields.length; i++){
    firstForm.appendChild(hiddenClonedFields[i]);
    formFields[i].onchange = (function(hf) {
        return function(event) {
            hf.value = event.target.value;
          console.log('Changed hidden cloned field:' + hf.name + '=>' + hf.value);
        };
    }(hiddenClonedFields[i]));
    console.log ( 'Added cloned field: ' + hiddenClonedFields[i].name );
}


//Add MunchkinId to form
var input = document.createElement("input");
input.setAttribute("type", "hidden");
input.setAttribute("name", "munchkinId");
input.setAttribute("value", MARKETO_MUNCHKIN_ID);
firstForm.appendChild(input);
console.log('Added hidden field:' + input.name + '=' + input.value);
```

Si certains champs personnalisés comportent des noms d’API non compatibles avec l’annulation du rebond, vous pouvez les ajouter au mappage dans le JavaScript. Par exemple, si l’un de vos champs personnalisés dans Marketo était `Comments_c`, mais que vous souhaitiez que le libellé du champ apparaisse sous la forme de commentaires, l’option Annuler le rebond ne vous permettrait pas de modifier le nom du champ en `Comments_c`.

```
//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['comments'] = 'Comments_c';
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
```

_comments est le nom du champ dans Unbounce. _Comments_c_ est le nom du champ dans Marketo. Pour les prochaines pages Annuler le rebond, il vous suffira d’accéder à JavaScript et d’activer le script Munchkin que nous avons créé. Il n’est pas nécessaire de la recréer.
**Partie 5 : Tests** La dernière étape consiste à tester le bon fonctionnement de l’intégration de ce formulaire. Créez un déclencheur dans Marketo qui s’active lors du remplissage du formulaire Marketo et assurez-vous que les prospects sont correctement insérés dans Marketo. Une fois le formulaire envoyé, la page doit vous rediriger vers l’URL de relance.

Publié le _2014-08-04_ par _

## Mises À Jour De Juillet 2014

### Mises à jour de Munchkin

La nouvelle version de Munchkin est la version 141. La version 142 n’est pas prise en charge et sera supprimée début septembre 2011. Dans la version 142, certaines fonctions accessibles au public n’étaient pas documentées sur le site des développeurs. Ces fonctions non documentées ne sont plus accessibles au public. Les fonctions documentées sur le site des développeurs continueront à être prises en charge à long terme.

### Mises à jour RTP

L’API RTP dispose d’une nouvelle fonction appelée Obtention des données du visiteur. Cet appel API RTP vous permet d’obtenir des données visiteur en temps réel, telles que la correspondance de l’organisation, du secteur, de l’emplacement et du code segment.

Publié le _2014-07-30_ par _Murta_

## Utilisation de plusieurs codes de suivi Munchkin sur une seule page

Supposons que vous ayez plusieurs instances Marketo et que vous souhaitiez envoyer des événements de tracking web tels que des visites de page ou des liens cliqués vers ces multiples instances. Il est possible de le faire avec Munchkin. Marketo effectue le suivi des visiteurs sur votre site web par domaine (par exemple, « marketo.com »). Si vous hébergez ce script Munchkin sur un domaine différent de votre domaine principal (par exemple, « company.com »), ces visiteurs apparaîtront comme des prospects anonymes jusqu’à ce qu’ils remplissent un formulaire sur cet autre domaine. Pour ce faire, ajoutez le paramètre `altIds` à votre appel `Munchkin.init`. Le paramètre altIds contient un tableau des identifiants Munchkin supplémentaires où les événements web doivent être envoyés. À l’aide de l’exemple ci-dessous, remplacez les ID Munchkin mis en surbrillance (XXX-XXX-XXX, YYY-YYYY et ZZZ-ZZ-ZZZ) par les ID Munchkin de chaque instance Marketo où les informations de suivi doivent être envoyées.

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"></script>
<script>Munchkin.init('XXX-XXX-XXX', { altIds:['YYY-YYY-YYY', 'ZZZ-ZZZ-ZZZ'] });</script>
```

Pour plus d’informations sur les paramètres d’initialisation de Munchkin, consultez [ce document](/help/javascript-api/configuration.md).

Publié le _2014-08-08_ par _Murta_

## Intégration de Munchkin à Google Tag Manager

Google Tag Manager permet d’ajouter des balises à votre site web. Plutôt que d’ajouter manuellement chaque script de suivi, tel que Munchkin, au code source de votre site web, vous pouvez placer [Google Tag Manager](https://marketingplatform.google.com/about/tag-manager/) sur votre site, puis ajouter des balises telles que [Munchkin](/help/javascript-api/lead-tracking.md) via l’interface utilisateur de Google Tag Manager. Dans cette publication, nous allons d’abord montrer comment générer le code de suivi Munchkin dans Marketo, puis comment ajouter ce code de suivi Munchkin au gestionnaire de balises Google.

### Génération d’un code de suivi Munchkin

1. Cliquez sur **Admin** en haut à droite de l’application.
1. Cliquez sur **Munchkin** dans l&#39;arborescence de gauche.
1. Sélectionnez **Asynchrone** pour le type de code de suivi.
1. Cliquez sur le code de suivi JavaScript et copiez-le.

**Comment ajouter le code de suivi Munchkin au gestionnaire de balises Google**

1. Connectez-vous à votre compte Gestionnaire de balises Google et **Ajouter une nouvelle balise**.
1. Créez une **balise HTML personnalisée**.
1. Copiez et collez votre code Munchkin dans le champ **HTML** et cliquez sur **Continuer**.
1. Sélectionnez **Déclencher sur toutes les pages** puis cliquez sur **Créer une balise.** Remarque : si vous avez un site Web à trafic extrêmement élevé, vous pouvez exclure des sections de votre site à l&#39;aide de **Fire On Some Pages**.
1. Cliquez sur Enregistrer , puis vérifiez que le code de suivi Munchkin est maintenant en cours de chargement sur votre site web.

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-08-05_ par _Murta_

## Masquer Lightbox Après L’Envoi Du Formulaire

### Vue d’ensemble

Le code incorporé Lightbox actuel généré par Marketo ne disparaît pas lors de l’envoi du formulaire. La page se recharge après l’envoi du formulaire et le formulaire s’affiche à nouveau. Si vous souhaitez créer une Lightbox qui se masque après l’envoi du formulaire, suivez les étapes ci-dessous.

### Guide pratique

1. Après avoir créé le formulaire dans Marketo, générez le code incorporé. Pour ce faire, cliquez sur Actions de formulaire, puis sur Lightbox comme type de code. Copiez et collez ce code dans un éditeur de texte afin de le modifier à l’étape suivante.
1. Remplacez tout le code après « (form) » dans votre code incorporé par le code ci-dessous :

```javascript
{
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Après l’étape 2, la version terminée doit ressembler au code ci-dessous. Le code est maintenant prêt à être utilisé sur votre site web.

```javascript
<script src="http://app-sj04.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_0000"></form>
<script>MktoForms2.loadForm("http://app-sj04.marketo.com", "AAA-BBB-CCC", 0000, function (form){
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-08-21_ par _Murta_

## Guide de démarrage rapide pour l’API REST Marketo

Ce guide vous explique comment effectuer votre premier appel à l’API REST Marketo en dix minutes. Nous vous montrons comment récupérer un prospect unique à l’aide du point d’entrée de l’API REST [Get Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Pour ce faire, nous vous guidons tout au long du processus d’authentification afin de générer un jeton d’accès que vous utilisez pour envoyer une requête HTTP GET à [Get Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Nous vous fournissons ensuite le code permettant d’effectuer la requête qui renvoie des informations de prospect au format JSON.

### Génération d’un jeton d’authentification

>[!NOTE]
> Depuis juin 2025, le jeton d’authentification n’est plus pris en charge. Vous devez utiliser l’en-tête d’authentification .
>

Un service personnalisé dans Marketo vous permet de décrire et de définir les données auxquelles votre application aura accès. Vous devez être connecté en tant qu’administrateur Marketo pour créer un service personnalisé et associer ce service à un seul utilisateur API uniquement.

1. Accédez à la zone d’administration de l’application Marketo.
1. Cliquez sur le nœud Utilisateurs et rôles dans le panneau de gauche.
1. Créez un rôle. Affichez la liste des autorisations de rôle en cliquant sur Accéder à l’API . Maintenant, faites défiler l’écran vers le bas et sélectionnez uniquement les autorisations dont vous avez besoin. Dans ce cas, nous sélectionnerons simplement l’autorisation Lead en lecture seule.
1. L’étape suivante consiste à créer un utilisateur API uniquement et à l’associer au rôle d’API que vous avez créé à l’étape précédente. Pour ce faire, cochez la case Utilisateur API uniquement au moment de la création de l’utilisateur.
1. Un service personnalisé est nécessaire pour identifier de manière unique votre application cliente. Pour créer une application personnalisée, accédez à l’écran Admin > LaunchPoint et créez un service.
1. Indiquez le nom d’affichage, choisissez le type de service « personnalisé », fournissez la description et l’adresse e-mail de l’utilisateur créée à l’étape 1. Nous vous recommandons d’utiliser un Nom d’affichage descriptif qui représente l’entreprise ou l’objectif de ce Service d’API REST personnalisé.
1. Cliquez sur le lien « Afficher les détails » sur la grille pour obtenir l’ID client et le secret client. Votre application cliente peut utiliser l’ID client et le secret client pour générer un jeton d’accès.
1. Copiez et collez votre jeton d’authentification dans un éditeur de texte. Votre jeton d’authentification ressemble à l’exemple ci-dessous :

`cdf01657-110d-4155-99a7-f986b2ff13a0:int`

### Comment déterminer l’URL du point d’entrée

Lorsque vous effectuez une requête à l’API Marketo, vous devez spécifier votre instance Marketo dans l’URL du point d’entrée. Toutes les requêtes d’API non en bloc à l’API REST Marketo suivront le format ci-dessous :

`<REST API Endpoint URL>/rest/`

L’URL du point d’entrée de l’API REST se trouve dans le panneau Marketo Admin > Services web . La structure de votre URL de point d’entrée Marketo doit ressembler à l’exemple ci-dessous :

`https://100-AEK-913.mktorest.com/rest/v1/lead/{id}.json`

**Remarque : les URL d’API en bloc ne comportent pas le préfixe « /rest/ ». Pour les API en bloc, vous devez uniquement utiliser l’hôte et ajouter le chemin d’accès approprié comme suit :**

`https://100-AEK-913.mktorest.com/bulk/v1/leads/export/create.json`

### Utilisation du jeton d’authentification pour appeler Get Lead by Id AP

Dans les sections précédentes, nous avons généré un jeton d’authentification et trouvé l’URL du point d’entrée. Nous allons maintenant envoyer une requête à un point d’entrée de l’API REST appelé [Get Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Le moyen le plus simple d’envoyer votre requête à l’API REST Marketo consiste à coller l’URL dans la barre d’adresse de votre navigateur web. Suivez le format ci-dessous :

`https://<REST API Endpoint URL for your Marketo instance>/rest/v1/<API that you are calling>?access_token=<access_token>`

### Exemple

`https://100-AEK-913.mktorest.com/rest/v1/lead/318581.json?access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

Si votre appel réussit, il renvoie le format JSON ci-dessous :

```json
{
    "requestId": "d82a#14e26755a9c",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2015-06-11T23:15:23Z",
            "lastName": "Doe",
            "email": "<jdoe@marketo.com>",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "John"
        }
    ],
    "success": true
}
```

Si vous souhaitez en savoir plus sur les API REST Marketo, voici un [bon début](https://developer.adobe.com/marketo-apis/).

Publié le _2015-09-04_ par _David_

## Suppression du cookie de suivi Marketo

Question : « Nous avons créé sur notre site Web un formulaire permettant à l&#39;équipe des ventes de désabonner les personnes qui ont demandé verbalement à être supprimées de tout message électronique. Ce que nous constatons cependant, c&#39;est qu&#39;ils entrent dans le courriel et soumettent qu&#39;ils sont copiés avec cette adresse électronique et commencent à recevoir des alertes pour leur activité sur notre site Web. Le formulaire dont nous disposons est actuellement un formulaire incorporé. Est-ce que quelqu’un a trouvé un moyen de désactiver le suivi des cookies sur un formulaire incorporé ? Je comprends comment le faire avec une page de destination Marketo. » Nous pouvons le faire. Pour implémenter cette méthode, accélérez l’expiration du cookie. Une fois le cookie créé par le formulaire, vous pouvez le forcer à expirer immédiatement. Le cookie ayant expiré, le navigateur le supprime automatiquement. Pour démarrer ce processus, nous allons utiliser la fonctionnalité de formulaire natif pour appeler sous la fonction appelée `deleteCookie`.

Publié le _2014-08-26_ par _Travis Kaufman_

## Intégrer une landing page Marketo à Wordpress

Supposons que votre site web soit créé à l’aide de Wordpress et que vous souhaitiez incorporer une page de destination Marketo sur l’une des pages. Cela est possible à l’aide d’un iframe. Un iframe vous permet d’incorporer une page dans une autre page. Dans cette publication, vous allez découvrir comment incorporer une landing page Marketo dans une page Wordpress. **Choisir un plug-in Wordpress** Wordpress Il y a plusieurs plugins Voici un plug-in Wordpress qui vous permet : `http://wordpress.org/plugins/iframe/` Voici quelques avantages et inconvénients liés à l&#39;utilisation de cette approche : `http://www.elixiter.com/marketo-landing-page-and-form-hosting-options/` Grand post Murtza ! Colby, l’iframe conserve la partie où, après l’envoi du formulaire, vous êtes redirigé vers un PDF. Nous avons utilisé des iframes sur notre site pendant longtemps. Super post Murtza ! Colby, l’iframe conserve la partie où, après l’envoi du formulaire, vous êtes redirigé vers un PDF. Nous avons utilisé des iframes sur notre site pendant longtemps. Il est à noter que si vous souhaitez transmettre les paramètres d’URL de l’URL principale à l’iframe, vous devez effectuer un codage. Assurez-vous également que votre Google Analytics est correctement configuré. Vous ne souhaitez pas que les pages vues soient comptabilisées deux fois pour chaque visite de la page avec l’iframe.

Publié le _1970-01-01_ par _Murta_

## Intégration optimisée à une page de destination Marketo

[Optimisation](https://www.optimizely.com/) vous permet d’effectuer des tests A/B, des tests de plusieurs pages et des tests multivariés sur votre site. Vous pouvez utiliser Optimizely avec une page de destination Marketo. Procédez comme suit :

1. Recherchez et copiez votre fragment de code Optimizer.** Accédez à votre tableau de bord dans Optimizely, puis cliquez sur le lien Code du projet . Copiez la ligne de code qui s’affiche dans le pop-up.
1. Connectez-vous à Marketo et sélectionnez votre modèle de page de destination. Cliquez ensuite sur Modifier le brouillon.**
1. Cliquez sur Actions de la page de destination. Cliquez ensuite sur Modifier les balises de métadonnées de page**
1. Collez votre fragment de code Optimizely dans la section HTML d’HEAD personnalisée, puis cliquez sur Enregistrer.
1. Testez votre page de destination pour vérifier que le fragment de code Optimizely fonctionne

Publié le _2014-09-18_ par _Murta_

## Recherche par nom complet via l’API REST Marketo

**Question :** Existe-t-il un moyen d’interroger un prospect à l’aide des API Marketo avec uniquement le nom complet d’un prospect ?
**Réponse :** ce n’est pas directement possible. Toutefois, la solution décrite ci-dessous vous permettra de le faire.

1. Créez un champ personnalisé appelé « Fullname » dans Marketo.
1. Utilisez [getMultipleLeads](/help/soap-api/getmultipleleads.md) l’API SOAP ou [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) pour interroger la base de données de vos prospects. Incluez votre prénom et votre nom en tant qu’attributs dans votre requête aux API REST ou SOAP.
1. Après avoir interrogé la base de données de leads, concaténez « Prénom » et « Nom » pour chaque lead et stockez ces données dans une colonne « Nom complet ». 1. Utilisez l’API SOAP [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) pour transférer ces données vers un champ personnalisé « Fullname ». Vous pouvez également utiliser l’API [Importer le prospect](/help/rest-api/leads.md) ou importer un fichier CSV ou XLS à l’aide de l’interface utilisateur de Marketo.
1. Désormais, vous pouvez effectuer une requête par nom complet à l’aide de l’API [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) pour rechercher ce champ personnalisé. Spécifiez « Fullname » comme « filterType » et « filterValue » serait « Joe Johnson » avec un appel API REST Get Multiple Leads by Filter Type .

Publié le _2014-09-09_ par _Murta_

## Suppression du cookie de suivi Marketo

Cette méthode ne doit être utilisée que si l’effet souhaité est de supprimer entièrement le cookie.

Cet exemple de code peut être utilisé pour supprimer un cookie de suivi Marketo du navigateur d’un utilisateur après l’envoi du formulaire Marketo. Elle fonctionne en appelant `deleteMarketoCookie` méthode après l’envoi d’un formulaire par un utilisateur. Cette méthode fait expirer le cookie en définissant la date d’expiration sur une date dans le passé. Le comportement par défaut du navigateur consiste alors à supprimer ce cookie, car il a expiré.

Publié le _2014-09-09_ par _Murta_

## Restreindre les domaines d’e-mail gratuits lors du remplissage du formulaire

Supposons que vous souhaitiez empêcher les visiteurs du site d’envoyer un formulaire avec un domaine d’e-mail gratuit, tel que Gmail ou Yahoo. Pour ce faire, incluez le script ci-dessous dans le code source de la page avec le formulaire Marketo. Il vérifie si un utilisateur a saisi un e-mail non professionnel (Gmail, Hotmail, etc.) et empêche l’envoi du formulaire Marketo si un utilisateur en saisit un. Vous pouvez étendre la liste des domaines de messagerie bloqués en modifiant la ligne 9 afin d’inclure les domaines que vous souhaitez restreindre.

Publié le _2014-09-09_ par _Murta_

## Déboguer un champ qui n’est pas accessible via l’API

Si vous accédez à ce champ <https://nation.marketo.com/> Lorsque nous tentons de mettre à jour le champ AnnualRevenue à l’aide de l’API SOAP, la réponse indique que l’enregistrement est mis à jour, mais le champ AnnualRevenue ne conserve pas la modification. Si nous tentons de mettre à jour le champ manuellement à l’aide de la base de données des prospects, les modifications apportées à ce champ ne sont pas conservées non plus. Vérifiez si le champ est bloqué dans Admin. L’autre chose qui peut parfois se produire est qu’il peut s’agir d’un champ de compte synchronisé à partir de SFDC. Nous bloquons par défaut les modifications apportées à ces champs, car SFDC est le système d’enregistrement dans de nombreux cas. 1) Création À 2) Marketo SFDC ID 3) Marketo Code Unique 4) SFDC Type 5) Mise À Jour À 6) Urgence 7) Priorité 8) Date De Création Des Ventes

Publié le _1970-01-01_ par _Murta_

## Ajout de code personnalisé à une page de destination Marketo

Supposons que vous souhaitiez ajouter un script de suivi tiers, tel que Google Analytics, à votre page de destination Marketo. Cela est possible via l’interface utilisateur de Marketo. Plus généralement, vous pouvez ajouter n’importe quel code personnalisé (HTML, CSS, JavaScript) à votre page de destination Marketo en suivant les instructions ci-dessous.

1. Sélectionnez votre page de destination et cliquez sur Modifier le brouillon.
1. Faites glisser l’élément HTML.
1. Saisissez votre code personnalisé, puis cliquez sur Enregistrer.

Publié le _2014-09-10_ par _Murta_

## Publication de formulaire côté serveur

`https://community.marketo.com/MarketoResource?id=kA650000000GsXXCA0`

Publié le _2014-09-11_ par _Murta_

## Suppression du cookie de suivi Marketo des envois Forms 2.0

### Vue d’ensemble

Forms 1.0 contenait la valeur du cookie de suivi Munchkin en tant que champ dans le DOM. Ce document a été soumis avec toutes les autres contributions. [Forms 2.0](/help/javascript-api/forms-api-reference.md) omet ce champ et renseigne dynamiquement la valeur lors de l&#39;envoi plutôt qu&#39;au chargement du formulaire. Bien que cela soit généralement acceptable, cela crée une classe de cas d’utilisation, qui nécessite l’effacement du cookie de suivi pour éviter le suivi et le préremplissage involontaires. Par exemple, cela peut se produire lors d’un salon professionnel où un client Marketo utilise le même formulaire sur le même appareil et obtient les coordonnées de plusieurs personnes. Le fragment de code ci-dessous vous permet d’effacer la valeur du cookie lors de l’envoi du formulaire, sans avoir à supprimer le cookie lui-même du navigateur de l’utilisateur.

### Exemple de code

Ce fragment de code nécessite un seul chargement de formulaire sur la page. S’il existe plusieurs formulaires, vous devez utiliser les méthodes [loadForm ou getForm](/help/javascript-api/forms-api-reference.md) pour implémenter vos rappels.

```javascript
<script>
//add a callback to the first ready form on the page
MktoForms2.whenReady( function(form){
 //add the tracking field to be submitted
        form.addHiddenFields({"_mkt_trk":""});
        //clear the value during the onSubmit event to prevent tracking association
 form.onSubmit( function(form){
  form.vals({"_mkt_trk":""});
 })
})
</script>
```

Publié le _2014-09-11_ par _Kenny_

## Mises À Jour De Septembre 2014

### Mises à jour de l’API REST

Ajout d’une nouvelle valeur de champs facultatifs à l’API [Get Multiple Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) qui renverra les valeurs de cookie Munchkin associées à un enregistrement de lead. Ajoutez simplement «?fields=cookies » à la requête.

Publié le _2014-09-16_ par _Murta_

## Ajout des données de localisation de l’API RTP au remplissage de formulaire Marketo

**Vous avez besoin d’abonnements au RTP et au MLM actifs pour implémenter le cas d’utilisation décrit dans cet article de blog.**
À l’aide des API [RTP JavaScript](/help/javascript-api/web-personalization.md) et de [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md), vous pouvez extraire les données d’emplacement déduites de RTP et les transmettre à Marketo via un remplissage de formulaire. Vous pouvez ainsi afficher l’emplacement déduit de l’utilisateur (en fonction de l’adresse IP) au cours de l’activité de formulaire la plus récente. Pour commencer, vous devez créer trois champs de chaîne personnalisés dans Marketo. Vous pouvez le faire via votre CRM s’il dispose d’une intégration native à Marketo, ou à partir du menu Gestion des champs de la section d’administration de Marketo. Je vous recommande de nommer ces champs « Pays le plus récent », « État le plus récent » et « Ville la plus récente ». Nous continuons ce blog en utilisant cette convention de nommage. Les noms d’API pour ces champs sont « mostRecentCountry », « mostRecentState » et « mostRecentCity ». Pour récupérer les données d’emplacement, nous utilisons la méthode [RTP pour obtenir les données d’emplacement du visiteur](/help/javascript-api/web-personalization.md), puis nous les transmettons dans le formulaire à l’aide des méthodes [addHiddenFields et vals](/help/javascript-api/forms-api-reference.md) de Marketo Forms 2.0. Sur votre page, ajoutez la balise RTP JS et un formulaire Marketo. Insérez ensuite le script ci-dessous. Vous devez modifier les noms des champs du formulaire cible dans l’exemple de code si vous utilisez une convention de nommage différente de celle décrite ci-dessus.

```javascript
<script>
//modify the form and grab the user
MktoForms2.whenReady( function(form) {
        //add the hidden fields to the form
 form.addHiddenFields({
  "mostRecentCountry":"",
  "mostRecentState":"",
  "mostRecentCity":""});
 //Grab the visitor data, a JS object with it is passed in the callback function of the third argument
 rtp('get','visitor',function(visitor){
  //add the desired info from the visitor object to the form fields
  form.vals({
   "mostRecentCountry":visitor.results.location.country,
   "mostRecentCity":visitor.results.location.city,
   "mostRecentState":visitor.results.location.state});
  }
  );
 });
</script>
```

Publié le _2014-09-17_ par _Kenny_

## Bloquer l’analyse et l’indexation des recherches d’une page de destination Marketo

Supposons que vous souhaitiez empêcher l’analyse et l’affichage d’une page de destination Marketo dans les résultats des moteurs de recherche tels que Google. Procédez comme suit :

1. Connectez-vous à Marketo et sélectionnez votre page de destination. Cliquez ensuite sur Modifier le brouillon.
1. Cliquez sur Actions de la page de destination. Cliquez ensuite sur Modifier les balises de métadonnées de page
1. Remplacez le champ Robots par Aucun index, Aucun suivi. Cliquez ensuite sur Enregistrer.

Publié le _2014-09-19_ par _Murta_

## FAQ sur les API REST Marketo par rapport aux API SOAP

**Mise à jour : mars 2016** Voici les réponses aux questions les plus fréquentes sur les API Marketo [REST](/help/rest-api/rest-api.md) et [SOAP](/help/soap-api/soap-api.md). **Q : Quelles sont les principales différences entre les API REST Marketo et SOAP ?** A : bien que la possibilité d’envoyer/extraire des données spécifiques via les API REST et SOAP soit généralement chevauchante, certaines fonctionnalités n’existent que dans les API REST ou SOAP. En termes de performances, l’API REST offre un [débit](https://en.wikipedia.org/wiki/Throughput) supérieur à celui de l’API SOAP. En termes de modèle d’authentification, l’API REST dispose d’un modèle d’authentification qui utilise un jeton arrivant à expiration. Notre API REST permet également d’accéder à Marketo [ressources](https://developer.adobe.com/marketo-apis/api/asset/).   **Q : Quelles fonctionnalités sont disponibles dans l’API REST qui ne sont pas disponibles dans l’API SOAP ?** A : [API List of lists](/help/rest-api/list-of-standard-fields.md), [supprimer un prospect d’une API list](/help/rest-api/lead-database.md), [API Usage](/help/rest-api/rest-api.md) et [API Error](/help/rest-api/rest-api.md) ne sont disponibles qu’avec l’API REST. **Q : Prévoit-on d’augmenter le nombre d’API disponibles pour l’API SOAP ?** A : Non. **Q : Prévoit-on d’augmenter le nombre d’API disponibles pour l’API REST ?** A : Oui. Actuellement, REST est le principal objectif du développement de l’API Marketo.

Publié le _2014-09-20_ par _Murta_

## Publication de formulaire côté serveur

**Remarque : il s’agit d’une API non publique et non prise en charge, elle n’est pas prise en charge et son comportement peut changer à tout moment** Si vous utilisez vos propres formulaires sur votre site web, vous pouvez toujours envoyer ces données à Marketo à l’aide d’une publication côté serveur. L’avantage de cette approche est que vous pouvez conserver votre formulaire existant et la logique de l’application, mais vous pouvez toujours utiliser une publication de formulaire réelle dans Marketo. Cela donne aux utilisateurs de Marketo un événement « Remplir le formulaire », qui peut être utilisé pour déclencher des processus automatisés ou pour la segmentation. **REMARQUE : le taux est limité à 30 publications de formulaires côté serveur par minute à partir d’une seule adresse IP.** Dans chaque langage de script ou de programmation, il existe différentes options pour envoyer un formulaire côté serveur, et ils peuvent avoir différents objets/méthodes qui peuvent être utilisés pour effectuer l’appel Post. Par exemple, en PHP beaucoup de gens utilisent la bibliothèque cURL. Dans tous les cas, vous publiez des paires nom-valeur sur une URL spécifiée. Les noms doivent être identiques aux noms d’API des champs Marketo. En outre, plusieurs champs système doivent être inclus pour capturer correctement l’envoi du formulaire.

1. Créez un formulaire. La première étape consiste à créer un formulaire dans Marketo ou à utiliser un formulaire existant que vous souhaitez envoyer. Le nom du formulaire doit être explicite, mais il n’a pas réellement besoin de champs de formulaire. Si vous créez un formulaire, entrez simplement un nom, décochez la case « Ouvrir l&#39;éditeur de formulaire » et vous avez terminé.
1. Recherchez un ID de formulaire. Dans l’interface utilisateur de Marketo, sélectionnez le formulaire et examinez l’URL : elle doit être au format `https://app-x.marketo.com/#FO8B2ZN12`. Derrière le signe #, regardez le numéro juste après « FO » pour trouver l&#39;ID du formulaire. Dans ce cas, l’ID du formulaire est 8. Dans certains cas, votre premier formulaire peut être numéroté 1001 et compter à partir de là. L’ID du formulaire est une variable, de sorte que vous pouvez déclencher l’envoi de différents formulaires.
1. Obtenez votre identifiant de compte Marketo. Accédez à Admin > Munchkin et copiez l’identifiant de compte Munchkin au format 000-AAA-000. Vous en avez besoin pour que le formulaire soit envoyé dans l’instance Marketo appropriée.
1. Déterminez l’URL POST. Dans l’interface utilisateur de Marketo, notez le domaine dans la barre d’emplacement, généralement au format `<http://app-x.marketo.com/>`. Ignorez tout ce qui se trouve après la barre oblique, puis ajoutez « index.php/leadCapture/save » pour obtenir l’URL POST de formulaire complet. Remarque 1 : respecte la casse. Remarque 2 : les sandbox Marketo peuvent avoir un domaine différent de celui de votre système Marketo de production. Voici donc un exemple d’URL : `http://app-x.marketo.com/index.php/leadCapture/save` Vous pouvez également utiliser HTTPS au lieu de HTTP (n’utilisez pas votre CNAME, car il présente une exception de sécurité).
1. Recherchez les noms des champs de formulaire** Accédez à Admin > Gestion des champs et cliquez sur le bouton « Exporter les noms des champs » pour télécharger une feuille de calcul avec les noms des champs de l’API. Utilisez le nom de l’API comme nom dans vos paires nom-valeur.
1. Choix des champs à publier. Vous pouvez inclure n’importe quel champ Lead Marketo dans votre envoi de formulaire. Notez que les noms de champ sont sensibles à la casse. Outre les champs que vous souhaitez envoyer, il existe deux champs obligatoires et deux champs recommandés : Champs obligatoires sur le formulaire : (1) `munchkinId` - Ce champ est utilisé pour votre identifiant de compte Munchkin (2) `formid` - Ce champ indique quel formulaire dans Marketo a été envoyé Champs recommandés sur le formulaire : (1) E-mail - ce champ est utilisé comme clé primaire pour la déduplication. Si Marketo trouve une adresse e-mail correspondante dans la base de données Marketo, il met à jour l’enregistrement existant, sinon il crée un enregistrement. S’il existe plusieurs correspondances, il met à jour l’enregistrement le plus récemment mis à jour (2) `_mkt_trk` - ce champ contient les informations sur le cookie, de sorte que vous puissiez suivre les visites de la page web de l’individu. Si la page de votre formulaire contient Munchkin, Munchkin saisit automatiquement une valeur dans ce champ de formulaire masqué. Dans le cas contraire, lisez-le à partir du cookie portant le même nom et transmettez-le à Marketo dans ce champ. Remarque : le corps de la requête POST à un formulaire Marketo doit être encodé en URL.
1. Voir la réponse ** La réponse à la publication du formulaire sera un code de redirection HTTP 302. Dans certains systèmes, cela s’affiche sous la forme d’une erreur. Cependant, dans ce cas, cela signifie que le prospect a bien été créé ou mis à jour. En cas d’erreur, vous recevez un code d’erreur 4xx ou 5xx.

Voici un [article](https://nation.marketo.com:443/t5/product-blogs/how-to-build-an-external-subscription-center/ba-p/242185) sur l’utilisation de cette technique pour les scénarios de désabonnement [par Justin Cooperman, chef principal de produit]

Publié le _2014-11-07_ par _Murta_

## Rechercher des leads mis à jour à une période spécifique

Supposons que vous souhaitiez rechercher des prospects mis à jour à des dates spécifiques via l’API [Marketo](/help/soap-api/soap-api.md). Cela est possible avec l’API SOAP [getMultipleLeads](/help/soap-api/getmultipleleads.md). Cette méthode renvoie tous les prospects ayant une modification de valeur de données ou une nouvelle activité dans Marketo pour la période que vous demandez. Pour le `leadSelector`, vous devez spécifier `LastUpdateAtSelector`. Ensuite, vous définissez les périodes avec des limites de temps `oldestUpdatedAt` et `latestUpdatedAt`. Veuillez consulter l&#39;exemple de demande XML ci-dessous, qui vous montre comment trouver les leads qui ont été mis à jour entre 00 h 00 PST le 6 juin 2014 et 00 h 00 PST le 7 juin 2011. Remarque : la période ne doit pas dépasser 30 jours.

**Exemple de requête XML pour rechercher des leads mis à jour par date**

```xml
<soapenv:Envelope xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
<soapenv:Header>
<mkt:AuthenticationHeader>
<mktowsUserId>REPLACE</mktowsUserId>
<requestSignature>REPLACE</requestSignature>
<requestTimestamp>2014-10-23T12:19:51-07:00</requestTimestamp>
</mkt:AuthenticationHeader>
</soapenv:Header>
<soapenv:Body>
<mkt:paramsGetMultipleLeads>
<leadSelector xsi:type="mkt:LastUpdateAtSelector">
<oldestUpdatedAt>2014-06-06T00:00:00-08:00</oldestUpdatedAt>
<latestUpdatedAt>2014-06-07T00:00:00-08:00</latestUpdatedAt>
</leadSelector>
</mkt:paramsGetMultipleLeads>
</soapenv:Body>
</soapenv:Envelope>
```

Publié le _2014-09-24_ par _Murta_

## Ajout de contenu dynamique à un e-mail

Supposons que vous envoyez un e-mail quotidien et que vous souhaitez inclure automatiquement la date de ce jour dans le modèle d’e-mail. Pour ce faire, utilisez des jetons et des scripts d’e-mail dans Marketo.

1. Créez un jeton.** Accédez au programme dans lequel vous souhaitez utiliser le jeton. Cliquez sur Mes jetons.
1. Double-cliquez sur Script d’e-mail. Attribuez ensuite un nom à ce jeton. Cliquez ensuite sur Modifier.
1. Collez le script d’e-mail ci-dessous dans cette fenêtre. Cliquez ensuite sur Enregistrer.

## Accès à l’objet de calendrier de Velocity

`set($x = $date.calendar)`

## Mettre en forme la date

`set($current_date = $date.format('dd-MM-yyyy', $x.getTime()))`

## Renvoie la date du jour

`$current_date`

1. Référencez le jeton dans le modèle d’e-mail.** Notez le nom du jeton. Accédez au brouillon de votre e-mail. Incluez le jeton .  Lorsque l’e-mail est envoyé, la valeur du jeton est renseignée. Pour plus d’informations, consultez la [documentation destinée aux développeurs et développeuses de scripts d’e-mail](https://experienceleague.adobe.com/fr/docs/marketo-developer/marketo/email-scripting).

Publié le _2014-11-22_ par _Murta_

## Avis de sécurité Bash

Marketo a effectué une enquête approfondie sur la vulnérabilité Bash, également connue sous le nom de [Shellock (CVE-2014-6271)](https://nvd.nist.gov/view/vuln/detail?vulnId=CVE-2014-6271), et a conclu que nous ne sommes pas sensibles à ces attaques. En outre, nous avons pris des mesures préventives en mettant à jour le logiciel vers la dernière version afin de garantir la conformité avec la recommandation [CERT](https://www.cisa.gov/news-events/alerts/2014/09/25/gnu-bourne-again-shell-bash-shellshock-vulnerability-cve-2014-6271).

Publié le _2014-09-26_ par _Murta_

## Comment mettre à jour les informations d’identification de l’API SOAP

Il est recommandé de mettre à jour régulièrement vos informations d’identification d’API [SOAP](/help/soap-api/soap-api.md). Actuellement, il n’existe aucun moyen d’effectuer cette opération par programmation via l’API Marketo. Les instructions ci-dessous vous montrent comment mettre à jour vos informations d’identification d’API SOAP via l’interface utilisateur de Marketo.

1. Accédez à la section Admin et cliquez sur Services web.
1. Définissez une clé de chiffrement d’au moins 10 caractères, puis cliquez sur Enregistrer les modifications.

Publié le _2014-09-29_ par _Murta_

## Nettoyage de la base de données Marketo

**NOTE : Ceci est un billet de blog invité. Josh Hill est responsable des pratiques Marketo chez Perkuto, une agence d’automatisation du marketing. Josh travaille à l&#39;intersection du marketing, des ventes et de la technologie pour fournir des systèmes de génération de revenus. Il écrit sur l&#39;automatisation du marketing et la génération de la demande sur [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/).** Pour administrer ce système puissant, il est important de garder votre base de données Marketo propre. Si vos données Marketo ou vos données CRM sont sales, votre travail en tant que responsable marketing devient beaucoup plus difficile car vous expliquez pourquoi les campagnes n’ont pas été dirigées vers les bonnes personnes ou pourquoi vos rapports sont « directionnels ». [Une étude de Gartner de 2011 a noté](https://www.data.com/export/sites/data/common/assets/pdf/DS_Gartner.pdf) que la mauvaise qualité des données a réduit la productivité du travail de 20 %. Cela représente 20 % de votre temps (8 heures/semaine ou 1 jour par semaine) perdu, car vous avez dû corriger les données. Mais cette perte est dérisoire par rapport à la perte de revenus due au mauvais ciblage. Voici les cinq principales raisons d’investir dans la propreté de vos données : - Une meilleure segmentation des prospects et des clients peut être effectuée, ce qui vous permet de concentrer votre message sur les bonnes personnes au bon moment. Ce coupon de 20 % ne devrait servir qu&#39;aux nouveaux clients, pas à vos meilleurs clients, n&#39;est-ce pas? - Évitez l’envoi en double d’e-mails. Malgré toutes les précautions, il est possible d’envoyer plusieurs e-mails au même prospect, généralement parce que les enregistrements en double ne sont pas fusionnés. - Des rapports précis à votre patron. Étant donné que le directeur marketing a une place à la table des recettes, vous devez disposer de rapports précis, cohérents et répétables... ou vous ne pouvez plus être un spécialiste du marketing des recettes. - Nuire aux offres en cours si vous envoyez par e-mail le mauvais matériel marketing aux prospects clés pendant les négociations de vente.

Si votre base de données ne fournit pas ces détails à l’étape du cycle de vie, cela peut entraîner un échec ou deux. - Supprimez les leads erronés ou anciens pour rester en dessous des seuils de prix. Beaucoup de choses ont été écrites sur le coût des mauvaises données. Votre préoccupation la plus immédiate est qu’un trop grand nombre de prospects incorrects, en double ou anciens vous fait passer au-dessus de votre niveau de tarification Marketo ou Salesforce, ce qui peut entraîner des milliers de frais par an. Alors, comment garder votre base de données propre ? Vous pouvez suivre ces principes de propreté des données, mais comment y parvenir dans Marketo ? Je fais quelques suppositions à propos de votre système : - Vous disposez d’un système Marketo standard. - Vous disposez d’une configuration Salesforce de compte de contact de lead standard. - Vous synchronisez tous les enregistrements entre les systèmes. - Vous n’utilisez pas de doublons ciblés.

Trouvez les bonnes pistes à corriger ** Commençons par trouver les pistes qui sont des problèmes potentiels. Pour chaque client, je passe en revue une série de listes intelligentes pour déterminer l&#39;intégrité de sa base de données. Je propose que vous fassiez de même tous les mois. Une fois que les listes intelligentes fonctionnent, cela ne vous prend pas plus de 15 minutes par mois. Il s’agit d’une excellente manière de démontrer le retour sur investissement de votre travail et de Marketo. Créez un tableau pour comprendre l’impact total sur votre base de données. L’exemple ci-dessous inclut les critères que je recherche. Veillez donc à inclure d’autres champs importants pour votre entreprise. Répartissez chaque groupe par Marketo uniquement, leads SFDC et contacts SFDC.

Utiliser l’automatisation pour corriger les valeurs de données : l’utilisation des règles d’automatisation pour corriger les fautes d’orthographe courantes ou les données manquantes remonte à plusieurs décennies. Dans les années 1960, la mauvaise qualité des données envoyées par publipostage direct pouvait entraîner le gaspillage de milliers de dollars. Aujourd’hui, les erreurs de qualité des données ruinent plus rapidement leur réputation qu’un courrier mal acheminé. La réputation des e-mails, le choix de la langue et l&#39;expérience client sont importants, mais ils le sont davantage car les erreurs peuvent devenir virales en quelques minutes. Sauvegardez la réputation de votre entreprise grâce au nettoyage automatisé des données. Ces flux de gestion des données sont l’une des premières choses que je configure dans Marketo. La plupart des entreprises mettent en place des flux similaires : - Correcteur de pays (bien que vous auriez dû suivre le Principe 1 pour ne pas en avoir besoin) - Correcteur d’état ou mappeur - souvent utile si vous avez Pays et Etat déduit. - Nombre d&#39;employés par plage d&#39;employés. - Source de lead incorrecte vers bon Source de lead - L’e-mail non valide dans l’e-mail est correct si l’e-mail a changé. - Traduisez les anciennes valeurs de champ en un nouveau champ (Complet à vide). Par exemple, ce flux ajuste la plage d&#39;employés en fonction du numéro d&#39;employé.

Outils d’ajout de données Il est essentiel de remplir des champs vides pour mieux segmenter votre base de données. Les prospects ne remplissent pas toujours le secteur ou la fonction approprié(e), ni même le titre sur un formulaire. Parfois, vous disposez de données héritées d’un système plus ancien. Ce manque de données signifie que vous devez envoyer cet e-mail à moins de personnes, ou éventuellement aux mauvaises personnes. Pour résoudre ce problème, vous avez besoin d’outils d’ajout de données.

Option 1 : remplissez-la vous-même. Vous pouvez peut-être interpoler les données pour renvoyer des champs vides. Peut-être avez-vous un code SIC au lieu du nom du secteur ou du chiffre d’affaires annuel par rapport à la plage de chiffres d’affaires annuels. Marketo peut facilement automatiser ces correctifs.

Option 2 : Rechercher un fournisseur d’ajout/d’enrichissement de données via LaunchPoint Il existe [plusieurs fournisseurs dans LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO), tels que NetProspex et ReachForce, qui peuvent vous aider à enrichir vos données de prospect. Certains vous demandent une feuille de vos données, qu&#39;ils nettoient et renvoient ensuite. La meilleure option est un outil automatisé dans Marketo ou Salesforce, qui vérifie les champs de votre choix, puis renvoie les données appropriées. Pour ce faire, la plupart des fournisseurs utilisent l’API ou les Webhooks [de Marketo](/help/home.md).

Option 3 : utiliser les API Marketo pour mettre à jour les prospects Vous pouvez utiliser les API Marketo pour identifier les prospects à nettoyer, puis les mettre à jour via l’API. L’API REST [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) est un bon point de départ pour extraire des données de Marketo qui correspondent à certains critères. Pour mettre à jour les prospects, consultez l’API REST [Créer/mettre à jour des prospects](/help/rest-api/leads.md).

Vous pouvez également configurer un [Webhook Marketo](/help/webhooks/webhooks.md) pour informer un système externe qu’un certain événement s’est produit, comme le remplissage d’un formulaire. Vous pouvez ensuite répondre avec des valeurs pour mettre à jour le prospect.

Que Devriez-Vous Automatiser ? J&#39;ai fait quelques suggestions à l&#39;étape 1. Mais si nous voulons nettoyer plus de la base de données, nous devons aller au-delà de la fixation des valeurs de données. - Listes noires des concurrents : assurez-vous d&#39;automatiser la collecte et la liste noire de vos concurrents. Si vous avez des concurrents-partenaires, il est toujours bon de les marquer correctement et de les placer dans une liste pour éventuellement les supprimer.  - Plusieurs Hard Bounces : automatisez définitivement ce flux. Si un lead fait l’objet de hard bounces plus de deux fois au cours d’une période de 30 jours, définissez-le sur Suspendu ou Non valide. Ensuite, allez une fois par mois pour vérifier si le problème était une faute de frappe ou autre chose.  - Plusieurs soft bounces en 30 jours : définissez-les sur marketing suspendu=TRUE pendant 30 jours.  - Suspension/suppression du piège à spam : faites attention si votre produit signifie que des personnes utilisent d&#39;éventuelles adresses de piège à spam. Voir la liste des pièges à spam. Liste dynamique.

**Que ne pas automatiser** n’automatisez jamais la suppression des prospects, car la suppression en masse accidentelle des enregistrements erronés présente un risque trop important. Plutôt, passez en revue une fois par mois les pistes marquées comme Corbeille. Mais si vous souhaitez supprimer les leads corrompus, exécutez un flux comme celui-ci avec une attente de 30 jours.

Avertissements : l’utilisation de l’automatisation est un excellent moyen de gagner du temps et de garder votre base de données propre. L&#39;automatisation est une épée à double tranchant, car si vous la mettez mal en place, elle peut causer des ravages en quelques minutes. Soyez prudent lorsque vous suivez ces processus et tenez tout le monde informé. En règle générale, je déconseille de supprimer les contacts SFDC, car ils sont plus susceptibles d’être des opportunités actives, des clients ou d’anciens clients. Le service des finances ou le service juridique peut vous demander de conserver certains documents et des contacts sont joints à ces documents. Concentrez-vous plutôt sur les contacts qui font l’objet d’un Hard Bounce, qui deviennent non valides ou qui n’y sont plus. Vous connaissez maintenant quelques-unes des techniques permettant de garder votre base de données Marketo propre. Dites-nous si vous avez trouvé d’autres moyens d’automatiser ces processus à l’aide de l’API ou des Webhooks.

Publié le _2014-10-08_ par _Josh_

## Mises À Jour De La Version D’Octobre 2014

### Préremplissage de page externe

Les formulaires Marketo ne fournissent pas de fonctionnalité de préremplissage native lorsqu’ils sont chargés en dehors d’une page de destination Marketo. Cependant, nous pouvons toujours implémenter cette méthode à l’aide des [API Marketo](/help/rest-api/rest-api.md) et de l’API JavaScript 2.0 de [Forms](/help/javascript-api/forms-api-reference.md/). La première étape consiste à récupérer les données de prospect de Marketo par l’intermédiaire d’un appel REST à partir du serveur. En supposant que nous ne disposions pas d’un moyen immédiat de faire référence aux identifiants de prospect ou à un autre identifiant unique du serveur, nous devons utiliser le cookie Munchkin « _mkto_trk » pour récupérer les données du serveur Marketo à l’aide de la méthode [Get Leads By Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).
Pour effectuer cet appel, nous avons besoin de vos points d’entrée d’authentification et REST à partir de votre instance . Une fois que vous vous êtes authentifié auprès de votre instance Marketo, nous devons effectuer un appel à l’API des prospects à l’adresse `https://<host>/rest/v1/leads.json`. Nous devons ensuite créer une chaîne de requête pour filtrer sur le cookie Marketo comme `?filterType=cookie&filterValues=`. Vous devez récupérer la valeur spécifique de la clé &#39;_mkto_trk&#39; envoyée à votre serveur par le client. REMARQUE : la valeur du cookie _mkto_trk comprend une esperluette et doit être codée en URL pour `%26` être correctement acceptée par le point d’entrée Marketo. Par défaut, l’API de leads renvoie quatre champs : `id`, `email`, `firstName` et `updatedAt`. Pour définir un ensemble spécifique de champs, vous devez inclure un paramètre de requête `fields`, avec des noms de champs séparés par des virgules comme celle-ci : `&fields=email,firstName,lastName,company`. En fin de compte, notre appel ressemblera à ceci :

`https://<host>/rest/v1/leads.json?filterType=cookie&filterValues=<cookie>&fields=email,firstName,lastName,company&access_token=<token>`

Lorsque nous effectuons cet appel, il renvoie un objet JSON qui ressemble à ceci :

```json
{
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
 "lastName":"Elkington",
        "email":"<mkto@example.com>",
 "company":"Marketo Inc."
        }]
};
```

Maintenant que nous disposons des détails de nos leads, nous pouvons les mapper dans un formulaire Marketo, à l’aide des méthodes whenReady et vals dans JavaScript. Tout d’abord, nous devons définir les résultats du prospect sous la forme d’une variable sur notre page :

```javascript
<script>
//print your JSON object dynamically as the mktoLead variable
var mktoLead = {
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[
        {
        "id":50,
        "firstName":"Kenny",
  "lastName":"Elkington",
        "email":"mkto@example.com",
  "company":"Marketo Inc."
        }]
}
</script>
```

Maintenant que nous avons nos résultats sur la page, nous pouvons les mapper dans nos champs de formulaire :

```javascript
<script>
MktoForms2.whenReady( function(form) {
 //set the first result as local variable
 var mktoLeadFields = mktoLead.result[0];
    //map your results from REST call to the corresponding field name on the form
 var prefillFields = {
   "Email" : mktoLeadFields.email,
   "FirstName" : mktoLeadFields.firstName,
   "LastName" : mktoLeadFields.lastName,
   "Company" : mktoLeadFields.company
   };
 //pass our prefillFields objects into the form.vals method to fill our fields
 form.vals(prefillFields);
 }
 );
</script>
```

Publié le _2014-10-24_ par _Kenny_

## Remplace l&#39;e-mail HTML

Cette publication vous explique comment supprimer l’HTML générée par Marketo pour un e-mail. Vous pouvez ensuite utiliser votre propre HTML sans la reformater avec Marketo.

1. Accédez à l’e-mail, puis cliquez sur Modifier le brouillon.
1. Cliquez sur Actions liées à l’e-mail, sur Outils HTML, puis sur Remplacer HTML.
1. Remplacez le code de cette zone par votre HTML. Cliquez ensuite sur Enregistrer.

Publié le _2014-10-28_ par _Murta_

## Obtention de l’identifiant de cookie d’un visiteur, puis requête sur les données de lead associées

À l’aide du point d’entrée REST [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), vous pouvez interroger les données de prospect en fonction de l’identifiant de cookie d’un utilisateur. Par exemple, vous pouvez utiliser cette approche pour préremplir un formulaire sur une page de destination autre que Marketo. Cette publication vous explique comment capturer la valeur du cookie de l’utilisateur lors d’une visite de la page web, effectuer une requête [Get Multiple Leads REST API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) avec cet identifiant de cookie, puis renvoyer les données de lead de l’utilisateur. Tout d’abord, nous avons besoin de la valeur du cookie Munchkin de l’utilisateur, « _mkto_trk ». Voici un exemple de fonction JavaScript que vous pouvez utiliser pour obtenir la valeur du cookie. Consultez [cette page StackOverflow](https://stackoverflow.com/questions/10730362/get-cookie-by-name) pour plus d’informations sur cette approche. Je vous recommande de définir un délai de 500 ms après l’événement de chargement de page avant d’appeler cette fonction. Munchkin a ainsi le temps de charger et de cookie.

```javascript
//Function to read value of a cookie
function readCookie(name) {
    var cookiename = name + "=";
    var ca = document.cookie.split(';');
    for(var i=0;i < ca.length;i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if (c.indexOf(cookiename) == 0) return c.substring(cookiename.length,c.length);
    }
    return null;
}

//Call readCookie function to get value of user's Marketo cookie
var value = readCookie('_mkto_trk');
```

Ensuite, transmettez la valeur du cookie &#39;_mkto_trk&#39; à votre serveur. Pour récupérer les données de prospect, à partir de votre serveur, vous effectuez un appel à l’[API REST Get Multiple Leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) avec cette valeur de cookie. Vous avez besoin de vos points d’entrée d’authentification et REST à partir de votre instance . Votre appel doit être structuré comme suit :

REMARQUE : la valeur du cookie `_mkto_trk` comprend une esperluette et doit être codée en URL pour `%26` être correctement acceptée par le point d’entrée Marketo.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "cde42eff-aca0-48cf-a1ac-576ffec65a84:ab"
# Replace with filter type and values
filter_type_and_values = "&filterType=cookies&filterValues=id:AAA-BBB-CCC%26token:_mch-marketo.com-1418418733122-51548&fields=cookies,email"
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

L’exemple ci-dessus renvoie l’e-mail et tous les cookies associés à l’utilisateur. Vous pouvez ensuite utiliser ces données pour personnaliser la page suivante visitée par l’utilisateur.

`{"requestId":"aa00#14a405aa786","result":[{"id":583,"email":"<testaccount@gmail.com>","cookies":"_mch-marketo.com-1418418733122-51548"}],"success":true}`

Publié le _2014-10-30_ par _Murta_

## Quand avez-vous besoin d’un développeur pour vous aider à automatiser le marketing ?

REMARQUE : Ceci est un article de blog d&#39;invité. Josh Hill est responsable des pratiques Marketo chez Perkuto, une agence d’automatisation du marketing. Josh travaille à l&#39;intersection du marketing, des ventes et de la technologie pour fournir des systèmes de génération de revenus. Il écrit sur l&#39;automatisation du marketing et la génération de la demande sur [MarketingRockstarGuides.com](https://www.marketingrockstarguides.com/).

Les plateformes d&#39;automatisation du marketing sont extrêmement puissantes, prêtes à l&#39;emploi et entre les mains d&#39;un opérateur expérimenté. Les plateformes, par définition, permettent l’utilisation d’applications d’extension pour que le système fasse encore plus de choses étonnantes pour votre équipe. Vous pensez peut-être que le moteur logique de Marketo est capable de tant de choses (et c’est le cas), mais il existe des limites. Marketo ne peut pas tout faire pour vous, et ne le devrait pas non plus.

Il existe d’autres outils qui remplissent leur fonction mieux que Marketo ne pourrait le créer. La plateforme de Marketo est très ouverte, ce qui permet à l’écosystème d’applications [LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) d’exister. Vous pouvez également utiliser cette ouverture pour étendre les fonctionnalités de votre site et de Marketo en fonction des besoins de votre entreprise. L’avantage d’une plateforme comme Marketo est qu’elle permet aux spécialistes du marketing de créer des pages, des e-mails et une logique de routage sans avoir à être un programmeur à part entière.
De nos jours, un spécialiste du marketing a besoin de comprendre la logique, mais il est préférable de laisser la programmation réelle aux experts. Alors, comment savoir quand vous devez faire appel à un développeur ? J’ai quelques règles de base, ou heuristiques, pour décider quand un programmeur doit être impliqué : - Lorsque Marketo n’a pas de filtre, de déclencheur ou de fonctionnalité évidents pour le besoin, cela peut souvent se faire avec JavaScript ou jQuery. - Cela sera-t-il trop complexe pour Marketo en soi ? - Marketo peut-il le faire ? - La personnalisation d’un site web n’est-elle pas facilement prise en charge ? - Marketo doit-il communiquer avec un site web ou une autre base de données ? - Est-ce que cela ressemble à quelque chose qu&#39;un ordinateur peut faire, mais Marketo n&#39;a pas de fonction pour lui ? N’oubliez pas que même si Marketo n’offre pas de fonction prête à l’emploi, il se connecte à de nombreuses intégrations tierces ainsi qu’à des connexions personnalisées.

Jetez un coup d’œil à quelques-unes de ces catégories sur la [marketplace LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) : - [Outils Analytics](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Ajout de données](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [Systèmes de gestion de contenu](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) Certaines applications tierces fournissent des panneaux de contrôle intuitifs et des outils de configuration directement dans la plateforme (GoToWebinar). Il s’agit d’intégrations « natives », où la tâche la plus difficile consiste à configurer la connexion, puis à l’utiliser dans Marketo. D’autres extensions, cependant, nécessitent l’utilisation de l’API plus complexe qui doit être programmée plus directement.

**Options d&#39;intégration de Marketo** - Intégration LaunchPoint - généralement une connexion ou des paramètres faciles. - Intégration d’API - nécessite la configuration de l’API et de la programmation : (1) [API REST](/help/rest-api/rest-api.md) (2) [API SOAP](/help/soap-api/soap-api.md) (3) [Intégration Webhook](/help/webhooks/webhooks.md) - nécessite la configuration d’un code spécial, mais assez facile. (4) [Script Email](./email-scripting.md) (Velocity) - JavaScript et jQuery : (1) [Forms 2.0](/help/javascript-api/forms-api-reference.md) (2) [Suivi des leads (Munchkin)](/help/javascript-api/lead-tracking.md) (3) [RTP JS](/help/javascript-api/web-personalization.md) Voici quelques cas d’utilisation de l’aide au développement pour étendre les fonctionnalités de la plateforme Marketo. Avez-vous l’un de ces cas d’utilisation ? Si c&#39;est le cas, il est peut-être temps de parler à un développeur. [Consultez la section Partenaires de services sur LaunchPoint](https://exchange.adobe.com/apps/browse/ec?product=MRKTO).

Publié le _2014-11-06_ par _Josh_

## Intégration de Slack à Marketo

[Slack est une plateforme de collaboration d’entreprise](https://slack.com/). Si votre équipe utilise Slack, vous pouvez facilement importer des notifications Marketo dans votre workflow. Cette publication vous explique comment ajouter une notification à votre journal de conversation lorsqu’une activité de prospect spécifique se produit dans Marketo. Les cas d’utilisation potentiels incluent la notification de l’ensemble de votre équipe à propos du remplissage d’un formulaire, d’une visite d’une page de tarification ou d’un prospect qui n’a pas été contacté depuis 30 jours. La capture d’écran ci-dessous montre à quoi ressemblera la notification Marketo dans Slack après avoir suivi les étapes de cet article d’aide.

1. Connectez-vous à Slack. Clic sur Intégrations dans Slack
1. Cliquez sur le bouton Ajouter pour les Webhooks entrants
1. Choisissez le canal de la notification Marketo. Cliquez Ensuite Sur Ajouter L’Intégration Webhook Entrante.
1. Copier-coller l’URL du Webhook (nécessaire pour l’étape 8)
1. Choisir un nom pour la notification
1. Connectez-vous à Marketo. Accédez à Admin. Cliquez sur Webhooks.
1. Cliquez sur Nouveau Webhook
1. Saisissez un Nom pour le Webhook. Saisissez l’URL du Webhook à l’étape 4. Saisissez Valider comme type de demande. Saisissez un modèle de payload.

Voici le modèle de payload de la capture d’écran. Il utilise des jetons de prénom, de société et d’adresse e-mail au niveau du prospect.

`payload={"text": "DEVELOPER SITE ALERT: {{lead.First Name:default=edit me}} {{lead.Company=edit me}}, {{lead.Email Address:default=no email address}}" }`

1. Configurez une campagne de déclenchement dans Marketo. L’étape de flux consiste à appeler le Webhook à Slack. La liste dynamique est une visite de page web.
1. Vérifiez Que Cela Fonctionne.

Pour plus d’informations sur les Webhooks dans Marketo[&#x200B; consultez la &#x200B;](./webhooks/webhooks.md) documentation pour les développeurs .

Publié le _2014-11-10_ par _Murta_

## Intégration de Litmus à Marketo

[Litmus est un service](https://www.litmus.com/) qui permet de tester des e-mails sur des navigateurs et des clients de messagerie. Litmus fournit également des analyses autour des e-mails, y compris les clics, les ouvertures et les suppressions. Cette publication vous explique comment intégrer Marketo à Litmus.

1. Lors de la configuration de votre programme de messagerie dans Marketo, cliquez sur « Mes jetons » dans le tableau de bord du programme
1. Faites glisser le jeton « Script d’e-mail » vers le panneau du milieu pour l’ajouter.
1. Nommez le jeton et cliquez sur « Cliquer pour modifier ».
1. À droite, sous « Objets standard », développez la catégorie « Lead ». Recherchez le champ « Adresse e-mail » et cochez la case. Dans l&#39;espace script vide à gauche de cette même page, collez le code de tracking fourni par Litmus. Dans le code Litmus, remplacez chaque instance de `{{lead.Email Address}}` par `${lead.Email}`. Cliquez sur Enregistrer pour fermer la fenêtre Lightbox et cliquez de nouveau sur « Enregistrer » sur la page des jetons.
1. Notez le nom du jeton `{{my.LitmusToken}}`. Ouvrez l’e-mail que vous souhaitez suivre. Tout en bas de votre e-mail, placez votre nouveau jeton de script. Vous pouvez également ajouter des informations par défaut pour correspondre à la version de Litmus `{{my.LitmusToken:default=editme}}`.

Lorsque l’e-mail est envoyé, le script est placé dans l’e-mail par Marketo.

Publié le _2014-11-18_ par _Murta_

## Spécifier une image lorsqu&#39;une landing page Marketo est partagée sur Facebook

Supposons que vous souhaitiez qu’une image s’affiche automatiquement lorsque vous partagez une page de destination Marketo sur Facebook. Peut-être souhaitez-vous également que cette image ne figure pas sur la page de destination de Marketo elle-même, mais simplement sur le partage Facebook. Pour ce faire, ajoutez une balise meta open-graph à votre page de destination Marketo. Pour ce faire, procédez comme suit.

1. Sélectionnez votre page de destination. Cliquez ensuite sur Modifier le brouillon.
1. Cliquez sur Modifier les balises de métadonnées de page.
1. Ajoutez des métadonnées open-graph à la section Balises de l’organisation Facebook. Cliquez ensuite sur Enregistrer. Voici le format : `<meta property="og:image" content="http://example.com/example.jpg"/>`

[Pour plus d’informations](https://developers.facebook.com/docs/sharing/best-practices) consultez la documentation pour les développeurs de Facebook sur les balises meta de graphique ouvert .

Publié le _2014-11-17_ par _Murta_

## Rediriger la page en fonction du référent

Supposons que vous souhaitiez empêcher le trafic direct vers une page de destination Marketo. Imaginez que cette page contienne du contenu téléchargeable, tel qu’un PDF, que vous souhaitez que l’utilisateur remplisse un formulaire avant de le recevoir. Vous pouvez résoudre ce problème en vérifiant si l’utilisateur provient d’une certaine page. Dans ce cas, il s’agit de la page sur laquelle l’utilisateur doit remplir un formulaire. Si l’utilisateur ne provient pas de cette page, vous pouvez le rediriger vers la page de remplissage du formulaire. Pour ce faire, vous devez vérifier si la page de référence de la page de destination avec du contenu est la page de remplissage du formulaire.
Remplacez les deux instances de `http://example.com/PageWithForm` dans le fragment de code ci-dessous par un lien vers la page d’où vous souhaitez que l’utilisateur vienne. Il peut s’agir de la page de remplissage du formulaire.**

```javascript
<script>
window.onload = function() {
  if (document.referrer !== "http://example.com/PageWithForm") {
    document.location.href = "http://example.com/PageWithForm";
  };
 };
</script>
```

Incluez le fragment de code JavaScript personnalisé avant la balise de fermeture sur votre page de destination Marketo avec du contenu.** Si l’utilisateur n’est pas envoyé à la page de destination avec du contenu provenant de la page de remplissage du formulaire, il est maintenant redirigé vers la page de remplissage du formulaire.

Publié le _2014-11-18_ par _Murta_

## Intégration de Trello à Marketo

Trello est une [application de gestion de projets web populaire](https://trello.com/). Si votre équipe utilise Trello, vous pouvez facilement importer des notifications Marketo dans votre workflow. Cette publication vous explique comment ajouter une carte avec une notification Marketo à votre tableau Trello. Cette carte sera ajoutée lorsqu’une activité de prospect spécifique se produit dans Marketo. Les cas d’utilisation potentiels incluent la notification de l’ensemble de votre équipe à propos du remplissage d’un formulaire, d’une visite d’une page de tarification ou d’un prospect qui n’a pas été contacté depuis 30 jours.

1. Connexion à Trello. Accédez au tableau Trello qui ajoutera des notifications Marketo dans . Cliquez sur Ajouter une liste, puis nommez-la.
1. Cliquez sur Afficher la barre latérale. Cliquez sur Paramètres d’e-mail à l’accueil. Notez l’adresse e-mail dans la zone « Votre adresse e-mail pour ce panorama ». Cet e-mail sera utilisé à l’étape 6. Choisissez la liste à laquelle ajouter la notification Marketo.
1. Connectez-vous à Marketo. Cliquez sur la nouvelle campagne intelligente. Saisissez un nom, puis cliquez sur Enregistrer.
1. Accédez à la liste dynamique. Choisissez un déclencheur pour cette campagne intelligente. Dans cet exemple, nous utilisons le déclencheur Remplir le formulaire . Faites glisser Remplit le déclencheur de formulaire vers le panneau central. Sélectionnez le formulaire qui doit déclencher cette notification.
1. Créez un e-mail. Cliquez sur Nouveau. Cliquez sur Ressource locale. Cliquez sur Nouvel e-mail. Nommez l’e-mail. Cliquez ensuite sur Créer.
1. Cliquez sur « Modifier le brouillon » pour l’e-mail que vous avez créé à l’étape 5. Faites glisser les jetons pertinents que vous souhaitez afficher dans votre carte Trello. L’objet de l’e-mail Marketo apparaît dans le titre de la carte Trello, et le corps de l’e-mail Marketo apparaît dans la description de la carte Trello. Par exemple, vous pouvez utiliser « ALERTE DU PROSPECT : `{{lead.First Name:default=edit me}}` `{{lead.Last Name:default=edit me}}` si vous souhaitez que le prénom et le nom du prospect figurent dans le titre de la carte Trello. Validez ensuite l’e-mail.
1. Accédez à Campagne intelligente. Cliquez sur Flux. Faites glisser Envoyer l’alerte vers le panneau du milieu. Sélectionnez l’e-mail qui vient d’être créé. Sélectionnez « Envoyer à » sur Aucun. Sélectionnez « À d’autres e-mails » comme e-mail Trello à l’étape 2.
1. Cliquez sur Planifier dans le menu supérieur. Cliquez sur Activer. Cliquez sur Confirmer.
1. Testez l’intégration. Une carte avec le prénom et le nom du prospect dans le titre s’affiche sur le tableau Trello. Pour plus d’informations, consultez la documentation de [Trello](https://support.atlassian.com/trello/).

Publié le _2014-11-18_ par _Murta_

## Rechercher des leads par valeur de champ personnalisé

Supposons que vous souhaitiez obtenir des prospects de l’API Marketo qui correspondent à certains critères d’activité ou d’inactivité. Par exemple, vous pouvez rechercher un prospect dont le score n’a pas changé au cours des 30 derniers jours. En suivant les étapes de cette publication, vous êtes en mesure d’obtenir cette liste de prospects. Pour ce faire, nous créons une campagne intelligente dans Marketo qui identifie les prospects dont le score n’a pas changé au cours des 30 derniers jours, puis stockons une valeur sur ces prospects pour les identifier. Nous allons ensuite interroger l’API avec cette valeur.

1. Créez un champ personnalisé appelé customLeadStatus** Connectez-vous à Marketo et accédez au panneau d’administration. Cliquez sur Gestion des champs . Cliquez sur « Nouveau champ personnalisé ».  Nommez le champ . Cliquez ensuite sur Créer.
1. Créez une campagne dynamique avec une liste dynamique qui recherche les prospects qui n’ont pas été mis à jour depuis 30 jours.** Cliquez Sur Nouvelle Campagne Intelligente. Nommez la nouvelle campagne intelligente. Faire glisser « Aucun score » a été modifié du panneau de droite au panneau du milieu.
1. Ajoutez une étape de flux à la campagne intelligente à partir de l’étape 3 pour mettre à jour le champ customLeadStatus avec une nouvelle valeur.** Faites glisser Modifier la valeur des données du panneau de droite vers le panneau du milieu.
1. Mettez à jour Smart Campaign pour autoriser les prospects à s’exécuter plusieurs fois.** Cliquez sur Planifier. Cliquez ensuite sur Modifier.  Sélectionnez à chaque fois. Cliquez ensuite sur Enregistrer. La campagne va maintenant commencer à fonctionner.
1. Interrogez l’[API REST Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Définition des paramètres filterType=customLeadStatus et filterValue=needsEnrichment.**

Il s’agit d’un exemple de requête qui renvoie ces données.

`<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?access_token=><yourAccessToken>&filterType=customLeadStatus&filterValues=needsEnrichment`

Un appel API réussi renvoie des données JSON avec des prospects dont le champ customLeadStatus correspond à la valeur needsEnrichment. Consultez la section [Obtenir plusieurs prospects par type de filtre API REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) pour plus d’informations.

Publié le _2014-11-22_ par _Murta_

## Synchronisation des opportunités via l’API SOAP

Cette publication décrit comment insérer des opportunités dans Marketo via l’API SOAP et les associer à des sociétés et des prospects. Elle commence par une explication du fonctionnement de ce processus, puis fournit des exemples de code pour chacun des scénarios.

**Diagramme de structure du tableau** Tout d’abord, le diagramme ci-dessous décrit la structure du tableau. L’identifiant est généré automatiquement lors de la création d’un enregistrement (numéro séquentiel). Le champ en gras est obligatoire : seul le rôle Personne de l’opportunité comporte des champs obligatoires. Les champs entre parenthèses sont facultatifs, de même que les connexions avec une ligne pointillée.

**Insertion de base de l’opportunité** Vous devez d’abord insérer l’opportunité, puis le rôle de personne/opportunité, qui lie l’opportunité au(x) lead(s). Pour cet exemple, nous vous recommandons d’utiliser l’ID de lead et l’ID d’opportunité pour spécifier le rôle de personne dans l’opportunité. Vous obtenez l’ID de l’opportunité dans la réponse SOAP lors de la création de l’opportunité. L’ID de lead est visible sur chaque lead dans la base de données de leads de Marketo.

**Lien d’entreprise** Dans la plupart des cas, vous souhaitez lier une opportunité à une entreprise, en plus d’un individu. Dans Marketo, vous ne pouvez pas accéder aux enregistrements Société via l’API SOAP, seuls les enregistrements Lead (les enregistrements Lead contiennent les champs Société). Il est toujours possible de lier les opportunités à une entreprise en ajoutant un identifiant d’entreprise unique à chaque prospect et en utilisant cet identifiant dans votre opportunité. L’étape 1 consiste à créer un champ « ID d’entreprise » sur l’enregistrement du prospect et à le renseigner avec un identifiant unique, généralement à partir d’un système principal. L’étape 2 consiste à créer un champ « Company Id » sur l’opportunité : vous devez demander l’assistance ou le conseil Marketo pour créer un tel champ pour vous. Renseignez ensuite ce champ lors de la création d’opportunités, ce qui connecte l’opportunité à l’entreprise. Cela est particulièrement important si vous utilisez Marketo Revenue Cycle Analytics et que vous souhaitez utiliser l’analyseur d’influence des opportunités, qui dépend des informations de l’entreprise.

**Utilisation d’identifiants externes** Dans de nombreux cas, vous pouvez disposer de vos propres identifiants uniques lors de l’intégration à un système back-end. Il est possible d’utiliser ces identifiants uniques dans Marketo via des clés étrangères. Pour les prospects, vous utilisez généralement l’ID de personne du système étranger (FSPID), qui est utilisé à la place de l’ID Marketo ou de l’adresse e-mail comme identifiant unique. Le FSPID est un champ système masqué, qui n’est pas visible dans Marketo. Si vous ne le faites pas déjà, il est nécessaire que la synchronisation des opportunités enregistre également le FSPID dans un champ personnalisé, par exemple « ID étranger » (vous pouvez nommer le champ comme vous le souhaitez). Vous pouvez créer ce champ vous-même en tant qu’administrateur ou administratrice Marketo. Pour les opportunités, vous demandez à l’assistance Marketo de créer un autre champ personnalisé sur l’opportunité, par exemple appelé « ID étranger » (vous pouvez le nommer comme bon vous semble). Renseignez ensuite ce champ lors de l’insertion d’opportunités. Enfin, lorsque vous créez le rôle de personne/opportunité, vous utilisez les deux clés étrangères pour spécifier le lien entre les leads et les opportunités, au lieu d’utiliser les identifiants Marketo. Vous pouvez également utiliser les clés étrangères pour mettre à jour les prospects des opportunités. À l’heure actuelle, il n’est pas possible d’ajouter une clé étrangère aux enregistrements de rôle de personne dans l’opportunité. Vous devrez donc utiliser l’ID Marketo généré automatiquement pour cela (que vous obtenez dans la réponse SOAP après la création du rôle de personne dans l’opportunité).

**Exemple de code** Étapes : 1. Insérer/mettre à jour le prospect avec clé étrangère et ID d’entreprise 1. Insérer une opportunité avec clé étrangère 1. Insérer un rôle de personne pour l’opportunité avec des clés étrangères 1. Requête SOAP - Mise à jour d’un prospect existant avec une clé étrangère et un ID d’entreprise Cette opération met à jour un prospect existant (ID Marketo « 6 ») avec la clé étrangère « 12346 » et l’ID de compte/société « C123 ». Nous enregistrons également la clé étrangère dans un champ personnalisé, car nous en avons besoin pour le rôle de personne/opportunité. L’utilisation d’une clé étrangère est facultative : vous pouvez également utiliser l’ID Marketo pour lier ce prospect à l’opportunité. L’utilisation de l’ID d’entreprise est également facultative, mais elle est obligatoire si vous souhaitez utiliser l’analyseur d’influence d’opportunité dans RCA. Requête :

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncLead>
         <leadRecord>
            <Id>6</Id>
            <ForeignSysPersonId>12346</ForeignSysPersonId>
            <leadAttributeList>
               <attribute>
                  <attrName>FSPID</attrName>
                  <attrValue>12346</attrValue>
               </attribute>
               <attribute>
                  <attrName>cAccountFSID</attrName>
                  <attrValue>C123</attrValue>
               </attribute>
            </leadAttributeList>
         </leadRecord>
         <returnLead>false</returnLead>
      </mkt:paramsSyncLead>
   </soapenv:Body>
</soapenv:Envelope>
```

Réponse :

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncLead>
         <result>
            <leadId>6</leadId>
            <syncStatus>
               <leadId>6</leadId>
               <status>UPDATED</status>
               <error xsi:nil="true"/>
            </syncStatus>
            <leadRecord xsi:nil="true"/>
         </result>
      </ns1:successSyncLead>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Requête SOAP - Création d’opportunité Dans ce cas, 2 champs personnalisés ont été créés sur la table des opportunités : - `opportunityId` → contient l’ID d’opportunité unique - `cAccountFSID` → contient la référence de la société au lieu de spécifier votre propre ID d’opportunité. Vous pouvez également utiliser l’identifiant d’opportunité généré par Marketo. Dans ce cas, vous laissez de côté le nœud Clé externe . L’association d’entreprise est également facultative, mais elle est obligatoire si vous souhaitez utiliser l’analyseur d’influence d’opportunité dans RCA. Requête :

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:03:28-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_4</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 4 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>501.00</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>501</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Réponse :

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>40</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Requête SOAP - Rôle de personne de l’opportunité Cette requête associe le prospect à l’opportunité. Vous pouvez spécifier plusieurs liens dans une seule demande SOAP (cet exemple lie l’opportunité à un seul prospect). Celui-ci utilise les clés étrangères pour spécifier le lien, mais dans les commentaires il indique également comment utiliser les ID réels (dans ce cas : 6 pour l’ID de lead et 40 pour l’ID d’opportunité). Ces champs « IsPrimary » et « Role » sont facultatifs. Requête :

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <!--id>40</id-->
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_4</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Réponse :

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>11</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Autre approche (effectuez les étapes 2 et 3 dans un appel)** Bien que vous puissiez d’abord insérer l’opportunité, puis le rôle de personne de l’opportunité, il est également possible de le faire dans un appel SOAP. Cependant, vous devez utiliser la clé étrangère pour l’opportunité (vous ne pouvez pas utiliser l’ID d’opportunité généré automatiquement dans le rôle de personne avec opportunité, car l’opportunité n’a pas encore été générée). Bien sûr, vous pouvez également lier plusieurs prospects à cette opportunité dans ce même appel API (cet exemple lie l’opportunité à un seul prospect). Requête :

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:44:08-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_5</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 5 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
             <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_5</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Réponse :

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>41</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
               <mObjStatus>
                  <id>12</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Publié le _2014-11-26_ par _Jep_

## Requêtes d’API REST multithread

Si vous souhaitez améliorer les performances lors de l’appel de l’API Marketo, vous pouvez effectuer des requêtes simultanées. Cette approche vous permet d’obtenir plus de données dans un délai plus court. Lors de l’exécution d’une requête API, une partie du temps d’aller-retour entre le client et le serveur correspond au temps de transfert sur le câble. Ainsi, si nous pouvons réduire le temps de transfert sur le fil pour les demandes dans leur ensemble, nous améliorons la performance. L’exemple de code ci-dessous montre comment procéder dans Ruby. Elle utilise EventMachine, qui est une bibliothèque de traitement des événements [&#x200B; utilisée pour effectuer des requêtes multithread](https://github.com/igrigorik/em-http-request/wiki/Parallel-Requests). L’exemple ci-dessous appelle l’[API des activités de lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) et effectue deux requêtes simultanées. Cette approche élimine le temps de transfert du client vers le serveur pour la deuxième requête. Pour ce faire, il inclut la deuxième demande en même temps que la première. Les réponses de l’API sont écrites dans un fichier texte.

```java
require 'em-http-request'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = ["&nextPageToken=A5YMOYZQBOGD2OSYYBYDAQGEMGLBDGDANAABQGRAQWAAKKID", "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"]
# Specify activities needed
activity_type_ids = "&activityTypeIds=1&activityTypeIds=12"
requesturl_a = marketo_instance + endpoint + auth_token + since_date_time.at(0) + activity_type_ids
requesturl_b = marketo_instance + endpoint + auth_token + since_date_time.at(1) + activity_type_ids

# Make request
EventMachine.run do
  http1 = EventMachine::HttpRequest.new(requesturl_a).get
  http2 = EventMachine::HttpRequest.new(requesturl_b).get

# When API response is received, write response to a text file
  http1.callback {
  File.open('response1.txt', 'w') do |t|
  t.puts http1.response
  end }

  http2.callback {
  File.open('response2.txt', 'w') do |t|
  t.puts http2.response
  end }
end
```

Publié le _2014-12-03_ par _Murta_

## Requêtes d’API d’optimisation des performances

Cette publication présente les stratégies permettant d’améliorer les performances lors de la demande de données à partir de l’API Marketo. Cependant, vous devez évaluer les avantages de ces stratégies par rapport à la contrainte de fonctionnement des limites quotidiennes de l’API Marketo.
**Stratégie 1 - Demander moins de données dans chaque appel API** Généralement, lorsque vous demandez plus de données dans un appel API, le temps nécessaire à la recherche des données dans la base de données par le serveur Marketo augmente. Si vous effectuez un appel API avec des périodes, comme l’API SOAP [getMultipleLeads](/help/soap-api/getmultipleleads.md), raccourcissez la période par appel et compensez par davantage d’appels. Par exemple, au lieu de demander des données du 1er juin au 1er juillet, demandez un seul jour à la fois, par exemple un appel pour le 1er au 2 juin, puis un autre pour le 2 au 1 juin. Si vous effectuez un appel API qui renvoie des données des champs de prospect Marketo, ne demandez que les champs nécessaires. Chaque champ de prospect supplémentaire augmente de manière incrémentielle le temps nécessaire à un appel API. Une autre méthode consiste à réduire la taille du lot ou le nombre de prospects demandés par appel.
**Stratégie 2 - Effectuer des demandes simultanées** Pour améliorer les performances et extraire plus de données à la fois. Vous pouvez envoyer des requêtes simultanées à l’API . Cette approche réduit le temps total passé sur les requêtes d’API câblées. Supposons, par exemple, que vous envoyiez des requêtes à l’option Obtenir plusieurs prospects par type de filtre. Vous pouvez effectuer des requêtes simultanées pour une requête en interrogeant les pistes 1 à 300 et pour une autre requête en interrogeant les pistes 301 à 600.
**Stratégie 3 - Mettre en cache les données** Certaines données de Marketo sont modifiées moins souvent, comme la liste des champs de prospect, que d’autres données, telles que les données d’activité de prospect. Si vous mettez en cache des données moins souvent mises à jour, vous réduisez le nombre d’appels API que vous devez effectuer. Vous obtiendrez également de meilleures performances, car la recherche locale des données est généralement plus rapide que l’accès à celles-ci à partir d’un service web distant.

Publié le _2014-12-05_ par _Murta_

## Envoyer les données de formulaire Marketo à Google Analytics

Dans Google Analytics, vous pouvez envoyer des événements de données personnalisés, puis utiliser les données pour segmenter et analyser les performances de votre site web. Le fragment de code JavaScript ci-dessous vous permet de transmettre automatiquement les données de formulaire Marketo 2.0 à Google Analytics après l’envoi d’un formulaire web par un visiteur. Voici comment configurer ce paramètre.

**Étape 1** insérez la balise JavaScript sur toute page qui inclut Marketo Forms au bas du code (avant la balise ). Le JavaScript envoie uniquement les champs non masqués (sendHiddenFields : false). Cela peut être ajusté en modifiant sendHiddenFields de false à true. Vous pouvez également sélectionner des champs à exclure en ajoutant des identifiants de champ supplémentaires dans le tableau « fieldsToExclude ».

```javascript
function pushFormDataToGa(a){
setTimeout(function () {
document.getElementsByTagName('form')[0].getElementsByClassName(a.submitButton)[0].addEventListener('click', function() {
  allFields = document.getElementsByTagName('form')[0].getElementsByTagName('input');
  for(i=0;i<allFields.length;i++){
   if( (allFields[i].type !="hidden" && allFields[i].type !="submit" && allFields[i].value !="" && a.fieldsToExclude.indexOf(allFields[i].id) === -1  ) || (allFields[i].type === "hidden" && a.sendHiddenFields) ){
    console.log( allFields[i].name + ": "  + allFields[i].value);
    if(typeof(_gaq) != "undefined"){
    //Classic
    _trackEvent("Marketo Form Submission", allFields[i].value , allFields[i].name
{'nonInteraction': 1});
    }else if(typeof(ga) !="undefined"){
    //Universal
    ga('send', 'event',"Marketo Form Submission", allFields[i].value , allFields[i].name, {'nonInteraction': 1});
}}}}, false);
}, 3000);}
pushFormDataToGa({
 submitButton: "mktoButton",
 fieldsToExclude: ["Email","LastName", "FirstName"],
 sendHiddenFields : false
});
```

**Étape 2** les données en disponibilité générale apparaissent dans la section Création de rapports. Accédez à Comportement > Événements > Principaux événements. **Restrictions de script :** - Cet exemple de code est uniquement compatible avec [Marketo Forms 2.0](/help/javascript-api/forms-api-reference.md). - En raison de la politique de confidentialité de Google, vous ne pouvez pas envoyer d&#39;informations personnelles (email ou nom). Outre les problèmes potentiels de confidentialité, il s’agit d’informations d’identification personnelle, ce qui enfreint les conditions d’utilisation [Google Analytics](https://marketingplatform.google.com/about/analytics/terms/us/) :

« Vous n&#39;utiliserez pas (et ne permettrez pas à un tiers d&#39;utiliser) le Service pour suivre, collecter ou charger des données qui identifient personnellement une personne (comme un nom, une adresse e-mail ou des informations de facturation), ou d&#39;autres données qui peuvent être raisonnablement liées à ces informations par Google. »

Publié le _2014-12-16_ par _Yanir_

## Ajout d’un champ de nom complet à un formulaire Marketo

Nous savons que les formulaires Web plus courts améliorent les taux de conversion. L’exemple de code JavaScript ci-dessous vous permet de raccourcir davantage vos formulaires en fusionnant les champs Prénom et Nom en un seul champ Nom complet. Lorsque les visiteurs saisissent leur nom complet, le script divise automatiquement le texte en champs de prénom et de nom. Pour les visiteurs connus, le script joint les noms et prénoms, puis les copie dans le nouveau champ afin qu’ils n’aient pas à remplir à nouveau le champ. Voici comment configurer ce paramètre.

**Étape 1** créez un champ personnalisé dans Marketo appelé Nom complet. Pas besoin de le créer dans votre plateforme CRM, car le script n&#39;utilisera que ce champ pour afficher le nom complet.
**Étape 2** ajoutez ce champ à tous vos formulaires web. Définissez vos champs de prénom et de nom sur masqués. Dans le JavaScript, modifiez la configuration « splitFullName » pour contenir les 3 noms de champ. Remarque : Assurez-vous que ces noms n’apparaissent nulle part ailleurs sur la page.
**Étape 3** insérez le JavaScript dans toutes vos pages de destination en bas du code, avant la balise .

```javascript
<script>
MktoForms2.whenReady(function (form){
        function splitFullName(a,b,c){
                String.prototype.capitalize = function(){
                        return this.replace( /(^|s)([a-z])/g , function(m,p1,p2){ return p1+p2.toUpperCase(); } );
                };
                document.getElementsByName[c](0).oninput=function(){
                        var fullName = document.getElementsByName[c](0).value;
                        if((fullName.match(/ /g) || []).length ===0 || fullName.substring(fullName.indexOf(" ")+1,fullName.length) === ""){
                                var first = fullName.capitalize();;
                                var last = "null";
                        }else if(fullName.substring(0,fullName.indexOf(" ")).indexOf(".")>-1){
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize() + " " + fullName.substring(fullName.indexOf(" ")+1,fullName.length).substring(0,fullName.substring(fullName.indexOf(" ")+1,fullName.length).indexOf(" ")).capitalize();
                                var last = fullName.substring(first.length +1,fullName.length).capitalize();
                        }else{
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize();
                                var last = fullName.substring(fullName.indexOf(" ")+1,fullName.length).capitalize();
                        }
                        document.getElementsByName[a](0).value = first;
                        document.getElementsByName[b](0).value = last;
                };
                //Initial Values
                if(document.getElementsByName[c](0).value.length < 2 && document.getElementsByName[b](0).value.length.length >2 && document.getElementsByName[a](0).value.length.length >2 ){
                        var first = document.getElementsByName[a](0).value.capitalize();
                        var last = document.getElementsByName[b](0).value.capitalize();
                        var fullName =  first + " " + last ;
                        console.log(fullName);
                        document.getElementsByName[c](0).value = fullName;
                }
        }
        splitFullName("FirstName","LastName","leadFullName");
});
</script>
```

Remarque : ce code fonctionne uniquement avec Marketo Forms 2.0.

Publié le _2014-12-16_ par _Yanir_

## Utiliser cURL pour importer des leads via l’API REST

Voulez-vous importer des prospects à partir d’un fichier CSV par le biais de l’API REST, mais avez remarqué que c’est difficile à faire à l’aide de l’extension Postman Chrome. Dans ce billet, nous expliquons comment procéder avec cURL.

1. [Téléchargez et installez cURL](https://curl.se/download.html), un outil de ligne de commande que nous utilisons pour transmettre des données à l’API REST Marketo.
1. Ouvrez la ligne de commande, puis accédez à l’emplacement du fichier CSV. Les en-têtes de colonne du fichier CSV doivent correspondre aux noms de champ de l’API, et non aux noms de champ Marketo.
1. Vous avez besoin d’un jeton d’accès. Connectez-vous à Marketo, accédez à Admin, puis à LaunchPoint. Recherchez votre utilisateur de l’API REST et cliquez sur « Afficher les détails ». Cliquez sur le bouton « Obtenir un jeton ».
1. Vous aurez également besoin de votre point d’entrée REST spécifique à votre instance Marketo. Connectez-vous à Marketo, puis accédez à Admin et ensuite à Web Services. Dans la section marquée « API REST » se trouve l’URL du point d’entrée .
1. Sur la ligne de commande, suivez ce format pour l’appel cURL. Remplacez `<accesstoken>` par votre jeton d’accès à l’étape 3 et remplacez `<REST API Endpoint URL>` par l’URL de point d’entrée de l’API REST à l’étape 4. Plus d’informations [disponibles ici](https://developer.adobe.com/marketo-apis/api/mapi/#operation/importLeadUsingPOST). Le caractère « /bulk » ici remplacera le caractère « /rest » à la fin de l’URL du point d’entrée. Si le point d’entrée est défini sur /rest/bulk, il renvoie une erreur.

`curl -i -F format=csv -F file=@leaddata.csv -F access_token=<accesstoken> <REST API Endpoint URL>/bulk/v1/leads.json`

Publié le _2014-12-16_ par _Jordan_

## Ajout d’une alerte de confirmation à un Marketo pour

Supposons que, lorsqu’un utilisateur clique sur le bouton « Envoyer » d’un formulaire Marketo, vous souhaitez afficher une notification demandant à l’utilisateur s’il est « Accepté d’envoyer ». Pour ce faire, implémentez quelques lignes de JavaScript, qui afficheront une boîte de confirmation lorsque quelqu’un cliquera sur le bouton Envoyer . Voici un exemple de la procédure à suivre. Ajoutez la fonction onSubmit à votre formulaire Marketo, comme illustré ci-dessous. Pour plus d’informations sur l’API Marketo Forms, consultez [&#x200B; documentation destinée aux développeurs](/help/javascript-api/forms-api-reference.md).

```javascript
<script src="//app-e.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_19"></form>
<script>
MktoForms2.loadForm("//app-e.marketo.com", "212-RBI-463", 19,function(form){

//Add this function to your Marketo form script
form.onSubmit(function(){
alert("Do you really want to submit the form?");
});
});
</script>
```

Publié le _2014-12-17_ par _David_

## Afficher un message de remerciement sans page de destination de relance

En règle générale, lorsque vous utilisez des formulaires Marketo, vous créez deux pages de destination : l’une sur laquelle placer le formulaire et l’autre vers laquelle le rediriger une fois le formulaire rempli. Cependant, dans certains cas, il se peut que vous ne souhaitiez pas conserver deux pages de destination distinctes, mais très similaires. Vous pouvez en fait utiliser la même page de destination pour le formulaire et pour le message de remerciement à l’aide de l’API JavaScript Forms 2.0. Pour ce faire, créez d’abord votre page de destination d’enregistrement et votre formulaire, puis placez le formulaire sur la page de destination comme vous le feriez normalement. Ajoutez ensuite un élément HTML à la page. Dans cet élément, nous ajoutons du code qui s’active au moment de l’envoi du formulaire. Il masque ensuite le formulaire et affiche un masqué <div> qui contient le message de remerciement. Votre JavaScript doit se présenter comme suit :

```javascript
//Edit host with your Marketo instance info
<script src="//<host>/js/forms2/js/forms2.js"></script>
<script>
MktoForms2.whenReady(function (form){
 //Add an onSuccess handler
  form.onSuccess(function(values, followUpUrl){
   //get the form's jQuery element and hide it
   form.getFormElem().hide();
   document.getElementById('confirmform').style.visibility = 'visible';
   //return false to prevent the submission handler from taking the lead to the follow up url.
   return false;
 });
});
</script>
```

Modifier le texte du message de remerciement.

`<div id="confirmform" style="visibility:hidden;"><p><strong>Thank you. Check your email for details on your request.</strong></p></div>`

Vous pouvez modifier le nom d’hôte et le message de remerciement dans l’exemple de code. La première doit référencer votre instance Marketo (par exemple, « //app-sj06.marketo.com/js/forms2/js/forms2.js ») et la seconde doit contenir le texte de remerciement à afficher une fois le formulaire rempli. Le texte s’affiche sur la page de destination à l’emplacement exact où vous placez l’élément HTML. Veillez donc à le modifier dans la feuille de propriétés. Vous devez également vous assurer que le calque de l’élément HTML est plus petit que le calque de votre formulaire. Par défaut, les deux sont placés au niveau du calque 15. Vous êtes donc en sécurité si vous créez votre élément HTML au niveau du calque 11. Si vous ne le faites pas, vous ne pourrez saisir aucune zone de formulaire qui chevauche le message de remerciement. Il n’est pas nécessaire de modifier le type de relance sur le formulaire ou sur la page de destination, car le JavaScript remplacera ces paramètres. Pour plus d’informations sur l’API Marketo Forms, consultez la [documentation destinée aux développeurs](/help/javascript-api/forms-api-reference.md).

Publié le _2014-12-19_ par _Kristin_

## Mise en surbrillance des projets Source ouverts créés sur la plateforme Marketo

Il s’agit du premier article d’une série consacrée aux projets open source créés autour de la plateforme Marketo par la communauté des développeurs et développeuses. Nous conservons [une liste sur le compte GitHub de Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) où nous effectuons le suivi des bibliothèques clientes et des projets créés par la communauté des développeurs de Marketo. Vous trouverez ci-dessous trois projets développés autour des API REST et SOAP de Marketo. Daniel Chesterton a créé [une bibliothèque cliente en PHP pour l’API REST Marketo](https://github.com/dchesterton/marketo-rest-api). La bibliothèque cliente couvre actuellement 12 points d’entrée de l’API REST.** Kyle Halstvedt d&#39;Elixiter a créé un projet pour [extraire les leads des listes statiques Marketo dans une feuille de calcul Google](https://github.com/Elixiter/mkto_google-spreadsheet). Le projet de Kyle utilise l’API REST Marketo.  David Santoso a créé un [Ruby gem pour l’API Marketo SOAP.](https://github.com/davidsantoso/markety) Ce projet peut vous aider à intégrer plus rapidement l’API Marketo SOAP à une application Ruby on Rails.  Nous sommes ravis de voir d’autres projets créés par la communauté des développeurs sur la plateforme Marketo. Si vous travaillez sur un projet open source pour la plateforme Marketo, [envoyez-le à ce référentiel GitHub par le biais d’une demande d’extraction](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publié le _2015-01-02_ par _Murta_

## Modifier de manière dynamique le contenu d’une page en fonction de l’emplacement d’un utilisateur

Supposons que vous souhaitiez modifier de manière dynamique le numéro de téléphone sur une page de destination en fonction de l’emplacement d’un utilisateur. Par exemple, si la personne se trouve en Californie, vous souhaiteriez lui montrer le numéro de téléphone de votre bureau en Californie sur votre page de destination, mais si elle se trouve au Japon, vous souhaiteriez lui montrer le numéro de téléphone du bureau japonais.  Pour ce faire, vous pouvez utiliser JavaScript et l’API de géolocalisation [HTML5](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API). L’avantage de cette approche est que nous pouvons créer une page de destination et la modifier dynamiquement en fonction de l’emplacement d’un utilisateur, au lieu de plusieurs pages de destination statiques. Nous examinons les détails de mise en œuvre technique ci-dessous. Nous créons un objet pour les emplacements de bureaux avec des coordonnées de latitude et de longitude, et un second objet avec des numéros de téléphone de bureau. En production, il serait préférable de combiner ces deux objets en un seul objet .

```json
//Coordinates for Marketo offices
var officeLocations = {
    "San Mateo": {latitude: 37.5596465, longitude: -122.2870142},
    "Atlanta": {latitude: 33.8547013, longitude: -84.35552349999999},
    "Tokyo": {latitude: 35.6895, longitude: 139.6917},
    "Dublin": {latitude: 53.3478, longitude: -6.2603097},
    "Sydney": {latitude: -33.873651, longitude: 151.2068896},
    "Portland": {latitude: 45.512089, longitude: -122.6763367},
    "Tel Aviv": {latitude: 32.0852999, longitude: 34.78176759999999}
}

//Phone numbers for Marketo offices
var officePhoneNumbers = {
    "San Mateo": "+1-650-376-2300",
    "Atlanta": "+1-877-260-6586",
    "Tokyo": "+81-03-6759-8280",
    "Dublin": "+353-1-242-3000",
    "Sydney": "+61-2-9045-2711",
    "Portland": "+1-877-260-6586",
    "Tel Aviv": "+1-877-260-6586"
}
```

Nous créons une méthode pour demander l’emplacement d’un utilisateur. Pour gérer les erreurs, si l’emplacement de l’utilisateur n’est pas accessible, nous utiliserons par défaut le siège social de Marketo et son numéro de téléphone.

```javascript
//Method to get user's current location. Returns a position object with user's geo coordinates
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(findNearestOffice);
    } else {
        x.innerHTML = "Marketo Location: San Mateo
Marketo Phone Number: +1-877-260-6586";
    }
}
```

Enfin, nous créons une méthode pour trouver le bureau le plus proche de l’emplacement de l’utilisateur, puis nous renvoyons le numéro de téléphone du bureau le plus proche sur la page. Cette méthode utilise la méthode findNearest de [Geolib, qui est une bibliothèque JavaScript qui fournit des opérations géospatiales](https://github.com/manuelbieh/Geolib).

```javascript
//Find nearest Marketo office to user's location
function findNearestOffice(position) {
        var nearestOffice = geolib.findNearest({latitude: position.coords.latitude, longitude: position.coords.longitude}, officeLocations);
        x.innerHTML = "Marketo Location: " + nearestOffice.key + "
Marketo Phone Number: " +  officePhoneNumbers[nearestOffice.key];
}
```

Voici l’implémentation complète. Nous déclenchons la méthode getLocation lorsque l’utilisateur clique sur le bouton de la page. Ce [référentiel GitHub](https://github.com/MurtzaM/Find-Nearest-Marketo-Office) contient les fichiers nécessaires à la configuration de cette démonstration.

Publié le _2014-12-20_ par _Murta_

## Suivi des leads et domaines multiples

Le code de suivi Munchkin Marketo vous permet de suivre les visites sur votre site web. Il est probable que vous souhaitiez utiliser le code de suivi Munchkin pour enregistrer des leads anonymes pour la plupart ou la totalité des pages de votre site web. Voyons comment fonctionne Munchkin. Les visites sur la page sont enregistrées pour les prospects existants. De plus, une visite de la page par un visiteur non cookie entraîne la création et le stockage d’un nouveau cookie et la création d’un prospect anonyme dans votre base de données Marketo. Le Munchkin-tracker crée automatiquement un cookie pour un visiteur s’il ne dispose pas déjà d’un cookie pour le domaine actuel. Dans Marketo, il consigne l’événement (clic sur un lien, visite une page web ou un nouveau prospect) dans son journal d’activité. La valeur stockée dans le cookie est unique pour un visiteur donné. La valeur est une combinaison de l’identifiant de suivi de compte Munchkin unique, du nom de domaine, de l’horodatage et d’un entier aléatoire.
**Que se passe-t-il si j’ai plusieurs domaines ?** Supposons que vous souhaitiez suivre deux sites : `<www.apples.com>` et `<www.bananas.com>`. Vous pouvez placer le code de suivi sur les deux sites, mais vous devez tenir compte des points suivants. Les cookies Marketo sont des « cookies propriétaires » et sont donc spécifiques au domaine. Cela signifie qu’un visiteur du site 1 sera créé en tant que prospect anonyme dans Marketo. Si ce même prospect accède ensuite au site 2, un second prospect anonyme distinct sera créé dans Marketo. Si le prospect remplit un formulaire sur le site 1, cet enregistrement est connu, l’enregistrement anonyme pour le site 2 est conservé et continue à accumuler les visites ultérieures sur ce site. Si le prospect remplit ensuite un formulaire sur le site 2 avec la même adresse e-mail que celle utilisée sur le site 1, les deux prospects connus fusionneront automatiquement et tous les comportements passés et futurs seront suivis sur un seul enregistrement dans Marketo. Les deux ID de cookie sont liés au même prospect et toutes les activités web (des deux domaines) s’y trouvent.
**Qu’en est-il des sous-domaines multiples ?Les sous-domaines** ne posent pas de problème. Prenons l’exemple de Marketo.com. Il comporte plusieurs sous-domaines pour différentes langues, telles que fr.marketo.com et de.marketo.com. Avec les sous-domaines, toutes les activités sont enregistrées par rapport au même enregistrement/cookie de prospect.

Publié le _2015-01-13_ par _David_

## Modification de la couleur du texte d’indice sur un formulaire Marketo

Supposons que vous souhaitiez modifier la couleur du texte de l’indice (également appelé texte d’espace réservé) dans Forms 2.0. Cela est possible via le CSS personnalisé. Par exemple, dans la capture d’écran ci-dessous, j’ai bleu le texte de l’indice dans ce formulaire Marketo. Il existe trois options pour ce faire en fonction de la manière dont vous utilisez Marketo Forms.

**Option 1 : si vous incorporez un formulaire Marketo, ajoutez le CSS ci-dessous directement dans votre fichier CSS principal.**

```css
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
```

**Option 2 : lorsque vous incorporez un formulaire Marketo, vous pouvez ajouter le CSS directement sur la page entre `<style></style>` balises dans la section `<head>`.**

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

**Option 3 : si vous utilisez un formulaire Marketo sur une page de destination Marketo, vous pouvez ajouter ce CSS personnalisé via l’interface utilisateur de Marketo.** Recherchez la page de destination dans l’arborescence de navigation de Marketo. Cliquez ensuite sur Modifier le brouillon. Cliquez sur Modifier les balises de métadonnées de page. Ajoutez le code CSS ci-dessous à la section HTML HEAD personnalisé . Les balises `<style></style>` doivent être incluses.

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder {
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
</style>
```

Cliquez sur Approuver le brouillon. Lorsque vous accédez à présent à la page de destination de Marketo, le texte indicateur correspond à la couleur que vous avez définie dans le CSS. Pour plus d’informations sur Marketo Forms, [consultez la documentation](/help/javascript-api/forms-api-reference.md).

Publié le _2015-01-14_ par _Murta_

## Obtention des données d’activité via l’API REST

Supposons que vous souhaitiez obtenir tous les prospects qui ont été ajoutés à une liste ce mois-ci. À l’aide de l’API REST [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET), vous pouvez obtenir ces données. Avant d’appeler l’API Get Lead Activities, il est nécessaire d’obtenir un jeton d’accès de l’API d’authentification ainsi qu’un jeton de date de début de l’API [&#x200B; Get Paging Token API &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getActivitiesPagingTokenUsingGET). Vous trouverez ci-dessous un exemple de code dans Ruby qui décrit les points d’entrée d’API individuels que vous devez appeler pour renvoyer tous les prospects ajoutés à une liste ce mois-ci. 1. Obtention du jeton d’accès**

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com/identity/oauth/token?grant_type=client_credentials>"
# Relace with your client id
client_id = "99985d09-22a9-3jl2-84av-f5baae7c3a45"
# Replace with your your  client secret
client_secret = "tZPVrKiEmUDezE18yZfeaPlTJ2vKn2fw"
request_url = marketo_instance + "&client_id=" + client_id + "&client_secret=" + client_secret

# Make request
response = RestClient.get request_url

# Parse reponse and return only access token
results = JSON.parse(response.body)
access_token = results["access_token"]
puts access_token
```

1. Obtenir le jeton de pagination

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities/pagingtoken.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify date
since_date_time = "&sinceDatetime=2015-01-01T00:00:00-08:00"
request_url = marketo_instance + endpoint + auth_token + since_date_time

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Obtenir les données d’activité ** pour déterminer l’ID de type d’activité nécessaire pour cet appel, interrogez l’API [Gotten Activity Types](/help/rest-api/activities.md). L’API Get Activity Types renvoie un schéma avec tous les types d’activités et les identifiants associés. Par exemple, elle renvoie l’ID 12 pour les nouveaux prospects créés et l’ID 1 pour les visites sur la page web.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
# Specify activities needed
activity_type_ids = "&activityTypeIds=24"
request_url = marketo_instance + endpoint + auth_token + since_date_time + activity_type_ids

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. L’API Get Lead Activities renvoie un jeton de pagination avec chaque réponse que vous pouvez utiliser pour paginer dans le jeu de résultats.** Pour plus d’informations, consultez la documentation de l’API [REST](/help/rest-api/rest-api.md).

Publié le _2015-01-20_ par _Murta_

## Mise en surbrillance des projets Open Source créés sur la plateforme Marketo : deuxième partie

Il s’agit du deuxième article d’une série consacrée aux projets open source créés autour de la plateforme Marketo par la communauté des développeurs et développeuses. Nous conservons [une liste sur le compte GitHub de Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) où nous effectuons le suivi des bibliothèques clientes et des projets créés par la communauté des développeurs de Marketo. Vous trouverez ci-dessous trois projets développés autour des API Marketo SOAP et Munchkin. [PunchTab](https://www.punchtab.com/) créé [une bibliothèque cliente en Python pour l’API Marketo SOAP](https://github.com/PunchTab/suds-marketo). [Flickerbox](https://www.flickerbox.com/) créé [une bibliothèque cliente en PHP pour l&#39;API Marketo SOAP](https://github.com/flickerbox/marketo).* [Richard Morrison](https://x.com/mozz100) a créé [un script PHP pour obtenir les données de lead de l’API Marketo SOAP, puis les transmettre au client à l’aide de JavaScript.](https://github.com/mozz100/marketo-whodat) Ce projet peut vous aider à modifier une page en fonction des données d’un utilisateur dans Marketo.  Nous sommes ravis de voir d’autres projets créés par la communauté des développeurs sur la plateforme Marketo. Si vous travaillez sur un projet open source pour la plateforme Marketo, [envoyez-le à ce référentiel GitHub par le biais d’une demande d’extraction](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publié le _2015-01-20_ par _Murta_

## Envoyer des clics du moteur de recommandation RTP à Google Analytics

Voici une solution pour que les utilisateurs de Marketo Real-Time Personalization (RTP) voient les clics effectués par le moteur de recommandation de contenu dans Google Analytics. Lorsqu’un visiteur clique sur la barre de recommandations de contenu, un événement est envoyé à Google Analytics sous la catégorie d’événement « RTP-Recommendations ». Dans Analytics, le texte de recommandation (tel qu’il apparaît dans la barre) est ajouté au libellé de l’événement et l’URL de la ressource recommandée est ajoutée à l’action d’événement. Le script fonctionne pour Classic Google Analytics et Google Universal Analytics. Cette balise doit être collée à la fin du code de page d’HTML. Il s’agit donc de la dernière balise avant la balise `</body>`.

```javascript
$( document ).ready(function() {
if(document.getElementsByClassName("insightera-bar-content").length
 >0){
document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].addEventListener("click",
 function(){
assetName
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].innerText;
assetURL
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].href;
assetURL=
 assetURL.substring(assetURL.lastIndexOf("/"),assetURL.indexOf("?iesrc"));
console.log(assetName

 * " | " + assetURL);
if(typeof(_gaq)
 != "undefined"){
//Classic
_trackEvent("RTP-Recommendations",
 assetName , assetURL , {'nonInteraction': 1});
}else
 if(typeof(ga) !="undefined"){
//Universal
ga('send',
 'event',"RTP-Recommendations", assetName , assetURL, {'nonInteraction': 1});
}
});
}
});
```

Publié le _2015-01-22_ par _Yanir_

## Utilisation de l’API REST Marketo avec Boomi : obtention et stockage d’un jeton d’authentification REST

La configuration d’une exportation automatique de prospects répondant à un certain critère est un cas d’utilisation très courant avec Marketo. Bien que cela ne soit pas possible actuellement dans l’interface de Marketo, il est assez simple d’utiliser l’outil tiers comme Dell Boomi, une liste statique avec certaines campagnes de gestion des données et l’API REST Marketo. L’API REST ? Je pensais que Boomi n’avait pas de connecteur API REST Marketo. Eh bien, actuellement, ce n’est pas le cas, mais il est possible d’accomplir la même chose en utilisant le connecteur HTTP et en définissant manuellement les formes de réponse jSON. La première étape consiste à configurer votre instance Marketo pour utiliser l’API REST comme indiqué dans la page [API REST pour le développeur Marketo](/help/rest-api/rest-api.md). Je suppose également que vous avez accès à un compte Dell Boomi et que vous disposez des compétences Boomi nécessaires pour créer ce type de processus d&#39;intégration. Le processus final ressemble à ce qui suit et inclura des appels aux opérations de l’API REST Marketo suivantes, chacune d’elles ayant une forme de réponse jSON associée que l’on peut trouver sur le site du développeur. Pour gagner du temps, je les ai répertoriés ci-dessous : Exemple JSON pour [Authentification](/help/rest-api/authentication.md)

```json
{
  "access_token": "",
  "token_type": "",
  "expires_in": 0,
  "scope": ""
}
```

Exemple de fichier JSON pour [Obtenir plusieurs prospects par ID de liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Exemple JSON pour [Supprimer des prospects de la liste](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

Définir les propriétés : avant de commencer à appeler REST, il est important d’externaliser et d’encapsuler les variables que vous utilisez. J’ai défini les éléments suivants comme illustré ci-dessous.

* ClientID : à obtenir à partir de votre service REST Launchpoint
* Secret client : obtenez-le à partir de votre service REST Launchpoint
* Jeton d’accès : nous l’obtenons à partir d’un appel REST
* Static ListID : ID de liste de la liste statique sur laquelle nous opérerons. Récupérez-la à partir de l’URL dans Marketo
* Champs : liste de champs séparés par des virgules que le service REST obtient de Marketo pour chaque prospect. La mienne est « id, email, firstName, lastName » * IDStringToDelete : contiendra éventuellement l&#39;identifiant de tous les leads de la liste statique à utiliser pour leur suppression de la liste
* ActivityTypes : Sera utilisé dans la Partie 2 de ce blog, où je développe ce sujet !
* SinceDateTime : Sera utilisé dans la Partie 2 de ce blog, où je développe ce sujet !
* PagingToken : sera utilisé dans la Partie 2 de ce blog, où j&#39;en parle plus en détail !
* Dossier - Sortant : le chemin d’accès au dossier sortant sur le serveur SFTP. Dans cet exemple, j’utilise « /data/outcoming ». Cela nous permet de paramétrer l’opération SFTP pour la rendre générique.

Le jeton d&#39;authentification : Comme je l&#39;ai mentionné, nous placerons un connecteur sur la zone de travail après avoir créé le processus avec une forme de début « Aucune donnée » (il s&#39;agit simplement d&#39;un choix personnel, j&#39;aime tous mes connecteurs qui ressemblent à des prises britanniques).
Le connecteur doit être configuré comme suit : - Le connecteur est un client HTTP GET - La connexion utilise l’URL : `https://123-ABC-456.mktorest.com` (notez qu’il n’y a pas de chaîne /rest à la fin, afin que nous puissions l’utiliser pour les appels REST et pour obtenir le jeton d’accès à l’identité. et modifiez le numéro 123-ABC-456 sur celui qui convient à votre instance Marketo) - L’opération est « Get oAuth Token » (nouveau !) - Profil de requête = Aucun - Profil de réponse = JSON - Nouveau profil appelé « Authentication Token Response » - Type de contenu : Brut - Méthode HTTP : GET - Chemin de ressource (ajoutez 4 sans guillemets) : « identity/oAuth/token?grant_type=client_credentials&amp;client_id= »; « ClientID (Variable de remplacement) »; «&amp;client_secret= »; « ClientSecret (Variable de remplacement) » - Définissez les paramètres sous Configurer -> Paramètres —>(+) : définissez ClientID = ID client de la propriété du processus ; définissez ClientSecret = Secret client de la propriété du processus Ensuite, stockez le jeton de succès dans la variable « AccessToken » des propriétés du processus, comme indiqué, en l’extrayant de la réponse jSON.
Le modèle de cette étape sera répété pour les étapes suivantes, mais en utilisant de nouvelles opérations avec différents profils de retour jSON. En fait, de nombreux appels REST seront traités de la même manière avec des modifications mineures ! Dans la prochaine partie, nous développerons ce sujet et obtiendrons une liste de prospects à partir d’une liste statique à l’aide de REST ! Pour l’instant, exécutez le processus, mais placez une forme d’arrêt après votre « Définition des propriétés », puis exécutez dans debug pour vous assurer de voir le même jeton que celui que vous voyez dans Marketo. Ils devraient parfaitement s&#39;accorder !

Publié le _2015-01-26_ par _John_

## Utilisation d’une API de police Google pour ajouter une police personnalisée à une page de destination Marketo

**Note : Voici un billet de blog de [Murtza Manzur](https://www.linkedin.com/in/murtzam). Murtza est une développeuse évangéliste de Marketo basée dans la région de la baie de San Francisco.**
Supposons que vous êtes en train de créer une page de destination dans Marketo et que vous souhaitiez utiliser une police personnalisée. Pour ce faire, utilisez l’API de polices Google.  Ajoutez une méthode d’importation à votre fichier CSS avec la référence aux polices Google :

`@import url(http://fonts.googleapis.com/css?family=Open+Sans:400,300,600);`

Publié le _2015-01-26_ par _Murta_

## Mettre le prénom d’un prospect en majuscule à l’aide d’un script de messagerie

Supposons qu’un prospect saisisse son nom en minuscules, par exemple « John doe ». Cependant, lorsque vous envoyez une campagne par e-mail, vous souhaitez mettre en majuscules le nom du prospect dans l’e-mail, comme John Doe. Vous pouvez mettre le nom d’un prospect en majuscule à l’aide d’un script de messagerie. Voici comment procéder.

1. Dans votre programme de messagerie, cliquez sur l’onglet « Mes jetons ».
1. Créez un jeton de script de messagerie en faisant glisser « Script de messagerie » du panneau de droite vers le panneau du milieu. Nommez le jeton .
1. Dans la zone de texte Modifier le jeton de script , collez le code ci-dessous. Dans le panneau de droite, sous Objet de prospect, cochez la case Prénom . Cliquez ensuite sur Enregistrer.

```javascript
# set($name = ${lead.FirstName})
# set($formattedFirstName = $name.substring(0).toUpperCase())
$formattedFirstName
```

1. Référencez le jeton dans la ressource e-mail. Il génère le prénom du prospect avec la première lettre en majuscule. Pour plus d’informations sur les scripts d’e-mail, consultez la [documentation sur les scripts d’e-mail](/help/email-scripting.md).

Publié le _2015-01-26_ par _Murta_

## Obtenir tous les leads à partir de l’API REST Marketo

Une [&#x200B; question se posait sur StackOverflow pour savoir comment obtenir une liste de tous les prospects provenant de Marketo via l’API REST](https://stackoverflow.com/questions/28184900/how-do-i-get-the-list-of-all-the-leads-in-marketo). Vous pouvez interroger ces données à l’aide du point d’entrée [API REST Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Dans Marketo, les leads se voient attribuer des ID de lead dans l’ordre séquentiel commençant par 1. À l’aide du point d’entrée de l’API REST [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), vous pouvez interroger 300 prospects par ID de prospect avec chaque appel. Vous devez spécifier id comme filterType et les id de prospect comme filterValues avec chaque appel à ce point d’entrée. Pour obtenir tous les prospects, vous devez effectuer une itération sur le nombre total de prospects 300 à la fois. Y
Vous pouvez obtenir le nombre total de prospects dans une instance Marketo via l’interface utilisateur de Marketo. Dans l’interface utilisateur de Marketo, accédez à l’onglet Base de données de leads, cliquez sur Listes dynamiques système, cliquez sur Liste dynamique tous les leads, puis enfin cliquez sur l’onglet « Leads ». Cliquez ensuite sur la colonne Id et effectuez un tri décroissant. Une fois les prospects triés, l’ID du premier prospect est la limite supérieure de l’ID des prospects lorsque vous interrogez tous les prospects. Si vous n’avez pas accès à l’interface utilisateur de Marketo pour obtenir le nombre total de prospects, il existe une [&#x200B; autre approche pour obtenir cette valeur à l’aide de l’API REST Obtenir les activités de prospect &#x200B;](https://stackoverflow.com/questions/28419967/get-all-leads-programmatically-in-marketo-v1).

1. Premier appel API : remplacez ... par toutes les valeurs comprises entre :

`/rest/v1/leads.json?filterType=Id&filterValues=1,2,3,...,298,299,300`

Voici un exemple de code en Ruby pour le premier appel.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Replace with filter type and values
ids_needed = (1..300).to_a.join(",")
filter_type_and_values = "&filterType=Id&filterValues=" + ids_needed
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Le deuxième appel API et chaque appel API suivant suivent le même modèle jusqu’à ce que le nombre total de prospects soit atteint :

```
//replace ... with all the values in between
/rest/v1/leads.json?filterType=Id&filterValues=301,302,303,...,598,599,600
```

Pour plus d’informations, consultez la [documentation de l’API REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).

Publié le _2015-01-28_ par _Murta_

## Exécuter des actions d’envoi de formulaire d’Iframe vers la page parent

Nous avons rencontré quelques cas où des utilisateurs et utilisatrices utilisaient des formulaires iFrame et souhaitaient rediriger les visiteurs et visiteuses ayant rempli le formulaire vers une page de remerciement ou un PDF, une vidéo, etc. Le problème est que, puisque le formulaire est incorporé à une page de destination différente de la page parente, l’action ne se produit que sur la page interne où se trouve le formulaire. Pour résoudre ce problème, vous trouverez ci-dessous les 2 balises JavaScript que nous avons créées. Insérez dans en tant qu’élément HTML dans vos pages iframe ou directement dans le modèle de page de destination que vous utilisez pour les iframes. Placez-le avant la dernière balise `</body>`. La première balise effectue l’action sur la page parente et la seconde balise l’ouvre dans un nouvel onglet.

**Action de formulaire sur une page parent**

```javascript
<script>
MktoForms2.whenReady(function (form){
form.onSuccess(function (values, url){
window.parent.location.assign(url);
return false;
           });
});
</script>
```

**Action de formulaire dans un nouvel onglet**

```javascript
<script>
MktoForms2.whenReady(function (form){
var newWin;
form.onSubmit(function (){
newWin = window.open('about:blank', 'myWindow');
});
form.onSuccess(function (values, url){
newWin.location.replace(url);
return
 false;
});
});
</script>
```

Publié le _2015-02-02_ par _Yanir_

## Envoi de données de vue d’une vidéo YouTube au marché

Supposons que vous souhaitiez segmenter les prospects dans Marketo selon qu’ils ont commencé ou terminé une vidéo spécifique. Il est possible d’effectuer cette opération à l’aide de Munchkin, de l’API Iframe de YouTube et de listes dynamiques dans Marketo. L’exemple de code de cette publication vous permet d’envoyer des événements vidéo lancée et vidéo terminée dans Marketo via Munchkin. Pour que cela fonctionne, Munchkin doit également être chargé sur la page avant de pouvoir commencer à envoyer des événements de vue vidéo dans Marketo. La vidéo commencée et terminée s’affiche dans le journal d’activité du prospect. Une fois que les données se trouvent dans Marketo, vous pouvez créer une liste dynamique et des prospects de segments qui ont commencé ou terminé une vidéo.

1. Obtenez l’identifiant de la vidéo YouTube que vous souhaitez incorporer.** Dans l’URL de la vidéo YouTube que vous souhaitez utiliser, notez l’identifiant, qui correspond à la série de caractères aléatoires après `v=`.
1. Placez l’identifiant vidéo YouTube de l’étape 1 à la huitième ligne de cet exemple de code. Placez ensuite le code avant le `</body>` dans l’HTML de votre page.

```javascript
<div id="player"></div>
<script>
var tag = document.createElement('script');
tag.src = "https://www.youtube.com/iframe_api";
document.getElementsByTagName('head')[0].appendChild(tag);

//Change 'iiqxcjxJ5Us' to video needed
var player, videoId = 'iiqxcjxJ5Us';
function onYouTubeIframeAPIReady() {
player = new YT.Player('player', {
height: '390',
width: '640',
videoId: videoId,
events: {
'onStateChange': onPlayerStateChange
}
});
}

function onPlayerStateChange(event) {
switch( event.data ) {
//Send video started event to Marketo
case YT.PlayerState.PLAYING: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=started'
}
);
break;
//Send video finished event to Marketo
case YT.PlayerState.ENDED: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=finished'
}
);
break;
}

}
</script>
```

1. Créez une liste dynamique dans Marketo avec l’URL de la vidéo et l’événement d’affichage que vous recherchez comme valeur de « Querystring contains ». Pour plus d’informations sur l’API YouTube Iframe, [consultez la documentation de l’API YouTube](https://developers.google.com/youtube/iframe_api_reference). Pour plus d’informations sur Munchkin, [consultez la documentation Marketo destinée aux développeurs](/help/javascript-api/lead-tracking.md).

Publié le _2015-02-02_ par _Murta_

## Conseils et astuces concernant l’API Marketo SOAP

REMARQUE : Ceci est un article de blog d&#39;invité. [Ed Blachman est architecte principal](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D2777965) chez [TIBCO Software, un fournisseur bien connu de logiciels d&#39;entreprise](https://exchange.adobe.com/apps/browse/ec?product=MRKTO). Ed travaille sur des produits qui permettent à ce que Gartner appelle des « développeurs citoyens » d’intégrer les services cloud qu’ils utilisent sans avoir à effectuer eux-mêmes de programmation. [L’API Marketo SOAP](/help/soap-api/soap-api.md) est un outil puissant grâce auquel les développeurs peuvent exploiter la puissance de Marketo et l’intégrer à nos propres applications. Entre [la documentation officielle](./getting-started.md) et [les ressources communautaires](https://nation.marketo.com/), il y a beaucoup d&#39;information disponible sur la façon de l&#39;utiliser. Quand j&#39;ai commencé, je me suis beaucoup appuyé sur cette information et je l&#39;ai trouvée précieuse. Cependant, au cours de ce processus, j&#39;ai élaboré des conseils et des astuces que je n&#39;avais pas vus dans ces endroits. Voici ce que j&#39;ai compris.

**Le sandbox des développeurs** Le sandbox est, bien sûr, une merveilleuse ressource pour les développeurs d’API : un endroit sûr où vous pouvez tester les fonctionnalités de Marketo, ajouter et supprimer des objets sans interférer avec les activités marketing réelles menées par les utilisateurs réels de Marketo de votre organisation. Cependant, le Sandbox n’est pas une panacée.
Par exemple, je devais partager notre Sandbox avec un autre groupe de développement, ce qui a pris du temps, car ils s’étaient habitués à l’idée qu’ils possédaient le Sandbox. Nous avons fini par identifier quelques bonnes pratiques de partage : - N’écrivez pas de tests qui dépendent d’une connaissance complète du contenu de votre sandbox. En tant que ressource partagée, les schémas peuvent faire l’objet de modifications sans préavis, ainsi que d’entrées entières dans votre base de données de prospects, vos programmes ou d’autres entités. Si vos tests supposent une connaissance complète du Sandbox, votre cycle de développement crée des périodes d’interruption pour les groupes avec lesquels vous le partagez. Comme généralement leur cycle de développement ne coïncide pas avec le vôtre, cela revient à encombrer la ressource, et non à la refroidir. Ce n&#39;est pas non plus nécessaire, si vous y réfléchissez bien. - Utilisez une convention pour étiqueter tous vos éléments - vos prospects, vos champs de schéma de prospect, vos programmes, etc. Si vous pouvez chacun identifier vos propres objets, et si vous pouvez vous mettre d&#39;accord avec vos co-locataires sur le fait que chacun d&#39;entre vous laissera les objets des autres seuls, vous devriez être sur une base solide pour le partage. Pour les prospects, vous pouvez créer un champ personnalisé, ainsi qu’une convention à l’aide de ce champ personnalisé pour identifier ces prospects en tant que prospects de test. Pour les listes ou les programmes, vous pouvez commencer les noms de vos objets par une chaîne qui identifie ces objets comme vous appartenant. - Pensez à écrire des tests qui se nettoient après eux-mêmes, qui créent d’abord les objets qui vous intéressent, puis accèdent à ces objets, les mettent à jour ou les suppriment de manière sélective, et enfin les suppriment. (Notez que cela n’est pas réalisable à 100 % dans l’API SOAP, car tout ce qui se trouve dans le sandbox ou dans une instance réelle ne peut pas être géré via l’API SOAP. Même ainsi, il est toujours utile de faire cela autant que vous le pouvez.)

**Instances réelles** le problème avec le sandbox est qu’il n’est pas utilisé en production. Il est donc difficile d’avoir une idée de l’utilisation réelle dans une instance Marketo. Maintenant, si vous avez la chance d’avoir un utilisateur Marketo expérimenté dans votre équipe ou si vous effectuez un développement sur mesure pour les utilisateurs Marketo internes, le problème n’est pas si grave. Mais dans le cas de mon équipe, c&#39;était une grosse affaire. Aucun d&#39;entre nous n&#39;était expert en Marketo, et comme on nous demandait de comprendre un grand nombre de services cloud, nous n&#39;avions tout simplement pas les effectifs nécessaires pour devenir experts de quoi que ce soit. Voici quelques-unes des informations que nous avons recueillies lors de l’accès à une instance réelle : - Schémas de leads volumineux. Le schéma de prospect dans l’instance de production à laquelle nous avons accédé comporte plus de 200 champs. Cela a clairement montré à nos concepteurs d’IU que l’IU qu’ils concevaient devait s’adapter à des schémas de cette taille (ou plus grands). - Utilisation en rafale. Nous avons constaté une différence de deux ordres de grandeur entre les temps d’utilisation les plus élevés et les temps d’utilisation faible (en termes de nombre de prospects créés ou mis à jour). Cela avait un impact à la fois sur le volume de données que nous recevions des appels API (évident) et sur le temps de réponse d’un appel API (peut-être moins évident).

**Temps de réponse de l’appel API** Selon l’heure de la journée, les détails de votre appel API et le contenu de votre instance, vous pouvez constater que le temps de réponse de l’API SOAP est plus long que la moyenne. À l’occasion, nous recevions des appels d’API auxquels il fallait une minute et demie pour répondre. Vous devez être conscient de la possibilité de le traiter : - Test. Peut-être que ce n&#39;est pas un problème pour votre utilisation. Mais ne supposez pas ça, faites des tests. - Ajustez votre utilisation. Dans notre cas, le plus gros problème était que nous avons défini la taille de page de nos appels à [getMultipleLeads](/help/soap-api/getmultipleleads.md) pour qu’elle soit aussi grande que le permet l’API. Dans notre contexte, cela semble logique, car notre objectif est d’être le plus efficace possible avec le quota d’API de notre client. Mais dans votre contexte, vous n’avez peut-être pas à vous soucier autant des quotas d’appels API de vos utilisateurs, auquel cas vous obtiendrez certainement un meilleur temps de réponse en demandant des pages de données plus petites.

**Partitionnement des leads** Marketo fournit de puissants outils, des partitions et des espaces de travail, qui permettent à plusieurs groupes marketing de partager une seule instance Marketo. Toutefois, ces outils ne sont pas directement répercutés dans l’API SOAP. Par exemple, lorsque vous utilisez getMultipleLeads pour obtenir tous les prospects qui ont été mis à jour ou créés depuis une certaine date-heure, vous récupérez tous les prospects de votre instance pour lesquels c’est le cas, sans tenir compte de (et sans rien indiquer) quelle partition ou espace de travail contient un prospect donné. La création de prospects et l’ajout de prospects aux listes sont d’autres contextes dans lesquels le partitionnement des prospects peut avoir un impact sur le fonctionnement réel de vos appels API. Notez que cela signifie que les partitions et les espaces de travail peuvent ne pas être la solution dont vous avez besoin pour résoudre le problème de partage de sandbox discuté ci-dessus. Alors, comment déterminer si c&#39;est un problème pour vous ? J&#39;ai trouvé tous ces éléments utiles : les évangélistes développeurs sont engagés envers notre succès dans l&#39;utilisation des API, et lorsqu&#39;il y a des questions, ils sont incroyablement bons pour travailler à trouver des réponses. - [&#x200B; Documentation de l’API &#x200B;](./getting-started.md). Les évangélistes ont déjà abordé cette question dans une partie de la documentation, et dans le cadre de leur engagement envers notre succès, ils sont très bons pour mettre à jour le document. - Vos Propres Cas De Test. Bien que l’utilisation de partitions et d’espaces de travail pour partager le sandbox ne soit pas une bonne idée, le sandbox est un excellent endroit pour jouer avec les partitions et les espaces de travail afin de déterminer s’ils posent des défis pour l’utilisation prévue. (C&#39;est aussi une bonne façon de préciser vos questions pour les évangélistes, qui sont toujours une bonne idée.)

**TIMTOWTDI et tests** « Il existe plusieurs façons de procéder » : la devise de programmation Perl s’applique en fait à l’API Marketo SOAP dans certains cas. Par exemple, je souhaitais combiner la mise à jour d’un ensemble de prospects et l’ajout de ces prospects à une liste. Pour ce faire, l’API SOAP vous propose deux méthodes : 1. [importToList](/help/soap-api/importtolist.md) + [getImportToListStatus](/help/soap-api/getimporttoliststatus.md). À la lecture de la documentation, il s’agit évidemment de la méthode « normale » pour le faire. Cependant, le fait que vous deviez interroger les responsables pour connaître l&#39;état de vos opérations d&#39;importation m&#39;a donné un signal d&#39;alarme. Était-ce vraiment la façon dont je voulais mettre en œuvre mon import ? 1. [syncMultipleLeads](/help/soap-api/syncmultipleleads.md) + [listOperation](/help/soap-api/listoperation.md). Cela semble beaucoup moins élégant qu&#39;un appel importToList unitaire, mais cela ne dépend pas d&#39;un sondage. Était-ce une option viable? Les évangélistes ont du mal à s&#39;occuper de cas comme ceux-là, parce qu&#39;ils dépendent vraiment de la nature des cas que vous traitez et de ce que vous essayez de faire exactement. Heureusement, si vous avez configuré un environnement de test unitaire robuste, vous devriez pouvoir l’utiliser pour explorer des questions comme celles-ci également. Dans ce cas particulier, il s’est avéré que l’option 2 était meilleure pour mon cas d’utilisation que l’option 1, non pas en raison du sondage, mais plutôt parce que j’ai rencontré des limitations orientées champ sur importToList et également parce que j’essayais d’écrire du code qui pouvait être utilisé dans des contextes et des instances sur lesquels je n’avais aucun contrôle. Cependant, votre cas d’utilisation peut être différent, et les tests sont le seul moyen de le découvrir.

**Conclusion** Je ne pense pas que tout ceci soit un grand secret. D&#39;un autre côté, j&#39;aurais été en avance si j&#39;avais su tout ça avant de commencer. J&#39;espère que vous la trouverez utile.

Publié le _2015-02-05_ par _David_

## Utilisation de l’API REST Marketo avec Boomi : obtention et suppression de leads à partir de listes statiques

Dans la partie 1 de cette série, j’ai expliqué comment il était possible de commencer à utiliser l’API REST via Boomi avec le connecteur HTTP Boomi, en obtenant spécifiquement le jeton d’authentification nécessaire pour accéder à l’API REST et en le stockant dans une variable de processus. Ensuite, nous allons commencer à lancer des appels dans Marketo. Dans cette partie, je vous montre comment vous pouvez [Obtenir plusieurs prospects par ID de liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists) et [Supprimer des prospects de la liste](/help/rest-api/lead-database.md). Portez une attention particulière à la suppression des prospects d&#39;une liste parce qu&#39;il y a un aspect très « légèrement documenté » et subtil de Boomi à l&#39;œuvre là-bas que je développe lorsque nous y arrivons.

Dans la prochaine partie, nous développerons cette fonctionnalité pour commencer à faire des choses intéressantes, comme obtenir une activité de prospect, mais ce blog vous attend pour un autre jour. Pour cette partie, nous examinerons les deuxième et troisième domaines mis en évidence. À titre d’examen, j’ai inclus ci-dessous les réponses JSON dont nous avons besoin. Rappelez-vous que pour créer un profil JSON dans Boomi, il suffit de créer un composant de profil de type JSON, puis de cliquer sur « importer » et de sélectionner le fichier. Boomi fait le reste, en extrapolant des éléments comme s’il devait y avoir plusieurs identifiants autorisés. Exemple de fichier JSON pour [Obtenir plusieurs prospects par ID de liste](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Exemple de fichier JSON pour [Supprimer des prospects de la demande de liste](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
   "input":[
      {
         "id": ""
      },
      {
         "id": ""
      }
   ]
}
```

Exemple JSON pour [Supprimer des prospects de la réponse de la liste](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

L’identifiant Get Multiple Leads by List ** déposez un autre connecteur (Get) dans votre processus, en utilisant la même connexion que celle définie dans l’article précédent. Créez une opération appelée « Get Multiple Leads by List ID » (Je suis un avis de cohérence). Ses attributs sont les suivants : - Request Profile : None (cette opération utilise l’URL de requête) - Response Profile : jSON - Response Profile : Créez un nouveau profil basé sur la réponse Get Multiple Leads by List ID ci-dessus. Notez que vous pouvez la modifier afin qu’elle renvoie les champs de votre choix, et pas seulement ceux répertoriés. Il est important de se rappeler que le profil de réponse JSON doit réellement correspondre à la liste des champs que vous demandez à l’API REST et que vous ne devez demander que les champs dont vous avez besoin. Dans l’objet Propriétés du processus , nous avons défini une propriété appelée « fields » qui est une liste séparée par des virgules des champs que vous souhaitez que REST renvoie. et c&#39;est la liste qui doit correspondre au profil. Type de contenu : text/plain (il s’agit uniquement d’une requête d’URL) Méthode HTTP : GET (vous le recherchez dans les documents de l’API REST, il est toujours répertorié) Chemin de la ressource (ajoutez 5) rest/v1/list/ listID (variable de remplacement) /leads.json?access_token= access_token (variable de remplacement) &amp;fields= fields (variable de remplacement). Ensuite, dans l’onglet paramètres du connecteur, vous pouvez saisir les valeurs de variable, qui ont toutes été précédemment renseignées dans les propriétés du processus. Dans la section suivante, je parlerai de la manière d’éviter de les remplir manuellement. Je vais ignorer la partie du processus où je mappe la réponse pour Obtenir plusieurs leads par ID de liste dans un profil de fichier plat et la coller sur un serveur FTP parce que c’est une fonctionnalité Boomi simple.

Pour que celui-ci soit intéressant, un de mes collègues, [Ken Niwa](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D7429494) m&#39;a appris cette technique suivante et c&#39;est plutôt cool et basé sur un article de Boomi intitulé « How to Build a POST Request for a RESTful Application », illustré ci-dessous.  ...mais d&#39;abord les choses. Dans le processus, en sortant de la forme « Obtenir plusieurs leads par ID de liste », nous avons la réponse Obtenir plusieurs leads par ID de liste , et nous devons la mapper dans la « Demande de suppression de leads de liste » pour que le mappage soit assez simple, en mappant simplement l&#39;identifiant que nous avons obtenu des leads dans la liste d&#39;origine dans la liste d&#39;identifiants que nous transmettons dans le jSON de suppression. Ensuite, déposez un autre connecteur avec une action « Envoyer », en utilisant la même connexion Créez une nouvelle opération appelée « Supprimer les prospects de la demande de liste ». dont les attributs sont Profil de requête : type de contenu jSON : profil de requête application/json : [profil JSON] supprimer les leads de la requête de liste (créée à partir du fichier ci-dessus) Type de profil de réponse : profil de réponse jSON : [profil JSON] supprimer les leads de la réponse de liste (créée à partir du fichier ci-dessus) Type de contenu : application/json Méthode HTTP : chemin de ressource DELETE (ajouter 4) rest/v1/lists/ listID (variable de remplacement) /leads.json?access_token= access_token (variable de remplacement) Voici ce qui est intéressant à propos de ce connecteur. Nous n’allons PAS ajouter explicitement les paramètres dans l’onglet Connecteur . Comme l’indique l’article, nous créons à la place des propriétés de document dynamique portant le même nom que les variables de remplacement. Dans ce cas, ces variables listID et access_token. Lorsque vous procédez de la sorte, la forme jSON s’écoule dans l’appel REST et les paramètres apparaissent à leur emplacement approprié sur l’URL. Nous ne pouvons pas effectuer cette opération avec l’appel précédent, car il s’agit d’un GET et non d’un POST. À ce stade, vous avez donc vu un appel API REST GET et POST et vous pouvez commencer à voir le modèle utilisé pour effectuer ces appels REST. Dans la prochaine partie, nous allons commencer à examiner l’exportation de l’activité de lead par le biais de l’API REST, qui est un peu plus impliquée.

Publié le _2015-02-06_ par _John_

## Intégration d’une vidéo YouTube avec suivi des leads sur une page de destination Marketo

Dans un article de blog précédent, j’ai décrit comment segmenter les prospects dans Marketo en fonction de la vidéo spécifique à YouTube qu’ils ont démarrée ou terminée. Dans cet article de blog, nous expliquons comment utiliser l’implémentation à partir de cet article et l’utiliser sur une page de destination Marketo.

1. Accédez au Programme dans Marketo où vous souhaitez créer la nouvelle page de destination. Cliquez sur Nouvelle ressource locale, puis sur Page de destination.
1. Nommez la page de destination. Attribuez une URL de page. Sélectionnez un modèle. Cliquez ensuite sur Créer.
1. Une fois la page de destination créée, cliquez sur Modifier le brouillon.
1. Dans le panneau de droite, faites glisser le bouton HTML vers la zone de travail principale à gauche.
1. Dans la zone Éditeur HTML personnalisé qui s’affiche. Cliquez ensuite sur Enregistrer.
1. Ajustez la taille d’un élément HTML en faisant glisser le contour de la boîte. Cliquez ensuite sur Approuver et fermer .
1. Testez la version active de la page de destination en cliquant sur Afficher la page approuvée . Une page de destination contenant le YouTube s’ouvre dans une nouvelle fenêtre. La vidéo commencée et terminée s’affichera dans le journal d’activité du prospect, comme illustré dans la première et la deuxième capture d’écran ci-dessous. Une fois que les données se trouvent dans Marketo, vous pouvez créer une liste dynamique et des prospects de segments qui ont commencé ou terminé une vidéo, comme illustré dans la capture d’écran ci-dessous. Pour plus d’informations sur l’API YouTube Iframe, [consultez la documentation de l’API YouTube](https://developers.google.com/youtube/iframe_api_reference). Pour plus d’informations sur Munchkin, [consultez la documentation destinée aux développeurs de Marketo](/help/javascript-api/lead-tracking.md).

Publié le _2015-02-09_ par _Murta_

## Suivi web des applications monopages avec Munchkin

Une application monopage est un site web qui charge toutes les ressources nécessaires pour naviguer sur le site au premier chargement de la page. Lorsqu’un utilisateur clique sur un lien, le contenu est chargé à partir des données de chargement de la première page. Pour l’utilisateur, le site web se comporte comme prévu, car l’URL figurant dans la barre d’adresse est similaire à la navigation traditionnelle sur la page. Munchkin fonctionne bien avec les sites web traditionnels, car Munchkin s’exécute chaque fois que les utilisateurs chargent une nouvelle page. Cependant, avec une application sur une seule page, si vous ne chargez pas de nouvelle page, Munchkin s’exécutera une seule fois. L’approche que je découvre dans cet article consiste à suivre le moment où un utilisateur clique sur un lien, puis à envoyer ces informations à Munchkin. Nous mettons en œuvre cette fonction à l’aide de la fonction `clickLink` Munchkin. Vous trouverez ci-dessous un exemple d’implémentation dans jQuery qui lie les événements for click à la méthode Munchkin `clickLink`. Lors de l’appel de la méthode `clickLink` Munchkin, il transmet le paramètre de l’URL sur laquelle l’utilisateur a cliqué.

```javascript
<script>
$("a").on('click', function(event) {
    var urlThatWasClicked = $(this).attr('href');
    Munchkin.munchkinFunction('clickLink', { href: urlThatWasClicked});
});
</script>
```

Publié le _2015-02-11_ par _Murta_

## Modifier le score d’un prospect via l’API REST

Supposons que vous souhaitiez modifier le score d’un prospect dans Marketo à l’aide des API. Cela est possible avec l’API REST à l’aide du point d’entrée Créer/Mettre à jour un prospect. Vous trouverez ci-dessous un exemple de code dans Ruby qui montre comment effectuer cet appel.

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "https://AAA-BBB-CCC.mktorest.com"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] }

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Dans le corps JSON de la requête, nous spécifions `updateOnly` comme action. Cela signifie que la requête ne fonctionnera que si le prospect existe, sinon elle échouera. Si vous souhaitez créer un prospect s’il n’existe pas, indiquez `createOrUpdate` comme action. Nous utilisons l’adresse e-mail du prospect comme identifiant principal pour rechercher l’enregistrement du prospect dans Marketo. Enfin, nous spécifions la valeur du score du prospect à l’aide de la `leadScore` de clé . Il est possible de mettre à jour 300 prospects à la fois à l’aide de cette méthode.

Publié le _2015-02-19_ par _Murta_

## Mise en évidence des projets Open Source créés sur la plateforme Marketo : troisième partie

Il s’agit du troisième article d’une série consacrée aux projets open source créés autour de la plateforme Marketo par la communauté des développeurs et développeuses. Nous conservons [une liste sur le compte GitHub de Marketo](https://github.com/Marketo/Community-Supported-Client-Libraries) où nous effectuons le suivi des bibliothèques clientes et des projets créés par la communauté des développeurs de Marketo. Vous trouverez ci-dessous trois projets développés autour des API REST Marketo. **[Usermind](http://www.usermind.com/) a créé [une bibliothèque cliente Node.js pour l’API REST Marketo](https://github.com/MadKudu/node-marketo).** **[Arunim Samat](https://github.com/asamat) a créé [une bibliothèque cliente en Python pour l’API REST Marketo](https://github.com/asamat/python_marketo).** **[Jacques Lemieux de Marketo](https://www.linkedin.com/in/jalemieux) a créé [une bibliothèque cliente dans Ruby pour l’API REST Marketo.](https://github.com/jalemieux/mkto_rest)** Nous sommes ravis de voir d’autres projets créés par la communauté des développeurs sur la plateforme Marketo. Si vous travaillez sur un projet open source pour la plateforme Marketo, [envoyez-le à ce référentiel GitHub par le biais d’une demande d’extraction](https://github.com/Marketo/Community-Supported-Client-Libraries).

Publié le _2015-02-20_ par _Murta_

## Insertion d’un formulaire Marketo dans une campagne RTP

De nombreux professionnels du marketing souhaitent placer un formulaire Marketo dans une campagne Marketo Real-Time Personalization (RTP). Qu&#39;il s&#39;agisse d&#39;une boîte de dialogue, d&#39;un type de campagne RTP Zone ou Widget, vous pouvez copier votre code Form HTML et le coller dans l&#39;éditeur de campagne de RTP. J’ai vu ces exemples d’utilisation : - Obtenir des visiteurs qu’ils s’inscrivent à votre newsletter après un 2e ou 3e clic sur votre site - Formulaire d’inscription rapide et efficace pour les webinaires - Télécharger une étude de cas sécurisée - Offrir des leads qui se sont désabonnés par le passé pour se réinscrire Remplissez un formulaire dans la campagne et recevez les remerciements ou le contenu demandé, ce qui entraîne un clic de moins pour atteindre vos objectifs. Voici donc l’explication de la procédure à suivre et de l’intégration d’un Marketo Form 2.0 dans une campagne RTP Marketo. Voici un excellent exemple de la part des utilisateurs d’eMarketer qui, au lieu d’orienter les visiteurs vers une page de remerciement, ont décidé d’afficher un message de remerciement dans le cadre de la campagne RTP. Le code de cette option est également ci-dessous. Profitez-en et je suis heureux d&#39;entendre parler de votre expérience !

1. Clic droit sur un formulaire approuvé. Sélectionnez **Code incorporé.**
1. Copiez le **Code.**
1. Dans le RTP Marketo, accédez à **Campagnes**.
1. Cliquez sur **CRÉER UNE CAMPAGNE**.
1. Dans l’éditeur de texte enrichi, cliquez sur l’icône **HTML**.
1. Collez le code incorporé du formulaire dans l’éditeur Source d’HTML. Cliquez sur **Mettre à jour**.
1. Le formulaire ne s’affichera pas en mode Éditeur, mais vous pouvez le prévisualiser pour voir comment il s’affichera dans une campagne.
1. Cliquez sur **Lancer** pour démarrer la campagne.

### Remarque

Toute modification apportée au formulaire doit être effectuée dans les Activités marketing de Marketo dans Modifier le brouillon du formulaire.

### Articles connexes

* [Formulaires 2.0](/help/javascript-api/forms-api-reference.md)

Publié le _2015-12-20_ par _Yanir_

## Ajout d’un bouton Réinitialiser à un formulaire Marketo

```javascript
<script src="//app-sj01.marketo.com/js/forms2/js/forms2.min.js"></script>
<form id="mktoForm_116"></form>
<script>MktoForms2.loadForm("//app-sj01.marketo.com", "410-XOR-673", 116,
function(form) { form.getFormElem()[0].querySelector('button[type="submit"]').insertAdjacentHTML('afterend','<button type="reset" class="mktoButton">Reset</button>') });
</script>
```

Publié le _2015-03-18_ par _Murta_

## Mises À Jour De Mars 2015

L’API de ressources REST [Marketo a été publiée dans la version de mars 2015](https://developer.adobe.com/marketo-apis/api/asset/). Cette API permet d’accéder aux objets de modèle de fichier, de dossier, de jeton, d’e-mail et d’e-mail Marketo. Notez que deux autorisations de rôle ont été ajoutées pour fournir l’accès aux points d’entrée de l’API Asset : Assets en lecture seule, Assets en lecture-écriture. Si le rôle d’utilisateur de l’API est antérieur à la publication des API de ressources, vous devez créer un rôle d’utilisateur de l’API avec ces autorisations pour activer l’accès. Sinon, vous recevez une réponse d’erreur 603 « Accès refusé ». Outre la publication de l’API REST Asset, des mises à jour ont été apportées aux points d’entrée REST API existants. Le point d’entrée [Fusionner le prospect de l’API REST](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) a été mis à jour pour permettre la fusion de plusieurs prospects. Le point d’entrée [Planifier l’API REST Campaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) a été mis à jour afin de permettre le clonage d’une campagne lors de la planification d’une campagne.

Publié le _2015-03-23_ par _Murta_

## Déclenchement des campagnes RTP avec un délai

Ce JavaScript personnalisé permet aux utilisateurs du RTP d’afficher les campagnes quelques secondes après le chargement de la page web. Ceci est recommandé pour les campagnes de boîte de dialogue et de widget. Elle peut être utilisée pour afficher une campagne après un délai, une fois que le visiteur a consulté le contenu normal sur la page. Il est recommandé de mettre en œuvre ce code uniquement sur des pages spécifiques où la ou les campagnes doivent être affichées. Il n’est pas recommandé d’utiliser ce code sur toutes les pages, car il peut affecter les performances. **Instructions de configuration** Le code personnalisé envoie un événement de données personnalisées RTP (t=timeOnPage, c’est-à-dire t=60), puis charge la campagne qui correspond à cet événement. Par défaut, elle est déclenchée au bout de 60 secondes. Vous pouvez le personnaliser en remplaçant le paramètre sendCustomRTPEvent par n’importe quel autre nombre. Placez le code immédiatement après le code RTP standard :

```javascript
<script>
function sendCustomRTPEvent(a){
 var eventValue="t="+a;
 setTimeout(function(){
  rtp('send', 'event', {value: eventValue});
  rtp('get', 'campaign',true);
 }, 1000 \* a);
}
sendCustomRTPEvent(60); //Seconds
</script>
```

Pour mettre en place une campagne RTP afin de réagir après un délai : 1. Connectez-vous à votre compte RTP 1. Créez un segment 1. Dans la section Événements de segment , ajoutez : `t=60`. Un visiteur ne peut correspondre à chaque segment HTTP qu’une seule fois par session. Par conséquent, chaque campagne ne s’affiche qu’une seule fois (à moins qu’elle ne soit définie sur « sticky »).

Publié le _2015-03-24_ par _Yanir_

## Mises À Jour De La Version D’Avril 2015

### Marketo Mobile Engagement SDK v0.3.2

Marketo comprend désormais l’automatisation du marketing et l’interaction client pour les applications mobiles. L’installation de [Marketo Mobile SDK](/help/mobile/mobile.md) dans votre application iOS ou Android permet aux marketeurs d’écouter les événements dans l’application et d’envoyer des notifications push pertinentes.

### Améliorations de l’API REST

* Objets personnalisés

De nouveaux [points d’entrée d’objet personnalisés](/help/rest-api/custom-objects.md) ont été introduits pour vous permettre de répertorier, décrire et CRUD par programmation les données résidant avec un objet personnalisé Marketo.

Notez que des autorisations de rôle ont été ajoutées pour fournir l’accès aux points d’entrée de l’API d’objet personnalisé : objet personnalisé en lecture seule, objet personnalisé en lecture-écriture. Si votre rôle d’utilisateur API est antérieur à la publication des API d’objets personnalisés, vous devez créer un nouveau rôle d’utilisateur API avec ces autorisations pour activer l’accès. Sinon, vous recevez une réponse d’erreur 603 « Accès refusé ».

* Planifier Campaign - Cloner le programme

Un nouveau paramètre facultatif « cloneToProgramName » a été introduit dans l’[API de campagne planifiée](/help/rest-api/data-ingestion.md). Lorsque ce paramètre est présent, le programme parent de la campagne est cloné et la nouvelle campagne est planifiée. Le paramètre spécifie le nom souhaité pour le programme qui en résulte.

Publié le _2015-04-28_ par _Travis Kaufman_

## Synchronisation des désabonnements aux e-mails entre les instances

Gérez-vous plusieurs instances de Marketo ? Garder les informations de prospect synchronisées entre les instances peut s’avérer difficile. Voici un moyen de synchroniser les désabonnements aux e-mails entre les instances à l’aide d’un webhook qui appelle un service web externe. Le service web externe effectue une boucle sur chaque instance pour rechercher le prospect connu qui a déclenché l’événement de désabonnement. Lorsqu’un prospect correspondant est trouvé, le champ « désabonné » de l’enregistrement de prospect correspondant est mis à jour. Voici un diagramme illustrant l&#39;idée.  C’est à vous de mettre en œuvre le service web, mais le code ci-dessous devrait vous aider à démarrer le processus.

### Service Web externe

Le service web externe effectue les étapes suivantes pour chaque instance Marketo qui doit être synchronisée :

1. Compose l’API REST spécifique à l’instance [URL du point d’entrée](/help/rest-api/endpoint-reference.md)
1. Obtient le jeton d’accès à l’aide de [Identity](/help/rest-api/authentication.md)
1. Obtient la liste des enregistrements de leads qui correspondent à l’adresse e-mail à l’aide de [Obtenir plusieurs leads par type de filtre](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET)
1. Met à jour le champ « désabonné » de chaque enregistrement de prospect à l’aide de Créer/Mettre à jour des prospects

Voici un autre diagramme présentant en détail l’appel de service web externe et les appels de l’API REST Marketo.  L’exemple de code ci-dessous n’est pas un service web prêt à l’emploi. Il s’agit plutôt d’un programme en mode console auquel vous pouvez transmettre des arguments via la ligne de commande. L’objectif ici est de montrer comment appeler les API Marketo appropriées pour mettre à jour les enregistrements de prospect sur plusieurs instances. La mise en œuvre du service web est réservée au lecteur comme un exercice.
**Exemple de code** Pour que l’exemple de code soit opérationnel, vous devez créer un projet Java dans votre IDE préféré. Après cela, vous devrez effectuer les changements suivants : 1. L’exemple de code utilise [json-simple](https://code.google.com/archive/p/json-simple) pour analyser les chaînes JSON. Ajoutez le jar json-simple à votre projet Java. 1. L’exemple de code possède une structure qui contient des métadonnées pour chaque instance Marketo. Placez les valeurs réelles de vos instances dans la structure comme suit :

```java
public static String instanceInfo[][] = {
{ "AccountId1", "ClientId1","ClientSecret1" },    // Instance 1 metadata
{ "AccountId2", "ClientId2","ClientSecret2" },    // Instance 2 metadata
{ "AccountId3", "ClientId3","ClientSecret3" }     // Instance 3 metadata
};
```

Les métadonnées de l’instance se trouvent dans le panneau d’administration de Marketo :

* ID de compte Admin > Intégration > Munchkin > Compte Munchkin
* ID client et secret client Admin > Intégration > LaunchPoint > Synchronisation du désabonnement des e-mails > Afficher les détails

L’exemple de code utilise deux arguments de ligne de commande qui simulent les paramètres de requête « id » et « email » pour le service web externe décrit ci-dessus.

1. args[0] = ID de compte
1. args[1] = Adresse e-mail

Transmettez les valeurs réelles de votre instance sous forme d’arguments au programme. Voici une capture d’écran de configuration de projet d’Intellij IDEA.

**SyncEmailUnsubscribe.java**

```java
package com.marketo;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Scanner;

public class SyncEmailUnsubscribe {
    // Define Marketo instance meta data here.
    // Each row contains three elements: Account Id, Client Id, Client Secret.
    // For example:
    //  public static String instanceData[][] = {
    //    {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    //  };
    //
    public static String instanceData[][] = {
            // ADD YOUR INSTANCE META DATA HERE
    };

    public static void main(String[] args) {
        String accountId = args[0];     // Account id that processed the unsubscribe
        String emailAddress = args[1];  // Email address of lead that unsubscribed

        SyncEmailUnsubscribe seu = new SyncEmailUnsubscribe();

        // Loop through each Marketo instance
        for (int i = 0; i < instanceData.length; i++) {

            // Make sure we skip instance that triggered the webhook
            if (!accountId.equals(instanceData[i][0])) {
                String endpointUrl = String.format("https://%s.mktorest.com", instanceData[i][0]);

                // Generate access token
                String identityUrl = String.format("%s/identity/oauth/token?grant_type=client_credentials&client_id=%s&client_secret=%s", endpointUrl, instanceData[i][1], instanceData[i][2]);
                String token = seu.getToken(identityUrl);

                // Get lead records for given email address (may be duplicates)
                String getLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=email&filterValues=%s", endpointUrl, token, emailAddress);
                String leads = seu.getLeads(getLeadsUrl);

                // Update unsubscribed field in lead record
                String updateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", endpointUrl, token);
                seu.updateLeads(updateLeadsUrl, leads, accountId);
            }
        }

        System.exit(0);
    }

    // Call Identity Service to generate access token
    public String getToken(String url) {
        // Call Identity Service
        String tokenData = getData(url);

        // Convert response into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(tokenData);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }

        // Retrieve access_token
        JSONObject jsonObject = (JSONObject)obj;
        return jsonObject.get("access_token").toString();
    }

    // Call Get Multiple Leads by Filter Type Service to get lead records
    public String getLeads(String url) {
        return getData(url);
    }

    // Call Create/Update Lead Service to update "unsubscribed" flag in lead record
    public void updateLeads(String url, String leads, String account) {
        JSONObject body = composeBody(leads, account);
        if (body != null) {
            postData(url, body);
        }
    }

    // Compose JSON body for Create/Update Leads Service
    private JSONObject composeBody(String leads, String account) {
        JSONObject body = new JSONObject();

        // Convert leads into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(leads);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }
        JSONObject leadsObj = (JSONObject)obj;

        Object success = leadsObj.get("success");
        if (success.equals(true)) {
            body.put("action", "updateOnly");
            body.put("lookupField", "id");
            body.put("asyncProcessing", "true");

            // Build array of lead objects
            JSONArray input = new JSONArray();
            JSONArray result = (JSONArray) leadsObj.get("result");
            Iterator<JSONObject> iterator = result.iterator();
            while (iterator.hasNext()) {
                JSONObject leadIn = (JSONObject)iterator.next();
                JSONObject lead = new JSONObject();
                lead.put("id", leadIn.get("id"));
                lead.put("unsubscribed", "true");
                lead.put("unsubscribedReason", "Cross instance synch triggered by webhook from: " + account);
                input.add(lead);
            }

            body.put("input", input);
        }
        return body;
    }

    // HTTP POST request
    private String postData(String endpoint, JSONObject body) {
        String data = "";
        try {
            // Make request
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("POST");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "application/json");
            urlConn.connect();
            OutputStream os = urlConn.getOutputStream();
            os.write(body.toJSONString().getBytes());
            os.close();
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    // HTTP GET request
    private String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

### Configuration de Marketo

Pour chaque instance Marketo que vous souhaitez synchroniser, procédez comme suit.

1. Créez un service personnalisé avec l’autorisation du rôle : Lead en lecture-écriture. Si vous ne connaissez pas la création d’un service personnalisé, cliquez [ici](/help/rest-api/custom-services.md).
1. Créez un Webhook qui appelle votre service web externe. Si vous ne connaissez pas la création de Webhooks, cliquez [ici](/help/webhooks/webhooks.md).
1. Ajoutez Webhook comme étape de flux dans Smart Campaign.

La capture d’écran ci-dessous montre comment créer un webhook pour appeler le service spécifié ci-dessus à l’aide de jetons afin de renseigner automatiquement les paramètres de requête. Maintenant que nous avons créé notre webhook, nous pouvons l’ajouter à une campagne intelligente en tant qu’action de flux.  La liste dynamique doit contenir un déclencheur « Désabonnements de l’e-mail ».

### Validation

Pour tester tout cela, créez un prospect avec la même adresse e-mail dans plusieurs instances Marketo. Assurez-vous de posséder l’adresse e-mail. Dans une instance, déclenchez une action de flux d’envoi d’e-mail, ouvrez l’e-mail généré, puis cliquez sur se désabonner. Pour valider les résultats, connectez-vous à chacune des autres instances et examinez les enregistrements de prospect associés à l’adresse e-mail. La case à cocher « Désabonné » doit être cochée et le champ « Raison du désabonnement » doit contenir une note avec l’identifiant du compte source qui a lancé la synchronisation.

Publié le _2015-05-11_ par _David_

## Synchronisation des modifications des données de lead à l’aide de l’API REST

Cette publication présentait un exemple de code qui pouvait être exécuté de manière récurrente pour interroger Marketo sur les mises à jour. L’idée était d’utiliser les API de Marketo pour identifier les modifications apportées aux données de prospect et extraire les données de prospect qui avaient été modifiées. Ces données peuvent ensuite être transmises à un système externe à des fins de synchronisation. L’exemple de code présenté utilisait notre API SOAP. Nous avons une [nouvelle façon de marcher](https://www.youtube.com/watch?v=G-7ZJjLy5D8&feature=youtu.be), qui consiste à utiliser l’API REST [Marketo](/help/rest-api/rest-api.md). Cette publication vous explique comment atteindre le même objectif à l’aide de deux points d’entrée REST : [Obtenir les modifications du lead](/help/rest-api/rest-api.md) et [Obtenir le lead par ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Le programme comprend 2 étapes principales :

1. Appelez Obtenir les modifications de lead pour générer une liste de tous les ID de lead pour lesquels des champs de lead spécifiques ont été modifiés ou ajoutés au cours d’une période donnée.
1. Appelez Get Lead Id pour chaque ID de lead de la liste afin de récupérer les données de champ de l’enregistrement du lead.

Nous allons prendre les données récupérées à l’étape 2 et les formater pour qu’elles soient utilisées par un système externe.  **Entrée du programme** Par défaut, le programme « revient » un jour en arrière à partir de la date actuelle pour rechercher des modifications. Ainsi, vous pouvez exécuter ce programme à la même heure chaque jour, par exemple. Pour revenir plus loin dans le temps, vous pouvez spécifier le nombre de jours comme argument de ligne de commande, ce qui augmente la fenêtre temporelle. Le programme contient plusieurs variables que vous pouvez modifier : CUSTOM_SERVICE_DATA - Contient vos données Marketo [Custom Service](/help/rest-api/custom-services.md) (ID de compte, ID client, Secret client). LEAD_CHANGE_FIELD_FILTER - Contient une liste de champs de prospect séparés par des virgules, que nous examinerons à la recherche de modifications. READ_BATCH_SIZE - Nombre d&#39;enregistrements à récupérer à la fois. Utilisez cette option pour ajuster la réponse à la taille du corps. **Sortie du programme** le programme rassemble tous les enregistrements de prospect modifiés et les formate dans JSON sous la forme d’un tableau d’objets de prospect comme suit :

```json
{
    "result": [
        {
            "leadId": "318592",
            "updatedAt": "2015-07-22T19:19:07Z",
            "firstName": "David",
            "lastName": "Everly",
            "email": "<deverly@marketo.com>"
        },

        ...more lead objects here...
    ]
}
```

L’idée est que vous puissiez ensuite transmettre ce fichier JSON en tant que payload de requête à un service web externe pour synchroniser les données. **Logique du programme** Tout d’abord, nous établissons notre fenêtre temporelle, composons nos URL de point d’entrée REST et obtenons notre jeton d’accès d’authentification. Nous allons ensuite déclencher une boucle Get Paging Token/Get Lead Changes dans laquelle s’exécute jusqu’à ce que nous ayons épuisé l’offre de modifications de lead. Le but de cette boucle est d’accumuler une liste d’ID de lead uniques afin de pouvoir les transmettre à Get Lead by Id plus tard dans le programme. Pour cet exemple, nous indiquons à Obtenir les modifications de lead de rechercher des modifications dans les champs suivants : firstName, lastName, email. Vous êtes libre de sélectionner n’importe quelle combinaison de champs à vos fins. Obtenir les modifications de lead renvoie des objets « result » contenant un ID de type d’activité, que nous pouvons utiliser pour filtrer les résultats. Remarque : vous pouvez obtenir une liste des types d’activités en appelant le point d’entrée REST [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getAllActivityTypesUsingGET). Deux types d’activités qui sont renvoyés nous intéressent : 1. Nouveau prospect (12)

```json
{
    "id": 12024682,
    "leadId": 318581,
    "activityDate": "2015-03-17T00:18:41Z",
    "activityTypeId": 12,
    "primaryAttributeValueId": 318581,
    "primaryAttributeValue": "David Everly",
    "attributes": [
        {
            "name": "Created Date",
            "value": "2015-03-16"
        },
        {
            "name": "Source Type",
            "value": "New lead"
        }
    ]
}
```

1. Modifier la valeur des données (13) Nous pouvons déterminer quel champ a été modifié en examinant la propriété « nom » dans la réponse Modifier la valeur des données.

```json
{
    "id": 12024689,
    "leadId": 318581,
    "activityDate": "2015-03-17T22:58:18Z",
    "activityTypeId": 13,
    "fields": [
        {
            "id": 31,
            "name": "lastName",
            "newValue": "Evely",
            "oldValue": "Everly"
        }
    ],
    "attributes": [
        {
            "name": "Source",
            "value": "Web form fillout"
        }
    ]
}
```

Lorsque l’un de ces deux types d’activités est renvoyé, l’ID de lead associé est stocké dans une liste. Une fois que nous avons notre liste, nous pouvons la parcourir en appelant Get Lead by Id pour chaque élément. Cette opération récupère les dernières données de prospect pour chaque prospect de la liste. Pour cet exemple, nous récupérons les champs de prospect suivants : `leadId`, `updatedAt`, `firstName`, `lastName` et `email`. Vous êtes libre de sélectionner n’importe quelle combinaison de champs à vos fins. Pour ce faire, spécifiez le paramètre fields pour Obtenir le prospect par ID. Enfin, nous utilisons JSONify pour obtenir les résultats sous la forme d’un tableau d’objets de prospect comme décrit ci-dessus.

**Code du programme**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadChanges {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            {INSERT YOUR CUSTOM SERVICE DATA HERE};

    // Lead fields that we are interested in
    private static final String LEAD_CHANGE_FIELD_FILTER = "firstName,lastName,email";

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Activity type ids that we are interested in
    private static final int ACTIVITY_TYPE_ID_NEW_LEAD = 12;
    private static final int ACTIVITY_TYPE_ID_CHANGE_DATA_VALE = 13;

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String leadChangesUrl = String.format("%s/rest/v1/activities/leadchanges.json?access_token=%s&fields=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_CHANGE_FIELD_FILTER, READ_BATCH_SIZE);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        HashSet leadIdList = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {

            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;

            do {
                moreResult = false;

                // Call Get Lead Changes API
                JsonObject leadChangesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadChangesUrl, nextPageToken)));
                if (leadChangesObj.get("success").asBoolean()) {
                    moreResult = leadChangesObj.get("moreResult").asBoolean();
                    nextPageToken = leadChangesObj.get("nextPageToken").asString();

                    if (leadChangesObj.get("result") != null) {
                        JsonArray resultAry = leadChangesObj.get("result").asArray();
                        for (JsonValue resultObj : resultAry) {
                            int activityTypeId = resultObj.asObject().get("activityTypeId").asInt();


                            // Store lead ids for later use
                            boolean storeThisId = false;
                            if (activityTypeId == ACTIVITY_TYPE_ID_NEW_LEAD) {
                                storeThisId = true;
                            } else if (activityTypeId == ACTIVITY_TYPE_ID_CHANGE_DATA_VALE) {
                                // See if any of the changed fields are of interest to us
                                JsonArray fieldsAry = resultObj.asObject().get("fields").asArray();
                                for (JsonValue fieldsObj : fieldsAry) {
                                    String name = fieldsObj.asObject().get("name").asString();
                                    if (LEAD_CHANGE_FIELD_FILTER.contains(name)) {
                                        storeThisId = true;
                                    }
                                }

                            }

                            if (storeThisId) {
                                leadIdList.add(resultObj.asObject().get("leadId").toString());
                            }
                        }
                    }
                }

            } while (moreResult);
        }

        JsonObject result = new JsonObject();
        JsonArray leads = new JsonArray();

        for (Object o : leadIdList) {
            String leadId = o.toString();

            // Compose Get Lead by Id URL
            String getLeadUrl = String.format("%s/rest/v1/lead/%s.json?access_token=%s",
                    baseUrl, leadId, accessToken);

            // Call Get Lead by Id API
            JsonObject leadObj = JsonObject.readFrom(getData(getLeadUrl));
            if (leadObj.get("success").asBoolean()) {
                if (leadObj.get("result") != null) {
                    JsonArray resultAry = leadObj.get("result").asArray();
                    for (JsonValue resultObj : resultAry) {

                        // Create lead object
                        JsonObject lead = new JsonObject();
                        lead.add("leadId", leadId);
                        lead.add("updatedAt", resultObj.asObject().get("updatedAt").asString());
                        lead.add("firstName", resultObj.asObject().get("firstName").asString());
                        lead.add("lastName", resultObj.asObject().get("lastName").asString());
                        lead.add("email", resultObj.asObject().get("email").asString());

                        // Add lead object to leads array
                        leads.add(lead);
                    }
                }
            }
        }

        // Add leads array to result object
        result.add("result", leads);

        // Print out result object
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Alors voilà, [la vie est une chanson heureuse](https://www.youtube.com/watch?v=zFaBwZDywLk). Bon appétit !

Publié le _2015-07-31_ par _David_

## Mises À Jour De Mai 2015

### API REST

* API d’opportunité. De nouveaux [points d’entrée d’opportunité](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities) ont été introduits pour vous permettre de répertorier, décrire et CRUD par programmation les données résidant dans un objet d’opportunité Marketo.

Remarque : des autorisations de rôle ont été ajoutées pour fournir l’accès aux points d’entrée d’opportunité : opportunité en lecture seule, opportunité en lecture-écriture. Si votre rôle d’utilisateur API est antérieur à la publication des API d’opportunité, vous devez créer un nouveau rôle d’utilisateur API avec ces autorisations pour activer l’accès. Sinon, vous recevez une réponse d’erreur 603 « Accès refusé ».

* API de ressources - Fragments de code. De nouveaux [points d’entrée de ressources pour les fragments de code](https://developer.adobe.com/marketo-apis/api/asset/#snippet_endpoints) ont été introduits pour vous permettre de manipuler par programmation les objets de fragment de code. Les fragments de code peuvent être utilisés comme blocs de contenu dynamiques dans les e-mails et les landing pages.
* API de leads - Mettre à jour la partition des leads. Un nouveau [point d’entrée de prospect pour les partitions](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updatePartitionsUsingPOST) a été ajouté pour vous permettre de mettre à jour la partition pour un ou plusieurs prospects.
* Correction d’un problème en raison duquel les API liées aux leads ne disposaient pas du décalage de fuseau horaire dans les attributs « createdAt » et « updatedAt ».
* Correction d’un problème en raison duquel la planification de Campaign ne renvoyait pas le code d’erreur correct lorsque le nombre maximal quotidien d’appels avait été dépassé.
* Correction d’un problème en raison duquel Get Folder by Id renvoyait parfois la valeur null pour les attributs « parent » et « description ».
* Correction d’un problème en raison duquel Approuver l’e-mail par ID générait une erreur système dans certains cas.
* Correction d’un problème en raison duquel Créer un jeton par ID de dossier générait un jeton inutilisable dans certains cas.

### Personalization en temps réel (RTP)

* API Rich Media Recommendations. De nouvelles fonctionnalités [recommandation pour les médias riches](/help/javascript-api/web-personalization.md) ont été ajoutées à l’API JavaScript RTP. La recommandation de contenu multimédia enrichi fait participer vos visiteurs web au contenu le plus pertinent optimisé par le machine learning et l’analyse prédictive. Améliorez vos ressources de contenu avec des descriptions textuelles et des images, et incorporez plusieurs recommandations de contenu sur votre site web.

### SDK d’engagement mobile

iOS v0.3.4/Android v0.3.3

* Actions personnalisées. Ajout de la possibilité de suivre les interactions utilisateur en envoyant des actions personnalisées. Pour plus d’informations, voir « Envoi d’actions personnalisées » [ici](/help/mobile/mobile.md).
* La méthode trackPushNotification est obsolète.

Publié le _2015-05-26_ par _David_

## Mises À Jour De Juin 2015

### API REST

* API d’entreprise

De nouveaux [points d’entrée de société](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies) ont été introduits pour vous permettre de répertorier, décrire et CRUD par programmation les données résidant dans un objet de société Marketo.

Remarque : des autorisations de rôle ont été ajoutées pour fournir l&#39;accès aux points d&#39;entrée du programme : Société en lecture seule, Société en lecture-écriture. Si votre rôle d’utilisateur API est antérieur à la publication des API d’entreprise, vous devez mettre à jour votre rôle d’utilisateur API avec ces autorisations pour activer l’accès. Sinon, vous recevez une réponse d’erreur 603 « Accès refusé »

* API de ressources - Programmes

Mise à jour de l’API Asset pour prendre en charge les dossiers Application et les dossiers Programme. Plusieurs API de ressources avaient accepté un ID de dossier dans la requête sous la forme d’un entier. L’ID de dossier peut être transmis dans le cadre de l’URI ou du corps de la requête, selon l’API.

Vous devez maintenant spécifier le type de dossier avec l’ID de dossier. Le type de dossier est soit « Programme », soit « Dossier ». « Dossier » spécifie un dossier d’application, tandis que « Programme » spécifie un dossier de programme. Le type de dossier est spécifié de deux manières différentes en fonction de l’API que vous appelez :

1. Utilisez la structure de données « FolderIdType ». FolderIdType est un bloc JSON simple contenant l’identifiant et la paire de types comme suit :

`{ "id" : **id**, "type" : "**type**" }`

Où **id** est l’ID de dossier et « **type** » est le type de dossier. Les valeurs autorisées pour « **type** » sont « Dossier » ou « Programme ».

Exemple - Créer un dossier

`POST /asset/v1/folders.json`

`parent={"id":416,"type":"Folder"}&name=Test Folder&description=This is a test folder`

1. Utilisez le paramètre d’identifiant existant et spécifiez son type à l’aide d’un paramètre de requête « type ».

Exemple - Obtenir le dossier par ID

`GET /rest/asset/v1/folder/1016.json?type=Program`

Toutes les réponses d’API qui contenaient un ID de dossier dans l’objet Résultat contiendront désormais également un attribut folderId dont la valeur est un FolderIdType. Vous pouvez l’utiliser pour déterminer le type de dossier d’un ID de dossier donné.

Exemple - Obtenir le dossier par nom

`GET /rest/asset/v1/folder/byName.json?name=Social Media`

```json
"result": [
{
...

"folderId": {"id":341, "type": "Program"},
...

"id":"341"
}
]
```

Pour déterminer le type de dossier pour un ID donné, vous pouvez utiliser l’API [Parcourir les dossiers](https://developer.adobe.com/marketo-apis/api/asset/browse-folders/).

L’attribut « type » dans les réponses de l’API a été renommé en « folderType ». Cela permet d’éviter toute confusion avec l’élément « type » contenu dans FolderIdType.

Par exemple, à partir de ce :

 »**type** »:« Dossier marketing »

en ceci :

 »**folderType** » : « Dossier marketing »

### SDK d’engagement mobile

iOS 0.3.5

* Correction d’un problème en raison duquel la boîte de dialogue Définir l’appareil de test s’exécutait sur le thread principal. [MOB-638]
* Ajout de la gestion des erreurs en cas d’échec d’enregistrement du périphérique de test. [MOB-639]

Android 0.3.3

* Ajout d’un attribut android:configChanges à l’élément de `<activity>` AndroidManifest.xml pour éviter que la boîte de dialogue de progression ne soit ignorée lorsque vous ajoutez un appareil de test et modifiez l’orientation. [MOB-687]

Publié le _2015-06-30_ par _David_

## Personalization Web In-App (Beta) à l’aide de l’API RTP

Plusieurs de nos clients fournissent des solutions d’applications web à leurs utilisateurs et nous recevons des demandes s’ils peuvent utiliser Marketo Real-Time Personalization (RTP) dans leur environnement d’applications web sécurisé. La réponse est oui ! Nous avons publié une API pour la messagerie In-App afin que vous puissiez personnaliser le contenu et promouvoir des activités marketing telles que des webinaires, de nouvelles mises à jour de fonctionnalités, des ventes incitatives et impliquer vos utilisateurs en fonction des données de votre application web. Par exemple, la personnalisation du contenu In-App pour :

* Offres d&#39;évaluation basées sur l&#39;activité de l&#39;utilisateur
* Différents types d’abonnement (vente incitative, vente croisée ou formation au webinaire)
* Nouvelles fonctionnalités relatives à l’activité de l’utilisateur

**Exemple de cas d’utilisation** l’équipe du succès client Marketo utilise In-App Web Personalization pour communiquer avec des types d’abonnement spécifiques (Spark, Standard, Select ou Enterprise) avec du contenu personnalisé, en s’assurant qu’ils voient des campagnes progressives et qu’ils encouragent les utilisateurs in-app en fonction de leur engagement. Voyons comment procéder pour un utilisateur disposant d’un type d’abonnement Entreprise . **Prérequis** comprendre l’[API de contexte utilisateur RTP](/help/javascript-api/web-personalization.md). **Activer l’API de contexte utilisateur** Demandez au support Marketo d’activer l’API de contexte utilisateur pour votre compte RTP. **Définir la variable personnalisée** Il existe 5 emplacements de variable personnalisée disponibles dans le RTP pour envoyer des données à . Dans cet exemple, nous allons envoyer un abonnement utilisateur de type Entreprise à la variable personnalisée 1.

`rtp('set', 'customVar1', 'Enterprise');`

**Créer un segment RTP** Accédez à **Segments**, cliquez sur **CRÉER**.

1. Faites glisser le filtre **API de contexte utilisateur** vers le créateur de segments.
1. Sélectionnez la **Variable personnalisée 1** (type d’abonnement) qui **valeur** est **Entreprise**.

**Afficher la campagne en fonction de l’historique des visites précédentes** Pour afficher une autre campagne au visiteur s’il a déjà cliqué sur une campagne lors d’une visite précédente.

1. Dans l’**API de contexte utilisateur**, cliquez sur **(+)** pour ajouter un autre attribut d’API de contexte utilisateur
1. Ajoutez l’**opérateur (AND ou OR).**
1. Sélectionnez **Campagnes - Sur lesquelles l’utilisateur a cliqué.** Définir **Identifiant de campagne** sur l’identifiant de la campagne. (Voir la note ci-dessous pour savoir comment trouver l’identifiant de campagne.)
1. Cliquez sur **ENREGISTRER ET DÉFINIR LA CAMPAGNE** pour créer le contenu créatif de la campagne.

Dans l’ensemble, ce segment correspond si un visiteur est associé à la variable personnalisée (type d’abonnement) égale à Entreprise et s’il a cliqué sur la campagne (ID : 5390) lors d’une visite précédente. L’étape suivante consiste à définir une campagne personnalisée pour ce segment. La capture d’écran ci-dessous montre une campagne de boîte de dialogue de RTP (en bas à gauche) apparaissant sur la page My Marketo faisant la promotion d’un webinaire pour les utilisateurs d’Enterprise.  **REMARQUE :** **Localisation de l’identifiant de campagne** Accédez à **Campagnes**, placez le curseur sur le **Nom de la campagne** pour localiser l’identifiant de campagne.
Publié le _2015-06-17_ par _David_

## Envoi d’e-mails transactionnels avec l’API REST Marketo : Partie 1

Il existe quelques exigences de configuration dans Marketo pour exécuter l’appel requis avec l’API REST Marketo.

* Le destinataire doit avoir un enregistrement dans Marketo
* Un e-mail transactionnel doit être créé et approuvé dans votre instance Marketo.
* Il doit y avoir une campagne de déclenchement active avec la campagne demandée, Source : API de service web, configurée pour envoyer l’e-mail

Tout d’abord[&#x200B; créez et approuvez votre e-mail](https://experienceleague.adobe.com/fr/docs/marketo/using/home). Si l’e-mail est réellement transactionnel, vous devrez probablement le définir sur opérationnel, mais assurez-vous qu’il est juridiquement qualifié comme opérationnel. Elle est configurée à partir de l’écran Modifier sous Actions d’e-mail > Paramètres d’e-mail. Validez-le et nous sommes prêts à créer notre campagne. Si vous êtes un débutant dans la création de campagnes, consultez l’article [Créer une campagne intelligente](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign) sur docs.marketo.com. Une fois que vous avez créé votre campagne, nous devons passer par ces étapes. Configurer votre liste dynamique avec le déclencheur La campagne est demandée : nous devons maintenant configurer le flux pour pointer une étape Envoyer un e-mail vers votre e-mail. Avant l’activation, vous devez définir certains paramètres dans l’onglet Planification . Si cet e-mail particulier ne doit être envoyé qu’une seule fois à un enregistrement donné, laissez les paramètres de qualification tels quels. S’il est nécessaire qu’ils reçoivent l’e-mail plusieurs fois, cependant, vous voudrez l’ajuster à chaque fois ou à l’une des cadences disponibles. Maintenant, nous sommes prêts à activer.

### Envoi des appels API

**Remarque :** dans les exemples Java ci-dessous, nous utilisons le package minimal-json pour gérer les représentations JSON dans notre code. Vous pouvez en savoir plus sur ce projet ici : [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) La première partie de l’envoi d’un e-mail transactionnel par le biais de l’API consiste à s’assurer qu’un enregistrement avec l’adresse e-mail correspondante existe dans votre instance Marketo et que nous avons accès à son ID de prospect. Pour les besoins de cette publication, nous supposerons que les adresses e-mail se trouvent déjà dans Marketo et qu’il nous suffit de récupérer l’identifiant de l’enregistrement. Pour cela, nous utilisons l’appel [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) et nous réutilisons une partie du code Java de la publication précédente. Examinons notre Méthode principale pour demander la campagne :

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("<requestCampaign.test@marketo.com>");

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

Pour obtenir ces résultats à partir de la réponse JsonObject de leadsRequest, nous devons écrire du code. Pour récupérer le premier résultat dans le tableau , nous devons extraire le tableau de JsonObject et indexer l’objet à 0 :

`JsonArray leadsResult = leadsRequest.getData().get("result").asArray();`
`int leadId = leadsResult.get(0).asObject().get("id").asInt();`

À partir de là, il nous suffit d’effectuer l’appel Demander la campagne . Pour ce faire, les paramètres requis sont ID dans l’URL de la requête et un tableau d’objets JSON contenant un membre, « id ». Jetons un coup d’œil au code pour ceci :

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
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   System.out.println("Executing RequestCampaign calln" + "Endpoint: " + s + "nRequest Body:n"  + requestBody);
   URL url = new URL(s);
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
   System.out.println("Result:n" + result);
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

Cette classe a un constructeur qui prend une authentification et l’identifiant de la campagne. Les leads sont ajoutés à l’objet soit en transmettant un `<Integer>` ArrayList contenant les identifiants des enregistrements à setLeads, soit en utilisant addLead, qui prend un entier et l’ajoute à l’ArrayList existant dans la propriété leads. Pour déclencher l’appel API afin de transmettre les enregistrements de prospect à la campagne, il faut appeler postData, qui renvoie un objet JsonObject contenant les données de réponse de la requête. Lorsqu’une campagne de requête est appelée, chaque prospect transmis à l’appel est traité par la campagne de déclenchement cible dans Marketo et l’e-mail précédemment créé lui est envoyé. Félicitations, vous avez déclenché un e-mail via l’API REST Marketo. Gardez un œil sur la Partie 2, où nous allons examiner la personnalisation dynamique du contenu d’un e-mail par le biais de Request Campaign.

Publié le _2015-07-17_ par _Kenny_

## Authentification et récupération des données de lead à partir de Marketo avec l’API REST

**Remarque :** dans les exemples Java ci-dessous, nous utilisons le package minimal-json pour gérer les représentations JSON dans notre code. Vous pouvez en savoir plus sur ce projet ici : [https://github.com/ralfstx/minimal-json](https://github.com/ralfstx/minimal-json) L’une des exigences les plus courantes lors de l’intégration à Marketo est la récupération des données de prospect. La plupart, voire toutes les intégrations, nécessiteront la récupération ou l’envoi des données de prospect d’un abonnement Marketo. Nous allons donc aujourd’hui examiner une tâche de base de récupération des informations de prospect, en [authentification](/help/rest-api/authentication.md) avec un abonnement, puis en récupérant les données de prospect à partir de celui-ci. Pour récupérer nos prospects, nous devons d’abord nous authentifier auprès de l’instance Marketo cible à l’aide d’OAuth 2.0. Il existe trois informations que nous devons authentifier avec Marketo : l’identifiant client, le secret client et l’hôte de l’instance Marketo. Voici la classe que nous utilisons pour l’authentification :

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Auth {
 protected String marketoInstance; //Instance Host obtained from Admin -> Web Service
 private String clientId; //clientId obtained from Admin -> Launchpoint -> View Details for selected service
 private String clientSecret; //clientSecret obtained from Admin -> Launchpoint -> View Details for selected service
 private String idEndpoint; //idEndpoint constructed to authenticate with service when constructor is used
 private String token; //token is stored for reuse until expiration
 private Long expiry; //used to store time of expiration

 //Creates an instance of Auth which is used to Authenticate with a particular service on a particular instance
 public Auth(String id, String secret, String instanceUrl) {
  this.clientId = id;
  this.clientSecret = secret;
  this.marketoInstance = instanceUrl;
  this.idEndpoint = marketoInstance + "/identity/oauth/token?grant_type=client_credentials&client_id=" + clientId + "&client_secret=" + clientSecret;
 }
 //The only public method of Auth, used to check if the current value of Token is valid, and then to retrieve a new one if it is not
 public String getToken(){
  long now  = System.currentTimeMillis();
  if (expiry == null || expiry < now){
   System.out.println("Token is empty or expired. Trying new authentication");
   JsonObject jo = getData();
   System.out.println("Got Authentication Response: " + jo);
   this.token = jo.get("access_token").asString();
                        //expires_in is reported as seconds, set expiry to system time in ms + expires \* 1000
   this.expiry = System.currentTimeMillis() + (jo.get("expires_in").asLong() \* 1000);
  }
  return this.token;
 }
 //Executes the authentication request
 private JsonObject getData(){
  JsonObject jsonObject = null;
  try {
   URL url = new URL(idEndpoint);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
   urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "application/json");
            System.out.println("Trying to authenticate with " + idEndpoint);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                Reader reader = new InputStreamReader(inStream);
                jsonObject = JsonObject.readFrom(reader);
            }else {
                throw new IOException("Status: " + responseCode);
            }
  } catch (MalformedURLException e) {
   e.printStackTrace();
  }catch (IOException e) {
            e.printStackTrace();
        }
  return jsonObject;
 }
}
```

Ce code vous permet de créer une instance d’Auth avec votre identifiant client, secret client et hôte (en tant que marketoInstance) à partir de Admin -> Point de lancement (ID et secret) et Admin -> Services web (hôte). Elle expose la méthode getToken qui teste si le jeton actuellement stocké est nul ou a expiré, puis renvoie le jeton existant ou effectue une nouvelle demande d’authentification, puis renvoie le nouveau jeton à partir du membre « access_token » de la réponse JSON. Maintenant que vous pouvez vous authentifier avec votre instance Marketo, l’étape suivante consiste à récupérer nos prospects. Nous utilisons cette classe :

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Leads {
 private StringBuilder endpoint;
 private Auth auth;
 public String filterType;
 public ArrayList filterValues = new ArrayList();
 public Integer batchSize;
 public String nextPageToken;
 public ArrayList fields = new ArrayList();

 public Leads(Auth a) {
  this.auth = a;
  this.endpoint = new StringBuilder(this.auth.marketoInstance + "/rest/v1/leads.json");
 }
 public Leads setFilterType(String filterType) {
  this.filterType = filterType;
  return this;
 }
 public Leads setFilterValues(ArrayList filterValues) {
  this.filterValues = filterValues;
  return this;
 }
 public Leads addFilterValue(String filterVal) {
  this.filterValues.add(filterVal);
  return this;
 }
 public Leads setBatchSize(Integer batchSize) {
  this.batchSize = batchSize;
  return this;
 }
 public Leads setNextPageToken(String nextPageToken) {
  this.nextPageToken = nextPageToken;
  return this;
 }
 public Leads setFields(ArrayList fields) {
  this.fields = fields;
  return this;
 }

 public JsonObject getData() {
        JsonObject result = null;
        try {
         endpoint.append("?access_token=" + auth.getToken() + "&filterType=" + filterType + "&filterValues=" + csvString(filterValues));
         if (batchSize != null && batchSize > 0 && batchSize <= 300){
             endpoint.append("&batchSize=" + batchSize);
            }
            if (nextPageToken != null){
             endpoint.append("&nextPageToken=" + nextPageToken);
            }
            if (fields != null){
             endpoint.append("&fields=" + csvString(fields));
            }
            URL url = new URL(endpoint.toString());
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "text/json");
            InputStream inStream = urlConn.getInputStream();
            Reader reader = new InputStreamReader(inStream);
            result = JsonObject.readFrom(reader);
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return result;
    }
 private String csvString(ArrayList fields) {
  StringBuilder fieldCsv = new StringBuilder();
     for (int i = 0; i < fields.size(); i++){
      fieldCsv.append(fields.get(i));
      if (i + 1 != fields.size()){
       fieldCsv.append(",");
      }
     }
  return fieldCsv.toString();
 }
}
```

Cette classe a un seul constructeur qui accepte un objet Auth, puis expose plusieurs définisseurs pour les paramètres facultatifs et obligatoires. Dans ce cas, il nous suffit de définir les valeurs filterType et filterValues pour exécuter des leads get par adresse e-mail. Nous utiliserons donc setFilterType pour « email » et addFilterValue pour l’adresse e-mail pour laquelle nous devons récupérer un identifiant. Lorsque vous avez défini vos paramètres, vous pouvez utiliser la méthode getData pour récupérer un objet JsonObject à partir du point d’entrée des prospects, contenant un tableau de résultats avec une représentation JSON des enregistrements de prospects récupérés.

### Assemblage

Maintenant que nous avons parcouru l’exemple de code que nous utilisons, jetons un coup d’œil à un exemple simple pour récupérer les prospects correspondant à une adresse e-mail de test, <testlead@marketo.com>. Pour cela, nous devons utiliser setFilterType pour « e-mail » et addFilterValue pour l’adresse e-mail pour laquelle nous devons récupérer des informations. Lorsque vous avez défini vos paramètres, vous pouvez utiliser la méthode getData pour récupérer un objet JsonObject à partir du point d’entrée des prospects, contenant un tableau de résultats avec une représentation JSON des enregistrements de prospects récupérés.

```java
package dev.marketo.blog_leads;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        //Change the credentials here to your own
        Auth auth = new Auth("CHANGE ME", "CHANGE ME", "CHANGE ME");
        //Create and parameterize an instance of Leads
        Leads getLeads = new Leads(auth)
              .setFilterType("email")
              .addFilterValue("<testlead@marketo.com>");
        //get the inner results array of the response
        JsonObject leadsResult = getLeads.getData();
        System.out.println(leadsResult);
    }
}
```

Dans cet exemple de méthode principale, nous créons une instance d’authentification, puis nous la transmettons à un nouveau constructeur Leads. En utilisant setFilterType et addFilterValue, nous configurons notre instance de leads pour récupérer uniquement les leads correspondant à l’adresse e-mail « <testlead@marketo.com> ». Cet exemple imprime ceci dans la console :

Le jeton est vide ou a expiré. Essai d’une nouvelle authentification
Tentative d’authentification avec <https://299-BYM-827.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id=b417d98f-9289-47d1-a61f-db141bf0267f&client_secret=0DipOvz4h2wP1ANeVjlfwMvECJpo0ZYc>
Réponse d&#39;authentification reçue : {« access_token »:« ec0f02c0-28ac-4d6c-b7d7-00e47ae85ff1:st »,« token_type »:« bearer »,« expires_in »:538,« scope »: »<apiuser@mktosupport.com>« }
{« requestId »:« 14fb6#14e6a7a9ad6 »,« result »:[{« id »:1026322,« updatedAt »:« 2015-07-07T21:43:25Z »,« lastName »:« Lead »,« email »: »<testlead@marketo.com> »,« createdAt »:« 2015-07-07T21:43:25Z »,« firstName »:« Test »},{« id »:1026323,« updatedAt »:« 2015-07 -07T21:43:43Z »,« lastName »:« Lead2 »,« email »: »<testlead@marketo.com> »,« createdAt »:« 2015-07-07T2143Z »,« firstName »:« Test »}:43:,« success »] :true}

Nous avons maintenant des données sur les leads que nous pouvons traiter de la manière dont nous en avons besoin. Merci d&#39;avoir lu cet article. Veuillez laisser vos commentaires dans les commentaires.

Publié le _2015-07-10_ par _Kenny_

## Mises À Jour De Juillet 2015

API REST

* API du commercial

De nouveaux [points d’entrée commerciaux](/help/rest-api/sales-persons.md) ont été introduits pour vous permettre de répertorier, décrire et CRUD par programmation les données résidant dans un objet commercial Marketo. En outre, un commercial peut être affecté à un prospect, à une opportunité ou à une entreprise. Pour ce faire, spécifiez un attribut « externalSalesPersonId » lors de l’appel du point d’entrée Create/Update/Upsert pour le prospect, l’opportunité ou l’entreprise.

Remarque : des autorisations de rôle ont été ajoutées pour fournir un accès aux points d’entrée du programme : commercial en lecture seule, commercial en lecture-écriture. Si votre rôle d’utilisateur API est antérieur à la publication des API de vendeur, vous devez mettre à jour votre rôle d’utilisateur API avec ces autorisations pour activer l’accès. Sinon, vous recevez une réponse d’erreur 603 « Accès refusé ».

* API de ressources - Modèle de page de destination

De nouveaux [points d’entrée de modèle de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#landing_page_templates_endpoints) ont été introduits pour vous permettre de répertorier, de créer et de mettre à jour par programmation les données associées à un modèle de page de destination.

* API de ressources - Segments

Deux points d’entrée liés aux segments ont été introduits :

[Obtenir les segments](https://developer.adobe.com/marketo-apis/api/asset/get-segments)

[Obtenir la segmentation par ID](https://developer.adobe.com/marketo-apis/api/asset/get-segmentation-by-id)

* Correction d’un problème en raison duquel Get Folder by Name ne respectait pas le paramètre « workSpace ». [LM-61059]
* Plusieurs améliorations des performances des API d’objet personnalisé ont été apportées.

Publié le _2015-07-17_ par _David_

## Créer et associer des prospects, des entreprises et des opportunités à l’API REST Marketo

Pour tirer pleinement parti des analyses Marketo, il est essentiel de créer des associations correctes et solides entre les enregistrements de votre prospect, de votre société et de vos opportunités. Lorsque vous n’utilisez pas de synchronisation CRM native, la création de ces relations peut poser certaines difficultés. Nous passons donc en revue leur création aujourd’hui.

### Relations de l&#39;objet

Dans Marketo, il existe quelques relations essentielles pour établir complètement le compte rendu des performances des opportunités :

* Les prospects et les opportunités ont une relation multiple à multiple avec l’objet OpportunityRole.
* Le rôle d’opportunité comporte un champ leadId et un champ d’ID d’opportunité externe pour créer la relation du prospect à l’opportunité.
* Pour pouvoir bénéficier d’un filtre de liste dynamique A une opportunité , un prospect doit avoir un rôle d’opportunité associé à une opportunité.
* Les opportunités ont une relation multiple-à-un avec l’objet Company via le champ externalCompanyId .
* Les leads ont une relation un-à-plusieurs avec les sociétés via le champ externalCompanyId .
* Les opportunités sont attribuées à un programme en fonction du programme d’acquisition d’un prospect ou de son appartenance et de sa réussite dans un programme (voir [Comprendre l’attribution](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/reporting/revenue-cycle-analytics/revenue-tools/attribution/understanding-attribution)).

La création de ces relations dans l’ensemble de votre base de données principale vous permettra d’exploiter pleinement Marketo Analytics et de voir l’influence de vos programmes sur la création d’opportunités et les taux de succès.

### Sociétés

La façon la plus simple de bâtir ces relations est de commencer par la création d&#39;une entreprise. Cela nous permet de transmettre externalCompanyId à nos opportunités lors de la création, au lieu d’avoir à effectuer des appels API supplémentaires pour mettre à jour les opportunités après leur création. Selon la configuration existante, il peut s’agir d’une étape nécessaire, mais les nouveaux prospects et contacts nets avec les sociétés associées doivent faire ajouter ces enregistrements à votre instance Marketo pour que les relations soient créées. Examinons donc le code de création des enregistrements de société.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertCompanies {
 public List<JsonObject> input; //a list of Companies to use for input.  Each must have a member "externalCompanyId".
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters(externalCompanyId), idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertCompanies(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/companies.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertCompanies(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 //adds input to existing list, creates arraylist if it was built without a list
 public UpsertCompanies addCompanies(JsonObject... companies){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : companies) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
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
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our company records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertCompanies instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 //sets or replaces existing input with list
 public UpsertCompanies setInput(List input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertCompanies setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertCompanies setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

L’API companies fournit deux options de déduplication, définies par le paramètre dedupeBy dans la requête, « dedupeFields » et « idField ». Ils peuvent être récupérés explicitement en appelant la fonction Décrire les entreprises. Si dedupeBy n’est pas défini, la valeur par défaut est dedupeFields. Dans le cas des enregistrements de société, dedupeFields correspond toujours à « externalCompanyId », qui est une chaîne arbitraire définie par une source externe, et idField, correspondant au champ « marketoId », qui est un entier généré et renvoyé par Marketo après création. Selon la sélection pour dedupeBy, un externalCompanyId ou marketoId doit être inclus dans tout appel upsert pour un enregistrement d’entreprise. Ces mêmes exigences s’appliquent aux API d’objet d’opportunité et de rôle d’opportunité. Notre code expose deux constructeurs : l’un acceptant un seul argument d’un objet Auth, et l’autre qui accepte Auth et une liste d’enregistrements d’entreprise [JsonObject](https://github.com/ralfstx/minimal-json). Si la propriété a été construite sans liste d’entrée, les enregistrements d’entreprise doivent être ajoutés par le biais de la méthode addCompanies, qui vérifie que créer un ArrayList si l’entrée est nulle, puis ajoute tous les arguments JsonObject à la liste d’entrée. Voici un exemple d’utilisation :

```java
//Create a new company to associate to
JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
JsonObject companiesResult = upsertCompanies.postData();
System.out.println(companiesResult);
```

Nous créons un JsonObject de société unique avec un seul champ, `externalCompanyId`, puis nous créons une instance d’UpsertCompanies et ajoutons notre société à la liste d’entrée avec `addCompanies`.

### Opportunités

Tout comme les objets d’entreprise, l’API d’opportunité comporte un paramètre `dedupeBy`, acceptant « dedupeFields » ou « idField », correspondant respectivement à « externalopportunityid » et « marketoGUID ». Voici notre code, qui ressemble beaucoup à la classe UpsertCompanies :

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunities {
 public List<JsonObject> input; //a list of Opportunities to use for input.  Each must have a member "externalopportunityid".  Each can optionally include "externalCompanyId" for company association
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object


 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertOpportunities(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunities(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunities addOpportunities(JsonObject... opp){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : opp) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }

 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
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
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our Opportunity records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunities setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunities setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunities setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Les mêmes options de constructeur sont fournies, prenant un Auth ou un `Auth+List<JsonObject>`, ainsi qu’une méthode `addOpportunities` pour saisir des opportunités JsonObject. Voici un exemple d’utilisation :

```java
//Create some JsonObjects for Opportunity Data
JsonObject opp1 = new JsonObject().add("name", "opportunity1")
    .add("externalopportunityid", "Opportunity1Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");
JsonObject opp2 = new JsonObject().add("name", "opportunity2")
    .add("externalopportunityid", "Opportunity2Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");

//Create an Instance of UpsertOpportunities and POST it
UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                        .setAction("createOnly")
                        .addOpportunities(opp1, opp2);
JsonObject oppsResult = upsertOpps.postData();
System.out.println(oppsResult);
```

Ici, nous créons deux exemples d’opportunités, puis nous leur donnons des valeurs pour les champs name, externalopportunityid, externalCompanyId et externalCreatedDate. Nous n’avons pas encore discuté de externalCreatedDate, mais il est important d’utiliser , car il est traité comme le champ principal dans RCE pour le moment où une opportunité a été créée, ce qui le rend important pour une attribution correcte. Vous pouvez utiliser la logique commerciale de votre organisation pour déterminer ce que vous saisissez dans ce champ, en fonction du remplacement des données d’opportunité existantes ou de la création de nouvelles données à la volée. Nous créons notre instance d’UpsertOpportunities, puis ajoutons nos JsonObjects via addOpportunities. Maintenant que l’instance est configurée, vous pouvez pousser ceci vers Marketo avec postData et imprimer vos résultats

### Rôles

Les rôles sont assez similaires aux deux objets précédents, sauf qu&#39;ils ont une exigence légèrement différente lorsque dedupeBy est défini sur dedupeFields. Les rôles nécessitent que trois champs soient inclus lors de la création ou de la mise à jour d’un enregistrement au moyen de cette méthode, « leadId », « role » et « externalopportunityid ». « role » peut être n’importe quelle valeur de chaîne, mais les deux autres doivent faire référence respectivement à un identifiant valide d’un prospect et à un identifiant valide d’une opportunité.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunityRoles {
 public List<JsonObject> input; //Array of Opportunity Roles as JsonObjects, must have "leadId", "role" and "externalopprtunityid"
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate, defaults to createOrUpdate if unset
 public String dedupeBy;//select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object

 //Constructs an UpsertOpportunityRoles with Auth, but with no input set
 public UpsertOpportunityRoles(Auth auth) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities/roles.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunityRoles(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunityRoles addRoles(JsonObject... role){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : role) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }
 //executes the request to Marketo, body will be empty if input is not set
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");//"application/json" content-type is required.
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream();  //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }

 public JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject();
  JsonArray in = new JsonArray();
  for (JsonObject jo : input) {
   in.add(jo);
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action);
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy);
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunityRoles setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunityRoles setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunityRoles setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }

}
```

Nous suivons un modèle similaire pour la méthode constructors et addRoles comme dans les exemples précédents. Voici un exemple.

```java
//Create Some opp roles now
JsonObject opp1Role = new JsonObject()
                        .add("role", "Captain")
                        .add("externalopportunityid", opp1.get("externalopportunityid").asString())
                        .add("leadId", 318794);
JsonObject opp2Role = new JsonObject()
                        .add("role", "Commander")
                        .add("externalopportunityid", opp2.get("externalopportunityid").asString())
                        .add("leadId", 318795);

//Create an Instance of UpsertOpportunityRoles and POST it
UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
                        .setAction("createOnly")
                        .addRoles(opp1Role, opp2Role);
JsonObject rolesResult = upsertRoles.postData();
System.out.println(rolesResult);
```

Ici, nous créons les nouveaux objets Json pour nos 2 rôles d’exemple, nous ajoutons leurs champs de déduplication requis, nous extrayons l’ID d’opportunité externe des opportunités que nous avons déjà créées, puis nous les transmettons à Marketo.

### Assemblage

Voici l&#39;exemple complet de notre méthode principale :

```java
package dev.marketo.opportunities;

import com.eclipsesource.json.JsonObject;

public class App
{
    public static void main( String[] args )
    {
     //create an Instance of Auth
        Auth auth = new Auth("CLIENT_ID_CHANGE_ME", "CLIENT_SECRET_CHANGE_ME", "MARKETO_HOST_CHANGE_ME");

        //Create a new company to associate to
        JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
        UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
        JsonObject companiesResult = upsertCompanies.postData();
        System.out.println(companiesResult);

        //Create some JsonObjects for Opportunity Data
        JsonObject opp1 = new JsonObject().add("name", "opportunity1")
           .add("externalopportunityid", "Opportunity1Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");
        JsonObject opp2 = new JsonObject().add("name", "opportunity2")
           .add("externalopportunityid", "Opportunity2Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");

        //Create an Instance of UpsertOpportunities and POST it
        UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                                .setAction("createOnly")
                                .addOpportunities(opp1, opp2);
        JsonObject oppsResult = upsertOpps.postData();
        System.out.println(oppsResult);

        //Create Some opp roles now
        JsonObject opp1Role = new JsonObject()
           .add("role", "Captain")
           .add("externalopportunityid", opp1.get("externalopportunityid").asString())
           .add("leadId", 318794);
        JsonObject opp2Role = new JsonObject()
           .add("role", "Commander")
           .add("externalopportunityid", opp2.get("externalopportunityid").asString())
           .add("leadId", 318795);

        //Create an Instance of UpsertOpportunityRoles and POST it
        UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
           .setAction("createOnly")
           .addRoles(opp1Role, opp2Role);
        JsonObject rolesResult = upsertRoles.postData();
        System.out.println(rolesResult);

    }
}
```

Vous pouvez voir la séquence de création des entreprises, des opportunités et des rôles. Désormais, vous êtes prêt à synchroniser les données de votre entreprise et de votre opportunité avec Marketo.

Publié le _2015-08-07_ par _Kenny_

## Envoi d’e-mails transactionnels avec l’API REST Marketo : Partie 2, Contenu personnalisé

Cette semaine, nous étudions comment transmettre du contenu dynamique à nos e-mails via l’appel API Request Campaign. Request Campaign permet non seulement de déclencher des e-mails en externe, mais vous pouvez également remplacer le contenu de [Mes jetons](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program) dans un e-mail. Mes jetons sont du contenu réutilisable qui peut être personnalisé au niveau du programme ou du dossier marketing. Ils peuvent également simplement exister en tant qu’espaces réservés à remplacer par le biais de votre appel de campagne de demande.

### Création de votre e-mail

Pour personnaliser notre contenu, nous devons d’abord configurer un [programme](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program) et un [e-mail](https://experienceleague.adobe.com/fr/docs/marketo/using/home) dans Marketo. Pour générer notre contenu personnalisé, nous devons créer des jetons à l’intérieur du programme, puis les placer dans l’e-mail que nous allons envoyer. Pour plus de simplicité, nous n’utilisons qu’un seul jeton dans cet exemple, mais vous pouvez remplacer un nombre illimité de jetons dans un e-mail, dans l’e-mail de l’expéditeur, le nom de l’expéditeur, le destinataire de la réponse ou tout autre élément de contenu de l’e-mail. Créons donc un jeton de texte enrichi à remplacer et appelons-le « bodyReplacement ». Le texte enrichi nous permet de remplacer n’importe quel contenu du jeton par HTML arbitraire que nous voulons saisir. Les jetons ne peuvent pas être enregistrés s’ils sont vides. Continuez donc et insérez du texte d’espace réservé ici. Nous devons maintenant insérer notre jeton dans l’e-mail : ce jeton sera désormais accessible pour être remplacé par un appel de campagne de demande. Ce jeton peut être aussi simple qu’une seule ligne de texte qui doit être remplacée par e-mail, ou peut inclure presque toute la disposition de l’e-mail.

### Le code

Nous étendons le code de la semaine dernière pour transmettre des jetons personnalisés à notre appel de campagne de demande.

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
        String bodyReplacement = "<div class="replacedContent"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Cette fois, nous créons le contenu de notre jeton dans la variable bodyReplacement , puis nous utilisons la méthode addToken pour l’ajouter à la requête. addToken prend une clé et une valeur, puis crée une représentation JsonObject et l’ajoute au tableau de jetons interne. Elle est ensuite sérialisée pendant la méthode postData et crée un corps qui ressemble à ceci :

`{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}`

Combinée, notre sortie console ressemble à ceci :

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with&nbsp;...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"<apiuser@mktosupport.com>"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json?access_token=19d51b9a-ff60-4222-bbd5-be8b206f1d40:st
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

### Conclusion

Cette méthode peut être étendue de différentes manières, en modifiant le contenu des e-mails dans des sections de disposition individuelles ou en dehors des e-mails, ce qui permet de transmettre des valeurs personnalisées dans des tâches ou des moments intéressants. Chaque fois qu’un jeton peut être utilisé à partir d’un programme, il peut être personnalisé à l’aide de cette méthode. Une fonctionnalité similaire est également disponible avec l’appel [Planifier la campagne](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) qui vous permet de traiter les jetons dans une campagne par lots entière. Elles ne peuvent pas être personnalisées par lead, mais sont très utiles pour personnaliser le contenu d’un large éventail de leads.

Publié le _2015-07-24_ par _Kenny_

## Mise à jour des données de lead dans Marketo à l’aide de l’API

Il y a plus d’un an, nous avons publié Mise à jour des informations sur les clients et les prospects dans Marketo à l’aide de l’API . Cette publication présentait un exemple de code qui pouvait être exécuté de manière récurrente pour interroger un système externe à la recherche de mises à jour. L’idée était de récupérer les données externes et de les pousser dans Marketo pour mettre à jour les informations du prospect. L’exemple de code présenté utilisait notre API SOAP. Point d’entrée REST pour atteindre le même objectif. **Entrée du programme** Il est probable que vous deviez transformer les données du système externe en un format exploitable par les API REST Marketo. Puisque nous utilisons l’API de création/mise à jour de leads, les données doivent être formatées au format JSON qui est envoyé dans le corps de la requête. La meilleure explication est un exemple. Dans l’exemple de code Java ci-dessous, nous avons placé des données de prospect simulées dans un tableau de chaînes. Chaque chaîne suit les données pour chaque prospect : prénom, nom, adresse e-mail, intitulé de la fonction.

```java
private static String externalLeadData[] = {
        "Henry, Adams, <henry@superstar.com>, Director of Demand Generation",
        "Suzie, Smith, <ssmith@gmail.com>, VP Marketing"
};
```

L’exemple de code transforme le tableau en bloc JSON ci-dessous.

```json
{
"input":
    [
        {"firstName":"Henry", "lastName":"Adams", "email":"<henry@superstar.com>", "title":"Director of Demand Generation"},
        {"firstName":"Suzie", "lastName":"Smith", "email":"<ssmith@gmail.com>", "title":"VP Marketing"}
    ]
}
```

Chaque élément de tableau « entrée » correspond à un prospect individuel dans Marketo. Les éléments du tableau sont des objets JSON qui contiennent un ou plusieurs noms de champ de prospect Marketo et leurs valeurs respectives. Les noms des champs que vous décidez de spécifier (dans ce cas, firstName, lastName, email et title) doivent correspondre au nom de l’API REST défini pour l’abonnement Marketo. Vous trouverez les noms des API REST dans la section Gestion des champs du panneau d’administration Marketo en exportant les noms des champs. Les noms des champs seront exportés dans un fichier Excel, comme illustré ci-dessous. Vous pouvez également rechercher des noms de champ par programmation en appelant l’API [Décrire le prospect](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). Par exemple, voici un extrait de code de réponse Description qui contient le nom de l’API REST pour le champ Prénom .

```json
{
    "id": 29,
    "displayName": "First Name",
    "dataType": "string",
    "length": 255,
    "soap": {
        "name": "FirstName",
        "readOnly": false
    },
    "rest": {
        "name": "firstName",
        "readOnly": false
    }
},
```

**Logique de programme** Une fois la payload au format approprié, nous sommes prêts à appeler Créer/Mettre à jour des prospects. Notez que dans cet exemple, nous ne spécifions aucun des paramètres facultatifs. Le comportement par défaut consiste à créer ou mettre à jour des enregistrements de prospect (upsert), à utiliser l’e-mail à des fins de déduplication et à utiliser le traitement synchrone. Si l’appel à Créer/Mettre à jour des leads est réussi, alors le corps de la réponse contient un tableau « result » contenant l’ID de lead Marketo et le statut de l’opération Créer/Mettre à jour . Selon le paramètre « action » que vous avez transmis dans la requête, le statut peut être « mis à jour », « créé » ou « ignoré ». Pour poursuivre notre exemple, supposons que le premier lead n&#39;existe pas (Henry Adams) et que le second lead existe (Suzie Smith). Une réponse similaire à la suivante indiquerait que le premier prospect a été créé et que le second a été mis à jour.

```json
{
    "success":true,
    "requestId":"118f8#14f1a0b82fc",
    "result":[
        {
            "id":318798,
            "status":"created"
        },
        {
            "id":318797,
            "status":"updated"
        }
    ]
}
```

C&#39;est tout pour l&#39;instant. Heureux codage ! **Code du programme**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStreamWriter;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

public class CreateUpdateLeads {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            { INSERT YOUR CUSTOM SERVICE DATA HERE };

    //
    // Mock up data that could be read from an external data source.
    // An array of CSV strings the form:
    //   "firstName, lastName, email, title"
    private static String externalLeadData[] = {
        "Henry, Adams, henry@superstar.com, Director of Demand Generation",
        "Suzie, Smith, ssmith@gmail.com, VP Marketing"
    };

    public static void main(String[] args) {
        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URL for Create/Update Leads
        String createUpdateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", baseUrl, accessToken);

        // Convert String array into JsonArray to pass as part of request body
        JsonArray input = convertExternalLeadDataToJson(externalLeadData);

        // Build request body JSON
        JsonObject requestObj = new JsonObject();
        requestObj.add("input", input);

        // Call Create/Update Lead API
        JsonArray result = new JsonArray();
        JsonObject leadObj = JsonObject.readFrom(httpRequest("POST", createUpdateLeadsUrl, requestObj.toString()));
        if (leadObj.get("success").asBoolean()) {
            if (leadObj.get("result") != null) {
                result = leadObj.get("result").asArray();
            }
        }

        // Print out results object
        System.out.println(result);
        System.exit(0);
    }

    // Convert array of CSV formatted Strings into an array of JSON objects
    private static JsonArray convertExternalLeadDataToJson(String input[]) {
        JsonArray output = new JsonArray();

        // Loop through each CSV row in array
        for (int i = 0; i < input.length; i++) {
            // Split CSV row into separate fields
            List items = Arrays.asList(input[i].split(","));

            // Add fields to JSON object
            JsonObject lead = new JsonObject();
            lead.add("firstName", items.get(0));
            lead.add("lastName", items.get(1));
            lead.add("email", items.get(2));
            lead.add("title", items.get(3));
            output.add(lead);
        }
        return output;
    }

    // Perform HTTP request
    private static String httpRequest(String method, String endpoint, String body) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setDoOutput(true);
            urlConn.setRequestMethod(method);
            switch (method) {
                case "GET":
                    break;
                case "POST":
                    urlConn.setRequestProperty("Content-type", "application/json");
                    urlConn.setRequestProperty("accept", "text/json");
                    OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                    wr.write(body);
                    wr.flush();
                    break;
                default:
                    System.out.println("Error: Invalid method.");
                    return data;
            }
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Publié le _2015-08-14_ par _David_

## Ajout de données SalesPerson à Marketo

Les nouvelles API SalesPerson vous permettent d’associer librement des prospects Marketo à des enregistrements SalesPerson dans des instances sans intégration native de CRM. Cela permet l’utilisation de {{lead.Lead Owner Email Address}} et des champs et jetons associés dans Marketo.

### Création d&#39;enregistrements SalesPerson

Pour associer des leads à des enregistrements SalesPerson, nous devons d&#39;abord saisir nos enregistrements SalesPerson dans Marketo. Voici un exemple de classe en PHP :

```php
<?php

class SalesPerson{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $dedupeBy;//dedupeFields or idField
 private $input;//array of salesperson objects for input

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->dedupeBy)){
   $body->dedupeBy = $this->dedupeBy;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/salespersons.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction($action){
  return $this->action;
 }
 public function setDedupeBy($dedupeBy){
  $this->dedupeBy = $dedupeBy;
 }
 public function getDedupeBy($dedupeBy){
  return $this->dedupeBy;
 }
 public function addSalesPersons($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else{
   $this->input = $input;
  }
 }
 public function getInput(){
  return $this->input;
 }

}
```

Dans la classe ci-dessus, la variable $input est un tableau d’objets stdClass avec des membres possibles :

* externalSalesPersonId (valide uniquement lors de la création)
* id (uniquement en tant que mode updateOnly intégré)
* E-mail
* télécopie
* Prénom
* Nom
* mobilePhone
* téléphone
* titre

Vous pouvez obtenir des informations plus détaillées sur les types et les longueurs de champs via l’appel Describe SalesPerson.

### Synchronisation des leads

Voici un exemple de classe rapide pour synchroniser les prospects dont nous avons besoin :

```php
<?php

class Leads{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $lookupField;//dedupeFields or idField
 private $input;//array of salesperson objects for input
 private $asyncProcessing;//specify whether input should be processed asynchronously
 private $partitionName;//partition name for update if requires

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->lookupField)){
   $body->lookupField = $this->lookupField;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/leads.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction(){
  return $this->action;
 }
 public function setDedupeBy($lookupField){
  $this->dedupeBy = $lookupField;
 }
 public function getDedupeBy(){
  return $this->dedupeBy;
 }
 public function addLeads($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else if (is_array($input)){
   $this->input = $input;
  }else{
   $this->input = [$input];
  }
 }
 public function getInput(){
  return $this->input;
 }
 public function setAsyncProcessing($asyncProcessing){
  $this->asyncProcessing = $asyncProcessing;
 }
 public function getAsyncProcessing() {
  return $this->asyncProcessing;
 }
 public function setPartitionName($partitionName){
  $this->partitionName = $partitionName;
 }
 public function getPartitionName() {
  return $this->partitionName;
 }

}
```

### Processus

Voici un exemple de création de deux enregistrements de vendeur et de leur association à deux enregistrements de prospect :

```php
<?php

require 'Auth.php';
require 'SalesPerson.php';
require 'Leads.php';

//Create an Auth object for authentication
$auth = new Auth("https://284-RPR-133.mktorest.com", "7f99bd49-0e0e-4715-a72a-0c9319f75552", "O5lZHhrQDfDKRhulY89IU42PfdhRSe6m");

//Compose new SalesPerson Records
$sales1 = new stdClass();
$sales1->externalSalesPersonId = "SalesPerson 1";
$sales1->email = "sales1@example.com";
$sales1->firstName = "Jane";
$sales1->lastName = "Doe";

$sales2 = new stdClass();
$sales2->externalSalesPersonId = "SalesPerson 2";
$sales2->email = "sales2@example.com";
$sales2->firstName = "John";
$sales2->lastName = "Doe";

//Compose lead records
$lead1 = new stdClass();
$lead1->email = "marketoDev@example.com";
//Add the external id of the desired salesperson
$lead1->externalSalesPersonId = $sales1->externalSalesPersonId;

$lead2 = new stdClass();
$lead2->email = "devBlog@example.com";
$lead2->externalSalesPersonId = $sales2->externalSalesPersonId;

//Create a new instance of SalesPerson to submit records
$salesPerson = new SalesPerson($auth, [$sales1, $sales2]);
$spResponse = $salesPerson->postData();
print_r($spResponse . "rn");
$json = json_decode($spResponse);

//Check for success on synching SalesPersons
if ($json->success){
 $leads = new Leads($auth, [$lead1, $lead2]);
 $leadsResponse = $leads->postData();
 print_r($leadsResponse . "rn");
}else{
 print_r("SalesPerson request failed with errors:rn");
 foreach ($json->errors as $error){
  print_r("code: " . $error->code . ", message: " . $error->message . "rn");
 }
}
```

Pour nos classes d’exemple, nous créons simplement des objets stdClass pour représenter nos enregistrements SalesPerson et Lead qui doivent être synchronisés, avec chaque champ souhaité ajouté en tant que membre. Après l’exécution de ce code, les champs E-mail du propriétaire du lead, Prénom du propriétaire du lead et Nom du propriétaire du lead seront renseignés dans les <marketoDev@example.com> et <devBlog@example.com> de leads. Ils pourront ainsi utiliser les jetons appropriés pour ces champs et être filtrés par les filtres de liste dynamique appropriés.

### Classe d’authentification

```php
<?php

class Auth{
 private $host;//host of the target Marketo instance
 private $clientId;//client Id
 private $clientSecret;//client secret
 private $token;//access_token
 private $expiry;

 function _construct($host, $clientId, $clientSecret){
  $this->host = $host;
  $this->clientId = $clientId;
  $this->clientSecret = $clientSecret;
 }
 public function getToken(){
  if (!isset($this->token) || $this->expiry > time()){
   $data = $this->getData();
   $this->expiry = time() + $data->expires_in;
   $this->token = $data->access_token;
   return $this->token;
  }else{
   return $this->token;
  }
 }
 private function getData(){
  $url = $this->host . "/identity/oauth/token?grant_type=client_credentials&client_id=" . $this->clientId . "&client_secret="
     . $this->clientSecret;
  $ch = curl_init($url);
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  $response = curl_exec($ch);
  $json = json_decode($response);
  curl_close($ch);
  return $json;
 }
 public function getHost(){
  return $this->host;
 }
}
```

Publié le _2015-08-21_ par _Kenny_

## Bonnes pratiques pour les utilisateurs d’API et les services personnalisés

Les API REST Marketo utilisent des services personnalisés pour l’authentification et chacun de ces services appartient à un utilisateur Marketo uniquement doté d’une API. Les fonctionnalités de chaque service personnalisé sont déterminées par les autorisations de chaque rôle affecté à cet utilisateur. L’affectation d’utilisateurs individuels et de services personnalisés à vos intégrations vous offre plusieurs avantages :

* Vous pouvez affiner les [autorisations](/help/rest-api/custom-services.md) accordées à chaque service individuel par le biais du rôle attribué à votre utilisateur.
* Vous pouvez empêcher des services web individuels d’effectuer des appels vers votre instance en supprimant le service personnalisé correspondant, sans désactiver les autres.
* La création de rapports sur l’utilisation des appels API sera ventilée par utilisateur, ce qui vous permettra d’identifier une utilisation élevée et anormale
* Il est plus facile de déterminer à quelles données chaque service web a accès
* Les instances Workspace peuvent restreindre l’accès à des unités opérationnelles spécifiques en n’attribuant des rôles qu’à des espaces de travail accessibles

### Utilisation de l’API

Chacun de vos utilisateurs d’API est signalé individuellement dans le rapport d’utilisation de l’API. La division de vos services web par utilisateur vous permet donc de rendre compte facilement de l’utilisation de chacune de vos intégrations. Si le nombre d’appels d’API à votre instance dépasse la limite et provoque l’échec des appels suivants, l’utilisation de cette pratique vous permet de tenir compte du volume de chacun de vos services et d’évaluer comment résoudre le problème. Affichez votre utilisation en accédant à Admin -> Services web et en cliquant sur le nombre d’appels au cours des 7 derniers jours.

### Désactivation d’un service

Si une intégration a des effets indésirables, il peut être fastidieux et difficile de la désactiver si vous ne leur avez pas affecté un service personnalisé individuel. Les ventiler un par un facilite autant la suppression du service fautif dans votre Admin -> Point de lancement.

### Gestion de Workspace

Pour les abonnements Marketo Enterprise, il est courant qu’un service n’ait besoin d’accéder qu’à un seul espace de travail, ce qui peut être [appliqué en attribuant le rôle à l’utilisateur de l’API](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/allow-user-access-to-a-workspace). Chaque rôle d’utilisateur peut être attribué globalement ou par espace de travail, de sorte que l’accès peut être limité dans les espaces de travail là où cela est approprié, en fournissant les autorisations les plus minimales possibles.

Publié le _2015-08-28_ par _Kenny_

## Spécification des partitions de lead à l’aide de l’API REST

**Partitionnement de lead** les partitions de lead Marketo offrent un moyen pratique d’isoler les leads. Les partitions peuvent permettre à différents groupes marketing au sein de votre organisation de partager une seule instance Marketo. Pour plus d’informations, voir [Présentation des espaces de travail et des partitions de lead](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/understanding-workspaces-and-person-partitions). Supposons que vous utilisiez des partitions de prospect et que vous créiez des prospects par programmation à l’aide de l’API REST Marketo. Comment vous assurez-vous que les prospects que vous créez se retrouvent dans la bonne partition ? Cet article vous montre comment ! Dans le cadre de cet exemple, nous utiliserons des espaces de travail et des partitions pour isoler nos prospects en fonction de la zone géographique.

Tout d’abord, nous définirons un espace de travail appelé « Pays ». Ensuite, nous créons deux partitions dans cet espace de travail appelées « Mexique » et « Canada ».  **Créer un prospect dans la partition** Supposons maintenant que nous voulions créer deux prospects dans la partition « Mexique ». Pour créer des prospects, nous appelons le . Pour spécifier la partition, nous devons inclure l’attribut « partitionName » dans le corps de la requête. Comment savons-nous quoi utiliser pour la valeur partitionName ? Nous pouvons récupérer une liste de valeurs de nom de partition valides pour notre instance en appelant l’API [Get Lead Partitions](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeProgramMemberUsingGET) comme suit :

`GET /rest/v1/leads/partitions.json`

```json
{
    "requestId": "20ae#14f9a1a5a30",
    "result": [
        {
            "id": 1,
            "name": "Default",
            "description": "Initial system lead partition"
        },
        {
            "id": 2,
            "name": "Mexico",
            "description": "Leads that live in Mexico"
        },
        {
            "id": 3,
            "name": "Canada",
            "description": "Leads that live in Canada"
        }
    ],
    "success": true
}
```

Dans ce cas, la valeur correcte est « Mexico ». Nous la transmettons ensuite à Créer/Mettre à jour des prospects comme suit :

`POST /rest/v1/leads.json`

```json
{
    "input": [
        {"email":"enrique.pena-nieto@gob.mx"},
        {"email":"angelica.rivera@gob.mx"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Voici nos nouveaux prospects dans Marketo.  **Mettre à jour le lead dans la partition** Pour mettre à jour les leads existants dans une partition, il suffit d’appeler Create/Update Leads et de spécifier partitionName comme nous l’avons fait auparavant. Dans le cadre de cette mise à jour, nous attribuerons un prénom, un nom et un titre aux prospects que nous avons créés ci-dessus.

`POST /rest/v1/leads.json`

```json
{
"input": [
        {"email":"enrique.pena-nieto@gob.mx", "firstName":"Enrique", "lastName":"Pena Neito", "title": "El Presidente"},
        {"email":"angelica.rivera@gob.mx", "firstName":"Angelica", "lastName": "Rivera", "title": "Primera Dama"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Voici nos prospects récemment mis à jour dans Marketo.
**Identifier la partition d’un prospect** Comment savons-nous dans quelle partition se trouve un prospect ? Pour ce faire, nous utilisons l’API [Get Lead by Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) et spécifions « leadPartitionId » dans le paramètre de requête « fields ». Dans ce cas, nous récupérerons les informations de l’ID de prospect 318816 que nous avons créé ci-dessus.

`GET /rest/v1/lead/318816.json?fields=leadPartitionId,email,firstName,lastName,title`

```json
{
    "requestId": "5c57#14f9a495b1f",
    "result": [
        {
            "id": 318816,
            "lastName": "Pena Neito",
            "title": "El Presidente",
            "email": "enrique.pena-nieto@gob.mx",
            "firstName": "Enrique",
            "leadPartitionId": 2
        }
    ],
    "success": true
}
```

Notez que nous récupérons leadPartitionId plutôt que partitionName. Pour obtenir le partitionName, nous devons revoir la sortie de l’API Get Lead Partitions ci-dessus.

```json
{
    "id": 2,
    "name": "Mexico",
    "description": "Leads that live in Mexico"
},
```

Dans ce cas, une valeur leadPartitionId de 2 correspond à partitionName de « Mexico ». C&#39;est tout pour l&#39;instant. Joyeux découpage !

Publié le _2015-09-04_ par _David_

## Comparaison des champs de score dans les scripts d’e-mail Marketo

**Note :** Ceci est un message invité de Cathal Moran. Cathal est consultante en solutions, travaillant au bureau EMEA de Marketo à Dublin, en Irlande.

### Comparaison des champs de score

De nombreux clients Marketo, en particulier ceux qui se concentrent sur la vente croisée, disposent de plusieurs champs de score. Ils sont souvent utilisés pour mesurer l’intérêt d’un prospect pour un produit/domaine particulier. Imaginez que je vends des pommes et des bananes, si un prospect a un score de 50 pour les pommes et de 10 pour les bananes, alors il est clair où se trouve la préférence, ne serait-il pas agréable si mon contenu reflétait cette préférence. Les scripts de courrier électronique peuvent être utilisés pour comparer les scores et personnaliser le contenu d’un courrier électronique en fonction du score le plus élevé (ou le plus bas) pour ce prospect particulier qui reçoit le courrier électronique.

### Comme je veux comparer 2 nombres, je dois convertir mes valeurs de champ en entiers

```
#set ($score1 = $math.toInteger(${lead.Apple_Score}))
#set ($score2 = $math.toInteger(${lead.Banana_Score}))
##check if the lead score is greater than feature score
#if($score1 >= $score2)
                ##if Apple score is greater
                #set($Interest = "Special offer on Apples")
##check is the feature score is higher
#elseif($score2 >= $score1)
                ##if Feature score is greater
                #set($Interest = "Special offer on Bananas")
#else
                ##otherwise
                #set($Interest = "Special offer on Fruit")
#end
##display the Interest as content
${Interest}
```

Dans l’exemple ci-dessus, je ne fais que personnaliser le texte, mais je pourrais tout aussi facilement afficher, par exemple, une image différente en fonction du score le plus élevé.

Publié le _2015-09-14_ par _Kenny_

## Envoyer un formulaire Marketo en arrière-plan

Lorsque votre entreprise dispose de nombreuses plateformes différentes pour héberger le contenu web et les données client, il devient assez courant de devoir effectuer des envois parallèles à partir d’un formulaire afin que les données obtenues puissent être collectées sur des plateformes distinctes. Il existe plusieurs stratégies pour ce faire, mais la meilleure est souvent la plus simple : utiliser l’API Forms 2 pour envoyer un formulaire Marketo masqué. Cela fonctionne avec tout nouveau formulaire Marketo, mais dans l’idéal, vous devez créer un formulaire vide pour celui-ci, qui ne comporte aucun champ. Cela permet de s’assurer que le formulaire ne charge pas plus de données que nécessaire, puisque nous n’avons pas besoin d’effectuer de rendu. Il vous suffit maintenant d’extraire le [code incorporé](https://experienceleague.adobe.com/fr/docs/marketo/using/home) de votre formulaire et de l’ajouter au corps de la page souhaitée, en apportant une petite modification. Votre code incorporé comprend un élément de formulaire comme suit :

`<form id="mktoForm_1068"></form>`

Vous souhaiterez ajouter &#39;style=« display:none« &#39; à l’élément afin qu’il ne soit pas visible, comme dans l’exemple suivant :

`<form id="mktoForm_1068" style="display:none"></form>`

Une fois le formulaire incorporé et masqué, le code permettant d’envoyer le formulaire est très simple :

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"test@example.com",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms envoyé de cette manière se comportera exactement comme si le prospect avait rempli et envoyé un formulaire visible. Le déclenchement de l’envoi varie d’une implémentation à l’autre, car chacune comporte une action différente pour l’inviter, mais vous pouvez la faire se produire pratiquement n’importe quelle action. La partie importante consiste à définir correctement vos champs et valeurs. Veillez à utiliser le nom de l’API SOAP de vos champs que vous trouverez avec [Exporter les noms de champ](/help/rest-api/list-of-standard-fields.md) pour assurer un envoi correct des valeurs.

Migration à partir du lead associé de Munchkin

L’envoi d’un formulaire d’arrière-plan est l’une des méthodes de remplacement recommandées pour le prospect associé Munchkin. L’exemple de page HTML ci-dessous illustre la migration à un niveau élevé, en réutilisant les mêmes valeurs qui sont envoyées au prospect associé.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName('head')[0].appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //submit the form
            form.submit();


        })
    </script>
</body>
</html>
```

Publié le _2015-09-25_ par _Kenny_

## Traitement des exceptions et des erreurs de l’API REST Marketo : Partie 1

L’API REST Marketo peut renvoyer une exception ou une erreur que, pour des raisons de commodité, nous appellerons simplement des erreurs à partir de maintenant. Les erreurs peuvent se produire à trois niveaux différents :

* Au niveau HTTP, ces erreurs sont signalées par un code 4xx
* Niveau de réponse : ces erreurs sont incluses dans le tableau « erreurs » de la réponse JSON
* Au niveau de l’enregistrement, ces erreurs sont incluses dans le tableau « résultat » de la réponse JSON et sont indiquées sur une base d’enregistrement individuel avec le champ « statut » et le tableau « raisons »

### Erreurs HTTP

Dans des circonstances de fonctionnement normales, Marketo ne doit renvoyer que deux erreurs de statut HTTP : 413 Entité de requête trop grande et 414 URI de requête trop long. Vous pouvez les récupérer en interceptant l’erreur, en modifiant la requête et en réessayant. Toutefois, avec les pratiques de codage intelligent, vous ne devriez jamais les rencontrer en mode sauvage. Marketo renvoie la valeur 413 si la payload de la requête dépasse 1 Mo, ou 10 Mo dans le cas du [prospect d’importation](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads). Dans la plupart des scénarios, il est peu probable que ces limites soient atteintes. Cependant, l’ajout d’une vérification à la taille de la requête et le déplacement de tous les enregistrements entraînant le dépassement de la limite vers une nouvelle requête doivent éviter toute circonstance entraînant le renvoi de cette erreur par un point d’entrée. 414 sera renvoyé lorsque l’URI d’une requête GET dépasse 8 Ko. Pour l’éviter, vérifiez la longueur de votre chaîne de requête pour voir si elle dépasse cette limite. S’il remplace votre requête par une méthode POST, saisissez votre chaîne de requête en tant que corps de la requête avec le paramètre supplémentaire « _method=GET ». Cela annule la limitation des URI. Il est rare d&#39;atteindre cette limite dans la plupart des cas, mais cela est assez courant lors de la récupération de lots volumineux d&#39;enregistrements avec de longues valeurs de filtre individuel, comme un GUID.

### Erreurs au niveau de la réponse

Des erreurs de niveau de réponse sont présentes lorsque le paramètre « success » de la réponse est défini sur false. Chaque entrée dans « erreurs » comporte deux membres, « code » un nombre, soit 6xx ou 7xx, et un « message » donnant la raison en texte brut de l&#39;erreur. Les codes 6xx indiquent toujours qu’une requête a complètement échoué et n’a pas été exécutée. Un exemple est un 601, « Jeton d’accès non valide », qui peut être récupéré en s’authentifiant à nouveau et en transmettant le nouveau jeton d’accès avec la requête. Les erreurs 7xx indiquent que la requête a échoué, soit parce qu’aucune donnée n’a été renvoyée, soit parce que la requête a été paramétrée de manière incorrecte, par exemple en incluant une date non valide, ou en raison d’un paramètre obligatoire manquant.

### Erreurs Au Niveau Des Enregistrements

Les erreurs de niveau enregistrement indiquent qu&#39;une opération n&#39;a pas pu être effectuée pour un enregistrement individuel, mais que la demande elle-même était valide. Elles se produisent dans des enregistrements individuels dans le tableau « résultat » d’une réponse. Le champ « statut » de ces enregistrements sera « ignoré » et un tableau « raisons » sera présent. Chaque raison contient un membre « code » et un membre « message ». Le code sera toujours 1xxx et le message indiquera pourquoi l’enregistrement a été ignoré. Par exemple, lorsqu’une demande de création/mise à jour de leads comporte une « action » définie sur « createOnly », mais qu’un lead existe déjà pour l’une des clés dans les enregistrements envoyés. Ce cas renvoie un code de 1005 et un message « Le prospect existe déjà ». Dans les publications suivantes, nous allons examiner les erreurs récupérables et quelques exemples de la manière de les gérer dans votre code.

Publié le _2015-10-09_ par _Kenny_

## Guest Post : connecteurs SQL directs à la base de données Marketo

**Voici un message de Sumit Sarkar de Progress, Inc.**

**Connecteurs SQL à la base de données Marketo** Marketo dispose d’API bien documentées pour accéder aux données. Cependant, les entreprises ont parfois besoin d’un accès SQL direct. Les lignes de démarcation entre le marketing et l’informatique sont en train de s’estomper dans les boutiques Marketo, ce qui accroît la demande pour une connectivité SQL basée sur des normes. Les connecteurs SQL directs vers Marketo sont disponibles via [Progress DataDirect Cloud](https://www.progress.com/connectors/cloud-data-integration). DataDirect Cloud est un service de connectivité des données qui expose les données Marketo par le biais de normes ouvertes du secteur pour l’accès SQL sur ODBC (« Open Database Connectivity ») ou JDBC (« Java Database Connectivity ») ; et REST avec OData (« Open Data Protocol »). Vous trouverez ci-dessous quelques cas pratiques courants pour connecter des données Marketo prêtes à l’emploi à l’aide de DataDirect Cloud :

* Découverte et visualisation de données (Qlik, Tableau, SAP Lumira)
* Enterprise Reporting (SAP Business Objects, Microstrategy, Cognos)
* Intégration de données (SQL Server Integration Services - SSIS, Oracle Data Integrator - ODI, Informatica)
* Fédération de données (SQL Server Linked Server, SAP Hana SDA, Oracle Database Gateway)
* Requête ad hoc (Microsoft Office, DB Visualizer, Aqua Data Studio)
* Préparation Des Données (Alteryx, Trifecta, Paxata)

**Comment fonctionne l’accès SQL à Marketo ?**

* DataDirect Cloud crée un schéma logique pour les données exposées par les API d&#39;intégration de Marketo.
* DataDirect Cloud traite les requêtes SQL des clients légers ODBC ou JDBC à l&#39;aide d&#39;un moteur de requête en temps réel élastique sur les données extraites des API Marketo.
* La connectivité cloud de DataDirect est directe et en temps réel, sans duplication des données.
* Le Cloud Service DataDirect abstrait l’API Marketo de sorte que les utilisateurs finaux n’aient pas à se soucier des modifications de version de l’API ou de l’utilisation de SOAP ou de REST.

DataDirect développe ce style de connectivité aux sources de données SaaS depuis 2006, en commençant par le premier pilote ODBC Salesforce construit sur ses API de service web. **Prise en main de la connexion à Marketo:**

1. [S’inscrire pour une connexion cloud DataDirect](https://pacific.progress.com/console/register?productName=d2c&ignoreCookie=true)
1. Cliquez sur « Sources de données », puis sur le bouton « +Nouveau Source de données »
1. Sélectionnez « Marketo » et saisissez les informations de connexion. Contactez votre administrateur Marketo ou connectez-vous pour trouver [les informations de connexion à l’intégration de SOAP](/help/soap-api/soap-api.md).
1. Cliquez sur le bouton Tester la connexion. Notez qu’il existe un onglet OData pour produire des OData à partir de Marketo et nous en discuterons dans un prochain article de blog.
1. Cliquez sur « Tests SQL » si vous souhaitez inspecter le schéma Marketo exposé ou émettre des requêtes SQL de base à partir de l’interface utilisateur.
1. Cliquez sur « Téléchargements » sur la gauche et sélectionnez le pilote ODBC ou JDBC du cloud DataDirect à installer pour votre application et votre plateforme.
1. Une fois le pilote ODBC ou JDBC du cloud DataDirect installé, vous pouvez connecter n’importe quelle application standard à Marketo.

Voici un exemple vidéo de [connexion à l’aide du client ODBC Cloud DataDirect](https://www.youtube.com/watch?v=H6PHra56Iig). Voici d’autres tutoriels sur le cloud DataDirect qui s’appliquent à Marketo :

* [SAP Data Analytics](http://scn.sap.com/community/lumira/blog/2015/08/05/connect-sap-lumira-to-eloqua-marketo-google-analytics)
* [Rapports d’entreprise de Microstrategy](https://community.microstrategy.com/t5/Tech-Corner/What-MSTR-developers-should-know-about-Cloud-Data-Sources/ba-p/253083)
* [Intégration des données Oracle](https://www.ateam-oracle.com/post/a-universal-cloud-applications-adapter-for-odi/)

**R&amp;D : défis liés à la création d’une connectivité SQL entre les sources cloud telles que Marketo**

Toutes les API SaaS n’exposent pas un langage de requête standard. Dans ce cas, l’équipe d’ingénieurs examine chaque objet individuellement. Chaque objet peut être exposé avec une API différente avec des règles uniques pour l’appel, le filtrage de recherche, etc. Un effort important a été nécessaire pour fournir une expérience d’interrogation standard sur l’ensemble du modèle de données.

Gestion des fonctionnalités de jointure complètes. Dans les cas où les API SaaS ne prennent pas en charge un langage de requête avec la fonctionnalité JOIN, l’équipe d’ingénieurs doit effectuer cette opération. Cela nécessite une traduction de SQL pour appeler efficacement les API Marketo afin de renvoyer la quantité minimale de données avant d’effectuer la jointure. Lors de la jonction de deux objets très volumineux, la couche d’accès aux données peut utiliser des ressources considérables sur le serveur d’applications ou le bureau. Par conséquent, le déploiement de la couche d’accès aux données sur un service cloud élastique tel que DataDirect Cloud est très logique pour deux raisons :

1. Performances plus rapides et utilisation de moins de ressources mémoire/CPU sur le serveur d’applications ou le bureau client
1. Tirez parti de la bande passante supérieure entre le cloud DataDirect et Marketo, où les jeux de données pré-joints sont échangés.

Comment gérer les modèles de données ? Est-elle statique ou dynamique ? Comment les modifications sont-elles détectées et communiquées au client ? Chaque source de données SaaS est différente et, dans le cas de Marketo, il est préférable d’interroger certains objets au moyen de vues, et d’autres au moyen de tableaux. La gestion de cette matrice de modèles et d&#39;objets de données à travers toutes les sources SaaS a certainement été un défi.

**Référence cloud Marketo et DataDirect pour les développeurs :**

* Propriétés de connexion Marketo ([lien vers les documents](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* SQL et extensions pris en charge avec Marketo ([lien vers la documentation](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html,-hubspot,-and-marketo.html))
* Tableaux et vues Marketo exposés ([lien vers les documents](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* Messages d’erreur courants renvoyés par Marketo ([lien vers la documentation](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))

**Biographie :** Sumit Sarkar est évangéliste en chef des données chez Progress, avec plus de 10 ans d&#39;expérience dans le domaine de la connectivité des données. Consultant de premier plan au monde sur la connectivité des normes de données ouvertes avec les données en nuage, Sumit s&#39;intéresse notamment à l&#39;optimisation de la performance de la couche d&#39;accès aux données pour laquelle il a développé une technologie en attente de brevet pour son analyse, à l&#39;intelligence d&#39;affaires et à l&#39;entreposage de données pour les plateformes SaaS, et à la connectivité des données pour les environnements PaaS, en mettant l&#39;accent sur les normes telles qu&#39;ODBC, JDBC, ADO.NET et ODATA. Il est consultant certifié IBM pour IBM Cognos Business Intelligence et membre de TDWI. Il a présenté des sessions sur la connectivité des données lors de différentes conférences, notamment Dreamforce, Oracle OpenWorld, Strata Hadoop, MongoDB World et SAP Analytics et Business Objects Conference, entre autres. Twitter : @SAsInSumit LinkedIn : [www.linkedin.com/in/meetsumit](http://www.linkedin.com/in/meetsumit)

Publié le _2015-12-07_ par _Kenny_

## Créer un tableau de bord pour l’utilisation de l’API et le nombre d’erreurs

En tant que consommateur d’API Marketo, il s’agit d’informations utiles que vous devez surveiller. Et si vous pouviez obtenir des données d’utilisation historiques pour aider à détecter les tendances au fil du temps ? Et si vous pouviez obtenir un résumé des codes d’erreur API pour vous aider à mesurer l’intégrité de votre intégration ? En tant que partenaire technologique de Marketo, que se passerait-il si vous pouviez obtenir des données d’utilisation et d’erreur sur tous vos comptes clients dans un seul tableau de bord ? Ce billet vous fournira une approche pour répondre aux questions ci-dessus. Bouclez-vous, c&#39;est parti !
**Tâche planifiée pour la récupération des statistiques** Créons une application qui récupère les données d’utilisation et d’erreur à l’aide des points d’entrée [Obtenir l’utilisation quotidienne](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLast7DaysErrorsUsingGET) et [Obtenir les erreurs quotidiennes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyErrorsUsingGET). L’application est conçue pour fonctionner une fois par jour. Chaque fois que l’application s’exécute, elle ajoute l’équivalent d’une journée de données d’utilisation à un fichier et l’équivalent d’une journée de données d’erreur à un autre fichier. Au début de chaque mois, une nouvelle paire de fichiers est créée. Ces fichiers serviront de documents historiques auxquels nous pourrons accéder à tout moment. Voici la logique de l’application...

* Lisez les informations du compte Marketo (ID Munchkin et Informations d’identification du client) à partir d’une source externe. Remarque : cette source doit être sécurisée pour empêcher d’autres utilisateurs d’accéder aux données du compte.
* Effectuez une itération sur chaque compte et...
   * Appelez Get Daily Usage pour récupérer les données d&#39;utilisation pendant un jour
   * Ajout de données d’utilisation quotidienne à un fichier « d’utilisation » mensuel
   * Appelez Get Daily Errors pour récupérer les données d&#39;erreurs d&#39;une journée
   * Ajouter des données d’erreurs quotidiennes à un fichier d’« erreurs » mensuel

Format du fichier de sortie Le format des fichiers de sortie est JSON, qui correspond au tableau « result » renvoyé par les appels d’API respectifs (utilisation et erreur). Chaque élément du tableau « result » est un objet JSON qui contient des données pour un jour. Dénomination du fichier de sortie Les fichiers de sortie sont nommés comme suit :

`<type>_<yyyy>_<mm>_<account>.json`

Où,

`<type>` - Type de données (« utilisation » ou « erreurs ») `<yyyy>` - Année (4 chiffres) `<mm>` - Mois (2 chiffres) `<account>` - ID de compte (ID Munchkin)

Exemples de fichier de sortie usage_2015_10_111-AAA-222.json

```json
[
    { "date": "2015-10-15", "total": 0, "users" : [] },
    { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
    { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] },
]
```

`errors_2015_10_111-AAA-222.json:`

```json
[
    { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
    { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
    { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
]
```

Le code de cette application est présenté à la fin de cette publication (Stats.java). **Service Web de statistiques** Nous avons donc besoin d’un moyen d’intégrer ces données dans notre navigateur. La proposition consiste à créer un service web pour diffuser les données. Le service web utilise les fichiers de sortie de l’application, puis renvoie les données au navigateur sous une forme facile à présenter. Par souci de simplicité et pour contourner les restrictions des politiques sur la même origine, nous utiliserons le modèle [JSONP](https://en.wikipedia.org/wiki/JSONP). Voici la spécification de point d’entrée REST proposée pour le service web externe : **URI** : /stats **Méthode** : GET

**Paramètre**

**Description**

**Exemple**

month

Récupérez les données de ce mois. Liste des mois à inclure séparés par des virgules (représentation à 2 chiffres). Valeur par défaut pour tous les mois.

10,11

year

Récupérez les données de cette année. Liste des années à inclure, séparées par des virgules (représentation à 4 chiffres). Valeur par défaut pour toutes les années.

2015

account

Récupérez les données de ce compte (Munchkin id).

111-AAA-222

rappel

Nom de la fonction avec laquelle encapsuler le contenu JSON.

processStats

Exemple de requête

`GET //localhost:8080/stats?month=10&year=2015&account=111-AAA-222&callback=processStats`

Le service web lit les fichiers « usage » et « error », les combine et les renvoie au format suivant :

```html
**<Name of Callback here>**

<Contents of Usage file here>,

<Contents of Error file here>
```

Il s’agit d’un rappel JSONP avec 2 arguments. Le premier argument est le tableau « result » d’utilisation. Le deuxième argument est le tableau des erreurs « result ». Exemple de réponse

```json
processStats(
    [
        { "date": "2015-10-15", "total": 0, "users" : [] },
        { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
        { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] }
    ],
    [
        { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
        { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
        { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
    ]
);
```

Comme vous pouvez le voir, le service Web a simplement encapsulé le contenu des deux fichiers de sortie de notre application. Nous avons créé une réponse de service web simulée à l’aide de [Mocky](https://designer.mocky.io). Voici un exemple du service web sur lequel la simulation est [.](http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats) la création de ce service web est laissée à l’appréciation du lecteur : **Page web du tableau de bord** Il ne nous manque plus qu’une page web pour appeler notre service web et formater les données. Pour utiliser le modèle JSONP, il nous suffit d’ajouter une balise `<script>` qui appelle le service web :

`<script src="http: //<hostname>/stats?month=10&year=2015&account=284-RPR-133&callback=processStats"></script>`

Le corps de la réponse du service web est alors directement injecté dans la page HTML. Nous ajoutons ensuite la fonction de rappel JSONP :

```javascript
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
;
```

Cette fonction est automatiquement appelée après l’appel du service web. Dans cet exemple, nous appelons un « dumper de variables » JavaScript simple appelé [prettyPrint.js](https://github.com/padolsey-archive/prettyprint.js/tree/master) sur chaque tableau. La fonction prettyPrint produit simplement un tableau HTML en utilisant le contenu du tableau . Voici une capture d’écran des tables HTML. Voilà, c&#39;est notre tableau de bord ! Certes, ce n&#39;est pas très élégant, mais cela devrait vous donner une idée de ce qui est possible. Rien ne vous empêche de transformer les données comme vous le souhaitez pour créer vos propres visualisations accrocheuses. La page HTML se trouve ci-dessous (Index.html). Pour afficher les tableaux ci-dessus en direct dans votre navigateur, procédez comme suit :

1. Enregistrer une copie locale d’Index.html
1. Enregistrer une copie locale de prettyPrint.js
1. Ouvrez Index.html dans votre navigateur

C&#39;est tout. J’espère que cet article vous a donné quelques idées sur la surveillance de vos statistiques d’API Marketo. Heureux codage ! **Stats.java**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import java.io.\*;
import java.lang.reflect.Array;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

import java.nio.file.Files;
import java.nio.file.Paths;

import javax.net.ssl.HttpsURLConnection;

public class Stats {
    //
    // Define Marketo instance meta data here. Each row contains three elements: Account Id, Client Id, Client Secret.
    //
    // Note: that this information would typically be stored read from an external source (i.e. database)
    //
    // For example:
    //
    // private static String final CUSTOM_SERVICE_DATA[][] = {
    // {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    // };
    //
    private static final String CUSTOM_SERVICE_DATA[][] = {
    };

    // Output directory for stats files
    private static final String OUTPUT_DIR = "C:stats";

public static void main(String[] args) {

    // Loop through each Marketo instance
    for (int i = 0; i < CUSTOM_SERVICE_DATA.length; i++) {

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA[i][0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
            baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[i][1], CUSTOM_SERVICE_DATA[i][2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose Get Last 7 Days Usage URL
        String usageUrl = String.format("%s/rest/v1/stats/usage/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Compose Get Last 7 Days Errors URL
        String errorsUrl = String.format("%s/rest/v1/stats/errors/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Process usage data
        JsonObject usageObj = JsonObject.readFrom(httpRequest("GET", usageUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (usageObj.get("result") != null) {
                updateFile(usageObj, "usage", CUSTOM_SERVICE_DATA[i][0]);
            }
        }

        // Process errors data
        JsonObject errorsObj = JsonObject.readFrom(httpRequest("GET", errorsUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (errorsObj.get("result") != null) {
                updateFile(errorsObj, "errors", CUSTOM_SERVICE_DATA[i][0]);
            }
        }
    }
    System.exit(0);
}

// Write yesterday's data to output file
private static void updateFile(JsonObject usageObj, String statsType, String account){

    JsonArray usageResultAry = usageObj.get("result").asArray();
    JsonObject yesterdayUsageResultObj = usageResultAry.get(1).asObject();

    String yesterdayDate = yesterdayUsageResultObj.get("date").asString();
    String[] yesterdayDateAry = yesterdayDate.split("[-]+");

    // "C:statsstats_yyyy_mm_account.json"
    String statsFile = String.format("%s%s_%s_%s_%s.json",
        OUTPUT_DIR, statsType, yesterdayDateAry[0], yesterdayDateAry[1], account);

    // Create file
    File file = new File(statsFile);
    try {
        if (file.createNewFile()) {
            // created new file, seed with empty array
            FileWriter fw = new FileWriter(file.getAbsoluteFile());
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[n]");
            bw.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Read file
    String content = null;
    try {
        content = new String(Files.readAllBytes(Paths.get(statsFile)));
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Remove trailing "]", append new record, append trailing "]"
    content = content.substring(0, content.length() - 1);
    content += yesterdayUsageResultObj.toString();
    content += "n]";

    // Write file
    FileWriter fw = null;
    try {
        fw = new FileWriter(file.getAbsoluteFile());
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write(content);
        bw.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}

// Perform HTTP request
private static String httpRequest(String method, String endpoint, String body) {
    String data = "";
    try {
        URL url = new URL(endpoint);
        HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
        urlConn.setDoOutput(true);
        urlConn.setRequestMethod(method);
        switch (method) {
            case "GET":
                break;
            case "POST":
                urlConn.setRequestProperty("Content-type", "application/json");
                urlConn.setRequestProperty("accept", "text/json");
                OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                wr.write(body);
                wr.flush();
                break;
            default:
                System.out.println("Error: Invalid method.");
                return data;
        }
        int responseCode = urlConn.getResponseCode();
        if (responseCode == 200) {
            InputStream inStream = urlConn.getInputStream();
            data = convertStreamToString(inStream);
        } else {
            System.out.println(responseCode);
            data = "Status:" + responseCode;
        }
    } catch (MalformedURLException e) {
        System.out.println("URL not valid.");
    } catch (IOException e) {
        System.out.println("IOException: " + e.getMessage());
        e.printStackTrace();
    }
    return data;
}

private static String convertStreamToString(InputStream inputStream) {
    try {
        return new Scanner(inputStream).useDelimiter("A").next();
    } catch (NoSuchElementException e) {
        return "";
    }
}
}
```

**Index.html**

```html
<html>
<head>
<title>Marketo API Stats</title>
<!-- Browser JavaScript variable dumper                  -->
<!-- https://github.com/padolsey-archive/prettyprint.js  -->
<script src="prettyPrint.js"></script>
</head>
<body>

<h1>Marketo API Stats</h1>

<script>
// JSONP callback that uses prettyPrint to format API stats
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
};
</script>

<!-- Web service for you to implement as an exercise                                                        -->
<!-- <script src="http://localhost:8080/stats?month=10&account=111-AAA-222&callback=processStats"></script> -->

<!-- Mock web service that returns sample payload -->
<!-- http://www.mocky.io/                         -->
<script src="http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats"></script>"
</body>
</html>
```

Publié le _2015-10-22_ par _David_

## Traitement des exceptions et des erreurs de l’API REST Marketo : Partie 3

La plupart du temps, les erreurs reçues en retour de l’API REST Marketo ne seront pas automatiquement récupérables. Cependant, il existe quelques cas où vous pouvez effectuer une récupération automatique ou vous assurer de ne jamais voir un certain type d’erreur.

### Erreurs de taille de requête

Comme nous l’avons vu dans le dernier article de cette série, Marketo émettra le code d’état HTTP 414 si la longueur de votre URI dépasse 8 Ko, ou 413 si le corps de votre requête dépasse 1 Mo, ou 10 Mo pour le lead d’importation. Bien que les 414 soient rares, vous pouvez les voir si vous utilisez le type Get Leads By Filter pour demander des enregistrements en fonction de 300 GUID distincts ou de critères similaires. Supposons que vous ayez la requête suivante : `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?filterType=customGUID&fields=email`, company...firstName, lastName> Lorsque vous soumettez la requête, Marketo renvoie un statut 414, car l’URI dépasse 8 Ko.

Pour gérer cela, nous devons modifier le modèle de cette requête, envoyer un POST au lieu d’un GET, ajouter « _method=GET » à l’URI et transmettre la chaîne de requête dans le corps de la requête sous la forme d’une requête x-www-form-urlencoded au lieu de : URI : `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?_method=GET>` Corps de la requête : filterType=customGUID&amp;fields=email, company...firstName, lastName Au lieu d’attraper cette exception dans la réponse HTTP, cependant, nous pouvons simplement vérifier la longueur totale de la requête au moment de l’exécution et déployer ce modèle alternatif si l’URI dépasse 8 000. Vous pouvez également utiliser la méthode POST dans tous les cas pour les récupérations par lots des enregistrements. Pour les requêtes 413, nous pouvons suivre un modèle similaire, en vérifiant la longueur du corps de la requête lors de l’ajout d’enregistrements au cours de l’étape de sérialisation, et en divisant la requête en plusieurs parties si cette limite est dépassée.

### Erreurs d’authentification

Notre prochaine classe d’erreurs récupérables est liée à l’authentification. Lorsqu’un jeton d’accès précédemment valide est utilisé après l’expiration de sa période expires_in, la première utilisation renvoie un code d’erreur 602, « Jeton d’accès expiré ». Ensuite, l’utilisation du même jeton renvoie un 601, « Jeton d’accès non valide ». Toute autre utilisation d’une chaîne qui n’est pas un jeton d’accès valide pour l’abonnement cible entraîne une erreur 601. Dans les deux cas, cette erreur peut être récupérée à partir de en s’authentifiant à nouveau et en transmettant le nouveau jeton d’accès avec une nouvelle tentative de la requête ayant échoué.

### Délais dépassés

Dans de très rares cas, un appel peut renvoyer un message 604, « Demande expirée », après l’expiration de la période de temporisation de 30 secondes. Pour les requêtes par lots, telles que les leads de création/mise à jour, la requête peut être divisée en lots plus petits et réessayée jusqu’à ce que le succès soit renvoyé (si le lot est divisé en moins de 100 enregistrements et que la requête expire toujours, vous devez probablement déposer un dossier d’assistance). L’autre cas le plus courant concerne les appels d’approbation de ressources, où un verrou peut être conservé sur l’enregistrement approuvé actuel par un autre utilisateur ou service, comme dans le cas d’un [e-mail](https://developer.adobe.com/marketo-apis/api/asset/approve-email-by-id/) ou d’un [modèle d’e-mail](https://developer.adobe.com/marketo-apis/api/asset/approve-email-template-by-id/). Dans ces cas, [reculade exponentielle](https://en.wikipedia.org/wiki/Exponential_backoff) doit être utilisée pour les reprises afin de permettre la résolution des verrous existants. Revenez dans les semaines à venir pour découvrir la dernière partie de la série où nous examinerons de plus près certaines erreurs spécifiques non récupérables.

Publié le _2015-10-30_ par _Kenny_

## Améliorations de la sécurité dans Marketo

Chez Marketo, nous prenons la sécurité au sérieux. Dans le cadre d’une **[initiative à l’échelle du secteur](https://security.googleblog.com/2014/09/gradually-sunsetting-sha-1.html)**, Marketo met à niveau notre authentification et notre chiffrement web pour améliorer nos protections de sécurité. Le déploiement est prévu pour le 12 décembre 2011. **Qui sera affecté ?** Seul un petit nombre d’utilisateurs sera affecté, uniquement ceux qui disposent d’une intégration à Marketo à partir d’un système vieux de plus de dix ans et qui n’a pas été mis à jour récemment. Voir **[cette liste](https://www.digicert.com/faq/sha2/sha-2-compatibility)** pour plus d’informations sur les systèmes et versions pris en charge. **Les utilisateurs suivants ne seront pas affectés :**

* Utilisateurs finaux accédant à Marketo.com via des navigateurs modernes ([voir liste](https://www.digicert.com/faq/sha2/sha-2-compatibility))
* Clients utilisant des partenaires d&#39;intégration tels qu&#39;Informatica, Dell Boomi et Scribe.
* Clients qui utilisent des partenaires Launchpoint.

Tous les autres domaines Marketo utilisent déjà des certificats SHA2.

Publié le _2015-11-18_ par _Kenny_

## Interrogation des activités à l’aide de l’API REST

Les activités sont un objet principal de la plateforme Marketo. Les activités sont des données comportementales stockées concernant chaque visite de page web, ouverture d’e-mail, participation à un webinaire ou à un salon. Un cas d’utilisation courant consiste à combiner des données d’activité avec des données provenant d’autres parties d’une organisation. Cet exemple de programme contient 6 étapes :

1. Appelez [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) pour générer une liste de tous les enregistrements d’activités créés à une date/heure donnée. Nous utilisons un filtre pour limiter le type d’enregistrements d’activité renvoyés.
1. Extrayez les champs d’intérêt de chaque enregistrement d’activité.
1. Appelez [Get Multiple Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) pour générer une liste d’enregistrements de leads qui correspondent aux activités de l’étape 1. Nous utilisons le champ leadId extrait de l’enregistrement d’activité à l’étape 2 comme filtre pour spécifier les prospects renvoyés.
1. Extrayez les champs d’intérêt de chaque enregistrement de prospect.
1. Rejoignez les données d’activité de l’étape 2 avec les données de prospect de l’étape 4.
1. Transformez les données de l’étape 5 dans un format consommable par un système externe.

Comme le montre le diagramme ci-dessous, pour cet exemple, nous avons choisi de capturer les activités liées aux e-mails.

**Entrée du programme** Par défaut, le programme remonte dans le temps d’un jour à partir de la date actuelle pour rechercher des modifications. Ainsi, vous pouvez exécuter ce programme à la même heure chaque jour, par exemple. Pour remonter plus loin dans le temps, vous pouvez spécifier le nombre de jours comme argument de ligne de commande, ce qui accroît la fenêtre temporelle. Le programme contient plusieurs variables que vous pouvez modifier : CUSTOM_SERVICE_DATA - Contient vos données de service personnalisées Marketo (ID de compte, ID client, Secret client). READ_BATCH_SIZE - Nombre d&#39;enregistrements à récupérer à la fois. Utilisez cette option pour ajuster la réponse à la taille du corps. LEAD_FIELDS - Contient une liste des champs de prospect que nous voulons collecter. ACTIVITY_TYPES - Contient une liste des types d’activités que nous voulons collecter.

**Logique du programme** Tout d’abord, nous établissons notre fenêtre temporelle, composons nos URL de point d’entrée REST et obtenons notre jeton d’accès d’authentification. Nous allons ensuite déclencher une boucle Get Paging Token/Get Lead Activities qui s’exécutera jusqu’à ce que nous ayons épuisé l’offre d’activités. L’objectif de cette boucle est de récupérer les enregistrements d’activité et d’extraire les champs d’intérêt de ces enregistrements. Nous indiquons aux activités Obtenir le lead de rechercher uniquement les types d’activités suivants :

* E-mail remis au destinataire
* Se désabonner des e-mails
* Ouvrir e-mail
* Cliquez sur E-mail.

Nous extrayons les champs d’intérêt suivants de chaque enregistrement d’activité :

* leadId
* activityType
* activityDate
* primaryAttributeValue

Vous pouvez sélectionner n’importe quelle combinaison de types d’activité et de champs d’activité en fonction de vos besoins. Ensuite, nous lançons une boucle Get Multiple Leads by Filter Type qui s&#39;exécute jusqu&#39;à ce que nous ayons épuisé l&#39;offre de leads. Notez que nous utilisons le paramètre « filterType=id » en combinaison avec une série de paramètres « filterValues » pour limiter les enregistrements de prospect récupérés uniquement aux prospects associés aux activités que nous avons récupérées précédemment. Nous extrayons les champs d’intérêt suivants de chaque enregistrement de prospect :

* Prénom
* Nom
* E-mail

Là encore, vous pouvez sélectionner n’importe quel champ de prospect de votre choix. Ensuite, nous joignons les champs de prospect aux champs d’activité à l’aide de l’ID de prospect pour les lier. Enfin, nous parcourons toutes les données, les transformons au format JSON et les imprimons sur la console. **Sortie du programme** Voici un exemple de sortie de l’exemple de programme. Cela affiche les champs d’activité et les champs de prospect combinés en tant qu’objets « résultat » JSON. L’idée ici est que vous puissiez transmettre ce fichier JSON en tant que payload de requête à un service web externe.

```json
{
    "result": [
        {
            "leadId": 318581,
            "activityType": "Email Delivered",
            "activityDate": "2015-03-17T20:00:06Z",
            "primaryAttributeValue": "My Email Program",
            "firstName": "David",
            "lastName": "Everly",
            "email": "everlyd@marketo.com"
        },
        {
            "leadId":318581,
            "activityType":"Open Email",
            "activityDate":"2015-03-17T23:23:12Z",
            "primaryAttributeValue":"My Email Program - Auto Response",
            "firstName":"David",
            "lastName":"Everly",
            "email":"everlyd@marketo.com"
       },
        ... more result objects here...
    ]
}
```

**Code du programme**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadActivities {
//
// Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    private static Map<String, String> CUSTOM_SERVICE_DATA;
    static {
        CUSTOM_SERVICE_DATA = new HashMap<String, String>();
//        CUSTOM_SERVICE_DATA.put("accountId", "111-AAA-222");
//        CUSTOM_SERVICE_DATA.put("clientId", "2f4a4435-f6fa-4bd9-3248-098754982345");
//        CUSTOM_SERVICE_DATA.put("clientSecret", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W");
    }

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Lookup lead records using lead id as primary key
    private static final String LEAD_FILTER_TYPE = "id";

    // Lead record lookup returns these fields
    private static final String LEAD_FIELDS = "firstName,lastName,email";

    // Lookup activity records for these activity types
    private static Map<Integer, String> ACTIVITY_TYPES;
    static {
        ACTIVITY_TYPES = new HashMap<Integer, String>();
        ACTIVITY_TYPES.put(7, "Email Delivered");
        ACTIVITY_TYPES.put(9, "Unsubscribe Email");
        ACTIVITY_TYPES.put(10, "Open Email");
        ACTIVITY_TYPES.put(11, "Click Email");
    }

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA.get("accountId"));

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA.get("clientId"), CUSTOM_SERVICE_DATA.get("clientSecret"));

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String activityTypesUrl = String.format("%s/rest/v1/activities/types.json?access_token=%s",
                baseUrl, accessToken);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        // Use activity ids to create filter parameter
        String activityTypeIds = "";
        for (Integer id : ACTIVITY_TYPES.keySet()) {
            activityTypeIds += "&activityTypeIds=" + id.toString();
        }

        // Compose URL for Get Lead Activities
        // Only retrieve activities that match our defined activity types
        String leadActivitiesUrl = String.format("%s/rest/v1/activities.json?access_token=%s%s&batchSize=%s",
                baseUrl, accessToken, activityTypeIds, READ_BATCH_SIZE);

        Map<Integer, List> activitiesMap = new HashMap<Integer, List>();
        Set leadsSet = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {
            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;
            do {
                moreResult = false;

                // Call Get Lead Activities API to retrieve activity data
                JsonObject leadActivitiesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadActivitiesUrl, nextPageToken)));
                if (leadActivitiesObj.get("success").asBoolean()) {
                    moreResult = leadActivitiesObj.get("moreResult").asBoolean();
                    nextPageToken = leadActivitiesObj.get("nextPageToken").asString();

                    if (leadActivitiesObj.get("result") != null) {
                        JsonArray activitiesResultAry = leadActivitiesObj.get("result").asArray();
                        for (JsonValue activitiesResultObj : activitiesResultAry) {

                            // Extract activity fields of interest (leadID, activityType, activityDate, primaryAttributeValue)
                            JsonObject leadObj = new JsonObject();
                            int leadId = activitiesResultObj.asObject().get("leadId").asInt();
                            leadObj.add("activityType", ACTIVITY_TYPES.get(activitiesResultObj.asObject().get("activityTypeId").asInt()));
                            leadObj.add("activityDate", activitiesResultObj.asObject().get("activityDate").asString());
                            leadObj.add("primaryAttributeValue", activitiesResultObj.asObject().get("primaryAttributeValue").asString());

                            // Store JSON containing activity fields in a map using lead id as key
                            List leadLst = activitiesMap.get(leadId);
                            if (leadLst == null) {
                                activitiesMap.put(leadId, new ArrayList());
                                leadLst = activitiesMap.get(leadId);
                            }
                            leadLst.add(leadObj);

                            // Store unique lead ids for use as lead filter value below
                            leadsSet.add(leadId);
                        }
                    }
                }
            } while (moreResult);
        }

        // Use unique lead id values to create filter parameter
        String filterValues = "";
        for (Object object : leadsSet) {
            if (filterValues.length() > 0) {
                filterValues += ",";
            }
            filterValues += String.format("%s", object);
        }

        // Compose Get Multiple Leads by Filter Type URL
        // Only retrieve leads that match the list of lead ids that was accumulated during activity query
        String getMultipleLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=%s&fields=%s&filterValues=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_FILTER_TYPE, LEAD_FIELDS, filterValues, READ_BATCH_SIZE);
        String nextPageToken = "";

        do {
            String gmlUrl = getMultipleLeadsUrl;

            // Append paging token to Get Multiple Leads by Filter Type URL
            if (nextPageToken.length() > 0) {
                gmlUrl = String.format("%s&nextPageToken=%s", getMultipleLeadsUrl, nextPageToken);
            }

            // Call Get Multiple Leads by Filter Type API to retrieve lead data
            JsonObject multipleLeadsObj = JsonObject.readFrom(getData(gmlUrl));
            if (multipleLeadsObj.get("success").asBoolean()) {
                if (multipleLeadsObj.get("result") != null) {
                    JsonArray multipleLeadsResultAry = multipleLeadsObj.get("result").asArray();

                    // Iterate through lead data
                    for (JsonValue leadResultObj : multipleLeadsResultAry) {
                        int leadId = leadResultObj.asObject().get("id").asInt();

                        // Join activity data with lead fields of interest (firstName, lastName, email)
                        List leadLst = activitiesMap.get(leadId);
                        for (JsonObject leadObj : leadLst) {
                            leadObj.add("firstName", leadResultObj.asObject().get("firstName").asString());
                            leadObj.add("lastName", leadResultObj.asObject().get("lastName").asString());
                            leadObj.add("email", leadResultObj.asObject().get("email").asString());
                        }
                    }
                }
            }

            nextPageToken = "";
            if (multipleLeadsObj.asObject().get("nextPageToken") != null) {
                nextPageToken = multipleLeadsObj.get("nextPageToken").asString();
            }
        } while (nextPageToken.length() > 0);

        // Now place activity data into an array of JSON objects
        JsonArray activitiesAry = new JsonArray();
        for (Map.Entry<Integer, List> activity : activitiesMap.entrySet()) {
            int leadId = activity.getKey();
            for (JsonObject leadObj : activity.getValue()) {
                // do something with leadId and each leadObj
                leadObj.add("leadId", leadId);
                activitiesAry.add(leadObj);
            }
        }

        // Print out result objects
        JsonObject result = new JsonObject();
        result.add("result", activitiesAry);
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

C&#39;est ça. Heureux codage !

Publié le _2015-11-20_ par _David_

## Intégration du lecteur SoundCloud à l’API Munchkin

SoundCloud offre une plateforme d&#39;hébergement audio incroyable, avec des analyses et des fonctionnalités riches pour tout, des artistes indépendants en herbe au rock, en passant par les artistes EDM au sommet de l&#39;industrie de la musique et les podcasts de contes. Outre l’incroyable fonctionnalité native de la plateforme, un programme d’API de classe mondiale est fourni pour déplacer vos données et suivre le comportement d’écoute. Cela s’avère particulièrement utile pour les podcasters et peut vous permettre de corréler des actions d’écoute spécifiques, telles que des lectures, des pauses et des partages, à un contenu spécifique dans le script et l’audio. Aujourd’hui, nous allons examiner l’utilisation de l’API de widget [SoundCloud](https://developers.soundcloud.com/docs/api/html5-widget) pour envoyer et suivre ces activités dans Marketo. Examinons d’abord la génération d’une activité Munchkin qui sera enregistrée dans le Marketo de connexion à l’activité d’un prospect. À la base, nous effectuons un appel à Munchkin.munchkinFunction et transmettons « visitWebPage » comme premier argument. Cette opération consigne une activité de page web Visites dans Marketo et enregistre toutes les données arbitraires d’URL et de chaîne de requête que nous transmettons à la méthode . Le deuxième argument accepte un objet JavaScript avec nos données, qui comporte deux membres, « url » et « params », les deux chaînes. Le membre d’URL correspond à la page web de l’activité dans Marketo, tandis que les paramètres correspondent à la chaîne de requête. À nos fins, nous utiliserons l’URL comme identifiant pour les actions associées à SoundCloud, « soundCloudInteraction », tandis que les paramètres contiendront des données supplémentaires sur l’activité particulière. Voici la fonction que nous utilisons pour effectuer le suivi de chaque action :

```javascript
var trackActivity = function(action){

    //set action param to be the string passed to the function
    var qs = "action=" + action;

    //use getCurrentSound callback to get the name of the current track
    soundCloudMunchkin.widget.getCurrentSound(function(currentSound){

        //add it to our querystring
        qs = qs + "&sound=" + currentSound.title;

        //use the getPosition callback to get the position of the track in ms
        soundCloudMunchkin.widget.getPosition(function(position){

            //add it to the querystring
            qs = qs + "&position=" + position;

            //assemble our data object for the munchkin activity
            var dataObject = {
                "url": "soundCloudInteraction",
                "params": qs
            }

            //call the munchkinFunction to submit the activity
            Munchkin.munchkinFunction("visitWebPage", dataObject);
        });
    });
}
```

Comme le widget SoundCloud standard est incorporé dans un iframe, il utilise des messages post pour communiquer et des rappels doivent être utilisés pour obtenir des données, comme vous pouvez le voir avec les méthodes currentSound et getPosition. L’API de widget SoundCloud fournit un ensemble de rappels JavaScript que nous pouvons utiliser pour répondre à des événements individuels dans le lecteur et les envoyer à Marketo. Les événements qui nous intéressent sont ce que l’utilisateur écoute, la durée d’écoute de l’utilisateur et les interactions de l’utilisateur avec le lecteur. Nous examinons donc les événements suivants :

* LECTURE
* PAUSE
* TERMINER
* SEEK
* CLICK_DOWNLOAD
* CLICK_BUY
* OPEN_SHARE_PANEL

Nous devrons également utiliser la méthode bind() du widget pour ajouter des rappels à chacun de ces événements. Prenons un exemple :

```javascript
widget.bind(SC.Widget.Events.PLAY, function(){
    soundCloudMunchkin.trackPlay();
});
```

Ainsi, chaque fois qu’une piste est lue, nous déclenchons la méthode trackPlay pour envoyer un événement à Marketo avec des données sur la piste active. Le script complet se trouve [ici](https://gist.github.com/kelkingtron/6750bb07c1397d93d9c7#file-soundcloudmunchkin-js). L’objet soundCloudMunchkin possède une méthode init, qui accepte un objet widget SoundCloud comme seul argument, qui lie les méthodes de suivi aux rappels appropriés et configure votre widget pour suivre l’activité jusqu’à Marketo. Votre page doit contenir votre [code Munchkin](/help/javascript-api/lead-tracking.md) ainsi que la bibliothèque d&#39;API [SoundCloud](https://w.soundcloud.com/player/api.js). Vous devrez également tout initialiser, en plus d’incorporer votre widget SoundCloud réel :

```javascript
window.onload=function(){
var iframe = document.getElementById(iframeId);
    if(iframe) {
        widget = SC.Widget(iframe);
        soundCloudMunchkin.init(widget);
    };
};
```

Publié le _2015-12-21_ par _Kenny_

## Le RTP et la directive européenne sur la vie privée et communications électroniques

Cet article explique comment utiliser le RTP pour avertir les visiteurs du site qu&#39;ils sont suivis ou désactiver automatiquement le suivi des visiteurs européens. Depuis 2012, tout site web accessible aux visiteurs européens doit être conforme à la directive européenne sur la vie privée et les communications électroniques. De nouvelles lois sont entrées en vigueur en 2011, qui empêchent que des informations d&#39;identification soient stockées sur l&#39;ordinateur d&#39;un utilisateur à son insu et sans son consentement. Si vous utilisez des cookies ou toute autre technologie pour le suivi non essentiel, vous devez :

1. Indiquez aux utilisateurs que des technologies de suivi sont utilisées.
1. Expliquez les raisons pour lesquelles vous utilisez ces technologies.
1. Obtenez le consentement de l&#39;utilisateur avant d&#39;utiliser cette technologie et permettez-lui de retirer sa permission à tout moment.

Publié le _1970-01-01_ par _Yanir_

## Mettre à jour les informations sur les clients et les prospects dans Marketo à l’aide de l’API

Il existe des scénarios où des systèmes propriétaires sont utilisés pour mettre à jour les informations sur les clients et les prospects. L’équipe marketing souhaite que ces mises à jour soient répercutées dans Marketo afin de disposer du système d’enregistrement le plus précis à utiliser dans ses campagnes marketing. Grâce à l’approche ci-dessous, vous pouvez configurer des chargements périodiques vers Marketo afin de maintenir à jour vos coordonnées Marketo avec les données modifiées dans ce système propriétaire. Le diagramme ci-dessous montre les appels API effectués selon un minuteur périodique défini. Lorsque le minuteur périodique est déclenché, la logique client récupère d’abord les contacts mis à jour du système propriétaire. La procédure varie d’un système à l’autre, en utilisant des API ou en exportant des données à partir du système propriétaire. Nous détaillerons les API Marketo qui sont exécutées une fois que les informations de contact mises à jour sont récupérées. Requête SOAP pour syncMultipleLeads :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadRecordList>
    <leadRecord>
      <Email>henry@superstar.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Henry</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Adams</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>Director of Demand Generation</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
    <leadRecord>
      <Email>ssmith@gmail.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Suzie</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Smith</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>VP Marketing</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
  </leadRecordList>
  <dedupEnabled>true</dedupEnabled>
</ns2:paramsSyncMultipleLeads>
```

Réponse SOAP de syncMultipleLeads :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
    <syncStatusList>
      <syncStatus>
        <leadId>1094593</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
      <syncStatus>
        <leadId>1094594</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
    </syncStatusList>
  </result>
</ns2:successSyncMultipleLeads>
```

`syncMultipleLeads` effectue une opération UPSERT. Si un contact dans Marketo existe déjà en fonction de l’adresse e-mail envoyée, les attributs sont mis à jour. Si un contact n’existe pas, il est créé. La réponse de `syncMultipleLeads` renvoie le statut de chacun des contacts envoyés. Les valeurs `<attrName/>` dans le `<leadAttributeList/>` doivent correspondre au nom de l’API SOAP défini pour cet abonnement Marketo. Vous pouvez découvrir les noms des API SOAP dans la section de gestion des champs du panneau d’administration de Marketo en exportant les noms des champs.

Consultez l’exemple de programme Java ci-dessous qui exécute le scénario décrit ci-dessus :

```java
 import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import java.util.\*;

public class SyncMultipleLeadsExample {

  public static void main(String[] args) {

    try {
      URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
      String marketoUserId = "CHANGE ME";
      String marketoSecretKey = "CHANGE ME";

      QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
      MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
      MktowsPort port = service.getMktowsApiSoapPort();

      // Create Signature
      DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
      String text = df.format(new Date());
      String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
      String encryptString = requestTimestamp + marketoUserId ;

      SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
      Mac mac = Mac.getInstance("HmacSHA1");
      mac.init(secretKey);
      byte[] rawHmac = mac.doFinal(encryptString.getBytes());
      char[] hexChars = Hex.encodeHex(rawHmac);
      String signature = new String(hexChars);

      // Set Authentication Header
      AuthenticationHeader header = new AuthenticationHeader();
      header.setMktowsUserId(marketoUserId);
      header.setRequestTimestamp(requestTimestamp);
      header.setRequestSignature(signature);

      // Create Request
      ParamsSyncMultipleLeads request = new ParamsSyncMultipleLeads();

      ObjectFactory objectFactory = new ObjectFactory();

      JAXBElement dedup = objectFactory.createParamsSyncMultipleLeadsDedupEnabled(true);
      request.setDedupEnabled(dedup);

      // The list of contacts defined here would be retrieved from the proprietary system
      Contact contact = new Contact("Henry","Adams","henry@superstar.com", "Director of Demand Generation");
      Contact contact2 = new Contact("Suzie","Smith","ssmith@gmail.com", "VP Marketing");

      ArrayList updatedContacts = new ArrayList();
      updatedContacts.add(contact);
      updatedContacts.add(contact2);

      ArrayOfLeadRecord arrayOfLeadRecords = new ArrayOfLeadRecord();

      Iterator it = updatedContacts.iterator();
      while(it.hasNext())
      {
        Contact c = it.next();

        LeadRecord leadRec = new LeadRecord();

        JAXBElement email = objectFactory.createLeadRecordEmail(c.email);
        leadRec.setEmail(email);

        Attribute attr1 = new Attribute();
        attr1.setAttrName("FirstName");
        attr1.setAttrValue(c.fname);

        Attribute attr2 = new Attribute();
        attr2.setAttrName("LastName");
        attr2.setAttrValue(c.lname);

        Attribute attr3 = new Attribute();
        attr3.setAttrName("Title");
        attr3.setAttrValue(c.title);

        ArrayOfAttribute aoa = new ArrayOfAttribute();
        aoa.getAttributes().add(attr1);
        aoa.getAttributes().add(attr2);
        aoa.getAttributes().add(attr3);

        QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
        JAXBElement attrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);

        leadRec.setLeadAttributeList(attrList);
        arrayOfLeadRecords.getLeadRecords().add(leadRec);

      }

      request.setLeadRecordList(arrayOfLeadRecords);

      JAXBContext context = JAXBContext.newInstance(SuccessSyncMultipleLeads.class);
      Marshaller m = context.createMarshaller();
      m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
      m.marshal(request, System.out);

      SuccessSyncMultipleLeads result = port.syncMultipleLeads(request, header);

      m.marshal(result, System.out);
    }

    catch(Exception e) {
      e.printStackTrace();
    }
  }

  public static class Contact {
    public String fname, lname, email, title;

      public Contact(String fname, String lname, String email, String title) {
          this.fname = fname;
          this.lname = lname;
          this.email = email;
          this.title = title;
      }
  }
}
```

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-03-24_ par _Travis Kaufman_

## Envoyer un e-mail transactionnel à partir de Marketo à l’aide de l’API

Une campagne intelligente existante doit être créée à l’aide de l’interface utilisateur de Marketo. Le destinataire de l’e-mail doit également exister dans Marketo. Par conséquent, avant d’appeler l’API requestCampaign, utilisez l’API [getLead]&#x200B;(/help/soap-api/getlead.md) pour vérifier si l’e-mail existe dans Marketo. Après avoir effectué un appel via l’API requestCampaign, vous pouvez le confirmer en vérifiant si la campagne intelligente s’est exécutée dans Marketo. Nous vous montrons d’abord comment créer une campagne intelligente, ensuite comment configurer un déclencheur pour envoyer une campagne via l’API, enfin comment définir un e-mail dans le cadre d’une action de flux et, enfin, un exemple de code qui serait utilisé pour exécuter cette campagne.
**Création d’une campagne intelligente dans Marketo** les campagnes intelligentes dans Marketo exécutent toutes vos activités marketing. Vous pouvez configurer une série d’actions automatisées pour prendre en charge une liste dynamique de contacts. Dans le cas de l&#39;envoi d&#39;e-mails transactionnels, vous configurez un déclencheur dans la campagne, comme illustré ci-dessous, pour envoyer des e-mails à l&#39;aide de l&#39;API . Commençons par configurer la campagne intelligente. 1. Dans Activités marketing, choisissez un programme puis, dans la liste déroulante Nouveau , cliquez sur Nouvelle ressource locale.

1. Clic sur la campagne intelligente
1. Saisissez un nom de campagne intelligente et cliquez sur Créer .

**Ajouter des triggers à une campagne intelligente** l&#39;ajout de triggers à une campagne intelligente permet d&#39;exécuter une campagne intelligente sur une personne à la fois en fonction d&#39;un événement en direct, qui est dans ce cas une requête via l&#39;API [requestCampaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST). 1. Recherchez le déclencheur « La campagne est demandée », puis faites-le glisser et déposez-le sur la zone de travail.

1. Dans le déclencheur, sélectionnez « est » et « API de service web ».

**Création d’une action de flux d’e-mail sur une campagne** l’association d’un e-mail à une campagne intelligente permet aux spécialistes marketing de gérer l’aspect qu’ils souhaitent donner à un e-mail et permet à l’application tierce de déterminer qui le reçoit et quand. Après avoir créé un e-mail en tant que nouvelle ressource locale, vous pouvez le définir en tant qu’action de flux dans une campagne.  Recherchez et sélectionnez l’e-mail à envoyer.

**Exemple de code pour appeler l’API requestCampaign** Après la configuration de la campagne et des déclencheurs dans l’interface Marketo, nous vous montrons comment utiliser l’API pour envoyer un email. Le premier exemple est une requête XML, le second est une réponse XML et le dernier est un exemple de code Java qui peut être utilisé pour générer la requête XML. Nous vous montrons également comment trouver l’identifiant de campagne utilisé lors d’un appel à l’API `requestCampaign`.
L’appel API nécessite également que vous connaissiez à l’avance l’identifiant de la campagne Marketo. Vous pouvez déterminer l&#39;identifiant de la campagne à l&#39;aide de l&#39;une des méthodes suivantes : 1. Utilisez l’API [getCampaignsForSource](/help/soap-api/getcampaignsforsource.md) 1. Ouvrez la campagne Marketo dans un navigateur et regardez la barre d’adresse URL. L’identifiant de campagne (représenté sous la forme d’un entier de 4 chiffres) se trouve immédiatement après « SC ». Par exemple : `<https://app-stage.marketo.com/#SC**1025**A1>`. La partie en gras est l’identifiant de campagne « 1025 ». Demande de `requestCampaign` SOAP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Réponse de SOAP pour requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Vous trouverez ci-dessous un exemple de programme Java qui exécute le scénario décrit ci-dessus.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");

            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-03-27_ par _Murta_

## Envoi d’un e-mail avec du contenu dynamique depuis Marketo à l’aide de l’API

Imaginez que vous souhaitiez automatiser les e-mails de suivi de votre centre d’appels. Une fois que votre représentant d’assistance a parlé à un client, vous souhaitez envoyer automatiquement un e-mail pour le remercier d’avoir contacté votre entreprise. Allons un peu plus loin et disons que vous souhaitez inclure le sujet de conversation spécifique discuté avec le client que vous suivez dans votre CRM. Vous pouvez le faire à partir de Marketo à l’aide de l’API requestCampaign SOAP pour envoyer un e-mail avec du contenu dynamique. L’API requestCampaign permet de transmettre un ou plusieurs prospects. Il vous permet également de transmettre des jetons de programme qui peuvent être utilisés avec une campagne existante pour envoyer du contenu dynamique. L’API requestCampaign SOAP nécessite que le destinataire de l’e-mail existe dans Marketo. Ainsi, avant d’appeler l’API requestCampaign, utilisez l’API [getLead](/help/soap-api/getlead.md) pour vérifier si l’e-mail existe dans Marketo. Nous vous montrons d’abord comment créer une campagne intelligente, ensuite comment configurer un déclencheur pour envoyer une campagne via l’API, enfin comment créer un e-mail qui accepte du contenu dynamique via des jetons de programme, enfin comment définir un e-mail dans le cadre d’une action de flux et enfin, comment utiliser un exemple de code pour exécuter cette campagne. **Création d’une campagne intelligente dans Marketo** les campagnes intelligentes dans Marketo exécutent toutes vos activités marketing. Vous pouvez configurer une série d’actions automatisées pour prendre en charge une liste dynamique de contacts. Dans le cas de l&#39;envoi d&#39;e-mails transactionnels, vous configurez un déclencheur dans la campagne, comme illustré ci-dessous, pour envoyer des e-mails à l&#39;aide de l&#39;API . Commençons par configurer la campagne intelligente. 1. Dans Activités marketing, choisissez un programme puis, dans la liste déroulante Nouveau , cliquez sur Nouvelle ressource locale

1. Clic sur la campagne intelligente
1. Saisissez le nom de la campagne intelligente et cliquez sur Créer **Ajouter des déclencheurs à une campagne intelligente** L’ajout de déclencheurs à une campagne intelligente vous permet de lancer une campagne intelligente sur une personne à la fois en fonction d’un événement réel, qui est dans ce cas une requête via l’API [requestCampaign](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST).
1. Recherchez le déclencheur « La campagne est demandée », puis faites-le glisser et déposez-le sur la zone de travail.
1. Dans le déclencheur, sélectionnez « est » et « API de service web ».

**Comment transmettre du contenu dynamique à l’aide de l’API** dans Marketo, mes jetons sont des variables que vous pouvez utiliser dans votre programme. Mes jetons vous permettent de saisir des informations relatives à votre programme à un emplacement, de remplacer ces informations par une valeur que vous spécifiez et de récupérer ces informations dans d’autres parties de l’application, comme un modèle de courrier électronique. À l’aide de l’API requestCampaign SOAP, vous pouvez transmettre un tableau de jetons de programme qui remplaceront les jetons existants. Une fois la campagne exécutée, les jetons sont ignorés. Vous créez mes jetons au niveau du dossier Campagne ou au niveau du programme. Mes jetons au niveau du dossier Campaign hériteront de tous les programmes contenus dans le dossier Campaign. Si vous créez Mes jetons au niveau du dossier Campagne, vous pouvez remplacer la valeur héritée au niveau du programme. Par exemple, si vous définissez des jetons pour la Date du programme et la Description du programme au niveau du dossier Campagne, vous pouvez remplacer ces valeurs au niveau du programme individuel.

Voici comment procéder. 1. Dans l&#39;arborescence des Activités marketing, sélectionnez le dossier ou le programme de la campagne dans lequel vous souhaitez créer les jetons. Dans la barre de menus supérieure, sélectionnez Mes jetons . La zone de travail Mes jetons s’affiche. Dans l’arborescence de droite, faites glisser un Type de jeton vers la zone de travail, qui est dans ce cas « Texte ». Dans le champ Nom du jeton , mettez en surbrillance Mon jeton et saisissez un Nom de jeton unique, qui est dans ce cas « my.conversationtopic ». Dans le champ Valeur , saisissez une valeur appropriée pour le jeton, qui est dans ce cas « Merci de nous avoir appelés aujourd’hui ». Notez qu’en utilisant l’API , nous remplacerons la valeur par défaut de Mon jeton . Cliquez sur « Enregistrer » pour enregistrer le jeton personnalisé.  1. Créez un nouvel e-mail en cliquant sur Nouveau. Cliquez ensuite sur Nouvelle Assets locale et sélectionnez Email. Remplissez ensuite les champs appropriés pour nommer votre e-mail. Lors de la rédaction de votre e-mail, cliquez sur l’icône Jeton pour inclure des jetons dans votre e-mail. Maintenant que vous avez créé votre modèle d’e-mail avec des jetons, nous allons ajouter l’e-mail en tant qu’action de flux pour la campagne à l’étape suivante. Ainsi, lorsque vous appelez la campagne via l’API, l’e-mail est envoyé.
**Création d’une action de flux d’e-mail sur une campagne** l’association d’un e-mail à une campagne intelligente permet aux spécialistes marketing de gérer l’aspect qu’ils souhaitent donner à un e-mail et permet à l’application tierce de déterminer qui le reçoit et quand. Après avoir créé un e-mail en tant que nouvelle ressource locale, vous pouvez le définir en tant qu’action de flux dans une campagne. Recherchez et sélectionnez l’e-mail à envoyer.
**Exemple de code pour appeler l’API requestCampaign** Après la configuration de la campagne et des déclencheurs dans l’interface Marketo, nous vous montrons comment utiliser l’API pour envoyer un email. Le premier exemple est une requête XML, le second est une réponse XML et le dernier est un exemple de code Java qui peut être utilisé pour générer la requête XML. Nous vous montrons également comment trouver l’identifiant de campagne utilisé lors d’un appel à l’API requestCampaign. L’appel API nécessite également que vous connaissiez à l’avance l’identifiant de la campagne Marketo. Vous pouvez déterminer l&#39;identifiant de la campagne à l&#39;aide de l&#39;une des méthodes suivantes : 1. Utilisez l’API [getCampaignsForSource](/help/soap-api/getcampaignsforsource.md) 1. Ouvrez la campagne Marketo dans un navigateur et regardez la barre d’adresse URL. L’identifiant de campagne (représenté sous la forme d’un entier de 4 chiffres) se trouve immédiatement après « SC ». Par exemple : `<https://app-stage.marketo.com/#SC&#x200B;**1025**&#x200B;A1>`. La partie en gras est l’identifiant de campagne « 1025 ». Requête SOAP pour requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
      </leadList>
      <programTokenList>
        <attrib>
          <name>{{my.conversationtopic}}</name>
          <value>Thank you for calling about adding a line of service to your current plan.</value>
        </attrib>
      </programTokenList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Réponse de SOAP pour requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Vous trouverez ci-dessous un exemple de programme Java qui exécute le scénario décrit ci-dessus.

```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class RequestCampaign {

    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";

            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();

            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);
            String encryptString = requestTimestamp + marketoUserId ;

            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars);

            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);

            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();

            request.setSource(ReqCampSourceType.MKTOWS);

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);

            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");

            leadKeyList.getLeadKeies().add(key);

            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            ArrayOfAttrib aoa = new ArrayOfAttrib();

            Attrib attrib = new Attrib();
            attrib.setName("{{my.conversationtopic}}");
            attrib.setValue("Thank you for calling about adding a line of service to your current plan.");

            aoa.getAttribs().add(attrib);

            JAXBElement<ArrayOfAttrib> arrayOfAttrib = objectFactory.createParamsRequestCampaignProgramTokenList(aoa);
            request.setProgramTokenList(arrayOfAttrib);

            SuccessRequestCampaign result = port.requestCampaign(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);

        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Après avoir effectué un appel via l’API requestCampaign, vous pouvez le confirmer en vérifiant si la campagne intelligente s’est exécutée dans Marketo.

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-04-03_ par _Murta_

## Capturer l’activité visiteur anonyme en fonction de la logique commerciale

Imaginez que vous souhaitiez suivre les utilisateurs qui consultent un article spécifique sur le blog de votre entreprise. Supposons que sur le nombre total d’utilisateurs qui visitent une publication, vous souhaitiez uniquement suivre les utilisateurs qui signalent leur intérêt en passant au moins 5 secondes et en faisant défiler la page. Pour les utilisateurs anonymes, vous souhaitez créer un prospect dans Marketo avec cet événement et pour les utilisateurs connus, vous souhaitez mettre à jour leur activité de prospect avec cet événement. Pour ce faire, utilisez le code de suivi [Munchkin](/help/javascript-api/lead-tracking.md) sur votre site web. Lorsqu’un utilisateur non-cookie accède à une page contenant le code de suivi Munchkin, un nouveau cookie est créé dans le navigateur de l’utilisateur et un nouveau prospect anonyme est créé dans Marketo. Si l’utilisateur est déjà cookie et qu’il existe déjà un prospect dans Marketo, la visite de la page est enregistrée dans le journal d’activité de l’utilisateur dans Marketo. Nous vous montrons tout d’abord comment générer le code de suivi Munchkin dans Marketo, ensuite comment modifier votre exemple de code Munchkin pour ne le déclencher que si certaines conditions sont remplies et enfin comment vérifier qu’une visite de page d’un utilisateur anonyme a été enregistrée dans Marketo.

**Comment générer le code de suivi Munchkin** le code de suivi Munchkin vous permet de suivre les visites sur votre site web. Il existe trois types de code Munchkin décrits ci-dessous, mais dans cet exemple, nous utilisons le code de suivi Munchkin asynchrone . A) Simple : contient le moins de lignes de code, mais ne s’optimise pas pour le temps de chargement des pages web. Ce code charge la bibliothèque jQuery chaque fois qu’une page web est chargée. B) Asynchrone : réduit le temps de chargement des pages web. Ce code vérifie si la bibliothèque jQuery existe déjà, la charge si elle est manquante et l’utilise pour exécuter le code de suivi une fois le reste de la page web chargé. C) Requête asynchrone : réduit le temps de chargement des pages web et améliore également les performances du système. Ce code suppose que vous disposez déjà de jQuery et ne vérifie pas pour le charger. 1. Cliquez sur Admin en haut à droite de l’application.  1. Cliquez sur Munchkin dans l&#39;arborescence de gauche.  1. Sélectionnez Asynchrone pour le type de code de suivi. 1. Cliquez sur le code de suivi JavaScript à placer sur votre site web, puis copiez-le.
**Exemple de code pour l’utilisateur de cookie et l’événement de suivi** Placez le code de suivi sur vos pages web juste avant la balise `</body>`. Les landing pages créées dans Marketo contiennent automatiquement le code de suivi. Vous n’avez donc pas besoin de placer ce code dessus. Cet exemple de code appelle l’API Munchkin une fois le script chargé :

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Cet exemple de code appelle l’API Munchkin une fois que l’utilisateur a séjourné sur la page pendant 5 secondes et qu’il a également fait défiler la page de 500 pixels vers le bas :

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script>
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked

  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }

     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }
 },5000); //Sets time delay before tracking user
});
</script>
```

**Comment vérifier que la visite d’une page par un utilisateur anonyme a été enregistrée dans Marketo**

1. Cliquez sur Analytics dans le menu supérieur, puis sur Nouveau rapport. Choisissez Activité de page web comme type de rapport, puis nommez le rapport.
1. Après avoir créé un rapport, cliquez sur Liste dynamique. Sélectionnez ensuite le filtre Page web visitée dans la zone de droite. Saisissez la page web dans laquelle vous avez placé le code de suivi Munchkin.
1. Cliquez sur Configuration. Sélectionnez Visiteurs anonymes à partir de FAI et définissez l’option sur Affiché .
1. Cliquez sur Rapport. L’activité est désormais suivie sur la page web que vous avez sélectionnée.
1. Double-cliquez sur l’enregistrement du prospect, qui affiche alors le journal d’activité sur lequel vous pouvez voir la page spécifique visitée par l’utilisateur anonyme.

Cet article contient le code utilisé pour implémenter des intégrations personnalisées. En raison de sa nature personnalisée, l’équipe d’assistance technique de Marketo ne peut pas résoudre les problèmes liés au travail personnalisé. N’essayez pas de mettre en œuvre l’exemple de code suivant sans expérience technique appropriée ou sans accès à un développeur expérimenté.

Publié le _2014-04-17_ par _Murta_

## Modifier dynamiquement le numéro de téléphone local à l’aide de RTP

Personalization, c’est tout. Nous l’avons compris il y a longtemps. Cela étant dit, je suis toujours étonné que chaque fois que j&#39;ai besoin d&#39;aide immédiate, il soit si difficile de trouver les numéros de téléphone locaux pertinents sur un site Web. Heureusement, [Marketo Real-Time Personalization](https://business.adobe.com/fr/products/marketo/content-personalization.html) (RTP) est installé sur <https://business.adobe.com/fr/products/marketo/adobe-marketo.html>. Nous pouvons tirer parti de l’[API visiteur RTP](/help/javascript-api/web-personalization.md) pour modifier de manière dynamique le numéro de téléphone qu’un visiteur web voit dans différentes sections du site web. Waouh ! Pouvez-vous le croire ? Comment fonctionne cette magie ? Tout d’abord, le RTP doit être installé sur votre site web comme décrit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript). Suivez ensuite les instructions ci-dessous et implémentez le code JavaScript sur votre site web :

1. Insérez votre numéro de téléphone international dans la configuration **defaultPhone**
1. Insérez le ou les identifiants des éléments HTML dans la configuration **divIds**
1. Si vous souhaitez que le numéro de téléphone soit un lien de clic à l’appel pour les navigateurs mobiles, définissez la configuration **mobileLink** sur **true**.
1. Mappez les différents emplacements avec leurs numéros de téléphone dans les configurations **cityPhone**, **statePhone** et **countryPhone**

Par exemple, voici des exemples de valeurs pour les paramètres de configuration :

```json
  defaultPhone:"+1.503.608.4679", // Optional
  divIds:["phoneId1","phoneId2"],
  mobileLink: true,
  cityPhone: {
    "<a href="#">yanir</a>": ["San Mateo", "San Francisco"],
    "+353.1.242.3000": ["tel-aviv"]
  },
  statePhone: {
    "+1.650.376.2300": ["CA"],
    "+1.650.376.2302": ["OR"]
  },
  countryPhone: {
    "+1.650.376.2300": ["United States"],
    "+353.1.242.3000": ["Israel"]
  }
```

Enfin, insérez une balise d’ancrage HTML contenant un identifiant correspondant à l’un des identifiants de **divIds** (voir l’étape 2 ci-dessus). Par exemple, si vous avez spécifié « phoneId1 » dans **divIds**, la balise d’ancrage HTML se présente comme suit :

`<a href="tel:+1800229933" id="phoneId1">+1800229933</a>`

Le script vérifie s’il existe une correspondance dans cet ordre : cityPhone > statePhone > countryPhone > defaultPhone Vous pouvez également remplacer les numéros de téléphone par le texte (exemple : « Rejoignez notre groupe d’utilisateurs de San Francisco ! »). ou le code HTML et modifiez dynamiquement le contenu en fonction de la géolocalisation. Bon appétit !

```html
<a href="tel:+1800229933" id="phoneId1">+1800229933</a>
<script>
(function(a){
    rtp('get','visitor',function(yc){
        var location = yc.results.location;
        var loop = true;
        var phoneChanged = false;
        console.log(yc.results);
        function checkObj(obj){
            return Object.getOwnPropertyNames(obj).length >0;
        }
        function changePhone(p){
            d=a.divIds;
            for(i=0;i<d.length;i++){
                if(document.getElementById(d[i]) !== null){
                    document.getElementById(d[i]).innerHTML = p;
                    if(a.mobileLink){
                        document.getElementById(d[i]).href= "tel:" + p;
                    }
                    console.log(p);
                }
            }
            loop = false;
            phoneChanged = true;
        }
        function matchLocation(loc,objc){
            for (var key in objc) {
                for(i=0;i<objc[key].length && loop;i++){
                    if (!loop) { return true;};
                    val = objc[key][i];
                    //console.log(loc + location[loc] + " ? " + val);
                    if(location[loc].toLowerCase() === val.toLowerCase()){
                        changePhone(key);
                    }
                }
            }
        }
        if(checkObj(a.cityPhone)){
            matchLocation("city",a.cityPhone);
        }else if(checkObj(a.statePhone)){
            matchLocation("state",a.statePhone);
        }else if(checkObj(a.countryPhone)){
            matchLocation("country", a.countryPhone);
        }else if(!phoneChanged && a.defaultPhone.length > 0 ){
            changePhone(a.divId,a.defaultPhone);
        }
    });
})({
    defaultPhone:"",  //  [Optional] the number to show if visitor does not match the mapping below
    divIds:["phoneId1","Floater"],  //the phone HTML element ID, can be <div>, <a>, <span>, <p> etc.
    mobileLink: true,  //if you use click to call link (with href="tel:") you can also change its number

    cityPhone: {
        "<a href='#'>yanir</a>": ["San Mateo", "San Francisco"],
        "+353.1.242.3000": ["tel-aviv"]
    },
    statePhone: {
        "+1.650.376.2300": ["CA"],
        "+1.650.376.2302": ["OR"]
    },
    countryPhone: {
        "+1.650.376.2300": ["United States"],
        "+353.1.242.3000": ["Israel"]
    }
});
</script>
```

Publié le _2016-02-02_ par _Yanir_

## Mises à jour de l&#39;hiver 2016

### Objets personnalisés

* Les relations [N pour les objets personnalisés:N sont désormais prises en charge](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects)
   * Les enregistrements de leads ou de comptes peuvent désormais avoir des relations multiples-à-multiples par le biais d’objets personnalisés via la définition d’objets intermédiaires. Après avoir créé un type d’objet personnalisé autonome, vous pouvez créer un type d’objet intermédiaire avec des champs de lien à la fois vers l’objet autonome et vers les prospects ou les comptes.
   * Il n’existe aucun nouvel appel API pour cette fonctionnalité, mais les définitions d’objet doivent être configurées correctement pour tirer parti de ces relations via l’API.
* `getLeadActivities` et `getLeadChanges` ne renverront plus les activités de prospects anonymes. Pour plus d’informations, consultez la [FAQ sur le suivi de Munchkin de nouvelle génération](https://experienceleague.adobe.com/fr/docs/marketo/using/home)

Publié le _2016-02-05_ par _Kenny_

## Récupérer des activités pour un prospect unique à l’aide de l’API REST

Voici une question que notre communauté de développeurs et développeuses ne cesse de nous poser :

« Comment puis-je obtenir une liste des activités passées pour un prospect individuel ? »

Jusqu’à récemment, il n’existait aucun moyen simple d’y parvenir à l’aide de l’API REST. Mais maintenant, il y en a ! La version d’hiver 2016 de notre API REST contient une petite amélioration. [Obtenir les activités de lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) accepte désormais le paramètre **leadIds** qui peut être utilisé pour spécifier un ID de lead. Lorsque le paramètre **leadIds** est spécifié, seules les activités correspondant à cet ID de lead sont renvoyées. Vous pouvez considérer cela comme un filtre d’ID de prospect. Notez que le paramètre **leadIds** peut prendre une liste d’ID de lead séparés par des virgules si vous souhaitez filtrer les résultats sur plusieurs leads (jusqu’à 30). Cela peut s’avérer utile, par exemple, lors de la limitation des activités aux prospects d’une entreprise spécifique. **Exemple** Vous trouverez ci-dessous un exemple de requête pour [Obtenir les activités de lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) qui contient le paramètre **leadIds**. J’ai spécifié une valeur de « 50 » pour le paramètre **leadIds**, qui correspond à un prospect arbitraire dans mon instance Marketo. J’ai spécifié une valeur « 129 » pour le paramètre activityTypeIds, qui correspond à l’activité « Session d’application mobile » sur mon instance Marketo.

`<https://123-abc-456.mktorest.com/rest/v1/activities.json?leadIds=50&activityTypeIds=129&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3J4SMAZRQO4RKIXCEMLFCM2APRSQ====>`

Vous trouverez ci-dessous un extrait de la réponse à cette requête ci-dessus. Comme vous pouvez le voir, il contient uniquement des objets de résultat avec « leadId » : 50 et « activityTypeId » : 129.

```json
{
    "id": 846,
    "leadId": 50,
    "activityDate": "2015-04-06T21:58:59Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 7
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 879,
    "leadId": 50,
    "activityDate": "2015-04-07T00:45:11Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 5
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1114,
    "leadId": 50,
    "activityDate": "2015-04-08T00:02:41Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 241
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1551,
    "leadId": 50,
    "activityDate": "2015-04-09T23:31:56Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1716,
    "leadId": 50,
    "activityDate": "2015-04-15T22:44:19Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
```

## Intégration transparente à Marketo et plus de 500 applications à l’aide de Zapier

Voici un article de Philippe Delle Case, Consultant principal en solutions, Marketo.

### Objectifs

Cet article explique en détail comment intégrer Marketo à potentiellement plus de 500 applications cloud, grâce à Zapier. Pour cela, nous allons créer de toutes pièces un connecteur Zapier pour Marketo et implémenter deux cas pratiques d’utilisation de l’intégration : Cas d’utilisation 1 : intégration unidirectionnelle des leads depuis FullContact Card Reader vers Marketo

* Scannez la carte de visite de n&#39;importe quel contact avec l&#39;application FullContact Mobile Card Reader et obtenez une piste créée automatiquement dans Marketo.
* Ajoutez un prospect existant à une liste statique dans Marketo et recherchez-le automatiquement ajouté à votre feuille Google.
* Modifiez n’importe quel prospect de votre feuille de Google et recherchez la modification renvoyée à Marketo.

### Conditions préalables

**Inscrivez-vous à un compte gratuit avec Zapier** [Zapier](https://zapier.com/) est un service d&#39;automatisation des applications web qui vous permet d&#39;automatiser facilement les tâches entre d&#39;autres applications en ligne sans avoir besoin de programmeurs ni de ressources informatiques. Une brève introduction est disponible [ici](https://zapier.com/api/v4/helpdocs/category/redirect/what-is-zapier). Aujourd&#39;hui, Zapier prend en charge plus de 500 applications dans de nombreux domaines différents tels que le marketing, CRM, CMS, le service clientèle, la signature électronique, Forms, etc. Une seule intégration entre une application et une autre est appelée Zap. Consultez le [zapbook](https://zapier.com/apps) de Zapier pour obtenir une liste exhaustive des applications web prises en charge.  Inscrivez-vous gratuitement [ici](https://zapier.com/sign-up/). Vous avez accès à un maximum de 100 tâches/mois, 5 zaps et zaps s’exécutant toutes les 15 minutes. Vous pouvez bien sûr obtenir beaucoup plus en souscrivant aux forfaits payants de Zapier (de base, business, business plus, etc.)

**Accès à une instance Marketo en tant qu’administrateur ou avec un compte utilisateur d’API fourni** Notre connecteur Zapier utilisera l’API REST Marketo pour transmettre les données de lead à Marketo. Pour utiliser cette API, vous avez besoin d’un utilisateur de l’API et d’un service personnalisé que vous pouvez créer vous-même si vous êtes administrateur de votre instance Marketo. Si ce n’est pas le cas, un administrateur ou une administratrice devra vous les fournir. Il existe également un Webhook à créer, accessible uniquement par un administrateur Marketo. Une explication détaillée de la création de l’utilisateur de l’API Marketo et du service personnalisé est disponible [ici](http://rest-api/custom-services/). Une fois que vous avez terminé, vous devez disposer des informations d’identification suivantes pour appeler l’API REST Marketo : ID client, secret client, ID de compte Munchkin, ID de compte Munchkin

Vous pouvez obtenir l’ID de compte Munchkin à partir des écrans Munchkin ou Web Services Admin . Son modèle ressemble à ceci : `000-XXX-000`.  Pas besoin d’obtenir un jeton d’accès, car il ne serait valide que pendant une seule heure. Le connecteur génère automatiquement des jetons pour vous.
**L&#39;inscription à un compte gratuit avec Google Docs, Sheets et Slides sont des applications de productivité qui vous permettent de créer différents types de documents en ligne, de travailler sur eux en temps réel avec d&#39;autres personnes et de les stocker en ligne dans votre Google Drive. Notre cas d’utilisation nécessite une feuille de Google. Les différentes fonctionnalités de Google Docs et de création de compte avec Google sont disponibles [ici](https://workspace.google.com/products/docs/).
**Inscrivez-vous à un compte gratuit avec FullContact** FullContact vous permet de rester pleinement connecté aux personnes qui comptent le plus en extrayant tous vos contacts et en les synchronisant en permanence avec les modifications apportées aux profils sociaux, aux photos, aux signatures d&#39;e-mails, aux informations sur l&#39;entreprise, etc. Ils offrent un lecteur de cartes de visite mobile qui peut numériser des cartes dans plus de 250 applications Web, y compris Zapier. Vous pouvez créer un compte gratuit [ici](https://app.fullcontact.com/login). Vous pouvez également vous abonner à un compte premium payant avec plus de fonctionnalités et de capacité. L&#39;application mobile peut être téléchargée à partir de [Apple AppStore](https://apps.apple.com/us/app/fullcontact-business-card/id576780807) ou de [Google Play](https://play.google.com/store/apps/details?id=com.fullcontact.cardreader). Les Zap FullContact sont documentées [ici](https://zapier.com/apps/contacts-plus/integrations).

### Mise en œuvre du connecteur Marketo pour Zapier

**Création de l&#39;application Marketo** Dans l&#39;interface web de Zapier, accédez au portail destiné aux développeurs. Cliquez sur **Ajouter une nouvelle application** et renseignez au minimum le titre (par exemple, « Marketo ») et la description. Le logo est facultatif, mais agréable à avoir.\ **Authentification** Dans cette section, nous déclarons les différents champs utilisés pour l’authentification de l’API REST Marketo et les paramètres d’authentification. Créez d’abord les champs suivants :

Modifiez les « Paramètres d’authentification » :

* Type d’authentification : Authentification de session
* Mappage d’authentification :

  `{"access_token":"{{access_token}}"}`

* Placement du jeton d’accès **:** dans Querystring

Une fois qu’un service personnalisé Marketo a été créé, l’identifiant client et le secret client sont disponibles. Nous utilisons l’ID client et le secret client pour générer un jeton d’accès via le point d’entrée de l’API REST [Authentification](/help/rest-api/authentication.md). Nous pouvons ensuite utiliser ce jeton d’accès pour effectuer des requêtes ultérieures auprès de l’API REST. Le jeton expire au bout d’une heure et doit être généré à nouveau pour continuer à appeler l’API REST. Nous avons choisi Type d’authentification = « Authentification de session », car il nous permet d’exécuter un script d’authentification personnalisé chaque fois que notre jeton de session expire. Dans la section « API de script », nous verrons comment implémenter ce mécanisme qui ne peut fonctionner qu’avec ce type d’authentification.
**Triggers** Zapier Triggers sont là pour importer des données dans Zapier. Nous n’en avons pas besoin pour nos cas d’utilisation, car nous utiliserons un Webhook Marketo à la place. Cependant, nous devons toujours écrire un déclencheur factice en tant que test obligatoire pour notre connecteur Marketo. Nous allons créer un déclencheur de test appelant l’API REST Marketo [Obtenir le point d’entrée d’utilisation quotidienne](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyUsageUsingGET). Cliquez sur **Ajouter un nouveau déclencheur** pour démarrer l&#39;assistant et remplir les champs suivants (les champs non mentionnés peuvent être laissés vides) : Nom et Description

* Nom : Déclencheur de test
* Clé : test_trigger
* Description : déclencheur de test de l’application Marketo
* Important ? Non coché
* Se cacher ? Coché

Champs de déclenchement

* Aucune

D’Où Proviennent Les Données

* Source de données : interrogation
* URL d&#39;interrogation : `https://{{munchkin_account_id}}.mktorest.com/rest/v1/stats/usage.json`

Exemple de résultat

* Laisser vide

Cliquez sur **Gérer les paramètres de déclencheur** et définissez notre déclencheur de test sur celui que nous utiliserons pour vérifier les informations d’identification d’un utilisateur. **Actions** Zapier Les actions sont là pour envoyer des données depuis Zapier. Nous allons mettre en œuvre l’action de lead Create_Update appelant l’API REST Marketo. Cette action permet de créer un prospect dans Marketo ou, si le prospect existe déjà, de le mettre à jour avec les valeurs envoyées. Nous utiliserons le champ « e-mail » pour la déduplication. Cliquez sur **Ajouter une nouvelle action** pour démarrer l&#39;assistant et renseigner les champs suivants (les champs non mentionnés peuvent être laissés vides) : Nom et Description

* Nom : Lead Create_Update
* Nom : Lead
* Clé : create-update-lead
* Description : créez un prospect dans Marketo ou, si le prospect existe déjà, mettez-le à jour avec les valeurs envoyées
* Important ? Coché
* Se cacher ? Non coché

Champs d’action Les champs d’action sont les champs dans lesquels les utilisateurs mapperont les données. Choisissez-les soigneusement en fonction de vos propres besoins, car elles représentent toutes les données que vous pouvez mettre à jour dans Marketo. Il existe une option dans Zapier pour proposer à l&#39;utilisateur final tous les champs disponibles dans Marketo, mais cela induirait plus de code et de complexité, non nécessaire pour un connecteur jetable. A titre d&#39;exemple, nous avons sélectionné les champs suivants.

Le nom de la partition est obligatoire dans notre cas, car des partitions de lead sont en service dans notre instance Marketo. Sinon, il pourrait être omis. Nous l’avons séparé du groupe « entrée » afin que l’utilisateur final comprenne qu’il ne s’agit pas d’un champ à synchroniser. Le champ « Notes » provient d’une synchronisation entre Marketo et Salesforce. Ne l’utilisez pas si vous ne l’avez pas dans votre instance Marketo. Le champ « Appelé » a été créé dans votre instance Marketo. Ne l’utilisez pas si vous ne l’avez pas dans votre instance Marketo. Bien sûr, l’objectif est de vous permettre de choisir les champs dont vous avez besoin dans Marketo. Il est recommandé de commencer petit et d’ajouter les champs supplémentaires ultérieurement. Où envoyer les données

* URL du point d’entrée de l’action : `https://{{munchkin_account_id}}.mktorest.com/rest/v1/leads.json`

Exemple de résultat

* Laisser vide

### API de script Zapier

La fonctionnalité de script de Zapier vous permet de manipuler les requêtes et les réponses échangées entre l’API de votre application et Zapier. Vous pouvez modifier les requêtes HTTP juste avant leur envoi et analyser les réponses avant que Zapier n&#39;en fasse quoi que ce soit. Nous en avons besoin pour terminer notre authentification « Authentification de session » personnalisée afin qu’elle fonctionne avec Marketo. Vous trouverez plus d’informations [ici](https://zapier.com/developer/documentation/scripting/). Copiez le code suivant et nous passerons en revue quelques explications plus loin :

```javascript
var Zap = {

    get_session_info: function(bundle) {

       console.log('Entering get_session_info method ...');

         var access_token,
            access_token_request_payload,
            access_token_response;


        // Assemble the meta data for our Access Token swap request
         console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
        access_token_request_payload = {
            method: 'POST',
            url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
            params: {
                'grant_type' : 'client_credentials',
                'client_id' : bundle.auth_fields.client_id,
                'client_secret' : bundle.auth_fields.client_secret
            },
            headers: {
                'Content-Type': 'application/json',  // Could be anything.
                Accept: 'application/json'
            }
        };

        // Fire off the Access Token request.
        access_token_response = z.request(access_token_request_payload);

        // Extract the Access Token from returned JSON.
        access_token = JSON.parse(access_token_response.content).access_token;
        console.log('New Access_Token=' + access_token);

        // This will be mixed into bundle.auth_fields in future calls.
        //bundle.auth_fields.access_token=access_token;
        return {'access_token': access_token};
    },


    test_trigger_pre_poll: function(bundle) {

         console.log('Entering test_trigger_pre_poll method ...');

         bundle.request.params = {
         'access_token':bundle.auth_fields.access_token
         };

         return bundle.request;

    },


    test_trigger_post_poll: function(bundle) {

        console.log('Entering test_trigger_post_poll method ...');

        var data = JSON.parse(bundle.response.content);
        if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

           throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }

        return JSON.parse(bundle.response.content);
    },

    create_update_lead_pre_write: function(bundle) {

       bundle.request.params = {'access_token':bundle.auth_fields.access_token};
       return bundle.request;
    },

    create_update_lead_post_write: function(bundle) {

         var data = JSON.parse(bundle.response.content);
         if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
            throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }
        return JSON.parse(bundle.response.content);
    }
};
```

**Méthodes** **get_session_info**

* Cette méthode est chargée de générer ou de régénérer un jeton d’accès appelant le point d’entrée de l’API REST Marketo [authentification](/help/rest-api/authentication.md).
* Elle est appelée chaque fois qu’une méthode « post_poll » rencontre une erreur « Jeton d’accès expiré ». Un jeton d’accès doit expirer toutes les 1 heure, c’est pourquoi cela est attendu.
* URL du point d’entrée de l’action : https://{{munchkin_account_id}}.mktorest.com/identity/oauth/token

**pre_poll, pre_write**

* Nous devons créer une méthode de « pré-sondage » sur tout déclencheur que nous avons créé afin de modifier la requête HTTP juste avant son envoi et de pouvoir ainsi ajouter le jeton d’accès Marketo à ses paramètres.
* Pour la même raison, nous devons créer une méthode de « pré-écriture » sur toute action que nous avons créée.

**post_poll, post_write**

* Nous devons créer une méthode &#39;post-sondage&#39; sur tout déclencheur que nous avons créé, pour analyser les réponses avant que Zapier n&#39;en fasse quoi que ce soit, et finalement intercepter l&#39;erreur &#39;Jeton d&#39;accès expiré&#39;.
* Pour la même raison, nous devons créer une méthode « post-écriture » sur toute action que nous avons créée.
* Si une telle erreur s&#39;est produite, nous lançons une InvalidSessionException qui indiquera à Zapier de relire l&#39;authentification et d&#39;exécuter à nouveau la méthode get_session_info.

Notez que vous pouvez accéder aux journaux de bundle à partir de l’API de script dans le menu « Liens rapides » dans le coin supérieur droit de l’écran. Cela s’avère très utile pour déboguer les scripts.

Cas d’utilisation 1 : intégration de Marketo à FullContact Card Reader

Pour cette intégration, nous créons un Zap unique de FullContact à Marketo. Avec ce Zap, vous pouvez numériser des cartes de visite avec le Reader FullContact Mobile Card et pousser les leads vers Marketo.   **Zap FullContact -> Marketo** Dans le tableau de bord Zapier, cliquez sur le bouton &#39;Créer un nouveau Zap&#39;.

**Déclencheur dans Zapier**

* Choisir l’application FullContact
* Choisir le déclencheur FullContact &#39;Nouvelle carte de visite&#39;
* Connexion à votre compte FullContact
* Tester l’application FullContact

**Action dans Zapier**

* Sélectionnez le Marketo d’application que nous venons de créer, il doit s’afficher dans Beta
* Choisir L’Action Marketo « Lead Create_Update »
* Connectez-vous à votre compte Marketo en renseignant les paramètres d’authentification (ID de compte Munchkin, ID client, secret client).
* Mapper les champs de FullContact à Marketo
* Complétez éventuellement un Nom de partition où vos nouveaux prospects doivent être placés (uniquement si des partitions existent dans votre instance Marketo).
* Tester l’application Marketo
* Activer votre Zap

Remarque : Veillez à télécharger les cartes de visite Reader depuis FullContact et à activer l&#39;intégration Zapier directement depuis votre appareil mobile.

Cas d’utilisation 2 : intégration de Marketo à Google Sheets-

Pour cette intégration, nous créons deux Zap. Une de Marketo vers Google Sheets et une autre de Google Sheets vers Marketo. Avec ce Zap, vous pouvez synchroniser certains de vos leads ou contacts entre Marketo et une feuille de Google.   **Webhook Zap Marketo -> Feuilles De Google**
Pour le premier Zap, nous ne dépendons pas d’un connecteur personnalisé pour Marketo, mais nous utilisons les Webhooks Marketo et le déclencheur « Webhooks by Zapier ». Dans le tableau de bord Zapier, cliquez sur le bouton &#39;Faire un nouveau Zap&#39;. **Déclencheur Partie 1 dans Zapier**

* Sélectionnez l’application Trigger « Webhooks by Zapier »
* Cochez &#39;Catch Hook&#39; qui permet d&#39;attendre un POST ou un GET vers une URL Zapier
* Pas besoin de prendre une clé enfant
* Zapier a généré une **URL webhook** personnalisée à laquelle vous pouvez envoyer des requêtes et la copier dans le presse-papiers

**Webhook dans Marketo (étapes à effectuer par un administrateur)**

* Accédez à Admin -> Webhooks .
* Créez un Webhook appelé « Push Lead to Zapier » et modifiez le formulaire Webhook.

Dans le champ du modèle, déclarez tous les champs du prospect que vous souhaitez transférer à Zapier et exploitez les jetons de Marketo. Pour nos cas d’utilisation, nous utilisons les mêmes champs que ceux que nous avons définis pour le connecteur Zapier personnalisé qui envoie les prospects vers Marketo :

```json
{
"firstName":"{{lead.First Name}}",
"lastName":"{{lead.Last Name}}",
"email":"{{lead.Email Address}}",
"phone":"{{lead.Phone Number}}",
"leadOwner":"{{lead.Lead Owner First Name}} {{lead.Lead Owner Last Name}}",
"leadOwnerEmail":"{{lead.Lead Owner Email Address}}",
"leadNotes":"{{lead.Lead Notes:default=edit me}}",
"called":"{{lead.Called}}"
}
```

* Enregistrer le formulaire
* Pas besoin de mappage de réponse, donc vous avez fini avec le Webhook

**Tester la campagne dans Marketo (étapes à effectuer par un spécialiste marketing ou un administrateur)**

* À partir des Activités marketing, créez une campagne intelligente

À des fins de test, nous allons créer une campagne qui déclenche notre Webhook chaque fois qu’un statut de prospect passe à MQL. Bien sûr, vous pouvez utiliser le Webhook à toute autre fin commerciale.

* Modification de la liste dynamique
* Appeler le Webhook dans le flux
* Planifier la campagne
* Vérifiez que chaque prospect peut parcourir le flux à chaque fois
* Activer la campagne intelligente

**Déclencheur Partie 2 dans Zapier**

* Pour exécuter l’application Trigger « Webhooks by Zapier », nous devons déclencher la campagne intelligente Marketo une fois et capturer le Webhook dans Zapier
* Dans notre cas de test, il nous suffit d’accéder à la base de données des prospects Marketo, d’ouvrir un prospect et de modifier son statut en « MQL »

**Créer la feuille de calcul dans Google Sheets**

* Créer une nouvelle feuille de calcul
* Créer une feuille de calcul ou utiliser la feuille par défaut
* Ajoutez une colonne pour chaque champ à synchroniser à partir de Marketo (ceux déclarés dans le webhook Marketo)

  **Action dans Zapier**

* Choisir les feuilles de Google de l’application
* Cochez l’option Créer une ligne de feuille de calcul .
* Connexion à votre compte Google Sheets
* Sélectionnez votre feuille de calcul Google Sheets
* Sélectionner la feuille de calcul
* Mappez tous les champs entre l’application Trigger « Webhooks by Zapier » et les feuilles de Google.

* Tester l’application Google Sheets
* Activer votre Zap

**Zap Google Sheets -> Marketo**

Dans le tableau de bord Zapier, cliquez sur le bouton &#39;Faire un nouveau Zap&#39;.

**Déclencheur dans Zapier**

* Sélectionnez l’application Trigger App « Google Sheets »
* Cochez la case « Ligne de feuille de calcul mise à jour » qui se déclenche lorsqu’une nouvelle ligne est ajoutée ou modifiée dans une feuille de calcul
* Connexion à votre compte Google
* Sélectionnez la feuille de calcul à déclencher (doit être la même que celle utilisée dans le ZAP précédent) et la feuille de calcul
* Définir la colonne de déclenchement sur « any_column »
* Tester l’application Google Sheets

**Action dans Zapier**

* Sélectionnez le Marketo d’application que nous venons de créer, il doit s’afficher dans Beta
* Choisir L’Action Marketo « Lead Create_Update »
* Connectez-vous à votre compte Marketo en renseignant les paramètres d’authentification (ID de compte Munchkin, ID client, secret client).
* Mapper les champs de Google Sheets à Marketo
* Complétez éventuellement un Nom de partition où vos nouveaux prospects doivent être placés (uniquement si des partitions existent dans votre instance Marketo).
* Tester l’application Marketo
* Activer votre Zap

### Conclusion

Voici quelques idées d’amélioration pour notre connecteur Marketo pour Zapier :

* Ajout d’autres déclencheurs et actions liés à divers objets Marketo (listes, objets personnalisés, etc.)
* Au lieu de coder en dur les champs depuis Marketo, il est possible d’extraire dynamiquement les champs depuis Marketo, mais cela nécessiterait un certain travail de traduction technique entre Marketo et Zapier.
* Partagez le connecteur avec l’équipe de développement et rendez-le disponible pour tous.

Il est possible que Zapier déploie un adaptateur Premium Marketo, ce qui faciliterait considérablement la mise en œuvre de nos cas pratiques. En tout état de cause, cet article peut toujours être utilisé pour intégrer Marketo à Zapier avec un plan Zapier gratuit, ainsi que pour créer des cas d’utilisation personnalisés qui pourraient ne pas être pris en charge par un adaptateur Premium. Nous espérons que vous avez apprécié cet article et qu&#39;il vous aidera à être encore plus efficace avec Marketo et Zapier. Merci !

Publié le _2016-04-17_ par _David_

## Mises à jour du printemps 2016

**API REST**

* API de ressources - Pages web
   * Les **landing pages** sont désormais accessibles via quinze nouveaux points d’entrée, ce qui permet la création, la mise à jour, la suppression, le clonage et la gestion des brouillons pour les landing pages. Les modèles de page de destination ont désormais également les points d’entrée de gestion de brouillon exposés
      * Obtenir les pages de destination
      * Obtenir la page de destination par ID
      * Obtenir la page de destination par nom
      * Créer la page de destination
      * Mettre à jour les métadonnées de la page de destination
      * Obtenir le contenu de la page de destination
      * Ajouter une section de contenu de page de destination
      * Mettre à jour la section de contenu de la page de destination
      * Supprimer la section de contenu de la page de destination
      * Obtenir la section de contenu dynamique
      * Mettre à jour la section de contenu dynamique
      * Ignorer le brouillon de la page de destination
      * Approuver la page de destination
      * Annuler l’approbation du brouillon de page de destination
      * Supprimer la page de destination
   * **Modèles de landing page**
      * Ignorer le brouillon du modèle de page de destination
      * Approuver le modèle de page de destination
      * Annuler l’approbation du modèle de page de destination
      * Supprimer le modèle de page de destination
   * **Forms** a publié 21 nouveaux points d’entrée qui offrent des fonctionnalités complètes de création, d’édition et de gestion via l’API. Les API ne prennent pas en charge les modifications apportées aux formulaires Forms 1.0.
      * Obtenir Forms
      * Obtenir le formulaire par ID
      * Obtenir le formulaire par nom
      * Obtenir la liste des champs de formulaire
      * Mettre à jour la liste des champs de formulaire
      * Créer un formulaire
      * Obtenir la page de remerciement du formulaire
      * Mettre à jour la page de remerciement du formulaire
      * Mettre à jour le formulaire
      * Ignorer le brouillon de formulaire
      * Approuver le formulaire
      * Désapprouver le formulaire
      * Cloner le formulaire
      * Supprimer le formulaire
      * Mettre à jour le champ de formulaire
      * Supprimer le champ de formulaire
      * Mettre à jour les règles de visibilité des champs de formulaire
      * Ajouter un champ de formulaire de texte enrichi
      * Ajouter un jeu de champs
      * Supprimer le champ de l’ensemble de champs
      * Obtenir les champs de formulaire disponibles
      * Modifier la position des champs de formulaire
      * Mettre à jour le bouton Envoyer
   * Lors de l’utilisation de **Obtenir ou parcourir les programmes**, l’identifiant de campagne SFDC est renvoyé pour les programmes liés à une campagne SFDC

**Objets personnalisés** les objets personnalisés prennent désormais en charge les types de données de zone de texte, ce qui permet de stocker des champs de chaîne de 2 000 caractères maximum dans des champs d’objet personnalisés de ce type. **Whiteliste des adresses IP** Les utilisateurs administrateurs pourront désormais gérer une liste blanche d’adresses IP afin d’empêcher tout accès non autorisé via les API. [Vous pouvez en savoir plus sur cette fonctionnalité ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home). **Interface utilisateur d’activité personnalisée** Les utilisateurs administrateurs pourront désormais définir des types d’activité personnalisés dans leur menu d’administration et ajouter des enregistrements aux prospects via l’API [Ajouter des activités personnalisées](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST). [Vous pouvez en savoir plus sur la définition de types d’activité personnalisés ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home).

Publié le _2016-06-01_ par _Kenny_

## Mises à jour de l’été 2016

Pour la version de l’été 2016 du 23 septembre, trois fonctionnalités orientées développeur seront publiées.

### Prise en charge d’Email 2.0 dans l’API REST

Toutes les [API de ressources préexistantes](/help/rest-api/assets.md) qui n’étaient compatibles qu’avec les e-mails et les modèles v1.0 sont désormais activées pour être utilisées avec les ressources de messagerie v2.0.

### Transmettre le lead à Marketo

[Intégrer le lead](/help/rest-api/leads.md) est une autre méthode de synchronisation de leads conçue pour faciliter le déclenchement dans les campagnes intelligentes. Vous pouvez créer un seul élément de journal d’activité, associer un prospect et mettre à jour l’enregistrement du prospect en un seul appel. Cela fonctionne de la même manière qu’un formulaire unique rempli par un prospect. En outre, il est plus facile d’utiliser cette méthode comme méthode proxy pour l’envoi de formulaire au lieu d’utiliser la méthode de synchronisation des prospects existante.

### Compression HTTP

L’API REST peut désormais compresser les réponses à l’aide de la norme définie par la spécification HTTP 1.1. Cela permet de réduire la taille de la réponse, ce qui augmente la vitesse de transfert et réduit l’utilisation de la bande passante.  

Publié le _2016-09-23_ par _Kenny_

## Utilisation de swagger-codegen avec Marketo

[Swagger-codegen](https://github.com/swagger-api/swagger-codegen) est une puissante bibliothèque Java qui peut générer des stubs de serveur et des clients d’API à partir de définitions Swagger. Cela peut considérablement réduire la difficulté et le coût de génération de clients pour une langue spécifique. Pour commencer et générer votre premier client, vous devez d’abord capturer une copie spécifique à l’instance de l’une des définitions Swagger de Marketo. Saisissez l’identifiant Munchkin à partir de l’instance avec laquelle vous souhaitez effectuer le test. Commencez par la définition de l’identité. Maintenant que vous disposez d’une définition spécifique à votre instance, vous devez télécharger et installer swagger-codegen. Le processus est spécifique à votre système d’exploitation et vous pouvez obtenir les instructions [ici](https://github.com/swagger-api/swagger-codegen#prerequisites) Avec les paramètres par défaut, codegen génère un client couvrant tous les points d’entrée et modèles fournis. Ils sont généralement gérés par le biais d’une classe appelée DefaultApi, contenant des méthodes pour appeler les points d’entrée disponibles avec des exemples fournis dans un dossier « docs » (toutes les langues n’incluent pas de modèles à cet effet par défaut). Créons maintenant le premier client. Créez un dossier dans lequel vous souhaitez que votre code réside, puis accédez-y dans votre session de terminal et utilisez la commande de génération pour créer un client dans la langue souhaitée (nous supposons que vous avez utilisé la méthode d’installation homebrew) :

swagger-codegen generate -i $definitionLocation -l $yourLanguage -o $yourLocation

Cela génère le code client à l’emplacement souhaité. Maintenant, envisageons d’utiliser ceci pour appeler le point d’entrée d’identité et obtenir un jeton d’accès :

```java
 public static void main(String[] args){
  String client_id = args[0];
  String client_secret = args[1];
  ApiClient client = new ApiClient();
  DefaultApi id = new DefaultApi();
  try {
   String token = id.identityOauthTokenGet(client_id, client_secret, "client_credentials").getAccessToken();
   System.out.println(token);
  } catch (ApiException e) {
   e.printStackTrace();
  }
 }
```

```php
<?php
require_once(_DIR_ . '/vendor/autoload.php');

$api_instance = new Swagger\\Client\\Api\\DefaultApi();
$client_id = "client_id_example";
$client_secret = "client_secret_example";
$grant_type = "grant_type_example";

try {
    $result = $api_instance->identityOauthTokenGet($client_id, $client_secret, $grant_type);
    print_r($result->getAccessToken);
} catch (Exception $e) {
    echo 'Exception when calling DefaultApi->identityOauthTokenGet: ', $e->getMessage(), "\\n";
}
?>
```

```c#
  public static void Main(string[] args)
  {
      string clientId = "CHANGE ME";
      string clientSecret = "CHANGE ME";

      IdentityApi instance = new IdentityApi();
      ResponseOfIdentity response = instance.IdentityUsingGET(clientId, clientSecret, "client_credentials");

      string message = string.Format("Access Token: {0}, Expires In: {1}, Scope: {2}, Token Type: {3}",
          response.AccessToken, response.ExpiresIn, response.Scope, response.TokenType);
      Console.WriteLine(message);
  }
```

Maintenant que nous savons comment obtenir une autorisation, nous allons examiner des cas d’utilisation plus avancés de clients générés automatiquement dans les semaines à venir.

Publié le _2016-10-10_ par _Kenny_

## Intégration Excel Partie 1 : Extraire et mettre en forme des données Marketo à l’aide de Power Query

Il s’agit du premier d’une série d’articles expliquant comment tirer parti de la technologie Power BI intégrée à Microsoft Excel pour créer une véritable expérience d’analyse commerciale en libre-service avec Marketo. Grâce aux concepts abordés dans ces articles, vous pouvez :

* Importer des données de Marketo dans Excel
* Importer et combiner des données provenant d&#39;autres sources (applications SaaS, bases de données, fichiers plats, etc.)
* Façonner les données pour les besoins de l’entreprise et à des fins d’analyse
* Actualiser à la demande les données depuis Excel
* Création de colonnes calculées et de mesures à l’aide de formules
* Créer des relations entre des données hétérogènes
* Analyse des données et création de rapports avancés avec des tableaux et des graphiques croisés dynamiques
* Produire des visualisations de données étonnantes

### Power Query pour Excel

Ce premier article couvre le processus d’importation et de mise en forme des données à l’aide de la technologie Power Query. Power Query est une technologie de connexion de données qui vous permet de découvrir, connecter, combiner et affiner des sources de données pour répondre à vos besoins en matière d’analyse. Les fonctionnalités de Power Query sont disponibles dans Excel et Power BI Desktop. Power Query peut se connecter à de nombreuses sources de données telles que des bases de données, Facebook, Salesforce, MS Dynamics CRM, etc. Marketo n’est pas pris en charge en standard, mais nous pouvons heureusement utiliser les API REST Marketo pour l’exécution à distance de nombreuses fonctionnalités du système. Power Query est fourni avec un riche ensemble de formules (appelées de manière informelle « M ») qui vous permettent de créer un script pour une source de données personnalisée.

### Connecteur personnalisé

Écrire un seul appel API REST est insignifiant avec Power Query, mais il devient plus difficile de gérer les exigences suivantes :

* Gestion des jetons d’accès avec mécanisme d’authentification et actualisation périodique des jetons
* Mécanisme de pagination pour un grand ensemble de données
* Traitement des erreurs

Cet article explique comment créer un connecteur personnalisé robuste qui peut utiliser les API REST de Marketo pour extraire tous les types de données (leads, activités, objets personnalisés, programmes, etc.). Votre seule restriction est réduite à votre limite de requêtes quotidiennes d’API Marketo. Les concepts présentés ici se concentrent sur Marketo, mais ils peuvent également être utilisés pour intégrer d’autres solutions SaaS qui fournissent une API REST.

### Conditions préalables

#### Power Query

Avant la version d&#39;Excel 2016, Microsoft Power Query pour Excel fonctionnait comme un complément Excel téléchargé et installé sur Excel 2010 ou Excel 2011. Depuis Excel 2016, cette technologie est une fonctionnalité native intégrée au ruban « Données » sous la section « Obtenir et transformer ». Tous les scripts produits pour cet article ont été testés sur Excel 2016 pour Windows. Les concepts devraient être les mêmes pour Excel 2013 ou Excel 2010, mais certaines adaptations pourraient être nécessaires.  Power Query n&#39;est actuellement disponible que sur la version Microsoft Windows d&#39;Excel ; la version Mac n&#39;est malheureusement pas prise en charge.

#### Marketo

Power Query utilisera les API REST Marketo pour accéder aux données de Marketo. Pour utiliser ces API, vous avez besoin d’un utilisateur d’API et d’un service personnalisé que vous pouvez créer vous-même si vous êtes administrateur de votre instance Marketo. Si ce n’est pas le cas, un administrateur ou une administratrice devra vous les fournir. Une explication détaillée de la création de l’utilisateur de l’API Marketo et du service personnalisé est disponible [ici](/help/rest-api/custom-services.md). Une fois que vous avez terminé, vous devez disposer des informations d’identification suivantes pour appeler les API REST Marketo : **ID client** et **Secret client**. Le **point d’entrée de l’API REST** se trouve dans la section API REST de l’administration des services web dans Marketo et il doit présenter le modèle suivant :

`<https://XXX-XXX-XXX.mktorest.com/rest>`

Marketo a une limite de requêtes quotidiennes pour son API. Vous pouvez trouver cette limite dans l’Administration des services web ainsi qu’un rapport de consommation. **Veillez à ne jamais dépasser votre limite quotidienne lors de la conception de vos requêtes, car vous pouvez manquer certaines données dans vos rapports**.

### Création de classeur Power Query

Commencez avec un nouveau classeur Excel. Nous allons créer une feuille de calcul de configuration spécifique pour déclarer tous les paramètres de l’API REST Marketo. Dans cette feuille de calcul, nous créons trois tableaux :

Table « **REST_API_Authentication** » avec les colonnes : **URL** : votre point d’entrée de l’API REST Marketo. **ID client** : à partir de vos informations d’identification OAuth2.0 de l’API REST Marketo. **Secret client** : à partir de vos informations d’identification OAuth2.0 de l’API REST Marketo.
Table « **Portée** » avec les colonnes : **Jeton de pagination depuis Datetime** : date suivant la notation de date standard ISO 8601 (par exemple, « 2016-10-06T13:22:17-08:00 », « 2016-10-06 » sont des dates/heures valides) utilisée pour récupérer les activités Marketo depuis une période donnée, grâce à un jeton de pagination initial « basé sur la date ». Cette date est principalement utilisée pour limiter la quantité de données à importer dans le classeur. **ID de liste** : l’identifiant d’une liste statique dans Marketo qui fait référence à tous les leads/contacts avec lesquels nous avons affaire. Cette liste statique peut être gérée librement dans Marketo (par exemple, une campagne intelligente peut l’alimenter périodiquement ou en temps réel en leads et contacts).
Pour obtenir l’identifiant d’une liste statique, ouvrez-le dans Marketo et obtenez son identifiant numérique à partir de l’URL, par exemple `<https://myorg.marketo.com/#ST3517A1LA1>`, ID de liste=3511. **Pages d’enregistrements max** : utilisé pour nos algorithmes pseudo-récursifs qui parcourent les données de sortie Marketo, à l’aide de jetons de pagination « basés sur la position », avec une capacité de 300 enregistrements max. par page. Puisque c&#39;est notre intérêt d&#39;obtenir autant de documents que possible par page, nous allons nous en tenir à 300. En règle générale, une taille de pages d’enregistrements max définie sur 33,333 signifie une capacité de 33,333 x 300 = 9,9999 millions d’enregistrements ; mais cela signifie également 33,333 000 K sur votre limite de requête quotidienne de l’API Marketo. Les algorithmes s’arrêteront de toute façon dès que toutes les données des requêtes seront obtenues. Ce paramètre n’est donc qu’une limite de sécurité pour une boucle.

Tableau `Leads` avec la colonne : **Champs de lead** : champs de lead séparés par des virgules à rassembler dans Marketo lors de l’interrogation des leads et des contacts. La déclaration d&#39;un tableau dans Excel est simple. Saisissez deux lignes dans la feuille de calcul avec les noms et les valeurs des colonnes, surlignez avec la souris le périmètre du tableau, puis sélectionnez l&#39;icône Tableau dans le menu &#39;Insérer&#39; et donnez-lui un nom. Les noms donnés aux tables et leurs colonnes sont importants car ils seront appelés directement par nos scripts.

## Jeton d’authentification et d’accès

### À propos de l’authentification de l’API REST Marketo

Les API REST Marketo sont authentifiées avec OAuth 2.0 à 2 pattes. Les ID client et les secrets client sont fournis par les services personnalisés que vous définissez. Chaque service personnalisé appartient à un utilisateur API uniquement qui dispose d’un ensemble de rôles et d’autorisations qui autorisent le service à effectuer des actions spécifiques. Un jeton d’accès est associé à un seul service personnalisé.
Le mécanisme d’authentification complet est documenté [ici](/help/rest-api/authentication.md) sur le site du développeur de Marketo. Lorsqu’un jeton d’accès est créé à l’origine, sa durée de vie est de 3 600 secondes ou 1 heure. Chaque appel d’authentification consécutif pour le même service personnalisé renvoie le jeton d’accès actuel avec sa durée de vie restante. Une fois le jeton expiré, l’authentification renvoie un tout nouveau jeton d’accès. La gestion de l’expiration des jetons d’accès est importante pour vous assurer que votre intégration fonctionne correctement et empêche les erreurs d’authentification inattendues de se produire pendant le fonctionnement normal.

#### Créer la requête

Créez une requête en cliquant sur l’icône « Nouvelle requête » dans la section « Get&amp;Transform » du menu « Données ». Sélectionnez une requête vierge pour commencer et donnez-lui un nom tel que « MktoAccessToken ». Lancez l’éditeur avancé à partir du Query Editor afin de pouvoir écrire manuellement des scripts pour certaines formules Power Query. Saisissez le code suivant dans l’éditeur avancé :

```
let
    // Get url and credentials from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
    clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

    // Calling Marketo API Get Access Token
    getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
    TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

    // Parsing access token
    accessTokenStr = TokenJson [access_token]

in
    accessTokenStr
```

Les commentaires incorporés dans le code source, précédés de la mention « / / » rendent le code explicite. Si vous avez besoin d’une référence de fonction, consultez les liens fournis dans la section Référence de cet article. Cliquez sur le bouton « Terminé ». Vérifiez que le jeton d’accès s’affiche correctement en sortie pour la dernière étape appliquée « accessTokenStr ».  Un bref commentaire sur la sécurité dans Excel ; il se peut que l’on vous demande occasionnellement d’activer Connexions de données externes à partir de la bannière jaune. Cela est nécessaire au bon fonctionnement des requêtes.

#### Conversion d’une requête en fonction

Revenez à l’éditeur avancé et encapsulez votre code avec la déclaration de fonction suivante :

```
let
    FnMktoGetAccessToken =()=>

        let
            // Get url and credentials from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
            clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
            clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

            // Calling Marketo API Get Access Token
           getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr,
           TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

            // Parsing access token from Json
           accessTokenStr = TokenJson [access_token]

        in
            accessTokenStr

in FnMktoGetAccessToken
```

La fonction ne prend aucun paramètre en entrée mais les obtient à partir de la feuille de calcul de configuration. Il génère le jeton d’accès en tant que sortie. Renommez votre `FnMktoGetAccessToken` de requête et enregistrez-la. Notez que vous pouvez voir toutes vos requêtes à tout moment dans Excel en cliquant sur le bouton « Afficher les requêtes » dans la section « Obtenir et transformer » du menu Données. Votre fonction doit maintenant être marquée avec l&#39;icône de fonction &#39;Fx&#39;.

### Charger les membres de la liste statique

#### Obtenir les leads

L’API de lead Marketo fournit des opérations CRUD simples par rapport aux enregistrements de lead, la possibilité de modifier l’appartenance d’un lead à des listes et programmes statiques et de lancer le traitement intelligent de campagne pour les leads. Toutes ces fonctionnalités sont documentées [ici](/help/rest-api/leads.md). Un grand nombre d’enregistrements de prospect peut être récupéré en fonction de l’appartenance à une liste statique ou à un programme. À l’aide de l’identifiant d’une liste statique, vous pouvez récupérer tous les enregistrements de prospect qui sont membres de cette liste statique. L’identifiant de la liste est un paramètre de chemin dans l’appel à . Pour plus d’informations, consultez le chapitre « Liste et appartenance à un programme » dans la documentation destinée aux développeurs Marketo. Le nombre maximal d’enregistrements de prospect que nous pouvons obtenir par appel API est de 300. Nous devons donc utiliser des jetons de pagination pour rassembler les enregistrements par page de 300 enregistrements. Nous obtenons le jeton de pagination dans la réponse Json après le premier appel et nous savons que nous avons terminé lorsque le jeton de pagination ne se trouve plus dans la sortie.

### Requête de base

Commençons avec une requête entièrement fonctionnelle visant à télécharger tous les prospects d’une liste statique. Créez une nouvelle requête vierge appelée &#39;MktoLeads&#39; et saisissez le code suivant dans l&#39;éditeur avancé :

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the number of iterations (pages of 300 records) - Table Scoping
    iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    pagingTokenParamStr = "",

    // Function iterating though the pages
    FnProcessOnePage =
    (accessTokenParamStr, pagingTokenParamStr) as record =>
        let

            // Send REST API Request
            content = Web.Contents(getMultipleLeadsByListIdUrl & accessTokenParamStr & pagingTokenParamStr),

            // Recover Json output and watch if token is expired, in that case, regenerate access token
            newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
            getMultipleLeadsByListIdJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(getMultipleLeadsByListIdUrl & newAccessTokenParamStr & pagingTokenParamStr)),

            // Parse Json outputs: data and next page token
            data = try getMultipleLeadsByListIdJson[result] otherwise null,
            next  = try  "&nextPageToken=" & getMultipleLeadsByListIdJson[nextPageToken] otherwise null,
            res = [Data=data, Next=next, Access=newAccessTokenParamStr]
        in
            res,

    // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
    GeneratedList =
        List.Generate(
            ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
            each [i]<iterationsNum and [res][Data]<>null,
            each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
            each [res][Data])
in
    GeneratedList
```

Power Query n&#39;offre pas de fonctions de bouclage traditionnelles (par ex. For-loop, While-loop) et ne prend pas en charge la récursivité. Une bonne solution consiste à implémenter une boucle For à l’aide de List.Generate. Cette fonction est documentée [ici](http://msdn.microsoft.com/query-bi/m/list-generate). Avec la liste. Générez une itération possible sur les pages. À chaque étape de l’itération, nous extrayons une page de données, conservant l’URL qui inclut le jeton de pagination pour la page suivante, et stockons les résultats dans l’élément suivant de la liste générée. Le blog de [Datachant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) a été une excellente ressource pour résoudre ce problème. Notre paramètre &#39;Max Records Pages&#39; permet de limiter le nombre de pages à une plage réaliste, évitant ainsi une boucle infinie. Un autre défi consiste à s’assurer que le jeton d’accès n’a jamais expiré. Le suivi de sa durée de vie restante serait trop complexe avec Power Query. Ainsi, tous les appels à l’API REST sont sauvegardés avec une vérification d’erreur ; si une erreur se produit, nous supposons que le jeton a expiré et nous le renouvelons d’abord et relayons l’appel. Si le deuxième appel échoue, le deuxième échec est notifié à Excel (dans le pire des cas, vous n’obtenez aucune donnée). Lancez la requête après l&#39;avoir enregistrée ou en cliquant sur le bouton &#39;Actualiser&#39; à tout moment.  Dans notre cas, 1364 enregistrements de leads ont été extraits en tenant compte de 5 pages de données dans 5 listes.

### Mise en forme des données

Nous devons structurer les données pour avoir tous ces enregistrements dans une seule liste plate d&#39;enregistrements. Pour ce faire, deux méthodes sont possibles :

* Utilisation de plus de code
* Utiliser l’interface utilisateur de Power Query

Cliquez avec le bouton droit sur la grille de sortie et choisissez &#39;A Tableau&#39; dans le menu contextuel pour la convertir en tableau de listes.  Dans le pop-up « Table de destination », laissez les valeurs par défaut dans les 2 listes de sélection.  Développez maintenant le tableau de listes qui en résulte. Désormais, nous avons tous les enregistrements dans une seule liste. Les enregistrements codés au format Json contiennent les champs et leurs valeurs associées. Développez à nouveau. Dans la fenêtre contextuelle, sélectionnez tous les champs à conserver et décochez la case « Utiliser le nom de colonne d’origine comme préfixe ».  Et voilà ! Tous les enregistrements sont bien affichés dans notre tableau. Si nous rouvrons l’éditeur avancé, nous pouvons constater que 3 lignes de code ont été ajoutées pour donner forme à nos données :

```
# "Converted to Table" = Table.FromList(GeneratedList, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
# "Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
# "Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"}, {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"})
```

Vous pouvez faire beaucoup plus avec Power Query, comme créer des colonnes supplémentaires avec des valeurs calculées, nous verrons d’autres possibilités plus tard. Enregistrons et fermons cette requête. Elle peut désormais être actualisée manuellement à tout moment ou automatiquement par le biais d’actualisations en arrière-plan.

### Redirection des résultats

La question est maintenant de savoir où envoyer les données de résultat. Pointez sur votre requête à l’aide de la souris et sélectionnez le menu « Charger dans... » dans le menu contextuel. Dans la fenêtre contextuelle, vous pouvez sélectionner :

* &#39;Table&#39; si vous souhaitez envoyer toutes les données mises en forme vers une feuille de calcul (nouvelle ou existante),
* &#39;Ne créer une connexion que&#39; si votre objectif est d&#39;effectuer une analyse plus approfondie dans Power Pivot.

La case à cocher &#39;Ajouter au modèle de données vous permet d&#39;exploiter les données du Power Pivot ; c&#39;est ce que nous voulons pour la deuxième partie de cet article.

### Gestion de la pagination

Puisque l’objectif de notre projet est de créer beaucoup plus de requêtes, nous allons effectuer une refactorisation et extraire une fonction réutilisable qui gérerait la pagination. Créez une requête vierge appelée **FnMktoGetPagedData** et saisissez le code suivant dans l’éditeur avancé :

```
let
    FnMktoGetPagedData =(url, accessTokenParamStr, pagingTokenParamStr)=>

    let

        // Get the number of iterations (pages of 300 records) - Table Scoping
        iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],

        // Sub-function iterating though the REST API service result pages
        FnProcessOnePage =
        (accessTokenParamStr, pagingTokenParamStr) as record =>
            let

                // Send REST API Request
                content = Web.Contents(url& accessTokenParamStr & pagingTokenParamStr),

                // Recover Json output and watch if token is expired, in that case, regenerate access token
                newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
                contentJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(url & newAccessTokenParamStr & pagingTokenParamStr)),

                // Parse Json outputs: data and next page token
                data = try contentJson[result] otherwise null,
                next  = try  "&nextPageToken=" & contentJson[nextPageToken] otherwise null,
                res = [Data=data, Next=next, Access=newAccessTokenParamStr]
            in
                res,

        // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
        GeneratedList =
            List.Generate(
                ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
                each [i]<iterationsNum and [res][Data]<>null,
                each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
                each [res][Data])
    in
        GeneratedList

in FnMktoGetPagedData
```

Enregistrez la requête. Nous allons l&#39;utiliser ensuite.

### Requête simplifiée

Réécrivons notre requête &#39;MktoLeads&#39; qui appellera la fonction **FnMktoGetPagedData**.

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,

    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    // No initial paging token required for this call
    pagingTokenParamStr = "",

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getMultipleLeadsByListIdUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

Comme vous pouvez le constater, notre requête est désormais très simple à lire et à gérer. Nous allons à nouveau utiliser la fonction **FnMktoGetPagedData** pour les autres requêtes.

### Chargement d’activités spécifiques à partir d’une période définie

#### Obtenir les activités avec pagination

Marketo permet une grande variété de types d’activités liés aux enregistrements de prospect. Presque chaque modification, action ou étape de flux est enregistrée par rapport au journal d’activité d’un prospect et peut être récupérée via l’API ou exploitée dans les filtres et triggers de liste dynamique et de campagne intelligente. Les activités sont toujours liées à l’enregistrement du prospect via l’ID de prospect, correspondant à l’ID de l’enregistrement, et possèdent également un identifiant entier unique qui leur est propre. La documentation complète sur l’API REST est disponible [ici](/help/rest-api/activities.md).

Il existe un très grand nombre de types d’activité potentiels, qui peuvent varier d’un abonnement à l’autre et avoir des définitions uniques pour chacun. Bien que chaque activité ait son propre id unique, leadId et activityDate, les valeurs primaryAttributeValueId et primaryAttributeValue ont une signification différente. Nous allons nous concentrer sur les moments significatifs, un type d’activités suivies par Marketo avec l’ID 41. Les nouveaux défis que nous allons relever sont les suivants :

* Nous devons lancer un jeton de pagination « basé sur la date » pour définir la période pendant laquelle les activités se sont produites,
* La mise en forme des données est un peu plus difficile, car en fonction des types d’activités, une liste d’attributs spécifiques à une activité est fournie dans Json et doit être analysée et aplatie pour faciliter l’analyse.

#### Jeton de pagination basé sur la date

Nous devons d’abord créer cette fonction pour générer le jeton de pagination « basé sur la date » initial, nécessaire pour définir la période de temps de nos requêtes d’activité. La documentation sur le jeton de pagination se trouve [ici](/help/rest-api/paging-tokens.md). Créez une requête vierge appelée **FnMktoGetPagingToken** et saisissez le code suivant dans l’éditeur avancé :

```
let
    FnMktoGetPagingToken =(accessTokenStr)=>

        let
            // Get url from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],

            // Get Paging Token SinceDatetime from config worksheet - Table Scoping
            mktoPTSinceDatetimeStr = DateTime.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Paging Token SinceDatetime], "yyyy-MM-ddThh:mm:ss"),

            // Building URL for API Call
            getPagingTokenUrl = mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & accessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr,

            // Calling Marketo API Get Paging Token
            content = Web.Contents(getPagingTokenUrl),

            // Recover Json output and watch if access token is expired, in that case, regenerate it
            newAccessTokenStr = if Json.Document(content)[success]=true then accessTokenStr else "?access_token=" & FnMktoGetAccessToken(),
            pagingTokenJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & newAccessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr)),

            // Parsing Paging Token
            pagingTokenStr = pagingTokenJson[nextPageToken]

        in
            pagingTokenStr

in FnMktoGetPagingToken
```

Enregistrez la fonction. Nous allons l&#39;utiliser ensuite.

#### Activités des moments significatifs

Écrivons maintenant la requête &#39;MktoInterestingMomentsActivities&#39; qui appellera les fonctions **FnMktoGetPagedData** et **FnMktoGetPagingToken**.

```
let

    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),

    // Build Get Activities URL
    getActivitiesUrl = mktoUrlStr & "/rest/v1/activities.json?ListId=" & listIdStr & "&activityTypeIds=46",

    // Build Marketo Access Token URL parameter
    accessTokenStr = FnMktoGetAccessToken(),
    accessTokenParamStr = "&access_token=" & accessTokenStr,

    // Obtain date-based paging token used to scope in time the activities
    pagingTokenParamStr = "&nextPageToken=" & FnMktoGetPagingToken(accessTokenStr),

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getActivitiesUrl , accessTokenParamStr, pagingTokenParamStr)

in
    result
```

Le résultat de cette requête est à nouveau une liste de listes. Il nécessite donc un traitement de données supplémentaire pour être utilisé pour l’analyse.

### Mise en forme des données

Réalisons les mêmes opérations de mise en forme que celles que nous avons effectuées pour les prospects :

* Effectuez un clic droit sur la grille de sortie et choisissez &#39;A Tableau&#39; dans le menu contextuel pour la convertir en tableau de listes,
* Développez le tableau de listes qui en résulte,
* Développez une nouvelle fois, en sélectionnant dans le pop-up tous les champs que vous souhaitez conserver (décochez la case &#39;Utiliser le nom de colonne d&#39;origine comme préfixe&#39;).

Vous pouvez voir les colonnes avec leurs valeurs, à l’exception de la colonne « attributs » qui contient toujours une liste d’attributs spécifiques associés aux moments intéressants. Développons ces attributs. Maintenant la liste s&#39;est développée en enregistrements, nous la développons à nouveau, en sélectionnant les champs que nous voulons (nom et valeur de chaque attribut) et nous décochons la case &#39;utiliser le nom de colonne d&#39;origine comme préfixe&#39;.  Par conséquent, toutes nos données sont visibles, y compris les attributs, mais chaque activité de moment intéressante est répartie sur 3 lignes. Cela va être difficile à utiliser pour notre analyse.  Idéalement, nous ne voulons qu’une seule ligne par activité, avec tous les attributs affichés sous forme de colonnes supplémentaires. Nous pouvons facilement le faire en faisant pivoter les 3 attributs de notre tableau. Sélectionnez les 2 colonnes &#39;Nom&#39; et &#39;Valeur&#39; dans les attributs d&#39;activité et cliquez sur &#39;Colonne de tableau croisé dynamique&#39; dans le menu &#39;Transformation&#39;. Demandez les options d’avance dans le pop-up et sélectionnez « Colonne de valeurs » = valeur et « Ne pas agréger » fonction de valeur.  Cliquez sur &#39;OK&#39; et vous devez générer une seule ligne de données par activité.  Les lignes de code &#39;mise en forme des données&#39; suivantes auraient dû être ajoutées automatiquement au script de votre requête :

```
  #"Converted to Table" = Table.FromList(result, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
    #"Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}, {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}),
    #"Expanded attributes" = Table.ExpandListColumn(#"Expanded Column2", "attributes"),
    #"Expanded attributes1" = Table.ExpandRecordColumn(#"Expanded attributes", "attributes", {"name", "value"}, {"name", "value"}),
    #"Pivoted Column" = Table.Pivot(#"Expanded attributes1", List.Distinct(#"Expanded attributes1"[name]), "name", "value")
in
    #"Pivoted Column"
```

### Étapes suivantes

Vous devriez maintenant être en mesure de concevoir toutes les requêtes dont vous avez besoin pour accéder à toutes les données Marketo spécifiques disponibles via ses API REST. Nous espérons que vous avez apprécié cet article et qu’il vous a permis de tirer parti des grands avantages d’Excel et de Marketo combinés. Un exemple de classeur contenant toutes les requêtes est également fourni dans le deuxième article.

### Références

#### Power Query

* [Power Query - Présentation et apprentissage](https://support.microsoft.com/en-us/article/Power-Query-Overview-and-Learning-ed614c81-4b00-4291-bd3a-55d80767f81d)
* [Référence de formule de requête Power](http://msdn.microsoft.com/query-bi/m/power-query-m-reference)
* [Matt Masson Blog](http://www.mattmasson.com/tag/power-query/) fournit quelques bonnes ressources sur Power Query
* Le [blog DataChant](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) est très utile pour la mise en œuvre du mécanisme de pagination

Publié le _2016-10-18_ par _Philippe_

## Mises à jour de l’automne 2016

Dans la version de l’automne 2016, nous ajoutons la prise en charge CRUD des variables et modules Email v2 et la prise en charge CRUD des comptes nommés. Vous pourrez désormais lire et modifier le contenu localement à l’aide des API REST Marketo, ainsi que les déplacer, les réorganiser et les supprimer. Consultez la liste complète des mises à jour ci-dessous :

### API de la base de données du lead

* [**Comptes nommés**](/help/rest-api/named-accounts.md)
   * Nouveaux points d’entrée pour la lecture, la mise à jour et la suppression des comptes nommés
   * Problèmes connus :
      * Depuis la version de l’automne 2016, les prospects ne peuvent plus être associés à des comptes nommés via l’API

### API d’actifs

* [**Email**](https://developer.adobe.com/marketo-apis/api/asset/#operation/describeUsingGET_5)
   * Nouveaux points d’entrée pour la manipulation des variables d’e-mail v2
   * Nouveaux points d’entrée pour la manipulation des modules Email v2
   * Problèmes connus :
      * Les requêtes et mises à jour des sections qui contiennent des jetons prédictifs renvoient une erreur
      * Les e-mails dont les sections de contenu contiennent des jetons prédictifs peuvent ne pas être approuvés à l’aide de l’API

Publié le _2016-12-07_ par _Kenny_

## Pourquoi nous avons pris notre retraite @MarketoDev Twitter Handl3

Nous avons décidé de retirer le pseudo @MarketoDev sur Twitter. Le compte sera désactivé le 9 décembre 2011. Curieux de savoir pourquoi ? **Remontez jusqu’au début de l’année 2014...** Nous venions de lancer le site Marketo Developers pour aider les développeurs à créer en fonction de nos API. Nous voulions accroître la sensibilisation à la plateforme Marketo et réduire les obstacles à l&#39;entrée pour les développeurs. Parallèlement à cet effort, nous avons créé des @MarketoDev pour collaborer avec nos partenaires technologiques et nos clients qui étaient enclins à créer des solutions intégrées avec Marketo. Comme pour toute nouvelle entreprise courageuse, nous n&#39;étions pas sûrs de la façon dont cela se déroulerait. Au départ, nous avons tweeté de nouveaux articles de blog et de nouvelles versions d&#39;API. Les choses allaient bien ; le trafic sur le site des développeurs a augmenté ! Nous avons aussi commencé à recevoir un assortiment de questions auxquelles nous avons répondu avec diligence. **Avance rapide jusqu’à fin 2016...** L’écosystème [LaunchPoint Partner](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) s’est développé et le volume d’activité des tweets a augmenté. Répondre au volume de questions postées à @MarketoDev était devenu un gros travail pour notre petite équipe. Nous avons reçu un nombre croissant de questions générales sur les produits et les ventes, que nous avons redirigées vers @Marketo ou @MarketoCares. Nous avons également constaté que 140 caractères n&#39;étaient tout simplement pas suffisants pour répondre aux questions liées au développement. Répondre à ces questions entraînait souvent de longs et longs fils de conversation, une approche qui ne prenait pas d’ampleur. Enfin, nous avons analysé les sources de trafic pour les visiteurs du site des développeurs et avons constaté que la majorité provient de la recherche organique, et de la fonctionnalité « S&#39;abonner maintenant » sur notre blog. Pour toutes ces raisons, nous avons décidé de @MarketoDev couper la prise. **A partir de maintenant...** Si vous êtes fan de Twitter (et qui ne l&#39;est pas), ne craignez rien ! Notre compte Twitter d’entreprise @Marketo est toujours actif, tout comme notre service clientèle @MarketoCares.

Publié le _2016-12-02_ par _David_

## Intégration d’Excel (partie 2) : création de rapports Marketo avancés et de visualisations de données à l’aide de Power Pivot et Power View-

Il s’agit du deuxième d’une série de deux articles qui expliquent comment tirer parti de la technologie Power BI intégrée à Microsoft Excel pour créer une véritable expérience d’analyse commerciale en libre-service avec Marketo. Grâce aux concepts abordés dans ces articles, vous pouvez :

* Importer des données de Marketo dans Excel
* Importer et combiner des données provenant d&#39;autres sources (applications SaaS, bases de données, fichiers plats, etc.)
* Façonner les données à des fins d’analyse et pour répondre aux besoins commerciaux
* Actualiser les données à la demande dans Excel
* Création de colonnes calculées et de mesures à l’aide de formules
* Créer des relations entre des données hétérogènes
* Analyse des données et création de rapports avancés avec des tableaux et des graphiques croisés dynamiques
* Produire des visualisations de données étonnantes

### Power Pivot et Power View pour Excel

Dans cet article, nous fournissons des exemples de création des éléments suivants :

* Rapports Marketo avancés qui exploitent les relations entre différentes collections de données Marketo à l’aide de Power Pivot
* Optimisation des visualisations statiques et animées à l’aide de Power View

**Power Pivot** est un complément Excel, déjà inclus dans Excel 2016, que vous pouvez utiliser pour effectuer une analyse de données puissante et créer des modèles de données sophistiqués. Avec Power Pivot, vous pouvez regrouper de grands volumes de données provenant de diverses sources, effectuer rapidement des analyses d&#39;informations et partager facilement des informations. Les données extraites de différentes sources de données avec Power Query peuvent être envoyées au modèle de données, à la feuille de calcul Excel, ou aux deux. Dans le premier article, nous avons importé et mis en forme des données à partir de Marketo, puis nous les avons envoyées au modèle de données afin d’effectuer une analyse plus sophistiquée avant de les rendre disponibles dans la feuille de calcul. **Power View** est une alternative au calque de visualisation Excel. Il s’agit d’une expérience interactive d’exploration, de visualisation et de présentation des données qui encourage la création de rapports et de tableaux de bord ad hoc intuitifs.

Toutes les étapes décrites dans cet article ont été testées sur Excel 2016 pour Windows. Les concepts devraient être les mêmes pour Excel 2013 ou Excel 2010 (sans Power View), mais certaines adaptations pourraient être nécessaires. Power Pivot et Power View ne sont actuellement disponibles que sur la version Microsoft Windows d&#39;Excel 2016, la version Office 365 d&#39;Excel 2016 ne prend pas entièrement en charge Power Pivot et Power View.

### Classeur Marketo Power

#### Télécharger le classeur

Dans le premier article, nous avons couvert le processus d’importation et de mise en forme des données à l’aide de la technologie Power Query. Nous avons appris à implémenter des requêtes d’alimentation avancées pour extraire des prospects et des activités de Marketo. Parce que certains d’entre vous souhaitent aller directement au point où ils créent des rapports et des visualisations, sans codage. Ce classeur contient toutes les requêtes détaillées dans le premier article et quelques autres. Nous avons amélioré la gestion des erreurs et ajouté des paramètres supplémentaires dans la feuille de calcul de configuration. Si vous avez lu le premier article, nous vous recommandons de télécharger le classeur Marketo Power et de vérifier ce qui a été ajouté.

Clause de non-responsabilité : le classeur Marketo Power n’est pas un produit Marketo officiel et n’est donc pas pris en charge par Marketo. N&#39;hésitez pas à utiliser et à développer pour vos besoins professionnels personnels, mais faites-le à vos propres risques.

#### Configurer le classeur

Renseignez toutes les informations requises à partir de la feuille de calcul de configuration de Marketo :

* **Authentification de l’API REST Marketo :** requise
* **Portée :** définissez le Jeton de pagination SinceDatetime et l’identifiant de votre liste statique Marketo contenant tous les prospects à analyser
* **Leads :** pour les rapports à venir, vous devez au moins spécifier les champs de lead suivants : `id`, `firstName`, `lastName`, `email`, create`edAt`, `updatedAt`, `title`, `company`, `industry`, `inferredCountry`, `inferredCity`
   * Si les informations sur la ville sont plus précises dans l’un de vos champs personnalisés, vous pouvez utiliser votre propre champ à la place
* **Activités :** les types d’activités à récupérer de la base de données Marketo sont spécifiés ici pour chaque ensemble d’activités. Il n’est pas nécessaire de modifier ceci maintenant.
   * Notez que nous avons fourni une requête utilitaire sur le classeur qui répertorie, directement dans le classeur Excel, tous les types d’activité existants si vous souhaitez ajuster ces informations ultérieurement

Notez que des fenêtres contextuelles liées à la sécurité peuvent s’afficher. Approuvez les connexions externes et définissez-les sur « Public ». Si la fenêtre pop-up ci-dessous s’affiche, conservez le contenu d’accès web « Anonyme ». L’authentification à Marketo est directement gérée par nos requêtes personnalisées. Il n’est donc pas nécessaire d’activer d’autres types d’accès.

#### Télécharger les données Marketo

Assurez-vous d’abord que le paramètre que vous définissez dans la zone Portée de la feuille de calcul de configuration de Marketo n’entraînera pas le téléchargement d’une quantité trop importante de données, dépassant la limite de requêtes quotidiennes de l’API Marketo. Une fois prêt, cliquez sur le bouton « Tout actualiser » du menu « Données » et attendez que toutes les données soient téléchargées dans le classeur. Si des messages d’erreur de formatage s’affichent lors du téléchargement des données, comme « colonne1 introuvable », cela signifie qu’une ou plusieurs requêtes ne parviennent pas à obtenir les données. Le formatage échoue donc également. Réessayez plus tard, si l&#39;erreur persiste, vérifiez votre version d&#39;Excel (n&#39;utilisez pas Excel 2016 d&#39;Office 365). Il est également important de respecter la latence de la plateforme Marketo. Si vous apportez des modifications à une liste statique ou à vos données de prospect, il est préférable d’attendre avant de lancer Power Queries.

### Modélisation des données dans Power Pivot

Ouvrez Power Pivot en cliquant sur le bouton « Gérer » dans le menu Power Pivot, disponible dans la barre de menus supérieure (si non disponible vérifiez votre version d&#39;Excel, Power Pivot peut être installé en tant que complément dans certaines versions d&#39;Excel). Toutes les données téléchargées à partir de Marketo et envoyées au modèle de données doivent être accessibles à partir des différents onglets situés au bas de la fenêtre Power Pivot.

### Expressions d’analyse des données (DAX)

Nous devons enrichir ou reformater les données de certains rapports. Utilisons les expressions d&#39;analyse de données Power Pivot (DAX) pour définir certains calculs personnalisés en tant que colonnes calculées et mesures (également appelées champs calculés). Consultez le lien « DAX dans Power Pivot » dans la section Références pour en savoir plus sur DAX. Assurez-vous que la zone de calcul est affichée dans la fenêtre Power Pivot ; dans le cas contraire, activez-la dans le menu Power Pivot Home.  Sélectionnez l&#39;onglet **MktoLeads** et ajoutez la mesure **Nombre de leads** n&#39;importe où dans la zone de calcul Leads : **Nombre de leads :=**&#x200B;**DISTINCTCOUNT**&#x200B;**([id])**. Cette mesure comptabilise les prospects distincts disponibles dans la liste, en fonction de leur identifiant. Elle prendrait également en compte les éventuels filtres en place dans le cadre d’un rapport. Cette mesure n&#39;est pas vraiment nécessaire puisque les rapports sont capables de résumer le nombre de leads, mais nous l&#39;avons fait pour avoir un nombre de leads avec un nom plus joli que &#39;somme de mktoleads&#39;. Il s’agit également d’un exemple simple qui vous permet d’imaginer facilement des mesures plus complexes effectuant des moyennes, min, max pour un type spécifique de saisie de données (par exemple, tous les prospects ayant un score supérieur à 50, score moyen, etc.). ...).  Maintenant, sélectionnons l&#39;onglet **MktoWebActivities** et créons trois colonnes calculées. Insérez les colonnes calculées suivantes en faisant défiler l’écran vers l’extrême droite du tableau, puis en cliquant sur la colonne « Ajouter une colonne ». **Activity :** obtenez le libellé d’activité convivial en recherchant l’ID d’activité dans la table MktoActivityTypes. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoActivityTypes[name],MktoActivityTypes[id],[activityTypeId])** Year-Month:**&#x200B;** reformattez la date d’activité avec un modèle « AAAAmm » mieux adapté à certains rapports. **\=**&#x200B;**LEFT**&#x200B;**([activityDate],4)&amp;**&#x200B;**MID**&#x200B;**([activityDate],6,2)** Date:**&#x200B;** La date d’activité est une chaîne de notre requête d’origine ; transformez-la en une date appropriée. **\=**&#x200B;**DATE**&#x200B;**(**&#x200B;**LEFT**&#x200B;**([activityDate],4),**&#x200B;**MID**&#x200B;**([activityDate],6,2),**&#x200B;**MID**&#x200B;**([activityDate],9,2))** Créons maintenant les trois mêmes mesures pour l’onglet **MktoEmailActivities** et 2 mesures supplémentaires : **Campaign:** Obtenez le nom convivial de la campagne en recherchant l’identifiant de la campagne dans la table MktoCampagnes. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampagnes[name],MktoCampagnes[id],[campaignId])** Program:**&#x200B;** Obtenez le nom convivial du programme en recherchant l’ID de campagne dans la table MktoCampagnes. Le tableau MktoProgrammes peut fournir plus de détails sur le programme tels que le dossier, l’espace de travail, etc. **\=**&#x200B;**LOOKUPVALUE**&#x200B;**(MktoCampaigns[programName],MktoCampaigns[id],[campaignId])**

### Entity-Relationships

Nous avons vu précédemment un moyen de rechercher des informations à partir d’une autre table dans le modèle pour compléter certaines informations manquantes. Power Pivot offre une option plus puissante pour définir les relations entre certains tableaux du modèle de données, ce qui nous permet d’exploiter ces relations directement à partir des rapports. Définissons les relations clés de nos rapports. Sélectionnez la vue Diagramme dans la fenêtre Power Pivot. Suivez les relations suivantes dans le diagramme de modèle de données :

* **MktoInterestingMomentActivities:leadId →** **MktoLeads:id**
* **MktoScoringActivities:leadId →** **MktoLeads:id**
* **MktoRevenueStageActivities:leadId →** **MktoLeads:id**
* **MktoWebActivities:leadId →** **MktoLeads:id**
* **MktoEmailActivities:leadId →** **MktoLeads : id**

Nous n’utiliserons pas toutes ces relations et tous ces objets dans nos rapports, mais uniquement les prospects, les activités web et les activités e-mail. Il est maintenant temps de créer des rapports.

### Graphique croisé dynamique des performances des e-mails

Ce premier rapport présente les KPI de performances des e-mails basés sur un graphique croisé dynamique Excel standard. Il nous permet de filtrer les données par secteur et/ou par campagne. Vous pouvez créer un graphique croisé dynamique directement à partir du menu Power Pivot en sélectionnant « Graphique croisé dynamique » dans le sélecteur « Tableau croisé dynamique ».  Vous pouvez également créer un graphique croisé dynamique directement à partir de la feuille de calcul Excel, en cochant l’option « Utiliser le modèle de données de ce classeur ».  Faites glisser et déposez les champs des tableaux **MktoEmailActivities** et **MktoLeads**, comme la figure ci-dessous : **MktoEmailActivities.Activity →** **Legend** (utilisez la colonne calculée DAX que nous avons implémentée sur **MktoEmailActivities** auparavant) **MktoEmailActivities.Date →** Axis **(utilisez la colonne calculée DAX que nous avons implémentée sur** MktoEmailActivities **auparavant)** MktoEmailActivities.Id →**&#x200B;**∑ Values **&#x200B;**&#x200B;MktoEmailActivities.Campaign →**Filter** MktoLeads.industry →**Filter** **&#x200B;**&#x200B;**&#x200B;**

Vous pouvez créer un nom personnalisé en sélectionnant « Paramètres de champ de valeur » sur chaque champ déposé. Dans ce cas, nous avons déposé le champ Identifiant de l’activité d’e-mail dans la section « Valeurs de ∑ » et avons modifié son nom personnalisé en tant que « Nombre d’activités ». Maintenant, nous allons configurer le graphique croisé dynamique. Cliquez avec le bouton droit sur le graphique et sélectionnez l&#39;option &#39;Modifier le type de graphique&#39; dans le menu contextuel. Et c&#39;est ainsi que nous avons sélectionné les différents types de graphiques pour toutes les séries de données.

### Carte des leads avec Power View

Le deuxième rapport affiche vos prospects et contacts par zone géographique sur une carte du monde et par secteur d’activité. Power View est nécessaire pour ce rapport. Veuillez suivre le lien de référence ci-dessous pour activer le menu dans Excel. Ou vous pouvez simplement taper « vue d&#39;ensemble » dans la zone de recherche Excel. Sélectionnez « Insérer un rapport Power View ».  Dans le rapport Power View vierge, sélectionnez le tableau **MktoLeads** dans le panneau de droite, puis faites glisser et déposez le champ de l’emplacement du prospect (par exemple **inferredCity**). Le menu « Conception » apparaît maintenant dans le menu principal.

Passez à la visualisation des cartes en sélectionnant « Carte » dans le menu « Conception » de Power View. Faites glisser et déposez les champs du tableau **MktoLeads**, comme la figure ci-dessous : **MktoLeads.industry →** **Color** **MktoLeads.inferredCity →** **Locations** **MktoLeads.Leads Nombre de leads →**∑ Taille **(utilisez la mesure DAX que nous avons implémentée sur** MktoLeads **&#x200B;**&#x200B;précédemment) Et votre carte Leads est prête ! Il vous suffit d’ajuster la taille de la carte, de personnaliser le titre et les légendes. Power View vous permet de créer des tableaux de bord avancés avec plusieurs graphiques sur une seule feuille de calcul. Consultez le tutoriel référencé ci-dessous « [Créer des rapports Power View incroyables](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7) » pour découvrir comment utiliser d’autres composants de tableau de bord avec Power View.

### Activités Web animées sur une carte 3D

Ce troisième rapport affiche vos activités Web de prospect, par secteur, sur une carte du monde en 3D. Il nous faut une carte 3D pour ce rapport. Tapez simplement « 3D » dans la zone de recherche Excel et sélectionnez « 3D Map ». Créez une nouvelle visite guidée à partir de la fenêtre pop-up.  Sélectionnez le graphique à bulles dans le panneau de droite. Faites glisser et déposez les champs des tableaux **MktoLeads** et **MktoWebActivities**, comme dans l’illustration ci-dessous : **MktoLeads.industry →** **Category** **MktoLeads.inferredCity →** **Location** MktoWebActivities.Activity →**Time** (utilisez la colonne calculée DAX que nous avons implémentée précédemment sur **MktoWebActivities** **&#x200B;**. Le champ id peut également être utilisé pour comptabiliser les activités.) **MktoWebActivities.Date →** **Time** (utilisez la colonne calculée DAX que nous avons implémentée sur **MktoWebActivities** auparavant) **MktoWebActivities.Activity** peut également être utilisé comme filtre pour exclure les différents types d’activités web.

Utilisez le bouton « Thèmes » pour modifier les couleurs de votre carte 3D. Ouvrez les « Options de la scène » pour personnaliser vos animations.
Et vous avez fini avec la carte du monde en 3D, maintenant vous pouvez vous amuser à animer le globe et à créer des vidéos à partir de celui-ci.

### Étapes suivantes

Nous venons d&#39;effleurer la surface de ce qu&#39;il est possible de faire avec les outils Excel Power BI. Nous vous recommandons de rechercher sur le Web d’autres articles et tutoriels intéressants pour développer vos compétences Excel et concevoir les rapports dont vous avez besoin pour atteindre vos objectifs commerciaux. Nous espérons que vous avez apprécié ces articles et qu’ils vous ont aidé à tirer parti des grands avantages d’Excel et de Marketo combinés.

### Références

#### Power Pivot

* [Power Pivot : Analyse et modélisation des données puissantes dans Excel](https://support.microsoft.com/en-us/article/Power-Pivot-Powerful-data-analysis-and-data-modeling-in-Excel-d7b119ed-1b3b-4f23-b634-445ab141b59b)
* [Expressions d’analyse des données (DAX) dans Power Pivot](https://support.microsoft.com/en-us/article/Data-Analysis-Expressions-DAX-in-Power-Pivot-bab3fbe3-2385-485a-980b-5f64d3b0f730)

#### Power View

* [Activer Power View dans Excel 2016](https://support.microsoft.com/en-us/article/Turn-on-Power-View-in-Excel-2016-for-Windows-f8fc21a6-08fc-407a-8a91-643fa848729a)
* [Tutoriel : Créer Des Rapports Power View Incroyables](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)

Publié le _2017-02-02_ par _Philippe_

## Modification importante des enregistrements d’activité dans l’API Marketo

**Remarque : cette publication sera mise à jour pour prendre en compte les modifications apportées aux enregistrements d’activité renvoyés par l’API en raison de la migration vers une nouvelle infrastructure.** **Dernière mise à jour : 13 septembre 2018** Avec le déploiement du service d’activité de nouvelle génération de Marketo qui débutera en septembre 2017, nous ne pourrons pas imposer l’unicité ou la présence du champ « id » entier dans les activités, les modifications de valeur de données ou les enregistrements de suppression de piste renvoyés par les API Marketo. Pour éviter toute interruption de service pour les intégrations qui récupèrent les enregistrements d’activité, le champ id doit être traité comme facultatif. Le basculement de cette modification commencera à affecter les abonnements et la prochaine version. Cette modification affectera les points d’entrée suivants : API REST

Les types de SOAP concernés sont `ActivityRecord` et `LeadChangeRecord`.

### Exemples

Les exemples suivants montrent les types d’enregistrements qui seront affectés. Dans les deux exemples, le champ concerné est appelé « id ».

**Exemple de champ REST : id**

```json
  {
      "id" : 102988,
      "leadId" : 1,
      "activityDate" : "2015-01-16T23:32:19Z",
      "activityTypeId" : 1,
      "primaryAttributeValueId" : 71,
      "primaryAttributeValue" : "localhost/munchkintest2.html",
      "attributes" : [
          {
              "name" : "Client IP Address",
              "value" : "10.0.19.252"
          },
          {
              "name" : "Query Parameters",
              "value" : ""
          },
          {
              "name" : "Referrer URL",
              "value" : ""
          },
          {
              "name" : "User Agent",
              "value" : "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
          },
          {
              "name" : "Webpage URL",
              "value" : "/munchkintest2.html"
          }
      ]
  }
```

**Exemple de champ SOAP : id**

```xml
  <activityRecord>
    <id>1030680</id>
    <activityDateTime>2013-07-09T16:44:28-05:00</activityDateTime>
    <activityType>Visit Webpage</activityType>
    <mktgAssetName>ClickDemo</mktgAssetName>
    <activityAttributes>
      <attribute>
        <attrName>Webpage ID</attrName>
        <attrType xsi:nil="true" />
        <attrValue>2547</attrValue>
      </attribute>
      <attribute>
        <attrName>Webpage URL</attrName>
        <attrType xsi:nil="true" />
        <attrValue>/ClickDemo.html</attrValue>
      </attribute>
    </activityAttributes>
    <campaign />
    <personName xsi:nil="true" />
    <mktPersonId>1089965</mktPersonId>
    <foreignSysId xsi:nil="true" />
    <orgName xsi:nil="true" />
    <foreignSysOrgId xsi:nil="true" />
  </activityRecord>
```

Publié le _2017-03-01_ par _Kenny_

## Mises À Jour De L&#39;Hiver 2017

Dans la version d’hiver 2017, nous ajoutons la possibilité d’importer en bloc des données d’objet personnalisées de manière asynchrone et de manipuler des variables dans des pages de destination guidées. Consultez la liste complète des mises à jour ci-dessous.

### API de la base de données du lead

#### Importation en bloc d’objets personnalisés

Nouveaux points d’entrée pour prendre en charge l’importation en bloc d’objets personnalisés. Vous trouverez des détails [ici](/help/rest-api/custom-objects.md).

#### Avis de modification à venir des activités

Notez un changement important qui se produira lorsque Marketo publiera son service d’activités de nouvelle génération. Cette modification affectera les points d’entrée suivants :

SOAP

[getLeadActivity](/help/soap-api/getleadactivity.md), [getLeadChanges](/help/soap-api/getleadchanges.md)

Le champ « id » entier contenu dans les enregistrements renvoyés par ces points d’entrée ne sera plus garanti comme unique. Cela aura un impact sur l’activité, la modification de la valeur des données et les types d’enregistrements de suppression de prospect. Pour éviter toute interruption de service pour les intégrations qui récupèrent ces types d’enregistrements, le champ d’identifiant doit être traité comme facultatif.

#### Suppression du jeton push

Ajout de la possibilité de supprimer des jetons push via l’API SDK. Vous trouverez des détails ici : [iOS](/help/mobile/push-notifications.md), [Android](/help/mobile/push-notifications.md).

Publié le _2017-03-01_ par _David_

## Mises à jour du printemps 2017

Dans la version du printemps 2017, nous ajoutons la possibilité d’extraire en bloc des données de lead et d’objet d’activité de manière asynchrone, et de manipuler des listes de comptes nommés. Consultez la liste complète des mises à jour ci-dessous.

### Extraction en bloc de leads

Nouveaux points d’entrée pour prendre en charge l’extraction de prospects en bloc. Spécifiez des critères de sélection d’enregistrements à l’aide de différentes options. Vous trouverez des détails [ici](/help/rest-api/bulk-lead-extract.md).

### Extraction en masse d’activités

Nouveaux points d’entrée pour prendre en charge l’extraction des activités en bloc. Spécifiez les critères de sélection des enregistrements à l’aide de la période et de la liste des activités. Vous trouverez des détails [ici](/help/rest-api/bulk-extract.md).

### Listes de comptes nommés

Nouveaux points d’entrée pour prendre en charge les opérations CRUD sur les listes de comptes nommés. Vous trouverez des détails [ici](/help/rest-api/named-account-lists.md).

### Autres améliorations

* Le point d’entrée [Obtenir des campagnes](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignByIdUsingGET) vous permet désormais de filtrer les campagnes « déclenchables ». Pour ce faire, transmettez « isTriggerable=true » en tant que paramètre de requête.
* Le point d’entrée [Cloner le programme](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) prend désormais en charge les programmes qui contiennent tous les types de ressources, à l’exception des messages SMS.

### Ionique

Vous pouvez désormais utiliser Marketo Mobile MME et avec le framework d’application [Ionic](https://ionicframework.com/). Vous trouverez des détails [ici](/help/mobile/ionic.md).

### Annuler l&#39;enregistrement du jeton de notification push

Une nouvelle méthode a été ajoutée au SDK pour vous permettre d’annuler l’enregistrement du jeton de notification push avec Marketo. Cela s’avère utile, par exemple, lorsqu’un utilisateur se déconnecte de votre application. Pour plus d’informations, consultez la méthode `unregisterPushDeviceToken` (iOS) ou `uninitializeMarketoPush` (Android) [ici](/help/mobile/push-notifications.md).

Publié le _2017-06-16_ par _David_

## Internet des objets pour les professionnels du marketing avec IFTTT et Zapier

L&#39;Internet des objets (IoT) est l&#39;interconnexion d&#39;appareils connectés, d&#39;appareils électroménagers, de dispositifs portables, de véhicules, etc. avec des composants électroniques intégrés, des logiciels, des capteurs et une connectivité réseau qui permettent à ces objets de collecter et d’échanger des données avec des systèmes d’information cloud. Ces technologies sont en croissance et en évolution si rapide qu&#39;elles auront un impact sur notre façon de vivre, de travailler et de faire des affaires en un rien de temps. Marketo, la principale plateforme d’engagement marketing, est prête pour l’IoT grâce à ses fonctionnalités permettant d’évoluer et d’interagir avec n’importe quel canal de communication. Marketo peut déjà suivre plus de 70 types d’activités liées aux e-mails, au web, aux appareils mobiles, au CRM, etc. Il prend également en charge les [activités personnalisées](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-activities/create-a-custom-activity.html?lang=fr) qui peuvent être alimentées par n’importe quel système tiers. Marketo [objets personnalisés](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects.html?lang=fr) permet d’effectuer le suivi de tous les types de mesures tierces liées à votre entreprise et permet aux spécialistes marketing d’exploiter ces mesures directement à partir des filtres et triggers de campagne intelligente Marketo. L’implémentation de l’IoT pour les consommateurs nécessiterait un serveur centralisé pour interagir avec les appareils des consommateurs et ce serveur échangerait des données avec la plateforme ouverte de Marketo, avec des fonctionnalités telles que l’API REST, les objets personnalisés, les activités personnalisées, etc. - documenté [ici](http://eto.com/). Pas facile à démontrer via un article de blog. Au lieu de cela, nous allons intégrer le service IFTTT à Marketo afin de mettre en œuvre des cas d’utilisation d’IoT intéressants pour les marketeurs comme :

* Revivifier votre équipe marketing chaque fois qu’un prospect est enregistré à un salon professionnel en clignotant une lumière colorée au bureau
* Donner un coup de pouce à votre équipe des ventes chaque fois qu&#39;une transaction est remportée en déclenchant automatiquement une cloche branchée sur une prise d&#39;alimentation connectée
* Publiez automatiquement les jalons de succès marketing sur les réseaux sociaux tels que LinkedIn, Facebook, Slack, etc. ...
* Lancez automatiquement une campagne marketing basée sur :
   * lorsqu&#39;une alerte météorologique se produit (vent, température, pluie, etc.)
   * lorsqu’un nouvel article est publié par un journal comme le New York Times, répondant à certains critères spécifiques
   * quand le Sénat ou la Chambre des représentants des États-Unis voteront
   * lorsque la Station spatiale internationale passe au-dessus d&#39;un certain endroit
   * etc. ...

Vous pouvez trouver ces scénarios amusants mais inutiles, mais ils sont ici pour démontrer une nouvelle façon conceptuelle de faire du marketing non seulement avec les gens, mais aussi avec des choses dans notre monde connecté. Un autre point intéressant abordé dans cet article est comment tirer parti d’une plateforme d’intégration web ouverte telle que Zapier comme une « trappe de service » entre un système tiers et Marketo, pour gérer l’authentification par exemple.

### Le service IFTTT

IFTTT est l&#39;acronyme de « IF This Then That ». Il s’agit d’un service web gratuit que les gens utilisent pour créer des chaînes d’instructions conditionnelles simples, appelées applets. Une applet est déclenchée par des modifications qui se produisent dans certains services web partenaires et, par conséquent, les actions sont envoyées à d’autres services web partenaires. IFTTT a été lancé en 2011 par Linden Tibbets, Jesse Tane, Scott Tong et Alexander Tibbets à San Francisco. À première vue, IFTTT ressemble à un service comme [Zapier](https://zapier.com/) par exemple, il est beaucoup plus axé sur les consommateurs et les appareils IoT (télécommandes, alarmes, lumières, thermostats, voitures, imprimantes, téléphones mobiles, et bien plus encore).  Tout d&#39;abord, vous devez créer un compte pour IFTTT à partir du [site Web IFTTT](https://ifttt.com/explore). N&#39;hésitez pas à découvrir toutes les applets cool déjà disponibles car cela vous donnera d&#39;autres idées de scénarios à coup sûr !

### Canal de création

Une application web qui n’a pas de canal, c’est-à-dire un partenariat avec IFTTT, doit utiliser le canal Maker. Avec le canal Maker, vous pouvez créer des applets qui fonctionnent avec n&#39;importe quel appareil ou application pouvant faire ou recevoir une requête web. Il propose les intégrations suivantes :

* Déclencheurs entrants pour recevoir une requête web d’un système tiers pour déclencher une action
* Actions sortantes pour rendre une requête web à un système tiers accessible au public sur Internet

Dans IFTTT, recherchez le service « Maker » et cliquez dessus.  La première fois, vous devez l&#39;activer en cliquant sur le bouton « Connexion ».  Le canal de création est maintenant actif. Vous pouvez obtenir votre clé secrète en cliquant sur le bouton Paramètres de création : copiez et collez l’URL fournie dans votre navigateur pour plus d’informations.

### Déclencher directement une action IFTTT du marché

Tout d’abord, nous allons nous concentrer sur le déclenchement de toutes sortes d’actions de services web tiers à partir de Marketo. Pour cela, nous allons utiliser un [Webhook Marketo](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook.html?lang=fr). Nous commencerons avec un message push sur votre téléphone mobile ou votre tablette via l&#39;application mobile IFTTT, puis nous implémenterons un scénario IoT clignotant une lumière Philips Hue.

### Webhook Marketo

Le déclenchement d’un événement à partir de Marketo, agissant comme le « si » d’IFTTT, est simple. Il vous suffit d’envoyer une requête web POST à IFTTT avec un nom d’événement et votre clé secrète, en suivant cette URL de modèle :

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}>`

Le Maker permet également de communiquer jusqu’à 3 paramètres via la requête web. Pour ce faire, utilisez des paramètres de requête,

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={value1}&value2={value2}&value3={value3}>`

ou utilisez un corps JSON composé de trois valeurs au maximum :

`{ "value1" : "{value1}", "value2" : "{value2}", "value3" : "{value3}" }`

Dans Marketo, créez un Webhook à partir de l’interface Admin.  Fournissez les informations suivantes pour votre nouveau Webhook :

**Nom du Webhook :** Succès du programme IFTTT

**Description :** déclencher un événement sur IFTTT à partir d’une campagne intelligente pour la réussite d’un programme

**URL:** `<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={{program.name}}&value2={{lead.Email> Address}}&value3={{lead.Full Name}}`

event_name, utilisez MarketoProgramSuccess, par exemple.

secret_key, utilisez la clé secrète de votre service IFTTT Maker

Utilisez du texte statique ou des jetons Marketo pour les trois valeurs disponibles. Vous pouvez envoyer davantage de messages interactifs en définissant vos propres jetons au niveau du programme et en les passant par ces valeurs.

**Type de demande :** POST

**Modèle :** laisser vide

**Encodage Du Jeton De Demande :** Formulaire/Url

**Type de réponse :** Aucun

Il n’est pas nécessaire de définir un mapping de réponse.

### Applet IFTTT

Dans le portail Web IFTTT, sélectionnez « Mes Applets » dans le menu principal.  Cliquez sur le bouton « Nouvelle applet » et cliquez sur la section **+ceci**.  Recherchez le service Maker.  Créez le déclencheur qui se déclenchera chaque fois que le service Maker recevra une demande web pour l’avertir d’un événement. Utilisez le même nom d’événement que celui spécifié dans l’URL de votre Webhook Marketo, par exemple « MarketoProgramSuccess » et cliquez sur le bouton « Créer un déclencheur ».  Il est maintenant temps de spécifier le service d’action en cliquant sur la section **+cela**.  Nous allons commencer avec un service d&#39;action simple que n&#39;importe qui pourrait tester sans avoir à investir dans des appareils IoT, le service de notifications. Recherchez et sélectionnez le service de notifications .
Sélectionnez l’action « Envoyer une notification » qui envoie une notification à vos appareils.  Vous pouvez exploiter les 3 valeurs que vous avez envoyées depuis Marketo en les ajoutant en tant qu’ingrédients pour envoyer une notification significative à l’utilisateur, comme dans l’exemple ci-dessous ... , puis cliquez sur le bouton « Créer une action ». Examinez et terminez l&#39;applet IFTTT. Assurez-vous qu’il est activé.

### Test de l&#39;applet IFTTT

Si vous souhaitez être averti sur votre Mobile, vous devez d&#39;abord télécharger l&#39;application IFTTT pour votre appareil.  Vous pouvez déclencher un événement de succès de programme Marketo à l’aide du Webhook dans un flux de campagne intelligente Marketo. N’oubliez pas que les Webhooks Marketo fonctionnent exclusivement avec des campagnes intelligentes déclenchées (par exemple, déclenchez une fois qu’un formulaire de contact rempli, a été ajouté à une liste, etc.).  Et voici un exemple de notification IFTT sur votre téléphone mobile.

### Découvrons Creative avec IFTTT

IFTTT propose Applet Actions avec plus de 300 partenaires, donc votre portefeuille d&#39;applications et d&#39;appareils ainsi que votre imagination sont les limites ... Prenons un exemple avec les lumières [Hue de Philips](https://www.philips-hue.com/en-us) que vous pouvez acheter n&#39;importe où dans les magasins d&#39;électronique ou en ligne. Les applets suivantes feraient clignoter l&#39;une de vos lumières avec sa couleur actuellement attribuée lorsque Marketo déclencherait le succès d&#39;un programme, ce qui pourrait stimuler votre équipe marketing au bureau. Nous créons une nouvelle Applet, en suivant les mêmes étapes que précédemment, où Marketo est déclenché avec un webhook, mais cette fois nous choisissons l&#39;action du service Philips Hue.

Sélectionnons l’action « Feux clignotants ». L&#39;application demandera à Philips Hue toutes vos lumières disponibles, afin que vous puissiez choisir celle qui clignote. Vous devez d&#39;abord créer un compte avec Philips Hue, le pont de Hue et bien sûr au moins une ampoule de Hue, une barrette de lumière, un projecteur ou une lampe.  Nous venons d&#39;ajouter une nouvelle Applet qui clignotera une lumière colorée chaque fois qu&#39;un prospect est enregistré à une tournée ou un webinaire. Votre équipe marketing se réjouira chaque jour de cette configuration au bureau.

### Exécuter une action Marketo depuis IFTTT, via Zapie

Nous allons maintenant déclencher une campagne intelligente Marketo à partir de la plateforme IFTTT. Pour cela, nous allons utiliser l’API REST [Marketo](/help/rest-api/rest-api.md). Comme cette API est sécurisée et nécessite une authentification OAuth2 avant d’appeler quoi que ce soit, nous devons gérer cette authentification via une autre plateforme telle que Zapier, car IFTTT ne permet pas d’enchaîner deux appels consécutifs sur une API avec le canal Maker. Nous avons choisi le service d’automatisation des applications web [Zapier](https://zapier.com/) depuis que nous avons publié le document présentant déjà Zapier et expliquant étape par étape comment mettre en œuvre un connecteur Marketo personnalisé pour Zapier. D&#39;autres plateformes telles que [Workato](https://www.workato.com/integrations/marketo) pourraient également être une solution.

### Marketo Campaign

Créez votre programme Marketo avec une campagne intelligente planifiée. À des fins de test, vous pouvez créer la campagne dynamique suivante à titre d’exemple : **Liste dynamique** Utilisez uniquement des filtres, et non des déclencheurs. Assurez-vous au moins d&#39;être admissible. **Flux** Envoyez-vous un e-mail ou toute autre notification. **Planification** Assurez-vous de pouvoir exécuter le flux à chaque fois pour gérer vos plusieurs tests. Vous pouvez obtenir l’identifiant de campagne intelligente à partir de l’URL. Exemple : _https://{{marketo_url}}/#SC4289A1_ - l’identifiant de campagne intelligente serait 4289. Vous pouvez déclencher cette campagne à l’aide de l’API REST Marketo. Vous pouvez utiliser par exemple le plug-in [Postman](https://www.postman.com/) pour Chrome et envoyer les 2 appels HTTPS consécutifs suivants : **Étape d’authentification :**

`https://{{Your Munchkin_Account_id}}.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id={{Your_Client_Id}}&client_secret={{Your_Client_Secret}}`

Récupérez le jeton d’accès à partir de la réponse JSON. **Étape de lancement de la campagne :**

`https://{{Your_Munchkin_Account_id}}.mktorest.com/rest/v1/campaigns/{{Campaign_Id}}/schedule.json?access_token={{access_token}}`

### Connecteur personnalisé Zapier intermédiaire pour lancer la campagne Marketo

Nous devons créer un connecteur Zapier personnalisé qui s’authentifie auprès de l’API REST Marketo et lance notre campagne intelligente.

* Conditions préalables
* Mise en œuvre du connecteur Marketo pour Zapier
* Utilisez un autre titre, tel que « Campagne Marketo ».
   * Effectuez l’étape « Authentification ».
   * Exécutez l’étape « Déclencheurs » (requise à des fins de test Zapier).
   * Effectuez l’étape « Actions » spécifique suivante, responsable du lancement d’une campagne Marketo, expliquée ci-dessous :

Où envoyer l’URL du point d’entrée de l’action de données :

`https://{{munchkin_account_id}}.mktorest.com/rest/v1/campaigns/{{CampaignId}}/schedule.json`

Laissez vides les autres champs facultatifs.

#### API de script

La fonctionnalité de script de Zapier vous permet de manipuler les requêtes et les réponses échangées entre l’API de votre application et Zapier. Vous pouvez modifier les requêtes HTTP juste avant leur envoi et analyser les réponses avant que Zapier n&#39;en fasse quoi que ce soit. Nous en avons besoin pour terminer notre authentification « Authentification de session » personnalisée. Vous trouverez plus d’informations dans l’article d’origine. Copiez le code suivant très similaire à l’original. Nous venons de modifier les méthodes d’action :

```javascript
var Zap = {

 get_session_info: function(bundle) {

 console.log('Entering get_session_info method ...');

 var access_token,
 access_token_request_payload,
 access_token_response;


 // Assemble the meta data for our Access Token swap request
 console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
 access_token_request_payload = {
 method: 'POST',
 url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
 params: {
 'grant_type' : 'client_credentials',
 'client_id' : bundle.auth_fields.client_id,
 'client_secret' : bundle.auth_fields.client_secret
 },
 headers: {
 'Content-Type': 'application/json', // Could be anything.
 Accept: 'application/json'
 }
 };

 // Fire off the Access Token request.
 access_token_response = z.request(access_token_request_payload);

 // Extract the Access Token from returned JSON.
 access_token = JSON.parse(access_token_response.content).access_token;
 console.log('New Access_Token=' + access_token);

 // This will be mixed into bundle.auth_fields in future calls.
 //bundle.auth_fields.access_token=access_token;
 return {'access_token': access_token};
 },

 test_trigger_pre_poll: function(bundle) {

 console.log('Entering test_trigger_pre_poll method ...');

 bundle.request.params = {
 'access_token':bundle.auth_fields.access_token
 };

 return bundle.request;

 },

 test_trigger_post_poll: function(bundle) {

 console.log('Entering test_trigger_post_poll method ...');

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);

 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }

 return JSON.parse(bundle.response.content);
 },

 launch_campaign_pre_write: function(bundle) {

 bundle.request.params = {'access_token':bundle.auth_fields.access_token};
 return bundle.request;
 },

 launch_campaign_post_write: function(bundle) {

 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }
 return JSON.parse(bundle.response.content);
 }

};
```

##### Nouveau Zap

Dans le tableau de bord Zapier, cliquez sur le bouton « Créer un nouveau Zap ». **Déclencheur**

* Sélectionnez l’application Trigger « Webhooks by Zapier »
* Cochez la case « Crochet d’accrochage ».
* Pas besoin de prendre une clé enfant
* Zapier a généré une URL webhook personnalisée à laquelle vous pouvez envoyer des requêtes, conservez-la en sécurité quelque part
* Testez l’URL webhook, en démarrant le scénario « IFTTT Applet that calls the Zapier Webhook » ci-dessous. Cela permet à Zapier d’en savoir plus sur la payload webhook et d’attribuer l’identifiant de campagne à l’action

**Action**

* Sélectionnez le connecteur Marketo Campaign précédemment créé
* Choisissez la seule action disponible : **Lancer Campaign**
* Connectez-vous à votre compte Marketo en renseignant les paramètres d’authentification (ID de compte Munchkin, ID client, secret client).
* Modifiez le modèle et associez l’identifiant de campagne du déclencheur au paramètre d’identifiant de campagne « Lancer la campagne »
* Testez l’étape et vérifiez que Marketo Campaign est lancé

### Applet IFTTT qui appelle le Webhook Zapier

Nous commençons avec un scénario simple qui est facile à tester. Nous sélectionnons dans IFTTT un déclencheur Date et heure qui lancera la campagne Marketo toutes les heures. L’action est une requête web qui publie sur l’URL du Webhook Zapier et transmet l’identifiant de campagne intelligente. Assurez-vous que le Zapier Zap et l&#39;Applet IFTTT sont tous deux actifs et vérifiez que tout fonctionne comme prévu.

### Prenons Creative avec IFTTT

IFTTT propose Applet Triggers avec plus de 300 partenaires, donc encore une fois votre portefeuille d&#39;applications et d&#39;appareils ainsi que votre imagination sont les limites ... Prenons un exemple avec le service [Weather Underground](https://www.wunderground.com/) que nous allons utiliser pour lancer notre campagne Marketo sur les alertes météo. Le déclencheur suivant se déclenche lorsqu’une condition Pluie est annoncée. Associez ensuite le déclencheur à l’action Webhook du créateur et, comme précédemment, renseignez les paramètres Webhook Zapier.  Et voilà, vous avez juste besoin maintenant d&#39;une bonne pluie pour venir vérifier que ça marche vraiment.

Nous espérons que vous aurez beaucoup de plaisir à appliquer les concepts fournis dans cet article. Mais plus important encore, nous pensons que cela aidera toute personne souhaitant intégrer Marketo à d’autres systèmes tiers, grâce aux concepts clés de cet article :

* API REST MARKETO
* Webhooks Marketo
* Comment tirer parti d’une plateforme d’intégration web ouverte telle que Zapier en tant que « trappe de diffusion » entre un système tiers et Marketo, pour gérer l’authentification par exemple

Publié le _2017-06-20_ par _Philippe_

## Mises à jour de l’été 2017

Dans la version de l’été 2017, nous proposons quelques améliorations mineures à nos API de programme.

### Parcourir les programmes

Nous ajoutons la possibilité d’obtenir des programmes par période à notre point d’entrée [Obtenir les programmes](https://developer.adobe.com/marketo-apis/api/asset/#operation/browseProgramsUsingGET), via l’ajout des paramètres facultatifs earleastUpdatedAt et latestUpdatedAt. Vous pouvez définir l’un des paramètres, ou les deux, avec la date et l’heure de votre choix pour renvoyer uniquement les programmes qui ont été créés ou mis à jour entre les deux dates et heures. Cela s’avère utile pour récupérer des ensembles nouveaux et mis à jour de dérivés marketing, principalement pour les cas d’utilisation de traduction et de Business Intelligence.

Publié le _1970-01-01_ par _Kenny_

## Étendez la logique commerciale de Marketo avec la fonction cloud Google -

Cet article propose une solution pour étendre Marketo avec certaines fonctionnalités de logique commerciale avec Google Cloud Platform (GCP), en fonction de l’exemple simple suivant : 3 champs personnalisés sur l’enregistrement Lead Marketo :

* **OnLinePreference** : score incrémentiel qui indique une apparence de prospect/client pour les communications en ligne.
* **OfflinePreference** : score incrémentiel qui indique une apparence de prospect/client pour les communications hors ligne.
* **Préférence** : champ calculé par GCP qui affiche « hors ligne » si le score hors ligne est supérieur au score en ligne, et « en ligne » dans l’autre sens

Cette technologie ouvre la voie à une logique commerciale plus avancée et, finalement, à l’appel à des services web externes, transformant et consolidant les résultats dans Marketo.  

### À propos de la plateforme cloud Google et de ses fonctions

[Google Cloud Platform](https://cloud.google.com/) (GCP) est une suite de services de cloud computing qui s’exécute sur la même infrastructure que celle utilisée en interne par Google pour ses produits d’utilisateurs finaux, tels que Google Search et YouTube. Outre un ensemble d&#39;outils de gestion, il fournit une série de services cloud modulaires, notamment l&#39;informatique, le stockage de données, l&#39;analyse de données, le machine learning, le big data et bien plus encore. Nous aurions pu utiliser de nombreux services GCP différents pour nos besoins, tels que Compute Engine, App Engine ou Kubernetes Engine, mais nous avons opté pour le [Cloud Functions](https://cloud.google.com/functions) (toujours en Beta) pour les principaux avantages suivants :

* Le cloud computing sans serveur permet d’activer la logique à la demande en réponse à des événements tels que des appels HTTP.
* Soulage la plupart des problèmes liés à la maintenance et aux déploiements des serveurs.
* Rentable, car vous payez GCP uniquement pour chaque appel de fonction et non pour maintenir un serveur en fonctionnement.
* Mise en œuvre simple et rapide lorsque vous vous concentrez uniquement sur la logique de votre application.
* Mise à l’échelle automatique, prête pour les charges de travail très élevées.

Veuillez consulter le site Web [GCP](https://cloud.google.com/) pour plus d&#39;informations sur cette technologie et son prix. En règle générale, ce tutoriel ne doit entraîner aucun coût important et s’inscrira parfaitement dans le crédit gratuit d’un essai GCP.  

### Préparation de votre environnement Google Cloud

Vous avez besoin d’un compte Google Cloud. Vous pouvez essayer GCP gratuitement avec un crédit qui est plus que suffisant pour exécuter ce tutoriel, il vous suffit de cliquer sur le bouton « Essayer gratuitement » sur le [site Web GCP](https://cloud.google.com/). Suivez toutes les étapes de la section « Avant de commencer » du [tutoriel HTTP](https://cloud.google.com/functions/docs/calling) de Google :

1. Créez un projet Cloud Platform : ACCÉDEZ À LA PAGE GESTION DES RESSOURCES .
1. Activer la facturation de votre projet : [ACTIVER LA FACTURATION](https://cloud.google.com/billing/docs/how-to/modify-project?visit_id=638816637273392093-1926929734&rd=1)
1. Activer l’API Cloud Functions : [ACTIVER L’API](https://accounts.google.com/InteractiveLogin?continue=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&followup=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&osid=1&passive=1209600&service=cloudconsole&ifkv=ASKV5Mh81NGNsqcJqhx7hst0KFnyA0MJ-2zay8ovyluBfpvDoM820nF9Wq_SKbC1m_sjQvvRNoKSuA)
1. [Installation et initialisation de Cloud SDK](https://cloud.google.com/sdk/docs/)
1. Mise à jour et installation des composants Gcloud

   **mise à jour des composants gcloud et&amp;** **installation des composants Gcloud bêta**

1. Préparation de votre environnement pour le développement de Node.js : [ACCÉDEZ AU GUIDE DE CONFIGURATION](https://cloud.google.com/nodejs/docs/setup)

### Implémentation de la fonction cloud scoreCompare

1. Créez un compartiment de stockage dans le cloud pour préparer vos fichiers de fonctions cloud. Vous pouvez le faire à l’aide de la ligne de commande :

   `gsutil mb gs://[YOUR_STAGING_BUCKET_NAME]`

   ou dans l’interface web de Google Cloud, en sélectionnant votre projet et en cliquant sur le menu Stockage :
   * Attribuez un nom unique à votre compartiment de stockage
   * Sélectionner la classe de stockage par défaut
   * Sélectionner l’emplacement régional le mieux adapté

1. Créez un répertoire sur votre système local pour le code de l’application.
1. Créez un fichier &#39;index.js&#39; dans ce répertoire avec le code JavaScript suivant : le code est vraiment simple à comprendre. Il analyse les deux paramètres d’entrée du corps de la requête HTTP dans JSON, effectue le traitement et encode la réponse HTTP dans JSON.

```javascript
 /\*\*
     \* HTTP scoreCompare Cloud Function.
     \*
     \* @param {Object} req Cloud Function request context.
     \* @param {Object} res Cloud Function response context.
     \*/
    exports.scoreCompare = function scoreCompare (req, res) {
     var onlineScore=parseInt(req.body.onlineScore);
     var offlineScore=parseInt(req.body.offlineScore);
     console.log('/scoreCompare: got values onlineScore =' + onlineScore + ', offlineScore =' + offlineScore);
     var result;
     if (onlineScore>offlineScore) {result = 'online';} else {result = 'offline';}
     console.log('/scoreCompare: and result is ' + result);
     res.status(200).json({output: result}).end();
    };
```

Déployez la fonction scoreCompare avec un déclencheur HTTP. Exécutez la commande suivante à partir de votre répertoire :

**les fonctions bêta gcloud déploient [FONCTION] —stage-bucket [YOUR_STAGING_BUCKET_NAME] —trigger-http**

Où [YOUR_STAGING_BUCKET_NAME] est le nom de votre compartiment de stockage dans le cloud intermédiaire. Dans notre exemple :

**fonctions bêta gcloud déploient scoreCompare —stage-bucket mktostorage —trigger-http**

1. Notez l’URL de la fonction cloud (URL httpsTrigger) à partir de la sortie de console, qui ressemble à ceci : `https://[YOUR_REGION]-[YOUR_PROJECT_ID].cloudfunctions.net/[FUNCTION]` où

   * [YOUR_REGION] est la région où votre fonction est déployée. Elle est visible dans votre terminal lorsque votre fonction a terminé son déploiement.
   * [YOUR_PROJECT_ID] est votre ID de projet cloud. Elle est visible dans votre terminal lorsque votre fonction a terminé son déploiement.
   * [FUNCTION] est le nom de votre fonction.

   Dans notre exemple : [**https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare**](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
1. Testez votre fonction avec un outil tel que [Postman](https://www.postman.com/) :
   * Verbe HTTP : POST
   * URL : [https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
   * En-têtes : content-type = application/json
   * Corps : {« onlineScore »:110, « offlineScore »:200}La sortie doit donner : {« output »: « offline »}.

### Appeler la fonction cloud à partir d’un Webhook Marketo

Les trois champs personnalisés suivants doivent être créés sur l’enregistrement Lead dans Marketo :

* **OnlinePreference** : entier
* **OfflinePreference** : Entier
* **Preference** : chaîne

Créez le webhook suivant à partir de l’interface d’administration de Marketo à l’aide de l’URL de votre fonction cloud « scoreCompare » et des jetons du champ personnalisé. Testez le webhook avec une campagne intelligente déclenchée par Marketo.

* **Les webhooks Marketo peuvent uniquement être appelés à partir de campagnes intelligentes déclenchées, et non de campagnes intelligentes par lots.**
* **Si vous n’utilisez pas votre fonction cloud, supprimez-la ou supprimez l’ensemble du projet, afin d’éviter des frais pour votre compte Google Cloud Platform.**

Nous espérons que ce tutoriel a été utile et qu’il vous fera réfléchir à des scénarios plus avancés impliquant un traitement complexe et des services tiers. Un bon exemple serait d’utiliser Google Cloud AI, les services de machine learning de Google. Vous pouvez, par exemple, analyser du texte libre d’un formulaire Marketo et demander à l’API de langage naturel de Google de révéler la structure et la signification du texte, puis enregistrer cette analyse dans Marketo ; cela vous permet simplement de recueillir des idées.

Publié le _2017-11-21_ par _Philippe_

## Mises à jour de l’automne 2017

Dans la version de l’automne 2017, nous avons apporté plusieurs améliorations à nos API de ressources. Consultez la liste complète des mises à jour ci-dessous.

### Parcourir les programmes par période

Nous avons ajouté la possibilité d’obtenir des programmes par période à notre point d’entrée [Obtenir les programmes](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST). Pour ce faire, utilisez les paramètres `earliestUpdatedAt` et `latestUpdatedAt` . Vous pouvez définir l’un des paramètres, ou les deux, avec la date et l’heure de votre choix pour renvoyer uniquement les programmes qui ont été créés ou mis à jour entre les deux dates et heures.

### Aperçu de l&#39;e-mail

Vous pouvez désormais prévisualiser un e-mail à l’aide du point d’entrée [Obtenir le contenu complet de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailFullContentUsingGET), qui renvoie la version HTML sérialisée d’un e-mail. Tous les jetons, fragments de code, contenu dynamique et composants incorporés sont entièrement rendus. Un paramètre **leadId** facultatif peut être transmis pour emprunter l’identité d’un prospect donné.

### Remplacer HTML d’Email 2.0

Nous avons ajouté le point d’entrée [Mettre à jour le contenu complet de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailFullContentUsingPOST) pour vous permettre de remplacer des blocs de contenu d’e-mail HTML. Si vous modifiez le code HTML d’un email Marketo à l’aide de l’éditeur d’email Marketo 2.0, la relation entre l’email et son modèle est rompue. Pour en savoir plus, cliquez [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html). À l’aide de ce point d’entrée, vous pouvez mettre à jour par programmation le contenu HTML d’un e-mail dont la relation a été rompue. En outre, nous avons modifié tous les autres points d’entrée liés au cycle de vie des e-mails pour les rendre compatibles avec les e-mails dans lesquels la relation a été rompue :

* Approuver le brouillon d’e-mail
* Désapprouver l’e-mail
* Supprimer l&#39;e-mail
* Ignorer le brouillon d&#39;e-mail
* Cloner l&#39;e-mail
* Mettre à jour les métadonnées de courrier électronique

### Autres améliorations

* La durée maximale des filtres de période a été augmentée à 31 jours. Cela concerne les filtres [Extraction de leads en bloc](/help/rest-api/bulk-lead-extract.md) (createdAd ou updatedAt) et [Filtre d’extraction d’activités en bloc](/help/rest-api/bulk-activity-extract.md) (createdAt).

Publié le _2017-12-15_ par _David_

## TEST - Lien vidéo externe sur la communauté

[Tutoriel Vidéo Du Power Pack Sur La Délivrabilité Des E-Mails](https://nation.marketo.com:443/t5/product-space-archive-videos/email-deliverability-power-pack-tutorial-video/m-p/283550)  

Publié le _1970-01-01_ par _David_

## Mises À Jour De L&#39;Hiver 2018

Dans la version de l’hiver 2018, nous publions quelques améliorations apportées à nos API. Consultez la liste complète des mises à jour ci-dessous.

### Activer/désactiver les campagnes de déclenchement

Nous avons ajouté la possibilité d’activer et de désactiver les campagnes de déclenchement, ce qui peut simplifier le processus d’automatisation des modèles de vos programmes. Pour ce faire, appelez deux points d’entrée nouvellement ajoutés : [&#x200B; Activer la campagne intelligente &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#operation/activateSmartCampaignUsingPOST) et [&#x200B; Désactiver la campagne intelligente &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#operation/deactivateSmartCampaignUsingPOST). Voir la section Déclencheur dans la documentation [Campagnes](/help/rest-api/assets.md) pour plus d’informations.

### Obtenir le programme par nom

Ajout de deux paramètres au point d’entrée [Obtenir le programme par nom](https://developer.adobe.com/marketo-apis/api/asset/#operation/getProgramByNameUsingGET) pour faciliter la récupération des coûts du programme et des balises du programme. Voir les paramètres **includeCost** et **includeTags** dans la documentation [Programmes](/help/rest-api/assets.md) pour plus d’informations.

### Autres améliorations

L’API Bulk Extract est désormais « compatible avec l’espace de travail ». Lorsque vous créez un utilisateur API uniquement pour un [service personnalisé](/help/rest-api/custom-services.md), vous devez sélectionner un rôle d’utilisateur avec un accès API pour un ou plusieurs espaces de travail. Auparavant, le service personnalisé se voyait accorder l’accès à tous les espaces de travail. Désormais, le service personnalisé n’a accès qu’aux espaces de travail sélectionnés lors de la création de l’utilisateur API uniquement.

Publié le _2018-03-02_ par _David_

## Mises à jour du printemps 2018

Dans la version du printemps 2018, nous publions de nouvelles API REST et des améliorations de la confidentialité du tracking web. Consultez la liste complète des mises à jour ci-dessous.

API REST

### Liste statique CRUD

Permet aux utilisateurs de créer, lire, mettre à jour et supprimer à distance des enregistrements de listes statiques. Permet la gestion de l’ensemble du cycle de vie d’une liste statique par le biais des API REST, y compris le remplissage et la conservation des adhésions. Vous trouverez des détails [ici](/help/rest-api/static-lists.md).

### Métadonnées d’activité personnalisées

Permet aux utilisateurs de créer à distance des enregistrements d’activité personnalisés. Permet la gestion de l’ensemble du cycle de vie des activités personnalisées par le biais des API REST, y compris la manipulation des types et des attributs de type. Vous trouverez des détails [ici](/help/rest-api/activities.md).

### Métadonnées de liste dynamique

Permet aux utilisateurs de lire, cloner et supprimer à distance des enregistrements de liste dynamique. Permet la gestion des listes dynamiques par le biais des API REST. Vous trouverez des détails [ici](/help/rest-api/smart-lists.md).

### Confidentialité quant au suivi Web

Le code de tracking web Munchkin JavaScript a été amélioré afin d’inclure les améliorations suivantes liées à la confidentialité :

* Exclusion : possibilité de permettre aux visiteurs de se désinscrire définitivement du tracking web. Vous trouverez des détails [ici](/help/javascript-api/lead-tracking.md).
* Anonymisation des adresses IP : respectez les réglementations locales et internationales en matière de confidentialité en permettant d’anonymiser les adresses IP des visiteurs et visiteuses web. Voir le paramètre de configuration **anonymizeIP** [ici](/help/javascript-api/configuration.md).

### Autres améliorations

* Le point d’entrée [Get Folders](https://developer.adobe.com/marketo-apis/api/asset/#operation/getFolderUsingGET) renvoie désormais tous les dossiers lorsqu’un parent non racine et maxDepth=1 sont spécifiés.
* Le point d’entrée [Get Landing Page by Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getLandingPageByIdUsingGET) renvoie désormais les attributs d’URL avec le protocole dans tous les cas (http:// ou https://).

Publié le _2018-06-29_ par _David_

## Mises À Jour De L’Été 2018

La version d’été 2018 est principalement une version de maintenance composée d’améliorations mineures et de résolutions des défauts. Consultez la liste complète des mises à jour ci-dessous.

### API REST

* Ajout de la prise en charge des champs de disposition des e-mails qui ont été inutilement omis à l’origine. Ces champs seront désormais disponibles pour la lecture et l’écriture sur REST, selon les besoins.
   * Sur liste noire
   * Marketing interrompu
   * E-mail interrompu
   * Urgence relative
* Le point d’entrée [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/lead-database-endpoint-reference/#/Leads/getLeadsByFilterUsingGET) prend désormais en charge leadPartionId en tant que filterType.

### Résolutions des défauts

**Point d’entrée REST**

**Description**

[Approuver le programme](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveProgramUsingPOST)

Si vous aviez défini Bloquer les e-mails non opérationnels sur false lors de la création du programme, un appel à Approuver le programme serait réinitialisé sur true.

[Extraction En Masse](/help/rest-api/bulk-extract.md)

Certains caractères Unicode ont été corrompus dans le fichier de sortie d&#39;extraction.

[Cloner le programme](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

* Si vous avez cloné le programme de messagerie, la logique du filtre Liste dynamique a été réinitialisée sur « Tous » dans le programme obtenu, quel que soit le paramètre initial.
* Si vous avez essayé de cloner un programme qui contenait une liste statique (qui a été supprimée), vous avez reçu l’erreur 709 « Les ressources suivantes ne sont pas prises en charge :List ».
* Si vous avez essayé de cloner un programme sur plusieurs espaces de travail, vous avez reçu l’erreur 611 « Impossible de cloner le programme ».

[Obtenir une liste statique par ID](https://developer.adobe.com/marketo-apis/api/asset/#operation/getStaticListByIdUsingGET)

Si votre service personnalisé disposait de l’autorisation de rôle Ressource en lecture seule, vous recevriez une erreur 603 « Accès refusé ».

[Intégrer le lead à Marketo](https://developer.adobe.com/marketo-apis/api/mapi/#operation/pushToMarketoUsingPOST)

Si l’attribut **cookies** a été spécifié dans un objet de lead de tableau **input**, l’activité anonyme précédente n’était pas associée au lead nouvellement créé.

[Planifier la campagne](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST)

Si vous avez spécifié une date **runAt** très éloignée dans le futur, la campagne n’a jamais été exécutée et **success=true** a été renvoyée. Désormais, si la date **runAt** remonte à plus de 2 ans, une erreur est renvoyée [1042](/help/rest-api/error-codes.md).

[Leads de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST)

Si vous avez spécifié `action=createDuplicate` et un paramètre `externalCompanyId` (pour associer le nouveau prospect à une entreprise existante), le prospect était associé à une entreprise vide (au lieu de l’entreprise spécifiée).

#### Divers

* Suppression de la valeur de modification des données suivante de toutes les réponses de point d’entrée : mktoClientReqId. Il était réservé à un usage interne uniquement.
* Ajout de la fonctionnalité de recherche d’erreurs. Saisissez un code d’erreur de l’API REST dans la zone de recherche et sélectionnez-en un dans la liste de saisie semi-automatique pour accéder à la description de l’erreur.
* Ajout de la page [Référence du point d’entrée](/help/rest-api/endpoint-reference.md). Il s’agit d’une liste triable de tous les points d’entrée de l’API REST au même endroit. Vous pouvez également utiliser cette page pour générer l’ensemble minimal d’autorisations nécessaires à votre application. Pratique lors de la création d’un service personnalisé.

Publié le _2018-10-12_ par _David_

## Mises À Jour De L’Automne 2018

La version de l’automne 2019 est principalement une version de maintenance composée d’améliorations mineures et de résolutions des défauts. Consultez la liste complète des mises à jour ci-dessous.

### Améliorations

* Ajout de la prise en charge de [Champs CC d’e-mail](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/email-cc) pour [API Asset](/help/rest-api/assets.md). Les paramètres du champ CC se propagent comme prévu lors des opérations d’approbation/clonage (approbation du brouillon d’e-mail ou de modèle d’e-mail, e-mail ou clonage de programme). Tous les points d’entrée liés aux e-mails renvoient désormais les valeurs des champs CC dans la propriété **ccFields**. Faites défiler la page vers le bas dans la réponse ci-dessous pour afficher un exemple. Cette modification affecte les points d’entrée suivants : [Get Email by ID](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByIdUsingGET), [Get Email by Name](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByNameUsingGET), [Get Emails](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailUsingGET), [Approve Email Draft](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [Approve Email Template Draft](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST_1), [Clone Email](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Clone Program.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

```json
{
    "success": true,
    "errors": [],
    "requestId": "cc96#166e836348b",
    "warnings": [],
    "result": [
        {
            "id": 2061,
            "name": "Test Email",
            "description": "This is a test",
            "createdAt": "2018-11-06T05:27:10Z+0000",
            "updatedAt": "2018-11-06T08:33:16Z+0000",
            "url": "<https://app-sjqe.marketo.com/#EM2061A1LA1>",
            "subject": {
                "type": "Text",
                "value": "This is a test with CC Fields"
            },
            "fromName": {
                "type": "Text",
                "value": "Tommy Tester"
            },
            "fromEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "replyEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "folder": {
                "type": "Program",
                "value": 7494,
                "folderName": "Initiative"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "approved",
            "template": 1218,
            "workspace": "Default",
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
                {
                    "attributeId": "855",
                    "objectName": "lead",
                    "displayName": "emailcc",
                    "apiName": "emailcc"
                },
                {
                    "attributeId": "857",
                    "objectName": "lead",
                    "displayName": "leadDetails",
                    "apiName": "leadDetails"
                },
                {
                    "attributeId": "859",
                    "objectName": "company",
                    "displayName": "headquarters",
                    "apiName": "headquarters"
                }
            ]
        }
    ]
}
```

### Résolutions des défauts

* Ajustement de la prise en charge [domaines de branding multiples](https://experienceleague.adobe.com/fr/docs/marketo/using/home) de [API de ressources](/help/rest-api/assets.md). Auparavant, les paramètres de domaines de branding multiples n’étaient pas propagés lors de l’approbation d’un brouillon d’e-mail, du clonage d’un e-mail ou du clonage d’un programme. Ce problème a été corrigé. Cette modification affecte les points d’entrée suivants : [Approuver le brouillon d’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST), [Cloner l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [Cloner le programme.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)
* Ajout du paramètre de configuration [apiOnly](/help/javascript-api/configuration.md). Par défaut, les pages web qui contiennent la balise Munchkin déclenchent un événement « Page web des visites » lorsque la page web est chargée dans le navigateur. Dans certains cas, cela n’est pas souhaitable. Par exemple, les applications web monopages qui nécessitent un contrôle total du moment où cet événement est déclenché. Pour prendre en charge ce cas d’utilisation, nous avons ajouté un nouveau paramètre de configuration **apiOnly**. Lorsque la valeur est définie sur true, la balise Munchkin ne génère pas d’activité « Page web de visites » lors du chargement de la page.
* Ajout du paramètre de configuration [domainSelectorV2](/help/javascript-api/configuration.md). Par défaut, la balise Munchkin ne gère pas correctement les pages web hébergées sur des sites comportant des domaines de niveau supérieur de code pays [code pays](https://en.wikipedia.org/wiki/Country_code_top-level_domain) à deux lettres (par exemple : .io, .co, .ly). En conséquence, l’attribut de domaine du cookie Munchkin est défini de manière incorrecte. Pour obtenir une meilleure expérience prête à l’emploi, nous avons ajouté un nouveau paramètre de configuration **domainSelectorV2**. Lorsque la valeur est définie sur « true », un algorithme amélioré est utilisé pour définir automatiquement l’attribut de domaine du cookie Munchkin.
* Domaine de cookie [Opt-out](/help/javascript-api/lead-tracking.md) ajusté. Dans certains cas, l’attribut de domaine du cookie d’exclusion Munchkin (mkto_opt_out) a été défini de manière incorrecte. Le cookie d’exclusion Munchkin utilise désormais la même logique que le cookie Munchkin (_mkto_trk) pour déterminer l’attribut du cookie de domaine, y compris le respect du paramètre de configuration **domainLevel**.
* Les développeurs d’applications Android peuvent désormais utiliser directement Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) avec ce SDK. Vous trouverez des détails [ici](/help/mobile/installation.md).

Publié le _2018-12-07_ par _David_

## Mises à jour du printemps 2019

La version du printemps 2019 est principalement une version de maintenance composée d’améliorations mineures et de résolutions des défauts. Consultez la liste complète des mises à jour ci-dessous.

Publié le _2019-03-19_ par _David_

## Mises À Jour De Juin 2019

La version de juin 2019 est principalement une version de maintenance composée d’améliorations mineures et de résolutions des défauts. Consultez la liste complète des mises à jour ci-dessous.

### API REST

1. Ajout d’une somme de contrôle aux points d’entrée du statut d’exportation en bloc. Vous pouvez comparer la somme de contrôle avec un hachage du fichier récupéré pour vérifier l’intégrité du fichier récupéré. La somme de contrôle est un hachage SHA-256 du fichier exporté et est stockée dans l’attribut fileCheckSum lorsqu’une tâche d’exportation est terminée.

Les points d’entrée suivants renvoient la somme de contrôle : [Obtenir le statut de la tâche d’exportation principale](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Obtenir les tâches d’exportation principale](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET), [Obtenir le statut de la tâche d’exportation d’activité](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [Obtenir les tâches d’exportation d’activité](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportActivitiesUsingGET)

#### Résolutions des défauts

1. Correction d’un problème lié à l’[importation d’objets personnalisés en bloc](/help/rest-api/bulk-custom-object-import.md) lors de l’importation de nombres décimaux dans des champs entiers. Avant la correction, le nombre décimal était converti en entier en attribuant la partie entière du nombre et en ignorant la partie fractionnaire (par exemple, 5,432 a été converti en 5). Désormais, une erreur « Type de données non valide dans le champ ID Source » est générée pour chaque ligne contenant une incohérence des données.
1. Correction d’un problème en raison duquel un programme de messagerie créé à l’aide du point d’entrée [Programme de clonage](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) ne respectait pas les paramètres des limites de communication dans certains cas.
1. Correction d’un problème lié au point d’entrée [Approuver le brouillon de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) où il renvoyait la mention 611. « Erreur système » lorsque la page de destination contenait le formulaire de désabonnement des e-mails.
1. Correction d’un problème lié au point d’entrée [Approuver le brouillon de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) où il renvoyait la mention 611. « Erreur système » lorsque la page de destination a été clonée à l’aide du point d’entrée [&#x200B; Cloner la page de destination &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneLandingPageUsingPOST).

#### Obsolescence

1. Dans le cadre de l’obsolescence de la version [Email Editor 1.0](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666), la version 1.0 d’Email Assets deviendra en lecture seule fin 2019. Toutes les Assets d’e-mail 1.0 doivent être converties en 2.0 comme descriDiscard Email Draft, bed [here](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666). Pour aider les développeurs à se préparer à cet événement, nous avons ajouté des avertissements à tous les points d’entrée liés aux e-mails qui tentent de modifier 1.0 Email Assets. Voici un exemple de réponse contenant l’avertissement :

`{` `"success": true,` `"errors": [],` `"requestId": "15c57#16b338d6e75",` `"warnings": [` `"This is a v1 email asset. API support for modifying v1 emails is being dropped, and this operation will not work on v1 emails in the future. To avoid service interruptions, upgrade this and related assets by editing them in the User Interface."` `],` `"result": [` `"{\"service\":\"sendTestEmail\",\"result\":true}"` `]` `}`

Les points d’entrée suivants liés aux e-mails renvoient l’avertissement :

* Mettre à jour le contenu complet de l’e-mail
* Mettre à jour le contenu de l’e-mail
* Envoyer un échantillon de l&#39;e-mail
* Désapprouver l’e-mail
* Cloner le programme
* Cloner l&#39;e-mail
* Approuver le brouillon d’e-mail
* Mettre à jour la section de contenu dynamique d’e-mail
* Mettre à jour les métadonnées de courrier électronique
* Approuver le programme

Script D’E-Mail (Apache Velocity)

#### Obsolescence

1. Un sous-ensemble de fonctionnalités de script Velocity a été désactivé pour des raisons de sécurité. Cela inclut les méthodes et variables suivantes : getClass(), $class, $context, $text. Vous trouverez plus d’informations [ici](https://nation.marketo.com:443/t5/knowledgebase/unsupported-velocity-tools-disabled-in-june-2019-release/ta-p/251177).

Publié le _2019-06-14_ par _David_

## Mises à jour d’août 2019

En août 2019, nous publierons de nouvelles API REST, améliorerons les API existantes et résoudrons les défauts. Consultez la liste complète des mises à jour ci-dessous.

### Améliorations

1. Amélioration des fonctionnalités de cycle de vie de Campaign intelligente. Ajout de nouveaux points d’entrée pour vous permettre d’effectuer les opérations suivantes sur les campagnes intelligentes : obtenir par nom, créer, mettre à jour, cloner et supprimer. Vous trouverez des informations complètes [ici](/help/rest-api/smart-campaigns.md).
1. Amélioration des points d’entrée de liste dynamique pour améliorer les fonctionnalités de requête.
   1. Le point d’entrée [Get Smart List by Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByIdUsingGET) renvoie désormais des descriptions de règle de liste dynamique (déclencheurs et filtres) lorsque vous transmettez le paramètre booléen **includeRules**.
   1. Le point d’entrée [Obtenir les listes dynamiques](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListsUsingGET) vous permet désormais de filtrer les résultats par période lorsque vous transmettez les paramètres de date et d’heure **earlyUpdatedAt** et **latestUpdatedAt**. En outre, ce point d’entrée renvoie désormais des listes intelligentes qui sont membres de campagnes et de programmes de messagerie.
1. Ajout de points d’entrée pour l’extraction des définitions de liste dynamique.
   1. Le point d’entrée Get [Smart List by Smart Campaign Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListBySmartCampaignIdUsingGET) renvoie l’enregistrement de liste dynamique pour un identifiant de campagne dynamique donné.
   1. Le point d’entrée Get [Smart List by Program Id](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByProgramIdUsingGET) renvoie l’enregistrement de liste dynamique pour un ID de programme donné.
1. Amélioration du point d’entrée [Mettre à jour le contenu de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/updateEmailContentUsingPOST) afin de permettre la mise à jour des champs d’en-tête des e-mails pour les e-mails rompus par leur modèle (objet, nom, e-mail, répondre à). Rompu à partir du modèle est décrit [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html).

### Résolutions des défauts

1. Correction d’un problème en raison duquel l’appel de [Supprimer la page de destination](https://developer.adobe.com/marketo-apis/api/asset/#operation/deleteLandingPageByIdUsingPOST) sur une page de destination approuvée supprimait la page de destination. Elle renvoie désormais correctement une erreur « 709, La page de destination approuvée ne peut pas être supprimée ». [LM-127271]
1. Correction d’un problème lié au point d’entrée [Envoyer un exemple d’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/sendSampleEmailUsingPOST) qui renvoyait 611. « Erreur système » lorsque l’e-mail a été rompu dans son modèle. [LM-127288]
1. Correction d’un problème lié au point d’entrée [Supprimer le programme](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) qui, dans certains cas, supprimait un programme en cours d’utilisation au lieu d’émettre un programme « 709, Impossible de supprimer ». L’erreur « Les ressources sont utilisées ailleurs ou ne peuvent pas être supprimées ». [LM-125431]

### Obsolescence

1. La prise en charge de l’API pour l’éditeur d’e-mail 1.0 devrait être abandonnée en janvier 2020. N’oubliez pas de convertir vos ressources en 2.0 avant cette date. Les tentatives d’écriture ou de clonage de ressources d’e-mail 1.0 après janvier entraînent des erreurs au lieu d’avertissements. En savoir plus sur les API de messagerie [ici](https://nation.marketo.com:443/t5/knowledgebase/email-2-0-and-email-api-faq-s/ta-p/251423).
1. Pour nous aligner sur les normes de sécurité de classe mondiale d’Adobe, nous allons abandonner la prise en charge de TLS (Transport Layer Security) 1.0 et 1.1 à compter du 13 décembre 2019. Les systèmes s’intégrant à Marketo et non conformes au protocole 1.2 peuvent perdre l’accès aux services Marketo Engage. Pour conserver votre accès à Marketo Engage, assurez-vous que tous les systèmes clients sont conformes au protocole TLS 1.2 avant le 13 décembre 2019. Vous trouverez de plus amples informations [ici](https://nation.marketo.com:443/t5/knowledgebase/tls-1-0-1-1-deprecation-faq/ta-p/249085).

1. Tout le contenu associé aux campagnes intelligentes réside désormais dans l’élément de menu [Campagnes intelligentes](/help/rest-api/smart-campaigns.md) (sous API REST > Assets).

Publié le _2019-08-16_ par _David_

## Mises À Jour De Janvier 2020

En janvier 2020, nous publierons de nouvelles API REST, améliorerons les API existantes et résoudrons les défauts. Consultez la liste complète des mises à jour ci-dessous.

* Ajout de la possibilité de créer par programmation des définitions de schéma d’objet personnalisé. Cela vous permet de définir un objet personnalisé une seule fois et de le configurer sur autant d’instances que nécessaire. Vous pouvez ainsi permettre aux utilisateurs d’utiliser efficacement les modèles de sandbox et de centre d’excellence. Cela permet également aux FAI de simplifier le processus d’intégration des clients. Vous avez besoin d’un type d’abonnement approprié pour accéder à l’API de métadonnées d’objet personnalisé.
* Ajout de la possibilité d’importer et d’exporter des membres du programme en bloc. Ce nouveau jeu de points d’entrée suit le modèle d’API REST Marketo existant pour la création de tâches de traitement en bloc asynchrones. Les enregistrements de membre de programme peuvent contenir des champs personnalisés de membre de programme et/ou des champs de lead.
* Ajout du point d’entrée Obtenir les champs de membre de programme disponibles pour prendre en charge l’utilisation des champs personnalisés de membre de programme en tant que champs de formulaire. Cette opération renvoie une liste de tous les champs personnalisés des membres du programme qui peuvent être utilisés dans un formulaire Marketo.
* Ajout de la mention [Obtenir le modèle d’e-mail utilisé par un point d’entrée &#x200B;](/help/rest-api/email-templates.md) qui renvoie une liste de ressources d’e-mail qui dépendent d’un modèle d’e-mail donné. Vous pouvez ainsi comprendre rapidement l’impact d’une modification potentielle de modèle d’e-mail et résoudre plus facilement ces dépendances.

Publié le _2020-01-17_ par _David_

## Comment récupérer chaque objet personnalisé

On nous demande souvent comment utiliser l’API de Marketo pour obtenir une liste de tous les [objets personnalisés](https://experienceleague.adobe.com/fr/docs/marketo/using/home) (CO). La recherche de CO exige plus que son nom : une certaine connaissance _a priori_ de chaque CO est également nécessaire. Les méthodes permettant d’obtenir ces connaissances peuvent ne pas être évidentes, car l’API ne fournit aucune méthode pour les interroger directement. Comme pour de nombreux objectifs dans Marketo Engage, les listes dynamiques fournissent une réponse pour les CO liés aux personnes (prospects). Les listes dynamiques fonctionnent différemment avec les sociétés et vous obtiendrez une liste de toutes les personnes dont les sociétés sont liées au type d’objet du filtre. Vous devrez donc peut-être dédupliquer les sociétés en fonction de vos objectifs. Chaque fois qu’un nouvel objet personnalisé est approuvé, un filtre associé est créé. Il sera nommé dans le format « **Has CO NAME** ». Dans l’exemple ci-dessous, le nom de l’objet personnalisé est « **Conference Track Subscription»** et son filtre est nommé « **Has Conference Track Subscription** ». Une fois la liste dynamique créée, vous pouvez récupérer les informations nécessaires à l’interrogation des CO associés à l’aide du point d’entrée [objets personnalisés](/help/rest-api/custom-objects.md). Exportez la liste en vous assurant que le champ lié est inclus (identifiant ou adresse e-mail). Vous pouvez exporter à l’aide de l’API [Bulk Lead Extract](/help/rest-api/bulk-lead-extract.md) en filtrant par le filtre **smartListName** ou **smartListId** ou [exporter à partir de l’interface utilisateur](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/managing-people-in-smart-lists/export-people-to-excel-from-a-list-or-smart-list). Vous utiliserez chaque valeur de champ lié pour interroger individuellement les objets personnalisés associés à l’étape suivante. Le nom de l’objet personnalisé est **« Conference Track Subscription »** dans cet exemple, et son nom d’API est **conferenceTrackSubscription_c**. Le nom de l’API apparaît à la fois dans l’interface utilisateur sous la forme « **nom de l’API** » et via l’API sous la forme « **nom** ».  Admin | Marketo Custom Objects[/caption ] Et voici un fragment renvoyé par le point d’entrée [List Custom Objects API](https://developer.adobe.com/marketo-apis/api/mapi/#operation/listCustomObjectsUsingGET) :

```json
{
    "name": "conferenceTrackSubscription_c",
    "displayName": "Conference Track Subscription",
    "description": "Track subscription for conference attendees",
    "createdAt": "2020-01-13T19:50:06Z",
    "updatedAt": "2020-01-13T19:50:06Z",
    "idField": "marketoGUID",
    "dedupeFields": [
        "subscriptionID"
    ],
    "searchableFields": [
        [
            "subscriptionID"
        ],
        [
            "marketoGUID"
        ],
        [
            "leadID"
        ]
    ],
    "relationships": [
        {
            "field": "leadID",
            "type": "child",
            "relatedTo": {
                "name": "Lead",
                "field": "Id"
            }
        }
    ]
}
```

Pour récupérer les objets personnalisés associés un à un (1:1) ou un à plusieurs (1:N) aux personnes de votre liste dynamique, effectuez une requête comme celle-ci :

`GET /rest/v1/customobjects/conferenceTrackSubscription_c.json?filterType=leadID&filterValues=1000302,1000303,1000304,1000306,1000307`

Dans cet exemple, cet objet personnalisé est lié aux personnes par le champ **leadID**, de sorte que le type de filtre est « **leadID** ». Le paramètre des valeurs de filtre est une liste séparée par des virgules des identifiants extraits de l’exportation de la liste dynamique. La requête peut inclure autant de valeurs de filtre que vous le pouvez dans un seul URI de requête : jusqu’à 8 000 caractères. Les requêtes dépassant cette longueur renvoient un code d’erreur de niveau HTTP 414. La réponse peut être renvoyée dans plusieurs blocs. Si tel est le cas, **moreResult** sera **true** et un **nextPageToken** sera inclus. Vous devrez ensuite [parcourir](/help/rest-api/paging-tokens.md) les résultats jusqu’à ce que **moreResult** soit **false**. Voici une partie du résultat de la requête API ci-dessus :

```json
"result": [
    {
        "seq": 0,
        "marketoGUID": "d6b3ed3d-4eb8-4066-a9cd-184c8d385cfe",
        "leadID": "1000302",
        "subscriptionID": "4ad59184-6bf1-4eeb-a583-d82aeee68210",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 1,
        "marketoGUID": "e5e0aba4-f27f-494d-93ed-9cb580989bf3",
        "leadID": "1000303",
        "subscriptionID": "fc5596d5-6fa2-4848-b4a2-89d96e245c59",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 2,
        "marketoGUID": "e65007cd-86b1-4c17-8d55-057c96e1788a",
        "leadID": "1000304",
        "subscriptionID": "7e54b8a0-2170-4d81-a809-4eac349508d0",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 3,
        "marketoGUID": "39d956b2-85e2-4c24-94e7-e9fa5a09d3d0",
        "leadID": "1000306",
        "subscriptionID": "644c8e5b-fc0c-4d4a-80f8-358a65ce0a68",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 4,
        "marketoGUID": "a2151350-50c8-437f-bc71-7a054bb601f0",
        "leadID": "1000307",
        "subscriptionID": "bf14218c-ae6a-42b3-a14e-f7182903cbcd",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    }
```

Vous disposez désormais des valeurs de chaque objet personnalisé directement associé aux personnes de votre liste dynamique. Outre la récupération des valeurs, vous pouvez utiliser le **marketoGUID** pour [mettre à jour](/help/rest-api/custom-objects.md) ou [supprimer](/help/rest-api/custom-objects.md) ces objets. Pour les objets personnalisés associés à des Personnes dans une relation multiple-à-multiple (N:N), la technique ci-dessus renvoie l&#39;objet de premier niveau qui est l&#39;objet intermédiaire reliant chaque Personne à plusieurs CO de second niveau.

Pour récupérer ces CO de 2ème niveau, lancez un nouvel ensemble de requêtes pour le type CO de 2ème niveau en filtrant sur le champ de lien et les valeurs extraites de l&#39;objet intermédiaire de 1er niveau. Par exemple, l’objet « **Conference Track Subscription »** ci-dessus pourrait avoir un autre niveau d’objets représentant les sessions appelées **« Session »** qui serait probablement lié par l’**subscriptionID**. La requête pour récupérer les sessions liées aux abonnements du suivi des conférences ci-dessus ressemblerait alors à ceci :

`GET /rest/v1/customobjects/session_c.json?filterType=subscriptionID&filterValues=4ad59184-6bf1-4eeb-a583-d82aeee68210,e5e0aba4-f27f-494d-93ed-9cb580989bf3,e65007cd-86b1-4c17-8d55-057c96e1788a,39d956b2-85e2-4c24-94e7-e9fa5a09d3d0,bf14218c-ae6a-42b3-a14e-f7182903cbcd`

_Note de bas de page_ _1) Les types de filtre **smartListName**&#x200B;et **smartListId**&#x200B;ne sont pas disponibles pour certains abonnements. Si vous n’êtes pas disponible pour votre abonnement, vous recevez une erreur lors de l’appel du point d’entrée Créer une tâche d’exportation de lead (**« 1035, Type de filtre non pris en charge pour l’abonnement cible »**). Les clients peuvent contacter le support Marketo pour que cette fonctionnalité soit activée dans leur abonnement._

Publié le _2020-01-14_ par _Tony_

## Comment récupérer chaque personne (lead)

Nous recevons de nombreuses demandes sur les processus requis pour récupérer chaque personne (prospect) d’une instance Marketo Engage. Nous avons fourni de nombreuses réponses utiles, mais aucune n&#39;a été aussi complète que celle-ci. J’ai identifié quelques concepts clés nécessaires pour extraire chaque prospect à l’aide de l’API d’extraction en bloc Marketo. Tous les autres détails peuvent être appris à partir du code de démonstration que j’ai créé pour l’accompagner. Après avoir lu cet article et exploré le code de démonstration, vous disposerez de toutes les informations dont vous avez besoin pour récupérer chaque prospect d’une instance Marketo Engage.

### Vue d’ensemble

La technique principale utilise l’API [Bulk Lead Extract](/help/rest-api/bulk-lead-extract.md). Vous devriez être en mesure de créer une tâche d’exportation de leads en bloc sans filtre, mais vous ne pouvez pas : **l’API nécessite un filtre**. Les filtres disponibles sont **createdAt**, **staticListName**, **staticListId** **updatedAt**, **smartListName** et **smartListId**. Le filtrage par liste dynamique sans filtre semble également attrayant. Faites un essai et vous constaterez que le système est suffisamment intelligent pour traiter de la même manière une liste dynamique sans filtre : l’API requiert également un filtre ici. Comme nous avons besoin d’un filtre, le filtre fiable et canonique à utiliser est **createdAt**. Ce type de filtre permet des périodes allant jusqu’à 31 jours. Nous devons donc exécuter plusieurs tâches et combiner les résultats. Nous commençons par trouver la date de création la plus ancienne possible pour un prospect dans l’instance cible. À partir de cette date la plus ancienne possible, nous créons des emplois sur 31 jours moins une seconde (nous y reviendrons plus tard). Après avoir créé chaque traitement, nous le mettrons en file d’attente et attendrons qu’il soit terminé. Ensuite, nous téléchargerons le fichier obtenu et vérifierons son intégrité à l’aide d’une somme de contrôle. Enfin, dédupliquez les prospects par ID, puis écrivez les prospects uniques dans un fichier CSV de sortie.

### Trouver Votre Ancien Prospect

J’utilise un petit « truc » pour obtenir la date la plus ancienne possible pour un prospect dans l’instance cible. Il n’existe aucun point d’entrée d’API dédié à cette tâche, nous avons donc besoin d’un peu de créativité. Ce que je fais, c’est interroger tous les dossiers avec un **maxDepth = 1** qui nous donnera une liste de tous les dossiers de niveau supérieur dans l’instance. Ensuite, je collecte les dates **createdAt**, les analyse et trouve la date la plus ancienne. Cette méthode fonctionne car certains dossiers de niveau supérieur par défaut sont créés avec l’instance et aucun prospect n’a pu être créé avant cette date.

### Sélectionner les champs obligatoires

Vous devez décider quels champs vous devez extraire. Recherchez les champs disponibles pour votre instance cible à l’aide du point d’entrée [Décrire le lead 2](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). La réponse à cette demande comprendra une liste nommée « champs ». Voici un extrait d&#39;un exemple de réponse :

```json
  "fields": [
      {
          "name": "AccountSource",
          "displayName": "Account Source",
          "dataType": "string",
          "length": 40,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "acquisitionProgramId",
          "displayName": "Acquisition Program",
          "dataType": "reference",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Active_c",
          "displayName": "Active",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "address",
          "displayName": "Address",
          "dataType": "text",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Address_lead",
          "displayName": "Address (L)",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "annualRevenue",
          "displayName": "Annual Revenue",
          "dataType": "currency",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "anonymousIP",
          "displayName": "Anonymous IP",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      }, ...
```

Ce point d’entrée renvoie une liste exhaustive comprenant des champs standard et personnalisés. Plus vous demandez de champs, plus la durée de votre tâche d’exportation est longue et plus le fichier obtenu est volumineux. En règle générale, vous devez choisir uniquement les champs dont vous avez besoin. Rien ne vous empêche de demander tous les champs disponibles. C’est donc ce que je démolis. L’identifiant de champ requis lors de la création d’une tâche d’exportation est la valeur **name**. J’extrairai les valeurs de nom dans une liste de tous les noms de champ. Ensuite, je l’utiliserai pour demander chaque champ disponible lorsque je créerai chaque tâche d’exportation.

### Exporter Les Périodes De Tâche : 31 Jours Chacune

Chaque tâche d’exportation peut durer jusqu’à 31 jours. L’instance de démonstration que j’utilise a été créée en août 2016. Je dois donc créer un peu plus de 40 emplois aujourd’hui. Il s’agit du nombre de jours écoulés depuis la première date de création, divisé par 31, arrondi à l’unité supérieure. L’API permet de traiter deux tâches d’exportation en même temps afin que vous puissiez effectuer l’extraction avec deux tâches exécutées en parallèle. Les tâches d’extraction en masse sont une ressource partagée avec toutes les autres intégrations, donc je vais être gentil. Je laisse l’autre tâche disponible pour d’autres intégrations et je présenterai l’exécution de tâches uniques l’une après l’autre. Les dates utilisées pour le filtre **createdAt** sont formatées à l’aide de la spécification [ISO 8601](https://www.w3.org/TR/NOTE-datetime). Ils sont toujours en GMT (Z+0000) donc le fuseau horaire sera simplement représenté comme « Z » ou « +00:00 ». Le 1er août 2016 est le **2016-08-01T00:00:00+00:00** et 31 jours plus tard, le 1er septembre 2016 est le **2016-09-01T00:00:00+00:00.** Les heures de début et de fin sont inclusives. Je vais donc soustraire 1 seconde de cette heure de fin : **2016-09-01T00:00:00+00:00** devient **2016-08-31T23:59:59+00:00**. Soustraire une seconde évite les temps de recouvrement. Comme GMT est la valeur par défaut, vous pouvez également laisser le **Z** ou **+00:00** désactivé.

### Déduplication

Même si j&#39;ai pris la peine d&#39;éviter les chevauchements, j&#39;ai également mis en œuvre la déduplication. Je l’ai fait, car il existe certains cas de figure où les heures changent ([heure d’été](https://en.wikipedia.org/wiki/Daylight_saving_time)), ce qui entraîne des valeurs ambiguës. Par conséquent, l’API d’extraction en bloc de Marketo peut renvoyer des prospects en double inattendus. Il est rare que cela se produise, mais cela doit être pris en compte dans toute intégration utilisant des plages de filtres datetime. J&#39;ai retiré une seconde pour préciser que les temps sont inclusifs. Je ne voudrais pas que vous pensiez que la création d&#39;un emploi avec les périodes **createdAt** et **endAt** de **2016-08-01T00:00:00Z** et **2016-09-01T00:00:00Z** n&#39;inclura pas les prospects créés sur **2016-09-01T00:00:00Z** respectivement ; elle le fera.

### Création d’un traitement

La première étape consiste à créer une tâche à l’aide du point d’entrée [Créer une tâche d’exportation principale](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createExportLeadsUsingPOST). Dans cette démonstration, la demande de création de notre première tâche d’exportation ressemble à ceci :

`POST /bulk/v1/leads/export/create.json`

```json
{ "filter": {"createdAt": {"startAt": "2016-08-01T00:00:00",
                           "endAt": "2016-09-01T00:00:00"}},
"fields":["AccountSource","acquisitionProgramId","Active_c","address","Address_lead","annualRevenue","anonymousIP","BillingAddress","billingCity","billingCountry","BillingGeocodeAccuracy","BillingLatitude","BillingLongitude","billingPostalCode","billingState","billingStreet","blackListed","blackListedCause","city","CleanStatus","CleanStatus_account","company","CompanyDunsNumber","contactCompany","cookies","country","createdAt","customfield","DandbCompanyId","DandbCompanyId_account","dateOfBirth","dddS","department","doNotCall","doNotCallReason","dS","dueDate","DunsNumber","email","EmailBouncedDate","EmailBouncedReason","emailInvalid","emailInvalidCause","emailSuspended","emailSuspendedAt","emailSuspendedCause","externalCompanyId","externalSalesPersonId","facebookDisplayName","facebookId","facebookPhotoURL","facebookProfileURL","facebookReach","facebookReferredEnrollments","facebookReferredVisits","fax","firstName","gender","GeocodeAccuracy","holll","id","industry","inferredCity","inferredCompany","inferredCountry","inferredMetropolitanArea","inferredPhoneAreaCode","inferredPostalCode","inferredStateRegion","interested","interestedIn","isAnonymous","IsEmailBounced","isLead","iSTRUE","Jigsaw","JigsawCompanyId_account","JigsawContactId_lead","Jigsaw_account","Languages_c","lastName","LastReferencedDate","LastReferencedDate_account","lastReferredEnrollment","lastReferredVisit","LastViewedDate","LastViewedDate_account","Latitude","leadPartitionId","leadPerson","leadRevenueCycleModelId","leadRevenueStageId","leadRole","leadScore","leadSource","leadStatus","linkedInDisplayName","linkedInId","linkedInPhotoURL","linkedInProfileURL","linkedInReach","linkedInReferredEnrollments","linkedInReferredVisits","links","Longitude","MailingAddress","MailingGeocodeAccuracy","MailingLatitude","MailingLongitude","mainPhone","marketingSuspended","marketingSuspendedCause","middleName","mktoAcquisitionDate","mktocomment1","mktocomments2","mktoCompanyNotes","mktoDoNotCallCause","mktoIsCustomer","mktoIsPartner","mktoName","mktoPersonNotes","mktosync","mktotest1","mobile","mobilePhone","NaicsCode","NaicsDesc","newcustom","numberOfEmployees","originalReferrer","originalSearchEngine","originalSearchPhrase","originalSourceInfo","originalSourceType","OtherAddress","OtherGeocodeAccuracy","OtherLatitude","OtherLongitude","personPrimaryLeadInterest","personTimeZone","personType","phone","PhotoUrl","PhotoUrl_account","postalCode","priority","ProductInterest_c","rating","referal","registrationSourceInfo","registrationSourceType","relativeScore","relativeUrgency","requiredNumberofCylinder","salutation","sfdcAccountId","sfdcContactId","sfdcId","sfdcLeadId","sfdcLeadOwnerId","sfdcType","ShippingAddress","ShippingGeocodeAccuracy","ShippingLatitude","ShippingLongitude","sicCode","SicDesc","site","state","surveyAnswers","syndicationId","testBooleanField","testscore","title","totalReferredEnrollments","totalReferredVisits","Tradestyle","twitterDisplayName","twitterId","twitterPhotoURL","twitterProfileURL","twitterReach","twitterReferredEnrollments","twitterReferredVisits","uNSUBSCIBE","unsubscribed","unsubscribedReason","updatedAt","urgency","url","website","YearStarted"]}
```

La réponse ressemble à ceci :

```json
{
  "requestId": "6902#16fb52118bf",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Created",
          "createdAt": "2020-01-17T20:10:43Z"
      }
  ],
  "success": true
}
```

### Mettre le traitement en file d’attente

L&#39;emploi est maintenant créé, mais il reste là à ne rien faire. Pour exécuter la tâche, nous devons appeler le point d’entrée [enqueue](https://developer.adobe.com/marketo-apis/api/mapi/#operation/enqueueExportLeadsUsingPOST) à l’aide de la valeur **exportId** afin de créer l’URI de la requête. Voici à quoi cela ressemble :

`POST /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/enqueue.json`

Il n’y a pas de corps pour ce POST, nous utilisons simplement le verbe HTTP POST ici. Cette requête va générer une réponse comme celle-ci :

```json
{
  "requestId": "1836a#16fb5238a48",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Queued",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z"
      }
  ],
  "success": true
}
```

Comme je l&#39;ai mentionné plus tôt, vous êtes limité dans le nombre d&#39;emplois qui peuvent être exécutés à la fois. Il existe également une limite au nombre de tâches en file d’attente à la fois : 10. Il nous en faut plus de 40 pour que cette limite nous empêche de créer tous les emplois en même temps. D’autres intégrations peuvent également exécuter des tâches, nous devons donc tenir compte de la possibilité que tous les emplacements soient pleins. Si vous essayez de mettre en file d’attente un nouveau traitement alors qu’il y a déjà 10 traitements en file d’attente, une erreur [1029](/help/rest-api/error-codes.md) se produit. Lorsque vous obtenez une **1029**, utilisez une exponentielle backoff jusqu’à ce que la tâche puisse être mise en file d’attente. J’attends 1 minute et double cette valeur chaque fois que j’obtiens un code d’erreur **1029** jusqu’à 4 minutes entre les requêtes, mais jamais plus longtemps. Cette technique est connue sous le nom de [Recul exponentiel binaire tronqué](https://devopedia.org/binary-exponential-backoff) et est recommandée pour les erreurs récupérables et les contrôles d’état.

### Attendre la fin du traitement

L’exécution de chaque tâche prend un certain temps. Nous appellerons donc le point d’entrée [status](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET) pour surveiller sa progression. Là encore, nous inclurons le **exportId** dans l’URI de requête comme suit :

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/status.json`

Avant que la tâche ne soit terminée, vous obtenez une réponse qui ressemble à celle-ci :

```json
{
    "requestId": "153cb#16fb525435d",
    "result": [
        {
            "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-17T20:10:43Z",
            "queuedAt": "2020-01-17T20:13:23Z",
            "startedAt": "2020-01-17T20:13:49Z"
        }
    ],
    "success": true
}
```

J’exécute la même rétroaction exponentielle (1 minute jusqu’à 4 minutes) pendant que le statut de la tâche est « En file d’attente ». Le statut n&#39;est pas en temps réel ; il est mis à jour une fois par minute et il y a très peu d&#39;avantages à aller plus vite dans les sondages. J’ai réinitialisé l’interruption lorsque le statut de la tâche passe à « Traitement ». Nous attendons un statut « Terminé », qui ressemble à ceci :

```json
{
  "requestId": "10ad9#16fb5268f9b",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Completed",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z",
          "startedAt": "2020-01-17T20:13:49Z",
          "finishedAt": "2020-01-17T20:15:28Z",
          "numberOfRecords": 59,
          "fileSize": 74436,
          "fileChecksum": "sha256:de553362e7ffad6556ae9ea749655c35010c7f0e944fc5a85782183130dca18d"
      }
  ],
  "success": true
}
```

La valeur **numberOfRecords** est nulle lorsque la requête ne renvoie aucun prospect. Je vérifie cette valeur et j’ignore les étapes suivantes lorsque cela est logique. Lorsque des prospects sont renvoyés, j’extrait la valeur **fileChecksum**. Nous l&#39;utilisons pour vérifier l&#39;intégrité du fichier lors de son téléchargement.

### Obtenir vos pistes

Si le **numberOfRecords** est supérieur à zéro, téléchargez le fichier exporté à l’aide du [Obtenir le fichier de lead d’exportation](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET) avec une requête comme celle-ci :

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/file.json`

Vérifiez que le fichier transféré n&#39;a pas d&#39;erreur : calculez la somme de contrôle du fichier et comparez-la à la **fileChecksum** que nous avons enregistrée précédemment. Calculez la somme de contrôle à l’aide de [SHA-2](https://en.wikipedia.org/wiki/SHA-2) et plus précisément de la fonction de hachage SHA256. Si les sommes de contrôle calculées ne correspondent pas, une erreur s&#39;est produite lors du transfert du fichier et vous pouvez réessayer le transfert ou l&#39;abandonner et le récupérer manuellement.

### Agréger les données

Après avoir parcouru toutes les plages de 31 jours à partir de la première piste jusqu&#39;à aujourd&#39;hui, vous aurez un ensemble complet. Vous disposerez également d&#39;un fichier pour chaque plage. La méthode la plus simple pour créer un fichier agrégé unique avec chaque prospect consiste à concaténer ces fichiers après avoir supprimé la ligne d’en-tête pour tous les fichiers, à l’exception du premier. Dans ce cas, n’oubliez pas de prévoir et de planifier une duplication éventuelle plus tard dans votre pipeline de traitement des données. Dans ma démonstration, je traite les fichiers au fur et à mesure de leur téléchargement. Avant d’ajouter chaque ligne de données au fichier de sortie, je déduplique en vérifiant si l’ID de prospect de la ligne a déjà été écrit.

J&#39;ai développé un code de démonstration hébergé [ici](https://github.com/Marketo/REST-Sample-Code/tree/master/python/LeadDatabase/Leads) qui, je l&#39;espère, renseigne les détails de ce processus et peut servir de modèle pour votre propre développement. Le code de démonstration est conçu pour être un outil d’apprentissage afin d’améliorer la robustesse requise pour un système d’exploitation. **Le code est fourni en l&#39;état** sous la licence MIT, mais il est probablement suffisant pour une utilisation ponctuelle avec supervision humaine. Rien ne vous arrête maintenant ! Lorsque vous suivez ce processus, vous extrayez chaque prospect à l’aide de l’API d’extraction en bloc Marketo et potentiellement chaque champ pour l’instance Marketo Engage cible. Pour étendre davantage vos données, obtenez les activités de chaque prospect à l’aide des techniques suivantes.

Publié le _2020-03-05_ par _Tony_

## Mises À Jour De Février 2020

En février 2020, nous publierons de nouvelles API REST. Consultez la liste complète des mises à jour ci-dessous.

### Annonces

* Après septembre 2020, les points d’entrée [API de ressources](/help/rest-api/assets.md) n’accepteront plus le paramètre de requête **_method**. Cela a été utilisé pour transmettre des paramètres de requête dans un corps POST afin de contourner les limites de longueur d’URI. Pour répondre aux requêtes qui nécessitaient ce paramètre, la limite d’URI pour les API de ressources passera de 6 Ko à 65 Ko.
* En ce qui concerne notre position sur ITP, consultez cet article de la communauté Marketo : [&#x200B; Mises à jour des cookies de navigateur : comment Marketo/Munchkin est affecté](https://nation.marketo.com:443/t5/knowledgebase/browser-cookie-updates-how-marketo-munchkin-is-affected/ta-p/251524)
* Une modification a été apportée à l&#39;activité « Changement de statut en cours ». L’attribut « ID de membre de programme » a été ajouté à pour prendre en charge la fonctionnalité à venir « Champs personnalisés de membre de programme ».

Publié le _2020-02-26_ par _David_

## Obsolescence de la méthode de lead associée Munchkin

Avec la prochaine version du client Munchkin JavaScript, la version 159, nous commencerons à abandonner la méthode Munchkin [Associer le prospect](/help/javascript-api/api-reference.md). À partir de cette version, lorsque la méthode est appelée, un avertissement s’affiche dans la console du navigateur pour indiquer que la méthode sera supprimée dans une version ultérieure. Une fois la méthode supprimée, toute tentative d’utilisation de la méthode échouera. Les clients Marketo que nous avons identifiés comme ayant utilisé cette méthode récemment seront notifiés individuellement de leur utilisation. Pour plus d’informations sur la version Munchkin 159, [voir cet article de Marketing Nation](https://nation.marketo.com:443/t5/product-documents/munchkin-javascript-version-159-amp-associate-lead-deprecation/ta-p/299687).

### Questions fréquentes

Comment savoir si je suis affecté ?

Adobe avertira les clients pour lesquels nous avons observé l’utilisation de cette méthode sur leur abonnement, et ce plusieurs fois pendant la période d’obsolescence.

Quand la méthode sera-t-elle supprimée ?

Cette méthode sera supprimée dans Munchkin JS v161, prévue pour la disponibilité générale avec la version d’octobre 2021 de Marketo.

Pourquoi cette méthode est-elle supprimée ?

Des méthodes plus performantes permettant de répondre aux mêmes cas d’utilisation ont été mises en œuvre et publiées depuis l’introduction de cette méthode. Afin d&#39;améliorer la performance et la santé de nos services, il est parfois nécessaire de supprimer les caractéristiques qui ne fonctionnent pas selon des normes acceptables.

Que dois-je utiliser à la place de cette méthode ?

Le prospect associé de Munchkin dispose de deux cas d’utilisation principaux : l’envoi de données de personne et l’association d’un cookie de suivi web du navigateur à l’enregistrement de personne correspondant dans Marketo. Depuis l’introduction de Munchkin Associate Lead, des méthodes plus fiables ont été mises en œuvre pour effectuer l’envoi des données et l’association des cookies.

#### Envoi Côté Serveur

Si vous n’avez pas besoin d’un envoi côté navigateur, l’API REST offre de nombreuses méthodes pour l’[envoi de données de personne](/help/rest-api/leads.md), ainsi qu’une [&#x200B; méthode spécialement conçue pour associer des cookies aux enregistrements de personne](/help/rest-api/leads.md).

Quand la version 159 de Munchkin est-elle déployée ?

La version 159 est en disponibilité générale depuis la version Marketo d’octobre 2020.

Publié le _2020-05-06_ par _Kenny_

## Envoyer un formulaire Marketo en arrière-plan

Lorsque votre entreprise dispose de nombreuses plateformes différentes pour héberger le contenu web et les données client, il devient assez courant de devoir effectuer des envois parallèles à partir d’un formulaire afin que les données obtenues puissent être collectées sur des plateformes distinctes. Il existe plusieurs stratégies pour ce faire, mais la meilleure est souvent la plus simple : utiliser l’API Forms 2 pour envoyer un formulaire Marketo masqué. Cela fonctionne avec tout nouveau formulaire Marketo, mais dans l’idéal, vous devez créer un formulaire vide pour celui-ci, qui ne comporte aucun champ. Cela permet de s’assurer que le formulaire ne charge pas plus de données que nécessaire, puisque nous n’avons pas besoin d’effectuer de rendu. Il vous suffit maintenant d’extraire le [code incorporé](https://experienceleague.adobe.com/fr/docs/marketo/using/home) de votre formulaire et de l’ajouter au corps de la page souhaitée, en apportant une petite modification. Votre code incorporé comprend un élément de formulaire comme suit :

`<form id="mktoForm_1068"></form>`

Vous souhaiterez ajouter &#39;style=« display:none« &#39; à l’élément afin qu’il ne soit pas visible, comme dans l’exemple suivant :

`<form id="mktoForm_1068" style="display:none"></form>`

Une fois le formulaire incorporé et masqué, le code permettant d’envoyer le formulaire est très simple :

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"<test@example.com>",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms envoyé de cette manière se comportera exactement comme si le prospect avait rempli et envoyé un formulaire visible. Le déclenchement de l’envoi varie d’une implémentation à l’autre, car chacune d’elles comporte une action différente pour l’envoyer, mais vous pouvez la faire se produire pratiquement n’importe quelle action. La partie importante consiste à définir correctement vos champs et valeurs. Veillez à utiliser le nom de l’API SOAP de vos champs que vous pouvez retrouver avec les noms des champs d’exportation afin d’assurer un envoi correct des valeurs.

### Migration à partir du lead associé de Munchkin

L’envoi d’un formulaire d’arrière-plan est l’une des méthodes de remplacement recommandées pour le prospect associé Munchkin. L’exemple de page HTML ci-dessous illustre la migration à un niveau élevé, en réutilisant les mêmes valeurs qui sont envoyées au prospect associé.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName['head'](0).appendChild(s);
        })();
        </script>
</head>

<body>
  <!--
    Start Embed code.
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //pass the same set of values to associateLead
            //hashString: secret + email
            Munchkin.munchkinFunction('associateLead', values, "CHANGE ME");

            //submit the form
            form.submit();


        })
    </script>
</body>

</html>
```

Publié le _2020-05-26_ par _Kenny_

## Mises À Jour De Juillet 2020

En juillet 2020, nous publierons de nouvelles API REST, améliorerons les API existantes et résoudrons les défauts. Consultez la liste complète des mises à jour ci-dessous.

* Ajout de deux points d’entrée qui vous permettent d’interroger et de supprimer les utilisateurs qui n’ont pas encore accepté leur invitation (c’est-à-dire les utilisateurs « en attente »). Le point d’entrée [Obtenir l’utilisateur invité par ID](/help/rest-api/user-management.md) vous permet d’interroger un utilisateur en attente. Le point d’entrée [Supprimer l’utilisateur invité](/help/rest-api/user-management.md) vous permet de supprimer un utilisateur en attente.
* Le point d’entrée [Inviter un utilisateur](/help/rest-api/user-management.md) a été mis à jour afin d’accepter les chaînes de date et d’heure conformes à la norme ISO 8601 pour le paramètre **expiresAt**.
* Les points d’entrée [Obtenir l’utilisateur par ID](/help/rest-api/user-management.md) et [Mettre à jour l’attribut utilisateur](/help/rest-api/user-management.md) ont été mis à jour afin de renvoyer l’heure de dernière connexion de l’utilisateur dans l’attribut **lastLoginAt**.
* Correction d’un problème en raison duquel le point d’entrée [Créer une liste statique](https://developer.adobe.com/marketo-apis/api/asset/#operation/createStaticListUsingPOST) renvoyait une erreur « 611, Erreur système » lorsque vous tentiez de créer une liste statique qui existait déjà. Modification pour renvoyer l’erreur « 709, une liste statique portant le même nom existe déjà ». [LM-135934]
* Correction d’un problème en raison duquel le point d’entrée [Créer un e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailUsingPOST) renvoyait une erreur « 611, Erreur système » lorsque vous tentiez de créer un e-mail qui existait déjà. Modification pour renvoyer l’erreur « 709, un e-mail portant le même nom existe déjà ». [LM-138648]
* Correction d’un problème en raison duquel les points d’entrée de requête de page de destination renvoyaient des valeurs **createdAt** incorrectes. Les points d’entrée renvoyaient l’heure de la dernière approbation d’une page de destination. Ils reviennent maintenant à l’heure à laquelle une page de destination a été créée. [LM-138648]
* Correction d’un problème en raison duquel le point d’entrée [Fusionner les leads](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) renvoyait une erreur « 611, Erreur système » pour une opération de fusion non valide. Se produisait lorsque la fusion entraînait un prospect en double et que **mergeinCRM** était défini sur true. Modification pour renvoyer l&#39;erreur « 712, Vous créez un enregistrement en double. Nous vous recommandons d’utiliser plutôt un enregistrement existant. » [LM-137463]

Publié le _2020-08-01_ par _David_

## Publication de l’invité - Séance d’immersion : objet personnalisé / activités personnalisées / champ personnalisé

**Ce message provient d’Amit Jain, Marketo Champion 2020, spécialiste informatique MarTech** En tant que plateforme d’automatisation marketing au niveau de l’entreprise, Marketo gère les informations sur vos utilisateurs, clients et prospects acquises à partir de plusieurs sources et les conserve dans Marketo pour une meilleure personnalisation, segmentation et création de rapports. Ces sources peuvent aller des formulaires de votre site web à la création de listes, en passant par vos données CRM et vos données d’e-commerce. Marketo propose des objets standard, à savoir Lead, Entreprise, Opportunité et Activité, etc. Ces objets standard ne sont parfois pas suffisants pour répondre aux besoins marketing émergents des jeux de données étendus disponibles dans Marketo.

Par exemple, les articles ajoutés au panier, les étudiants qui ont postulé pour différents cours, les produits appartenant à une personne spécifique, les consentements pour différents abonnements, etc. Ces informations peuvent s’avérer essentielles pour que les professionnels du marketing maintiennent l’engagement du client/prospect en offrant un contenu plus personnalisé et une expérience utilisateur unifiée sur l’ensemble des plateformes. Pour répondre à ces besoins professionnels, Marketo propose des méthodes personnalisées pour stocker ce type de données dans des champs personnalisés, des activités personnalisées et des objets personnalisés. J&#39;ai constaté que des gens avaient du mal à comprendre quand utiliser l&#39;une ou l&#39;autre des options. Dans cet article de blog, **nous allons comprendre en détail ce que sont ces trois choses et quand utiliser laquelle avec quelques exemples.**

**Champs personnalisés**

L’objet « Lead » de Marketo est l’objet principal et tout le reste est lié à celui-ci, directement ou indirectement. Marketo vous permet de créer des champs personnalisés sur l’objet « Lead » ou « Société », et a récemment annoncé la prise en charge des champs personnalisés « Membre du programme ». Ces champs personnalisés répondent à votre besoin de stocker certains types de données. Par exemple, vous pouvez avoir besoin de stocker la fonction de travail ou différents consentements de l’utilisateur. Il existe deux types de champs personnalisés dans Marketo :

1. **Synchronisé à partir des champs CRM :** dans le cadre de la synchronisation automatique Marketo ↔︎ SFDC, Marketo synchronise tous les champs personnalisés visibles par l’utilisateur Marketo dans SFDC sur l’objet lead, contact, compte et opportunité.
1. **Champs Marketo uniquement :** vous pouvez directement créer un champ dans Marketo. Les données de ces champs ne seront pas synchronisées avec SFDC.

**Conseils rapides**

* Disposez-vous d’un champ Marketo uniquement et souhaitez-vous maintenant synchroniser les données dans SFDC ? Consultez [ce blog](https://themarketingautomationblog.com/2019/12/06/field-management-merging-remapping-hiding-marketo-fields/) pour découvrir comment y parvenir.
* En savoir plus sur les champs personnalisés [ici](https://themarketingautomationblog.com/2019/11/15/knowing-your-marketo-fields/).
* Pour l’instant, les API Marketo ne prennent pas en charge la mise à jour/création de champs personnalisés.

**Objets personnalisés**

Outre les objets standard, Marketo vous permet de créer vos propres objets personnalisés. _Vous pouvez créer des objets personnalisés et les mettre en relation avec un objet Société ou Lead ou un autre objet personnalisé._ Un objet personnalisé est un ensemble d’enregistrements personnalisés qui complètent les enregistrements standard Lead et Société. Les objets personnalisés vous permettent de stocker des données supplémentaires de manière évolutive et de les lier à un enregistrement Lead ou Company. Vous pouvez créer un objet personnalisé avec n’importe quelle combinaison de champs standard (Champs de lien) et personnalisés, remplir ces champs pour créer un objet personnalisé _enregistrements_, puis lier ces enregistrements à un enregistrement Lead ou Company. Puissants et flexibles, les enregistrements liés enrichissent vos segments, listes dynamiques et campagnes en vous permettant de créer ces ressources avec des informations introuvables dans les champs de prospect et d’entreprise. Un objet personnalisé peut avoir l’un des types de relations suivants :

* **Relation un-à-un** : où chaque objet personnalisé a un seul enregistrement d’objet de lead/entreprise.
* **Relation un-à-plusieurs** : où chaque objet personnalisé contient plusieurs enregistrements d’objet personnalisés liés à un prospect/une entreprise.
* **Relation multiple-à-multiple :** dans laquelle plusieurs enregistrements d’objets personnalisés peuvent être liés à plusieurs objets de prospect/entreprise. Par exemple, plusieurs étudiants sont inscrits à plusieurs cours à partir d’un catalogue de cours.

**Conseils rapides**

* En savoir plus sur la configuration d’objets personnalisés [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/home).
* Vous pouvez utiliser l’objet personnalisé Marketo comme objet intermédiaire, c’est-à-dire comme objet personnalisé d’un objet personnalisé.

### Pourquoi des objets personnalisés ?

Les objets personnalisés vous permettent de compiler et d’utiliser des données uniques qui sont pertinentes pour une entreprise ou un prospect, mais qui ne sont pas nécessairement des informations statiques sur l’entreprise ou le prospect lui-même. Bien que les champs de prospect se rapportent aux informations de prospect d’une personne (_adresse e-mail_, _code postal_, etc.), aux informations commerciales (_titre de la fonction_, _secteur_, etc.) ou aux informations pilotées par le système (_Marketo ID_, un SFDC ID ou une date de création, etc.), les champs d’objet personnalisés sont entièrement personnalisables. Par exemple, utilisez des objets personnalisés pour stocker des informations telles que les suivantes :

* Historique des achats d’un utilisateur.
* Informations sur le panier.
* Si un client a utilisé ou non l’un de vos codes de réduction promotionnels à durée limitée.
* Informations complémentaires acquises à partir des résultats du sondage, des entretiens et de la participation à des événements.
* Informations sur l’évaluation d’un utilisateur, notamment l’URL de l’instance d’évaluation, la date de début, la date de fin, le nombre d’utilisateurs, etc.

### Limites des objets personnalisés Marketo

1. Par défaut, Marketo vous permet de créer 10 objets personnalisés. Vous pouvez augmenter la limite avec des frais d&#39;abonnement supplémentaires.
1. Le nombre par défaut de champs par objet est de 50, mais vous pouvez demander la prise en charge de Marketo pour des champs supplémentaires si nécessaire. Merci [Michael Florin](https://www.linkedin.com/in/michaelflorin/) pour vos commentaires.
1. Le nombre d’enregistrements que vous pouvez avoir sur tous les objets personnalisés est limité. Cela dépend de votre abonnement avec Marketo. Cette limite peut être augmentée avec des frais d&#39;abonnement supplémentaires.
1. Si un objet personnalisé a été créé à l’aide de l’API, Marketo ne permet pas de modifier le schéma CO à partir de l’interface utilisateur de Marketo.

**Conseils rapides**

* Prise en charge par l’API Marketo d’une opération CRUD (Create, Read, Update et Delete) sur des objets personnalisés.
* Vous pouvez utiliser ces données d’objet personnalisées pour la personnalisation des e-mails à l’aide du script Velocity.
* Vous trouverez tous les points d’entrée liés à l’objet personnalisé [ici](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportProgramMembersStatusUsingGET).

### Custom Object Terminology

**Objet personnalisé** : conteneur contenant un regroupement de tous les enregistrements d’objet personnalisé. Formellement appelé jeu de cartes de données ou tableau personnalisé. **Enregistrement d’objet personnalisé** : enregistrement de données contenant des informations de champ supplémentaires qui peuvent être liées à un prospect ou à une entreprise. Un enregistrement peut être constitué de champs de prospect ou d&#39;entreprise standard et de champs d&#39;enregistrement d&#39;objet personnalisés. Formellement appelée ligne de carte de données ou de tableau de données. **Champ d’enregistrement d’objet personnalisé** : champs entièrement personnalisables pour collecter des informations uniques ou temporaires. Ces champs sont créés et hébergés dans l’objet personnalisé lui-même. Formellement appelé champ de carte de données ou champ/colonne de table de base de données. **Champ de lien** : type spécial de champ d’enregistrement d’objet personnalisé pour définir la relation entre l’enregistrement d’objet personnalisé et l’enregistrement d’objet de lead/société lié. Lorsque vous créez des objets personnalisés, vous devez fournir des champs de lien pour connecter l&#39;enregistrement d&#39;objet personnalisé à l&#39;enregistrement parent correct.

* Pour une structure personnalisée un-à-plusieurs ou un-à-un, utilisez le champ Lien dans l’objet personnalisé pour le connecter à une personne ou à une entreprise.
* Pour une structure multiple-à-multiple, vous utilisez deux champs de lien, connectés à partir d’un objet intermédiaire créé séparément (qui est également un type d’objet personnalisé). Un lien se connecte aux personnes ou aux sociétés de votre base de données et l’autre se connecte à l’objet personnalisé. Dans ce cas, le champ de lien ne se trouve pas dans l’objet personnalisé lui-même.

### Activités personnalisées

**Il existe plusieurs façons d’interagir avec notre organisation. Ils peuvent visiter le site Web de votre entreprise, assister à l&#39;un de vos salons professionnels ou peut-être cliquer sur un lien dans un courriel que vous avez envoyé. Ces actions sont des activités** et quelle que soit l’action qu’elles entreprennent, Marketo la capture afin que vos équipes marketing et commerciales puissent mieux comprendre le comportement de l’utilisateur ou de l’utilisatrice pour un engagement personnalisé et unifié. **_Activités personnalisées_** _peut vous aider à suivre une activité qui n’est pas liée à un formulaire Marketo, un e-mail ou une page de destination_. Par exemple, si vous souhaitez suivre le moment où une personne a visionné une vidéo sur un site web ou a répondu à un questionnaire, utilisez une activité personnalisée. Les activités personnalisées diffèrent des objets personnalisés. Utilisez des objets personnalisés lorsque la valeur peut changer (par exemple, la « couleur de la voiture » passe de bleu à rouge). Utilisez des activités personnalisées lors du suivi des moments qui se sont produits et dont les détails ne peuvent pas changer (par exemple, « voiture achetée »). Par défaut, le nombre maximal d’activités personnalisées pouvant être définies est de 10. Ce montant peut être majoré de frais d&#39;abonnement supplémentaires. Conformément à la politique de conservation des données de [Marketo](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents), les activités personnalisées seront automatiquement supprimées après 25 mois.

**Activité personnalisée :** événements non Marketo dont vous souhaitez effectuer le suivi dans Marketo. **Identifiant d’activité personnalisé :** Marketo attribuez un identifiant numérique à l’activité personnalisée qui peut être utilisée lors d’une tentative de notification push/pull des données d’activité à l’aide de l’API Marketo. **Champs d’activité personnalisés :** les métadonnées d’activité peuvent être stockées dans un champ d’activité. Par exemple, si vous effectuez le suivi des vues en vidéo, les champs peuvent être l’URL de la page, le titre de la vidéo, etc. **Champ de Principal d’activité personnalisé :** champs d’activité personnalisés pouvant être utilisés comme critères de filtre de liste dynamique.

**Conseils rapides**

* Les points d’entrée d’API pour les activités personnalisées sont disponibles [ici.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST)

## Objet personnalisé par rapport à activité personnalisée

**Objet personnalisé**

**Activité personnalisée**

1

10 objets personnalisés maximum par défaut par instance.

10 activités personnalisées maximum par défaut.

2

50 champs d’objet personnalisés maximum par objet personnalisé.

Chaque type d’activité personnalisé peut avoir jusqu’à 20 attributs secondaires.

3

Nombre maximal d’enregistrements d’objets personnalisés de 1 MM ; peut être très élevé en fonction de votre abonnement.

Au bout de 25 mois, les activités personnalisées seront supprimées conformément à la politique de conservation des données de Marketo.

4

Peut être utilisé comme filtre et déclencheur dans les listes dynamiques et les campagnes dynamiques.

Peut être utilisé comme filtre et déclencheur dans les listes dynamiques et les campagnes dynamiques.

5

Peut être utilisé pour personnaliser le contenu de l’e-mail.

Ne peut pas être utilisé pour personnaliser le contenu de l’e-mail.

6

Peut effectuer une opération CRUD sur un enregistrement d&#39;objet personnalisé.

Seules les opérations Créer et Lire sont autorisées dans les activités personnalisées.

7

Utilisez des objets personnalisés lorsque la valeur peut changer (par exemple, la « couleur de la voiture » passe de bleu à rouge).

Utilisez des activités personnalisées lors du suivi des moments qui se sont produits et dont les détails ne peuvent pas changer (par exemple, « voiture achetée »).

8

Les objets personnalisés vous le disent. c’est-à-dire la valeur actuelle.

Les activités personnalisées vous informent des événements qui se sont produits dans le passé.

Publié le _2020-10-18_ par _Amit_

## Mises À Jour De Janvier 2021

En janvier 2021, nous publierons de nouvelles API REST et résoudrons plusieurs défauts. Consultez la liste complète des mises à jour ci-dessous.

* Ajout d’un point d’entrée [Envoyer le formulaire](/help/rest-api/leads.md) qui vous permet d’effectuer des envois de formulaire programmatiques. Les formulaires tiers peuvent désormais s’intégrer aux formulaires Marketo pour tirer parti des workflows marketing existants.
* Ajout d’un point d’entrée [Obtenir le contenu complet de la page de destination](/help/rest-api/landing-pages.md) qui renvoie la version HTML sérialisée d’une page de destination. Vous permet de générer des aperçus entièrement personnalisés de pages de destination sans avoir à vous connecter à Marketo Engage. Cela peut vous aider à rationaliser les workflows de modification et de traduction dans les applications intégrées.
* Vous pouvez désormais configurer le nombre d’objets personnalisés auxquels vous pouvez accéder via le script Velocity. Les instructions de configuration sont disponibles [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).

### Résolutions des défauts

* Correction d’un problème en raison duquel le point d’entrée [Supprimer un utilisateur](/help/rest-api/user-management.md) vous permettait de supprimer un utilisateur API uniquement qui était utilisé par un service personnalisé. Elle renvoie désormais une erreur « 611, You cannot delete an API user is used in API service ». [LM-141893]
* Correction d’un problème en raison duquel le point d’entrée [Get Users](/help/rest-api/user-management.md) renvoyait des utilisateurs supprimés dans certains cas. [LM-141542]
* Correction d’un problème en raison duquel le point d’entrée [Cloner le programme](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST). Si vous avez spécifié un nom de programme qui dépasse 255 caractères, il renvoie « 611, erreur « Impossible de cloner le programme ». Désormais, il renvoie « 701, le nom ne peut pas dépasser 255 caractères ». [LM-143436]
* Correction d’un problème lié au point d’entrée [Approuver le brouillon de page de destination](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST). Lorsque vous avez approuvé une page de destination avec la version mobile activée, le contenu de la version mobile s’affiche dans la version bureau dans certains cas. [LM-146867]
* Correction d’un problème lié au point d’entrée [Annuler l’approbation de la page de destination](https://developer.adobe.com/marketo-apis/api/asset/#operation/unapproveLandingPageByIdUsingPOST) qui permettait d’annuler l’approbation d’une page de destination utilisée comme page de suivi par un ou plusieurs formulaires. Elle renvoie désormais une erreur « 709, échec de l’annulation de l’approbation de la page de destination. Une page de destination est utilisée par un ou plusieurs formulaires comme page de suivi avec les ID de formulaire : [_formId1,formId2,..._] ». [LM-143326]

Publié le _2021-01-15_ par _David_

## API Beta et de balise Munchkin 160

**27 janvier 2021 :** certains utilisateurs Marketo qui seront affectés par l’obsolescence du prospect associé ont reçu une notification par e-mail par erreur indiquant que le paramètre Munchkin Beta est activé sur une ou plusieurs de leurs instances. Cette version sera publiée jusqu’à ce que l’audience appropriée puisse être avertie. À partir de la version 160 du JavaScript Munchkin, l’[API de balise](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) devient la méthode par défaut par laquelle Munchkin communique avec le serveur principal Marketo. Elle est devenue disponible en option à l’été 2020 avec la version 159 via le paramètre de configuration **useBeaconAPI**. L’API Beacon présente plusieurs avantages par rapport à l’utilisation de l’ancienne méthode XMLHttpRequest, mais l’amélioration principale est qu’il s’agit d’une API asynchrone non bloquante pour la communication HTTP, disponible dans tous les navigateurs Internet modernes. Bien que la plupart des utilisateurs et utilisatrices de Munchkin ne remarquent aucun changement dans le comportement du site web, cette mise à jour empêche Munchkin Munchkin de bloquer la navigation lors de l’attente de l’envoi d’un événement de clic au serveur principal. Plus simplement, elle élimine quasiment la possibilité qu’un navigateur se « bloque » après avoir cliqué sur un lien vers une nouvelle page. Il s’agit d’une situation rare, mais frustrante, pour certains clients de Marketo.

Depuis le 27 janvier 2021, le déploiement de cette version est en attente de replanification. Bien qu’aucun problème lié à cette modification ne soit anticipé et qu’aucun n’ait été identifié lors du test, il est impossible pour Marketo de tester toutes les configurations de déploiement possibles de Munchkin. Vous pouvez tester ces modifications avant ou renoncer à cette modification jusqu’à la disponibilité générale de cette version. Les instructions relatives à divers scénarios sont présentées ci-dessous.

### Tester l’API de balise

Si vous souhaitez tester l’API de balise mise à jour en prévision de la version à venir, vous pouvez le faire en ajoutant le paramètre **useBeaconAPI** à votre configuration Munchkin sur une page de test externe. Ce test fonctionne avec la version Munchkin généralement disponible ou en version bêta. Le paramètre de configuration est présenté ci-dessous dans le deuxième argument de l&#39;appel de la méthode `Munchkin.init()` à la ligne 7 : `{ 'useBeaconAPI': true}`

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827', {"useBeaconAPI":true});
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

### Désactivation de Munchkin Beta sur les pages de destination de Marketo

Pour désactiver Munchkin Beta sur les pages de destination de Marketo, vous devez accéder au menu [Coffre au trésor](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) dans la section Admin de votre abonnement et définir le paramètre Munchkin Beta sur Désactivé.

### Désactiver Munchkin Beta sur les pages externes

Si vous avez déployé la version Beta de Munchkin JavaScript sur des pages web externes et que vous souhaitez renoncer à cette modification jusqu’à ce qu’elle soit disponible pour tous, vous devez modifier votre fragment de code JS Munchkin pour cibler le **chaton).Fichier &#x200B;**&#x200B;**js** au lieu du fichier **munchkin-beta.Fichier &#x200B;**&#x200B;**js**. Dans l’exemple ci-dessous, il s’agit de la valeur de la variable **s.src** à la ligne 11. Votre fragment de code peut ne pas ressembler beaucoup à l’exemple ou peut être déployé par un gestionnaire de balises sur vos pages externes. Il se peut que vous deviez contacter vos ressources informatiques ou la personne qui gère vos sites web avec le suivi Munchkin activé.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';//This line should have the munchkin.js file, not munchkin-beta.js
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

Publié le _2021-01-08_ par _Kenny_

## Obsolescence finale de l’API de l’e-mail V1

[La dépréciation d’Email V1 a commencé il y a près de deux ans](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666) et à partir de la version de maintenance de mars pour les abonnements à Londres et aux Pays-Bas le 17 mars 2021 et de tous les autres abonnements le 19 mars 2021, toute prise en charge d’API pour les emails V1 prendra fin. Après cette version, toute tentative d’interaction avec des e-mails V1 via les API de ressources entraînera des erreurs et aucune action. Tous les utilisateurs restants connus depuis le 24 février 2021 ont été avertis, mais il est possible qu’il existe encore des intégrations qui tentent d’interagir avec ces ressources. Les types d’intégrations les plus courants sont les services qui offrent la gestion des ressources numériques, la traduction et la localisation. Si vous constatez des échecs d’intégration suite à cette modification, [vous pourrez toujours mettre à niveau les ressources problématiques en les modifiant et en les approuvant](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/transitioning-to-email-editor-2-0). Une fois qu’une ressource d’e-mail est mise à niveau vers la version V2, vous devriez être en mesure de la reprendre avec les services intégrés.

Publié le _2021-03-17_ par _Kenny_

## Mises À Jour De Mai 2021

En mai 2021, nous publierons de nouvelles API REST, améliorerons les API REST existantes et résoudrons plusieurs défauts. Consultez la liste complète des mises à jour ci-dessous.

* Ajout d’API de membre de programme qui vous permettent de récupérer, mettre à jour et supprimer des enregistrements d’appartenance à un programme. Pour plus d’informations, consultez [API REST > Base de données de leads > Membres du programme](/help/rest-api/program-members.md).
* Ajout d’API d’extraction d’objets personnalisés en bloc qui vous permettent d’exporter des enregistrements d’objets personnalisés Marketo de premier niveau associés aux prospects dans une relation un-à-plusieurs. Pour plus d’informations, consultez [API REST > Extraction en bloc > Extraction d’objet personnalisé en bloc](/help/rest-api/bulk-custom-object-extract.md).
* Nous avons amélioré l’[API de lead](/help/rest-api/leads.md) et l’[API d’extraction de lead en bloc](/help/rest-api/bulk-lead-extract.md) pour permettre aux utilisateurs de récupérer l’ID de Adobe Experience Cloud (ECID). Cela permet aux utilisateurs qui [synchronisent les audiences à partir de Adobe Experience Cloud](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-experience-cloud-audience-sharing.html?lang=fr) d’identifier les prospects auxquels des ECID sont associés. Cela ouvre la porte [possibilités d’intégration](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360024277392-Adobe-Experience-Cloud-Using-the-ECID-for-integration) à d’autres produits Adobe Experience Cloud.
* Nous avons amélioré l’[API d’importation de leads en bloc](/help/rest-api/bulk-lead-import.md) pour prendre en charge l’ajout de leads aux enregistrements d’entreprise pendant le processus d’importation. Pour ce faire, incluez le champ **externalCompanyId** dans le fichier d’importation.
* Nous avons amélioré plusieurs points d’entrée du programme pour fournir la parité avec les fonctionnalités de l’interface utilisateur de Marketo Engage. Nous avons amélioré les points d’entrée [Créer des programmes](/help/rest-api/assets.md) et [Cloner des programmes](https://developer.adobe.com/marketo-apis/api/asset/) pour permettre la création, le clonage ou le déplacement d’opérations sur les programmes d’événement. Ceci est destiné aux utilisateurs qui organisent des programmes d’événement en les « imbriquant » sous d’autres types de programmes. Nous avons également amélioré le point d’entrée [Supprimer le programme](https://developer.adobe.com/marketo-apis/api/asset/) afin de permettre la suppression des programmes contenant les ressources suivantes : notifications push, messages In-App, rapports, pages de destination avec social Assets intégré.
* En tant qu’administrateur Marketo, vous pouvez [marquer un champ spécifique comme « sensible »](https://experienceleague.adobe.com/fr/docs/marketo/using/home) de sorte que ses valeurs [ne soient jamais préremplies dans les formulaires](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/forms/form-fields/disable-pre-fill-for-a-form-field), protégeant ainsi les données sensibles des utilisateurs. Nous avons amélioré plusieurs points d’entrée de champ de formulaire pour fournir une parité avec cette fonctionnalité disponible dans l’interface utilisateur de Marketo Engage.

### Résolutions des défauts

* Correction d’un problème lié au point d’entrée du programme de suppression. Si vous tentez de supprimer un programme dans un dossier partagé, il renvoie « 611, Erreur système ». Il renvoie désormais « ». Le programme cible se trouve dans un dossier partagé et ne peut pas être supprimé. Le partage du dossier doit être annulé avant toute tentative de suppression. »
* Correction d’un problème lié au point d’entrée du programme de clonage. Si vous tentez de cloner un programme contenant une valeur DateTime dans une étape de flux, il renvoie « 611, Erreur système ». Le programme a été cloné.
* Correction d’un problème lié au point d’entrée Créer des programmes qui vous permettait par inadvertance de créer un programme sous un programme de messagerie (ce qui n’est pas autorisé).
* Correction d’un problème lié au point d’entrée du programme de clonage. Si vous avez cloné un programme qui contenait une page de destination, il manquait un trait de soulignement entre le nom du programme et le nom de la page de destination dans le programme cible. Exemple : `http://<_pod_\>.marketo.com/lp/<_munchkin_\>/<_program name_\>**_**<_LP name_\>.html`

Publié le _2021-05-07_ par _David_

## Offre de durée limitée pour rejoindre Adobe Exchange

L’assistance de la communauté des partenaires Marketo Engage est l’un des piliers du succès de nos clients. Nous voulons nous assurer que l’écosystème d’intégration de Marketo Engage est bien représenté sur Exchange Marketplace et avons une offre spéciale pour les partenaires LaunchPoint. Pour une durée très limitée qui ne sera pas prolongée, nous offrons à nos partenaires LaunchPoint un partenariat innovant gratuit dans le programme Exchange jusqu’à la fin de 2022 (valeur d&#39;environ 15 000 $). Nous avons conçu cette offre pour encourager les partenaires LaunchPoint à créer leurs listes d’intégration dans le portail Partenaires Exchange, qui pourront ensuite faire l’objet de recherches publiques sur Adobe Exchange Marketplace. Pour voir la liste complète des avantages du partenariat Innovate que vous recevez gratuitement jusqu&#39;en décembre 2022.

1. Accédez au [Centre de support partenaire Adobe Exchange](https://adobeexchangeec.zendesk.com/hc/en-us?mkt_tok=NjA4LURIVi05MTUAAAF-P5lIeVWOuBmKMS_uE_NpgFKtC0ukt7z_ksnq_Sbzb6mzXUuXpqpqQeujtPdZ24WcjMdptygQSR9XrYt_Cw)
1. Cliquez sur « Soumettre une demande » dans le coin supérieur droit
1. Dans la liste déroulante **Veuillez choisir votre problème ci-dessous** choisissez « Assistance Adobe Exchange »
1. Dans **Votre adresse e-mail** saisissez votre adresse e-mail
1. Dans la zone **Objet**, saisissez « Offre LaunchPoint »
1. Dans la zone **Description** saisissez « Offre LaunchPoint »
1. Dans le menu déroulant **Type de prise en charge** sélectionnez « Assistance du programme »
1. Dans la liste déroulante **Produit Adobe Exchange** sélectionnez « Programme Adobe Exchange »
1. Envoyez le formulaire. Notre équipe est en contact avec vous sous peu !

Publié le _2021-07-22_ par _David_

## Mises à jour d’août 2021

En août 2021, nous améliorerons les API REST existantes et résoudrons plusieurs défauts. Consultez la liste complète des mises à jour ci-dessous.

* Nous avons amélioré l’API d’extraction d’activité en bloc afin de permettre aux utilisateurs de filtrer à l’aide d’attributs principaux pour 6 types d’activité différents. Pour plus d’informations, voir [Extraction d’activité en bloc](/help/rest-api/bulk-activity-extract.md).
* Pour permettre aux utilisateurs de Marketo Sales Connect d’accéder plus facilement aux données de leur activité de vente, nous avons activé des attributs d’activité de vente supplémentaires. Nous avons ajouté les attributs suivants aux activités Envoyer un e-mail commercial, Ouvrir un e-mail commercial et Cliquer sur l’e-mail commercial :

* ID de commercial Marketo - ID unique pour l’enregistrement de personne dans Sales Connect
* Nom de la campagne de vente : nom de la campagne de vente, si l’e-mail faisait partie d’une campagne de vente.
* URL de campagne commerciale : URL de connexion de vente pour une campagne commerciale si l’e-mail faisait partie d’une campagne commerciale.
* Nom du modèle de vente : nom du modèle d’e-mail dans Sales Connect, si un modèle a été utilisé
* URL du modèle de vente : URL de connexion des ventes pour le modèle d’e-mail, si un modèle a été utilisé

### E-mails

* Nous avons amélioré le point d’entrée Get Emails en ajoutant le filtre `earliestUpdatedAt`/`latestUpdatedAt` . Vous pouvez ainsi utiliser le champ `updatedAt` pour rechercher uniquement un sous-ensemble d’e-mails, ce qui permet une synchronisation incrémentielle.
* Nous avons amélioré les points d’entrée Get Emails, Get Email by Name, Get Email by Id pour prendre en charge la récupération des enregistrements d’e-mail de type [&#x200B; Champion et Challenger &#x200B;](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/email-tests-champion-challenger/add-an-email-champion-challenger).

### Résolutions des défauts

* Correction d’un problème lié au point d’entrée Get Users. Les utilisateurs auxquels une licence [Calendrier marketing](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/marketing-calendar/understanding-the-calendar/issue-revoke-a-marketing-calendar-license) avait été attribuée n’étaient pas renvoyés. Les utilisateurs du calendrier marketing sont désormais correctement renvoyés.
* Correction d’un problème lié au point d’entrée de formulaire Envoyer. Dans le cas d’enregistrements de prospect en double, le formulaire d’envoi est utilisé pour émettre une erreur « 1007, Plusieurs critères de recherche de correspondance de prospect ». Le formulaire d’envoi met désormais à jour l’enregistrement le plus récemment mis à jour de la même manière que l’API [Forms 2.0](/help/javascript-api/forms-api-reference.md).
* Amélioration de plusieurs messages d’erreur trompeurs renvoyés par les points d’entrée Mettre à jour le champ de prospect et Créer des champs de prospect . [LM-151890, LM-151888, LM-151889]
* Correction d’un problème lié aux points d’entrée Obtenir le champ de prospect par nom et Obtenir les champs de prospect . Les deux points d’entrée peuvent potentiellement renvoyer des informations légèrement obsolètes. Ils renvoient désormais toujours les informations les plus récentes.
* Correction d’un problème lié à l’[API d’extraction en bloc](/help/rest-api/bulk-extract.md) lors de l’utilisation de l’en-tête HTTP « range » pour la récupération partielle où le dernier octet de la plage n’était pas renvoyé.
* Correction d’un problème lié au point d’entrée Mettre à jour les métadonnées de fragment de code. Lors de la mise à jour du nom ou de la description du fragment de code, le statut du fragment de code a été modifié en « approuvé avec le brouillon ». Désormais, le statut du fragment de code reste inchangé après la mise à jour du nom ou de la description du fragment.
* Correction d’un problème lié au point d’entrée Ajouter un module d’e-mail . Lors de l’ajout d’un module contenant un fragment de code, il a renvoyé une « erreur système 611 ». Il ajoute désormais correctement le module à l’e-mail.
* Correction d’un problème lié au point d’entrée du programme de suppression. Lors de la suppression d’un programme qui contenait une ressource locale Message In-App, il a renvoyé une « erreur système 611 ». Il supprime désormais correctement le programme.

Publié le _2021-08-22_ par _David_

## Déploiement de Munchkin version 161

Le 7 septembre 2021, la version 161 de Munchkin commencera à être déployée sur 10 % des abonnements avec Munchkin Beta activé, suivie de 50 % le 16 septembre et de 100 % le 30 septembre. Cette modification affectera les pages de destination Marketo et la version du fichier munchkin-beta.js diffusée sur les pages de destination externes chargées à partir des abonnements auxquels la nouvelle version a été déployée. Cette version rend complètement obsolète la méthode Lead associé de Munchkin, qui est une fonctionnalité permettant l’envoi de données de personne vers un abonnement Marketo et l’historique de navigation Web associé avec un enregistrement de personne connu. Le prospect associé est supprimé au profit d’alternatives plus modernes et plus sécurisées, telles que l’[API JS Forms](/help/javascript-api/forms-api-reference.md), l’API d’envoi de formulaire et l’API REST [Associer le prospect](/help/rest-api/leads.md). Si vous ou votre organisation utilisez cette méthode, vous devez abandonner l’utilisation d’ici le 12 octobre 2021, date à laquelle le déploiement de la version d’octobre est prévu. Si vous ne souhaitez plus vous inscrire à la version Beta de Munchkin, vous pouvez désactiver l’utilisation des pages de destination de Marketo en activant la fonction « Munchkin Beta sur les pages de destination » pour `disabled` dans le menu [Coffre au trésor](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features). Si vous avez déployé Munchkin Beta JavaScript sur des pages web externes et que vous souhaitez passer au canal de publication par défaut de Munchkin, vous devez mettre à jour votre fragment de code pour charger Munchkin JavaScript à partir de munchkin.js au lieu de munchkin-beta.js.

Publié le _2021-08-24_ par _Kenny_

## Fin de vie de TLS 1.0 et TLS 1.1 pour le service Munchkin

À compter du 21 octobre 2021, munchkin.marketo.net, qui est utilisé pour fournir Munchkin JavaScript aux visiteurs, n’acceptera plus les connexions à l’aide de [TLS 1.0 ou TLS 1.1.](https://en.wikipedia.org/wiki/Transport_Layer_Security). Ces normes de chiffrement ne sont plus acceptées comme faisant partie des bonnes pratiques de sécurité web et ne sont plus prises en charge par les versions modernes des navigateurs. Il n&#39;y a aucune incidence négative prévue à la suite de ce changement.

Publié le _2021-10-04_ par _Kenny_

## Mises À Jour D’Octobre 2021

En octobre 2021, nous améliorerons les API REST existantes et résoudrons plusieurs défauts. Consultez la liste complète des mises à jour ci-dessous.

* Nous avons amélioré le point d’entrée [Envoyer le formulaire](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) pour prendre en charge les champs personnalisés des membres du programme dans le cadre de l’envoi du formulaire. Vous pouvez éventuellement spécifier un programme en tant que programme auquel ajouter le formulaire et/ou le programme auquel ajouter les champs personnalisés des membres du programme, comme décrit [ici](/help/rest-api/leads.md).
Nous avons amélioré le point d’entrée [Obtenir les membres du programme](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getProgramMembersUsingGET) afin de prendre en charge les requêtes basées sur les périodes en fonction de l’attribut updatedAt. Pour ce faire, transmettez les paramètres datetime de début et de fin comme décrit [ici](/help/rest-api/program-members.md).
* Nous avons amélioré les API [champs de leads](/help/rest-api/leads.md) pour prendre en charge les [champs sensibles](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/field-management/mark-a-field-as-sensitive). Les points d’entrée [Obtenir le champ de lead par nom](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldByNameUsingGET), [Obtenir les champs de lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldsUsingGET), [Créer des champs de lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) et [Mettre à jour le champ de lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updateLeadFieldUsingPOST) prennent désormais en charge l’attribut isSensitive.

### Résolutions des défauts

* Correction d’un problème lié à l’API [&#x200B; User Management &#x200B;](/help/rest-api/user-management.md). Se rapporte aux utilisateurs de Marketo configurés pour être utilisés avec [Sales Insight](https://business.adobe.com/fr/products/marketo/sales-insight.html). Ces utilisateurs sont désormais renvoyés par le point d’entrée [Get Users](https://developer.adobe.com/marketo-apis/api/user/#operation/getUsersUsingGET) et peuvent désormais être supprimés à l’aide du point d’entrée [Delete User](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST). [LM-155864]
* Correction d’un problème lié au point d’entrée Ajouter [champ de texte enrichi](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/addRichTextFieldUsingPOST). Lors de l’ajout d’un champ de texte enrichi de plus de 65 000 caractères à un e-mail, une page de destination, un fragment de code ou un formulaire, il a renvoyé une « erreur système 611 ». Il renvoie désormais l’erreur « 701, l’opération ne peut pas être effectuée. &#39;content&#39; dépasse une longueur maximale de 65 535 octets. »

Publié le _2021-10-25_ par _David_

## Mises À Jour De Janvier 2022

En janvier 2022, nous améliorerons les API REST existantes et résoudrons plusieurs défauts. Consultez la liste complète des mises à jour ci-dessous.

* Nous avons amélioré l’API [Extraction d’objet personnalisé en bloc](/help/rest-api/bulk-custom-object-extract.md) pour permettre aux utilisateurs de filtrer à l’aide d’une période **updatedAt**.
* Ajout d’API de métadonnées de champ de membre de programme qui vous permettent de créer, mettre à jour et récupérer des métadonnées pour les champs de membre de programme. Pour plus d’informations, voir [Membres du programme > Champs](/help/rest-api/program-members.md).
* Ajout d’API de métadonnées de champ d’entreprise qui vous permettent de récupérer les métadonnées des champs d’entreprise. Pour plus d’informations, voir [Entreprises > Champs](/help/rest-api/companies.md).
* Ajout d’API de métadonnées de champ d’opportunité qui vous permettent de récupérer des métadonnées pour les champs d’opportunité. Pour plus d’informations, consultez [Opportunités > Champs](/help/rest-api/opportunities.md).
* Ajout d’API de métadonnées de champ de compte nommé qui vous permettent de récupérer les métadonnées des champs de compte nommé. Pour plus d’informations, voir [Comptes nommés > Champs](/help/rest-api/named-accounts.md).
* Mise à jour des points d’entrée de métadonnées de champ pour renvoyer une nouvelle propriété booléenne **isApiCreated** afin d’indiquer si un champ a été créé ou non par l’API REST.

### Résolutions des défauts

* Correction d’un problème de latence entre l’heure de l’appel au point d’entrée [Créer des champs de prospect](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) et l’heure à laquelle un champ de prospect nouvellement créé était disponible dans la liste dynamique. [LM-152838]
* Correction d’un problème lié au point d’entrée [Créer des champs de prospect](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) en raison duquel les champs créés n’étaient pas disponibles dans la liste déroulante des champs de formulaire utilisée pour [ajouter des champs au formulaire](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/demand-generation/forms/creating-a-form/add-a-field-to-a-form) dans l’interface utilisateur de Marketo Engage. [LM-158243]
* Correction d’un problème lié au point d’entrée [Get Campaigns](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET) en raison duquel les campagnes déclenchables n’étaient pas renvoyées lorsque le paramètre isTriggerable=true était spécifié. [LM-158283]
* Correction d’un problème en raison duquel le point d’entrée [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteTokenByNameUsingPOST) renvoyait une erreur « 611, Erreur système » dans certains cas. [LM-157214]
* Nettoyage de plusieurs messages d’erreur renvoyés par le point d’entrée [Mettre à jour le champ de prospect](/help/rest-api/leads.md). [LM-151886, LM-151888, LM-151889]

Publié le _2022-01-27_ par _David_

## Mises À Jour De Mars 2022

En mars 2022, nous améliorerons les API REST existantes et résoudrons plusieurs défauts. Consultez la liste complète des mises à jour ci-dessous.

* Nous avons ajouté le champ **actionResult** au fichier d’exportation généré par l’API Bulk Activity Extract. Ce champ peut être utilisé pour faire la distinction entre les activités réussies, ignorées et ayant échoué.
* Nous avons ajouté le champ **isOpenTrackingDisabled** aux réponses de l’API [Emails](/help/rest-api/emails.md). Ce champ peut être utilisé pour déterminer si la fonction [Désactiver le suivi des ouvertures](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-editor-v2-0-overview) est activée.
* Nous avons ajouté deux points d’entrée qui vous permettent de gérer de manière sélective les balises de programme. Le point d’entrée [Mettre à jour les balises du programme](/help/rest-api/programs.md) vous permet de mettre à jour une balise de programme de manière sélective. Le point d’entrée [Supprimer les balises de programme](/help/rest-api/programs.md) vous permet de supprimer une balise de programme de manière sélective.
* Nous avons ajouté le paramètre **isExecutable** au point d’entrée [Cloner une campagne intelligente](/help/rest-api/smart-campaigns.md). Ce paramètre vous permet de cloner un programme en tant que programme exécutable.
* Le champ **headStart** a été ajouté à l’[API de programmes](/help/rest-api/programs.md). Vous pouvez ainsi créer, mettre à jour et récupérer le paramètre [Démarrage rapide](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/head-start-for-email-programs) pour les programmes de messagerie électronique.

### Résolutions des défauts

* Correction d’un problème lié au point d’entrée [Obtenir le contenu dynamique de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET). Lors de la tentative de récupération des lignes d’objet avec du contenu dynamique dans des e-mails qui avaient rompu les relations de modèle, une erreur 709 est renvoyée : « L’API autorise uniquement les opérations sur les e-mails avec un modèle ». Le point d’entrée renvoie désormais le contenu dynamique. [LM-152331]
* Correction d’un problème lié au point d’entrée [Leads de synchronisation](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST). Lorsque vous utilisez externalSalesPersonId pour associer un commercial à un prospect à l’aide de externalSalesPersonId et que vous utilisez action = createDuplicate, l’association du commercial n’a pas lieu. [LM-158990]

### Intégration Adobe IMS

* Ceux qui ont intégré [Adobe IMS](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) ne peuvent pas utiliser toutes les [API User Management de Marketo](/help/rest-api/user-management.md). Les points d’entrée suivants renvoient une erreur lors de l’appel sur les instances Marketo qui ont été intégrées à Adobe IMS : [Inviter un utilisateur](https://developer.adobe.com/marketo-apis/api/user/#operation/inviteUserUsingPOST), [Obtenir l’utilisateur invité par ID](https://developer.adobe.com/marketo-apis/api/user/#operation/getInvitedUserUsingGET), [Mettre à jour les attributs d’utilisateur](https://developer.adobe.com/marketo-apis/api/user/#operation/updateUserAttributeUsingPOST), [Supprimer l’utilisateur](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST) et [Supprimer l’utilisateur invité](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteInvitedUserUsingPOST). En remplacement, les [API User Management d’Adobe](https://developer.adobe.com/umapi/) doivent être utilisées.

Publié le _2022-03-14_ par _David_

## Mises à jour de mai 2022-

En mai 2022, nous améliorerons les API REST existantes et résoudrons plusieurs défauts. Consultez la liste complète des mises à jour ci-dessous.

* Nous avons ajouté la possibilité de récupérer les enregistrements [Société](/help/rest-api/companies.md), [Opportunité](/help/rest-api/opportunities.md) et [Vendeurs](/help/rest-api/sales-persons.md) lorsque la [Synchronisation de SFDC](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) ou la [Synchronisation de Microsoft Dynamics](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) sont activées dans votre instance Marketo Engage.
* Nous avons mis à jour le point d’entrée [Obtenir le contenu dynamique de l’e-mail](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET) pour vous permettre de récupérer [Contenu dynamique](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/using-dynamic-content-in-an-email) à partir de l’objet d’un e-mail. Cela fonctionne que l’e-mail donné soit ou non lié à un modèle d’e-mail.

`POST /rest/asset/v1/form/{id}/field/State.json?values=[{"label":"Alaska"},{"value":"AK"},{"label":"West Virginia","value":"WV"},{"label":"Wyoming","value":"WY"}]`

* Nous avons mis à jour le point d’entrée [Ajouter des règles de visibilité des champs de formulaire](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllProgramMemberFieldsUsingGET) afin de vous permettre d’ajouter plusieurs valeurs de comparaison pour le type **isNot** [Règles d’invisibilité](/help/rest-api/forms.md). Voici un exemple :

`POST /rest/asset/v1/form/{id}/field/LastName/visibility.json?visibilityRule={"ruleType":"show","rules":[{"subjectField":"LastName","operator":"isNot","values":["A","B","C"]}`

### Résolutions des défauts

* Correction d’un problème lié au point d’entrée [Envoyer le formulaire](/help/rest-api/leads.md) qui se produisait lors de la transmission de la valeur « null » pour un attribut dans le paramètre [leadFormFields](/help/rest-api/leads.md). Une erreur « 611, Erreur système » a été renvoyée. Elle renvoie désormais correctement l’erreur « 1003, Échec de la validation du formulaire ». [LM-162213]

Publié le _2022-05-09_ par _David_

## Mises À Jour D’Août 2022

En août 2022, nous améliorerons les API REST existantes. Consultez la liste complète des mises à jour ci-dessous.

Nous avons ajouté plusieurs nouveaux filtres qui peuvent être utilisés lors de l’appel du point d’entrée Créer une tâche membre du programme d’exportation . Notez que de nombreux filtres peuvent être utilisés en combinaison les uns avec les autres pour affiner le jeu de données extrait.

* Le filtre **programIds** peut être utilisé pour spécifier jusqu’à 10 identifiants de programme, ce qui peut améliorer le débit.
* Le filtre **isExhausted** peut être utilisé pour filtrer les enregistrements pour les [personnes qui ont épuisé le contenu](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content).
* Le filtre **nurtureCadence** peut être utilisé pour filtrer les enregistrements en fonction du [rythme du programme d’engagement](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/program-flow-actions/change-engagement-program-cadence).
* Le filtre **statusNames** peut être utilisé pour filtrer les enregistrements pour un ou plusieurs statuts de [&#x200B; programme](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/understanding-program-membership).
* Le filtre **updatedAt** peut être utilisé pour filtrer les enregistrements en fonction d’une période.

### Annonces

* Le comportement du point d’entrée [Identity](https://developer.adobe.com/marketo-apis/api/identity/#operation/identityUsingGET) a changé. Lorsque vous appelez le point d’entrée et n’incluez pas de paramètre **access_token**, l’erreur « 603, Accès refusé » est renvoyée. Auparavant, l’erreur « 600, Jeton d’accès vide » était renvoyée. Notez que l’erreur « 600, Jeton d’accès vide » a été abandonnée.

Publié le _2022-09-03_ par _David_

## Mises À Jour D’Octobre 2022

En octobre 2022, nous améliorerons les API REST existantes. Consultez la liste complète des mises à jour ci-dessous.

* Nous avons amélioré l’[API d’importation de leads en bloc](/help/rest-api/bulk-lead-import.md) afin de prendre en charge l’ajout de leads aux enregistrements de commerciaux pendant le processus d’importation. Pour ce faire, incluez le champ **externalSalesPersonId** dans le fichier d’importation.
* Correction d’un problème lié au point d’entrée [Créer des champs de prospect](/help/rest-api/leads.md) qui se produisait lors de la création de champs de type Score. Ces champs n’étaient pas disponibles pour l’utilisation dans l’action de flux [&#x200B; Modifier le score &#x200B;](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/change-score) dans l’interface utilisateur de Marketo Engage. [LM-166815]

### Annonces

* L’attribut `acquiredBy` de l’appartenance au programme a été modifié pour pouvoir être mis à jour.

Publié le _2022-10-18_ par _David_

## Modification à venir de l’API REST Marketo Forms

À compter de la version 2022.R2, actuellement prévue pour la semaine du 24 mars 2023, les API de ressources Forms Adobe Marketo Engage renverront systématiquement uniquement le nom du formulaire sans nom de programme préfixé, que le formulaire soit un enfant d’un programme ou non. Cette modification rendra les comportements de l’API Forms en ce qui concerne les noms de ressources cohérents avec le reste des API de ressources Adobe Marketo Engage. Pour éviter toute interruption de service, vous devez consulter toutes les intégrations qui utilisent les API Marketo Engage Forms et travailler avec vos intégrateurs pour voir si des modifications sont nécessaires pour répondre à cette situation. Avant cette modification à venir, les noms renvoyés par les API Forms avaient un préfixe incohérent dans le nom d’un programme pour les formulaires qui sont des enfants de programmes dans les activités marketing. Par exemple, un formulaire nommé « Formulaire 1 », qui était un enfant de « Programme 1 », peut avoir son nom renvoyé par l’API comme suit : Programme 1.Formulaire 1 Ou Formulaire 1 À partir de la version 2022.R2, le nom d’un formulaire sera toujours renvoyé sans préfixe de nom de programme. Dans le même exemple, le nom serait toujours : Formulaire 1

Publié le _2022-11-04_ par _Kenny_

## Mises À Jour De Janvier 2023

En janvier 2023, nous avons apporté une amélioration liée à l’API à l’interface utilisateur d’administration et nous faisons deux annonces. Consultez la liste complète des mises à jour ci-dessous.

Interface utilisateur d’administration

### Extrait de lead en masse

* Nous avons amélioré l’interface utilisateur d’administration de Marketo Engage pour vous permettre d’afficher l’attribution de capacité quotidienne de l’API d’extraction en bloc pour votre abonnement. En outre, vous pouvez afficher l’utilisation de la capacité par API-User au cours des 7 derniers jours. Vous trouverez plus d’informations [ici](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/settings/bulk-export-api-information).

### Résolutions des défauts

* Correction d’un problème lié au point d’entrée [Supprimer des opportunités](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST). Dans certains cas, une activité « Supprimer de l’opportunité » n’était pas générée. [LM-172208]

### Annonces

* Consultez [cet article](https://nation.marketo.com/t5/product-documents/upcoming-change-to-marketo-rest-api/ta-p/331698) sur la communauté Marketo concernant l’API REST et une modification du message de réponse HTTP [phrase de raison](https://www.rfc-editor.org/rfc/rfc7230#section-3.1.2).
* L’attribut d’appartenance au programme **statusReason** a été modifié pour pouvoir être mis à jour.

Publié le _2023-01-21_ par _David_
