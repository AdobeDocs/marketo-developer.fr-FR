---
title: Exemples
description: Exemples de code Marketo pour la configuration des actions de formulaire
feature: Javascript
exl-id: dc5f0cc5-ff5a-48b0-be36-52c10e56f798
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Exemples

Vous trouverez ci-dessous un ensemble d’exemples de formulaires web Forms 2.0 illustratifs.

## Masquer Le Formulaire Après Un Envoi Réussi

Cet exemple n’emmène pas le visiteur à la page de relance ni ne recharge la page active.

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

Cet exemple dirige le visiteur vers une URL déterminée par JavaScript après un envoi réussi, au lieu de la page de remerciement configurée.

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

Cet exemple lit les champs de formulaire lors de l’envoi du formulaire.

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

Cet exemple montre comment envoyer un formulaire basé sur un événement de clic sur un autre élément ou événement qui ne fait pas partie du formulaire.

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

Dans le cadre de cet exemple, vous devez cliquer sur le bouton compteur de clics au moins trois fois avant que le bouton d’envoi du formulaire ne fonctionne.

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

Cet exemple montre le formulaire dans une boîte de dialogue de style Lightbox si l’URL contient un paramètre `lightboxForm=true`.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    if (location.href.indexOf("lightboxForm=true") != -1) {
        MktoForms2.lightbox(form).show();
    }
});
```

## Afficher le message d&#39;erreur personnalisé

Cet exemple montre un message d’erreur personnalisé lors de l’envoi en fonction de la logique commerciale personnalisée.

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
