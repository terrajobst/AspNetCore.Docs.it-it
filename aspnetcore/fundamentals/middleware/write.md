---
title: Scrivere middleware di ASP.NET Core personalizzato
author: rick-anderson
description: Informazioni su come scrivere middleware di ASP.NET Core personalizzato.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744911"
---
# <a name="write-custom-aspnet-core-middleware"></a>Scrivere middleware di ASP.NET Core personalizzato

[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)

Il middleware è un software che viene assemblato in una pipeline dell'app per gestire richieste e risposte. ASP.NET Core offre un ampio set di componenti middleware integrati, ma in alcuni scenari si potrebbe voler scrivere un middleware personalizzato.

## <a name="middleware-class"></a>Classe middleware

Il middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione. Si consideri il middleware seguente, che specifica le impostazioni cultura per la richiesta corrente da una stringa di query:

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

Il codice di esempio precedente viene usato per illustrare la creazione di un componente middleware. Per il supporto di localizzazione incorporato di ASP.NET Core, vedere <xref:fundamentals/localization>.

È possibile testare il middleware passando le impostazioni cultura. Ad esempio `http://localhost:7997/?culture=no`.

Il codice seguente sposta il delegato middleware in una classe:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

Il nome del metodo `Task` del middleware deve essere `Invoke`. In ASP.NET Core 2.0 o versioni successive il nome può essere `Invoke` o `InvokeAsync`.

::: moniker-end

## <a name="middleware-extension-method"></a>Metodo di estensione del middleware

Il metodo di estensione seguente espone il middleware tramite <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

Il codice seguente chiama il middleware da `Startup.Configure`:

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

Il middleware deve seguire il [principio delle dipendenze esplicite](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) esponendo le dipendenze nel costruttore. Il middleware viene costruito una volta per ogni *durata applicazione*. Se è necessario condividere servizi con il middleware all'interno di una richiesta, vedere la sezione [Dipendenze per richiesta](#per-request-dependencies).

I componenti middleware possono risolvere le dipendenze dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection) mediante i parametri del costruttore. [UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) può anche accettare direttamente parametri aggiuntivi.

## <a name="per-request-dependencies"></a>Dipendenze per richiesta

Poiché il middleware viene creato all'avvio dell'app e non per richiesta, i servizi di durata *con ambito* usati dai costruttori del middleware non vengono condivisi con altri tipi di inserimento di dipendenze durante ogni richiesta. Se è necessario condividere un servizio *con ambito* tra il proprio middleware e altri tipi, aggiungere i servizi alla firma del metodo `Invoke`. Il metodo `Invoke` può accettare parametri aggiuntivi popolati dall'inserimento delle dipendenze:

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>