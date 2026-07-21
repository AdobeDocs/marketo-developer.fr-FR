---
title: Mode d’accès de sécurité avancé
feature: Mobile Marketing
description: Découvrez le mode d’accès de sécurité avancé pour Marketo Mobile SDK, avec la génération de signatures HMAC, la configuration des points d’entrée du serveur, l’utilisation des identifiants d’appareil et des exemples iOS et Android
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
TQID: https://experienceleague.adobe.com/F6lH1aGbCakK-E6IU4wLwYw58BG2-CRE-Ras2bMHeO8
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 217
ht-degree: 1%

---

# Mode d’accès de sécurité avancé

Le mode d’accès de sécurité avancé nécessite que le SDK Marketo récupère et définisse une signature de sécurité. Le SDK fournit des méthodes pour définir et supprimer la signature ainsi qu’une méthode utilitaire pour récupérer l’ID de l’appareil.

Lors de la connexion, envoyez l’ID d’appareil et l’adresse e-mail au serveur client pour calculer la signature de sécurité. Le SDK appelle ensuite le point d’entrée client pour récupérer les champs requis pour instancier l’objet de signature. Si le mode d’accès de sécurité est activé dans Marketo Mobile Admin, vous devez définir cette signature dans SDK.

## Configuration du mode d&#39;accès sécurisé

Mettez en œuvre cette configuration avant d’activer le mode d’accès sécurisé sur la page Marketo Admin > Applications et appareils mobiles .

Le mode d’accès sécurisé nécessite un algorithme de signature côté serveur et un point d’entrée client. Le point d’entrée renvoie les valeurs suivantes :

- Clé d’accès
- Signature calculée
- Date et heure d’expiration
- Adresse e-mail

L’algorithme utilise la clé d’accès utilisateur, le secret d’accès, l’adresse e-mail, l’horodatage et l’identifiant de l’appareil. Le ou la client(e) doit configurer le point d’entrée , implémenter le calcul de signature et conserver la date et l’heure d’expiration à jour.

```python
import argparse
import datetime
import hashlib
import hmac


ACCESS_KEY = 'Your Access Key'
ACCESS_SECRET = 'Your access secret'

# Key should not be unicode
def get_signing_key(timestamp):
    return 'MKTO' + ACCESS_SECRET + str(timestamp)

def get_string_to_sign(email, uuid):
    return email + uuid

def get_hmac(key, string_to_sign):
    return hmac.new(key, string_to_sign.encode('utf-8'), hashlib.sha256).hexdigest()

def get_epoch_plus_day():
    epoch = datetime.datetime.utcfromtimestamp(0)
    valid_until_dt = datetime.datetime.utcnow() + datetime.timedelta(days=1)
    return long((valid_until_dt - epoch).total_seconds())

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument("-e", "--email", required=True, help="email address")
    parser.add_argument("-u", "--uuid", required=True, help="Device install id")
    parser.add_argument("-t", "--timestamp", type=int, help="Valid until timestamp")
    args = parser.parse_args()
    string_to_sign = get_string_to_sign(args.email, args.uuid)
    if not args.timestamp:
        valid_until = get_epoch_plus_day()
    else:
        valid_until = args.timestamp
    signing_key = get_signing_key(valid_until)
    hmac_string = get_hmac(signing_key, string_to_sign)
    print 'HMAC is ', hmac_string
```

Utilisez les méthodes spécifiques à la plateforme pour définir ou supprimer la signature de sécurité et récupérer l’identifiant de l’appareil.

### iOS

```objectivec
Marketo * sharedInstance =[Marketo sharedInstance];

// set secure signature
MKTSecuritySignature *signature =
[[MKTSecuritySignature alloc] initWithAccessKey:<ACCESS_KEY> signature:<SIGNATURE_TOKEN> timestamp:<EXPIRY_TIMESTAMP> email:<EMAIL>];
[sharedInstance setSecureSignature:signature];

// remove signature
[sharedInstance removeSecureSignature];

// get device id
[sharedInstance getDeviceId];
```

```swift
let sharedInstance = Marketo.sharedInstance()

 // set secure signature
let signature = MKTSecuritySignature(accessKey: <ACCESS_KEY>, signature: <SIGNATURE_TOKEN> , timestamp: <EXPIRY_TIMESTAMP>, email: <EMAIL>)
sharedInstance.setSecureSignature(signature)

// remove signature
[sharedInstance removeSecureSignature];

// get device id
sharedInstance.getDeviceId()
```

### Android

```java
Marketo sdk = Marketo.getInstance(getApplicationContext());

// set signature
MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
secureMode.setAccessKey(<ACCESS_KEY>);
secureMode.setEmail(<EMAIL_ADDRESS>);
secureMode.setSignature(<SIGNATURE_TOKEN>);
secureMode.setTimestamp(<EXPIRY_DATE>);
if (secureMode.isValid()) {
  sdk.setSecureSignature(secureMode);
}

// remove signature
sdk.removeSecureSignature();

// get device id
sdk.getDeviceId();
```
