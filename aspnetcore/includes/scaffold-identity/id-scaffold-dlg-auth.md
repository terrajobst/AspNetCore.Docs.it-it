::: moniker range=">= aspnetcore-3.0"

Eseguire l'impalcatura delle identità:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse sul progetto > **Aggiungi** > **nuovo elemento con impalcatura**.
* Dal riquadro sinistro della finestra di dialogo **Aggiungi impalcatura** selezionare **Identity** > **Aggiungi**.
* Nella finestra di dialogo **Aggiungi identità** selezionare le opzioni desiderate.
  * Selezionare la pagina layout esistente. in alternativa, il file di layout verrà sovrascritto con un markup errato. Quando si seleziona un file *\_layout. cshtml* esistente, questo **non** viene sovrascritto.

 Ad esempio: `~/Pages/Shared/_Layout.cshtml` per Razor Pages `~/Views/Shared/_Layout.cshtml` per i progetti MVC
* Per usare il contesto dei dati esistente, selezionare almeno un file di cui eseguire l'override. È necessario selezionare almeno un file per aggiungere il contesto dei dati.
  * Selezionare la classe del contesto dati.
  * Selezionare **Aggiungi**.
* Per creare un nuovo contesto utente ed eventualmente creare una classe utente personalizzata per l'identità:
  * Selezionare il **+** per creare un nuovo pulsante **classe contesto dati**.
  * Selezionare **Aggiungi**.

Nota: se si sta creando un nuovo contesto utente, non è necessario selezionare un file di cui eseguire l'override.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Aggiungere i riferimenti ai pacchetti NuGet necessari al file del progetto (\*. csproj). Eseguire il comando seguente nella directory del progetto:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

Nella cartella del progetto eseguire l'impalcatura di identità con le opzioni desiderate. Ad esempio, per configurare Identity con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente. Usare il nome completo corretto per il contesto del database:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

PowerShell usa il punto e virgola come separatore di comandi. Quando si usa PowerShell, usare il carattere di escape per i punti e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie. Ad esempio:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Se si esegue l'impalcatura delle identità senza specificare il flag di `--files` o il flag di `--useDefaultUI`, tutte le pagine dell'interfaccia utente di identità disponibili verranno create nel progetto.

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Eseguire l'impalcatura delle identità:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse sul progetto > **Aggiungi** > **nuovo elemento con impalcatura**.
* Dal riquadro sinistro della finestra di dialogo **Aggiungi impalcatura** selezionare **Identity** > **Aggiungi**.
* Nella finestra di dialogo **Aggiungi identità** selezionare le opzioni desiderate.
  * Selezionare la pagina layout esistente. in alternativa, il file di layout verrà sovrascritto con un markup errato. Quando si seleziona un file *\_layout. cshtml* esistente, questo **non** viene sovrascritto.

 Ad esempio: `~/Pages/Shared/_Layout.cshtml` per Razor Pages `~/Views/Shared/_Layout.cshtml` per i progetti MVC
* Per usare il contesto dei dati esistente, selezionare almeno un file di cui eseguire l'override. È necessario selezionare almeno un file per aggiungere il contesto dei dati.
  * Selezionare la classe del contesto dati.
  * Selezionare **Aggiungi**.
* Per creare un nuovo contesto utente ed eventualmente creare una classe utente personalizzata per l'identità:
  * Selezionare il **+** per creare un nuovo pulsante **classe contesto dati**.
  * Selezionare **Aggiungi**.

Nota: se si sta creando un nuovo contesto utente, non è necessario selezionare un file di cui eseguire l'override.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Aggiungere un riferimento al pacchetto [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al file del progetto (\*. csproj). Eseguire il comando seguente nella directory del progetto:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

Nella cartella del progetto eseguire l'impalcatura di identità con le opzioni desiderate. Ad esempio, per configurare Identity con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente. Usare il nome completo corretto per il contesto del database:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

PowerShell usa il punto e virgola come separatore di comandi. Quando si usa PowerShell, usare il carattere di escape per i punti e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie. Ad esempio:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Se si esegue l'impalcatura delle identità senza specificare il flag di `--files` o il flag di `--useDefaultUI`, tutte le pagine dell'interfaccia utente di identità disponibili verranno create nel progetto.

---

::: moniker-end