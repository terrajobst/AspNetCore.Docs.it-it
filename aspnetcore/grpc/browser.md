---
title: Usare gRPC nelle app del browser
author: jamesnk
description: Informazioni su come configurare i servizi gRPC in ASP.NET Core per essere richiamabili dalle app del browser usando gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/10/2020
uid: grpc/browser
ms.openlocfilehash: 333fc8c4277bbac47042d4904c276e963186914a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172267"
---
# <a name="use-grpc-in-browser-apps"></a>Usare gRPC nelle app del browser

Di [James Newton-King](https://twitter.com/jamesnk)

> [!IMPORTANT]
> **gRPC-supporto Web in .NET è sperimentale**
>
> gRPC-Web per .NET è un progetto sperimentale, non un prodotto di cui è stato eseguito il commit. Si desidera:
>
> * Testare l'approccio all'implementazione di gRPC-Web Works.
> * Ottenere commenti e suggerimenti su se questo approccio è utile per gli sviluppatori .NET rispetto al metodo tradizionale di configurazione di gRPC-Web tramite un proxy.
>
> Invia commenti e suggerimenti in [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) per assicurarsi di creare qualcosa di cui gli sviluppatori desiderano e siano produttivi.

Non è possibile chiamare un servizio HTTP/2 gRPC da un'app basata su browser. [gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) è un protocollo che consente alle app JavaScript e blazer del browser di chiamare i servizi gRPC. Questo articolo illustra come usare gRPC-Web in .NET Core.

## <a name="configure-grpc-web-in-aspnet-core"></a>Configurare gRPC-Web in ASP.NET Core

i servizi gRPC ospitati in ASP.NET Core possono essere configurati per supportare gRPC-Web insieme a HTTP/2 gRPC. gRPC-Web non richiede alcuna modifica ai servizi. L'unica modifica è la configurazione di avvio.

Per abilitare gRPC-Web con un servizio gRPC ASP.NET Core:

* Aggiungere un riferimento al pacchetto [Grpc. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .
* Configurare l'app per l'uso di gRPC-Web aggiungendo `AddGrpcWeb` e `UseGrpcWeb` a *Startup.cs*:

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

Il codice precedente:

* Aggiunge il middleware gRPC-Web, `UseGrpcWeb`, dopo il routing e prima degli endpoint.
* Specifica che il metodo `endpoints.MapGrpcService<GreeterService>()` supporta gRPC-Web con `EnableGrpcWeb`. 

In alternativa, configurare tutti i servizi per il supporto di gRPC-Web aggiungendo `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` a ConfigureServices.

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

Potrebbe essere necessaria una configurazione aggiuntiva per chiamare gRPC-Web dal browser, ad esempio la configurazione di ASP.NET Core per il supporto di CORS. Per ulteriori informazioni, vedere [supporto di CORS](xref:security/cors).

## <a name="call-grpc-web-from-the-browser"></a>Chiamare gRPC-Web dal browser

Le app browser possono usare gRPC-Web per chiamare i servizi gRPC. Esistono alcuni requisiti e limitazioni quando si chiamano i servizi gRPC con gRPC-Web dal browser:

* Il server deve essere stato configurato per supportare gRPC-Web.
* Il flusso client e le chiamate di streaming bidirezionali non sono supportate. Il flusso del server è supportato.
* Per chiamare i servizi gRPC in un dominio diverso è necessario configurare [CORS](xref:security/cors) nel server.

### <a name="javascript-grpc-web-client"></a>JavaScript gRPC-client Web

È disponibile un client JavaScript gRPC-Web. Per istruzioni su come usare gRPC-Web da JavaScript, vedere [scrivere codice client JavaScript con gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).

### <a name="configure-grpc-web-with-the-net-grpc-client"></a>Configurare gRPC-Web con il client gRPC .NET

Il client gRPC .NET può essere configurato per effettuare chiamate gRPC-Web. Questa operazione è utile per le app [Webassembly Blazer](xref:blazor/index#blazor-webassembly) , che sono ospitate nel browser e hanno le stesse limitazioni http del codice JavaScript. La chiamata di gRPC-Web con un client .NET equivale a [http/2 gRPC](xref:grpc/client). L'unica modifica è la modalità di creazione del canale.

Per usare gRPC-Web:

* Aggiungere un riferimento al pacchetto [Grpc .NET. client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .
* Verificare che il riferimento a [Grpc .NET. client](https://www.nuget.org/packages/Grpc.Net.Client) Package sia 2.27.0 o versione successiva.
* Configurare il canale per l'uso del `GrpcWebHandler`:

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

Il codice precedente:

* Configura un canale per l'uso di gRPC-Web.
* Crea un client ed effettua una chiamata utilizzando il canale.

Al momento della creazione, nel `GrpcWebHandler` sono disponibili le opzioni di configurazione seguenti:

* **InnerHandler**: <xref:System.Net.Http.HttpMessageHandler> sottostante che esegue la richiesta http gRPC, ad esempio `HttpClientHandler`.
* **Mode**: tipo di enumerazione che specifica se la richiesta di richiesta HTTP gRPC `Content-Type` è `application/grpc-web` o `application/grpc-web-text`.
    * `GrpcWebMode.GrpcWeb` configura il contenuto da inviare senza codifica. Valore predefinito.
    * `GrpcWebMode.GrpcWebText` configura il contenuto con codifica Base64. Obbligatorio per le chiamate di streaming del server nei browser.
* **HttpVersion**: il protocollo http `Version` usato per impostare [HttpRequestMessage. Version](xref:System.Net.Http.HttpRequestMessage.Version) sulla richiesta http gRPC sottostante. gRPC-Web non richiede una versione specifica e non esegue l'override dell'impostazione predefinita, a meno che non sia specificato.

> [!IMPORTANT]
> I client gRPC generati hanno metodi Sync e Async per la chiamata di metodi unaria. Ad esempio, `SayHello` è Sync e `SayHelloAsync` è Async. La chiamata di un metodo di sincronizzazione in un'app webassembly Blaze provocherà la mancata risposta dell'app. I metodi asincroni devono essere sempre usati in un webassembly blazer.

## <a name="additional-resources"></a>Risorse aggiuntive

* [progetto GitHub di gRPC per client Web](https://github.com/grpc/grpc-web)
* <xref:security/cors>
