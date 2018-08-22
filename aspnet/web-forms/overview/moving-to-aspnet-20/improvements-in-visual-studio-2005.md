---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Miglioramenti in Visual Studio 2005 | Microsoft Docs
author: microsoft
description: Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web con un lungo elenco di miglioramenti ai progetti Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 60259ceb99de536410aa5f53db64fb2dca68bf66
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827067"
---
<a name="improvements-in-visual-studio-2005"></a>Miglioramenti in Visual Studio 2005
====================
da [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web con un lungo elenco di miglioramenti ai progetti Web.


Visual Studio 2005 fornisce agli sviluppatori di applicazioni Web con un lungo elenco di miglioramenti ai progetti Web. Nonostante la potenza offerta Visual Studio .NET 2002 e 2003, esistevano molte lamentele in modo che i progetti Web sono stati gestiti. Visual Studio 2005 aggiunge un numero significativo di nuove funzionalità per affrontare queste lamentele. Per coloro che preferiscono la modalità di gestione da parte di Visual Studio .NET 2003 la compilazione di applicazioni Web, vedere [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).

In questo modulo verranno trattate miglioramenti nella creazione del progetto Web, la gestione e sviluppo. In un modulo più avanti, verranno trattate miglioramenti per la compilazione di progetti Web e la loro distribuzione.

## <a name="frontpage-server-extensions"></a>Estensioni del Server di FrontPage

Visual Studio .NET 2002 e 2003 necessari estensioni del Server nella scheda per creare o compilare i progetti Web. Gli sviluppatori è stato possibile scegliere tra due modalità di accesso diversi (estensioni del Server o File modalità di accesso), le estensioni del Server di FrontPage entrambi usati per eseguire attività quali l'impostazione della radice dell'applicazione in IIS e così via.

Visual Studio 2005 consente di rimuovere l'affidamento sulle estensioni del Server di FrontPage per i progetti locali. Visual Studio 2005 consente di accedere a questo punto la metabase IIS direttamente anziché usare le estensioni del Server di FrontPage. Visual Studio 2005 aggiunge anche il supporto per FTP che consente l'accesso remoto progetto senza FrontPage Server Extensions.

Per gli sviluppatori che intendono usare le estensioni del Server di FrontPage nei propri progetti, l'opzione è ancora disponibile. Tuttavia, in base al feedback sicuro dalla community degli sviluppatori ASP.NET, non è un requisito.

> [!NOTE]
> Estensioni del Server sono ancora necessari per la creazione del progetto remota, apertura e così via.


## <a name="aspnet-development-server"></a>server di sviluppo ASP.NET

Visual Studio 2005 viene fornito con un nuovo server Web denominato Server di sviluppo ASP.NET. (Il server Web è stato precedentemente noto come Cassini).

Esistono diversi vantaggi di ASP.NET Development Server.

- A questo punto è possibile che utenti non amministratori sviluppare ed eseguire il debug con un server Web.
- Il Server di sviluppo ASP.NET esegue il mapping in modo dinamico le directory virtuali in qualsiasi posizione nel file system che consente per i percorsi di progetto flessibili.
- Gli utenti in Windows XP Professional che già utilizzano IIS a questo punto saranno possibile creare nuove applicazioni Web che non avrà effetto sulla struttura di file o una cartella della loro sito Web predefinito in IIS.

Per sfruttare i vantaggi di ASP.NET Development Server è necessaria alcuna configurazione speciale. Quando un progetto Web ospitata nel file system viene eseguito il debug o esplorato, Visual Studio 2005 avvierà automaticamente un'istanza del Server di sviluppo ASP.NET su una porta casuale per soddisfare la richiesta.

Verranno trattate altre informazioni sul Server di sviluppo ASP.NET più avanti in questo modulo.

## <a name="improved-file-management"></a>Gestione di File migliorata

In Visual Studio 2002 e 2003, un file di progetto (con estensione vbproj per VB.NET) e file con estensione csproj per c# archiviate informazioni su tutti i file nell'applicazione Web. La visualizzazione di Esplora soluzioni si basa sulle informazioni del file nel file di progetto. Per questo motivo, Esplora soluzioni visualizzerebbe spesso informazioni non corrette nei casi in cui sono stati usati editor esterni. Visual Studio 2002 e 2003 sarebbe spesso sovrascrivere le modifiche ai file o non visualizzare la versione più recente dei file.

Visual Studio 2005 è assente il file di progetto. Legge invece le informazioni di file e cartelle direttamente dal disco, risultante in una visualizzazione accurata dei file nel progetto. Poiché la cartella dei riferimenti in Visual Studio 2002 e 2003 non rappresenta una cartella effettiva nell'applicazione Web, Visual Studio 2005 rimuove anche la cartella dei riferimenti da Esplora soluzioni. Per accedere ai riferimenti per il progetto in Visual Studio 2005, è consigliabile usare le pagine delle proprietà per il progetto.

## <a name="creating-web-projects"></a>Creazione di progetti Web

Gli sviluppatori Web hanno molte nuove opzioni disponibili per la creazione del progetto in Visual Studio 2005. Siti Web è ora possibile creare un punto qualsiasi nel file system e quindi eseguire il debug o esplorare usando il nuovo Server di sviluppo ASP.NET. Gli sviluppatori possono anche creare nuovi siti Web tramite FTP.

Fare clic qui per visualizzare un video della procedura dettagliata di creazione di progetti Web in Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Aprirlo Video a schermo intero](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Progetti di file System

Come illustrato nella procedura dettagliata video, è possibile scegliere di creare siti Web nel file system nel computer locale o in una posizione remota tramite una condivisione file. Siti Web che vengono creati nel file system possono essere sfogliati e sottoposti a debug utilizzando il Server di sviluppo ASP.NET.

> [!NOTE]
> Il Server di sviluppo ASP.NET potrebbe causare confusione per i clienti. Se un progetto Web creato nel file system nella struttura di directory IISs (ad esempio c: inetpub//wwwroot), verrà ancora visitato il sito Web tramite il Server di sviluppo ASP.NET quando avviato dall'interno di Visual Studio 2005. Di conseguenza, le configurazioni di IIS (ad esempio i metodi di autenticazione) non applicabile.


Il progetto web predefinito rimuove anche notevolmente il sovraccarico per include solo una pagina default. aspx, file default.cs e una cartella App/_Data. Il file Web. config e cartelle speciali (ad esempio code) vengono aggiunti all'occorrenza. Il progetto web include solo i file e cartelle che è necessario.

### <a name="http-projects"></a>Progetti HTTP

I progetti HTTP possono essere progetti creati in un sito Web IIS locale o in un sito Web remoto. Il percorso del progetto predefinita `http://localhost`. Se si fa clic sul pulsante Sfoglia, sono disponibili due opzioni di HTTP: IIS locale e sito remoto. La differenza principale in queste due opzioni è il metodo in cui le informazioni sul sito web viene visualizzati nella finestra di dialogo Scegli percorso e nel modo in cui i file vengono copiati nel server Web.

L'opzione IIS locale legge le informazioni del sito dalla metabase nel computer locale e i file vengono copiati usando il file system. L'opzione sito remoto utilizza estensioni del Server e le informazioni del sito e i file vengono copiati tramite HTTP e le chiamate RPC di estensioni del Server di FrontPage.

> [!NOTE]
> Il file vs###/_tmp.htm get/_aspx/_ver.aspx non consentono di determinare le informazioni sulla versione.


L'opzione HTTP predefinito è IIS locale. Questa opzione legge la Metabase di IIS per determinare quali siti sono disponibili e il percorso in cui creare il contenuto. È possibile selezionare una cartella diversa o la directory virtuale, selezionarlo nella visualizzazione albero. È possibile anche creare una nuova directory virtuale, contrassegnare le cartelle come applicazioni, nonché eliminare le directory virtuali esistenti dalla finestra di dialogo.


![La finestra di dialogo posizione Scegli](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: la finestra di dialogo posizione Scegli


A differenza nelle versioni precedenti di Visual Studio, se si seleziona il **utilizza Secure Sockets Layer** casella di controllo e il certificato SSL non corrisponde all'URL a cui si sta esplorando, verrà visualizzata una finestra di dialogo Avviso di sicurezza che chiede se preferisci come procedere. Usa Visual Studio .NET 2003, se il certificato non è una corrispondenza, la creazione del progetto è avrebbe esito negativo.


![Certificato SSL relativa a avvisi di sicurezza](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: avviso di sicurezza riguardanti il certificato SSL


### <a name="note-on-host-headers"></a>Nota sulle intestazioni Host

Se si sta creando un'applicazione Web in un sito associato a uno specifico indirizzo IP, è necessario assicurarsi che sia configurata in un'intestazione host. In caso contrario, Visual Studio verrà creato il sito all'indirizzo `http://localhost`, ma l'indirizzo IP non verrà risolti correttamente quando il sito è esplorare o eseguire il debug dall'interno dell'IDE.

Se si seleziona l'opzione sito remoto, la finestra di dialogo Cambia per consentire di immettere l'URL di destinazione per il nuovo sito Web. Questo URL deve essere in un server che dispone di estensioni del Server abilitata. Se si desidera lavorare con i server Web locale usando le estensioni del Server di FrontPage, è possibile usare l'opzione sito remoto e specificare un URL locale.


![Creazione di un sito Web in un Server remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: creazione di un sito Web in un Server remoto


Quando si crea un'applicazione in un sito remoto tramite SSL, se il certificato SSL non corrispondono, la finestra di dialogo di conferma è leggermente diverso dalla finestra di dialogo visualizzata quando si usa l'opzione IIS locale.


![Avviso di sicurezza sito remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: avviso di sicurezza sito remoto


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 viene introdotta l'opzione per creare siti Web tramite FTP. Quando si usa questa opzione, l'IDE crea i file in locale nella cartella temp dell'utente e quindi Usa FTP per spostare i file nel percorso FTP.

> [!NOTE]
> Il percorso della cartella temp è c: / Documents and Settings /&lt;utente&gt;/locale impostazioni/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nome dell'applicazione&gt;


Quando si usa l'opzione di FTP, verrà visualizzata una finestra di dialogo Scegli percorso. Immettere le informazioni di connessione FTP necessarie in questa finestra di dialogo come illustrato di seguito.


![La finestra di dialogo di percorso per il servizio FTP Scegli](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: la finestra di dialogo di percorso per il servizio FTP Scegli


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratorio: Installazione FTP del sito e creare un progetto

I passaggi seguenti configurano il sito FTP in modo che un utente dispone di un percorso a cui solo è possibile caricare tramite FTP.

### <a name="install-the-ftp-service"></a>Installare il servizio FTP

1. Aprire Installazione applicazioni, selezionare Installazione componenti di Windows
2. Selezionare Internet Information Services (Server applicazioni in Windows 2003) e fare clic su **dettagli**.
3. Controllare **servizio FTP File Transfer Protocol ()** e fare clic su **OK**.
4. Fare clic su **successivo** per installare il servizio FTP.

### <a name="create-a-new-folder-for-content"></a>Creare una nuova cartella per il contenuto

1. In Windows Explorer, creare una nuova cartella denominata **User1** all'interno di c: / inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurare le cartelle e le autorizzazioni sulle cartelle.

1. Aprire lo snap-in Internet Information Services da strumenti di amministrazione. A questo punto si avrà una cartella siti FTP nel nodo del nome del computer.
2. Espandere **siti FTP**.
3. Fare doppio clic il **predefiniti sito FTP**, selezionare **New**, quindi **Directory virtuale**, quindi fare clic su **Avanti**.
4. Immettere **User1** il nome della directory virtuale e fare clic su **successivo**.
5. Immettere **c: / inetpub/wwwroot/User1** per il percorso e fare clic su **successivo**.
6. Fare clic su **successivo** e quindi **fine** per completare la procedura guidata.
7. Fare doppio clic il **User1** directory virtuale nel sito FTP predefinito e selezionare **proprietà**.
8. Verificare i **scrivere** casella di controllo e fare clic su **OK** per chiudere la finestra di dialogo.
9. Fare doppio clic su **predefiniti sito FTP** e selezionare **proprietà**.
10. Nel **gli account di sicurezza** scheda, deselezionare **Consenti connessioni anonime**.
11. Fare clic su **Sì** nella finestra di dialogo che chiede se si desidera continuare.
12. Fare clic su **OK** per chiudere la finestra di dialogo.
13. Espandere la **sito Web predefinito** sotto il **siti Web** nodo.
14. Fare doppio clic il **User1** directory e selezionare **proprietà**
15. Nel **Application Settings** fare clic su **crea** per contrassegnare la cartella di un'applicazione.
16. Fare clic su **OK** per chiudere la finestra di dialogo.
17. Chiudere lo snap-in Internet Information Services.

### <a name="create-web-project"></a>Creare il progetto web

1. Aprire Visual Studio 2005.
2. Dal **File** dal menu **nuovo sito Web**.
3. Nel **ubicazione** elenco a discesa, seleziona **FTP**.
4. Fare clic su **Sfoglia**.
5. Immettere **localhost** nel **Server** nella casella di testo.
6. Immettere **User1** nella casella di testo di Directory.
7. Fare clic su **aperto**. Il percorso del FTP verrà immesso nella finestra di dialogo Nuovo sito Web.
8. Fare clic su **OK**.
9. Deselezionare l'opzione **accesso anonimo** nella finestra di dialogo connessione FTP, immettere le credenziali e fare clic su **OK**.
10. Che cos'è l'URL per il progetto? (Verrà visualizzato l'URL per il progetto in Esplora soluzioni).
11. Dal **compilare** dal menu **Compila sito Web** oppure **Compila soluzione**.
12. Fare clic su default. aspx in Esplora soluzioni e selezionare **Visualizza nel Browser**.
13. Nella finestra di dialogo URL del sito Web necessario, immettere `http://localhost/user1` per l'URL e fare clic su **OK**.

> [!NOTE]
> Se si verifica un errore che indicano l'impossibilità di caricare il tipo /_Default, assicurarsi che si siano in esecuzione ASP.NET 2.0 nel sito Web e non una versione precedente. È possibile farlo dalla scheda ASP.NET in Internet Information Services.


## <a name="opening-web-projects"></a>Apertura di progetti Web

Apertura di progetti Web è simile alla creazione di progetti. Le seguenti sezioni sottolineare aree per tenere d'occhio out per mentre si lavora all'interno dell'IDE. Viene anche descritto come lavorare con progetti Web tramite HTTP e FTP.

Per aprire un progetto Web, selezionare Apri sito Web dal menu File. Verrà richiesto con la finestra di dialogo Scegli percorso stesso illustrato in precedenza e si hanno le stesse opzioni disponibili quattro: File System, IIS locale, FTP e sito remoto.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>File system

Come indicato in precedenza in questo modulo, Visual Studio non usa più un file di progetto. Pertanto, se si sceglie di aprire un sito Web dal file system, in realtà hanno la possibilità di scegliere una cartella che si desidera, anche se la cartella selezionata non è stata creata come progetto Web inizialmente in Visual Studio. Ad esempio, è possibile scegliere di aprire la cartella documenti come sito Web e Visual Studio Fortunatamente aprirla e visualizzare i file, come illustrato di seguito.


![La cartella documenti aperti come un sito Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *la cartella documenti* aperto come un sito Web


Poiché Visual Studio crea solo altri file e cartelle se necessario, nessun file o cartelle aggiuntivi vengono aggiunti nel percorso che è aprire. Un effetto collaterale di questa architettura è che impedisce la nidificazione di siti Web nel file system. Ad esempio, si consideri la seguente struttura di directory.

Progetto Web presso c: /

Un altro progetto web in c: / MyWebSite/Nested

Quando si apre il sito Web all'indirizzo c: / MyWebSite, la cartella Nested verrà visualizzato come una sottocartella di quell'applicazione.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Quando si apre i siti Web tramite HTTP, le impostazioni vengono lette dalla metabase di IIS (IIS locale) o tramite estensioni del Server (sito remoto). Se sono presenti applicazioni web annidati, questi vengono visualizzati anche con un'icona che li identifica come un'applicazione. Se si ha familiarità con l'utilizzo di applicazioni web di FrontPage, il comportamento in Visual Studio 2005 è simile.

Anche se Visual Studio verrà visualizzata un'icona per le applicazioni nidificate sotto l'applicazione che è attualmente aperto all'interno dell'IDE, non consentirà di è possibile espanderle per visualizzare il relativo contenuto. È possibile, tuttavia, fare doppio clic su di essi per aprirli. Quando esegue questa operazione, si verrà visualizzata una finestra di dialogo che richiede di aprire l'applicazione web (e sostituire la soluzione attualmente aperta) o aggiungere l'applicazione Web alla soluzione corrente.


![Fare doppio clic su un'icona applicazione annidata viene visualizzata questa finestra di dialogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: facendo doppio clic su un'icona applicazione annidata viene visualizzata questa finestra di dialogo


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Sito FTP

Quando si apre un sito tramite FTP, i file vengono tutti copiati in locale della cartella temp. Il percorso completo per il percorso di archiviazione locale viene visualizzato nel riquadro proprietà per il progetto e viene creato usando il formato seguente.

C: / Documents and Settings /&lt;utente&gt;/locale impostazioni/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nome dell'applicazione&gt;

Quando si Usa FTP, Visual Studio sarà necessario specificare l'URL di base per il progetto in modo che è possibile visualizzarlo come illustrato di seguito. Se non si specifica un URL di base, Visual Studio chiederà perché la prima volta che si prova a esplorare una pagina nel sito Web.


![Specificare un URL di Base per i siti FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: specificare un URL di Base per i siti FTP


## <a name="improvements-in-compilation"></a>Miglioramenti nella compilazione

Utilizzo delle applicazioni Web in Visual Studio 2005 è significativamente più veloce rispetto alle versioni precedenti. Ciò è dovuto in nessuna parte di piccole dimensioni per le modifiche nell'architettura di compilazione.

In Visual Studio 2002 e 2003, le applicazioni Web sono state compilate in un unico assembly primario che si trovano nella cartella /bin. In Visual Studio 2005, è stata aggiunta una cartella code. Le classi e altro codice non di interfaccia utente vengono aggiunti alla cartella code. Quando Visual Studio compila il progetto, tutti i file nella cartella code vengono compilati in un unico file App/_Code.dll. Il risultato di questa modifica è che le compilazioni successive sono molto più veloce nelle versioni precedenti.

> [!NOTE]
> L'utilità della riga di comando MSBuild è anche utilizzabile per compilare applicazioni Web ASP.NET. Tale strumento verrà trattato nel modulo 9.


Un altro miglioramento di compilazione è la nuova opzione pagina compilazione dal menu Compila. Questa funzionalità consente agli sviluppatori di ricompilare solo la pagina corrente (insieme a, naturalmente, e le dipendenze) in modo che le modifiche possono essere compilate in modo più rapido. Perché c# non offre la compilazione in background per scopi di aggiornamento e così via, IntelliSense, questi ultimi potranno avvalersi estremamente questa funzionalità poiché consentirà di IntelliSense aggiornare rapidamente ricompilando semplicemente una singola pagina.

Le proprietà di compilazione per un progetto consentono di configurare il tipo di compilazione che si verifica prima che venga eseguita la pagina di avvio. Gli sviluppatori possono scegliere di compilare solo la pagina corrente in modo che Visual Studio è possibile avviare il debug di applicazioni più rapidamente dopo le modifiche al codice.


![L'azione di avvio pagina compilazione](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: l'azione di avvio pagina compilazione


Un altro miglioramento ideale per l'architettura ASP.NET e Visual Studio si trova nell'area di modifica e continuazione. In Visual Studio 2005, gli sviluppatori possono avviare il debug di un progetto e apportare modifiche al codice del progetto senza scollegare il debugger. In effetti, è possibile letteralmente avviare il debug di un progetto, aggiungere una nuova classe, aggiungere codice a tale classe, aggiungere codice alla pagina che consente di creare una nuova istanza della classe ed eseguire un metodo della classe, senza richiedere la disconnessione del debugger. L'esecuzione del codice nuovo è letteralmente semplice come l'aggiornamento del browser.

Fare clic qui per visualizzare una procedura dettagliata video della modifica e continuazione funzionalità in Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Aprirlo Video a schermo intero](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Robusto sistema di modifica e continuazione di funzionalità in ASP.NET 2.0 e Visual Studio 2005 è dovuto a una modifica dell'architettura per le applicazioni ASP.NET. In ASP.NET 1.x, le applicazioni create in Visual Studio 2002/2003 sono stati compilati in un assembly principale che è stato archiviato nella cartella /bin. Tutte le classi, le pagine, e così via, per l'applicazione sono state compilate in una DLL. In fase di runtime ASP.NET sarebbe quindi compilare tutti i controlli, markup e codice ASP.NET all'interno delle pagine e copiare tali DLL nella cartella temporanea ASP.NET.

In Visual Studio 2005 con ASP.NET 2.0, i modelli di due compilazione descrivono sopra, uno per Visual Studio e uno per ASP.NET in fase di esecuzione sono state unite in un modello comune di compilazione. Ciò significa che tutti i problemi di compilazione vengono ora rilevati durante la fase di sviluppo anziché in fase di esecuzione. Consente inoltre di progettazione e il supporto IntelliSense per funzionalità quali controlli utente e le pagine master.

Fare clic qui per visualizzare una procedura dettagliata video di supporto di progettazione per i controlli utente.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Aprirlo Video a schermo intero](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Quando un controllo utente viene rimosso da una pagina, il @Register direttiva rimane nel markup e deve essere rimossa manualmente per evitare errori del parser, se il controllo utente viene eliminato dal sito Web.


Un altro miglioramento nel modello di compilazione di Visual Studio è la funzionalità Pubblica sito Web. Poiché la funzionalità di pubblicazione consente di precompilare il sito Web, gli sviluppatori possono sfruttare le prestazioni di aggiunta di non dover compilare qualsiasi elemento su richiesta. Anche consente di precompilare tutto il codice sorgente nella cartella code in una DLL in modo che nessun codice sorgente è necessario distribuire.


![Finestra di dialogo Pubblica sito Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: finestra di dialogo Pubblica sito Web


> [!NOTE]
> L'utilità aspnet/_compile.exe può anche essere usata per precompilare un'applicazione Web ASP.NET. Tale strumento verrà trattato nel modulo 9.


Quando si pubblica un sito Web, i file precompilati vengono archiviati nella cartella di file ASP.NET temporanei come illustrato di seguito. I file con un *Compiled* estensione di file sono file XML che definiscono le dipendenze per le DLL particolare. Tutti i controlli Web Form o un utente vengono compilati in DLL casuali che iniziano con *App /_Web /_*.

Se si lascia il *sito precompilato deve essere aggiornabile* casella di controllo selezionata, markup all'interno di controlli Web Form e l'utente non saranno pre-compilate in un file DLL ed è possibile apportare le modifiche apportate dopo la distribuzione. Se si preferisce bloccare il markup in modo che non sono consentite modifiche al contenuto distribuito, deselezionare questa casella di controllo.

Il *Usa assembly con pagina singola e nomi fissi* casella di controllo consente di disabilitare la compilazione in batch in modo che ogni pagina viene compilata in un assembly con nome predefinito. Lasciare deselezionata questa casella consente di sfruttare i vantaggi della compilazione batch.

Il *Enable sicuro su assembly precompilati o meno* casella di controllo consente di assegnare un nome sicuro di assembly precompilati.

> [!NOTE]
> In ASP.NET 1.x, assembly con nome sicuro dovevano essere installato nella Global Assembly Cache (GAC). In ASP.NET 2.0, non viene richiesto di installare gli assembly con nome sicuro nella Global Assembly Cache.


![Un file di pre-compilate applicazioni ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: un file di pre-compilate applicazioni ASP.NET


> [!NOTE]
> Nell'applicazione precedente, si è verificato alcun file Web. config. Se fossero stati, sarebbe stata chiamata *PrecompiledApp* Pubblica sito Web dopo il processo del sito.


## <a name="improvements-in-deployment"></a>Miglioramenti apportati alla distribuzione

Come con Visual Studio 2002 e 2003, Visual Studio 2005 offre una funzionalità di copia del progetto. Tuttavia, la funzionalità è stata beefed in Visual Studio 2005 e viene ora chiamata Copia sito Web.

La finestra di dialogo Copia sito Web viene suddivisa in un riquadro a sinistra e un riquadro di destra. Riquadro a sinistra è denominato sito Web di origine e il frame destro viene chiamato dal sito Web remoto. Una cosa che potrebbe confondere alcuni sviluppatori è che il sito visualizzato nel riquadro di destra non è necessariamente un sito remoto. Potrebbe trattarsi di un sito nel file system locale o nell'istanza locale di IIS. Inoltre, il sito visualizzato nel riquadro di sinistra non è necessariamente il sito Web di origine perché la finestra di dialogo consente di pubblicare i dati dal sito Web remoto *a* sito Web di origine.

Se si copia un progetto a un sito Web remoto, tale sito deve avere le estensioni di Server di FrontPage installate su di esso. In caso contrario, è necessario connettersi tramite FTP. D'altra parte, se si copia un progetto nell'istanza locale di IIS, le estensioni del Server di FrontPage non sono necessari.

> [!NOTE]
> Se si prova a creare un nuovo sito Web dell'istanza locale di IIS e installate le estensioni del Server di FrontPage 2002, si otterrà un messaggio di errore che informa che la creazione di siti Web non è supportata in un server SharePoint. In tal caso, è possibile scegliere di installare le estensioni del Server di FrontPage 2000 o la rimozione di estensioni del Server.


Fare clic qui per una procedura dettagliata video della funzionalità di Copia sito Web.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Aprirlo Video a schermo intero](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Miglioramenti di debug

Esistono quattro importanti miglioramenti nel debug in Visual Studio 2005.

- Debug in locale come un utente non amministratore è possibile impostazione predefinita.
- L'attributo di Debug per l'elemento di compilazione a questo punto è false per impostazione predefinita.
- Il programma di installazione e configurazione di debug remoto è più semplice rispetto a prima.
- È ora possibile eseguire il debug di un sito Web aperto mediante un percorso FTP.

## <a name="debugging-as-a-non-administrator"></a>Debug come Non amministratore

L'aggiunta di Server di sviluppo ASP.NET consente a utenti non amministratori di eseguire facilmente il debug di applicazioni ASP.NET immediatamente la. Quando viene eseguito il debug di un'applicazione ASP.NET in esecuzione nel file system locale, Visual Studio avvia il Server di sviluppo ASP.NET nel contesto dell'utente connesso. Tale utente può quindi eseguire il debug di tale applicazione senza alcuna configurazione aggiuntiva.

## <a name="debug-is-false-by-default"></a>Debug è impostata su False per impostazione predefinita

In ASP.NET 1.x, la *debug* attributo il *compilazione* elemento del file Web. config è stato impostato su *true* per impostazione predefinita. È sempre stato consigliabile che gli sviluppatori di impostano questo attributo su *false* prima di distribuire un'applicazione nell'ambiente di produzione, tuttavia, poiché la maggior parte degli sviluppatori non aiutino a comprendere appieno le conseguenze di lasciare l'attributo di debug impostato su true, vengono semplicemente da sinistra come-è.

Il problema più grave dalla presenza dell'attributo di debug impostato su true è che lo disabilita ASP.NETs il modello di compilazione batch. Pertanto, ogni pagina viene compilato in una DLL separata. Se un sito Web dell'applicazione è costituita da migliaia di pagine (non mai raggiunti prima di qualsiasi mezzo), che significa che diverse DLL di piccole dimensioni delle migliaia verrà creata da tale applicazione. Sebbene queste DLL sono di piccole dimensioni, non sono caricati in qualsiasi percorso specifico nella memoria. Pertanto, provocare la frammentazione nella memoria di sistema e può contribuire a occorrenze OutOfMemoryException.

In ASP.NET 2.0, l'attributo di debug è impostata su false per impostazione predefinita. Come si è già visto, quando uno sviluppatore esegue il debug di un'applicazione ASP.NET in Visual Studio 2005, cui viene richiesto di aggiungere un file Web. config con il debug abilitato. Questa operazione comporta lo stessi svantaggi che erano presenti in ASP.NET 1.x, ma ora lo sviluppatore è chiaramente un avviso che l'attributo deve essere reimpostato su false prima di spostare l'applicazione nell'ambiente di produzione.

## <a name="remote-debugging-setup-and-configuration"></a>Configurazione e installazione del debug remoto

In Visual Studio 2002/2003, debug remoto si basava sulla gestione Debug del computer (mdm.exe) e il processo vs7jit.exe. Per questo motivo, la risoluzione dei problemi di debug remoti era spesso una scatola nera per i clienti e spesso non era molto migliore per PSS.

Visual Studio 2005 consente di rimuovere l'affidamento i processi mdm.exe e vs7jit.exe. A questo punto Usa invece il servizio Remote Debug Monitor (msvsmon.exe).

Il requisito per il debug in modalità remota in Visual Studio 2005 è piuttosto semplice. È necessario eseguire msvsmon.exe nel server remoto prima del debug. È possibile installare Remote Debug Monitor dal CD di Visual Studio oppure è possibile eseguire semplicemente msvsmon.exe da una condivisione senza installare alcun componente affatto sul server Web.

Quando si esegue msvsmon.exe, è probabile che trovare da ridire sulle porte bloccate per il debug remoto. Fortunatamente, è possibile sbloccare con facilità le porte da destra nella finestra di dialogo di avviso, come illustrato di seguito.


![Notifica di Windows Firewall blocca il debug remoto](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: notifica che il Firewall Windows è bloccando il debug remoto


Se si hanno sbloccata porte necessarie per eseguire il debug, si noterà Remote Debugging Monitor come illustrato di seguito. Da questa interfaccia, è possibile monitorare le connessioni e modificare le autorizzazioni di debug con facilità.


![Remote Debugging Monitor](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: Remote Debugging Monitor


È anche possibile eseguire il debug remoto di un'applicazione Web aperta tramite FTP. I passaggi sono identici a quelli illustrati in precedenza. Tuttavia, è necessario specificare un URL di base per l'esplorazione del progetto FTP, come descritto in precedenza in questo modulo.

## <a name="lab-2"></a>Laboratorio 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Debug remoto con Visual Studio 2005

Questa esercitazione illustra il debug remoto con Visual Studio 2005.

Fare clic qui per una procedura dettagliata video di questa esercitazione.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Aprirlo Video a schermo intero](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Questo laboratorio è necessario disporre di due computer, uno in esecuzione Visual Studio 2005 e altro in esecuzione IIS 5 o versione successiva.

1. Aprire Visual Studio 2005 e creare un nuovo sito Web nel server remoto.

> [!NOTE]
> È possibile creare il sito Web in un'istanza remota di IIS o tramite FTP.


1. Dal server Web remoto, individuare msvsmon.exe nel computer di sviluppo utilizzando un percorso UNC ed eseguirlo.  
 Il percorso predefinito per msvsmon.exe è //server/c$/Program file/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Se viene richiesto di sbloccare le porte per il debug remoto, eseguire questa operazione.
3. Dal computer di sviluppo, aprire il code-behind per default. aspx e impostare un punto di interruzione nel metodo di pagina/bi_lanciamento.
4. Avviare il debug dal computer di sviluppo.

Si raggiungerà il punto di interruzione nel modo previsto.

## <a name="aspnet-development-server"></a>server di sviluppo ASP.NET

Come già illustrato weve, Visual Studio 2005 viene fornito con un server Web denominato Server di sviluppo ASP.NET. (Il Server di sviluppo ASP.NET è talvolta detta Cassini.) Il server Web è un modo pratico per esplorare ed eseguire il debug di applicazioni Web in esecuzione nel file system.

Il Server di sviluppo ASP.NET è un server Web con restrizioni. Non consente le connessioni remote, non consente le richieste da qualsiasi utente diverso da quello che ha avviato il server Web. Inoltre non è la capacità di servire le pagine ASP. Vengono gestite solo le risorse ASP.NET e le risorse HTML (incluse le immagini, file CSS e così via).

Il Server di sviluppo ASP.NET può essere avviato tramite riga di comando eseguendo il file WebDev.WebServer.exe in c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. Finestra di dialogo seguente consente di visualizzare i parametri disponibili.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**


> [!NOTE]
> Il Server di sviluppo ASP.NET non è supportato quando avviata in modo esplicito tramite la riga di comando.
