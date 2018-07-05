---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: All'inizio le funzionalità disponibili in ASP.NET Web Pages 2 | Microsoft Docs
author: microsoft
description: In questo argomento viene fornita una panoramica delle principali nuove funzionalità in ASP.NET Web Pages 2, un framework di programmazione web semplice che è incluso con il WebMatr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: 3cdb9d83e0f612ad7404bfaa9721b580916e112d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385266"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Le funzionalità migliori in ASP.NET Web Pages 2
====================
da [Microsoft](https://github.com/microsoft)

> Questo articolo offre una panoramica delle principali nuove funzionalità in ASP.NET Web Pages 2 RC, un framework di programmazione web semplice che è accluso [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Cosa è incluso:** 
> 
> - [Installazione di WebMatrix](#install)
> - [Funzionalità nuove e migliorate](#New_and_Enhanced_Features)
> 
>     - [Modifiche per la versione RC](#Changes_for_the_RC_Version)
>     - [Modifiche della versione Beta](#Changes_for_the_Beta_Version)
>     - [Usando i modelli di sito nuovi e aggiornati](#templates)
>     - [Convalida dell'Input utente](#validation)
>     - [Abilitare gli account di accesso di Facebook e altri siti usando OAuth e OpenID](#oauthsetup)
>     - [Aggiunta di mappe usando l'Helper di mappe](#maphelper)
>     - [Esecuzione di applicazioni Web Pages Side-by-Side](#sidebyside)
>     - [Il rendering di pagine per i dispositivi mobili](#mobile)
> - [Risorse aggiuntive](#resources)
> 
> > [!NOTE]
> > Questo argomento si presuppone che si utilizza WebMatrix per lavorare con il codice ASP.NET Web Pages 2. Tuttavia, come con 1 le pagine Web, è anche possibile creare siti Web di Web Pages 2 usando Visual Studio, che offre migliorato le funzionalità di IntelliSense e il debug. Per usare le pagine Web in Visual Studio, è innanzitutto necessario installare Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 o Visual Studio 11 Beta. Quindi installare ASP.NET MVC 4 Beta, che include i modelli e strumenti per la creazione di applicazioni ASP.NET MVC 4 e Web Pages 2 in Visual Studio.
> 
> 
> *Ultimo aggiornamento: 18 giugno 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Installazione di WebMatrix

Per installare le pagine Web, è possibile usare l'installazione guidata piattaforma Web Microsoft, che è un'applicazione gratuita che rende più semplice installare e configurare le tecnologie relative a web. Installare la versione Beta 2 di WebMatrix, che include una versione Beta di Web Pages 2.

1. Passare alla pagina di installazione per la versione più recente del programma di installazione piattaforma Web:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Se è già installato WebMatrix 1, l'installazione degli aggiornamenti a WebMatrix 2 Beta. È possibile eseguire i siti Web che sono stati creati con la versione 1 o 2 nello stesso computer. Per altre informazioni, vedere la sezione sul [esecuzione di applicazioni Web Pages fianco a fianco](#sidebyside).
2. Scegli **Installa ora**. 

    Se si usa Internet Explorer, andare al passaggio successivo. Se si utilizza un browser diverso, ad esempio Google Chrome o Mozilla Firefox, viene chiesto di salvare il *Webmatrix.exe* file nel computer. Salvare il file e quindi fare clic su esso per avviare il programma di installazione.
3. Eseguire il programma di installazione e scegliere il **installare** pulsante. Ciò consente di installare WebMatrix e pagine Web.

## <a id="New_and_Enhanced_Features"></a>  Funzionalità nuove e migliorate

### <a id="Changes_for_the_RC_Version"></a>  Modifiche per la versione RC (giugno 2012)

Il rilascio della versione RC di giugno 2012 presenta alcune modifiche dall'aggiornamento della versione Beta che è stato rilasciato nel mese di marzo 2012. Queste modifiche sono:

- Oggetto `Validation.AddFormError` metodo è stato aggiunto per il `Validation` helper. Ciò è utile se si esegue la convalida manualmente (ad esempio, si convalida un valore che viene passato nella stringa di query) e si desidera aggiungere un messaggio di errore che può essere visualizzato dal `Html.ValidationSummary` (metodo). Per altre informazioni, vedere la sezione [convalida dei dati che non provengono direttamente da utenti](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) nelle [Validating User Input nei siti ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253002).
- La funzionalità per la creazione di bundle e minimizzazione è stato rimosso dagli assembly di ASP.NET Web Pages 2 core. Di conseguenza, il `Assets` helper elencati più avanti in questo documento non è disponibile. In alternativa, è necessario installare il [ottimizzazione ASP.NET](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) pacchetto NuGet. Per altre informazioni, vedere [creazione di bundle e minimizzazione di risorse in un sito di ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Sono stati aggiunti altri assembly per il supporto ASP.NET Web Pages 2. L'effetto solo apprezzabile di questa modifica è che si possano ottenere più assembly in un sito *bin* cartella dopo aver creato un sito o distribuire un sito in un provider di hosting.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Modifiche per la versione Beta (febbraio 2012)

La versione Beta rilasciata a febbraio 2012 presenta solo alcune modifiche dalla versione Beta che è stato rilasciato nel dicembre 2011. Queste modifiche sono:

- Razor supporta ora gli attributi condizionali. In un elemento HTML elemento, se si imposta un attributo a un valore che viene risolta nel codice del server `false` o `null`, ASP.NET non esegue il rendering dell'attributo. Ad esempio, si supponga di che avere il seguente markup per una casella di controllo:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Se il valore di `checked1` si risolve in `false` o su `null`, il `checked` attributo non viene eseguito il rendering. Si tratta di una modifica sostanziale.
- Il `Validation.GetHtml` metodo è stato rinominato in `Validation.For`. Si tratta di una modifica sostanziale; `Validation.GetHtml` non funzionerà nella versione Beta.
- È ora possibile includere il `~` operatore nel markup alla radice del sito di riferimento senza usare il `Href` (funzione). (Vale a dire, il parser Razor può ora trovare e risolvere i `~` operatore senza richiedere una chiamata al metodo esplicito `Href`.) Il `Href` metodo continui a funzionare, in modo che non si tratta di una modifica sostanziale.

    Ad esempio, se in precedenza era markup simile al seguente:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    È ora possibile usare codice simile al seguente:

    `<a href="~/Default.cshtml">Home</a>`
- Il `Scripts` helper per la gestione di asset (risorsa) è stata sostituita con la `Assets` helper, che dispone di metodi leggermente diversi, come il seguente:

  - Per `Scripts.Add`, usare `Assets.AddScript`
  - Per `Scripts.GetScriptTags`, usare `Assets.GetScripts`

    Si tratta di una modifica sostanziale; il `Scripts` classe non è disponibile nella versione Beta. Esempi di codice che usano la gestione delle risorse di questo documento sono stati aggiornati con questa modifica.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Usando i modelli di sito nuovi e aggiornati

Il **Starter Site** modello è stato aggiornato in modo che venga eseguito in Web Pages 2 per impostazione predefinita. Include anche le nuove funzionalità seguenti:

- Rendering della pagina per dispositivi mobili. Tramite l'utilizzo di stili CSS e il `@media` selettore, i **Starter Site** consente un rendering migliorato delle pagine su schermi più piccoli, incluse le schermate dei dispositivi mobili.
- Opzioni migliorate di appartenenza e autenticazione. È possibile consentire agli utenti di accedere al sito usando i propri account da altri siti di social networking, ad esempio Twitter, Facebook e Windows Live. Per altre informazioni, vedere la [abilitazione gli account di accesso di Facebook e altri siti con OAuth e OpenID](#oauthsetup) sezione.
- Elementi HTML5.

Il nuovo **Personal Site** modello consente di creare un sito Web che include un blog personale, una pagina di foto e una pagina di Twitter. È possibile personalizzare un sito di base di **sito personale** modello effettuando le seguenti operazioni:

- Modificare l'aspetto del sito, modificare il file di layout (*\_SiteLayout.cshtml*) e il file di stili (*CSS*).
- Installare i pacchetti NuGet che aggiungono funzionalità al sito. Per informazioni su come installare i pacchetti, tra cui ASP.NET Web Helpers Library, vedere l'esercitazione [installazione helper](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Per l'accesso il **sito personale** modello, scegliere **modelli** nella finestra di WebMatrix **Quick Start** dello schermo.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

Nel **modelli** finestra di dialogo scegliere la **sito personale** modello.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Pagina di destinazione della **Personal Site** modello consente di selezionare i collegamenti per impostare i blog, Twitter, pagina e pagina foto.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Convalida dell'Input utente

In Web Pages 1, per convalidare l'input dell'utente nei form inviate, si utilizza il `System.Web.WebPages.Html.ModelState` classe. (Questa operazione viene illustrata in molti degli esempi di codice dell'esercitazione di Web Pages 1 intitolata [utilizzo di dati](../data/5-working-with-data.md).) È comunque possibile usare questo approccio in Web Pages 2. Tuttavia, Web Pages 2 offre inoltre strumenti migliorati per la convalida dell'input dell'utente:

- Nuove classi di convalida, incluse `System.Web.WebPages.ValidationHelper` e `System.Web.WebPages.Validator`, che consentono di eseguire attività di convalida avanzati con poche righe di codice.
- Facoltativamente, convalida sul lato client, che offre un feedback immediato all'utente anziché richiedere un round trip al server per controllare gli errori di convalida. (Per motivi di sicurezza, la convalida viene eseguita nel server anche se i controlli sono stati eseguiti nel client in anticipo.)

Per usare le nuove funzionalità di convalida, effettuare le operazioni seguenti:

Nel codice della pagina, un elemento da convalidare da usando i metodi di registrare il `Validation` helper: `Validation.RequireField`, `Validation.RequireFields` (per registrare più elementi di modo che sia obbligatoria), o `Validation.Add`. Il `Add` metodo consente di specificare altri tipi di controlli di convalida, ad esempio tipo di dati per il controllo, il confronto voci nei campi diversi controlli di lunghezza della stringa e modelli (usando le espressioni regolari). Ecco alcuni esempi:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Per visualizzare un errore specifico del campo, chiamare `Html.ValidationMessage` nel markup per ogni elemento da convalidare:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Per visualizzare un riepilogo (`<ul>` elenco) di tutti gli errori nella pagina `Html.ValidationSummary` nel markup:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Questi passaggi sono sufficienti per implementare la convalida sul lato server. Se si desidera aggiungere la convalida lato client, seguire questa procedura anche.

Aggiungere i seguenti riferimenti di file di script all'interno di `<head>` sezione di una pagina web. I primi due riferimenti a script scegliere i file remoti in un server di distribuzione di contenuti (CDN) di rete. Il terzo riferimento punta a un file di script locale. Le app di produzione devono implementare un fallback quando non è disponibile la rete CDN. Testare il fallback.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Il modo più semplice per ottenere una copia locale del *jquery.validate.unobtrusive.min.js* libreria consiste nel creare un nuovo sito Web Pages basato su uno dei modelli di sito (ad esempio, il sito di base). Il sito creato tramite il modello include *jquery.validate.unobtrusive.js* file nella cartella di script, da cui è possibile copiarlo nel sito.

Se il sito Web Usa un<em>\_SiteLayout</em> pagina per controllare il layout di pagina, è possibile includere riferimenti a questi script in tale pagina, in modo che la convalida è disponibile per tutte le pagine di contenuto. Se si desidera eseguire la convalida solo su determinate pagine, è possibile usare la gestione di asset per registrare gli script solo dalle pagine stesse. A tale scopo, chiamare `Assets.AddScript(path)` nella pagina che si desidera convalidare e fare riferimento a ciascuno dei file di script. Aggiungere quindi una chiamata a `Assets.GetScripts` nella  <em>\_SiteLayout</em> pagina per eseguire il rendering registrato `<script>` tag. Per altre informazioni, vedere la sezione [script di registrazione con la gestione di asset](#resmanagement).

Nel markup per un singolo elemento, chiamare il `Validation.For` (metodo). Questo metodo genera gli attributi che jQuery possibile associarli per fornire la convalida lato client. Ad esempio:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Nell'esempio seguente mostra una pagina che convalida l'input dell'utente in un form. Per eseguire e testare il codice di convalida, eseguire questa operazione:

1. Creare un nuovo sito web utilizzando uno dei modelli di sito WebMatrix 2 che include un *gli script* cartella, ad esempio il **Starter Site** modello.
2. Nel nuovo sito, creare una nuova *cshtml* pagina e sostituire il contenuto della pagina con il codice seguente.
3. Eseguire la pagina in un browser. Immettere i valori validi e non è validi per osservare gli effetti sulla convalida. Ad esempio, lasciare vuoto un campo obbligatorio oppure immettere una lettera il **crediti** campo.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Ecco la pagina quando un utente invia un input valido:

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Quando un utente lo invia con un campo obbligatorio lasciato vuoto, ecco la pagina:

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Ecco la pagina durante l'invio con un valore diverso da integer nel **crediti** campo:

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Per altre informazioni, vedere il post di blog seguenti:

- [Aggiornata la convalida in Web Pages v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) nozioni di base dell'aggiunta di convalida usando il `Validation` helper (solo server-side)
- [Aggiornata la convalida in Web Pages v2, parte 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) aggiunta della convalida lato client.
- [Aggiornamento di convalida in Web Pages v2, parte 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) formattazione gli errori di convalida.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>La registrazione di script utilizzando la gestione di asset

Il gestore di risorse è una nuova funzionalità che è possibile usare nel codice server per registrare e il rendering degli script client. Questa funzionalità è utile quando si lavora con il codice da più file (ad esempio pagine di layout, le pagine di contenuto, gli helper e così via) che vengono combinati in una singola pagina in fase di esecuzione. La gestione di asset coordina i file di origine per assicurarsi che i file di script vengono fatto riferimento in modo corretto e in modo efficiente nella pagina di cui è stato eseguito rendering, indipendentemente dal fatto che i file di codice vengono chiamati dall'o quante volte vengono chiamati. Esegue il rendering anche di gestione di asset `<script>` tag nella posizione corretta, in modo che la pagina è possibile caricare rapidamente (senza scaricare gli script durante il rendering) e per evitare errori che possono verificarsi se gli script vengono chiamati prima del rendering è stata completata.

Ad esempio, si supponga di crea un helper personalizzato che chiama un file JavaScript e si chiama questo helper in tre posizioni differenti nel codice della pagina contenuto. Se non si usa la gestione di asset per registrare lo script chiama nell'helper, tre diversi `<script>` tag che fanno riferimento allo stesso file di script verrà visualizzato nella pagina sottoposta a rendering. Inoltre, a seconda di dove il `<script>` i tag vengono inseriti nella pagina sottoposta a rendering, potrebbero verificarsi errori se lo script tenta di accedere a determinati elementi di pagina prima che la pagina viene caricata completamente. Se si usa la gestione di asset per registrare lo script, evitare questi problemi.

In questo modo, è possibile registrare uno script con la gestione di asset:

- Nel codice che deve fare riferimento allo script, chiamare il `Assets.AddScript` (metodo).
- In un  *\_SiteLayout* pagina, chiamare il `Assets.GetScripts` metodo per eseguire il rendering di `<script>` tag. 

    > [!NOTE]
    > Inserire chiamate a `Assets.GetScripts` come ultimo elemento nel `<body>` elemento delle  *\_SiteLayout* pagina. In questo modo la pagina di caricamento più veloce e può contribuire a evitare gli errori di script.

Nell'esempio seguente viene illustrato come funziona la gestione di asset. Il codice contiene gli elementi seguenti:

- Un helper personalizzato denominato `MakeNote`. Questo helper esegue il rendering di una stringa all'interno di una casella eseguendo il wrapping di un `div` elemento intorno a esso che abbia uno stile con un bordo e aggiungendo &quot;Nota:&quot; ad esso. L'helper chiama anche un file JavaScript che aggiunge il comportamento di runtime per la nota. Anziché fare riferimento allo script con un `<script>` tag, l'helper registra lo script chiamando `Assets.AddScript` .
- Un file JavaScript. Si tratta del file che viene chiamato da helper e questo aumenta temporaneamente la dimensione del carattere di elementi nota durante un `mouseover` evento.
- Una pagina di contenuto, che fa riferimento il<em>\_SiteLayout</em> esegue il rendering di alcuni contenuti nel corpo della pagina e quindi chiama il `MakeNote` helper.
- Oggetto  *\_SiteLayout* pagina. Questa pagina offre un'intestazione comune e una struttura di layout di pagina. Include anche una chiamata a `Assets.GetScripts`, ovvero come la gestione di asset viene eseguito il rendering dello script viene chiamato in una pagina.

Per eseguire l'esempio:

1. Creare un sito Web vuoto Web Pages 2. È possibile usare il WebMatrix **sito vuoto** per questo modello.
2. Creare una cartella denominata *script* nel sito.
3. Nel *gli script* cartella, creare un file denominato *test. js*, copia il *test. js* dall'esempio del contenuto al suo interno e salvare il file...
4. Creare una cartella denominata *App\_codice* nel sito.
5. Nel *App\_codice* cartella, creare un file denominato *Helpers.cshtml*copiarvi il codice di esempio e salvarlo in una cartella denominata *App\_codice*nella cartella radice.
6. Nella cartella radice del sito, creare un file denominato  *\_SiteLayout.cshtml,* copiare l'esempio al suo interno e salvare il file.
7. Nella radice del sito, creare un file denominato *ContentPage.cshtml*, aggiungere il codice di esempio e salvarlo.
8. Eseguire *ContentPage* in un browser. La stringa è stato passato per il `MakeNote` helper viene eseguito il rendering come nota boxed.
9. Passare il puntatore del mouse sulla nota. Lo script aumenta temporaneamente la dimensione del carattere della nota.
10. Visualizzare l'origine della pagina sottoposta a rendering. A causa di cui è stato eseguito la chiamata a `Assets.GetScripts`, il rendering `<script>` tag che chiama *test. js* è l'ultimo elemento nel corpo della pagina.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

La schermata seguente mostra *ContentPage.cshtml* in un browser quando si sposta il puntatore del mouse sulla nota:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Abilitare gli account di accesso di Facebook e altri siti usando OAuth e OpenID

Web Pages 2 offre opzioni avanzate per l'autenticazione e l'appartenenza. La funzionalità avanzata principale è che esistono nuove [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provider. Con questi provider, è possibile consentire agli utenti di accedere al sito usando le proprie credenziali da Facebook, Twitter, Windows Live, Google e Yahoo. Ad esempio, per accedere con un account di Facebook, gli utenti possono semplicemente scegliere un'icona di Facebook, che li reindirizza alla pagina di accesso di Facebook in cui immettere le informazioni utente. È quindi possibile associare l'account di accesso di Facebook con il proprio account nel sito. Un miglioramento correlato alle funzionalità di appartenenza a pagine Web è che gli utenti possono associare più account di accesso (inclusi gli accessi dai siti di social networking) con un singolo account nel sito Web.

La seguente immagine illustra la pagina di accesso dal **Starter Site** modello, in cui un utente può scegliere un'icona di Facebook, Twitter o Windows Live per abilitare la registrazione con un account esterno:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

È possibile abilitare l'appartenenza di OAuth e OpenID usando solo poche righe di codice. I metodi e proprietà si usano per lavorare con OAuth e OpenID provider è nel `WebMatrix.Security.OAuthWebSecurity` classe.

Tuttavia, invece di usare codice per abilitare gli account di accesso da altri siti, un metodo consigliato per iniziare a usare i nuovi provider consiste nell'utilizzare il nuovo **Starter Site** modello che è incluso in WebMatrix 2 Beta. Il **Starter Site** modello include un'infrastruttura completa dell'appartenenza, completa di una pagina di accesso, un database di appartenenza e tutto il codice necessario consentire agli utenti di accedere al sito usando le credenziali locali o quelle di un altro sito .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Come abilitare gli account di accesso usando i provider di OpenID e OAuth

In questa sezione fornisce un esempio di come consentire agli utenti di accedere da siti esterni (Facebook, Twitter, Windows Live, Google o Yahoo) a un sito di base di **Starter Site** modello. Dopo aver creato un sito di base, tale scopo (dettagli):

- Per i siti che usano un provider OAuth (Facebook, Twitter e Windows Live), creare un'applicazione nel sito esterno. In questo modo le chiavi dell'applicazione che è necessario per richiamare la funzionalità di accesso per tali siti. Per i siti che usano un provider di OpenID (Google, Yahoo), non è necessario creare un'applicazione. Per tutti questi siti, è necessario un account per accedere e creare applicazioni per sviluppatori. 

    > [!NOTE]
    > Applicazioni Windows Live accettano solo un URL in tempo reale per un sito Web funzionante, pertanto non è possibile utilizzare un URL del sito Web locale per testare gli account di accesso.
- Modificare alcuni file nel sito Web per specificare il provider di autenticazione appropriato e per l'invio di un account di accesso al sito da usare.

**Per abilitare gli account di accesso di Google e Yahoo**:

1. In un sito Web, modificare il  *\_AppStart.cshtml* pagina e aggiungere le seguenti due righe di codice nel blocco di codice Razor dopo la chiamata al `WebSecurity.InitializeDatabaseConnection` (metodo). Questo codice consente a provider di OpenID di Yahoo e Google. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. Nel *~/Account/Login.cshtml* pagina, rimuovere i commenti dalla seguente `<fieldset>` blocchi di markup verso la fine della pagina. Per rimuovere il commento nel codice, rimuovere il `@*` caratteri che precedono e seguono il `<fieldset>` blocco. Il blocco di codice risultante è simile alla seguente:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Aggiungere un `<input>` (elemento) per il provider Google o Yahoo per il `<fieldset>` gruppo le *~/Account/Login.cshtml* pagina. Aggiornato `<fieldset>` al gruppo `<input>` elementi per Google e Yahoo ha un aspetto simile alla seguente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. Nel *~/Account/AssociateServiceAccount.cshtml* pagina, aggiungere `<input>` elementi per Google o Yahoo al `<fieldset>` gruppo verso la fine del file. È possibile copiare lo stesso `<input>` elementi appena aggiunto per il `<fieldset>` sezione il *~/Account/Login.cshtml* pagina. 

    Il *~/Account/AssociateServiceAccount.cshtml* pagina nel modello Starter Site può essere usata se si desidera creare una pagina in cui gli utenti possono associare più account di accesso da altri siti con un singolo account nel sito Web.

È ora possibile testare gli account di accesso di Google e Yahoo.

1. Eseguire la *default. cshtml* page del sito e scegliere il **Accedi** pulsante.
2. Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** , quindi scegliere il **Google** o **Yahoo** pulsante di invio. Questo esempio Usa l'account di accesso di Google. 

    La pagina web reindirizza la richiesta alla pagina di accesso di Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Immettere le credenziali per un account Google esistente.
4. Se Google viene chiesto se si vuole consentire Localhost utilizzare le informazioni dell'account, fare clic su **Consenti**.

    Il codice Usa il token di Google per l'autenticazione dell'utente e quindi restituisce a questa pagina nel sito Web. Questa pagina consente agli utenti di associare i loro account di accesso di Google con un account esistente nel sito Web, o possono registrare un nuovo account nel sito per associare l'account di accesso esterno con.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Scegliere il **associare** pulsante. Il browser torna alla home page dell'applicazione.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Per abilitare gli account di accesso di Facebook**:

1. Andare alla [sito degli sviluppatori Facebook](https://developers.facebook.com/apps) (accesso, se non è già stato effettuato).
2. Scegliere il **Create New App** pulsante e quindi seguire le istruzioni per assegnare un nome e creare la nuova applicazione.
3. Nella sezione **selezionare come si integrerà l'app con Facebook**, scegliere il **sito Web** sezione.
4. Immettere il **URL sito** campo con l'URL del sito (ad esempio, [ `http://www.example.com` ](http://www.example.com)). Il **Domain** campo è facoltativo; è possibile utilizzare questo per fornire l'autenticazione per un intero dominio (ad esempio *example.com*). 

    > [!NOTE]
    > Se si esegue un sito nel computer locale con un URL come `http://localhost:12345` (dove il numero è un numero di porta locale), è possibile aggiungere questo valore per il **URL del sito** campo per il test del sito. Tuttavia, ogni volta che il numero di porta delle modifiche al sito locale, dovrai aggiornare il **URL sito** campo dell'applicazione.
5. Scegliere il **Save Changes** pulsante.
6. Scegliere il **app** scheda nuovo e quindi visualizzare la pagina iniziale per l'applicazione.
7. Copia il **App ID** e **segreto App** i valori per l'applicazione e incollarli in un file di testo temporaneo. Si passerà questi valori per il provider di Facebook nel codice del sito Web.
8. Chiudere il sito per sviluppatori di Facebook.

A questo punto è apportare modifiche alle due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account di Facebook.

1. In un sito Web, modificare il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Facebook. Il blocco di codice senza commenti è simile al seguente: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Copia il **App ID** valore dall'applicazione Facebook come valore del `consumerKey` parametro (all'interno delle virgolette).
3. Copia **segreto App** valore dall'applicazione Facebook come il `consumerSecret` valore del parametro.
4. Salvare e chiudere il file.
5. Modificare il *~/Account/Login.cshtml* pagina e rimuovere i commenti dal `<fieldset>` blocco verso la fine della pagina. Per rimuovere il commento nel codice, rimuovere il `@*` caratteri che precedono e seguono il `<fieldset>` blocco. Il blocco di codice con commenti rimosso ha un aspetto simile al seguente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Salvare e chiudere il file.

È ora possibile testare l'accesso di Facebook.

1. Esecuzione del sito *default. cshtml* pagina, quindi scegliere il **Login** pulsante.
2. Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** keychains il **Facebook** icona. 

    La pagina web reindirizza la richiesta alla pagina di accesso di Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Accedere a un account di Facebook. 

    Il codice Usa il token di Facebook per l'autenticazione dell'utente e quindi torna in una pagina in cui è possibile associare l'account di accesso di Facebook con account di accesso del sito. L'indirizzo di posta elettronica o nome utente viene compilato nel **messaggio di posta elettronica** campo nel form.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Scegliere il **associare** pulsante. 

    Il browser torna alla home page e si è connessi.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Per abilitare gli account di accesso di Twitter:** 

1. Selezionare il [sito agli sviluppatori di Twitter](https://dev.twitter.com/).
2. Scegliere il **creare un'App** collegamento e quindi accedere al sito.
3. Nel **creare un'applicazione** formano, compilare il **Name** e **descrizione** campi.
4. Nel **sito Web** immettere l'URL del sito (ad esempio [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Se si sta verificando il sito in locale (tramite un URL come `http://localhost:12345`), Twitter potrebbero non accettare l'URL. Tuttavia, potrebbe essere possibile usare l'indirizzo IP di loopback locale (ad esempio `http://127.0.0.1:12345`). Ciò semplifica il processo di test dell'applicazione in locale. Tuttavia, ogni volta che viene modificato il numero di porta del sito locale, è necessario aggiornare il **sito Web** campo dell'applicazione.
5. Nel **URL di Callback** immettere un URL per la pagina nel sito Web che si desidera che gli utenti a cui tornare dopo la registrazione in Twitter. Ad esempio, per inviare gli utenti alla home page del sito Starter (che riconoscerà il relativo stato connesso), immettere lo stesso URL immesso nella **sito Web** campo.
6. Accettare le condizioni e scegliere il **Crea applicazione Twitter** pulsante.
7. Nel **applicazioni personali** pagina di destinazione scegliere l'applicazione creata.
8. Nel **informazioni dettagliate** scheda, scorrere verso il basso e scegliere il **Create My Access Token** pulsante.
9. Nel **dettagli** scheda, copiare la **Consumer Key** e **segreto Consumer** i valori per l'applicazione e incollarli in un file di testo temporaneo. Questi valori verranno passate al provider di Twitter nel codice del sito Web.
10. Uscire dal sito di Twitter.

A questo punto è apportare modifiche alle due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Twitter.

1. In un sito Web, modificare il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Twitter. Il blocco di codice senza commenti è simile alla seguente: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Copia il **Consumer Key** valore dell'applicazione Twitter come valore del `consumerKey` parametro (all'interno delle virgolette).
3. Copia il **Consumer Secret** valore dell'applicazione Twitter come valore del `consumerSecret` parametro.
4. Salvare e chiudere il file.
5. Modificare il *~/Account/Login.cshtml* pagina e rimuovere i commenti dal `<fieldset>` blocco verso la fine della pagina. Per rimuovere il commento nel codice, rimuovere il `@*` caratteri che precedono e seguono il `<fieldset>` blocco. Il blocco di codice con commenti rimosso ha un aspetto simile al seguente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Salvare e chiudere il file.

È ora possibile testare l'accesso a Twitter.

1. Eseguire la *default. cshtml* page del sito e scegliere il **Login** pulsante.
2. Nel *account di accesso* nella pagina il **usi un altro servizio per l'accesso** keychains il **Twitter** icona. 

    La pagina web reindirizza la richiesta a una pagina di accesso di Twitter per l'applicazione creata.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Accedere a un account Twitter.
4. Il codice Usa il token di Twitter per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso con l'account del sito Web. L'indirizzo di posta elettronica o nome viene compilato nel **messaggio di posta elettronica** campo nel form.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Scegliere il **associare** pulsante. 

    Il browser torna alla home page e si è connessi.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Aggiunta di mappe usando l'Helper di mappe

Web Pages 2 include le aggiunte di ASP.NET Web Helpers Library, ovvero un pacchetto di componenti aggiuntivi per un sito Web Pages. Uno di questi è un componente di mapping fornito dal `Microsoft.Web.Helpers.Maps` classe. È possibile usare il `Maps` classe per cui generare le mappe basate su un indirizzo o su un set di coordinate di longitudine e latitudine. Il `Maps` classe consente di chiamare direttamente nei motori di mappa più diffusi tra cui Bing, Google, MapQuest e Yahoo.

Usare il nuovo `Maps` classe nel sito Web, è innanzitutto necessario installare la versione 2 del Web Helpers Library. A tale scopo, passare alle istruzioni per installare la versione attualmente rilasciata del [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) e installare la versione 2.

I passaggi per l'aggiunta di mapping a una pagina sono gli stessi indipendentemente da quale dei motori di mappa è chiamare. È sufficiente aggiungere un riferimento al file JavaScript alla pagina di mapping e quindi aggiungere una chiamata che esegue il rendering di `<script>` tag nella pagina. Nella pagina del mapping, quindi chiamare il motore di mappa da usare.

Nell'esempio seguente viene illustrato come creare una pagina che esegue il rendering di una mappa in base all'indirizzo e un'altra pagina che esegue il rendering di una mappa in base alle coordinate di longitudine e latitudine. L'esempio di mapping di indirizzi Usa Google Maps e l'esempio di mapping delle coordinate Usa Bing Maps. Tenere presente i seguenti elementi nel codice:

- La chiamata a `Assets.AddScript` nella parte superiore di due pagine di mapping. Questo metodo aggiunge un riferimento al *jquery 1.6.2.min.js* file incluso con il **Starter Site** modello di cui è necessaria per eseguire il `Maps` classe.
- La chiamata al `Assets.GetScripts` metodo nel file di layout. Questo metodo esegue il rendering di `<script>` tag nelle pagine di mapping di due.
- La chiamata per il `@Maps.GetGoogleHtml` e il `@Maps.GetBingHtml` metodi nelle pagine di mapping. Per eseguire il mapping di un indirizzo, è necessario passare una stringa di indirizzo. Per eseguire il mapping delle coordinate, è necessario passare la longitudine e latitudine delle coordinate. Per il motore di Bing mappe, è necessario passare anche una chiave (che è ottenere gratuitamente iscrivendosi al [sito degli sviluppatori di Bing mappe](https://www.microsoft.com/maps/developers/web.aspx)). I metodi per altri motori di mappa funzionano in modo analogo (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Per creare pagine di mapping:

1. Creare un sito Web basato sul **Starter Site** modello.
2. Creare un file denominato *MapAddress.cshtml* nella radice del sito. Questa pagina genera una mappa in base all'indirizzo che viene passato ad esso.
3. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Creare un file denominato  *\_MapLayout.cshtml* nella radice del sito. Questa pagina sarà la pagina di layout per le due pagine di mapping.
5. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Creare un file denominato *MapCoordinates.cshtml* nella radice del sito. Questa pagina genera una mappa basata su un set di coordinate che viene passato ad esso.
7. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Per testare le pagine di mapping:

1. Eseguire la pagina *MapAddress.cshtml* file.
2. Immettere una stringa di indirizzo completo incluso un numero civico, stato o provincia e codice postale e quindi scegliere il **Map It** pulsante. La pagina esegue il rendering di una mappa da Google Maps: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Trovare un set di coordinate di latitudine e longitudine per una località specifica.
4. Eseguire la pagina *MapCoordinates.cshtml*. Immettere le coordinate e quindi scegliere il **Map It** pulsante. La pagina esegue il rendering di una mappa da Bing mappe: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Esecuzione di applicazioni Web Pages Side-by-Side

Web Pages 2 aggiunge la possibilità di eseguire applicazioni affiancate. È possibile continuare a eseguire le applicazioni Web Pages 1, creare nuove applicazioni Web Pages 2 ed eseguire tutti gli elementi nello stesso computer.

Ecco alcuni aspetti da ricordare quando si installa la versione Beta 2 di pagine Web con WebMatrix:

- Per impostazione predefinita, le applicazioni Web Pages esistente verranno eseguita come applicazioni della versione 2 nel computer. (Gli assembly per la versione 2 vengono installati nella Global Assembly Cache e verranno usati automaticamente).
- Se si desidera eseguire un sito con Web Pages versione 1 (anziché il valore predefinito, come illustrato al punto precedente), è possibile configurare il sito a tale scopo. Se il sito non ha ancora un *Web. config* file nella radice del sito, crearne uno nuovo e copiarvi il codice XML seguente, sovrascrivendo il contenuto esistente. Se il sito contiene già un *Web. config* Aggiungi un' `<appSettings>` elemento simile a quello riportato di seguito per il `<configuration>` sezione.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-Se non si specifica una versione nel *Web. config* file, un sito viene distribuito come un sito di versione 2. (Gli assembly della versione 2 vengono copiati i *bin* cartella nel sito distribuito.)
- Le nuove applicazioni che si creano utilizzando i modelli di sito in versione Web Matrix Beta 2 includono gli assembly di Web Pages versione 2 del sito *bin* cartella.

In generale, è sempre possibile controllare quale versione di pagine Web da utilizzare con il sito con NuGet per installare gli assembly adatti al sito *bin* cartella. Per trovare i pacchetti, visitare [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Il rendering di pagine per i dispositivi mobili

Web Pages 2 consente di creare visualizzazioni personalizzate per il rendering del contenuto in altri dispositivi o per dispositivi mobili.

Il `System.Web.WebPages` spazio dei nomi contiene le classi seguenti che consentono di lavorare con le modalità di visualizzazione: `DefaultDisplayMode`, `DisplayInfo`, e `DisplayModes`. È possibile usare direttamente queste classi e scrivere il codice che esegue il rendering di output destra per i dispositivi specifici.

In alternativa, è possibile creare pagine specifiche del dispositivo usando un modello di denominazione file simile al seguente: <em>nomefile.</em> <em>Mobile</em><em>cshtml</em>. Ad esempio, è possibile creare due versioni di una pagina, uno denominato <em>MyFile.cshtml</em> e uno denominato <em>MyFile.Mobile.cshtml</em>. In fase di esecuzione, quando un dispositivo mobile richiede <em>MyFile.cshtml</em>, le pagine Web viene eseguito il rendering del contenuto dalla <em>MyFile.Mobile.cshtml</em>. In caso contrario, <em>MyFile.cshtml</em> viene eseguito il rendering.

Nell'esempio seguente viene illustrato come abilitare il rendering per dispositivi mobili tramite l'aggiunta di una pagina di contenuto per i dispositivi mobili. *Page1.cshtml* contiene contenuto oltre a una barra laterale di navigazione. *Page1.Mobile.cshtml* lo stesso contenuto, ma omette l'intestazione laterale.

Per compilare ed eseguire l'esempio di codice:

1. In un sito Web Pages, creare un file denominato *Page1.cshtml* e copiare la *Page1.cshtml* contenuti al suo interno dall'esempio.
2. Creare un file denominato *Page1.Mobile.cshtml* e copiare la *Page1.Mobile.cshtml* contenuti al suo interno dall'esempio. Si noti che la versione mobile della pagina omette la sezione di spostamento per una migliore riproduzione su uno schermo piccolo.
3. Eseguire un browser desktop e passare alla *Page1.cshtml*.
4. Eseguire un browser per dispositivi mobili (o un emulatore di dispositivi mobili) e passare alla *Page1.cshtml*. Si noti che questa volta le pagine Web viene eseguito il rendering di versione per dispositivi mobili della pagina. 

    > [!NOTE]
    > Per testare le pagine per dispositivi mobili, è possibile usare un simulatore di dispositivi mobili che viene eseguito in un computer desktop. Questo strumento consente di testare le pagine web come appaiono nei dispositivi mobili (vale a dire, in genere con un notevolmente ridotta visualizzare l'area). Un esempio di un simulatore è il [componente aggiuntivo di selezione dell'agente utente](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) per Mozilla Firefox, che consente di emulare diversi browser per dispositivi mobili da una versione desktop di Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* sottoposto a rendering in un browser desktop:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* visualizzato in una visualizzazione di simulatore iPhone Apple nel browser Firefox. Anche se la richiesta è su *Page1.cshtml*, l'applicazione esegue il rendering *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages 1 risorse

> [!NOTE]
> La maggior parte dei programmatori di Web Pages 1 e alle risorse dell'API vengono mantenuti da Web Pages 2.

- [Introduzione alla programmazione di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Risorse di WebMatrix

- [WebMatrix 2 nuove funzionalità](http://webmatrix.com/next)
- [Sito di Microsoft WebMatrix](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Avviare lo sviluppo Web con Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(include un'applicazione di pagine Web di esempio esaustiva)
