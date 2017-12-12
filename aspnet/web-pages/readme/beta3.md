---
uid: web-pages/readme/beta3
title: Web matrice e il file Leggimi versione Beta 3 di ASP.NET Web Pages (Razor) | Documenti Microsoft
author: rick-anderson
description: Una matrice e file Leggimi versione Beta 3 di Web ASP.NET (Razor) pagine Web
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5fad4b659dafe5470aeb84d320ff711b8840d1e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Una matrice e file Leggimi versione Beta 3 di Web ASP.NET (Razor) pagine Web
====================
> Una matrice e file Leggimi versione Beta 3 di Web ASP.NET (Razor) pagine Web

9 novembre 2010

## <a name="contents"></a>Contenuto

- [Panoramica](#Overview)
- [Installazione](#Installation_Notes)
- [Nuove funzionalità, le modifiche e problemi noti nella versione Beta 3](#Known_Issues)

    - [Problemi di installazione di WebMatrix](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [L'installazione di applicazioni](#Known_Issues_Installing_Applications)
    - [Pubblicazione di applicazioni](#Known_Issues_Publishing_Applications)
    - [Altri problemi](#Known_Issues_Other_Issues)
- [Per ulteriori informazioni](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Panoramica

> Microsoft WebMatrix Beta è uno stack di sviluppo web gratuito che installa in pochi minuti. Si integra un server web con il database e programmazione Framework per creare un'esperienza unica e integrata. È possibile utilizzare una versione Beta di WebMatrix per ottimizzare le attività di codice, testare e pubblicare il proprio sito Web ASP.NET o PHP oppure è possibile utilizzare una versione Beta di WebMatrix per avviare un nuovo sito Web utilizzando più comuni App open source come DotNetNuke, Umbraco, WordPress o Joomla. Versione Beta di WebMatrix utilizza stesso server web potente, motore di database e ambiente di Framework che verrà eseguito il sito Web in internet, che effettua la transizione dallo sviluppo alla produzione modo diretto e immediato.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installazione

> Per installare WebMatrix Beta 3, utilizzare [programma di installazione di piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Dopo aver installato l'installazione guidata piattaforma Web, è possibile utilizzare, per installare una versione Beta 3 di WebMatrix.
> 
> Se si verificano problemi durante l'installazione, fare riferimento a [risoluzione dei problemi con l'installazione guidata piattaforma Web di Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Istruzioni per la pubblicazione di applicazioni

> Vedere [istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nuove funzionalità, le modifiche, andKnown problemi

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Installazione di WebMatrix Beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix Beta 3 è disponibile solo su piattaforme che supportano Microsoft .NET Framework 4

> .NET Framework versione 4 è obbligatorio per la versione Beta di WebMatrix. In alcuni casi, il programma di installazione di WebMatrix Beta consente di provare a installare su una piattaforma che non fa parte del set di configurazione supportata. In particolare, Windows Vista senza l'aggiornamento SP1 consentono di iniziare l'installazione della versione Beta di WebMatrix, ma il componente di .NET Framework 4 avrà esito negativo e bloccare l'installazione.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: Impossibile installare WebMatrix Beta 3 se è installato Microsoft Visual Studio 2008 senza Microsoft Visual Studio 2008 SP1

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

In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e problemi noti con la versione Beta 3 di ASP.NET Web Pages con sintassi Razor.

- [Nuove funzionalità](#NewFeatures)
- [Modifiche](#Changes)
- [Problemi](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nuove funzionalità nella versione Beta 3 per ASP.NET Web Pages con sintassi Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nuovo: "Html.Raw" metodo esegue il rendering di markup non codificato

> Il nuovo `Html.Raw` metodo consente di eseguire il rendering di markup HTML come markup anziché il rendering di output codificata. (Per impostazione predefinita, ASP.NET Razor codifica le stringhe prima di essere.) La sintassi è:
> 
> `Html.Raw(value)`
> 
> Nell'esempio riportato di seguito viene illustrato come usare `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Modifiche nella versione Beta 3 per ASP.NET Web Pages con sintassi Razor

#### <a name="change-hrefattribute-method-removed"></a>Modifica: Metodo "HrefAttribute" rimosso

> Il `HrefAttribute` metodo la `WebPage` classe è stata rimossa. Questo supporto è stato usato per codificare i caratteri non sicuri in URL. Non è più necessario perché ASP.NET Razor codifica automaticamente le stringhe. (Usare il nuovo `Html.Raw` metodo per eseguire il rendering delle stringhe non codificate.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Modifica: Sintassi dichiarativa "@helper" helper modificato

> Nella versione Beta 3, ASP.NET viene modificato l'analisi helper che vengono creati utilizzando il `@helper` sintassi. In pratica, il `@helper` sintassi ora viene analizzata come un blocco di codice anziché come un blocco di codice che può contenere codice. Pertanto, il codice all'interno dell'helper non è necessario essere racchiuso tra parentesi `@{ }` blocchi. Al contrario, deve essere incluso in modo esplicito gli elementi HTML o ASP.NET Razor markup all'interno dell'helper `<text></text>` tag.
> 
> Ad esempio, `@helper` funziona nella versione Beta 3 di sintassi:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Nella versione Beta 3, è necessario modificare questo helper per simile all'esempio seguente:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Si noti che il `@{ }` caratteri intorno al codice iniziale nell'helper non viene più utilizzato. Questo avviene perché il contenuto degli helper viene considerato come un blocco di codice per impostazione predefinita. L'helper esegue il rendering di markup, che inizia con l'apertura `<a>` tag. Se l'helper necessario eseguire il rendering in testo normale o tag che non includono un tag di chiusura (ad esempio, `<meta>` tag), il contenuto da sottoporre a rendering deve essere `<text></text>` tag.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Modifica: Rimuovere "WebPageContext.HttpContext"

> Il `WebPageContext.HttpContext` proprietà è stata rimossa. In alternativa, usare `HttpContext.Current` . (Il `WebPageContext.HttpContext` proprietà semplicemente incapsulati.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Modifica: Supporto "Facebook" spostato di nuovo pacchetto

> Il `Facebook` supporto è stato spostato il *Facebook.Helper* libreria, che include il `Facebook` helper e funzionalità aggiuntive. È necessario installare questa libreria come pacchetto separato, come descritto in "Installare helper con Gestione pacchetti" nell'esercitazione [Guida introduttiva a pagine ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>: Modifica L'appartenenza al ruolo e sicurezza tipi passa al nuovo assembly

> Sono stati spostati i seguenti tipi di `WebMatrix.WebData` assembly:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Modifica: Classe "TagBuilder" Sposta all'assembly System.Web.WebPages.dll

> La `TagBuilde` classe r è stato spostato all'assembly System.Web.WebPages.dll. In precedenza, era in un assembly che fa parte di ASP.NET MVC. Ciò significa che non è necessario installare ASP.NET MVC per utilizzare il `TagBuilder` classe.
> 
> Tuttavia, la classe è ancora nel `System.Web.Mvc` dello spazio dei nomi. Per utilizzare il `TagBuilder` classe (ad esempio, in un supporto ASP.NET Razor personalizzato), è necessario fare riferimento lo spazio dei nomi (ad esempio, aggiungendo `@using System.Web.Mvc` al codice).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Modifica: Sintassi di convalida modificata; di richiesta Classe "Convalida" rimosso

> Nella versione Beta 3, per disattivare la convalida per un singolo campo o un set di campi, è possibile chiamare il `Validation.Exclude` , passando il nome o i nomi dei campi da escludere dalla convalida. Una nuova sintassi è disponibile nella versione Beta 3 per ignorare la convalida. Il `Validation` metodo usato nella versione Beta 3 è stato rimosso.
> 
> > [!NOTE]
> > Se non si disabilita la convalida della richiesta, se gli utenti tentano di caricare il markup HTML (ad esempio, utilizzando un editor di testo RTF in una pagina), il sito Web verrà segnalato un errore di *valore potenzialmente pericoloso Request. Form è stato rilevato dal client*e non è stato accettato l'input dell'utente. Se si disabilita la convalida della richiesta, è necessario archiviare manualmente input dell'utente per assicurarsi che non contengono il markup potenzialmente pericoloso o uno script simile utilizzando il [Microsoft Anti-Cross Site Scripting Library v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Per disabilitare la convalida automatica della richiesta, chiamare il `Request.Unvalidated` metodo, passando il nome del campo o un altro oggetto post che si desidera ignorare la convalida delle richieste per. È possibile utilizzare questo metodo per ignorare la convalida per gli elementi di `Form`, `QueryString`, `Cookies`, e `ServerVariables` raccolte. Nell'esempio seguente viene illustrato come utilizzare il `Unvalidated` metodo:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problemi noti per ASP.NET Web Pages con sintassi Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: Un comportamento imprevisto quando si utilizza una tabella utente personalizzato per l'appartenenza

> Per inizializzare il provider di appartenenze per un sito Web ASP.NET Razor, si chiama il `WebSecurity.InitializeDatabaseConnection` metodo. (In WebMatrix, il modello Starter Site include una chiamata al metodo nel  *\_AppStart.cshtml* file.) Se il `autoCreateTables` parametro di questo metodo è impostato su true (per impostazione predefinita, viene impostata su true nel modello Starter Site), e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera un errore. Al contrario, viene creato automaticamente nella tabella.
> 
> Può trattarsi di un problema, se si prevede di utilizzare una tabella utente personalizzata per l'appartenenza ma passare il nome della tabella non corretto per il `WebSecurity.InitializeDatabaseConnection` metodo. Poiché il metodo per impostazione predefinita genera un errore se la tabella specificata non esiste, e invece creata una nuova tabella, può sembrare che l'applicazione si sta lavorando. Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui campi in essa contenuti) può avere esito negativo con errori imprevisti.
> 
> **Soluzione alternativa**  
> Assicurarsi che il nome passato il `InitializeDatabaseConnection` corrispondenze metodo tabella nel database delle appartenenze, il profilo utente o assicurarsi che il `autoCreateTables` parametro è impostato su false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Errore: problema "Impossibile generare un'istanza utente di SQL Server"

> Se un'applicazione Web di WebMatrix utilizza SQL Server Express è in esecuzione IIS 7.5 in Windows 7 o Windows Server 2008 R2, si potrebbe essere visualizzato un errore che indica che SQL Server non è possibile recuperare il percorso dell'applicazione locale dell'utente in fase di esecuzione.
> 
> **Soluzione alternativa** assicurarsi che l'account di Windows a cui viene eseguito l'applicazione (in genere un servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *App\_dati*. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge Base [problemi istanze utente di SQL Server Express e progetti di applicazione Web ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problema: In Visual Studio, gli spazi dei nomi per gli assembly personalizzati (DLL) non vengono importati automaticamente

> Se si utilizzano assembly personalizzati in un progetto in Visual Studio, gli spazi dei nomi dichiarati in tali assembly non vengono importati automaticamente in fase di progettazione. Di conseguenza, i riferimenti a tipi personalizzati potrebbero non essere riconosciuti in fase di progettazione e sono contrassegnati come non riconosciuto in Visual Studio (con una sottolineatura di""). Questo problema si verifica solo in fase di progettazione in Visual Studio. l'applicazione venga eseguita correttamente.
> 
> **Soluzione alternativa**  
> Includere un `using` istruzione (`imports` in Visual Basic) che fa riferimento l'entità che non sono riconosciute in fase di progettazione.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: Visual Studio IntelliSense e progetto modelli disponibili solo in ASP.NET MVC versione 3

> L'installazione di ASP.NET Web Pages non installare anche gli strumenti per Visual Studio, ad esempio i modelli di progetto e IntelliSense per le applicazioni ASP.NET Web Pages.
> 
> **Soluzione alternativa** per utilizzare i modelli di progetto e IntelliSense per le applicazioni di pagine Web ASP.NET in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problema: "&lt;helper&gt; Impossibile trovare la classe" errore

> Dopo l'aggiornamento a Beta 3, si potrebbe essere visualizzato un errore che una classe helper (ad esempio, la `Facebook` classe) non può non essere trovato. A partire da Beta 2 e continuando nella versione Beta 3, helper sono stati spostati in modo esplicito, è necessario installare i pacchetti. I siti esistenti non vengono aggiornati per includere questi pacchetti; sono inclusi i siti di *\My Documents\IISExpress* o *\My Documents\My Web* cartelle. In particolare, verrà visualizzato questo errore se si utilizza il sito predefinito in *siti personali* (WebSite1), che include un riferimento di `Twitter` helper.
> 
> **Soluzione alternativa**  
> Impostare come commento le chiamate a qualsiasi helper del sito, eseguire il  *\_Admin* pagina e installare i pacchetti che includono gli helper che si desidera utilizzare. Dopo aver installato il pacchetto, è possibile rimuovere il commento le righe che fanno riferimento gli helper.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problema: La distribuzione di Beta 3 ASP.NET Razor assembly nella cartella Bin potrebbero non funzionare in siti

> Se si distribuisce un sito Web ASP.NET Web Pages in un sito di hosting e, se si distribuiscono gli assembly ASP.NET Razor Beta 3 per il sito *Bin* cartella, potrebbero verificarsi errori, inclusi i seguenti:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Questa situazione può verificarsi se il provider di hosting è installato l'assembly di ASP.NET Web Pages versione Beta 1 nella cache globale dell'applicazione del server (GAC). Gli assembly nella GAC ottenere la precedenza rispetto agli assembly installati localmente nel *Bin* cartella.
> 
> **Soluzione alternativa** contattare il provider di hosting per confermare che gli errori visualizzati sono a causa di un conflitto tra le versioni del provider degli assembly e quelle in uso. In questo caso, chiedere al provider di hosting di aggiornare gli assembly nella Global Assembly Cache del server.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: Lettura feed o altri dati esterni tramite un server proxy

> Se il server che esegue il sito è un server proxy, potrebbe essere necessario configurare le informazioni sul proxy nel *Web. config* file per poter essere in grado di leggere informazioni proveniente dall'esterno del sito. Ad esempio, se si utilizza il `ReCaptcha` helper, il supporto comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato da server proxy. Analogamente, feed che vengono utilizzati in ASP.NET Web Pages, ad esempio il feed utilizzato dal gestore del pacchetto, potrebbe richiedere la configurazione del proxy.
> 
> Se si verificano problemi nell'utilizzo di un servizio esterno o l'utilizzo con il feed del pacchetto, è possibile inserire gli elementi seguenti nella directory principale dell'applicazione *Web. config* file:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Per ulteriori informazioni sulla configurazione di un server proxy, vedere [ &lt;proxy&gt; elemento (impostazioni di rete)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) nel sito Web MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problema: Errore "Impossibile caricare Microsoft.Web.Infrastructure.dll"

> Se è installato in precedenza la versione Beta 1 di ASP.NET Web Pages con sintassi Razor e quindi installare la versione Beta 3, tutti gli assembly appropriati vengono installati nella Global Assembly Cache, ad eccezione *Microsoft.Web.Infrastructure.dll*. Di conseguenza, quando si eseguono ASP.NET Razor pagine, viene visualizzato un errore che indica che *Microsoft.Web.Infrastructure.dll* non può essere caricato.
> 
> Questo problema si verifica se è stata caricata la versione Beta 3 in un computer pulito.
> 
> **Soluzione alternativa**  
> Nel Pannello di controllo, disinstallare ASP.NET Web Pages. Reinstallare la versione Beta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: Disinstallazione di .NET Framework versione 4 Disabilita ASP.NET Web Pages con sintassi Razor

> Se si disinstalla .NET Framework versione 4 e quindi reinstallarlo, ASP.NET Web Pages con sintassi Razor è disabilitato. Pagine con il *. cshtml* estensione non vengono eseguite correttamente. ASP.NET Web Pages registra un assembly nella directory principale macchina *Web. config* file e la rimozione di .NET Framework consente di rimuovere tale file. Reinstallare .NET Framework viene installata una nuova versione del file di configurazione, ma non aggiunge il riferimento per l'assembly di ASP.NET Web Pages.
> 
> **Soluzione alternativa** dopo la reinstallazione di .NET Framework, reinstallare ASP.NET Web Pages con sintassi Razor. Aggiunge l'elemento seguente per il *Web. config* file nella radice della macchina, ovvero in genere nel percorso seguente:  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
>   
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problema: Le applicazioni distribuite in precedenza con gli assembly ASP.NET nella cartella Bin si verifichino errori

> Durante la distribuzione, copie degli assembly ASP.NET Web Pages (ad esempio, *Microsoft.WebPages.dll*) per il *Bin* cartella del sito Web nel server. (Ciò potrebbe essersi verificato automaticamente durante la distribuzione o perché lo sviluppatore copiati in modo esplicito gli assembly.) Tuttavia, quando si installa la versione Beta 3, errori si verifica, ad esempio gli errori che non è possibile trovare alcuni tipi. Questo errore si verifica perché un numero di tipi di pagine Web ASP.NET sono stato spostato in diversi spazi dei nomi per la versione Beta 3.
> 
> **Soluzione alternativa**   
> Cancella il *Bin* cartella dell'applicazione distribuita, copiare i nuovi assembly nella cartella o distribuire nuovamente l'applicazione, quindi riavviare l'applicazione.


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
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problema: Utilizzando il progetto di applicazione Web o le pagine Web ASP.NET e MVC ASP.NET nella stessa applicazione

> Se si utilizzano pagine Web ASP.NET in un progetto di applicazione Web o l'applicazione ASP.NET MVC, si potrebbe essere visualizzato un errore che *WebPageHttpApplication* non è stato trovato.
> 
> **Soluzione alternativa**  
> Se si verifica questo errore, modificare la classe base da cui deriva l'applicazione. Nel *Global. asax* file, modificare la riga seguente:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> a questo scopo:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> In effetti viene invertito una modifica che è stato introdotto per la versione Beta 1 di ASP.NET Web Pages con sintassi Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: Distribuire un'applicazione in un computer che non dispone di SQL Server Compact è installato

> Le applicazioni che includono database di SQL Server Compact è possono eseguire in un computer in cui SQL Server Compact non è installato. Microsoft WebMatrix Beta 3 automaticamente copia i file binari per l'utente ed esegue appropriata *Web. config* file trasformazioni.
> 
> **Soluzione alternativa** se è necessario copiare questi file, assicurarsi di *Web. config* modifiche al file manualmente, eseguire le operazioni seguenti:
> 
> 1. Copiare gli assembly del motore di database per il *Bin* (cartelle e sottocartelle) dell'applicazione nel computer di destinazione: 
> 
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **a** *\bin.*
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **a** *\Bin\x86*
>     - Copia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **a** *\Bin\amd64*
> 2. Nella cartella radice del sito Web, creare o aprire un *Web. config* file. (Nella versione Beta 3 di WebMatrix, è disponibile se si fa clic su questo tipo di file **tutti** nel **scegliere un tipo di File** la finestra di dialogo.)
> 3. Aggiungere il seguente elemento come figlio del  **&lt;configurazione&gt;**  elemento (non all'interno di  **&lt;System. Web&gt;**  elemento):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: Gli helper di Database e WebGrid non funzionano in attendibilità media in Visual Basic

> Se si utilizza Visual Basic (creazione *. vbhtml* file), il `Database` e `WebGrid` helper non funzionerà se l'applicazione è impostato per utilizzare l'attendibilità Media.
> 
> **Soluzione alternativa**  
> Impostare temporaneamente l'applicazione di usare l'attendibilità totale.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problema: La proprietà "Encrypt" non riconosciuta.

> SQL Server Compact 4.0 non riconosce il `Encrypt` proprietà la `SqlCeConnection` classe. Utilizzare questa proprietà non di crittografare i file di database. Il `Encrypt` proprietà è stata deprecata nella versione 3.5 di SQL Server Compact ed è stata mantenuta solo per compatibilità con le versioni precedenti. 
> 
> **Soluzione alternativa**  
> Utilizzare il `Encryption Mode` proprietà la `SqlCeConnection` classe per crittografare i file di database di SQL Server Compact 4.0. Nell'esempio seguente viene illustrato come creare un database di SQL Server Compact 4.0 crittografati con la `Encryption Mode` proprietà:
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
> Per crittografare un database di SQL Server Compact 4.0 non crittografato, procedere come segue:
>  
> [!code-csharp[Main](beta3/samples/sample15.cs)]
>  
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problema: Sono necessarie le librerie di runtime di Microsoft Visual C++ 2008

> Il native DLL di SQL Server Compact 4.0 è necessario di Microsoft Visual C++ 2008 librerie di Runtime (x86, IA64 e x64), Service Pack 1.
> 
> **Soluzione alternativa**  
> Installare .NET Framework 3.5 SP1. Viene inoltre installato Visual C++ 2008 Runtime librerie SP1. È possibile scaricare le librerie dal percorso seguente:   
>   
> [Aggiornamento della sicurezza per Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Si noti che l'installazione di .NET Framework 2.0, 3.0, o 4 non *non* installare Visual C++ 2008 Runtime librerie SP1.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problema: Se è installato SQL Server Compact prima di installare .NET Framework nel computer, il nome invariante del provider non è registrato nel file Machine. config di .NET Framework

> SQL Server Compact può essere installato in un computer che non dispone di .NET Framework installata perché SQL Server Compact richiede .NET framework. Se .NET Framework versione 3.5 né 4 è installato prima di installare SQL Server Compact, l'installazione di SQL Server Compact non registra il nome invariante del provider nel *Machine. config* file. Qualsiasi applicazione che si basa sulla voce in SQL Server Compact di *Machine. config* file avrà esito negativo. La voce di registrazione del nome non variante in *Machine. config* aspetto nell'esempio seguente:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Soluzione alternativa**  
> Disinstallare SQL Server Compact 4.0 CTP1. Scaricare e installare le versioni di .NET Framework complete dal percorso seguente:
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (pacchetto completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Versione di Microsoft .NET Framework 4.0 (pacchetto completo)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Reinstallare [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>L'installazione di applicazioni

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: L'installazione di un'applicazione può richiedere molto tempo se cartella documenti dell'utente viene reindirizzata a una condivisione di rete

> **Soluzione alternativa**  
> Nessuno. L'applicazione potrebbe richiedere qualche istante per l'installazione, ma verrà installato correttamente.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Pubblicazione di applicazioni

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Altri problemi

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problema: Il filtro di ricerca o non funziona nei report per Group By: tipo di problema

> Quando si esegue un report per un sito, se si immette testo nella *filtro dall'URL* casella e fare clic su *ricerca*, non accade nulla. Infatti, questo controllo non è funzionale il *Group By* stato del report è impostato su *tipo di problema*, ovvero il valore predefinito.
> 
> **Soluzione alternativa** nel *Group By* scheda della barra multifunzione, fare clic su *URL* per raggruppare le voci per l'URL di origine. La casella di testo e un pulsante per filtrare le voci sono funzionale in questo stato.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problema: Le applicazioni WCF non è possibile eseguire con IIS Express

> La selezione di un'applicazione WCF genera un errore simile a quello seguente:
> 
> *Impossibile caricare il file o l'assembly ' Microsoft.Web.Administration, versione = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle sue dipendenze. Il sistema non riesce a trovare il file specificato.*
> 
> Questo errore si verifica poiché WCF non supporta la versione Beta di IIS Express per impostazione predefinita.
> 
> **Soluzione alternativa** utilizzare una delle seguenti soluzioni alternative (soluzione alternativa &#2; richiede Microsoft Windows Vista o versione successiva):
> 
> 
> 1. Copia il *Microsoft.Web.dll* e *Microsoft.Web.Administration.dll* assembly dal percorso di installazione di WebMatrix per la *bin* directory di WCF applicazione. Per impostazione predefinita, WebMatrix è installato nel *Microsoft WebMatrix* sottocartella il sistema *programmi* cartella.
> 2. In Microsoft Windows Vista o versioni successive, creare un collegamento simbolico per gli assembly di *bin* directory utilizzando i comandi seguenti. (Questo approccio ha il vantaggio che non crea una copia degli assembly).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Installare due assembly nella GAC. Da un prompt dei comandi con privilegi elevati, eseguire i comandi seguenti:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix Beta 3 è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi

> WebMatrix Beta 3 è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:
> 
> - In Windows Vista o Windows 7, si è connessi con un account che dispone di privilegi amministrativi e il controllo dell'Account utente (UAC) è disabilitata.
> - Si utilizza Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Soluzione alternativa**  
> La maggior parte delle attività nella versione Beta 3 di WebMatrix non richiedono autorizzazioni amministrative. Se sono presenti, è possibile eseguire l'operazione come amministratore o eseguire la procedura seguente:
> 
> - In Windows Vista o Windows 7, abilitare il controllo dell'account utente.
> - In Windows XP, aggiungere l'utente al gruppo di sicurezza Administrators.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "Del sito dalla raccolta di Web" è disabilitata

> Il **sito dalla raccolta Web** opzione è disabilitata se l'installazione guidata piattaforma Web 3.0 non è installato.
> 
> **Soluzione alternativa**  
> Installare il [installazione guidata piattaforma Web Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problema: In Windows Server 2003, IIS Express non viene avviato per un utente non amministratore

> In Windows Server 2003, quando una pagina di avvio o l'avvio di IIS Express, IIS Express non viene avviato. Per le pagine Web, viene visualizzato un errore che indica che l'applicazione è stata avviata da un utente non amministratore.
> 
> **Soluzione alternativa**  
> Avviare WebMatrix Beta 3 come utente con privilegi amministrativi. Per ulteriori informazioni, vedere l'articolo della Knowledge Base seguente:  
>   
> [Un'applicazione che viene avviata da un utente non amministrativi non può essere in ascolto per il traffico HTTP del computer in cui l'applicazione è in esecuzione in Windows Vista, Windows Server 2003 o Windows XP.](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>Problema: Il pulsante "Relazioni" è disabilitato

> Il **relazioni** in corrispondenza di **tabella** nella scheda il **database** dell'area di lavoro è disabilitato per i database di SQL Server Compact.
> 
> **Soluzione alternativa**  
> Nessuno. SQL Server Compact non supporta le relazioni tra tabelle.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problema: Le query SQL con parametri generano eccezioni

> In SQL Server Compact 4.0, se non si specifica un tipo di dati, ad esempio `SqlDbType` o `DbType` per i parametri nelle query con parametri, viene generata un'eccezione quando viene eseguita la query.
> 
> **Soluzione alternativa**  
> Impostare in modo esplicito il tipo di dati per i parametri, ad esempio `SqlDbType` o `DbType`. Ciò è fondamentale nel caso di tipi di dati BLOB (`image` e `ntext`). Utilizzare codice simile al seguente:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
>  
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Ulteriori informazioni

Per ulteriori informazioni su WebMatrix Beta 3, vedere i siti Web seguenti:

- [IIS.net](http://iis.net/)
- [ASP.NET 2.0](https://asp.net/webmatrix)
- [Microsoft.com](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Tutti i diritti riservati. [Condizioni di utilizzo](https://msdn.microsoft.com/en-us/cc300389.aspx).
