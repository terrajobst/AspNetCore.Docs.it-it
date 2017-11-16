<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="da621-101">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="da621-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="da621-102">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="da621-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="da621-103">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="da621-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```