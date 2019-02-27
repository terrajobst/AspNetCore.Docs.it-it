---
title: Risolvere i problemi di progetti ASP.NET Core
author: Rick-Anderson
description: Riconoscere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: c8b34f51fd329eb9a7c34f7be93bd7f2aa054283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899288"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Risolvere i problemi di progetti ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

I collegamenti seguenti forniscono indicazioni sulla risoluzione dei problemi:

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [NDC Conference (Londra, 2018): Diagnosi dei problemi nelle applicazioni ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Blog su ASP.NET: Risoluzione dei problemi di prestazioni di ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Avvisi di .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Entrambe le versioni a 64 bit di .NET Core SDK e a 32 bit installate

Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

> Vengono installate sia 32 e 64 versioni di bit di .NET Core SDK. Solo i modelli dalle versioni a 64 bit installate in ' c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/both32and64bit.png)

Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) del [.NET Core SDK](https://www.microsoft.com/net/download/all) siano installati. Cause più comuni che è possibile installare entrambe le versioni includono:

* Si originariamente scaricato il programma di installazione di .NET Core SDK Usa un computer a 32 bit, ma quindi copiata quest'ultimo e viene installato in un computer a 64 bit.
* 32 bit .NET Core SDK è stato installato da un'altra applicazione.
* La versione errata è stata scaricata e installata.

Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK è installato in più posizioni

Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

> .NET Core SDK è installato in più posizioni. Solo i modelli dagli SDK installati in ' c:\\Program Files\\dotnet\\sdk\\' verranno visualizzati.

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/multiplelocations.png)

Viene visualizzato questo messaggio quando si dispone di almeno un'installazione di .NET Core SDK in una directory fuori *c:\\Program Files\\dotnet\\sdk\\*. In genere ciò si verifica quando .NET Core SDK è stato distribuito in un computer tramite Copia/Incolla anziché il programma di installazione MSI.

Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="no-net-core-sdks-were-detected"></a>Nessun SDK per .NET Core sono stati rilevati

Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

> Nessun SDK per .NET Core sono stati rilevati, assicurarsi che siano inclusi nella variabile di ambiente 'PATH'.

![Screenshot della finestra di dialogo OneASP.NET viene visualizzato il messaggio di avviso](troubleshoot/_static/NoNetCore.png)

Questo avviso viene visualizzato quando la variabile di ambiente `PATH` non fa riferimento a qualsiasi SDK per .NET Core nel computer (ad esempio `C:\Program Files\dotnet\` e `C:\Program Files (x86)\dotnet\`). Per risolvere questo problema:

* Installare o verificare che sia installato .NET Core SDK. Ottenere il programma di installazione più recente dal [.NET Downloads](https://dotnet.microsoft.com/download). 
* Verificare che il `PATH` variabile di ambiente punta alla posizione in cui è installato il SDK. Il programma di installazione è imposta in genere il `PATH`.

## <a name="obtain-data-from-an-app"></a>Ottenere dati da un'app

Se un'app è in grado di rispondere alle richieste, è possibile ottenere i dati seguenti dell'App Usa il middleware:

* Richiedere &ndash; stringa, le intestazioni di metodo, lo schema, host, pathbase, percorso, query
* Connessione &ndash; indirizzo IP remoto, porta remota, indirizzo IP locale, porta locale, il certificato client
* Identità &ndash; Name, nome visualizzato
* Impostazioni di configurazione
* Variabili di ambiente

Inserire quanto segue [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) all'inizio del codice di `Startup.Configure` pipeline di elaborazione di richiesta del metodo. L'ambiente viene verificata prima che il middleware viene eseguito per garantire che il codice venga eseguito solo nell'ambiente di sviluppo.

Per ottenere l'ambiente, utilizzare uno degli approcci seguenti:

* Inserire il `IHostingEnvironment` nella `Startup.Configure` (metodo) e controllare l'ambiente con la variabile locale. Esempio di codice seguente illustra questo approccio.

* Assegnare l'ambiente a una proprietà nel `Startup` classe. Controllare l'ambiente utilizzando la proprietà (ad esempio, `if (Environment.IsDevelopment())`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```
