---
title: Implementazione del server Web HTTP.sys in ASP.NET Core
author: rick-anderson
description: "Presentazione di HTTP.sys, un server Web per ASP.NET Core in Windows. Basato sul driver in modalità kernel Http.Sys, HTTP.sys è un'alternativa a Kestrel che consente la connessione diretta a Internet senza IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementazione del server Web HTTP.sys in ASP.NET Core

[Tom Dykstra](https://github.com/tdykstra) e [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Questo argomento si applica solo ad ASP.NET Core 2.0 e versioni successive. Nelle versioni precedenti di ASP.NET Core, HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys è un [server Web per ASP.NET Core](index.md) che può essere eseguito solo in Windows. È basato sul [driver in modalità kernel Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). HTTP.sys è un'alternativa a [Kestrel](kestrel.md) e offre alcune funzionalità non presenti in Kestrel. **HTTP.sys non può essere usato con IIS o IIS Express, che non è compatibile con il [modulo ASP.NET Core](aspnet-core-module.md).**

HTTP.sys supporta le funzionalità seguenti:

- [Autenticazione di Windows](xref:security/authentication/windowsauth)
- Condivisione delle porte
- HTTPS con SNI
- HTTP/2 su TLS (Windows 10)
- Trasmissione diretta dei file
- Memorizzazione nella cache delle risposte
- WebSocket (Windows 8)

Versioni di Windows supportate:

- Windows 7 e Windows Server 2008 R2 e versioni successive

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Quando usare HTTP.sys

HTTP.sys è utile per le distribuzioni in cui è necessario esporre il server direttamente a Internet senza l'utilizzo di IIS.

![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

Poiché è basato su Http.Sys, HTTP.sys non richiede un server proxy inverso per la protezione dagli attacchi. Http.Sys è una tecnologia avanzata che protegge da molti tipi di attacchi e offre l'affidabilità, la sicurezza e la scalabilità di un server Web con funzionalità complete. IIS viene eseguito come listener HTTP in Http.Sys. 

HTTP.sys è una scelta ottimale per le distribuzioni interne quando è necessario usare una funzionalità non disponibile in Kestrel, ad esempio l'autenticazione di Windows.

![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Come usare HTTP.sys

Di seguito sono descritte le attività di configurazione per il sistema operativo host e l'applicazione ASP.NET Core.

### <a name="configure-windows-server"></a>Configurare Windows Server

* Installare la versione di .NET richiesta dall'applicazione, ad esempio [.NET Core](https://www.microsoft.com/net/download/core) o [.NET Framework](https://www.microsoft.com/net/download/framework).

* Pre-registrare i prefissi URL per l'associazione a HTTP.sys e impostare i certificati SSL

   Se non si pre-registrano i prefissi URL in Windows, è necessario eseguire l'applicazione con i privilegi di amministratore. L'unica eccezione si verifica quando si esegue l'associazione a localhost usando HTTP (non HTTPS) con un numero di porta maggiore di 1024; in tal caso, i privilegi di amministratore non sono necessari.

   Per informazioni dettagliate, vedere [Pre-registrare i prefissi URL e configurare SSL](#preregister-url-prefixes-and-configure-ssl) più avanti in questo articolo.

* Aprire le porte del firewall per consentire al traffico di raggiungere HTTP.sys.

   È possibile usare *netsh.exe* o i [cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).

Sono anche disponibili le [Impostazioni del Registro di sistema HTTP. sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Configurare l'applicazione ASP.NET Core per l'utilizzo di HTTP. sys

* Se si usa il metapacchetto [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) non è necessario installare alcun pacchetto. Il pacchetto [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) è incluso nel metapacchetto.

* Chiamare il metodo di estensione `UseHttpSys` in `WebHostBuilder` nel metodo `Main`, specificando le [opzioni di HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) necessarie, come illustrato nell'esempio seguente:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Configurare le opzioni di HTTP.sys

Di seguito sono riportate alcune impostazioni di HTTP.sys e i limiti che è possibile configurare.

**Numero massimo di connessioni client**

È possibile impostare il numero massimo di connessioni TCP aperte simultaneamente per l'intera applicazione con il seguente codice in *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

Per impostazione predefinita, il numero massimo di connessioni è illimitato (null).

**Dimensione massima del corpo della richiesta**

La dimensione massima predefinita del corpo della richiesta è pari a 30.000.000 byte, equivalenti a circa 28,6 MB.

Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET Core MVC è l'uso dell'attributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) in un metodo di azione:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

L'esempio seguente mostra come configurare il vincolo per l'intera applicazione e ogni richiesta:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

È possibile eseguire l'override dell'impostazione per una richiesta specifica in *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Se si tenta di configurare il limite per una richiesta dopo che l'applicazione ha avviato la lettura della richiesta, viene generata un'eccezione. Una proprietà `IsReadOnly` indica se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.

Per informazioni sulle altre opzioni di HTTP.sys, vedere [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Configurare gli URL e le porte su cui eseguire l'ascolto 

Per impostazione predefinita ASP.NET Core è associato a `http://localhost:5000`. Per configurare i prefissi URL e le porte, è possibile usare il metodo di estensione `UseUrls`, l'argomento della riga di comando `urls`, la variabile di ambiente ASPNETCORE_URLS o la proprietà `UrlPrefixes` in [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). L'esempio di codice seguente usa `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Un vantaggio offerto da `UrlPrefixes` è la ricezione immediata di un messaggio di errore se si tenta di aggiungere un prefisso con formato errato. Un vantaggio offerto da `UseUrls` (condiviso con `urls` e ASPNETCORE_URLS) è la possibilità di passare più facilmente da Kestrel a HTTP.sys e viceversa.

Se si usa sia `UseUrls` (oppure `urls` o ASPNETCORE_URLS) che `UrlPrefixes`, le impostazioni in `UrlPrefixes` eseguono l'override delle impostazioni in `UseUrls`. Per altre informazioni, vedere [Hosting](xref:fundamentals/hosting).

HTTP.sys usa i [formati di stringa UrlPrefix dell'API del server HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Assicurarsi di specificare le stesse stringhe di prefisso in `UseUrls` o `UrlPrefixes` che vengono pre-registrate nel server. 

### <a name="dont-use-iis"></a>Non usare IIS

Verificare che l'applicazione non sia configurata per l'esecuzione di IIS o IIS Express.

In Visual Studio il profilo di avvio predefinito è per IIS Express. Per eseguire il progetto come applicazione console, modificare manualmente il profilo selezionato, come illustrato nello screenshot seguente.

![Selezionare il profilo dell'applicazione console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Pre-registrare i prefissi URL e configurare SSL

Sia IIS sia HTTP.sys si basano sul driver in modalità kernel Http. sys sottostante per l'ascolto delle richieste e l'elaborazione iniziale. In IIS l'interfaccia utente di gestione consente di configurare tutti gli elementi in modo relativamente semplice. Http.Sys deve essere tuttavia configurato manualmente. Lo strumento predefinito per questo scopo è *netsh.exe*. 

Con *netsh.exe* è possibile riservare i prefissi URL e assegnare i certificati SSL. Per usare lo strumento sono necessari i privilegi amministrativi.

L'esempio seguente illustra i prefissi URL minimi da riservare per le porte 80 e 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

L'esempio seguente illustra come assegnare un certificato SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

La documentazione di riferimento per *netsh.exe* è la seguente:

* [Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (Comandi di Netsh per il protocollo HTTP)
* [UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (Stringhe UrlPrefix)

Le risorse seguenti offrono istruzioni dettagliate per scenari diversi. Gli articoli che fanno riferimento a HttpListener si applicano anche a HTTP.sys poiché sono entrambi basati su Http.Sys.

* [Procedura: Configurare una porta con un certificato SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (Comunicazione HTTPS - Hosting e certificazione client basati su HttpListener) Blog di terze parti non recente ma che contiene informazioni utili.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (Procedura: Procedura dettagliata per l'uso di HttpListener o di codice non gestito del server HTTP (C++) come SSL Simple Server) Blog non recente contenente informazioni utili.

Di seguito sono elencati alcuni strumenti di terze parti che possono risultare più semplici da usare rispetto alla riga di comando *netsh.exe*. Questi strumenti non sono forniti o promossi da Microsoft. Gli strumenti vengono eseguiti come amministratore per impostazione predefinita, poiché anche *netsh.exe* richiede i privilegi di amministratore.

* [http.sys Manager](http://httpsysmanager.codeplex.com/) offre l'interfaccia utente per visualizzare e configurare i certificati e le opzioni SSL, le prenotazioni dei prefissi e gli elenchi scopi consentiti ai certificati. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) consente di visualizzare o configurare i certificati SSL e i prefissi URL. L'interfaccia utente è più avanzata di http.sys Manager ed espone un numero maggiore di opzioni di configurazione ma offre comunque una funzionalità simile. Non consente di creare un nuovo elenco scopi consentiti ai certificati (CTL), ma consente di assegnare elenchi esistenti.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* [App di esempio per questo articolo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Codice sorgente di HTTP.sys](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/hosting)
