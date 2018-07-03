---
uid: web-pages/readme/beta3
title: WebMatrix e ASP.NET Web Pages (Razor) Beta 3 Release Leggimi | Microsoft Docs
author: rick-anderson
description: WebMatrix e ASP.NET Web Pages (Razor) Beta 3 Release Leggimi
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5aa609bf31499485dc7a1298fa689f3a7cee4774
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363533"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>WebMatrix e ASP.NET Web Pages (Razor) Beta 3 Release Leggimi
====================
> WebMatrix e ASP.NET Web Pages (Razor) Beta 3 Release Leggimi

9 novembre 2010

## <a name="contents"></a>Sommario

- [Panoramica](#Overview)
- [Installazione](#Installation_Notes)
- [Nuove funzionalità, le modifiche e problemi noti nella versione Beta 3](#Known_Issues)

    - [Problemi di installazione di WebMatrix](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Installazione di applicazioni](#Known_Issues_Installing_Applications)
    - [Pubblicazione di applicazioni](#Known_Issues_Publishing_Applications)
    - [Altri problemi](#Known_Issues_Other_Issues)
- [Per altre informazioni](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Panoramica

> Microsoft WebMatrix Beta è uno stack di sviluppo web gratuito che consente di installare in pochi minuti. Si integra un server web con il database e Framework per creare un'esperienza unica e integrata di programmazione. È possibile usare Beta di WebMatrix per ottimizzare le attività di codice, testare e pubblicare il proprio sito Web ASP.NET o PHP oppure è possibile usare una versione Beta di WebMatrix per avviare un nuovo sito Web con più comuni App open source quali DotNetNuke, Umbraco, WordPress o Joomla. Versione Beta di WebMatrix utilizza sullo stesso server web potente, motore di database e ambiente di Framework che verrà eseguito il sito Web su internet, che semplifica il passaggio dallo sviluppo alla produzione diretto e immediato.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installazione

> Per installare WebMatrix Beta 3, usare [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Dopo aver installato l'installazione guidata piattaforma Web, è possibile usarlo per installare WebMatrix Beta 3.
> 
> Se si verificano problemi durante l'installazione, fare riferimento a [risoluzione dei problemi con Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Istruzioni per la pubblicazione di applicazioni

> Vedere [istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nuove funzionalità, le modifiche, andKnown problemi

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Installazione della versione Beta di WebMatrix 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix Beta 3 è disponibile solo sulle piattaforme che supportano Microsoft .NET Framework 4

> .NET Framework versione 4 è obbligatorio per la versione Beta di WebMatrix. In alcuni casi, il programma di installazione di WebMatrix Beta consente di provare a installare su una piattaforma che non fa parte del set di configurazione supportata. In particolare, Windows Vista senza l'aggiornamento SP1 consentirà di avviare l'installazione della versione Beta di WebMatrix, ma il componente di .NET Framework 4 avrà esito negativo e bloccare l'installazione.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: Non è possibile installare WebMatrix Beta 3 se Microsoft Visual Studio 2008 viene installato senza Microsoft Visual Studio 2008 SP1

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

In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e problemi noti con la versione Beta 3 di ASP.NET Web Pages con sintassi Razor.

- [Nuove funzionalità](#NewFeatures)
- [Modifiche](#Changes)
- [Problemi](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nuove funzionalità in versione Beta 3 per ASP.NET Web Pages con sintassi Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Novità: Il metodo "HTML. RAW" esegue il rendering di markup non codificato

> Il nuovo `Html.Raw` metodo consente di eseguire il rendering di markup HTML come markup anziché il rendering di output codificata. (Per impostazione predefinita, ASP.NET Razor consente di codificare le stringhe prima eseguito il rendering.) La sintassi è:
> 
> `Html.Raw(value)`
> 
> Nell'esempio riportato di seguito viene illustrato come usare `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Modifiche nella versione Beta 3 per ASP.NET Web Pages con sintassi Razor

#### <a name="change-hrefattribute-method-removed"></a>Modifica: Il metodo "HrefAttribute" rimosso

> Il `HrefAttribute` metodo di `WebPage` classe è stata rimossa. Questo supporto è stato usato per codificare i caratteri non sicuri nell'URL. Non è più necessario perché ASP.NET Razor codifica automaticamente stringhe. (Usare il nuovo `Html.Raw` metodo per eseguire il rendering delle stringhe non codificate.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Modifica: Sintassi dichiarativa "@helper" helper modificato

> Nella versione Beta 3, ASP.NET viene modificato come analizza gli helper che vengono creati utilizzando il `@helper` sintassi. In sostanza, il `@helper` sintassi ora viene analizzata come un blocco di codice invece che come blocco di markup che può includere il codice. Pertanto, il codice all'interno dell'helper non dovrà essere racchiuso tra parentesi `@{ }` blocchi. Al contrario, markup all'interno dell'helper deve essere incluso in modo esplicito gli elementi HTML o ASP.NET Razor `<text></text>` tag.
> 
> Ad esempio, il seguente `@helper` sintassi funziona in versione Beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Nella versione Beta 3, questo supporto deve essere modificato per essere simile all'esempio seguente:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Si noti che il `@{ }` caratteri intorno al codice iniziale nell'helper non viene più utilizzata. Questo avviene perché il contenuto degli helper viene considerato come un blocco di codice per impostazione predefinita. L'helper di rendering del markup, che inizia con l'apertura `<a>` tag. Se è necessario eseguire il rendering dell'helper tag che non includono un tag di chiusura o testo normale (ad esempio, `<meta>` tag), il rendering del contenuto deve essere `<text></text>` tag.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Change: "WebPageContext.HttpContext" removed

> Il `WebPageContext.HttpContext` proprietà è stata rimossa. In alternativa, usare `HttpContext.Current`. (Il `WebPageContext.HttpContext` proprietà semplicemente eseguito il wrapping in questo.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Modifica: Helper "Facebook" spostato al nuovo pacchetto

> Il `Facebook` helper è stata spostata nel *Facebook.Helper* libreria, che include il `Facebook` helper e funzionalità aggiuntive. È necessario installare questa libreria come pacchetto separato, come descritto in "Installare helper con Gestione pacchetti" nell'esercitazione [Introduzione alle pagine ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Modifica: L'appartenenza al ruolo e sicurezza tipi passa al nuovo assembly

> I tipi seguenti sono stati spostati le `WebMatrix.WebData` assembly:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Modifica: Classe "TagBuilder" Sposta System.Web.WebPages.dll assembly

> Il `TagBuilde` classe r è state spostate in assembly System.Web.WebPages.dll. In precedenza, questo è stato in un assembly che fa parte di ASP.NET MVC. Questa modifica significa che è necessario non installare ASP.NET MVC per usare il `TagBuilder` classe.
> 
> Tuttavia, la classe è ancora nel `System.Web.Mvc` dello spazio dei nomi. Per usare la `TagBuilder` classe (ad esempio, in un helper ASP.NET Razor personalizzato), è necessario fare riferimento a spazio dei nomi (ad esempio, aggiungendo `@using System.Web.Mvc` al codice).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Modifica: Stata modificata; la sintassi di convalida richiesta Classe di "Convalida" rimosso

> Nella versione Beta 3, per disabilitare la convalida per un singolo campo o un set di campi, è possibile chiamare il `Validation.Exclude` , passando il nome o nomi dei campi da escludere dalla convalida. Una nuova sintassi è disponibile nella versione Beta 3 per la convalida verrà ignorata. Il `Validation` metodo utilizzato nella versione Beta 3 è stata rimossa.
> 
> > [!NOTE]
> > Se non si disabilita la convalida della richiesta, se gli utenti provano a caricare il markup HTML (ad esempio, usando un editor di testo RTF su una pagina), il sito Web verrà segnalato un errore simile *è stato rilevato un valore Request. Form potenzialmente pericoloso dal client*e non è stato accettato l'input dell'utente. Se si disabilita la convalida della richiesta, è necessario archiviare manualmente input dell'utente per assicurarsi che non contengono il markup potenzialmente pericoloso o creare script usando uno strumento come il [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Per disabilitare la convalida delle richieste automatica, chiamare il `Request.Unvalidated` passandogli il nome del campo o un altro oggetto post che si desidera ignorare la convalida delle richieste per. È possibile usare questo metodo per ignorare la convalida per tutti gli elementi nel `Form`, `QueryString`, `Cookies`, e `ServerVariables` raccolte. Gli esempi seguenti illustrano come usare il `Unvalidated` metodo:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problemi noti per ASP.NET Web Pages con sintassi Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Un comportamento imprevisto quando si usa una tabella utente personalizzato per l'appartenenza

> Per inizializzare il provider di appartenenze per un sito Web ASP.NET Razor, si chiama il `WebSecurity.InitializeDatabaseConnection` (metodo). (In WebMatrix, il modello Starter Site include una chiamata al metodo nel  *\_AppStart.cshtml* file.) Se il `autoCreateTables` parametro di questo metodo è impostato su true (per impostazione predefinita, viene impostata su true nel modello Starter Site), e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera un errore. Al contrario, viene creato automaticamente nella tabella.
> 
> Può trattarsi di un problema, se si prevede di usare una tabella utente personalizzata per l'appartenenza, ma passare il nome della tabella non corretto per il `WebSecurity.InitializeDatabaseConnection` (metodo). Poiché il metodo non per impostazione predefinita genera un errore se la tabella specificata non esiste e perché crea invece una nuova tabella, è possibile l'applicazione sembra funzionare. Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui campi in essa) può avere esito negativo con errori imprevisti.
> 
> **Soluzione alternativa**  
> Assicurarsi che il nome passato il `InitializeDatabaseConnection` corrispondenze di metodo del profilo utente di tabella nel database di appartenenza o assicurarsi che il `autoCreateTables` parametro è impostato su false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: errore "Non riuscita a generare un'istanza utente di SQL Server"

> Se un'applicazione Web di WebMatrix utilizza SQL Server Express ed è in esecuzione IIS 7.5 in Windows 7 o Windows Server 2008 R2, si potrebbe essere visualizzato un errore che indica che SQL Server non è possibile recuperare il percorso di applicazioni locali dell'utente in fase di esecuzione.
> 
> **Soluzione alternativa** assicurarsi che l'account di Windows che l'applicazione viene eseguita con (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *App\_Data*. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [i problemi delle istanze utente di SQL Server Express e ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problema: In Visual Studio, gli spazi dei nomi per gli assembly personalizzati (DLL) non vengono importati automaticamente

> Se si usano gli assembly personalizzati in un progetto in Visual Studio, gli spazi dei nomi dichiarati in tali assembly non vengono importati automaticamente in fase di progettazione. Di conseguenza, i riferimenti a tipi personalizzati potrebbero non essere riconosciuti in fase di progettazione e sono contrassegnati come non riconosciuti in Visual Studio (con una sottolineatura di""). Questo problema si verifica solo in fase di progettazione in Visual Studio. l'applicazione venga eseguita correttamente.
> 
> **Soluzione alternativa**  
> Includere un `using` istruzione (`imports` in Visual Basic) che fa riferimento l'entità che non sono riconosciute in fase di progettazione.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense e modelli di progetto disponibili solo in ASP.NET MVC versione 3

> Installazione di ASP.NET Web Pages anche installare gli strumenti per Visual Studio, ad esempio i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages.
> 
> **Soluzione alternativa** per usare i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o il [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problema: "&lt;helper&gt; Impossibile trovare la classe" errore

> Dopo l'aggiornamento alla versione Beta 3, si potrebbe essere visualizzato un errore che una classe helper (ad esempio, il `Facebook` classe) non è stato nebyla nalezena. A partire da Beta 2 e continuando nella versione Beta 3, gli helper sono stati spostati ai pacchetti che è necessario installare in modo esplicito. I siti esistenti non vengono aggiornati in modo da includere questi pacchetti; inclusi i siti nel *\My Documents\IISExpress* oppure *\My Documents\My Web* cartelle. In particolare, verrà visualizzato questo errore se si usa il sito predefinito in *siti personali* (WebSite1), che include un riferimento al `Twitter` helper.
> 
> **Soluzione alternativa**  
> Rimuovere i commenti per le chiamate a tutti gli helper del sito, eseguire la  *\_Admin* pagina e installare i pacchetti che includono gli helper che si desidera utilizzare. Dopo aver installato il pacchetto, è possibile rimuovere il commento le righe che fanno riferimento a funzioni di supporto.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problema: La distribuzione di Beta 3 ASP.NET Razor assembly nella cartella Bin potrebbe non funzionare in siti di hosting

> Se si distribuisce un sito Web di ASP.NET Web Pages in un sito di hosting, e se si distribuiscono gli assembly di ASP.NET Razor Beta 3 per il sito *Bin* cartella, si potrebbero riscontrare gli errori, inclusi i seguenti:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Questa situazione può verificarsi se il provider di hosting ha installato l'assembly di ASP.NET Web Pages versione Beta 1 nella cache di applicazione globale del server (GAC). Gli assembly nella Global Assembly Cache ottengono la precedenza su assembly installati in locale nel *Bin* cartella.
> 
> **Soluzione alternativa** contattare il provider di hosting per confermare che gli errori vengono visualizzati sono a causa di un conflitto tra le versioni del provider degli assembly e quelle in uso. In questo caso, chiedere al provider di hosting di aggiornare gli assembly nella Global Assembly Cache del server.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: I feed di lettura o altri dati esterni tramite un server proxy

> Se il server che esegue il sito si trova dietro un server proxy, si potrebbe essere necessario configurare le informazioni sul proxy nel *Web. config* file per poter essere in grado di leggere le informazioni provenienti dall'esterno del sito. Ad esempio, se si usa il `ReCaptcha` helper, l'helper comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato dal server proxy. Feed utilizzati nelle pagine Web ASP.NET, ad esempio il feed usato da Gestione pacchetti in modo analogo, potrebbero richiedere la configurazione del proxy.
> 
> Se si verificano problemi nell'uso di un servizio esterno o l'utilizzo di feed di pacchetti, inserire gli elementi seguenti nella radice dell'applicazione *Web. config* file:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Per altre informazioni sulla configurazione di un server proxy, vedere [ &lt;proxy&gt; (impostazioni di rete)](https://msdn.microsoft.com/library/sa91de1e.aspx) sul sito Web MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problema: Errore "Impossibile caricare Microsoft.Web.Infrastructure.dll"

> Se già installata la versione Beta 1 di ASP.NET Web Pages con sintassi Razor e quindi installare la versione Beta 3, tutti gli assembly appropriati sono installati nella Global Assembly Cache, ad eccezione *Microsoft.Web.Infrastructure.dll*. Di conseguenza, quando si esegue ASP.NET Razor pages, viene visualizzato un errore che indica che *Microsoft.Web.Infrastructure.dll* non può essere caricato.
> 
> Questo problema si verifica se è stato caricato la versione Beta 3 in un computer pulito.
> 
> **Soluzione alternativa**  
> Nel Pannello di controllo disinstallare ASP.NET Web Pages. Reinstallare la versione Beta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Disinstallazione di .NET Framework versione 4 Disabilita ASP.NET Web Pages con sintassi Razor

> Se si disinstalla .NET Framework versione 4 e quindi reinstallarlo, ASP.NET Web Pages con sintassi Razor è disabilitato. Le pagine con le *cshtml* estensioni non vengono eseguite correttamente. ASP.NET Web Pages registra un assembly nella directory radice macchina *Web. config* file e rimozione di .NET Framework consente di rimuovere tale file. Reinstallare .NET Framework viene installata una versione nuova del file di configurazione, ma non aggiunge il riferimento per l'assembly di ASP.NET Web Pages.
> 
> **Soluzione alternativa** dopo la reinstallazione di .NET Framework, reinstallare ASP.NET Web Pages con sintassi Razor. Verrà aggiunto l'elemento seguente per il *Web. config* file nella radice del computer, ovvero in genere nel percorso seguente:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problema: Le applicazioni distribuite in precedenza con ASP.NET gli assembly nella cartella Bin del verificano di errori

> Durante la distribuzione, copie degli assembly di ASP.NET Web Pages (ad esempio, *Microsoft.WebPages.dll*) per il *Bin* cartella del sito Web nel server. (Ciò si sono verificati automaticamente durante la distribuzione o perché lo sviluppatore copiati in modo esplicito gli assembly.) Tuttavia, quando viene installata la versione Beta 3, gli errori si verifica, ad esempio gli errori che non è possibile trovare alcuni tipi. Ciò si verifica perché un numero di tipi di pagine Web ASP.NET sono stato spostato in diversi spazi dei nomi per la versione Beta 3.
> 
> **Soluzione alternativa**   
> Cancella il *Bin* cartella dell'applicazione distribuita, copiare i nuovi assembly nella cartella (o ridistribuire l'applicazione), quindi riavviare l'applicazione.


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
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problema: Utilizzando le pagine Web ASP.NET e ASP.NET MVC o Web Application Project nella stessa applicazione

> Se si usa ASP.NET Web Pages in un'applicazione ASP.NET MVC o il progetto di applicazione Web, si potrebbe essere visualizzato un errore che *WebPageHttpApplication* nebyla nalezena.
> 
> **Soluzione alternativa**  
> Se si verifica questo errore, modificare la classe di base da cui deriva l'applicazione. Nel *Global. asax* file, modificare la riga seguente:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> A questo scopo:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Ciò in effetti Annulla una modifica che è stata introdotta per la versione Beta 1 di ASP.NET Web Pages con sintassi Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Distribuire un'applicazione in un computer che non dispone di SQL Server Compact è installato

> Le applicazioni che includono i database di SQL Server Compact possono eseguire in un computer in cui SQL Server Compact non è installato. Microsoft WebMatrix Beta 3 automaticamente consente di copiare tali file binari per l'utente ed esegue l'appropriato *Web. config* trasformazioni di file.
> 
> **Soluzione alternativa** se è necessario copiare tali file e apportare le *Web. config* modifiche al file manualmente, eseguire le operazioni seguenti:
> 
> 1. Copiare gli assembly del motore di database per il *Bin* cartella (e sottocartelle) dell'applicazione nel computer di destinazione: 
> 
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **al** *\Bin*
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **alla** *\Bin\x86*
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **alla** *\Bin\amd64*
> 2. Nella cartella radice del sito Web, creare o aprire una *Web. config* file. (In WebMatrix Beta 3, questo tipo di file è disponibile se si fa clic **tutte** nel **scegliere un tipo di File** nella finestra di dialogo.)
> 3. Aggiungere l'elemento seguente come elemento figlio il **&lt;configuration&gt;** elemento (non all'interno di **&lt;System. Web&gt;** elemento):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: Database e WebGrid helper non funzionano in Medium Trust in Visual Basic

> Se si usa Visual Basic (creando *vbhtml* file), il `Database` e `WebGrid` helper non funzionerà se l'applicazione è impostata per usare Medium Trust.
> 
> **Soluzione alternativa**  
> Impostare temporaneamente l'applicazione per usare l'attendibilità.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problema: La proprietà "Encrypt" non è riconosciuta

> SQL Server Compact 4.0 non riconosce i `Encrypt` proprietà del `SqlCeConnection` classe. Utilizzare questa proprietà non crittografare i file di database. Il `Encrypt` proprietà è stata deprecata nella versione 3.5 di SQL Server Compact ed è stata mantenuta solo per compatibilità con le versioni precedenti. 
> 
> **Soluzione alternativa**  
> Usare la `Encryption Mode` proprietà del `SqlCeConnection` classe per crittografare i file di database di SQL Server Compact 4.0. Nell'esempio seguente viene illustrato come creare un database di SQL Server Compact 4.0 crittografate tramite il `Encryption Mode` proprietà:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Per modificare la modalità di crittografia di un database di SQL Server Compact 4.0 esistente, eseguire le operazioni seguenti:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Per crittografare un database di SQL Server Compact 4.0 non crittografato, eseguire le operazioni seguenti:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problema: Sono necessarie le librerie di runtime di Microsoft Visual C++ 2008

> Il nativa le DLL di SQL Server Compact 4.0 necessario di Microsoft Visual C++ 2008 librerie di Runtime (x86, IA64 e x64), Service Pack 1.
> 
> **Soluzione alternativa**  
> Installare .NET Framework 3.5 SP1. Viene inoltre installato Visual C++ 2008 Runtime librerie SP1. È possibile scaricare le librerie dal percorso seguente:   
>   
> [Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Si noti che l'installazione di .NET Framework 2.0, 3.0, o 4 li *non* installare Visual C++ 2008 Runtime librerie SP1.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problema: Se è installato SQL Server Compact prima di installare .NET Framework nel computer, il nome invariante del provider non è registrato nel file Machine. config di .NET Framework

> SQL Server Compact può essere installato in un computer che non dispone di .NET Framework installata perché SQL Server Compact richiede .NET framework. Se .NET Framework versione 3.5 né 4 è installato prima di installare SQL Server Compact, l'installazione di SQL Server Compact non registra il nome invariante del provider nel *Machine. config* file. Qualsiasi applicazione che si basa sulla voce in SQL Server Compact il *Machine. config* file avrà esito negativo. La voce di registrazione nome invariante nelle *Machine. config* aspetto simile al seguente:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Soluzione alternativa**  
> Disinstallare SQL Server Compact 4.0 CTP1. Scaricare e installare le versioni complete di .NET Framework dal percorso seguente:
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (pacchetto completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Versione di Microsoft .NET Framework 4.0 (pacchetto completo)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Quindi si reinstalla [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Installazione di applicazioni

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: Installazione di un'applicazione può richiedere molto tempo se cartella documenti dell'utente viene reindirizzata a una condivisione di rete

> **Soluzione alternativa**  
> Nessuno. L'applicazione potrebbe richiedere del tempo per l'installazione, ma verrà installato correttamente.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Pubblicazione di applicazioni

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Altri problemi

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problema: / Filtro di ricerca non funziona nei report di Group By: tipo di problema

> Quando si esegue un report per un sito, se si immette testo nella *Filtra per URL* casella e fare clic su *ricerca*, non accade nulla. Infatti, questo controllo non funziona durante la *Group By* stato del report è impostato su *tipo di problema*, ovvero l'impostazione predefinita.
> 
> **Soluzione alternativa** nella *Group By* della scheda della barra multifunzione, fare clic su *URL* per raggruppare le voci per i rispettivi URL di origine. La casella di testo e il pulsante per filtrare le voci sono funzionale durante questo stato.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problema: Le applicazioni WCF non è possibile eseguire con IIS Express

> Passare a un'applicazione WCF genera un errore simile a quello seguente:
> 
> *Impossibile caricare il file o l'assembly ' Microsoft.Web.Administration, versione = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze. Il sistema non riesce a trovare il file specificato.*
> 
> Ciò si verifica perché WCF non supporta il versione Beta di IIS Express per impostazione predefinita.
> 
> **Soluzione alternativa** usare una qualsiasi delle seguenti soluzioni alternative (soluzione alternativa 2 # richiede Microsoft Windows Vista o versioni successive):
> 
> 
> 1. Copia il *Microsoft.Web.dll* e *Administration* assembly dal percorso di installazione di WebMatrix per il *bin* directory di WCF applicazione. Per impostazione predefinita, WebMatrix viene installato nel *Microsoft WebMatrix* sottocartella del sistema *i file di programma* cartella.
> 2. In Microsoft Windows Vista o versione successiva, creare un collegamento simbolico per gli assembly di *bin* directory usando i comandi seguenti. (Questo approccio ha il vantaggio che non crea una copia degli assembly).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Installare i due assembly nella Global Assembly Cache. Da un prompt dei comandi con privilegi elevati, eseguire i comandi seguenti:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: nella versione WebMatrix Beta 3 non riesce a eseguire determinate attività che richiedono l'elevazione dei privilegi

> WebMatrix Beta 3 è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:
> 
> - In Windows Vista o Windows 7, si è connessi con un account che dispone di privilegi amministrativi e controllo Account utente (UAC) è disabilitato.
> - Si utilizza Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Soluzione alternativa**  
> La maggior parte delle attività nella versione Beta 3 di WebMatrix non richiedono autorizzazioni amministrative. Per questi, è possibile eseguire l'operazione perché un amministratore o seguire questa procedura:
> 
> - In Windows Vista o Windows 7, abilitare la UAC.
> - In Windows XP, aggiungere l'utente al gruppo di sicurezza Administrators.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "Del sito da Web Gallery" è disabilitato

> Il **sito da Web Gallery** opzione è disabilitata se l'installazione guidata piattaforma Web 3.0 non è installato.
> 
> **Soluzione alternativa**  
> Installare il [installazione guidata piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problema: In Windows Server 2003, IIS Express non viene avviato per un utente senza privilegi di amministratore

> In Windows Server 2003, quando si avvia una pagina o l'avvio di IIS Express, IIS Express non viene avviato. Per le pagine Web, viene visualizzato un errore che indica che l'applicazione sia stata avviata da un utente senza privilegi di amministratore.
> 
> **Soluzione alternativa**  
> Avviare WebMatrix Beta 3 come un utente amministratore. Per altre informazioni, vedere l'articolo della Knowledge Base seguente:  
>   
> [Può restare in attesa di un'applicazione che viene avviata da un utente senza privilegi di amministratore per il traffico HTTP del computer in cui l'applicazione è in esecuzione in Windows Vista, Windows Server 2003 o Windows XP.](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>Problema: Il pulsante "Relazioni" è disabilitato

> Il **relazioni** pulsante sotto il **tabella** scheda il **database** dell'area di lavoro è disabilitato per i database di SQL Server Compact.
> 
> **Soluzione alternativa**  
> Nessuno. SQL Server Compact non supporta le relazioni tra tabelle.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problema: Le query SQL con parametri vengono generate eccezioni

> In SQL Server Compact 4.0, se non si specifica un tipo di dati, ad esempio `SqlDbType` o `DbType` per i parametri nelle query con parametri, viene generata un'eccezione quando viene eseguita la query.
> 
> **Soluzione alternativa**  
> Impostare in modo esplicito il tipo di dati per i parametri, ad esempio `SqlDbType` o `DbType`. Questo aspetto è fondamentale nel caso di tipi di dati BLOB (`image` e `ntext`). Usare codice simile al seguente:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Ulteriori informazioni

Per ulteriori informazioni su WebMatrix Beta 3, vedere i siti Web seguenti:

- [IIS.net](http://iis.net/)
- [ASP.NET 2.0](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Tutti i diritti riservati. [Le condizioni d'uso](https://msdn.microsoft.cos/cc300389.aspx).
