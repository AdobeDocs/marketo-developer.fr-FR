---
title: syncLead
feature: SOAP
description: appels SOAP syncLead
exl-id: e6cda794-a9d4-4153-a5f3-52e97a506807
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 2%

---

# syncLead

Cette fonction insère ou met à jour un seul enregistrement de prospect. Lors de la mise à jour d’un prospect existant, celui-ci est identifié par l’une des clés suivantes :

- ID de Marketo
- Identifiant du système étranger (implémenté en tant que `foreignSysPersonId`)
- Cookie Marketo (créé par le script JS Munchkin)
- E-mail

Si une correspondance existante est trouvée, l’appel effectue une mise à jour. Sinon, il insère et crée un prospect. Les leads anonymes peuvent être mis à jour à l’aide de l’identifiant de cookie Marketo et seront connus lors de la mise à jour.

À l’exception des e-mails, tous ces identifiants sont traités comme des clés uniques. L’ID Marketo est prioritaire sur toutes les autres clés. Si les `foreignSysPersonId` et l’ID Marketo sont tous deux présents dans l’enregistrement de prospect, l’ID Marketo est prioritaire et l’`foreignSysPersonId` est mis à jour pour ce prospect. Si la seule `foreignSysPersonId` est donnée, elle est utilisée comme identifiant unique. Si les `foreignSysPersonId` et l’e-mail sont présents mais que l’ID Marketo n’est pas présent, l’`foreignSysPersonId` est prioritaire et l’e-mail sera mis à jour pour ce prospect.

Vous pouvez éventuellement spécifier un en-tête contextuel pour nommer l’espace de travail cible.

Lorsque les espaces de travail Marketo sont activés et que l’en-tête est utilisé, les règles suivantes sont appliquées :

- Si des règles d’affectation sont définies et qu’un nouveau prospect est admissible pour l’une des règles configurées, de nouveaux prospects sont créés dans la partition définie par la règle d’affectation. Dans le cas contraire, de nouveaux prospects sont créés dans la partition principale de l’espace de travail nommé.
- Les leads associés par l’ID de lead Marketo, un ID de système étranger ou un cookie Marketo doivent exister dans la partition principale de l’espace de travail nommé. Dans le cas contraire, une erreur est renvoyée
- Si un prospect existant est mis en correspondance par e-mail, l’espace de travail nommé est ignoré et le prospect est mis à jour dans sa partition actuelle

Lorsque les espaces de travail Marketo sont activés et que l’en-tête n’est PAS utilisé, les règles suivantes sont appliquées :

- Si des règles d’affectation sont définies et qu’un nouveau prospect est admissible pour l’une des règles configurées, de nouveaux prospects sont créés dans la partition définie par la règle d’affectation. Dans le cas contraire, de nouveaux prospects sont créés dans la partition principale de l’espace de travail « Par défaut ».
- Les prospects existants sont mis à jour dans leur partition actuelle

Si les espaces de travail Marketo NE sont PAS activés, l’espace de travail cible DOIT être l’espace de travail « Par défaut ». Il n’est pas nécessaire de transmettre l’en-tête .

## Requête

| Nom du champ | Obligatoire / Facultatif | Description |
| --- | --- | --- |
| leadRecord->Id | Obligatoire - Uniquement en l’absence d’e-mail ou de `foreignSysPersonId` | Identifiant Marketo de l’enregistrement du prospect |
| leadRecord->E-mail | Obligatoire - Uniquement lorsque l’ID ou l’`foreignSysPersonId` n’est pas présent | Adresse e-mail associée à l’enregistrement de prospect |
| leadRecord->`foreignSysPersonId` | Obligatoire - Uniquement en l’absence d’ID ou d’e-mail | ID système étranger associé à l’enregistrement de prospect |
| leadRecord->foreignSysType | Facultatif - Requis uniquement en présence de l’`foreignSysPersonId` | Type de système étranger. Valeurs possibles : CUSTOM, SFDC, NETSUITE |
| leadRecord->leadAttributeList->attribute->attrName | Obligatoire | Nom de l’attribut de prospect dont vous souhaitez mettre à jour la valeur. |
| leadRecord->leadAttributeList->attribute->attrValue | Obligatoire | Valeur à définir sur l’attribut de prospect spécifié dans attrName. |
| returnLead | Obligatoire | Lorsque la valeur est true, renvoie l’enregistrement de prospect complet mis à jour lors de la mise à jour. |
| marketoCookie | Facultatif | Le cookie [Munchkin javascript](../javascript-api/lead-tracking.md) |

## XML de la demande

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://www.marketo.com/mktows/">
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
        <Email>t@t.com</Email>
        <leadAttributeList>
          <attribute>
            <attrName>FirstName</attrName>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <returnLead>false</returnLead>
    </ns1:paramsSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## XML de réponse

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncLead>
      <result>
        <leadId>1089965</leadId>
        <syncStatus>
          <leadId>1089965</leadId>
          <status>UPDATED</status>
          <error xsi:nil="true" />
        </syncStatus>
        <leadRecord xsi:nil="true" />
      </result>
    </ns1:successSyncLead>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Exemple de code - PHP

```php
 <?php

  $debug = true;

  $marketoSoapEndPoint     = "";  // CHANGE ME
  $marketoUserId           = "";  // CHANGE ME
  $marketoSecretKey        = "";  // CHANGE ME
  $marketoNameSpace        = "http://www.marketo.com/mktows/";

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
  $options = array("connection_timeout" =20, "location" =$marketoSoapEndPoint);
  if ($debug) {
    $options["trace"] = true;
  }

  // Create Request
  $leadKey = new stdClass();
  $leadKey->Email = "george@jungle.com";

  // Lead attributes to update
  $attr1 = new stdClass();
  $attr1->attrName  = "FirstName";
  $attr1->attrValue = "George";

  $attr2= new stdClass();
  $attr2->attrName  = "LastName";
  $attr2->attrValue = "of the Jungle";

  $attrArray = array($attr1, $attr2);
  $attrList = new stdClass();
  $attrList->attribute = $attrArray;
  $leadKey->leadAttributeList = $attrList;

  $leadRecord = new stdClass();
  $leadRecord->leadRecord = $leadKey;
  $leadRecord->returnLead = false;
  $params = array("paramsSyncLead" =$leadRecord);

  $soapClient = new SoapClient($marketoSoapEndPoint ."?WSDL", $options);
  try {
    $result = $soapClient->__soapCall('syncLead', $params, $options, $authHdr);
  }
  catch(Exception $ex) {
    var_dump($ex);
  }

  if ($debug) {
    print "RAW request:\n" .$soapClient->__getLastRequest() ."\n";
    print "RAW response:\n" .$soapClient->__getLastResponse() ."\n";
  }
  print_r($result);

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
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;


public class SyncLead {

    public static void main(String[] args) {
        System.out.println("Executing syncLead");
        try {
            URL marketoSoapEndPoint = new URL("https://100-AEK-913.mktoapi.com/soap/mktows/2_1" + "?WSDL");
            String marketoUserId = "demo17_1_809934544BFABAE58E5D27";
            String marketoSecretKey = "27272727aa";

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
            ParamsSyncLead request = new ParamsSyncLead();
            LeadRecord key = new LeadRecord();

            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Stringemail = objectFactory.createLeadRecordEmail("george@jungle.com");
            key.setEmail(email);
            request.setLeadRecord(key);

            Attribute attr1 = new Attribute();
            attr1.setAttrName("FirstName");
            attr1.setAttrValue("George2");

            Attribute attr2 = new Attribute();
            attr2.setAttrName("LastName");
            attr2.setAttrValue("of the Jungle");

            ArrayOfAttribute aoa = new ArrayOfAttribute();
            aoa.getAttributes().add(attr1);
            aoa.getAttributes().add(attr2);

            QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
            JAXBElement<ArrayOfAttributeattrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);
            key.setLeadAttributeList(attrList);

            MktowsContextHeader headerContext = new MktowsContextHeader();
            headerContext.setTargetWorkspace("default");

            SuccessSyncLead result = port.syncLead(request, header, headerContext);

            JAXBContext context = JAXBContext.newInstance(SuccessSyncLead.class);
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
    'ns1:AuthenticationHeader' ={ "mktowsUserId" =mktowsUserId, "requestSignature" =requestSignature,
    "requestTimestamp"  =requestTimestamp
    }
}

client = Savon.client(wsdl: 'http://app.marketo.com/soap/mktows/2_3?WSDL', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

#Create Request
request = {
    :lead_record ={
        :Email ="t@t.com",
          :lead_attribute_list ={
              :attribute ={
                :attr_name ="FirstName",
                :attr_value ="George" },
              :attribute! ={
                :attr_name ="LastName",
                :attr_value ="of the Jungle" }
        }
    },
    :return_lead ="false"
}

response = client.call(:sync_lead, message: request)

puts response
```
