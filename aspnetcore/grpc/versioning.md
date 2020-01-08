---
title: Controllo delle versioni dei servizi gRPC
author: jamesnk
description: Informazioni su come eseguire la versione dei servizi gRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/09/2020
uid: grpc/versioning
ms.openlocfilehash: 61dd5dfca4064af3ae58ac288a1ee636450ff56b
ms.sourcegitcommit: 79850db9e79b1705b89f466c6f2c961ff15485de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2020
ms.locfileid: "75694208"
---
# <a name="versioning-grpc-services"></a>Controllo delle versioni dei servizi gRPC

Di [James Newton-King](https://twitter.com/jamesnk)

Le nuove funzionalità aggiunte a un'app possono richiedere i servizi gRPC forniti ai client da modificare, a volte in modi imprevisti e di interruzioni. Quando i servizi gRPC cambiano:

* È necessario considerare il modo in cui le modifiche influiscano sui client.
* È necessario implementare una strategia di controllo delle versioni per supportare le modifiche.

## <a name="backwards-compatibility"></a>Compatibilità con le versioni precedenti

Il protocollo gRPC è progettato per supportare servizi che cambiano nel tempo. In genere, le aggiunte ai servizi e ai metodi gRPC sono senza interruzioni. L'assenza di interruzioni significa che i client esistenti continuano a funzionare. La modifica o l'eliminazione dei servizi gRPC sono modifiche di rilievo. Le modifiche di rilievo indicano un errore dei client esistenti.

Apportare modifiche non di rilievo a un servizio presenta diversi vantaggi:

- I client esistenti continuano a essere eseguiti.
- Evita il lavoro necessario per notificare ai client le modifiche di rilievo e aggiornarle.
- Solo una versione del servizio deve essere documentata e mantenuta.

Questo contenuto è incentrato sulla possibilità di **suddividere le modifiche a un protocollo gRPC e a un livello di compatibilità binaria .NET**. Quando si apportano modifiche, valutare se i client meno recenti possono continuare a lavorare logicamente. Ad esempio, l'aggiunta di un nuovo campo a un messaggio di richiesta:

* Non è una modifica di rilievo del protocollo.
* Se il nuovo campo non è impostato, la restituzione di uno stato di errore nel server rende una modifica di rilievo per i client obsoleti.

### <a name="non-breaking-changes"></a>Modifiche che non causano un'interruzione

Queste modifiche sono senza interruzioni a livello di protocollo gRPC e a livello binario .NET.

- **Aggiunta di un nuovo servizio**
- **Aggiunta di un nuovo metodo a un servizio**
- L' **aggiunta di un campo a un messaggio di richiesta** -i campi aggiunti a un messaggio di richiesta vengono deserializzati con il [valore predefinito](https://developers.google.com/protocol-buffers/docs/proto3#default) nel server quando non è impostato. Per evitare interruzioni, il servizio dovrà avere esito positivo se non è impostato dai client meno recenti.
- L' **aggiunta di un campo a un messaggio di risposta** : i campi aggiunti a un messaggio di risposta vengono deserializzati nella raccolta di [campi sconosciuti](https://developers.google.com/protocol-buffers/docs/proto3#unknowns) del messaggio nel client.
- L' **aggiunta di un valore a** enum-enums viene serializzata come valore numerico. I nuovi valori enum vengono deserializzati nel client nel valore enum senza nome enum. Per essere i client meno recenti devono essere eseguiti correttamente quando ricevono un valore diverso da quello di.

### <a name="binary-breaking-changes"></a>Modifiche di rilievo binarie

Le modifiche seguenti non sono in fase di suddivisione a livello di protocollo gRPC, ma il client deve essere aggiornato se esegue l'aggiornamento al contratto *. proto* più recente o all'assembly .NET del client. La compatibilità binaria è importante se si prevede di pubblicare una libreria gRPC in NuGet.

- La **rimozione di un** valore di campo da un campo rimosso viene deserializzata [nei campi sconosciuti](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)di un messaggio. Non si tratta di una modifica di rilievo del protocollo gRPC, ma il client deve essere aggiornato in caso di aggiornamento al contratto più recente. È importante che un numero di campo rimosso non venga accidentalmente riutilizzato in futuro. Un modo per assicurarsi che non si verifichi questo problema consiste nel specificare i nomi e i numeri di campo eliminati nel messaggio usando la parola chiave [`reserved`](https://developers.google.com/protocol-buffers/docs/proto3#reserved) di protobuf.
- **La ridenominazione di un** campo-i nomi dei campi vengono utilizzati solo nel codice generato. Il numero di campo viene usato per identificare i campi sulla rete. Il client dovrà essere aggiornato in caso di aggiornamento al contratto più recente.
- **Ridenominazione di un messaggio** : i nomi dei messaggi non vengono inviati sulla rete, quindi non si tratta di una modifica di rilievo del protocollo gRPC, ma il client deve essere aggiornato se viene aggiornato al contratto più recente.
- Se si **modifica csharp_namespace** `csharp_namespace` cambiano lo spazio dei nomi dei tipi .NET generati. Non si tratta di una modifica di rilievo del protocollo gRPC, ma il client deve essere aggiornato in caso di aggiornamento al contratto più recente.

### <a name="breaking-changes"></a>Modifiche che causano un'interruzione

Si tratta di modifiche di protocollo e di rilievo binarie.

- **Modifica del tipo di dati** di un campo: la modifica del tipo di dati di un campo in un [tipo incompatibile](https://developers.google.com/protocol-buffers/docs/proto3#updating) provocherà errori durante la deserializzazione del messaggio. Anche se il nuovo tipo di dati è compatibile, è probabile che il client debba essere aggiornato per supportare il nuovo tipo se si aggiorna al contratto più recente.
- **Modifica di un numero di campo** : il numero di campo viene usato per identificare i campi sulla rete.
- La **ridenominazione di un pacchetto, di un servizio o di un metodo** -gRPC usa il nome del pacchetto, il nome del servizio e il nome del metodo per compilare l'URL. Il client ottiene uno stato non *implementato* dal server.
- **Rimozione di un servizio o di un metodo** : il client ottiene uno stato non *implementato* dal server quando viene chiamato il metodo rimosso.

## <a name="version-number-services"></a>Servizi del numero di versione

I servizi devono rimanere compatibili con le versioni precedenti dei client. Alla fine, le modifiche apportate all'app potrebbero richiedere modifiche di rilievo. Suddividere i client obsoleti e forzarli a essere aggiornati insieme al servizio non è un'esperienza utente ottimale. Un modo per mantenere la compatibilità con le versioni precedenti durante l'esecuzione di modifiche di rilievo consiste nel pubblicare più versioni di un servizio.

gRPC supporta un identificatore [`package`](https://developers.google.com/protocol-buffers/docs/proto3#packages) facoltativo, che funziona in modo molto simile a uno spazio dei nomi .NET. Infatti, il `package` verrà utilizzato come spazio dei nomi .NET per i tipi .NET generati se `option csharp_namespace` non è impostato nel file con *estensione proto* . Il pacchetto può essere usato per specificare un numero di versione per il servizio e i relativi messaggi:

[!code-protobuf[](versioning/sample/greet.v1.proto?highlight=3)]

Il nome del pacchetto viene combinato con il nome del servizio per identificare un indirizzo del servizio. Un indirizzo del servizio consente l'hosting side-by-side di più versioni di un servizio:

* `greet.v1.Greeter`
* `greet.v2.Greeter`

Le implementazioni del servizio con versione sono registrate in *Startup.cs*:

```csharp
app.UseEndpoints(endpoints =>
{
    // Implements greet.v1.Greeter
    endpoints.MapGrpcService<GreeterServiceV1>();

    // Implements greet.v2.Greeter
    endpoints.MapGrpcService<GreeterServiceV2>();
});
```

L'inclusione di un numero di versione nel nome del pacchetto consente di pubblicare una versione *v2* del servizio con modifiche di rilievo, continuando a supportare i client meno recenti che chiamano la versione *V1* . Dopo che i client sono stati aggiornati per l'uso del servizio *v2* , è possibile scegliere di rimuovere la versione precedente. Quando si pianifica la pubblicazione di più versioni di un servizio:

- Evitare modifiche di rilievo se ragionevole.
- Non aggiornare il numero di versione a meno che non vengano apportate modifiche di rilievo.
- Aggiornare il numero di versione quando si apportano modifiche di rilievo.

La pubblicazione di più versioni di un servizio la Duplica. Per ridurre la duplicazione, provare a trasferire la logica di business dalle implementazioni del servizio a una posizione centralizzata che può essere riutilizzata dalle implementazioni precedenti e nuove:

[!code-csharp[](versioning/sample/GreeterServiceV1.cs?highlight=10,19)]

I servizi e i messaggi generati con nomi di pacchetti diversi sono **tipi .NET diversi**. Per lo spostamento della logica di business in una posizione centralizzata è necessario eseguire il mapping dei messaggi ai tipi comuni.
