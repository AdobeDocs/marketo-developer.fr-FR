---
title: getMultipleLeads
feature: SOAP
description: appels SOAP getMultipleLeads
exl-id: db9aabec-8705-40c6-b264-740fdcef8a52
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---

# getMultipleLeads

Comme `getLead`, `getMultipleLeads` récupère les enregistrements de prospect à partir de Marketo. Au lieu des données d’un prospect unique, cet appel renvoie les données d’un lot de prospects correspondant aux critères transmis dans le paramètre leadSelector. Les critères peuvent être une période, telle que la date de la dernière mise à jour, un tableau de clés de prospect ou une liste statique.

Remarque : si vous utilisez un tableau de clés de prospect, vous êtes limité à 100 par lot ; les clés supplémentaires seront ignorées.

Si seul un sous-ensemble des champs de prospect est requis, le paramètre `includeAttributes` doit être utilisé pour spécifier les champs souhaités.

Chaque appel de fonction `getMultipleLeads` renvoie jusqu’à 1 000 prospects. Si vous devez récupérer plus de 1 000 prospects, le résultat renvoie une [position du flux](stream-position.md), qui peut être utilisée dans les appels suivants pour récupérer le lot suivant de 1 000 prospects. Le nombre restant dans le résultat vous indique exactement combien de prospects restent. Lors de l’extraction à partir d’une liste statique, la condition d’arrêt est remainingCount == 0.

Un cas d’utilisation courant de ce point d’entrée consiste à rechercher les prospects qui ont été mis à jour à des dates spécifiques. Le `LastUpdateAtSelector` vous permet de le faire.

## Requête

| Nom du champ | Obligatoire / Facultatif | Description |
| --- | --- | --- |
| leadSelector | Obligatoire | Il peut s’agir de l’un des 3 types suivants : `LeadKeySelector`, `StaticListSelector`, `LastUpdateAtSelector` |
| keyType | Obligatoire | Type d’identifiant que vous souhaitez interroger. Les valeurs incluent IDNUM, COOKIE, EMAIL, LEADOWNEREMAIL, SFDCACCOUNTID, SFDCCONTACTID, SFDCLEADID, SFDCLEADOWNERID, SFDCOPPTYID. |
| keyValues->stringItem | Obligatoire | Liste des valeurs clés. C’est-à-dire « <lead@email.com> » |
| LastUpdateAtSelector : leadSelector->plusAncienMisÀJourAu | Obligatoire | Horodatage utilisé pour spécifier le critère « depuis ». En d’autres termes, renvoyez tous les prospects mis à jour depuis l’heure spécifiée. (format date-heure WSDL W3C) |
| LastUpdateAtSelector : leadSelector->latestUpdatedAt | Facultatif | Horodatage utilisé pour spécifier le critère « jusqu’au ». En d’autres termes, renvoyez tous les prospects mis à jour jusqu’à l’heure spécifiée. (format date-heure WSDL W3C) |
| StaticListSelector : leadSelector->staticListName | Facultatif lorsque `leadSelector->staticListId` est présent | Nom de la liste statique |
| StaticListSelector : leadSelector->staticListId | Facultatif lorsque `leadSelector->staticListName` est présent | L’identifiant de la liste statique |
| lastUpdatedAt | **Obsolète** | Utilisez plutôt `LastUpdateAtSelector` . |
| includeAttributes | Facultatif | Liste des attributs que vous souhaitez récupérer. La limitation des champs de prospect renvoyés peut améliorer le temps de réponse de l’API. |
| batchSize | Facultatif | Nombre maximum d’enregistrements à renvoyer. Le système est limité à 100 ou `batchSize`, la valeur la moins élevée étant retenue |
| streamPosition | Facultatif | Utilisé pour paginer dans un grand nombre de réponses de prospect. La valeur `streamPosition` est renvoyée par le champ de réponse des appels précédents `newStreamPosition` |

## XML de la demande

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>146ec93f4f1e2a45f7a545f7e42e2d053c0457e7</requestSignature>
      <requestTimestamp>2013-08-02T15:47:06-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsGetMultipleLeads>
      <leadSelector xsi:type="ns1:LeadKeySelector">
        <keyType>EMAIL</keyType>
        <keyValues>
          <stringItem>formtest1@marketo.com</stringItem>
          <stringItem>joe@marketo.com</stringItem>
        </keyValues>
      </leadSelector>
      <batchSize>100</batchSize>
    </ns1:paramsGetMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML de réponse

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successGetMultipleLeads>
      <result>
        <returnCount>2</returnCount>
        <remainingCount>0</remainingCount>
        <newStreamPosition />
        <leadRecordList>
          <leadRecord>
            <Id>65835</Id>
            <Email>formtest1@marketo.com</Email>
            <ForeignSysPersonId xsi:nil="true" />
            <ForeignSysType xsi:nil="true" />
            <leadAttributeList>
              <attribute>
                <attrName>Company</attrName>
                <attrType>string</attrType>
                <attrValue>Marketo Test</attrValue>
              </attribute>
              <attribute>
                <attrName>FirstName</attrName>
                <attrType>string</attrType>
                <attrValue>FnameForm1</attrValue>
              </attribute>
              <attribute>
                <attrName>LastName</attrName>
                <attrType>string</attrType>
                <attrValue>LnameForm1</attrValue>
              </attribute>
              <attribute>
                <attrName>LeadScore</attrName>
                <attrType>integer</attrType>
                <attrValue>16</attrValue>
              </attribute>
              <attribute>
                <attrName>Website</attrName>
                <attrType>url</attrType>
                <attrValue>marketo.com</attrValue>
              </attribute>
            </leadAttributeList>
          </leadRecord>
          <leadRecord>
            <Id>67508</Id>
            <Email>joe@marketo.com</Email>
            <ForeignSysPersonId xsi:nil="true" />
            <ForeignSysType xsi:nil="true" />
            <leadAttributeList>
              <attribute>
                <attrName>Company</attrName>
                <attrType>string</attrType>
                <attrValue>Marketo</attrValue>
              </attribute>
              <attribute>
                <attrName>Date_of_purchase</attrName>
                <attrType>date</attrType>
                <attrValue>2012-04-04</attrValue>
              </attribute>
              <attribute>
                <attrName>FirstName</attrName>
                <attrType>string</attrType>
                <attrValue>Joe</attrValue>
              </attribute>
              <attribute>
                <attrName>LastName</attrName>
                <attrType>string</attrType>
                <attrValue>Orante</attrValue>
              </attribute>
              <attribute>
                <attrName>LeadScore</attrName>
                <attrType>integer</attrType>
                <attrValue>11</attrValue>
              </attribute>
            </leadAttributeList>
          </leadRecord>
        </leadRecordList>
      </result>
    </ns1:successGetMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Exemple de code - PHP

```php
<?php
$debug = true;
$marketoSoapEndPoint    = "";  // CHANGE ME
$marketoUserId      = "";  // CHANGE ME
$marketoSecretKey   = "";  // CHANGE ME
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
$options = array("connection_timeout" => 15, "location" => $marketoSoapEndPoint);
if ($debug) {
  $options["trace"] = 1;
}

// Create Request
/*
//Query by Email
$leadSel = new stdClass();
$leadSel->keyType = 'EMAIL';
$keyValues = array("formtest1@marketo.com", "joe@marketo.com");
$leadKeys = new stdClass();
$leadKeys->stringItem = $keyValues;
$leadSel->keyValues = $leadKeys;
$leadSelSoap = new stdClass();
$leadSelSoap = array("leadSelector" => $leadSel);
// $leadSelParams = array("leadSelector" => $leadSelSoap, "batchSize" => 10, "streamPosition" => $startPosition);
// $params = array("paramsGetMultipleLeads" => $leadSelParams);
$leadSelSoap = new SoapVar($leadSel, SOAP_ENC_OBJECT, "LeadKeySelector", "http://www.marketo.com/mktows/");
*/

/*
// Query by Update time
$leadSel = new stdClass();
$leadSel->latestUpdatedAt = "2013-08-06T15:45:00-07:00";
$leadSel->oldestUpdatedAt = "2013-08-06T12:00:00-07:00";
$leadSelSoap = new stdClass();
$leadSelSoap = array("leadSelector" => $leadSel);
$leadSelSoap = new SoapVar($leadSel, SOAP_ENC_OBJECT, "LastUpdateAtSelector", "http://www.marketo.com/mktows/");
*/

// Query from a Static List
$leadSel = new stdClass();
//ProgramName.ListName
$leadSel->staticListName = "SMSProgram.listForTesting";
$leadSelSoap = new stdClass();
$leadSelSoap = array("leadSelector" => $leadSel);
$leadSelSoap = new SoapVar($leadSel, SOAP_ENC_OBJECT, "StaticListSelector", "http://www.marketo.com/mktows/");

$params->leadSelector = $leadSelSoap;
$params->streamPosition = $startPosition;
$params->batchSize = 100;
$soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
try {
  $leads = $soapClient->__soapCall('getMultipleLeads', array($params), $options, $authHdr);
}
catch(Exception $ex) {
  var_dump($ex);
}
if ($debug) {
  print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
  print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
}
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
            ParamsGetMultipleLeads request = new ParamsGetMultipleLeads();

            // Request Using LeadKey Selector
            ////////////////////////////////////////////////////////
            LeadKeySelector keySelector = new LeadKeySelector();
            keySelector.setKeyType(LeadKeyRef.EMAIL);

            ArrayOfString aos = new ArrayOfString();
            aos.getStringItems().add("formtest1@marketo.com");
            aos.getStringItems().add("joe@marketo.com");
            keySelector.setKeyValues(aos);
            request.setLeadSelector(keySelector);

            /*
            // Request Using LastUpdateAtSelector
            ////////////////////////////////////////////////////////
            LastUpdateAtSelector leadSelector = new LastUpdateAtSelector();

            GregorianCalendar gc = new GregorianCalendar();
            gc.setTimeInMillis(new Date().getTime());
            gc.add( GregorianCalendar.DAY_OF_YEAR, -2);

            DatatypeFactory factory = DatatypeFactory.newInstance();

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<XMLGregorianCalendar> until =objectFactory.createLastUpdateAtSelectorLatestUpdatedAt(factory.newXMLGregorianCalendar(gc));

            GregorianCalendar since = new GregorianCalendar();
            since.setTimeInMillis(new Date().getTime());
            since.add( GregorianCalendar.DAY_OF_YEAR, -5);

            leadSelector.setOldestUpdatedAt(factory.newXMLGregorianCalendar(since));
            leadSelector.setLatestUpdatedAt(until);

            request.setLeadSelector(leadSelector);
            */

            /*
            // Request Using StaticList Selector
            ////////////////////////////////////////////////////////
            StaticListSelector staticListSelector = new StaticListSelector();

            //staticListSelector.setStaticListId(value)
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<String> listName = objectFactory.createStaticListSelectorStaticListName("SMSProgram.listForTesting");
            staticListSelector.setStaticListName(listName);

            // JAXBElement<Integer> listId = objectFactory.createStaticListSelectorStaticListId(6926);
            // staticListSelector.setStaticListId(listId);

            request.setLeadSelector(staticListSelector);
            */


            ArrayOfString attributes = new ArrayOfString();
            attributes.getStringItems().add("FirstName");
            attributes.getStringItems().add("AnonymousIP");
            attributes.getStringItems().add("Company");

            request.setIncludeAttributes(attributes);

            JAXBElement<Integer> batchSize = new ObjectFactory().createParamsGetMultipleLeadsBatchSize(10);
            request.setBatchSize(batchSize);

            SuccessGetMultipleLeads result = port.getMultipleLeads(request, header);

            JAXBContext context = JAXBContext.newInstance(SuccessGetLead.class);
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
    :lead_selector => {
        :key_type => "EMAIL",
        :key_values => {
            :string_item => ["formtest1@marketo.com", "joe@marketo.com"]
        }

    },
    :batch_size => "100"
}

response = client.call(:get_multiple_leads, message: request)

puts response
```
