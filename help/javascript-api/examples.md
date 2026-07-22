---
title: Exemples
description: Exemples de JavaScript Marketo Forms 2.0 pour masquer ou rediriger lors de l’envoi, de la définition et de la lecture de champs, de la validation avec des erreurs personnalisées, de lightbox et de triggers externes.
feature: Javascript
exl-id: dc5f0cc5-ff5a-48b0-be36-52c10e56f798
TQID: https://experienceleague.adobe.com/dH1yaglpL3odGZfGk-JC8oGljBF2gDpdjdg1BPE6OcQ
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 234
ht-degree: 0%

---

# Exemples

Ces exemples illustrent les workflows de formulaires web Forms 2.0 courants.

## Masquer Le Formulaire Après Un Envoi Réussi

Cet exemple montre comment maintenir le visiteur sur la page active après un envoi réussi. Cette action n’ouvre pas la page de relance et ne recharge pas la page active.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Add an onSuccess handler
    form.onSuccess(function(values, followUpUrl) {
        // Get the form's jQuery element and hide it
        form.getFormElem().hide();
        // Return false to prevent the submission handler from taking the lead to the follow up url
        return false;
    });
});
```

## Emmener le visiteur à l’URL définie par l’utilisateur

Cet exemple montre comment envoyer le visiteur à une URL définie dans JavaScript après un envoi réussi. L’URL de JavaScript remplace la page de remerciement configurée.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    //Add an onSuccess handler
    form.onSuccess(function(values, followUpUrl) {
        // Take the lead to a different page on successful submit, ignoring the form's configured followUpUrl
        location.href = "https://google.com/?q=marketo+forms+v2+examples";
        // Return false to prevent the submission handler continuing with its own processing
        return false;
    });
});
```

## Définir les valeurs des champs de formulaire

Cet exemple montre comment définir des champs de formulaire.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Set the value of the Phone and Country fields
    form.vals({ "Phone":"555-555-1234", "Country":"USA"});
});
```

## Lire les valeurs des champs de formulaire lors de l’envoi du formulaire

Cet exemple montre comment lire les valeurs des champs du formulaire lors de l’envoi du formulaire.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Add an onSubmit handler
    form.onSubmit(function(){
        // Get the form field values
        var vals = form.vals();
        // You may wish to call other function calls here, for example to fire google analytics tracking or the like
        // callSomeFunction(vals);
        // We'll just alert them to show the principle
        alert("Submitted values: " + JSON.stringify(vals));
    });
});
```

## Envoi de formulaire sur un événement de clic non lié au formulaire

Cet exemple montre comment envoyer un formulaire lorsque le visiteur sélectionne un élément en dehors du formulaire.

```javascript
// Load the form normally
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057);

// Find the button element that you want to attach the event to
var btn = document.getElementById("MyAlternativeSubmitButtonId");
btn.onclick = function() {
    // When the button is clicked, get the form object and submit it
    MktoForms2.whenReady(function (form) {
        form.submit();
    });
};
```

## Empêcher un utilisateur d’envoyer un formulaire

Dans cet exemple, le visiteur doit sélectionner le bouton de compteur de clics au moins trois fois avant que le bouton d’envoi du formulaire ne fonctionne.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    // Check if the form is submittable
    if (form.submittable()) {
        // Set it to be non submittable
        form.submittable(false);
    }
});

var clickCount = 0;
// Wire up the click counter button
var clickCounterBtn = document.getElementById("ClickCounter");
clickCounterBtn.onclick = function() {
    clickCount++;
    // Update the buttons's text
    clickCounterBtn.innerHTML = "Click Counter Button ("+clickCount+")";

    if (clickCount >= 3) {
        // Get the form and set it to be submittable
        MktoForms2.whenReady(function (form) {
            form.submittable(true);
        });
    }
};
```

## Définir des valeurs dans les champs masqués du formulaire

Cet exemple montre comment définir des valeurs sur des champs masqués.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    // Set values for the hidden fields, "userIsAwesome" and "enrollDate"
    // Note that these fields were configured in the form editor as hidden fields already
    form.vals({"userIsAwesome":"true", "enrollDate":"2014-01-01"});
});
```

## Afficher le formulaire dans LightBox

Cet exemple montre comment afficher le formulaire dans une boîte de dialogue de style Lightbox lorsque l’URL contient le paramètre `lightboxForm=true`.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    if (location.href.indexOf("lightboxForm=true") != -1) {
        MktoForms2.lightbox(form).show();
    }
});
```

## Afficher le message d&#39;erreur personnalisé

Cet exemple applique une logique commerciale personnalisée lors de l’envoi et affiche un message d’erreur personnalisé lorsque les valeurs ne remplissent pas la condition requise.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    //listen for the validate event
    form.onValidate(function() {
        // Get the values
        var vals = form.vals();
        //Check your condition
        if (vals.Country == "USA" && vals.vehicleSize != "Massive") {
            // Prevent form submission
            form.submittable(false);
            // Show error message, pointed at VehicleSize element
            var vehicleSizeElem = form.getFormElem().find("#vehicleSize");
            form.showErrorMessage("All Americans must have a massive vehicle", vehicleSizeElem);
        }
        else {
            // Enable submission for those who met the criteria
            form.submittable(true);
        }
  });
});
```
