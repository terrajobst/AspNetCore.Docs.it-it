Eseguire l'utilità di scaffolding di identità:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal **Esplora soluzioni**, fare doppio clic sul progetto > **Add** > **nuovo elemento di scaffolding**.
* Dal riquadro sinistro della finestra di **Add Scaffold** finestra di dialogo, seleziona **identità** > **aggiungere**.
* Nel **identità ADD** finestra di dialogo, selezionare le opzioni desiderate.
  * Selezionare la pagina di layout esistente verrà sovrascritto il file di layout con markup non corretto. Quando si seleziona un file layout. cshtml esistente, si tratta **non** sovrascritto.

 Ad esempio `~/Pages/Shared/_Layout.cshtml` per le pagine Razor `~/Views/Shared/_Layout.cshtml` per i progetti MVC
* Per usare il contesto dati esistente, selezionare almeno un file per eseguire l'override. È necessario selezionare almeno un file per aggiungere il contesto dei dati.
  * Selezionare la classe del contesto dati.
  * Selezionare **aggiungere**.
* Per creare un nuovo contesto utente e possibilmente creare una classe utente personalizzata per l'identità:
  * Selezionare il **+** per creare un nuovo pulsante **classe contesto dati**.
  * Selezionare **aggiungere**.

Nota: Se si crea un nuovo contesto utente, non è necessario selezionare un file per eseguire l'override.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se l'utilità di scaffolding ASP.NET non è stato precedentemente installato, installarlo ora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Aggiungere un riferimento al pacchetto [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al progetto (\*csproj) file. Eseguire il comando seguente nella directory del progetto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:

```cli
dotnet aspnet-codegenerator identity -h
```

Nella cartella del progetto, eseguire l'utilità di scaffolding di identità con le opzioni desiderate. Ad esempio, per la configurazione di identità con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente. Usare il nome completo corretto per il contesto del database:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
