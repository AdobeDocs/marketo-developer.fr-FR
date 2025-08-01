---
title: getLeadChanges
feature: SOAP
description: appels SOAP getLeadChanges
exl-id: 23445684-d8d9-407b-8f19-cb69e806795c
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 3%

---

# getLeadChanges

Cette API est similaire à la `getLeadActivity`, sauf qu’elle fonctionne sur plusieurs prospects à la fois. L’opération recherche les nouveaux prospects créés, les mises à jour des champs de prospect et d’autres activités.

Le résultat contient des activités qui ont provoqué la modification ainsi qu’une [position du flux](stream-position.md) pour paginer dans des jeux de résultats volumineux.

Vous devez inclure un paramètre d’entrée identifiant les filtres d’activité que vous souhaitez renvoyer dans le résultat. Si vous souhaitez que toutes les activités soient effectuées, une valeur vide peut être transmise. Pour plusieurs filtres d’activité, transmettez une liste de filtres d’activité.

Voici quelques exemples de types d’activité : « Visiter la page web », « Remplir le formulaire » et « Cliquer sur le lien ».

Après la version 2_2 de l’API SOAP, vous pouvez inclure un `leadSelector`.

Par `LastUpdateAtSelector`, la valeur `oldestUpdatedAt` correspond à la valeur `oldestCreatedAt` dans le `startPosition` . Et la valeur `latestUpdatedAt` correspondrait à la valeur `latestCreatedAt` dans le `startPosition`.

Remarque : le nombre maximal de prospects pris en charge dans un `LeadKeySelector` est de 100. Si le nombre de prospects dépasse 100, l’API renvoie une exception de paramètre erronée et renvoie une erreur SOAP.

## Requête

| Nom du champ | Obligatoire / Facultatif | Description |
| --- | --- | --- |
| activityFilter->includeAttributes->activityType | Facultatif (obsolète) Utilisez plutôt `activityNameFilter` | Limite la réponse pour inclure uniquement les types d’activités spécifiés. Voir WSDL pour tous les types d’activités. |
| activityFilter->excludeAttributes->activityType | Facultatif | Limite la réponse pour exclure les types d’activité spécifiés. Voir WSDL pour tous les types d’activités. REMARQUE : vous ne pouvez pas spécifier à la fois `includeAttributes` et `excludeAttributes` dans le même appel. |
| activityNameFilter | Facultatif | Limite la réponse pour n’inclure que les filtres d’activité spécifiés. |
| batchSize | Facultatif | Nombre maximum d’enregistrements à renvoyer. Système limité à 1 000 ou `batchSize`, le chiffre le moins élevé étant retenu. |
| startPosition | Obligatoire | Permet de paginer dans un grand nombre de réponses d’activité. |
| startPosition->décalage | Facultatif | La valeur de décalage est renvoyée par le champ de réponse d&#39;appels précédent newStartPosition->offset. |
| startPosition->plus ancienCrééÀ | Facultatif | La date et l’heure utilisées pour filtrer les résultats afin d’inclure uniquement les prospects créés depuis la création la plus ancienne de CreatedAt. REMARQUE : vous pouvez utiliser `LastUpdateAtSelector->oldestUpdatedAt` date et l’heure pour spécifier des `oldestCreatedAt`. |
| startPosition->activityCreatedAt | Facultatif | La date et l’heure utilisées pour filtrer les résultats afin d’inclure uniquement les prospects ayant une activité depuis activityCreatedAt. REMARQUE : vous pouvez utiliser `LastUpdateAtSelector->latestUpdatedAt` date et l’heure pour spécifier des `activityCreatedAt`. |
| leadSelector | Facultatif | Il peut s’agir de l’un des 3 types suivants : `LeadKeySelector`, `StaticListSelector`, `LastUpdateAtSelector` |
| LeadKeySelector : leadSelector->keyType | Obligatoire | Type d’identifiant que vous souhaitez interroger. Les valeurs incluent `IDNUM`, `COOKIE`, `EMAIL`, `LEADOWNEREMAIL`, `SFDCACCOUNTID`, `SFDCCONTACTID`, `SFDCLEADID`, `SFDCLEADOWNERID` et `SFDCOPPTYID`. |
| LeadKeySelector : leadSelector->keyValues->stringItem | Obligatoire | Liste des valeurs clés. C’est-à-dire « lead@email.com » |
| StaticListSelector : leadSelector->staticListName | Facultatif lorsque `leadSelector->staticListId` est présent | Nom de la liste statique |
| StaticListSelector : leadSelector->staticListId | Facultatif lorsque `leadSelector->staticListName` est présent | L’identifiant de la liste statique |

## XML de la demande

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>59e88471266a7495590941586ef6a96849ea8e11</requestSignature>
      <requestTimestamp>2013-08-02T17:22:53-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsGetLeadChanges>
      <startPosition>
        <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
      </startPosition>
      <activityNameFilter>
        <stringItem>Visit Webpage</stringItem>
        <stringItem>Click Link</stringItem>
      </activityNameFilter>
      <batchSize>10</batchSize>
    </ns1:paramsGetLeadChanges>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML de réponse

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successGetLeadChanges>
      <result>
        <returnCount>10</returnCount>
        <remainingCount>576</remainingCount>
        <newStartPosition>
          <latestCreatedAt xsi:nil="true" />
          <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
          <activityCreatedAt xsi:nil="true" />
          <offset>ID:1086173</offset>
        </newStartPosition>
        <leadChangeRecordList>
          <leadChangeRecord>
            <id>1037189</id>
            <activityDateTime>2013-08-01T00:00:09-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Lead Score</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>402</attrValue>
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>403</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Japanese form fillout.test time zone action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Step ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>19582</attrValue>
              </attribute>
              <attribute>
                <attrName>Choice Number</attrName>
                <attrType xsi:nil="true" />
                <attrValue>0</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>84105</attrValue>
              </attribute>
            </activityAttributes>
            <campaign>Japanese form fillout.test time zone</campaign>
            <mktPersonId>84105</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1037190</id>
            <activityDateTime>2013-08-01T02:37:17-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Marketo Social Facebook Reach</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Marketo Social Facebook Reach</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>574</attrValue>
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>571</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Form fill-out, URL: <unknown></attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1086478</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1086478</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1037193</id>
            <activityDateTime>2013-08-01T11:19:26-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Lead Score</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Visit any web page action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Step ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>11156</attrValue>
              </attribute>
              <attribute>
                <attrName>Choice Number</attrName>
                <attrType xsi:nil="true" />
                <attrValue>0</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090235</attrValue>
              </attribute>
            </activityAttributes>
            <campaign>Visit any web page</campaign>
            <mktPersonId>1090235</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1082831</id>
            <activityDateTime>2013-08-01T13:10:17-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>First Name</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>First Name</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>George</attrValue>
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Travis</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>SOAP API</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1089965</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1089965</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1082843</id>
            <activityDateTime>2013-08-01T13:10:19-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Last Name</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Last Name</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>of the Jungle</attrValue>
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Kaufman</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>SOAP API</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1089965</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1089965</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086153</id>
            <activityDateTime>2013-08-01T13:21:51-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Urgency</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Urgency</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>25</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Source</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead update</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086166</id>
            <activityDateTime>2013-08-01T13:21:53-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Relative Urgency</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Relative Urgency</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Source</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead update</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086167</id>
            <activityDateTime>2013-08-01T13:21:53-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Relative Score</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Relative Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Source</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead update</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086169</id>
            <activityDateTime>2013-08-01T13:21:53-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Priority</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Priority</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>70</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Source</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead update</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
          <leadChangeRecord>
            <id>1086173</id>
            <activityDateTime>2013-08-01T13:21:53-05:00</activityDateTime>
            <activityType>Change Data Value</activityType>
            <mktgAssetName>Lead Score</mktgAssetName>
            <activityAttributes>
              <attribute>
                <attrName>Attribute Name</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Lead Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Old Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue />
              </attribute>
              <attribute>
                <attrName>New Value</attrName>
                <attrType xsi:nil="true" />
                <attrValue>25</attrValue>
              </attribute>
              <attribute>
                <attrName>Reason</attrName>
                <attrType xsi:nil="true" />
                <attrValue>Changed by Smart Campaign Operational Campaign - ML action Change Score</attrValue>
              </attribute>
              <attribute>
                <attrName>Step ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>12259</attrValue>
              </attribute>
              <attribute>
                <attrName>Choice Number</attrName>
                <attrType xsi:nil="true" />
                <attrValue>0</attrValue>
              </attribute>
              <attribute>
                <attrName>Lead ID</attrName>
                <attrType xsi:nil="true" />
                <attrValue>1090236</attrValue>
              </attribute>
            </activityAttributes>
            <campaign>Operational Campaign - ML</campaign>
            <mktPersonId>1090236</mktPersonId>
          </leadChangeRecord>
        </leadChangeRecordList>
      </result>
    </ns1:successGetLeadChanges>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Exemple de code - PHP

```php
<?php
$debug = true;
$marketoSoapEndPoint    = "";  // CHANGE ME
$marketoUserId      = "";  // CHANGE ME
$marketoSecretKey   = ""; // CHANGE ME
$marketoNameSpace   = "http://www.marketo.com/mktows/";

// Create Signature
$dtzObj = new DateTimeZone("America/Los_Angeles");
$dtObj  = new DateTime('now', $dtzObj);
$timeStamp = $dtObj->format(DATE_W3C);
$encryptString = $timeStamp . $marketoUserId;
$signature = hash_hmac('sha1', $encryptString, $marketoSecretKey);

// Create SOAP Header
$attrs = new stdClass();
$attrs->mktowsUserId = $marketoUserId;
$attrs->requestSignature = $signature;
$attrs->requestTimestamp = $timeStamp;
$authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
$options = array("connection_timeout" => 600, "location" => $marketoSoapEndPoint);
if ($debug) {
  $options["trace"] = 1;
}

// Create Request
$params = new stdClass();
$filter = new stdClass();

$keyValues = array("Visit Webpage", "Click Link");
$filter->stringItem = $keyValues;
$params->activityNameFilter = $filter;

$streamPos = new stdClass();
$streamPos->oldestCreatedAt = "2013-08-01T00:13:13+00:00";
$params->startPosition = $streamPos;
$params->batchSize = 10;
$authHdr = new SoapHeader($marketoNameSpace, 'AuthenticationHeader', $attrs);
$client = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
$doPage = true;
while($doPage) {
  $success = $client->__soapCall('getLeadChanges', array($params), null, $authHdr);
  $result = $success->result;
  $doPage = ($result->remainingCount > 0);
  $streamPos = $result->newStartPosition;
  $params->startPosition = $streamPos;
  if ($debug) {
    print "RAW request:\n" .$client->__getLastRequest() ."\n";
    print "RAW response:\n" .$client->__getLastResponse() ."\n";
  }
}
return;
?>
```

## Exemple de code - Java

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.xml.datatype.XMLGregorianCalendar;
import javax.xml.datatype.DatatypeFactory;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import java.util.GregorianCalendar;


public class GetLeadChanges {

    public static void main(String[] args) {
        System.out.println("Executing Get Lead Changes");
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
            String encryptString = requestTimestamp + marketoUserId;

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
            ParamsGetLeadChanges request = new ParamsGetLeadChanges();

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> batchSize = objectFactory.createParamsGetLeadActivityBatchSize(10);
            request.setBatchSize(batchSize);

            ArrayOfString activities = new ArrayOfString();
            activities.getStringItems().add("Visit Webpage");
            activities.getStringItems().add("Click Link");

            JAXBElement<ArrayOfString> activityFilter = objectFactory.createParamsGetLeadChangesActivityNameFilter(activities);
            request.setActivityNameFilter(activityFilter);

            // Create oldestCreateAt timestamp from 5 days ago
            GregorianCalendar gc = new GregorianCalendar();
            gc.setTimeInMillis(new Date().getTime());
            gc.add( GregorianCalendar.DAY_OF_YEAR, -5);

            DatatypeFactory factory = DatatypeFactory.newInstance();
            JAXBElement<XMLGregorianCalendar> oldestCreateAtValue =objectFactory.createStreamPositionOldestCreatedAt(factory.newXMLGregorianCalendar(gc));

            StreamPosition sp = new StreamPosition();
            sp.setOldestCreatedAt(oldestCreateAtValue);
            request.setStartPosition(sp);

            SuccessGetLeadChanges result = port.getLeadChanges(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessGetLeadChanges.class);
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

## Exemple de code - Ruby

```ruby
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "http://www.marketo.com/mktows/"

#Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

#Create SOAP Header
headers = {
    'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,
    "requestTimestamp"  => requestTimestamp
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :start_position => {
        :oldest_created_at => "2013-08-01T00:13:13+00:00" },
    :activity_name_filter => {
        :stringItem => ["Visit Webpage", "Click Link"] },
    :batch_size => "10"
}

response = client.call(:get_lead_changes, message: request)

puts response
```
