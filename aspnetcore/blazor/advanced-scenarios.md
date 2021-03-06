---
title: Scenari ASP.NET Core Blazor avanzati
author: guardrex
description: Informazioni sugli scenari avanzati in Blazor, inclusa la modalità di incorporamento della logica RenderTreeBuilder manuale in un'app.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5edbbe36e8389bac0335594b1e4331aee1c02867
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659453"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a>Scenari avanzati di ASP.NET Core Blazer

Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)

## <a name="blazor-server-circuit-handler"></a>Gestore circuito del server Blazer

Il server Blazer consente al codice di definire un *gestore di circuito*, che consente l'esecuzione di codice in base alle modifiche apportate allo stato del circuito di un utente. Un gestore di circuito viene implementato derivando da `CircuitHandler` e registrando la classe nel contenitore del servizio dell'app. L'esempio seguente di un gestore di circuito tiene traccia delle connessioni a SignalR aperte:

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

I gestori del circuito vengono registrati usando l'inserimento DI dipendenze. Le istanze con ambito vengono create per ogni istanza di un circuito. Utilizzando la `TrackingCircuitHandler` nell'esempio precedente, viene creato un servizio singleton perché è necessario tenere traccia dello stato di tutti i circuiti:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

Se i metodi di un gestore di circuito personalizzato generano un'eccezione non gestita, l'eccezione è fatale per il circuito del server blazer. Per tollerare le eccezioni nel codice di un gestore o i metodi chiamati, eseguire il wrapping del codice in una o più istruzioni [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con gestione e registrazione degli errori.

Quando un circuito termina perché un utente si è disconnesso e il Framework pulisce lo stato del circuito, il Framework Elimina l'ambito di. Con l'eliminazione dell'ambito vengono eliminati tutti i servizi con ambito DI circuito che implementano <xref:System.IDisposable?displayProperty=fullName>. Se un servizio DI INSERIMENTO DI dipendenze genera un'eccezione non gestita durante l'eliminazione, il Framework registra l'eccezione.

## <a name="manual-rendertreebuilder-logic"></a>Logica RenderTreeBuilder manuale

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` fornisce metodi per la modifica di componenti ed elementi, inclusa la compilazione manuale C# dei componenti nel codice.

> [!NOTE]
> L'uso di `RenderTreeBuilder` per creare componenti è uno scenario avanzato. Un componente con formato non valido, ad esempio un tag di markup non chiuso, può causare un comportamento indefinito.

Si consideri il componente `PetDetails` seguente, che può essere incorporato manualmente in un altro componente:

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

Nell'esempio seguente il ciclo nel metodo `CreateComponent` genera tre componenti `PetDetails`. Quando si chiamano `RenderTreeBuilder` metodi per creare i componenti (`OpenComponent` e `AddAttribute`), i numeri di sequenza sono numeri di riga del codice sorgente. L'algoritmo di differenza Blaze si basa sui numeri di sequenza corrispondenti a righe di codice distinte, non sulle chiamate di chiamata distinti. Quando si crea un componente con `RenderTreeBuilder` metodi, impostare come hardcoded gli argomenti per i numeri di sequenza. **L'utilizzo di un calcolo o di un contatore per generare il numero di sequenza può causare un calo delle prestazioni.** Per ulteriori informazioni, vedere la sezione [numeri di sequenza correlati ai numeri di riga del codice e non all'ordine di esecuzione](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .

componente `BuiltContent`:

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> I tipi in `Microsoft.AspNetCore.Components.RenderTree` consentono l'elaborazione dei *risultati* delle operazioni di rendering. Questi sono i dettagli interni dell'implementazione del Framework Blazor. Questi tipi devono essere considerati *instabili* e soggetti a modifiche nelle versioni future.

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>I numeri di sequenza sono correlati ai numeri di riga del codice e non all'ordine di esecuzione

I file dei componenti Razor (*Razor*) vengono sempre compilati. La compilazione è un potenziale vantaggio rispetto all'interpretazione del codice perché il passaggio di compilazione può essere usato per inserire informazioni che migliorano le prestazioni dell'app in fase di esecuzione.

Un esempio fondamentale di questi miglioramenti riguarda i *numeri di sequenza*. I numeri di sequenza indicano al runtime quali output provengono da righe di codice distinte e ordinate. Il runtime usa queste informazioni per generare differenze di albero efficienti nel tempo lineare, che è molto più veloce rispetto a quanto normalmente è possibile per un algoritmo diff della struttura ad albero generale.

Si consideri il seguente file di componente Razor (*Razor*):

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

Il codice precedente viene compilato in un modo simile al seguente:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Quando il codice viene eseguito per la prima volta, se `someFlag` è `true`, il generatore riceve:

| Sequenza | Type      | data   |
| :------: | --------- | :----: |
| 0        | Nodo testo | First (Primo)  |
| 1        | Nodo testo | Second |

Si supponga che `someFlag` diventi `false`e che venga nuovamente eseguito il rendering del markup. Questa volta, il generatore riceve:

| Sequenza | Type       | data   |
| :------: | ---------- | :----: |
| 1        | Nodo testo  | Second |

Quando il runtime esegue una diff, rileva che l'elemento in sequenza `0` è stato rimosso, quindi genera lo *script di modifica*semplice seguente:

* Rimuovere il primo nodo di testo.

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a>Il problema della generazione di numeri di sequenza a livello di codice

Si supponga invece che sia stata scritta la logica del generatore di albero di rendering seguente:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

A questo punto, il primo output è:

| Sequenza | Type      | data   |
| :------: | --------- | :----: |
| 0        | Nodo testo | First (Primo)  |
| 1        | Nodo testo | Second |

Questo risultato è identico al caso precedente, pertanto non esistono problemi negativi. `someFlag` è `false` sul secondo rendering e l'output è:

| Sequenza | Type      | data   |
| :------: | --------- | ------ |
| 0        | Nodo testo | Second |

Questa volta, l'algoritmo Diff rileva che si sono verificate *due* modifiche e l'algoritmo genera lo script di modifica seguente:

* Modificare il valore del primo nodo di testo in `Second`.
* Rimuovere il secondo nodo di testo.

La generazione dei numeri di sequenza ha perso tutte le informazioni utili sul punto in cui i `if/else` rami e i cicli erano presenti nel codice originale. In questo modo si ottiene una differenza **due volte più a lungo** .

Questo è un esempio semplice. Nei casi più realistici con strutture complesse e profondamente annidate e soprattutto con i cicli, il costo delle prestazioni è in genere più elevato. Invece di identificare immediatamente i blocchi o i rami del ciclo che sono stati inseriti o rimossi, l'algoritmo Diff deve ripresentarsi in modo approfondito negli alberi di rendering. Ciò comporta in genere la necessità di compilare script di modifica più lunghi perché l'algoritmo Diff viene informato in modo non più approfondito sulle relazioni tra le vecchie e le nuove strutture.

### <a name="guidance-and-conclusions"></a>Linee guida e conclusioni

* Le prestazioni dell'app soffrono se i numeri di sequenza vengono generati dinamicamente.
* Il Framework non è in grado di creare automaticamente i propri numeri di sequenza in fase di esecuzione perché le informazioni necessarie non esistono a meno che non vengano acquisite in fase di compilazione.
* Non scrivere blocchi lunghi di logica di `RenderTreeBuilder` implementata manualmente. Preferire i file *Razor* e consentire al compilatore di gestire i numeri di sequenza. Se non si è in grado di evitare la logica manuale `RenderTreeBuilder`, suddividere i blocchi di codice lunghi in parti più piccole racchiuse in `OpenRegion`/chiamate `CloseRegion`. Ogni area ha il proprio spazio separato dei numeri di sequenza, quindi è possibile riavviare da zero (o qualsiasi altro numero arbitrario) all'interno di ogni area.
* Se i numeri di sequenza sono hardcoded, l'algoritmo Diff richiede solo che i numeri di sequenza aumentino nel valore. Il valore iniziale e i gap sono irrilevanti. Una delle opzioni legittime consiste nell'usare il numero di riga del codice come numero di sequenza oppure iniziare da zero e aumentare di uno o di centinaia (o qualsiasi intervallo preferito). 
* Blazor usa i numeri di sequenza, mentre altri Framework dell'interfaccia utente per le differenze tra gli alberi non li usano. La diffing è molto più veloce quando si usano i numeri di sequenza e Blazor ha il vantaggio di un passaggio di compilazione che gestisce automaticamente i numeri di sequenza per gli sviluppatori che creano file con *estensione Razor* .

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a>Eseguire trasferimenti di dati di grandi dimensioni nelle app Blazor server

In alcuni scenari, è necessario trasferire grandi quantità di dati tra JavaScript e Blazor. Generalmente, i trasferimenti di dati di grandi dimensioni si verificano quando:

* Le API del browser file system vengono usate per caricare o scaricare un file.
* È necessaria l'interoperabilità con una libreria di terze parti.

In Blazor server è prevista una limitazione per impedire il passaggio di singoli messaggi di grandi dimensioni che potrebbero causare problemi di prestazioni.

Quando si sviluppa codice che trasferisce i dati tra JavaScript e Blazor, tenere presenti le linee guida seguenti:

* Sezionare i dati in parti più piccole e inviare i segmenti di dati in sequenza fino a quando non vengono ricevuti tutti i dati dal server.
* Non allocare oggetti di grandi dimensioni C# in JavaScript e codice.
* Non bloccare il thread principale dell'interfaccia utente per periodi prolungati durante l'invio o la ricezione di dati.
* Liberare la memoria utilizzata quando il processo viene completato o annullato.
* Per motivi di sicurezza, applicare i seguenti requisiti aggiuntivi:
  * Dichiarare le dimensioni massime di file o dati che è possibile passare.
  * Dichiarare la velocità di caricamento minima dal client al server.
* Dopo la ricezione dei dati da parte del server, i dati possono essere:
  * Archiviati temporaneamente in un buffer di memoria fino a quando non vengono raccolti tutti i segmenti.
  * Utilizzato immediatamente. Ad esempio, i dati possono essere archiviati immediatamente in un database o scritti su disco durante la ricezione di ogni segmento.

La classe del caricatore di file seguente gestisce l'interoperabilità JS con il client. La classe Uploader usa l'interoperabilità JS per:

* Eseguire il polling del client per inviare un segmento di dati.
* Interrompere la transazione se si verifica il timeout del polling.

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

Nell'esempio precedente:

* Il `_maxBase64SegmentSize` è impostato su `8192`, che viene calcolato da `_maxBase64SegmentSize = _segmentSize * 4 / 3`.
* Le API di gestione della memoria .NET Core di basso livello vengono usate per archiviare i segmenti di memoria nel server `_uploadedSegments`.
* Viene usato un metodo di `ReceiveFile` per gestire il caricamento tramite l'interoperabilità JS:
  * Le dimensioni del file vengono determinate in byte tramite l'interoperabilità JS con `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.
  * Il numero di segmenti da ricevere viene calcolato e archiviato in `numberOfSegments`.
  * I segmenti sono richiesti in un ciclo `for` tramite l'interoperabilità JS con `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`. Tutti i segmenti ma l'ultimo devono essere 8.192 byte prima della decodifica. Il client è forzato a inviare i dati in modo efficiente.
  * Per ogni segmento ricevuto, i controlli vengono eseguiti prima della decodifica con <xref:System.Convert.TryFromBase64String*>.
  * Un flusso con i dati viene restituito come nuovo <xref:System.IO.Stream> (`SegmentedStream`) dopo il completamento del caricamento.

La classe del flusso segmentato espone l'elenco di segmenti come <xref:System.IO.Stream>di sola lettura non ricercabile:

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

Il codice seguente implementa le funzioni JavaScript per ricevere i dati:

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```
