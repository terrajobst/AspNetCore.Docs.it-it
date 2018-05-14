* Startup.cs: [classe Startup](../fundamentals/startup.md) -classe configura la pipeline di richieste che gestisce tutte le richieste effettuate all'applicazione.
* Program.cs: [classe Program](../fundamentals/index.md) che contiene il punto di ingresso principale dell'applicazione.
* firstapp.csproj: [file di progetto](https://docs.microsoft.com/dotnet/articles/core/preview3/tools/csproj) formato di file di progetto MSBuild per applicazioni ASP.NET Core. Contiene i riferimenti da progetto a progetto, i riferimenti di NuGet e altri elementi correlati del progetto.
* appSettings. JSON / appsettings. Development.JSON: File di configurazione delle impostazioni base app ambiente. [Vedere la sezione configurazione](xref:fundamentals/configuration).
* bower. JSON: Bower le dipendenze dei pacchetti per il progetto.
* . bowerrc: file di configurazione bower che definisce la posizione in cui installare i componenti Bower Scarica l'asset.
* bundleconfig.JSON: file di configurazione per l'aggregazione e la riduzione di asset front-end di JavaScript e CSS.
* Visualizzazioni: Contiene le visualizzazioni Razor. Le visualizzazioni sono componenti che visualizzano l'interfaccia utente dell'applicazione (UI). In genere, questa interfaccia consente di visualizzare i dati del modello.
* : Controller Contiene controller MVC, inizialmente *HomeController.cs*. I controller sono classi che gestiscono le richieste del browser.
* Wwwroot: cartella radice dell'applicazione Web.

Per ulteriori informazioni vedere [modello MVC il](xref:mvc/overview).
