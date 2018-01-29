---
title: Riferimento per gli errori comuni di servizio App di Azure e IIS con ASP.NET Core
author: guardrex
description: "Distinguere gli errori più comuni per l'hosting di applicazioni ASP.NET Core servizio app di Azure e IIS."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 58c5ec6bb2603499332698fd4225e2fe636256e4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Riferimento per gli errori comuni di servizio App di Azure e IIS con ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

L'elenco di errori che segue non è completo. Se si verifica un errore non è elencato qui, [aprire un nuovo problema](https://github.com/aspnet/Docs/issues/new) con istruzioni dettagliate per riprodurre l'errore.

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Il programma di installazione non riesce ad ottenere VC++ Ridistribuibile

* **Eccezione del programma di installazione:** 0x80072efd o 0x80072f76 - Errore non specificato

* **Installer Log Exception&#8224;:** Error 0x80072efd oppure0x80072f76: Impossibile eseguire i file EXE del pacchetto

  &#8224; Il log si trova in C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Risoluzione dei problemi:

* Se il sistema non ha accesso a Internet durante l'installazione dell'aggregazione di hosting del server, questa eccezione si verifica quando il programma di installazione non riesce ad ottenere *Microsoft Visual C++ 2015 Redistributable*. Ottenere da un programma di installazione di [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Se il programma di installazione non riesce, il server potrebbe non ricevere il runtime .NET Core necessario per ospitare una distribuzione dipendenti dal framework (protezione). Se l'hosting di un tipo di protezione, verificare che il runtime viene installato nei programmi &amp; funzionalità. Se è necessario ottenere un programma di installazione di runtime da [Scarica .NET](https://www.microsoft.com/net/download/core). Dopo aver installato il runtime, riavviare il sistema o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>L'aggiornamento del sistema operativo ha rimosso il modulo di ASP.NET Core a 32 bit

* **Registro dell'applicazione:** Il modulo DLL**C:\WINDOWS\system32\inetsrv\aspnetcore.dll** non è riuscito a caricare. L'errore è nei dati.

Risoluzione dei problemi:

* I file non appartenenti al sistema operativo presenti nella directory **C:\Windows\SysWOW64\inetsrv** non vengono mantenuti durante un aggiornamento del sistema operativo. Se è installato il modulo di base di ASP.NET precedenti a questo problema viene rilevato un aggiornamento del sistema operativo e quindi qualsiasi pool di applicazioni viene eseguito in modalità a 32 bit dopo un aggiornamento del sistema operativo. Dopo un aggiornamento del sistema operativo, ripristinare il Modulo di ASP.NET Core. Vedere [Installare l'aggregazione di Hosting di.NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Selezionare **ripristino** quando si esegue il programma di installazione.

## <a name="platform-conflicts-with-rid"></a>La piattaforma è in conflitto con RID

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro applicazioni:** applicazione ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' con radice fisica ' c:\{percorso}\' Impossibile avviare il processo con la riga di comando ' "c:\\{percorso} {assembly}. { exe | dll} "', codice errore = ' 0x80004005: ff.

* **Log di modulo ASP.NET Core:** eccezione non gestita: System. BadImageFormatException: Impossibile caricare il file o l'assembly '{assembly}. dll'. Tentativo di caricare un programma con un formato non corretto.

Risoluzione dei problemi:

* Confermare che l'app sia eseguita in locale su Kestrel. Un errore del processo può essere causato da un problema interno dell'applicazione. Per ulteriori informazioni, vedere [risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).

* Verificare che il `<PlatformTarget>` nel *csproj* non sia in conflitto con il RID. Ad esempio, non specificare un `<PlatformTarget>` di `x86` e pubblicare con un RID di `win10-x64`, tramite l'utilizzo *dotnet pubblicare - c - r di versione win10-x64* o impostando la `<RuntimeIdentifiers>` nel *csproj*  a `win10-x64`. Il progetto viene pubblicato senza avvisi o errori, ma la sua esecuzione non riesce e nel sistema vengono registrate le eccezioni sopra indicate.

* Se questa eccezione si verifica per la distribuzione di un'app di Azure durante l'aggiornamento di un'app e distribuzione di assembly più recente, eliminare manualmente tutti i file dalla distribuzione precedente. Le assembly incompatibili con il tempo di ritardo possono causare una eccezione `System.BadImageFormatException` quando si distribuisce un'app aggiornata.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Endpoint dell'URI non corretto o sito web arrestato

* **Browser:** ERR_CONNECTION_REFUSED

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Verificare che l'endpoint URI corretto per l'app è in uso. Controllare le associazioni.

* Verificare che il sito Web IIS non è nel *arrestato* stato.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Le funzionalità del server CoreWebEngine o W3SVC sono disabilitate

* **Eccezione del sistema operativo:** per usare il modulo di ASP.NET Core è necessario installare le funzionalità di IIS 7.0 CoreWebEngine e W3SVC.

Risoluzione dei problemi:

* Verificare che il ruolo appropriato e funzionalità siano abilitate. Vedere [Configurazione di IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Percorso fisico del sito Web non corretto o mancante app

* **Browser:** 403 Non consentito: accesso negato **- OPPURE -** 403.14 Non consentito - Il server Web è configurato per non visualizzare il contenuto di questa directory.

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Controllare il sito Web IIS **le impostazioni di base** e la cartella fisica app. Verificare che l'app si trova nella cartella nel sito Web IIS **percorso fisico**.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Ruolo non corretto, modulo non installato o autorizzazioni non corrette

* **Browser:** 500.19 Errore interno al server – non è possibile accedere alla pagina richiesta perché i dati di configurazione per la pagina non sono validi.

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi:

* Verificare che sia abilitato il ruolo appropriato. Vedere [Configurazione di IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Controllare **Programmi &amp; Funzionalità** e confermare che il **Modulo di Microsoft ASP.NET Core** sia stato installato. Se il **modulo Microsoft ASP.NET Core** non è presente nell'elenco dei programmi installati, installare il modulo. Vedere [Installare l'aggregazione di Hosting di.NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).

* Assicurarsi che il **Pool di applicazioni** > **modello di processo** > **identità** è impostato su **ApplicationPoolIdentity** o l'identità personalizzato ha le autorizzazioni corrette per accedere alla cartella di distribuzione dell'app.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>ProcessPath non corretto, variabile di percorso mancante, aggregazione di hosting non installata, sistema/IIS non riavviato, VC Redistributable non installato o violazione dell'accesso a dotnet.exe

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro applicazioni:** applicazione ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' con radice fisica ' c:\\{percorso}\' Impossibile avviare il processo con la riga di comando ' ".\{ assembly} .exe"', codice errore = ' 0x80070002: 0.

* **Registro del modulo di ASP.NET Core:** il file di registro è stato creato ma è vuoto

Risoluzione dei problemi:

* Confermare che l'app sia eseguita in locale su Kestrel. Un errore del processo può essere causato da un problema interno dell'applicazione. Per ulteriori informazioni, vedere [risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).

* Controllare il *processPath* attributo la `<aspNetCore>` elemento *Web. config* per verificare che sia *dotnet* per una distribuzione dipendenti dal framework (protezione) o *. \{assembly} .exe* per una distribuzione indipendente (dimensione a modifica lenta).

* Per una distribuzione dipendente dal framework, *dotnet.exe* potrebbe non essere accessibile tramite le impostazioni del percorso. Confermare che *C:\Program Files\dotnet\* esiste nelle impostazioni del percorso di sistema.

* Per una distribuzione dipendente da framework, *dotnet.exe* potrebbe non essere accessibile per l'identità utente del pool di applicazioni. Confermare che l'identità dell'utente di AppPool disponga dell'accesso alla directory *C:\Program Files\dotnet*. Verificare che non sono presenti regole di negazione configurate per l'identità dell'utente AppPool sul *Files\dotnet c:\Programmi\Microsoft* e di directory dell'app.

* Un tipo di protezione sono stata distribuita e .NET Core installato senza il riavvio di IIS. Riavviare il server o riavviare IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

* Un tipo di protezione può essere distribuito senza installare il runtime .NET Core nel sistema host. Se il runtime .NET Core non è stato installato, eseguire il **programma di installazione di .NET Core Windows Server che ospita bundle** nel sistema. Vedere [Installare l'aggregazione di Hosting di.NET Core Windows Server](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Se si tenta di installare il runtime .NET Core in un sistema senza una connessione Internet, ottenere il runtime da [Scarica .NET](https://www.microsoft.com/net/download/core) ed eseguire il programma di installazione hosting bundle per installare il modulo di base di ASP.NET. Completare l'installazione riavviando il sistema o riavviando IIS eseguendo **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.

* Un tipo di protezione sono stata distribuita e *Microsoft Visual C++ 2015 Redistributable (x64)* non è installato nel sistema. Ottenere da un programma di installazione di [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Argomenti non corretti dell'elemento \<aspNetCore\>

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro applicazioni:** applicazione ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' con radice fisica ' c:\\{percorso}\' Impossibile avviare il processo con la riga di comando ' "dotnet".\{ DLL di assembly}', codice errore = ' 0x80004005: 80008081.

* **Log di modulo ASP.NET Core:** l'esecuzione dell'applicazione non esiste: ' percorso\{assembly}. dll '

Risoluzione dei problemi:

* Confermare che l'app sia eseguita in locale su Kestrel. Un errore del processo può essere causato da un problema interno dell'applicazione. Per ulteriori informazioni, vedere [risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).

* Esaminare il *argomenti* attributo la `<aspNetCore>` elemento *Web. config* per confermare che si tratta (a) *.\{ DLL di assembly}* per una distribuzione dipendenti dal framework (protezione); o (b) non è presente, una stringa vuota (*argomenti = ""*), o un elenco di argomenti dell'app (*argomenti = "arg1, arg2,..."*) per una distribuzione indipendente (dimensione a modifica lenta).

## <a name="missing-net-framework-version"></a>Versione di .NET Framework non presente

* **Browser:** 502.3 Gateway non valido - si è verificato un errore di connessione durante il tentativo di routing della richiesta.

* **Registro applicazioni:** ErrorCode = Application ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' con radice fisica ' c:\\{percorso}\' Impossibile avviare il processo con la riga di comando ' "dotnet".\{ DLL di assembly}', codice errore = ' 0x80004005: 80008081.

* **Registro di modulo di ASP.NET Core:** Metodo, file o eccezione dell'assembly mancanti. Il metodo, il file o l'assembly specificati nell'eccezione sono un metodo, un file o un'assembly .NET Framework.

Risoluzione dei problemi:

* Installare la versione di .NET Framework mancante dal sistema.

* Per una distribuzione del framework dipendente (protezione), verificare che nel sistema è installato il runtime corretto. Se il progetto viene aggiornato da 1.1 a 2.0, distribuito il sistema host, e risultati di questa eccezione, assicurarsi che il framework 2.0 sia nel sistema host.

## <a name="stopped-application-pool"></a>Pool di applicazioni arrestato

* **Browser:** 503 Servizio non disponibile

* **Registro dell'applicazione:** Nessuna voce

* **Registro del modulo di ASP.NET Core:** il file di registro non è stato creato

Risoluzione dei problemi

* Verificare che il Pool di applicazioni non è nel *arrestato* stato.

## <a name="iis-integration-middleware-not-implemented"></a>Middleware di integrazione di IIS non implementato

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro applicazioni:** applicazione ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' con radice fisica ' c:\\{percorso}\' creato un processo con la riga di comando ' "c:\\{percorso}\{assembly}. { exe | dll} "' ma danneggiato o ha non risposta o non in ascolto sulla porta specificata '{porta}', codice errore = '0x800705b4'

* **Registro di modulo di ASP.NET Core:** Il file di registro ha creato e mostra il normale funzionamento.

Risoluzione dei problemi

* Confermare che l'app sia eseguita in locale su Kestrel. Un errore del processo può essere causato da un problema interno dell'applicazione. Per ulteriori informazioni, vedere [risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).

* Verificare che sia:
  * Il middleware di IIS Integration è referencedby la chiamata di `UseIISIntegration` metodo dell'app `WebHostBuilder` (ASP.NET Core 1. x)
  * L'App viene utilizzato il `CreateDefaultBuilder` metodo (ASP.NET Core 2. x).
  
  Per informazioni dettagliate, vedere [Hosting in ASP.NET Core](xref:fundamentals/hosting).

## <a name="sub-application-includes-a-handlers-section"></a>La sotto-applicazione include una sezione \<gestori\>

* **Browser:** errore HTTP 500.19 - errore del server interno

* **Registro dell'applicazione:** Nessuna voce

* **Log di modulo ASP.NET Core:** file di Log creato e viene illustrato il normale funzionamento per l'applicazione principale. File di log non è stato creato per la sub-app.

Risoluzione dei problemi

* Verificare che il file *web.config* della sotto-applicazione non includa una sezione `<handlers>`.

## <a name="application-configuration-general-issue"></a>Problema generale della configurazione dell'applicazione

* **Browser:** errore HTTP 502.5 - errore del processo

* **Registro applicazioni:** applicazione ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' con radice fisica ' c:\\{percorso}\' creato un processo con la riga di comando ' "c:\\{percorso}\{assembly}. { exe | dll} "' ma danneggiato o ha non risposta o non in ascolto sulla porta specificata '{porta}', codice errore = '0x800705b4'

* **Registro del modulo di ASP.NET Core:** il file di registro è stato creato ma è vuoto

Risoluzione dei problemi

* Questa eccezione generale indica che il processo non è stato possibile avviare, probabilmente a causa di un problema di configurazione app. Riferimento a [struttura di Directory](xref:host-and-deploy/directory-structure), verificare che l'app distribuita file e cartelle sono appropriate e che siano presenti file di configurazione dell'applicazione e contiene le impostazioni corrette per l'app e l'ambiente. Per ulteriori informazioni, vedere [risoluzione dei problemi](xref:host-and-deploy/iis/troubleshoot).
