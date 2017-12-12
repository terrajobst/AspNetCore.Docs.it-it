---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Miglioramenti in Visual Studio 2005 | Documenti Microsoft
author: microsoft
description: Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web con un lungo elenco di miglioramenti per i progetti Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 2c1f9a7291d8eab675bac3e1c37d6922131e3761
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="improvements-in-visual-studio-2005"></a>Miglioramenti in Visual Studio 2005
====================
da [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web con un lungo elenco di miglioramenti per i progetti Web.


Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web con un lungo elenco di miglioramenti per i progetti Web. Nonostante la potenza offerta Visual Studio .NET 2002 e 2003, si sono verificati reclami molte in modo che i progetti Web sono stati gestiti. Visual Studio 2005 aggiunge un numero significativo di nuove funzionalità per indirizzare tali reclami. Per coloro che preferiscono la modalità di gestione della compilazione di applicazioni Web di Visual Studio .NET 2003, vedere [progetti applicazione Web](https://go.microsoft.com/fwlink/?LinkId=57870).

In questo modulo riguardano anche miglioramenti nella creazione del progetto Web, gestione e sviluppo. In un modulo successivo, riguardano anche miglioramenti nella compilazione dei progetti Web e distribuirli.

## <a name="frontpage-server-extensions"></a>Estensioni del Server di FrontPage

Visual Studio .NET 2002 e 2003 necessarie estensioni del Server di FrontPage per creare o compilare i progetti Web nella finestra di. Gli sviluppatori conteneva una scelta tra le due modalità di accesso diverso (estensioni del Server di FrontPage o File modalità di accesso), estensioni del Server di FrontPage entrambi utilizzati per eseguire attività quali l'impostazione di radice dell'applicazione in IIS e così via.

Visual Studio 2005 rimuove la dipendenza da estensioni per i progetti locali. Visual Studio 2005 consente di accedere a questo punto la metabase IIS direttamente anziché utilizzare le estensioni del Server di FrontPage. Visual Studio 2005 aggiunge inoltre supporto per FTP che consente l'accesso remoto progetto senza estensioni del Server di FrontPage.

Per gli sviluppatori che desiderano utilizzare le estensioni del Server di FrontPage nei progetti, l'opzione è ancora disponibile. Tuttavia, in base al feedback sicuro dalla community degli sviluppatori ASP.NET, non è un requisito.

> [!NOTE]
> Estensioni del Server sono ancora necessari per la creazione del progetto remoto, apertura e così via.


## <a name="aspnet-development-server"></a>server di sviluppo ASP.NET

Visual Studio 2005 viene fornito con un nuovo server Web denominato Server di sviluppo ASP.NET. (Il server Web è stato precedentemente noto come Cassini).

Esistono diversi vantaggi del Server di sviluppo ASP.NET.

- È ora possibile che utenti non amministratori sviluppare ed eseguire il debug con un server Web.
- Il Server di sviluppo ASP.NET esegue il mapping in modo dinamico le directory virtuali in qualsiasi posizione nel file system che per i progetti flessibile.
- Gli utenti in Windows XP Professional che già utilizzano IIS ora saranno possibile creare nuove applicazioni Web che non influiscono sulla struttura di file o una cartella del sito Web predefinito in IIS.

Nessuna configurazione speciale è necessario per sfruttare i vantaggi del Server di sviluppo ASP.NET. Quando un progetto Web che è ospitato nel file system viene eseguito il debug o esplorare, Visual Studio 2005 su una porta casuale per soddisfare la richiesta verrà automaticamente avviato un'istanza del Server di sviluppo ASP.NET.

Verranno fornite ulteriori informazioni sul Server di sviluppo ASP.NET più avanti in questo modulo.

## <a name="improved-file-management"></a>Gestione migliorata di File

In Visual Studio 2002 e 2003, un file di progetto (con estensione vbproj per Visual Basic.NET) e con estensione csproj per c# archiviate informazioni su tutti i file nell'applicazione Web. La visualizzazione di Esplora soluzioni si basa le informazioni del file nel file di progetto. Per questo motivo, Esplora soluzioni verrebbe spesso contengono informazioni non corrette nei casi in cui sono stati utilizzati editor esterni. Visual Studio 2002 e 2003 sarebbe spesso sovrascrivere le modifiche al file o non visualizzare la versione più recente dei file.

Visual Studio 2005 non immediatamente con il file di progetto. Invece, legge le informazioni di file e cartelle direttamente dal disco, risultante in una visualizzazione accurata dei file nel progetto. Poiché la cartella riferimenti in Visual Studio 2002 e 2003 non rappresenta una cartella effettiva nell'applicazione Web, Visual Studio 2005 rimuove anche la cartella dei riferimenti da Esplora soluzioni. Per accedere i riferimenti per il progetto in Visual Studio 2005, è consigliabile utilizzare le pagine delle proprietà per il progetto.

## <a name="creating-web-projects"></a>La creazione di progetti Web

Gli sviluppatori di Web hanno molte nuove opzioni disponibili per la creazione del progetto in Visual Studio 2005. Siti Web è ora possibile creare un punto qualsiasi nel file system e può quindi eseguire il debug o visualizzati utilizzando il nuovo Server di sviluppo ASP.NET. Gli sviluppatori possono anche creare nuovi siti Web tramite FTP.

Fare clic qui per visualizzare una procedura dettagliata video di creazione di progetti Web in Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Aprire Video a schermo intero](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Progetti di file System

Come osservato nella procedura dettagliata video, è possibile scegliere di creare siti Web nel file system nel computer locale o in una posizione remota tramite una condivisione file. Siti Web che vengono creati nel file system vengono visualizzati e sottoposti a debug tramite il Server di sviluppo ASP.NET.

> [!NOTE]
> Il Server di sviluppo ASP.NET potrebbe confondere per i clienti. Se un progetto Web viene creato nel file system nella struttura di directory IISs (ad esempio c:\inetpub\wwwroot), il sito Web verrà comunque visualizzare tramite il Server di sviluppo ASP.NET quando avviato dall'interno di Visual Studio 2005. Pertanto, qualsiasi configurazione di IIS (ad esempio i metodi di autenticazione) non è applicabile.


Il progetto web predefinito consente inoltre di rimuovere una grande quantità di sovraccarico da include solo una pagina aspx, file default.cs e un'App\_cartella dati. Il file Web. config e le cartelle speciali (ad esempio app\_codice) vengono aggiunti, se necessario. Il progetto web include solo i file e cartelle in cui è necessario.

### <a name="http-projects"></a>Progetti HTTP

Progetti HTTP possono essere i progetti creati in un sito Web IIS locale o in un sito Web remoto. Il percorso di progetto predefinito è `http://localhost`. Se si fa clic sul pulsante Sfoglia, sono disponibili due opzioni di HTTP: IIS locale e del sito remoto. La differenza principale in queste due opzioni è il metodo in cui le informazioni sul sito web è visualizzati nella finestra di dialogo Scegli percorso e in modalità con cui i file vengono copiati nel server Web.

L'opzione IIS locale legge le informazioni sul sito dalla metabase nel computer locale e i file vengono copiati mediante il file system. L'opzione del sito remoto utilizza estensioni del Server e le informazioni sul sito e i file vengono copiati tramite HTTP e chiamate RPC di estensioni del Server di FrontPage.

> [!NOTE]
> Vs # # #\_file tmp. htm e get\_aspx\_ver.aspx non vengono più utilizzati per determinare le informazioni sulla versione.


L'opzione HTTP predefinita è IIS locale. Questa opzione legge la Metabase di IIS per determinare quali siti sono disponibili e il percorso in cui creare il contenuto. È possibile selezionare una cartella diversa o la directory virtuale, selezionarlo nella visualizzazione albero. È possibile anche creare una nuova directory virtuale, contrassegnare le cartelle come applicazioni, nonché eliminare le directory virtuali esistenti da questa finestra di dialogo.


![La finestra di dialogo percorso Scegli](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: la finestra di dialogo percorso Scegli


A differenza nelle versioni precedenti di Visual Studio, se si seleziona il **utilizza Secure Sockets Layer** casella di controllo e il certificato SSL non corrisponde all'URL per l'esplorazione, verrà visualizzata una finestra di dialogo Avviso di sicurezza in cui viene richiesto se si come procedere. Utilizzando Visual Studio .NET 2003, se il certificato non è una corrispondenza, la creazione del progetto riuscirebbero.


![Avviso certificato SSL relative alla sicurezza](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: avviso di sicurezza riguardante il certificato SSL


### <a name="note-on-host-headers"></a>Nota sulle intestazioni Host

Se si sta creando un'applicazione Web in un sito associato a un indirizzo IP specifico, è necessario assicurarsi che sia configurata in un'intestazione host. In caso contrario, Visual Studio creerà il sito all'indirizzo `http://localhost`, ma l'indirizzo IP verrà risolto correttamente quando il sito è esplorare o eseguire il debug dall'interno dell'IDE.

Se si seleziona l'opzione del sito remoto, la finestra di dialogo Modifica che consente di immettere l'URL di destinazione per il nuovo sito Web. Questo URL deve essere in un server con le estensioni del Server di FrontPage abilitato. Se si desidera utilizzare con il server Web locale utilizzando le estensioni del Server di FrontPage, è possibile utilizzare l'opzione del sito remoto e specificare un URL locale.


![Creazione di un sito Web in un Server remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: creazione di un sito Web in un Server remoto


Quando si crea un'applicazione in un sito remoto tramite SSL, se il certificato SSL non corrispondono, la finestra di dialogo di conferma è leggermente diverso dalla finestra di dialogo visualizzata quando si utilizza l'opzione IIS locale.


![L'avviso di sicurezza del sito remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: l'avviso di sicurezza del sito remoto


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 è stata introdotta l'opzione per creare siti Web tramite FTP. Quando si utilizza questa opzione, l'IDE crea localmente i file nella cartella temporanea di utenti e quindi utilizza il protocollo FTP per spostare i file nel percorso FTP.

> [!NOTE]
> Il percorso della cartella temp è c:\Documents and Settings\&lt; Utente&gt;\Local Settings\Temp\VWDWebCache\&lt; Server&gt;\_&lt;nome dell'applicazione&gt;


Quando si utilizza l'opzione FTP, verrà visualizzata una finestra di dialogo Scegli percorso. Immettere le informazioni di connessione FTP necessarie in questa finestra di dialogo come illustrato di seguito.


![La finestra di dialogo percorso FTP di scegliere](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: la finestra di dialogo percorso FTP di scegliere


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratorio: Configurazione FTP del sito e creare un progetto

I passaggi seguenti configurare il sito FTP in modo che un utente dispone di un percorso che è solo possibile caricare in tramite FTP.

### <a name="install-the-ftp-service"></a>Installare il servizio FTP

1. Aprire Installazione applicazioni, selezionare Installazione componenti di Windows
2. Selezionare Internet Information Services (Server applicazioni in Windows 2003) e fare clic su **dettagli**.
3. Controllare **servizio protocollo FTP (File Transfer)** e fare clic su **OK**.
4. Fare clic su **Avanti** per installare il servizio FTP.

### <a name="create-a-new-folder-for-content"></a>Creare una nuova cartella per il contenuto

1. In Esplora risorse, creare una nuova cartella denominata **User1** all'interno di c:\inetpub\wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurare cartelle e autorizzazioni per le cartelle.

1. Aprire lo snap-in Internet Information Services da strumenti di amministrazione. È ora una cartella siti FTP nel nodo del nome del computer.
2. Espandere **siti FTP**.
3. Fare doppio clic su di **sito FTP predefinito**selezionare **New**, quindi **Directory virtuale**, quindi fare clic su **Avanti**.
4. Immettere **User1** per il nome della directory virtuale e fare clic su **Avanti**.
5. Immettere **c:\inetpub\wwwroot\User1** per il percorso e fare clic su **Avanti**.
6. Fare clic su **Avanti** e quindi **fine** per completare la procedura guidata.
7. Fare doppio clic su di **User1** directory virtuale nel sito FTP predefinito e selezionare **proprietà**.
8. Controllare il **scrivere** casella di controllo e fare clic su **OK** per chiudere la finestra di dialogo.
9. Fare doppio clic su **sito FTP predefinito** e selezionare **proprietà**.
10. Nel **gli account di sicurezza** scheda, deselezionare **consentire connessioni anonime**.
11. Fare clic su **Sì** nella finestra di dialogo che chiede se desideri continuare.
12. Fare clic su **OK** per chiudere la finestra di dialogo.
13. Espandere il **sito Web predefinito** sotto il **siti Web** nodo.
14. Fare doppio clic su di **User1** directory e selezionare **proprietà**
15. Nel **le impostazioni dell'applicazione** fare clic su **crea** per contrassegnare la cartella di un'applicazione.
16. Fare clic su **OK** per chiudere la finestra di dialogo.
17. Chiudere lo snap-in Internet Information Services.

### <a name="create-web-project"></a>Creare il progetto web

1. Aprire Visual Studio 2005.
2. Dal **File** dal menu **nuovo sito Web**.
3. Nel **percorso** elenco a discesa Seleziona **FTP**.
4. Fare clic su **Sfoglia**.
5. Immettere **localhost** nel **Server** casella di testo.
6. Immettere **User1** nella casella di testo di Directory.
7. Fare clic su **aprire**. Il percorso del FTP verrà immessa nella finestra di dialogo Nuovo sito Web.
8. Fare clic su **OK**.
9. Deselezionare **anonima accesso** nella finestra di dialogo connessione FTP, immettere le credenziali e fare clic su **OK**.
10. Che cos'è l'URL per il progetto? (Verrà visualizzato l'URL per il progetto in Esplora soluzioni).
11. Dal **compilare** dal menu **Compila sito Web** o **Compila soluzione**.
12. Fare clic su Default.aspx in Esplora soluzioni e selezionare **Visualizza nel Browser**.
13. Nella finestra di dialogo URL del sito Web necessario immettere `http://localhost/user1` per l'URL e fare clic su **OK**.

> [!NOTE]
> Se viene visualizzato un errore che indica l'impossibilità di caricare il tipo \_predefinito, assicurarsi che nel sito Web e non una versione precedente in esecuzione ASP.NET 2.0. È possibile farlo dalla scheda ASP.NET in Internet Information Services.


## <a name="opening-web-projects"></a>Apertura di progetti Web

Apertura di progetti Web è simile alla creazione di progetti. Nelle sezioni seguenti vengono chiamate le aree per tenere sotto controllo out per durante l'utilizzo all'interno dell'IDE. Descrive inoltre lavorare con progetti Web tramite HTTP e FTP.

Per aprire un progetto Web, selezionare Apri sito Web dal menu File. Verrà richiesto con la finestra di dialogo Scegli percorso stesso descritte in precedenza e si hanno le stesse opzioni disponibili quattro: File System, IIS locale, FTP e del sito remoto.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>File system

Come indicato in precedenza in questo modulo, Visual Studio non usa un file di progetto. Pertanto, se si sceglie di aprire un sito Web dal file system, è effettivamente il possibilità di scegliere una cartella che si desidera, anche se la cartella selezionata non è stata creata come un progetto Web inizialmente in Visual Studio. Ad esempio, è possibile scegliere di aprire la cartella documenti come sito Web e verrà invece aprirlo e visualizzare i file, come illustrato di seguito.


![I documenti aperti come un sito Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *documenti* aperto in un sito Web


Poiché Visual Studio crea solo altri file e cartelle se necessario, nessun file o cartelle aggiuntivi vengono aggiunti per il percorso che è aprire. Un effetto collaterale di questa architettura è che impedisce l'annidamento di siti Web nel file system. Ad esempio, si consideri la seguente struttura di directory.

Progetto Web in C:\MyWebSite

Un altro progetto web in C:\MyWebSite\Nested

Quando si apre il sito Web al c:\MyWebSite, la cartella Nested verrà visualizzato come una sottocartella di tale applicazione.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Quando si apre i siti Web tramite HTTP, le impostazioni vengono lette dalla metabase di IIS (IIS locali) o tramite estensioni del Server di FrontPage (sito remoto). Se sono presenti applicazioni web annidati, questi vengono visualizzati anche con un'icona che li identifica come applicazione. Se si ha familiarità con l'utilizzo di applicazioni web di FrontPage, il comportamento in Visual Studio 2005 è simile.

Anche se Visual Studio verrà visualizzata un'icona per le applicazioni che sono annidati all'applicazione che è attualmente aperto all'interno dell'IDE, quindi verrà non consente di espandere in modo da vedere il relativo contenuto. Possono, tuttavia, fare doppio clic su di essi per aprirli. Quando si esegue l'operazione, si visualizzerà una finestra di dialogo viene richiesto di aprire l'applicazione web (e sostituire la soluzione attualmente aperta) o aggiungere l'applicazione Web alla soluzione corrente.


![Fare doppio clic su un'icona applicazione annidata viene visualizzata questa finestra di dialogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: fare doppio clic su un'icona applicazione annidata viene visualizzata questa finestra di dialogo


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Sito FTP

Quando si apre un sito tramite FTP, i file vengono tutti copiati localmente nella cartella temporanea. Il percorso completo per il percorso di archiviazione locale viene visualizzato nel riquadro proprietà per il progetto e viene creato utilizzando il formato seguente.

C:\Documents and Settings\&lt; Utente&gt;\Local Settings\Temp\VWDWebCache\&lt; Server&gt;\_&lt;nome dell'applicazione&gt;

Quando si utilizza FTP, Visual Studio sarà necessario specificare l'URL di base per il progetto in modo che possano visualizzarlo come illustrato di seguito. Se non si specifica un URL di base, Visual Studio verrà richiesta la prima volta che si tenta di visualizzare una pagina nel sito Web.


![Specificare un URL di Base per i siti FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: specifica un URL di Base per i siti FTP


## <a name="improvements-in-compilation"></a>Miglioramenti nella compilazione

Utilizzo di applicazioni Web in Visual Studio 2005 è notevolmente più veloce rispetto alle versioni precedenti. Si tratta in nessuna parte di piccole dimensioni a causa di modifiche all'architettura di compilazione.

In Visual Studio 2002 e 2003, le applicazioni Web sono state compilate in un unico assembly principale che si trovano nella cartella /bin. In Visual Studio 2005, un'App\_cartella del codice è stato aggiunto. Classi e altro codice dell'interfaccia utente non vengono aggiunte all'App\_cartella del codice. Quando Visual Studio compila il progetto, tutti i file nell'App\_cartella del codice vengono compilati in una singola App\_Code.dll file. Il risultato di questa modifica è che le compilazioni successive sono molto più veloce nelle versioni precedenti.

> [!NOTE]
> L'utilità della riga di comando MSBuild nonché delle applicazioni Web ASP.NET. Tale strumento verrà descritta nel modulo 9.


Un altro miglioramento alla compilazione è la nuova opzione di compilazione pagina dal menu Compila. Questa funzionalità consente agli sviluppatori di ricompilare solo la pagina corrente (insieme a, naturalmente, e le dipendenze) in modo che le modifiche possono essere compilate in modo più rapido. Poiché c# non consente di compilazione in background per scopi di aggiornamento di IntelliSense e così via, sono sfrutterà sono estremamente questa funzionalità poiché ciò permette di IntelliSense aggiornare rapidamente ricompilando semplicemente una singola pagina.

Le proprietà di compilazione per un progetto consentono di configurare il tipo di compilazione che si verifica prima che venga eseguita la pagina di avvio. Gli sviluppatori possono scegliere di creare solo la pagina corrente in modo che Visual Studio è possibile avviare il debug di applicazioni più velocemente modifiche apportate al codice.


![L'azione di avvio di pagina compilazione](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: l'azione di avvio di pagina compilazione


Un altro miglioramento alla grande a Visual Studio e l'architettura ASP.NET si trova nell'area di modifica e continuazione. In Visual Studio 2005, gli sviluppatori possono avviare il debug di un progetto e apportare modifiche al codice nel progetto senza scollegare il debugger. In effetti, è possibile letteralmente avviare il debug di un progetto, aggiungere una nuova classe, aggiungere codice a tale classe, aggiungere codice che crea una nuova istanza della classe ed eseguire un metodo della classe, senza la disconnessione del debugger. Il nuovo codice in esecuzione è letteralmente facile quanto l'aggiornamento del browser.

Fare clic qui per visualizzare una procedura dettagliata video della modifica e continuazione di funzionalità in Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Aprire Video a schermo intero](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Le affidabili modifica e continuazione di funzionalità in ASP.NET 2.0 e Visual Studio 2005 è dovuto a una modifica dell'architettura per le applicazioni ASP.NET. In ASP.NET 1. x, le applicazioni create in Visual Studio 2002/2003 sono stati compilati in un assembly principale che è stato archiviato nella cartella /bin. Tutte le classi, pagine, e così via, per l'applicazione sono stati compilati in una DLL. In fase di esecuzione, ASP.NET sarebbe quindi compilare tutti i controlli, markup e il codice all'interno di pagine ASP.NET e copiare le DLL nella cartella temporanea ASP.NET.

In Visual Studio 2005 con ASP.NET 2.0, i modelli di due compilazione descrive sopra (uno per Visual Studio) e uno per ASP.NET in fase di esecuzione sono state unite in un modello comune di compilazione. Ciò significa che tutti i problemi di compilazione vengono ora rilevati durante la fase di sviluppo anziché in fase di esecuzione. Consente inoltre di progettazione e il supporto IntelliSense per le funzionalità, ad esempio pagine master e controlli utente.

Fare clic qui per visualizzare una procedura dettagliata video di supporto della finestra di progettazione per controlli utente.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Aprire Video a schermo intero](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Quando un controllo utente viene rimosso da una pagina, il @Register direttiva rimane nel markup e deve essere rimosso manualmente per evitare errori del parser, se il controllo utente viene eliminato dal sito Web.


Un altro miglioramento nel modello di compilazione di Visual Studio è la funzionalità Pubblica sito Web. Poiché la funzionalità di pubblicazione consente di precompilare il sito Web, gli sviluppatori possono sfruttare le prestazioni di aggiunta di non dover compilare qualsiasi su richiesta. Inoltre consente di precompilare tutto il codice sorgente nell'App\_cartella del codice in una DLL in modo che nessun codice di origine deve essere distribuito.


![Finestra di dialogo Pubblica sito Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: finestra di dialogo Pubblica sito Web


> [!NOTE]
> Aspnet\_compile.exe utilità consente inoltre di precompilazione di un'applicazione Web ASP.NET. Tale strumento verrà descritta nel modulo 9.


Quando si pubblica un sito Web, i file precompilati vengono archiviati nella cartella Temporary ASP.NET Files come illustrato di seguito. I file con un *Compiled* estensione di file sono file XML che definiscono le dipendenze per le DLL specifiche. Tutti i controlli Web Form o un utente vengono compilati in DLL casuale che iniziano con *App\_Web\_*.

Se si lascia il *consentire l'aggiornamento del sito precompilato* casella di controllo è selezionata, markup all'interno dei controlli Web Form e l'utente non saranno pre-compilate in una DLL che consente di apportare modifiche dopo la distribuzione. Se si preferisce bloccare il markup in modo che non sono consentite modifiche al contenuto distribuito, deselezionare questa casella di controllo.

Il *Usa assembly con pagina singola e nomi fissi* casella di controllo consente di disabilitare la compilazione batch in modo che ogni pagina viene compilata in un assembly con nome predefinito. Lasciare deselezionata questa casella consente di sfruttare i vantaggi della compilazione batch.

Il *Enable sicuro su assembly precompilati* casella di controllo consente di nomi sicuri agli assembly precompilati.

> [!NOTE]
> In ASP.NET 1. x, assembly con nome sicuro deve essere installato nella Global Assembly Cache (GAC). In ASP.NET 2.0, non verrà richiesto di installare l'assembly con nome sicuro nella Global Assembly Cache.


![Un file di pre-compilate applicazioni ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: un file di pre-compilate applicazioni ASP.NET


> [!NOTE]
> Nell'applicazione, si è verificato alcun file Web. config. Se fossero stati, sarebbe stata chiamata *PrecompiledApp* dopo aver Pubblica sito Web del sito processo.


## <a name="improvements-in-deployment"></a>Miglioramenti apportati alla distribuzione

Come con Visual Studio 2002 e 2003, Visual Studio 2005 offre una funzionalità del progetto copia. Tuttavia, la funzionalità è stata beefed in Visual Studio 2005 ed è denominata Copia sito Web.

La finestra di dialogo Copia sito Web è suddiviso in un riquadro a sinistra e un riquadro di destra. Riquadro a sinistra viene chiamato il sito Web di origine e il riquadro di destra viene chiamato dal sito Web remoto. Una cosa che potrebbe confondere alcuni sviluppatori è che il sito visualizzato nel riquadro di destra non è necessariamente un sito remoto. Potrebbe trattarsi di un sito nel file system locale o nell'istanza locale di IIS. Inoltre, il sito visualizzato nel riquadro di sinistra non è necessariamente il sito Web di origine perché la finestra di dialogo consente di pubblicare il sito Web remoto *a* sito Web di origine.

Se si copia un progetto a un sito Web remoto, tale sito deve disporre di estensioni del Server è installato. In caso contrario, è necessario connettersi tramite FTP. D'altra parte, se si copia un progetto per l'istanza locale di IIS, estensioni del Server di FrontPage non sono necessari.

> [!NOTE]
> Se si tenta di creare un nuovo sito Web nell'istanza locale di IIS e sono installate le estensioni del Server di FrontPage 2002, si otterrà un messaggio di errore indicante che la creazione di siti Web non è supportata in un server SharePoint. In tal caso, è possibile installare le estensioni del Server di FrontPage 2000 o rimuovere le estensioni del Server di FrontPage.


Fare clic qui per una procedura dettagliata video della funzionalità di Copia sito Web.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Aprire Video a schermo intero](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Miglioramenti di debug

Esistono quattro importanti miglioramenti nel debug in Visual Studio 2005.

- Il debug in locale come amministratore non è possibile all'esterno della casella.
- L'attributo di Debug per l'elemento di compilazione è false per impostazione predefinita.
- Il programma di installazione e configurazione di debug remoto è più semplice rispetto a prima.
- È ora possibile eseguire il debug di un sito Web aperto tramite un percorso FTP.

## <a name="debugging-as-a-non-administrator"></a>Debug come Non amministratore

L'aggiunta di Server di sviluppo ASP.NET consente a utenti non amministratori di facilmente il debug di applicazioni ASP.NET subito. Durante il debug di un'applicazione ASP.NET in esecuzione nel file system locale, Visual Studio avvia il Server di sviluppo ASP.NET nel contesto dell'utente connesso. Tale utente può quindi eseguire il debug dell'applicazione senza configurazione aggiuntiva.

## <a name="debug-is-false-by-default"></a>Debug è impostata su False per impostazione predefinita

In ASP.NET 1. x, il *debug* attributo la *compilazione* elemento del file Web. config è stato impostato su *true* per impostazione predefinita. È sempre stato consiglia che gli sviluppatori di impostare questo attributo su *false* prima di distribuire un'applicazione nell'ambiente di produzione, ma la maggior parte degli sviluppatori non hanno una comprensione delle conseguenze di lasciare l'attributo di debug impostato su true, vengono semplicemente da sinistra come-è.

Il problema più grave a con l'attributo di debug impostato su true è che disabilita ASP.NETs modello di compilazione batch. Pertanto, ogni pagina viene compilata in una DLL separata. Se un sito Web dell'applicazione è costituito da migliaia di pagine (non parla di con qualsiasi mezzo), che significa diverse DLL di piccole dimensioni delle migliaia verrà creata da tale applicazione. Anche se queste DLL sono di piccole dimensioni, che non vengano caricati in una posizione particolare in memoria. Pertanto, provocare la frammentazione nella memoria di sistema e possono contribuire a OutOfMemoryException occorrenze.

In ASP.NET 2.0, l'attributo di debug è impostato su false per impostazione predefinita. Come è stato già illustrato, quando uno sviluppatore esegue il debug di un'applicazione ASP.NET in Visual Studio 2005, viene richiesto di aggiungere un file Web. config con il debug abilitato. Questa operazione comporta lo stessi svantaggi che erano presenti in ASP.NET 1. x, ma ora lo sviluppatore chiaramente viene avvisato che l'attributo deve essere reimpostato su false prima di spostare l'applicazione nell'ambiente di produzione.

## <a name="remote-debugging-setup-and-configuration"></a>Configurazione e installazione del debug remoto

In Visual Studio 2002/2003, il debug remoto si basava su Machine Debug Manager (mdm.exe) e il processo vs7jit.exe. Per questo motivo, la risoluzione dei problemi di debug remoti era spesso una casella nera per i clienti e spesso non è stato decisamente migliore per PSS.

Visual Studio 2005 rimuove la dipendenza dai processi di mdm.exe e vs7jit.exe. Al contrario, si utilizza il servizio Remote Debug Monitor (msvsmon.exe).

Il requisito per il debug in modalità remota in Visual Studio 2005 è piuttosto semplice. È necessario eseguire msvsmon.exe nel server remoto prima di debug. È possibile installare Remote Debug Monitor dal CD di Visual Studio oppure è possibile eseguire msvsmon.exe da una condivisione senza alcuna operazione di installazione in tutti i server Web.

Quando si esegue msvsmon.exe, è probabile che verificheranno problemi sulle porte bloccate per il debug remoto. Fortunatamente, è possibile sbloccare facilmente le porte da destra nella finestra di dialogo di avviso, come illustrato di seguito.


![Notifica di Windows Firewall blocca il debug remoto](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: è la notifica che Windows Firewall blocca il debug remoto


Se si hanno sbloccata le porte necessarie per il debug, si noterà Remote Debugging Monitor come illustrato di seguito. Da questa interfaccia, è possibile monitorare le connessioni e modificare le autorizzazioni di debug con facilità.


![Remote Debugging Monitor](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: Remote Debugging Monitor


È inoltre possibile eseguire il debug remoto di un'applicazione Web aperta tramite FTP. I passaggi sono identici a quelli precedentemente menzionate. Tuttavia, è necessario specificare un URL di base per l'esplorazione del progetto FTP, come descritto in precedenza in questo modulo.

## <a name="lab-2"></a>Laboratorio 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Debug remoto con Visual Studio 2005

Questa esercitazione verrà illustrati il debug remoto con Visual Studio 2005.

Fare clic qui per una procedura dettagliata video di questa esercitazione.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Aprire Video a schermo intero](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Questa esercitazione è necessario disporre di due computer, uno in esecuzione Visual Studio 2005 e altri in esecuzione IIS 5 o versione successiva.

1. Aprire Visual Studio 2005 e creare un nuovo sito Web nel server remoto.

> [!NOTE]
> È possibile creare il sito Web in un'istanza remota di IIS o tramite FTP.


1. Dal server Web remoto, individuare msvsmon.exe nel computer di sviluppo utilizzando un percorso UNC ed eseguirlo.  
 Il percorso predefinito per msvsmon.exe è \\server\c$ \Programmi\Microsoft Visual Studio 8 \ Common7\IDE\Remote Debugger\x86.
2. Se viene richiesto di sbloccare le porte per il debug remoto, è necessario farlo.
3. Dal computer di sviluppo, aprire il code-behind per Default.aspx e impostare un punto di interruzione nella pagina\_metodo Load.
4. Avviare il debug dal computer di sviluppo.

È necessario raggiungere il punto di interruzione, come previsto.

## <a name="aspnet-development-server"></a>server di sviluppo ASP.NET

Come weve già descritto, Visual Studio 2005 viene fornito con un server Web denominato Server di sviluppo ASP.NET. (Il Server di sviluppo ASP.NET è talvolta detta Cassini.) Il server Web è un modo pratico per individuare ed eseguire il debug di applicazioni Web in esecuzione nel file system.

Il Server di sviluppo ASP.NET è un server Web con restrizioni. Non consente le connessioni remote, non consente le richieste da qualsiasi utente diverso da quello che ha avviato il server Web. È che non abbia la possibilità di servire le pagine ASP. Vengono gestite solo le risorse ASP.NET e risorse HTML (incluse le immagini, file CSS e così via).

Il Server di sviluppo ASP.NET può essere avviato dalla riga di comando eseguendo il file di WebDev.WebServer.exe in c:\Windows\Microsoft.NET\Framework\v2.0. \*\*\*\*\*. Finestra di dialogo seguente mostra i parametri disponibili.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Nella figura 14**


> [!NOTE]
> Il Server di sviluppo ASP.NET non è supportato quando avviata in modo esplicito tramite la riga di comando.
