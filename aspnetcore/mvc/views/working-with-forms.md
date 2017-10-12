---
title: Helper di tag nei form in ASP.NET Core
author: rick-anderson
description: Descrive gli helper di Tag utilizzati con i moduli predefiniti.
keywords: Helper di tag ASP.NET Core, l'Helper di Tag, Form HTML nel form
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff6fee6eee539fc77b6c6180a816daa760202848
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a>Introduzione all'utilizzo di helper di tag nei form in ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), e [Jerrie Pelser](https://github.com/jerriep)

Questo documento viene illustrato l'utilizzo di moduli e gli elementi HTML comunemente usati in un Form. Il codice HTML [modulo](https://www.w3.org/TR/html401/interact/forms.html) elemento fornisce l'uso di App web meccanismo principale per inviare dati al server. La maggior parte di questo documento descrive [gli helper di Tag](tag-helpers/intro.md) e come consentono in modo produttivo creare form HTML affidabile. È consigliabile leggere [introduzione per gli helper di Tag](tag-helpers/intro.md) prima di leggere questo documento.

In molti casi, l'helper HTML forniscono un'alternativa a un Helper Tag specifici, ma è importante tenere presente che gli helper di Tag non sostituiscono l'helper HTML e non esiste un Helper di Tag per ogni HTML Helper. Se esiste un'alternativa di HTML Helper, è già indicato.

<a name=my-asp-route-param-ref-label></a>

## <a name="the-form-tag-helper"></a>L'Helper di Tag di Form

Il [modulo](https://www.w3.org/TR/html401/interact/forms.html) Helper di Tag:

* Genera il codice HTML [ \<FORM >](https://www.w3.org/TR/html401/interact/forms.html) `action` valore di attributo per un'azione del controller MVC o una route denominata

* Genera l'errore nascosta [richiesta verifica Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) per impedire richieste intersito false (se usato con il `[ValidateAntiForgeryToken]` attributo nel metodo di azione HTTP Post)

* Fornisce il `asp-route-<Parameter Name>` attributo, in cui `<Parameter Name>` viene aggiunto per i valori della route. Il `routeValues` parametri `Html.BeginForm` e `Html.BeginRouteForm` offrono funzionalità simili.

* È un'alternativa di HTML Helper `Html.BeginForm` e`Html.BeginRouteForm`

Esempio:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

L'Helper di Tag Form precedente genera il codice HTML seguente:

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

Il runtime MVC viene generato il `action` valore dell'attributo dagli attributi, gli Helper di Tag Form `asp-controller` e `asp-action`. L'Helper di Tag Form genera inoltre nascosta [richiesta verifica Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) per impedire richieste intersito false (se usato con il `[ValidateAntiForgeryToken]` attributo nel metodo di azione HTTP Post). La protezione di un HTML Form puro da richieste intersito false è difficile, l'Helper di Tag Form offre questo servizio per l'utente.

### <a name="using-a-named-route"></a>Utilizzo di una route denominata

Il `asp-route` attributo Helper di Tag possa anche generare codice per l'HTML `action` attributo. Un'app con un [route](../../fundamentals/routing.md) denominato `register` Impossibile utilizzare il markup seguente per la pagina di registrazione:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

Molte delle visualizzazioni di *viste/Account* cartella (generato quando si crea una nuova app web con *singoli account utente di*) contengono il [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attributo:

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
>Con i modelli predefiniti, `returnUrl` solo viene popolato automaticamente quando si tenta di accedere a una risorsa autorizzata ma non autenticate o autorizzate. Quando si tenta l'accesso non autorizzato, il middleware di sicurezza si viene reindirizzati alla pagina di accesso con il `returnUrl` impostato.

## <a name="the-input-tag-helper"></a>L'Helper di Tag di Input

L'Helper di Tag di Input viene associata a un elemento HTML [ \<input >](https://www.w3.org/wiki/HTML/Elements/input) elemento da un'espressione di modello nella propria visualizzazione razor.

Sintassi:

```HTML
<input asp-for="<Expression Name>" />
```

L'Helper di Tag di Input:

* Genera il `id` e `name` attributi HTML per il nome dell'espressione specificata nel `asp-for` attributo. `asp-for="Property1.Property2"` è equivalente a `m => m.Property1.Property2`. Il nome dell'espressione viene utilizzato per il `asp-for` valore dell'attributo. Vedere il [i nomi delle espressioni](#expression-names) sezione per ulteriori informazioni.

* Imposta il codice HTML `type` valore in base al tipo di modello dell'attributo e [annotazione dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) gli attributi applicati per la proprietà del modello

* Il codice HTML non comporta la sovrascrittura `type` quando è specificato un valore dell'attributo

* Genera l'errore [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) gli attributi di convalida da [annotazione dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) gli attributi applicati alle proprietà del modello

* Dispone di una funzione HTML Helper si sovrappongono con `Html.TextBoxFor` e `Html.EditorFor`. Vedere il **alternative HTML Helper per l'Helper di Tag di Input** sezione per informazioni dettagliate.

* Fornisce la tipizzazione forte. Se il nome di proprietà viene modificato e non aggiorna l'Helper di Tag si otterrà un errore simile al seguente:

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

Il `Input` Helper di Tag imposta il codice HTML `type` attributo in base al tipo di .NET. Nella tabella seguente sono elencati alcuni tipi .NET comuni e un tipo HTML generato (non tutti i tipi .NET sono elencato).

|Tipo .NET|Tipo di input|
|---|---|
|Bool|tipo = "checkbox"|
|Stringa|tipo = "text"|
|DateTime|tipo = "datetime"|
|Byte|tipo = "number"|
|Int|tipo = "number"|
|Single, Double|tipo = "number"|


La tabella seguente illustra alcuni comuni [le annotazioni dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) gli attributi che l'helper di tag di input verrà eseguito il mapping a tipi specifici di input (non tutti gli attributi di convalida sono elencato):


|Attributo|Tipo di input|
|---|---|
|[EmailAddress]|tipo = "email"|
|[Url]|tipo = "url"|
|[HiddenInput]|tipo = "hidden"|
|[Phone]|tipo = "tel"|
|[DataType(DataType.Password)]| tipo = "password"|
|[DataType(DataType.Date)]| tipo = "Data"|
|[DataType(DataType.Time)]| tipo = "ora"|


Esempio:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

Il codice precedente genera il codice HTML seguente:

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

Le annotazioni dei dati applicate al `Email` e `Password` proprietà generano metadati sul modello. L'Helper di Tag di Input utilizza i metadati del modello e produce [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributi (vedere [la convalida del modello](../models/validation.md)). Tali attributi descrivono i validator da associare ai campi di input. In questo modo HTML5 non intrusivi e [jQuery](https://jquery.com/) convalida. Gli attributi non intrusivi sono del formato `data-val-rule="Error Message"`, in cui regola è il nome della regola di convalida (ad esempio `data-val-required`, `data-val-email`, `data-val-maxlength`, ecc.) Se un messaggio di errore viene fornito nell'attributo, viene visualizzato come valore per il `data-val-rule` attributo. Esistono anche gli attributi della maschera `data-val-ruleName-argumentName="argumentValue"` che forniscono dettagli aggiuntivi sulla regola, ad esempio, `data-val-maxlength-max="1024"` .

### <a name="html-helper-alternatives-to-input-tag-helper"></a>Alternative di HTML Helper per l'Helper di Tag di Input

`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` e `Html.EditorFor` sovrapposti funzionalità con l'Helper di Tag di Input. L'Helper di Tag di Input verrà impostato automaticamente il `type` attributo; `Html.TextBox` e `Html.TextBoxFor` No. `Html.Editor`e `Html.EditorFor` gestire raccolte, gli oggetti complessi e i modelli; non l'Helper di Tag di Input. L'Helper di Tag di Input, `Html.EditorFor` e `Html.TextBoxFor` sono fortemente tipizzati (usano le espressioni lambda); `Html.TextBox` e `Html.Editor` non (utilizzano i nomi delle espressioni).

### <a name="htmlattributes"></a>HtmlAttributes

`@Html.Editor()`e `@Html.EditorFor()` utilizzare una speciale `ViewDataDictionary` voce denominata `htmlAttributes` durante l'esecuzione dei modelli predefiniti. Questo comportamento è facoltativamente ampliato usando `additionalViewData` parametri. La chiave "htmlAttributes" è tra maiuscole e minuscole. La chiave "htmlAttributes" viene gestita in modo analogo al `htmlAttributes` oggetto passato a input helper come `@Html.TextBox()`.

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a>Nomi delle espressioni

Il `asp-for` valore dell'attributo è un `ModelExpression` e il lato destro di un'espressione lambda. Pertanto, `asp-for="Property1"` diventa `m => m.Property1` nel codice generato motivo per cui non è necessario prefisso `Model`. È possibile utilizzare il « @ » caratteri per avviare un'espressione inline e spostare prima la `m.`:

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

Genera quanto segue:

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

Con le proprietà di raccolta, `asp-for="CollectionProperty[23].Member"` genera lo stesso nome `asp-for="CollectionProperty[i].Member"` quando `i` ha il valore `23`.

### <a name="navigating-child-properties"></a>Proprietà di navigazione figlio

È anche possibile accedere alle proprietà figlio utilizzando il percorso della proprietà del modello di visualizzazione. Si consideri una classe modello più complessa che contiene un elemento figlio `Address` proprietà.

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

Nella visualizzazione, è associare `Address.AddressLine1`:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

Il codice HTML seguente viene generato per `Address.AddressLine1`:

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a>I nomi delle espressioni e raccolte

Esempio, un modello contenente una matrice di `Colors`:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

Il metodo di azione:

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

Razor seguente viene illustrata la modalità di accesso di un oggetto specifico `Color` elemento:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

Il *Views/Shared/EditorTemplates/String.cshtml* modello:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

Utilizzo di esempio `List<T>`:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

Razor seguente viene illustrato come scorrere una raccolta:

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

Il *Views/Shared/EditorTemplates/ToDoItem.cshtml* modello:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
>Utilizzare sempre `for` (e *non* `foreach`) per eseguire l'iterazione su un elenco. La valutazione di un indicizzatore in LINQ espressione può essere costoso e deve essere ridotta a icona.

&nbsp;

>[!NOTE]
>Il codice di esempio commentato precedente viene illustrato come sostituire l'espressione lambda con la `@` operatore per accedere a ogni `ToDoItem` nell'elenco.

## <a name="the-textarea-tag-helper"></a>L'Helper di Tag Textarea

Il `Textarea Tag Helper` helper di tag è simile all'Helper di Tag di Input.

* Genera il `id` e `name` gli attributi e gli attributi di convalida dei dati dal modello per un [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elemento.

* Fornisce la tipizzazione forte.

* In alternativa HTML Helper:`Html.TextAreaFor`

Esempio:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

Viene generato il codice HTML seguente:

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a>L'Helper di Tag di etichetta

* Genera la didascalia dell'etichetta e `for` dell'attributo su un [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) elemento per un nome di espressione

* In alternativa HTML Helper: `Html.LabelFor`.

Il `Label Tag Helper` offre i vantaggi seguenti su un elemento HTML label puro:

* Si ottiene automaticamente il valore di etichetta descrittiva dal `Display` attributo. Il nome visualizzato desiderato potrebbe cambiare nel tempo e la combinazione di `Display` attributo e Helper di Tag di etichetta verrà applicata la `Display` ovunque viene utilizzato.

* Meno markup nel codice sorgente

* Tipizzazione forte con la proprietà del modello.

Esempio:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

Il codice HTML seguente viene generato per il `<label>` elemento:

```HTML
<label for="Email">Email Address</label>
```

L'Helper di Tag etichetta generato il `for` valore di "Email", che è l'ID dell'attributo è associato il `<input>` elemento. Gli helper di Tag generare coerente `id` e `for` elementi affinché possano essere associati correttamente. La didascalia in questo esempio proviene dal `Display` attributo. Se il modello non contiene un `Display` attributo, la didascalia sarà il nome della proprietà dell'espressione.

## <a name="the-validation-tag-helpers"></a>Gli helper di Tag di convalida

Sono disponibili due helper di Tag di convalida. Il `Validation Message Tag Helper` (che viene visualizzato un messaggio di convalida per una singola proprietà nel modello) e `Validation Summary Tag Helper` (Visualizza un riepilogo degli errori di convalida). Il `Input Tag Helper` aggiunge attributi di convalida sul lato client HTML5 per gli elementi in base ai dati gli attributi di annotazione sulle classi del modello di input. Viene inoltre eseguita la convalida sul server. L'Helper di Tag di convalida vengono visualizzati questi messaggi di errore quando si verifica un errore di convalida.

### <a name="the-validation-message-tag-helper"></a>L'Helper di Tag di messaggio di convalida

* Aggiunge il [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` attributo la [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento, che collega i messaggi di errore di convalida nel campo di input della proprietà del modello specificato. Quando si verifica un errore di convalida sul lato client, [jQuery](https://jquery.com/) viene visualizzato il messaggio di errore nel `<span>` elemento.

* La convalida avviene anche nel server. I client debba JavaScript disabilitato e una convalida può essere eseguita solo sul lato server.

* In alternativa HTML Helper:`Html.ValidationMessageFor`

Il `Validation Message Tag Helper` viene utilizzato con il `asp-validation-for` attributo HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento.

```HTML
<span asp-validation-for="Email"></span>
```

L'Helper di Tag di messaggio di convalida viene generato il codice HTML seguente:

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

In genere viene utilizzata la `Validation Message Tag Helper` dopo un `Input` Helper di Tag per la stessa proprietà. In questo modo consente di visualizzare eventuali messaggi di errore di convalida in prossimità di input che ha causato l'errore.

> [!NOTE]
> È necessario disporre di una vista con il codice JavaScript corretto e [jQuery](https://jquery.com/) riferimenti agli script sul posto per la convalida lato client. Vedere [la convalida del modello](../models/validation.md) per ulteriori informazioni.

Se (ad esempio quando si utilizza una convalida sul lato server personalizzato o viene disabilitata la convalida lato client), si verifica un errore di convalida sul lato server, MVC inserisce il messaggio di errore come corpo del metodo di `<span>` elemento.

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a>L'Helper di convalida Summary (tag)

* Destinazioni `<div>` gli elementi con il `asp-validation-summary` attributo

* In alternativa HTML Helper:`@Html.ValidationSummary`

Il `Validation Summary Tag Helper` viene utilizzato per visualizzare un riepilogo dei messaggi di convalida. Il `asp-validation-summary` valore dell'attributo può essere uno dei seguenti:

|riepilogo di convalida ASP|Messaggi di convalida visualizzati|
|--- |--- |
|ValidationSummary.All|Livello di proprietà e il modello|
|ValidationSummary.ModelOnly|Modello|
|ValidationSummary.None|Nessuno|

### <a name="sample"></a>Esempio

Nell'esempio seguente, il modello di dati è decorato con `DataAnnotation` attributi, che genera i messaggi di errore di convalida per il `<input>` elemento.  Quando si verifica un errore di convalida, l'Helper di Tag di convalida viene visualizzato il messaggio di errore:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

Il codice HTML generato (quando il modello è valido):

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a>L'Helper di Tag di selezione

* Genera l'errore [selezionare](https://www.w3.org/wiki/HTML/Elements/select) e [opzione](https://www.w3.org/wiki/HTML/Elements/option) elementi per le proprietà del modello.

* È un'alternativa di HTML Helper `Html.DropDownListFor` e`Html.ListBoxFor`

Il `Select Tag Helper` `asp-for` specifica il nome della proprietà di modello per il [selezionare](https://www.w3.org/wiki/HTML/Elements/select) elemento e `asp-items` specifica il [opzione](https://www.w3.org/wiki/HTML/Elements/option) elementi.  Ad esempio:

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

Esempio:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

Il `Index` metodo inizializza la `CountryViewModel`, imposta il paese selezionato e lo passa al `Index` visualizzazione.

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

HTTP POST `Index` metodo visualizza la selezione:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

Il `Index` Vista:

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

Che genera il codice HTML seguente (con "CA" selezionata):

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> Non è consigliabile utilizzare `ViewBag` o `ViewData` con l'Helper di Tag selezionare. Un modello di visualizzazione è più affidabile di fornire metadati MVC e in genere meno problematici.

Il `asp-for` valore dell'attributo è un caso speciale e non richiede un `Model` prefisso, non altri attributi Helper di Tag (ad esempio `asp-items`)

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a>Associazione di enum

Spesso è consigliabile usare `<select>` con un `enum` proprietà e generare il `SelectListItem` gli elementi dal `enum` valori.

Esempio:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

Il `GetEnumSelectList` metodo genera un `SelectList` per un enum.

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

È possibile decorare all'elenco di enumeratore con il `Display` attributo per ottenere un'interfaccia utente di più dettagliata:

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

Viene generato il codice HTML seguente:

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a>Gruppo di opzioni

Il codice HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) elemento viene generato quando il modello di visualizzazione contiene uno o più `SelectListGroup` oggetti.

Il `CountryViewModelGroup` gruppi di `SelectListItem` elementi nei gruppi "North America" e "Europe":

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

I due gruppi sono illustrati di seguito:

![esempio di gruppo di opzione](working-with-forms/_static/grp.png)

Il codice HTML generato:

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a>Selezione multipla

L'Helper di Tag selezionare genererà automaticamente le [più = "più"](http://w3c.github.io/html-reference/select.html) attributo se la proprietà specificata nel `asp-for` attributo è un `IEnumerable`. Ad esempio, dato il modello seguente:

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

Con la vista seguente:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

Genera il codice HTML seguente:

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a>Nessuna selezione

Se è necessario utilizzare l'opzione "non è specificato" in più pagine, è possibile creare un modello per eliminare il codice HTML di ripetizione:

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

Il *Views/Shared/EditorTemplates/CountryViewModel.cshtml* modello:

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

Aggiunta di HTML [ \<opzione >](https://www.w3.org/wiki/HTML/Elements/option) elementi non è limitato al *Nessuna selezione* case. Ad esempio, il metodo di visualizzazione e l'azione seguente genererà HTML simile al codice precedente:

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

Il corretto `<option>` verrà selezionato l'elemento (contengono il `selected="selected"` attributo) a seconda di corrente `Country` valore.

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Helper tag](tag-helpers/intro.md)

* [Elemento di HTML Form](https://www.w3.org/TR/html401/interact/forms.html)

* [Token di richiesta di verifica](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [Associazione di modelli](../models/model-binding.md)

* [Convalida del modello](../models/validation.md)

* [annotazioni dei dati](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* [Frammenti per il documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).
