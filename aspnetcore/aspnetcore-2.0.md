---
title: "Novità di ASP.NET Core 2.0"
author: rick-anderson
description: "Novità di ASP.NET Core 2.0"
keywords: "ASP.NET Core, note sulla versione, novità"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: 08c9f457-9c24-40f9-a08b-47dc251e4cec
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-2.0
ms.openlocfilehash: f0061fe483bdd5d46e776f9c8b726078cf92ff3b
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="whats-new-in-aspnet-core-20"></a>Novità di ASP.NET Core 2.0

Questo articolo evidenzia le modifiche più significative apportate ad ASP.NET Core 2.0, con collegamenti alla relativa documentazione.

## <a name="razor-pages"></a>Razor Pages

Razor Pages è una nuova funzionalità di ASP.NET Core MVC che semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine.

Per altre informazioni, vedere l'introduzione e l'esercitazione:

* [Introduzione a Razor Pages in ASP.NET Core](xref:mvc/razor-pages/index)
* [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) (Guida introduttiva a Razor Pages)

## <a name="aspnet-core-metapackage"></a>Metapacchetto ASP.NET Core

Un nuovo metapacchetto ASP.NET Core include tutti i pacchetti creati e supportati dai team di ASP.NET Core ed Entity Framework Core, con le relative dipendenze interne e di terze parti. Non è più necessario scegliere le singole funzionalità di ASP.NET Core in base al pacchetto. Tutte le funzionalità sono incluse nel pacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All). I modelli predefiniti usano questo pacchetto.

Per altre informazioni, vedere [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0](xref:fundamentals/metapackage) (Metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2.0).

## <a name="runtime-store"></a>Archivio di runtime

Le applicazioni che usano il metapacchetto `Microsoft.AspNetCore.All` sfruttano automaticamente il nuovo archivio di runtime di .NET Core. L'archivio contiene tutti gli asset di runtime necessari per eseguire le applicazioni ASP.NET Core 2.0. Quando si usa il metapacchetto `Microsoft.AspNetCore.All`, con l'applicazione non vengono distribuiti asset dai pacchetti NuGet di riferimento di ASP.NET Core poiché sono già presenti nel sistema di destinazione. Gli asset contenuti nell'archivio di runtime vengono anche precompilati per migliorare i tempi di avvio dell'applicazione.

Per altre informazioni, vedere l'articolo relativo all'[archivio dei pacchetti di runtime](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)

## <a name="net-standard-20"></a>.NET Standard 2.0

I pacchetti di ASP.NET Core 2.0 hanno come destinazione .NET Standard 2.0. Ai pacchetti possono fare riferimento altre librerie .NET Standard 2.0 ed è possibile eseguire i pacchetti in implementazioni di .NET compatibili con .NET Standard 2.0, tra cui .NET Core 2.0 e .NET Framework 4.6.1. 

Il metapacchetto `Microsoft.AspNetCore.All` è riservato esclusivamente a .NET Core 2.0, poiché è destinato all'uso con l'archivio di runtime di .NET Core 2.0.

## <a name="configuration-update"></a>Aggiornamento della configurazione

In ASP.NET Core 2.0 viene aggiunta per impostazione predefinita un'istanza di `IConfiguration` al contenitore dei servizi. `IConfiguration` nel contenitore dei servizi semplifica per le applicazioni il recupero dei valori di configurazione dal contenitore.

Per informazioni sullo stato della documentazione prevista, vedere l'[argomento su GitHub](https://github.com/aspnet/Docs/issues/3387).

## <a name="logging-update"></a>Aggiornamento della registrazione

In ASP.NET Core 2.0 la registrazione è incorporata nel sistema di inserimento delle dipendenze per impostazione predefinita. Si aggiungono i provider e si configurano i filtri nel file *Program.cs* anziché nel file *Startup.cs*. E l'oggetto `ILoggerFactory` predefinito supporta i filtri in modo tale da consentire l'uso di un unico approccio flessibile sia per il filtraggio tra provider, sia per il filtraggio di un provider specifico.

Per altre informazioni, vedere l'[introduzione alla registrazione in ASP.NET Core](xref:fundamentals/logging).

## <a name="authentication-update"></a>Aggiornamento dell'autenticazione

Un nuovo modello di autenticazione rende più semplice configurare l'autenticazione per un'applicazione che usa l'inserimento delle dipendenze.

I nuovi modelli sono disponibili per la configurazione dell'autenticazione per le applicazioni Web e le API Web che usano [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).

Per informazioni sullo stato della documentazione prevista, vedere l'[argomento su GitHub](https://github.com/aspnet/Docs/issues/3054).

## <a name="identity-update"></a>Aggiornamento dell'identità

La compilazione di API Web sicure è stata semplificata usando l'identità in ASP.NET 2.0 Core. È possibile acquisire i token di accesso per accedere alle API Web usando la libreria di autenticazione [MSAL](https://www.nuget.org/packages/Microsoft.Identity.Client).

Per altre informazioni sulle modifiche apportate all'autenticazione nella versione 2.0, vedere le risorse seguenti:

* [Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)
* [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) (Abilitazione della generazione di codice a matrice per le app di autenticazione in ASP.NET Core)
* [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x) (Migrazione di autenticazione e identità ad ASP.NET Core 2.0)

## <a name="spa-templates"></a>Modelli SPA

Sono disponibili modelli di progetto di applicazione a pagina singola, o SPA (Single-Page Application), per Angular, Aurelia, Knockout.js, React.js e React.js con Redux. Il modello Angular è stato aggiornato alla versione 4. I modelli Angular e React sono disponibili per impostazione predefinita. Per informazioni su come ottenere gli altri modelli, vedere la sezione relativa alla [creazione di un nuovo progetto SPA](xref:client-side/spa-services#creating-a-new-project). Per informazioni su come compilare un'applicazione a pagina singola in ASP.NET Core, vedere [Using JavaScriptServices for Creating Single Page Applications](xref:client-side/spa-services) (Uso di JavaScriptServices per la creazione di applicazioni a pagina singola).

## <a name="kestrel-improvements"></a>Miglioramenti di Kestrel

Il server Web Kestrel include nuove funzionalità che lo rendono più adatto all'uso come server con connessione Internet. Sono state aggiunte diverse opzioni di configurazione dei vincoli in una nuova proprietà della classe `KestrelServerOptions`, `Limits`. È ora possibile aggiungere limiti per gli elementi seguenti:

- Numero massimo di connessioni client
- Dimensione massima del corpo della richiesta
- Velocità minima dei dati del corpo della richiesta

Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel).

## <a name="weblistener-renamed-to-httpsys"></a>WebListener rinominato in HTTP.sys

I pacchetti `Microsoft.AspNetCore.Server.WebListener` e `Microsoft.Net.Http.Server` sono state uniti in un nuovo pacchetto `Microsoft.AspNetCore.Server.HttpSys`. Gli spazi dei nomi sono stati aggiornati di conseguenza.

Per altre informazioni, vedere l'introduzione all'[implementazione del server Web HTTP.sys in ASP.NET Core](xref:fundamentals/servers/httpsys).

## <a name="enhanced-http-header-support"></a>Supporto ottimizzato per le intestazioni HTTP

Quando si usa MVC per trasmettere un oggetto `FileStreamResult` o `FileContentResult`, è ora possibile impostare un `ETag` o una data `LastModified` per il contenuto trasmesso. È possibile impostare questi valori per il contenuto restituito con codice simile al seguente:

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

Il file restituito ai visitatori verrà decorato con le intestazioni HTTP appropriate per i valori `ETag` e `LastModified`.

Se il visitatore di un'applicazione richiede contenuto con un'intestazione di richiesta di intervallo, ASP.NET lo riconosce e gestisce l'intestazione. Se il contenuto richiesto può essere recapitato parzialmente, ASP.NET ignora e considera le varie parti in modo appropriato e restituisce solo il set di byte richiesto.  Non è necessario scrivere gestori speciali nei metodi per adattare o gestire questa funzionalità, poiché è gestita automaticamente.

## <a name="hosting-startup-and-application-insights"></a>Avvio in hosting e Application Insights

Gli ambienti di hosting sono ora in grado di inserire le dipendenze aggiuntive dei pacchetti e di eseguire codice durante l'avvio dell'applicazione, senza che per l'applicazione sia necessario accettare una dipendenza o chiamare metodi in modo esplicito. Questa funzionalità può essere usata per consentire ad alcuni ambienti di attivare le proprie funzionalità esclusive senza che sia necessario indicarlo anticipatamente all'applicazione. 

In ASP.NET Core 2.0 questa funzionalità viene usata per abilitare automaticamente la diagnostica di Application Insights durante il debug in Visual Studio e, dopo aver scelto questa opzione, durante l'esecuzione in Servizi app di Azure. Di conseguenza, i modelli di progetto non aggiungono più i pacchetti e il codice di Application Insights per impostazione predefinita.

Per informazioni sullo stato della documentazione prevista, vedere l'[argomento su GitHub](https://github.com/aspnet/Docs/issues/3389).

## <a name="automatic-use-of-anti-forgery-tokens"></a>Uso automatico dei token antifalsificazione

ASP.NET Core ha sempre agevolato la codifica HTML del contenuto per impostazione predefinita, ma con la nuova versione è stato fatto un passo avanti nella prevenzione degli attacchi basati sulla falsificazione della richiesta tra siti (XSRF). ASP.NET Core ora genera token antifalsificazione per impostazione predefinita e li convalida per le pagine e le azioni POST dei form senza configurazione aggiuntiva.

Per altre informazioni, vedere [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](xref:security/anti-request-forgery) (Prevenzione degli attacchi di falsificazione della richiesta tra siti (XSRF/CSRF) in ASP.NET Core).

## <a name="automatic-precompilation"></a>Precompilazione automatica

La precompilazione delle visualizzazioni Razor è abilitata durante la pubblicazione per impostazione predefinita, riducendo le dimensioni dell'output di pubblicazione e il tempo di avvio dell'applicazione.

## <a name="razor-support-for-c-71"></a>Supporto Razor per C# 7.1

Il motore di visualizzazione Razor è stato aggiornato in modo da funzionare con il nuovo compilatore Roslyn. Questo include il supporto per funzionalità di C# 7.1 tra cui le espressioni predefinite, i nomi di tupla dedotti e i criteri di ricerca con i generics. Per usare C# 7.1 nel progetto, aggiungere la proprietà seguente nel file di progetto e quindi ricaricare la soluzione:

```xml
<LangVersion>latest</LangVersion>
```

Per informazioni sullo stato delle funzionalità di C# 7.1, vedere il [repository GitHub per Roslyn](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).

## <a name="other-documentation-updates-for-20"></a>Altri aggiornamenti alla documentazione per la versione 2.0

* [Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs) (Creare profili di pubblicazione per Visual Studio e MSBuild, per distribuire applicazioni ASP.NET Core)
* [Key Management](xref:security/data-protection/implementation/key-management) (Gestione delle chiavi)
* [Configuring Facebook authentication](xref:security/authentication/facebook-logins) (Configurazione dell'autenticazione di Facebook)
* [Configuring Twitter authentication](xref:security/authentication/twitter-logins) (Configurazione dell'autenticazione di Twitter)
* [Configuring Google authentication](xref:security/authentication/google-logins) (Configurazione dell'autenticazione di Google)
* [Configuring Microsoft Account authentication](xref:security/authentication/microsoft-logins) (Configurazione dell'autenticazione account Microsoft)
* [Setting up HTTPS for development in ASP.NET Core](xref:security/https) (Impostazione di HTTPS per lo sviluppo in ASP.NET Core)

## <a name="migration-guidance"></a>Materiale sussidiario di migrazione

Per indicazioni su come eseguire la migrazione delle applicazioni ASP.NET Core 1.x ad ASP.NET Core 2.0, vedere le risorse seguenti:

* [Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0](xref:migration/1x-to-2x/index) (Migrazione da ASP.NET Core 1.x ad ASP.NET Core 2.0)
* [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x) (Migrazione di autenticazione e identità ad ASP.NET Core 2.0)

## <a name="additional-information"></a>Informazioni aggiuntive

Per l'elenco completo delle modifiche, vedere le [note sulla versione di ASP.NET Core 2.0](https://github.com/aspnet/Home/releases/tag/2.0.0).

Per essere aggiornati sull'avanzamento del lavoro e sui piani dei team di sviluppo di ASP.NET Core, partecipare alle riunioni settimanali in [ASP.NET Community Standup](https://live.asp.net/).
