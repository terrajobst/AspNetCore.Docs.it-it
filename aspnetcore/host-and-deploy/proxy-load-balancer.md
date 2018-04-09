---
title: Configurare ASP.NET Core per essere usati con i server proxy e bilanciamento del carico
author: guardrex
description: Informazioni sulla configurazione per le app ospitate dietro server proxy e servizi di bilanciamento del carico, che spesso oscurare importante richiedere informazioni.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Configurare ASP.NET Core per essere usati con i server proxy e bilanciamento del carico

Dal [Luke Latham](https://github.com/guardrex) e [Chris Ross](https://github.com/Tratcher)

Nella configurazione consigliata per ASP.NET Core, l'app viene ospitato mediante Apache, Nginx o modulo Core accoppiato. Server proxy, servizi di bilanciamento del carico e altri dispositivi di rete spesso nascondano informazioni sulla richiesta prima di raggiungere l'app:

* Quando le richieste HTTPS vengono elaborate tramite HTTP, lo schema originale (HTTPS) viene perso e deve essere inoltrato in un'intestazione.
* Poiché un'app riceve una richiesta dal proxy e non la relativa origine true su Internet o rete aziendale, indirizzo IP di origine client deve essere inoltrato anche in un'intestazione.

Queste informazioni potrebbero essere importante nell'elaborazione delle richieste, ad esempio nel reindirizzamenti, l'autenticazione, la generazione di collegamento, la valutazione dei criteri e geoloation client.

## <a name="forwarded-headers"></a>Intestazioni inoltrate

Per convenzione, i proxy inoltrano le informazioni nelle intestazioni HTTP.

| Header | Descrizione |
| ------ | ----------- |
| X-inoltrati-per | Contiene informazioni sul client che ha avviato la richiesta e i proxy successivi in una catena di proxy. Questo parametro può contenere IP indirizzi (e, facoltativamente, i numeri di porta). In una catena di server proxy, il primo parametro indica il client in cui è stata innanzitutto eseguita la richiesta. Gli identificatori di proxy successive seguire. Il proxy ultimo della catena non è nell'elenco dei parametri. Indirizzo IP del proxy ultima e, facoltativamente, un numero di porta, sono disponibili come l'indirizzo IP remoto a livello di trasporto. |
| X-Forwarded-Proto | Il valore dello schema di origine (HTTP/HTTPS). Il valore può essere anche un elenco di schemi se la richiesta è attraversato più proxy. |
| X-Forwarded-Host | Il valore originale del campo dell'intestazione Host. I proxy in genere, non modificare l'intestazione Host. Vedere [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) per informazioni su una vulnerabilità di elevazione dei privilegi che interessa i sistemi in cui il proxy non vengono convalidate o restict intestazioni Host per valori buona noti. |

Il Middleware di intestazioni inoltrati dal [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) del pacchetto, legge le intestazioni e compilati i campi associati nella [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext). 

Gli aggiornamenti di middleware:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; impostate utilizzando il `X-Forwarded-For` valore dell'intestazione. Impostazioni aggiuntive influire sul modo in cui il middleware imposta `RemoteIpAddress`. Per informazioni dettagliate, vedere la [opzioni del Middleware intestazioni inoltrati](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; impostate utilizzando il `X-Forwarded-Proto` valore dell'intestazione.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; impostate utilizzando il `X-Forwarded-Host` valore dell'intestazione.

Si noti che non tutti i dispositivi di rete aggiungere la `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni senza alcuna configurazione aggiuntiva. Se le richieste elaborate non contengono le intestazioni quando raggiungono l'app, consultare la Guida del produttore del dispositivo.

Inoltrati intestazioni Middleware [le impostazioni predefinite](#forwarded-headers-middleware-options) possono essere configurate. Le impostazioni predefinite sono:

* Solo *un proxy* tra l'app e l'origine delle richieste.
* Solo gli indirizzi di loopback configurati per i proxy noti e noti reti.

## <a name="iisiis-express-and-aspnet-core-module"></a>Modulo Core ASP.NET e IIS/IIS Express

Inoltrato Middleware intestazioni è abilitata per impostazione predefinita dal Middleware di integrazione IIS quando si esegue l'app associata a IIS e il modulo di base di ASP.NET. Inoltrato Middleware intestazioni viene attivato prima l'esecuzione della pipeline middleware con una configurazione con restrizioni specifica per il modulo di base di ASP.NET per motivi di trust con le intestazioni inoltrate (ad esempio [lo spoofing degli indirizzi IP](https://www.iplocation.net/ip-spoofing)). Il middleware è configurato per inoltrare il `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni ed è limitato a un proxy localhost singolo. Se è richiesta una configurazione aggiuntiva, vedere la [opzioni del Middleware intestazioni inoltrati](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Altri scenari del servizio di bilanciamento carico e il server proxy

Se non si utilizza IIS Integration Middleware, inoltrati Middleware intestazioni non è abilitato per impostazione predefinita. Middleware intestazioni inoltrato deve essere abilitato per un'app per le intestazioni di processo inoltrato con [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Dopo aver abilitato il middleware se nessun [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) specificato per il middleware, il valore predefinito [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) sono [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Configurare il middleware con [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) per l'inoltro di `X-Forwarded-For` e `X-Forwarded-Proto` intestazioni in `Startup.ConfigureServices`. Richiamare il [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metodo `Startup.Configure` prima di chiamare altro middleware:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> Se nessun [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) specificato `Startup.ConfigureServices` o direttamente al metodo di estensione con [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), il valore predefinito le intestazioni per l'inoltro sono [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). Il [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) proprietà deve essere configurata con le intestazioni per l'inoltro.

## <a name="forwarded-headers-middleware-options"></a>Opzioni del Middleware intestazioni inoltrate

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) controllano il comportamento del Middleware di intestazioni inoltrati:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| Opzione | Descrizione |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Il valore predefinito è `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Identifica i server d'inoltro devono essere elaborati. Vedere la [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) per l'elenco dei campi che si applicano. I valori tipici assegnati a questa proprietà sono <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Il valore predefinito è [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Il valore predefinito è `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Il valore predefinito è `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Limita il numero di voci nelle intestazioni che vengono elaborate. Impostato su `null` per disabilitare il limite, ma questo deve essere eseguita solo se `KnownProxies` o `KnownNetworks` configurati.<br><br>Il valore predefinito è 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Intervalli dei proxy noti per accettare le intestazioni inoltrate di indirizzi. Specificare gli intervalli IP usando la notazione Classless Interdomain Routing (CIDR).<br><br>Il valore predefinito è un [IList](/dotnet/api/system.collections.generic.ilist-1)\<[rete IP](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> contenente un'unica voce per `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Indirizzi dei proxy noti per accettare le intestazioni inoltrate dal. Utilizzare `KnownProxies` per specificare l'indirizzo IP esatto corrispondenze.<br><br>Il valore predefinito è un [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> contenente un'unica voce per `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Il valore predefinito è `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Il valore predefinito è `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Utilizzare l'intestazione specificata da questa proprietà anziché di quello specificato da [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Il valore predefinito è `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Richiedere il numero di valori di intestazione alla sincronizzazione tra il [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) in fase di elaborazione.<br><br>Il valore predefinito in ASP.NET Core 1.x è `true`. Il valore predefinito di ASP.NET Core 2.0 o versione successiva è `false`. |

## <a name="scenarios-and-use-cases"></a>Scenari e casi d'uso

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Quando non è possibile aggiungere inoltrato intestazioni e tutte le richieste sono sicure

In alcuni casi, potrebbe non essere possibile aggiungere intestazioni inoltrate alle richieste elaborate all'app. Se il proxy consiste nell'imporre che tutte le richieste esterne pubbliche siano HTTPS, lo schema può essere impostato manualmente `Startup.Configure` prima di utilizzare qualsiasi tipo di middleware:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Questo codice può essere disabilitato tramite una variabile di ambiente o altre impostazioni di configurazione in un ambiente di sviluppo o gestione temporanea.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Gestire con percorso di basa e proxy che modificano il percorso della richiesta

Alcuni proxy passare il percorso intatto, ma con un'app di percorso di base che deve essere rimossi in modo che il routing funziona correttamente. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware suddivide il percorso nella [HttpRequest. Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) e il percorso di base in app [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Se `/foo` è il percorso dell'app di base passato come un percorso di proxy `/foo/api/1`, i set di middleware `Request.PathBase` a `/foo` e `Request.Path` a `/api/1` con il comando seguente:

```csharp
app.UsePathBase("/foo");
```

Il percorso e il percorso di basa originale vengono riapplicate quando viene chiamato nuovamente il middleware in ordine inverso. Per ulteriori informazioni sull'elaborazione degli ordini middleware, vedere [Middleware](xref:fundamentals/middleware/index).

Se il proxy Elimina il percorso (ad esempio, l'inoltro `/foo/api/1` al `/api/1`), correzione reindirizza e collega impostando la richiesta [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) proprietà:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Se il proxy aggiunge i dati del percorso, eliminare parte del percorso di correggere i reindirizzamenti e i collegamenti utilizzando [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) e l'assegnazione per il [percorso](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) proprietà:

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a>Risolvere i problemi

Quando le intestazioni non vengono inoltrate come previsto, attivare [registrazione](xref:fundamentals/logging/index). Se i registri non forniscono informazioni sufficienti per risolvere il problema, enumerare le intestazioni delle richieste ricevute dal server. Le intestazioni possono essere scritti una risposta di app utilizzando middleware inline:

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

Verificare che la X - inoltrata-* intestazioni vengono ricevute dal server con i valori previsti. Se sono presenti più valori in una determinata intestazione, tenere presente le intestazioni di processi inoltrati Middleware intestazioni in ordine inverso da destra a sinistra.

IP remoto originale della richiesta deve corrispondere una voce nella `KnownProxies` o `KnownNetworks` elenca prima dell'elaborazione X-inoltrati-per. Questo riduce lo spoofing degli indirizzi di intestazione non accettando server d'inoltro da proxy non attendibili.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core elevazione dei privilegi](https://github.com/aspnet/Announcements/issues/295)
