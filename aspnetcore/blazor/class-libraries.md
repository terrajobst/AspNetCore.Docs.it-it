---
title: Librerie di classi dei componenti di ASP.NET Core Razor
author: guardrex
description: Scopri in che modo è possibile includere i componenti nelle app Blazor da una libreria di componenti esterna.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/class-libraries
ms.openlocfilehash: f8e8688cdb3d1aef0d470e0e2d8c3857140ef65f
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160028"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>Librerie di classi dei componenti di ASP.NET Core Razor

Di [Simon Timms](https://github.com/stimms)

I componenti possono essere condivisi in una [libreria di classi Razor (RCL)](xref:razor-pages/ui-class) tra i progetti. È possibile includere una *libreria di classi di componenti Razor* da:

* Un altro progetto nella soluzione.
* Un pacchetto NuGet.
* Libreria .NET A cui si fa riferimento.

Così come i componenti sono tipi .NET normali, i componenti forniti da un RCL sono assembly .NET normali.

## <a name="create-an-rcl"></a>Creare un RCL

Per configurare l'ambiente per Blazor, seguire le istruzioni riportate nell'articolo <xref:blazor/get-started>.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Creare un nuovo progetto.
1. Selezionare **libreria di classi Razor**. Scegliere **Avanti**.
1. Nella finestra di dialogo **Crea una nuova libreria di classi Razor** selezionare **Crea**.
1. Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito. Gli esempi in questo argomento usano il nome del progetto `MyComponentLib1`. Scegliere **Crea**.
1. Aggiungere RCL a una soluzione:
   1. Fare clic con il pulsante destro del mouse sulla soluzione. Selezionare **aggiungi** > **progetto esistente**.
   1. Passare al file di progetto di RCL.
   1. Selezionare il file di progetto di RCL (con*estensione csproj*).
1. Aggiungere un riferimento a RCL dall'app:
   1. Fare clic con il pulsante destro del mouse sul progetto app. Selezionare **Aggiungi** **riferimento** > .
   1. Selezionare il progetto RCL. Scegliere **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

1. Usare il modello di **libreria di classi Razor** (`razorclasslib`) con il comando [DotNet New](/dotnet/core/tools/dotnet-new) in una shell dei comandi. Nell'esempio seguente viene creato un RCL denominato `MyComponentLib1`. La cartella che include `MyComponentLib1` viene creata automaticamente quando si esegue il comando:

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Per aggiungere la libreria a un progetto esistente, usare il comando [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) in una shell dei comandi. Nell'esempio seguente viene aggiunto RCL all'app. Eseguire il comando seguente dalla cartella del progetto dell'app con il percorso della libreria:

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>Utilizzare un componente di libreria

Per utilizzare i componenti definiti in una raccolta in un altro progetto, utilizzare uno degli approcci seguenti:

* Usare il nome completo del tipo con lo spazio dei nomi.
* Usare la direttiva Razor [\@using](xref:mvc/views/razor#using) . I singoli componenti possono essere aggiunti in base al nome.

Negli esempi seguenti `MyComponentLib1` è una libreria di componenti contenente un componente di `SalesReport`.

È possibile fare riferimento al componente `SalesReport` usando il nome completo del tipo con lo spazio dei nomi:

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

È possibile fare riferimento al componente anche se la libreria viene inserita nell'ambito con una direttiva `@using`:

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Includere la direttiva `@using MyComponentLib1` nel file *_Import. Razor* di primo livello per rendere disponibili i componenti della libreria a un intero progetto. Aggiungere la direttiva a un file *_Import. Razor* a qualsiasi livello per applicare lo spazio dei nomi a una singola pagina o a un set di pagine all'interno di una cartella.

## <a name="build-pack-and-ship-to-nuget"></a>Compilare, comprimere e spedire a NuGet

Poiché le librerie dei componenti sono librerie .NET standard, la creazione di pacchetti e la spedizione a NuGet non è diversa dalla creazione di pacchetti e dalla distribuzione di qualsiasi libreria a NuGet. La creazione di pacchetti viene eseguita usando il comando [DotNet Pack](/dotnet/core/tools/dotnet-pack) in una shell dei comandi:

```dotnetcli
dotnet pack
```

Caricare il pacchetto in NuGet usando il comando [DotNet NuGet push](/dotnet/core/tools/dotnet-nuget-push) in una shell dei comandi.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Creare una libreria di classi di componenti Razor con asset statici

Un RCL può includere asset statici. Gli asset statici sono disponibili per qualsiasi app che usa la libreria. Per ulteriori informazioni, vedere <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:razor-pages/ui-class>
