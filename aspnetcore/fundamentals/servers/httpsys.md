---
title: Implementazione del server Web HTTP.sys in ASP.NET Core
author: rick-anderson
description: Informazioni su HTTP.sys, un server Web per ASP.NET Core in Windows. Basato sul driver in modalità kernel HTTP.sys, HTTP.sys è un'alternativa a Kestrel che consente la connessione diretta a Internet senza IIS.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: dff798b19ad6d10a8ce93001ed4cebe732c54320
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729315"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementazione del server Web HTTP.sys in ASP.NET Core

Di [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) e [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Questo argomento si applica ad ASP.NET Core 2.0 o versioni successive. Nelle versioni precedenti di ASP.NET Core, HTTP.sys è denominato [WebListener](xref:fundamentals/servers/weblistener).

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) è un [server Web per ASP.NET Core](xref:fundamentals/servers/index) che può essere eseguito solo in Windows. HTTP.sys è un'alternativa a [Kestrel](xref:fundamentals/servers/kestrel) e offre alcune funzionalità non presenti in Kestrel.

> [!IMPORTANT]
> HTTP.sys non è compatibile con il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e non può essere usato con IIS o IIS Express.

HTTP.sys supporta le funzionalità seguenti:

* [Autenticazione di Windows](xref:security/authentication/windowsauth)
* Condivisione delle porte
* HTTPS con SNI
* HTTP/2 su TLS (Windows 10 o versioni successive)
* Trasmissione diretta dei file
* Memorizzazione nella cache delle risposte
* WebSockets (Windows 8 o versioni successive)

Versioni di Windows supportate:

* Windows 7 o versione successiva
* Windows Server 2008 R2 o versioni successive

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Quando usare HTTP.sys

HTTP.sys è utile per le distribuzioni nei casi seguenti:

* È necessario esporre il server direttamente a Internet senza usare IIS.

  ![HTTP.sys comunica direttamente con Internet](httpsys/_static/httpsys-to-internet.png)

* Una distribuzione interna richiede una funzionalità non disponibile in Kestrel, ad esempio l'[autenticazione di Windows](xref:security/authentication/windowsauth).

  ![HTTP.sys comunica direttamente con la rete interna](httpsys/_static/httpsys-to-internal.png)

HTTP.sys è una tecnologia consolidata che protegge da molti tipi di attacchi e offre l'affidabilità, la sicurezza e la scalabilità di un server Web con funzionalità complete. IIS viene eseguito come listener HTTP in HTTP.sys. 

## <a name="how-to-use-httpsys"></a>Come usare HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Configurare l'app ASP.NET Core per l'uso di HTTP.sys

1. Non è necessario un riferimento al pacchetto nel file di progetto quando si usa il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 o versioni successive). Se non si usa il metapacchetto `Microsoft.AspNetCore.App` aggiungere un riferimento al pacchetto a [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

2. Chiamare il metodo di estensione [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) quando si compila l'host web, specificando le eventuali [opzioni di HTTP.sys](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions) necessarie:

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Le configurazioni aggiuntive di HTTP.sys vengono gestite tramite [impostazioni del Registro di sistema](https://support.microsoft.com/kb/820129).

   **Opzioni di HTTP.sys**

   | Proprietà | Descrizione | Impostazione predefinita |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Controllare se l'input e/o l'output sincroni sono consentiti per `HttpContext.Request.Body` e `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Consentire richieste anonime. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | Specificare gli schemi di autenticazione consentiti. Può essere modificata in qualsiasi momento prima dell'eliminazione del listener. I valori sono forniti dall'[enumerazione AuthenticationSchemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None` e `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Tentare la memorizzazione nella cache in [modalità kernel](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) per le risposte con intestazioni idonee. La risposta potrebbe non includere intestazioni `Set-Cookie`, `Vary` o `Pragma`. Deve includere un'intestazione `Cache-Control` `public` con valore `shared-max-age` o `max-age` o un'intestazione `Expires`. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | Numero massimo di accettazioni simultanee. | 5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Numero massimo di connessioni simultanee da accettare. Usare `-1` per un numero infinito. Usare `null` per usare l'impostazione a livello di computer del Registro di sistema. | `null`<br>(illimitato) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | Vedere la sezione <a href="#maxrequestbodysize">MaxRequestBodySize</a>. | 30000000 byte<br>(~28,6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Numero massimo di richieste che è possibile accodare. | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Indica se le scritture del corpo della risposta che hanno esito negativo a causa di disconnessioni del client devono generare eccezioni o vengono completate normalmente. | `false`<br>(completamento normale) |
   | [Timeouts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | Espone la configurazione di [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) HTTP.sys, che può essere configurata anche nel Registro di sistema. Seguire i collegamenti API per altre informazioni su ogni impostazione, inclusi i valori predefiniti:<ul><li>[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) &ndash; Tempo consentito all'API HTTP Server per svuotare il corpo dell'entità in una connessione keep-alive.</li><li>[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) &ndash; Tempo consentito per l'arrivo del corpo dell'entità della richiesta.</li><li>[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) &ndash; Tempo consentito all'API del server HTTP per analizzare l'intestazione della richiesta.</li><li>[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) &ndash; Tempo consentito per una connessione inattiva.</li><li>[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) &ndash; Velocità di invio minima per la risposta.</li><li>[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) &ndash; Tempo consentito alla richiesta per rimanere in coda prima che sia selezionata dall'app.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Specificare l'[UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) da registrare per HTTP.sys. Il più utile è il metodo [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add) usato per aggiungere un prefisso alla raccolta. Queste impostazioni possono essere modificate in qualsiasi momento prima dell'eliminazione del listener. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Dimensioni massime consentite per qualsiasi corpo della richiesta in byte. Con l'impostazione `null`, le dimensioni massime del corpo della richiesta sono illimitate. Questo limite non ha effetto sulle connessioni aggiornate, che sono sempre illimitate.

   Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET Core MVC per un singolo `IActionResult` prevede l'uso dell'attributo [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) in un metodo di azione:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   Se l'app tenta di configurare il limite per una richiesta dopo che l'app ha avviato la lettura della richiesta stessa, viene generata un'eccezione. È possibile usare una proprietà `IsReadOnly` per indicare se la proprietà `MaxRequestBodySize` è in stato di sola lettura e pertanto è troppo tardi per configurare il limite.

   Se l'app deve eseguire l'override di [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) per ogni richiesta, usare [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. Se si usa Visual Studio, assicurarsi che l'app non sia configurata per l'esecuzione di IIS o IIS Express.

   In Visual Studio il profilo di avvio predefinito è per IIS Express. Per eseguire il progetto come app console, modificare manualmente il profilo selezionato, come illustrato nello screenshot seguente:

   ![Selezionare il profilo dell'applicazione console](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Configurare Windows Server

1. Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), installare .NET Core, .NET Framework o entrambi (se l'app è un'app .NET Core destinata a .NET Framework).

   * **.NET Core** &ndash; Se l'app richiede .NET Core, ottenere ed eseguire il programma di installazione di .NET Core da [.NET All Downloads](https://www.microsoft.com/net/download/all) (Tutti i download per .NET).
   * **.NET framework** &ndash; Se l'app richiede .NET Framework, vedere [.NET Framework: Guida all'installazione](/dotnet/framework/install/) per trovare le istruzioni di installazione. Installare la versione di .NET Framework richiesta. Il programma di installazione per la versione più recente di .NET Framework è disponibile in [.NET All Downloads](https://www.microsoft.com/net/download/all) (Tutti i download per .NET).

2. Configurare gli URL e le porte per l'app.

   Per impostazione predefinita, ASP.NET Core è associato a `http://localhost:5000`. Per configurare le porte e i prefissi URL, è possibile usare:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * L'argomento della riga di comando `urls`
   * La variabile di ambiente `ASPNETCORE_URLS`
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   L'esempio di codice seguente mostra come usare [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Un vantaggio offerto da `UrlPrefixes` è che viene generato immediatamente un messaggio di errore per i prefissi non formattati correttamente.

   Le impostazioni di `UrlPrefixes` sostituiscono le impostazioni `UseUrls`/`urls`/`ASPNETCORE_URLS`. Pertanto, un vantaggio offerto da `UseUrls`, `urls` e dalla variabile di ambiente `ASPNETCORE_URLS` è che risulta più semplice alternare Kestrel e HTTP.sys. Per altre informazioni su `UseUrls`, `urls` e `ASPNETCORE_URLS`, vedere l'argomento [Hosting in ASP.NET Core](xref:fundamentals/host/index).

   HTTP.sys usa i [formati di stringa UrlPrefix dell'API del server HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Le associazioni con caratteri jolly di livello superiore (`http://*:80/` e `http://+:80`) **non** devono essere usate, poiché possono introdurre vulnerabilità a livello di sicurezza nell'app. Questo concetto vale sia per i caratteri jolly sicuri che vulnerabili. Usare nomi host espliciti al posto di caratteri jolly. L'associazione con caratteri jolly del sottodominio (ad esempio, `*.mysub.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile). Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.

3. Pre-registrare i prefissi URL per il binding a HTTP.sys e impostare i certificati x.509.

   Se i prefissi URL non sono pre-registrati in Windows, eseguire l'app con privilegi di amministratore. L'unica eccezione è il binding a localhost tramite HTTP (non HTTPS) con un numero di porta superiore a 1024. In tal caso, i privilegi di amministratore non sono necessari.

   1. Lo strumento predefinito per la configurazione di HTTP.sys è *netsh.exe*. *Netsh.exe* viene usato per riservare i prefissi URL e assegnare i certificati X.509. Per questo strumento sono necessari privilegi di amministratore.

      L'esempio seguente mostra i comandi per riservare i prefissi URL per le porte 80 e 443:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      L'esempio seguente mostra come assegnare un certificato X.509:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      Documentazione di riferimento per *netsh.exe*:

      * [Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (Comandi di Netsh per il protocollo HTTP)
      * [UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (Stringhe UrlPrefix)

   2. Creare certificati X.509 autofirmati, se necessario.

      [!INCLUDE [How to make an X.509 cert](../../includes/make-x509-cert.md)]

4. Aprire le porte del firewall per consentire al traffico di raggiungere HTTP.sys. Usare *netsh.exe* o i [cmdlet di PowerShell](https://technet.microsoft.com/library/jj554906).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

Per le app ospitate da HTTP.sys che interagiscono con richieste da Internet o da una rete aziendale, potrebbero essere necessari interventi di configurazione aggiuntivi in caso di hosting dietro server proxy e servizi di bilanciamento del carico. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Risorse aggiuntive

* [API di HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [Repository di GitHub aspnet/HttpSysServer (codice sorgente)](https://github.com/aspnet/HttpSysServer/)
* [Hosting in ASP.NET Core](xref:fundamentals/host/index)
