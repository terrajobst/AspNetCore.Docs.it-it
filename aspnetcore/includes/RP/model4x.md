<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Eseguire lo scaffolding del modello di filmato

* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Eseguire il comando seguente:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
Se viene visualizzato l'errore:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).