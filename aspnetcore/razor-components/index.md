---
title: Introduzione a Razor Components
author: guardrex
description: 'Esplorare ASP.NET Core Razor Components, un modo per creare un''interfaccia utente Web sul lato client interattiva con .NET in un''app ASP.NET Core.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/index
---
# <a name="introduction-to-razor-components"></a>Introduzione a Razor Components

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

*Razor Components* è un modo per sviluppare un'interfaccia utente Web sul lato client interattiva con .NET:

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

Per ulteriori informazioni, vedere <xref:razor-components/hosting-models#server-side-hosting-model>.

*Blazor* è il modello di hosting sul lato client sperimentale per Razor Components. Blazor viene eseguito in .NET nel browser tramite gli standard Web aperti senza plug-in o transpiling del codice. Per altre informazioni, vedere <xref:spa/blazor/index> e <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Componenti

Un *componente Razor* è una parte dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati. I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile. I componenti possono essere annidati e riutilizzati.

I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come pacchetti NuGet. La classe viene in genere scritta sotto forma di pagina di markup Razor in un file con estensione *.razor*.

[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#. Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense). Anche le pagine Razor e le viste MVC usano Razor. Diversamente dalle pagine Razor e dalle viste MVC, che sono basate su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente. È possibile usare Razor Components in modo specifico per la composizione e la logica dell'interfaccia utente sul lato client.

Il markup seguente è un esempio di un componente finestra di dialogo personalizzato in un file Razor (*DialogComponent.razor*):

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

Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser nota come *albero di rendering*, che può quindi essere usata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.

## <a name="javascript-interop"></a>Interoperabilità JavaScript

Per le app che richiedono librerie JavaScript e API browser di terze parti, i componenti supportano l'interoperabilità con JavaScript. I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript. Il codice C# può effettuare chiamate nel codice JavaScript e vice versa. Per altre informazioni, vedere [Interoperabilità JavaScript](xref:razor-components/javascript-interop).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:spa/blazor/index>
* [WebAssembly](http://webassembly.org/)
* [Guida a C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
