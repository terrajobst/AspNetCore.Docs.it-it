---
title: Errori comuni di Servizio app di Azure e IIS con ASP.NET Core
author: guardrex
description: Riconoscere gli errori comuni dell'hosting di app ASP.NET Core in Servizio app di Azure e IIS.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 30b7f1d8e1cfdfd3d1db865ff428eb2094a84d32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277319"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Errori comuni di Servizio app di Azure e IIS con ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

L'elenco di errori che segue non è completo. Qualora si riscontrasse un errore non è elencato qui, [aprire un nuovo problema](https://github.com/aspnet/Docs/issues/new) con istruzioni dettagliate per la riproduzione dell'errore.

Raccogliere le seguenti informazioni:

* Comportamento del browser
* Voci del log eventi dell'applicazione
* Voci del log stdout del modulo ASP.NET Core

Confrontare le informazioni con i seguenti errori comuni. Se viene trovata una corrispondenza, seguire le indicazioni per la risoluzione dei problemi.

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Il programma di installazione non riesce ad ottenere VC++ Ridistribuibile

* **Eccezione del programma di installazione:** 0x80072efd o 0x80072f76 - Errore non specificato

* **Installer Log Exception&#8224;:** Error 0x80072efd oppure0x80072f76: Impossibile eseguire i file EXE del pacchetto

  & #8224; Il log si trova in C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Risoluzione dei problemi:

* Se il sistema non ha accesso a Internet durante l'installazione del bundle di hosting, questa eccezione si verifica quando il programma di installazione non riesce a ottenere *Microsoft Visual C++ 2015 Redistributable*. Ottenere un programma di installazione dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=53840). Se l'esecuzione del programma di installazione non riesce, il server potrebbe non ricevere il runtime .NET Core necessario per ospitare una distribuzione dipendente dal framework. Se si ospita una distribuzione dipendente dal framework, verificare che il runtime sia installato in Programmi e funzionalità. Se necessario, ottenere un programma di installazione del runtime dalla [pagina di tutti i download per .NET](https://www.microsoft.com/net/download/all). Dopo aver installato il runtime, riavviare il sistema o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>L'aggiornamento del sistema operativo ha rimosso il modulo di ASP.NET Core a 32 bit

* **Registro dell'applicazione:** Il modulo DLL**C:\WINDOWS\system32\inetsrv\aspnetcore.dll** non è riuscito a caricare. L'errore è nei dati.

Risoluzione dei problemi:

* I file non appartenenti al sistema operativo presenti nella directory **C:\Windows\SysWOW64\inetsrv** non vengono mantenuti durante un aggiornamento del sistema operativo. Se il modulo ASP.NET Core viene installato prima di un aggiornamento del sistema operativo e quindi si esegue qualsiasi pool di applicazioni in modalità a 32 bit dopo l'aggiornamento, si verifica questo problema. Dopo un aggiornamento del sistema operativo, ripristinare il Modulo di ASP.NET Core. Vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Selezionare **Ripara** quando viene eseguito il programma di installazione.

## <a name="platform-conflicts-with-rid"></a>La piattaforma è in conflitto con RID

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con radice fisica "C:\{PATH}\' Impossibile avviare il processo con la riga di comando '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.

* **Registro del modulo ASP.NET Core:** Eccezione non gestita: System.BadImageFormatException: Impossibile caricare il file o l'assembly "{assembly}.dll". Tentativo di caricare un programma con un formato non corretto.

Risoluzione dei problemi:

* Confermare che l'app sia eseguita in locale su Kestrel. Un errore del processo può essere causato da un problema interno dell'applicazione. Per altre informazioni, vedere [Risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).

* Verificare che `<PlatformTarget>` nel file *.csproj* non sia in conflitto con il RID. Ad esempio, non specificare un `<PlatformTarget>` di `x86` e pubblicare con un RID di `win10-x64`, tramite l'uso di *dotnet publish - c Release -r win10-x64* o impostando `<RuntimeIdentifiers>` nel *csproj* su `win10-x64`. Il progetto viene pubblicato senza avvisi o errori, ma la sua esecuzione non riesce e nel sistema vengono registrate le eccezioni sopra indicate.

* Se questa eccezione si verifica per una distribuzione di App di Azure durante l'aggiornamento di un'app e la distribuzione di assembly più recenti, eliminare manualmente tutti i file dalla distribuzione precedente. Le assembly incompatibili con il tempo di ritardo possono causare una eccezione `System.BadImageFormatException` quando si distribuisce un'app aggiornata.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Endpoint dell'URI non corretto o sito web arrestato

* **Browser:** ERR_CONNECTION_REFUSED

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Verificare che sia in uso l'endpoint URI corretto per l'app. Controllare i binding.

* Verificare che il sito Web IIS non sia nello stato *In arresto*.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Le funzionalità del server CoreWebEngine o W3SVC sono disabilitate

* **Eccezione del sistema operativo:** per usare il modulo di ASP.NET Core è necessario installare le funzionalità di IIS 7.0 CoreWebEngine e W3SVC.

Risoluzione dei problemi:

* Verificare che il ruolo e le funzionalità appropriati siano abilitati. Vedere [Configurazione di IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Percorso fisico del sito Web non corretto o app mancante

* **Browser:** 403 Non consentito: accesso negato **- OPPURE -** 403.14 Non consentito - Il server Web è configurato per non visualizzare il contenuto di questa directory.

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Controllare il sito Web IIS **Impostazioni di base** e la cartella dell'app fisica. Verificare che l'app si trovi nella cartella nel sito Web IIS **Percorso fisico**.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Ruolo non corretto, modulo non installato o autorizzazioni non corrette

* **Browser:** 500.19 Errore interno al server – non è possibile accedere alla pagina richiesta perché i dati di configurazione per la pagina non sono validi.

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Verificare che il ruolo appropriato sia abilitato. Vedere [Configurazione di IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Controllare **Programmi &amp; Funzionalità** e confermare che il **Modulo di Microsoft ASP.NET Core** sia stato installato. Se il **modulo Microsoft ASP.NET Core** non è presente nell'elenco dei programmi installati, installare il modulo. Vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Assicurarsi che **Pool di applicazioni** > **Modello di processo** > **Identità** sia impostato su **ApplicationPoolIdentity** o che l'identità personalizzata disponga delle autorizzazioni appropriate per accedere alla cartella di distribuzione dell'applicazione.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>ProcessPath non corretto, variabile di percorso mancante, aggregazione di hosting non installata, sistema/IIS non riavviato, VC Redistributable non installato o violazione dell'accesso a dotnet.exe

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con radice fisica "C:\\{PATH}\' Impossibile avviare il processo con la riga di comando '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.

* **Registro del modulo di ASP.NET Core:** il file di registro è stato creato ma è vuoto

Risoluzione dei problemi:

* Confermare che l'app sia eseguita in locale su Kestrel. Un errore del processo può essere causato da un problema interno dell'applicazione. Per altre informazioni, vedere [Risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).

* Verificare che l'attributo *processPath* dell'elemento `<aspNetCore>` in *web.config* sia *dotnet* per una distribuzione dipendente da framework (FDD, Framework-Dependent Deployment) o *.\{assembly}.exe* per una distribuzione autonoma (SCD, Self-Contained Deployment).

* Per una distribuzione dipendente dal framework, *dotnet.exe* potrebbe non essere accessibile tramite le impostazioni del percorso. Confermare che *C:\Program Files\dotnet\* esiste nelle impostazioni del percorso di sistema.

* Per una distribuzione dipendente da framework, *dotnet.exe* potrebbe non essere accessibile per l'identità utente del pool di applicazioni. Confermare che l'identità dell'utente di AppPool disponga dell'accesso alla directory *C:\Program Files\dotnet*. Verificare che non siano presenti regole di negazione configurate per l'identità dell'utente di AppPool in *C:\Program Files\dotnet* e nelle directory dell'app.

* È possibile che sia stata eseguita una distribuzione FDD e che sia stato installato .NET Core senza riavviare IIS. Riavviare il server o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

* È possibile che sia stata eseguita una distribuzione FDD senza installare il runtime .NET Core nel sistema host. Se il runtime .NET Core non è stato installato, eseguire il **programma di installazione del bundle di hosting .NET Core** nel sistema. Vedere [Installare il bundle di hosting .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Se si prova a installare il runtime .NET Core in un sistema senza connessione a Internet, ottenere il runtime dalla [pagina di tutti i download per .NET](https://www.microsoft.com/net/download/all) ed eseguire il programma di installazione dell'aggregazione di hosting per installare il modulo ASP.NET Core. Completare l'installazione riavviando il sistema o riavviando IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

* È possibile che sia stata eseguita una distribuzione FDD e che *Microsoft Visual C++ 2015 Redistributable (x64)* non sia installato nel sistema. Ottenere un programma di installazione dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Argomenti non corretti dell'elemento \<aspNetCore\>

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con radice fisica "C:\\{PATH}\' Impossibile avviare il processo con la riga di comando '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.

* **Registro di modulo di ASP.NET Core:** l'applicazione da eseguire non esiste: "PATH\{assembly}.dll"

Risoluzione dei problemi:

* Confermare che l'app sia eseguita in locale su Kestrel. Un errore del processo può essere causato da un problema interno dell'applicazione. Per altre informazioni, vedere [Risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).

* Verificare che l'attributo *arguments* dell'elemento `<aspNetCore>` in *web.config* sia (a) *.\{assembly}.dll* per una distribuzione dipendente da framework (FDD) o (b) non disponibile, una stringa vuota (*arguments=""*) o un elenco degli argomenti dell'app (*arguments="arg1, arg2, ..."*) per una distribuzione autonoma (SCD).

## <a name="missing-net-framework-version"></a>Versione di .NET Framework non presente

* **Browser:** 502.3 Gateway non valido - si è verificato un errore di connessione durante il tentativo di routing della richiesta.

* **Registro dell'applicazione:** ErrorCode = l'applicazione "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con radice fisica "C:\\{PATH}\' non è riuscita ad avviare il processo con la riga di comando '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.

* **Registro di modulo di ASP.NET Core:** Metodo, file o eccezione dell'assembly mancanti. Il metodo, il file o l'assembly specificati nell'eccezione sono un metodo, un file o un'assembly .NET Framework.

Risoluzione dei problemi:

* Installare la versione di .NET Framework mancante dal sistema.

* Per una distribuzione dipendente da framework (FDD), verificare che nel sistema sia installato il runtime corretto. Se viene generata questa eccezione dopo che il progetto è stato aggiornato dalla versione 1.1 a quella 2.0 e distribuito nel sistema host, assicurarsi che il framework 2.0 sia presente nel sistema host.

## <a name="stopped-application-pool"></a>Pool di applicazioni arrestato

* **Browser:** 503 Servizio non disponibile

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi

* Confermare che il pool di applicazioni non sia nello stato *Arrestato*.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware di integrazione di IIS non implementato

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con radice fisica "C:\\{PATH}\' ha creato un processo con la riga di comando" "C:\\{PATH}\{assembly}.{exe|dll}" ma ha danneggiato o non ha risposto o non è stata in ascolto sulla porta specificata "{PORT}", ErrorCode = "0x800705b4"

* **Registro di modulo di ASP.NET Core:** Il file di registro ha creato e mostra il normale funzionamento.

Risoluzione dei problemi

* Confermare che l'app sia eseguita in locale su Kestrel. Un errore del processo può essere causato da un problema interno dell'applicazione. Per altre informazioni, vedere [Risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).

* Verificare quanto segue:
  * Viene fatto riferimento al middleware di integrazione IIS eseguendo una chiamata del metodo `UseIISIntegration` nell'oggetto `WebHostBuilder` dell'app (ASP.NET Core 1.x)
  * L'app usa il metodo `CreateDefaultBuilder` (ASP.NET Core 2.x).
  
  Per informazioni dettagliate, vedere [Hosting in ASP.NET Core](xref:fundamentals/host/index).

## <a name="sub-application-includes-a-handlers-section"></a>La sotto-applicazione include una sezione \<gestori\>

* **Browser:** errore HTTP 500.19 - errore del server interno

* **Registro dell'applicazione:** Nessuna voce

* **Registro di modulo di ASP.NET Core:** Il file di log è stato creato e funziona normalmente per l'app radice. Il file di log non è stato creato per l'app secondaria.

Risoluzione dei problemi

* Verificare che il file *web.config* della sotto-applicazione non includa una sezione `<handlers>`.

## <a name="stdout-log-path-incorrect"></a>percorso errato del log stdout

* **Browser:** l'app risponde normalmente.

* **Registro dell'applicazione:** Avviso: impossibile creare stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi

* Il percorso `stdoutLogFile` specificato nell'elemento `<aspNetCore>` di *web.config* non esiste. Per altre informazioni, vedere la sezione [Creazione e reindirizzamento dei log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) dell'argomento Guida di riferimento per la configurazione del modulo ASP.NET Core.

## <a name="application-configuration-general-issue"></a>Problema generale della configurazione dell'applicazione

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro dell'applicazione:** l'applicazione "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" con radice fisica "C:\\{PATH}\' ha creato un processo con la riga di comando" "C:\\{PATH}\{assembly}.{exe|dll}" ma ha danneggiato o non ha risposto o non è stata in ascolto sulla porta specificata "{PORT}", ErrorCode = "0x800705b4"

* **Registro del modulo di ASP.NET Core:** il file di registro è stato creato ma è vuoto

Risoluzione dei problemi

* Questa eccezione generale indica che non è stato possibile avviare il processo, probabilmente a causa di un problema di configurazione dell'app. Facendo riferimento alla [struttura di directory](xref:host-and-deploy/directory-structure), verificare che le cartelle e i file distribuiti dell'app siano corretti e che i file di configurazione dell'app siano presenti e contengano le impostazioni corrette per l'app e l'ambiente. Per altre informazioni, vedere [Risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).
