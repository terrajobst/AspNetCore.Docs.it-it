---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Precompilazione del sito Web (VB) | Documenti Microsoft
author: rick-anderson
description: 'Visual Studio offre agli sviluppatori ASP.NET due tipi di progetti: progetti di applicazione Web (WAP) e i progetti di sito Web (WSPs). Uno dei betwe differenze principali...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 7296808480fa48b4afd0b308cd27707378519747
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="precompiling-your-website-vb"></a>Precompilazione del sito Web (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio offre agli sviluppatori ASP.NET due tipi di progetti: progetti di applicazione Web (WAP) e i progetti di sito Web (WSPs). Una delle principali differenze tra due tipi di progetto è che WAP deve avere il codice compilato in modo esplicito prima della distribuzione, mentre il codice in un pacchetto WSP può essere compilato automaticamente nel server web. Tuttavia, è possibile precompilare un pacchetto WSP prima della distribuzione. In questa esercitazione vengono illustrati i vantaggi di precompilazione e viene illustrato come precompilare un sito Web da Visual Studio e dalla riga di comando.


## <a name="introduction"></a>Introduzione

Visual Studio offre agli sviluppatori ASP.NET due diversi tipi di progetto: progetti applicazione Web (WAP) e i progetti di sito Web (WSP). Una delle principali differenze tra questi tipi di progetto è che richiedono WAP *compilazione esplicita* mentre utilizzano WSPs *compilazione automatica*, per impostazione predefinita. Con WAP, si compila il codice dell'applicazione web in un unico assembly, viene creato il sito Web `Bin` cartella. Distribuzione comporta la copia del contenuto del markup (il `.aspx.ascx`, e `.master` file) nel progetto, insieme all'assembly nel `Bin` cartella; il code-behind non è necessario distribuire i file di classe. D'altra parte, si distribuisce WSPs copiando le pagine di codice sia le classi di codice corrispondenti all'ambiente di produzione. Le classi di codice vengano compilate su richiesta nel server web.

> [!NOTE]
> Vedere la sezione "Esplicita compilazione o compilazione automatica" nel [ *determinare quali file devono essere distribuiti* esercitazione](determining-what-files-need-to-be-deployed-vb.md) per ulteriori informazioni sulle differenze tra il progetto modelli, la compilazione esplicita e automatico e distribuzione di influenza il modello di compilazione.


L'opzione di compilazione automatica è semplice da usare. Non vi è alcun passaggio di compilazione esplicita e solo i file che sono stati modificati necessario per la distribuzione, mentre compilazione esplicita richiede la distribuzione le pagine di codice modificato e l'assembly compilato solo. Tuttavia, la distribuzione automatica presenta due svantaggi potenziali:

- Poiché le pagine devono essere compilate automaticamente quando innanzitutto visitato, può esistere un ritardo breve ma percettibile quando viene richiesta una pagina ASP.NET per la prima volta dopo la distribuzione.
- Compilazione automatica richiede che entrambi i dichiarativo markup e il codice sorgente sia presente nel server web. Può trattarsi di un'opzione della indesiderati se si prevede di vendere l'applicazione web per i clienti che verrà installato nei server web.

Se una di due sopra punti deboli sono breaker trattativa, è possibile passare al modello WAP o *precompilare* WSP prima della distribuzione. In questa esercitazione vengono esaminate le opzioni di precompilazione più adatte per un sito Web ospitato e vengono illustrati il processo di precompilazione e la distribuzione di un sito Web precompilato.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Cenni preliminari di generazione di codice ASP.NET e la compilazione

Prima di esaminare le opzioni di precompilazione disponibili, esaminiamo innanzitutto la generazione di codice e la compilazione che si verifica quando viene richiesta una pagina ASP.NET per la prima volta, poiché è stato creato o ultimo aggiornamento. Come è noto, pagine ASP.NET sono costituite da due parti: codice dichiarativo di `.aspx` file, nonché una porzione di codice sorgente, in genere in un file di classe code-behind separato (`.aspx.vb`). I passaggi eseguiti dal runtime quando viene richiesta una pagina ASP.NET dipende dal modello di compilazione dell'applicazione.

Con WAP, codice sorgente alla pagina deve essere compilato in modo esplicito in un unico assembly prima di essere distribuiti. Durante la distribuzione, questo assembly e le pagine di markup diversi vengono copiate nell'ambiente di produzione. Quando arriva una richiesta al server web per una pagina ASP.NET, il runtime crea un'istanza della classe code-behind della pagina e richiama il `ProcessRequest` metodo, che inizia il ciclo di vita di pagina e, infine, genera il contenuto della pagina, che viene restituito al il richiedente. Il runtime è possibile operare con classe code-behind della pagina ASP.NET la classe code-behind è stato già compilata in un assembly prima della distribuzione.

Con WSPs e compilazione automatica, non c'è alcuna fase di compilazione esplicita prima della distribuzione. Al contrario, la distribuzione comporta la copia dichiarativo sia il contenuto del codice sorgente nell'ambiente di produzione. Quando arriva una richiesta al server web per una pagina ASP.NET per la prima volta dopo la creazione o l'ultimo aggiornamento della pagina, il runtime deve compilare innanzitutto la classe code-behind in un assembly. L'assembly compilato viene salvato nella cartella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, anche se il percorso di questa cartella può essere personalizzato tramite la `<pages>` elemento `Web.config`. Poiché l'assembly viene salvato su disco, non è necessario per la ricompilazione durante le richieste successive nella stessa pagina.

> [!NOTE]
> Come prevedibile, è un leggero ritardo quando si richiede una pagina per la prima volta (o per la prima volta, poiché è stata modificata) in un sito che usa la compilazione automatica richiede molto tempo per il server compilare il codice della pagina e salvare l'assembly risultante per disco.


In breve, con la compilazione esplicita è necessario per compilare codice sorgente del sito Web prima della distribuzione, salvare il runtime di dover eseguire questo passaggio. Con la compilazione automatica con il runtime gestisce la compilazione del codice sorgente alla pagina, ma con un costo di inizializzazione leggero per la prima visita la pagina perché è stata creata o aggiornata.

Ma per quanto riguarda la parte dichiarativa delle pagine ASP.NET (il `.aspx` file)? È chiaro che vi sia una relazione tra il `.aspx` file e il codice nella loro classi code-behind, come i controlli Web definiti nel markup dichiarativo sono accessibili nel codice. È evidente che il contenuto di `.aspx` file influenza notevolmente il markup sottoposto a rendering generato dalla pagina di. Come funziona il runtime con il testo, HTML, e sintassi per il controllo Web definiti nel `.aspx` file per generare la pagina richiesta del rendering del contenuto?

Non si desidera deviare troppo sui dettagli di implementazione di basso livello, che variano tra WAP e WSPs, ma in pratica il runtime genera automaticamente un file di classe che contiene i diversi controlli Web come metodi e membri protetti. Questo file generato viene implementato come un *classe parziale* per la classe code-behind corrispondente. ([Classi parziali](http://www.dotnet-guide.com/partialclasses.html) consentono il contenuto di una singola classe di essere disseminate in più file.) Pertanto, la classe code-behind è definita in due posizioni: nel `.aspx.vb` file creati e in questa classe generata automaticamente creati dal runtime. Questa classe generata automaticamente viene archiviata nel `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` cartella.

L'importante sottolineare che per una pagina ASP.NET da sottoporre a rendering dal runtime sia relativo dichiarativa e parti di codice di origine devono essere compilate in un assembly. Con WAP, il codice sorgente viene compilato in modo esplicito in un assembly prima della distribuzione, ma il markup dichiarativo ancora deve essere convertito nel codice e compilato dal runtime nel server web. Con WSPs utilizzando compilazione automatica, il codice sorgente sia dichiarativo devono essere compilati dal server web.

È possibile utilizzare la compilazione esplicita con il modello WSP. È possibile compilare in modo esplicito la porzione del codice sorgente, ad esempio con il modello WAP. Inoltre, è anche possibile compilare il markup dichiarativo.

## <a name="precompilation-options"></a>Opzioni di precompilazione

.NET Framework viene fornito con un [strumento di compilazione ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) che consente di compilare il codice sorgente (e anche il contenuto) di un'applicazione ASP.NET compilata utilizzando il modello WSP. Questo strumento è stato rilasciato con .NET Framework versione 2.0 e si trova nella `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` cartella; può essere utilizzato dalla riga di comando o avviato dall'interno di Visual Studio tramite l'opzione Pubblica sito Web del menu Compila.

Lo strumento di compilazione fornisce due tipi generali di compilazione: sul posto precompilazione e precompilazione per la distribuzione. Con la precompilazione sul posto, si esegue il `aspnet_compiler.exe` strumento da riga di comando e specificare il percorso della directory virtuale o un percorso fisico di un sito Web che si trova nel computer in uso. Lo strumento di compilazione verrà quindi compilato il progetto, l'archiviazione della versione compilata in ogni pagina ASP.NET il `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` cartella esattamente come se le pagine avevano ogni state visitate per la prima volta da un browser. La prima richiesta effettuata per le pagine ASP.NET appena distribuite nel sito, perché allevia il runtime dalla necessità di eseguire questo passaggio è possibile velocizzare la precompilazione sul posto. Precompilazione sul posto non è tuttavia utile per la maggior parte dei siti Web ospitati perché richiede di essere in grado di eseguire programmi del server web della riga di comando. Questo livello di accesso non è consentito in ambienti host condivisi.

> [!NOTE]
> Per ulteriori informazioni sulla precompilazione sul posto, consultare [How To: precompilare siti Web di ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) e [precompilazione ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Anziché compilando le pagine nel sito Web per il `Temporary ASP.NET Files` cartella precompilazione per la distribuzione consente di compilare le pagine in una directory di propria scelta e in un formato che può essere distribuito nell'ambiente di produzione.

Esistono due tipologie di precompilazione per la distribuzione che vengono analizzate in questa esercitazione: precompilazione con un'interfaccia utente aggiornabile e precompilazione con un'interfaccia utente non è aggiornabile. Precompilazione con un'interfaccia utente aggiornabile lascia il markup dichiarativo di `.aspx`, `.ascx`, e `.master` file, consentendo agli sviluppatori di visualizzare e, se si desidera, modificare il markup dichiarativo sul server di produzione. Genera l'errore precompilazione con un'interfaccia utente non aggiornabile `.aspx` pagine void di qualsiasi contenuto e rimuove `.ascx` e `.master` file, in tal modo nascondere il markup dichiarativo e divieto di uno sviluppatore di modificare dal ambiente di produzione.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Precompilazione per la distribuzione con un'interfaccia utente aggiornabile

Il modo migliore per comprendere la precompilazione per la distribuzione è per un esempio in azione. Di seguito precompilare WSP revisioni Rubrica per la distribuzione tramite un'interfaccia utente aggiornabile. Lo strumento di compilazione ASP.NET può essere richiamato dal menu Compila di Visual Studio o dalla riga di comando. Questa sezione viene esaminata utilizzando lo strumento da Visual Studio. la sezione "precompilazione dalla riga di comando" analizza esecuzione dello strumento compilatore da riga di comando.

Apri revisione WSP libro in Visual Studio, passare al menu di compilazione e selezionare l'opzione di menu Pubblica sito Web. Verrà avviata la finestra di dialogo Pubblica sito Web (vedere **figura 1**), in cui è possibile specificare il percorso di destinazione o meno l'interfaccia utente del sito precompilato è aggiornabile e altre opzioni dello strumento compilatore. Il percorso di destinazione può essere un server web remoto o un server FTP, ma per ora, scegliere una cartella sul disco rigido del computer. Poiché si desidera precompilare il sito con un'interfaccia utente aggiornabile, lasciare la casella di controllo "Consenti aggiornamento del sito precompilato" selezionata e fare clic su OK.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Figura 1**: lo strumento di compilazione ASP.NET eseguirà la precompilazione del sito Web nel percorso di destinazione specificata  
 ([Fare clic per visualizzare l'immagine ingrandita](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> L'opzione Pubblica sito Web del menu Compila non è disponibile in Visual Web Developer. Se si utilizza Visual Web Developer, sarà necessario utilizzare la versione della riga di comando dello strumento di compilazione ASP.NET, come illustrato nella sezione "precompilazione dalla riga di comando".


Dopo la precompilazione del sito Web, passare al percorso di destinazione che immesso nella finestra di dialogo Pubblica sito Web. Richiedere qualche istante per confrontare il contenuto della cartella con il contenuto del sito Web. **Figura 2** Mostra la cartella del sito Web recensioni. Si noti che contiene entrambi `.aspx` e `.aspx.cs` file. Si noti inoltre che il `Bin` directory include un solo file, `Elmah.dll`, che viene aggiunto nel [esercitazione precedente](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Figura 2**: la Directory di progetto contiene `.aspx` e `.aspx.cs` file; la `Bin` cartella include solo `Elmah.dll`  
 ([Fare clic per visualizzare l'immagine ingrandita](precompiling-your-website-vb/_static/image6.png))

**Figura 3** Mostra la cartella del percorso di destinazione il cui contenuto sono stato creato dallo strumento di compilazione ASP.NET. Questa cartella non contiene alcun file code-behind. Inoltre, questa cartella `Bin` directory include diversi assembly e due `.compiled` file oltre al `Elmah.dll` assembly.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Figura 3**: la cartella del percorso di destinazione include i file per la distribuzione  
 ([Fare clic per visualizzare l'immagine ingrandita](precompiling-your-website-vb/_static/image9.png))

A differenza di compilazione esplicita in WAP, l'intestazione per il processo di distribuzione non crea un assembly per l'intero sito. Al contrario, batch insieme diverse pagine in ogni assembly. Compila inoltre il `Global.asax` file (se presente) nel proprio assembly, nonché tutte le classi nel `App_Code` cartella. I file che contengono il markup dichiarativo per ASP.NET web pagine master, pagine e controlli utente (`.aspx`, `.ascx`, e `.master` file, rispettivamente) vengono copiate come-consiste nella directory di percorso di destinazione. Analogamente, il `Web.config` file viene copiato direttamente, insieme a eventuali file statici, ad esempio immagini, le classi CSS e i file PDF. Per una descrizione più formale di come lo strumento di compilazione gestisce vari tipi di file, fare riferimento a [gestione File durante la precompilazione ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> È possibile indicare lo strumento di compilazione per creare un assembly per la pagina ASP.NET, controllo utente o pagina master selezionando la casella di controllo "Assembly con pagina singola e nomi fissato utilizzato" nella finestra di dialogo Pubblica sito Web. Con ogni pagina ASP.NET compilata nel proprio assembly consente un controllo più preciso sulla distribuzione. Ad esempio, se è aggiornata una singola pagina web ASP.NET e necessarie per distribuire le modifiche, è necessario distribuire solo tale pagina `.aspx` file e assembly associato all'ambiente di produzione. Consultare [procedura: generare nomi fissi con lo strumento di compilazione ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) per ulteriori informazioni.


La directory del percorso di destinazione contiene inoltre un file che non fa parte del progetto web precompilato, vale a dire `PrecompiledApp.config`. Questo file informa il runtime ASP.NET che l'applicazione è stato precompilato e se è stata precompilata con un'interfaccia utente aggiornabile o mezzogiorno aggiornabile.

Infine, è opportuno aprire uno del `.aspx` file nel percorso di destinazione utilizzando Visual Studio o l'editor di testo preferito. Durante la precompilazione per la distribuzione con un'interfaccia utente aggiornabile, nelle pagine ASP.NET nella directory di percorso di destinazione contengono il markup stesso esatto dei file corrispondenti nel sito Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Precompilazione per la distribuzione con un'interfaccia utente Non aggiornabile

Lo strumento compilatore ASP.NET anche utilizzabile per precompilare un sito per la distribuzione con interfaccia utente non è aggiornabile. La precompilazione del sito con un'interfaccia utente non aggiornabile funziona in modo analogo la precompilazione con un'interfaccia utente aggiornabile, la differenza principale che vengono rimosse le pagine ASP.NET, controlli utente e le pagine master nella directory di destinazione del codice. Per precompilare un sito Web per la distribuzione con interfaccia utente non è aggiornabile, scegliere l'opzione Pubblica sito Web dal menu Compila, tuttavia, deselezionare l'opzione "Consenti aggiornamento del sito precompilato" (vedere **figura 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Figura 4**: deselezionare l'opzione "Consenti a questo sito precompilato aggiornabile" opzione per precompilare con un Non aggiornabili interfaccia utente  
 ([Fare clic per visualizzare l'immagine ingrandita](precompiling-your-website-vb/_static/image12.png))

**Figura 5** Mostra la cartella del percorso di destinazione dopo la precompilazione con un'interfaccia utente non aggiornabile.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Figura 5**: la cartella del percorso di destinazione per la distribuzione con un'interfaccia utente Non aggiornabile  
 ([Fare clic per visualizzare l'immagine ingrandita](precompiling-your-website-vb/_static/image15.png))

Confrontare **figura 3** a **figura 5**. Mentre le due cartelle potrebbero apparire identiche, si noti che la cartella non aggiornabile di interfaccia utente non dispone della pagina master, `Site.master`. Mentre **figura 5** include varie pagine ASP.NET, se si visualizza il contenuto di questi file si noterà che è stato rimossi del codice dichiarativo e sostituiti con il testo segnaposto: "questo è un file marcatore generato da lo strumento di precompilazione e non deve essere eliminato. "

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Figura 5**: il Markup dichiarativo è stato rimosso dalle pagine ASP.NET

Il `Bin` cartelle **3 cifre** e **5** più sostanzialmente diversi. Oltre agli assembly, il `Bin` cartella **figura 5** include un `.compiled` file per ogni pagina ASP.NET, controllo utente e una pagina master.

Precompilazione di un sito con un'interfaccia utente non aggiornabile è utile nelle situazioni in cui non si desidera il contenuto alla pagina ASP.NET per essere modificato da una persona o all'azienda che consente di installare o gestisce il sito Web nell'ambiente di produzione. Se si compila un'applicazione web ASP.NET che viene venduto ai clienti per installare nel proprio server web, è consigliabile assicurarsi di non modificare l'aspetto del sito modificando direttamente il `.aspx` pagine della spedizione. La precompilazione del sito Web con un'interfaccia utente non è aggiornabile, si effettua la spedizione del segnaposto `.aspx` pagine come parte dell'installazione, impedendo in tal modo i clienti di esaminare o modificare il relativo contenuto.

### <a name="precompiling-from-the-command-line"></a>Precompilazione dalla riga di comando

Dietro le quinte, la finestra di dialogo Pubblica sito Web di Visual Studio richiamato lo strumento di compilazione di ASP.NET (`aspnet_compiler.exe`) per la precompilazione del sito Web. In alternativa, è possibile richiamare questo strumento da riga di comando. Infatti, se si utilizza Visual Web Developer quindi è necessario eseguire lo strumento compilatore dalla riga di comando, come menu della Visual Web Developer compilazione non include l'opzione Pubblica sito Web.

Per utilizzare lo strumento compilatore dalla riga di comando, avviare eliminando alla riga di comando e passare alla directory del framework, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Quindi, immettere l'istruzione seguente nella riga di comando:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Il comando precedente consente di avviare lo strumento compilatore ASP.NET (`aspnet_compiler.exe`) e, tramite il `-p` commutatore, istruzioni per precompilare il sito Web radice *fisico\_percorso\_a\_app*; Questo valore sarà simile al seguente `C:\MySites\BookReviews`e devono essere delimitate da virgolette.

Il `-v` consente di specificare la directory virtuale del sito. Se il sito è registrato come sito Web predefinito nella metabase di IIS, è possibile omettere il `-p` passa e specificare solo la directory virtuale dell'applicazione. Se si utilizza il `-p` passare, procedere il valore di `-v` commutatore indica la radice del sito Web e viene utilizzato per risolvere i riferimenti alla radice dell'applicazione. Ad esempio, se si specifica un valore di `-v /MySite` quindi viene fatto riferimento nell'applicazione per `~/path/file` verrà risolto come `~/MySite/path/file`. Perché si trova nella directory principale in my società di hosting web il sito recensioni ho utilizzato l'opzione `-v /`.

Il `-f` switch, se presente, indica allo strumento di compilazione per sovrascrivere il *destinazione\_percorso\_cartella* directory se esiste già. Se si omette il `-f` commutatore e la cartella del percorso di destinazione esiste già, lo strumento di compilazione verrà terminato con l'errore: "errore ASPRUNTIME: la directory di destinazione non è vuota. Sarà necessario eliminarlo manualmente o scegliere una destinazione diversa."

Il `-u` switch, se presente, che indica lo strumento per creare un'interfaccia utente aggiornabile. Omettere questa opzione per precompilare il sito con un'interfaccia utente non è aggiornabile.

Infine, il *destinazione\_percorso\_cartella* è il percorso fisico della directory di destinazione percorso; questo valore sarà simile al seguente `C:\MySites\Output\BookReviews`e devono essere delimitate da virgolette.

## <a name="deploying-the-precompiled-website"></a>Distribuire il sito Web precompilato

A questo punto è stato descritto come utilizzare lo strumento di compilazione ASP.NET per precompilare un sito Web con entrambe le opzioni dell'interfaccia utente aggiornabili e non aggiornabili. Tuttavia, negli esempi finora sono precompilato il sito Web in una cartella locale e non nell'ambiente di produzione. Buone notizie si distribuisce il sito Web precompilato è molto semplice che può essere eseguita tramite Visual Studio o tramite un altro meccanismo copia file, ad esempio da un client FTP autonomo.

La finestra di dialogo Pubblica sito Web (nelle **figura 1**) è un'opzione di percorso di destinazione, che indica dove vengono copiati i file del sito Web precompilato. Questo percorso può essere un server web remoto o un server FTP. Immissione di un server remoto in questa casella di testo precompilato e consente di distribuire il sito Web al server specificato in un unico passaggio. In alternativa, è possibile precompilare il sito Web in una cartella locale e quindi copiare manualmente il contenuto della cartella nell'ambiente di produzione tramite FTP o altri approcci.

Con il sito Web precompilato distribuiti automaticamente tramite la finestra di dialogo Pubblica sito Web di Visual Studio è utile per i siti semplici in cui non esistono differenze configurazione tra gli ambienti di sviluppo e produzione. Tuttavia, come indicato nel [ *comuni configurazione differenze tra lo sviluppo e produzione* esercitazione](common-configuration-differences-between-development-and-production-vb.md) non è insolito per tali differenze esista. Ad esempio, l'applicazione web recensioni utilizza un database diverso nell'ambiente di produzione nell'ambiente di sviluppo. Quando Visual Studio pubblica il sito Web a un server remoto ciecamente copia il file le informazioni di configurazione nell'ambiente di sviluppo.

Per i siti con le differenze di configurazione tra gli ambienti di sviluppo e produzione potrebbe essere preferibile precompilare il sito in una directory locale, copiare i file di configurazione specifiche di produzione e quindi copiare il contenuto dell'output precompilato produzione.

Per un aggiornamento nella copia dei file dall'ambiente di sviluppo all'ambiente di produzione, vedere il [ *la distribuzione del sito Web utilizzando un FTP Client* ](deploying-your-site-using-an-ftp-client-vb.md) e [  *Distribuzione del sito Web tramite Visual Studio* ](determining-what-files-need-to-be-deployed-vb.md) esercitazioni.

## <a name="summary"></a>Riepilogo

ASP.NET supporta due modalità di compilazione: automatico ed esplicito. Come illustrato nelle esercitazioni precedenti, progetti di applicazione Web (WAP) usare la compilazione esplicita mentre i progetti di sito Web (WSPs) usare la compilazione automatica, per impostazione predefinita. Tuttavia, è possibile compilare in modo esplicito un pacchetto WSP prima della distribuzione tramite lo strumento di compilazione ASP.NET.

In questa esercitazione è incentrata sulla precompilazione dello strumento di compilazione per il supporto di distribuzione. Durante la precompilazione per la distribuzione, lo strumento di compilazione crea una cartella del percorso di destinazione, compila il codice sorgente dell'applicazione web specificato e consente di copiare questi compilati gli assembly e i file di contenuto nella cartella del percorso di destinazione. Lo strumento di compilazione può essere configurato per creare un'interfaccia utente aggiornabile o non è aggiornabile. Durante la precompilazione con un'opzione dell'interfaccia utente non è aggiornabile, viene rimosso il markup dichiarativo nei file di contenuto. In breve, la precompilazione consente di distribuire l'applicazione basata sul progetto di sito Web senza includere eventuali file del codice sorgente e con il markup dichiarativo rimosso, se lo si desidera.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Precompilazione del sito Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [CodeBehind e compilazione in ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Precompilazione ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opzioni del sito precompilato in ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Precedente](logging-error-details-with-elmah-vb.md)
> [Successivo](users-and-roles-on-the-production-website-vb.md)
