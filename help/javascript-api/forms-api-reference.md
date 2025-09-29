---
title: Référence de l’API Forms
description: Référence complète pour l’API Marketo Forms 2.0, détaillant les méthodes MktoForms2 et Form, les paramètres, les rappels et les retours pour le chargement et le rendu des formulaires.
feature: Forms, Javascript
exl-id: 0f8d242f-0b27-4087-b080-3d41ebaa25b3
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1345'
ht-degree: 2%

---

# Référence de l’API Forms

Vous interagirez avec deux objets principaux à l’aide de l’API Forms 2.0. L’objet `MktoForms2` et l’objet `Form`. L’objet `MktoForms2` est l’espace de noms de niveau supérieur visible publiquement pour la fonctionnalité Forms2 et contient des fonctions pour créer, charger et récupérer des objets de formulaire.

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
      <td>Charge un descripteur de formulaire depuis les serveurs Marketo et crée un objet de formulaire.</td>
      <td> baseUrl(String) - URL de l’instance de serveur Marketo pour votre abonnement</td>
      <td>indéfini</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>munchkinId (chaîne) : ID Munchkin de l’abonnement</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>formId (chaîne ou nombre) - ID de version de formulaire (Vid) du formulaire à charger</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (facultatif) (Fonction) - Fonction de rappel à laquelle transmettre l’objet de formulaire construit une fois qu’il a été chargé et initialisé.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.lightbox(form, opts)</td>
      <td>Rend une boîte de dialogue modale de style Lightbox contenant l’objet Form.</td>
      <td>form (objet de formulaire) - Instance d’un objet de formulaire que vous souhaitez rendre dans une lightbox.</td>
      <td>Objet Lightbox avec les méthodes .show() et .hide().</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>opts (optional)(Object) - Objet d’options transmises à l’objet Lightbox</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>onSuccess(Function) - Un rappel déclenché lors de l’envoi du formulaire.</td>
      <td></td>
    </tr>
    <tr>
          <td></td>
      <td></td>
      <td>closeBtn(Boolean) default true - Contrôle si un bouton de fermeture (X) est affiché dans la boîte de dialogue Lightbox.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.newForm(formData, callback)</td>
      <td>Crée un nouvel objet Form à partir d’un objet JS de descripteur de formulaire. Ajoute une fonction de rappel qui est appelée une fois que toutes les feuilles de style et les informations de prospect connues ont été récupérées et que l’objet Form a été créé.</td>
      <td>formData (objet Descripteur de formulaire) : objet descripteur de formulaire, tel que créé par l’éditeur Forms V2 de Marketo</td>
      <td>indéfini</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (facultatif)(Fonction) - Ce rappel est appelé avec un seul argument, une instance nouvellement créée de l’objet Form.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.getForm(formId)</td>
      <td>Obtient un objet Form créé précédemment par identifiant de formulaire</td>
      <td> formId (Nombre ou Chaîne) - Identifiant du formulaire valide.</td>
      <td>Objet de formulaire</td>
    </tr>
    <tr valign="top">
      <td>.allForms()</td>
      <td>Récupère un tableau de tous les objets de formulaire qui ont été précédemment construits sur la page.</td>
      <td>s/o</td>
      <td>Tableau d’objets de formulaire</td>
    </tr>
    <tr valign="top">
      <td>.getPageFields()</td>
      <td>Obtient un objet JS contenant des données de l’URL et du référent qui peuvent être intéressantes à des fins de suivi.</td>
      <td>s/o</td>
      <td>Objet</td>
    </tr>
    <tr valign="top">
      <td>.whenReady(callback)</td>
      <td>Ajoute un rappel qui est appelé exactement une fois pour chaque formulaire de la page qui devient « ready ». La préparation signifie que le formulaire existe, qu’il a été initialement rendu et que ses rappels initiaux ont été appelés. Si un formulaire est déjà prêt au moment de l’appel de cette fonction, le rappel transmis est appelé immédiatement.</td>
      <td>callback(Function) - Le rappel est transmis à un seul argument, un objet de formulaire.</td>
      <td>MktoForms2, objet</td>
    </tr>
    <tr valign="top">
      <td>.onFormRender(callback)</td>
      <td>Ajoute un rappel qui est appelé à chaque fois qu’un formulaire est rendu sur la page. Les Forms sont rendues lors de leur création initiale, puis à chaque fois que les règles de visibilité modifient la structure du formulaire.</td>
      <td>callback (fonction) - Un seul argument est transmis au rappel : il s’agit de l’objet du formulaire dont le rendu a été effectué.</td>
      <td>MktoForms2, objet</td>
    </tr>
    <tr valign="top">
      <td>.whenRendered(callback)</td>
      <td>Comme onFormRender, cela ajoute un rappel qui est appelé à chaque fois qu’un formulaire est rendu. En outre, cela appelle également immédiatement le rappel pour tous les formulaires qui ont déjà été rendus.</td>
      <td>callback(Function) - Un seul argument est transmis au rappel, l’objet de formulaire du formulaire rendu.</td>
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
      <td>.render(formElement)</td>
      <td>Rend un objet de formulaire, renvoyant un objet jQuery enveloppant un élément de formulaire contenant le formulaire. Si un formElement est transmis, il l’utilise comme élément de formulaire, sinon il en crée un nouveau.</td>
      <td>formElement (facultatif) - Élément de formulaire enveloppé d’un objet jQuery dans lequel effectuer le rendu.</td>
      <td> Un élément de formulaire enveloppé d’un objet jQuery contenant le formulaire rendu.</td>
    </tr>
    <tr valign="top">
      <td>.getId()</td>
      <td>Obtient l'identifiant du formulaire.</td>
      <td>s/o</td>
      <td>Nombre - Identifiant de l’objet de formulaire que ce formulaire représente</td>
    </tr>
    <tr valign="top">
      <td>.getFormElement()</td>
      <td>Obtient l'élément jQuery wrapped form d'un formulaire rendu.</td>
      <td>s/o</td>
      <td>Un élément de formulaire enveloppé d’un objet jQuery ou la valeur null si le formulaire n’a pas encore été rendu avec la méthode render().</td>
    </tr>
    <tr valign="top">
      <td>.validate()</td>
      <td>Force la validation du formulaire, en mettant en surbrillance les erreurs éventuelles et en renvoyant le résultat. N’envoie pas le formulaire.</td>
      <td>s/o</td>
      <td>Booléen - Renvoie true si tous les validateurs du formulaire ont été transmis, false dans le cas contraire.</td>
    </tr>
    <tr valign="top">
      <td>.onValidate(callback)</td>
      <td>Ajoute un rappel de validation qui sera appelé chaque fois que la validation sera déclenchée.</td>
      <td>callback(Function) - Rappel qui sera déclenché chaque fois que la validation aura lieu. Un paramètre est transmis au rappel, une valeur booléenne indiquant si la validation a réussi.</td>
      <td>Objet de formulaire - Objet de formulaire identique à celui sur lequel la méthode a été appelée, à des fins de chaîne.</td>
    </tr>
    <tr valign="top">
      <td>.submit()</td>
      <td>Déclenche l’événement d’envoi du formulaire. Cela lancera le flux d’envoi du formulaire, exécutera la validation, déclenchera tous les événements onSubmit, enverra le formulaire et déclenchera tous les événements onSuccess si l’envoi du formulaire a réussi.</td>
      <td>s/o</td>
      <td>Objet de formulaire - Objet de formulaire identique à celui sur lequel la méthode a été appelée, à des fins de chaîne.</td>
    </tr>
    <tr valign="top">
      <td>.onSubmit(callback)</td>
      <td>Ajoute un rappel qui sera appelé lors de l’envoi du formulaire. Cette erreur se déclenche lorsque l’envoi commence, avant que la réussite ou l’échec de la requête ne soit connu.</td>
      <td>callback : fonction qui sera appelée lors de l’envoi du formulaire. Un seul argument, cet objet Form, sera transmis à ce rappel.</td>
      <td>Objet de formulaire - Objet de formulaire identique à celui sur lequel la méthode a été appelée, à des fins de chaîne.</td>
    </tr>
    <tr valign="top">
      <td>.onSuccess(callback)</td>
      <td>Ajoute un rappel qui sera appelé lorsque le formulaire a bien été envoyé mais avant que le prospect ne soit transféré vers la page de relance. Peut être utilisé pour empêcher le transfert du prospect vers la page de suivi après un envoi réussi.</td>
      <td>callback : fonction qui sera appelée une fois le formulaire envoyé avec succès. Deux arguments seront transmis à ce rappel. Un objet JS contenant les valeurs envoyées et une URL de chaîne de la page de relance vers laquelle l’utilisateur sera transféré, ou une chaîne nulle ou vide s’il n’existe aucune page de relance configurée. Comportement spécial : si ce rappel renvoie « false » (mesuré à l’aide de ===), le visiteur ne sera PAS redirigé vers la page de suivi et la page ne sera PAS rechargée. Cela permet à l’implémentateur d’effectuer un traitement supplémentaire sur l’URL de relance ou d’agir sur la page à l’aide de JavaScript au lieu de quitter la page.</td>
      <td>Objet de formulaire - Objet de formulaire identique à celui sur lequel la méthode a été appelée, à des fins de chaîne.</td>
    </tr>
    <tr valign="top">
      <td>.submittable(canSubmit) <em>également disponible sous :</em> <em>.submitable(canSubmit)</em></td>
      <td>Obtient ou définit si le formulaire peut être envoyé. Si elle est appelée sans argument, elle obtient la valeur, si elle est appelée avec un argument, elle définit la valeur. Cela peut être utilisé pour empêcher l’envoi d’un formulaire tandis que d’autres critères en dehors du formulaire normal doivent être remplis.</td>
      <td>canSubmit (facultatif)(booléen) : définit le formulaire comme pouvant être envoyé ou non envoyé.</td>
      <td>Booléen ou objet de formulaire : si cette propriété est appelée sans argument, renvoie une valeur booléenne indiquant si le formulaire peut être envoyé. Si elle est appelée avec un seul argument, renvoie cet objet Form à des fins de chaîne. </td>
    </tr>
    <tr valign="top">
      <td>.allFieldsFilled()</td>
      <td>Renvoie « true » si des valeurs non vides sont définies pour tous les champs du formulaire.</td>
      <td>s/o</td>
      <td>Booléen : valeur true si tous les champs ont des valeurs non vides, vides, non définies ou nulles, false dans le cas contraire.</td>
    </tr>
    <tr valign="top">
      <td>.setValues(vals)</td>
      <td>Définit des valeurs sur un ou plusieurs champs du formulaire.</td>
      <td>vals - Objet JS. Pour chaque paire clé/valeur dans l’objet, le champ de formulaire nommé clé est défini sur valeur.</td>
      <td>indéfini</td>
    </tr>
    <tr valign="top">
      <td>.getValues()</td>
      <td>Obtient toutes les valeurs de tous les champs du formulaire.</td>
      <td>s/o</td>
      <td>Objet - Objet JS contenant des paires clé/valeur représentant les noms et les valeurs des champs du formulaire.</td>
    </tr>
    <tr valign="top">
      <td>.addHiddenFields(values)</td>
      <td>Ajoute les champs de type d’entrée=masqués au formulaire.</td>
      <td>values - Un objet JS contenant des paires clé/valeur représentant les noms et les valeurs des champs masqués à ajouter au formulaire.</td>
      <td>indéfini</td>
    </tr>
    <tr valign="top">
      <td>.vals(values)</td>
      <td>jQuery style .vals() setter/getter. Si elle est appelée sans argument, équivaut à appeler getValues(). Si elle est appelée avec un argument, équivaut à appeler setValues()</td>
      <td>values (facultatif) - Objet</td>
      <td>indéfini</td>
    </tr>
    <tr valign="top">
      <td>.showErrorMessage(msg, element)</td>
      <td>Affiche un message d’erreur, pointant vers l’élément .</td>
      <td>msg (String of HTML) : chaîne contenant le texte de l’erreur à afficher.</td>
            <td>Objet de formulaire - Cet objet de formulaire, à chaîner.</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>elem (facultatif)(objet jQuery) : élément vers lequel l’erreur doit pointer. Si cette option n’est pas définie, le bouton d’envoi du formulaire est utilisé.</td>
<td></td>
    </tr>
  </tbody>
</table>
