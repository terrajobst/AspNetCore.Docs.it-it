Eseguire il scaffolder identità:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal **Esplora soluzioni**, fare clic su progetto > **Add** > **nuovo elemento di scaffolding**.
* Nel riquadro sinistro della **aggiungere lo scaffolding** finestra di dialogo, seleziona **identità** > **aggiungere**.
* Nel **identità Aggiungi** finestra di dialogo, selezionare le opzioni desiderate.
  * Selezionare la pagina layout esistente verrà sovrascritto il file di layout con il tag non corretto. Quando un file layout. cshtml esistente è selezionato, è **non** sovrascritto.

 Ad esempio `~/Pages/Shared/_Layout.cshtml` per pagine Razor `~/Views/Shared/_Layout.cshtml` per i progetti MVC
* Per utilizzare il contesto dati esistente, selezionare almeno un file per eseguire l'override. È necessario selezionare almeno un file per aggiungere contesto dati.
  * Selezionare la classe del contesto dati.
  * Selezionare **aggiungere**.
* Per creare un nuovo contesto utente ed eventualmente, creare una classe utente personalizzata per l'identità:
  * Selezionare il **+** pulsante per creare un nuovo **classe contesto dati**.
  * Selezionare **aggiungere**.

Nota: Se si sta creando un nuovo contesto utente, non è necessario selezionare un file per eseguire l'override.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se il scaffolder ASP.NET non si è precedentemente installato, installarlo ora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Aggiungere un riferimento pacchetto [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al progetto (\*csproj) file. Eseguire il comando seguente nella directory del progetto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Eseguire il comando seguente per elencare le opzioni di scaffolder identità:

```cli
dotnet aspnet-codegenerator identity -h
```

Nella cartella del progetto, eseguire il scaffolder identità con le opzioni desiderate. Ad esempio, per configurare l'identità con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente. Utilizzare il nome completo corretto per il contesto DB:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
