---
title: Introduzione a Razor Components
author: guardrex
description: Esplorare Blazor, un framework Web .NET con C#/Razor e HTML che viene eseguito nel browser con WebAssembly.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667941"
---
# <a name="introduction-to-razor-components"></a>Introduzione a Razor Components

Di [Steve Sanderson](http://blog.stevensanderson.com), [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

*ASP.NET Core Razor Components* viene eseguito sul lato server in ASP.NET Core, mentre *Blazor* (la versione di Razor Components eseguita sul lato client) è un framework Web .NET sperimentale che usa C#/Razor e HTML e viene eseguito nel browser con WebAssembly. Blazor offre tutti i vantaggi di un framework dell'interfaccia utente Web sul lato client con .NET nel client.

Lo sviluppo di applicazioni Web è migliorato in molti modi nel corso degli anni, ma la creazione di app Web moderne comporta ancora varie sfide. L'uso di .NET nel browser offre molti vantaggi che contribuiscono a rendere lo sviluppo Web più semplice e produttivo:

* **Stabilità e coerenza**: .NET offre framework di programmazione standardizzati multipiattaforma stabili, ricchi di funzionalità e facili da usare.
* **Linguaggi innovativi moderni**: i linguaggi .NET migliorano costantemente grazie a nuove funzionalità innovative.
* **Strumenti leader nel settore**: la famiglia di prodotti Visual Studio offre un'eccellente esperienza di sviluppo .NET per varie piattaforme in Windows, Linux e macOS.
* **Velocità e scalabilità**: .NET può contare su una tradizione consolidata di prestazioni, affidabilità e sicurezza per lo sviluppo di app. L'uso di .NET come soluzione per stack completo facilita la creazione di app veloci, affidabili e sicure.
* **Sviluppo per stack completo che sfrutta le competenze esistenti**: gli sviluppatori C#/Razor usano le competenze esistenti per C#/Razor per scrivere codice sul lato client e condividere la logica sul lato server e client tra le app.
* **Ampio supporto di browser**: Razor Components esegue il rendering dell'interfaccia utente come markup ordinario e JavaScript. Blazor viene eseguito in .NET nel browser tramite gli standard Web aperti senza alcun plug-in e transpile del codice. Blazor funziona in tutti i Web browser moderni, inclusi i browser per dispositivi mobili.

## <a name="hosting-models"></a>Modelli di hosting

### <a name="server-side-hosting-model"></a>Modello di hosting sul lato server

Dato che Razor Components separa la logica di rendering di un componente dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente, c'è flessibilità nelle modalità di hosting di Razor Components. ASP.NET Core Razor Components in .NET Core 3.0 aggiunge il supporto per l'hosting di Razor Components nel server in un'app ASP.NET Core in cui tutti gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR. Il runtime gestisce l'invio degli eventi dell'interfaccia utente dal browser al server e quindi applica gli aggiornamenti dell'interfaccia utente inviati dal server nel browser dopo l'esecuzione dei componenti. La stessa connessione viene usata anche per gestire le chiamate di interoperabilità JavaScript.

![Razor Components esegue codice .NET sul server e interagisce con il modello DOM (Document Object Model) nel client tramite una connessione SignalR](index/_static/aspnet-core-razor-components.png)

Per ulteriori informazioni, vedere <xref:razor-components/hosting-models#server-side-hosting-model>.

### <a name="client-side-hosting-model"></a>Modello di hosting sul lato client

L'esecuzione di codice .NET all'interno di Web browser è resa possibile da una tecnologia relativamente nuova, [WebAssembly](http://webassembly.org) (abbreviata *wasm*). WebAssembly è un standard Web aperto ed è supportato nei Web browser senza plug-in. WebAssembly è un formato bytecode compatto ottimizzato per il download veloce e la velocità massima di esecuzione.

Il codice WebAssembly può accedere a tutte le funzionalità del browser tramite interoperabilità JavaScript. Allo stesso tempo, il codice WebAssembly viene eseguito nello stesso ambiente sandbox attendibile di JavaScript per evitare azioni dannose nel computer client.

![Blazor esegue codice .NET nel browser con WebAssembly.](index/_static/blazor.png)

Quando un'app Blazor viene compilata ed eseguita in un browser:

1. I file di codice C# e i file Razor vengono compilati in assembly .NET.
1. Gli assembly e il runtime .NET vengono scaricati nel browser.
1. Blazor usa JavaScript per il bootstrap del runtime .NET e configura il runtime per caricare i riferimenti agli assembly necessari. La modifica del modello DOM (Document Object Model) e le chiamate API al browser vengono gestite dal runtime Blazor tramite interoperabilità JavaScript.

Per supportare i browser precedenti che non supportano WebAssembly, è possibile usare il [modello di hosting sul lato server](#server-side-hosting-model) di ASP.NET Core Razor Components.

Per ulteriori informazioni, vedere <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Componenti

Le app vengono realizzate con *componenti*. Un componente è una parte dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati. I componenti possono essere annidati, riutilizzati e condivisi tra progetti.

Un *componente* è una classe .NET. La classe può essere scritta direttamente, come classe C# (*\*.cs*), o più comunemente in forma di pagina di markup Razor (*\*cshtml*).

[Razor](/aspnet/core/mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#. Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense). Il markup seguente è un esempio di un componente finestra di dialogo personalizzata semplice in un file Razor (*DialogComponent.cshtml*):

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Quando questo componente viene usato in altre posizioni nell'app, IntelliSense accelera lo sviluppo con il completamento di sintassi e parametri.

I componenti possono essere:

* Annidati.
* Creati con codice Razor (*\*cshtml*) o C# (*\*cs*).
* Condivisi tramite le librerie di classi.
* Sottoposti a unit test senza richiedere un DOM del browser.

## <a name="infrastructure"></a>Infrastruttura

Razor Components offre le funzionalità principali richieste dalla maggior parte delle app, tra cui:

* Layout
* Routing
* Inserimento di dipendenze

Tutte queste funzionalità sono facoltative. Quando una di queste funzionalità non viene usata in un'app, l'implementazione viene rimossa dall'app al momento della pubblicazione tramite il [linker di linguaggio intermedio (IL)](xref:host-and-deploy/razor-components/configure-linker).

## <a name="code-sharing-and-net-standard"></a>Condivisione del codice e .NET Standard

Le app possono fare riferimento a librerie [.NET Standard](/dotnet/standard/net-standard) esistenti e usarle. .NET Standard è una specifica formale delle API .NET comuni tra le implementazioni di .NET. Sono supportati .NET standard 2.0 o versioni successive. Le API non applicabili all'interno di un Web browser (ad esempio, per l'accesso al file system, l'apertura di un socket, la gestione dei thread e altre funzionalità) generano [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception). Le librerie di classi .NET Standard possono essere condivise tra il codice sul lato server e nelle app basate su browser.

## <a name="javascript-interop"></a>Interoperabilità JavaScript

Per le app che richiedono librerie JavaScript e API browser di terze parti, WebAssembly è progettato per l'interoperabilità con JavaScript. Razor Components è in grado di usare qualsiasi libreria o API supportata da JavaScript. Il codice C# può effettuare chiamate nel codice JavaScript e vice versa. Per altre informazioni, vedere [Interoperabilità JavaScript](xref:razor-components/javascript-interop).

## <a name="optimization"></a>Ottimizzazione

Per le applicazioni sul lato client, le dimensioni del payload sono cruciali. Blazor consente di ottimizzare le dimensioni del payload per ridurre i tempi di download. Ad esempio, le parti inutilizzate degli assembly .NET vengono rimosse durante il processo di compilazione, le risposte HTTP vengono compresse e il runtime e gli assembly .NET vengono memorizzati nella cache del browser.

Razor Components offre dimensioni di payload anche inferiori rispetto a Blazor grazie alla gestione degli assembly .NET, dell'assembly dell'app e del runtime sul lato server. Le app realizzate con Razor Components forniscono solo markup, script e fogli di stile ai client.

## <a name="deployment"></a>Distribuzione

Usare Blazor per creare un'app sul lato client autonoma pura o un'app ASP.NET Core per stack completo che contiene sia le app server che client:

* In un'**app sul lato client autonoma**, l'app Blazor viene compilata in una cartella *dist* che contiene solo file statici. I file possono essere ospitati in Servizio app di Azure, in pagine di GitHub, in IIS (configurato come file server statico), in server Node.js e in molti altri server e servizi. .NET non è necessario nel server in produzione.
* In un'**app ASP.NET Core per stack completo**, il codice può essere condiviso tra le app client e server. L'app ASP.NET Core Razor Components risultante, che fornisce l'interfaccia utente sul lato client e altri endpoint dell'API sul lato server, può essere compilata e distribuita in qualsiasi host cloud o locale supportato da ASP.NET Core.

## <a name="suggest-a-feature-or-file-a-bug-report"></a>Suggerire una funzionalità o inviare un report sui bug

Per inviare suggerimenti e report sui bug, [aprire un problema](https://github.com/aspnet/AspNetCore/issues/new). Per informazioni generali e per ottenere risposte dalla community, partecipare alla conversazione su [Gitter](https://gitter.im/aspnet/Blazor).

## <a name="additional-resources"></a>Risorse aggiuntive

* [WebAssembly](http://webassembly.org/)
* [Guida a C#](/dotnet/csharp/)
* [Razor](/aspnet/core/mvc/views/razor)
* [HTML](https://www.w3.org/html/)
