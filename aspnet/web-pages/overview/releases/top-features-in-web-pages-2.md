---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Nella parte superiore delle funzionalità in ASP.NET Web Pages 2 | Documenti Microsoft
author: microsoft
description: In questo argomento viene fornita una panoramica delle nuove funzionalità in ASP.NET Web Pages 2, un framework di programmazione web semplice che è incluso il WebMatr superiore...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: e8fc758936953970ff3e9ba289516925dee9ef45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Le funzionalità principali di ASP.NET Web Pages 2
====================
da [Microsoft](https://github.com/microsoft)

> In questo articolo viene fornita una panoramica delle nuove funzionalità in ASP.NET Web Pages 2 RC, un framework di programmazione web semplice inclusa superiore [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Cosa è incluso:** 
> 
> - [Installazione di WebMatrix](#install)
> - [Funzionalità nuove e migliorate](#New_and_Enhanced_Features)
> 
>     - [Modifiche per la versione RC](#Changes_for_the_RC_Version)
>     - [Modifiche per la versione Beta](#Changes_for_the_Beta_Version)
>     - [Utilizzando i modelli di sito nuovi e aggiornati](#templates)
>     - [Convalida dell'Input utente](#validation)
>     - [Abilitazione di account di accesso di Facebook e ad altri siti con OAuth e OpenID](#oauthsetup)
>     - [Aggiunta di mapping utilizzando l'Helper di mappe](#maphelper)
>     - [Esecuzione di applicazioni Web Pages Side-by-Side](#sidebyside)
>     - [Il rendering di pagine per i dispositivi mobili](#mobile)
> - [Risorse aggiuntive](#resources)
> 
> > [!NOTE]
> > In questo argomento si presuppone che si utilizza WebMatrix per lavorare con il codice ASP.NET Web Pages 2. Tuttavia, come con 1 le pagine Web, è inoltre possibile creare siti Web di Web Pages 2 utilizzando Visual Studio, che consente di ora le funzionalità di IntelliSense e il debug. Per utilizzare le pagine Web in Visual Studio, è necessario installare Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 o Visual Studio 11 Beta. Quindi installare ASP.NET MVC 4 Beta, che include modelli e strumenti per la creazione di applicazioni ASP.NET MVC 4 e 2 pagine Web in Visual Studio.
> 
> 
> *Ultimo aggiornamento: 18 giugno 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Installazione di WebMatrix

Per installare le pagine Web, è possibile utilizzare l'installazione guidata piattaforma Web Microsoft, un'applicazione gratuita che rende più semplice installare e configurare tecnologie di web. Sarà necessario installare la versione Beta 2 di WebMatrix, che include una versione Beta 2 di pagine Web.

1. Passare alla pagina di installazione per la versione più recente dell'installazione guidata piattaforma Web:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Se è già installato WebMatrix 1, l'installazione degli aggiornamenti a Beta 2 di WebMatrix. È possibile eseguire i siti Web che sono stati creati con la versione 1 o 2 nello stesso computer. Per ulteriori informazioni, vedere la sezione su [applicazioni pagine Web in esecuzione Side-by](#sidebyside).
2. Scegliere **installa**. 

    Se si utilizza Internet Explorer, andare al passaggio successivo. Se si utilizza un altro browser Mozilla Firefox o Google Chrome, viene chiesto di salvare il *Webmatrix.exe* file nel computer. Salvare il file e quindi fare clic su esso per avviare il programma di installazione.
3. Eseguire il programma di installazione e scegliere il **installare** pulsante. Consente di installare WebMatrix e pagine Web.

## <a id="New_and_Enhanced_Features"></a>Funzionalità nuove e migliorate

### <a id="Changes_for_the_RC_Version"></a>Modifiche per la versione RC (giugno 2012)

La versione RC versione nel giugno 2012 presenta alcune modifiche dall'aggiornamento della versione Beta è stata rilasciata nel marzo 2012. Queste modifiche sono:

- Oggetto `Validation.AddFormError` metodo è stato aggiunto il `Validation` helper. Ciò è utile se si esegue manualmente la convalida (ad esempio, si convalida un valore che viene passato nella stringa di query) e si desidera aggiungere un messaggio di errore che può essere visualizzato per il `Html.ValidationSummary` metodo. Per ulteriori informazioni, vedere la sezione [convalida dati che non derivano direttamente da utenti](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) in [convalida dell'Input utente nei siti di pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253002).
- La funzionalità per l'aggregazione e riduzione è stato rimosso dagli assembly di ASP.NET Web Pages 2 core. Di conseguenza, il `Assets` helper elencati più avanti in questo documento non è disponibile. In alternativa, è necessario installare il [ottimizzazione ASP.NET](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) pacchetto NuGet. Per ulteriori informazioni, vedere [Bundling and Minifying asset in un sito di pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Assembly aggiuntivi per supportare ASP.NET Web Pages 2 sono stati aggiunti. L'effetto di questa modifica solo evidente è che potrebbero essere visualizzati più assembly in un sito *bin* cartella dopo aver creato un sito o distribuire un sito per un provider di hosting.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Modifiche per la versione Beta (febbraio 2012)

La versione Beta rilasciata in febbraio 2012 dispone solo poche modifiche dalla versione Beta è stata rilasciata nel dicembre 2011. Queste modifiche sono:

- Razor supporta ora attributi condizionali. In un oggetto elemento, se si imposta un attributo a un valore che si risolve nel codice server `false` o `null`, ASP.NET non esegue il rendering dell'attributo. Si supponga, ad esempio, che è il seguente codice per una casella di controllo:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Se il valore di `checked1` si risolve in `false` o `null`, `checked` attributo non viene eseguito il rendering. Si tratta di una modifica di rilievo.
- Il `Validation.GetHtml` metodo è stato rinominato in `Validation.For`. Si tratta di una modifica di rilievo; `Validation.GetHtml` non funzionerà nella versione Beta.
- È ora possibile includere il `~` operatore nel markup di riferimento radice del sito senza usare il `Href` (funzione). (Ovvero, il parser Razor può ora trovare e risolvere il `~` operatore senza una chiamata esplicita a `Href`.) Il `Href` metodo continui a funzionare, in modo non si tratta di una modifica di rilievo.

    Ad esempio, se in precedenza era markup simile al seguente:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    È ora possibile usare codice simile al seguente:

    `<a href="~/Default.cshtml">Home</a>`
- Il `Scripts` helper per la gestione delle risorse (resource) è stato sostituito con il `Assets` helper, che dispone di metodi leggermente diversi, come illustrato di seguito:

    - Per `Scripts.Add`, usare`Assets.AddScript`
    - Per `Scripts.GetScriptTags`, usare`Assets.GetScripts`

    Si tratta di una modifica di rilievo; la `Scripts` classe non è disponibile nella versione Beta. Esempi di codice che utilizzano la gestione delle risorse in questo documento sono stati aggiornati con questa modifica.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Utilizzando i modelli di sito nuovi e aggiornati

Il **Starter Site** modello è stato aggiornato in modo che venga eseguito in Web Pages 2 per impostazione predefinita. Sono inoltre incluse le nuove funzionalità seguenti:

- Rendering della pagina mobile descrittivi. Tramite l'utilizzo di stili CSS e `@media` selettore, il **Starter Site** fornisce una migliore per il rendering di pagine su schermi più piccoli, inclusi gli schermi dei dispositivi mobili.
- Opzioni migliorate di appartenenza e l'autenticazione. È possibile consentire agli utenti l'accesso al sito utilizzando account di altri siti di social networking, quali Twitter, Facebook e Windows Live. Per ulteriori informazioni, vedere il [abilitazione di account di accesso di Facebook e altri siti con OAuth e OpenID](#oauthsetup) sezione.
- Elementi di HTML5.

Il nuovo **sito personale** modello consente di creare un sito Web che contiene un blog personale, una pagina foto e una pagina di Twitter. È possibile personalizzare un sito basato sul **sito personale** modello effettuando le operazioni seguenti:

- Modificare l'aspetto del sito modificando il file di layout (*\_SiteLayout.cshtml*) e il file di stili (*Site*).
- Installare i pacchetti NuGet che aggiungono funzionalità al sito. Per informazioni su come installare i pacchetti, tra cui ASP.NET Web Helpers Library, vedere l'esercitazione [installazione helper](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Per l'accesso di **sito personale** modello, scegliere **modelli** di WebMatrix **avvio rapido** dello schermo.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

Nel **modelli** finestra di dialogo scegliere la **sito personale** modello.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

La pagina di destinazione del **sito personale** modello consente di seguire i collegamenti per configurare il tuo blog, Twitter, pagina e foto.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Convalida dell'Input utente

In pagine Web 1, per convalidare l'input dell'utente nei form inviato, utilizzare la `System.Web.WebPages.Html.ModelState` classe. (Come illustrato in molti degli esempi di codice dell'esercitazione 1 di pagine Web denominato [utilizzo dei dati](../data/5-working-with-data.md).) È comunque possibile utilizzare questo approccio 2 pagine Web. Tuttavia, pagine Web 2 offre inoltre strumenti ottimizzati per la convalida dell'input dell'utente:

- Nuove classi di convalida, tra cui `System.Web.WebPages.ValidationHelper` e `System.Web.WebPages.Validator`, che consentono di eseguire attività di convalida potente con poche righe di codice.
- Facoltativamente, la convalida lato client, che fornisce un feedback immediato all'utente anziché richiedere un round trip al server di controllo degli errori di convalida. (Per motivi di sicurezza, la convalida viene eseguita nel server anche se i controlli sono stati eseguiti nel client in anticipo.)

Per utilizzare le nuove funzionalità di convalida, eseguire le operazioni seguenti:

Nel codice della pagina, registrare un elemento da convalidare tramite metodi del `Validation` helper: `Validation.RequireField`, `Validation.RequireFields` (per registrare più elementi per essere necessari), o `Validation.Add`. Il `Add` metodo consente di specificare altri tipi di controlli di convalida, come tipo di dati per il controllo, il confronto di campi diversi, i controlli di lunghezza della stringa e modelli (usando espressioni regolari). Ecco alcuni esempi:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Per visualizzare un errore specifico del campo, chiamare `Html.ValidationMessage` nel markup per ogni elemento da convalidare:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Per visualizzare un riepilogo (`<ul>` elenco) di tutti gli errori di pagina, `Html.ValidationSummary` nel markup:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Questi passaggi sono sufficienti per implementare la convalida sul lato server. Se si desidera aggiungere la convalida lato client, eseguire inoltre le operazioni seguenti.

Aggiungere i seguenti riferimenti di file di script all'interno di `<head>` sezione di una pagina web. I primi due riferimenti a script scegliere i file remoti in un server di distribuzione di contenuti (CDN) di rete. Il terzo riferimento punta a un file di script locale.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Il modo più semplice per ottenere una copia locale del *jquery.validate.unobtrusive.min.js* libreria consiste nel creare un nuovo sito di pagine Web in base a uno dei modelli di sito (ad esempio Starter Site). Il sito creato tramite il modello include *jquery.validate.unobtrusive.js* file nella cartella di script, da cui è possibile copiarlo nel sito.

Se il sito Web utilizza un*\_SiteLayout* pagina per controllare il layout di pagina, è possibile includere riferimenti a questi script in questa pagina in modo che la convalida è disponibile per tutte le pagine di contenuto. Se si desidera eseguire la convalida solo in determinate pagine, è possibile utilizzare il gestore delle risorse per registrare gli script solo le pagine. A tale scopo, chiamare `Assets.AddScript(path)` nella pagina che si desidera convalidare e fare riferimento a ciascuno dei file di script. Aggiungere quindi una chiamata a `Assets.GetScripts` nel  *\_SiteLayout* pagina per eseguire il rendering registrato `<script>` tag. Per ulteriori informazioni, vedere la sezione [gli script di registrazione con il gestore di risorse](#resmanagement).

Nel markup per un singolo elemento, chiamare il `Validation.For` metodo. Questo metodo genera gli attributi possono associare tale jQuery per fornire la convalida lato client. Ad esempio:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Nell'esempio seguente viene mostrata una pagina che convalida l'input dell'utente in un form. Per eseguire e testare questo codice di convalida, eseguire questa operazione:

1. Creare un nuovo sito web utilizzando uno dei modelli di sito WebMatrix 2 che include un *script* cartella, ad esempio il **Starter Site** modello.
2. Nel nuovo sito, creare un nuovo *. cshtml* pagina e sostituire il contenuto della pagina con il codice seguente.
3. Eseguire la pagina in un browser. Immettere i valori validi e non validi per osservare gli effetti sulla convalida. Ad esempio, lasciare vuoto un campo obbligatorio o immettere una lettera di **crediti** campo.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Quando un utente invia un input valido, ecco la pagina:

[![topSeven 1 valido](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Quando un utente invia un campo obbligatorio lasciato vuoto, ecco la pagina:

[![topSeven 2 valido](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Di seguito è la pagina quando un utente invia con un valore diverso da un numero intero di **crediti** campo:

[![topSeven 3 valido](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Per ulteriori informazioni, vedere il post di blog seguenti:

- [Convalida in pagine Web v2 aggiornato](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) nozioni fondamentali sull'aggiunta di convalida utilizzando il `Validation` helper (solo server-side)
- [Convalida in pagine Web v2, parte 2 di aggiornamento](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) aggiunta della convalida lato client.
- [Convalida in pagine Web v2, parte 3 aggiornato](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) formattazione gli errori di convalida.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>La registrazione di script utilizzando la gestione di risorse

La gestione di asset è una nuova funzionalità che è possibile utilizzare nel codice server per registrare ed eseguire il rendering di script client. Questa funzionalità è utile quando si lavora con il codice da più file (ad esempio pagine di layout, pagine di contenuto, helper e così via) che vengono combinate in una singola pagina in fase di esecuzione. La gestione di asset coordina i file di origine per assicurarsi che i file di script vengono fatto riferimento in modo corretto e in modo efficiente nella pagina di cui è stato eseguito rendering, indipendentemente dal fatto che i file di codice vengono chiamati o quante volte vengono chiamati. Esegue il rendering anche di gestione di asset `<script>` tag nella posizione corretta, in modo che la pagina è possibile caricare rapidamente (senza dover scaricare script durante il rendering) e per evitare errori che possono verificarsi se gli script vengono chiamati prima del rendering è stata completata.

Ad esempio, si supponga di crea un helper personalizzato che chiama un file JavaScript, e si chiama questo helper in tre posizioni differenti nel codice della pagina contenuto. Se non si utilizza la gestione di asset per registrare lo script chiama nell'helper della tre diverse `<script>` tag che tutti i punti nello stesso file di script verranno visualizzato nella pagina sottoposta a rendering. Inoltre, a seconda di dove il `<script>` i tag vengono inseriti nella pagina sottoposta a rendering, potrebbero verificarsi errori se lo script tenta di accedere a determinati elementi di pagina prima del caricamento della pagina completamente. Se si utilizza la gestione di asset per registrare lo script, evitare questi problemi.

In questo modo, è possibile registrare uno script con il gestore di risorse:

- Nel codice che deve fare riferimento allo script, chiamare il `Assets.AddScript` metodo.
- In un  *\_SiteLayout* pagina, chiamare il `Assets.GetScripts` metodo per eseguire il rendering di `<script>` tag. 

    > [!NOTE]
    > Inserire le chiamate a `Assets.GetScripts` come ultimo elemento nel `<body>` elemento del  *\_SiteLayout* pagina. In questo modo la pagina caricamento più rapido e consentono di evitare gli errori di script.

Nell'esempio seguente viene illustrato il funzionamento di gestione di asset. Il codice contiene gli elementi seguenti:

- Un helper personalizzato denominato `MakeNote`. Questo supporto viene eseguito il rendering di una stringa all'interno di una casella eseguendo il wrapping di un `div` elemento che abbia uno stile con un bordo e aggiungendo &quot;Nota:&quot; a esso. Il supporto chiama inoltre un file JavaScript che aggiunge il comportamento in fase di esecuzione per la nota. Anziché fare riferimento allo script con un `<script>` tag, l'helper registra lo script chiamando `Assets.AddScript` .
- Un file JavaScript. Si tratta del file che viene chiamato da helper e temporaneamente aumenta le dimensioni del carattere di elementi di nota durante un `mouseover` evento.
- Una pagina di contenuto, che fa riferimento il*\_SiteLayout* esegue il rendering di alcuni contenuti nel corpo della pagina e quindi chiama il `MakeNote` helper.
- Oggetto  *\_SiteLayout* pagina. Questa pagina fornisce un'intestazione comune e una struttura di layout di pagina. Include inoltre una chiamata a `Assets.GetScripts`, ovvero come la gestione di attività esegue il rendering di script chiama in una pagina.

Per eseguire l'esempio:

1. Creare un sito Web di Web Pages 2 vuoto. È possibile utilizzare il WebMatrix **sito vuoto** modello per questo oggetto.
2. Creare una cartella denominata *script* nel sito.
3. Nel *script* cartella, creare un file denominato *Test.js*, copia il *Test.js* dell'esempio di contenuto al suo interno e salvare il file...
4. Creare una cartella denominata *App\_codice* nel sito.
5. Nel *App\_codice* cartella, creare un file denominato *Helpers.cshtml*, copiarvi il codice di esempio e salvarlo in una cartella denominata *App\_codice*nella cartella radice.
6. Nella cartella radice del sito, creare un file denominato  *\_SiteLayout.cshtml,* copiare l'esempio al suo interno e salvare il file.
7. Nella radice del sito, creare un file denominato *ContentPage.cshtml*, aggiungere il codice di esempio e salvarlo.
8. Eseguire *ContentPage* in un browser. La stringa passata al `MakeNote` helper viene visualizzato come una nota boxed.
9. Passare il puntatore del mouse sulla nota. Lo script aumenta temporaneamente la dimensione del carattere della nota.
10. Visualizzare l'origine della pagina rendering. A causa di cui è stato eseguito la chiamata a `Assets.GetScripts`, il rendering `<script>` tag che chiama *Test.js* è l'ultimo elemento nel corpo della pagina.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

La seguente schermata mostra *ContentPage.cshtml* in un browser quando si posiziona il puntatore del mouse su Nota:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Abilitazione di account di accesso di Facebook e ad altri siti con OAuth e OpenID

Pagine Web 2 offre opzioni avanzate per l'appartenenza e l'autenticazione. La funzionalità avanzata di principale è che esistono nuove [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provider. Utilizzando questi provider, è possibile consentire agli utenti l'accesso al sito utilizzando le credenziali esistenti da Facebook, Twitter, Windows Live, Google e Yahoo. Ad esempio, per accedere con un account Facebook, gli utenti possono solo scegliere un'icona di Facebook, che li reindirizza alla pagina di accesso di Facebook in cui inserire le informazioni utente. È quindi possibile associare l'account di accesso di Facebook con il proprio account nel sito. Una funzionalità avanzata correlata alle funzionalità di appartenenza a pagine Web è che gli utenti possono associare più account di accesso (inclusi gli accessi dai siti di social networking) con un singolo account nel sito Web.

La seguente immagine illustra la pagina account di accesso di **Starter Site** modello, in cui un utente può scegliere un'icona di Facebook, Twitter o Windows Live per abilitare l'accesso con un account esterno:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

È possibile abilitare l'appartenenza OAuth e OpenID con poche righe di codice. I metodi e proprietà consentono di lavorare con OAuth e i provider OpenID sono nella `WebMatrix.Security.OAuthWebSecurity` classe.

Tuttavia, anziché utilizzare il codice per abilitare gli account di accesso da altri siti, un metodo consigliato per iniziare a nuovi provider consiste nell'utilizzare il nuovo **Starter Site** modello incluso in WebMatrix 2 Beta. Il **Starter Site** modello include un'infrastruttura completa dell'appartenenza, completo di una pagina di accesso, un database di appartenenza e tutto il codice necessario consentire agli utenti l'accesso al sito usando le credenziali locali o da un altro sito .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Come abilitare gli account di accesso con l'OAuth e il provider OpenID

In questa sezione viene fornito un esempio di come consentire agli utenti di accedere da siti esterni (Facebook, Twitter, Windows Live, Google o Yahoo) a un sito che si basa sul **Starter Site** modello. Dopo aver creato un sito di partenza, tale scopo (dettagli):

- Per i siti che usano un provider OAuth (Facebook, Twitter e Windows Live), creare un'applicazione nel sito esterno. In questo modo le chiavi dell'applicazione che è necessario per richiamare la funzionalità di accesso per tali siti. Per i siti che usano un provider OpenID (Google, Yahoo), non si dispone creare un'applicazione. Per tutti i siti, è necessario disporre di un account per accedere e creare applicazioni dello sviluppatore. 

    > [!NOTE]
    > Applicazioni di Windows Live accettano solo un URL in tempo reale per un sito Web di lavoro, non è possibile utilizzare un URL del sito Web locale per il test degli account di accesso.
- Modifica di alcuni file nel sito Web per specificare il provider di autenticazione appropriato e per l'invio di un account di accesso al sito che si desidera utilizzare.

**Per abilitare gli account di accesso di Google e Yahoo**:

1. Nel sito Web, è possibile modificare il  *\_AppStart.cshtml* pagina e aggiungere le due righe seguenti del codice nel blocco di codice Razor dopo la chiamata al `WebSecurity.InitializeDatabaseConnection` metodo. Questo codice consente ai provider di Google sia OpenID di Yahoo. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. Nel *~/Account/Login.cshtml* pagina, rimuovere i commenti dalla seguente `<fieldset>` blocco di markup verso la fine della pagina. Per rimuovere il commento di codice, rimuovere il `@*` caratteri che precedono e seguono il `<fieldset>` blocco. Il blocco di codice risultante è simile al seguente:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Aggiungere un `<input>` elemento per il provider di Google o Yahoo per il `<fieldset>` gruppo il *~/Account/Login.cshtml* pagina. L'aggiornamento `<fieldset>` al gruppo `<input>` elementi ha un aspetto sia Google e Yahoo come nell'esempio seguente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. Nel *~/Account/AssociateServiceAccount.cshtml* pagina, aggiungere `<input>` elementi per Google o Yahoo per il `<fieldset>` gruppo verso la fine del file. È possibile copiare lo stesso `<input>` elementi appena aggiunto per il `<fieldset>` sezione la *~/Account/Login.cshtml* pagina. 

    Il *~/Account/AssociateServiceAccount.cshtml* pagina nel modello Starter Site può essere utilizzata se si desidera creare una pagina in cui gli utenti possono associare più account di accesso da altri siti con un singolo account nel sito Web.

È ora possibile testare gli account di accesso di Google e Yahoo.

1. Eseguire il *cshtml* pagina del sito e scegliere il **Accedi** pulsante.
2. Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** sezione, scegliere il **Google** o **Yahoo** pulsante di invio. Questo esempio viene utilizzato l'account di accesso di Google. 

    La pagina web reindirizza la richiesta alla pagina di accesso di Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Immettere le credenziali per un account esistente di Google.
4. Se Google viene chiesto se si desidera consentire Localhost utilizzare le informazioni dell'account, fare clic su **Consenti**.

    Il codice Usa il token di Google per autenticare l'utente e torna a questa pagina nel sito Web. Questa pagina consente agli utenti di associare i relativi account di accesso di Google a un account esistente nel sito Web o cui possono registrare un nuovo account del sito per associare l'account di accesso esterno con.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Scegliere il **associare** pulsante. Il browser restituisce alla pagina iniziale dell'applicazione.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Per abilitare gli account di accesso di Facebook**:

1. Passare al [sito agli sviluppatori di Facebook](https://developers.facebook.com/apps) (accedere se non è già connesso).
2. Scegliere il **Create New App** pulsante e quindi seguire le istruzioni per assegnare un nome e creare la nuova applicazione.
3. Nella sezione **selezionare come permetterà di combinare l'app con Facebook**, scegliere il **sito Web** sezione.
4. Compilare il **URL del sito** campo con l'URL del sito (ad esempio, [ `http://www.example.com` ](http://www.example.com)). Il **dominio** campo è facoltativo; è possibile utilizzare questo metodo per fornire l'autenticazione per un intero dominio (ad esempio *example.com*). 

    > [!NOTE]
    > Se si esegue un sito nel computer locale con un URL come `http://localhost:12345` (dove il numero è un numero di porta locale), è possibile aggiungere questo valore per il **URL del sito** campo per il test del sito. Tuttavia, ogni volta che il numero di porta delle modifiche del sito locale, sarà necessario aggiornare il **URL del sito** campo dell'applicazione.
5. Scegliere il **Salva modifiche** pulsante.
6. Scegliere il **app** scheda nuovo e quindi visualizzare la pagina iniziale per l'applicazione.
7. Copia il **ID App** e **segreto dell'App** valori per l'applicazione e incollarli in un file di testo temporaneo. Questi valori verrà passato al provider di Facebook nel codice del sito Web.
8. Chiudere il sito per sviluppatori di Facebook.

Ora è apportare modifiche a due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Facebook.

1. Nel sito Web, è possibile modificare il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Facebook. Il blocco di codice senza commenti è simile al seguente: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Copia il **ID App** valore dall'applicazione Facebook come il valore di `consumerKey` parametro (racchiuso tra virgolette).
3. Copia **segreto dell'App** valore dall'applicazione Facebook come il `consumerSecret` valore del parametro.
4. Salvare e chiudere il file.
5. Modificare il *~/Account/Login.cshtml* pagina e rimuovere i commenti dal `<fieldset>` blocco verso la fine della pagina. Per rimuovere il commento di codice, rimuovere il `@*` caratteri che precedono e seguono il `<fieldset>` blocco. Il blocco di codice con commenti rimosso ha un aspetto simile al seguente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Salvare e chiudere il file.

Ora è possibile verificare l'account di accesso di Facebook.

1. Eseguire il sito *cshtml* pagina e selezionare il **accesso** pulsante.
2. Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** , scegliere il **Facebook** icona. 

    La pagina web reindirizza la richiesta alla pagina di accesso Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Accedere a un account Facebook. 

    Il codice Usa il token di Facebook per autenticare l'utente e quindi restituisce a una pagina in cui è possibile associare l'account di accesso di Facebook con account di accesso del sito. L'indirizzo di posta elettronica o nome utente verrà inserita nel **posta elettronica** campo nel form.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Scegliere il **associare** pulsante. 

    Il browser restituisce alla home page e si è connessi.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Per abilitare gli account di accesso Twitter:** 

1. Individuare il [sito agli sviluppatori di Twitter](https://dev.twitter.com/).
2. Scegliere il **creare un'App** collegamento e quindi accedere al sito.
3. Nel **creare un'applicazione** modulo, compilare il **nome** e **descrizione** campi.
4. Nel **sito Web** immettere l'URL del sito (ad esempio, [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Se si sta testando il sito locale (tramite un URL come `http://localhost:12345`), Twitter potrebbero non accettare l'URL. Tuttavia, potrebbe essere in grado di utilizzare l'indirizzo IP di loopback locale (ad esempio `http://127.0.0.1:12345`). Questa operazione semplifica il processo di test dell'applicazione in locale. Tuttavia, ogni volta che viene modificato il numero di porta del sito locale, è necessario aggiornare il **sito Web** campo dell'applicazione.
5. Nel **URL Callback** immettere un URL per la pagina nel sito Web che si desidera che gli utenti restituire dopo la registrazione in Twitter. Ad esempio, per inviare gli utenti alla home page del sito Starter (che verrà riconosciuto loro stato di accesso), immettere lo stesso URL immesso nel **sito Web** campo.
6. Accettare le condizioni e scegliere il **creare un'applicazione Twitter** pulsante.
7. Nel **applicazioni personali** pagina di destinazione scegliere l'applicazione creata.
8. Nel **dettagli** scheda, scorrere verso il basso e scegliere il **creare risorse del Token di accesso** pulsante.
9. Nel **dettagli** scheda, copiare il **chiave Consumer** e **segreto del cliente** i valori per l'applicazione e incollarli in un file di testo temporaneo. Questi valori verrà passato al provider di Twitter nel codice del sito Web.
10. Chiudere il sito di Twitter.

Ora è apportare modifiche a due pagine nel sito Web in modo che gli utenti saranno in grado di accedere al sito usando i propri account Twitter.

1. Nel sito Web, è possibile modificare il  *\_AppStart.cshtml* pagina e rimuovere il commento il codice per il provider OAuth di Twitter. Il blocco di codice senza commenti è simile al seguente: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Copia il **chiave Consumer** valore dall'applicazione Twitter come valore della `consumerKey` parametro (racchiuso tra virgolette).
3. Copia il **segreto del cliente** l'applicazione di Twitter come valore del valore di `consumerSecret` parametro.
4. Salvare e chiudere il file.
5. Modificare il *~/Account/Login.cshtml* pagina e rimuovere i commenti dal `<fieldset>` blocco verso la fine della pagina. Per rimuovere il commento di codice, rimuovere il `@*` caratteri che precedono e seguono il `<fieldset>` blocco. Il blocco di codice con commenti rimosso ha un aspetto simile al seguente: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Salvare e chiudere il file.

Ora è possibile verificare l'account di accesso Twitter.

1. Eseguire il *cshtml* pagina del sito e scegliere il **accesso** pulsante.
2. Nel *account di accesso* nella pagina di **utilizzare un altro servizio per l'accesso** , scegliere il **Twitter** icona. 

    La pagina web reindirizza la richiesta a una pagina di accesso Twitter per l'applicazione creata.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Accedere a un account Twitter.
4. Il codice Usa il token di Twitter per autenticare l'utente e quindi torna a una pagina in cui è possibile associare l'account di accesso con l'account del sito Web. Il nome o indirizzo di posta verrà inserita nel **posta elettronica** campo nel form.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Scegliere il **associare** pulsante. 

    Il browser restituisce alla home page e si è connessi.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Aggiunta di mapping utilizzando l'Helper di mappe

2 pagine Web include le aggiunte di ASP.NET Web Helpers Library, che è un pacchetto dei componenti aggiuntivi per un sito Web Pages. Uno di questi è fornito da un componente di mapping di `Microsoft.Web.Helpers.Maps` classe. È possibile utilizzare la `Maps` classe per generare mappe in base a un indirizzo o su un set di coordinate di longitudine e latitudine. La `Maps` classe consente di chiamare direttamente nei motori di mappa più diffusi tra Bing, Google, ricerca mappa e Yahoo.

Usare il nuovo `Maps` classe nel sito Web, è necessario innanzitutto installare la versione 2 di Web Helpers Library. A tale scopo, utilizzare le istruzioni per installare la versione di attualmente il [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) e installare la versione 2.

I passaggi per aggiungere il mapping a una pagina sono gli stessi indipendentemente da quale dei motori di mappa è chiamare il metodo. È sufficiente aggiungere un riferimento al file JavaScript alla pagina mapping e quindi aggiungere una chiamata che esegue il rendering di `<script>` tag della pagina. Nella pagina mapping, quindi chiamare il motore di mappa da utilizzare.

Nell'esempio seguente viene illustrato come creare una pagina che esegue il rendering di una mappa in base all'indirizzo e un'altra pagina che esegue il rendering di una mappa in base alle coordinate di longitudine e latitudine. Nell'esempio di mapping di indirizzi Usa Google Map, e nell'esempio di mapping coordinate Usa Bing Maps. Tenere presente i seguenti elementi nel codice:

- La chiamata a `Assets.AddScript` nella parte superiore delle due pagine di mapping. Questo metodo aggiunge un riferimento al *jquery 1.6.2.min.js* file incluso con il **Starter Site** modello e che è richiesta dalla `Maps` classe.
- La chiamata al `Assets.GetScripts` metodo nel file di layout. Questo metodo esegue il rendering di `<script>` tag le due pagine di mapping.
- La chiamata al `@Maps.GetGoogleHtml` e `@Maps.GetBingHtml` metodi nelle pagine di mapping. Per eseguire il mapping di un indirizzo, è necessario passare una stringa di indirizzo. Le coordinate della mappa, è necessario passare longitudine e latitudine coordinate. Per il motore di Bing mappe, è necessario passare anche una chiave (che è ottenere gratuitamente iscrivendosi al [sito agli sviluppatori di mappe di Bing](https://www.microsoft.com/maps/developers/web.aspx)). I metodi per i motori di mappa funzionano in modo analogo (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Per creare pagine di mapping:

1. Creare un sito Web basato sul **Starter Site** modello.
2. Creare un file denominato *MapAddress.cshtml* nella radice del sito. Questa pagina genererà una mappa in base all'indirizzo passato a esso.
3. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Creare un file denominato  *\_MapLayout.cshtml* nella radice del sito. Questa pagina sarà la pagina di layout per le due pagine di mapping.
5. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Creare un file denominato *MapCoordinates.cshtml* nella radice del sito. Questa pagina genererà una mappa basata su un set di coordinate che si passa a esso.
7. Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Il test delle pagine di mapping:

1. Eseguire la pagina *MapAddress.cshtml* file.
2. Immettere una stringa di indirizzo completo, incluso un indirizzo stradale, stato o provincia e codice postale e quindi scegliere il **Map It** pulsante. La pagina esegue il rendering di una mappa di mappe di Google: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Trovare un set di coordinate di latitudine e longitudine per un percorso specifico.
4. Eseguire la pagina *MapCoordinates.cshtml*. Immettere le coordinate e quindi scegliere il **Map It** pulsante. La pagina esegue il rendering di una mappa di Bing mappe: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Esecuzione di applicazioni Web Pages Side-by-Side

Pagine Web 2 aggiunge il possibilità di eseguire applicazioni side-by. Ciò consente di continuare a eseguire le applicazioni 1 pagine Web, creare nuove applicazioni Web Pages 2 ed eseguire tutti gli elementi nello stesso computer.

Di seguito sono illustrati alcuni aspetti da ricordare quando si installa la versione Beta 2 di pagine Web con WebMatrix:

- Per impostazione predefinita, le applicazioni esistenti di pagine Web verranno eseguita come applicazioni versione 2 nel computer in uso. (Gli assembly per la versione 2 vengono installati nella Global Assembly Cache e verranno utilizzati automaticamente).
- Se si desidera eseguire un sito di Web Pages versione 1 (anziché il valore predefinito, come illustrato al punto precedente), è possibile configurare il sito a tale scopo. Se il sito non ha ancora un *Web. config* file nella radice del sito, crearne uno nuovo e copiarvi il seguente codice XML, sovrascrivendo il contenuto esistente. Se il sito contiene già un *Web. config* file, aggiungere un `<appSettings>` elemento simile a quello riportato di seguito per il `<configuration>` sezione.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
'-Se non si specifica una versione di *Web. config* file, un sito viene distribuito come un sito di versione 2. (Vengono copiati gli assembly della versione 2 di *bin* cartella nel sito distribuito.)
- Nuove applicazioni create utilizzando i modelli di sito nella versione Web Matrix Beta 2 includono gli assembly della versione 2 pagine Web del sito *bin* cartella.

In generale, è sempre possibile controllare quale versione delle pagine Web da usare con il sito usando NuGet per installare gli assembly appropriati al sito *bin* cartella. Per trovare i pacchetti, visitare [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Il rendering di pagine per i dispositivi mobili

Pagine Web 2 consente di creare visualizzazioni personalizzate per il rendering del contenuto su dispositivi mobili o altri dispositivi.

Il `System.Web.WebPages` spazio dei nomi contiene le classi seguenti che consentono di lavorare con le modalità di visualizzazione: `DefaultDisplayMode`, `DisplayInfo`, e `DisplayModes`. È possibile utilizzare direttamente queste classi e scrivere codice che esegue il rendering nell'output di destra per dispositivi specifici.

In alternativa, è possibile creare pagine specifiche di dispositivo utilizzando un modello di denominazione dei file simile al seguente: *FileName.* *Mobile**. cshtml*. Ad esempio, è possibile creare due versioni di una pagina, uno denominato *MyFile.cshtml* e uno denominato *MyFile.Mobile.cshtml*. In fase di esecuzione, quando un dispositivo mobile richiede *MyFile.cshtml*, pagine Web viene eseguito il rendering del contenuto dalla *MyFile.Mobile.cshtml*. In caso contrario, *MyFile.cshtml* viene eseguito il rendering.

Nell'esempio seguente viene illustrato come attivare il rendering di dispositivi mobili mediante l'aggiunta di una pagina contenuto per i dispositivi mobili. *Page1.cshtml* contiene contenuto più di una barra laterale di navigazione. *Page1.Mobile.cshtml* contiene lo stesso contenuto, ma omette l'intestazione laterale.

Per compilare ed eseguire l'esempio di codice:

1. In un sito di pagine Web, creare un file denominato *Page1.cshtml* e copiare il *Page1.cshtml* contenuto al suo interno dall'esempio.
2. Creare un file denominato *Page1.Mobile.cshtml* e copiare il *Page1.Mobile.cshtml* contenuto al suo interno dall'esempio. Si noti che la versione per dispositivi mobili della pagina omette la sezione di spostamento per una migliore riproduzione su uno schermo piccolo.
3. Eseguire un browser per desktop e passare a *Page1.cshtml*.
4. Eseguire un browser per dispositivi mobili (o un emulatore di dispositivo mobile) e passare a *Page1.cshtml*. Si noti che questa volta pagine Web esegue il rendering di versione per dispositivi mobili della pagina. 

    > [!NOTE]
    > Per testare pagine per dispositivi mobili, è possibile utilizzare un simulatore di dispositivi mobili che viene eseguito in un computer desktop. Questo strumento consente di testare le pagine web come appaiono nei dispositivi mobili (ovvero, in genere con molto piccola Visualizza area). È un esempio di un simulatore di [componente aggiuntivo di selezione dell'agente utente](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) per Mozilla Firefox, che consente di emulare diversi browser per dispositivi mobili da una versione desktop di Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* sottoposto a rendering in un browser desktop:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* visualizzato in una vista di simulatore Apple iPhone nel browser Firefox. Anche se la richiesta è su *Page1.cshtml*, l'applicazione esegue il rendering *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages risorse 1

> [!NOTE]
> La maggior parte delle pagine Web 1 programmazione e risorse di API anche per le pagine Web 2.

- [Introduzione alla programmazione di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Risorse di WebMatrix

- [WebMatrix 2 Novità](http://webmatrix.com/next)
- [Sito di Microsoft WebMatrix](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Avviare lo sviluppo Web con Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(include un'applicazione di pagine Web di esempio completi dei)
