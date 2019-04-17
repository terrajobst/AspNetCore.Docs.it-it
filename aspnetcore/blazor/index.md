---
title: Introduzione a Blazor in ASP.NET Core
author: guardrex
description: Esplorare ASP.NET Core Blazor, un modo per creare un'interfaccia utente Web sul lato client interattiva con .NET in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614597"
---
# <a name="introduction-to-blazor"></a>Introduzione a Blazor

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

## <a name="razor-components"></a>Razor Components

Il modello di hosting sul lato server di Blazor, *Razor Components*, è un modo per sviluppare un'interfaccia utente Web sul lato client interattiva con .NET:

* Sviluppare interfacce utente interattive avanzate usando C# invece che JavaScript.
* Condividere la logica dell'app, interamente scritta con .NET, sul lato client e sul lato server.
* Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.

Razor Components supporta gli elementi fondamentali richiesti dalla maggior parte delle app, tra cui:

* Parametri
* Gestione di eventi
* Associazione dati
* Routing
* Inserimento di dipendenze
* Layout
* Modelli
* Valori a cascata

Razor Components separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente. ASP.NET Core Razor Components in .NET Core 3.0 aggiunge il supporto per l'hosting di Razor Components nel server in un'app ASP.NET Core. Tutti gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.

Il runtime:

* Gestisce l'invio degli eventi dell'interfaccia utente dal browser al server.
* Applica gli aggiornamenti dell'interfaccia utente inviati dal server nel browser dopo l'esecuzione dei componenti.

La connessione usata da Razor Components per comunicare con il browser viene usata anche per gestire le chiamate di interoperabilità JavaScript.

![Razor Components esegue codice .NET sul server e interagisce con il modello DOM (Document Object Model) nel client tramite una connessione SignalR](index/_static/aspnet-core-razor-components.png)

Per ulteriori informazioni, vedere <xref:blazor/hosting-models#server-side-hosting-model>.

## <a name="components"></a>Componenti

Un *componente Razor* è una parte dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati. I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile. I componenti possono essere annidati e riutilizzati.

I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come pacchetti NuGet. La classe viene in genere scritta in forma di pagina di markup Razor con estensione di file *razor* (Razor Components) o di pagina di markup Razor con estensione *cshtml* (Blazor).

[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#. Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense). Anche le pagine Razor e le viste MVC usano Razor. Diversamente dalle pagine Razor e dalle viste MVC, che sono basate su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente. È possibile usare Razor Components in modo specifico per la composizione e la logica dell'interfaccia utente sul lato client.

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

Quando questo componente viene usato in altre posizioni nell'app, IntelliSense accelera lo sviluppo con il completamento di sintassi e parametri.

Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser nota come *albero di rendering*, che può quindi essere usata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.

## <a name="javascript-interop"></a>Interoperabilità JavaScript

Per le app che richiedono librerie JavaScript e API browser di terze parti, i componenti supportano l'interoperabilità con JavaScript. I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript. Il codice C# può effettuare chiamate nel codice JavaScript e vice versa. Per altre informazioni, vedere [Interoperabilità JavaScript](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Condivisione del codice e .NET Standard

Le app possono fare riferimento a librerie [.NET Standard](/dotnet/standard/net-standard) esistenti e usarle. .NET Standard è una specifica formale delle API .NET comuni tra le implementazioni di .NET. Blazor implementa .NET Standard 2.0. Le API non applicabili all'interno di un Web browser (ad esempio, per l'accesso al file system, l'apertura di un socket, la gestione dei thread e altre funzionalità) generano <xref:System.PlatformNotSupportedException>. Le librerie di classi .NET Standard possono essere condivise tra diverse piattaforme .NET, come Blazor, .NET Framework, .NET Core, Xamarin, Mono e Unity.

## <a name="blazor"></a>Blazor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor è un framework per app a pagina singola per la creazione di app Web sul lato client interattive con .NET. Blazor usa gli standard Web aperti senza plug-in o transpiling del codice. Blazor funziona in tutti i Web browser moderni, inclusi i browser per dispositivi mobili.

L'uso di .NET nel browser per lo sviluppo Web sul lato client offre molti vantaggi:

* **Linguaggio C#**: scrivere codice in C# invece che in JavaScript.
* **Ecosistema .NET**: sfruttare l'ecosistema esistente di librerie .NET.
* **Sviluppo per lo stack completo**: condividere la logica sul lato client e server.
* **Velocità e scalabilità**: .NET è stato progettato per prestazioni, affidabilità e sicurezza.
* **Strumenti leader nel settore**: produttività con Visual Studio in Windows, Linux e macOS.
* **Stabilità e coerenza**:  basato su un set comune di linguaggi, framework e strumenti che sono stabili, ricchi di funzionalità e facili da usare.

L'esecuzione di codice .NET all'interno di Web browser è resa possibile da [WebAssembly](http://webassembly.org) (tecnologia nota anche con l'abbreviazione *wasm*). WebAssembly è un standard Web aperto ed è supportato nei Web browser senza plug-in. WebAssembly è un formato bytecode compatto ottimizzato per il download veloce e la velocità massima di esecuzione.

Il codice WebAssembly può accedere a tutte le funzionalità del browser tramite interoperabilità JavaScript. Allo stesso tempo, il codice .NET eseguito tramite WebAssembly viene eseguito nello stesso ambiente sandbox attendibile di JavaScript per evitare azioni dannose nel computer client.

![Blazor esegue codice .NET nel browser con WebAssembly.](index/_static/blazor.png)

Quando un'app Blazor viene compilata ed eseguita in un browser:

* I file di codice C# e i file Razor vengono compilati in assembly .NET.
* Gli assembly e il runtime .NET vengono scaricati nel browser.
* Blazor esegue il bootstrap del runtime .NET e configura il runtime per caricare gli assembly per l'app. La modifica del modello DOM (Document Object Model) e le chiamate API al browser vengono gestite dal runtime Blazor tramite interoperabilità JavaScript.

Blazor supporta gli elementi fondamentali richiesti dalla maggior parte delle app, tra cui:

* Parametri
* Gestione di eventi
* Associazione dati
* Routing
* Inserimento di dipendenze
* Layout
* Modelli
* Valori a cascata

Per ridurre le dimensioni dell'app scaricata, il codice inutilizzato viene rimosso dall'app quando viene pubblicata dal [linker Intermediate Language (IL)](xref:host-and-deploy/blazor/configure-linker).

Blazor sul lato client è un modello di hosting lato client. Dato che Blazor separa la logica di rendering di un componente dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente, c'è flessibilità nelle modalità di hosting di Blazor. Usare ASP.NET Core [Razor Components](#razor-components) per ospitare Razor Components nel server in un'app ASP.NET Core in cui gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR. Per ulteriori informazioni, vedere <xref:blazor/hosting-models#server-side-hosting-model>. 

Le dimensioni del payload sono un fattore cruciale per le prestazioni per l'usabilità dell'app. Blazor consente di ottimizzare le dimensioni del payload per ridurre i tempi di download:

* Le parti inutilizzate degli assembly .NET vengono rimosse durante il processo di compilazione.
* Le risposte HTTP vengono compresse.
* Il runtime e gli assembly .NET vengono memorizzati nella cache nel browser.

[Razor Components](#razor-components) offre dimensioni di payload anche inferiori rispetto a Blazor grazie alla gestione degli assembly .NET, dell'assembly dell'app e del runtime sul lato server. Le app realizzate con Razor Components forniscono solo file di markup e asset statici ai client.

## <a name="additional-resources"></a>Risorse aggiuntive

* [WebAssembly](http://webassembly.org/)
* [Guida a C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
