---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Creazione dello Schema di appartenenza in SQL Server (c#) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione inizia esaminando le tecniche per l'aggiunta lo schema necessario per il database per utilizzare il provider SqlMembershipProvider. In seguito, si wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: e1aec1ffbb052901fe34389943b5bb1db58348a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400042"
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>Creazione dello Schema di appartenenza in SQL Server (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Questa esercitazione inizia esaminando le tecniche per l'aggiunta lo schema necessario per il database per utilizzare il provider SqlMembershipProvider. In seguito, verranno esaminare le tabelle principali nello schema e discutere lo scopo e l'importanza. Questa esercitazione termina con uno sguardo a come stabilire quale provider debba usare il framework di appartenenza di un'applicazione ASP.NET.


## <a name="introduction"></a>Introduzione

Due nelle esercitazioni precedenti esaminate tramite autenticazione basata su form per identificare i visitatori del sito Web. Il framework di autenticazione Form semplifica per gli sviluppatori per accedere un utente a un sito Web e per ricordarle tra visite di pagina tramite l'utilizzo di ticket di autenticazione. Il `FormsAuthentication` classe include metodi per generare il ticket e aggiungendolo al cookie del visitatore. Il `FormsAuthenticationModule` esamina tutte le richieste in ingresso e, per chi dispone di un ticket di autenticazione valido, crea e associa un `GenericPrincipal` e un `FormsIdentity` oggetto con la richiesta corrente. Autenticazione basata su form è semplicemente un meccanismo per concedere un ticket di autenticazione per un visitatore durante l'accesso in e, nelle richieste successive, l'analisi di tale ticket per determinare l'identità dell'utente. Per un'applicazione web supportare gli account utente, è comunque necessario implementare un archivio utente e aggiungere funzionalità a convalidare le credenziali, registrare nuovi utenti e la miriade di altre attività relative all'account dell'utente.

Prima di ASP.NET 2.0, gli sviluppatori dovevano l'hook per implementare tutte queste attività relative all'account utente. Per fortuna il team ASP.NET riconosciuto questo difetto e introdotto il framework di appartenenza con ASP.NET 2.0. Il framework di appartenenza è un set di classi che forniscono un'interfaccia programmatica per portare a termine le attività relative all'account utente di core in .NET Framework. Questo framework è incorporato nella parte superiore di [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che consente agli sviluppatori di collegare un'implementazione personalizzata in un'API standardizzata.

Come descritto nel <a id="Tutorial1"> </a> [ *nozioni fondamentali sulla sicurezza e supporto di ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) esercitazione .NET Framework viene fornito con due provider di appartenenze predefinito: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) e [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Come suggerisce il nome, il `SqlMembershipProvider` Usa un database Microsoft SQL Server come archivio dell'utente. Per usare questo provider in un'applicazione, è necessario indicare al provider quale database da usare come archivio. Come si può immaginare, la `SqlMembershipProvider` prevede che il database dell'archivio utente avere determinate tabelle di database, viste e stored procedure. È necessario aggiungere lo schema previsto per il database selezionato.

Questa esercitazione inizia esaminando le tecniche per l'aggiunta di schema necessari al database per usare il `SqlMembershipProvider`. In seguito, verranno esaminare le tabelle principali nello schema e discutere lo scopo e l'importanza. Questa esercitazione termina con uno sguardo a come stabilire quale provider debba usare il framework di appartenenza di un'applicazione ASP.NET.

Iniziamo!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Passaggio 1: Decidere dove posizionare la Store utente

Dati di un'applicazione ASP.NET sono in genere archiviati in un numero di tabelle in un database. Quando si implementa il `SqlMembershipProvider` lo schema del database è necessario decidere se inserire lo schema di appartenenza nello stesso database i dati dell'applicazione o in un database alternativo.

Consiglia di individuazione dello schema di appartenenza nello stesso database i dati dell'applicazione per i motivi seguenti:

- **Manutenibilità** un'applicazione la cui data è incapsulato in un database è più facile da comprendere, gestire e distribuire un'applicazione che ha due distinti database.
- **Integrità relazionale** individuando le tabelle correlate alla appartenenza nello stesso database, come l'applicazione di tabelle, è possibile stabilire [vincoli foreign key](http://en.wikipedia.org/wiki/Foreign_key) tra le chiavi primarie di Tabelle correlate ai appartenenza e le tabelle dell'applicazione correlato.

Separando i dati di archivio e dell'applicazione utente in database separati ha senso solo se sono presenti più applicazioni ognuna Usa database separati, ma desidera condividere un archivio utente comune.

### <a name="creating-a-database"></a>Creazione di un Database

L'applicazione di che sviluppo poiché nella seconda esercitazione non è ancora necessario un database. È necessario uno a questo punto, tuttavia, per l'archivio utente. È possibile crearne uno e quindi aggiungervi lo schema richiesto dal `SqlMembershipProvider` provider (vedere il passaggio 2).

> [!NOTE]
> In questa serie di esercitazioni si userà un [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) database per archiviare le tabelle dell'applicazione e il `SqlMembershipProvider` dello schema. Questa decisione è stata presa per due motivi: in primo luogo, a causa del costo relativo - gratuito - la Express Edition è la versione di SQL Server 2005; più readably accessibile in secondo luogo, i database di SQL Server 2005 Express Edition possono essere posizionati direttamente dell'applicazione web `App_Data` cartella, rendendola una passeggiata a comprimere il database e applicazioni web tra loro in un unico file ZIP e ridistribuirla senza eventuali istruzioni speciali o le opzioni di configurazione. Se si preferisce seguire la procedura Usa una versione non - Express Edition di SQL Server, è possibile. I passaggi sono praticamente identici. Il `SqlMembershipProvider` schema collaborerà con qualsiasi versione di Microsoft SQL Server 2000 e backup.


Da Esplora soluzioni, fare clic su di `App_Data` cartella e scegliere Aggiungi nuovo elemento. (Se non viene visualizzata un' `App_Data` cartella nel progetto, pulsante destro del mouse sul progetto in Esplora soluzioni, selezionare Aggiungi cartella ASP.NET e scegliere `App_Data`.) Dalla finestra di dialogo Aggiungi nuovo elemento, scegliere di aggiungere un nuovo Database SQL denominato `SecurityTutorials.mdf`. In questa esercitazione si aggiungerà il `SqlMembershipProvider` dello schema a questo database, nelle esercitazioni successive si creerà aggiuntivi le tabelle da acquisire i dati dell'applicazione.


[![Aggiungere un nuovo Database SQL, denominato Database SecurityTutorials.mdf nella cartella App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Figura 1**: aggiungere un nuovo Database SQL denominato `SecurityTutorials.mdf` del Database per il `App_Data` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))


Aggiunta di un database per il `App_Data` cartella lo include automaticamente nella vista Esplora Database. (Nella versione edizione non Express di Visual Studio, Esplora Database è detta Esplora Server.) Passare a Esplora Database ed espandere l'appena aggiunte dal `SecurityTutorials` database. Se non è possibile visualizzare Esplora Database nella schermata, passare al menu View e scegliere Database Explorer oppure premere Ctrl + Alt + S. Come illustrato nella figura 2, il `SecurityTutorials` database è vuoto, contiene alcun tabelle, nessuna visualizzazione e Nessuna stored procedure.


[![Il SecurityTutorials Database è attualmente vuoto](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Figura 2**: il `SecurityTutorials` Database è attualmente vuoto ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Passaggio 2: Aggiunta di`SqlMembershipProvider`dello Schema al Database

Il `SqlMembershipProvider` richiede un determinato set di tabelle, viste e stored procedure da installare nel database dell'archivio utente. Questi oggetti di database necessari possono essere aggiunti utilizzando la [ `aspnet_regsql.exe` strumento](https://msdn.microsoft.com/library/ms229862.aspx). Questo file si trova nel `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` cartella.

> [!NOTE]
> Il `aspnet_regsql.exe` strumento offre funzionalità di riga di comando e un'interfaccia utente grafica. L'interfaccia grafica è più semplice ed è ciò che verranno esaminate in questa esercitazione. L'interfaccia della riga di comando è utile quando l'aggiunta del `SqlMembershipProvider` schema deve essere automatizzate, ad esempio compilazione degli script o gli scenari di test automatizzati.


Il `aspnet_regsql.exe` viene usato per aggiungere o rimuovere *servizi delle applicazioni ASP.NET* a un database di SQL Server specificato. I servizi delle applicazioni ASP.NET includono gli schemi per la `SqlMembershipProvider` e `SqlRoleProvider`, con gli schemi per i provider basato su SQL per altri framework di ASP.NET 2.0. È necessario fornire due bit di informazioni per il `aspnet_regsql.exe` strumento:

- Se si vuole aggiungere o rimuovere i servizi delle applicazioni, e
- Il database da cui si desidera aggiungere o rimuovere lo schema di servizi dell'applicazione

Nella richiesta di conferma per il database da usare, la `aspnet_regsql.exe` strumento vengono poste Microsoft per fornire il nome del server si trova il database on, le credenziali di sicurezza per la connessione al database e il nome del database. Se si usa il non Express Edition di SQL Server, che sia già acquisito queste informazioni, poiché tratta delle stesse informazioni che è necessario fornire tramite una stringa di connessione quando si lavora con il database tramite una pagina web ASP.NET. Determinare il nome di database e server quando si usa un database di SQL Server 2005 Express Edition nel `App_Data` cartella, tuttavia, è un po' più complessa.

La sezione seguente esamina un modo semplice per specificare il nome di server e database per un database di SQL Server 2005 Express Edition nel `App_Data` cartella. Se non si usa SQL Server 2005 Express Edition è possibile proseguire con il passaggio di installazione della sezione servizi dell'applicazione.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Determinare il nome del Database per un Database SQL Server 2005 Express Edition nel Server di`App_Data`cartella

Per poter utilizzare il `aspnet_regsql.exe` strumento è necessario conoscere i nomi di server e database. Il nome del server è `localhost\InstanceName`. Molto probabilmente, il *InstanceName* è `SQLExpress`. Tuttavia, se SQL Server 2005 Express Edition è stato installato manualmente (vale a dire, sono stati non installata automaticamente durante l'installazione di Visual Studio), è possibile che è selezionato un nome istanza diverso.

È un po' più complicato determinare il nome del database. I database nel `App_Data` cartella hanno in genere un nome di database che include un [identificatore univoco globale](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) insieme al percorso del file di database. È necessario determinare questo nome di database per aggiungere lo schema di servizi dell'applicazione tramite `aspnet_regsql.exe`.

Il modo più semplice per verificare il nome del database è esaminarlo tramite SQL Server Management Studio. SQL Server Management Studio fornisce un'interfaccia grafica per la gestione dei database di SQL Server 2005, ma non viene fornito con la Express Edition di SQL Server 2005. La buona notizia è che [è possibile scaricare](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) il libero Express Edition di SQL Server Management Studio.

> [!NOTE]
> Se si dispone anche di una versione non - Express Edition di SQL Server 2005 installato sul desktop, quindi probabile che sia installata la versione completa di Management Studio. È possibile usare la versione completa per determinare il nome del database, seguendo gli stessi passaggi come indicato di seguito per l'edizione Express.


Per iniziare, chiudere Visual Studio per garantire che tutti i blocchi imposti da Visual Studio per il file di database vengano chiuse. Successivamente, avviare SQL Server Management Studio e connettersi al `localhost\InstanceName` database per SQL Server 2005 Express Edition. Come indicato in precedenza, è probabile è il nome dell'istanza `SQLExpress`. Per l'opzione di autenticazione, selezionare l'autenticazione di Windows.


[![Connettersi all'istanza di SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Figura 3**: connettersi all'istanza di SQL Server 2005 Express Edition ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))


Dopo la connessione all'istanza di SQL Server 2005 Express Edition, Management Studio consente di visualizzare le cartelle per i database, le impostazioni di sicurezza, gli oggetti di Server e così via. Se si espande la scheda di database si noterà che il `SecurityTutorials.mdf` database viene *non* registrato nell'istanza del database, è necessario collegare il database prima di tutto.

Pulsante destro del mouse sulla cartella database e scegliere Connetti dal menu di scelta rapida. Questo visualizzerà la finestra di dialogo Collega database. A questo punto, fare clic sul pulsante Aggiungi, selezionare il `SecurityTutorials.mdf` del database e fare clic su OK. Figura 4 mostra la finestra di dialogo Collega database dopo il `SecurityTutorials.mdf` database è stato selezionato. Figura 5 Mostra Esplora oggetti di Management Studio dopo che il database è stato collegato correttamente.


[![Collegare il Database SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Figura 4**: collegare il `SecurityTutorials.mdf` Database ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))


[![Il Database SecurityTutorials.mdf viene visualizzato nella cartella di database](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Figura 5**: il `SecurityTutorials.mdf` Database viene visualizzato nella cartella di database ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))


Come illustrato nella figura 5, il `SecurityTutorials.mdf` database ha un nome piuttosto abstruse. È possibile modificarlo per una più facili da ricordare (e facile da digitare) nome. Pulsante destro del mouse sul database, scegliere di ridenominazione dal menu di scelta rapida e rinominarlo `SecurityTutorialsDatabase`. Ciò non modifica il nome del file, solo il nome del database utilizza per identificarsi con SQL Server.


[![Rinominare il Database con SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Figura 6**: rinominare il Database in `SecurityTutorialsDatabase`([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))


A questo punto si conoscono il nome del server e database di `SecurityTutorials.mdf` file di database: `localhost\InstanceName` e `SecurityTutorialsDatabase`, rispettivamente. A questo punto siamo pronti installare i servizi delle applicazioni tramite il `aspnet_regsql.exe` dello strumento.

### <a name="installing-the-application-services"></a>L'installazione dei servizi dell'applicazione

Per avviare il `aspnet_regsql.exe` dello strumento, passare al menu start e scegliere Esegui. Immettere `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` nella casella di testo e fare clic su OK. In alternativa, è possibile utilizzare Windows Explorer per eseguire il drill-down nella cartella appropriata e fare doppio clic il `aspnet_regsql.exe` file. Entrambi gli approcci comporta gli stessi risultati.

Esegue il `aspnet_regsql.exe` strumento senza gli argomenti della riga di comando avvia l'interfaccia utente grafica di installazione guidata di ASP.NET SQL Server. La procedura guidata semplifica aggiungere o rimuovere i servizi delle applicazioni ASP.NET in un database specificato. La prima schermata della procedura guidata, mostrata nella figura 7, viene descritto lo scopo dello strumento.


[![Usare la procedura guidata stabilita di ASP.NET SQL Server il programma di installazione per aggiungere lo Schema di appartenenza](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Figura 7**: usare ASP.NET SQL Server Setup Wizard rende per aggiungere lo Schema di appartenenza ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))


Il secondo passaggio della procedura guidata viene chiesto se si vuole aggiungere i servizi delle applicazioni o rimuoverli. Poiché si vogliono aggiungere le tabelle, viste e stored procedure necessarie per il `SqlMembershipProvider`, scegliere Configura SQL Server per l'opzione servizi dell'applicazione. In un secondo momento, se si desidera rimuovere questo schema dal database, eseguire nuovamente questa procedura guidata, ma invece scegliere le informazioni di servizi di rimozione dell'applicazione da un'opzione di database esistente.


[![Scegliere la configurazione di SQL Server per l'opzione servizi dell'applicazione](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Figura 8**: scegliere Configura SQL Server per l'opzione servizi dell'applicazione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))


Il terzo passaggio richiede le informazioni di database: il nome del server, le informazioni di autenticazione e il nome del database. Se si sono state eseguite insieme a questa esercitazione e aver aggiunto la `SecurityTutorials.mdf` database `App_Data`, è associato a `localhost\InstanceName`e sua ridenominazione in `SecurityTutorialsDatabase`, quindi usare i valori seguenti:

- Server: `localhost\InstanceName`
- Autenticazione di Windows
- Database: `SecurityTutorialsDatabase`


[![Immettere le informazioni sul Database](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Figura 9**: immettere le informazioni del Database ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))


Dopo aver immesso le informazioni sul database, fare clic su Avanti. Il passaggio finale vengono riepilogati i passaggi da eseguire. Fare clic su Avanti per installare i servizi delle applicazioni e quindi scegliere Fine per completare la procedura guidata.

> [!NOTE]
> Se si usa Management Studio per collegare il database e rinominare il file di database, assicurarsi di scollegare il database e chiudere Management Studio prima di riaprire Visual Studio. Per scollegare il `SecurityTutorialsDatabase` del database, fare doppio clic sul nome del database e, dal menu attività scegliere Disconnetti.


Al termine della procedura guidata, tornare a Visual Studio e passare a Esplora Database. Espandere la cartella di tabelle. Si dovrebbe essere visualizzata una serie di tabelle i cui nomi iniziano con il prefisso `aspnet_`. Analogamente, un'ampia gamma di visualizzazioni e stored procedure sono disponibili nelle cartelle di viste e Stored procedure. Questi oggetti di database compongono lo schema di servizi dell'applicazione. Esamineremo gli oggetti di database di appartenenza e ruoli specifici nel passaggio 3.


[![Un'ampia gamma di tabelle, viste e Stored procedure sono stati aggiunti al Database](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Figura 10**: un'ampia gamma di tabelle, viste e Stored procedure sono stati aggiunti al Database ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))


> [!NOTE]
> Il `aspnet_regsql.exe` interfaccia utente grafica dello strumento viene installato lo schema di servizi di tutta l'applicazione. Ma quando si esegue `aspnet_regsql.exe` dalla riga di comando è possibile specificare i componenti da installare (o rimuovere) dei servizi dell'applicazione specifico. Pertanto, se si desidera aggiungere semplicemente le tabelle, viste e stored procedure necessarie per la `SqlMembershipProvider` e `SqlRoleProvider` provider, eseguire `aspnet_regsql.exe` dalla riga di comando. In alternativa, è possibile eseguire manualmente l'appropriato sottoinsieme di T-SQL crea gli script usati da `aspnet_regsql.exe`. Questi script si trovano nel `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` cartelle con nomi come `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`e così via.


A questo punto è stato creato gli oggetti di database necessiti per il `SqlMembershipProvider`. Tuttavia, è comunque necessario indicare il framework di appartenenza che è necessario utilizzare il `SqlMembershipProvider` (confronti, ad esempio, il `ActiveDirectoryMembershipProvider`) e che il `SqlMembershipProvider` utilizzino il `SecurityTutorials` database. Esamineremo come specificare quali provider da usare e come personalizzare le impostazioni del provider selezionato nel passaggio 4. Prima di tutto, diamo uno sguardo più approfondito gli oggetti di database appena creati.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Passaggio 3: Esaminare le tabelle di base dello Schema

Quando si lavora con i framework di appartenenza e ruoli in un'applicazione ASP.NET, i dettagli di implementazione sono incapsulati dal provider. Esercitazioni in futuro si dovrà interfacciare con questi Framework tramite .NET Framework `Membership` e `Roles` classi. Quando si utilizzano queste API ad alto livello non è necessario preoccuparsi di noi stessi con i dettagli di basso livello, ad esempio quali sono le query vengono eseguite o le tabelle vengono modificati dal `SqlMembershipProvider` e `SqlRoleProvider`.

Detto questo, è possibile usare i framework di appartenenza e ruoli in tutta sicurezza senza avere esaminato lo schema del database creato nel passaggio 2. Tuttavia, durante la creazione di tabelle per archiviare i dati dell'applicazione potrebbe dobbiamo creare entità correlate a utenti o ruoli. È utile per avere una certa familiarità con le `SqlMembershipProvider` e `SqlRoleProvider` schemi quando si stabiliscono esterna chiave vincoli tra le tabelle di dati dell'applicazione e le tabelle create nel passaggio 2. Inoltre, in alcuni casi rari si potrebbe essere necessario interfacciarsi con l'utente e ruolo archivia direttamente a livello di database (anziché attraverso il `Membership` o `Roles` classi).

### <a name="partitioning-the-user-store-into-applications"></a>Partizionamento di Store utente nelle applicazioni

I framework di appartenenza e ruoli sono progettati in modo che un singolo archivio utente e il ruolo può essere condiviso tra diverse applicazioni. Un'applicazione ASP.NET che usa i framework di appartenenza o ruoli è necessario specificare quale partizione di applicazione da utilizzare. In breve, le applicazioni web più possono usare gli stessi archivi utente e il ruolo. Figura 11 illustra gli archivi utente e il ruolo che vengono partizionati in tre applicazioni: HRSite CustomerSite e SalesSite. Questi tre le applicazioni web ogni hanno i propri utenti univoci e i ruoli, ma tutti archivia fisicamente le informazioni di account e il ruolo utente nelle tabelle del database stesso.


[![Gli account utente possono essere partizionati tra più applicazioni](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Figura 11**: gli account potrebbero essere partizionato tra più applicazioni utente ([fare clic per visualizzare l'immagine con dimensioni normali](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))


Il `aspnet_Applications` tabella è ciò che definisce tali partizioni. Ogni applicazione che usa il database per archiviare informazioni sull'account utente è rappresentato da una riga in questa tabella. Il `aspnet_Applications` tabella presenta quattro colonne: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, e `Description`. `ApplicationId` JE typu [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) e chiave primaria della tabella. `ApplicationName` fornisce un nome Converto univoco per ogni applicazione.

Le altre tabelle correlate al ruolo e l'appartenenza al collegano al `ApplicationId` campo `aspnet_Applications`. Ad esempio, il `aspnet_Users` dispone di tabella che contiene un record per ogni account utente, un `ApplicationId` campo di chiave esterna; analogo al parametro precedente per il `aspnet_Roles` tabella. Il `ApplicationId` campo in queste tabelle consente di specificare la partizione applicativa l'account utente o ruolo a cui appartiene.

### <a name="storing-user-account-information"></a>L'archiviazione delle informazioni sull'Account utente

Informazioni sull'account utente si trova in due tabelle: `aspnet_Users` e `aspnet_Membership`. Il `aspnet_Users` tabella contiene campi che contengono le informazioni essenziali dell'account. Le tre colonne più rilevanti sono:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` è la chiave primaria (e di tipo `uniqueidentifier`). `UserName` è di tipo `nvarchar(256)` e, con la password, costituiscono le credenziali dell'utente. (La password dell'utente viene archiviata nel `aspnet_Membership` tabella.) `ApplicationId` collega l'account utente a un'applicazione specifica in `aspnet_Applications`. È un composto [ `UNIQUE` vincolo](https://msdn.microsoft.com/library/ms191166.aspx) sul `UserName` e `ApplicationId` colonne. Ciò garantisce che in una determinata applicazione ogni nome utente è univoco, ma consente lo stesso `UserName` da utilizzare in applicazioni diverse.

Il `aspnet_Membership` tabella include informazioni sull'account utente aggiuntivi, ad esempio la password dell'utente, indirizzo di posta elettronica, l'ultimo account di accesso data e ora e così via. Non è presente una corrispondenza uno a uno tra i record nel `aspnet_Users` e `aspnet_Membership` tabelle. Questa relazione è garantita dal `UserId` campo `aspnet_Membership`, che funge da chiave primaria della tabella. Ad esempio il `aspnet_Users` tabella `aspnet_Membership` include un `ApplicationId` campo che consente di associare queste informazioni per una determinata partizione applicativa.

### <a name="securing-passwords"></a>Protezione delle password

Informazioni password vengono archiviate nel `aspnet_Membership` tabella. Il `SqlMembershipProvider` consente per le password da archiviare nel database di utilizzando una delle tre tecniche descritte di seguito:

- **Cancella** -la password viene archiviata nel database come testo normale. Fortemente sconsigliato l'uso di questa opzione. Se il database è compromesso, sia esso un dispositivo da un pirata informatico che trova una backdoor o un dipendente insoddisfatto chi ha accesso al database - credenziali dell'utente ogni singolo esistono prontamente.
- **Eseguito l'hashing** -password viene eseguito l'hashing mediante un algoritmo di hash unidirezionale e un valore salt generato casualmente. Questo valore con hash (con il valore salt) viene archiviato nel database.
- **Crittografati** -una versione crittografata della password verrà archiviata nel database.

La tecnica di archiviazione di password usata dipende il `SqlMembershipProvider` le impostazioni specificate `Web.config`. Si esaminerà la personalizzazione di `SqlMembershipProvider` impostazioni nel passaggio 4. Il comportamento predefinito consiste nell'archiviare l'hash della password.

Le colonne responsabile per archiviare la password siano `Password`, `PasswordFormat`, e `PasswordSalt`. `PasswordFormat` è un campo di tipo `int` il cui valore indica la tecnica utilizzata per archiviare la password: 0 per Clear; 1 per Hashed; 2 per la crittografia. `PasswordSalt` viene assegnato a una stringa generata casualmente indipendentemente dalla tecnica di archiviazione di password utilizzata; il valore di `PasswordSalt` viene usato solo quando il calcolo dell'hash della password. Infine, il `Password` colonna contiene i dati di password effettiva, ovvero la password non crittografata, l'hash della password o la password crittografata.

Tabella 1 illustra queste tre colonne aspetto per le diverse tecniche di archiviazione quando si archiviano password MySecret! .

| **Archiviazione tecnica&lt;\_o3a\_p /&gt;** | **La password&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Cancella | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Eseguito l'hashing | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Crittografato | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabella 1**: i valori di esempio per i campi correlati alle Password per l'archiviazione MySecret la Password!

> [!NOTE]
> La crittografia specifica o un algoritmo di hash utilizzato per il `SqlMembershipProvider` è determinato dalle impostazioni nel `<machineKey>` elemento. Abbiamo discusso di questo elemento di configurazione nel passaggio 3 della <a id="Tutorial3"> </a> [ *configurazione dell'autenticazione form e argomenti avanzati* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) esercitazione.


### <a name="storing-roles-and-role-associations"></a>L'archiviazione dei ruoli e le associazioni di ruolo

Il framework di ruoli consente agli sviluppatori di definire un set di ruoli e specificare quali utenti appartengono a quali ruoli. Queste informazioni vengono acquisite nel database tramite due tabelle: `aspnet_Roles` e `aspnet_UsersInRoles`. Ogni record di `aspnet_Roles` tabella rappresenta un ruolo per una determinata applicazione. In modo analogo i `aspnet_Users` tabella, la `aspnet_Roles` tabella contiene tre colonne pertinenti per la discussione:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` è la chiave primaria (e di tipo `uniqueidentifier`). `RoleName` è di tipo `nvarchar(256)`. E `ApplicationId` collega l'account utente a un'applicazione specifica in `aspnet_Applications`. È un composto `UNIQUE` vincolo per il `RoleName` e `ApplicationId` colonne, assicurandosi che in una determinata applicazione il nome del ruolo sia univoco.

Il `aspnet_UsersInRoles` tabella funge da un mapping tra utenti e ruoli. Sono disponibili solo due colonne - `UserId` e `RoleId` - che insieme costituiscono una chiave primaria composta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Passaggio 4: Specificare il Provider e personalizzandone le impostazioni

Tutti i framework che supportano il modello di provider, ad esempio i framework di appartenenza e ruoli - mancano i dettagli di implementazione se stessi e invece delegare tale compito a una classe di provider. Nel caso il framework di appartenenza, il `Membership` classe definisce l'API per la gestione degli account utente, ma non interagisce direttamente con qualsiasi archivio utente. Piuttosto, il `Membership` la richiesta al provider configurato - trasferiscono i metodi della classe verrà usata la `SqlMembershipProvider`. Quando si richiama uno dei metodi nel `Membership` classe modo il framework di appartenenza può sapere per delegare la chiamata al `SqlMembershipProvider`?

Il `Membership` classe ha un [ `Providers` proprietà](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) che contiene un riferimento a tutte le classi del provider registrato disponibile per l'uso di dal framework di appartenenza. Ogni provider registrato con un nome associato e il tipo. Il nome offre un modo Converto per fare riferimento a un determinato provider nel `Providers` insieme, mentre il tipo identifica la classe del provider. Inoltre, ogni provider registrato può includere le impostazioni di configurazione. Includono le impostazioni di configurazione per il framework di appartenenza `passwordFormat` e `requiresUniqueEmail`, tra le molte altre. Vedere la tabella 2 per un elenco completo delle impostazioni di configurazione usate dal `SqlMembershipProvider`.

Il `Providers` contenuto della proprietà è specificato tramite le impostazioni di configurazione dell'applicazione web. Per impostazione predefinita, tutte le applicazioni web dispongono di un provider denominato `AspNetSqlMembershipProvider` di tipo `SqlMembershipProvider`. Questo provider di appartenenze predefinito è registrato `machine.config` (disponibile all'indirizzo `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Come il markup precedente, il [ `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx) definisce le impostazioni di configurazione per il framework di appartenenza durante la [ `<providers>` elemento figlio](https://msdn.microsoft.com/library/6d4936ht.aspx) specifica registrato provider. I provider possono essere aggiunti o rimossi utilizzando il [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) o [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementi; usare la [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) elemento da rimuovere tutti attualmente provider registrati. Come sopra illustrato, il markup `machine.config` aggiunge un provider denominato `AspNetSqlMembershipProvider` di tipo `SqlMembershipProvider`.

Oltre al `name` e `type` attributi, la `<add>` elemento contiene attributi che definiscono i valori per varie impostazioni di configurazione. Tabella 2 elenca disponibili `SqlMembershipProvider`-impostazioni di configurazione specifici, insieme a una descrizione di ognuno.

> [!NOTE]
> Tutti i valori predefiniti indicati nella tabella 2 fa riferimento ai valori predefiniti definiti nel `SqlMembershipProvider` classe. Si noti che non tutte le impostazioni di configurazione nel `AspNetSqlMembershipProvider` corrispondono ai valori predefiniti del `SqlMembershipProvider` classe. Ad esempio, se non specificato in un provider di appartenenza, il `requiresUniqueEmail` impostazione le impostazioni predefinite su true. Tuttavia, il `AspNetSqlMembershipProvider` esegue l'override di questo valore predefinito, in modo esplicito specificando un valore di `false`.


| **L'impostazione&lt;\_o3a\_p /&gt;** | **Descrizione&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | È importante ricordare che consenta il framework di appartenenza per un archivio utente singolo essere partizionato tra più applicazioni. Questa impostazione indica il nome della partizione applicativa utilizzato dal provider di appartenenze. Se questo valore non è specificato in modo esplicito, viene impostata, in fase di esecuzione, il valore del percorso radice virtuale dell'applicazione. |
| `commandTimeout` | Specifica il valore di timeout del comando SQL (in secondi). Il valore predefinito è 30. |
| `connectionStringName` | Il nome della stringa di connessione nel `<connectionStrings>` elemento da utilizzare per la connessione al database di archivio utente. Questo valore è obbligatorio. |
| `description` | Fornisce una descrizione Converto del provider registrato. |
| `enablePasswordRetrieval` | Specifica se gli utenti possono recuperare le loro password dimenticate. Il valore predefinito è `false`. |
| `enablePasswordReset` | Indica se gli utenti sono autorizzati a reimpostare la password. Il valore predefinito è `true`. |
| `maxInvalidPasswordAttempts` | Il numero massimo di tentativi di accesso non riuscito che possono verificarsi durante l'oggetto specificato per un determinato utente `passwordAttemptWindow` prima che l'utente venga bloccato. Il valore predefinito è 5. |
| `minRequiredNonalphanumericCharacters` | Il numero minimo di caratteri non alfanumerici deve essere specificata in una password dell'utente. Questo valore deve essere compreso tra 0 e 128; il valore predefinito è 1. |
| `minRequiredPasswordLength` | Il numero minimo di caratteri richiesti nella password. Questo valore deve essere compreso tra 0 e 128; il valore predefinito è 7. |
| `name` | Il nome del provider registrato. Questo valore è obbligatorio. |
| `passwordAttemptWindow` | Il numero di minuti durante i quali non vengono registrati i tentativi di accesso. Se un utente fornisce credenziali di accesso valido `maxInvalidPasswordAttempts` volte all'interno di questa finestra è specificato, questi vengono bloccati. Il valore predefinito è 10. |
| `PasswordFormat` | Il formato di archiviazione di password: `Clear`, `Hashed`, o `Encrypted`. Il valore predefinito è `Hashed`. |
| `passwordStrengthRegularExpression` | Se specificato, questa espressione regolare viene usata per valutare la complessità della password dell'utente selezionato quando si crea un nuovo account o la modifica delle password. Il valore predefinito è una stringa vuota. |
| `requiresQuestionAndAnswer` | Specifica se un utente deve rispondere per la domanda di sicurezza durante il recupero o reimpostare la password. Il valore predefinito è `true`. |
| `requiresUniqueEmail` | Indica se tutti gli account utente in una partizione di applicazione specificata devono avere un indirizzo di posta elettronica univoco. Il valore predefinito è `true`. |
| `type` | Specifica il tipo del provider. Questo valore è obbligatorio. |

**Tabella 2**: l'appartenenza e `SqlMembershipProvider` le impostazioni di configurazione

Oltre a `AspNetSqlMembershipProvider`, altri provider di appartenenze può essere registrato in modo da applicazione aggiungendo markup simile al `Web.config` file.

> [!NOTE]
> Il framework di ruoli funziona in gran parte allo stesso modo: è disponibile un provider di ruoli registrato predefinito nel `machine.config` e i provider registrati possono essere personalizzati in modo da applicazioni in `Web.config`. Verrà esaminato il framework di ruoli e il markup di configurazione in modo dettagliato in un'esercitazione futura.


### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalizzazione di`SqlMembershipProvider`impostazioni

Il valore predefinito `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) è relativo `connectionStringName` attributo impostato su `LocalSqlServer`. Ad esempio la `AspNetSqlMembershipProvider` provider, il nome di stringa di connessione `LocalSqlServer` definito nella `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Come si può notare, questa stringa di connessione definisce un database che si trova in SQL 2005 Express Edition | DataDirectory|aspnetdb.mdf. La stringa | DataDirectory | viene convertita in fase di esecuzione in modo che punti la `~/App_Data/` directory, pertanto il percorso del database | DataDirectory|aspnetdb.mdf"viene convertita in `~/App_Data` / `aspnet.mdf`.

Se non si specificava le informazioni sul provider di appartenenza nella nostra applicazione `Web.config` file, l'applicazione usa il provider di appartenenze predefinito registrato, `AspNetSqlMembershipProvider`. Se il `~/App_Data/aspnet.mdf` database non esiste, il runtime ASP.NET creerà automaticamente e aggiungere lo schema di servizi dell'applicazione. Tuttavia, non si vuole usare il `aspnet.mdf` database; piuttosto, è possibile usare il `SecurityTutorials.mdf` database creati nel passaggio 2. Questa modifica può essere eseguita in uno dei due modi:

- <strong>Specificare un valore per il</strong><strong>`LocalSqlServer`</strong><strong>nome stringa di connessione in</strong><strong>`Web.config`</strong><strong>.</strong> Sovrascrivendo il `LocalSqlServer` valore di nome di stringa di connessione nel `Web.config`, è possibile usare il provider di appartenenze predefinito registrato (`AspNetSqlMembershipProvider`) e assicurarsi che funzioni correttamente con il `SecurityTutorials.mdf` database. Questo approccio è ideale se si ritiene con le impostazioni di configurazione specificate dalla `AspNetSqlMembershipProvider`. Per altre informazioni su questa tecnica, vedere [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [configurazione di servizi dell'applicazione di ASP.NET 2.0 per usare SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Aggiungere un nuovo provider registrati typu</strong><strong>`SqlMembershipProvider`</strong><strong>e configurare relativo</strong><strong>`connectionStringName`</strong><strong>impostazione in modo che punti la</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>database.</strong> Questo approccio è utile negli scenari in cui si desidera personalizzare altre proprietà di configurazione oltre alla stringa di connessione di database. Nei miei progetti io uso sempre questo approccio a causa di flessibilità e la leggibilità.

Prima di poter aggiungere un nuovo provider registrato che fa riferimento il `SecurityTutorials.mdf` database, è innanzitutto necessario aggiungere un valore di stringa di connessione appropriato nel `<connectionStrings>` sezione `Web.config`. Il markup seguente aggiunge una nuova stringa di connessione denominata `SecurityTutorialsConnectionString` che fa riferimento a SQL Server 2005 Express Edition `SecurityTutorials.mdf` del database nel `App_Data` cartella.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Se si usa un file di database alternativo, aggiornare la stringa di connessione in base alle esigenze. Per altre informazioni su che costituiscono la stringa di connessione corretta, fare riferimento a [ConnectionStrings.com](http://www.connectionstrings.com/).

Successivamente, aggiungere il seguente markup di configurazione di appartenenza per il `Web.config` file. Questo markup registra un nuovo provider denominato `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Oltre a registrare il `SecurityTutorialsSqlMembershipProvider` provider, il markup precedente definisce il `SecurityTutorialsSqlMembershipProvider` come provider predefinito (tramite il `defaultProvider` attributo di `<membership>` elemento). È importante ricordare che il framework di appartenenza può avere più provider registrati. Poiché `AspNetSqlMembershipProvider` viene registrata come il primo provider nella `machine.config`, funge da provider predefinito a meno che non viene indicato in caso contrario.

Attualmente, l'applicazione dispone di due provider registrati: `AspNetSqlMembershipProvider` e `SecurityTutorialsSqlMembershipProvider`. Tuttavia, prima di registrare le `SecurityTutorialsSqlMembershipProvider` provider è stato possibile aver deselezionato tutte in precedenza provider registrati mediante l'aggiunta di un [ `<clear />` elemento](https://msdn.microsoft.com/library/t062y6yc.aspx) immediatamente prima nostro `<add>` elemento. Questo sarebbe cancelliamo i `AspNetSqlMembershipProvider` dall'elenco dei provider registrati, vale a dire che il `SecurityTutorialsSqlMembershipProvider` sarà l'unico provider di appartenenza registrato. Se è stato usato questo approccio, quindi non è necessario contrassegnare il `SecurityTutorialsSqlMembershipProvider` come il provider predefinito, poiché potrebbe essere l'unico provider di appartenenza registrato. Per altre informazioni sull'uso `<clear />`, vedere [Using `<clear />` provider quando si aggiunge](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Si noti che il `SecurityTutorialsSqlMembershipProvider`del `connectionStringName` impostazione di just-aggiunta di riferimenti `SecurityTutorialsConnectionString` nome stringa di connessione e che il `applicationName` impostazione è stata impostata su un valore di SecurityTutorials. Inoltre, il `requiresUniqueEmail` impostazione è stata impostata su `true`. Tutte le altre opzioni di configurazione sono identici ai valori in `AspNetSqlMembershipProvider`. È possibile apportare alcuna modifica di configurazione in questo caso, se si desidera. È ad esempio, è stato possibile rafforzare la complessità della password richiedendo due caratteri non alfanumerici invece di uno, o se si aumenta la lunghezza della password su otto caratteri anziché sette.

> [!NOTE]
> È importante ricordare che consenta il framework di appartenenza per un archivio utente singolo essere partizionato tra più applicazioni. Il provider di appartenenze `applicationName` impostazione indica al provider viene utilizzato quando si lavora con l'archivio dell'utente dell'applicazione. È importante impostare in modo esplicito un valore per il `applicationName` impostazione di configurazione in quanto se il `applicationName` non è impostato in modo esplicito, viene assegnato al percorso radice virtuale dell'applicazione web in fase di esecuzione. Questo approccio va bene, purché non cambia percorso radice virtuale dell'applicazione, ma se si sposta l'applicazione a un percorso diverso, il `applicationName` impostazione cambierà troppo. In questo caso, il provider di appartenenze inizierà a utilizzo di una partizione di applicazione diversa da quella utilizzata in precedenza. Gli account utente creati prima dello spostamento risiederà in una partizione di applicazione diversi e tali utenti non saranno in grado di accedere al sito. Per una discussione più approfondita in merito, vedere [sempre impostato il `applicationName` proprietà durante la configurazione di ASP.NET 2.0 Membership e altri provider](https://weblogs.asp.net/scottgu/443634).


## <a name="summary"></a>Riepilogo

A questo punto si dispone di un database con i servizi applicazione configurata (`SecurityTutorials.mdf`) e avere configurato la nostra applicazione web in modo che usi il framework di appartenenza di `SecurityTutorialsSqlMembershipProvider` provider abbiamo appena registrati. Questo provider registrato JE typu `SqlMembershipProvider` e ha relativi `connectionStringName` impostato sulla stringa di connessione appropriata (`SecurityTutorialsConnectionString`) e la relativa `applicationName` valore impostato in modo esplicito.

A questo punto siamo pronti usare il framework di appartenenza dall'applicazione. Nella prossima esercitazione verrà esaminato come creare nuovi account utente. In seguito che verrà esaminata l'autenticazione degli utenti, esegue l'autorizzazione basata sull'utente e archiviare informazioni aggiuntive relative all'utente nel database.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Impostare sempre la `applicationName` proprietà durante la configurazione di ASP.NET 2.0 Membership e altri provider di servizi](https://weblogs.asp.net/scottgu/443634)
- [La configurazione di ASP.NET 2.0 dei servizi dell'applicazione per l'uso di SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Scaricare SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Analisi di ASP.NET 2.0 s appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Il `<add>` (elemento) per Providers per Membership](https://msdn.microsoft.com/library/whae3t94.aspx)
- [Il `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [Il `<providers>` (elemento) per l'appartenenza](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Usando `<clear />` quando si aggiungono i provider](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Utilizzo diretto di `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formazione in video su argomenti contenuti in questa esercitazione

- [Informazioni sulle appartenenze ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurazione di SQL per l'uso degli schemi di appartenenza](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modifica delle impostazioni di appartenenza nello schema di appartenenza predefinito](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Alicja Maziarz. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [avanti](creating-user-accounts-cs.md)
