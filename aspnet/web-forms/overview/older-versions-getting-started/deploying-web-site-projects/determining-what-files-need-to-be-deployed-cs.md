---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Determinare quali file devono essere distribuiti (c#) | Documenti Microsoft
author: rick-anderson
description: Quali file devono essere distribuiti dall'ambiente di sviluppo all'ambiente di produzione dipende in parte se ci è stato compilato l'applicazione ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: ff5f1d7d156efa12d97382db56211a07c43178fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Determinare quali file devono essere distribuiti (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Quali file devono essere distribuiti dall'ambiente di sviluppo all'ambiente di produzione dipende in parte se l'applicazione ASP.NET è stato compilato utilizzando il modello di sito Web o applicazione Web. Ulteriori informazioni su questi due modelli di progetto e l'impatto della distribuzione nel modello di progetto.


## <a name="introduction"></a>Introduzione

Distribuzione di un'applicazione web ASP.NET comporta la copia dei file relativi ad ASP.NET dall'ambiente di sviluppo all'ambiente di produzione. I file di ASP.NET includono markup della pagina web ASP.NET e i file di supporto di codice e sul lato client e server. File di supporto sul lato client sono quelli a cui fa riferimento alle pagine web e inviato direttamente al browser, immagini, file CSS e JavaScript file, ad esempio. File di supporto sul lato server comprendono quelli che vengono utilizzati per elaborare una richiesta sul lato server. Questo include i file di configurazione, servizi web, file di classe, di dataset tipizzati e LINQ per i file SQL, tra gli altri.

In generale, tutti i file di supporto sul lato client devono essere copiati dall'ambiente di sviluppo all'ambiente di produzione, ma i file di supporto sul lato server vengano copiati varia a seconda se si esegue in modo esplicito la compilazione del codice server in un assembly (un `.dll` file) o se si verificano questi assembly generato automaticamente. In questa esercitazione illustra quali file devono essere distribuiti quando in modo esplicito la compilazione del codice in un assembly e che questa operazione di compilazione si verificano automaticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilazione esplicita e compilazione automatica

Pagine web ASP.NET sono suddivise in dichiarativo markup e il codice sorgente. La parte di markup dichiarativo include HTML, controlli Web e la sintassi di associazione dati. la parte di codice contiene gestori eventi scritti in codice Visual Basic o c#. Le parti di codice e markup in genere sono suddivise in diversi file: `WebPage.aspx` contiene markup dichiarativo durante `WebPage.aspx.cs` contiene il codice.

Si consideri una pagina ASP.NET denominata Clock.aspx contenente un controllo etichetta cui testo è impostata la proprietà per la data e ora correnti quando il caricamento della pagina. La parte di codice dichiarativo (in `Clock.aspx`) contiene il markup per un controllo etichetta Web -`<asp:Label runat="server" id="TimeLabel" />` : durante la parte di codice (in `Clock.aspx.cs`) avrebbe un `Page_Load` gestore dell'evento con il codice seguente:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Affinché il motore ASP.NET soddisfare una richiesta per questa pagina, la parte di codice della pagina (il `WebPage.aspx.cs` file) deve prima essere compilato. Questa compilazione può verificarsi in modo esplicito o automaticamente.

Se la compilazione viene eseguita in modo esplicito il codice sorgente dell'applicazione intera viene compilato in uno o più assembly (`.dll` file) all'interno dell'applicazione `Bin` directory. Se la compilazione viene eseguita automaticamente, quindi il valore risultante autogenerati assembly è per impostazione predefinita, in modalità di `Temporary ASP.NET` cartella dei file, che è reperibile in `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;versione&gt;*, Sebbene questo percorso è configurabile tramite il [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) in `Web.config`. Con la compilazione esplicita è necessario adottare alcune misure per compilare il codice dell'applicazione ASP.NET in un assembly e questo passaggio si verifica prima della distribuzione. Con la compilazione automatica sul server web prima di accesso alla risorsa si verifica il processo di compilazione.

Indipendentemente dal quale modello di compilazione si utilizza, la parte di markup di tutte le pagine ASP.NET (il `WebPage.aspx` file) devono essere copiati nell'ambiente di produzione. Con la compilazione esplicita è necessario copiare l'assembly nel `Bin` cartella, ma non è necessario copiare le parti di codice alla pagina ASP.NET (il `WebPage.aspx.cs` file). Con la compilazione automatica è necessario copiare i file di parte di codice in modo che il codice è presente e può essere compilato automaticamente quando si visita la pagina. La parte di markup della pagina web ASP.NET include un `@Page` direttiva con attributi che indicano se il codice della pagina associato è stato compilato già in modo esplicito o se deve essere compilato automaticamente. Di conseguenza, l'ambiente di produzione è possibile lavorare direttamente con un modello di compilazione e non è necessario applicare le impostazioni di configurazione speciale per indicare che la compilazione automatica o esplicita viene utilizzata.

Tabella 1 sono riepilogati i diversi file da distribuire quando si utilizza la compilazione esplicita e compilazione automatica. Si noti che indipendentemente dalla compilazione modello utilizzato è consigliabile distribuire sempre gli assembly di `Bin` cartella, se tale cartella è presente. Il `Bin` cartella contiene gli assembly specifici dell'applicazione web, che includono il codice sorgente compilati quando si utilizza il modello di compilazione esplicita. Il `Bin` directory contiene anche gli assembly da altri progetti e gli assembly di terze parti o open source che si stia utilizzando e questi devono trovarsi sul server di produzione. Pertanto, per una regola empirica generale, copiare il `Bin` cartella fino a quando la distribuzione di produzione. (Se si utilizza il modello di compilazione automatica e non si utilizza qualsiasi assembly esterni, si avrà un `Bin` directory - OK!)

| **Modello di compilazione** | **Distribuire il File di parte Markup?** | **Distribuire i File del codice sorgente?** | **Distribuire gli assembly nella `Bin` Directory?** |
| --- | --- | --- | --- |
| Compilazione esplicita | Yes | No | Yes |
| Compilazione automatica | Yes | Yes | Sì (se presente) |

**Tabella 1:** i file che si distribuisce dipende dal modello di compilazione utilizzato.

## <a name="taking-a-trip-down-memory-lane"></a>Richiede un Trip corsia memoria

Viene usato l'approccio di compilazione dipende in parte, modalità di gestione dell'applicazione ASP.NET in Visual Studio. Dopo. Inizio della rete dell'anno 2000 sono state quattro versioni diverse di Visual Studio - Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 e Visual Studio 2008. Visual Studio .NET 2002 e 2003 gestiti applicazioni ASP.NET utilizzando il *modello di progetto di applicazione Web*. Le funzionalità principali del modello di progetto di applicazione Web sono:

- I file di definizione di struttura del progetto in un singolo file del progetto. Tutti i file non è definiti nel file di progetto non sono considerati parte dell'applicazione web da Visual Studio.
- Utilizza la compilazione esplicita. Compilazione del progetto, i file di codice all'interno del progetto viene compilato in un singolo assembly che viene inserito nella `Bin` cartella.

Quando Microsoft ha rilasciato Visual Studio 2005 vengono eliminati il supporto per il modello di progetto di applicazione Web e viene sostituito con il modello di progetto di sito Web. Il modello di progetto di sito Web si dal modello di progetto di applicazione Web nei modi seguenti:

- Invece di un singolo file del progetto che definiscono i file del progetto, il file system viene utilizzato. In breve, tutti i file all'interno della cartella dell'applicazione web (o le sottocartelle) sono considerati parte del progetto.
- Compilazione di un progetto in Visual Studio non crea un assembly di `Bin` directory. Al contrario, la creazione di un progetto di sito Web report errori in fase di compilazione.
- Supporto per la compilazione automatica. Progetti di siti Web sono in genere distribuiti copiando il markup e il codice sorgente nell'ambiente di produzione, anche se il codice può essere precompilata (compilazione esplicita).

Quando rilasciato Visual Studio 2005 Service Pack 1, Microsoft ripristinata nel modello di progetto applicazione Web. Tuttavia, Visual Web Developer costantemente supportano solo il modello di progetto di sito Web. Buone notizie sono che questa limitazione è stata eliminata con Visual Web Developer 2008 Service Pack 1. Oggi è possibile creare applicazioni ASP.NET in Visual Studio e Visual Web Developer, utilizzando il modello di progetto di applicazione Web o il modello di progetto di sito Web. Entrambi i modelli sono i vantaggi e svantaggi. Fare riferimento a [Introduction to Web Application Projects: confronto tra progetti di siti Web e progetti di applicazione Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) per un confronto dei due modelli e per consentire di decidere quale modello di progetto è adatto alla propria situazione.

## <a name="exploring-the-sample-web-application"></a>Esplorare l'applicazione Web di esempio

Il download per questa esercitazione include un'applicazione ASP.NET denominata recensioni. Il sito Web in grado di simulare un utente potrebbe creare un sito Web di hobby per condividere i loro libro esamina con la community online. L'applicazione web ASP.NET è molto semplice e include le risorse seguenti:

- `Web.config`, il file di configurazione dell'applicazione.
- Una pagina master (`Site.master`).
- Sette diverse pagine ASP.NET: 

    - ~`/Default.aspx`-Home page del sito.
    - ~`/About.aspx` -una pagina "Sul sito".
    - ~`/Fiction/Default.aspx` -elenco di libri fittizio che sono stati verificati. 

        - ~`/Fiction/Blaze.aspx` -una revisione del romanzo Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx` -una pagina che elenca i libri di tecnologia che sono stati verificati. 

        - ~/`Tech/CYOW.aspx`-una revisione del *creare il sito Web proprio*.
        - ~/`Tech/TYASP35.aspx` -una revisione del *insegna manualmente ASP.NET 3.5 nelle 24 ore*.
- Tre file CSS diversi nella cartella Styles.
- Quattro file di immagine - una tecnologia ASP.NET logo e immagini di copertura dei libri riviste tre - tutti all'interno di `Images` cartella.
- Oggetto `Web.sitemap` file, che definisce la mappa del sito e consente di visualizzare menu il `Default.aspx` pagine nella directory radice e `Fiction` e `Tech` cartelle.
- Un file di classe denominato `BasePage.cs` che definisce una base `Page` classe. Questa classe estende la funzionalità del `Page` classe mediante l'impostazione automatica di `Title` proprietà in base alla posizione della pagina nella mappa del sito. In pratica, qualsiasi classe code-behind ASP.NET che estende `BasePage` (anziché `System.Web.UI.Page`) avrà il titolo impostato su un valore in base alla posizione nella mappa del sito. Ad esempio, quando si visualizzano i ~ /`Tech/CYOW.aspx` pagina, il titolo è impostato su "Home: tecnologia: creare il proprio sito Web".

Figura 1 mostra una schermata del sito Web recensioni quando viene visualizzato tramite un browser. Di seguito viene visualizzata la pagina ~ /`Tech/TYASP35.aspx`, che esamina il libro *insegna manualmente ASP.NET 3.5 nelle 24 ore*. Che si estende nella parte superiore della pagina e il menu nella colonna sinistra della barra di navigazione sono basati sulla struttura della mappa del sito definita in `Web.sitemap`. L'immagine nell'angolo superiore sinistro è uno della copertura della Rubrica le immagini memorizzate nella `Images` cartella. Aspetto del sito Web sono definiti tramite pronunciate dai file CSS nella cartella Styles, mentre il layout di pagina globale viene definito nella pagina master, regole dei fogli di stile CSS `Site.master`.


[![Il sito Web esamina manuale offre le revisioni su un'ampia gamma di titoli](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Figura 1:** il sito Web esamina manuale offre le revisioni su un'ampia gamma di titoli ([fare clic per visualizzare l'immagine ingrandita](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Questa applicazione non utilizza un database. ogni revisione viene implementato come una pagina web separata nell'applicazione. Viene illustrata la distribuzione di un'applicazione web che non dispone di un database in questa esercitazione (e le esercitazioni più avanti). Tuttavia, in un'esercitazione in futura è contribuisce a migliorare questa applicazione per archiviare le revisioni, lettore commenti e altre informazioni all'interno di un database e verranno esplorati necessario passaggi da eseguire per distribuire correttamente un'applicazione web basate sui dati.

> [!NOTE]
> Queste esercitazioni concentrano sull'hosting di applicazioni ASP.NET con un provider di hosting web e non esplorare ausiliari argomenti come ASP. Sistema di mappa del sito di rete o utilizzando una base `Page` classe. Per ulteriori informazioni su queste tecnologie e per ulteriori informazioni su altri argomenti trattati nel corso dell'esercitazione, vedere la sezione di approfondimento alla fine di ogni esercitazione.


Download di questa esercitazione ha due copie dell'applicazione web, ciascuno implementato come un diverso tipo di progetto di Visual Studio: BookReviewsWAP, un progetto di applicazione Web e BookReviewsWSP, un progetto di sito Web. Entrambi i progetti creati con Visual Web Developer 2008 SP1 e utilizzano ASP.NET 3.5 SP1. Per l'utilizzo di questi progetti per iniziare, decomprimere il contenuto sul desktop. Per aprire il progetto di applicazione Web (BookReviewsWAP), passare alla cartella BookReviewsWAP e fare doppio clic sul file di soluzione `BookReviewsWAP.sln`. Per aprire il progetto di sito Web (BookReviewsWSP), avviare Visual Studio e quindi dal menu File, scegliere l'opzione Apri sito Web, individuare il `BookReviewsWSP` cartella sul Desktop, fare clic su OK.


Due sezioni rimanenti in questa esercitazione esaminerà i file che sarà necessario copiare nell'ambiente di produzione quando si distribuisce l'applicazione. Le due esercitazioni - *[la distribuzione del sito tramite FTP](deploying-your-site-using-an-ftp-client-cs.md)* e *[la distribuzione del sito tramite Visual Studio](deploying-your-site-using-visual-studio-cs.md)* -Mostra i diversi modi per copiare questi file in un provider di hosting web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinare i file da distribuire per il progetto di applicazione Web

Il modello di progetto di applicazione Web utilizza la compilazione esplicita - codice sorgente del progetto viene compilato in un singolo assembly ogni volta che si compila l'applicazione. Questa compilazione include file code-behind alla pagina ASP.NET (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`e così via), nonché la `BasePage.cs` classe. L'assembly risultante è denominato BookReviewsWAP.dll e all'interno dell'applicazione `Bin` directory.

Figura 2 mostra i file che costituiscono il progetto di applicazione Web revisioni Rubrica.


[![Esplora soluzioni sono elencati i file che costituiscono il progetto di applicazione Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Figura 2**: Esplora soluzioni sono elencati i file che costituiscono il progetto di applicazione Web


Per distribuire un'applicazione ASP.NET sviluppato utilizzando l'avvio di modello di progetto di applicazione Web per la compilazione dell'applicazione in modo da compilare in modo esplicito il codice sorgente più recente in un assembly. Successivamente, copiare i file seguenti nell'ambiente di produzione:

- I file che contengono il markup dichiarativo per ogni ASP.NET pagina, ad esempio ~ /`Default.aspx`, ~ /`About.aspx`e così via. Inoltre, copia di backup il markup dichiarativo per le pagine master e nei controlli utente.
- Gli assembly (`.dll` file) nei `Bin` cartella. Non è necessario copiare i file di database di programma (`.pdb`) o qualsiasi file XML in cui è possibile il `Bin` directory.

Non è necessario copiare i file del codice sorgente alla pagina ASP.NET nell'ambiente di produzione, né è necessario copiare il `BasePage.cs` file di classe.

> [!NOTE]
> Come illustrato nella figura 2, il `BasePage` classe viene implementata come un file di classe nel progetto, nella cartella denominata `HelperClasses`. Quando il progetto viene compilato il codice di `BasePage.cs` file compilato con classi ASP.NET alla pagina code-behind in un singolo assembly, `BookReviewsWAP.dll.` ASP.NET dispone di una cartella speciale denominata `App_Code` che è progettato per contenere i file di classe per il Web Progetti di sito. Il codice di `App_Code` cartella viene automaticamente compilato e pertanto non devono essere utilizzata con i progetti di applicazione Web. Al contrario, inserire il file di classe dell'applicazione in una normale cartella denominata `HelperClasses`, o `Classes`, o simile. In alternativa, è possibile inserire i file di classe in un progetto libreria di classi separato.


Oltre a copiare i file di codice relativi ad ASP.NET e l'assembly nel `Bin` cartella, è anche necessario copiare i file di supporto sul lato client, le immagini e file CSS - nonché altri file di supporto sul lato server, `Web.config` e `Web.sitemap`. Questi client e lato server file per il supporto da copiare nell'ambiente di produzione sia che si utilizzi la compilazione automatica o esplicita.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinare i file da distribuire per i file di progetto di sito Web

Il modello di progetto di sito Web supporta la compilazione automatica, una funzionalità non disponibile quando si utilizza il modello di progetto di applicazione Web. Con la compilazione esplicita è necessario compilare codice sorgente del progetto in un assembly e copiare l'assembly nell'ambiente di produzione. D'altra parte, con la compilazione automatica è sufficiente copiare il codice sorgente nell'ambiente di produzione e viene compilato dal runtime su richiesta in base alle esigenze.

L'opzione di menu di compilazione in Visual Studio è presente in progetti applicazione Web e progetti di siti Web. Compilazione di progetti applicazione Web consente di compilare codice sorgente del progetto in un unico assembly si trova nel `Bin` directory; verifica la presenza di eventuali errori in fase di compilazione di un progetto di sito Web, ma non crea tutti gli assembly di compilazione. Per distribuire un'applicazione ASP.NET sviluppata utilizzando il modello di progetto di sito Web tutto che è necessario eseguire è copia i file appropriati all'ambiente di produzione, ma è consigliabile da compilare prima il progetto per assicurarsi che non siano presenti errori in fase di compilazione.

Figura 3 mostra i file che costituiscono il progetto di sito Web revisioni Rubrica.


 [![Esplora soluzioni sono elencati i file che costituiscono il progetto di sito Web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Figura 3**: Esplora soluzioni sono elencati i file che costituiscono il progetto di sito Web


Distribuzione di un progetto di sito Web comporta la copia di tutti i file relativi ad ASP.NET nell'ambiente di produzione, che include le pagine di markup per le pagine ASP.NET, pagine master e controlli utente, insieme ai relativi file di codice. È anche necessario copiare i file di classe, ad esempio BasePage.cs. Si noti che il `BasePage.cs` file si trova nella `App_Code` cartella, ovvero una cartella speciale di ASP.NET usata nei progetti di sito Web per i file di classe. Della cartella speciale è necessario creare in produzione, nonché, come i file di classe nel `App_Code` cartella nell'ambiente di sviluppo deve essere copiato il `App_Code` cartella sulla produzione.

Oltre a copiare i file di codice sorgente e markup ASP.NET, è anche necessario copiare i file di supporto sul lato client, le immagini e file CSS - nonché altri file di supporto sul lato server, `Web.config` e `Web.sitemap`.

> [!NOTE]
> Progetti di sito Web è anche possibile usare la compilazione esplicita. Un'esercitazione in futura verrà esaminato compilare in modo esplicito un progetto di sito Web.


## <a name="summary"></a>Riepilogo

Distribuzione di un'applicazione ASP.NET comporta la copia dei file necessari dall'ambiente di sviluppo all'ambiente di produzione. Il set di file che devono essere sincronizzate preciso dipende se il codice dell'applicazione ASP.NET viene automaticamente o in modo esplicito compilato. La strategia di compilazione utilizzata è influenzata dalle se Visual Studio è configurato per gestire l'applicazione ASP.NET mediante il modello di progetto di applicazione Web o il modello di progetto di sito Web.

Il modello di progetto di applicazione Web viene utilizzata la compilazione esplicita e compila il codice del progetto in un singolo assembly nel `Bin` cartella. Quando si distribuisce l'applicazione, la parte di codice delle pagine ASP.NET e il contenuto del `Bin` cartella debba essere inserita nell'ambiente di produzione, il codice sorgente dell'applicazione - file di codice e classi code-behind, ad esempio, non è necessario da copiare nell'ambiente di produzione.

Il modello di progetto di sito Web utilizza la compilazione automatica per impostazione predefinita, sebbene sia possibile compilare in modo esplicito un progetto di sito Web, come verrà illustrato in futuro esercitazioni. Distribuzione di un'applicazione ASP.NET che viene utilizzata la compilazione automatica, è necessario che la parte di markup *e* codice sorgente deve essere copiato nell'ambiente di produzione. Il codice viene compilato automaticamente nell'ambiente di produzione quando è richiesto per la prima volta.

Ora che sono state esaminate quali file devono essere sincronizzate tra ambienti di sviluppo e produzione sono pronti distribuire l'applicazione recensioni a un provider di hosting web.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Cenni preliminari sulla compilazione di ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controlli utente ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Esame ASP. Esplorazione del sito della rete](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introduzione ai progetti di applicazione Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Esercitazioni di pagina master](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Condivisione del codice tra le pagine](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Utilizzando una classe di Base personalizzata per le classi di Code-Behind di pagine ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema di progetto di sito Web di Visual Studio 2005: informazioni sulla funzionalità e perché è farlo?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Procedura dettagliata: Conversione di un progetto di sito Web in un progetto di applicazione Web in Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Precedente](asp-net-hosting-options-cs.md)
> [Successivo](deploying-your-site-using-an-ftp-client-cs.md)
