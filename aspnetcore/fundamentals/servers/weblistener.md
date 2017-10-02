---
title: Implementazione del server web WebListener in ASP.NET Core
author: rick-anderson
description: "Introduce WebListener, un server web per ASP.NET Core in Windows. Basato sul driver in modalità kernel HTTP. sys, WebListener è un'alternativa a Kestrel che può essere utilizzato per la connessione diretta a Internet senza IIS."
keywords: ASP.NET Core, WebListener, HttpListener, prefissi url, SSL
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1abb3558546cd907c78b44d9353d9c9f1f5aff1
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Implementazione del server web WebListener in ASP.NET Core

Da [Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> In questo argomento si applica solo a ASP.NET di base 1. x. In ASP.NET 2.0 Core, è denominata WebListener [HTTP.sys](httpsys.md).

WebListener è un [server web per ASP.NET Core](index.md) che viene eseguita solo su Windows. Si basa sul [driver in modalità kernel HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener è un'alternativa a [Kestrel](kestrel.md) che può essere utilizzato per la connessione diretta a Internet senza basarsi su IIS come server proxy inverso. In effetti, **WebListener non può essere utilizzato con IIS o IIS Express, che non è compatibile con il [ASP.NET Core modulo](aspnet-core-module.md).**

Anche se WebListener è stata sviluppata per ASP.NET Core, può essere utilizzato direttamente in qualsiasi applicazione .NET Core o .NET Framework tramite il [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pacchetto NuGet.

WebListener supporta le funzionalità seguenti:

- [Autenticazione di Windows](xref:security/authentication/windowsauth)
- Condivisione delle porte
- Utilizzo di HTTPS con SNI
- HTTP/2 tramite TLS (Windows 10)
- Trasmissione diretta del file
- La memorizzazione nella cache di risposta
- WebSocket (Windows 8)

Versioni supportate di Windows:

- Windows 7 e Windows Server 2008 R2 e versioni successive

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Quando utilizzare WebListener

WebListener è utile per le distribuzioni in cui è necessario esporre il server direttamente a Internet senza l'utilizzo di IIS.

![WebListener comunica direttamente con Internet](weblistener/_static/weblistener-to-internet.png)

Poiché è stato progettato su Http.Sys, WebListener non richiede un server proxy inverso per la protezione contro gli attacchi. Http.Sys è una tecnologia avanzata che consente di proteggere molti tipi di attacchi e fornisce l'affidabilità, sicurezza e la scalabilità di un server web con caratteristiche complete. IIS viene eseguito come un listener HTTP in Http.Sys. 

WebListener è anche una buona scelta per le distribuzioni interne quando è necessaria una delle funzionalità che offerte che non è possibile ottenere utilizzando Kestrel.

![Weblistener comunica direttamente con la rete interna](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Come usare WebListener

Di seguito viene fornita una panoramica delle attività di configurazione per il sistema operativo host e l'applicazione ASP.NET Core.

### <a name="configure-windows-server"></a>Configurare Windows Server

* Installare la versione di .NET che richiede l'applicazione, ad esempio [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) o .NET Framework 4.5.1.

* Pre-registrare prefissi URL per associare WebListener e configurare i certificati SSL

   Se si non pre-registrare prefissi URL in Windows, è necessario eseguire l'applicazione con privilegi di amministratore. L'unica eccezione è se si associa a localhost tramite HTTP (non HTTPS) con un numero di porta maggiore di 1024; In questo caso i privilegi di amministratore non sono necessari.

   Per informazioni dettagliate, vedere [come pre-registrare prefissi e configurare SSL](#preregister-url-prefixes-and-configure-ssl) più avanti in questo articolo.

* Aprire le porte del firewall per consentire il traffico raggiungere WebListener.

   È possibile utilizzare netsh.exe o [i cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).

Sono inoltre disponibili [impostazioni del Registro di sistema Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Configurare l'applicazione ASP.NET di base

* Installare il pacchetto NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Viene inoltre installato [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) come dipendenza.

* Chiamare il `UseWebListener` metodo di estensione su [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) nel `Main` metodo, specificando qualsiasi WebListener [opzioni](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) e [impostazioni](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) che è necessario , come illustrato nell'esempio seguente:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Configurare gli URL e le porte per l'attesa su 

  Per impostazione predefinita ASP.NET Core associa a `http://localhost:5000`. Per configurare le porte e i prefissi URL, è possibile utilizzare il `UseURLs` metodo di estensione, il `urls` argomento della riga di comando o il sistema di configurazione di ASP.NET Core. Per ulteriori informazioni, vedere [Hosting](../../fundamentals/hosting.md).

  Usa Listener Web il [formati di stringa di prefisso Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Non sono previsti requisiti del formato di stringa di prefisso WebListener specifici.

  > [!NOTE]
  > Assicurarsi di specificare le stesse stringhe di prefisso in `UseUrls` che pre-registrare il server. 

* Verificare che l'applicazione non è configurato per l'esecuzione di IIS o IIS Express.

  In Visual Studio, il profilo di avvio predefinito è per IIS Express.  Per eseguire il progetto come applicazione console è necessario modificare manualmente il profilo selezionato, come illustrato nella schermata seguente.

  ![Selezionare il profilo di applicazione console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Come usare WebListener all'esterno di ASP.NET Core

* Installare il [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pacchetto NuGet.

* [Pre-registrare prefissi URL per associare WebListener e configurare i certificati SSL](#preregister-url-prefixes-and-configure-ssl) come farebbe per l'uso in ASP.NET Core.

Sono inoltre disponibili [impostazioni del Registro di sistema Http.Sys](https://support.microsoft.com/kb/820129).


Di seguito è riportato un esempio di codice che illustra WebListener uso all'esterno di ASP.NET Core:

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Pre-registrare prefissi URL e configurare SSL

Sia IIS sia WebListener si basano su driver sottostante in modalità kernel HTTP. sys in ascolto delle richieste e di elaborazione iniziale. In IIS, la gestione dell'interfaccia utente fornisce un modo semplice per configurare tutti gli elementi. Tuttavia, se si utilizza WebListener è necessario configurare manualmente Http.Sys. Lo strumento predefinito per questo scopo è netsh.exe. 

È necessario utilizzare netsh.exe per attività più comuni sono riservare i prefissi URL e l'assegnazione di certificati SSL.

NetSh.exe non è uno strumento facile da usare per utenti meno esperti. L'esempio seguente mostra il livello minimo necessario per riservare i prefissi URL per le porte 80 e 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Nell'esempio seguente viene illustrato come assegnare un certificato SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

Di seguito è riportata la documentazione di riferimento ufficiale:

* [Comandi Netsh per Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix stringhe](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Le risorse seguenti forniscono istruzioni dettagliate per scenari diversi. Gli articoli che fanno riferimento a `HttpListener` si applicano ugualmente a `WebListener`, come entrambe basate su HTTP. sys.

* [Procedura: configurare una porta con un certificato SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Le comunicazioni HTTPS - HttpListener basato su host e la certificazione Client](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) questo è un blog di terze parti e si è piuttosto precedente ma si dispone ancora di informazioni utili.
* [Procedura: HttpListener utilizzando questa procedura dettagliata o un Http Server codice non gestito (C++) come Server semplice SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) anche questo va un blog precedente con informazioni utili.
* [Come impostare un WebListener Core .NET con SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Ecco alcuni strumenti di terze parti che possono essere più la riga di comando netsh.exe semplici da utilizzare. Questi non vengono forniti da o approvati da Microsoft. Gli strumenti di Esegui come amministratore per impostazione predefinita, poiché netsh.exe stesso richiede privilegi di amministratore.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) fornisce un'interfaccia utente per l'elenco e la configurazione di opzioni e i certificati SSL, le prenotazioni del prefisso ed elenchi di certificati attendibili. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) consente di elencare o configurare i certificati SSL e i prefissi URL. L'interfaccia utente è più precise di http.sys Manager ed espone alcuni ulteriori opzioni di configurazione, ma in caso contrario fornisce funzionalità simili. Non è possibile creare un nuovo elenco di certificati attendibili (CTL), ma può assegnare a quelli esistenti.

Per la generazione di certificati SSL autofirmati, Microsoft fornisce gli strumenti da riga di comando: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) e il cmdlet PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Sono inoltre strumenti di terze parti dell'interfaccia utente che rendono più semplice per generare i certificati SSL autofirmati:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Interfaccia utente di Makecert](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [Applicazione di esempio per questo articolo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Codice sorgente WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hosting](../hosting.md)
