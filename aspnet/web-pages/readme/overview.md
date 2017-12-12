---
uid: web-pages/readme/overview
title: File Leggimi di WebMatrix | Documenti Microsoft
author: rick-anderson
description: WebMatrix e il file Leggimi ASP.NET Web Pages (Razor) versione 1.0
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 90f24550d2bb50147bab6be545be63c1838f312a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="webmatrix-readme"></a>File Leggimi di WebMatrix
====================
13 gennaio 2011

## <a name="contents"></a>Contenuto

> [!NOTE]
> Questo file Leggimi si applica alla 1.0 versione di WebMatrix.


- [Panoramica](#Overview)
- [Installazione](#Installation_Notes)
- [Come pubblicare applicazioni](#InstructionsForPublishingApplications)
- [Le modifiche e problemi](#ChangesAndIssues)

    - [Installazione di WebMatrix 1.0](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [L'installazione di applicazioni](#Known_Issues_Installing_Applications)
    - [Pubblicazione di applicazioni](#Known_Issues_Publishing_Applications)
- [Per ulteriori informazioni](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Panoramica

> Microsoft WebMatrix 1.0 è uno stack di sviluppo web gratuito che installa in pochi minuti. Si integra un server web con il database e programmazione Framework per creare un'esperienza unica e integrata. È possibile utilizzare WebMatrix per ottimizzare le attività di codice, testare e pubblicare il proprio sito Web ASP.NET o PHP oppure è possibile utilizzare WebMatrix per avviare un nuovo sito Web utilizzando più comuni App open source come DotNetNuke, Umbraco, WordPress o Joomla. WebMatrix utilizza stesso server web potente, motore di database e ambiente di Framework che verrà eseguito il sito Web in internet, che effettua la transizione dallo sviluppo alla produzione modo diretto e immediato.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installazione

> Per installare WebMatrix 1.0, è necessario installare il [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Dopo aver installato l'installazione guidata piattaforma Web, è possibile utilizzare, per installare WebMatrix.
> 
> Se si verificano problemi durante l'installazione, fare riferimento a [risoluzione dei problemi con l'installazione guidata piattaforma Web di Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Come pubblicare applicazioni

> Vedere [istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Le modifiche e problemi

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemi di installazione 1.0 di WebMatrix

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix 1.0 è disponibile solo su piattaforme che supportano Microsoft .NET Framework 4

> .NET Framework versione 4 è obbligatorio per WebMatrix. In alcuni casi, il programma di installazione di WebMatrix 1.0 consente di provare a installare su una piattaforma che non fa parte del set di configurazione supportata. In particolare, Windows Vista senza l'aggiornamento SP1 consentono di iniziare l'installazione di WebMatrix, ma il componente di .NET Framework 4 avrà esito negativo e bloccare l'installazione.
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


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: Impossibile installare WebMatrix 1.0 se è installato Microsoft Visual Studio 2008 senza Microsoft Visual Studio 2008 SP1

> **Soluzione alternativa**  
> Installare [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) dall'area Download Microsoft.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problema: Alcuni assembly di SQL Server Compact 4.0 non sono installati nella Global Assembly Cache

> Quando si installa SQL Server Compact 4.0 in un computer a 64 bit e il computer dispone solo di .NET Framework 3.5 SP1 Client Profile installato, gli assembly gestiti per SQL Server Compact 4.0 non vengono inseriti in global assembly cache (GAC). Gli assembly gestiti che non sono installati nella Global Assembly Cache sono:
> 
> - *SqlServerCe* (provider ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Soluzione alternativa**  
> Disinstallare SQL Server Compact 4.0. Scaricare e installare la versione completa di .NET Framework 3.5 SP1 dal percorso seguente:  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (pacchetto completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Reinstallare SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: Impossibile disinstallare SQL Server Compact mediante la riga di comando

> La disinstallazione di SQL Server Compact mediante le opzioni della riga di comando non funziona in questa versione.
> 
> **Soluzione alternativa**  
> Utilizzare *programmi e funzionalità* nel Pannello di controllo per disinstallare Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Pagine Web ASP.NET

In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e problemi noti con la 1.0 versione di ASP.NET Web Pages con sintassi Razor.

- [Nuove funzionalità](#NewFeatures)
- [Modifiche](#Changes)
- [Problemi](#Issues)

#### <a id="NewFeatures"></a>Nuove funzionalità

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Novità: Impostazione di configurazione aggiunta per disabilitare la gestione dei pacchetti

> Un nuovo `asp:AdminManagerEnabled` chiave è disponibile per il `<appSettings>` elemento il *Web. config* file, che consente di disabilitare completamente la gestione dei pacchetti. Il valore predefinito per questo elemento è true, il che significa che se non è incluso nel *Web. config* file, la gestione dei pacchetti è abilitato. Per disabilitare la gestione dei pacchetti, aggiungere il seguente elemento di *Web. config* file nella radice del sito Web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>Modifiche

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Modifica: tasto "webPages:AdminFolderVirtualPath" rinominato in "asp: AdminFolderVirtualPath"

> Il `webPages:AdminFolderVirtualPath` chiave che può essere aggiunti al *Web. config* file per specificare il percorso di package manager è stato rinominato per utilizzare il `asp:` dello spazio dei nomi anziché i `webPages` dello spazio dei nomi. Se si utilizza questo elemento, è necessario rinominare la cartella nel file di configurazione.


#### <a id="Issues"></a>Problemi noti

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problema: Le password degli utenti di appartenenza non riconosciuti.

> L'algoritmo per la creazione e l'archiviazione delle password di appartenenza (accesso) è stato modificato per essere più sicura. Di conseguenza, le password archiviate per i membri (utenti) creati in versioni Beta di ASP.NET Razor non verrà riconosciute. 
> 
> **Soluzione alternativa** se il sito non è ancora stato inserito nell'ambiente di produzione, è possibile rimuovere i record utente dal database di appartenenza. Se il database è in tempo reale, è necessario rigenerare a livello di codice le password esistenti nel database delle appartenenze.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Un comportamento imprevisto quando si utilizza una tabella utente personalizzato per l'appartenenza

> Per inizializzare il provider di appartenenze per un sito Web ASP.NET Razor, si chiama il `WebSecurity.InitializeDatabaseConnection` metodo. (In WebMatrix, il modello Starter Site include una chiamata al metodo nel  *\_AppStart.cshtml* file.) Se il `autoCreateTables` parametro di questo metodo è impostato su true (per impostazione predefinita, viene impostata su true nel modello Starter Site), e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera un errore. Al contrario, viene creato automaticamente nella tabella.
> 
> Può trattarsi di un problema, se si prevede di utilizzare una tabella utente personalizzata per l'appartenenza ma passare il nome della tabella non corretto per il `WebSecurity.InitializeDatabaseConnection` metodo. Poiché il metodo per impostazione predefinita genera un errore se la tabella specificata non esiste, e invece creata una nuova tabella, può sembrare che l'applicazione si sta lavorando. Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui campi in essa contenuti) può avere esito negativo con errori imprevisti.
> 
> **Soluzione alternativa**  
> Assicurarsi che il nome passato il `InitializeDatabaseConnection` corrispondenze metodo tabella nel database delle appartenenze, il profilo utente o assicurarsi che il `autoCreateTables` parametro è impostato su false.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>: Problema Messaggio di errore "il modulo di amministrazione richiede l'accesso a ~/App\_dati"

> In alcuni casi, durante la creazione di utenti o in altro modo il sistema di appartenenze ASP.NET può determinare la pagina visualizzare l'errore *il modulo di amministrazione richiede l'accesso a ~/App\_dati*. Questo errore si verifica se è in esecuzione IIS o IIS Express con l'account non dispone delle autorizzazioni per creare e scrivere il *App\_dati* cartella nella radice del sito Web. 
> 
> **Soluzione alternativa** creare manualmente un *App\_dati* cartella per il sito Web. Assicurarsi che l'account di Windows a cui viene eseguito l'applicazione (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio App\_dati. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [problemi istanze utente di SQL Server Express e progetti di applicazione Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Errore: problema "Impossibile generare un'istanza utente di SQL Server"

> Se un'applicazione Web di WebMatrix utilizza SQL Server Express è in esecuzione IIS 7.5 in Windows 7 o Windows Server 2008 R2, si potrebbe essere visualizzato un errore che indica che SQL Server non è possibile recuperare il percorso dell'applicazione locale dell'utente in fase di esecuzione.
> 
> **Soluzione alternativa** assicurarsi che l'account di Windows a cui viene eseguito l'applicazione (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *App\_dati*. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [problemi istanze utente di SQL Server Express e progetti di applicazione Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problema: I file che contiene le risorse di gestione di pacchetti o le password di gestione di pacchetti sono servable in IIS 6.0 e versioni precedenti

> Se si distribuisce un'applicazione ASP.NET Web Pages (Razor) che è stata compilata utilizzando la versione RC2, e se l'applicazione contiene un *password.txt* o *packagesources.txt* file */App\_ Dati/admin*, IIS 6.0 verrà utilizzato il file, se richiesto, può esporre le password per l'istanza di gestione del pacchetto. 
> 
> **Soluzione alternativa** rinominare il *password.txt* o *packagesources.txt* file *password.config* o *packagesources.config*. Per impostazione predefinita, IIS 6.0 non serve i file con il *config* estensione. (In IIS 7, nessun file di *App\_dati* cartella vengono soddisfatte, pertanto non è necessario rinominare i file.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problema: I pacchetti installati utilizzando la versione Beta 3 di disinstallazione non rimuovere completamente i componenti del pacchetto

> Se è installato un pacchetto tramite Gestione pacchetti nella versione Beta 3 e quindi provare a disinstallare utilizzando la versione corrente, il pacchetto non è completamente disinstallato. Utilizzando la gestione di pacchetti **Disinstalla** pulsante consente di rimuovere alcuni componenti, ma lascia il codice di libreria del pacchetto e non aggiorna il *package.config* file.
> 
> **Soluzione alternativa**   
> Eseguire questi passaggi:  
> 1. Eliminare il *App\_Data\packages* cartella. Questa operazione rimuove tutti i pacchetti.   
> 2. Eliminare il *Packages* file nella radice del sito Web.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problema: In Visual Studio, richiamare la gestione dei pacchetti basata sul web richiede l'applicazione non in linea

> Se si lavora in Visual Studio (non WebMatrix) e utilizzare il  *\_admin* funzionalità per la gestione di pacchetti, Visual Studio di avviare l'applicazione viene portata offline e registra il *app\_ offline.htm* nella radice del sito Web, che interferisce con la possibilità di utilizzare la gestione dei pacchetti.
> 
> [!NOTE]
> Sebbene questo comportamento si vedrà più di frequente quando si utilizza l'interfaccia di gestione basato su web pacchetto, lo stesso comportamento si verifica se aggiungere, rimuovere o modificare i file di *App\_dati* cartella.
> 
> **Soluzione alternativa**   
> Per utilizzare i pacchetti in Visual Studio, utilizzare l'estensione NuGet anziché la gestione dei pacchetti basata sul web. Per informazioni, vedere il [documentazione di NuGet](https://docs.microsoft.com/nuget/). Se si lavora con altri file di *App\_dati* cartella, si consiglia di conservare i file in un' posizione per evitare questo problema. Se non è pratico, eliminare il *app\_offline.htm* file manualmente o attendere che il sito torna online automaticamente (impostazione predefinita, dopo 30 secondi).


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense e progetto modelli disponibili solo in ASP.NET MVC versione 3

> L'installazione di ASP.NET Web Pages non installare anche gli strumenti per Visual Studio, ad esempio i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages.
> 
> **Soluzione alternativa** per utilizzare i modelli di progetto e IntelliSense per le applicazioni di pagine Web ASP.NET in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: Lettura feed o altri dati esterni tramite un server proxy

> Se il server che esegue il sito è un server proxy, potrebbe essere necessario configurare le informazioni sul proxy nel *Web. config* file per poter essere in grado di leggere informazioni proveniente dall'esterno del sito. Ad esempio, se si utilizza il `ReCaptcha` helper, il supporto comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato da server proxy. Analogamente, feed che vengono utilizzati in ASP.NET Web Pages, ad esempio il feed utilizzato dal gestore del pacchetto, potrebbe richiedere la configurazione del proxy.
> 
> Se si verificano problemi nell'utilizzo di un servizio esterno o l'utilizzo con il feed del pacchetto, è possibile inserire gli elementi seguenti nella directory principale dell'applicazione *Web. config* file:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Per ulteriori informazioni sulla configurazione di un server proxy, vedere [ &lt;proxy&gt; elemento (impostazioni di rete)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) nel sito Web MSDN.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Disinstallazione di .NET Framework versione 4 Disabilita ASP.NET Web Pages con sintassi Razor

> Se si disinstalla .NET Framework versione 4 e quindi reinstallarlo, ASP.NET Web Pages con sintassi Razor è disabilitato. Pagine con il *. cshtml* estensione non vengono eseguite correttamente. ASP.NET Web Pages registra un assembly nella directory principale macchina *Web. config* file e la rimozione di .NET Framework consente di rimuovere tale file. Reinstallare .NET Framework viene installata una nuova versione del file di configurazione, ma non aggiunge il riferimento per l'assembly di ASP.NET Web Pages.
> 
> **Soluzione alternativa** dopo la reinstallazione di .NET Framework, reinstallare ASP.NET Web Pages con sintassi Razor. Aggiunge l'elemento seguente per il *Web. config* file nella radice della macchina, ovvero in genere nel percorso seguente:  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: URL senza estensione non si trova il file.cshtml/.vbhtml in IIS 7 o IIS 7.5

> In IIS 7 o IIS 7.5, le richieste con un URL simile al seguente non sono in grado di trovare le pagine che hanno il *. cshtml* o *. vbhtml* estensione:  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> Il problema si verifica perché la riscrittura URL non è abilitata per impostazione predefinita per IIS 7 o IIS 7.5. Lo scenario probabile che è che il problema non viene visualizzato durante il test in locale utilizzando IIS Express, ma si verificano quando si distribuisce il sito Web in un sito Web di hosting.
> 
> **Soluzione alternativa**
> 
> - Se si dispone di un controllo sul computer server, nel computer del server, installare l'aggiornamento descritto in [è disponibile che consente di determinati gestori di IIS 7.0 o IIS 7.5 di gestire le richieste il cui URL non terminano con un periodo di un aggiornamento](https://support.microsoft.com/kb/980368).
> - Se non è un controllo sul computer server (ad esempio, si distribuisce in un sito di hosting Web), aggiungere quanto segue per il sito Web *Web. config* file: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Distribuire un'applicazione in un computer che non dispone di SQL Server Compact è installato

> Le applicazioni che includono database di SQL Server Compact è possono eseguire in un computer in cui SQL Server Compact non è installato. Microsoft WebMatrix 1.0 automaticamente copia i file binari per l'utente ed esegue appropriata *Web. config* file trasformazioni.
> 
> **Soluzione alternativa** se è necessario copiare questi file, assicurarsi di *Web. config* modifiche al file manualmente, eseguire le operazioni seguenti:
> 
> 1. Copiare gli assembly del motore di database per il *Bin* (cartelle e sottocartelle) dell'applicazione nel computer di destinazione:  
> 
>     - Copia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>         **per** *\bin.*
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* * * per***\Bin\x86*
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **a***\Bin\amd64*
> 2. Nella cartella radice del sito Web, creare o aprire un *Web. config* file. (In WebMatrix 1.0, è disponibile se si fa clic su questo tipo di file **tutti** nel **scegliere un tipo di File** la finestra di dialogo.)
> 3. Aggiungere il seguente elemento come figlio del `<configuration>` elemento (non all'interno di `<system.web>` elemento):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: "Database" e "WebGrid" helper non funzionano in attendibilità media in Visual Basic

> Se si utilizza Visual Basic (creazione *. vbhtml* file), il `Database` e `WebGrid` helper non funzionerà se l'applicazione è impostato per utilizzare l'attendibilità Media.
> 
> **Soluzione alternativa**  
> Se si utilizza Visual Studio 2010, è possibile risolvere il problema installando la versione del Service Pack 1. Fino a quando non è disponibile la versione finale della versione SP1, è possibile scaricare la versione Beta di SP1 dal [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) pagina Microsoft Download Center.   
>   
> Se questo non è pratico, o se non si utilizza Visual Studio 2010, è possibile temporaneamente impostare l'applicazione di usare l'attendibilità totale.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problema: "ApplicationPart" risorse sono accessibili dall'esterno

> Se un assembly contiene gli oggetti da cui deriva il `ApplicationPart` che le risorse dell'assembly vengono esposti dalla classe di `ResourceRouteHandler` classe. Si consideri ad esempio l'URL seguente:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> La richiesta di download di tutte le stringhe di risorsa nel *System.Web.WebPages.Administration.dll* assembly. Tutte le risorse incorporate, anche quelli che non sono destinati a essere utilizzato come contenuto statico, vengono scaricate. Se le risorse incorporate contengono informazioni riservate, questo può rappresentare un rischio per la sicurezza. 
> 
> **Soluzione alternativa**   
> Se si crea un **ApplicationPart** dell'oggetto, assicurarsi che le risorse incorporate associato che **ApplicationPart** assembly dell'oggetto non contengono informazioni riservate.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Per informazioni sui problemi di installazione per WebMatrix, vedere [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.


In questa sezione del documento vengono descritti i problemi noti per l'ambiente di sviluppo WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problema: Il nome utente o password di una stringa di connessione di database in un file Web. config non vengono riflesse nell'area di lavoro database

> **Soluzione alternativa**  
> 
> 1. Nel *Web. config* file, modificare il nome del database nella stringa di connessione (ad esempio, aggiungervi "1").
> 2. Salvare il *Web. config* file.
> 3. Fare clic su **database** e aggiornare.
> 4. Modificare il nome del database nella stringa di connessione nel *Web. config* file il nome del database originale.
> 5. Salvare il *Web. config* file.
> 6. Fare clic su **database** e aggiornare.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problema: Impossibile eliminare le cartelle create da WebMatrix

> Se WebMatrix è in esecuzione con autorizzazioni elevate (vale a dire, iniziare a utilizzare WebMatrix il **Esegui come amministratore** opzione in Windows), Impossibile eliminare le cartelle create da WebMatrix utilizzando Esplora risorse.
> 
> **Soluzione alternativa**  
> Eseguire Esplora risorse di Windows con autorizzazioni elevate. Attenersi ai passaggi riportati di seguito.  
> 
> 1. In Windows, fare clic su **avviare**.
> 2. Immettere "Esplora" e fare clic sulla voce per **Esplora**.
> 3. Fare clic su **Esegui come amministratore**. È quindi possibile eliminare le cartelle.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix 1.0 non è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi

> WebMatrix 1.0 non è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:
> 
> - In Windows Vista o Windows 7, si è connessi con un account che dispone di privilegi amministrativi e il controllo dell'Account utente (UAC) è disabilitata.
> - Si utilizza Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Soluzione alternativa**  
> La maggior parte delle attività in WebMatrix 1.0 non richiedono autorizzazioni amministrative. Se sono presenti, è possibile eseguire l'operazione come amministratore o eseguire la procedura seguente:
> 
> - In Windows Vista o Windows 7, abilitare il controllo dell'account utente.
> - In Windows XP, aggiungere l'utente al gruppo di sicurezza Administrators.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "Del sito dalla raccolta di Web" è disabilitata

> Il **sito dalla raccolta Web** opzione è disabilitata se l'installazione guidata piattaforma Web 3.0 non è installato.
> 
> **Soluzione alternativa**  
> Installare il [installazione guidata piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: Google Chrome non è disponibile come opzione di esecuzione

> Google Chrome non viene visualizzata nell'elenco dei browser in **eseguire** sul **Home** scheda.
> 
> **Soluzione alternativa**  
> Alcune versioni di Google Chrome non effettuano la registrazione in modo corretto con la funzionalità di programmi predefiniti di Windows. In alternativa, avviare Google Chrome, scegliere il *personalizzare e controllo Google Chrome* menu, fare clic su *opzioni*e quindi fare clic su *verificare Google Chrome il browser predefinito*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: La finestra di dialogo "Foreign Key" non consente l'immissione di una chiave primaria

> Il **Foreign Key** la finestra di dialogo non consente di immettere il nome della chiave primario della tabella di chiave primaria.
> 
> **Soluzione alternativa**  
> Ciò è intenzionale. Non è necessario immettere il nome della chiave primaria della tabella della chiave primaria.


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problema: IntelliSense non è disponibile in WebMatrix per Razor sintassi c# o Visual Basic

> IntelliSense è supportato in WebMatrix per HTML e CSS. Non è tuttavia disponibile per altri linguaggi. 
> 
> **Soluzione alternativa**   
> Nessuno.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problema: IntelliSense per HTML e CSS suggerisce gli elementi che non sono appropriati in base al contesto

> IntelliSense per il markup in WebMatrix supporta l'HTML usando il [schema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e l'utilizzo di CSS di [dello schema CSS 2.1](http://www.w3.org/TR/CSS2/). Perché IntelliSense è basato su tali schemi specifici, determinati tag, attributi o proprietà potrebbero suggerite che non sono appropriate per la definizione di stile o di pagina corrente. Per HTML, può anche provocare ai suggerimenti imprevisti nel contenuto che potrebbe essere interpretata come XHTML in formato non valido (ad esempio, quando i tag non chiusi). Questo problema potrebbe essere più evidente se il punto di inserimento si trova all'interno di un tag incompleto; In tal caso, IntelliSense potrebbe suggerire nuovi tag di apertura o altri suggerimenti non corretto. 
> 
> **Soluzione alternativa**   
> Per HTML, assicurarsi che l'utilizzo all'interno di una pagina XHTML completezza in formato corretto. Per CSS, è disponibile alcuna soluzione.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problema: IntelliSense non è stato richiamato durante la digitazione

> In alcuni casi, IntelliSense potrebbe non richiamato come HTML o CSS vengano immessi nell'editor. In particolare, questo può verificarsi quando il punto di inserimento è direttamente accanto a un altro elemento o alla fine di un file. 
> 
> **Soluzione alternativa**   
> Assicurarsi che vi sia uno spazio vuoto attorno al punto di inserimento e che il punto di inserimento non è alla fine di un file. È anche possibile richiamare IntelliSense manualmente premendo Ctrl + barra spaziatrice.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problema: Alcuna interfaccia utente non è disponibile per la disabilitazione di IntelliSense

> WebMatrix 1.0 non fornisce alcuna interfaccia utente o movimento per la disabilitazione di IntelliSense. 
> 
> **Soluzione alternativa**   
> Avviare WebMatrix mediante il comando seguente, che include un'opzione che disabilita IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express è il proprio file Leggimi, che è disponibile all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkId=207675&amp;clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact è il proprio file Leggimi, che è disponibile all'URL seguente:

[https://go.microsoft.com/fwlink/?LinkId=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Per informazioni sui problemi che comportano l'installazione di SQL Server Compact come parte di WebMatrix, vedere [problemi di installazione di WebMatrix](#Known_Issues_Installation) più indietro in questo documento.

### <a id="Known_Issues_Installing_Applications"></a>L'installazione di applicazioni

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: L'installazione di un'applicazione può richiedere molto tempo se cartella documenti dell'utente viene reindirizzata a una condivisione di rete

> **Soluzione alternativa**  
> Nessuno. L'applicazione potrebbe richiedere qualche istante per l'installazione, ma verrà installato correttamente.


### <a id="Known_Issues_Publishing_Applications"></a>Pubblicazione di applicazioni

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problema: "richiesto non è possibile acquisire le autorizzazioni" Errore durante la pubblicazione di un Database di SQL Compact

> WebMatrix non supporta completamente la distribuzione di file binari del supporto per SQL Server Compact a un server che esegue .NET Framework versione 3.5 con una configurazione a livello di attendibilità medio.
> 
> **Soluzione alternativa**  
> La soluzione migliore consiste nell'installare .NET Framework 4 nel server. In alternativa, eseguire le operazioni seguenti:
> 
> 1. Aggiungere i seguenti elementi per il `SecurityClasses` sezione *Web\_MediumTrust.config* file:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Creare un nuovo set di autorizzazioni nel *Web\_MediumTrust.config* file con le autorizzazioni necessarie seguenti:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Applicare l'autorizzazione impostata su SQL Server Compact inserisce gli elementi seguenti nel *Web\_MediumTrust.config* file:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problema: Le applicazioni web della raccolta e PhpBB visualizzano un errore "Servizio non è disponibile" dopo la pubblicazione

> In alcune circostanze, la pubblicazione di un'applicazione genera un errore di "servizio non è disponibile".
> 
> **Soluzione alternativa**  
> In WebMatrix, aggiungere una barra rovesciata (\) alla fine del nome del server nel **impostazioni di pubblicazione** finestra e quindi pubblicare nuovamente l'applicazione.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problema: I collegamenti e il layout del sito Web di Moodle vengono interrotti dopo la pubblicazione

> Dopo la pubblicazione di un'applicazione di Moodle, l'applicazione non funziona correttamente.
> 
> **Soluzione alternativa**  
> In WebMatrix, aggiungere una barra (/) alla fine del **nome sito** campo il **impostazioni di pubblicazione** finestra e quindi pubblicare nuovamente l'applicazione.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problema: La pubblicazione nopCommerce viene generato un errore di database

> NopCommerce pubblicazione ha esito negativo e restituisce un errore di database come "inserire la nop\_tabella del log non è riuscita."
> 
> **Soluzione alternativa**  
> 
> 1. In WebMatrix, fare clic su **eseguire** per avviare nopCommerce localmente.
> 2. Accedere alla pagina Amministrazione.
> 3. Fare clic su di **sistema** menu.
> 4. Fare clic su di **Log** opzione.
> 5. Fare clic su di **Cancella Log** pulsante.
> 6. Pubblicare nuovamente nopCommerce.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problema: Silverstripe CMS viene visualizzato un "500 PHP FCGI errore HTTP" quando si scarica un sito pubblicato

> **Soluzione alternativa**  
> Dopo aver fatto clic **Download pubblicazione sito**, ignorare `silverstripe-cache/manifest_main` in **Anteprima pubblicazione**. Questo file viene utilizzato per la memorizzazione ed è specifico per ogni computer.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problema: Subtext viene visualizzato "Errore Server nell'applicazione '/'" quando si scarica un sito pubblicato

> **Soluzione alternativa**  
> Aprire il sito *Web. config* file e sostituire l'ID utente e password nella stringa di connessione del database con le credenziali di amministratore di SQL Server (le credenziali "sa").
> 
> In alternativa, eseguire questi passaggi per concedere all'account utente si è connessi con `db_owner` autorizzazioni:
> 
> 1. Installare SQL Server Management Studio utilizzando l'installazione guidata piattaforma Web.
> 2. Connettersi all'istanza locale di SQL Server Express (per impostazione predefinita, `.\SQLEXPRESS`).
> 3. Fare clic su **database** &gt; *[localSubtextDatabase]* &gt; **sicurezza** &gt; **utenti** &gt; *[localSubtextUser*] (valore predefinito è `subtextuser`], mouse e scegliere **proprietà**.
> 4. Selezionare **db\_proprietario** nella sezione l'appartenenza al ruolo.


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: Sito potrebbe non funzionare dopo la pubblicazione se il campo "URL di destinazione" non è preceduto da http:// o https://

> Nel **le impostazioni di pubblicazione** la finestra di dialogo, se l'URL di destinazione non inizia con `http://` o `https://`, il sito potrebbe non funzionare dopo la distribuzione.
> 
> **Soluzione alternativa**  
> Verificare che prima di pubblicare un sito, l'URL di destinazione nel **impostazioni di pubblicazione** la finestra di dialogo inizia con `http://` o `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: La pubblicazione di un database MySQL non riesce con l'errore "Impossibile pubblicare il database. Questa situazione può verificarsi se il database remoto non è possibile eseguire lo script."

> L'errore può verificarsi per diversi motivi. Un motivo per cui che è possibile visualizzare questo errore è se lo script del database contiene un carattere di virgoletta singola (') e il set di caratteri predefinito del database MySQL di destinazione non è presente in UTF-8.
> 
> **Soluzione alternativa**  
> Impostare il carattere predefinito impostato per il database MySQL remoto su UTF-8.


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problema: Alcuni collegamenti non sono visibili in DotNetNuke dopo la pubblicazione o il sito di download

> Se si pubblica o scarica un sito di DotNetNuke, si potrebbe essere necessario cancellare la cache per ottenere i nuovi collegamenti vengano visualizzati nel sito.
> 
> **Soluzione alternativa**
> 
> 1. Accedere come "Host".
> 2. Passare al menu di host e selezionare **impostazioni Host**.
> 3. Scorrere verso il basso e in **impostazioni avanzate**, espandere **le impostazioni delle prestazioni**.
> 4. Fare clic su di **Cancella Cache** collegamento delle pagine.
> 5. Passare alla parte inferiore della pagina e riavviare l'applicazione.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problema: Alcuni collegamenti in AtomSite vengono interrotti dopo il download di un sito pubblicato

> **Soluzione alternativa**  
> Nel *service.config* file, *users.config* file e tutti *XML* file, sostituire la stringa URL (ad esempio, `http://myhost.com/atomsite`) con quello locale (ad esempio, `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problema: Le applicazioni basate su MySQL WordPress non è possibile pubblicare e segnalare un errore di database

> Per impostazione predefinita, WebMatrix installa MySQL con il set di caratteri UTF-8. Se è possibile installare MySQL per conto proprio e il set di caratteri non UTF-8 (ad esempio, è Latin1), il processo di pubblicazione per i database potrebbe non riuscire.
> 
> **Soluzione alternativa**
> 
> 1. Modificare il set di caratteri per MySQL UTF-8. (Per informazioni dettagliate, vedere [Server Set di caratteri e le regole di confronto](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) sul sito Web di MySQL.)
> 2. Reinstallare l'applicazione.
> 3. Ripubblicare l'applicazione.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problema: "Sito di Download pubblicato" non riesce per le applicazioni che dispongono di un'installazione basata su browser

> Alcune applicazioni (ad esempio, Kentico CMS) è necessario avviarli nel browser per eseguire il programma di installazione di post-installazione, ad esempio la creazione di un database. Se si pubblica un'applicazione simile al seguente senza completare l'installazione basata su browser, verrà eseguito il tentativo di scaricare lo stesso sito da un server remoto.
> 
> **Soluzione alternativa**  
> Completare l'installazione basata su browser prima di pubblicare il sito.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problema: "Sito di Download pubblicato" viene generato un errore di database per DotNetNuke e Kooboo CMS

> Se si tenta di scaricare un'applicazione da un server e si dispone delle credenziali nella stringa di connessione del database nel **impostazioni di pubblicazione** finestra di dialogo, si potrebbe vedere il seguente errore nel Registro di pubblicazione:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Soluzione alternativa**  
> Se possibile, eseguire di nuovo il sito (o pubblicarlo) usando credenziali senza privilegi di amministratore per il database.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Ulteriori informazioni

Per ulteriori informazioni su WebMatrix 1.0, vedere i siti Web seguenti:

- [IIS.net](http://iis.net/)
- [ASP.NET 2.0](https://asp.net/webmatrix)
- [Microsoft.com](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Tutti i diritti riservati. [Condizioni di utilizzo](https://msdn.microsoft.com/en-us/cc300389.aspx).
