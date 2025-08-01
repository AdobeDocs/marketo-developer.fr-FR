---
title: Script de l'e-mail
feature: Email Programs
description: Présentation des scripts d’e-mail
exl-id: ff396f8b-80c2-4c87-959e-fb8783c391bf
source-git-commit: 9012135dc7a295c2462574dad1aca2d06a9077ea
workflow-type: tm+mt
source-wordcount: '963'
ht-degree: 0%

---

# Script de l&#39;e-mail

REMARQUE : il est vivement recommandé de lire le [Guide de l’utilisateur Velocity](https://velocity.apache.org/engine/devel/user-guide.html) pour une analyse approfondie du comportement du langage de modèle Velocity.

[Apache Velocity](https://velocity.apache.org/) est un langage basé sur Java qui a été conçu pour créer des modèles et des scripts de contenu HTML. Marketo permet de l’utiliser dans le contexte des e-mails à l’aide de jetons de script. Cela donne accès aux données stockées dans les opportunités et les objets personnalisés, et permet la création de contenu dynamique dans les e-mails. Velocity offre un flux de contrôle standard de haut niveau avec if/else, for et for each pour permettre la manipulation conditionnelle et itérative du contenu. Voici un exemple simple pour imprimer un message d’accueil avec la formule de salutation appropriée :

```java
##check if the lead is male
#if(${lead.MarketoSocialGender} == "Male")
    ##if the lead is male, use the salutation 'Mr.'
    #set($greeting = "Dear Mr. ${lead.LastName},")
##check is the lead is female
#elseif(${lead.MarketoSocialGender} == "Female")
    ##if female, use the salutation 'Ms.'
    #set($greeting = "Dear Ms. ${lead.LastName},")
#else
    ##otherwise, use the first name
    #set($greeting = "Dear ${lead.FirstName},")
#end
##print the greeting and some content
${greeting}

    Lorem ipsum dolor sit amet...
```

## Variables

Les variables sont toujours précédées du préfixe « $ » et sont définies et mises à jour à l’aide de #set :

```
#set($variable = "value")
```

Leurs valeurs peuvent ensuite être récupérées via plusieurs types de référence différents avec des comportements différents :

```
$variable ##outputs 'value'
$variablename ##outputs '$variablename'
${variable}name ##outputs 'valuename'
```

Il existe également une notation de référence silencieuse, où un `!` est Inclus après le `$`. Normalement, lorsque la vitesse rencontre une référence non définie, la chaîne représentant la référence est laissée en place. Avec la notation de référence silencieuse, si une référence non définie est rencontrée, alors aucune valeur n’est émise :

```
##Defined Reference

#set($foo = "bar")
$foo ##outputs "bar"

##Undefined Reference

##normal
$baz ##outputs "$baz"

##quiet
$!baz ##outputs nothing
```

Pour plus d’informations sur la référence à des variables, consultez le [Guide d’utilisation d’Apache](https://velocity.apache.org/engine/devel/user-guide.html#formal-reference-notation).

## Outils Velocity

Le projet Apache Velocity rend les fonctionnalités disponibles à l’aide des [outils Velocity](https://velocity.apache.org/tools/devel/apidocs/overview-summary.html). Il s’agit simplement de wrappers pour les objets Java, qui exposent leurs méthodes par le biais de variables globales accessibles à tous les scripts.

- [OutilAlternateur](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/AlternatorTool.html)
- [ComparisonDateTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ComparisonDateTool.html)
- [Outil de conversion](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/ConversionTool.html)
- [Outil de date](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DateTool.html)
- [DisplayTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/DisplayTool.html)
- [Outil mathématique](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/MathTool.html)
- [NumberTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/NumberTool.html)
- [OutilÉchap](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/EscapeTool.html)
- [LoopTool](https://velocity.apache.org/tools/devel/apidocs/org/apache/velocity/tools/generic/LoopTool.html)

Par exemple, pour utiliser une méthode à partir de `ComparisonDateTool`, accédez à depuis la variable `$date` dans un jeton de script :

```
#set($birthday = $convert.parseDate("2015-08-07","yyyy-MM-dd"))
##use whenIs to determine how many days away it is
$date.whenIs($birthday).days ##outputs 1
```

## Création d’un jeton de script

Le script Velocity est inclus dans les e-mails à l’aide de jetons de script de messagerie. Ils peuvent être créés dans des activités marketing dans un dossier marketing ou un programme. Pour qu’un jeton soit utilisé dans un e-mail, l’e-mail doit être l’enfant d’un programme qui détient le jeton ou qui l’hérite d’un dossier marketing. Pour créer un jeton, accédez à un dossier ou à un programme, puis sélectionnez l’onglet [!UICONTROL Mes jetons]. Dans le menu de droite, faites glisser l’option « Script d’e-mail » dans la liste des jetons

![Jeton de script](assets/script-token.png)

À partir de là, vous pouvez modifier le nom du jeton et ouvrir l’éditeur via l’option [!UICONTROL Cliquer pour modifier] :

![Modifier le script](assets/script-edit.png)

Une fois que vous êtes dans l’éditeur, vous pouvez créer un script ayant accès à toutes les variables dans des objets accessibles par script. Pour obtenir une référence de champ à partir d’un objet , faites-la glisser de l’arborescence de droite vers votre script :

![Modifier le jeton de script](assets/edit-script-token.png)

## Incorporation et test de scripts

Une fois que votre script est défini dans un Jeton Mon programme , vous pouvez le référencer dans un e-mail donné à l’aide de l’éditeur d’e-mail de Marketo.

![Script Email](assets/email-script-marketo-email.png)

Vous pouvez tester votre script à l’aide de l’action d’e-mail [!UICONTROL Envoyer un exemple d’e-mail] dans le concepteur d’e-mail Marketo. Pour que le script s’exécute correctement, vous devez sélectionner un prospect existant à emprunter dans le champ [!UICONTROL Lead]. Si vous effectuez un test avec `$TriggerObject`, vous pouvez sélectionner l’objet de déclenchement à l’aide du paramètre [!UICONTROL Trigger]. Cette méthode utilise les données de l’objet de ce type le plus récemment mis à jour comme variable `$TriggerObject`.

![Test du script d’e-mail](assets/velocity-test.png)

Vous pouvez également utiliser l’[!UICONTROL Aperçu de l’e-mail] pour tester votre script. Pour ce faire, vous devez sélectionner **[!UICONTROL Afficher en tant que : Détails du lead]**, puis sélectionner un lead dans une liste statique disponible. Cela a l’avantage supplémentaire de générer des exceptions qui peuvent s’être produites lors de l’exécution du script :

![Afficher l’e-mail sous](assets/view-as.png)

## Conseils utiles

La longueur combinée de tous les jetons de script d’e-mail dans un e-mail donné ne peut pas dépasser 100 000 octets. Cette limite concerne la longueur totale des chaînes de jeton elles-mêmes (et non la longueur totale après le développement des jetons).

- Les variables référencées dans le script de courrier électronique doivent exister dans Marketo sur l’un des objets disponibles pour le script.
- Vous pouvez référencer des objets personnalisés de premier et deuxième niveau provenant de votre CRM nativement intégré directement connectés au lead ou au contact, mais pas des objets personnalisés de troisième niveau. Les objets personnalisés ne peuvent pas être des parents du prospect ou de l&#39;entreprise
- Pour les objets personnalisés Marketo, vous pouvez référencer des objets personnalisés de deuxième niveau avec une relation parent-enfant. Par exemple, `Lead <- Parent <- Child`. Vous ne pouvez pas référencer d’objets personnalisés de deuxième niveau avec une relation Edge-Bridge. par ex., `Lead <- Bridge -> Edge`
- Vous pouvez référencer des objets personnalisés connectés à un prospect, un contact ou un compte, mais pas plus d’un.
- Les objets personnalisés ne peuvent être référencés que par le biais d’une seule connexion, d’un seul lead, contact ou compte
- Vous devez cocher la case dans l’éditeur de script pour les champs que vous utilisez, sans quoi ils ne seront pas traités
- Pour chaque objet personnalisé, les dix enregistrements mis à jour le plus récemment par personne/contact sont disponibles au moment de l’exécution et sont classés du plus récemment mis à jour (à 0) au plus ancien mis à jour (à 9). Vous pouvez augmenter le nombre d&#39;enregistrements disponibles en [suivant les instructions](https://experienceleague.adobe.com/fr/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting).
- Si vous incluez plusieurs scripts d’e-mail dans un e-mail, ils s’exécutent de haut en bas. La portée des variables définies dans le premier script à exécuter sera disponible dans les scripts suivants.
- Référence des outils : [https://velocity.apache.org/tools/2.0/index.html](https://velocity.apache.org/tools/2.0/index.html)
- Une note concernant les jetons qui contiennent des caractères de nouvelle ligne « \\n » ou « \\r\\n ». Lorsqu’un e-mail est envoyé via Envoyer un exemple ou via une campagne par lots, les caractères de nouvelle ligne dans les jetons sont remplacés par des espaces. Lorsque l’e-mail est envoyé via Trigger Campaign, les caractères de nouvelle ligne ne sont pas touchés.
- Pour garantir une analyse correcte des URL, le chemin d’accès complet doit être défini en tant que variable, puis imprimé, et la variable ne doit pas être imprimée dans les références d’URL. Le protocole (http:// ou https://) doit être inclus et doit être distinct du reste de l’URL. L’URL doit également faire partie d’une balise d’ancrage entièrement formée (<a>). Le script doit générer une balise d’ancrage entièrement formée pour que les liens soient suivis. Les liens ne sont pas suivis s’ils sont générés à partir d’une boucle for ou foreach.

```html
<!-- Correct -->
#set($url = "www.example.com/${object.id}")
<a href="http://${url}">Link Text</a>

<!-- Correct -->
<a href="http://www.example.com/${object.id}">Link Text</a>

<!-- Incorrect -->
<a href="${url}">Link Text</a>

<!-- Incorrect -->
<a href="{{my.link}}">Link Text</a>

<!-- Incorrect -->
<a href="http://{{my.link}}">Link Text</a>
```
