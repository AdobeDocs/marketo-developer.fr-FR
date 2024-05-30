---
title: "Référence de l’API Forms"
description: "Référence de l’API Forms"
feature: Forms, Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1327'
ht-degree: 2%

---


# Référence de l’API Forms

Il existe deux objets principaux avec lesquels vous allez interagir à l’aide de l’API Forms 2.0. La variable `MktoForms2` et la variable `Form` . La variable `MktoForms2` est l’espace de noms de niveau supérieur visible publiquement pour la fonctionnalité Forms2 et contient des fonctions pour créer, charger et récupérer des objets de formulaire.

## Méthodes MktoForms2

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Méthode</strong></td>
      <td><strong>Description</strong></td>
      <td><strong>Paramètres</strong></td>
      <td><strong>Retours</strong></td>
    </tr>
    <tr valign="top">
      <td>.loadForm(baseUrl, munchkinId, formId, callback)</td>
      <td>Charge un descripteur de formulaire à partir des serveurs Marketo et crée un objet de formulaire.</td>
      <td> baseUrl(String) - URL de l’instance du serveur Marketo pour votre abonnement</td>
      <td>indéfini</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>munchkinId (chaîne) - Munchkin ID de l’abonnement</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>formId (chaîne ou nombre) - ID de version de formulaire (Vid) du formulaire à charger.</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (facultatif) (fonction) - Fonction de rappel à laquelle transmettre l’objet de formulaire construit une fois qu’il a été chargé et initialisé.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.lightbox(form, opts)</td>
      <td>Génère une boîte de dialogue modale de style Lightbox avec l’objet de formulaire qu’elle contient.</td>
      <td>form (objet de formulaire) : instance d’un objet de formulaire que vous souhaitez rendre dans un cadre lumineux.</td>
      <td>Objet Lightbox avec les méthodes .show() et .hide() .</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>opts (facultatif) (objet) - Objet d’options transmis à l’objet Lightbox.</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>onSuccess(Function) : rappel déclenché lors de l’envoi du formulaire.</td>
      <td></td>
    </tr>
    <tr>
          <td></td>
      <td></td>
      <td>closeBtn(Boolean) default true : contrôle si un bouton de fermeture (X) s’affiche dans la boîte de dialogue Lightbox.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.newForm(formData, callback)</td>
      <td>Crée un objet de formulaire à partir d’un objet JS de descripteur de formulaire. Ajoute une fonction de rappel appelée une fois que toutes les feuilles de style et les informations de piste connues ont été récupérées et que l’objet de formulaire a été créé.</td>
      <td>formData (objet de descripteur de formulaire) - Un objet descripteur de formulaire, tel que créé par Marketo Forms V2 Editor</td>
      <td>indéfini</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (facultatif)(fonction) : ce rappel est appelé avec un seul argument, une instance nouvellement créée de l’objet de formulaire.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.getForm(formId)</td>
      <td>Obtient un objet de formulaire créé précédemment par l’identifiant de formulaire</td>
      <td> formId (nombre ou chaîne) - Identifiant de l’identifiant de l’identifiant de formulaire.</td>
      <td>Objet de formulaire</td>
    </tr>
    <tr valign="top">
      <td>.allForms()</td>
      <td>Récupère un tableau de tous les objets de formulaire qui ont été précédemment construits sur la page.</td>
      <td>s/o</td>
      <td>Tableau d’objet de formulaire</td>
    </tr>
    <tr valign="top">
      <td>.getPageFields()</td>
      <td>Obtient un objet JS contenant des données de l’URL et du référent qui peuvent être intéressantes à des fins de suivi.</td>
      <td>s/o</td>
      <td>Objet</td>
    </tr>
    <tr valign="top">
      <td>.whenReady(callback)</td>
      <td>Ajoute un rappel appelé une seule fois pour chaque formulaire de la page qui devient "prêt". L’état de préparation signifie que le formulaire existe, a été initialement rendu et que ses premiers rappels ont été appelés. S’il existe déjà un formulaire prêt au moment de l’appel de cette fonction, le rappel transmis est appelé immédiatement.</td>
      <td>callback(Fonction) : le rappel est transmis par un seul argument, un objet de formulaire.</td>
      <td>Objet MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.onFormRender(callback)</td>
      <td>Ajoute un rappel qui est appelé chaque fois qu’un formulaire de la page est rendu. Forms est rendu lors de sa création initiale, puis à chaque fois que les règles de visibilité modifient la structure du formulaire.</td>
      <td>callback (fonction) : le rappel est transmis par un seul argument, l’objet de formulaire du formulaire rendu.</td>
      <td>Objet MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.whenRendered(callback)</td>
      <td>Comme onFormRender, un rappel est appelé chaque fois qu’un formulaire est généré. En outre, cela appelle également le rappel immédiatement pour tous les formulaires déjà rendus.</td>
      <td>callback(Fonction) : le rappel est transmis par un seul argument, l’objet de formulaire du formulaire rendu.</td>
      <td></td>
    </tr>
</table>


## Méthodes de formulaire

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Méthode</strong></td>
      <td><strong>Description</strong></td>
      <td><strong>Paramètres</strong></td>
      <td><strong>Retours</strong></td>
    </tr>
    <tr valign="top">
      <td>.render(formElem)</td>
      <td>Génère un objet de formulaire, renvoyant un objet jQuery encapsulant un élément de formulaire contenant le formulaire. S’il transmet un élément formElem, il l’utilise comme élément de formulaire, sinon il en crée un nouveau.</td>
      <td>formElem (facultatif) : élément de formulaire encapsulé d’objet jQuery dans lequel effectuer le rendu.</td>
      <td> Elément de formulaire encapsulé d’objet jQuery contenant le formulaire rendu.</td>
    </tr>
    <tr valign="top">
      <td>.getId()</td>
      <td>Obtient l’identifiant du formulaire.</td>
      <td>s/o</td>
      <td>Nombre : identifiant de l’objet de formulaire représenté par ce formulaire.</td>
    </tr>
    <tr valign="top">
      <td>.getFormElem()</td>
      <td>Obtient l’élément de formulaire jQuery encapsulé d’un formulaire rendu.</td>
      <td>s/o</td>
      <td>Elément de formulaire encapsulé d’objet jQuery ou null si le formulaire n’a pas encore été rendu avec la méthode render() .</td>
    </tr>
    <tr valign="top">
      <td>.validate()</td>
      <td>Force le formulaire à valider, surligne les erreurs qui peuvent se produire et renvoie le résultat. N’envoie pas le formulaire.</td>
      <td>s/o</td>
      <td>Booléen : renvoie true si tous les validateurs du formulaire sont transmis, false dans le cas contraire.</td>
    </tr>
    <tr valign="top">
      <td>.onValidate(callback)</td>
      <td>Ajoute un rappel de validation qui sera appelé chaque fois que la validation est déclenchée.</td>
      <td>callback(Fonction) : rappel qui sera déclenché chaque fois que la validation se produit. Le rappel est transmis par un paramètre, une valeur booléenne indiquant si la validation a réussi.</td>
      <td>Objet de formulaire : objet de formulaire sur lequel la méthode a été appelée, à des fins de chaîne.</td>
    </tr>
    <tr valign="top">
      <td>.submit()</td>
      <td>Déclenche l’événement d’envoi du formulaire. Cela lancera le flux d’envoi à partir de l’envoi, en effectuant la validation, en déclenchant tout événement onSubmit, en envoyant le formulaire et en déclenchant tout événement onSuccess si l’envoi du formulaire a réussi.</td>
      <td>s/o</td>
      <td>Objet de formulaire : objet de formulaire sur lequel la méthode a été appelée, à des fins de chaîne.</td>
    </tr>
    <tr valign="top">
      <td>.onSubmit(callback)</td>
      <td>Ajoute un rappel qui sera appelé lors de l’envoi du formulaire. Cela est déclenché lorsque l’envoi commence, avant que la réussite/l’échec de la requête ne soit connu.</td>
      <td>callback : fonction qui sera appelée lors de l’envoi du formulaire. Ce rappel sera transmis par un argument, cet objet de formulaire.</td>
      <td>Objet de formulaire : objet de formulaire sur lequel la méthode a été appelée, à des fins de chaîne.</td>
    </tr>
    <tr valign="top">
      <td>.onSuccess(callback)</td>
      <td>Ajoute un rappel qui sera appelé lorsque le formulaire a été envoyé avec succès, mais avant que la piste ne soit transférée vers la page de suivi. Peut être utilisé pour empêcher le transfert de la piste vers la page de suivi après un envoi réussi.</td>
      <td>callback : fonction qui sera appelée lorsque le formulaire a été envoyé avec succès. Ce rappel sera transmis par deux arguments. Objet JS contenant les valeurs qui ont été envoyées et une URL de chaîne de la page de suivi vers laquelle l’utilisateur sera transféré, ou chaîne nulle ou vide si aucune page de relance n’est configurée. Comportement spécial : si ce rappel renvoie "false" (mesuré à l’aide de ===), le visiteur NE sera PAS transféré vers la page de suivi et la page NE sera PAS rechargée. Cela permet à l’implémentateur d’effectuer un traitement supplémentaire sur l’URL de suivi ou d’effectuer une action sur la page à l’aide de JavaScript au lieu de quitter la page.</td>
      <td>Objet de formulaire : objet de formulaire sur lequel la méthode a été appelée, à des fins de chaîne.</td>
    </tr>
    <tr valign="top">
      <td>.submittable(canSubmit) <em>également disponible sous :</em> <em>.submitable(canSubmit)</em></td>
      <td>Obtient ou définit si le formulaire peut être envoyé. Appelé sans argument, il obtient la valeur, si appelé avec un argument, il définit la valeur. Elle peut être utilisée pour empêcher l’envoi d’un formulaire alors que d’autres critères en dehors du formulaire normal doivent être remplis.</td>
      <td>canSubmit (facultatif)(booléen) : définit le formulaire à soumettre ou à ne pas soumettre.</td>
      <td>Booléen ou objet de formulaire : s’il est appelé sans argument, renvoie une valeur booléenne indiquant si le formulaire est soumis. S’il est appelé avec un argument, renvoie cet objet de formulaire à des fins de chaîne. </td>
    </tr>
    <tr valign="top">
      <td>.allFieldsFill()</td>
      <td>Renvoie "true" si des valeurs non vides sont définies pour tous les champs du formulaire.</td>
      <td>s/o</td>
      <td>Booléen : vrai si tous les champs ont des valeurs non vides/vides/non définies/nulles, faux dans le cas contraire.</td>
    </tr>
    <tr valign="top">
      <td>.setValues(vals)</td>
      <td>Définit des valeurs sur un ou plusieurs champs du formulaire.</td>
      <td>vals - Objet JS. Pour chaque paire clé/valeur de l’objet, le champ de formulaire nommé clé est défini sur valeur.</td>
      <td>indéfini</td>
    </tr>
    <tr valign="top">
      <td>.getValues()</td>
      <td>Obtient toutes les valeurs de tous les champs du formulaire.</td>
      <td>s/o</td>
      <td>Objet : objet JS contenant des paires clé/valeur représentant les noms et les valeurs des champs du formulaire.</td>
    </tr>
    <tr valign="top">
      <td>.addHiddenFields(values)</td>
      <td>Ajoute des champs input type=hidden au formulaire.</td>
      <td>values : objet JS contenant des paires clé/valeur représentant les noms et les valeurs des champs masqués à ajouter au formulaire.</td>
      <td>indéfini</td>
    </tr>
    <tr valign="top">
      <td>.vals(values)</td>
      <td>jQuery style .vals() setter/getter. S’il est appelé sans argument, il équivaut à appeler getValues(). S’il est appelé avec un argument, il équivaut à appeler setValues()</td>
      <td>values (facultatif) - Object</td>
      <td>indéfini</td>
    </tr>
    <tr valign="top">
      <td>.showErrorMessage(msg, elem)</td>
      <td>Affiche un message d’erreur, pointant vers elem.</td>
      <td>msg (chaîne de HTML) - Chaîne contenant le texte de l’erreur que vous souhaitez afficher.</td>
            <td>Objet de formulaire : objet de formulaire, à des fins de chaîne.</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>elem (facultatif) (objet jQuery) : élément vers lequel l’erreur doit pointer. Si cette option est désactivée, le bouton d’envoi du formulaire est utilisé.</td>
<td></td>
    </tr>
  </tbody>
</table>
