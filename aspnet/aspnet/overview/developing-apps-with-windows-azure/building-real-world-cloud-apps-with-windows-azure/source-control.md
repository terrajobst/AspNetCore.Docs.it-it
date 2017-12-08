---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Controllo (creazione di applicazioni con Azure Cloud del mondo reale) del codice sorgente | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: f244e6bd1cd8abd23b64d07ccafcef5c4db1029b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Controllo del codice sorgente (creazione di applicazioni Cloud reale in Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


Controllo del codice sorgente è essenziale per tutti i progetti di sviluppo cloud, non solo gli ambienti di team. Non sarebbe pensa della modifica del codice sorgente o di un documento di Word senza una funzione di annullamento e backup automatici e controllo del codice sorgente consente di tali funzioni a livello di progetto in cui possono salvare anche più tempo quando si verifica un errore. Con servizi di controllo codice sorgente di cloud, non è più necessario preoccuparsi di configurazione complesse ed è possibile utilizzare Visual Studio Online gratuito per fino a 5 utenti controllo del codice sorgente.

La prima parte di questa sezione vengono illustrate tre procedure consigliate principali da tenere presenti:

- [Gli script di automazione di considerare come codice sorgente](#scripts) versione e usarle con il codice dell'applicazione.
- [Non controllare mai in segreti](#secrets) (dati sensibili quali le credenziali) in un repository di codice sorgente.
- [Impostare i rami di origine](#devops) per abilitare il flusso di lavoro DevOps.

Il resto del capitolo fornisce alcuni esempi di implementazione di questi modelli in Visual Studio, Azure e Visual Studio Online:

- [Aggiungere script al controllo del codice sorgente in Visual Studio](#vsscripts)
- [Archiviare i dati sensibili in Azure](#appsettings)
- [Usare Git in Visual Studio e Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Gli script di automazione di considerare come codice sorgente

Quando si lavora in un progetto cloud che si desidera modificare frequentemente operazioni e si desidera essere in grado di rispondere rapidamente ai problemi segnalati dai clienti. Rispondere rapidamente implica l'utilizzo di script di automazione, come illustrato nel [automatizzare tutti gli elementi](automate-everything.md) capitolo. Tutti gli script che consente di creare l'ambiente, distribuire su di esso, scala, e così via, devono essere sincronizzati con il codice sorgente dell'applicazione.

Per mantenere sincronizzato con il codice script, archiviarli nel sistema di controllo del codice sorgente. Se è necessario eseguire il rollback delle modifiche o effettuare una correzione rapida per il codice di produzione è diverso dal codice di sviluppo, quindi non dispone di sprecare tempo durante il tentativo di individuare quali impostazioni sono state modificate o i membri del team sono disponibili copie della versione che è necessario. Si è certi che gli script che necessari vengono sincronizzati con la base di codice che occorre per, e si è certi che tutti i membri del team lavorano con gli stessi script. Se si desidera automatizzare il test e la distribuzione di un aggiornamento rapido sviluppo delle nuove funzionalità o di produzione, quindi sarà necessario lo script giusto per il codice che deve essere aggiornato.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Non archivia i segreti

Un repository di codice sorgente è in genere accessibile a troppi utenti per tale da un luogo sicuro in modo appropriato per i dati sensibili, ad esempio le password. Se gli script si basano su informazioni riservate, ad esempio password, parametrizzare tali impostazioni in modo che non vengono salvati nel codice sorgente e archiviare i segreti in un altro.

Ad esempio, di Azure consente di che scaricare i file che contengono impostazioni di pubblicazione per automatizzare la creazione di profili di pubblicazione. Questi file includono i nomi utente e password che sono autorizzate a gestire i servizi di Azure. Se si utilizza questo metodo per creare profili di pubblicazione e, se si archivia questi file al controllo del codice sorgente, chiunque disponga dell'accesso al repository può tali nomi utente e password. È possibile archiviare in modo sicuro la password nel profilo di pubblicazione stesso perché è crittografato e è un *. pubxml.user* file che, per impostazione predefinita, non è incluso nel controllo del codice sorgente.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Rami di origine struttura per facilitare DevOps del flusso di lavoro

Modalità di implementazione di rami nel repository influisce sulla capacità di sia sviluppare nuove funzionalità e risolvere i problemi nell'ambiente di produzione. Di seguito è riportato un modello che molta medie dimensioni utilizzare Team:

![Struttura di branch di origine](source-control/_static/image1.png)

Il ramo master corrisponde sempre il codice che è in produzione. Branch di sotto di master corrispondono a diverse fasi del ciclo di vita di sviluppo. Il ramo development è dove è implementare nuove funzionalità. Per un team di piccole dimensioni potrebbe semplicemente essere master e lo sviluppo, ma spesso, è consigliabile che gli utenti dispongano di un ramo di gestione temporanea tra lo sviluppo e master. È possibile utilizzare gestione temporanea per il test di integrazione finale prima di un aggiornamento viene spostato nell'ambiente di produzione.

Per i team di grandi dimensioni potrebbe essere branch separati per ogni nuova funzionalità; per un team più piccolo potrebbe essere everyone archiviando al ramo development.

Se si dispone di un ramo per ogni funzionalità, quando una funzionalità è pronta è merge relativo modifiche al codice sorgente allo sviluppo branching e down gli altri rami di funzionalità. Questo codice sorgente l'unione di processo può richiedere molto tempo e per evitare tale lavoro mantenendo comunque funzionalità separati, alcune squadre di implementano un'alternativa denominata  *[attiva o disattiva la funzionalità](http://en.wikipedia.org/wiki/Feature_toggle)*  (noto anche come *funzionalità flag*). Ciò significa tutto il codice per tutte le funzionalità sia nello stesso ramo, ma è abilitare o disabilitare singole funzionalità utilizzando i commutatori nel codice. Si supponga, ad esempio, di funzionalità A un nuovo campo per le attività Correggi app e funzionalità B aggiunge funzionalità di memorizzazione nella cache. Il codice per entrambe le funzionalità può essere nel ramo development, ma la visualizzazione dell'app verrà solo il nuovo campo quando una variabile è impostata su true e utilizzerà solo la memorizzazione nella cache quando una variabile diversa è impostata su true. Se A funzionalità non è pronta per essere innalzate di livello ma Feature B è pronto, è possibile alzare di livello tutto il codice nell'ambiente di produzione con l'opzione della funzionalità A disattivare e attivare la funzionalità di B. È quindi possibile fine A funzionalità e alzare di livello in un secondo momento, tutte con alcun codice di origine l'unione.

Se si utilizza rami o elementi Toggle per le funzionalità, una struttura con rami simile al seguente consente di propagare codice dallo sviluppo alla produzione in un modo agile e ripetibile.

Questa struttura consente inoltre di rispondere rapidamente ai commenti dei clienti. Se è necessario apportare una correzione rapida nell'ambiente di produzione, è possibile anche farlo in modo efficiente in modo flessibile. È possibile creare un ramo master o di gestione temporanea e quando è pronto unire master alto e verso il basso in rami di sviluppo e funzionalità.

![Branch hotfix](source-control/_static/image2.png)

Senza una struttura con rami simile al seguente con la separazione di produzione e branch di sviluppo, un problema di produzione potrebbe essere è nella posizione di dover alzare di livello nuovo codice di funzionalità con la correzione di produzione. Il nuovo codice di funzionalità potrebbe non essere completamente testata e pronta per la produzione e potrebbe essere necessario eseguire numerose operazioni di annullamento delle modifiche che non sono pronte. O potrebbe essere necessario ritardare la correzione per testare le modifiche e renderle pronti per la distribuzione.

Successivamente, si noterà esempi di come implementare questi tre modelli in Visual Studio, Azure e Visual Studio Online. Questi sono esempi anziché di procedure su-it istruzioni dettagliate; Per istruzioni dettagliate che forniscono il contesto necessario tutte, vedere il [risorse](#resources) sezione alla fine del capitolo.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Aggiungere script al controllo del codice sorgente in Visual Studio

È possibile aggiungere script al controllo del codice sorgente in Visual Studio inserendoli in una cartella della soluzione di Visual Studio (presupponendo che il progetto è contenuto nel controllo del codice sorgente). Ecco un modo per eseguire l'operazione.

Creare una cartella per gli script nella cartella della soluzione (della stessa cartella che ha il *sln* file).

![Cartella di automazione](source-control/_static/image3.png)

Copiare i file di script nella cartella.

![Contenuto della cartella automazione](source-control/_static/image4.png)

In Visual Studio, aggiungere una cartella della soluzione per il progetto.

![Selezione dei menu nuova cartella soluzione](source-control/_static/image5.png)

E aggiungere i file di script alla cartella della soluzione.

![Aggiungi elemento esistente selezione dei menu](source-control/_static/image6.png)

![Finestra di dialogo Aggiungi elemento esistente](source-control/_static/image7.png)

I file di script sono ora inclusi nel progetto e controllo del codice sorgente stia registrando le modifiche apportate alla versione insieme di modifiche del codice sorgente corrispondente.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Archiviare i dati sensibili in Azure

Se si esegue l'applicazione in un sito Web di Azure, è possibile evitare di archiviare le credenziali nel controllo del codice sorgente è archiviarli in Azure.

Ad esempio, l'applicazione Correggi archivia nel relativo file Web. config file due stringhe di connessione contenenti le password in produzione e una chiave che consente di accedere al proprio account di archiviazione di Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Se si inserisce i valori di produzione effettivo per queste impostazioni nel *Web. config* file, o se li si inserisce il *Web.Release.config* file per configurare una trasformazione Web. config per inserirli durante la distribuzione, che verrà essere archiviate nel repository di origine. Se si immette le stringhe di connessione di database di produzione profilo di pubblicazione, la password verrà incluso il *pubxml* file. (È possibile escludere il *pubxml* file dal controllo del codice sorgente, ma in seguito si perde il vantaggio della condivisione di tutte le altre impostazioni di distribuzione.)

Azure offre un'alternativa per il **appSettings** e sezioni di stringhe di connessione di *Web. config* file. Di seguito è la parte rilevante del **configurazione** scheda per un sito web nel portale di gestione di Azure:

![appSettings e connectionStrings nel portale](source-control/_static/image8.png)

Quando si distribuisce un progetto per il sito web e l'esecuzione dell'applicazione, indipendentemente dai valori archiviati in Azure sostituire i valori sono presenti nel file Web. config.

È possibile impostare questi valori in Azure tramite il portale di gestione o gli script. Lo script di automazione di creazione ambiente in cui si è visto il [automatizzare tutti gli elementi](automate-everything.md) capitolo crea un Database SQL di Azure, ottiene l'archiviazione e le stringhe di connessione di Database SQL e archivia questi segreti nelle impostazioni per il sito web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Si noti che gli script siano con parametri in modo che i valori effettivi non vengono mantenuti nel repository di origine.

Quando si esegue in locale nell'ambiente di sviluppo, l'app legge il file Web. config locale e la connessione di punti di stringa a un database LocalDB di SQL Server nel *App\_dati* cartella del progetto web. Quando si esegue l'app in Azure e l'applicazione tenta di leggere questi valori dal file Web. config, cosa consente di ottenere e Usa sono i valori archiviati per il sito Web, non è effettivamente nel file Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Usare Git in Visual Studio e Visual Studio Online

È possibile utilizzare qualsiasi ambiente di controllo di origine per implementare la struttura con rami DevOps presentata in precedenza. Per i team distribuiti un [distribuito il sistema di controllo di versione](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) potrebbe essere la scelta ottimale per altri team un [sistema centralizzato](http://en.wikipedia.org/wiki/Revision_control) potrebbe funzionare meglio.

[GIT](http://git-scm.com/) è un DVCS che è diventata molto diffusa. Quando si usa Git per controllo del codice sorgente, è una copia completa del repository con tutta la sua cronologia sul computer locale. Molti utenti preferiscono tuttavia che poiché è più semplice per continuare a lavorare quando non si è connessi alla rete, è possibile continuare a eseguire il commit e rollback, creare e cambiare rami e così via. Anche quando si è connessi alla rete, è più semplice e rapido creare rami e cambiare rami quando tutto è locale. È anche possibile eseguire rollback e commit locale senza un impatto su altri sviluppatori. Ed è possibile raggruppare in batch commit prima di inviarli al server.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), precedentemente noto come Team Foundation Service, offre sia Git e [Team Foundation Version Control](https://msdn.microsoft.com/en-us/library/ms181237(v=vs.120).aspx) (TFVC; centralizzato di controllo del codice sorgente). Qui in Microsoft Azure gruppo alcune squadre di utilizzano controllo del codice sorgente centralizzato, utilizzato per scopi di distribuzione, e alcuni utilizzano una combinazione (centralizzata per alcuni progetti e distribuiti per altri progetti). Il servizio di Visual Studio Online è disponibile per gli utenti fino a 5. È possibile iscriversi per un piano gratuito [qui](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 include incorporato prima classe [supporto Git](https://msdn.microsoft.com/en-us/library/hh850437.aspx); Ecco una rapida dimostrazione di come funziona.

Con un progetto aperto in Visual Studio 2013, fare doppio clic sulla soluzione in **Esplora**e scegliere **Aggiungi soluzione al controllo del codice sorgente**.

![Aggiungi soluzione al controllo del codice sorgente](source-control/_static/image9.png)

Visual Studio chiede se si desidera utilizzare TFVC (controllo della versione centralizzato) o Git.

![Controllo del codice sorgente](source-control/_static/image10.png)

Quando si seleziona Git e si sceglie **OK**, Visual Studio crea un nuovo repository Git locale nella cartella della soluzione. Il nuovo repository non sono presenti file ancora; è necessario aggiungerli al repository eseguendo un'operazione di commit Git. Fare doppio clic sulla soluzione in **Esplora**, quindi fare clic su **Commit**.

![Commit](source-control/_static/image11.png)

Visual Studio automaticamente tutti i file di progetto per il commit di fasi e li elenca in **Team Explorer** nel **modifiche incluse** riquadro. (Se vi sono alcune non si desidera includere nel commit, è possibile selezionare, li pulsante destro del mouse e scegliere **escludere**.)

![Team Explorer](source-control/_static/image12.png)

Immettere un commento di commit e fare clic su **Commit**, e Visual Studio viene eseguito il commit e di visualizzare l'ID del commit.

![Modifiche di Team Explorer](source-control/_static/image13.png)

Ora se si modifica il codice in modo che sia diversa da quella nel repository, è possibile visualizzare facilmente le differenze. Scelta di un file che sono stati modificati, selezionare **confrontare Unmodified**, e si otterrà una visualizzazione di confronto che mostra le modifiche non salvate.

![Confronta con non modificato](source-control/_static/image14.png)

![Modifiche con diff](source-control/_static/image15.png)

È possibile visualizzare facilmente le modifiche si apportano e li archivia.

Si supponga che è necessario creare un ramo, è possibile farlo troppo in Visual Studio. In **Team Explorer**, fare clic su **nuovo ramo**.

![Nuovo ramo di Team Explorer](source-control/_static/image16.png)

Immettere un nome di ramo, fare clic su **Crea ramo**, e se è selezionata **branch estrazione**, Visual Studio estrae automaticamente il nuovo ramo.

![Nuovo ramo di Team Explorer](source-control/_static/image17.png)

È possibile apportare modifiche ai file e li archivia in questo ramo. Ed è possibile passare facilmente tra branch e Visual Studio automaticamente le sincronizzazioni file la seconda diramazione è estratto. In questo esempio la pagina web title in  *\_cshtml* è stato modificato in "Correzione 1" in HotFix1 branch.

![Ramo Hotfix1](source-control/_static/image18.png)

Se si passa nuovamente al master Crea un ramo, il contenuto del  *\_cshtml* file ripristinato automaticamente sono nel ramo master.

![Ramo master](source-control/_static/image19.png)

Questo un semplice esempio di come creare un ramo e rapidamente scorrere avanti e indietro tra branch. Questa funzionalità consente a un flusso di lavoro agile elevata utilizzando la struttura di rami e gli script di automazione presentati nel [automatizzare tutti gli elementi](automate-everything.md) capitolo. Ad esempio, si utilizza il ramo Development, creare un ramo correzione master, passare al nuovo ramo, apportare le modifiche, eseguirne il commit e quindi tornare al ramo Development e continuare le operazioni.

Ciò che si è visto è l'utilizzo di un repository Git locale in Visual Studio. In un ambiente di team in genere anche push delle modifiche in un repository comune. Gli strumenti di Visual Studio consentono inoltre di scegliere un repository Git remoto. È possibile utilizzare GitHub.com a tale scopo oppure è possibile utilizzare [Git in Visual Studio Online](https://msdn.microsoft.com/en-us/library/hh850437.aspx) integrato con tutte le altre funzionalità Visual Studio Online, ad esempio l'elemento di lavoro e rilevamento dei bug.

Non è l'unico modo è possibile implementare una strategia di diramazione agile, ovviamente. È possibile abilitare il flusso di lavoro agile stesso utilizzando un repository di controllo del codice sorgente centralizzato.

## <a name="summary"></a>Riepilogo

Valutare il successo del sistema di controllo del codice sorgente in base a rapidità con cui è possibile apportare una modifica e scaricarlo in tempo reale in modo sicuro e prevedibile. Se è necessario impaurito apportare una modifica perché è necessario effettuare un o due giorni di test manuali su di esso, è possibile chiedere manualmente ciò che è necessario eseguire process-wise o test-wise, in modo che è possibile apportare tale modifica, in minuti o a non più peggiore rispetto a un'ora. Una strategia per questo scopo consiste nell'implementare l'integrazione continua e la distribuzione continua, che verranno trattati nel [capitolo successivo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Risorse

Il [Visual Studio Online](https://www.visualstudio.com/) portale fornisce servizi di supporto e documentazione ed è possibile iscriversi per un account. Se si dispone di Visual Studio 2012 e si desidera utilizzare Git, vedere [Visual Studio Tools per Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Per ulteriori informazioni su TFVC (controllo della versione centralizzato) e Git (controllo della versione distribuita), vedere le risorse seguenti:

- [Il sistema di controllo della versione è preferibile usare: TFVC o Git?](https://msdn.microsoft.com/en-us/library/vstudio/ms181368.aspx#tfvc_or_git_summary) Documentazione di MSDN, include una tabella di riepilogo le differenze tra TFVC e Git.
- [In genere Team Foundation Server e desidero Git, ma che è preferibile?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Confronto tra Git e TFVC.

Per ulteriori informazioni sulle strategie di diramazione, vedere le risorse seguenti:

- [La creazione di una Pipeline di rilascio con Team Foundation Server 2012](https://msdn.microsoft.com/en-us/library/dn449957.aspx). Documentazione di Microsoft Patterns and Practices. Vedere il capitolo 6 per informazioni sulle strategie di diramazione. Funzionalità sostenitori attiva o disattiva su rami di funzionalità e se si utilizzano i branch per le funzionalità, sostiene mantenendoli breve durata (ore o giorni al massimo).
- [Versione controllo Guida](https://aka.ms/vsarsolutions). Guida alle strategie di diramazione da ALM Rangers. Nella scheda dei download, vedere Strategies.pdf diramazione.
- [Sviluppo di software con attiva o Disattiva funzionalità](https://msdn.microsoft.com/en-us/magazine/dn683796.aspx). Articolo di MSDN Magazine.
- [Attivazione/disattivazione delle funzionalità](http://martinfowler.com/bliki/FeatureToggle.html). Introduzione alla funzionalità attiva o Disattiva / funzionalità flag sul blog di Martin Fowler.
- [Funzionalità di vs attiva o disattiva la funzionalità rami](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Un altro post di blog su Attiva o disattiva la funzionalità, Dylan Smith.

Per ulteriori informazioni su come gestire le informazioni riservate che non devono essere mantenute nel repository del controllo codice sorgente, vedere le risorse seguenti:

- [Procedure consigliate per la distribuzione delle password e altri dati sensibili per ASP.NET e servizio App di Azure](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Siti Web di Azure: La connessione e le stringhe dell'applicazione stringhe lavoro](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Viene illustrata la funzionalità di Azure che esegue l'override `appSettings` e `connectionStrings` dati il *Web. config* file.
- [Custom impostazioni di configurazione e applicazione in Azure Web Sites - con Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Per informazioni su altri metodi per mantenere le informazioni riservate da controllo del codice sorgente, vedere [ASP.NET MVC: mantenere privato le impostazioni di controllo del codice sorgente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

>[!div class="step-by-step"]
[Precedente](automate-everything.md)
[Successivo](continuous-integration-and-continuous-delivery.md)
