---
title: Mode d’accès à la sécurité avancé
feature: Mobile Marketing
description: Détails sur le mode d’accès de sécurité avancé
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Mode d’accès à la sécurité avancé

Le SDK Marketo expose les méthodes permettant de définir et de supprimer la signature de sécurité. Il existe également une méthode utilitaire pour récupérer l’identifiant de l’appareil. L’identifiant de l’appareil doit être transmis avec l’e-mail, lors de la connexion, au serveur du client pour être utilisé dans le calcul de la signature de sécurité. Le SDK doit accéder au nouveau point de terminaison , en pointant vers l’algorithme répertorié ci-dessus, pour récupérer les champs nécessaires pour instancier l’objet de signature. La définition de cette signature dans le SDK est une étape nécessaire si le mode Accès à la sécurité a été activé dans l’administration mobile Marketo.

## Configuration du mode d’accès sécurisé

Cette configuration doit être implémentée avant que le mode Accès sécurisé n’ait été activé via la page Marketo Admin > Applications et périphériques mobiles . Les étapes supplémentaires suivantes décrivent le processus requis pour terminer le processus de validation de la sécurité :

Le mode Accès sécurisé nécessite l’implémentation de l’algorithme de signature côté serveur du client, qui fournit un point de terminaison pour récupérer la clé d’accès, la signature calculée, l’horodatage d’expiration et le courrier électronique. Cet algorithme nécessite que l’utilisateur dispose de la clé d’accès, du secret d’accès, de l’e-mail, de l’horodatage et de l’identifiant de périphérique pour effectuer le calcul. Le client est chargé de configurer le point de terminaison, de mettre en oeuvre l’algorithme pour effectuer les calculs de signature et de conserver l’horodatage d’expiration.

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

Le SDK Marketo expose de nouvelles méthodes pour définir et supprimer la signature de sécurité. Il existe également une méthode utilitaire pour récupérer l’identifiant de l’appareil. L’identifiant de l’appareil doit être transmis avec l’e-mail, lors de la connexion, au serveur du client pour être utilisé dans le calcul de la signature de sécurité. Le SDK doit accéder au nouveau point de terminaison , en pointant vers l’algorithme répertorié ci-dessus, pour récupérer les champs nécessaires pour instancier l’objet de signature. La définition de cette signature dans le SDK est une étape nécessaire si le mode Accès à la sécurité a été activé dans l’administration mobile Marketo.

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
