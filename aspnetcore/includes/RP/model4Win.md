<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="fa480-101">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="fa480-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="fa480-102">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="fa480-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fa480-103">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fa480-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="fa480-104">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="fa480-104">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="fa480-105">Chiudere Visual Studio ed eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="fa480-105">Exit Visual Studio and run the command again.</span></span>