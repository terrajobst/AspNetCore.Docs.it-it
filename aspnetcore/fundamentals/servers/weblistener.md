---
title: Implementazione del server Web WebListener in ASP.NET Core
author: rick-anderson
description: Informazioni su WebListener, un server Web per ASP.NET Core in Windows che può essere usato per la connessione diretta a Internet senza IIS.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 46871edb744ad152df8eb958b344068b7408dd1e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34248453"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Implementazione del server Web WebListener in ASP.NET Core

[Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Questo argomento si applica solo ad ASP.NET Core 1.x. In ASP.NET Core 2.0 WebListener è denominato [HTTP.sys](httpsys.md).

WebListener è un [server Web per ASP.NET Core](index.md) che può essere eseguito solo in Windows. È basato sul [driver in modalità kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener è un'alternativa a [Kestrel](kestrel.md) che consente la connessione diretta a Internet dover usare IIS come server proxy inverso. Infatti **WebListener non può essere usato con IIS o IIS Express, poiché non è compatibile con il [modulo ASP.NET Core](aspnet-core-module.md).**

Nonostante WebListener sia stato sviluppato per ASP.NET Core, può essere usato direttamente in tutte le applicazioni .NET Core o .NET Framework tramite il pacchetto NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

WebListener supporta le funzionalità seguenti:

- [Autenticazione di Windows](xref:security/authentication/windowsauth)
- Condivisione delle porte
- HTTPS con SNI
- HTTP/2 su TLS (Windows 10)
- Trasmissione diretta dei file
- Memorizzazione nella cache delle risposte
- WebSocket (Windows 8)

Versioni di Windows supportate:

- Windows 7 e Windows Server 2008 R2 e versioni successive

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Quando usare WebListener

WebListener è utile per le distribuzioni in cui è necessario esporre il server direttamente a Internet senza usare IIS.

![WebListener comunica direttamente con Internet](weblistener/_static/weblistener-to-internet.png)

Poiché è basato su Http.Sys, WebListener non richiede un server proxy inverso per la protezione dagli attacchi. WebListener è una tecnologia avanzata che protegge da molti tipi di attacchi e offre l'affidabilità, la sicurezza e la scalabilità di un server Web con funzionalità complete. IIS viene eseguito come listener HTTP in Http.Sys. 

WebListener è anche una scelta ottimale per le distribuzioni interne quando è necessario usare una funzionalità non disponibile in Kestrel.

![Weblistener comunica direttamente con la rete interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Come usare WebListener

Di seguito sono descritte le attività di configurazione per il sistema operativo host e l'applicazione ASP.NET Core.

### <a name="configure-windows-server"></a>Configurare Windows Server

* Installare la versione di .NET richiesta dall'applicazione, ad esempio [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.

* Pre-registrare i prefissi URL per l'associazione a WebListener e configurare i certificati SSL

   Se non si pre-registrano i prefissi URL in Windows, è necessario eseguire l'applicazione con i privilegi di amministratore. L'unica eccezione si verifica quando si esegue l'associazione a localhost usando HTTP (non HTTPS) con un numero di porta maggiore di 1024; in tal caso, i privilegi di amministratore non sono necessari.

   Per informazioni dettagliate, vedere [Pre-registrare i prefissi URL e configurare SSL](#preregister-url-prefixes-and-configure-ssl) più avanti in questo articolo.

* Aprire le porte del firewall per consentire al traffico di raggiungere WebListener.

   È possibile usare netsh.exe o i [cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).

Sono anche disponibili le [Impostazioni del Registro di sistema HTTP. sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Configurare l'applicazione ASP.NET Core

* Installare il pacchetto NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/), che installa anche [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) come dipendenza.

* Chiamare il metodo di estensione `UseWebListener` in [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) nel metodo `Main`, specificando le [opzioni](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) e le [impostazioni](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) necessarie di WebListener, come illustrato nell'esempio seguente:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Configurare gli URL e le porte su cui eseguire l'ascolto 

  Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`. Per configurare le porte e i prefissi URL, è possibile usare il metodo di estensione `UseURLs`, l'argomento della riga di comando `urls` o il sistema di configurazione di ASP.NET Core. Per altre informazioni, vedere [Hosting in ASP.NET Core(xref:fundamentals/host/index).

  WebListener usa i [formati di stringa di prefisso di Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). WebListener non richiede un formato specifico per le stringhe di prefisso.

  > [!WARNING]
  > Le associazioni con caratteri jolly di livello superiore (`http://*:80/` e `http://+:80`) **non** devono essere usate, poiché possono introdurre vulnerabilità a livello di sicurezza nell'app. Questo concetto vale sia per i caratteri jolly sicuri che vulnerabili. Usare nomi host espliciti al posto di caratteri jolly. L'associazione con caratteri jolly del sottodominio (ad esempio, `*.mysub.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile). Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.

  > [!NOTE]
  > Assicurarsi di specificare le stesse stringhe di prefisso in `UseUrls` che vengono pre-registrate nel server. 

* Verificare che l'applicazione non sia configurata per l'esecuzione di IIS o IIS Express.

  In Visual Studio il profilo di avvio predefinito è per IIS Express.  Per eseguire il progetto come applicazione console, modificare manualmente il profilo selezionato, come illustrato nello screenshot seguente.

  ![Selezionare il profilo dell'applicazione console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Come usare WebListener all'esterno di ASP.NET Core

* Installare il pacchetto NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

* [Pre-registrare i prefissi URL per l'associazione a WebListener e configurare i certificati SSL](#preregister-url-prefixes-and-configure-ssl), come se venissero usati in ASP.NET Core.

Sono anche disponibili le [Impostazioni del Registro di sistema HTTP. sys](https://support.microsoft.com/kb/820129).


Di seguito è riportato un esempio di codice che illustra l'uso di WebListener all'esterno di ASP.NET Core:

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Pre-registrare i prefissi URL e configurare SSL

Sia IIS sia WebListener si basano sul driver in modalità kernel Http. sys sottostante per l'ascolto delle richieste e l'elaborazione iniziale. In IIS l'interfaccia utente di gestione consente di configurare tutti gli elementi in modo relativamente semplice. Se si usa WebListener, Http.Sys deve comunque essere configurato manualmente. Lo strumento predefinito per questo scopo è netsh.exe. 

Netsh.exe viene comunemente usato per riservare i prefissi URL e assegnare i certificati SSL.

NetSh.exe non è uno strumento facile da usare per gli utenti meno esperti. L'esempio seguente illustra i prefissi URL minimi necessari da riservare per le porte 80 e 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

L'esempio seguente illustra come assegnare un certificato SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

Di seguito è riportata la documentazione di riferimento ufficiale:

* [Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (Comandi di Netsh per il protocollo HTTP)
* [UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (Stringhe UrlPrefix)

Le risorse seguenti offrono istruzioni dettagliate per scenari diversi. Gli articoli che fanno riferimento a `HttpListener` si applicano anche a `WebListener` poiché sono entrambi basati su Http.Sys.

* [Procedura: Configurare una porta con un certificato SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicazione HTTPS - Hosting e certificazione client basati su HttpListener) Blog di terze parti non recente ma che contiene informazioni utili.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedura: Procedura dettagliata per l'uso di HttpListener o di codice non gestito del server HTTP (C++) come SSL Simple Server) Blog non recente contenente informazioni utili.
* [How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (Come configurare WebListener in .NET Core con SSL)

Di seguito sono elencati alcuni strumenti di terze parti che possono risultare più semplici da usare rispetto alla riga di comando di netsh.exe. Questi strumenti non sono offerti o promossi da Microsoft. Gli strumenti vengono eseguiti come amministratore per impostazione predefinita, poiché anche netsh.exe richiede i privilegi di amministratore.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) offre l'interfaccia utente per visualizzare e configurare i certificati e le opzioni SSL, le prenotazioni dei prefissi e gli elenchi scopi consentiti ai certificati. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) consente di visualizzare o configurare i certificati SSL e i prefissi URL. L'interfaccia utente è più avanzata di http.sys Manager ed espone un numero maggiore di opzioni di configurazione ma offre comunque una funzionalità simile. Non consente di creare un nuovo elenco scopi consentiti ai certificati (CTL), ma consente di assegnare elenchi esistenti.

Per la generazione di certificati SSL autofirmati, Microsoft offre gli strumenti da riga di comando: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) e il cmdlet di PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Per generare facilmente i certificati SSL autofirmati, è possibile usare anche gli strumenti di interfaccia utente di terze parti:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert UI](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [App di esempio per questo articolo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Codice sorgente di WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/host/index)
