---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Creare il progetto | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 7cfceb38204b6cfd3589a082761273e54ac122ca
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
<a name="create-the-project"></a>Creare il progetto
====================
da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


In questa esercitazione è creare, esaminare e si esegue il progetto predefinito in Visual Studio, che consente di acquisire familiarità con le funzionalità di ASP.NET. Esaminare anche l'ambiente di Visual Studio.

## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come creare un nuovo progetto di Web Form.
- La struttura di file del progetto Web Form.
- Come eseguire il progetto in Visual Studio.
- Le diverse funzionalità dell'applicazione predefinito Web Form.
- Alcuni concetti di base su come usare l'ambiente di Visual Studio.

## <a name="creating-the-project"></a>Creazione del progetto

1. Aprire Visual Studio.
2. Selezionare **nuovo progetto** dal **File** menu in Visual Studio. 

    ![Creare il progetto - nuova voce di Menu progetto](create-the-project/_static/image1.png)
3. Selezionare il **modelli**  - &gt; **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra.
4. Scegliere il **applicazione Web ASP.NET** modello nella colonna centrale.  
 Questa serie di esercitazioni utilizza .NET Framework 4.5.2.
5. Denominare il progetto *WingtipToys* e scegliere il **OK** pulsante. 

    ![Creare il progetto - finestra di dialogo Nuovo progetto](create-the-project/_static/image2.png)

    > [!NOTE]
    > È il nome del progetto in questa serie di esercitazioni **WingtipToys**. È consigliabile utilizzare questo *esatta* nome del progetto in modo che il codice fornito in tutta la serie di esercitazioni funzioni come previsto.

6. Fare clic su di **Modifica autenticazione** pulsante. Selezionare **singoli account utente** e fare clic sui **OK** pulsante.

7. Selezionare il **Web Form** modello, quindi scegliere il **OK** pulsante.

    ![Creare il progetto - nuovo modello di progetto](create-the-project/_static/image3.png)

Il progetto richiederà un po' di tempo per creare. Quando è pronto, aprire il **Default.aspx** pagina.

![Creare il progetto - nuovo modello di progetto](create-the-project/_static/image4.png)

È possibile passare da **progettazione** Vista e **origine** visualizzazione selezionando un'opzione nella parte inferiore della finestra center. **Progettazione** Visualizza le pagine Web ASP.NET, pagine master, pagine di contenuto, pagine HTML e controlli utente tramite una visualizzazione WYSIWYG. **Origine** consente di visualizzare il markup HTML della pagina Web, è possibile modificare.

> [!TIP] 
> 
> **Comprendere i framework ASP.NET**
> 
> Web Form ASP.NET consente di creare siti Web dinamici usando un comune modello di trascinamento e rilascio, basato su eventi. Un'area di progettazione e centinaia di controlli e componenti che consentono di creare rapidamente potenti siti basati su interfaccia utente con accesso ai dati. L'archivio di giocattoli Wingtip è basato su Web Form ASP.NET, ma molti dei concetti che si apprenderà in questa serie di esercitazioni sono applicabili a tutti di ASP.NET.
> 
> ASP.NET offre quattro Framework di sviluppo principali:
> 
> - [Web Form ASP.NET](../../../index.md)  
>  Il framework di Web Form è destinata agli sviluppatori che preferiscono la programmazione dichiarativa e basati sul controllo, ad esempio Microsoft Windows Form (Winform) e XAML/WPF o Silverlight. Offre un modello di sviluppo basato su finestra di progettazione WYSIWYG, pertanto viene usato di frequente con gli sviluppatori che desiderano per un ambiente di sviluppo (RAD) rapido di applicazioni per lo sviluppo web. Se si ha esperienza di programmazione web e si ha familiarità con gli strumenti di sviluppo di client Microsoft RAD tradizionale (ad esempio, per Visual Basic e Visual c#), è possibile compilare rapidamente un'applicazione web senza la necessità di esperienza in HTML e JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC è destinata agli sviluppatori che desiderano principi come sviluppo basato su test, la separazione dei compiti, inversione di controllo (IoC) e l'inserimento di dipendenze (DI) e i modelli. Questo framework incoraggia che separa il livello di logica di business di un'applicazione web dal relativo livello di presentazione.
> - [ASP.NET Web Pages](../../../../web-pages/index.md)  
>  ASP.NET Web Pages è destinata agli sviluppatori che desiderano una storia di sviluppo web semplice, lungo le righe di PHP. Nel modello di pagine Web, si creano pagine HTML e aggiunta quindi codice basato su server alla pagina per controllare in modo dinamico la modalità con cui viene eseguito il rendering di markup. Pagine Web è progettato specificamente per essere un framework semplice ed è il punto di ingresso più semplice in ASP.NET per persone che conoscono HTML ma potrebbero non disporre ampia esperienza di programmazione, ad esempio, gli studenti o appassionati. È anche un ottimo modo per gli sviluppatori web che conoscono PHP o Framework simile per iniziare a usare ASP.NET.
> - [Applicazione a pagina singola di ASP.NET](../../../../single-page-application/index.md)  
>  Applicazione a pagina singola di ASP.NET (SPA) consente di compilare applicazioni che includono significativo sul lato client, le interazioni con HTML 5, 3 CSS e JavaScript. ASP.NET e aggiornamento di 2012.2 degli strumenti Web viene fornito un nuovo modello per la compilazione di applicazioni a pagina singola utilizzando knockout.js e ASP.NET Web API. Oltre al nuovo modello SPA, nuovi modelli SPA creati dalla community sono anche disponibili per il download.
> 
> Oltre a quattro Framework di sviluppo principale, ASP.NET offre tecnologie aggiuntive che sono importanti da considerare e abbia familiarità con, ma non sono trattate in questa serie di esercitazioni:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -un framework per compilare servizi HTTP in grado di raggiungere una vasta gamma di client, inclusi browser e dispositivi mobili.
> - [ASP.NET SignalR](../../../../signalr/index.md) -una libreria che semplifica la funzionalità di sviluppo web in tempo reale.


### <a name="reviewing-the-project"></a>Revisione del progetto

In Visual Studio, il **Esplora** finestra consente di gestire file per il progetto. Esaminiamo le cartelle che sono stati aggiunti all'applicazione in **Esplora**. Il modello di applicazione web aggiunge una struttura di cartelle di base:

![Creare il progetto - Esplora soluzioni](create-the-project/_static/image5.png)

Visual Studio crea alcuni file per il progetto e le cartelle iniziali. I file prima che si utilizzeranno con più avanti in questa esercitazione sono i seguenti:

| **File** | **Scopo** |
| --- | --- |
| *Default.aspx* | In genere la prima pagina visualizzata quando l'applicazione viene eseguita in un browser. |
| *Site.Master* | Una pagina che consente di creare un comportamento standard di layout e l'utilizzo coerente per le pagine dell'applicazione. |
| *Global.asax* | Un file facoltativo che contiene il codice di risposta agli eventi a livello di applicazione e a livello di sessione generati da ASP.NET o moduli HTTP. |
| *Web.config* | I dati di configurazione per un'applicazione. |

### <a name="running-the-default-web-application"></a>Esegue l'applicazione Web predefinita

L'applicazione Web predefinita fornisce un'esperienza avanzata basata su supporto e le funzionalità predefinite. Senza apportare modifiche al progetto predefinito Web Form, l'applicazione è pronta per essere eseguita nel browser Web locale.

1. Premere il ***F5*** chiave mentre in Visual Studio.   
 Compilare l'applicazione e visualizzato nel browser Web.  

    ![Creare il progetto - pagina predefinita](create-the-project/_static/image6.png)
2. Dopo aver completato la revisione dell'applicazione in esecuzione, chiudere la finestra del browser.

Esistono tre pagine principale in questa applicazione Web predefinita: *Default.aspx* (Home), *About*, e *Contact.aspx*. Ognuna di queste pagine può essere raggiunto nella barra di spostamento superiore. Esistono inoltre due pagine aggiuntive contenute nella cartella Account, le pagine Register e Login. Queste due pagine consentono di utilizzare le funzionalità di appartenenza di ASP.NET per creare, archiviare e convalidare le credenziali utente.

## <a name="aspnet-web-forms-background"></a>Web Form ASP.NET in Background

Web Form ASP.NET sono pagine che sono basate sulla tecnologia Microsoft ASP.NET, in cui il codice eseguito sul server in modo dinamico genera output delle pagine Web nel dispositivo client o browser. Una pagina Web Form ASP.NET esegue automaticamente il rendering dell'HTML conforme al browser corretto per funzionalità quali gli stili, layout e così via. Web Form sono compatibili con qualsiasi linguaggio supportato da .NET common language runtime, ad esempio Microsoft Visual Basic e Microsoft Visual c#. Inoltre, Web Form sono compilati sul [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), che offre i vantaggi, ad esempio un ambiente gestito, indipendenza dai tipi e l'ereditarietà.

Quando una pagina Web Form ASP.NET in esecuzione, la pagina passa attraverso un ciclo di vita in cui esegue una serie di passaggi di elaborazione. Tali passaggi includono l'inizializzazione, creazione di controlli, ripristino e mantenimento dello stato, l'esecuzione di codice del gestore eventi e il rendering. Dopo aver acquisito familiarità con la potenza di Web Form ASP.NET, è importante comprendere il [ciclo di vita della pagina ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) in modo che sia possibile scrivere codice fase del ciclo di vita appropriato per l'effetto desiderato.

Quando un server Web riceve una richiesta per una pagina, trova la pagina, viene elaborato, invia al browser e quindi lo ignora tutte le informazioni di pagina. Se l'utente richiede nuovamente la stessa pagina, il server si ripete l'intera sequenza, la rielaborazione da zero. In altre parole, un server non ha memoria delle pagine che dispone di pagine elaborati sono senza stato. Il framework della pagina ASP.NET gestisce automaticamente le attività di mantenere lo stato della pagina e i relativi controlli e consente di mantenere lo stato di informazioni specifiche dell'applicazione in modo esplicito.

> [!TIP] 
> 
> **Funzionalità dell'applicazione Web nel modello di applicazione Web Form**
> 
> Il modello di applicazioni Web Form ASP.NET fornisce un set completo di funzionalità predefinite. Fornisce non solo con un *aspx* pagina, un *About* pagina, un *Contact.aspx* pagina, ma include anche funzionalità di appartenenza che registra gli utenti e Salva le credenziali in modo che l'accesso al sito Web. Questa panoramica fornisce ulteriori informazioni su alcune delle funzionalità contenute nel modello di applicazioni Web Form ASP.NET e come vengono utilizzati nell'applicazione Wingtip Toys.
> 
> **Appartenenza**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) identità archivia le credenziali degli utenti in un database creato dall'applicazione. Quando l'accesso, l'applicazione convalida le credenziali per la lettura del database. Il progetto *Account* cartella contenente i file che implementano le varie parti di appartenenza: la registrazione, accesso, la modifica di una password e autorizzare l'accesso. Inoltre, Web Form ASP.NET supporta OAuth e OpenID. Questi miglioramenti di autenticazione consentono agli utenti di accedere al sito utilizzando le credenziali esistenti, tali account come Facebook, Twitter, Windows Live e Google.
> 
> ![Creare il progetto - Esplora soluzioni (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Per impostazione predefinita, il modello crea un database delle appartenenze utilizzando un nome predefinito del database in un'istanza di SQL Server Express LocalDB, il server di database di sviluppo che viene fornito con Visual Studio Express 2013 per Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) è una versione leggera di SQL Server che ha molte funzionalità di programmabilità di un database di SQL Server. SQL Server Express LocalDB viene eseguito in modalità utente e dispone di un'installazione veloce senza operazioni di configurazione con un breve elenco di prerequisiti di installazione. In Microsoft SQL Server, qualsiasi database o il codice Transact-SQL può essere spostato da SQL Server Express LocalDB a SQL Server e SQL Azure senza alcun passaggio di aggiornamento. In tal caso, SQL Server Express LocalDB è utilizzabile come un ambiente di sviluppo per applicazioni destinate a tutte le edizioni di SQL Server. SQL Server Express LocalDB Abilita le funzionalità, ad esempio stored procedure, funzioni definite dall'utente e funzioni di aggregazione, integrazione di .NET Framework, i tipi spaziali e ad altri utenti che non sono disponibili in SQL Server Compact.
> 
> **Pagine master**
> 
> Un [pagina master ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definisce un aspetto coerenza e il comportamento per tutte le pagine dell'applicazione. Il layout della pagina master unisce con il contenuto da una singola pagina contenuto per produrre la pagina finale visualizzato all'utente. Nell'applicazione Wingtip Toys, modifica il *Site. master* pagina master, in modo che tutte le pagine nel sito Web Wingtip Toys condividono la barra stesso logo e navigazione distintivo.
> 
> **HTML5**
> 
> Il modello di applicazioni Web Form ASP.NET supporta [HTML5](http://www.w3schools.com/html/html5_intro.asp), ovvero la versione più recente del linguaggio di markup HTML. HTML5 supporta nuovi elementi e le funzionalità che rendono più semplice creare siti Web.
> 
> **Modernizr**
> 
> Per i browser che non supportano HTML5, è possibile utilizzare [Modernizr](http://www.modernizr.com/). Modernizr è una libreria JavaScript open source in grado di rilevare se un browser supporta le funzionalità di HTML5 e attivarli in caso contrario. Nel modello di applicazioni Web Form ASP.NET, Modernizr viene installato come un pacchetto NuGet.
> 
> **Bootstrap**
> 
> Utilizzano i modelli di progetto di Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un framework di layout e i temi creato da Twitter. Bootstrap utilizza CSS3 per fornire una progettazione reattiva, ovvero layout può adattarsi dinamicamente alle dimensioni della finestra del browser diverse. Inoltre, è possibile utilizzare funzionalità dei temi del Bootstrap facilmente attuare una modifica nell'aspetto dell'applicazione. Per impostazione predefinita, il modello di applicazione Web ASP.NET in Visual Studio 2013 include Bootstrap come pacchetto NuGet.
> 
> **Pacchetti NuGet**
> 
> Il modello di applicazioni Web Form ASP.NET include un set di [NuGet](http://www.nuget.org/) pacchetti. Questi pacchetti è costituito da componenti funzionalità sotto forma di strumenti e librerie open source. È un'ampia gamma di pacchetti consentono di creare e testare le applicazioni. Visual Studio rende più semplice aggiungere, rimuovere e aggiornare i pacchetti NuGet. Gli sviluppatori possono creare e aggiungere anche i pacchetti di NuGet.
> 
> ![Creare il progetto - finestra di dialogo NuGet](create-the-project/_static/image8.png)
> 
> Quando si installa un pacchetto, NuGet consente di copiare i file di soluzione e di automaticamente tutte le modifiche sono necessarie, ad esempio l'aggiunta di riferimenti e la modifica della configurazione associata all'applicazione Web. Se si decide di rimuovere la libreria, NuGet consente di rimuovere i file e inverte le modifiche apportata nel progetto in modo che non viene lasciato confusione. NuGet è disponibile il **strumenti** menu in Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) è una libreria JavaScript veloce e conciso che semplifica l'attraversamento documento HTML, la gestione degli eventi, l'animazione e interazioni di Ajax per lo sviluppo rapido di web. La libreria JavaScript jQuery è incluso nel modello di applicazioni Web Form ASP.NET come pacchetto NuGet.
> 
> **Convalida non intrusiva**
> 
> Controlli di convalida predefinite sono stati configurati per l'utilizzo di JavaScript non intrusivo per la logica di convalida sul lato client. Questo notevolmente riduce la quantità di codice JavaScript eseguito il rendering inline nel markup della pagina e riduce le dimensioni complessive della pagina. Convalida non intrusiva viene aggiunto a livello globale per il modello di applicazioni Web Form ASP.NET in base all'impostazione nel &lt;appSettings&gt; elemento del *Web. config* file alla radice dell'applicazione.
> 
> **Entity Framework Code First**
> 
> Oltre alle funzionalità del modello di applicazioni Web Form ASP.NET, l'applicazione Wingtip Toys utilizza [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), che è una libreria di NuGet che consente di sviluppo basato su codice quando si lavora con dati. In breve, viene creata la parte del database dell'applicazione per l'utente in base al codice da scrivere. Tramite Entity Framework, recuperare e manipolare i dati come oggetti fortemente tipizzati. Ciò consente di concentrarsi sulla logica di business nell'applicazione anziché con i dettagli della modalità di accesso ai dati.
> 
> Per ulteriori informazioni sulle librerie installate e pacchetti inclusi con il modello Web Form ASP.NET, vedere l'elenco dei pacchetti NuGet installati. A tale scopo, In Visual Studio creare un nuovo progetto di Web Form, seleziona **strumenti**  - &gt; **Gestione pacchetti libreria**  - &gt; **Gestisci Pacchetti di NuGet per la soluzione**e selezionare **i pacchetti installati** nel **Gestisci pacchetti NuGet** la finestra di dialogo.


### <a name="touring-visual-studio"></a>Touring Visual Studio

Windows principale in Visual Studio includono il **Esplora soluzioni**, **Esplora Server** (**Esplora Database** in Express), il **proprietà Finestra**, **della casella degli strumenti**, il **barra degli strumenti**e **finestra documento**.

![Creare il progetto - finestra di dialogo NuGet](create-the-project/_static/image9.png)

Per ulteriori informazioni su Visual Studio, vedere [Visual Guida di Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Riepilogo

In questa esercitazione sono creati esaminate e, eseguire l'applicazione Web Form predefinito. Le diverse funzionalità dell'applicazione Web Form predefinita verificati e appreso alcune nozioni di base su come usare l'ambiente di Visual Studio. Nelle esercitazioni seguenti si creeranno il livello di accesso ai dati.

## <a name="additional-resources"></a>Risorse aggiuntive

[Scelta del modello di programmazione](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Progetti di applicazione Web e progetti di siti Web](https://msdn.microsoft.com/library/dd547590.aspx)   
[Web Form ASP.NET pagine Panoramica](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Precedente](introduction-and-overview.md)
> [Successivo](create_the_data_access_layer.md)
