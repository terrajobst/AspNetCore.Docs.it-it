---
title: Metapacchetto Microsoft.AspNetCore.App per ASP.NET Core 2.1 e versioni successive
author: Rick-Anderson
description: Il metapacchetto Microsoft.AspNetCore.App include tutti i pacchetti ASP.NET Core e Entity Framework Core supportati.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: d27c3aa53d6edd235006dc136f09558395e15b6e
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538453"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Metapacchetto Microsoft.AspNetCore.App per ASP.NET Core 2.1

Questa funzionalità richiede ASP.NET Core 2.1 e versioni successive con destinazione .NET Core 2.1 e versioni successive.

Il [metapacchetto](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [Microsoft.AspNetCore.App](/dotnet/core/packages#metapackages) per ASP.NET Core:

* Non include le dipendenze di terze parti, ad eccezione di [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) e [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Queste dipendenze di terze parti sono necessarie per garantire il funzionamento delle principali funzionalità dei framework.
* Include tutti i pacchetti supportati dal team di ASP.NET Core ad eccezione di quelli che contengono dipendenze di terze parti (diverse da quelle indicate in precedenza).
* Include tutti i pacchetti supportati dal team di Entity Framework Core ad eccezione di quelli che contengono dipendenze di terze parti (diverse da quelle indicate in precedenza).

Tutte le funzionalità di ASP.NET Core 2.1 e versioni successive e Entity Framework Core 2.1 e versioni successive sono incluse nel pacchetto `Microsoft.AspNetCore.App`. I modelli di progetto predefiniti destinati ad ASP.NET Core 2.1 e versioni successive usano questo pacchetto. È consigliabile che le applicazioni con destinazione ASP.NET Core 2.1 e versioni successive e Entity Framework Core 2.1 e versioni successive usino il pacchetto `Microsoft.AspNetCore.App`.

Il numero di versione del metapacchetto `Microsoft.AspNetCore.App` rappresenta la versione di ASP.NET Core e la versione di Entity Framework Core.

L'uso del metapacchetto `Microsoft.AspNetCore.App` implica restrizioni di versione che proteggono l'app:

* Se viene incluso un pacchetto che presenta una dipendenza transitiva (non diretta) in un pacchetto in `Microsoft.AspNetCore.App` e i numeri di versione sono diversi, NuGet genererà un errore.
* Altri pacchetti aggiunti all'app non possono modificare la versione dei pacchetti inclusi in `Microsoft.AspNetCore.App`.
* La coerenza della versione garantisce un'esperienza affidabile. `Microsoft.AspNetCore.App` è stato progettato per impedire le combinazioni di versioni non testate di bit correlati usati contemporaneamente nella stessa applicazione.

Le applicazioni che usano il metapacchetto `Microsoft.AspNetCore.App` sfruttano automaticamente il framework condiviso di ASP.NET Core. Quando si usa il metapacchetto `Microsoft.AspNetCore.App`, con l'applicazione **non** vengono distribuiti asset dai pacchetti NuGet di riferimento di ASP.NET Core. Questi asset sono infatti inclusi nel framework condiviso di .NET Core. Gli asset contenuti nel framework condiviso sono precompilati per migliorare i tempi di avvio dell'applicazione. Per altre informazioni, vedere la sezione relativa ai pacchetti condivisi in [Pacchetti di distribuzione di .NET Core](/dotnet/core/build/distribution-packaging).

Il file di progetto seguente fa riferimento al metapacchetto `Microsoft.AspNetCore.App` per ASP.NET Core e rappresenta un modello ASP.NET Core 2.1 tipico:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.4" />
  </ItemGroup>

</Project>
```

Il numero di versione nel riferimento `Microsoft.AspNetCore.App` **non** garantisce che verrà usata la versione del framework condiviso. Ad esempio, si supponga che venga specificata la versione `2.1.1` e venga invece installata la versione `2.1.3`. In questo caso l'app usa la versione `2.1.3`. Benché non sia consigliabile, è possibile disabilitare il roll forward (patch e/o versioni secondarie). Per altre informazioni sul comportamento di roll-forward delle versioni dei pacchetti, vedere [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

## <a name="update-aspnet-core"></a>Aggiornare ASP.NET Core

Il [metapacchetto](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` non è un pacchetto tradizionale aggiornato da NuGet. Analogamente a `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` rappresenta un runtime condiviso, con una semantica di controllo delle versioni speciale gestita all'esterno di NuGet. Per altre informazioni, vedere [Pacchetti, metapacchetti e framework](/dotnet/core/packages).

Per aggiornare ASP.NET Core:

* Nei computer di sviluppo e nei server di compilazione: scaricare e installare [.NET Core SDK](https://www.microsoft.com/net/download).
* Nei server di distribuzione: scaricare e installare il [runtime di .NET Core](https://www.microsoft.com/net/download).

 Le applicazioni eseguiranno il roll forward all'ultima versione installata al riavvio dell'applicazione. Non è necessario aggiornare il numero di versione di `Microsoft.AspNetCore.App` nel file di progetto. Per altre informazioni, vedere [Roll forward delle app dipendenti dal framework](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Se l'applicazione usava in precedenza `Microsoft.AspNetCore.All`, vedere [Migrazione da Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
