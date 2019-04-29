---
title: Introduzione a Blazor in ASP.NET Core
author: guardrex
description: Esplorare ASP.NET Core Blazor, un modo per creare un'interfaccia utente Web sul lato client interattiva con .NET in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982997"
---
# <a name="introduction-to-blazor"></a>Introduzione a Blazor

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

Introduzione a Blazor

Sviluppare un'interfaccia utente Web interattiva sul lato client con .NET:

* Sviluppare interfacce utente interattive avanzate usando C# invece che JavaScript.
* Condividere la logica dell'app scritta con .NET, sul lato client e sul lato server.
* Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.

Blazor supporta i principali scenari richiesti dalla maggior parte delle app:

* Parametri
* Gestione di eventi
* Associazione dati
* Routing
* Inserimento di dipendenze
* Layout
* Modelli
* Valori a cascata

## <a name="components"></a>Componenti

Un *componente* in Blazor è un elemento dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati. I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile. I componenti possono essere annidati e riutilizzati.

I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come pacchetti NuGet. La classe dei componenti viene in genere scritta sotto forma di pagina di markup di Razor con un'estensione di file *razor*. I componenti di Blazor vengono a volte definiti componenti Razor.

[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#. Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense). Anche le pagine Razor e le viste MVC usano Razor. Diversamente dalle pagine Razor e dalle viste MVC, che sono basate su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente. È possibile usare i componenti Razor specificamente per la composizione e la logica dell'interfaccia utente sul lato client.

Il markup seguente è un esempio di un componente finestra di dialogo personalizzato:

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Quando questo componente viene usato altrove nell'app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) velocizza lo sviluppo con il completamento di sintassi e parametri.

Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser nota come *albero di rendering*, che può quindi essere usata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.

## <a name="blazor-server-side"></a>Blazor sul lato server

Blazor separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente. Blazor sul lato server fornisce il supporto per l'hosting di componenti Razor nel server in un'app ASP.NET Core. Tutti gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.

Il runtime:

* Gestisce l'invio degli eventi dell'interfaccia utente dal browser al server.
* Applica gli aggiornamenti dell'interfaccia utente inviati dal server nel browser dopo l'esecuzione dei componenti.

La connessione usata da Blazor sul lato server per comunicare con il browser viene usata anche per gestire le chiamate di interoperabilità JavaScript.

![Blazor sul lato server esegue codice .NET sul server e interagisce con il modello DOM (Document Object Model) nel client tramite una connessione SignalR](index/_static/blazor-server-side.png)

Per ulteriori informazioni, vedere <xref:blazor/hosting-models#server-side>.

## <a name="blazor-client-side"></a>Blazor sul lato client

Blazor sul lato client è un framework per app a pagina singola per la creazione di app Web sul lato client interattive con .NET. Blazor sul lato client usa standard Web aperti senza plug-in o transpiling del codice. Blazor sul lato client funziona in tutti i Web browser moderni, inclusi quelli per dispositivi mobili.

L'uso di .NET nel browser per lo sviluppo Web sul lato client offre molti vantaggi:

* **Linguaggio C#**: scrivere codice in C# invece che in JavaScript.
* **Ecosistema .NET**: sfruttare l'ecosistema esistente di librerie .NET.
* **Sviluppo per lo stack completo**: condividere la logica sul lato client e server.
* **Velocità e scalabilità**: .NET è stato progettato per prestazioni, affidabilità e sicurezza.
* **Strumenti leader nel settore**: produttività con Visual Studio in Windows, Linux e macOS.
* **Stabilità e coerenza**:  basato su un set comune di linguaggi, framework e strumenti che sono stabili, ricchi di funzionalità e facili da usare.

L'esecuzione di codice .NET all'interno di Web browser è resa possibile da [WebAssembly](http://webassembly.org) (tecnologia nota anche con l'abbreviazione *wasm*). WebAssembly è un standard Web aperto ed è supportato nei Web browser senza plug-in. WebAssembly è un formato bytecode compatto ottimizzato per il download veloce e la velocità massima di esecuzione.

Il codice WebAssembly può accedere a tutte le funzionalità del browser tramite interoperabilità JavaScript. Allo stesso tempo, il codice .NET eseguito tramite WebAssembly viene eseguito nello stesso ambiente sandbox attendibile di JavaScript per evitare azioni dannose nel computer client.

![Blazor sul lato client esegue codice .NET nel browser con WebAssembly.](index/_static/blazor-client-side.png)

Quando un'app Blazor sul lato client viene compilata ed eseguita in un browser:

* I file di codice C# e i file Razor vengono compilati in assembly .NET.
* Gli assembly e il runtime .NET vengono scaricati nel browser.
* Blazor sul lato client esegue il bootstrap del runtime .NET e configura il runtime per caricare gli assembly per l'app. La modifica del modello DOM (Document Object Model) e le chiamate API al browser vengono gestite dal runtime di Blazor sul lato client tramite interoperabilità JavaScript.

Per ridurre le dimensioni dell'app scaricata, il codice inutilizzato viene rimosso dall'app quando viene pubblicata dal [linker Intermediate Language (IL)](xref:host-and-deploy/blazor/configure-linker).

Blazor sul lato client è un modello di hosting lato client. Dato che Blazor separa la logica di rendering di un componente dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente, c'è flessibilità nelle modalità di hosting di Blazor. Usare [Blazor sul lato server](#blazor-server-side) per ospitare Blazor nel server in un'app ASP.NET Core in cui gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR. Per ulteriori informazioni, vedere <xref:blazor/hosting-models#server-side>. 

Le dimensioni del payload sono un fattore cruciale per le prestazioni per l'usabilità dell'app. Blazor sul lato client ottimizza le dimensioni del payload per ridurre i tempi di download:

* Le parti inutilizzate degli assembly .NET vengono rimosse durante il processo di compilazione.
* Le risposte HTTP vengono compresse.
* Il runtime e gli assembly .NET vengono memorizzati nella cache nel browser.

[Blazor sul lato server](#blazor-server-side) offre dimensioni di payload inferiori rispetto a Blazor sul lato client mantenendo assembly .NET, assembly dell'app e runtime sul lato server. Le app Blazor sul lato server forniscono solo file di markup e asset statici ai client.

## <a name="javascript-interop"></a>Interoperabilità JavaScript

Per le app che richiedono librerie JavaScript e API browser di terze parti, i componenti supportano l'interoperabilità con JavaScript. I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript. Il codice C# può effettuare chiamate nel codice JavaScript e vice versa. Per altre informazioni, vedere [Interoperabilità JavaScript](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Condivisione del codice e .NET Standard

Le app possono fare riferimento a librerie [.NET Standard](/dotnet/standard/net-standard) esistenti e usarle. .NET Standard è una specifica formale delle API .NET comuni tra le implementazioni di .NET. Blazor implementa .NET Standard 2.0. Le API non applicabili all'interno di un Web browser (ad esempio, per l'accesso al file system, l'apertura di un socket, la gestione dei thread e altre funzionalità) generano <xref:System.PlatformNotSupportedException>. Le librerie di classi .NET Standard possono essere condivise tra diverse piattaforme .NET, come Blazor, .NET Framework, .NET Core, Xamarin, Mono e Unity.

## <a name="additional-resources"></a>Risorse aggiuntive

* [WebAssembly](http://webassembly.org/)
* [Guida a C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
