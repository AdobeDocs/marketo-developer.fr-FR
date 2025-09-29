---
title: Mode d’accès de sécurité avancé
feature: Mobile Marketing
description: Découvrez le mode d’accès de sécurité avancé pour Marketo Mobile SDK, avec la génération de signatures HMAC, la configuration des points d’entrée du serveur, l’utilisation des identifiants d’appareil et des exemples iOS et Android
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 0%

---

# Mode d’accès de sécurité avancé

Le SDK Marketo expose des méthodes pour définir et supprimer la signature de sécurité. Il existe également une méthode utilitaire pour récupérer l’identifiant de l’appareil. L’ID d’appareil doit être transmis avec l’e-mail, lors de la connexion, au serveur client pour être utilisé dans le calcul de la signature de sécurité. Le SDK doit accéder au nouveau point d’entrée, en pointant vers l’algorithme répertorié ci-dessus, pour récupérer les champs nécessaires à l’instanciation de l’objet de signature. La définition de cette signature dans SDK est une étape nécessaire si le mode d’accès de sécurité a été activé dans Marketo Mobile Admin.

## Configuration du mode d&#39;accès sécurisé

Cette configuration doit être implémentée avant que le mode d’accès sécurisé ne soit activé via la page Marketo Admin > Applications et appareils mobiles . Les étapes suivantes décrivent le processus requis pour terminer le processus de validation de la sécurité :

Le mode d’accès sécurisé nécessite la mise en œuvre de l’algorithme de signature côté serveur du client qui fournira un point d’entrée pour récupérer la clé d’accès, la signature calculée, l’horodatage d’expiration et l’e-mail. Cet algorithme nécessite la clé d’accès utilisateur, le secret d’accès, l’adresse e-mail, l’horodatage et l’ID d’appareil pour effectuer le calcul. Le client est chargé de configurer le point d’entrée, de mettre en œuvre l’algorithme pour effectuer les calculs de signature et de conserver un horodatage d’expiration à jour.

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

Le SDK Marketo propose de nouvelles méthodes pour définir et supprimer la signature de sécurité. Il existe également une méthode utilitaire pour récupérer l’identifiant de l’appareil. L’ID d’appareil doit être transmis avec l’e-mail, lors de la connexion, au serveur client pour être utilisé dans le calcul de la signature de sécurité. Le SDK doit accéder au nouveau point d’entrée, en pointant vers l’algorithme répertorié ci-dessus, pour récupérer les champs nécessaires à l’instanciation de l’objet de signature. La définition de cette signature dans SDK est une étape nécessaire si le mode d’accès de sécurité a été activé dans Marketo Mobile Admin.

### iOS

```
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

```
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

```
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
