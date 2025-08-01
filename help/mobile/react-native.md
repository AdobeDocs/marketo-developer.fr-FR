---
title: React Native
feature: Mobile Marketing
description: Installation de React Native pour Marketo
exl-id: 462fd32e-91f1-4582-93f2-9efe4d4761ff
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 0%

---

# React Native

Cet article fournit des informations sur la manière d’installer et de configurer Marketo native SDK pour intégrer votre application mobile à notre plateforme.

## Conditions préalables

[Ajoutez une application dans Marketo Admin](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (obtenez la clé secrète de l’application et l’ID Munchkin).

## Intégration de SDK

### Intégration d’Android SDK

**Configuration à l’aide de Gradle**

Ajoutez la dépendance Marketo SDK à la version la plus récente : dans le fichier `build.gradle` de niveau application, sous la section dépendances , ajoutez (y compris la version appropriée de Marketo SDK).

```
implementation 'com.marketo:MarketoSDK:0.x.x'
```

**Ajouter un référentiel mavencentral**

Marketo SDK est disponible sur le [référentiel central maven](https://mvnrepository.com/). Pour synchroniser ces fichiers, ajoutez `mavencentral` référentiel au `build.gradle` racine

```
build script {
  repositories {
    google()
    mavencentral()
  }
}
```

Ensuite, synchronisez votre projet avec les fichiers Gradle.

#### Intégration d’iOS SDK

Avant de créer un pont pour votre projet React Native, il est important de configurer notre SDK dans votre projet Xcode.

Intégration de **SDK - Utilisation de CocoaPods**

L’utilisation de notre SDK iOS dans votre application est facile. Effectuez les étapes suivantes pour le configurer dans le projet Xcode de votre application à l’aide de CocoaPods, afin de pouvoir intégrer notre plateforme à votre application.

Téléchargez [CocoaPods](https://cocoapods.org/) - Distribué en tant que bijou Ruby, il s’agit d’un gestionnaire de dépendances pour Objective-C et Swift qui simplifie le processus d’utilisation de bibliothèques tierces dans votre code, telles que iOS SDK.

Pour le télécharger et l’installer, lancez un terminal de ligne de commande sur votre Mac et exécutez la commande suivante :

1. Installez CocoaPods.

`$ sudo gem install cocoapods`

1. Ouvrez votre fichier PDF. (Dans le dossier iOS du projet ReactNative)

`$ open -a Xcode Podfile`

1. Ajoutez la ligne suivante à votre fichier PDF.

`$ pod 'Marketo-iOS-SDK'`

1. Enregistrez et fermez votre fichier PDF.

1. Téléchargez et installez Marketo iOS SDK.

`$ pod install`

1. Ouvrez l’espace de travail dans Xcode.

`$ open App.xcworkspace`

## Instructions d’installation du module natif de l’

Parfois, une application React Native doit accéder à une API de plateforme native qui n’est pas disponible par défaut dans JavaScript, par exemple les API natives pour accéder à Apple ou Google Pay. Vous souhaitez peut-être réutiliser des bibliothèques Objective-C, Swift, Java ou C++ existantes sans avoir à les implémenter à nouveau dans JavaScript, ou écrire du code multithread haute performance pour des choses comme le traitement des images.

Le système NativeModule expose les instances de classes Java/Objective-C/C++ (natives) à JavaScript (JS) en tant qu’objets JS, ce qui vous permet d’exécuter du code natif arbitraire à partir de JS. Bien que nous ne nous attendions pas à ce que cette fonctionnalité fasse partie du processus de développement habituel, il est essentiel qu’elle existe. Si React Native n’exporte pas une API native dont votre application JS a besoin, vous devriez être en mesure de l’exporter vous-même.

Le pont React Native est utilisé pour communiquer entre les couches JSX et de l’application native. Dans notre cas, l’application hôte sera en mesure d’écrire le code JSX qui peut appeler les méthodes de SDK Marketo.

### Android

Ce fichier contient les méthodes wrapper qui peuvent appeler les méthodes de SDK Marketo en interne avec les paramètres que vous fournissez.

```
public class RNMarketoModule extends ReactContextBaseJavaModule {

   final Marketo marketoSdk;
   RNMarketoModule(ReactApplicationContext context) {
       super(context);
       marketoSdk = Marketo.getInstance(context);
   }

   @NonNull
   @Override
   public String getName() {
       return "RNMarketoModule";
   }

   @ReactMethod
   public void associateLead(ReadableMap leadData) {

       MarketoLead mLead = new MarketoLead();
       try {
           mLead.setCity(leadData.getString(MarketoLead.KEY_CITY));
           mLead.setFirstName(leadData.getString(MarketoLead.KEY_FIRST_NAME));
           mLead.setLastName(leadData.getString(MarketoLead.KEY_LAST_NAME));
           mLead.setAddress(leadData.getString(MarketoLead.KEY_ADDRESS));
           mLead.setEmail(leadData.getString(MarketoLead.KEY_EMAIL));
           mLead.setBirthDay(leadData.getString(MarketoLead.KEY_BIRTHDAY));
           mLead.setCountry(leadData.getString(MarketoLead.KEY_COUNTRY));
           mLead.setFacebookId(leadData.getString(MarketoLead.KEY_FACEBOOK));
           mLead.setGender(leadData.getString(MarketoLead.KEY_GENDER));
           mLead.setState(leadData.getString(MarketoLead.KEY_STATE));
           mLead.setPostalCode(leadData.getString(MarketoLead.KEY_POSTAL_CODE));
           mLead.setTwitterId(leadData.getString(MarketoLead.KEY_TWITTER));
           marketoSdk.associateLead(mLead);
       }
       catch (MktoException e){
       }
   }
   @ReactMethod
   public void setSecureSignature(ReadableMap readableMap) {
       MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
       secureMode.setAccessKey(readableMap.getString("accessKey"));
       secureMode.setEmail(readableMap.getString("email"));
       secureMode.setSignature(readableMap.getString("signature"));
       secureMode.setTimestamp(readableMap.getInt("timeStamp"));
       marketoSdk.setSecureSignature(secureMode);
   }
   @ReactMethod
      public void initializeSDK(String frameworkType, String munchkinId, String appSecreteKey){
          marketoSdk.initializeSDK(munchkinId,appSecreteKey,frameworkType);
    }


   @ReactMethod
   public void initializeMarketoPush(String projectId){
       marketoSdk.initializeMarketoPush( projectId);
   }

   @ReactMethod
   public void initializeMarketoPush(String projectId, String channelName){
       marketoSdk.initializeMarketoPush( projectId, channelName);
   }

   @ReactMethod
   public void uninitializeMarketoPush(){
       marketoSdk.uninitializeMarketoPush();
   }

   @ReactMethod
   public void reportAction(String action){
       Marketo.reportAction(action, null);
   }

   @ReactMethod
   public void reportAction(String action, ReadableMap readableMap){
       MarketoActionMetaData marketoActionMetaData = new MarketoActionMetaData();
       marketoActionMetaData.setActionDetails(readableMap.getString("setMetric"));
       marketoActionMetaData.setActionMetric(readableMap.getString("setLength"));
       marketoActionMetaData.setActionLength(readableMap.getString("actionDetails"));
       marketoActionMetaData.setActionType(readableMap.getString("actionType"));
       Marketo.reportAction(action, marketoActionMetaData);
   }
}
```

**Enregistrer le package**

Informez les utilisateurs natifs de react du package Marketo.

```
public class MarketoPluginPackage implements ReactPackage {

   @NonNull
   @Override
   public List createNativeModules(@NonNull ReactApplicationContext reactContext) {
           List modules = new ArrayList<>();

           modules.add(new RNMarketoModule(reactContext));

           return modules;
   }

   @NonNull
   @Override
   public List createViewManagers(@NonNull ReactApplicationContext reactContext) {
       return Collections.emptyList();
   }
}
```

Pour terminer l’enregistrement du package, ajoutez MarketoPluginPackage à la liste des packages React dans la classe d’application :

```
public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost =
      new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
          return BuildConfig.DEBUG;
        }

        @Override
        protected List getPackages() {
          @SuppressWarnings("UnnecessaryLocalVariable")
          List packages = new PackageList(this).getPackages();
          packages.add(new MarketoPluginPackage());     //Add the Marketo Package here.
          // Packages that cannot be autolinked yet can be added manually here, for example:
          return packages;
        }
}
```

### iOS

Dans le guide suivant, vous allez créer un module natif, _RNMarketoModule_, qui vous permettra d’accéder aux API de Marketo à partir de JavaScript.

Pour commencer, ouvrez le projet iOS dans votre application React Native dans Xcode. Votre projet iOS est disponible ici, dans une application React Native. Nous vous recommandons d’utiliser Xcode pour écrire votre code natif. Xcode est conçu pour le développement iOS et son utilisation vous aidera à résoudre rapidement des erreurs plus petites telles que la syntaxe du code.

Créez votre principal en-tête de module natif personnalisé et vos fichiers d’implémentation. Créez un fichier appelé `MktoBridge.h` et ajoutez-y les éléments suivants :

```
//
//  MktoBridge.h
//
//  Created by Marketo, An Adobe company.
//

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface MktoBridge : NSObject

@end

NS_ASSUME_NONNULL_END
```

Créez le fichier d’implémentation correspondant, `MktoBridge.m`, dans le même dossier et incluez le contenu suivant :

```
//
//  MktoBridge.h
//  Created by Marketo, An Adobe company.
//

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface MktoBridge : NSObject <RCTBridgeModule>

@end

NS_ASSUME_NONNULL_END


//
//  MktoBridge.m
//  Created by Marketo, An Adobe company.
//

#import "MktoBridge.h"
#import <MarketoFramework/Marketo.h>
#import <React/RCTBridge.h>
#import "ConstantStringsHeader.h"

@implementation MktoBridge

RCT_EXPORT_MODULE(RNMarketoModule);

+(BOOL)requiresMainQueueSetup{
  return NO;
}

RCT_EXPORT_METHOD(initializeSDK:(NSString *) munchkinId SecretKey: (NSString *) secretKey andFrameworkType: (NSString *) frameworkType){
  [[Marketo sharedInstance] initializeWithMunchkinID:munchkinId appSecret:secretKey mobileFrameworkType:frameworkType launchOptions:nil];
}

RCT_EXPORT_METHOD(reportAction:(NSString *)actionName withMetaData:(NSDictionary *)metaData){
  MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
  [meta setType:[metaData objectForKey:KEY_ACTION_TYPE]];
  [meta setDetails:[metaData objectForKey:KEY_ACTION_DETAILS]];
  [meta setLength:[metaData valueForKey:KEY_ACTION_LENGTH]];
  [meta setMetric:[metaData valueForKey:KEY_ACTION_METRIC]];
  [[Marketo sharedInstance] reportAction:actionName withMetaData:meta];
}

RCT_EXPORT_METHOD(associateLead:(NSDictionary *)leadDetails){
  MarketoLead *lead = [[MarketoLead alloc] init];
  if ([leadDetails objectForKey:KEY_EMAIL] != nil) {
    [lead setEmail:[leadDetails objectForKey:KEY_EMAIL]];
  }
  if ([leadDetails objectForKey:KEY_FIRST_NAME] != nil) {
    [lead setFirstName:[leadDetails objectForKey:KEY_FIRST_NAME]];
  }

  if ([leadDetails objectForKey:KEY_LAST_NAME] != nil) {
    [lead setLastName:[leadDetails objectForKey:KEY_LAST_NAME]];
  }

  if ([leadDetails objectForKey:KEY_CITY] != nil) {
    [lead setCity:[leadDetails objectForKey:KEY_CITY]];
  }
    [[Marketo sharedInstance] associateLead:lead];
}

RCT_EXPORT_METHOD(uninitializeMarketoPush){
  [[Marketo sharedInstance] unregisterPushDeviceToken];
}

RCT_EXPORT_METHOD(reportAll){
  [[Marketo sharedInstance] reportAll];
}

RCT_EXPORT_METHOD(setSecureSignature:(NSDictionary *)secureSignature){
  MKTSecuritySignature *secSignature = [[MKTSecuritySignature alloc]
                                        initWithAccessKey:[secureSignature objectForKey:KEY_ACCESSKEY]
                                        signature:[secureSignature objectForKey:KEY_SIGNATURE]
                                        timestamp: [secureSignature objectForKey:KEY_EMAIL]
                                        email:[secureSignature objectForKey:KEY_EMAIL]];

    [[Marketo sharedInstance] setSecureSignature:secSignature];
}

RCT_EXPORT_METHOD(requestPermission:(RCTPromiseResolveBlock)resolve rejecter:(RCTPromiseRejectBlock)reject) {
  UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
  [center requestAuthorizationWithOptions:(UNAuthorizationOptionAlert + UNAuthorizationOptionSound + UNAuthorizationOptionBadge)
                        completionHandler:^(BOOL granted, NSError * _Nullable error) {
    if (error) {
      reject(@"PERMISSION_ERROR", @"Permission request failed", error);
    } else {
      resolve(@(granted));
    }
  }];
}

RCT_EXPORT_METHOD(registerForRemoteNotifications) {
  dispatch_async(dispatch_get_main_queue(), ^{
    [[UIApplication sharedApplication] registerForRemoteNotifications];
  });
}


@end
```

#### Initialiser Marketo SDK

Recherchez un emplacement de votre application où vous souhaitez ajouter un appel à la méthode createCalendarEvent() du module natif. Vous trouverez ci-dessous un exemple de composant, NewModuleButton, que vous pouvez ajouter à votre application. Vous pouvez appeler le module natif dans la fonction onPress() de NewModuleButton.

```
import React from 'react';
import { NativeModules, Button } from 'react-native';

const NewModuleButton = () => {
  const onPress = () => {
    console.log('We will invoke the native module here!');
  };

  return (

  );
  };

export default NewModuleButton;
```

Ce fichier JavaScript charge le module natif dans la couche JavaScript.

```javascript
import React from 'react';
import {Node} from 'react';
import { NativeModules } from 'react-native';

const { RNMarketoModule } = NativeModules;
```

Une fois les fichiers ci-dessus placés correctement, nous pouvons importer le module js dans n’importe quelle classe js et appeler directement ses méthodes. Par exemple :

Notez que nous devons transmettre « reactNative » comme type de framework pour les applications natives React. 

```
// Initialize marketo SDK with Munchkin & Seretkey you have from step 1.
RNMarketoModule.initializeSDK("MunchkinID","SecreteKEY","FrameworkType")

//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})

//You can report any user performed action by calling the reportaction function.
RNMarketoModule.reportAction("Bought Shirt", {actionType:"Shopping", actionDetails: "Red Shirt", setLength : 20, setMetric : 30 })

//You can set Secure Signature by calling this method.
RNMarketoModule.setSecureSignature({accessKey: "Key102", email: "testleadrk@001.com", signature : "asdfghjkloiuytds", timeStamp: "12345678987654"})

//This function will Enable user notifications (Only Android)
RNMarketoModule.initializeMarketoPush("350312872033", "MKTO")

//The token can also be unregistered on logout.
RNMarketoModule.uninitializeMarketoPush()
```

#### Configuration Des Notifications Push

Initialiser la notification push avec l&#39;ID de projet et le nom de canal

```
RNMarketoModule.initializeMarketoPush("ProjectId", "Channel_name")
```

Ajoutez le service suivant à `AndroidManifest.xml`

```xml
<service android:exported="true" android:name=".MyFirebaseMessagingService" android:stopWithTask="true">
    <intent-filter>
        <action  android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter/>
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter/>
</activity/>
```

Créez une classe nommée `FirebaseMessagingService.java` et ajoutez le code suivant

```java
import com.google.firebase.messaging.FirebaseMessagingService;
import com.google.firebase.messaging.RemoteMessage;
import com.marketo.Marketo;

public class MyFirebaseMessagingService extends FirebaseMessagingService {

   @Override
   public void onNewToken(String token) {
       super.onNewToken(token);
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.setPushNotificationToken(token);
   }

   @Override
   public void onMessageReceived(RemoteMessage remoteMessage) {
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.showPushNotification(remoteMessage);
   }
}
```

Les autorisations doivent être activées dans votre projet Xcode pour envoyer des notifications push à l’appareil de l’utilisateur.

Pour envoyer des notifications push, [ ajoutez des notifications push ](push-notifications.md).

Configurer des notifications push iOS,
Créez le fichier PushNotifications.tsx et ajoutez les éléments suivants :

```
import { NativeModules } from 'react-native';
const { RNMarketoModule } = NativeModules;

const requestPermission = (): Promise<boolean> => {
return RNMarketoModule.requestPermission()
.then((granted: boolean) => {
console.log('Permission granted:', granted);
return granted;
})
.catch((error: any) => {
console.error('Permission error:', error);
throw error;
});
};

const registerForRemoteNotifications = (): void => {
RNMarketoModule.registerForRemoteNotifications();
};

export { requestPermission, registerForRemoteNotifications };
```


Ajouter des `App.tsx` pour autoriser les notifications push

```
import React, { useEffect } from 'react';

useEffect(() => {
requestPermission().then(granted => {
if (granted) {
registerForRemoteNotifications();
}
});
}, []);
```

Mettez à jour `AppDelegate.mm` avec les méthodes déléguées APNS :

```
#import "AppDelegate.h"
#import "MktoBridge.h"
#import <MarketoFramework/Marketo.h>
#import <React/RCTBundleURLProvider.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  self.moduleName = @"MyNewApp";
  // You can add your custom initial props in the dictionary below.
  // They will be passed down to the ViewController used by React Native.
  self.initialProps = @{};

  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge
{
  return [self bundleURL];
}

-(void)userNotificationCenter:(UNUserNotificationCenter *)center
      willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{
    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}

- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center
                      didReceiveNotificationResponse:response
                               withCompletionHandler:completionHandler];
}

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}

- (void)applicationWillTerminate:(UIApplication *)application {
    [[Marketo sharedInstance] unregisterPushDeviceToken];
}

-(void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error{
  NSLog(@"Failed to register for remote notification - %@", [error userInfo]);
}

- (NSURL *)bundleURL
{
#if DEBUG
  return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index"];
#else
  return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif
}

@end
```

### Ajouter des appareils de test

**Android**

Ajoutez « MarketoActivity » au fichier `AndroidManifest.xml` dans la balise de l’application.

```xml
<activity android:name="com.marketo.MarketoActivity" android:configChanges="orientation|screenSize" android:exported="true">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

**iOS**

1. Sélectionnez Projet > Cible > Informations > Types d’URL.

1. Ajouter un identifiant : ${PRODUCT_NAME}

1. Définir les schémas d&#39;URL : `mkto-<S_ecret Key_>`

1. Inclure les `application:openURL:sourceApplication:annotation:` dans `AppDelegate.m` fichier (Objective-C)

**iOS - Gérer le type d’URL personnalisé/les liens profonds dans AppDelegate** 

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary *)options{

    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];
}
```

Ces constantes sont utilisées lors de l’appel de l’API depuis JavaScript. Vous devez créer des fichiers constants et ajouter les éléments suivants.

```
// Lead attributes.
static NSString *const KEY_FIRST_NAME = @"firstName";
static NSString *const KEY_LAST_NAME = @"lastName";
static NSString *const KEY_ADDRESS = @"address";
static NSString *const KEY_CITY = @"city";
static NSString *const KEY_STATE = @"state";
static NSString *const KEY_COUNTRY = @"country";
static NSString *const KEY_GENDER = @"gender";
static NSString *const KEY_EMAIL = @"email";
static NSString *const KEY_TWITTER = @"twitterId";
static NSString *const KEY_FACEBOOK = @"facebookId";
static NSString *const KEY_LINKEDIN = @"linkedinId";
static NSString *const KEY_LEAD_SOURCE = @"leadSource";
static NSString *const KEY_BIRTHDAY = @"dateOfBirth";
static NSString *const KEY_FACEBOOK_PROFILE_URL = @"facebookProfileURL";
static NSString *const KEY_FACEBOOK_PROFILE_PIC = @"facebookPhotoURL";

// Custom actions
static NSString *const KEY_ACTION_TYPE = @"actionType";
static NSString *const KEY_ACTION_DETAILS = @"actionDetails";
static NSString *const KEY_ACTION_LENGTH = @"setLength";
static NSString *const KEY_ACTION_METRIC = @"setMetric";

//Secure Signature
static NSString *const KEY_ACCESSKEY = @"accessKey";
static NSString *const KEY_SIGNATURE = @"signature";
static NSString *const KEY_TIMESTAMP = @"timeStamp";
```

Exemple d’utilisation

```
//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})
```
