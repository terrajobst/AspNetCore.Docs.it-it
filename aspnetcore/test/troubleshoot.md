---
title: Risolvere i problemi di progetti ASP.NET Core
author: Rick-Anderson
description: Riconoscere e risolvere i problemi di avvisi ed errori con i progetti ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: test/troubleshoot
ms.openlocfilehash: bcec8a55a5111e1f3acf53ae2f57b45e6e609d25
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313680"
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

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Entrambe le versioni a 32 e 64 bit di .NET Core SDK vengono installate

Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

> Le versioni a 32 bit sia 64 bit di .NET Core SDK vengono installate. Solo i modelli rispetto alle versioni a 64 bit installate in ' c:\\Program Files\\dotnet\\sdk\\' vengono visualizzati.

Questo avviso viene visualizzato quando (x86) 32 bit sia versioni a 64 bit (x64) del [.NET Core SDK](https://www.microsoft.com/net/download/all) siano installati. Cause più comuni che è possibile installare entrambe le versioni includono:

* Si originariamente scaricato il programma di installazione di .NET Core SDK Usa un computer a 32 bit, ma quindi copiata quest'ultimo e viene installato in un computer a 64 bit.
* 32 bit .NET Core SDK è stato installato da un'altra applicazione.
* La versione errata è stata scaricata e installata.

Disinstallare il SDK di .NET Core a 32 bit per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK è installato in più posizioni

Nel **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

> .NET Core SDK è installato in più posizioni. Solo i modelli dagli SDK installati in ' c:\\Program Files\\dotnet\\sdk\\' vengono visualizzati.

Viene visualizzato questo messaggio quando si dispone di almeno un'installazione di .NET Core SDK in una directory fuori *c:\\Program Files\\dotnet\\sdk\\* . In genere ciò si verifica quando .NET Core SDK è stato distribuito in un computer tramite Copia/Incolla anziché il programma di installazione MSI.

Disinstallare tutte le 32-bit .NET Core SDK e runtime per evitare questo avviso. Disinstallare dal **Pannello di controllo** > **programmi e funzionalità** > **Disinstalla o modifica programma**. Se si conosce il motivo per cui l'avviso viene generato e le relative implicazioni, è possibile ignorare l'avviso.

### <a name="no-net-core-sdks-were-detected"></a>Nessun SDK per .NET Core sono stati rilevati

* In Visual Studio **nuovo progetto** finestra di dialogo per ASP.NET Core, si può vedere l'avviso seguente:

  > Nessun SDK per .NET Core sono stati rilevati, assicurarsi che siano inclusi nella variabile di ambiente `PATH`.

* Quando si esegue un `dotnet` comando, viene visualizzato l'avviso come:

  > Non è stato possibile trovare qualsiasi installato dotnet SDK.

Questi avvisi vengono visualizzati quando la variabile di ambiente `PATH` non punta a qualsiasi SDK per .NET Core nel computer. Per risolvere questo problema:

* Installare .NET Core SDK. Ottenere il programma di installazione più recente dal [.NET Downloads](https://dotnet.microsoft.com/download).
* Verificare che il `PATH` variabile di ambiente punta alla posizione in cui è installato il SDK (`C:\Program Files\dotnet\` 64-bit o x64 o `C:\Program Files (x86)\dotnet\` per 32-bit/x86). Il programma di installazione SDK imposta in genere il `PATH`. Installare sempre lo stesso numero di bit del SDK e Runtime nello stesso computer.

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a>SDK mancante dopo aver installato il Bundle di Hosting di .NET Core

Installare il [Bundle di Hosting di .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifica le `PATH` quando installa il runtime di .NET Core in modo che punti alla versione a 32 bit (x86) di .NET Core (`C:\Program Files (x86)\dotnet\`). Ciò può comportare mancante SDK quando i 32 bit (x86) di .NET Core `dotnet` comando viene usato ([SDK per .NET Core non sono state rilevate](#no-net-core-sdks-were-detected)). Per risolvere questo problema, spostare `C:\Program Files\dotnet\` in una posizione precedente `C:\Program Files (x86)\dotnet\` nel `PATH`.

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
