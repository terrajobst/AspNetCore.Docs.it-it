---
title: Globalizzazione e localizzazione
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: 70f11cc9de8e885745e7d08cb98ac68e3cc8ef95
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="globalization-and-localization"></a>Globalizzazione e localizzazione

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Roberto Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), e [Ateya Hisham Bin](https://twitter.com/hishambinateya)

Creazione di un sito Web multilingue con ASP.NET Core consentirà il sito raggiungere un gruppo di destinatari più ampio. ASP.NET Core fornisce servizi e middleware per la localizzazione in diverse lingue e impostazioni cultura.

Internazionalizzazione implica [globalizzazione](https://msdn.microsoft.com/library/aa292081(v=vs.71).aspx) e [localizzazione](https://msdn.microsoft.com/library/aa292137(v=vs.71).aspx). Globalizzazione è il processo di progettazione di applicazioni che supportano impostazioni cultura diverse. Globalizzazione aggiunge il supporto per input, visualizzazione e l'output di un set definito di alfabeti relativi ad aree geografiche specifiche.

Localizzazione è il processo di adattamento di un'applicazione globalizzata, che è già stata elaborata per la localizzazione, per una particolare lingua/impostazioni locali.  Per ulteriori informazioni vedere **termini globalizzazione e localizzazione** verso la fine di questo documento.

Localizzazione App comporta quanto segue:

1. Rendere il contenuto dell'applicazione localizzabile

2. Fornire risorse localizzate per le lingue e impostazioni cultura supportate

3. Implementare una strategia per selezionare la lingua per ogni richiesta

## <a name="make-the-apps-content-localizable"></a>Rendere il contenuto dell'applicazione localizzabile

Introdotto in ASP.NET Core, `IStringLocalizer` e `IStringLocalizer<T>` sono state progettate per migliorare la produttività durante lo sviluppo di App localizzata. `IStringLocalizer`Usa il [ResourceManager](https://msdn.microsoft.com/library/system.resources.resourcemanager(v=vs.110).aspx) e [ResourceReader](https://msdn.microsoft.com/library/system.resources.resourcereader(v=vs.110).aspx) per fornire risorse specifiche delle impostazioni cultura in fase di esecuzione. L'interfaccia semplice dispone di un indicizzatore e un `IEnumerable` per la restituzione di stringhe localizzate. `IStringLocalizer`non è necessario archiviare le stringhe di linguaggio predefinito in un file di risorse. È possibile sviluppare un'app di destinazione per la localizzazione e non è necessario creare file di risorse nelle prime fasi di sviluppo. Il codice riportato di seguito viene illustrato come eseguire il wrapping la stringa "Title su" per la localizzazione.

[!code-csharp[Principale](localization/sample/Controllers/AboutController.cs)]

Nel codice precedente, il `IStringLocalizer<T>` implementazione proviene da [Dependency Injection](dependency-injection.md). Se non viene trovato il valore localizzato di "Su Title", quindi la chiave dell'indicizzatore viene restituita, ovvero la stringa "Title su". È possibile lasciare l'impostazione predefinita, le stringhe letterali lingua nell'app e wrap in localizzatore, in modo che sia possibile concentrarsi sullo sviluppo di app. Sviluppare l'applicazione con la lingua predefinita e prepararlo per il passaggio di localizzazione senza prima creare un file di risorse predefinito. In alternativa, è possibile utilizzare l'approccio tradizionale e fornire una chiave per recuperare la stringa di lingua predefinita. Per molti sviluppatori nuovo flusso di lavoro di non avere una lingua predefinita *resx* file e semplicemente il wrapping i valori letterali stringa possono ridurre l'overhead di localizzazione di un'app. Come può rendere più facile lavorare con più valori letterali stringa e rendono più semplice aggiornare le stringhe localizzate, altri sviluppatori preferiranno il flusso di lavoro tradizionale.

Utilizzare il `IHtmlLocalizer<T>` implementazione per le risorse che contengono HTML. `IHtmlLocalizer`HTML codifica argomenti in cui vengono utilizzati la stringa di risorsa, ma non la stringa di risorsa. Nell'esempio evidenziate, solo il valore di `name` parametro è codificato in formato HTML.

[!code-csharp[Principale](../fundamentals/localization/sample/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

Nota: In genere necessario localizzare solo testo e non il codice HTML.

Al livello inferiore, è possibile ottenere `IStringLocalizerFactory` fuori [Dependency Injection](dependency-injection.md):

[!code-csharp[Principale](localization/sample/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

Il codice sopra riportato di seguito viene illustrato ogni della factory due metodi di creazione.

È possibile partizionare le stringhe localizzate dal controller, area o avere un solo contenitore. Nell'applicazione di esempio, una classe fittizia denominata `SharedResource` viene utilizzato per le risorse condivise.

[!code-csharp[Principale](localization/sample/Resources/SharedResource.cs)]

Alcuni sviluppatori utilizzano il `Startup` classe per contenere stringhe globale o condivise.  Nell'esempio seguente, il `InfoController` e `SharedResource` vengono utilizzati i localizzatori:

[!code-csharp[Principale](localization/sample/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Localizzazione di visualizzazione

Il `IViewLocalizer` servizio fornisce stringhe localizzate per un [vista](http://docs.asp.net/projects/mvc/en/latest/views/index.html). La `ViewLocalizer` classe implementa questa interfaccia e consente di trovare il percorso della risorsa dal percorso del file di visualizzazione. Il codice seguente viene illustrato come utilizzare l'implementazione predefinita di `IViewLocalizer`:

[!code-HTML[Principale](localization/sample/Views/Home/About.cshtml)]

L'implementazione predefinita di `IViewLocalizer` trova il file di risorse basato sul nome di file della vista. Non è disponibile alcuna opzione per utilizzare un file di risorsa globale condivisa. `ViewLocalizer`implementa il localizzatore utilizzando `IHtmlLocalizer`, pertanto Razor non HTML codificare la stringa localizzata. È possibile parametrizzare le stringhe di risorsa e `IViewLocalizer` HTML codificherà i parametri, ma non la stringa di risorsa. Si consideri il seguente codice Razor:

```HTML
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
   ```

Un file di risorse francese potrebbe contenere quanto segue:

| Chiave | Valore |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

Visualizzazione sottoposta a rendering conterrebbe il markup HTML dal file di risorse.

Note:
- La localizzazione di visualizzazione richiede il pacchetto NuGet "Localization.AspNetCore.TagHelpers".
- In genere si desidera localizzare solo testo e non il codice HTML.

Per utilizzare un file di risorse condivise in una vista, inserire `IHtmlLocalizer<T>`:

[!code-HTML[Principale](../fundamentals/localization/sample/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>Localizzazione DataAnnotations

I messaggi di errore DataAnnotations vengono localizzati con `IStringLocalizer<T>`. Utilizzo dell'opzione `ResourcesPath = "Resources"`, i messaggi di errore nella `RegisterViewModel` possono essere archiviati in uno dei seguenti percorsi:

* Resources/ViewModels.Account.RegisterViewModel.fr.resx
* Resources/ViewModels/Account/RegisterViewModel.fr.resx

[!code-csharp[Principale](localization/sample/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

In ASP.NET MVC di base 1.1.0 e superiori, non convalida gli attributi sono localizzati. Componenti di base di ASP.NET MVC 1,0 **non** cercare stringhe localizzate per gli attributi non di convalida.

<a name=one-resource-string-multiple-classes></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Utilizzando una stringa di risorsa per le classi più

Il codice seguente viene illustrato come utilizzare una stringa di risorsa per gli attributi di convalida con più classi:

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

Nel codice precedente, `SharedResource` è la classe corrispondente per il resx in cui sono archiviati i messaggi di convalida. Con questo approccio, verrà utilizzato soltanto DataAnnotations `SharedResource`, anziché la risorsa per ogni classe. 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Fornire risorse localizzate per le lingue e impostazioni cultura supportate  

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures e SupportedUICultures

ASP.NET Core consente di specificare i due valori delle impostazioni cultura, `SupportedCultures` e `SupportedUICultures`. Il [CultureInfo](https://msdn.microsoft.com/library/system.globalization.cultureinfo(v=vs.110).aspx) oggetto `SupportedCultures` determina i risultati delle funzioni dipendenti dalla lingua, ad esempio date, time, numero e la formattazione della valuta. `SupportedCultures`determina anche l'ordinamento del testo, le convenzioni di maiuscole e minuscole e i confronti di stringhe. Vedere [CultureInfo. CurrentCulture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.currentculture%28v=vs.110%29.aspx) per ulteriori informazioni sul modo in cui il server Ottiene le impostazioni cultura. Il `SupportedUICultures` determina che converte le stringhe (da *resx* file) vengono cercati in base il [ResourceManager](https://msdn.microsoft.com/library/system.resources.resourcemanager(v=vs.110).aspx). Il `ResourceManager` cerca semplicemente stringhe specifiche delle impostazioni cultura che è determinato dal `CurrentUICulture`. Ogni thread in .NET è `CurrentCulture` e `CurrentUICulture` oggetti. ASP.NET Core controlla se questi valori durante il rendering delle funzioni dipendenti dalla lingua. Ad esempio, se le impostazioni cultura del thread corrente sono impostata su "en-US" (in lingua inglese, Stati Uniti), `DateTime.Now.ToLongDateString()` Visualizza "Giovedì, febbraio 18 2016", ma se `CurrentCulture` è impostato su "es-ES", spagnolo (Spagna) l'output sarà "jueves, 18 de febrero de 2016".

## <a name="working-with-resource-files"></a>Utilizzo di file di risorse

Un file di risorse è un meccanismo utile per la separazione di stringhe localizzabili dal codice. Le stringhe tradotte per la lingua predefinita non sono isolate *resx* file di risorse. Potrebbe ad esempio, si desidera creare il file di risorse spagnolo denominato *Welcome.es.resx* contenente convertire stringhe. "es" è il codice di lingua per lo spagnolo. Per creare questo file di risorse in Visual Studio:

1. In **Esplora**, fare clic con il pulsante destro sulla cartella che conterrà il file di risorse > **Aggiungi** > **nuovo elemento**.

    ![Menu di scelta rapida nidificato: In Esplora soluzioni, un menu di scelta rapida è aperto per le risorse. Un secondo menu di scelta rapida è aperto per l'aggiunta che mostra il comando nuovo elemento evidenziato.](localization/_static/newi.png)

2. Nel **Cerca modelli installati** immettere "resource", quindi denominare il file.

    ![Aggiungere una finestra di dialogo Nuovo elemento](localization/_static/res.png)

3. Immettere il valore della chiave (stringa nativi) nei **nome** colonna e la stringa tradotta nel **valore** colonna.

    ![Welcome.es.resx (il file iniziale risorse per lo spagnolo) con la parola Hello nella colonna nome e la parola Hola (Buongiorno in spagnolo) nella colonna valore](localization/_static/hola.png)

    Visual Studio Mostra il *Welcome.es.resx* file.

    ![Esplora soluzioni con file di risorse iniziale spagnolo (es)](localization/_static/se.png)

<a name="error"></a>

Se si utilizza una versione di Visual Studio 2017 Preview 15.3, verrà visualizzato un indicatore di errore nell'editor di risorse. Rimuovere il *ResXFileCodeGenerator* valore il *lo strumento personalizzato* griglia proprietà per evitare che questo messaggio di errore:

![Editor resx](localization/_static/err.png)

In alternativa, è possibile ignorare questo errore. Ci auguriamo che risolvere il problema nella versione successiva.

## <a name="resource-file-naming"></a>La denominazione dei file di risorse

Le risorse sono denominate per il nome completo del tipo della relativa classe di meno il nome dell'assembly. Ad esempio, una risorsa francese in un progetto dell'assembly principale è `LocalizationWebsite.Web.dll` per la classe `LocalizationWebsite.Web.Startup` verrebbe denominato *Startup.fr.resx*. Una risorsa per la classe `LocalizationWebsite.Web.Controllers.HomeController` verrebbe denominato *Controllers.HomeController.fr.resx*. Se lo spazio dei nomi della classe di destinazione non è identico al nome di assembly, è necessario il nome completo del tipo. Ad esempio, nell'esempio di una risorsa per il tipo di progetto `ExtraNamespace.Tools` verrebbe denominato *ExtraNamespace.Tools.fr.resx*.

Nel progetto di esempio, il `ConfigureServices` metodo imposta la `ResourcesPath` in "Resources", pertanto il percorso relativo del progetto per il file di risorse francese del controller home è *Resources/Controllers.HomeController.fr.resx*. In alternativa, è possibile utilizzare cartelle per organizzare i file di risorse. Per il controller home, il percorso dovrebbe essere *Resources/Controllers/HomeController.fr.resx*. Se non si utilizza il `ResourcesPath` opzione, il *resx* verrebbero file nella directory di base del progetto. File di risorse per `HomeController` verrebbe denominato *Controllers.HomeController.fr.resx*. La scelta di utilizzare la convenzione di denominazione di punto o percorso dipende da come si desidera organizzare i file di risorse.

| Nome della risorsa | Punto o dei nomi di percorso |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Punto  |
| Resources/Controllers/HomeController.fr.resx  | Path |
|    |     |

File di risorse utilizzando `@inject IViewLocalizer` nelle visualizzazioni Razor seguono un modello simile. File di risorse per una vista è possibile denominare tramite denominazione punto o dei nomi di percorso. File di risorse di visualizzazione Razor simulano il percorso del file di visualizzazione associata. Supponendo che viene impostato il `ResourcesPath` "Resources", il file di risorse francese associato il *Views/Home/About.cshtml* visualizzazione potrebbe essere uno dei seguenti:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Se non si utilizza il `ResourcesPath` opzione, il *resx* file per una vista potrebbe trovarsi nella stessa cartella della vista.

Se si rimuove l'identificatore delle impostazioni cultura "fr" e si hanno le impostazioni cultura impostate sulla lingua francese (tramite cookie o un altro meccanismo), viene letto il file di risorse predefinito e le stringhe localizzate. Il gestore delle risorse designa una risorsa di fallback o predefinite, quando non soddisfa la lingua richiesta si è l'utilizzo del file *.resx senza un identificatore delle impostazioni cultura. Se si desidera restituire semplicemente la chiave quando manca una risorsa per la richiesta di lingua non deve essere un file di risorse predefinito.

### <a name="generating-resource-files-with-visual-studio"></a>Generazione di file di risorse con Visual Studio

Se si crea un file di risorse in Visual Studio senza le impostazioni cultura nel nome del file (ad esempio, *Welcome.resx*), Visual Studio verrà creata una classe c# con una proprietà per ogni stringa. Che è in genere non si desidera con ASP.NET Core; in genere non è un valore predefinito *resx* file di risorse (A *resx* file senza il nome delle impostazioni cultura). Si consiglia creare il *resx* file con un nome di impostazioni cultura (ad esempio *Welcome.fr.resx*). Quando si crea un *resx* file con un nome di impostazioni cultura, Visual Studio non genererà il file di classe. Si prevede che molti sviluppatori **non** creare un file di risorse di lingua predefinita.

### <a name="adding-other-cultures"></a>Aggiunta di altre impostazioni cultura

Ogni combinazione di lingua e le impostazioni cultura (diversa da quella predefinita) richiede un file di risorse univoco. Creare file di risorse per diverse impostazioni cultura e le impostazioni locali mediante la creazione di nuovi file di risorse in cui i codici di lingua ISO fanno parte del nome file (ad esempio, **en-us**, **fr-ca**, e  **en-gb**). Questi codici vengono inseriti tra il nome del file e *resx* estensione nome file, come in *Welcome.es MX* (spagnolo/Messico). Per specificare una lingua indipendente dalle impostazioni cultura, rimuovere il codice paese (`MX` nell'esempio precedente). Il nome del file di risorse spagnolo indipendenti dalle impostazioni cultura *Welcome.es.resx*.

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Implementare una strategia per selezionare la lingua per ogni richiesta  

### <a name="configuring-localization"></a>Configurazione di localizzazione

Localizzazione è configurata nel `ConfigureServices` metodo:

[!code-csharp[Principale](localization/sample/Startup.cs?range=45-49)]

* `AddLocalization`Aggiunge i servizi di localizzazione per il contenitore dei servizi. Il codice sopra imposta inoltre il percorso delle risorse in "Resources".

* `AddViewLocalization`Aggiunge il supporto per i file della localizzata visualizzazione. In questa vista di esempio localizzazione si basa sul suffisso del file di visualizzazione. Ad esempio "fr" nel *Index.fr.cshtml* file.

* `AddDataAnnotationsLocalization`Aggiunge il supporto per localizzate `DataAnnotations` messaggi di convalida tramite `IStringLocalizer` astrazioni.

### <a name="localization-middleware"></a>Middleware di localizzazione

Le impostazioni cultura correnti per una richiesta sono stata impostata la localizzazione [Middleware](middleware.md). Il middleware di localizzazione è abilitato nel `Configure` metodo *Startup.cs* file. Si noti che il middleware di localizzazione deve essere configurato prima middleware che è possibile controllare le impostazioni cultura richiesta (ad esempio, `app.UseMvc()`).

[!code-csharp[Principale](localization/sample/Startup.cs?highlight=13-35&range=123-159)]

`UseRequestLocalization`Inizializza un `RequestLocalizationOptions` oggetto. Ogni richiesta, l'elenco di `RequestCultureProvider` nel `RequestLocalizationOptions` viene enumerata e viene utilizzato il primo provider in grado di determinare correttamente le impostazioni cultura richiesta. I provider predefiniti provengono dalla `RequestLocalizationOptions` classe:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

L'elenco predefinito passa dal più specifico al meno specifico. Avanti in questo articolo verrà illustrata la modalità con cui è possibile modificare l'ordine e anche aggiungere un provider di impostazioni cultura personalizzate. Se nessuno dei provider può determinare la lingua richiesta, il `DefaultRequestCulture` viene utilizzato.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Alcune App verrà utilizzata una stringa di query per impostare il [culture e Uiculture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx#Current). Per le applicazioni che utilizzano il cookie o un approccio di intestazione Accept-Language, l'aggiunta di una stringa di query all'URL è utile per il debug e test del codice. Per impostazione predefinita, il `QueryStringRequestCultureProvider` viene registrato come provider di localizzazione prima nel `RequestCultureProvider` elenco. Si passa la query con parametri di stringa `culture` e `ui-culture`. Nell'esempio seguente imposta le impostazioni cultura specifiche (lingua e paese) in spagnolo/Messico:

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Se si passa solo in uno dei due (`culture` o `ui-culture`), il provider di stringa di query verrà impostato con quello è passato in entrambi i valori. Ad esempio, impostare solo le impostazioni cultura imposterà entrambi il `Culture` e `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Le applicazioni di produzione saranno spesso forniscono un meccanismo per configurare le impostazioni cultura con il cookie di impostazioni cultura ASP.NET Core. Utilizzare il `MakeCookieValue` metodo per creare un cookie.

Il `CookieRequestCultureProvider` `DefaultCookieName` preferita il nome di cookie predefinito utilizzato per tenere traccia dell'utente restituisce informazioni sulle impostazioni cultura. Il nome di cookie predefinito è ". AspNetCore.Culture".

Il formato del cookie è `c=%LANGCODE%|uic=%LANGCODE%`, dove `c` è `Culture` e `uic` è `UICulture`, ad esempio:

    c=en-UK|uic=en-US

Se si specifica solo una delle informazioni sulle impostazioni cultura e le impostazioni cultura dell'interfaccia utente, le impostazioni cultura specificate da utilizzare per informazioni sulle impostazioni cultura e le impostazioni cultura dell'interfaccia utente.

### <a name="the-accept-language-http-header"></a>L'intestazione HTTP Accept-Language

Il [intestazione Accept-Language](https://www.w3.org/International/questions/qa-accept-lang-locales) è impostabile nella maggior parte dei browser e progettata originariamente per specificare la lingua dell'utente. Questa impostazione indica ciò che il browser è stato impostato per l'invio o ha ereditato dal sistema operativo sottostante. L'intestazione HTTP Accept-Language da una richiesta del browser non è un modo infallibile per rilevare la lingua preferita dell'utente (vedere [impostazione delle preferenze di lingua in un browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Un'app di produzione deve includere un modo per un utente di personalizzare la scelta della lingua.

### <a name="setting-the-accept-language-http-header-in-ie"></a>Impostazione dell'intestazione HTTP Accept-Language in Internet Explorer

1. Dall'icona dell'ingranaggio, toccare **Opzioni Internet**.

2. Toccare **lingue**.

    ![Opzioni Internet](localization/_static/lang.png)

3. Toccare **impostare le preferenze della lingua**.

4. Toccare **Aggiungi una lingua**.

5. Aggiungere la lingua.

6. Scegliere la lingua, quindi **Sposta su**.

### <a name="using-a-custom-provider"></a>Utilizzando un provider personalizzato

Si supponga che si desidera consentire ai clienti di archiviare la lingua e delle impostazioni cultura nei database. È possibile scrivere un provider di ricerca di questi valori per l'utente. Il codice seguente viene illustrato come aggiungere un provider personalizzato:

```csharp
services.Configure<RequestLocalizationOptions>(options =>
   {
       var supportedCultures = new[]
       {
           new CultureInfo("en-US"),
           new CultureInfo("fr")
       };

       options.DefaultRequestCulture = new RequestCulture(culture: "en-US", uiCulture: "en-US");
       options.SupportedCultures = supportedCultures;
       options.SupportedUICultures = supportedCultures;

       options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
       {
         // My custom request culture logic
         return new ProviderCultureResult("en");
       }));
   });
   ```

Utilizzare `RequestLocalizationOptions` per aggiungere o rimuovere i provider di localizzazione.

### <a name="setting-the-culture-programmatically"></a>Impostazione a livello di codice le impostazioni cultura

In questo esempio **Localization.StarterWeb** progetto [GitHub](https://github.com/aspnet/entropy) contiene l'interfaccia utente di impostare il `Culture`. Il *Views/Shared/_SelectLanguagePartial.cshtml* file consente di selezionare le impostazioni cultura dall'elenco delle impostazioni cultura supportate:

[!code-HTML[Principale](localization/sample/Views/Shared/_SelectLanguagePartial.cshtml)]

Il *Views/Shared/_SelectLanguagePartial.cshtml* file viene aggiunto per il `footer` sezione del file di layout in modo che sia disponibile per tutte le viste:

[!code-HTML[Principale](localization/sample/Views/Shared/_Layout.cshtml?range=48-61&highlight=10)]

Il `SetLanguage` metodo imposta il cookie di impostazioni cultura.

[!code-csharp[Principale](localization/sample/Controllers/HomeController.cs?range=57-67)]

Non è possibile collegare il *_SelectLanguagePartial.cshtml* al codice di esempio per questo progetto. Il **Localization.StarterWeb** progetto [GitHub](https://github.com/aspnet/entropy) include il codice per il flusso di `RequestLocalizationOptions` per un Razor parziale tramite il [Dependency Injection](dependency-injection.md) contenitore.

## <a name="globalization-and-localization-terms"></a>Condizioni di globalizzazione e localizzazione

Il processo di localizzazione dell'app richiede una conoscenza di base pertinente di set di caratteri comunemente usati nello sviluppo di software moderno e la comprensione dei problemi associati. Anche se tutti i computer archiviano testo come numeri (codici), sistemi diversi archiviano lo stesso testo utilizzando numeri diversi. Si intende il processo di localizzazione traduzione dell'interfaccia utente dell'applicazione (UI) per una lingua/impostazioni locali specifiche.

[Possibilità di localizzazione](https://msdn.microsoft.com/library/aa292135(v=vs.71).aspx) è un processo intermedio per la verifica che un'applicazione globalizzata sia pronta per la localizzazione.

Il [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) il formato per il nome delle impostazioni cultura è "<languagecode2>-< codicepaese2 >", dove <languagecode2> è il codice lingua e < codicepaese2 > è riportato il codice delle impostazioni cultura secondarie. Ad esempio, `es-CL` per lo spagnolo (Cile), `en-US` per inglese (Stati Uniti), e `en-AU` per inglese (Australia). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) è una combinazione di un ISO 639 a due lettere minuscole associata a una lingua e un codice di due lettere maiuscole sottolingua associata a un paese o area geografica di ISO 3166.  Vedere [nome delle impostazioni cultura della lingua](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Internazionalizzazione viene spesso abbreviato in "I18N". L'abbreviazione accetta il primo e l'ultima lettera e il numero di lettere tra di esse, pertanto 18 indica il numero di lettere comprese tra il primo "I" e ultimi "N". Lo stesso vale per (G11N) di globalizzazione e localizzazione (L10N).

Condizioni:

* Globalizzazione (G11N): Il processo di supportare diverse lingue e le aree di un'app.
* Localizzazione (L10N): Il processo di personalizzazione di un'app per una lingua specifica e una regione.
* Internazionalizzazione (I18N): Descrive sia globalizzazione e localizzazione.
* Impostazioni cultura: È un linguaggio e, facoltativamente, un'area.
* Impostazioni cultura di sistema: le impostazioni cultura sono la lingua specificata, ma non una regione. (ad esempio "IT", "es")
* Impostazioni cultura specifiche: impostazioni cultura che dispone di una lingua specificata e area. (ad esempio "en-US", "en-GB", "es-CL")
* Impostazioni locali: Le impostazioni locali sono lo stesso come impostazioni cultura.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Progetto Localization.StarterWeb](https://github.com/aspnet/entropy) utilizzata nell'articolo.
* [File di risorse in Visual Studio](https://msdn.microsoft.com/library/xbx3z216(v=vs.110).aspx#VSResFiles)
* [Risorse in file con estensione resx](https://msdn.microsoft.com/library/xbx3z216(v=vs.110).aspx#ResourcesFiles)
