---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: migrazione a SQL Server - 10 pari a 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc0bca18d2f6e4cdbb527ab75952e9a50eb49b20
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827362"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: migrazione a SQL Server - 10 pari a 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come eseguire la migrazione da SQL Server Compact a SQL Server. Un motivo per cui che si potrebbe voler eseguire questa operazione è possa sfruttare i vantaggi delle funzionalità di SQL Server che SQL Server Compact non supporta, ad esempio le stored procedure, trigger, viste o replica. Per altre informazioni sulle differenze tra SQL Server Compact e SQL Server, vedere la [distribuzione di SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) esercitazione.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express e completa di SQL Server per lo sviluppo

Una volta che si è deciso di eseguire l'aggiornamento a SQL Server, si potrebbe voler usare SQL Server o SQL Server Express negli ambienti di sviluppo e test. Oltre alle differenze nel supporto di strumenti e funzionalità del motore di database, esistono differenze nelle implementazioni del provider SQL Server Compact e altre versioni di SQL Server. Queste differenze possono causare lo stesso codice generare risultati diversi. Pertanto, se si decide di mantenere il database di sviluppo di SQL Server Compact, è necessario testarne il sito in SQL Server o SQL Server Express in un ambiente di test prima di ogni distribuzione nell'ambiente di produzione.

A differenza di SQL Server Compact, SQL Server Express è essenzialmente lo stesso motore di database e Usa lo stesso provider .NET come completa di SQL Server. Quando esegue il test con SQL Server Express, è possibile essere certi di ottenere gli stessi risultati così come con SQL Server. È possibile usare la maggior parte degli stessi strumenti di database con SQL Server Express, è possibile usare con SQL Server (da un'eccezione rilevante [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), e supporta altre funzionalità di SQL Server, ad esempio stored procedure, viste, trigger, e la replica. (In genere è necessario tuttavia utilizzare completa di SQL Server in un sito Web di produzione. SQL Server Express è possibile eseguire in un ambiente di hosting condiviso, ma non è stato progettato per tale e non è supportato da molti provider di hosting.)

Se si usa Visual Studio 2012, in genere è scegliere LocalDB di SQL Server Express per l'ambiente di sviluppo perché questo è ciò che viene installato per impostazione predefinita con Visual Studio. Tuttavia, Local DB non funziona in IIS, quindi per l'ambiente di test è necessario utilizzare SQL Server o SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Combinare i database rispetto a mantenendoli separati

L'applicazione Contoso University è disponibili due database di SQL Server Compact: il database delle appartenenze (*aspnet.sdf*) e il database dell'applicazione (*School.sdf*). Quando esegue la migrazione, è possibile migrare i database a due distinti database o a un singolo database. È possibile combinarli per facilitare il join di database tra il database dell'applicazione e il database di appartenenza. Il piano di hosting può anche fornire un motivo per combinarle. Ad esempio, il provider di hosting può addebitare informazioni per più database oppure potrebbe non consentire anche di più di un database. Che avviene con l'account utilizzato per questa esercitazione, che consente solo un singolo database di SQL Server di hosting Cytanium Lite.

In questa esercitazione, si saranno la migrazione dei due database in questo modo:

- Eseguire la migrazione a due database LocalDB nell'ambiente di sviluppo.
- Eseguire la migrazione a due database di SQL Server Express nell'ambiente di test.
- Eseguire la migrazione a un database SQL Server completo combinato nell'ambiente di produzione.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Installazione di SQL Server Express

SQL Server Express viene installato automaticamente per impostazione predefinita con Visual Studio 2010, ma per impostazione predefinita non è installato con Visual Studio 2012. Per installare SQL Server 2012 Express, fare clic sul collegamento seguente

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Scegli *ita/x64/SQLEXPR\_x64\_ENU.exe* oppure *ita/x86/SQLEXPR\_x86\_ENU.exe*e nell'installazione guidata accettare l'impostazione predefinita Impostazioni. Per altre informazioni sulle opzioni di installazione, vedere [installare SQL Server 2012 dall'installazione guidata (programma di installazione)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Creazione di database SQL Server Express per l'ambiente di Test

Il passaggio successivo consiste nel creare i database dell'istituto di istruzione e il sistema di appartenenze ASP.NET.

Dal **View** dal menu **Esplora Server** (**Esplora Database** in Visual Web Developer) e quindi fare doppio clic su **connessioni dati**e selezionare **Crea nuovo Database di SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

Nel **Crea nuovo Database di SQL Server** finestra di dialogo immettere ". \SQLExpress" nel **nome Server** casella e "aspnet-Test" nel **nuovo nome del database** casella, quindi fare clic su **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Seguire la stessa procedura per creare un nuovo database dell'istituto di istruzione di SQL Server Express denominato "Test dell'istituto di istruzione".

(Che si sta accodando "Test" per questi nomi di database perché è necessario essere in grado di differenziare i due set di database e in seguito si creerà un'istanza aggiuntiva di ogni database per l'ambiente di sviluppo.)

**Esplora server** ora mostra i due nuovi database.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Creazione di uno Script di concessione per i nuovi database

Quando l'applicazione viene eseguita in IIS nel computer di sviluppo, l'applicazione accede al database usando le credenziali del pool di applicazioni predefinito. Tuttavia, per impostazione predefinita, l'identità del pool di applicazioni non dispone dell'autorizzazione per aprire i database. Pertanto, è necessario eseguire uno script per concedere tale autorizzazione. In questa sezione si crea lo script che verrà eseguito successivamente per assicurarsi che l'applicazione può aprire i database quando è in esecuzione in IIS.

Della soluzione *SolutionFiles* creato nella cartella la [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione, creare un nuovo file SQL denominato *Grant.sql*. Copiare i comandi SQL seguenti nel file, quindi salvare e chiudere il file:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Questo script è progettato per funzionare con SQL Server 2008 e le impostazioni di IIS in Windows 7, secondo quanto specificato in questa esercitazione. Se si usa una versione diversa di SQL Server o di Windows, o se si configura IIS nel computer in modo diverso, le modifiche apportate allo script potrebbero essere necessari. Per altre informazioni sugli script di SQL Server, vedere [documentazione Online di SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Nota sulla sicurezza** nello script viene db\_autorizzazioni di proprietario per l'utente che accede al database in fase di esecuzione, che è ciò che è possibile nell'ambiente di produzione. In alcuni scenari è possibile specificare un utente che contiene lo schema completo del database, aggiornare le autorizzazioni solo per la distribuzione e specificare in fase di esecuzione di un utente diverso che disponga delle autorizzazioni solo per leggere e scrivere dati. Per altre informazioni, vedere **rivedono le modifiche automatiche di Web. config per le migrazioni Code First** nelle [distribuzione in IIS come ambiente di Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Configurazione della distribuzione di Database per l'ambiente di Test

Si configurerà quindi Visual Studio in modo che eseguiranno le attività seguenti per ogni database:

- Generare uno script SQL che crea una struttura del database di origine (tabelle, colonne, vincoli e così via) nel database di destinazione.
- Generare uno script SQL che inserisce i dati del database di origine nelle tabelle nel database di destinazione.
- Eseguire gli script generati e lo script di concessione che è stato creato, nel database di destinazione.

Aprire il **proprietà progetto** finestra e selezionare il **pubblicazione/creazione pacchetto SQL** scheda.

Assicurarsi che **attiva (rilascio)** o **Release** sia selezionato nel **configurazione** elenco a discesa.

Fare clic su **abilita questa pagina**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

Il **pubblicazione/creazione pacchetto SQL** scheda è generalmente disabilitata perché specifica un metodo di distribuzione legacy. Per la maggior parte degli scenari, è necessario configurare la distribuzione di database nel **pubblica sul Web** procedura guidata. La migrazione da SQL Server Compact a SQL Server o SQL Server Express è un caso speciale per il quale questo metodo è una buona scelta.

Fare clic su **Importa da Web. config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio cerca le stringhe di connessione nel *Web. config* file, non viene trovata una per il database di appartenenza e uno per il database School e aggiunge una riga corrispondente a ogni stringa di connessione nel **voci di Database**  tabella. Trova le stringhe di connessione sono per i database di SQL Server Compact esistenti e il passaggio successivo sarà possibile configurare come e dove distribuire questi database.

È possibile immettere impostazioni di distribuzione di database nel **dettagli voci Database** sezione riportata di seguito il **voci di Database** tabella. Le impostazioni riportate nella **dettagli voci Database** sezione si riferiscono a nella seconda riga il **voci di Database** tabella è selezionata, come illustrato nella figura seguente.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configurazione delle impostazioni di distribuzione per il Database di appartenenza

Selezionare il **DefaultConnection-Deployment** nella riga il **voci di Database** tabella per configurare le impostazioni che si applicano al database di appartenenza.

Nelle **stringa di connessione per il database di destinazione**, immettere una stringa di connessione che punti al nuovo database di appartenenze di SQL Server Express. È possibile ottenere la stringa di connessione è necessario richiedere al **Esplora Server**. Nella **Esplora Server**, espandere **connessioni dati** e selezionare il **aspnetTest** database, quindi dal **proprietà** copia finestra il **Stringa di connessione** valore.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Stessa stringa di connessione viene riprodotto di seguito:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copiare e incollare questa stringa di connessione nel **stringa di connessione per il database di destinazione** nel **pubblicazione/creazione pacchetto SQL** scheda.

Verificare che l'opzione **eseguire il Pull dei dati e/o schema da un database esistente** sia selezionata. Questo è ciò che fa in modo che gli script SQL essere automaticamente generato ed eseguito nel database di destinazione.

Il **stringa di connessione per il database di origine** estrazione del valore dalle *Web. config* file e i punti al database di SQL Server Compact di sviluppo. Si tratta del database di origine che verrà usato per generare gli script che verranno eseguito in un secondo momento nel database di destinazione. Poiché si vuole distribuire la versione di produzione del database, modificare "aspnet-Dev.sdf" in "aspnet-Prod.sdf".

Change **opzioni di scripting del Database** dalla **solo Schema** a **schemi e dati**, dal momento che si desidera copiare i dati (account utente e ruoli), nonché la struttura del database.

Per configurare la distribuzione per eseguire gli script di concessione creato in precedenza, è necessario aggiungerli al **script di Database** sezione. Fare clic su **Aggiungi Script**e nel **aggiungere gli script SQL** finestra di dialogo passare alla cartella in cui è archiviato lo script di concessione (questa è la cartella che contiene i file di soluzione). Selezionare il file denominato *Grant.sql*, fare clic su **Open**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Le impostazioni per il **DefaultConnection-Deployment** nella riga **voci di Database** ora simile al seguente:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configurazione delle impostazioni di distribuzione per il Database School

Selezionare quindi il **SchoolContext-Deployment** nella riga il **voci di Database** tabella per configurare le impostazioni di distribuzione per il database School.

È possibile usare lo stesso metodo usato in precedenza per ottenere la stringa di connessione per il nuovo database di SQL Server Express. Copiare questa stringa di connessione in **stringa di connessione per il database di destinazione** nel **pubblicazione/creazione pacchetto SQL** scheda.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Verificare che l'opzione **eseguire il Pull dei dati e/o schema da un database esistente** sia selezionata.

Il **stringa di connessione per il database di origine** estrazione del valore dalle *Web. config* file e i punti al database di SQL Server Compact di sviluppo. Modificare "Dev.sdf dell'istituto di istruzione" in "School-Prod.sdf" per distribuire la versione di produzione del database. (Mai creato un file dell'istituto di istruzione Prod.sdf nell'App\_cartella di dati, pertanto è possibile copiare tale file dall'ambiente di testing all'App\_cartella di dati nella cartella del progetto di ContosoUniversity in un secondo momento.)

Change **opzioni di scripting del Database** al **dello Schema e dati**.

Si desidera eseguire lo script per concedono autorizzazioni di lettura e scrittura l'autorizzazione per questo database per l'identità del pool, aggiungere il *Grant.sql* file di script come fatto in precedenza per il database di appartenenza.

Al termine, le impostazioni il **SchoolContext-Deployment** nella riga **voci di Database** simile al seguente:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Salvare le modifiche per il **pubblicazione/creazione pacchetto SQL** scheda.

Copia il *School-Prod.sdf* del file dal *c:\inetpub\wwwroot\ContosoUniversity\App\_Data* cartella in cui il *App\_dati* cartella in il progetto ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Specifica della modalità transazionale per lo Script di concessione

Il processo di distribuzione che genera l'errore di script che distribuisce lo schema di database e i dati. Per impostazione predefinita, questi script vengono eseguiti in una transazione. Tuttavia, gli script personalizzati (ad esempio, gli script di concessione) per impostazione predefinita non vengono eseguiti in una transazione. Se il processo di distribuzione sono presenti le modalità di transazione, si verifichi un errore di timeout quando gli script eseguiti durante la distribuzione. In questa sezione si modifica il file di progetto per configurare gli script personalizzati per l'esecuzione in una transazione.

Nelle **Esplora soluzioni**, fare doppio clic il **ContosoUniversity** del progetto e selezionare **Scarica progetto**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Quindi fare nuovamente clic sul progetto e selezionare **modifica Contosouniversity**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Editor di Visual Studio Mostra il contenuto XML del file di progetto. Si noti che esistono diversi `PropertyGroup` elementi. (Nell'immagine, il contenuto del `PropertyGroup` gli elementi sono stati omessi.)

![Finestra dell'editor di file progetto](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Il primo webhook che non ha alcun `Condition` dell'attributo, sia per le impostazioni che si applicano indipendentemente dalla configurazione di compilazione. Uno `PropertyGroup` elemento si applica solo alla configurazione della build di Debug (nota di `Condition` attributo), si applica solo alla configurazione della build di rilascio e si applica solo alla configurazione della build di Test. All'interno di `PropertyGroup` (elemento) per la configurazione della build di rilascio, si noterà una `PublishDatabaseSettings` elemento che contiene le impostazioni immesse nella **pubblicazione/creazione pacchetto SQL** scheda. È presente un `Object` elemento che corrisponde a ognuno degli script di concessione è (si noti che le due istanze specificate dell'oggetto "Grant.sql"). Per impostazione predefinita, il `Transacted` attributo del `Source` (elemento) per ogni script di concessione è `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Modificare il valore della `Transacted` attributo del `Source` elemento `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Salvare e chiudere il file di progetto e quindi fare clic sul progetto in **Esplora soluzioni** e selezionare **Ricarica progetto**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Configurazione di trasformazioni di Web. config per le stringhe di connessione

Le stringhe di connessione per la versione SQL Express nuovi database che sono stati immessi **pubblicazione/creazione pacchetto SQL** scheda vengono utilizzati da distribuzione Web solo per l'aggiornamento del database di destinazione durante la distribuzione. Comunque necessario configurare *Web. config* trasformazioni in modo che le stringhe di connessione nella distribuito *Web. config* file punto con i nuovi database di SQL Server Express. (Quando si usa la **pubblicazione/creazione pacchetto SQL** scheda, è possibile configurare le stringhe di connessione nel profilo di pubblicazione.)

Aprire *Web.Test.config* e sostituire il `connectionStrings` elemento con la `connectionStrings` elemento nell'esempio seguente. (Assicurarsi di copiare solo l'elemento connectionStrings, non il codice circostante riportato di seguito per fornire contesto).

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Questo codice fa in modo che il `connectionString` e `providerName` attributi della ognuno `add` elemento da sostituire in distribuito *Web. config* file. Queste stringhe di connessione non sono identici a quelli immessi nella **pubblicazione/creazione pacchetto SQL** scheda. L'impostazione "MultipleActiveResultSets = True" è stato aggiunto ad essi, perché è necessaria per Entity Framework e i provider universali.

## <a name="installing-sql-server-compact"></a>Installazione di SQL Server Compact

Il pacchetto SqlServerCompact NuGet fornisce database di SQL Server Compact assembly motore per l'applicazione Contoso University. Ma ora non è l'applicazione, ma che devono essere in grado di leggere i database di SQL Server Compact, per poter creare gli script da eseguire nel database di SQL Server di distribuzione Web. Per abilitare distribuzione Web leggere i database di SQL Server Compact, installare SQL Server Compact nel computer di sviluppo usando il collegamento seguente: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Distribuzione nell'ambiente di Test

Per pubblicare l'ambiente di Test, è necessario creare un profilo di pubblicazione che è configurato per usare la **pubblicazione/creazione pacchetto SQL** scheda per il database di pubblicazione anziché le impostazioni del database di profilo di pubblicazione.

In primo luogo, eliminare il profilo di Test esistente.

Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity e fare clic su **Publish**.

Selezionare il **profilo** scheda.

Fare clic su **gestire i profili**.

Selezionare **Test**, fare clic su **rimuovere**, quindi fare clic su **Chiudi**.

Chiudi il **pubblica sul Web** guidata consente di salvare questa modifica.

Successivamente, creare un nuovo profilo di Test e usarlo per pubblicare il progetto.

Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity e fare clic su **Publish**.

Selezionare il **profilo** scheda.

Selezionare **&lt;New... &gt;** dall'elenco a discesa elenco e immettere "Test" come nome del profilo.

Nel **URL del servizio** casella, immettere *localhost*.

Nel **sito/applicazione** casella, immettere *Default Web Site/ContosoUniversity*.

Nel **URL di destinazione** immettere `http://localhost/ContosoUniversity/`.

Scegliere **Avanti**.

Il **le impostazioni** scheda avvisa l'utente che la **pubblicazione/creazione pacchetto SQL** scheda è stata configurata e offre la possibilità di eseguirne l'override scegliendo Abilita il nuovo database ottimizzazioni per la pubblicazione. Per questa distribuzione non si vuole eseguire l'override di **pubblicazione/creazione pacchetto SQL** impostazioni della scheda, quindi è sufficiente fare clic su **successivo**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Un messaggio sulla **Preview** scheda indica che **Nessun database selezionato per la pubblicazione**, ma questo significa semplicemente che la pubblicazione di database non è configurata nel profilo di pubblicazione.

Fare clic su **Pubblica**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio distribuisce l'applicazione e apre il browser alla home page del sito nell'ambiente di test. Eseguire la pagina instructors (insegnanti) per verificare che l'it consente di visualizzare gli stessi dati che hai visto in precedenza. Eseguire la **aggiungere gli studenti** pagina, aggiungere un nuovo studente e quindi visualizzare il nuovo studente nella **studenti** pagina. In questo modo si verifica che è possibile aggiornare il database. Selezionare il **crediti Update** pagina (è necessario per l'accesso) per verificare che il database di appartenenza è stato distribuito e si ha accesso a esso.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Creazione di un Database di SQL Server per l'ambiente di produzione

Ora che è stato distribuito nell'ambiente di test, si è pronti a configurare la distribuzione nell'ambiente di produzione. Si inizia come fatto in precedenza per l'ambiente di test, mediante la creazione di un database da distribuire per. Come si ricorderà da una panoramica di, il piano di hosting Cytanium Lite consente solo un singolo database di SQL Server, in modo che si desidera impostare un solo database, non due. Tutte le tabelle e i dati di appartenenza e dell'istituto di istruzione SQL Server Compact database verranno distribuite in un unico database di SQL Server nell'ambiente di produzione.

Passare al pannello di controllo Cytanium all'indirizzo [ http://panel.cytanium.com ](http://panel.cytanium.com). Posiziona il puntatore del mouse su **database** e quindi fare clic su **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

Nel **SQL Server 2008** pagina, fare clic su **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nome del database "School", quindi scegliere **salvare**. (La pagina aggiunge automaticamente il prefisso "contosou", in modo che il nome effettivo sarà "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Nella stessa pagina, fare clic su **Create User**. Nei server del Cytanium, piuttosto che usa la sicurezza integrata di Windows e lasciare che l'identità del pool di aprire il database, si creerà un utente che dispone delle autorizzazioni per aprire il database. Si aggiungerà le credenziali dell'utente per le stringhe di connessione che passano in produzione *Web. config* file. In questo passaggio viene creato tali credenziali.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Compilare i campi obbligatori nella **le proprietà utente SQL** pagina:

- Immettere "ContosoUniversityUser" come nome.
- Immettere una password.
- Selezionare **contosouSchool** come database predefinito.
- Selezionare il **contosouSchool** casella di controllo.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Configurazione della distribuzione di Database per l'ambiente di produzione

A questo punto è possibile configurare le impostazioni di distribuzione di database nel **pubblicazione/creazione pacchetto SQL** scheda, come in precedenza per l'ambiente di test.

Aprire il **le proprietà del progetto** finestra, seleziona la **pubblicazione/creazione pacchetto SQL** scheda e verificare che **attivo (versione)** o **versione** è selezionato nel **configurazione** elenco a discesa.

Quando si configurano le impostazioni di distribuzione per ogni database, la differenza principale tra operazioni per gli ambienti di produzione e di test è in modalità di configurazione le stringhe di connessione. Per l'ambiente di prova immesso le stringhe di connessione di database di destinazione diverso, ma per l'ambiente di produzione la stringa di connessione di destinazione sarà lo stesso per entrambi i database. Questo avviene perché si siano distribuendo entrambi i database in un database nell'ambiente di produzione.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configurazione delle impostazioni di distribuzione per il Database di appartenenza

Per configurare le impostazioni che si applicano al database di appartenenza, selezionare la **DefaultConnection-Deployment** nella riga il **voci di Database** tabella.

Nelle **stringa di connessione per il database di destinazione**, immettere una stringa di connessione che punti al nuovo database SQL Server di produzione appena creato. È possibile ottenere la stringa di connessione dal messaggio di benvenuto. La parte pertinente del messaggio di posta elettronica contiene la stringa di connessione di esempio seguente:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Dopo aver sostituito le tre variabili, la stringa di connessione che è necessario simile all'esempio:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copiare e incollare questa stringa di connessione nel **stringa di connessione per il database di destinazione** nel **pubblicazione/creazione pacchetto SQL** scheda.

Assicurarsi che **effettuare il Pull dei dati e/o schema da un database esistente** è ancora selezionato e che **opzioni di scripting del Database** resta **schemi e dati**.

Nel **script di Database** finestra, deselezionare la casella di controllo accanto a script Grant.sql.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configurazione delle impostazioni di distribuzione per il Database School

Selezionare quindi il **SchoolContext-Deployment** nella riga il **voci di Database** tabella per configurare le impostazioni del database School.

Copiare la stessa stringa di connessione in **stringa di connessione per il database di destinazione** copiato in tale campo per il database di appartenenza.

Assicurarsi che **effettuare il Pull dei dati e/o schema da un database esistente** è ancora selezionato e che **opzioni di scripting del Database** resta **schemi e dati**.

Nel **script di Database** finestra, deselezionare la casella di controllo accanto a script Grant.sql.

Salvare le modifiche per il **pubblicazione/creazione pacchetto SQL** scheda.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Trasformazioni di configurazione Web. config per le stringhe di connessione al database di produzione

Successivamente, si configurerà *Web. config* trasformazioni in modo che le stringhe di connessione nella distribuito *Web. config* file in modo che punti al nuovo database di produzione. La stringa di connessione che è stato immesso il **pubblicazione/creazione pacchetto SQL** scheda per distribuzione Web da utilizzare è identico a quello dell'applicazione deve usare, fatta eccezione per l'aggiunta del `MultipleResultSets` opzione.

Aprire *Web.Production.config* e sostituire il `connectionStrings` elemento con un `connectionStrings` elemento simile al seguente. (Copia solo il `connectionStrings` elemento, non i tag forniti per mostrare il contesto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Talvolta riscontrare indicazioni, che consente di crittografare sempre stringhe di connessione nel *Web. config* file. Ciò potrebbe essere appropriata se si distribuisse ai server nella rete dell'azienda. Quando distribuisce in un ambiente di hosting condiviso, tuttavia, vengono considerati attendibili le procedure di sicurezza del provider di hosting, e non è necessario o consigliabile crittografare le stringhe di connessione.

## <a name="deploying-to-the-production-environment"></a>Distribuzione nell'ambiente di produzione

È ora possibile distribuire nell'ambiente di produzione. La funzionalità distribuzione Web leggerà i database di SQL Server Compact del progetto *App\_dati* cartella e ricreare tutte le tabelle e i dati nel database di SQL Server di produzione. Per eseguire la pubblicazione usando il **pubblicazione/creazione pacchetto Web** le impostazioni di tabulazione, è necessario creare un nuovo profilo di pubblicazione per la produzione.

In primo luogo, eliminare il profilo di produzione esistente come il profilo di Test in precedenza.

Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity e fare clic su **Publish**.

Selezionare il **profilo** scheda.

Fare clic su **gestire i profili**.

Selezionare **produzione**, fare clic su **rimuovere**, quindi fare clic su **Chiudi**.

Chiudi il **pubblica sul Web** guidata consente di salvare questa modifica.

Successivamente, creare un nuovo profilo di produzione e usarlo per pubblicare il progetto.

Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity e fare clic su **Publish**.

Selezionare il **profilo** scheda.

Fare clic su **importazione**e selezionare il file con estensione publishsettings scaricato in precedenza.

Nel **Connection** scheda, modificare il **URL di destinazione** all'URL corretto temporaneo, che in questo esempio è http://contosouniversity.com.vserver01.cytanium.com.

Rinomina il profilo nell'ambiente di produzione. (Selezionare il **profilo** scheda e fare clic su **Gestione profili** a tale scopo).

Chiudi il **pubblica sul Web** guidata consente di salvare le modifiche.

In un'applicazione reale in cui è stato viene aggiornato il database nell'ambiente di produzione, è necessario effettuare ora i due passaggi aggiuntivi prima di pubblicare:

1. Caricare *app\_offline.htm*, come illustrato nella [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione.
2. Usare la **gestione File** funzionalità del Pannello di controllo Cytanium per copiare la *aspnet-Prod.sdf* e *School-Prod.sdf* file dal sito di produzione per il *App\_dati* cartella del progetto ContosoUniversity. Ciò garantisce che i dati che si distribuisce il nuovo database di SQL Server includano gli aggiornamenti più recenti apportati da siti Web di produzione.

Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, assicurarsi che le **produzione** profilo sia selezionata e quindi fare clic su **Publish**.

Se è stato caricato <em>app\_offline.htm</em> prima della pubblicazione, è necessario usare i <strong>gestione File</strong> utilità nel Pannello di controllo Cytanium eliminare <em>app\_offline.</em> htm prima di testare. È anche possibile allo stesso tempo eliminare il <em>sdf</em> dei file dalle <em>App\_dati</em> cartella.

È ora possibile aprire un browser e passare all'URL del sito pubblico per testare l'applicazione allo stesso modo che è stato eseguito dopo la distribuzione nell'ambiente di test.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Il passaggio a SQL Server Express LocalDB in fase di sviluppo

Come descritto nella panoramica, è preferibile usare lo stesso motore di database in fase di sviluppo in uso in produzione e test. (Tenere presente che il vantaggio dell'uso di SQL Server Express in fase di sviluppo è che il database funzionerà lo stesso nei propri ambienti di sviluppo, test e produzione). In questa sezione verrà configuri il progetto ContosoUniversity usare SQL Server Express LocalDB quando si esegue l'applicazione da Visual Studio.

Il modo più semplice per eseguire la migrazione è in Code First e il sistema di appartenenze creare entrambi i nuovi database lo sviluppo per l'utente. Con questo metodo per eseguire la migrazione richiede tre passaggi:

1. Modificare le stringhe di connessione per specificare i nuovi database LocalDB di SQL Express.
2. Eseguire lo strumento Amministrazione sito Web per creare un utente amministratore. Verrà creato il database di appartenenza.
3. Usare il comando update-database migrazioni Code First per creare ed eseguire il seeding del database dell'applicazione.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aggiornamento delle stringhe di connessione nel file Web. config

Aprire il *Web. config* del file e sostituire il `connectionStrings` elemento con il codice seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Creazione del Database di appartenenza

Nelle **Esplora soluzioni**, selezionare il progetto ContosoUniversity e quindi fare clic su **configurazione di ASP.NET** nel **progetto** menu.

Selezionare la scheda sicurezza.

Fare clic su **crea o Gestisci ruoli**, quindi creare un **amministratore** ruolo.

Tornare alla scheda sicurezza.

Fare clic su **Create user**, quindi selezionare la **amministratore** casella di controllo e creare un utente denominato amministratore.

Chiudi il **strumento Amministrazione sito Web**.

### <a name="creating-the-school-database"></a>Creazione del Database School

Aprire la finestra della Console di gestione pacchetti.

Nel **progetto predefinito** elenco a discesa selezionare il progetto ContosoUniversity.DAL.

Immettere il comando seguente:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migrazioni Code First si applica la migrazione iniziale che crea il database e quindi applica la migrazione AddBirthDate, quindi viene eseguito il metodo di inizializzazione.

Eseguire il sito premendo CTRL+F5. Come per gli ambienti di test e produzione, eseguire la **aggiungere gli studenti** pagina, aggiungere un nuovo studente e quindi visualizzare il nuovo studente nella **studenti** pagina. Verifica che il database School è stato creato e inizializzato e che si sono letti e l'accesso in scrittura a esso.

Selezionare il **crediti Update** pagina ed eseguire l'accesso per verificare che il database di appartenenza è stato distribuito e di avere accesso a esso. Se non sono stati migrati gli account utente, creare un account amministratore e quindi selezionare il **crediti Update** pagina per verificarne il funzionamento.

## <a name="cleaning-up-sql-server-compact-files"></a>Pulizia dei file SQL Server Compact

Non è più necessario file e pacchetti NuGet che sono stati inclusi per supportare SQL Server Compact. Se si desidera (in questo passaggio non è obbligatorio), è possibile pulire i file non necessari e i riferimenti.

Nella **Esplora soluzioni**, eliminare il *sdf* dei file dal *App\_dati* cartella e il *amd64* e*x86* le cartelle dal *bin* cartella.

Nelle **Esplora soluzioni**, fare doppio clic su della soluzione (non uno dei progetti) e quindi fare clic su **Gestisci pacchetti NuGet per la soluzione**.

Nel riquadro sinistro della finestra di **Gestisci pacchetti NuGet** finestra di dialogo **i pacchetti installati**.

Selezionare il **EntityFramework.SqlServerCompact** del pacchetto e fare clic su **Gestisci**.

Nel **Seleziona progetti** finestra di dialogo, entrambi i progetti sono selezionati. Per disinstallare il pacchetto in entrambi i progetti, entrambe le caselle di controllo, quindi fare clic su **OK**.

Nella finestra di dialogo che chiede se si desidera disinstallare anche i pacchetti dipendenti, fare clic su No. Uno di questi è il pacchetto di Entity Framework che è necessario conservare.

Seguire la stessa procedura per disinstallare il **SqlServerCompact** pacchetto. (Necessario disinstallare i pacchetti in quest'ordine perché la **EntityFramework.SqlServerCompact** dipende dal pacchetto i **SqlServerCompact** package.)

A questo punto è stata la migrazione a SQL Server Express e completa di SQL Server. Nella prossima esercitazione si apporteranno un'altra modifica del database e si verrà illustrato come distribuire le modifiche del database se i database di test e produzione usano SQL Server Express e completa di SQL Server.

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
