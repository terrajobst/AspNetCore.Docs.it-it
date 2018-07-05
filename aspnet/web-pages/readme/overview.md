---
uid: web-pages/readme/overview
title: File Leggimi di WebMatrix | Microsoft Docs
author: rick-anderson
description: WebMatrix e ASP.NET Web Pages (Razor) versione 1.0 Leggimi
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 924bf04772a1d73c7fdfb1168090daef388438ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398800"
---
<a name="webmatrix-readme"></a>File Leggimi di WebMatrix
====================
13 gennaio 2011

## <a name="contents"></a>Sommario

> [!NOTE]
> Si applica questo file Leggimi per la 1.0 versione di WebMatrix.


- [Panoramica](#Overview)
- [Installazione](#Installation_Notes)
- [Come pubblicare applicazioni](#InstructionsForPublishingApplications)
- [Le modifiche e problemi](#ChangesAndIssues)

    - [Installazione di WebMatrix 1.0](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Installazione di applicazioni](#Known_Issues_Installing_Applications)
    - [Pubblicazione di applicazioni](#Known_Issues_Publishing_Applications)
- [Per altre informazioni](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Panoramica

> Microsoft WebMatrix 1.0 è uno stack di sviluppo web gratuito che consente di installare in pochi minuti. Si integra un server web con il database e Framework per creare un'esperienza unica e integrata di programmazione. È possibile usare WebMatrix per ottimizzare le attività di codice, testare e pubblicare il proprio sito Web ASP.NET o PHP, oppure è possibile usare WebMatrix per avviare un nuovo sito Web con più comuni App open source quali DotNetNuke, Umbraco, WordPress o Joomla. WebMatrix utilizza sullo stesso server web potente, motore di database e ambiente di Framework che verrà eseguito il sito Web su internet, che semplifica il passaggio dallo sviluppo alla produzione diretto e immediato.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installazione

> Per installare WebMatrix 1.0, è necessario installare innanzitutto le [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Dopo aver installato l'installazione guidata piattaforma Web, è possibile usarlo per installare WebMatrix.
> 
> Se si verificano problemi durante l'installazione, fare riferimento a [risoluzione dei problemi con Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Come pubblicare applicazioni

> Vedere [istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Le modifiche e problemi

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemi di installazione 1.0 WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix 1.0 è disponibile solo nelle piattaforme che supportano Microsoft .NET Framework 4

> .NET Framework versione 4 è obbligatorio per WebMatrix. In alcuni casi, il programma di installazione di WebMatrix 1.0 consente di provare a installare su una piattaforma che non fa parte del set di configurazione supportata. In particolare, Windows Vista senza l'aggiornamento SP1 consentirà di avviare l'installazione di WebMatrix, ma il componente di .NET Framework 4 avrà esito negativo e bloccare l'installazione.
> 
> **Soluzione alternativa**  
> Installare in una piattaforma supportata, che include:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 o versione successiva
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: Non è possibile installare WebMatrix 1.0 se Microsoft Visual Studio 2008 viene installato senza Microsoft Visual Studio 2008 SP1

> **Soluzione alternativa**  
> Installare [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) dall'area download Microsoft.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problema: Alcuni assembly per SQL Server Compact 4.0 non sono installati nella Global Assembly Cache

> Gli assembly gestiti per SQL Server Compact 4.0 non vengono inseriti in global assembly cache (GAC) quando si installa SQL Server Compact 4.0 in un computer a 64 bit e il computer ha solo .NET Framework 3.5 SP1 Client installato il profilo. Gli assembly gestiti che non sono installati nella Global Assembly Cache sono:
> 
> - *SqlServerCe. dll* (provider ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Soluzione alternativa**  
> Disinstallare SQL Server Compact 4.0. Scaricare e installare la versione completa di .NET Framework 3.5 SP1 dal percorso seguente:  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (pacchetto completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Reinstallare SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: Non è possibile disinstallare SQL Server Compact mediante la riga di comando

> La disinstallazione di SQL Server Compact mediante le opzioni della riga di comando non funziona in questa versione.
> 
> **Soluzione alternativa**  
> Uso *programmi e funzionalità* nel Pannello di controllo di Windows per disinstallare Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Pagine Web ASP.NET

In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e problemi noti con la 1.0 versione di ASP.NET Web Pages con sintassi Razor.

- [Nuove funzionalità](#NewFeatures)
- [Modifiche](#Changes)
- [Problemi](#Issues)

#### <a id="NewFeatures"></a>  Nuove funzionalità

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Novità: Impostazione di configurazione aggiunte per disabilitare la gestione dei pacchetti

> Una nuova `asp:AdminManagerEnabled` chiave è disponibile per il `<appSettings>` elemento le *Web. config* file, che consente di disabilitare completamente la gestione dei pacchetti. Il valore predefinito per questo elemento è impostato su true, il che significa che se non è incluso nel *Web. config* file, la gestione dei pacchetti è abilitato. Per disabilitare la gestione dei pacchetti, aggiungere il seguente elemento per il *Web. config* file nella radice del sito Web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Modifiche

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>: Modifica della chiave "webPages:AdminFolderVirtualPath" rinominato in "asp: AdminFolderVirtualPath"

> Il `webPages:AdminFolderVirtualPath` chiave che può essere aggiunti al *Web. config* ridenominazione di file per specificare il percorso di gestione pacchetti per usare i `asp:` dello spazio dei nomi anziché il `webPages` dello spazio dei nomi. Se si usa questo elemento, è necessario rinominarlo nel file di configurazione.


#### <a id="Issues"></a>  Problemi noti

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problema: Le password degli utenti di appartenenza non è più riconosciuti

> L'algoritmo per la creazione e l'archiviazione delle password di appartenenza (account di accesso) è stato modificato per essere più sicuro. Di conseguenza, le password archiviate per i membri (utenti) creati nelle versioni Beta di ASP.NET Razor non verrà riconosciute. 
> 
> **Soluzione alternativa** se il sito non è ancora stato inserito nell'ambiente di produzione, rimuovere i record utente dal database di appartenenza. Se il database si trova in tempo reale, a livello di programmazione rigenerare le password esistenti nel database delle appartenenze.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Un comportamento imprevisto quando si usa una tabella utente personalizzato per l'appartenenza

> Per inizializzare il provider di appartenenze per un sito Web ASP.NET Razor, si chiama il `WebSecurity.InitializeDatabaseConnection` (metodo). (In WebMatrix, il modello Starter Site include una chiamata al metodo nel  *\_AppStart.cshtml* file.) Se il `autoCreateTables` parametro di questo metodo è impostato su true (per impostazione predefinita, viene impostata su true nel modello Starter Site), e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera un errore. Al contrario, viene creato automaticamente nella tabella.
> 
> Può trattarsi di un problema, se si prevede di usare una tabella utente personalizzata per l'appartenenza, ma passare il nome della tabella non corretto per il `WebSecurity.InitializeDatabaseConnection` (metodo). Poiché il metodo non per impostazione predefinita genera un errore se la tabella specificata non esiste e perché crea invece una nuova tabella, è possibile l'applicazione sembra funzionare. Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui campi in essa) può avere esito negativo con errori imprevisti.
> 
> **Soluzione alternativa**  
> Assicurarsi che il nome passato il `InitializeDatabaseConnection` corrispondenze di metodo del profilo utente di tabella nel database di appartenenza o assicurarsi che il `autoCreateTables` parametro è impostato su false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Problema: Messaggio errore "il modulo di amministrazione richiede l'accesso a ~/App\_Data"

> In alcune circostanze, il tentativo di creare utenti o in caso contrario, utilizzare il sistema di appartenenze ASP.NET può causare la pagina visualizzare l'errore *il modulo di amministrazione richiede l'accesso a ~/App\_dati*. Ciò si verifica se l'account che sta utilizzando IIS o IIS Express non dispone di autorizzazioni per creare e scrivere il *App\_dati* cartella sotto la radice del sito Web. 
> 
> **Soluzione alternativa** creare manualmente un' *App\_dati* cartella per il sito Web. Assicurarsi che l'account di Windows che l'applicazione viene eseguita con (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio App\_dei dati. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [i problemi delle istanze utente di SQL Server Express e ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: errore "Non riuscita a generare un'istanza utente di SQL Server"

> Se un'applicazione Web di WebMatrix utilizza SQL Server Express ed è in esecuzione IIS 7.5 in Windows 7 o Windows Server 2008 R2, si potrebbe essere visualizzato un errore che indica che SQL Server non è possibile recuperare il percorso di applicazioni locali dell'utente in fase di esecuzione.
> 
> **Soluzione alternativa** assicurarsi che l'account di Windows che l'applicazione viene eseguita con (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *App\_Data*. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [i problemi delle istanze utente di SQL Server Express e ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problema: I file che contiene le risorse di gestione pacchetti o le password di gestione pacchetti sono utilizzabili in IIS 6.0 e versioni precedenti

> Se si distribuisce un'applicazione ASP.NET Web Pages (Razor) che è stata creata usando la versione RC2, e se l'applicazione contiene un *password.txt* oppure *packagesources.txt* file */App\_ Dati/admin*, IIS 6.0 servirà il file se richiesto, può esporre le password per l'istanza di gestione pacchetti. 
> 
> **Soluzione alternativa** rinominare il *password.txt* oppure *packagesources.txt* file *password.config* o *packagesources.config*. Per impostazione predefinita, IIS 6.0 non fornirà i file con il *config* estensione. (In IIS 7, nessun file nei *App\_dati* cartella vengono gestite, in modo che non è necessario rinominare i file.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problema: I pacchetti installati con la versione Beta 3 di disinstallazione non rimuovere completamente i componenti del pacchetto

> Se è installato un pacchetto utilizzando la gestione dei pacchetti in versione Beta 3 e quindi provare a disinstallare l'estensione Usa la versione corrente, il pacchetto non viene disinstallato completamente. Tramite la gestione di pacchetti **disinstallazione** pulsante consente di rimuovere alcuni componenti, ma lascia il codice di libreria del pacchetto e non aggiorna il *package* file.
> 
> **Soluzione alternativa**   
> Seguire questi passaggi:  
> 1. Eliminare il *App\_Data\packages* cartella. Questa operazione rimuove tutti i pacchetti.   
> 2. Eliminare il *Packages. config* file nella radice del sito Web.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problema: In Visual Studio, richiamare la gestione dei pacchetti basata sul web viene portata offline l'applicazione

> Se si lavora in Visual Studio (non WebMatrix) e usare la  *\_admin* funzionalità per la gestione di pacchetti, Visual Studio di avviare l'applicazione viene portata offline e registra il *app\_ offline.htm* nella radice del sito, viene ostacolata la possibilità di usare Gestione pacchetti.
> 
> [!NOTE]
> Sebbene questo comportamento si vedrebbe principalmente quando si usa l'interfaccia di gestione pacchetti basato su web, lo stesso comportamento si verifica se si aggiungere, rimuovere o modificare tutti i file nei *App\_dati* cartella.
> 
> **Soluzione alternativa**   
> Per utilizzare i pacchetti in Visual Studio, usare l'estensione NuGet anziché la gestione pacchetti basato su web. Per informazioni, vedere la [documentazione di NuGet](https://docs.microsoft.com/nuget/). Se si lavora con gli altri file nei *App\_dati* cartella, si consiglia di mantenere i file in un' posizione per evitare questo problema. Se non è pratico, eliminare il *app\_offline.htm* file manualmente o attendere fino a quando il sito verrà ripristinato automaticamente (per impostazione predefinita dopo 30 secondi).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense e modelli di progetto disponibili solo in ASP.NET MVC versione 3

> Installazione di ASP.NET Web Pages anche installare gli strumenti per Visual Studio, ad esempio i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages.
> 
> **Soluzione alternativa** per usare i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o il [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: I feed di lettura o altri dati esterni tramite un server proxy

> Se il server che esegue il sito si trova dietro un server proxy, si potrebbe essere necessario configurare le informazioni sul proxy nel *Web. config* file per poter essere in grado di leggere le informazioni provenienti dall'esterno del sito. Ad esempio, se si usa il `ReCaptcha` helper, l'helper comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato dal server proxy. Feed utilizzati nelle pagine Web ASP.NET, ad esempio il feed usato da Gestione pacchetti in modo analogo, potrebbero richiedere la configurazione del proxy.
> 
> Se si verificano problemi nell'uso di un servizio esterno o l'utilizzo di feed di pacchetti, inserire gli elementi seguenti nella radice dell'applicazione *Web. config* file:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Per altre informazioni sulla configurazione di un server proxy, vedere [ &lt;proxy&gt; (impostazioni di rete)](https://msdn.microsoft.com/library/sa91de1e.aspx) sul sito Web MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Disinstallazione di .NET Framework versione 4 Disabilita ASP.NET Web Pages con sintassi Razor

> Se si disinstalla .NET Framework versione 4 e quindi reinstallarlo, ASP.NET Web Pages con sintassi Razor è disabilitato. Le pagine con le *cshtml* estensioni non vengono eseguite correttamente. ASP.NET Web Pages registra un assembly nella directory radice macchina *Web. config* file e rimozione di .NET Framework consente di rimuovere tale file. Reinstallare .NET Framework viene installata una versione nuova del file di configurazione, ma non aggiunge il riferimento per l'assembly di ASP.NET Web Pages.
> 
> **Soluzione alternativa** dopo la reinstallazione di .NET Framework, reinstallare ASP.NET Web Pages con sintassi Razor. Verrà aggiunto l'elemento seguente per il *Web. config* file nella radice del computer, ovvero in genere nel percorso seguente:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: Gli URL senza estensioni non si trova il file.cshtml/.vbhtml in IIS 7 o IIS 7.5

> In IIS 7 o IIS 7.5, le richieste con un URL analogo al seguente non sono in grado di trovare le pagine che hanno le *. cshtml* oppure *vbhtml* estensione:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Il problema si verifica perché la riscrittura degli URL non è abilitato per impostazione predefinita per IIS 7 o IIS 7.5. Lo scenario probabile che è che il problema non viene visualizzato durante il test in locale tramite IIS Express, ma si verificano quando si distribuisce il sito Web in un sito Web di hosting.
> 
> **Soluzione alternativa**
> 
> - Se si dispone di controllo sul computer del server, nel computer del server, installare l'aggiornamento descritto nel [è disponibile che consente di determinati gestori di IIS 7.0 o IIS 7.5 di gestire le richieste il cui URL non terminano con un periodo di un aggiornamento](https://support.microsoft.com/kb/980368).
> - Se non è un controllo sul computer server (ad esempio, si distribuisce in un sito Web di hosting), aggiungere il codice seguente nel sito Web *Web. config* file: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Distribuire un'applicazione in un computer che non dispone di SQL Server Compact è installato

> Le applicazioni che includono i database di SQL Server Compact possono eseguire in un computer in cui SQL Server Compact non è installato. Microsoft WebMatrix 1.0 automaticamente consente di copiare tali file binari per l'utente ed esegue l'appropriato *Web. config* trasformazioni di file.
> 
> **Soluzione alternativa** se è necessario copiare tali file e apportare le *Web. config* modifiche al file manualmente, eseguire le operazioni seguenti:
> 
> 1. Copiare gli assembly del motore di database per il *Bin* cartella (e sottocartelle) dell'applicazione nel computer di destinazione:  
> 
>    - Copia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>alla</em></strong>\Bin\x86*
>    - Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>alla</strong><em>\Bin\amd64</em>
> 
> 2. Nella cartella radice del sito Web, creare o aprire una *Web. config* file. (In WebMatrix 1.0, questo tipo di file è disponibile se si fa clic **tutte** nel **scegliere un tipo di File** nella finestra di dialogo.)
> 3. Aggiungere l'elemento seguente come elemento figlio di `<configuration>` elemento (non all'interno di `<system.web>` elemento):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: "Database" e "WebGrid" helper non funzionano in Medium Trust in Visual Basic

> Se si usa Visual Basic (creando *vbhtml* file), il `Database` e `WebGrid` helper non funzionerà se l'applicazione è impostata per usare Medium Trust.
> 
> **Soluzione alternativa**  
> Se si usa Visual Studio 2010, è possibile risolvere il problema installando la versione Service Pack 1. Fino a quando non è disponibile la versione finale della versione SP1, è possibile scaricare la versione Beta di SP1 dal [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) pagina nel Microsoft Download Center.   
>   
> Se questo non è pratico, o se non si utilizza Visual Studio 2010, è possibile temporaneamente impostare l'applicazione per usare l'attendibilità.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problema: "ApplicationPart" le risorse siano accessibili esternamente

> Se un assembly contiene gli oggetti da cui deriva il `ApplicationPart` che le risorse dell'assembly vengono esposte dalla classe di `ResourceRouteHandler` classe. Si consideri ad esempio l'URL seguente:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Questa richiesta di download di tutte le stringhe di risorse nel *System.Web.WebPages.Administration.dll* assembly. Tutte le risorse incorporate, anche quelli che non sono destinati a essere gestite come contenuto statico, vengono scaricate. Se le risorse incorporate contengono informazioni riservate, ciò può rappresentare un rischio per la sicurezza. 
> 
> **Soluzione alternativa**   
> Se si crea un' **ApplicationPart** dell'oggetto, assicurarsi che le risorse incorporate associati **ApplicationPart** assembly dell'oggetto non contengono informazioni riservate.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Per informazioni sui problemi di installazione per WebMatrix, vedere [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.


In questa sezione del documento vengono descritti problemi noti per l'ambiente di sviluppo WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problema: Il nome utente o password di una stringa di connessione di database in un file Web. config non vengono riflesse nell'area di lavoro di database

> **Soluzione alternativa**  
> 
> 1. Nel *Web. config* file, modificare il nome del database nella stringa di connessione (ad esempio, aggiungere "1" a esso).
> 2. Salvare il *Web. config* file.
> 3. Fare clic su **database** e aggiornare.
> 4. Modificare il nome del database nella stringa di connessione nel *Web. config* file il nome del database originale.
> 5. Salvare il *Web. config* file.
> 6. Fare clic su **database** e aggiornare.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problema: Non è possibile eliminare le cartelle create da WebMatrix

> Se WebMatrix viene eseguito con autorizzazioni elevate (vale a dire, si inizia a utilizzare WebMatrix il **Esegui come amministratore** opzione in Windows), non è possibile eliminare le cartelle che vengono create da WebMatrix utilizza Windows Explorer.
> 
> **Soluzione alternativa**  
> Eseguire Esplora Windows con autorizzazioni elevate. Attenersi ai passaggi riportati di seguito.  
> 
> 1. In Windows, fare clic su **avviare**.
> 2. Immettere "Windows Explorer" e fare clic sulla voce relativa **Windows Explorer**.
> 3. Fare clic su **Esegui come amministratore**. È quindi possibile eliminare le cartelle.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix 1.0 non riesce a eseguire determinate attività che richiedono l'elevazione dei privilegi

> WebMatrix 1.0 non riesce a eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:
> 
> - In Windows Vista o Windows 7, si è connessi con un account che dispone di privilegi amministrativi e controllo Account utente (UAC) è disabilitato.
> - Si utilizza Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Soluzione alternativa**  
> La maggior parte delle attività in WebMatrix 1.0 non richiedono autorizzazioni amministrative. Per questi, è possibile eseguire l'operazione perché un amministratore o seguire questa procedura:
> 
> - In Windows Vista o Windows 7, abilitare la UAC.
> - In Windows XP, aggiungere l'utente al gruppo di sicurezza Administrators.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "Del sito da Web Gallery" è disabilitato

> Il **sito da Web Gallery** opzione è disabilitata se l'installazione guidata piattaforma Web 3.0 non è installato.
> 
> **Soluzione alternativa**  
> Installare il [installazione guidata piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: Google Chrome non è disponibile come opzione di esecuzione

> Google Chrome non viene visualizzata nell'elenco dei browser nel **eseguiti** nel **Home** scheda.
> 
> **Soluzione alternativa**  
> Alcune versioni di Google Chrome non si registrano correttamente con la funzionalità programmi predefiniti in Windows. Risolvere il problema, avviare Google Chrome, scegliere il *personalizzare e controllo Google Chrome* menu, fare clic su *opzioni*e quindi fare clic su *marca Google Chrome il browser predefinito installato*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: La finestra di dialogo "Foreign Key" non consente l'immissione di una chiave primaria

> Il **Foreign Key** nella finestra di dialogo non consente di immettere il nome della chiave primario della tabella di chiave primaria.
> 
> **Soluzione alternativa**  
> Ciò è intenzionale. Non devi immettere il nome della chiave primaria della tabella della chiave primaria.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problema: IntelliSense non è disponibile in WebMatrix per Razor sintassi, c# o Visual Basic

> IntelliSense è supportata in WebMatrix per HTML e CSS. Non è tuttavia disponibile per altri linguaggi. 
> 
> **Soluzione alternativa**   
> Nessuno.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problema: IntelliSense per HTML e CSS suggerisce gli elementi che non sono appropriati in base al contesto

> IntelliSense per il markup in WebMatrix supporta HTML usando il [schema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e l'utilizzo di CSS il [dello schema CSS 2.1](http://www.w3.org/TR/CSS2/). Poiché IntelliSense si basa su questi schemi specifici, alcuni tag, gli attributi o proprietà potrebbero proposto che non sono appropriate per la definizione di stile o di pagina corrente. Per HTML, può anche causare suggerimenti imprevisti nel contenuto che potrebbero essere interpretati come XHTML in formato non valido (ad esempio, quando i tag non vengono chiuse). Questo problema potrebbe essere più evidente se il punto di inserimento si trova all'interno di un tag incompleto; In tal caso, IntelliSense potrebbe suggerire nuovi tag di apertura o offrire altri suggerimenti non corretti. 
> 
> **Soluzione alternativa**   
> Per HTML, assicurarsi di lavorare all'interno di una pagina XHTML completata in formato corretto. Per CSS, non è disponibile alcuna soluzione.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problema: IntelliSense non viene richiamato durante la digitazione

> In alcuni casi, IntelliSense potrebbe non essere richiamato come HTML o CSS venga immesso nell'editor di. In particolare, questo può verificarsi quando il punto di inserimento è direttamente accanto a un altro elemento o alla fine di un file. 
> 
> **Soluzione alternativa**   
> Assicurarsi che vi sia uno spazio vuoto attorno al punto di inserimento e che il punto di inserimento non è alla fine di un file. È anche possibile richiamare IntelliSense manualmente premendo Ctrl + barra spaziatrice.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problema: Nessuna interfaccia utente è disponibile per la disabilitazione di IntelliSense

> WebMatrix 1.0 non fornisce alcuna interfaccia utente o il movimento per disabilitare IntelliSense. 
> 
> **Soluzione alternativa**   
> Iniziare a WebMatrix mediante il comando seguente, che include un'opzione che disabilita IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express ha un proprio file Leggimi, che è disponibile all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact ha un proprio file Leggimi, che è disponibile all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Per informazioni sui problemi che interessano l'installazione di SQL Server Compact come parte di WebMatrix, vedere [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.

### <a id="Known_Issues_Installing_Applications"></a>  Installazione di applicazioni

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: Installazione di un'applicazione può richiedere molto tempo se cartella documenti dell'utente viene reindirizzata a una condivisione di rete

> **Soluzione alternativa**  
> Nessuno. L'applicazione potrebbe richiedere del tempo per l'installazione, ma verrà installato correttamente.


### <a id="Known_Issues_Publishing_Applications"></a>  Pubblicazione di applicazioni

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problema: "richiesto non è possibile acquisire le autorizzazioni" Errore durante la pubblicazione di un Database di SQL Compact

> WebMatrix non supporta completamente la distribuzione di file binari di supporti per SQL Server Compact a un server che esegue .NET Framework versione 3.5 con una configurazione a livello di attendibilità medio.
> 
> **Soluzione alternativa**  
> La soluzione migliore consiste nell'installare .NET Framework 4 nel server. In alternativa, eseguire le operazioni seguenti:
> 
> 1. Aggiungere i seguenti elementi per il `SecurityClasses` nella sezione *Web\_MediumTrust.config* file:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Creare un nuovo set di autorizzazioni nel *Web\_MediumTrust.config* file con le autorizzazioni necessarie seguenti:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Applicare l'autorizzazione impostata su SQL Server Compact inserendo gli elementi seguenti nel *Web\_MediumTrust.config* file:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problema: Le applicazioni web della raccolta e PhpBB visualizzano un errore "Servizio non disponibile" dopo la pubblicazione

> In alcune circostanze, la pubblicazione di un'applicazione causa un errore "servizio non disponibile".
> 
> **Soluzione alternativa**  
> In WebMatrix, aggiungere una barra rovesciata (\) alla fine del nome del server nel **impostazioni di pubblicazione** finestra e quindi pubblicare nuovamente l'applicazione.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problema: I collegamenti e il layout di sito Web di Moodle vengono interrotti dopo la pubblicazione

> Dopo aver pubblicato un'applicazione di Moodle, l'applicazione non funziona correttamente.
> 
> **Soluzione alternativa**  
> In WebMatrix, aggiungere una barra (/) alla fine del **nome del sito** campo le **impostazioni di pubblicazione** finestra e quindi pubblicare nuovamente l'applicazione.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problema: La pubblicazione di nopCommerce ha esito negativo con un errore di database

> NopCommerce pubblicazione ha esito negativo e viene segnalato un errore di database, ad esempio "inserire il nop\_tabella del log non è riuscita."
> 
> **Soluzione alternativa**  
> 
> 1. In WebMatrix, fare clic su **eseguire** avviare nopCommerce in locale.
> 2. Accedere alla pagina Amministrazione.
> 3. Scegliere il **sistema** menu.
> 4. Scegliere il **Log** opzione.
> 5. Scegliere il **Cancella Log** pulsante.
> 6. Pubblicare di nuovo nopCommerce.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problema: Silverstripe CMS Visualizza un "errore HTTP 500 PHP FCGI" quando si scarica un sito pubblicato

> **Soluzione alternativa**  
> Dopo aver fatto clic **Download pubblicati site**, ignorare `silverstripe-cache/manifest_main` nelle **Publish Preview**. Questo file viene usato per la memorizzazione nella cache ed è specifico per ogni computer.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problema: Subtext Visualizza "Errore Server nell'applicazione '/'" quando si scarica un sito pubblicato

> **Soluzione alternativa**  
> Aprire il sito *Web. config* file e sostituire l'ID utente e password nella stringa di connessione del database con le credenziali di amministratore di SQL Server (le credenziali "sa").
> 
> In alternativa, seguire questi passaggi per concedere all'account utente si è connessi con `db_owner` autorizzazioni:
> 
> 1. Installare SQL Server Management Studio utilizzando l'installazione guidata piattaforma Web.
> 2. Connettersi all'istanza di SQL Server Express locale (per impostazione predefinita, `.\SQLEXPRESS`).
> 3. Fare clic su **database** &gt; *[localSubtextDatabase]* &gt; **sicurezza** &gt; **utenti** &gt; *[localSubtextUser*] (valore predefinito è `subtextuser`], pulsante destro del mouse e scegliere **proprietà**.
> 4. Selezionare **db\_proprietario** nella sezione l'appartenenza al ruolo.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: Sito potrebbe non funzionare dopo la pubblicazione se il campo "URL di destinazione" non è il prefisso http:// o https://

> Nel **impostazioni di pubblicazione** finestra di dialogo, se l'URL di destinazione non inizia con `http://` o `https://`, il sito potrebbe non funzionare dopo la distribuzione.
> 
> **Soluzione alternativa**  
> Assicurarsi che prima di pubblicare un sito, l'URL di destinazione nel **impostazioni di pubblicazione** finestra di dialogo inizia con `http://` o `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: La pubblicazione di un database MySQL non riesce con l'errore "non è stato possibile pubblicare il database. Questa situazione può verificarsi se il database remoto non è possibile eseguire lo script."

> L'errore può verificarsi per diversi motivi. Un motivo per cui che è possibile visualizzare questo errore è se lo script di database contiene un carattere di virgoletta singola (') e set di caratteri predefinito del database MySQL di destinazione non è presente in UTF-8.
> 
> **Soluzione alternativa**  
> Impostare il carattere predefinito impostato per il database MySQL remoto su UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problema: Alcuni collegamenti non sono visibili in DotNetNuke dopo la pubblicazione o il download del sito

> Se si pubblica o un sito di DotNetNuke di download, si potrebbe essere necessario cancellare la cache per ottenere i nuovi collegamenti vengano visualizzati nel sito.
> 
> **Soluzione alternativa**
> 
> 1. Accedere come "Host".
> 2. Passare al menu di host e selezionare **delle impostazioni dell'Host**.
> 3. Scorrere verso il basso e sotto **impostazioni avanzate**, espandere **le impostazioni delle prestazioni**.
> 4. Scegliere il **Cancella Cache** collegamento per le pagine.
> 5. Passare alla parte inferiore della pagina e riavviare l'applicazione.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problema: Alcuni collegamenti in AtomSite vengono interrotte dopo il download di un sito pubblicato

> **Soluzione alternativa**  
> Nel *service.config* file, *users.config* file e tutti i *. XML* file, sostituire la stringa URL (ad esempio, `http://myhost.com/atomsite`) con quello locale (ad esempio, `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problema: Le applicazioni basate su MySQL, ad esempio WordPress non è possibile pubblicare e segnalano un errore di database

> Per impostazione predefinita, WebMatrix installa MySQL con il set di caratteri UTF-8. Se si installa MySQL per conto proprio, e il set di caratteri non è UTF-8 (ad esempio, è Latin1), il processo di pubblicazione per i database potrebbe non riuscire.
> 
> **Soluzione alternativa**
> 
> 1. Modificare il set di caratteri per MySQL in UTF-8. (Per informazioni dettagliate, vedere [regole di confronto e Set di caratteri Server](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) sul sito Web di MySQL.)
> 2. Reinstallare l'applicazione.
> 3. Ripubblicare l'applicazione.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problema: "Download pubblicati sito" ha esito negativo per le applicazioni con il programma di installazione basata su browser

> Alcune applicazioni (ad esempio, Kentico CMS) è necessario avviarli in del browser per eseguire il programma di installazione post-installazione, ad esempio la creazione di un database. Se si pubblica un'applicazione simile al seguente senza completare l'installazione basata su browser, il download dello stesso sito da un server remoto non riuscirà.
> 
> **Soluzione alternativa**  
> Completare la configurazione basata su browser prima di pubblicare il sito.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problema: "Download pubblicati sito" ha esito negativo con un errore di database per DotNetNuke e Kooboo CMS

> Se si prova a scaricare un'applicazione da un server e si dispone di credenziali di amministratore nella stringa di connessione del database nel **impostazioni di pubblicazione** finestra di dialogo, si potrebbe essere visualizzato l'errore seguente nel log di pubblicazione:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Soluzione alternativa**  
> Se è più pratico, ripubblicare il sito (oppure che venga pubblicato) usando credenziali senza privilegi di amministratore per il database.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Ulteriori informazioni

Per ulteriori informazioni su WebMatrix 1.0, vedere i siti Web seguenti:

- [IIS.net](http://iis.net/)
- [ASP.NET 2.0](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Tutti i diritti riservati. [Le condizioni d'uso](https://msdn.microsoft.cos/cc300389.aspx).
