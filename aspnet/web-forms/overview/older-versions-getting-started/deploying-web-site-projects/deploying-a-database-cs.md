---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Distribuzione di un Database (c#) | Microsoft Docs
author: rick-anderson
description: Distribuzione di un'applicazione web ASP.NET comporta ottenendo i file necessari e le risorse dall'ambiente di sviluppo all'ambiente di produzione. Per da...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: fe85e583618cbab772c788f9a53509000aa415c5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386536"
---
<a name="deploying-a-database-c"></a>Distribuzione di un Database (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Distribuzione di un'applicazione web ASP.NET comporta ottenendo i file necessari e le risorse dall'ambiente di sviluppo all'ambiente di produzione. Per le applicazioni web basate sui dati sono inclusi lo schema di database e i dati. Questa esercitazione è il primo di una serie che illustra i passaggi necessari per distribuire correttamente il database dall'ambiente di sviluppo nell'ambiente di produzione.


## <a name="introduction"></a>Introduzione

Distribuzione di un'applicazione web ASP.NET comporta ottenendo i file necessari e le risorse dall'ambiente di sviluppo all'ambiente di produzione. Nel corso delle ultime sei esercitazioni abbiamo esaminato la distribuzione di una semplice applicazione web le recensioni dei libri. Questo sito demo era costituito da un numero di risorse sul lato server - pagine ASP.NET, i file di configurazione, un `Web.sitemap` file e così via, e le risorse lato client, come immagini e file CSS. Ma per quanto riguarda basate sui dati di applicazioni web? Quali passaggi aggiuntivi devono essere eseguite per distribuire un'applicazione web che utilizza un database?

Tramite le esercitazioni più avanti vengono verranno discussi i passaggi necessari per distribuire un'applicazione web basata sui dati. Questa esercitazione inizia esaminando come ottenere uno schema di database s e il contenuto dall'ambiente di sviluppo all'ambiente di produzione, mentre l'esercitazione successiva esamina le modifiche di configurazione necessari. Dopo che esploreremo sfide della distribuzione di un database che utilizza i servizi dell'applicazione (appartenenza, ruoli, profilo e così via).

## <a name="examining-the-updated-book-reviews-web-application"></a>Esaminare l'applicazione Web di revisioni Rubrica aggiornata

Per illustrare la distribuzione di un'applicazione web basata sui dati, è stato aggiornato l'applicazione web le recensioni dei libri da un sito Web semplice e statici a uno basato sui dati. Come in precedenza, sono disponibili due versioni dell'applicazione in questo download di esercitazione s: uno che utilizza il modello di progetto di applicazione Web e uno che usa il modello di progetto di sito Web.

Applicazione web di aggiornate le recensioni dei libri una [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) database, in cui è archiviato nel sito s `App_Data` cartella (`~/App_Data/Reviews.mdf`). Se hai installato nel computer SQL Server 2008 la demo dovrebbe funzionare senza errori. Se si dispone di una versione precedente di SQL Server è possibile installare il disponibile SQL Server 2008 Express Edition o è possibile usare il database di scaricare gli script disponibili in questa esercitazione s per creare il database manualmente.

Il `Reviews.mdf` database contiene quattro tabelle:

- `Genres` -include un record per ogni genere, ad esempio di tecnologia, fittizio e Business.
- `Books` -include un record per ogni recensione, con le colonne, ad esempio `Title`, `GenreId`, `ReviewDate`, e `Review`, tra gli altri.
- `Authors` -include informazioni su ogni autore che ha contribuito a un libro rivisto.
- `BooksAuthors` -una tabella di join molti-a-molti che specifica quali autori hanno scritto quali libri.
  

Figura 1 mostra un diagramma di Expressroute per queste quattro tabelle.


[![L'applicazione Web di Book Reviews Database è comprensivo di quattro tabelle](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Figura 1**: l'applicazione Web le verifiche di libro s Database è costituita da di quattro tabelle ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image3.jpg))


La versione precedente del sito Web le recensioni dei libri aveva una pagina ASP.NET separata per ogni libro. Ad esempio, si è verificato una pagina denominata `~/Tech/TYASP35.aspx` che conteneva la revisione *insegnare manualmente ASP.NET 3.5 in 24 ore*. Questa nuova versione del sito Web basato sui dati con le verifiche archiviate nel database e una singola pagina ASP.NET, Review.aspx?ID=*bookId*, che consente di visualizzare la revisione al libro specificato. Analogamente, è presente un Genre.aspx?ID=*genreId* pagina che elenca i libri esaminati nel genere specificato.

Figure 2 e 3 mostra le `Genre.aspx` e `Review.aspx` pagine in azione. Prendere nota dell'URL nella barra degli indirizzi per ogni pagina. Nella figura 2 it s Genre.aspx? ID = c 47-82a0-c8ec75de7e0e 85d164ba-1123-4. Poiché è 85d164ba-1123-4c47-82a0-c8ec75de7e0e il `GenreId` valore per il genere di tecnologia, le letture di intestazione di pagina s "Technology esamina" e nell'elenco puntato enumera le recensioni sul sito che rientrano in questo genere.


[![La pagina delle tecnologie di genere](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Figura 2**: la pagina delle tecnologie di genere ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image6.jpg))


[![La verifica per apprendere ASP.NET 3.5 in 24 ore](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Figura 3**: la recensione relativa *insegnare manualmente ASP.NET 3.5 in 24 ore* ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image9.jpg))


Il libro esamina applicazione web include anche una sezione di amministrazione in cui gli amministratori possono aggiungere, modificare e generi, revisioni, eliminare e creare le informazioni. Attualmente, qualsiasi visitatore può accedere alla sezione di amministrazione. In un'esercitazione futura verrà aggiunto il supporto per gli account utente e consentono solo gli utenti autorizzati nelle pagine di amministrazione.

Se si scarica l'applicazione le recensioni dei libri, tenere presente che il suo scopo consiste nel dimostrare la distribuzione di un'applicazione basata su dati. Non offre procedure consigliate per quanto riguarda progettazione dell'applicazione. Non è ad esempio, non separato Data Access Layer (DAL); le pagine ASP.NET di comunicare direttamente con il database tramite il controllo SqlDataSource o codice ADO.NET nelle relative classi di code-behind. Per informazioni più dettagliate sulla creazione di applicazioni basate sui dati usando un'architettura a più livelli, fare riferimento alla mia [ *utilizzo di dati* esercitazioni](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Database in sviluppo o produzione

Quando si avvia lo sviluppo in un'applicazione web basata sui dati è necessario specificare una stringa di connessione del database, che fornisce i dettagli dell'applicazione per la connessione al database. Questa stringa di connessione specifica, tra altre cose, il server di database, il nome del database e le informazioni di sicurezza. In genere, il database utilizzato dall'applicazione durante lo sviluppo è diverso da quello utilizzato quando il database è s nell'ambiente di produzione. Esistono diversi vantaggi dell'uso di database diversi per lo sviluppo e produzione. Con un altro database nel mezzo di sviluppo a cui si don t necessario preoccuparsi accidentalmente modificare o eliminare dati in tempo reale. Permette anche di inserire nei dati di test fittizio o apportare modifiche di rilievo al modello di dati senza doversi preoccupare gli effetti sull'applicazione nell'ambiente di produzione. Lo svantaggio di disporre di un database diverso in ambienti di sviluppo e produzione che quando l'applicazione è distribuito il database e tutte le modifiche pertinenti allo schema del database s o devono essere distribuiti anche i dati.

Prima della prima distribuzione, c'è solo un'istanza del database e tale istanza sia nell'ambiente di sviluppo. Quando si distribuisce l'applicazione nell'ambiente di produzione per la prima volta, è necessario non solo copiare i file lato client e lato server necessari, ma anche copiare il database dall'ambiente di sviluppo all'ambiente di produzione. Si tratta di dove si indicano a destra ora con l'applicazione web le recensioni dei libri - il database si trova nel `App_Data` cartella nell'ambiente di sviluppo, ma non ha ancora stato eseguito il push nell'ambiente di produzione.

Dopo aver distribuita l'applicazione sono presenti due copie del database. Come l'applicazione viene perfezionata, è possibile aggiungere nuove funzionalità, è necessario eseguire una modifica al modello di dati (ad esempio aggiungendo nuove colonne alle tabelle esistenti, apportare modifiche alle colonne esistenti, aggiunta di nuove tabelle e così via). Quando l'applicazione web viene successivamente distribuita, le modifiche applicate al database nell'ambiente di sviluppo poiché l'ultima distribuzione deve essere applicato al database di produzione. Alcune strategie per la gestione di questo processo sono illustrati in un'esercitazione futura. Questa esercitazione è incentrata sulla distribuzione dell'intero database dall'ambiente di sviluppo nell'ambiente di produzione.

## <a name="deploying-the-database-to-the-production-environment"></a>Distribuzione del Database nell'ambiente di produzione

Nella parte restante di questa esercitazione verrà illustrato come distribuire il database dall'ambiente di sviluppo all'ambiente di produzione. Se si segue lungo è necessario assicurarsi che l'account con il provider di hosting web include Microsoft SQL Server database supportano. È anche necessario avere alcune informazioni in questione, vale a dire il nome del server di database, il nome del database e il nome utente e password utilizzata per connettersi al database.

Come accennato in precedenza in questa esercitazione, il database di sito Web s le recensioni dei libri è archiviato in un database di SQL Server 2008 Express Edition il `App_Data` cartella. Sarebbe in evidenza al motivo che la distribuzione di un database di questo tipo sarebbe semplice quanto la copia di `App_Data` cartella dall'ambiente di sviluppo all'ambiente di produzione. Tuttavia, la maggior parte dei provider di host web non supportano i database di hosting nel `App_Data` cartella motivi di sicurezza. Host web, invece, specificare un account in un server di database di SQL Server nel proprio ambiente. Distribuzione del database dall'ambiente di sviluppo all'ambiente di produzione, è necessario ottenere il database registrato nel server di database web host s.

Quindi, come ottenere il database dall'ambiente di sviluppo all'ambiente di produzione? Esistono un paio di modi per eseguire questa operazione a seconda di quali servizi le offerte di host web. Con alcuni host, ad esempio DiscountASP.NET, è possibile FTP un backup del database o effettivi `.mdf` file nel sito Web e quindi ripristinare il file di backup dal Pannello di controllo o collegare il `.mdf` file al server di database di SQL Server. Con questi strumenti di distribuzione del database è semplice quanto la copia di `App_Data` cartella in cui l'ambiente di produzione e quindi collegarlo tramite il pannello di controllo. Questo è probabilmente il modo più semplice e rapido per il database di pubblicazione per la prima volta.

Un altro approccio consiste nell'usare la pubblicazione guidata Database. La pubblicazione guidata Database è un'applicazione desktop di Windows che genera i comandi SQL per creare lo schema del database s - le tabelle, stored procedure, viste, funzioni definite dall'utente e così via - e, facoltativamente, i dati nelle tabelle di. È possibile quindi connettersi al server web host provider s database tramite SQL Server Management Studio e quindi eseguire questo script per duplicare il database in ambiente di produzione. Meglio ancora, il provider di hosting web supporta Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) è possibile avere lo script generato dalla creazione guidata pubblicazione Database eseguiti automaticamente nel server di database per tuo conto. Perché la pubblicazione guidata Database genera uno script che crea i dati e schema database s, funzionerà indipendentemente dal fatto che il provider di hosting web offre funzionalità come l'associazione un caricato `.mdf` file.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generazione di comandi SQL per creare lo Schema di Database e i dati usando la pubblicazione guidata Database

Let s illustrato l'utilizzo di Database Publishing Wizard per distribuire il database le recensioni dei libri nell'ambiente di produzione. Se si usa Visual Studio 2008 o versioni successive, la pubblicazione guidata Database è già installata. Se si usa Visual Studio 2005, è necessario innanzitutto [scaricare e installare](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) la procedura guidata.

Aprire Visual Studio e passare al `Reviews.mdf` database. Se si utilizza Visual Web Developer, passare a Esplora risorse di Database. Se si usa Visual Studio, usare Esplora Server. La figura 4 illustra il `Reviews.mdf` database in Database Explorer in Visual Web Developer. Come illustrato nella figura 4, il `Reviews.mdf` database è costituito da quattro tabelle, tre stored procedure e una funzione definita dall'utente.


[![Individuare il Database in cui il Database Explorer o Esplora Server](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Figura 4**: individuare il Database in cui il Database Explorer o Esplora Server ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image12.jpg))


Fare doppio clic sul nome del database e scegliere l'opzione "Pubblica sul provider" dal menu di scelta rapida. Verrà avviata la pubblicazione guidata Database (vedere la figura 5). Fare clic su Avanti per passare superato la schermata iniziale.


[![Database Publishing Wizard la schermata iniziale](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Figura 5**: il Database di pubblicazione guidata schermata iniziale ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image15.jpg))


La seconda schermata della procedura guidata elenca i database accessibili a Database Publishing Wizard e consente di scegliere se tutti gli oggetti nel database selezionato o scegliere quali oggetti allo script. Selezionare il database appropriato e lasciare selezionata l'opzione di "Script di tutti gli oggetti nel database selezionato".

> [!NOTE]
> Se viene visualizzato l'errore "non sono presenti oggetti nel database *databaseName* dei tipi gestibile tramite script da questa procedura guidata" quando facendo clic su Avanti nella schermata illustrata nella figura 6, assicurarsi che il percorso al file di database non sia troppo lungo. È stato individuato che questo errore può verificarsi se il percorso del file di database è troppo lungo.


[![Database Publishing Wizard la schermata iniziale](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Figura 6**: il Database di pubblicazione guidata schermata iniziale ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image18.jpg))


Nella schermata successiva è possibile generare un file di script o, se supporta l'host web, il database di pubblicazione direttamente al server di database web host provider s. Come illustrato nella figura 7, si è verificato lo script scritto nel file `C:\REVIEWS.MDF.sql`.


[![Il Database in un File di script o pubblicazione direttamente in Your Provider di hosting Web](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Figura 7**: il Database in un File di Script o pubblicazione direttamente in Your Provider di hosting Web ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image21.jpg))


La schermata successiva viene richiesto per un'ampia gamma di opzioni di scripting. È possibile specificare se lo script deve includere le istruzioni drop per rimuovere questi oggetti esistenti. Il valore predefinito è True, che è bene quando si distribuisce un database per la prima volta. È anche possibile specificare se il database di destinazione è SQL Server 2000, SQL Server 2005 o SQL Server 2008. Infine, è possibile indicare se generare lo script lo schema e dati, solo i dati o solo lo schema. Lo schema è la raccolta di oggetti di database, tabelle, stored procedure, viste e così via. I dati sono le informazioni che si trovano nelle tabelle.

Come illustrato nella figura 8, si è verificato la procedura guidata configurata per eliminare oggetti di database esistente, per generare script per un database di SQL Server 2008 e pubblicare lo schema e i dati.


[![Specificare la pubblicazione di opzioni](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Figura 8**: specificare le opzioni di pubblicazione ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image24.jpg))


Le due schermate finale riepilogano le azioni che verranno intraprese e quindi visualizzare lo stato della creazione di script. Il risultato dell'esecuzione della procedura guidata è che abbiamo un file di script che contiene i comandi SQL necessari per creare il database nell'ambiente di produzione che viene popolato con gli stessi dati come sullo sviluppo.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Esegue i comandi SQL nel Database ambiente di produzione

Ora che abbiamo lo script che contiene i comandi SQL per creare il database e i dati che rimane consiste nell'eseguire lo script nel database di produzione. Alcuni provider host web offre una casella di testo nel proprio pannello di controllo in cui è possibile immettere comandi SQL da eseguire sul database. Se si dispone di un file di script molto grandi, questa opzione potrebbe non funzionare (il `REVIEWS.MDF.sql` file di script è ad esempio dimensioni, oltre a 425 KB).

Un approccio migliore consiste nella connessione direttamente al server di database di produzione usando SQL Server Management Studio (SSMS). Se si dispone di un non Express Edition di SQL Server installata nel computer quindi probabilmente già installato SQL Server Management Studio. In caso contrario, è possibile [scaricare e installare](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) una copia gratuita di SQL Server Management Studio Express Edition.

Avviare SSMS e connettersi al server web host s database utilizzando le informazioni fornite dal provider host web.


[![Connettersi al Server di Database Web Host Provider s](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Figura 9**: connessione a Provider di hosting Web Your s Server di Database ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image27.jpg))


Espandere la scheda di database e individuare il database. Fare clic sul pulsante Nuova Query nell'angolo superiore sinistro della barra degli strumenti, incollare i comandi SQL dal file script creato dalla pubblicazione guidata Database e fare clic sul pulsante Esegui per eseguire questi comandi nel server di database di produzione. Se il file di script è particolarmente elevato potrebbe richiedere alcuni minuti per eseguire i comandi.


[![Connettersi al Server di Database Web Host Provider s](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Figura 10**: connessione a Provider di hosting Web Your s Server di Database ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image30.jpg))


Che s ne è a esso. A questo punto il database di sviluppo è stato duplicato nell'ambiente di produzione. Se si aggiorna il database in SQL Server Management Studio si dovrebbero vedere i nuovi oggetti di database. Figura 11 Mostra le tabelle di database s di produzione, stored procedure e funzioni definite dall'utente, che rispecchiano quelli nel database di sviluppo. E perché abbiamo indicato la pubblicazione guidata Database per pubblicare i dati, le tabelle di produzione del database s abbiano gli stessi dati come tabelle di sviluppo database s al momento che è stata eseguita la procedura guidata. Figura 12 mostra i dati nel `Books` tabella nel database di produzione.


[![Gli oggetti di Database sono stati duplicati nel Database di produzione](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Figura 11**: il Database di oggetti sono stati duplicati nel Database di produzione ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image33.jpg))


[![Il Database di produzione contiene gli stessi dati nel Database di sviluppo](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Figura 12**: il Database di produzione contiene gli stessi dati come nel Database di sviluppo ([fare clic per visualizzare l'immagine con dimensioni normali](deploying-a-database-cs/_static/image36.jpg))


A questo punto è stato distribuito solo nel database di sviluppo nell'ambiente di produzione. È in stato non ancora preso in esame la distribuzione dell'applicazione web stessa o esaminate le modifiche di configurazione necessarie affinché l'applicazione in produzione usare il database di produzione. Questi problemi verranno trattati nella prossima esercitazione.

## <a name="summary"></a>Riepilogo

Distribuzione di un'applicazione web basata sui dati richiede la copia del database utilizzato durante lo sviluppo nell'ambiente di produzione. Molti provider host web offre strumenti per semplificare il processo di distribuzione di un database. Ad esempio, con DiscountASP.NET FTP è il database `.mdf` file (o una copia di backup) e quindi collegare il database al server di database dal Pannello di controllo. Un'altra opzione che funziona indipendentemente dal fatto che le funzionalità delle offerte di provider host web è strumento pubblicazione guidata Database di Microsoft s, che genera uno script di comandi SQL per creare lo schema di s database di sviluppo e dati. Dopo che è stato generato lo script è possibile eseguirla nel database di produzione.

Ora che il database di s le recensioni dei libri dell'applicazione web è in produzione è possibile distribuire l'applicazione. Tuttavia, le informazioni di configurazione di web application s specificano la stringa di connessione al database e tale stringa di connessione fa riferimento al database di sviluppo. È necessario aggiornare queste informazioni sulla stringa di connessione durante la distribuzione del sito di produzione. L'esercitazione successiva esamina queste differenze di configurazione e descrive i passaggi necessari per pubblicare il sito delle recensioni dei libri basati sui dati nell'ambiente di produzione.

Buona programmazione!

#### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Scaricare Microsoft SQL Server Database Publishing Wizard 1.1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Scaricare Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Precedente](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [Successivo](configuring-the-production-web-application-to-use-the-production-database-cs.md)
