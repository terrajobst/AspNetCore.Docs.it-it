---
title: Librerie di classi di componenti Razor
author: guardrex
description: Scopri come i componenti possono essere inclusi nelle App Razor componenti dalla libreria componente esterno.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515462"
---
# <a name="razor-components-class-libraries"></a>Librerie di classi di componenti Razor

Da [Simon Timms](https://github.com/stimms)

I componenti possono essere condivise nelle librerie di classi Razor tra progetti. I componenti possono essere inclusi da:

* Un altro progetto nella soluzione.
* Un pacchetto NuGet.
* Una libreria .NET di cui viene fatto riferimento.

Proprio come i componenti sono i tipi regolari di .NET, i componenti forniti dalle librerie di classi Razor sono assembly .NET normale.

Usare la `razorclasslib` modello (libreria di classi Razor) con i [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando:

```console
dotnet new razorclasslib -o MyComponentLib1
```

Aggiungere i file Razor componenti (*.razor*) alla libreria di classi Razor.

Per aggiungere la libreria a un progetto esistente, usare il [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> Librerie di classi Razor non sono compatibili con le app Blazor nell'anteprima 3 di ASP.NET Core.
>
> Per creare i componenti in una libreria che può essere condivisi con le app Blazor e componenti di Razor, usare una libreria di classi Blazor creata il `blazorlib` modello.
>
> Librerie di classi Razor non supportano asset statici in ASP.NET Core Preview 3. Librerie dei componenti utilizzando il `blazorlib` modello può includere i file statici, ad esempio immagini, JavaScript e fogli di stile. In fase di compilazione, i file statici vengono incorporati nel file di assembly compilato (*DLL*), che consente l'utilizzo dei componenti senza la necessità di preoccuparsi di come includere le relative risorse. File inclusi nel `content` directory sono contrassegnate come risorsa incorporata.

## <a name="consume-a-library-component"></a>Usare un componente della libreria

Per utilizzare i componenti definiti in una libreria in un altro progetto, il [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) direttiva deve essere utilizzata. È possibile aggiungere i singoli componenti in base al nome.

Il formato generale della direttiva è:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

Ad esempio, aggiunge la direttiva seguente `Component1` di `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Tuttavia, è comune per includere tutti i componenti da un assembly con un carattere jolly (`*`):

```cshtml
@addTagHelper *, MyComponentLib1
```

Il `@addTagHelper` direttiva può essere incluso nella *_ViewImport.cshtml* per rendere i componenti disponibili per un intero progetto o applicate a una singola pagina o un set di pagine all'interno di una cartella. Con la `@addTagHelper` direttiva posto, i componenti della libreria di componenti possono essere utilizzati come se fossero nello stesso assembly dell'app.

## <a name="build-pack-and-ship-to-nuget"></a>Compilazione, Service pack e spedire a NuGet

Poiché le librerie dei componenti sono librerie .NET standard, creazione di pacchetti e l'invio in NuGet non è diverso da imballaggio e spedizione qualsiasi libreria da NuGet. Creazione di pacchetti viene eseguita utilizzando il [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:

```console
dotnet pack
```

Caricare il pacchetto NuGet usando il [dotnet nuget pubblicare](/dotnet/core/tools/dotnet-nuget-push) comando:

```console
dotnet nuget publish
```

Quando si usa il `blazorlib` modello di risorse statiche sono inclusi nel pacchetto NuGet. Consumer della libreria ricevono automaticamente gli script e fogli di stile, in modo che i consumer non è necessari installare manualmente le risorse.
