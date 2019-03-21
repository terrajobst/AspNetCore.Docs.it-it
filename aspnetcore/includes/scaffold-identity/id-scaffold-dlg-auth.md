---
ms.openlocfilehash: 53774177030adf8a61606a696af85cd1f57d6ab9
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320292"
---
Eseguire l'utilità di scaffolding di identità:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal **Esplora soluzioni**, fare doppio clic sul progetto > **Add** > **nuovo elemento di scaffolding**.
* Dal riquadro sinistro della finestra di **Add Scaffold** finestra di dialogo, seleziona **identità** > **aggiungere**.
* Nel **identità ADD** finestra di dialogo, selezionare le opzioni desiderate.
  * Selezionare la pagina di layout esistente verrà sovrascritto il file di layout con markup non corretto. Quando un oggetto esistente  *\_layout. cshtml* è selezionato un file, è **non** sovrascritto.

 Ad esempio `~/Pages/Shared/_Layout.cshtml` per le pagine Razor `~/Views/Shared/_Layout.cshtml` per i progetti MVC
* Per usare il contesto dati esistente, selezionare almeno un file per eseguire l'override. È necessario selezionare almeno un file per aggiungere il contesto dei dati.
  * Selezionare la classe del contesto dati.
  * Selezionare **aggiungere**.
* Per creare un nuovo contesto utente e possibilmente creare una classe utente personalizzata per l'identità:
  * Selezionare il **+** per creare un nuovo pulsante **classe contesto dati**.
  * Selezionare **aggiungere**.

Nota: Se si crea un nuovo contesto utente, non è necessario selezionare un file per eseguire l'override.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

Aggiungere un riferimento al pacchetto [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al progetto (\*csproj) file. Eseguire il comando seguente nella directory del progetto:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:

```console
dotnet aspnet-codegenerator identity -h
```

Nella cartella del progetto, eseguire l'utilità di scaffolding di identità con le opzioni desiderate. Ad esempio, per la configurazione di identità con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente. Usare il nome completo corretto per il contesto del database:

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

PowerShell Usa un punto e virgola come separatore di comandi. Quando si usa PowerShell, eseguire l'escape di punti e virgola nell'elenco di file o inserire l'elenco di file tra virgolette doppie. Ad esempio:

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Se si esegue l'utilità di scaffolding di identità senza specificare il `--files` flag o `--useDefaultUI` flag, tutte le pagine dell'interfaccia utente di identità disponibili verranno create nel progetto.

---
