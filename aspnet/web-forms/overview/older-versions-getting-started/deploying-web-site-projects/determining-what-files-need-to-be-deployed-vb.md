---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: Determinazione dei file da distribuire (VB) | Microsoft Docs
author: rick-anderson
description: Quali file devono essere distribuiti dall'ambiente di sviluppo all'ambiente di produzione dipende in parte che ci sia stato generato l'applicazione ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: 89b45bff705996b799242ad55f156083f310af97
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380389"
---
<a name="determining-what-files-need-to-be-deployed-vb"></a>Determinazione dei file da distribuire (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> Quali file devono essere distribuiti dall'ambiente di sviluppo all'ambiente di produzione dipende in parte che l'applicazione ASP.NET sia stato generato usando il modello di sito Web o applicazione Web. Altre informazioni su questi due modelli di progetto e l'influenza di distribuzione nel modello di progetto.


## <a name="introduction"></a>Introduzione

Distribuzione di un'applicazione web ASP.NET vengono copiati i file relativi ad ASP.NET dall'ambiente di sviluppo all'ambiente di produzione. I file relativi ad ASP.NET includono markup della pagina web ASP.NET e i file di supporto di codice e client e lato server. File di supporto sul lato client sono tali file a cui fanno riferimento le pagine web e inviati direttamente al browser - immagini, file CSS e file JavaScript, ad esempio. File di supporto sul lato server comprendono quelli che vengono utilizzati per elaborare una richiesta sul lato server. Questo include i file di configurazione, servizi web, file di classe, dataset tipizzati e LINQ per i file SQL, tra gli altri.

In generale, tutti i file di supporto sul lato client devono essere copiati dall'ambiente di sviluppo all'ambiente di produzione, ma copiati i file di supporto sul lato server varia a seconda se si esegue in modo esplicito la compilazione di codice lato server in un assembly (un `.dll` file) o se si verificano questi assembly generato automaticamente. Questa esercitazione illustra quali file devono essere distribuiti quando si compila in modo esplicito il codice in un assembly e che questo passaggio di compilazione vengono eseguiti automaticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilazione esplicita rispetto a compilazione automatica

Pagine web ASP.NET sono suddivise in dichiarativo markup e codice sorgente. La parte di markup dichiarativo include HTML, controlli Web e sintassi di associazione dati; la parte di codice contiene i gestori di eventi scritti in codice Visual Basic o c#. Le parti di codice e markup in genere vengono suddivisi in file diversi: `WebPage.aspx` contiene il markup dichiarativo mentre `WebPage.aspx.vb` ospita il codice.

Si consideri una pagina ASP.NET denominata `Clock.aspx` che contiene un controllo etichetta con la proprietà Text è impostata per la data e ora correnti quando la pagina viene caricata. La parte di markup dichiarativo (in `Clock.aspx`) contiene il markup per un controllo Web Label - `<asp:Label runat="server" id="TimeLabel" />` - durante la parte di codice (in `Clock.aspx.vb`) avrebbe un `Page_Load` gestore dell'evento con il codice seguente:

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

Affinché il motore di ASP.NET servire una richiesta per questa pagina, parte di codice della pagina (la *`WebPage`* `.aspx.vb` file) prima di tutto deve essere compilato. Questa compilazione può verificarsi in modo esplicito o automaticamente.

Se la compilazione viene eseguita in modo esplicito il codice sorgente dell'intera applicazione viene compilato in uno o più assembly (`.dll` file) all'interno dell'applicazione `Bin` directory. Se la compilazione viene eseguita automaticamente quindi risultante autogenerato assembly è, per impostazione predefinita, in modalità i `Temporary ASP.NET Files` cartella, che è reperibile in `%WINDOWS%\Microsoft.NET\Framework\<version>`, anche se questo percorso è configurabile tramite il [ &lt; compilation&gt; elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) in `Web.config`. Con la compilazione esplicita è necessario adottare alcune misure per compilare il codice dell'applicazione ASP.NET in un assembly e questo passaggio si verifica prima della distribuzione. Con la compilazione automatica del processo di compilazione si verifica sul server web quando si accede prima di tutto la risorsa.

Indipendentemente dal fatto che il modello di compilazione si utilizza, la parte di markup di tutte le pagine ASP.NET (il `WebPage.aspx` file) devono essere copiati nell'ambiente di produzione. Con la compilazione esplicita è necessario copiare l'assembly nel `Bin` cartella, ma non è necessario copiare le parti di codice alla pagina ASP.NET (il `WebPage.aspx.vb` file). Con la compilazione automatica è necessario copiare i file di parte del codice in modo che il codice è presente e può essere compilato automaticamente quando si visita la pagina. La parte di markup di ogni pagina web ASP.NET include un `@Page` direttiva con gli attributi che indicano se il codice della pagina associata è stata già esplicitamente compilato o se deve essere compilato automaticamente. Di conseguenza, l'ambiente di produzione è possibile lavorare direttamente con dei modelli compilazione e non è necessario applicare le impostazioni di configurazione speciale per indicare che viene usata la compilazione automatica o esplicita.

Tabella 1 si riassumono i file diversi da distribuire quando si usa la compilazione esplicita rispetto a compilazione automatica. Tenere presente che indipendentemente dalla compilazione modello utilizzato è sempre consigliabile distribuire gli assembly nel `Bin` cartella, se tale cartella esiste. Il `Bin` cartella contiene gli assembly specifici dell'applicazione web, che includono il codice sorgente compilati quando si usa il modello di compilazione esplicita. Il `Bin` directory contiene anche gli assembly da altri progetti e gli assembly di terze parti o open source che si stiano usando e questi elementi dovranno essere nel server di produzione. Pertanto, come regola empirica generale, copiare il `Bin` cartella fino a quando la distribuzione di produzione. (Se si usa il modello di compilazione automatica e non si usa qualsiasi assembly esterni, non sarà necessario un `Bin` directory - OK!)

| **Modello di compilazione** | **Distribuire i File di Markup parte?** | **Distribuire i File di codice sorgente?** | **Distribuire gli assembly nella `Bin` Directory?** |
| --- | --- | --- | --- |
| Compilazione esplicita | Yes | No | Yes |
| Compilazione automatica | Yes | Yes | Sì (se presente) |

**Tabella 1: I file che si distribuisce dipende dal modello di compilazione utilizzato.**

## <a name="taking-a-trip-down-memory-lane"></a>Richiede un viaggio

Viene usato l'approccio di compilazione dipende in parte, come viene gestito l'applicazione ASP.NET in Visual Studio. Poiché. Inizio del NET dell'anno 2000 sono state apportate quattro versioni diverse di Visual Studio - Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 e Visual Studio 2008. Visual Studio .NET 2002 e 2003 gestite le applicazioni ASP.NET utilizzando il *progetto di applicazione Web* modello. Le funzionalità principali del modello di progetto di applicazione Web sono:

- I file che struttura del progetto sono definiti in un unico file di progetto. Tutti i file non è definiti nel file di progetto non sono considerati parte dell'applicazione web da Visual Studio.
- Usa la compilazione esplicita. Compilare il progetto compila i file di codice all'interno del progetto in un singolo assembly in cui è posizionato il `Bin` cartella.

Quando Microsoft ha rilasciato Visual Studio 2005 sono eliminazione del supporto per il modello di progetto di applicazione Web e sostituito con il modello di progetto di sito Web. Il modello di progetto di sito Web differenziate stesso mediante la *progetto di applicazione Web* modello nei modi seguenti:

- Anziché un singolo file del progetto che illustra in dettaglio i file del progetto, viene invece usato il file system. In breve, tutti i file all'interno della cartella dell'applicazione web (o nelle sottocartelle) sono considerati parte del progetto.
- Compila un progetto in Visual Studio non crea un assembly nel `Bin` directory. Al contrario, la creazione di un progetto sito Web segnala eventuali errori in fase di compilazione.
- Supporto per la compilazione automatica. Progetti di siti Web vengono in genere distribuiti copiando il markup e codice sorgente nell'ambiente di produzione, anche se il codice può essere precompilata (compilazione esplicita).

Microsoft riattivata il modello di progetto di applicazione Web quando rilasciato Visual Studio 2005 Service Pack 1. Tuttavia, Visual Web Developer continuato a supportare solo il modello di progetto di sito Web. La buona notizia è che questa limitazione è stata rilasciata con Visual Web Developer 2008 Service Pack 1. Oggi è possibile creare applicazioni ASP.NET in Visual Studio (e Visual Web Developer) usando il modello di progetto di applicazione Web o il modello di progetto di sito Web. Entrambi i modelli hanno vantaggi e svantaggi. Fare riferimento a [Introduzione a Web Application Projects: confronto tra progetti di siti Web e Web Application Projects](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) per un confronto tra i due modelli e per decidere quale modello di progetto è adatta alla situazione specifica.

## <a name="exploring-the-sample-web-application"></a>Esplorare l'applicazione Web di esempio

Il download per questa esercitazione include un'applicazione ASP.NET denominata le recensioni dei libri. Il sito Web in grado di simulare un sito Web hobby potrebbe creare un utente di condividere il libro esamina con la community online. Questa applicazione web ASP.NET è molto semplice e costituita dalle risorse seguenti:

- `Web.config`, il file di configurazione dell'applicazione.
- Una pagina master (`Site.master`).
- Sette diverse pagine ASP.NET:

    - ~/`Default.aspx` -Home page del sito.
    - ~/`About.aspx` -una pagina "Sul sito".
    - ~/`Fiction/Default.aspx` -una pagina che elenca i libri fittizio che sono stati verificati.

        - ~/`Fiction/Blaze.aspx` -una revisione del romanzo di Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx` -una pagina che elenca la documentazione di tecnologia che sono stati verificati.

        - ~/`Tech/CYOW.aspx` -un riesame *creare il sito Web proprio*.
        - ~/`Tech/TYASP35.aspx` -un riesame *insegnare manualmente ASP.NET 3.5 in 24 ore*.
- Tre diversi file CSS nel `Styles` cartella.
- Quattro file di immagine, una basata su ASP.NET logo e immagini di copertura di libri esaminati tre - tutti i che si trova nel `Images` cartella.
- Oggetto `Web.sitemap` file, che definisce la mappa del sito e consente di visualizzare menu di scelta nel `Default.aspx` pagine nella directory radice e `Fiction` e `Tech` cartelle.
- Un file di classe denominato `BasePage.vb` che consente di definire una base `Page` classe. Questa classe estende la funzionalità dei `Page` classe mediante l'impostazione automatica di `Title` proprietà in base alla posizione della pagina della mappa del sito. In pratica, qualsiasi classe code-behind ASP.NET che estende `BasePage` (invece di `System.Web.UI.Page`) avrà il relativo titolo impostato su un valore in base alla relativa posizione nella mappa del sito. Ad esempio, quando si visualizzano i ~ /`Tech/CYOW.aspx` pagina, il titolo è impostato su "Home: tecnologia: creare il proprio sito Web".

Figura 1 mostra una cattura di schermata del sito Web le recensioni dei libri quando viene visualizzato tramite un browser. Di seguito viene visualizzata la pagina ~ / Tech/TYASP35.aspx, che esamina il libro *insegnare manualmente ASP.NET 3.5 in 24 ore*. Il percorso di navigazione che si estende nella parte superiore della pagina di menu nella colonna sinistra si basano nella struttura della mappa del sito definita `Web.sitemap`. L'immagine nell'angolo superiore sinistro è uno della copertura della Rubrica immagini che si trovano nel `Images` cartella. Aspetto del sito Web vengono definite tramite le regole di foglio di stile venga specificate dai file CSS nella `Styles` cartella, mentre il layout delle pagine globale viene definito nella pagina master, `Site.master`.


[![Il sito Web del libro esamina offre le revisioni su un'ampia gamma di titoli](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**Figura 1**: sito Web esamina il libro offre le revisioni su un'ampia gamma di titoli ([fare clic per visualizzare l'immagine con dimensioni normali](determining-what-files-need-to-be-deployed-vb/_static/image3.png))


Questa applicazione non usa un database; ogni verifica viene implementato come una pagina web separata nell'applicazione. In dettaglio la distribuzione di un'applicazione web che non dispone di un database in questa esercitazione (e le esercitazioni più avanti). Tuttavia, in un'esercitazione futura è miglioreranno questa applicazione archiviare recensioni, commenti dei lettori e altre informazioni contenute in un database e vengono illustrati quali passaggi devono essere eseguite per distribuire correttamente un'applicazione web basata sui dati.

> [!NOTE]
> Queste esercitazioni riguardano l'hosting di applicazioni ASP.NET con un provider di hosting web e che non esplori ausiliari argomenti quali ASP. Sistema della mappa del sito di NET o tramite una classe base di pagina. Per altre informazioni su queste tecnologie e per altre informazioni su altri argomenti trattati nel corso dell'esercitazione, vedere la sezione di letture di approfondimento alla fine di ogni esercitazione.


Download di questa esercitazione ha due copie dell'applicazione web, ciascuno implementato come un diverso tipo di progetto di Visual Studio: BookReviewsWAP, un progetto di applicazione Web e BookReviewsWSP, un progetto sito Web. Entrambi i progetti creati con Visual Web Developer 2008 SP1 e utilizzano ASP.NET 3.5 SP1. Per usare questi progetti per iniziare, decomprimere il contenuto sul desktop. Per aprire il progetto di applicazione Web (BookReviewsWAP), passare al `BookReviewsWAP` cartella e fare doppio clic sul file di soluzione, `BookReviewsWAP.sln`. Per aprire il progetto di sito Web (BookReviewsWSP), avviare Visual Studio e quindi, dal menu File, scegliere l'opzione Apri sito Web, selezionare il `BookReviewsWSP` cartella sul Desktop, fare clic su OK.


Le due sezioni rimanenti in questa esercitazione esaminare i file che sarà necessario copiare nell'ambiente di produzione quando si distribuisce l'applicazione. Le due esercitazioni - [ *distribuzione del sito tramite FTP* ](deploying-your-site-using-an-ftp-client-vb.md) e [ *distribuzione del sito tramite Visual Studio* ](deploying-your-site-using-visual-studio-vb.md) -mostrano diverse modalità per copiare questi file in un provider di hosting web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinare i file da distribuire per il progetto di applicazione Web

Il modello di progetto di applicazione Web Usa la compilazione esplicita: codice sorgente del progetto viene compilato in un unico assembly ogni volta che si compila l'applicazione. Questa compilazione include file code-behind alla pagina ASP.NET (~ /`Default.aspx.vb`, ~ /`About.aspx.vb`e così via), nonché il `BasePage.vb` classe. L'assembly risulta è denominato `BookReviewsWAP.dll` e all'interno dell'applicazione `Bin` directory.

Figura 2 mostra i file che costituiscono il progetto di applicazione Web di revisioni del libro.


[![Esplora soluzioni Elenca i file che costituiscono il progetto di applicazione Web.](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**Figura 2**: Esplora soluzioni Elenca i file che costituiscono il progetto di applicazione Web


> [!NOTE]
> Come illustrato nella figura 2, file code-behind ASP.NET alla pagina non vengono visualizzati in Esplora soluzioni per un progetto di applicazione Web di Visual Basic. Per visualizzare la classe code-behind per una pagina, fare doppio clic nella pagina in Esplora soluzioni e scegliere Visualizza codice.


Per distribuire un'applicazione ASP.NET sviluppata utilizzando l'avvio di modello di progetto di applicazione Web tramite la compilazione dell'applicazione in modo da compilare in modo esplicito il codice sorgente più recente in un assembly. Successivamente, copiare i file seguenti nell'ambiente di produzione:

- I file che contengono il markup dichiarativo per ogni ASP.NET pagina, ad esempio ~ /`Default.aspx`, ~ /`About.aspx`e così via. Inoltre, copia di markup dichiarativo per tutte le pagine master e controlli utente.
- Gli assembly (`.dll` file) nel `Bin` cartella. Non è necessaria copiare i file di database di programma (`.pdb`) o file XML può risultare nel `Bin` directory.

Non è necessaria copiare i file del codice sorgente alla pagina ASP.NET nell'ambiente di produzione, né è necessario copiare il `BasePage.vb` file di classe.

> [!NOTE]
> Come illustrato nella figura 2, il `BasePage` classe viene implementata come un file di classe nel progetto, inserito nella cartella denominata `HelperClasses`. Quando il progetto viene compilato il codice nel `BasePage.vb` file viene compilato con classi ASP.NET alla pagina code-behind in un singolo assembly, `BookReviewsWAP.dll`. ASP.NET dispone di una cartella speciale denominata `App_Code` che è progettato per contenere i file di classe per i progetti di sito Web. Il codice nel `App_Code` cartella viene compilato automaticamente e pertanto non devono essere usato con Web Application Projects. In alternativa, è necessario inserire i file di classe dell'applicazione in una normale cartella denominata `HelperClasses`, o `Classes`, o un approccio simile. In alternativa, è possibile inserire i file di classe in un progetto libreria di classi separato.


Oltre a copiare i file di markup relativi ad ASP.NET e l'assembly nel `Bin` cartella, è inoltre necessario copiare i file di supporto sul lato client - immagini e file CSS -, altri file del supporto sul lato server, nonché `Web.config` e `Web.sitemap`. Questi client e lato server file per il supporto da copiare nell'ambiente di produzione sia che si utilizzi la compilazione automatica o esplicita.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinare i file da distribuire per i file di progetto sito Web

Il modello di progetto di sito Web supporta la compilazione automatica, una funzionalità non disponibile quando si usa il modello di progetto di applicazione Web. Con la compilazione esplicita è necessario compilare codice sorgente del progetto in un assembly e copiare l'assembly nell'ambiente di produzione. D'altra parte, con la compilazione automatica è sufficiente copiare il codice sorgente nell'ambiente di produzione e viene compilato dal runtime su richiesta in base alle esigenze.

L'opzione di menu di compilazione in Visual Studio è presente in entrambi i progetti applicazione Web e progetti di siti Web. Creazione di un Web Application Projects compila il codice sorgente del progetto in un singolo assembly che si trova nel `Bin` directory; creazione di un progetto sito Web verifica la presenza di eventuali errori in fase di compilazione, ma non crea tutti gli assembly. Per distribuire un'applicazione ASP.NET sviluppata usando il modello di progetto di sito Web tutto che è necessario eseguire è copia i file appropriati all'ambiente di produzione, ma è consigliabile iniziare compilando il progetto per assicurarsi che non siano presenti errori in fase di compilazione.

Figura 3 mostra i file che costituiscono il progetto di sito Web di revisioni del libro.


[![Esplora soluzioni Elenca i file che costituiscono il progetto di sito Web.](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**Figura 3**: Esplora soluzioni Elenca i file che costituiscono il progetto di sito Web


Distribuzione di un progetto sito Web richiede la copia di tutti i file relativi ad ASP.NET nell'ambiente di produzione, che include le pagine di markup per le pagine ASP.NET, pagine master e controlli utente, insieme ai file di codice. È inoltre necessario copiare i backup di tutti i file di classe, ad esempio `BasePage.vb`. Si noti che il `BasePage.vb` file si trova nel `App_Code` cartella, ovvero una speciale cartella ASP.NET usata in progetti di siti Web per i file di classe. La speciale cartella deve essere creato in fase di produzione, come i file di classe nel `App_Code` cartella nell'ambiente di sviluppo deve essere copiato il `App_Code` cartella sulla produzione.

Oltre a copiare i file di codice ASP.NET markup e di origine, è inoltre necessario copiare i file di supporto sul lato client - immagini e file CSS -, altri file del supporto sul lato server, nonché `Web.config` e `Web.sitemap`.

> [!NOTE]
> Progetti di siti Web è anche possibile usare compilazione esplicita. Un'esercitazione futura verrà esaminato da compilare in modo esplicito un progetto sito Web.


## <a name="summary"></a>Riepilogo

Distribuzione di un'applicazione ASP.NET vengono copiati i file necessari dall'ambiente di sviluppo all'ambiente di produzione. Lo specifico set di file che devono essere sincronizzati con probabilità se il codice dell'applicazione ASP.NET è in modo esplicito o automaticamente compilato. La strategia di compilazione utilizzata è influenzata dal fatto che Visual Studio è configurato per gestire l'applicazione ASP.NET usando il modello di progetto di applicazione Web o il modello di progetto di sito Web.

Il modello di progetto di applicazione Web Usa la compilazione esplicita e compila il codice del progetto in un singolo assembly nel `Bin` cartella. Quando si distribuisce l'applicazione, la parte di markup delle pagine ASP.NET e il contenuto del `Bin` cartella debba essere inserita nell'ambiente di produzione, il codice sorgente dell'applicazione - i file di codice e code-behind, ad esempio le classi - non è necessario da copiare nell'ambiente di produzione.

Il modello di progetto di sito Web Usa la compilazione automatica per impostazione predefinita, sebbene sia possibile compilare in modo esplicito un progetto di sito Web, come vedremo in futuro le esercitazioni. La distribuzione di un'applicazione ASP.NET che usa la compilazione automatica richiede che la parte di markup *e* codice sorgente deve essere copiato nell'ambiente di produzione. Il codice viene compilato automaticamente nell'ambiente di produzione quando richiesto per la prima volta.

Ora che sono state esaminate quali file devono essere sincronizzate tra ambienti di sviluppo e produzione siamo pronti distribuire l'applicazione le recensioni dei libri in un provider di hosting web.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Cenni preliminari sulla compilazione di ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controlli utente ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Esame di ASP. Navigazione nel sito di rete](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introduzione a Web Application Projects](https://msdn.microsoft.com/library/aa730880.aspx)
- [Esercitazioni di pagina master](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Condivisione del codice tra le pagine](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Usando una classe di Base personalizzata per le classi di ASP.NET alla pagina Code-Behind](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema di progetto sito Web di Visual Studio 2005: cosa si tratta e perché abbiamo fatto nello script si?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Procedura dettagliata: Conversione di un progetto sito Web a un progetto di applicazione Web in Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Precedente](asp-net-hosting-options-vb.md)
> [Successivo](deploying-your-site-using-an-ftp-client-vb.md)
