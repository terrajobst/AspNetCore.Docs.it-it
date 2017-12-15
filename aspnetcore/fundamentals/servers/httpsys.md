---
title: Implementazione del server web HTTP.sys in ASP.NET Core
author: rick-anderson
description: "Introduce HTTP.sys, un server web per ASP.NET Core in Windows. Basato sul driver Http.Sys in modalità kernel, HTTP.sys è un'alternativa a Kestrel che può essere utilizzato per la connessione diretta a Internet senza IIS."
keywords: Prefissi Core,HttpSys,HTTP.sys,HttpListener,url ASP.NET, SSL
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 8d46862af44379d8592efdf214a80214dce2d69d
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementazione del server web HTTP.sys in ASP.NET Core

Da [Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Questo argomento si applica solo ai componenti di base di ASP.NET 2.0 e versioni successive. Nelle versioni precedenti di ASP.NET Core, denominato HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys è un [server web per ASP.NET Core](index.md) che viene eseguita solo su Windows. Si basa sul [driver in modalità kernel HTTP. sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys è un'alternativa a [Kestrel](kestrel.md) che offre alcune funzionalità che non Kestel. **HTTP.sys non può essere utilizzato con IIS o IIS Express, che non è compatibile con il [ASP.NET Core modulo](aspnet-core-module.md).**

HTTP.sys supporta le funzionalità seguenti:

- [Autenticazione di Windows](xref:security/authentication/windowsauth)
- Condivisione delle porte
- Utilizzo di HTTPS con SNI
- HTTP/2 tramite TLS (Windows 10)
- Trasmissione diretta del file
- La memorizzazione nella cache di risposta
- WebSocket (Windows 8)

Versioni supportate di Windows:

- Windows 7 e Windows Server 2008 R2 e versioni successive

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Quando utilizzare HTTP.sys

HTTP.sys è utile per le distribuzioni in cui è necessario esporre il server direttamente a Internet senza l'utilizzo di IIS.

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

Poiché è stato progettato su Http.Sys, HTTP.sys non richiede un server proxy inverso per la protezione contro gli attacchi. Http.Sys è una tecnologia avanzata che consente di proteggere molti tipi di attacchi e fornisce l'affidabilità, sicurezza e la scalabilità di un server web con caratteristiche complete. IIS viene eseguito come un listener HTTP in Http.Sys. 

HTTP.sys è una scelta ottimale per le distribuzioni interne quando occorre una funzionalità non disponibile in Kestrel, ad esempio l'autenticazione di Windows.

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Come usare HTTP.sys

Di seguito viene fornita una panoramica delle attività di configurazione per il sistema operativo host e l'applicazione ASP.NET Core.

### <a name="configure-windows-server"></a>Configurare Windows Server

* Installare la versione di .NET che richiede l'applicazione, ad esempio [.NET Core](https://www.microsoft.com/net/download/core) o [.NET Framework](https://www.microsoft.com/net/download/framework).

* Pre-registrare prefissi URL per associare a HTTP.sys e configurare i certificati SSL

   Se si non pre-registrare prefissi URL in Windows, è necessario eseguire l'applicazione con privilegi di amministratore. L'unica eccezione è se si associa a localhost tramite HTTP (non HTTPS) con un numero di porta maggiore di 1024; In tal caso, i privilegi di amministratore non sono necessari.

   Per informazioni dettagliate, vedere [come pre-registrare prefissi e configurare SSL](#preregister-url-prefixes-and-configure-ssl) più avanti in questo articolo.

* Aprire le porte del firewall per consentire il traffico raggiungere HTTP.sys.

   È possibile utilizzare *netsh.exe* o [i cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).

Sono inoltre disponibili [impostazioni del Registro di sistema Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Configurare l'applicazione ASP.NET Core per utilizzare HTTP. sys

* Installazione del pacchetto non è necessaria se si usa il [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. Il [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) pacchetto è incluso nel metapackage.

* Chiamare il `UseHttpSys` metodo di estensione su `WebHostBuilder` nel `Main` metodo, specificando uno [HTTP.sys opzioni](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) che è necessario, come illustrato nell'esempio seguente:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Configurare le opzioni di HTTP.sys

Di seguito sono riportate alcune impostazioni HTTP.sys e i limiti che è possibile configurare.

**Connessioni client massima**

Impostare il numero massimo di connessioni simultanee aperte TCP per l'intera applicazione con il codice seguente in *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

Il numero massimo di connessioni è illimitato (null) per impostazione predefinita.

**Dimensione massima del corpo della richiesta**

Dimensioni predefinite del corpo massima della richiesta sono 30,000,000 byte, ovvero circa 28,6 MB.

Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET MVC di base consiste nell'utilizzare il [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attributo in un metodo di azione:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Di seguito è riportato un esempio che illustra come configurare il vincolo per l'intera applicazione, ogni richiesta:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

È possibile ignorare l'impostazione di una richiesta specifica in *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Se si tenta di configurare il limite per una richiesta dopo l'applicazione ha avviato la richiesta di lettura, viene generata un'eccezione. È presente un `IsReadOnly` proprietà che indica se il `MaxRequestBodySize` proprietà è in stato di sola lettura, pertanto è troppo tardi per configurare il limite.

Per informazioni sulle altre opzioni di HTTP.sys, vedere [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Configurare gli URL e le porte per l'attesa su 

Per impostazione predefinita ASP.NET Core associa a `http://localhost:5000`. Per configurare le porte e i prefissi URL, è possibile utilizzare il `UseUrls` metodo di estensione, il `urls` argomento della riga di comando, la variabile di ambiente ASPNETCORE_URLS o `UrlPrefixes` proprietà [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). Nell'esempio di codice viene illustrato come utilizzare `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Un vantaggio offerto da `UrlPrefixes` ottengono immediatamente un messaggio di errore se si tenta di aggiungere un prefisso che viene formattato errato. Un vantaggio offerto da `UseUrls` (condiviso con `urls` e ASPNETCORE_URLS) è che è possibile passare più facilmente tra Kestrel e HTTP.sys.

Se si utilizzano entrambi `UseUrls` (o `urls` o ASPNETCORE_URLS) e `UrlPrefixes`, le impostazioni di `UrlPrefixes` eseguire l'override di quelli nei `UseUrls`. Per altre informazioni, vedere [Hosting](xref:fundamentals/hosting).

HTTP.sys Usa il [formati stringa HTTP Server API UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Assicurarsi di specificare le stesse stringhe di prefisso in `UseUrls` o `UrlPrefixes` che pre-registrare il server. 

### <a name="dont-use-iis"></a>Non utilizzare IIS

Verificare che l'applicazione non è configurato per l'esecuzione di IIS o IIS Express.

In Visual Studio, il profilo di avvio predefinito è per IIS Express. Per eseguire il progetto come applicazione console, modificare manualmente il profilo selezionato, come illustrato nella schermata seguente.

![Selezionare il profilo di applicazione console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Pre-registrare prefissi URL e configurare SSL

Sia IIS sia HTTP.sys si basano su driver sottostante in modalità kernel HTTP. sys in ascolto delle richieste e di elaborazione iniziale. In IIS, la gestione dell'interfaccia utente fornisce un modo semplice per configurare tutti gli elementi. Tuttavia, è necessario configurare manualmente Http.Sys. Lo strumento incorporato al riguardo sono *netsh.exe*. 

Con *netsh.exe* è possibile riservare i prefissi URL e assegnare i certificati SSL. Lo strumento richiede privilegi amministrativi.

Nell'esempio seguente viene illustrato il requisito minimo necessario per riservare i prefissi URL per le porte 80 e 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Nell'esempio seguente viene illustrato come assegnare un certificato SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Ecco la documentazione di riferimento per *netsh.exe*:

* [Comandi Netsh per Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix stringhe](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Le risorse seguenti forniscono istruzioni dettagliate per scenari diversi. Gli articoli che fanno riferimento a HttpListener applicano ugualmente a HTTP.sys, come entrambe basate su HTTP. sys.

* [Procedura: configurare una porta con un certificato SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Le comunicazioni HTTPS - HttpListener basato su host e la certificazione Client](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) questo è un blog di terze parti e si è piuttosto precedente ma si dispone ancora di informazioni utili.
* [Procedura: HttpListener utilizzando questa procedura dettagliata o un Http Server codice non gestito (C++) come Server semplice SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) anche questo va un blog precedente con informazioni utili.

Ecco alcuni strumenti di terze parti che possono essere più facili da utilizzare più di *netsh.exe* riga di comando. Questi non vengono forniti da o approvati da Microsoft. Gli strumenti di Esegui come amministratore per impostazione predefinita, poiché *netsh.exe* stesso richiede privilegi di amministratore.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) fornisce un'interfaccia utente per l'elenco e la configurazione di opzioni e i certificati SSL, le prenotazioni del prefisso ed elenchi di certificati attendibili. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) consente di elencare o configurare i certificati SSL e i prefissi URL. L'interfaccia utente è più precise di http.sys Manager ed espone alcuni ulteriori opzioni di configurazione, ma in caso contrario fornisce funzionalità simili. Non è possibile creare un nuovo elenco di certificati attendibili (CTL), ma può assegnare a quelli esistenti.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [Applicazione di esempio per questo articolo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Codice sorgente HTTP.sys](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/hosting)
