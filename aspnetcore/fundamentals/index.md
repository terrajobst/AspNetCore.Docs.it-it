---
title: Nozioni fondamentali su ASP.NET Core
author: rick-anderson
description: Questo articolo offre una panoramica generale dei concetti fondamentali da comprendere nella creazione di applicazioni ASP.NET Core.
keywords: ASP.NET Core, nozioni di base, panoramica
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d8ca35b0e2e4b6e9b1ec745a3a7cf7c3983c461
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-fundamentals-overview"></a>ASP.NET Core, nozioni di base, panoramica

Un'applicazione ASP.NET Core è un'applicazione console che crea un server Web nel suo metodo `Main`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Principale](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

Il metodo `Main` richiama `WebHost.CreateDefaultBuilder`, che segue il modello di generatore per creare un host applicazioni Web. Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`). Nell'esempio precedente, viene allocato automaticamente un server Web [Kestrel](xref:fundamentals/servers/kestrel). L'host Web di ASP.NET Core tenterà l'esecuzione in IIS, se disponibile. Altri server Web, come [HTTP.sys](xref:fundamentals/servers/httpsys), possono essere usati richiamando il metodo di estensione appropriato. `UseStartup` viene spiegato dettagliatamente nella sezione successiva.

`IWebHostBuilder`, il tipo restituito della chiamata `WebHost.CreateDefaultBuilder`, offre molti metodi facoltativi. Alcuni di questi metodi includono `UseHttpSys` per ospitare l'applicazione in HTTP.sys e `UseContentRoot` per specificare la directory del contenuto radice. I metodi `Build` e `Run` generano l'oggetto `IWebHost` che ospiterà l'applicazione e inizierà a essere in ascolto delle richieste HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Principale](../getting-started/sample/aspnetcoreapp/Program.cs)]

Il metodo `Main` usa `WebHostBuilder`, che segue il modello di generatore per creare un host applicazioni Web. Il generatore dispone di metodi che definiscono il server Web (ad esempio, `UseKestrel`) e la classe di avvio (`UseStartup`). Nell'esempio precedente viene usato il server Web [Kestrel](xref:fundamentals/servers/kestrel). Altri server Web, come [WebListener](xref:fundamentals/servers/weblistener), possono essere usati richiamando il metodo di estensione appropriato. `UseStartup` viene spiegato dettagliatamente nella sezione successiva.

`WebHostBuilder` offre molti metodi facoltativi, incluso `UseIISIntegration` per l'hosting in IIS e IIS Express e `UseContentRoot` per specificare la directory del contenuto radice. I metodi `Build` e `Run` generano l'oggetto `IWebHost` che ospiterà l'applicazione e inizierà a essere in ascolto delle richieste HTTP.

---

## <a name="startup"></a>Avvio

Il metodo `UseStartup` su `WebHostBuilder` specifica la classe `Startup` per l'app:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Principale](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Principale](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

Nella classe `Startup` viene definita la pipeline di gestione delle richieste e vengono configurati tutti i servizi necessari per l'applicazione. La classe `Startup` deve essere pubblica e deve contenere i metodi seguenti:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* `ConfigureServices` definisce i [servizi](#services) usati dall'applicazione in uso (come ASP.NET Core MVC, Entity Framework Core, Identity, ecc.).

* `Configure` definisce il [middleware](xref:fundamentals/middleware) nella pipeline delle richieste.

Per altre informazioni, vedere [Avvio dell'applicazione](xref:fundamentals/startup).

## <a name="services"></a>Servizi

Un servizio è un componente destinato a un utilizzo comune in un'applicazione. I servizi sono resi disponibili tramite l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection). ASP.NET Core include un'inversione nativa del contenitore del controllo (IoC) che supporta l'[inserimento del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection) per impostazione predefinita. Il contenitore nativo può essere sostituito dal contenitore scelto dall'utente. Oltre al suo ampio beneficio di accoppiamento, l'inserimento delle dipendenze rende disponibili i servizi in tutta l'applicazione. Ad esempio, la [registrazione](xref:fundamentals/logging) è disponibile in tutta l'applicazione.

Per altre informazioni, vedere [Dependency injection](xref:fundamentals/dependency-injection) (Inserimento delle dipendenze).

## <a name="middleware"></a>Middleware

In ASP.NET Core, la richiesta di pipeline viene composta usando [Middleware](xref:fundamentals/middleware). ASP.NET Core middleware esegue la logica asincrona su un `HttpContext`, quindi richiama il middleware successivo nella sequenza oppure termina la richiesta direttamente. Viene aggiunto un componente del middleware denominato "XYZ" richiamando un metodo di estensione `UseXYZ` nel metodo `Configure`.

ASP.NET Core viene fornito con un'ampia gamma di middleware incorporato:

* [File statici](xref:fundamentals/static-files)

* [Routing](xref:fundamentals/routing)

* [Autenticazione](xref:security/authentication/index)

È possibile utilizzare qualunque middleware basato su [OWIN](http://owin.org) con ASP.NET Core ed è possibile scrivere il proprio middleware personalizzato.

Per altre informazioni, vedere [Middleware](xref:fundamentals/middleware) e [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) (Interfaccia Web aperta per .NET (OWIN).

## <a name="servers"></a>Server

Il modello di hosting di ASP.NET Core non è direttamente in ascolto delle richieste; si basa piuttosto su un'implementazione del server HTTP per inoltrare la richiesta all'applicazione. La richiesta inoltrata viene inclusa come un insieme di oggetti di funzionalità cui è possibile accedere tramite le interfacce. L'applicazione compone questo insieme in un `HttpContext`. ASP.NET Core include un server Web gestito, multipiattaforma, denominato [Kestrel](xref:fundamentals/servers/kestrel). Kestrel viene eseguito in genere protetto da un server Web di produzione come [IIS](https://www.iis.net/) o [nginx](http://nginx.org).

Per altre informazioni, vedere [Server](xref:fundamentals/servers/index) e [Hosting](xref:fundamentals/hosting).

## <a name="content-root"></a>Radice del contenuto

La radice del contenuto è il percorso di base per i contenuti usati dall'applicazione, ad esempio viste, [Razor Pages](xref:mvc/razor-pages/index)e asset statici. Per impostazione predefinita, la radice del contenuto è uguale al percorso di base dell'applicazione per il file eseguibile che ospita l'applicazione. Viene specificato un percorso alternativo per la radice del contenuto con `WebHostBuilder`.

## <a name="web-root"></a>Radice Web

La radice Web di un'applicazione è la directory del progetto contenente risorse statiche pubbliche, come CSS, JavaScript e i file di immagine. Per impostazione predefinita, il middleware dei file statici verrà usato solo dai file della directory radice Web e relative sottodirectory. Per altre informazioni, vedere [Uso di file statici](xref:fundamentals/static-files). Il percorso predefinito della radice Web è */wwwroot*, ma è possibile specificare un percorso diverso tramite `WebHostBuilder`.

## <a name="configuration"></a>Configurazione

ASP.NET Core usa un nuovo modello di configurazione per la gestione di coppie nome-valore semplici. Il nuovo modello di configurazione non è basato su `System.Configuration` o *web.config*, ma deriva invece da un set ordinato di provider di configurazione. I provider di configurazione integrati supportano un'ampia gamma di formati di file (XML, JSON, INI) e di variabili di ambiente per abilitare la configurazione basata sull'ambiente. È anche possibile scrivere i propri provider di configurazione personalizzata.

Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration).

## <a name="environments"></a>Ambienti

Gli ambienti come "Sviluppo" e "Produzione" sono un concetto fondamentale in ASP.NET Core e possono essere impostati usando variabili di ambiente.

Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).

## <a name="net-core-vs-net-framework-runtime"></a>Runtime .NET core e .NET Framework

Un'applicazione ASP.NET Core può essere finalizzata al runtime di .NET Core o .NET Framework. Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="additional-information"></a>Informazioni aggiuntive

Vedere anche gli argomenti seguenti:

- [Gestione degli errori](xref:fundamentals/error-handling)
- [Provider di file](xref:fundamentals/file-providers)
- [Globalizzazione e localizzazione](xref:fundamentals/localization)
- [Registrazione](xref:fundamentals/logging)
- [Gestione dello stato di un'applicazione](xref:fundamentals/app-state)
