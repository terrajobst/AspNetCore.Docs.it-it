---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Distribuzione di un Database (c#) | Documenti Microsoft
author: rick-anderson
description: Distribuzione di un'applicazione web ASP.NET comporta l'uso di ottenere i file necessari e le risorse dall'ambiente di sviluppo all'ambiente di produzione. Per da...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: f71e3cd1e81644df7b3dfed363b6f2ca826e610d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-database-c"></a>Distribuzione di un Database (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Distribuzione di un'applicazione web ASP.NET comporta l'uso di ottenere i file necessari e le risorse dall'ambiente di sviluppo all'ambiente di produzione. Per le applicazioni web basate sui dati sono inclusi lo schema di database e i dati. In questa esercitazione è il primo di una serie in cui vengono illustrati i passaggi necessari per distribuire correttamente il database dall'ambiente di sviluppo alla produzione.


## <a name="introduction"></a>Introduzione

Distribuzione di un'applicazione web ASP.NET comporta l'uso di ottenere i file necessari e le risorse dall'ambiente di sviluppo all'ambiente di produzione. Nel corso delle sei esercitazioni precedenti abbiamo esaminato la distribuzione di una semplice applicazione web recensioni. Questo sito demo è costituito da un numero di risorse sul lato server, le pagine ASP.NET, i file di configurazione un `Web.sitemap` file e così via, insieme a risorse sul lato client, ad esempio immagini e file CSS. Ma cosa succede basati sui dati di applicazioni web? Per distribuire un'applicazione web che utilizza un database, è necessario eseguire i passaggi aggiuntivi?

Tramite le esercitazioni più avanti si risolverà i passaggi necessari per distribuire un'applicazione web basate sui dati. In questa esercitazione inizia esaminando come ottenere uno schema di database s e il contenuto dall'ambiente di sviluppo all'ambiente di produzione, mentre l'esercitazione successiva esamina le modifiche di configurazione necessari. Dopo che si esamineranno problematiche di distribuzione di un database che utilizza i servizi delle applicazioni (appartenenza, ruoli, profilo e così via).

## <a name="examining-the-updated-book-reviews-web-application"></a>Esaminare l'applicazione Web di revisioni Rubrica aggiornato

Per illustrare la distribuzione di un'applicazione web basate sui dati, si viene aggiornato l'applicazione web recensioni da un sito Web statico, semplice con una basati sui dati. Come prima, esistono due versioni dell'applicazione in questo download esercitazione s: uno che utilizza il modello di progetto di applicazione Web e uno che utilizza il modello di progetto di sito Web.

Applicazione web le recensioni aggiornato un [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) database, che viene archiviato nel sito s `App_Data` cartella (`~/App_Data/Reviews.mdf`). Se hai installato nel computer SQL Server 2008 la demo devono essere eseguite senza errori. Se si dispone di una versione precedente di SQL Server, è possibile installare il disponibile SQL Server 2008 Express Edition o è possibile utilizzare il database di scaricare gli script disponibili in questa esercitazione s per creare il database manualmente.

Il `Reviews.mdf` database contiene quattro tabelle:

- `Genres`-include un record per ogni genere, ad esempio di tecnologia, fittizio e Business.
- `Books`-include un record per ogni revisione, con le colonne come `Title`, `GenreId`, `ReviewDate`, e `Review`, tra gli altri.
- `Authors`-include informazioni su ogni autore che ha contribuito a un libro di revisione.
- `BooksAuthors`-una tabella di join molti-a-molti che specifica quali autori hanno scritto quali libri.
  

Figura 1 mostra un diagramma di queste quattro tabelle.


[![L'applicazione Web di revisioni Rubrica Database è composta da di quattro tabelle](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Figura 1**: l'applicazione Web di revisioni Rubrica Database è composta da di quattro tabelle ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image3.jpg))


La versione precedente del sito Web recensioni aveva una pagina ASP.NET separata per ogni libro. Ad esempio, si è verificato una pagina denominata `~/Tech/TYASP35.aspx` contenente la revisione per *insegna manualmente ASP.NET 3.5 nelle 24 ore*. Questa nuova versione del sito Web basato sui dati è archiviate nel database e una singola pagina ASP.NET, Review.aspx?ID= recensioni*bookId*, che consente di visualizzare la revisione per il runbook specificato. Analogamente, sussiste un Genre.aspx?ID=*genreId* pagina che elenca i libri di revisionati specificato appartenente al genere.

Nelle figure 2 e 3 mostra il `Genre.aspx` e `Review.aspx` in azione le pagine. Prendere nota dell'URL nella barra degli indirizzi per ogni pagina. Nella figura 2 it s Genre.aspx? ID = c 47-82a0-c8ec75de7e0e 85d164ba-1123-4. Poiché è 85d164ba-1123-4c47-82a0-c8ec75de7e0e il `GenreId` valore per il genere della tecnologia, le letture di intestazione di pagina s "Tecnologia esamina" e l'elenco puntato enumera le revisioni nel sito che rientrano in questo genere.


[![La pagina delle tecnologie di genere](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Figura 2**: la pagina delle tecnologie di genere ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image6.jpg))


[![La revisione per insegnare manualmente ASP.NET 3.5 in 24 ore](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Figura 3**: la revisione per *insegna manualmente ASP.NET 3.5 nelle 24 ore* ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image9.jpg))


L'applicazione web revisioni Rubrica include anche una sezione di amministrazione in cui gli amministratori possono aggiungere, modificare e generi, le revisioni, eliminare e creare informazioni. Attualmente, qualsiasi visitatore può accedere alla sezione di amministrazione. Nell'esercitazione future viene aggiunto il supporto per gli account utente e consentire solo agli utenti autorizzati nelle pagine Amministrazione.

Se si scarica l'applicazione recensioni, tenere presente che il suo scopo consiste nel dimostrare la distribuzione di un'applicazione basata sui dati. Non offre procedure consigliate per progettazione dell'applicazione. È ad esempio, non separato Data Access Layer (DAL); le pagine ASP.NET di comunicare direttamente con il database tramite il controllo SqlDataSource o codice ADO.NET nelle relative classi di codice. Per informazioni più dettagliate sulla creazione di applicazioni guidate dai dati utilizzando un'architettura a più livelli, fare riferimento a my [ *utilizzo dei dati* esercitazioni](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Database di sviluppo e produzione

Quando si avvia lo sviluppo di un'applicazione web guidate dai dati è necessario specificare una stringa di connessione di database, che fornisce i dettagli dell'applicazione per la connessione al database. Specifica la stringa di connessione, tra le altre cose, il server di database, il nome del database e informazioni di sicurezza. In genere, il database utilizzato dall'applicazione durante lo sviluppo è diverso da quello utilizzato quando il database è s nell'ambiente di produzione. Esistono numerosi vantaggi dell'utilizzo di database diversi per lo sviluppo e produzione. Con un altro database di sviluppo significa che si don t necessario preoccuparsi accidentalmente modifica o eliminazione di dati in tempo reale. È anche possibile inserire nei dati di test fittizio o apportare modifiche di rilievo apportate al modello di dati senza doversi preoccupare gli effetti dell'applicazione nell'ambiente di produzione. Lo svantaggio di disporre di un database diverso negli ambienti di sviluppo e produzione è che quando l'applicazione è distribuito il database e le relative modifiche allo schema del database s o devono essere distribuiti anche dati.

Prima della distribuzione iniziale, è solo un'istanza del database, e tale istanza nell'ambiente di sviluppo. Quando si distribuisce l'applicazione nell'ambiente di produzione per la prima volta, è necessario non solo copia dei file necessari sul lato server e lato client, ma anche copiare il database dall'ambiente di sviluppo all'ambiente di produzione. Si tratta di dove si indicano destra ora con l'applicazione web recensioni, cui si trova il database di `App_Data` la cartella in questo ambiente di sviluppo ma non ha ancora effettuato il push di nell'ambiente di produzione.

Una volta distribuito l'applicazione sono presenti due copie del database. Evoluzione dell'applicazione, possono essere aggiunte nuove funzionalità, necessitando un cambio al modello di dati (ad esempio aggiungendo nuove colonne alle tabelle esistenti, è necessario apportare modifiche alle colonne esistenti, aggiunta di nuove tabelle e così via). Quando l'applicazione web viene quindi distribuito, le modifiche applicate al database nell'ambiente di sviluppo, poiché l'ultima distribuzione, è necessario applicare al database di produzione. Alcune strategie per la gestione di questo processo vengono discussi in un'esercitazione in futura. In questa esercitazione viene illustrata la distribuzione dell'intero database dall'ambiente di sviluppo alla produzione.

## <a name="deploying-the-database-to-the-production-environment"></a>Distribuzione del Database nell'ambiente di produzione

Il resto di questa esercitazione verrà illustrato come distribuire il database dall'ambiente di sviluppo all'ambiente di produzione. Se si segue lungo è necessario per assicurarsi che l'account con il provider di hosting web include Microsoft SQL Server di database supportano. Sarà inoltre necessario alcune informazioni in questione, vale a dire il nome del server di database, il nome del database e il nome utente e password utilizzati per connettersi al database.

Come notato in precedenza in questa esercitazione, il database di sito Web s recensioni è archiviato in un database di SQL Server 2008 Express Edition di `App_Data` cartella. Sarebbe stand per motivo che la distribuzione di un database deve essere semplice come copiare il `App_Data` cartella dall'ambiente di sviluppo all'ambiente di produzione. Tuttavia, la maggior parte dei provider di host web non supportano database di hosting di `App_Data` cartella per motivi di sicurezza. Host web, invece, specificare un account in un server di database di SQL Server nel proprio ambiente. Distribuzione del database nell'ambiente di sviluppo all'ambiente di produzione, è necessario ottenere il database registrato nel server di database s host web.

Come ottenere il database dall'ambiente di sviluppo all'ambiente di produzione? Esistono un paio di modi per eseguire questa operazione, a seconda di quali servizi le offerte di host web. Con alcuni host, ad esempio DiscountASP.NET, è possibile FTP un backup del database o l'effettivo `.mdf` file al sito Web e quindi, dal Pannello di controllo, ripristinare il file di backup o allegare il `.mdf` file al server di database di SQL Server. Con questi strumenti di distribuzione del database è semplice come la copia di `App_Data` cartella per l'ambiente di produzione e quindi collegarlo tramite il pannello di controllo. Questo è probabilmente il modo più semplice e rapido per il database di pubblicazione per la prima volta.

Un altro approccio consiste nell'utilizzare la pubblicazione guidata Database. La pubblicazione guidata Database è un'applicazione desktop di Windows che genera comandi SQL per creare lo schema s del database - le tabelle, stored procedure, viste, funzioni definite dall'utente e così via - e, facoltativamente, i dati nelle tabelle di. È possibile quindi connettersi al server web host provider s database tramite SQL Server Management Studio e quindi eseguire questo script per duplicare il database in produzione. Inoltre, se il provider di hosting web supporta Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) è consentito lo script generato per la pubblicazione guidata Database viene eseguito automaticamente nel server di database per conto dell'utente. Poiché la pubblicazione guidata Database genera uno script che crea lo schema di database s e i dati, il corretto funzionamento indipendentemente dal fatto che il provider di hosting web offre funzionalità come l'associazione di un caricamento `.mdf` file.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generazione di comandi SQL per creare lo Schema di Database e i dati utilizzando la pubblicazione guidata Database

Consente di scorrere usando la pubblicazione guidata Database per distribuire il database recensioni nell'ambiente di produzione s. Se si utilizza Visual Studio 2008 o successivo, la pubblicazione guidata Database è già installata. Se si utilizza Visual Studio 2005, è necessario prima [scaricare e installare](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) la procedura guidata.

Aprire Visual Studio e passare il `Reviews.mdf` database. Se si utilizza Visual Web Developer, accedere a Esplora Database; Se si utilizza Visual Studio, è possibile utilizzare Esplora Server. La figura 4 illustra il `Reviews.mdf` database in Esplora Database in Visual Web Developer. Come illustrato nella figura 4, il `Reviews.mdf` database è costituito da quattro tabelle, le tre stored procedure e una funzione definita dall'utente.


[![Individuare il Database di Esplora Database o Esplora Server](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Figura 4**: individuare il Database di Esplora Database o Esplora Server ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image12.jpg))


Fare clic sul nome del database e scegliere l'opzione "Pubblica sul provider" dal menu di scelta rapida. Verrà avviata la pubblicazione guidata Database (vedere Figura 5). Fare clic su Avanti per passare oltre la schermata iniziale.


[![Il Database di pubblicazione guidata schermata](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Figura 5**: il Database di pubblicazione guidata schermata ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image15.jpg))


La seconda schermata della procedura guidata elenca i database accessibili per la pubblicazione guidata Database e consente di scegliere se creare script di tutti gli oggetti nel database selezionato o scegliere quali oggetti allo script. Selezionare il database appropriato e lasciare l'opzione "Script tutti gli oggetti nel database selezionato" selezionata.

> [!NOTE]
> Se viene visualizzato l'errore "non sono presenti oggetti nel database *databaseName* dei tipi di script da questa procedura guidata" quando facendo clic su Avanti nella schermata illustrata nella figura 6, assicurarsi che il percorso al file di database non è eccessivamente lungo. È stato rilevato che questo errore può verificarsi se il percorso del file di database è troppo lungo.


[![Il Database di pubblicazione guidata schermata](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Figura 6**: il Database di pubblicazione guidata schermata ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image18.jpg))


Nella schermata successiva è possibile generare un file di script o, se il web host lo supporta, è possibile pubblicare il database direttamente al server web host provider s database. Come illustrato nella figura 7, si è verificato lo script scritto nel file `C:\REVIEWS.MDF.sql`.


[![Il Database in un File di script o pubblicarlo direttamente per il Provider di hosting Web](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Figura 7**: il Database in un File di Script o pubblicarlo direttamente per il Provider di hosting Web ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image21.jpg))


La schermata successiva viene richiesto per un'ampia gamma di opzioni di scripting. È possibile specificare se lo script deve includere le istruzioni drop per rimuovere questi oggetti esistenti. L'impostazione predefinita è True, che è appropriato quando si distribuisce un database per la prima volta. È inoltre possibile specificare se il database di destinazione è SQL Server 2000, SQL Server 2005 o SQL Server 2008. Infine, è possibile indicare se generare lo script lo schema e i dati, solo i dati o solo lo schema. Lo schema è la raccolta di oggetti di database, tabelle, stored procedure, viste e così via. I dati sono le informazioni che si trovano nelle tabelle.

Come illustrato nella figura 8, si invitiamo inoltre la procedura guidata configurata per eliminare oggetti di database esistente, per generare script per un database di SQL Server 2008 e per la pubblicazione sia lo schema e dati.


[![La pubblicazione di specificare le opzioni](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Figura 8**: specificare le opzioni di pubblicazione ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image24.jpg))


Le due schermate finale riepilogano le azioni che verranno intraprese e quindi visualizzare lo stato di esecuzione di script. Il risultato dell'esecuzione della procedura guidata è che si dispone di un file di script che contiene i comandi SQL necessari per creare il database in produzione e popolarlo con gli stessi dati come sullo sviluppo.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>L'esecuzione di comandi SQL nel Database di ambiente di produzione

Dopo aver creato lo script che contiene i comandi SQL per creare il database e i dati che rimane consiste nell'eseguire lo script del database di produzione. Alcuni provider host web offrono una casella di testo nel proprio pannello di controllo in cui è possibile immettere comandi SQL da eseguire sul database. Se si dispone di un file di script di dimensioni molto grandi, questa opzione potrebbe non funzionare (il `REVIEWS.MDF.sql` file script è ad esempio, dimensioni, oltre a 425 KB).

Un approccio migliore consiste nel connettersi direttamente al server di database di produzione utilizzando SQL Server Management Studio (SSMS). Se si dispone di un non Express Edition di SQL Server installata nel computer quindi probabile che abbiano già installato con SQL Server Management Studio. In caso contrario, è possibile [scaricare e installare](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) una copia gratuita di SQL Server Management Studio Express Edition.

Avviare SQL Server Management Studio e connettersi al server web host s database utilizzando le informazioni fornite dal provider host web.


[![Connettersi al Server di Database Web Host Provider s](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Figura 9**: connettersi al Provider di hosting Web di s Server di Database ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image27.jpg))


Espandere la scheda di database e individuare il database. Fare clic sul pulsante Nuova Query nell'angolo superiore sinistro della barra degli strumenti, incollare nei comandi SQL dal file di script creato dalla pubblicazione guidata Database e fare clic sul pulsante Esegui per eseguire questi comandi nel server di database di produzione. Se il file di script è particolarmente elevato potrebbe richiedere alcuni minuti per eseguire i comandi.


[![Connettersi al Server di Database Web Host Provider s](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Figura 10**: connettersi al Provider di hosting Web di s Server di Database ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image30.jpg))


Sono disponibili tutte le che s è! A questo punto, il database di sviluppo è stato duplicato nell'ambiente di produzione. Se si aggiorna il database in SQL Server Management Studio si dovrebbero vedere i nuovi oggetti di database. Figura 11 Mostra le tabelle di database s di produzione, stored procedure e funzioni definite dall'utente, che rispecchiano quelli nel database di sviluppo. E poiché è strutturato il Database di pubblicazione guidata per pubblicare i dati, tabelle s del database di produzione hanno gli stessi dati di tabelle di sviluppo database s al momento che è stata eseguita la procedura guidata. Figura 12 illustra i dati di `Books` tabella nel database di produzione.


[![Gli oggetti di Database sono stati duplicati nel Database di produzione](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Figura 11**: il Database gli oggetti sono stati duplicati nel Database di produzione ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image33.jpg))


[![Il Database di produzione contiene gli stessi dati nel Database di sviluppo](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Figura 12**: il Database di produzione contiene gli stessi dati in Database di sviluppo ([fare clic per visualizzare l'immagine ingrandita](deploying-a-database-cs/_static/image36.jpg))


A questo punto è stato distribuito solo il database di sviluppo alla produzione. È non ancora abbiamo esaminato la distribuzione dell'applicazione web stessa o esaminare le modifiche di configurazione necessarie per disporre dell'applicazione in produzione di utilizzare il database di produzione. Questi problemi verranno descritte nella prossima esercitazione!

## <a name="summary"></a>Riepilogo

Distribuzione di un'applicazione web basate sui dati richiede una copia del database utilizzato durante lo sviluppo nell'ambiente di produzione. Molti provider di host web offrono gli strumenti per semplificare il processo di distribuzione di un database. Ad esempio, con DiscountASP.NET FTP è il database `.mdf` file (o un backup) e quindi collegare il database al server di database dal Pannello di controllo. Un'altra opzione che funziona indipendentemente dalla funzionalità del provider di host web offre è strumento pubblicazione guidata Database di Microsoft s, che genera uno script di comandi SQL per creare lo schema di s database di sviluppo e i dati. Una volta che è stato generato lo script è possibile eseguirla nel database di produzione.

Ora che il database di s recensioni dell'applicazione web è in produzione è possibile distribuire l'applicazione. Tuttavia, le informazioni di configurazione s applicazione web specificano la stringa di connessione al database e la stringa di connessione fa riferimento al database di sviluppo. È necessario aggiornare tali informazioni di stringa di connessione quando si distribuisce il sito di produzione. L'esercitazione successiva esamina le differenze di configurazione e vengono illustrati i passaggi necessari per pubblicare il sito di recensioni basati sui dati di produzione.

Buona programmazione!

#### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Scaricare Microsoft SQL Server Database Publishing Wizard 1.1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Scaricare Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

>[!div class="step-by-step"]
[Precedente](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
[Successivo](configuring-the-production-web-application-to-use-the-production-database-cs.md)
