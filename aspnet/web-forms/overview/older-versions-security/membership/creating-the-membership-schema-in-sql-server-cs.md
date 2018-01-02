---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Creazione dello Schema di appartenenza in SQL Server (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione inizia esaminando le tecniche per l'aggiunta dello schema necessario per il database per utilizzare SqlMembershipProvider. Successivamente, abbiamo wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 2155f9b0893a0b1d3cf60bc63d80df4417649beb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>Creazione dello Schema di appartenenza in SQL Server (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> In questa esercitazione inizia esaminando le tecniche per l'aggiunta dello schema necessario per il database per utilizzare SqlMembershipProvider. Successivamente, verrà esaminare le tabelle nello schema di chiave e discutere lo scopo e l'importanza. In questa esercitazione termina con un'occhiata a come stabilire il provider deve utilizzare il framework di appartenenza di un'applicazione ASP.NET.


## <a name="introduction"></a>Introduzione

Le esercitazioni precedenti due esaminate tramite l'autenticazione basata su form per identificare i visitatori del sito Web. Il framework di autenticazione form rende più semplice per gli sviluppatori di accedere un utente a un sito Web e per memorizzare le visite pagina tramite l'utilizzo di ticket di autenticazione. La `FormsAuthentication` classe include metodi per la generazione di ticket e l'aggiunta dei cookie del visitatore. Il `FormsAuthenticationModule` esamina tutte le richieste in ingresso e, per quelle con un ticket di autenticazione valido, crea e associa un `GenericPrincipal` e `FormsIdentity` oggetto con la richiesta corrente. Autenticazione basata su form è semplicemente un meccanismo per concedere un ticket di autenticazione per un visitatore per l'accesso in e, nelle richieste successive, l'analisi di tale ticket per determinare l'identità dell'utente. Per un'applicazione web supportare gli account utente, è ancora necessario implementare un archivio utente e aggiungere funzionalità a convalidare le credenziali, registrare i nuovi utenti e le numerosissime altre attività relative all'account utente.

Prima di ASP.NET 2.0, gli sviluppatori dovevano l'hook per l'implementazione di tutte queste attività relative all'account utente. Per fortuna, il team ASP.NET riconosciuto questo difetto ed è stato introdotto il framework di appartenenza con ASP.NET 2.0. Il framework di appartenenza è un set di classi in .NET Framework che forniscono un'interfaccia di programmazione per il completamento delle attività relative all'account utente di base. Questo framework è incorporato nella parte superiore di [modello provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che consente agli sviluppatori di aggiungere un'implementazione personalizzata in un'API standard.

Come descritto nel <a id="Tutorial1"> </a> [ *nozioni fondamentali sulla sicurezza e il supporto ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) esercitazione, .NET Framework viene fornito con due provider di appartenenze predefinito: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.activedirectorymembershipprovider.aspx) e [ `SqlMembershipProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.sqlmembershipprovider.aspx). Come suggerisce il nome, il `SqlMembershipProvider` utilizza un database di Microsoft SQL Server come archivio dell'utente. Per utilizzare questo provider in un'applicazione, è necessario indicare al provider i database da utilizzare come archivio. Come si può immaginare, la `SqlMembershipProvider` prevede che il database dell'archivio utente per alcune tabelle di database, viste e stored procedure. È necessario aggiungere lo schema previsto per il database selezionato.

In questa esercitazione inizia esaminando le tecniche per l'aggiunta dello schema necessario per il database per utilizzare il `SqlMembershipProvider`. Successivamente, verrà esaminare le tabelle nello schema di chiave e discutere lo scopo e l'importanza. In questa esercitazione termina con un'occhiata a come stabilire il provider deve utilizzare il framework di appartenenza di un'applicazione ASP.NET.

Iniziamo!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Passaggio 1: Decidere dove posizionare l'archivio dell'utente

Dati di un'applicazione ASP.NET sono solitamente archiviati in un numero di tabelle in un database. Quando si implementa il `SqlMembershipProvider` lo schema del database è necessario decidere se inserire lo schema di appartenenza nello stesso database i dati dell'applicazione o in un database alternativo.

È consigliabile individuare lo schema di appartenenza nello stesso database i dati dell'applicazione per i motivi seguenti:

- **Manutenibilità** un'applicazione i cui dati vengono incapsulati in un database non è più facile da comprendere, gestire e distribuire un'applicazione che dispone di due database separati.
- **Integrità relazionale** individuando le tabelle correlate appartenenza nello stesso database, come l'applicazione di tabelle, è possibile stabilire [vincoli di chiave esterna](http://en.wikipedia.org/wiki/Foreign_key) tra le chiavi primarie nel Tabelle correlate di appartenenza e applicazione correlata.

Separazione i dati di archivio e l'applicazione dell'utente in database separati ha senso solo se sono presenti più applicazioni che ogni utilizzare database separati, ma devono condividere un archivio utente comune.

### <a name="creating-a-database"></a>Creazione di un Database

L'applicazione che sviluppo poiché l'esercitazione secondo non è ancora necessario un database. È necessario uno a questo punto, tuttavia, per l'archivio dell'utente. Possibile crearne uno e quindi aggiungervi lo schema necessario per il `SqlMembershipProvider` provider (vedere il passaggio 2).

> [!NOTE]
> In questa serie di esercitazioni che verrà usato un [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/en-us/sql/Aa336346.aspx) database per archiviare le tabelle di applicazione e `SqlMembershipProvider` dello schema. Questa decisione è stata effettuata per due motivi: prima di tutto, a causa il costo - libero - Express Edition è la versione più readably accessibile di SQL Server 2005. in secondo luogo, i database di SQL Server 2005 Express Edition possono essere inseriti direttamente nella finestra dell'applicazione web `App_Data` cartella, rendendolo un semplice pacchetto del database e applicazione web insieme in un unico file ZIP e ridistribuirlo senza le istruzioni di installazione speciali o opzioni di configurazione. Se si preferisce seguire la procedura utilizzando una versione non - Express Edition di SQL Server, è possibile. I passaggi sono praticamente identici. Il `SqlMembershipProvider` schema verrà funziona con qualsiasi versione di Microsoft SQL Server 2000 e backup.


In Esplora soluzioni, fare clic su di `App_Data` cartella e scegliere Aggiungi nuovo elemento. (Se non viene visualizzato un `App_Data` cartella del progetto, pulsante destro del mouse sul progetto in Esplora soluzioni, scegliere Aggiungi cartella ASP.NET e selezionare `App_Data`.) Nella finestra di dialogo Aggiungi nuovo elemento, scegliere di aggiungere un nuovo Database SQL denominato `SecurityTutorials.mdf`. In questa esercitazione si aggiungeranno il `SqlMembershipProvider` dello schema per questo database; nelle esercitazioni successive si creerà anche altre tabelle per acquisire i dati dell'applicazione.


[![Aggiungere un nuovo Database SQL denominato Database SecurityTutorials.mdf nella cartella App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Figura 1**: aggiungere un nuovo Database SQL denominato `SecurityTutorials.mdf` Database il `App_Data` cartella ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))


Aggiunta di un database per il `App_Data` cartella lo include automaticamente nella vista Esplora Database. (Nella versione edizione non Express di Visual Studio, Esplora Database è denominato Esplora Server.) Passare a Esplora Database ed espandere l'appena aggiunti `SecurityTutorials` database. Se non è possibile visualizzare Esplora Database sullo schermo, passare al menu Visualizza e scegliere Esplora Database oppure premere Ctrl + Alt + S. Come illustrato nella figura 2, il `SecurityTutorials` database è vuoto, contiene alcuna tabella, non le visualizzazioni e Nessuna stored procedure.


[![Il SecurityTutorials Database è attualmente vuoto](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Figura 2**: il `SecurityTutorials` Database è attualmente vuoto ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Passaggio 2: Aggiunta di`SqlMembershipProvider`Schema al Database

Il `SqlMembershipProvider` richiede un particolare set di tabelle, viste e stored procedure da installare nel database dell'archivio utente. Questi oggetti di database necessari possono essere aggiunti mediante la [ `aspnet_regsql.exe` strumento](https://msdn.microsoft.com/en-us/library/ms229862.aspx). Questo file si trova nel `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` cartella.

> [!NOTE]
> Il `aspnet_regsql.exe` strumento offre funzionalità di riga di comando e un'interfaccia utente grafica. L'interfaccia grafica è più semplice ed è ciò che verrà esaminato in questa esercitazione. L'interfaccia della riga di comando è utile quando l'aggiunta del `SqlMembershipProvider` schema deve essere automatizzato, ad esempio una compilazione di script o scenari di test automatizzati.


Il `aspnet_regsql.exe` viene usato per aggiungere o rimuovere *servizi delle applicazioni ASP.NET* a un database di SQL Server specificato. I servizi delle applicazioni ASP.NET includono gli schemi per il `SqlMembershipProvider` e `SqlRoleProvider`, con gli schemi per i provider basato su SQL per altri framework di ASP.NET 2.0. È necessario fornire due bit di informazioni per il `aspnet_regsql.exe` strumento:

- Se si desidera aggiungere o rimuovere i servizi delle applicazioni, e
- Il database da cui si desidera aggiungere o rimuovere lo schema di servizi di applicazione

Nella richiesta di conferma per il database da utilizzare, il `aspnet_regsql.exe` strumento chiesto di fornire il nome del server si trova il database on, le credenziali di sicurezza per la connessione al database e il nome del database. Se si utilizza il non Express Edition di SQL Server, è necessario conoscere già queste informazioni, come le stesse informazioni che è necessario fornire tramite una stringa di connessione quando si lavora con il database tramite una pagina web ASP.NET. Determinando il nome del server e database quando si utilizza un database di SQL Server 2005 Express Edition nel `App_Data` cartella, tuttavia, è un po' più complicata.

Nella sezione seguente esamina specificando il nome di server e database per un database di SQL Server 2005 Express Edition in un modo semplice il `App_Data` cartella. Se non si utilizza è SQL Server 2005 Express Edition possibile ignorare l'installazione della sezione servizi delle applicazioni.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Determinare il nome del Database per un Database SQL Server 2005 Express Edition nel Server di`App_Data`cartella

Per utilizzare il `aspnet_regsql.exe` strumento è necessario conoscere i nomi di server e database. Il nome del server è `localhost\InstanceName`. Molto probabilmente, il *InstanceName* è `SQLExpress`. Tuttavia, se è stato installato manualmente SQL Server 2005 Express Edition (ovvero, si non installato automaticamente durante l'installazione di Visual Studio), è possibile che sia selezionato un nome istanza diverso.

Il nome del database è un po' più difficile determinare. Nei database di `App_Data` cartella hanno in genere un nome di database che include un [identificatore univoco globale](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) insieme al percorso del file di database. È necessario determinare il nome del database per aggiungere lo schema di servizi di applicazione tramite `aspnet_regsql.exe`.

Il modo più semplice per verificare il nome del database è esaminarlo tramite SQL Server Management Studio. SQL Server Management Studio fornisce un'interfaccia grafica per la gestione di database di SQL Server 2005, ma non viene fornito con la Express Edition di SQL Server 2005. Buone notizie consiste nel fatto che [è possibile scaricare](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) il libero Express Edition di SQL Server Management Studio.

> [!NOTE]
> Se si dispone anche di una versione non - Express Edition di SQL Server 2005 installato sul desktop è installata probabilmente la versione completa di Management Studio. È possibile utilizzare la versione completa per determinare il nome del database, seguendo la stessa procedura, come descritto di seguito per l'edizione Express.


Per iniziare, chiudere Visual Studio per assicurarsi che tutti i blocchi imposti da Visual Studio per il file di database vengano chiuse. Successivamente, avviare SQL Server Management Studio e connettersi al `localhost\InstanceName` database per SQL Server 2005 Express Edition. Come notato in precedenza, probabilità sono il nome dell'istanza è `SQLExpress`. Per l'opzione di autenticazione, selezionare l'autenticazione di Windows.


[![Connettersi all'istanza di SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Figura 3**: connettersi all'istanza di SQL Server 2005 Express Edition ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))


Dopo la connessione all'istanza di SQL Server 2005 Express Edition, Management Studio consente di visualizzare le cartelle per i database, le impostazioni di sicurezza, gli oggetti Server e così via. Se si espande la scheda di database si noterà che il `SecurityTutorials.mdf` database *non* registrato nell'istanza del database, è necessario collegare prima il database.

Pulsante destro del mouse sulla cartella database e scegliere Connetti dal menu di scelta rapida. Questo verrà visualizzata la finestra di dialogo Collega database. Da qui, fare clic sul pulsante Aggiungi, selezionare il `SecurityTutorials.mdf` database e fare clic su OK. La figura 4 mostra la finestra di dialogo Collega database dopo il `SecurityTutorials.mdf` database è stato selezionato. Figura 5 Mostra Esplora oggetti di Management Studio dopo che il database è stato collegato correttamente.


[![Collegare il Database SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Figura 4**: collegare il `SecurityTutorials.mdf` Database ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))


[![Il Database SecurityTutorials.mdf viene visualizzato nella cartella di database](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Figura 5**: il `SecurityTutorials.mdf` Database visualizzato nella cartella di database ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))


Come illustrato nella figura 5, il `SecurityTutorials.mdf` database ha un nome piuttosto abstruse. Modificare il più significativo (e più facile da digitare) nome. Pulsante destro del mouse sul database, scegliere Rinomina dal menu di scelta rapida e rinominarlo `SecurityTutorialsDatabase`. Ciò non modifica il nome del file, solo il nome del database viene utilizzato per identificare se stessa a SQL Server.


[![Rinominare il Database per SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Figura 6**: rinominare il Database in `SecurityTutorialsDatabase`([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))


A questo punto, il nome del server e database di `SecurityTutorials.mdf` file di database: `localhost\InstanceName` e `SecurityTutorialsDatabase`, rispettivamente. A questo punto si è pronti installare i servizi delle applicazioni tramite il `aspnet_regsql.exe` dello strumento.

### <a name="installing-the-application-services"></a>Installare i servizi delle applicazioni

Per avviare il `aspnet_regsql.exe` strumento, passare al menu start e scegliere Esegui. Immettere `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` nella casella di testo e fare clic su OK. In alternativa, è possibile utilizzare Esplora risorse di Windows per eseguire il drill-down nella cartella appropriata e fare doppio clic su di `aspnet_regsql.exe` file. Entrambi gli approcci comporta gli stessi risultati.

Esecuzione di `aspnet_regsql.exe` strumento senza gli argomenti della riga di comando consente di avviare l'interfaccia utente grafica di installazione guidata di ASP.NET SQL Server. La procedura guidata consente di aggiungere o rimuovere i servizi delle applicazioni ASP.NET in un database specificato. La prima schermata della procedura guidata, illustrata nella figura 7, descrive lo scopo dello strumento.


[![Utilizzare la procedura guidata consente di SQL ASP.NET Server il programma di installazione per aggiungere lo Schema di appartenenza](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Figura 7**: utilizzare ASP.NET SQL Server l'installazione guidata consente di aggiungere lo Schema di appartenenza ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))


Il secondo passaggio della procedura guidata viene chiesto se si desidera aggiungere i servizi delle applicazioni o rimuoverli. Poiché si desidera aggiungere tabelle, viste e stored procedure necessarie per il `SqlMembershipProvider`, scegliere Configura SQL Server per l'opzione dei servizi dell'applicazione. In un secondo momento, se si desidera rimuovere questo schema dal database, eseguire nuovamente questa procedura guidata, ma scegliere invece le informazioni sui servizi di installazione applicazione da un'opzione di database esistente.


[![Scegliere la configurazione di SQL Server per l'opzione di servizi di applicazione](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Figura 8**: scegliere di configurare SQL Server per l'opzione dei servizi dell'applicazione ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))


Il terzo passaggio richiesto per le informazioni sul database: il nome del server, le informazioni di autenticazione e il nome del database. Se si segue insieme a questa esercitazione e aver aggiunto il `SecurityTutorials.mdf` database `App_Data`, collegato a `localhost\InstanceName`e sua ridenominazione in `SecurityTutorialsDatabase`, quindi utilizzare i valori seguenti:

- Server:`localhost\InstanceName`
- Autenticazione di Windows
- Database:`SecurityTutorialsDatabase`


[![Immettere le informazioni del Database](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Figura 9**: immettere le informazioni del Database ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))


Dopo aver immesso le informazioni sul database, fare clic su Avanti. Il passaggio finale vengono riepilogati i passaggi che verranno eseguiti. Fare clic su Avanti per installare i servizi delle applicazioni e quindi scegliere Fine per completare la procedura guidata.

> [!NOTE]
> Se si usa Management Studio per collegare il database e rinominare il file di database, assicurarsi di scollegare il database e chiudere Management Studio prima di riaprire Visual Studio. Per scollegare il `SecurityTutorialsDatabase` del database, fare clic sul nome del database e scegliere Disconnetti dal menu attività.


Al termine della procedura guidata, tornare a Visual Studio e passare a Esplora Database. Espandere la cartella di tabelle. Si noterà una serie di tabelle i cui nomi iniziano con il prefisso `aspnet_`. Analogamente, un'ampia gamma di visualizzazioni e stored procedure sono disponibili nelle cartelle di viste e Stored procedure. Questi oggetti di database che compongono lo schema di servizi di applicazione. Esamineremo gli oggetti di database di appartenenza e ruoli specifici nel passaggio 3.


[![Un'ampia gamma di tabelle, viste e Stored procedure sono stati aggiunti al Database](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Figura 10**: un'ampia gamma di tabelle, viste e Stored procedure sono stati aggiunti al Database ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))


> [!NOTE]
> Il `aspnet_regsql.exe` interfaccia utente grafica dello strumento viene installato lo schema di servizi di tutta l'applicazione. Ma quando si esegue `aspnet_regsql.exe` dalla riga di comando è possibile specificare quali particolare applicazione Servizi componenti per l'installazione (o rimozione). Pertanto, se si desidera aggiungere solo le tabelle, viste e stored procedure necessarie per il `SqlMembershipProvider` e `SqlRoleProvider` provider, eseguire `aspnet_regsql.exe` dalla riga di comando. In alternativa, è possibile eseguire manualmente i subset di T-SQL, creare script utilizzati da `aspnet_regsql.exe`. Questi script si trovano nel `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` cartella con nomi come `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`e così via.


A questo punto è stato creato gli oggetti di database necessiti per il `SqlMembershipProvider`. Tuttavia, è comunque necessario indicare il framework di appartenenza che utilizzi il `SqlMembershipProvider` (rispetto, ad esempio, il `ActiveDirectoryMembershipProvider`) e che il `SqlMembershipProvider` deve utilizzare il `SecurityTutorials` database. Verrà esaminato come specificare il provider da utilizzare e come personalizzare le impostazioni del provider selezionato nel passaggio 4. Innanzitutto, esaminiamo un dettaglio gli oggetti di database che sono stati appena creato.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Passaggio 3: Esaminare le tabelle di base dello Schema

Quando si utilizzano i framework di appartenenza e ruoli in un'applicazione ASP.NET, i dettagli di implementazione sono incapsulati dal provider. In futuro esercitazioni si possono interagire con questi Framework tramite .NET Framework `Membership` e `Roles` classi. Quando si utilizzano queste API di alto livello non occorre preoccuparsi effettuata con i dettagli di basso livello, ad esempio le query vengono eseguite o le tabelle vengono modificate dal `SqlMembershipProvider` e `SqlRoleProvider`.

Detto questo, è possibile usare il framework di appartenenza e i ruoli con fiducia senza avere esaminato lo schema del database creato nel passaggio 2. Tuttavia, durante la creazione di tabelle per archiviare i dati dell'applicazione è possibile che sia necessario creare entità correlate a utenti o ruoli. È utile disporre di una certa familiarità con la `SqlMembershipProvider` e `SqlRoleProvider` schemi quando si stabilisce esterna chiave vincoli tra le tabelle di dati dell'applicazione e tali tabelle create nel passaggio 2. Inoltre, in alcuni casi rari è possibile che sia necessario interfacciarsi con l'utente e la archivia ruolo direttamente a livello di database (anziché tramite il `Membership` o `Roles` classi).

### <a name="partitioning-the-user-store-into-applications"></a>Partizionamento archivio dell'utente nelle applicazioni

I framework di appartenenza e i ruoli sono progettati in modo che un singolo archivio utente e il ruolo può essere condivisi tra diverse applicazioni. Un'applicazione ASP.NET che utilizza il framework di ruoli o delle appartenenze è necessario specificare quale partizione di applicazione da utilizzare. In breve, più applicazioni web è possono utilizzare gli archivi di utente e il ruolo stesso. Figura 11 illustra archivi utente e il ruolo che sono suddivisi in tre applicazioni: HRSite CustomerSite e SalesSite. Queste applicazioni tre web ogni hanno i propri utenti univoci e i ruoli, ma tutti fisicamente archiviano le informazioni di account e il ruolo utente nelle tabelle del database stesso.


[![Gli account utente possono essere partizionati tra più applicazioni](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Figura 11**: utente account possono essere partizionati tra più applicazioni ([fare clic per visualizzare l'immagine ingrandita](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))


Il `aspnet_Applications` tabella è ciò che definisce tali partizioni. Ogni applicazione che utilizza il database per archiviare informazioni sull'account utente è rappresentato da una riga in questa tabella. Il `aspnet_Applications` tabella contiene quattro colonne: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, e `Description`. `ApplicationId`è di tipo [ `uniqueidentifier` ](https://msdn.microsoft.com/en-us/library/ms187942.aspx) e chiave primaria della tabella. `ApplicationName` fornisce un nome semplice umane univoco per ogni applicazione.

Le altre tabelle correlate di appartenenza e ruolo collegano nuovamente il `ApplicationId` campo `aspnet_Applications`. Ad esempio, il `aspnet_Users` tabella che contiene un record per ogni account utente, è un `ApplicationId` campo di chiave esterna; deriva per il `aspnet_Roles` tabella. Il `ApplicationId` campo in queste tabelle consente di specificare la partizione applicativa l'account utente o ruolo a cui appartiene.

### <a name="storing-user-account-information"></a>L'archiviazione delle informazioni sull'Account utente

Informazioni sull'account utente si trova in due tabelle: `aspnet_Users` e `aspnet_Membership`. Il `aspnet_Users` tabella contiene campi che contengono le informazioni essenziali dell'account. Le tre colonne più pertinenti sono:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId`è la chiave primaria (e di tipo `uniqueidentifier`). `UserName`è di tipo `nvarchar(256)` e, e la password, costituiscono le credenziali dell'utente. (La password dell'utente viene archiviata nel `aspnet_Membership` tabella.) `ApplicationId` l'account utente è collegato a una specifica applicazione in `aspnet_Applications`. Vi è un raggruppamento [ `UNIQUE` vincolo](https://msdn.microsoft.com/en-us/library/ms191166.aspx) sul `UserName` e `ApplicationId` colonne. Ciò garantisce che una determinata applicazione ogni nome utente è univoco, ma consente per lo stesso `UserName` da utilizzare in applicazioni diverse.

Il `aspnet_Membership` tabella include informazioni sull'account utente aggiuntivi, ad esempio la password dell'utente, indirizzo di posta elettronica, l'ultimo account di accesso data e ora e così via. È presente una corrispondenza tra i record di `aspnet_Users` e `aspnet_Membership` tabelle. Questa relazione è garantita tramite la `UserId` campo `aspnet_Membership`, che funge da chiave primaria della tabella. Ad esempio il `aspnet_Users` tabella `aspnet_Membership` include un `ApplicationId` campo che consente di associare queste informazioni per una determinata partizione applicativa.

### <a name="securing-passwords"></a>Protezione delle password

Informazioni relative alle password viene archiviati nel `aspnet_Membership` tabella. Il `SqlMembershipProvider` consente la password da archiviare nel database di utilizzando una delle tre tecniche seguenti:

- **Deselezionare** -la password viene archiviata nel database come testo normale. Fortemente scoraggiare utilizzando questa opzione. Se il database è compromesso, sia esso un dispositivo da un pirata informatico che consente di trovare una porta o un scontento dipendente che ha accesso al database, le credenziali dell'utente ogni singolo esistono per il prelievo.
- **L'hashing** -password viene eseguito l'hashing mediante un algoritmo di hash unidirezionale e un valore salt generato casualmente. Questo valore con hash (con il valore salt) viene archiviato nel database.
- **Crittografati** -una versione crittografata della password viene archiviata nel database.

La tecnica di archiviazione della password utilizzata varia a seconda di `SqlMembershipProvider` le impostazioni specificate in `Web.config`. Verranno esaminati personalizzazione di `SqlMembershipProvider` impostazioni nel passaggio 4. Il comportamento predefinito consiste nell'archiviare l'hash della password.

Le colonne responsabile per la memorizzazione della password sono `Password`, `PasswordFormat`, e `PasswordSalt`. `PasswordFormat`è un campo di tipo `int` il cui valore indica la tecnica usata per archiviare la password: 0 per cancellare, 1 per Hashed; 2 per la crittografia. `PasswordSalt`viene assegnata una stringa generata in modo casuale indipendentemente dalla tecnica di archiviazione di password utilizzata; il valore di `PasswordSalt` viene utilizzato solo quando il calcolo dell'hash della password. Infine, il `Password` colonna contiene i dati di password effettiva, ovvero la password in testo normale, l'hash della password o la password crittografata.

Tabella 1 illustra queste tre colonne aspetto per le varie tecniche di archiviazione quando si archiviano le password MySecret! .

| **Archiviazione tecnica&lt;\_o3a\_p /&gt;** | **Password&lt;\_o3a\_p /&gt;** | **Elemento PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Cancella | MySecret! | 0 | tTnkPlesqissc2y2SMEygA = = |
| L'hashing | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM = | 1 | wFgjUfhdUFOCKQiI61vtiQ = = |
| Crittografato | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | Aa/LSRzhGS/oqAXGLHJNBw = = |

**Tabella 1**: i valori di esempio per i campi correlati alla Password per l'archiviazione MySecret la Password.

> [!NOTE]
> La crittografia specifica o un algoritmo hash utilizzato dal `SqlMembershipProvider` è determinato dalle impostazioni di `<machineKey>` elemento. Descritto nel passaggio 3 di questo elemento di configurazione di <a id="Tutorial3"> </a> [ *configurazione dell'autenticazione form e argomenti avanzati* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) esercitazione.


### <a name="storing-roles-and-role-associations"></a>L'archiviazione dei ruoli e le associazioni di ruolo

Il framework di ruoli consente agli sviluppatori di definire un set di ruoli e specificare gli utenti che appartengono a quali ruoli. Queste informazioni vengono acquisite nel database tramite due tabelle: `aspnet_Roles` e `aspnet_UsersInRoles`. Ogni record di `aspnet_Roles` tabella rappresenta un ruolo per una particolare applicazione. Analogamente il `aspnet_Users` tabella, la `aspnet_Roles` tabella contiene tre colonne pertinenti per l'articolo:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId`è la chiave primaria (e di tipo `uniqueidentifier`). `RoleName` è di tipo `nvarchar(256)`. E `ApplicationId` l'account utente è collegato a una specifica applicazione in `aspnet_Applications`. Vi è un raggruppamento `UNIQUE` vincolo il `RoleName` e `ApplicationId` colonne, garantendo che una determinata applicazione il nome del ruolo sia univoco.

Il `aspnet_UsersInRoles` tabella funge da un mapping tra utenti e ruoli. Sono presenti solo due colonne - `UserId` e `RoleId` -insieme che costituiscono una chiave primaria composta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Passaggio 4: Specifica del Provider e le relative impostazioni di personalizzazione

Tutti i framework che supportano il modello di provider, ad esempio i framework di appartenenza e ruoli - non dispongono di dettagli di implementazione autonomamente e invece delegare tale compito a una classe di provider. Nel caso il framework di appartenenza, la `Membership` classe definisce l'API per la gestione degli account utente, ma non interagisce direttamente con qualsiasi archivio dell'utente. Invece di `Membership` la richiesta al provider di configurazione - consegnare metodi della classe che verrà usato il `SqlMembershipProvider`. Quando si richiama uno dei metodi di `Membership` (classe), come il framework di appartenenza conosce per delegare la chiamata al `SqlMembershipProvider`?

Il `Membership` classe dispone di un [ `Providers` proprietà](https://msdn.microsoft.com/en-us/library/system.web.security.membership.providers.aspx) che contiene un riferimento a tutte le classi provider registrato disponibile per l'utilizzo dal framework di appartenenza. Ogni provider registrato con un nome associato e il tipo. Il nome offre un modo semplice umane per fare riferimento a un determinato provider nel `Providers` raccolta, mentre il tipo identifica la classe provider. Inoltre, ogni provider registrato può includere le impostazioni di configurazione. Impostazioni di configurazione per il framework di appartenenza includono `passwordFormat` e `requiresUniqueEmail`, tra molti altri. Vedere la tabella 2 per un elenco completo delle impostazioni di configurazione di `SqlMembershipProvider`.

Il `Providers` contenuto della proprietà è specificato tramite le impostazioni di configurazione dell'applicazione web. Per impostazione predefinita, tutte le applicazioni web dispongono di un provider denominato `AspNetSqlMembershipProvider` di tipo `SqlMembershipProvider`. Questo provider di appartenenze predefinito è registrato `machine.config` (posizione `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Come il codice precedente, il [ `<membership>` elemento](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx) definisce le impostazioni di configurazione per il framework di appartenenza durante la [ `<providers>` elemento figlio](https://msdn.microsoft.com/en-us/library/6d4936ht.aspx) specifica registrato provider. I provider possono essere aggiunti o rimossi tramite il [ `<add>` ](https://msdn.microsoft.com/en-us/library/whae3t94.aspx) o [ `<remove>` ](https://msdn.microsoft.com/en-us/library/aykw9a6d.aspx) elementi; usare il [ `<clear>` ](https://msdn.microsoft.com/en-us/library/t062y6yc.aspx) elemento da rimuovere tutti attualmente provider registrati. Come il codice precedente, `machine.config` aggiunge un provider denominato `AspNetSqlMembershipProvider` di tipo `SqlMembershipProvider`.

Oltre al `name` e `type` gli attributi, la `<add>` elemento contiene attributi che definiscono i valori per varie impostazioni di configurazione. Nella tabella 2 sono disponibili `SqlMembershipProvider`-impostazioni di configurazione specifiche, insieme a una descrizione di ognuno.

> [!NOTE]
> Tutti i valori predefiniti indicati nella tabella 2 fare riferimento ai valori predefiniti definiti nel `SqlMembershipProvider` classe. Si noti che non tutte le impostazioni di configurazione in `AspNetSqlMembershipProvider` corrispondono ai valori predefiniti del `SqlMembershipProvider` classe. Ad esempio, se non specificato in un provider di appartenenze di `requiresUniqueEmail` definizione delle impostazioni predefinite su true. Tuttavia, il `AspNetSqlMembershipProvider` esegue l'override di questo valore predefinito, in modo esplicito specificando un valore di `false`.


| **Impostazione&lt;\_o3a\_p /&gt;** | **Descrizione&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Tenere presente che il framework di appartenenza consente per un archivio utente singolo essere partizionati tra più applicazioni. Questa impostazione indica il nome della partizione di applicazione utilizzata dal provider di appartenenze. Se questo valore non è specificato in modo esplicito, è impostato, in fase di esecuzione, il valore del percorso radice virtuale dell'applicazione. |
| `commandTimeout` | Specifica il valore di timeout del comando SQL (in secondi). Il valore predefinito è 30. |
| `connectionStringName` | Il nome della stringa di connessione nel `<connectionStrings>` elemento da utilizzare per la connessione al database di archivio utente. Questo valore è obbligatorio. |
| `description` | Fornisce una descrizione umane descrittivo del provider registrati. |
| `enablePasswordRetrieval` | Specifica se gli utenti possono recuperare la password dimenticata. Il valore predefinito è `false`. |
| `enablePasswordReset` | Indica se gli utenti sono autorizzati a reimpostare la password. Il valore predefinito è `true`. |
| `maxInvalidPasswordAttempts` | Il numero massimo di tentativi di accesso non riuscito che possono verificarsi per un determinato utente durante l'oggetto specificato `passwordAttemptWindow` prima che l'utente venga bloccato. Il valore predefinito è 5. |
| `minRequiredNonalphanumericCharacters` | Il numero minimo di caratteri non alfanumerici deve essere visualizzato in una password. Questo valore deve essere compreso tra 0 e 128; il valore predefinito è 1. |
| `minRequiredPasswordLength` | Il numero minimo di caratteri richiesti nella password. Questo valore deve essere compreso tra 0 e 128; il valore predefinito è 7. |
| `name` | Il nome del provider registrati. Questo valore è obbligatorio. |
| `passwordAttemptWindow` | Il numero di minuti durante i quali non vengono registrati i tentativi di accesso. Se un utente fornisce credenziali di accesso non valido `maxInvalidPasswordAttempts` sono state specificate ore all'interno di questa finestra, sono bloccati. Il valore predefinito è 10. |
| `PasswordFormat` | Il formato di archiviazione delle password: `Clear`, `Hashed`, o `Encrypted`. Il valore predefinito è `Hashed`. |
| `passwordStrengthRegularExpression` | Se specificato, questa espressione regolare viene utilizzata per valutare il livello di attendibilità della password dell'utente selezionato quando si crea un nuovo account o la modifica delle password. Il valore predefinito è una stringa vuota. |
| `requiresQuestionAndAnswer` | Specifica se un utente deve rispondere la domanda di sicurezza durante il recupero o la reimpostazione della password. Il valore predefinito è `true`. |
| `requiresUniqueEmail` | Indica se tutti gli account utente in una partizione di applicazione specificata devono avere un indirizzo di posta elettronica univoco. Il valore predefinito è `true`. |
| `type` | Specifica il tipo del provider. Questo valore è obbligatorio. |

**Tabella 2**: l'appartenenza e `SqlMembershipProvider` le impostazioni di configurazione

Oltre a `AspNetSqlMembershipProvider`, altri provider di appartenenze può essere registrato su base dall'applicazione aggiungendo markup simile al `Web.config` file.

> [!NOTE]
> Il framework di ruoli funziona nello stesso modo: è un provider di ruoli registrato predefinito nel `machine.config` e i provider registrati possono essere personalizzati in modo da applicazioni in `Web.config`. Verrà esaminato il framework di ruoli e il codice di configurazione in modo dettagliato in un'esercitazione future.


### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalizzazione di`SqlMembershipProvider`impostazioni

Il valore predefinito `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) è relativo `connectionStringName` attributo impostato su `LocalSqlServer`. Ad esempio il `AspNetSqlMembershipProvider` provider, il nome di stringa di connessione `LocalSqlServer` è definito in `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Come si può notare, la stringa di connessione definisce una posizione del database di SQL 2005 Express Edition | DataDirectory|aspnetdb.mdf. La stringa | DataDirectory | viene convertito in fase di esecuzione in modo che punti al `~/App_Data/` directory, pertanto il percorso del database | DataDirectory|aspnetdb.mdf"viene convertita in `~/App_Data` / `aspnet.mdf`.

Se non è stato specificato le informazioni di provider di appartenenze dell'applicazione `Web.config` file, l'applicazione utilizza il provider di appartenenze predefinito registrato, `AspNetSqlMembershipProvider`. Se il `~/App_Data/aspnet.mdf` database non esiste, il runtime di ASP.NET verrà automaticamente creato e aggiungere lo schema di servizi di applicazione. Tuttavia, non si desidera utilizzare il `aspnet.mdf` database; invece, si desidera utilizzare il `SecurityTutorials.mdf` database creati nel passaggio 2. Questa modifica può essere eseguita in uno dei due modi:

- **Specificare un valore per il * * *`LocalSqlServer`* * * il nome di stringa di connessione in * * *`Web.config`* * *.** Sovrascrivendo il `LocalSqlServer` valore di nome di stringa di connessione in `Web.config`, è possibile utilizzare il provider di appartenenze predefinito registrato (`AspNetSqlMembershipProvider`) e fare in modo funzionare correttamente con il `SecurityTutorials.mdf` database. Questo approccio è appropriato se si è contenuto con le impostazioni di configurazione specificate da `AspNetSqlMembershipProvider`. Per ulteriori informazioni su questa tecnica, vedere [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog, [la configurazione di servizi applicativi di ASP.NET 2.0 per utilizzare SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- **Aggiungere un nuovo provider registrati di tipo * * *`SqlMembershipProvider`* * * e configurare il * * *`connectionStringName`* * * impostazione in modo che punti al * * *`SecurityTutorials.mdf`* * * database.** Questo approccio è utile negli scenari in cui si desidera personalizzare altre proprietà di configurazione oltre alla stringa di connessione di database. In progetti io uso sempre questo approccio a causa di sua flessibilità e la leggibilità.

Per poter aggiungere un nuovo provider registrato che fa riferimento il `SecurityTutorials.mdf` database, è innanzitutto necessario aggiungere un valore di stringa di connessione appropriata nel `<connectionStrings>` sezione `Web.config`. Il markup seguente aggiunge una nuova stringa di connessione denominata `SecurityTutorialsConnectionString` che fa riferimento a SQL Server 2005 Express Edition `SecurityTutorials.mdf` database il `App_Data` cartella.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Se si utilizza un file di database alternativo, aggiornare la stringa di connessione in base alle esigenze. Per ulteriori informazioni su che costituiscono la stringa di connessione corretto, fare riferimento a [ConnectionStrings.com](http://www.connectionstrings.com/).

Successivamente, aggiungere il seguente markup di configurazione di appartenenza per il `Web.config` file. In questo markup registra un nuovo provider denominato `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Oltre a registrare il `SecurityTutorialsSqlMembershipProvider` provider, il codice precedente viene definito il `SecurityTutorialsSqlMembershipProvider` come provider predefinito (tramite il `defaultProvider` attributo il `<membership>` elemento). Tenere presente che il framework di appartenenza può avere più provider registrati. Poiché `AspNetSqlMembershipProvider` viene registrato come primo provider nella `machine.config`, ma funge da provider predefinito a meno che non è indicato in caso contrario.

Attualmente, l'applicazione dispone di due provider registrati: `AspNetSqlMembershipProvider` e `SecurityTutorialsSqlMembershipProvider`. Tuttavia, prima di registrare il `SecurityTutorialsSqlMembershipProvider` provider è stato possibile sono cancellate tutte in precedenza provider registrati mediante l'aggiunta di un [ `<clear />` elemento](https://msdn.microsoft.com/en-us/library/t062y6yc.aspx) immediatamente prima il nostro `<add>` elemento. Questo potrebbe cancellare il `AspNetSqlMembershipProvider` dall'elenco dei provider registrati, il che significa che il `SecurityTutorialsSqlMembershipProvider` sarà l'unico provider di appartenenza registrato. Se viene utilizzato questo approccio, quindi non è necessario contrassegnare il `SecurityTutorialsSqlMembershipProvider` come il provider predefinito, poiché sia l'unico provider di appartenenza registrato. Per ulteriori informazioni sull'utilizzo `<clear />`, vedere [Using `<clear />` quando Aggiunta provider](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Si noti che il `SecurityTutorialsSqlMembershipProvider`del `connectionStringName` impostazione di riferimenti di appena aggiunti `SecurityTutorialsConnectionString` nome stringa di connessione e che il relativo `applicationName` impostazione è stata impostata su un valore di SecurityTutorials. Inoltre, il `requiresUniqueEmail` impostazione è stata impostata su `true`. Tutte le altre opzioni di configurazione sono identici a quelli in `AspNetSqlMembershipProvider`. È possibile apportare le modifiche di configurazione in questo caso, se si desidera. Ad esempio, si potrebbe rafforzare la complessità della password richiedendo due caratteri non alfanumerici invece di uno o aumentando la lunghezza della password a otto caratteri anziché sette.

> [!NOTE]
> Tenere presente che il framework di appartenenza consente per un archivio utente singolo essere partizionati tra più applicazioni. Il provider di appartenenze `applicationName` impostazione indica quale applicazione viene utilizzato il provider quando si utilizza l'archivio dell'utente. È importante impostare in modo esplicito un valore per il `applicationName` impostazione di configurazione perché se il `applicationName` non è impostato in modo esplicito, viene assegnato al percorso radice virtuale dell'applicazione web in fase di esecuzione. Utilizzare questa opzione, purché non cambia percorso radice virtuale dell'applicazione, ma se si sposta l'applicazione a un percorso diverso, il `applicationName` impostazione cambierà troppo. In questo caso, il provider di appartenenze inizierà a funzionare correttamente con una partizione di applicazione diverso rispetto a quello usato in precedenza. Gli account utente creati prima dello spostamento si trovano in una partizione di applicazione diverso e tali utenti non saranno in grado di accedere al sito. Per una discussione più dettagliata in merito, vedere [impostare sempre la `applicationName` proprietà quando la configurazione di ASP.NET 2.0 l'appartenenza e altri provider](https://weblogs.asp.net/scottgu/443634).


## <a name="summary"></a>Riepilogo

A questo punto si dispone di un database con i servizi delle applicazioni configurate (`SecurityTutorials.mdf`) e aver configurato l'applicazione web in modo che usi il framework di appartenenza di `SecurityTutorialsSqlMembershipProvider` provider è stato appena registrati. Questo provider registrato è di tipo `SqlMembershipProvider` e ha il `connectionStringName` impostato sulla stringa di connessione appropriata (`SecurityTutorialsConnectionString`) e il relativo `applicationName` valore impostato in modo esplicito.

Ora è pronti a utilizzare il framework di appartenenza dall'applicazione. Nella prossima esercitazione verrà esaminato creare nuovi account utente. Dopo che verranno analizzati l'autenticazione degli utenti, eseguire l'autorizzazione basata sull'utente e l'archiviazione di informazioni aggiuntive relative agli utenti nel database.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Impostare sempre la `applicationName` proprietà durante la configurazione di ASP.NET 2.0 l'appartenenza e altri provider](https://weblogs.asp.net/scottgu/443634)
- [Configurazione di ASP.NET 2.0 servizi delle applicazioni per utilizzare SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Scaricare SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Analisi di ASP.NET 2.0 s appartenenza, ruoli e profilo](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Il `<add>` elemento per i provider di appartenenza](https://msdn.microsoft.com/en-us/library/whae3t94.aspx)
- [Il `<membership>` elemento](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx)
- [Il `<providers>` elemento per l'appartenenza](https://msdn.microsoft.com/en-us/library/6d4936ht.aspx)
- [Utilizzando `<clear />` quando si aggiunge provider](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Si lavora direttamente con il`SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video di formazione su argomenti contenuti in questa esercitazione

- [Informazioni sulle appartenenze ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurazione di SQL per l'utilizzo con gli schemi di appartenenza](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modifica delle impostazioni di appartenenza nello Schema di appartenenze predefinito](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Alicja Maziarz. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Successivo](creating-user-accounts-cs.md)
