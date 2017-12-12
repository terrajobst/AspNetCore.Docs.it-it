---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Distribuzione di un''applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migrazione a SQL Server - 10 12 | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: 31d83a11488212ab0ff83494d5e896ffcbeaa8a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migrazione a SQL Server - 10 12
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come eseguire la migrazione da SQL Server Compact a SQL Server. Uno dei motivi che è consigliabile eseguire tale operazione è possa sfruttare i vantaggi delle funzionalità di SQL Server che SQL Server Compact non supporta, ad esempio stored procedure, trigger, viste o replica. Per ulteriori informazioni sulle differenze tra SQL Server Compact e SQL Server, vedere il [la distribuzione di SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) esercitazione.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express e completa di SQL Server per lo sviluppo

Una volta che si è deciso di eseguire l'aggiornamento a SQL Server, potrebbe voler usare SQL Server o SQL Server Express negli ambienti di sviluppo e test. Oltre alle differenze nel supporto di strumenti e funzionalità del motore di database, esistono differenze nelle implementazioni del provider SQL Server Compact e altre versioni di SQL Server. Queste differenze possono causare lo stesso codice generare risultati diversi. Pertanto, se si decide di mantenere come database di sviluppo di SQL Server Compact, è necessario testarne il sito in SQL Server o SQL Server Express in un ambiente di test prima di ogni distribuzione nell'ambiente di produzione.

A differenza di SQL Server Compact, SQL Server Express è essenzialmente lo stesso motore di database e utilizza lo stesso provider .NET come completa di SQL Server. Quando si testano con SQL Server Express, è possibile essere certi di ottenere gli stessi risultati, come viene visualizzata con SQL Server. È possibile utilizzare la maggior parte degli strumenti database con SQL Server Express che è possibile utilizzare con SQL Server (un'eccezione non trascurabile da [SQL Server Profiler](https://msdn.microsoft.com/en-us/library/ms181091.aspx)), e supporta altre funzionalità di SQL Server quali stored procedure, viste, trigger, e la replica. (In genere è necessario utilizzare completa di SQL Server in un sito Web di produzione, tuttavia. SQL Server Express è possibile eseguire in un ambiente di hosting condiviso, ma non è stato progettato per tale e non è supportato da molti provider di hosting.)

Se si utilizza Visual Studio 2012, in genere si scegliere SQL Server Express LocalDB per l'ambiente di sviluppo perché questo è ciò che viene installato per impostazione predefinita con Visual Studio. Tuttavia, database locale non funziona in IIS, per l'ambiente di test è necessario utilizzare SQL Server o SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Combinare i database e mantenendoli separato

L'applicazione Contoso University è disponibili due database di SQL Server Compact: il database delle appartenenze (*aspnet.sdf*) e il database dell'applicazione (*School.sdf*). Quando esegue la migrazione, è possibile migrare i database per due database separati o a un singolo database. È possibile combinarle per facilitare il join del database tra il database dell'applicazione e il database di appartenenza. Il piano di hosting può anche fornire un motivo per combinarle. Ad esempio, il provider di hosting potrebbe addebitare i costi ulteriori per più database oppure potrebbe non consentire anche di più di un database. Che avviene con l'account utilizzato per questa esercitazione, che consente solo un singolo database di SQL Server di hosting Cytanium Lite.

In questa esercitazione, si saranno eseguire la migrazione dei due database in questo modo:

- Eseguire la migrazione a due database LocalDB nell'ambiente di sviluppo.
- Eseguire la migrazione a due database di SQL Server Express nell'ambiente di test.
- Eseguire la migrazione a un database SQL Server completo combinato nell'ambiente di produzione.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>L'installazione di SQL Server Express

SQL Server Express viene installato automaticamente con Visual Studio 2010 per impostazione predefinita, ma per impostazione predefinita non è installato con Visual Studio 2012. Per installare SQL Server 2012 Express, fare clic sul collegamento seguente

- [SQL Server Express 2012](https://www.microsoft.com/en-us/download/details.aspx?id=29062)

Scegliere *x64/ita/SQLEXPR\_x64\_ENU.exe* o *ita/x86/SQLEXPR\_x86\_ENU.exe*e nell'installazione guidata di accettare il valore predefinito Impostazioni. Per ulteriori informazioni sulle opzioni di installazione, vedere [installare SQL Server 2012 dall'installazione guidata (programma di installazione)](https://msdn.microsoft.com/en-us/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Creazione di database SQL Server Express per l'ambiente di Test

Il passaggio successivo consiste nel creare l'appartenenza ASP.NET e i database School.

Dal **vista** menu selezionare **Esplora Server** (**Esplora Database** in Visual Web Developer) e quindi fare doppio clic su **connessioni dati**e selezionare **Crea nuovo Database di SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

Nel **Crea nuovo Database di SQL Server** finestra di dialogo immettere ". \SQLExpress" nel **nome Server** casella e "aspnet-Test" nel **nome nuovo database** casella, quindi fare clic su **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Seguire la stessa procedura per creare un nuovo database di SQL Server Express School denominato "Test dell'istituto di istruzione".

(Che si sta accodando "Test" i nomi di database perché è necessario essere in grado di distinguere i due set di database e in seguito si creerà un'altra istanza di ogni database per l'ambiente di sviluppo.)

**Esplora server** verranno visualizzati due nuovi database.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Creazione di uno Script di concessione per i nuovi database

Quando l'applicazione viene eseguita in IIS nel computer di sviluppo, l'applicazione accede al database utilizzando le credenziali del pool di applicazioni predefinito. Tuttavia, per impostazione predefinita, l'identità del pool di applicazioni non dispone dell'autorizzazione per aprire i database. Pertanto, è necessario eseguire uno script per concedere tale autorizzazione. In questa sezione creare lo script che verrà eseguito successivamente per assicurarsi che l'applicazione è possibile aprire i database quando viene eseguito in IIS.

La soluzione *SolutionFiles* cartella creata nel [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione, creare un nuovo file SQL denominato *Grant.sql*. Copiare i seguenti comandi SQL nel file, quindi salvare e chiudere il file:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Questo script è progettato per funzionare con SQL Server 2008 e con le impostazioni di IIS in Windows 7, secondo quanto specificato in questa esercitazione. Se si utilizza una versione diversa di SQL Server o di Windows o configuri IIS nel computer in modo diverso, le modifiche apportate a questo script potrebbero essere necessarie. Per ulteriori informazioni sugli script di SQL Server, vedere [la documentazione Online di SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Nota sulla sicurezza** questo script fornisce db\_le autorizzazioni del proprietario per l'utente che accede al database in fase di esecuzione, ovvero è necessario nell'ambiente di produzione. In alcuni scenari è possibile specificare un utente che contiene lo schema completo del database, aggiornare le autorizzazioni solo per la distribuzione e specificare in fase di esecuzione di un altro utente che disponga delle autorizzazioni solo per leggere e scrivere dati. Per ulteriori informazioni, vedere **esaminare le modifiche automatiche di Web. config per le migrazioni Code First** in [distribuzione in IIS come ambiente di Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Configurazione di distribuzione del Database per l'ambiente di Test

Successivamente, si configurerà Visual Studio in modo che eseguiranno le attività seguenti per ogni database:

- Generare uno script SQL che crea una struttura del database di origine (tabelle, colonne, vincoli e così via) nel database di destinazione.
- Generare uno script SQL che inserisce i dati del database di origine in tabelle nel database di destinazione.
- Eseguire gli script generati e lo script di concessione che è stato creato, nel database di destinazione.

Aprire il **le proprietà del progetto** window e selezionare il **pubblicazione/creazione pacchetto SQL** scheda.

Assicurarsi che **attiva (rilascio)** o **versione** è selezionata nel **configurazione** elenco a discesa.

Fare clic su **abilitare questa pagina**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

Il **pubblicazione/creazione pacchetto SQL** scheda è disabilitata in genere perché specifica un metodo di distribuzione legacy. Per la maggior parte degli scenari, è necessario configurare la distribuzione di database nel **pubblica sul Web** procedura guidata. Migrazione da SQL Server Compact a SQL Server o SQL Server Express è un caso speciale per cui questo metodo è una scelta ottimale.

Fare clic su **Importa da Web. config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio cerca le stringhe di connessione nel *Web. config* trova uno per il database delle appartenenze e uno per il database School, file e aggiunge una riga corrispondente a ogni stringa di connessione nel **voci di Database**  tabella. Trova le stringhe di connessione sono per i database di SQL Server Compact esistenti e il passaggio successivo, è necessario configurare come e dove distribuire questi database.

Immettere impostazioni di distribuzione di database nel **dettagli voci Database** sezione riportata di seguito il **voci di Database** tabella. Le impostazioni visualizzate nella **dettagli voci Database** riguardano a seconda del valore di riga nella sezione di **voci di Database** tabella è selezionata, come illustrato nella figura seguente.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configurazione delle impostazioni di distribuzione per il Database delle appartenenze

Selezionare il **DefaultConnection distribuzione** riga il **voci di Database** tabella per configurare le impostazioni che si applicano al database delle appartenenze.

In **stringa di connessione per il database di destinazione**, immettere una stringa di connessione che punti al nuovo database di appartenenza di SQL Server Express. È possibile ottenere la stringa di connessione è necessario da **Esplora Server**. In **Esplora Server**, espandere **connessioni dati** e selezionare il **aspnetTest** database, quindi dal **proprietà** copia finestra il **Stringa di connessione** valore.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

La stringa di connessione viene riprodotto di seguito:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copiare e incollare la stringa di connessione in **stringa di connessione per il database di destinazione** nel **pubblicazione/creazione pacchetto SQL** scheda.

Assicurarsi che **Pull dei dati e/o schema da un database esistente** è selezionata. Questo è quello che determina gli script SQL essere automaticamente generato ed eseguito nel database di destinazione.

Il **stringa di connessione per il database di origine** valore viene estratto dal *Web. config* punti al database di SQL Server Compact di sviluppo e file. Questo è il database di origine che verrà utilizzato per generare gli script che verranno eseguito in un secondo momento nel database di destinazione. Poiché si desidera distribuire la versione di produzione del database, modificare "aspnet Dev.sdf" in "aspnet Prod.sdf".

Modifica **opzioni di scripting del Database** da **solo Schema** a **Schema e dati**, dal momento che si desidera copiare i dati (account utente e ruoli) nonché la struttura del database.

Per configurare la distribuzione per eseguire gli script di concessione creato in precedenza, è necessario aggiungerli al **degli script di Database** sezione. Fare clic su **Aggiungi Script**e il **aggiungere script SQL** finestra di dialogo casella, passare alla cartella in cui è archiviato lo script di concessione (questa è la cartella che contiene il file di soluzione). Selezionare il file denominato *Grant.sql*, fare clic su **aprire**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Le impostazioni per il **DefaultConnection distribuzione** riga **voci di Database** ora essere simile alla figura seguente:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configurazione delle impostazioni di distribuzione per il Database School

Selezionare quindi il **SchoolContext distribuzione** riga il **voci di Database** tabella per configurare le impostazioni di distribuzione per il database School.

È possibile utilizzare lo stesso metodo usato in precedenza per ottenere la stringa di connessione per il nuovo database di SQL Server Express. Copiare la stringa di connessione in **stringa di connessione per il database di destinazione** nel **pubblicazione/creazione pacchetto SQL** scheda.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Assicurarsi che **Pull dei dati e/o schema da un database esistente** è selezionata.

Il **stringa di connessione per il database di origine** valore viene estratto dal *Web. config* punti al database di SQL Server Compact di sviluppo e file. Modificare "Dell'istituto di istruzione Dev.sdf" in "Dell'istituto di istruzione-Prod.sdf" per distribuire la versione di produzione del database. (Il file School Prod.sdf è mai creato nell'App\_cartella dati, sarà copiare tale file dall'ambiente di testing all'App\_cartella di dati nella cartella del progetto di ContosoUniversity in un secondo momento.)

Modifica **opzioni di scripting del Database** a **Schema e dati**.

Si desidera eseguire lo script per concedono autorizzazioni di lettura e l'autorizzazione di scrittura per il database all'identità del pool di applicazioni, aggiungere il *Grant.sql* anche per il database delle appartenenze, file di script.

Al termine, le impostazioni il **SchoolContext distribuzione** riga **voci di Database** simile al seguente:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Salvare le modifiche per il **pubblicazione/creazione pacchetto SQL** scheda.

Copia il *dell'istituto di istruzione-Prod.sdf* file dal *c:\inetpub\wwwroot\ContosoUniversity\App\_dati* cartella in cui il *App\_dati* cartella il progetto ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Specificare la modalità di transazione per lo Script di concessione

Il processo di distribuzione che genera l'errore di script che distribuisce lo schema di database e i dati. Per impostazione predefinita, questi script vengono eseguiti in una transazione. Tuttavia, gli script personalizzati (ad esempio gli script di concessione) per impostazione predefinita non vengono eseguiti in una transazione. Se il processo di distribuzione sono presenti le modalità di transazione, è possibile che venga visualizzato un errore di timeout quando gli script eseguiti durante la distribuzione. In questa sezione, si modifica il file di progetto per configurare gli script personalizzati per l'esecuzione in una transazione.

In **Esplora**, fare doppio clic su di **ContosoUniversity** del progetto e selezionare **Scarica progetto**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Quindi fare di nuovo il progetto e selezionare **ContosoUniversity.csproj modifica**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Editor di Visual Studio viene illustrato il contenuto XML del file di progetto. Si noti che esistono diversi `PropertyGroup` elementi. (Nella figura, il contenuto del `PropertyGroup` elementi sono stati omessi.)

![Finestra editor del file di progetto](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Il primo, che non ha alcun `Condition` attributo, per le impostazioni che si applicano indipendentemente dalla configurazione di compilazione. Un `PropertyGroup` elemento si applica solo alla configurazione di compilazione di Debug (si noti il `Condition` attributo), si applica solo alla configurazione di compilazione di rilascio e si applica solo alla configurazione di compilazione di Test. All'interno di `PropertyGroup` elemento per la configurazione della build di rilascio, si noterà un `PublishDatabaseSettings` elemento che contiene le impostazioni immesse nella **pubblicazione/creazione pacchetto SQL** scheda. È presente un `Object` elemento che corrisponde a ogni script grant è (notare le due istanze specificate dell'oggetto "Grant.sql"). Per impostazione predefinita, il `Transacted` attributo del `Source` elemento per ogni script di concessione `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Modificare il valore della `Transacted` attributo del `Source` elemento `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Salvare e chiudere il file di progetto e quindi fare clic sul progetto in **Esplora** e selezionare **Ricarica progetto**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Configurazione di trasformazioni di Web. config per le stringhe di connessione

La connessione di stringhe per il nuovo SQL Express database immesso **pubblicazione/creazione pacchetto SQL** scheda vengono utilizzati da distribuzione Web solo per l'aggiornamento del database di destinazione durante la distribuzione. È necessario impostare *Web. config* trasformazioni in modo che le stringhe di connessione distribuito *Web. config* file punto con i nuovi database di SQL Server Express. (Quando si utilizza il **pubblicazione/creazione pacchetto SQL** scheda, è possibile configurare le stringhe di connessione nel profilo di pubblicazione.)

Aprire *Web.Test.config* e sostituire il `connectionStrings` elemento con la `connectionStrings` elemento nell'esempio seguente. (Assicurarsi di copiare solo l'elemento connectionStrings, non il codice circostante riportata di seguito per fornire contesto).

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Questo codice causa il `connectionString` e `providerName` gli attributi di ogni `add` elemento da sostituire in distribuito *Web. config* file. Queste stringhe di connessione non sono identiche a quelli immessi nella **pubblicazione/creazione pacchetto SQL** scheda. L'impostazione "MultipleActiveResultSets = True" è stato aggiunto ad essi perché è necessaria per Entity Framework e i provider Universal.

## <a name="installing-sql-server-compact"></a>L'installazione di SQL Server Compact

Il pacchetto SqlServerCompact NuGet fornisce database di SQL Server Compact assembly motore per l'applicazione Contoso University. Ma ora non è l'applicazione ma che deve essere in grado di leggere i database di SQL Server Compact, per creare script da eseguire nel database di SQL Server di distribuzione Web. Per abilitare distribuzione Web per leggere i database di SQL Server Compact, installare SQL Server Compact nel computer di sviluppo utilizzando il collegamento seguente: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>La distribuzione nell'ambiente di testing

Per pubblicare l'ambiente di Test, è necessario creare un profilo di pubblicazione è configurato per utilizzare il **pubblicazione/creazione pacchetto SQL** scheda per il database di pubblicazione anziché le impostazioni del database di profilo di pubblicazione.

Eliminare prima il profilo di Test esistente.

In **Esplora**, fare clic sul progetto ContosoUniversity e fare clic su **pubblica**.

Selezionare il **profilo** scheda.

Fare clic su **gestire i profili**.

Selezionare **Test**, fare clic su **rimuovere**, quindi fare clic su **Chiudi**.

Chiudi il **pubblica sul Web** procedura guidata per salvare questa modifica.

Successivamente, creare un nuovo profilo di Test e utilizzarla per pubblicare il progetto.

In **Esplora**, fare clic sul progetto ContosoUniversity e fare clic su **pubblica**.

Selezionare il **profilo** scheda.

Selezionare  **&lt;New... &gt;**  dall'elenco a discesa, elenco e immettere "Test" come nome del profilo.

Nel **URL del servizio** immettere *localhost*.

Nel **sito/applicazione** immettere *Default Web Site/ContosoUniversity*.

Nel **URL di destinazione** immettere `http://localhost/ContosoUniversity/`.

Scegliere **Avanti**.

Il **impostazioni** scheda avvisa che il **pubblicazione/creazione pacchetto SQL** scheda è stata configurata e offre la possibilità di eseguirne l'override scegliendo Abilita il nuovo database di pubblicazione miglioramenti. Per la distribuzione non si desidera eseguire l'override di **pubblicazione/creazione pacchetto SQL** impostazioni della scheda, quindi fare clic su **Avanti**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Un messaggio sul **anteprima** scheda indica che **Nessun database selezionato per la pubblicazione**, ma ciò significa solo che la pubblicazione di database non è configurata nel profilo di pubblicazione.

Fare clic su **Pubblica**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio distribuisce l'applicazione e apre il browser alla home page del sito nell'ambiente di test. Eseguire la pagina istruttori per vedere che consente di visualizzare gli stessi dati illustrato in precedenza. Eseguire il **aggiungere studenti** pagina, aggiungere un nuovo studente e quindi visualizzare il nuovo studente nel **studenti** pagina. Verifica che è possibile aggiornare il database. Selezionare il **aggiornamento crediti** pagina (è necessario effettuare l'accesso) per verificare che il database di appartenenza è stato distribuito e si ha accesso a esso.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Creazione di un Database SQL Server per l'ambiente di produzione

Ora che è stato distribuito l'ambiente di test, è pronti per configurare la distribuzione nell'ambiente di produzione. Come per l'ambiente di test, tramite la creazione di un database da distribuire per iniziare. Come si ricorda la panoramica, il piano di hosting Cytanium Lite consente solo un singolo database di SQL Server, pertanto sarà necessario impostare un solo database, non due. Tutte le tabelle e i dati dal database di SQL Server Compact dell'istituto di istruzione e appartenenza verranno distribuite in un database di SQL Server nell'ambiente di produzione.

Passare al pannello di controllo Cytanium in [http://panel.cytanium.com](http://panel.cytanium.com). Posiziona il puntatore del mouse su **database** e quindi fare clic su **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

Nel **SQL Server 2008** pagina, fare clic su **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Il database "Dell'istituto di istruzione" e fare clic su **salvare**. (La pagina aggiunge automaticamente il prefisso "contosou", in modo che il nome effettivo sia "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Nella stessa pagina, fare clic su **Create User**. Nei server del Cytanium, piuttosto che utilizza la sicurezza integrata di Windows e consentendo l'identità del pool di applicazioni di aprire il database, si creerà un utente che dispone di autorizzazioni sufficienti per aprire il database. Aggiungere le credenziali dell'utente per le stringhe di connessione che rientrano nella produzione *Web. config* file. In questo passaggio è creare tali credenziali.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Compilare i campi obbligatori nel **proprietà utente SQL** pagina:

- Immettere "ContosoUniversityUser" come nome.
- Immettere una password.
- Selezionare **contosouSchool** come database predefinito.
- Selezionare il **contosouSchool** casella di controllo.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Configurazione della distribuzione di Database per l'ambiente di produzione

Ora si è pronti per configurare le impostazioni di distribuzione di database nel **pubblicazione/creazione pacchetto SQL** scheda come fatto in precedenza per l'ambiente di test.

Aprire il **le proprietà del progetto** finestra, seleziona il **pubblicazione/creazione pacchetto SQL** scheda e verificare che **attiva (rilascio)** o **versione** è selezionato nel **configurazione** elenco a discesa.

Quando si configurano le impostazioni di distribuzione per ogni database, la differenza principale tra le operazioni per gli ambienti di produzione e di test è in modalità di configurazione delle stringhe di connessione. Per l'ambiente di test è stato immesso le stringhe di connessione di database di destinazione diverso, ma per l'ambiente di produzione la stringa di connessione di destinazione sarà lo stesso per entrambi i database. Questo avviene perché si siano distribuendo entrambi i database in un database di produzione.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configurazione delle impostazioni di distribuzione per il Database delle appartenenze

Per configurare le impostazioni relative al database di appartenenza, selezionare il **DefaultConnection distribuzione** riga il **voci di Database** tabella.

In **stringa di connessione per il database di destinazione**, immettere una stringa di connessione che punti al nuovo database SQL Server di produzione appena creato. È possibile ottenere la stringa di connessione dal messaggio di benvenuto. La parte pertinente del messaggio di posta elettronica contiene la stringa di connessione di esempio seguente:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Dopo la sostituzione di tre variabili, la stringa di connessione che è necessario è simile al seguente:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copiare e incollare la stringa di connessione in **stringa di connessione per il database di destinazione** nel **pubblicazione/creazione pacchetto SQL** scheda.

Assicurarsi che **Pull dei dati e/o schema da un database esistente** è ancora selezionato e che **opzioni di scripting del Database** è ancora **Schema e dati**.

Nel **degli script di Database** deselezionare la casella di controllo accanto a script Grant.sql.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configurazione delle impostazioni di distribuzione per il Database School

Selezionare quindi il **SchoolContext distribuzione** riga il **voci di Database** tabella per configurare le impostazioni del database School.

Copiare la stessa stringa di connessione in **stringa di connessione per il database di destinazione** copiato in tale campo per il database delle appartenenze.

Assicurarsi che **Pull dei dati e/o schema da un database esistente** è ancora selezionato e che **opzioni di scripting del Database** è ancora **Schema e dati**.

Nel **degli script di Database** deselezionare la casella di controllo accanto a script Grant.sql.

Salvare le modifiche per il **pubblicazione/creazione pacchetto SQL** scheda.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Trasforma impostazione Web. config per le stringhe di connessione al database di produzione

Successivamente, si imposterà *Web. config* trasformazioni in modo che le stringhe di connessione distribuito *Web. config* file in modo che punti al nuovo database di produzione. La stringa di connessione che è stato immesso il **pubblicazione/creazione pacchetto SQL** per distribuzione Web per utilizzare la scheda è uguale a quello dell'applicazione deve utilizzare, a eccezione dell'aggiunta del `MultipleResultSets` opzione.

Aprire *Web.Production.config* e sostituire il `connectionStrings` elemento con un `connectionStrings` elemento che è simile all'esempio seguente. (Solo copia il `connectionStrings` elemento, non i tag che vengono forniti per mostrare il contesto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Talvolta vedere suggerimenti che sono necessario crittografare sempre stringhe di connessione nel *Web. config* file. Questo potrebbe essere appropriato se sono stati distribuito in server nella rete dell'azienda. Quando si distribuisce in un ambiente di hosting condiviso, tuttavia, si sta trusting consigliate per la protezione del provider di hosting e non è pratico crittografare le stringhe di connessione o necessarie.

## <a name="deploying-to-the-production-environment"></a>La distribuzione nell'ambiente di produzione

Ora si è pronti per la distribuzione nell'ambiente di produzione. Distribuzione Web verrà letti i database di SQL Server Compact del progetto *App\_dati* cartella e ricreare tutte le tabelle e i dati nel database di SQL Server di produzione. Per eseguire la pubblicazione utilizzando il **pubblicazione/creazione pacchetto Web** le impostazioni di tabulazione, è necessario creare un nuovo profilo di pubblicazione per la produzione.

Eliminare prima il profilo di produzione esistente come fatto in precedenza il profilo di prova.

In **Esplora**, fare clic sul progetto ContosoUniversity e fare clic su **pubblica**.

Selezionare il **profilo** scheda.

Fare clic su **gestire i profili**.

Selezionare **produzione**, fare clic su **rimuovere**, quindi fare clic su **Chiudi**.

Chiudi il **pubblica sul Web** procedura guidata per salvare questa modifica.

Successivamente, creare un nuovo profilo di produzione e usarlo per pubblicare il progetto.

In **Esplora**, fare clic sul progetto ContosoUniversity e fare clic su **pubblica**.

Selezionare il **profilo** scheda.

Fare clic su **importazione**e selezionare il file con estensione publishsettings scaricato in precedenza.

Nel **connessione** scheda, modificare il **URL di destinazione** all'URL corretto temporaneo, che in questo esempio è http://contosouniversity.com.vserver01.cytanium.com.

Rinominare il profilo nell'ambiente di produzione. (Selezionare il **profilo** scheda e fare clic su **Gestione profili** a tale scopo).

Chiudi il **pubblica sul Web** procedura guidata per salvare le modifiche.

In un'applicazione reale, in cui è stato aggiornato il database in produzione, è necessario effettuare ora i due passaggi aggiuntivi prima di pubblicare:

1. Caricare *app\_offline.htm*, come illustrato nel [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione.
2. Utilizzare il **gestione File** funzionalità del Pannello di controllo Cytanium per copiare il *aspnet Prod.sdf* e *dell'istituto di istruzione-Prod.sdf* file dal sito di produzione per il *App\_dati* nella cartella del progetto ContosoUniversity. Ciò garantisce che i dati di cui che si distribuisce il nuovo database di SQL Server includano gli aggiornamenti più recenti apportati dal sito Web di produzione.

Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, verificare che il **produzione** profilo sia selezionata e quindi fare clic su **pubblica**.

Se è stata caricata *app\_offline.htm* prima della pubblicazione, è necessario utilizzare il **gestione File** utilità nel Pannello di controllo Cytanium eliminare *app\_offline.* htm prima di testare. È anche possibile contemporaneamente eliminare il *sdf* i file dal *App\_dati* cartella.

È ora possibile aprire un browser e passare all'URL del sito pubblico per testare l'applicazione allo stesso modo che è stato eseguito dopo la distribuzione nell'ambiente di testing.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Il passaggio a SQL Server Express LocalDB in fase di sviluppo

Come è stato illustrato nella panoramica, è in genere consigliabile utilizzare lo stesso motore di database in fase di sviluppo utilizzato in prova e di produzione. (Tenere presente che il vantaggio di utilizzare SQL Server Express in fase di sviluppo è che il database verrà eseguito nello stesso negli ambienti di sviluppo, test e produzione). In questa sezione verrà impostare il progetto ContosoUniversity per utilizzare SQL Server Express LocalDB quando si esegue l'applicazione da Visual Studio.

Il modo più semplice per eseguire la migrazione è in Code First e il sistema di appartenenze crea entrambi nuovi database di sviluppo per l'utente. Questo metodo per eseguire la migrazione richiede tre passaggi:

1. Modificare le stringhe di connessione per specificare i nuovi database di SQL Express LocalDB.
2. Eseguire lo strumento Amministrazione sito Web per creare un utente amministratore. Verrà creato il database delle appartenenze.
3. Utilizzare il comando update-database migrazioni Code First per creare e inizializzare il database dell'applicazione.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aggiornamento delle stringhe di connessione nel file Web. config

Aprire il *Web. config* file e sostituire il `connectionStrings` elemento con il codice seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Creazione del Database di appartenenza

In **Esplora**, selezionare il progetto ContosoUniversity e quindi fare clic su **configurazione ASP.NET** nel **progetto** menu.

Selezionare la scheda sicurezza.

Fare clic su **crea o Gestisci ruoli**e quindi creare un **amministratore** ruolo.

Tornare alla scheda sicurezza.

Fare clic su **Create user**, quindi selezionare il **amministratore** casella di controllo e creare un utente denominato amministratore.

Chiudi il **strumento Amministrazione sito Web**.

### <a name="creating-the-school-database"></a>Creazione del Database School

Aprire la finestra della Console di gestione pacchetti.

Nel **progetto predefinito** elenco a discesa selezionare il progetto ContosoUniversity.DAL.

Immettere il comando seguente:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migrazioni Code First applica la migrazione iniziale che crea il database e quindi applica la migrazione di AddBirthDate, quindi viene eseguito il metodo di inizializzazione.

Eseguire il sito premendo CTRL + F5. Come per gli ambienti di test e produzione, eseguire il **aggiungere studenti** pagina, aggiungere un nuovo studente e quindi visualizzare il nuovo studente nel **studenti** pagina. Verifica che il database School è stato creato e inizializzato e aver letto e accesso in scrittura a esso.

Selezionare il **aggiornamento crediti** pagina e di log in per verificare che il database di appartenenza è stato distribuito e di disporre dell'accesso a esso. Se non sono stati migrati gli account utente, creare un account amministratore e quindi selezionare il **aggiornamento crediti** pagina per verificarne il funzionamento.

## <a name="cleaning-up-sql-server-compact-files"></a>Pulizia dei file SQL Server Compact

Non è più necessario file e i pacchetti NuGet a cui sono stati inclusi per il supporto di SQL Server Compact. Se si desidera (questo passaggio non è obbligatorio), è possibile pulire i file non necessari e i riferimenti.

In **Esplora**, eliminare il *sdf* i file dal *App\_dati* cartella e *amd64* e *x86* cartelle dal *bin* cartella.

In **Esplora**destro la soluzione (non uno dei progetti) e quindi fare clic su **Gestisci pacchetti NuGet per la soluzione**.

Nel riquadro sinistro della finestra di **Gestisci pacchetti NuGet** nella finestra di dialogo **i pacchetti installati**.

Selezionare il **EntityFramework.SqlServerCompact** pacchetto e fare clic su **Gestisci**.

Nel **selezionare progetti** la finestra di dialogo, entrambi i progetti selezionati. Per disinstallare il pacchetto in entrambi i progetti, entrambe le caselle di controllo, quindi fare clic su **OK**.

Nella finestra di dialogo che chiede se si desidera disinstallare anche i pacchetti di dipendenti, fare clic su No. Uno di questi è il pacchetto di Entity Framework che è necessario mantenere.

Seguire la stessa procedura per disinstallare il **SqlServerCompact** pacchetto. (Necessario disinstallare i pacchetti in quest'ordine perché la **EntityFramework.SqlServerCompact** dipende dal pacchetto di **SqlServerCompact** pacchetto.)

Avere ora correttamente la migrazione a SQL Server Express e completa di SQL Server. Nella prossima esercitazione si renderanno un'altra modifica del database e si noterà come distribuire le modifiche del database quando i database di test e produzione utilizzano SQL Server Express e completa di SQL Server.

>[!div class="step-by-step"]
[Precedente](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[Successivo](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
