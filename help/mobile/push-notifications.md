---
title: "Notifications push"
feature: "Mobile Marketing"
description: Activation des notifications push pour Marketo Mobile
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---


# Notifications Push

Comment activer les notifications push.

## Configuration de la notification push sur iOS

Pour activer les notifications push, trois étapes sont nécessaires :

1. Configurez les notifications push sur le compte développeur Apple.
1. Activez les notifications push dans xCode.
1. Activez les notifications push dans l’application avec le SDK Marketo.

### Configuration des notifications push sur le compte développeur Apple

1. Connectez-vous au développeur Apple [Centre de membres](http://developer.apple.com/membercenter).
1. Cliquez sur &quot;Certificats, identifiants et profils&quot;.
1. Cliquez sur le dossier &quot;Certificats->Tous&quot; sous &quot;iOS, tvOS, watchOS&quot;.
1. Sélectionnez le &quot;+&quot; dans l’écran supérieur gauche en regard des certificats. ![](assets/certificates-plus.png)
1. Cochez la case &quot;Apple Push Notification service SSL (Sandbox &amp; Production)&quot;, puis cliquez sur &quot;Continuer&quot;.
1. Sélectionnez l’identifiant de l’application que vous utilisez pour créer l’application.![](assets/push-appid.png)
1. Créez et téléchargez une demande de signature de certificat (CSR) pour générer le certificat push. ![](assets/push-ssl.png)
1. Téléchargez le certificat sur l’ordinateur local et double-cliquez pour l’installer. ![](assets/certificate-download.png)
1. Ouvrez &quot;Accès au trousseau&quot;, cliquez avec le bouton droit sur le certificat, puis exportez 2 éléments dans le `.p12` fichier .![key_chain](assets/key-chain.png)
1. Téléchargez ce fichier via Marketo Admin Console pour configurer des notifications.
1. Mettez à jour les profils de mise en service d’application.

### Activation des notifications push dans xCode

Activez la fonctionnalité de notification push dans le projet xCode.![](assets/push-xcode.png)

### Activation des notifications push dans l’application avec le SDK Marketo

Ajoutez le code suivant à `AppDelegate.m` pour envoyer des notifications push aux appareils de votre client.

**Remarque** - Si vous utilisez la variable [!DNL Adobe Launch] extension, utiliser `ALMarketo` as class name

Importez les éléments suivants dans `AppDelegate.h`.

>[!BEGINTABS]

>[!TAB Objectif C]

```
#import <UserNotifications/UserNotifications.h>
```

>[!TAB Swift]

```
import UserNotifications
```

>[!ENDTABS]

Ajouter `UNUserNotificationCenterDelegate` to `AppDelegate` comme illustré ci-dessous.

>[!BEGINTABS]

>[!TAB Objectif C]

```
@interface AppDelegate : UIResponder <UIApplicationDelegate, UNUserNotificationCenterDelegate>
```

>[!TAB Swift]

```
class AppDelegate: UIResponder, UIApplicationDelegate , UNUserNotificationCenterDelegate
```

>[!ENDTABS]

Lancer le service de notification push Pour activer la notification push, ajoutez le code ci-dessous.

>[!BEGINTABS]

>[!TAB Objectif C]

```objectivec
BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
        center.delegate = self;
        [center requestAuthorizationWithOptions:(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge) completionHandler:^(BOOL granted, NSError * _Nullable error){
            if(!error){
                dispatch_async(dispatch_get_main_queue(), ^{
                    [[UIApplication sharedApplication] registerForRemoteNotifications];
                });
            }
        }];

    return YES;
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
            
    UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound,    .badge]) { granted, error in
            if let error = error {
                print("\(error.localizedDescription)")
            } else {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
        
        return true
}
```

>[!ENDTABS]

Appelez cette méthode pour lancer le processus d’enregistrement avec le service Apple Push. Si l’enregistrement réussit, l’application appelle le de l’objet délégué de votre application. `application:didRegisterForRemoteNotificationsWithDeviceToken:` et lui transmet un jeton d’appareil.

Si l’enregistrement échoue, l’application appelle le de son délégué d’application. `application:didFailToRegisterForRemoteNotificationsWithError:` à la place.

Enregistrez le jeton push avec Marketo. Pour recevoir des notifications push de Marketo, vous devez enregistrer le jeton de périphérique avec Marketo.

>[!BEGINTABS]

>[!TAB Objectif C]

```
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    // Register the push token with Marketo
    Marketo.sharedInstance().registerPushDeviceToken(deviceToken)
}
```

>[!ENDTABS]

Le jeton peut également être désinscrit lorsque l’utilisateur se déconnecte.

>[!BEGINTABS]

>[!TAB Objectif C]

```
[[Marketo sharedInstance] unregisterPushDeviceToken];
```

>[!TAB Swift]

```
Marketo.sharedInstance().unregisterPushDeviceToken
```

>[!ENDTABS]

Pour réenregistrer le jeton push, extrayez le code de l’étape 3 dans une méthode AppDelegate et appelez à partir de la méthode de connexion ViewController .

Gérer les notifications push. Pour recevoir des notifications push de Marketo, vous devez enregistrer le jeton de périphérique avec Marketo.

>[!BEGINTABS]

>[!TAB Objectif C]

```
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
    [[Marketo sharedInstance] handlePushNotification:userInfo];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any]) {
    Marketo.sharedInstance().handlePushNotification(userInfo)
}
```

>[!ENDTABS]

Ajoutez la méthode suivante dans AppDelegate .

Grâce à cette méthode, vous pouvez présenter une alerte, son ou augmenter le badge pendant que l’application est en premier plan. Vous devez appeler completionHandler de votre choix dans cette méthode.

>[!BEGINTABS]

>[!TAB Objectif C]

```
-(void)userNotificationCenter:(UNUserNotificationCenter *)center
    willPresentNotification:(UNNotification *)notification
        withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{

    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}
```

>[!TAB Swift]

```
func userNotificationCenter(_ center: UNUserNotificationCenter, 
            willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (
    UNNotificationPresentationOptions) -> Void) {
    completionHandler([.alert, .sound,.badge])
}
```

>[!ENDTABS]

Gestion des notifications push nouvellement reçues dans AppDelegate

La méthode sera appelée sur le délégué lorsque l’utilisateur aura répondu à la notification en ouvrant l’application, en ignorant la notification ou en choisissant un UNNotificationAction. Le délégué doit être défini avant que l’application ne soit renvoyée d’applicationDidFinishLaunching:.

>[!BEGINTABS]

>[!TAB Objectif C]

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
}
```

>[!TAB Swift]

```
func userNotificationCenter(_ center: UNUserNotificationCenter,
                                didReceive response: UNNotificationResponse,
                                withCompletionHandler
                                completionHandler: @escaping () -> Void) {
        Marketo.sharedInstance().userNotificationCenter(center, didReceive: response, withCompletionHandler: completionHandler)
}
```

>[!ENDTABS]

Suivi des notifications push

Si votre application s’exécute en arrière-plan (ou n’est pas active), l’appareil recevra une notification push, comme illustré ci-dessous. Marketo effectue le suivi lorsque l’utilisateur appuie sur la notification.

![mobile8](assets/mobile8.png)

Si l’appareil reçoit une notification push, il est transmis à `application:didReceiveRemoteNotification:` rappel sur votre délégué d’application.

Voici un journal d’activité Marketo de Marketo qui affiche les événements d’application et les événements de notification push.

![mobile9](assets/mobile9.png)

## Configuration de la notification push sur Android

1. Ajoutez l’autorisation suivante dans la balise de l’application.

   Ouvrir `AndroidManifest.xml` et ajoutez les autorisations suivantes. Votre application doit demander les autorisations &quot;INTERNET&quot; et &quot;ACCESS_NETWORK_STATE&quot;. Si votre application demande déjà ces autorisations, ignorez cette étape.

   ```xml
   <uses‐permission android:name="android.permission.INTERNET"/>
   <uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
   
   <!‐‐Following permissions are required for push notification.‐‐>
   <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
   <!‐‐Keeps the processor from sleeping when a message is received.‐‐>
   <uses-permission android:name="android.permission.WAKE_LOCK"/>
   <permission android:name="<PACKAGE_NAME>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
   <uses-permission android:name="<PACKAGE_NAME>.permission.C2D_MESSAGE" />
   <!-- This app has permission to register and receive data message. -->
   <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   ```

1. Configuration de FCM avec HTTPv1 (Google dispose de [protocole XMPP obsolète](https://firebase.google.com/docs/cloud-messaging/xmpp-server-ref) le 12 juin 2023 et sera supprimé en juin 2024) 

- Activation de MME FCM HTTP v1 dans le gestionnaire de fonctionnalités Marketo ![](assets/feature-manager.png)
   - Téléchargez le fichier Json du compte de service pour l’application dans MLM.
   - Vous pouvez télécharger le fichier Json du compte de service depuis Firebase Console.   ![](assets/fcm-console.png)
   - Patientez une heure après avoir chargé le fichier Json du compte de service dans Marketo avant d’envoyer des notifications push.  

## Appareils de test Android

Ajoutez l’activité Marketo dans le fichier manifeste à l’intérieur de la balise de l’application.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

## Enregistrement du service Marketo Push

1. Pour recevoir des notifications push de Marketo, vous devez ajouter le service de messagerie Firebase à votre `AndroidManifest.xml`. Ajoutez avant la balise de fermeture de l’application.

   ```xml
   <meta-data
       android:name="com.google.android.gms.version"
       android:value="@integer/google_play_services_version" />
   <service android:name=".MyFirebaseMessagingService">
   <intent-filter>
   <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
   <action android:name="com.google.firebase.MESSAGING_EVENT"/>
   </intent-filter>
   </service>
   ```

1. Ajout des méthodes du SDK Marketo dans le fichier `MyFirebaseMessagingService` comme suit

   ```java
   import com.marketo.Marketo;
   
   public class MyFirebaseMessagingService extends FirebaseMessagingService {
   
       @Override
       public void onNewToken(String s) {
           super.onNewToken(s);
           Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
           marketoSdk.setPushNotificaitonToken(s);
           // Add your code here...
       }
   
       @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
           Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
           marketoSdk.showPushNotificaiton(remoteMessage);
           // Add your code here...
       }
   
   }
   ```

   **Remarque** - Si vous utilisez l’extension Adobe, ajoutez comme ci-dessous

   ```java
   import com.marketo.Marketo;
   
   public class MyFirebaseMessagingService extends FirebaseMessagingService {
   
       @Override
       public void onNewToken(String token) {
           super.onNewToken(token);
           ALMarketo.setPushNotificationToken(token);
           // Add your code here...
       }
   
       @Override
       public void onMessageReceived(RemoteMessage remoteMessage) {
           ALMarketo.showPushNotification(remoteMessage);
           // Add your code here...
       }
   
   }
   ```

**REMARQUE**: le SDK FCM ajoute automatiquement toutes les autorisations requises ainsi que la fonctionnalité de récepteur requise. Veillez à supprimer les éléments obsolètes (et potentiellement dangereux, comme ils peuvent entraîner la duplication des messages) suivants du manifeste de votre application si vous avez utilisé les versions précédentes du SDK.

```xml
<receiver android:name="com.marketo.MarketoBroadcastReceiver" android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <!‐‐Receives the actual messages.‐‐>
        <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
        <!‐‐Register to enable push notification‐‐>
        <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
        <!‐‐‐Replace YOUR_PACKAGE_NAME with your own package name‐‐>
        <category android:name="YOUR_PACKAGE_NAME"/>
    </intent-filter>
</receiver>

<!‐‐Marketo service to handle push registration and notification‐‐>
<service android:name="com.marketo.MarketoIntentService"/>
```

1. Initialisez Marketo Push après avoir enregistré la configuration ci-dessus, vous devez initialiser la notification push Marketo. Créez ou ouvrez votre classe Application et copiez/collez le code ci-dessous. Vous pouvez obtenir votre ID d’expéditeur à partir de la console Firebase.


   ```java
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   
   // Enable push notification here. The push notification channel name can by any string
   marketoSdk.initializeMarketoPush(SENDER_ID,"ChannelName");
   ```

   Si vous utilisez [!DNL Adobe Launch] Extension, utilisez les instructions suivantes

   ```java
   // Enable push notification here. The push notification channel name can by any string
   ALMarketo.initializeMarketoPush(SENDER_ID,"ChannelName");
   ```

   Si vous ne possédez pas de SENDER_ID, activez le service de messagerie cloud Google en suivant les étapes décrites dans la section [ce tutoriel](https://developers.google.com/cloud-messaging/).

   Le jeton peut également être désinscrit lorsque l’utilisateur se déconnecte.

   ```java
   marketoSdk.uninitializeMarketoPush();
   ```

   Si vous utilisez [!DNL Adobe Launch] extension, utilisez l’instruction ci-dessous

   ```java
   ALMarketo.uninitializeMarketoPush();
   ```

   Remarque : Pour réenregistrer le jeton push, extrayez le code de l’étape 3 dans une méthode AppDelegate et appelez à partir de la méthode de connexion ViewController .

1. Définir l’icône de notification (facultatif) Pour configurer une icône de notification personnalisée, la méthode suivante doit être appelée.

   ```java
   MarketoConfig.Notification config = new MarketoConfig.Notification();
   // Optional bitmap for honeycomb and above
   config.setNotificationLargeIcon(bitmap);
   
   // Required icon Resource ID
   config.setNotificationSmallIcon(R.drawable.notification_small_icon); 
   
   // Set the configuration 
   //Use the static methods on ALMarketo class when using Adobe Extension
   Marketo.getInstance(context).setNotificationConfig(config); 
   
   // Get the configuration set 
   Marketo.getInstance(context).getNotificationConfig();
   ```

## Dépannage

La configuration des messages push mobiles implique de nombreuses étapes et la coordination des développeurs et des marketeurs. Si vous rencontrez des difficultés, vous pouvez vérifier certaines choses simples.

Après avoir vérifié que les choses simples sont correctes, vous pouvez approfondir les détails de la programmation.

### Le message push ne s’affiche pas

Tout d’abord, vérifiez si les messages push sont désactivés sur le combiné. Les utilisateurs mobiles peuvent contrôler la réception de messages pour une application spécifique ou non. Souvent, les développeurs (et les marketeurs) désactivent ces messages à un moment donné pendant le développement. La première chose à vérifier est donc de savoir si le destinataire a désactivé les messages push pour votre application.

Deuxièmement, l’application est-elle déjà ouverte et active sur l’appareil ? Lorsque votre application est l’application active sur l’appareil, les messages push mobiles ne s’affichent pas à l’écran. Ils apparaissent à la place dans la zone &quot;Notifications locales&quot; de votre application.

### Affichage des journaux d’activité dans Marketo

Le premier emplacement à rechercher lors du suivi d’une erreur se trouve dans les journaux d’activité de Marketo. Vous pouvez utiliser les journaux d’activité pour vérifier qu’un message a été envoyé.

Dans le journal des activités, consultez les enregistrements d’activité d’une personne censée recevoir un message. Si le message a été envoyé, un enregistrement est présent dans le journal des activités. Si ce n’est pas le cas, le problème est probablement dû à la configuration du certificat iOS ou de la clé de l’API Android dans Marketo.

### Certificat ou clé non valide

Vérifiez deux fois votre configuration pour vous assurer que le certificat approprié est chargé pour Sandbox ou Production. Il est parfois préférable que le développeur réexporte les certificats (iOS) ou les clés (Android), puis les recharge dans Marketo pour s’assurer qu’ils sont corrects.

### Le fichier .p12 comporte une clé ou un certificat manquant (iOS)

Lorsque vous exportez le certificat, veillez à exporter la clé _et_ le certificat.

### Profils d’approvisionnement obsolètes (iOS)

Chaque fois que vous ajoutez un nouvel appareil, vous devez mettre à jour vos profils de mise en service et générer de nouveaux certificats. Assurez-vous que votre projet Xcode pointe ensuite vers les profils et les certificats appropriés, puis importez ces certificats dans Marketo.

### Impossible de télécharger le certificat iOS (IOS)

Assurez-vous que le mot de passe utilisé lors de l’exportation du certificat ne contient pas d’espaces.  Par exemple, au lieu de :

`Hello World 123`

utilisez ceci :

`HelloWorld123`

### Dépannage des certificats iOS

Pour les applications Sandbox, vous pouvez utiliser un certificat &quot;développeur&quot; ou &quot;universel&quot;. Mais pour les applications de production, vous devez télécharger un certificat &quot;distribution&quot; ou &quot;universel&quot; valide.

### Rebond push/Jeton non valide

Un jeton d’enregistrement existant peut cesser d’être valide dans plusieurs cas, notamment :

- Si l’application cliente ne s’enregistre plus avec GCM.
- Si l’application cliente est automatiquement désenregistrée, ce qui peut se produire si l’utilisateur désinstalle l’application. Par exemple, sur iOS, si le service de retour APNS signalait que le jeton APNS n’était pas valide.
- Si le jeton d’enregistrement expire. Par exemple, Google peut décider d’actualiser les jetons d’enregistrement ou le jeton APNS a expiré pour les appareils iOS.
- Si l’application cliente est mise à jour mais que la nouvelle version n’est pas configurée pour recevoir des messages.
