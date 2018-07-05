---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Creare il progetto | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 81d3b07fa0710d03bc350b93a02f7d3aba2bb684
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814669"
---
<a name="create-the-project"></a>Creare il progetto
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


In questa esercitazione si verranno creare, esaminare ed eseguire il progetto predefinito in Visual Studio, che consentirà di acquisire familiarità con le funzionalità di ASP.NET. Inoltre, si esaminerà l'ambiente di Visual Studio.

## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come creare un nuovo progetto di Web Form.
- La struttura di file del progetto Web Form.
- Come eseguire il progetto in Visual Studio.
- Le diverse funzionalità dell'applicazione Web forms predefinita.
- Nozioni fondamentali su come usare l'ambiente di Visual Studio.

## <a name="creating-the-project"></a>Creazione del progetto

1. Aprire Visual Studio.
2. Selezionare **nuovo progetto** dalle **File** menu di Visual Studio. 

    ![Creare il progetto - nuova voce di Menu progetto](create-the-project/_static/image1.png)
3. Selezionare il **modelli**  - &gt; **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra.
4. Scegliere il **applicazione Web ASP.NET** modello nella colonna centrale.  
 Questa serie di esercitazioni utilizza .NET Framework 4.5.2.
5. Denominare il progetto *WingtipToys* e scegliere il **OK** pulsante. 

    ![Creare il progetto - finestra di dialogo Nuovo progetto](create-the-project/_static/image2.png)

    > [!NOTE]
    > È il nome del progetto in questa serie di esercitazioni **WingtipToys**. È consigliabile usare ciò *esatto* nome del progetto in modo che il codice fornito in tutta la serie di esercitazioni funzioni come previsto.

6. Scegliere il **Modifica autenticazione** pulsante. Selezionare **account utente individuali** e fare clic sui **OK** pulsante.

7. Selezionare il **Web Form** modello, quindi scegliere il **OK** pulsante.

    ![Creare il progetto - modello nuovo progetto](create-the-project/_static/image3.png)

Il progetto richiederà un po' di tempo per creare. Quando è pronto, aprire il **default. aspx** pagina.

![Creare il progetto - modello nuovo progetto](create-the-project/_static/image4.png)

È possibile passare **Design** visualizzazione e **origine** visualizzazione selezionando un'opzione nella parte inferiore della finestra Centro. **Progettazione** Vista sono riportati le pagine Web ASP.NET, pagine master, pagine di contenuto, pagine HTML e controlli utente tramite una visualizzazione WYSIWYG. **Origine** Vista consente di visualizzare il markup HTML per la pagina Web, che è possibile modificare.

> [!TIP] 
> 
> **Comprendere i framework ASP.NET**
> 
> Web Form ASP.NET consente di creare siti Web dinamici usando un modello noto di trascinamento e rilascio, basata su eventi. Un'area di progettazione e centinaia di controlli e componenti che consentono di creare rapidamente potenti e sofisticati siti basati su interfaccia utente con accesso ai dati. La Store giocattoli Wingtip è basata su Web Form ASP.NET, ma molti dei concetti che apprese in questa serie di esercitazioni sono applicabili a tutti di ASP.NET.
> 
> ASP.NET offre quattro Framework di sviluppo primario:
> 
> - [Web Form ASP.NET](../../../index.md)  
>  Il framework di Web Form è destinato agli sviluppatori che preferiscono programmazione dichiarativa e basata sul controllo, ad esempio Microsoft Windows Forms (WinForms) e XAML/WPF o Silverlight. Che offre un modello di sviluppo basato su finestra di progettazione WYSIWYG, quindi viene usato di frequente con gli sviluppatori che desiderano per un ambiente di sviluppo (RAD) rapido di applicazioni per lo sviluppo web. Se si ha familiarità con la programmazione web e si ha familiarità con gli strumenti di sviluppo di client RAD Microsoft tradizionali (ad esempio, per Visual Basic e Visual c#), è possibile compilare rapidamente un'applicazione web senza la necessità di esperienza in HTML e JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC è destinato agli sviluppatori che desiderano principi, come lo sviluppo basato su test, la separazione dei compiti, inversione del controllo (IoC) e l'inserimento delle dipendenze (dipendenze) e modelli. Questo framework incoraggia che separa il livello di logica di business di un'applicazione web dal relativo livello di presentazione.
> - [ASP.NET Web Pages](../../../../web-pages/index.md)  
>  ASP.NET Web Pages è destinato agli sviluppatori che desiderano una storia di sviluppo web semplice, nelle righe di PHP. Nel modello di Web Pages, si creano pagine HTML e quindi aggiungerla codice basato su server per la pagina per controllare in modo dinamico la modalità con cui viene eseguito il rendering di markup. Pagine Web è appositamente progettato per essere un framework leggero ed è il punto di ingresso più semplice in ASP.NET per le persone che conoscono HTML ma non abbia ampia esperienza di programmazione, ad esempio, gli studenti o appassionati. È anche un ottimo modo per gli sviluppatori web che conoscono PHP o Framework simile per iniziare a utilizzare ASP.NET.
> - [Applicazione a pagina singola ASP.NET](../../../../single-page-application/index.md)  
>  Applicazione a pagina singola ASP.NET (SPA) consente di compilare applicazioni che includono significative interazioni lato client usando HTML5, CSS 3 e JavaScript. ASP.NET e Web Tools 2012.2 Update viene fornito un nuovo modello per la compilazione di applicazioni a pagina singola usando Knockout. js e ASP.NET Web API. Oltre il nuovo modello di applicazione a singola pagina, nuovi modelli di applicazione a singola pagina creati dalla community sono anche disponibili per il download.
> 
> Oltre i quattro Framework di sviluppo principale, ASP.NET offre anche altre tecnologie che sono importante essere consapevoli e abbia familiarità con, ma non sono trattate in questa serie di esercitazioni:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -un framework per la creazione di servizi HTTP che soddisfano una vasta gamma di client, inclusi browser e dispositivi mobili.
> - [ASP.NET SignalR](../../../../signalr/index.md) -una libreria che semplifica lo sviluppo funzionalità web in tempo reale.


### <a name="reviewing-the-project"></a>Revisione del progetto

In Visual Studio, il **Esplora soluzioni** finestra consente di gestire i file di progetto. Diamo un'occhiata le cartelle che sono stati aggiunti all'applicazione nel **Esplora soluzioni**. Il modello di applicazione web aggiunge una struttura di cartelle di base:

![Creare il progetto - Esplora soluzioni](create-the-project/_static/image5.png)

Visual Studio crea alcuni file per il progetto e cartelle iniziale. I primo file che è possibile lavorare con più avanti in questa esercitazione sono i seguenti:

| **File** | **Scopo** |
| --- | --- |
| *Default.aspx* | In genere la prima pagina visualizzata quando l'applicazione viene eseguita in un browser. |
| *Site.Master* | Una pagina che consente di creare un layout e l'utilizzo standard un comportamento coerente per le pagine nell'applicazione. |
| *Global.asax* | Un file facoltativo che contiene il codice di risposta agli eventi a livello di applicazione e a livello di sessione generati da ASP.NET o moduli HTTP. |
| *Web.config* | I dati di configurazione per un'applicazione. |

### <a name="running-the-default-web-application"></a>Esegue l'applicazione Web predefinita

L'applicazione Web predefinita fornisce un'esperienza avanzata basata su supporto e le funzionalità predefinite. Senza alcuna modifica al progetto predefinito Web Form, l'applicazione è pronta per l'esecuzione nel Web browser locale.

1. Premere il ***F5*** chiave mentre in Visual Studio.   
 L'applicazione verrà compilata e visualizzato nel browser Web.  

    ![Creare il progetto: pagina predefinita](create-the-project/_static/image6.png)
2. Dopo aver completato la revisione dell'applicazione in esecuzione, chiudere la finestra del browser.

Esistono tre pagine principale in questa applicazione Web predefinita: *default. aspx* (Home), *About*, e *aspx*. Ognuna di queste pagine può essere raggiunto da barra di spostamento superiore. Esistono anche due pagine aggiuntive contenute nella cartella di Account, la pagina Register. aspx e pagina Login. aspx. Queste due pagine consentono di usare le funzionalità di appartenenza di ASP.NET per creare, archiviare e convalidare le credenziali dell'utente.

## <a name="aspnet-web-forms-background"></a>Web Form ASP.NET in Background

Web Form ASP.NET sono le pagine che sono basate sulla tecnologia di Microsoft ASP.NET, in cui il codice che viene eseguito nel server in modo dinamico genera output delle pagine Web nel dispositivo client o browser. Una pagina Web Form ASP.NET esegue automaticamente il rendering dell'HTML browser conforme corretto per le funzionalità, ad esempio stili, layout e così via. Web Form sono compatibili con qualsiasi linguaggio supportato da .NET common language runtime, ad esempio Microsoft Visual Basic e Microsoft Visual c#. Inoltre, Web Form si basano sul [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), che offre i vantaggi, ad esempio un ambiente gestito, indipendenza dai tipi e l'ereditarietà.

Quando una pagina Web Form ASP.NET in esecuzione, la pagina passa attraverso un ciclo di vita in cui esegue una serie di passaggi di elaborazione. Tali passaggi includono l'inizializzazione, creazione di controlli, il ripristino e gestione dello stato, l'esecuzione di codice del gestore eventi e il rendering. Dopo avere acquisito maggiore familiarità con la potenza del Web Form ASP.NET, è importante comprendere le [ciclo di vita della pagina ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) in modo che sia possibile scrivere codice fase del ciclo di vita appropriato per ottenere l'effetto desiderato.

Quando un server Web riceve una richiesta per una pagina, individua la pagina, li elabora, lo invia al browser e quindi rimuove tutte le informazioni sulla pagina. Se l'utente richiede la stessa pagina anche in questo caso, il server si ripete l'intera sequenza, la rielaborazione da zero. In altre parole, un server non ha memoria delle pagine che dispone di pagine elaborati sono senza stato. Framework della pagina ASP.NET gestisce automaticamente l'attività di gestione dello stato della pagina e i relativi controlli e consente di mantenere lo stato di informazioni specifiche dell'applicazione in modo esplicito.

> [!TIP] 
> 
> **Funzionalità dell'applicazione Web nel modello di applicazione Web Form**
> 
> Il modello di applicazione Web Form ASP.NET fornisce un set completo di funzionalità incorporate. Fornisce non solo con un *ridefinisce* pagina, un *About* pagina, una *aspx* pagina, ma include anche funzionalità di appartenenza che registra gli utenti e Salva le credenziali in modo che è possibile accedere al sito Web. Questa panoramica fornisce altre informazioni su alcune delle funzionalità contenute nel modello di applicazione Web Form ASP.NET e come usarle nell'applicazione Wingtip Toys.
> 
> **Appartenenza**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) identità archivia le credenziali degli utenti in un database creato dall'applicazione. Quando l'accesso, l'applicazione convalida le proprie credenziali per la lettura del database. Il progetto *Account* cartella contiene i file che implementano le varie parti di appartenenza: la registrazione, accesso, modifica di una password e l'autorizzazione dell'accesso. Inoltre, Web Form ASP.NET supporta OAuth e OpenID. Questi miglioramenti di autenticazione consentono agli utenti di accedere al sito usando le credenziali esistenti, da tali account come Facebook, Twitter, Windows Live e Google.
> 
> ![Creare il progetto - Esplora soluzioni (identità di ASP.NET)](create-the-project/_static/image7.png)
> 
> Per impostazione predefinita, il modello crea un database di appartenenza usando un nome predefinito del database in un'istanza di SQL Server Express LocalDB, il server di database di sviluppo fornita con Visual Studio Express 2013 per Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) è una versione leggera di SQL Server che dispone di numerose funzionalità di programmabilità di un database di SQL Server. SQL Server Express LocalDB viene eseguito in modalità utente e ha un'installazione veloce senza operazioni di configurazione con un breve elenco di prerequisiti di installazione. In Microsoft SQL Server, qualsiasi database o codice Transact-SQL può essere spostato da SQL Server Express LocalDB a SQL Server e SQL Azure senza alcun passaggio di aggiornamento. Pertanto, SQL Server Express LocalDB è utilizzabile come un ambiente di sviluppo per applicazioni destinate a tutte le edizioni di SQL Server. SQL Server Express LocalDB Abilita le funzionalità, ad esempio stored procedure, funzioni definite dall'utente e funzioni di aggregazione, integrazione con .NET Framework, i tipi spaziali e ad altri utenti che non sono disponibili in SQL Server Compact.
> 
> **Pagine master**
> 
> Un' [pagina master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definisce un aspetto uniforme e il comportamento per tutte le pagine nell'applicazione. Consente di unire il layout della pagina master con il contenuto da una singola pagina contenuto per produrre la pagina finale dall'utente. Nell'applicazione, Wingtip Toys è modificare il *Site. master* pagina master in modo che tutte le pagine nel sito Web di Wingtip Toys condividano la stessa barra di navigazione e logo distintivo.
> 
> **HTML5**
> 
> Il modello di applicazione Web Form ASP.NET supporta [HTML5](http://www.w3schools.com/html/html5_intro.asp), ovvero la versione più recente del linguaggio di markup HTML. HTML5 supporta funzionalità che rendono più semplice creare siti Web e i nuovi elementi.
> 
> **Modernizr**
> 
> Per i browser che non supportano HTML5, è possibile usare [Modernizr](http://www.modernizr.com/). Modernizr è una libreria JavaScript open source che consente di rilevare se un browser supporta le funzionalità HTML5 e attivarli in caso contrario. Nel modello di applicazione Web Form ASP.NET, Modernizr viene installato come un pacchetto NuGet.
> 
> **Bootstrap**
> 
> Usano i modelli di progetto di Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un framework di layout e temi creato da Twitter. Bootstrap Usa CSS3 per fornire progettazione reattiva, ovvero i layout possono adattarsi dinamicamente alle dimensioni di finestra diversa del browser. È possibile usare anche funzionalità temi del Bootstrap per rendere facilmente effettiva una modifica nell'aspetto dell'applicazione. Per impostazione predefinita, il modello di applicazione Web ASP.NET in Visual Studio 2013 include Bootstrap come pacchetto NuGet.
> 
> **Pacchetti NuGet**
> 
> Il modello di applicazione Web Form ASP.NET include un set di [NuGet](http://www.nuget.org/) pacchetti. Questi pacchetti forniscono funzionalità componentizzata sotto forma di strumenti e librerie open source. È disponibile un'ampia gamma di pacchetti che consentono di creare e testare le applicazioni. Visual Studio è facile da aggiungere, rimuovere e aggiornare i pacchetti NuGet. Gli sviluppatori possono creare e aggiungere anche i pacchetti per NuGet.
> 
> ![Creare il progetto - finestra di dialogo di NuGet](create-the-project/_static/image8.png)
> 
> Quando si installa un pacchetto, NuGet copia file della soluzione ed esegue automaticamente tutte le modifiche sono necessarie, ad esempio l'aggiunta di riferimenti e modifica della configurazione associata all'applicazione Web. Se si decide di rimuovere la libreria, NuGet rimuove i file e inverte le modifiche apportata nel progetto in modo che non rimanga alcun disordine. NuGet è disponibile il **strumenti** menu di Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) è una libreria JavaScript veloce e conciso che semplifica l'attraversamento dei documenti HTML, la gestione degli eventi, l'animazione e interazioni di Ajax per lo sviluppo rapido di web. La libreria JavaScript jQuery è incluso nel modello di applicazione Web Form ASP.NET come pacchetto NuGet.
> 
> **Convalida discreta**
> 
> Controlli validator incorporati sono stati configurati per l'uso di JavaScript non intrusivo per la logica di convalida lato client. Ciò notevolmente riduce la quantità di JavaScript viene eseguito il rendering in linea nel markup della pagina e riduce le dimensioni complessive della pagina. La convalida discreta viene aggiunto a livello globale per il modello di applicazione Web Form ASP.NET in base all'impostazione nel &lt;appSettings&gt; elemento delle *Web. config* file nella radice dell'applicazione.
> 
> **Entity Framework Code First**
> 
> Oltre alle funzionalità nel modello di applicazione Web Form ASP.NET, viene utilizzata l'applicazione Wingtip Toys [Code First di Entity Framework](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), che è una libreria di NuGet che consente lo sviluppo incentrato sul codice quando si lavora con i dati. In parole semplici, crea la parte di database dell'applicazione per l'utente in base al codice scritto. Si usa Entity Framework, recuperare e manipolare i dati come oggetti fortemente tipizzati. Questo consente di concentrarsi sulla logica di business dell'applicazione anziché i dettagli della modalità di accesso ai dati.
> 
> Per altre informazioni sulle librerie installate e i pacchetti inclusi con il modello Web Form ASP.NET, vedere l'elenco dei pacchetti NuGet installati. A questo scopo, In Visual Studio creare un nuovo progetto di Web Form, selezionare **degli strumenti**  - &gt; **Library Package Manager**  - &gt; **Gestisci Pacchetti NuGet per la soluzione**e selezionare **dei pacchetti installati** nel **Gestisci pacchetti NuGet** nella finestra di dialogo.


### <a name="touring-visual-studio"></a>Touring Visual Studio

Le finestre primarie in Visual Studio includono i **Esplora soluzioni**, il **Esplora Server** (**Esplora Database** in Express), il **proprietà Finestra**, il **casella degli strumenti**, il **sulla barra degli strumenti**e la **finestra del documento**.

![Creare il progetto - finestra di dialogo di NuGet](create-the-project/_static/image9.png)

Per altre informazioni su Visual Studio, vedere [guida visiva per Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione si hanno creato, esaminato ed eseguire l'applicazione Web Form predefinito. Avere esaminato le diverse funzionalità dell'applicazione Web forms predefinita e appreso alcune nozioni di base su come usare l'ambiente di Visual Studio. Nelle esercitazioni seguenti si creerà il livello di accesso ai dati.

## <a name="additional-resources"></a>Risorse aggiuntive

[Scelta del modello di programmazione](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Progetti applicazione Web e progetti di siti Web](https://msdn.microsoft.com/library/dd547590.aspx)   
[Web Form ASP.NET le pagine Panoramica](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Precedente](introduction-and-overview.md)
> [Successivo](create_the_data_access_layer.md)
