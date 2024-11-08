---
title: Signature d’authentification
feature: SOAP
description: Sécurité des API avec des signatures d’authentification
exl-id: d6bed8ee-77fa-440c-8f35-a71cf77f45d3
source-git-commit: 28b040f6473677abaaa0c73f1bb6e887e9e35a81
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 4%

---

# Signature d’authentification

La sécurité de l’API Marketo utilise un modèle simple mais hautement sécurisé, basé sur les signatures HMAC-SHA1 avec les messages transmis via HTTPS. L’un des principaux avantages de ce modèle est qu’il fournit une authentification sans état.

Les signatures HMAC-SHA1 nécessitent les éléments suivants :

- Identifiant utilisateur (également appelé clé d’accès) transmis avec la demande de service
- Une signature calculée à l’aide d’une clé secrète partagée et d’un contenu de message transmis avec la demande de service
- Clé secrète partagée (également appelée clé de chiffrement) qui n’est pas transmise avec la demande de service

Le programme client calcule la signature HMAC-SHA1 à l’aide de la clé secrète partagée et d’une partie du contenu du message de demande. Le client doit inclure un en-tête SOAP, AuthenticationHeaderInfo, pour transmettre des informations d’authentification avec le message SOAP.

Le pseudo-code suivant illustre l’algorithme :

```javascript
// Request timestamp: a timestamp string in W3C WSDL date format
stringToEncrypt = requestTimestamp + clientAccessID;

signatureBytes = Encryter.encrypt('SHA1', secretKey, stringToEncrypt);

signature = toLowerCase( hexEncode(signatureBytes) );

authHeader = "<ns1:AuthenticationHeader>" +
    "<mktowsUserId>" + clientAccessID + "</mktowsUserId>" +
    "<requestSignature>" + signature + "</requestSignature>" +
    "<requestTimestamp>" + requestTimestamp + "</requestTimestamp>"
    "</ns1:AuthenticationHeader>";
```

## En-tête de requête

| Nom du champ | Obligatoire / Facultatif | Description |
| --- | --- | --- |
| `mktowsUserId` | Obligatoire | L’ID d’accès du client Marketo se trouve dans le panneau de l’API d’administration Marketo SOAP sous Intégration. |
| `requestSignature` | Obligatoire | Signature HMAC-SHA1 basée sur la clé secrète partagée `requestTimestamp` et l’identifiant utilisateur Marketo |
| `requestTimestamp` | Obligatoire | Demander l’horodatage (format de date WSDL W3C Ex. &quot;2013-06-09T14:04:54-08:00&quot;) |
| `partnerId` | Facultatif | [Clé API](../launchpoint-api.pdf) du partenaire technologique LaunchPoint. |

## Request XML - getLeadActivity

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:mkt="http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>mktodemoaccount881_536240405411DF5316D5C9</mktowsUserId>
         <requestSignature>3f4b21eb586063dc65774a2733713cac342e9c81</requestSignature>
         <requestTimestamp>2017-03-09T17:40:00-08:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsGetLeadActivity>
         <leadKey>
            <keyType>IDNUM</keyType>
            <keyValue>318815</keyValue>
         </leadKey>
         <activityFilter>
               <includeTypes>
                   <activityType>NewLead</activityType>
               </includeTypes>
         </activityFilter>
        </mkt:paramsGetLeadActivity>
   </soapenv:Body>
</soapenv:Envelope>
```

## XML de réponse - Succès

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successGetLeadActivity>
         <leadActivityList>
            <returnCount>1</returnCount>
            <remainingCount>0</remainingCount>
            <newStartPosition>
               <latestCreatedAt xsi:nil="true"/>
               <oldestCreatedAt xsi:nil="true"/>
               <activityCreatedAt xsi:nil="true"/>
               <offset>1</offset>
            </newStartPosition>
            <activityRecordList>
               <activityRecord>
                  <id>12025714</id>
                  <activityDateTime>2015-09-04T16:07:16-05:00</activityDateTime>
                  <activityType>New Lead</activityType>
                  <mktgAssetName/>
                  <activityAttributes>
                     <attribute>
                        <attrName>Source Type</attrName>
                        <attrType xsi:nil="true"/>
                        <attrValue>Web service API</attrValue>
                     </attribute>
                     <attribute>
                        <attrName>Source Info</attrName>
                        <attrType xsi:nil="true"/>
                        <attrValue>Web service API</attrValue>
                     </attribute>
                     <attribute>
                        <attrName>Created Date</attrName>
                        <attrType xsi:nil="true"/>
                        <attrValue>2015-09-04</attrValue>
                     </attribute>
                     <attribute>
                        <attrName>Lead ID</attrName>
                        <attrType xsi:nil="true"/>
                        <attrValue>318815</attrValue>
                     </attribute>
                  </activityAttributes>
                  <campaign/>
                  <personName xsi:nil="true"/>
                  <mktPersonId>318815</mktPersonId>
                  <foreignSysId xsi:nil="true"/>
                  <orgName xsi:nil="true"/>
                  <foreignSysOrgId xsi:nil="true"/>
               </activityRecord>
            </activityRecordList>
         </leadActivityList>
      </ns1:successGetLeadActivity>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope
```

## Réponse XML - Échec (informations d’identification non valides)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
  <SOAP-ENV:Body>
    <SOAP-ENV:Fault>
      <faultcode>SOAP-ENV:Client</faultcode>
      <faultstring>20014 - Authentication failed</faultstring>
      <detail>
        <ns1:serviceException xmlns:ns1="http://www.marketo.com/mktows/">
          <name>mktServiceException</name>
          <message>Authentication failed (20014)</message>
          <code>20014</code>
        </ns1:serviceException>
      </detail>
    </SOAP-ENV:Fault>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Exemple de code - PHP

```php
<?php
 
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
 
  print_r($authHdr)
 
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
import javax.xml.bind.Marshaller;
 
public class AuthenticationHeader {
 
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
 
            JAXBContext context = JAXBContext.newInstance(AuthenticationHeader.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(header, System.out);
 
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
```
