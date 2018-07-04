---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Precompilazione del sito Web (c#) | Microsoft Docs
author: rick-anderson
description: 'Visual Studio offre agli sviluppatori ASP.NET due tipi di progetti: i progetti applicazione Web (WAP) e progetti di siti Web (WSPs). Uno dei betwe differenze principali...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: abe2c3329129259b9d83cb202fe730eda6abc94d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369434"
---
<a name="precompiling-your-website-c"></a>Precompilazione del sito Web (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio offre agli sviluppatori ASP.NET due tipi di progetti: i progetti applicazione Web (WAP) e progetti di siti Web (WSPs). Una delle principali differenze tra due tipi di progetto è che WAP deve avere il codice compilato in modo esplicito prima della distribuzione, mentre il codice in un WSP può essere compilato automaticamente nel server web. Tuttavia, è possibile precompilare un WSP prima della distribuzione. Questa esercitazione illustra i vantaggi della precompilazione e Mostra come precompilare un sito Web da Visual Studio e dalla riga di comando.


## <a name="introduction"></a>Introduzione

Visual Studio offre agli sviluppatori ASP.NET due diversi tipi di progetto: i progetti applicazione Web (WAP) e i progetti di sito Web (WSP). Una delle differenze principali tra questi tipi di progetto è che richiedono WAP *compilazione esplicita* mentre usano WSPs *compilazione automatica*, per impostazione predefinita. Con WAP, si compila codice dell'applicazione web in un singolo assembly, che viene creato del sito Web `Bin` cartella. Distribuzione comporta la copia del contenuto del markup (il `.aspx.ascx`, e `.master` file) del progetto, insieme all'assembly nel `Bin` cartella; il code-behind stessi file di classe non sono necessario per la distribuzione. D'altra parte, si distribuisce WSPs copiando sia le pagine di codice e delle relative classi di code-behind corrispondente all'ambiente di produzione. Le classi di code-behind vengono compilate su richiesta nel server web.

> [!NOTE]
> Fare riferimento alla sezione "Compilazione Versus automatica compilazione esplicita" i [ *determinare quali file devono essere distribuiti* esercitazione](determining-what-files-need-to-be-deployed-cs.md) per altre informazioni sulle differenze tra il progetto i modelli, la compilazione esplicita e automatica e influenza il modello di compilazione sulla distribuzione.


L'opzione di compilazione automatica è semplice da usare. Non è necessario alcun passaggio di compilazione esplicita e solo i file che sono stati modificati necessità per la distribuzione, mentre compilazione esplicita richiede la distribuzione di pagine modificate markup e l'assembly compilato just. Tuttavia, la distribuzione automatica presenta due potenziali svantaggi:

- Poiché le pagine devono essere compilate automaticamente quando sono visitati prima di tutto, può esserci un ritardo breve ma percettibile quando viene richiesta una pagina ASP.NET per la prima volta dopo la distribuzione.
- La compilazione automatica richiede che entrambi i dichiarativo markup e codice sorgente sia presente nel server web. Può trattarsi di un'opzione poco gradevole se si prevede di vendere l'applicazione web per i clienti che verrà installato nei server web.

Se una di due sopra difetti sono trattative breaker, è possibile si passa al modello di WAP oppure *precompilare* WSP prima della distribuzione. Questa esercitazione esamina le opzioni di precompilazione più adatte per un sito Web ospitato e illustra in modo dettagliato il processo di precompilazione e distribuzione di un sito Web precompilato.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Una panoramica della compilazione e la generazione del codice ASP.NET

Prima di esaminare le opzioni di precompilazione disponibili, esaminiamo innanzitutto la generazione di codice e la compilazione che si verifica quando viene richiesta una pagina ASP.NET per la prima volta, poiché è stato creato o ultimo aggiornamento. Come è noto, le pagine ASP.NET sono composti da due parti: markup dichiarativo di `.aspx` file, nonché una porzione di codice sorgente, in genere in un file di classe code-behind distinto (`.aspx.cs`). I passaggi eseguiti dal runtime quando viene richiesta una pagina ASP.NET dipende dal modello di compilazione dell'applicazione.

Con WAP, codice di origine alla pagina deve essere compilato in modo esplicito in un unico assembly prima di essere distribuito. Durante la distribuzione, questo assembly e le varie pagine di markup vengono copiate nell'ambiente di produzione. Quando arriva una richiesta al server web per una pagina ASP.NET, il runtime crea un'istanza della classe code-behind della pagina e richiama il `ProcessRequest` metodo, che viene avviato il ciclo di vita di pagina e, infine, genera il contenuto della pagina, viene restituito al il richiedente. Il runtime può lavorare con classe code-behind della pagina ASP.NET perché la classe code-behind è stato già compilata in un assembly prima della distribuzione.

Con compilazione automatica e WSPs, non c'è alcun passaggio di compilazione esplicita prima della distribuzione. Al contrario, la distribuzione comporta la copia dichiarativo sia il contenuto del codice sorgente nell'ambiente di produzione. Quando arriva una richiesta al server web per una pagina ASP.NET per la prima volta poiché la pagina è stata creata o aggiornata, il runtime deve compilare innanzitutto la classe code-behind in un assembly. Questo assembly compilato viene salvato nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, anche se il percorso di questa cartella può essere personalizzato tramite il `<compilation tempDirectory="" />` dell'elemento `<system.web>`, in genere in `Web.config`. Poiché l'assembly viene salvato su disco, non dovrà essere ricompilato con le richieste successive nella stessa pagina.

> [!NOTE]
> Come si può immaginare, vi sarà un lieve ritardo quando si richiede una pagina per la prima volta (o per la prima volta, poiché è stata modificata) in un sito che usa la compilazione automatica perché richiede qualche secondo per il server compilare il codice della pagina e salvare l'assembly risulta per disco.


In breve, con la compilazione esplicita è necessario per compilare codice sorgente del sito Web prima della distribuzione, salvando il runtime di dover eseguire questo passaggio. Compilazione automatica con il runtime gestisce la compilazione del codice sorgente alla pagina, ma con un costo di inizializzazione leggero per la prima visita alla pagina perché è stato creato o ultimo aggiornamento.

Ma per quanto riguarda la parte dichiarativa delle pagine ASP.NET (il `.aspx` file)? È ovvio che vi sia una relazione tra il `.aspx` file e il codice nelle relative classi di code-behind, come i controlli Web definiti nel markup dichiarativo sono accessibili nel codice. È anche evidente che il contenuto nel `.aspx` file influisce notevolmente sui rendering dei tag generati dalla pagina. Quindi, come funziona il runtime con il testo, HTML e sintassi per il controllo Web definiti nel `.aspx` il contenuto renderizzato del file per generare la pagina richiesta?

Non desidero deviare troppo sui dettagli di implementazione di basso livello, che variano tra WAP e WSPs, ma in pratica il runtime genera automaticamente un file di classe che contiene i vari controlli Web come metodi e membri protetti. Questo file generato viene implementato come un *classe parziale* alla classe code-behind corrispondente. ([Classi parziali](http://www.dotnet-guide.com/partialclasses.html) consentire per il contenuto di una singola classe siano distribuiti in più file.) Pertanto, la classe code-behind è definita in due posizioni: nel `.aspx.cs` file che si crea e in questa classe generate automaticamente create dal runtime. Questa classe generata automaticamente viene archiviata nel `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` cartella.

L'importante sottolineare qui è che per una pagina ASP.NET da sottoporre a rendering dal runtime sia relativo dichiarativa e parti di codice sorgente devono essere compilate in un assembly. Con WAP, il codice sorgente viene compilato in modo esplicito in un assembly prima della distribuzione, ma markup dichiarativo ancora deve essere convertito nel codice e compilate tramite il runtime nel server web. Con WSPs con compilazione automatica, il codice sorgente sia markup dichiarativo devono essere compilate con il server web.

È possibile utilizzare la compilazione esplicita con il modello di WSP. È possibile compilare in modo esplicito la porzione del codice sorgente, ad esempio con il modello di WAP. Inoltre, è anche possibile compilare il markup dichiarativo.

## <a name="precompilation-options"></a>Opzioni di precompilazione

.NET Framework viene fornito con un [strumento di compilazione ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) che consente di compilare il codice sorgente (e anche il contenuto) di un'applicazione ASP.NET compilata usando il modello WSP. Questo strumento è stato rilasciato con .NET Framework versione 2.0 e si trova nel `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` cartella; può essere usato dalla riga di comando o avviato dall'interno di Visual Studio tramite l'opzione Pubblica sito Web del menu Compila.

Lo strumento di compilazione fornisce due tipi generali di compilazione: posto la precompilazione e precompilazione per la distribuzione. Con la precompilazione posto si esegue la `aspnet_compiler.exe` dello strumento da riga di comando e specificare il percorso alla directory virtuale o al percorso fisico di un sito Web che si trova nel computer. Lo strumento di compilazione compila quindi per ogni pagina del progetto, l'archiviazione della versione compilata in ASP.NET il `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` cartella esattamente come se le pagine avevano ciascuno state visitate per la prima volta da un browser. La precompilazione posto può velocizzare la prima richiesta fatta per le pagine ASP.NET appena distribuite nel sito, perché allevia il runtime di dover eseguire questo passaggio. La precompilazione posto non è tuttavia utile per la maggior parte dei siti Web ospitati perché richiede che sia possibile eseguire programmi da quella del server web della riga di comando. In ambienti di hosting condivisi, questo livello di accesso non è consentito.

> [!NOTE]
> Per altre informazioni sulla precompilazione sul posto, consultare [How To: precompilazione di siti Web ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) e [precompilazione ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Invece di compilare le pagine nel sito Web per il `Temporary ASP.NET Files` cartella precompilazione per la distribuzione consente di compilare le pagine in una directory di propria scelta e in un formato che può essere distribuito nell'ambiente di produzione.

Esistono due tipologie di precompilazione per la distribuzione che verrà esaminata in questa esercitazione: precompilazione con un'interfaccia utente aggiornabili e precompilazione con un'interfaccia utente non aggiornabili. La precompilazione con un'interfaccia utente aggiornabili lascia il markup dichiarativo nel `.aspx`, `.ascx`, e `.master` file, consentendo agli sviluppatori di visualizzare e, se si desidera, modificare il markup dichiarativo nel server di produzione. Genera la precompilazione con un'interfaccia utente non aggiornabili `.aspx` pagine void di qualsiasi contenuto e rimuove `.ascx` e `.master` file, in tal modo nascondere il markup dichiarativo e divieto di uno sviluppatore di modificarla dal ambiente di produzione.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Precompilazione per la distribuzione con un'interfaccia utente aggiornabili

Il modo migliore per comprendere la precompilazione per la distribuzione è un esempio in azione. È possibile precompilare libro revisioni WSP per la distribuzione usando un'interfaccia utente aggiornabili. Lo strumento di compilazione ASP.NET può essere richiamato dal menu di compilazione di Visual Studio o dalla riga di comando. Questa sezione viene esaminata utilizzando lo strumento da Visual Studio. la sezione "precompilazione dalla riga di comando" analizza esegue lo strumento del compilatore da riga di comando.

Aprire WSP revisione libro in Visual Studio, passare al menu della compilazione e selezionare l'opzione di menu Pubblica sito Web. Verrà avviata la finestra di dialogo Pubblica sito Web (vedere **figura 1**), in cui è possibile specificare il percorso di destinazione, se è o meno dell'interfaccia utente del sito precompilato aggiornabile e altre opzioni degli strumenti del compilatore. Il percorso di destinazione può essere un server web remoto o un server FTP, ma per ora selezionare una cartella nel disco rigido del computer. Poiché desideriamo precompilare il sito con un'interfaccia utente aggiornabili, lasciare la casella di controllo "Consenti aggiornamento del sito precompilato" selezionata e fare clic su OK.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Figura 1**: strumento di compilazione ASP.NET verrà precompilare il sito Web nel percorso di destinazione specificato  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> L'opzione Pubblica sito Web nel menu di compilazione non è disponibile in Visual Web Developer. Se si usa Visual Web Developer, sarà necessario usare la versione della riga di comando dello strumento di compilazione ASP.NET, come illustrato nella sezione "precompilazione dalla riga di comando".


Dopo la precompilazione del sito Web, passare al percorso di destinazione che è stato immesso nella finestra di dialogo Pubblica sito Web. Si consiglia di confrontare il contenuto della cartella con il contenuto del sito Web. **Figura 2** Mostra la cartella del sito Web le recensioni dei libri. Si noti che lo contiene entrambe `.aspx` e `.aspx.cs` file. Si noti inoltre che il `Bin` directory include un solo file, `Elmah.dll`, che è stato aggiunto nel [esercitazione precedente](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Figura 2**: contiene la Directory del progetto `.aspx` e `.aspx.cs` file; la `Bin` cartella include semplicemente `Elmah.dll`  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](precompiling-your-website-cs/_static/image6.png))

**Figura 3** Mostra la cartella del percorso di destinazione il cui contenuto sono stato creato dallo strumento di compilazione ASP.NET. Questa cartella non contiene alcun file code-behind. Inoltre, questa cartella `Bin` directory include diversi assembly e due `.compiled` oltre al file il `Elmah.dll` assembly.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Figura 3**: la cartella del percorso di destinazione include i file per la distribuzione  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](precompiling-your-website-cs/_static/image9.png))

A differenza di compilazione esplicita in WAP, di precompilazione per processo di distribuzione non crea un assembly per l'intero sito. Al contrario, lo invia in batch insieme pagine diverse in ogni assembly. Compila inoltre il `Global.asax` del file (se presente) in un proprio assembly, nonché tutte le classi nel `App_Code` cartella. I file che contengono il markup dichiarativo per ASP.NET web pages, controlli utente e le pagine master (`.aspx`, `.ascx`, e `.master` file, rispettivamente) vengono copiate come-consiste nella directory di percorso di destinazione. Analogamente, il `Web.config` file viene copiato direttamente, insieme agli eventuali file statici, ad esempio immagini, le classi CSS e file PDF. Per una descrizione più formale del modo in cui lo strumento di compilazione gestisce vari tipi di file, fare riferimento a [gestione di File durante la precompilazione ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> È possibile indicare allo strumento di compilazione per creare un assembly per ogni pagina ASP.NET, controllo utente o pagina master selezionando la casella di controllo "Usati nomi fissato e assembly con pagina singola" dalla finestra di dialogo Pubblica sito Web. Disponibilità di ogni pagina ASP.NET compilata in un proprio assembly consente un controllo più accurato sulla distribuzione. Ad esempio, se si aggiornata una singola pagina web ASP.NET ed è necessaria distribuire tale modifica, è necessario distribuire solo tale pagina `.aspx` file e assembly associato all'ambiente di produzione. Consultare [procedura: generare nomi fissi con lo strumento di compilazione ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) per altre informazioni.


La directory del percorso di destinazione contiene anche un file che non faceva parte del progetto web precompilate, vale a dire `PrecompiledApp.config`. Questo file informa il runtime ASP.NET che l'applicazione è stata precompilata e indica se è stato precompilato con un'interfaccia utente aggiornabile o mezzogiorno aggiornabile.

Infine, si consiglia di aprire uno del `.aspx` file nel percorso di destinazione usando Visual Studio o l'editor di testo preferito. Quando la precompilazione per la distribuzione con un'interfaccia utente aggiornabili, le pagine ASP.NET nella directory di percorso di destinazione contengono lo stesso markup esattamente come i file corrispondenti nel sito Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Precompilazione per la distribuzione con un'interfaccia utente Non aggiornabili

Lo strumento compilatore ASP.NET è anche utilizzabile per precompilare un sito per la distribuzione con un'interfaccia utente non aggiornabili. La precompilazione del sito con un'interfaccia utente non aggiornabili funziona in modo analogo la precompilazione con un'interfaccia utente aggiornabile, la differenza principale è che le pagine ASP.NET, controlli utente e le pagine master nella directory di destinazione vengono rimossi i markup. Per precompilare un sito Web per la distribuzione con un'interfaccia utente non aggiornabili, scegliere l'opzione Pubblica sito Web dal menu Compila, ma deselezionare l'opzione "Consenti aggiornamento del sito precompilato" (vedere **figura 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Figura 4**: deselezionare l'opzione "Consenti aggiornamento del sito precompilato" opzione per precompilare con un Non aggiornabili interfaccia utente  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](precompiling-your-website-cs/_static/image12.png))

**Figura 5** Mostra la cartella del percorso di destinazione dopo la precompilazione con un'interfaccia utente non aggiornabili.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Figura 5**: la cartella del percorso di destinazione per la distribuzione con un'interfaccia utente Non aggiornabili  
 ([Fare clic per visualizzare l'immagine con dimensioni normali](precompiling-your-website-cs/_static/image15.png))

Confrontare **figura 3** al **figura5**. Anche se le due cartelle potrebbero essere identiche, si noti che la cartella non aggiornabili dell'interfaccia utente non dispone della pagina master `Site.master`. E mentre **figura 5** include varie pagine ASP.NET, se si visualizza il contenuto di questi file si noterà che è stata prive di loro markup dichiarativo e sostituiti con il testo segnaposto: "This is generato da un file di marcatore lo strumento di precompilazione e non devono essere eliminati! "

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Figura 5**: Markup dichiarativo è stata rimossa dalle pagine di ASP.NET

Il `Bin` le cartelle **figure 3** e **5** differiscono notevolmente più. Oltre all'assembly, il `Bin` nella cartella **figura5** include un `.compiled` file per ogni pagina ASP.NET, controllo utente e pagina master.

La precompilazione di un sito con un'interfaccia utente non aggiornabile è utile nelle situazioni in cui non si desidera contenuto ASP.NET alla pagina da modificare tramite la persona o azienda che consente di installare o gestisce il sito Web nell'ambiente di produzione. Se si compila un'applicazione web ASP.NET che viene venduto ai clienti per installare nel proprio server web, è possibile assicurarsi che non modificano l'aspetto del sito modificando direttamente il `.aspx` pagine te li rimandiamo. Mediante la precompilazione del sito Web con un'interfaccia utente non aggiornabili, aver spedito il segnaposto `.aspx` pagine come parte dell'installazione, impedendo in tal modo i clienti di esaminare o modificare il relativo contenuto.

### <a name="precompiling-from-the-command-line"></a>Precompilazione dalla riga di comando

Dietro le quinte, finestra di dialogo Pubblica sito Web di Visual Studio richiama lo strumento di compilazione ASP.NET (`aspnet_compiler.exe`) per la precompilazione del sito Web. In alternativa, è possibile richiamare questo strumento da riga di comando. Infatti, se si utilizza Visual Web Developer quindi è necessario eseguire lo strumento del compilatore da riga di comando, menu di Visual Web Developer compilazione include l'opzione Pubblica sito Web.

Per usare lo strumento del compilatore dalla riga di comando, avviare eliminando alla riga di comando e passare alla directory di framework, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Successivamente, immettere l'istruzione seguente nella riga di comando:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Il comando precedente consente di avviare lo strumento compilatore ASP.NET (`aspnet_compiler.exe`) e, tramite il `-p` interruttore, indica a pcs1 di precompilare il sito Web radice corrisponde alla *fisici\_percorso\_a\_app*; Questo valore sarà simile a `C:\MySites\BookReviews`e deve essere racchiuso tra virgolette.

Il `-v` consente di specificare la directory virtuale del sito. Se il sito è registrato come il sito Web predefinito nella metabase di IIS, è possibile omettere il `-p` passare e specificare solo la directory virtuale dell'applicazione. Se si usa la `-p` passa, procedere il valore di `-v` commutatore indica la radice del sito Web e consente di risolvere i riferimenti alla radice dell'applicazione. Ad esempio, se si specifica un valore pari `-v /MySite` quindi viene fatto riferimento nell'applicazione per `~/path/file` verrà risolto come `~/MySite/path/file`. Perché si trova nella directory radice alla mia società di hosting web il sito le recensioni dei libri ho utilizzato l'opzione `-v /`.

Il `-f` opzione, se presente, indica allo strumento di compilazione per sovrascrivere le *destinazione\_posizione\_cartella* se esiste già nella directory. Se si omette il `-f` switch e la cartella del percorso di destinazione esiste già, lo strumento di compilazione verrà terminato con l'errore: "errore ASPRUNTIME: la directory di destinazione non è vuota. . Eliminarlo manualmente o scegliere una destinazione diversa."

Il `-u` switch, se presente, informa lo strumento per creare un'interfaccia utente aggiornabili. Omettere questa opzione è attivata per precompilare il sito con un'interfaccia utente non aggiornabili.

Infine, il *destinazione\_posizione\_cartella* è il percorso fisico alla directory di percorso di destinazione; questo valore sarà simile a `C:\MySites\Output\BookReviews`e deve essere racchiuso tra virgolette.

## <a name="deploying-the-precompiled-website"></a>Distribuzione del sito Web precompilato

A questo punto è stato descritto come utilizzare lo strumento di compilazione ASP.NET per precompilare un sito Web con entrambe le opzioni dell'interfaccia utente aggiornabile e non aggiornabili. Tuttavia, negli esempi fino ad ora sono precompilati o meno il sito Web in una cartella locale e non nell'ambiente di produzione. La buona notizia è che la distribuzione del sito Web precompilato è un gioco da ragazzi e può essere eseguita tramite Visual Studio o tramite un altro meccanismo copia file, ad esempio da un client FTP autonomo.

La finestra di dialogo Pubblica sito Web (mostrata per prima nel **figura 1**) è un'opzione di percorso di destinazione, che indica dove vengono copiati i file del sito Web precompilato. Questo percorso può essere un server web remoto o un server FTP. Immissione di un server remoto nella casella di testo consente di precompilare e consente di distribuire il sito Web al server specificato in un unico passaggio. In alternativa, è possibile precompilare il sito Web in una cartella locale e quindi copiare manualmente il contenuto della cartella nell'ambiente di produzione tramite FTP o altri approcci.

Il sito Web precompilato distribuiti automaticamente tramite la finestra di dialogo Pubblica sito Web di Visual Studio è utile per i siti semplici in cui non sono presenti differenze di configurazione tra gli ambienti di sviluppo e produzione. Tuttavia, come indicato nel [ *differenze di configurazione comuni tra sviluppo e produzione* esercitazione](common-configuration-differences-between-development-and-production-cs.md) non è insolito per tali differenze esista. Ad esempio, l'applicazione web le recensioni dei libri Usa un database diverso nell'ambiente di produzione più nell'ambiente di sviluppo. Quando Visual Studio pubblica il sito Web in un server remoto alla cieca copia il file le informazioni di configurazione nell'ambiente di sviluppo.

Per i siti con differenze di configurazione tra gli ambienti di sviluppo e produzione potrebbe essere preferibile precompilare il sito in una directory locale, copiare i file di configurazione specifica dell'ambiente di produzione e quindi copiare il contenuto dell'output precompilate ambiente di produzione.

Per un aggiornamento della copia di file dall'ambiente di sviluppo all'ambiente di produzione consultare il [ *distribuisce il sito Web usando un FTP Client* ](deploying-your-site-using-an-ftp-client-cs.md) e [  *Distribuzione del sito Web con Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md) esercitazioni.

## <a name="summary"></a>Riepilogo

ASP.NET supporta due modalità di compilazione: automatico ed esplicito. Come illustrato nelle esercitazioni precedenti, i progetti applicazione Web (WAP) usare la compilazione esplicita mentre i progetti di siti Web (WSPs) utilizzano la compilazione automatica, per impostazione predefinita. Tuttavia, è possibile compilare in modo esplicito un WSP prima della distribuzione utilizzando lo strumento di compilazione ASP.NET.

Questa esercitazione è incentrata sulla precompilazione dello strumento di compilazione per il supporto di distribuzione. Quando la precompilazione per la distribuzione, lo strumento di compilazione crea una cartella del percorso di destinazione, compila il codice sorgente dell'applicazione web specificata e copia questi compilati gli assembly e i file di contenuto nella cartella dell'indirizzo di destinazione. Lo strumento di compilazione può essere configurato per creare un'interfaccia utente aggiornabili o non aggiornabili. Quando la precompilazione con un'opzione dell'interfaccia utente non è aggiornabile, il markup dichiarativo nei file di contenuto viene rimosso. In breve, la precompilazione consente di distribuire l'applicazione in base al progetto di sito Web senza includere eventuali file del codice sorgente e con markup dichiarativo rimosso, se lo si desidera.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Precompilazione del sito Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Compilazione in ASP.NET 2.0 e codebehind](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Precompilazione ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opzioni di aggiornamento del sito precompilato in ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Precedente](logging-error-details-with-elmah-cs.md)
> [Successivo](users-and-roles-on-the-production-website-cs.md)
