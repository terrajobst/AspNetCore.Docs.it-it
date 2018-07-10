Il modello predefinito crea i collegamenti e le pagine **RazorPagesMovie**, **Home**, **Info** e **Contatto**. A seconda delle dimensioni della finestra del browser, può essere necessario fare clic sull'icona di spostamento per visualizzare i collegamenti.

![Pagina Home o di indice](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

Eseguire il test dei collegamenti. I collegamenti **RazorPagesMovie** e **Home** rimandano alla pagina di indice. I collegamenti **Info** e **Contatto** rimandano alle pagine `About` e `Contact`, rispettivamente.

## <a name="project-files-and-folders"></a>File e cartelle di progetto

La tabella seguente elenca i file e le cartelle nel progetto. Per questa esercitazione, il file più importante da comprendere è *Startup.cs*. Non è necessario rivedere ogni collegamento indicato di seguito. I collegamenti sono forniti come riferimento quando sono necessarie altre informazioni su un file o una cartella nel progetto.

| File o cartella | Scopo |
| -------------- | ------- |
| *wwwroot* | Contiene asset statici. Vedere [File statici](xref:fundamentals/static-files). |
| *Pagine* | Cartella per [Pagine Razor](xref:razor-pages/index). |
| *appsettings.json* | [Configurazione](xref:fundamentals/configuration/index) |
| *Program.cs* | Configura l'[host](xref:fundamentals/host/index) dell'app ASP.NET Core. |
| *Startup.cs* | Configura i servizi e la pipeline della richiesta. Vedere [Avvio](xref:fundamentals/startup). |

### <a name="the-pagesshared-folder"></a>Cartella Pages/Shared

Il file *_Layout.cshtml* contiene elementi HTML comuni (collegamenti a script e fogli di stile) e imposta il layout per l'app. Ad esempio, quando si seleziona **RazorPagesMovie**, **Home**, **About** o **Contact**, nella pagina Web viene visualizzato un set comune di elementi. Gli elementi comuni includono il menu di navigazione nella parte superiore e l'intestazione nella parte inferiore della finestra. Per altre informazioni, vedere [Layout](xref:mvc/views/layout).

Il file *_ValidationScriptsPartial.cshtml* fornisce un riferimento agli script di convalida [jQuery](https://jquery.com/). Quando si aggiungono le pagine `Create` e `Edit` in un secondo momento nell'esercitazione, viene usato il file *_ValidationScriptsPartial.cshtml*.

Il file *_CookieConsentPartial.cshtml* fornisce una barra di navigazione e il contenuto di riepilogo dell'informativa sulla privacy e dei criteri d'uso dei cookie. Per altre informazioni sulle risorse GDPR incluse nel progetto, vedere [EU General Data Protection Regulation (GDPR) support in ASP.NET Core)](xref:security/gdpr) (Supporto del Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea in ASP.NET Core).

### <a name="the-pages-folder"></a>Cartella delle pagine

*_ViewStart.cshtml* imposta la proprietà pagine Razor `Layout` per l'uso del file *_Layout.cshtml*. Vedere [Layout](xref:mvc/views/layout) per altre informazioni.

Il file *_ViewImports.cshtml* contiene le direttive Razor che vengono importate in ogni pagina Razor. Per altre informazioni, vedere [Importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives).

Le pagine `About`, `Contact` e `Index` sono pagine di base che è possibile usare per avviare un'app. La pagina `Error` viene usata per visualizzare informazioni sugli errori.
