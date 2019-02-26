---
title: Introduzione a ASP.NET Core
author: rick-anderson
description: 'Introduzione ad ASP.NET Core, un framework multipiattaforma, ad alte prestazioni, open source per la compilazione di applicazioni moderne basate sul cloud, connesse a Internet.'
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: index
---
# <a name="introduction-to-aspnet-core"></a>Introduzione a ASP.NET Core

Di [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core è un framework multipiattaforma, ad alte prestazioni, [open source](https://github.com/aspnet/home) per la compilazione di moderne applicazioni basate sul cloud, connesse a Internet. Con ASP.NET Core, è possibile:

* Compilare app web e servizi, app [IoT](https://www.microsoft.com/internet-of-things/) e back-end per dispositivi mobili.
* Usare gli strumenti di sviluppo preferiti in Windows, macOS e Linux.
* Distribuire nel cloud o in locale.
* Eseguire in [.NET Core o .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Perché usare ASP.NET Core?

Milioni di sviluppatori hanno usato, e continuano a usare, [ASP.NET 4.x](/aspnet/overview) per creare app Web. ASP.NET Core è una riprogettazione di ASP.NET 4.x, con modifiche a livello di architettura che comportano un framework più efficiente e modulare.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Compilare API web e interfaccia utente web tramite ASP.NET Core MVC

ASP.NET Core MVC offre funzionalità per la compilazione di [API Web](xref:tutorials/first-web-api) e [app Web](xref:tutorials/razor-pages/index):

* Il [Model-View-Controller (MVC)](xref:mvc/overview) consente di rendere le API web e le app web testabili.
* [Razor Pages](xref:razor-pages/index) è un modello di programmazione basato su pagine che rende la creazione di un'interfaccia utente Web più semplice ed efficace.
* [Il markup Razor](xref:mvc/views/razor) offre una sintassi produttiva per le [pagine Razor](xref:razor-pages/index) e le [visualizzazioni MVC](xref:mvc/views/overview).
* Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.
* Il supporto incorporato per [più formati di dati e per la negoziazione del contenuto](xref:web-api/advanced/formatting) consente alle API web di raggiungere una vasta gamma di client, inclusi i browser e i dispositivi mobili.
* L'[associazione di modelli](xref:mvc/models/model-binding) esegue automaticamente il mapping dei dati dalle richieste HTTP ai parametri del metodo di azione.
* La [convalida dei modelli](xref:mvc/models/validation) esegue automaticamente la convalida sul lato server e sul lato client.

## <a name="client-side-development"></a>Sviluppo lato client

ASP.NET Core si integra perfettamente con framework e librerie lato client di grande diffusione, tra cui [Razor Components](xref:razor-components/index), [Angular](xref:spa/angular), [React](xref:spa/react) e [Bootstrap](https://getbootstrap.com/). Per altre informazioni, vedere [Razor Components](xref:razor-components/index) e gli argomenti correlati in *Sviluppo lato client*.

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a>ASP.NET Core per .NET Framework

ASP.NET Core 2.x può avere come destinazione .NET Core o .NET Framework. Le app ASP.NET Core destinate a .NET Framework non sono multipiattaforma, ma funzionano solo in Windows. ASP.NET Core 2.x è costituito a livello generale da librerie [.NET Standard](/dotnet/standard/net-standard). Le app scritte con .NET 2.0 Standard funzionano ovunque sia supportato .NET Standard 2.0.

ASP.NET Core 2.x è supportato nelle versioni di .NET Framework compatibili con .NET Standard 2.0:

* È consigliabile usare .NET Framework 4.7.1 e versioni successive.
* .NET Framework 4.6.1 e versioni successive.

L'esecuzione di ASP.NET Core 3.0 e versioni successive sarà consentita solo in .NET Core. Per altri dettagli riguardanti questa modifica, vedere [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/) (Una prima occhiata alle modifiche previste per ASP.NET Core 3.0).

Usare .NET Core come destinazione offre diversi vantaggi, che aumentano con ogni versione. Alcuni vantaggi di .NET Core in .NET Framework sono:

* Funzionamento multipiattaforma. Esecuzione con macOS, Linux e Windows.
* Miglioramento delle prestazioni
* Controllo delle versioni side-by-side
* Nuove API
* Open source

È in corso un'intensa attività volta a colmare il divario da .NET Framework a .NET Core relativo alle API. [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) ha reso disponibili in .NET Core migliaia di API solo per Windows. Queste API non erano disponibili in .NET Core 1. x.

## <a name="recommended-learning-path"></a>Percorso di apprendimento consigliato

Per un'introduzione allo sviluppo delle app ASP.NET Core, è consigliabile eseguire la sequenza di esercitazioni e articoli seguente:

1. Eseguire un'esercitazione relativa al tipo di app che si vuole sviluppare o gestire:

   |Tipo di app  |Scenario  |Esercitazione  |
   |----------|----------|----------|
   |App Web       | Per un nuovo sviluppo        |[Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start) |
   |App Web       | Per gestire un'app MVC |[Introduzione a MVC](xref:tutorials/first-mvc-app/start-mvc)|
   |API Web       |                            |[Creare un'API Web](xref:tutorials/first-web-api)\*  |
   |App in tempo reale |                            |[Introduzione a SignalR](xref:tutorials/signalr) |

1. Eseguire un'esercitazione che illustra la procedura di accesso ai dati di base:

   |Scenario  |Esercitazione  |
   |----------|----------|
   | Per un nuovo sviluppo        |[Razor Pages con Entity Framework Core](xref:data/ef-rp/intro) |
   | Per gestire un'app MVC |[MVC con Entity Framework Core](xref:data/ef-mvc/intro)

1. Leggere una panoramica delle funzionalità di ASP.NET Core applicabili a tutti i tipi di app:

   * [Concetti fondamentali](xref:fundamentals/index)

1. Esplorare il Sommario per cercare altri argomenti di interesse.

\* È disponibile una nuova [esercitazione sulle API Web da eseguire interamente nel browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core) che non richiede alcuna installazione nell'IDE locale.  Il codice viene eseguito in un'[Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) e per il testing viene usato [curl](https://curl.haxx.se/).

## <a name="how-to-download-a-sample"></a>Come scaricare un esempio

Molti articoli ed esercitazioni includono collegamenti al codice di esempio.

1. [Scaricare il file ZIP del repository ASP.NET](https://codeload.github.com/aspnet/Docs/zip/master).
1. Decomprimere il file *Docs-master.zip*.
1. Usare l'URL nel collegamento di esempio per passare alla directory di esempio.

### <a name="preprocessor-directives-in-sample-code"></a>Direttive per il preprocessore nel codice di esempio

Per illustrare più scenari, le app di esempio usano le istruzioni C# `#define` e `#if-#else/#elif-#endif` per compilare in modo selettivo ed eseguire sezioni diverse del codice di esempio. Per gli esempi che usano questo approccio, impostare l'istruzione `#define` nella parte superiore dei file C# sul simbolo associato allo scenario che si vuole eseguire. Alcuni esempi richiedono l'impostazione del simbolo nella parte superiore di più file per eseguire uno scenario.

Ad esempio, l'elenco di simboli `#define` seguente indica che sono disponibili quattro scenari, ovvero uno scenario per simbolo. La configurazione di esempio corrente esegue lo scenario `TemplateCode`:

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

Per modificare l'esempio in modo che venga eseguito lo scenario `ExpandDefault`, definire il simbolo `ExpandDefault` e lasciare i simboli rimanenti con commento:

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

Per altre informazioni sull'uso delle [direttive del preprocessore C#](/dotnet/csharp/language-reference/preprocessor-directives/) per compilare in modo selettivo le sezioni di codice, vedere [#define (Riferimenti per C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) e [#if (Riferimenti per C#)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).

### <a name="regions-in-sample-code"></a>Aree del codice di esempio

Alcune app di esempio contengono sezioni di codice racchiuse tra le istruzioni C# [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) e [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion). Il sistema di compilazione della documentazione inserisce queste aree negli argomenti della documentazione visualizzabile.  

I nomi delle aree in genere contengono la parola "frammento". L'esempio seguente mostra un'area denominata `snippet_FilterInCode`:

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

Il file markdown dell'argomento fa riferimento al frammento di codice C# precedente con la riga seguente:

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

È possibile ignorare o rimuovere le istruzioni `#region` e `#endregion` che racchiudono il codice. Non modificare il codice all'interno di queste istruzioni se si prevede di eseguire gli scenari di esempio descritti nell'argomento. È possibile modificare il codice durante la sperimentazione con altri scenari.

Per altre informazioni, vedere [Contribuire alla documentazione ASP.NET: Frammenti di codice](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere le seguenti risorse:

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Nozioni fondamentali su ASP.NET Core](xref:fundamentals/index)
* [La trasmissione settimanale della community di ASP.NET](https://live.asp.net/) offre una presentazione dei progressi e dei piani del team. Vengono segnalati nuovi blog e software di terze parti.
