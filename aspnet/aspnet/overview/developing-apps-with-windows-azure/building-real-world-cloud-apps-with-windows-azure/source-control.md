---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Controllo (creazione di App Cloud funzionanti con Azure) del codice sorgente | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: aspnetcontent
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 045dc654057802be4ad96f5ecc3ed6c3d7a1ccb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823313"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Controllo del codice sorgente (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


Controllo del codice sorgente è essenziale per tutti i progetti di sviluppo cloud, non solo gli ambienti di team. Non sarebbe pensi di modifica del codice sorgente o di un documento di Word senza una funzione di annullamento e i backup automatici e controllo del codice sorgente ti offre le funzioni a un livello di progetto in cui è possibile risparmiare ancora più tempo quando si verifica un problema. Con servizi cloud di controllo origine, non è più necessario preoccuparsi di configurazione complesse ed è possibile usare Visual Studio Online gratuito per un massimo di 5 utenti controllo del codice sorgente.

La prima parte di questo capitolo illustra tre principali procedure consigliate da tenere presenti:

- [Considerare gli script di automazione come codice sorgente](#scripts) e versione essi con il codice dell'applicazione.
- [Non controllare mai nei segreti](#secrets) (dati sensibili, ad esempio le credenziali) in un repository del codice sorgente.
- [Impostare i rami di origine](#devops) per abilitare il flusso di lavoro DevOps.

Il resto del capitolo offre alcuni esempi di implementazione di questi modelli in Visual Studio, Azure e Visual Studio Online:

- [Aggiungere script al controllo del codice sorgente in Visual Studio](#vsscripts)
- [Dati sensibili di Store in Azure](#appsettings)
- [Usare Git in Visual Studio e Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Considerare gli script di automazione come codice sorgente

Quando si lavora in un progetto cloud che si desidera modificare frequentemente le cose e si desidera essere in grado di reagire rapidamente ai problemi segnalati dai clienti. Rispondere rapidamente prevede l'uso di script di automazione, come spiegato nel [automatizzare tutto](automate-everything.md) capitolo. Tutti gli script utilizzabili per creare l'ambiente, distribuirvi, scalabilità e così via, devono essere sincronizzati con il codice sorgente dell'applicazione.

Per mantenere sincronizzati con codice di script, archiviarle nel sistema di controllo sorgente. Quindi quando si avvia il rollback delle modifiche o rendere una correzione rapida per il codice di produzione è diverso dal codice di sviluppo, non devi perdere tempo al tentativo di rilevare le impostazioni che sono stati modificati o i membri del team dispongono di copie della versione che necessaria. Si è certi che gli script che necessari vengono sincronizzati con la base di codice che ti servono per, e si è certi che tutti i membri del team lavora con gli stessi script. Se si desidera automatizzare il test e la distribuzione di un aggiornamento rapido in produzione o sviluppo delle nuove funzionalità, sarà necessario lo script giusto per il codice che deve essere aggiornato.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Non si collegano i segreti

Un repository del codice sorgente è in genere accessibili alle persone troppi appositamente da un luogo sicuro in modo appropriato per i dati sensibili, ad esempio le password. Se gli script si basano su segreti, ad esempio password, tali impostazioni vengono parametrizzati in modo che non vengono salvati nel codice sorgente e archiviare i segreti in un'altra posizione.

Ad esempio, Azure consente che scaricare i file che contengono impostazioni di pubblicazione per automatizzare la creazione di profili di pubblicazione. Questi file contengono nomi utente e password autorizzati a gestire i servizi di Azure. Se si usa questo metodo per creare profili di pubblicazione e se si archivia questi file al controllo del codice sorgente, chiunque abbia accesso al repository può visualizzare i nomi utente e password. È possibile archiviare in modo sicuro la password nel profilo di pubblicazione stesso perché è crittografato e si trova in una *. pubxml* file che, per impostazione predefinita, non è incluso nel controllo del codice sorgente.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Rami di origine di struttura per facilitare del flusso di lavoro di DevOps

Modalità di implementazione di rami nel repository influisce sulla capacità sia sviluppare nuove funzionalità e risolvere i problemi nell'ambiente di produzione. Ecco un modello ridimensionati i team utilizzano i numerosi Media:

![Struttura di rami di origine](source-control/_static/image1.png)

Il ramo master corrisponde sempre al codice in produzione. I rami sotto master corrispondono a varie fasi nel ciclo di vita di sviluppo. Il ramo di sviluppo è in cui si implementano nuove funzionalità. Per un team di piccole dimensioni potrebbe semplicemente essere master e lo sviluppo, ma è spesso consigliabile che gli utenti dispongono di un ramo di staging tra sviluppo e master. È possibile utilizzare gestione temporanea per il test di integrazione finale prima che un aggiornamento viene spostato nell'ambiente di produzione.

Per i team di grandi dimensioni possono esistere rami separati per ogni nuova funzionalità; per un team più piccolo potrebbe essere everyone archiviando al ramo development.

Se si dispone di un ramo per ogni funzionalità, quando la funzionalità A è pronto è merge le modifiche del codice sorgente backup nello sviluppo ramo e verso il basso in altri rami funzionalità. Questo codice sorgente unione processo può richiedere molto tempo e per evitare tale lavoro garantendo nel contempo le funzionalità separata, alcuni team implementare alternativa denominata *[feature Toggle](http://en.wikipedia.org/wiki/Feature_toggle)* (noto anche come *flag funzionalità*). Ciò significa tutto il codice per tutte le funzionalità nel ramo stesso, ma è abilitare o disabilitare ogni funzionalità tramite gli switch nel codice. Si supponga, ad esempio, di funzionalità A un nuovo campo per Fix It attività delle app e funzionalità B aggiunge funzionalità di memorizzazione nella cache. Il codice per entrambe le funzionalità può essere nel ramo di sviluppo, ma la visualizzazione dell'app verranno solo il nuovo campo quando una variabile è impostata su true che userà solo la memorizzazione nella cache quando una variabile diversa è impostata su true. Se una funzionalità non è pronto da alzare di livello, ma la funzione B è pronto, è possibile alzare di livello tutto il codice nell'ambiente di produzione con l'opzione della funzionalità A disattivare e attivare la funzione B. Sarà possibile completare una funzionalità e alzato di livello in un secondo momento, tutto senza alcuna unione di codice sorgente.

Se si utilizza rami o gli elementi Toggle per le funzionalità, una struttura ramificata simile al seguente consente di far passare il codice dallo sviluppo alla produzione in modo agile e ripetibile.

Questa struttura consente inoltre di reagire rapidamente ai commenti dei clienti. Se è necessario rendere una correzione rapida nell'ambiente di produzione, è possibile anche farlo in modo efficiente in modo agile. È possibile creare un ramo master o di gestione temporanea e quando sarà pronto unirlo backup master e verso il basso in rami di sviluppo e delle funzionalità.

![Branch hotfix](source-control/_static/image2.png)

Senza una struttura ramificata simile al seguente con la separazione dei rami di sviluppo e produzione, un problema di produzione è stato possibile inserire è nella posizione di dover promuovere nuovo codice di funzionalità con la correzione di produzione. Il nuovo codice di funzionalità potrebbe non essere completamente testato e pronto per l'ambiente di produzione e potrebbe essere necessario eseguire numerose operazioni di backup le modifiche non pronte. Oppure è possibile ritardare la correzione per testare le modifiche e prepararli per la distribuzione.

Quindi sono riportati esempi di come implementare i tre modelli in Visual Studio, Azure e Visual Studio Online. Questi sono esempi anziché how-to--it istruzioni dettagliate; Per istruzioni dettagliate che forniscono tutte il contesto necessario, vedere la [risorse](#resources) sezione alla fine del capitolo.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Aggiungere script al controllo del codice sorgente in Visual Studio

È possibile aggiungere gli script di controllo del codice sorgente in Visual Studio includendoli in una cartella della soluzione Visual Studio (presupponendo che il progetto sia nel controllo del codice sorgente). Ecco un modo per eseguire questa operazione.

Creare una cartella per gli script nella cartella della soluzione (la stessa cartella che contiene il *sln* file).

![Cartella di automazione](source-control/_static/image3.png)

Copiare i file di script nella cartella.

![Contenuto della cartella automazione](source-control/_static/image4.png)

In Visual Studio, aggiungere una cartella della soluzione per il progetto.

![Selezione dei menu nuova cartella soluzione](source-control/_static/image5.png)

E aggiungere i file di script alla cartella della soluzione.

![Aggiungere la selezione di menu elemento esistente](source-control/_static/image6.png)

![Finestra di dialogo Aggiungi elemento esistente](source-control/_static/image7.png)

I file di script sono ora inclusi nel progetto e controllo del codice sorgente tiene traccia delle modifiche di versione insieme alle modifiche del codice sorgente corrispondente.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Dati sensibili di Store in Azure

Se si esegue l'applicazione in un sito Web di Azure, archiviarli in Azure è un modo per evitare di archiviare le credenziali nel controllo del codice sorgente.

Ad esempio, l'applicazione Fix It archivia nel relativo file Web. config file due stringhe di connessione che avranno le password nell'ambiente di produzione e una chiave che fornisce l'accesso all'account di archiviazione di Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Se si inserisce i valori di produzione effettivo per queste impostazioni nella finestra di *Web. config* file, o se li si inserisce *Release* file per configurare una trasformazione Web. config per inserirli durante la distribuzione, che sarà essere archiviate nel repository di origine. Se si sottoscrivono le stringhe di connessione di database di produzione profilo di pubblicazione, la password verrà incluso il *pubxml* file. (È possibile escludere le *pubxml* file dal controllo del codice sorgente, ma così si perderanno i vantaggi della condivisione di tutte le altre impostazioni di distribuzione.)

Azure ti offre un'alternativa per il **appSettings** e sezioni di stringhe di connessione il *Web. config* file. Ecco la parte interessata il **configurazione** scheda per un sito web nel portale di gestione di Azure:

![nel portale connectionStrings e appSettings](source-control/_static/image8.png)

Quando si distribuisce un progetto a questo sito web e l'esecuzione dell'applicazione, indipendentemente dai valori archiviati in Azure eseguire l'override indipendentemente dai valori sono nel file Web. config.

È possibile impostare questi valori in Azure usando il portale di gestione o gli script. Lo script di automazione di creazione ambiente si è visto nella [automatizzare tutto](automate-everything.md) capitolo viene creato un Database SQL di Azure, ottiene l'archiviazione e le stringhe di connessione di Database SQL e archivia questi segreti nelle impostazioni per il sito web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Si noti che gli script vengono parametrizzati in modo che i valori effettivi non vengono mantenuti al repository di origine.

Quando si esegue in locale nell'ambiente di sviluppo, l'app legge il file Web. config locale e la connessione di punti di stringa a un database LocalDB di SQL Server nel *App\_dati* cartella del progetto web. Quando si esegue l'app in Azure e l'app prova a leggere questi valori dal file Web. config, le operazioni che riceve e Usa sono i valori archiviati per il sito Web, non che cos'è effettivamente nel file Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Usare Git in Visual Studio e Visual Studio Online

È possibile usare qualsiasi ambiente di controllo di origine per implementare la struttura con rami DevOps presentata in precedenza. Per i team distribuiti un [sistema di controllo della versione distribuito](http://en.wikipedia.org/wiki/Distributed_revision_control) potrebbe funzionare meglio (molto DIFFUSO); per gli altri team un [sistema centralizzato](http://en.wikipedia.org/wiki/Revision_control) potrebbero funzionare meglio.

[GIT](http://git-scm.com/) è un molto DIFFUSO che è diventato molto diffuso. Quando si usa Git per controllo del codice sorgente, si ha una copia completa del repository con tutta la relativa cronologia nel computer locale. Molte persone preferiscono che perché è più facile continuare a lavorare quando non si è connessi alla rete, è possibile continuare a eseguire operazioni esegue il commit e rollback, creare e cambiare ramo e così via. Anche quando si è connessi alla rete, è più semplice e rapido creare rami e cambiare rami quando tutto è locale. È anche possibile eseguire i rollback e commit locali senza impatto sugli altri sviluppatori. Ed è possibile creare batch commit prima di inviarli al server.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), precedentemente noto come Team Foundation Service, offre sia Git e [Team Foundation Version Control](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; centralizzato di controllo del codice sorgente). Presso Microsoft nel gruppo di Azure qui alcuni team usano controllo del codice sorgente centralizzato, utilizzato per scopi distribuito, e alcune usano una combinazione (centralizzata per alcuni progetti e distribuita per altri progetti). Il servizio di Visual Studio Online è disponibile per un massimo di 5 utenti. È possibile iscriversi per un piano gratuito [qui](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 include incorporati di prim'ordine [supporto per Git](https://msdn.microsoft.com/library/hh850437.aspx); Ecco una rapida dimostrazione del funzionamento.

Con un progetto aperto in Visual Studio 2013, fare doppio clic la soluzione in **Esplora soluzioni**, quindi scegliere **Aggiungi soluzione al controllo del codice sorgente**.

![Aggiungi soluzione al controllo del codice sorgente](source-control/_static/image9.png)

Visual Studio chiede se si desidera usare Git o TFVC (controllo della versione centralizzato).

![Scegli controllo del codice sorgente](source-control/_static/image10.png)

Quando si seleziona Git e fare clic su **OK**, Visual Studio crea un nuovo repository Git locale nella cartella della soluzione. Il nuovo repository non sono presenti file ancora; è necessario aggiungerli al repository eseguendo un'operazione di commit di Git. Fare doppio clic la soluzione in **Esplora soluzioni**, quindi fare clic su **Commit**.

![Commit](source-control/_static/image11.png)

Visual Studio automaticamente delle fasi di tutti i file di progetto per il commit e li elenca nel **Team Explorer** nel **modifiche incluse** riquadro. (Se si sono verificati alcuni non si desidera includere nel commit, è possibile selezionare, pulsante destro del mouse e scegliere **escludere**.)

![Team Explorer](source-control/_static/image12.png)

Immettere un commento di commit e fare clic su **Commit**, Visual Studio e viene eseguito il commit viene visualizzato l'ID commit.

![Modifiche di Team Explorer](source-control/_static/image13.png)

Ora se si modifica un codice in modo che sia diverso rispetto a quanto nel repository, è possibile visualizzare facilmente le differenze. Un file che è stato modificato, selezionare pulsante destro del mouse **confronta con Unmodified**, e si otterrà una visualizzazione di confronto che illustra le modifiche non sottoposte a commit.

![Confronta con versione non modificata](source-control/_static/image14.png)

![Modifiche che Mostra differenze](source-control/_static/image15.png)

È possibile visualizzare facilmente le modifiche si apportano e archiviarle nella.

Si supponga che è necessario rendere un ramo – è possibile farlo in Visual Studio troppo. Nelle **Team Explorer**, fare clic su **nuovo ramo**.

![Nuovo ramo di Team Explorer](source-control/_static/image16.png)

Immettere un nome di ramo, fare clic su **Crea ramo**, e se è stato selezionato **Esegui Checkout del ramo**, Visual Studio estrae automaticamente il nuovo ramo.

![Nuovo ramo di Team Explorer](source-control/_static/image17.png)

È ora possibile apportare modifiche ai file e archiviarle questo ramo. Ed è possibile passare facilmente tra i rami e Visual Studio automaticamente estratto i file in qualsiasi ramo si esegue la sincronizzazione. In questo esempio la pagina web del titolo  *\_layout. cshtml* è stato modificato in "Correzione 1" in HotFix1 ramo.

![Ramo Hotfix1](source-control/_static/image18.png)

Se si passa nuovamente al master ramo, il contenuto del  *\_layout. cshtml* file ripristinato automaticamente quali operazioni sono nel ramo master.

![Ramo master](source-control/_static/image19.png)

Questo un semplice esempio di come può rapidamente creare un ramo e passa alternativamente tra rami. Questa funzionalità consente un flusso di lavoro altamente agile usando la struttura di branch e gli script di automazione presentati nel [automatizzare tutto](automate-everything.md) capitolo. Ad esempio, è possibile essere lavora nel ramo di sviluppo, creare un correzione rapida ramo master, passare al nuovo ramo, apportare le modifiche non esiste e, eseguirne il commit e quindi passare al ramo Development e continuare a quello che stavi.

Ho illustrato di seguito è l'utilizzo di un repository Git locale in Visual Studio. In un ambiente di team in genere anche push delle modifiche un repository comune. Gli strumenti di Visual Studio consentono, inoltre, in modo che punti a un repository Git remoto. È possibile usare GitHub.com a tale scopo oppure è possibile usare [Git in Visual Studio Online](https://msdn.microsoft.com/library/hh850437.aspx) integrato con tutte le altre funzionalità Visual Studio Online, ad esempio l'elemento di lavoro e rilevamento dei bug.

Non è l'unico modo è possibile implementare una strategia di creazione rami agile, ovviamente. È possibile abilitare il flusso di lavoro agile stesso usando un repository di controllo del codice sorgente centralizzato.

## <a name="summary"></a>Riepilogo

Valutare il successo del sistema del controllo codice sorgente basato su rapidità con cui è possibile apportare una modifica e ottenerlo in tempo reale in modo sicuro e prevedibile. Se ci si ritrova mischiare apportare una modifica perché è necessario eseguire un giorno o due dei test manuali su di esso, si potrebbe chiedersi che cosa è necessario eseguire process-wise o test-wise in modo da poter apportare tale modifica in minuti o a non più peggiore rispetto a un'ora. Una strategia per farlo consiste nell'implementare l'integrazione continua e recapito continuo, che verranno trattati nel [capitolo successivo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Risorse

Il [Visual Studio Online](https://www.visualstudio.com/) portale fornisce servizi di supporto e documentazione ed è possibile iscriversi per un account. Se si dispone di Visual Studio 2012 e si vuole usare Git, vedere [Visual Studio Tools per Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Per ulteriori informazioni su (controllo della versione centralizzato) TFVC e Git (controllo della versione distribuito), vedere le risorse seguenti:

- [Quale sistema di controllo della versione usare: TFVC o Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) Documentazione di MSDN, include una tabella che riepiloga le differenze tra TFVC e Git.
- [Beh, mi piace Team Foundation Server e mi piace Git, ma che è preferibile?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Confronto tra Git e TFVC.

Per altre informazioni sulle strategie di diramazione, vedere le risorse seguenti:

- [Creazione di una Pipeline di rilascio con Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentazione di Microsoft Patterns and Practices. Vedere il capitolo 6 per informazioni sulle strategie di diramazione. Funzione di rappresentanti del attiva/disattiva su rami delle funzionalità e se vengono utilizzati i rami per le funzionalità, rappresentanti del servizio mantenendoli breve durata (ore o giorni al massimo).
- [Guida di controllo di versione](https://aka.ms/vsarsolutions). Guida alle strategie di diramazione da ALM Rangers. Nella scheda download, vedere Strategies.pdf diramazione.
- [Sviluppo di software con i Feature Toggle](https://msdn.microsoft.com/magazine/dn683796.aspx). Articolo di MSDN Magazine.
- [Attivazione/disattivazione delle funzionalità](http://martinfowler.com/bliki/FeatureToggle.html). Introduzione alla funzionalità attivata e disattivata / flag delle funzionalità nel blog di Fowler.
- [Funzionalità di Visual Studio attiva o Disattiva funzionalità rami](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Un altro post di blog sui feature Toggle, Dylan Smith.

Per altre informazioni su come gestire le informazioni riservate che non devono essere conservate nei repository di controllo codice sorgente, vedere le risorse seguenti:

- [Procedure consigliate per la distribuzione delle password e altri dati sensibili in ASP.NET e servizio App di Azure](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Siti Web di Azure: Come le stringhe applicazione e connessione funzionamento delle stringhe](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Viene illustrata la funzionalità di Azure che esegue l'override `appSettings` e `connectionStrings` i dati nel *Web. config* file.
- [Personalizzato le impostazioni di configurazione e dell'applicazione in Azure Web Sites - con Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Per informazioni sugli altri metodi per mantenere informazioni sensibili all'esterno di controllo del codice sorgente, vedere [ASP.NET MVC: mantenere privato le impostazioni di controllo del codice sorgente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Precedente](automate-everything.md)
> [Successivo](continuous-integration-and-continuous-delivery.md)
