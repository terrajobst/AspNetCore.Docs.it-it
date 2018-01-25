Il modello predefinito crea i collegamenti e le pagine **RazorPagesMovie**, **Home**, **Info** e **Contatto**. A seconda delle dimensioni della finestra del browser, può essere necessario fare clic sull'icona di spostamento per visualizzare i collegamenti.

![Pagina Home o di indice](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

Eseguire il test dei collegamenti. I collegamenti **RazorPagesMovie** e **Home** rimandano alla pagina di indice. I collegamenti **Info** e **Contatto** rimandano alle pagine `About` e `Contact`, rispettivamente.

## <a name="project-files-and-folders"></a>File e cartelle di progetto

La tabella seguente elenca i file e le cartelle nel progetto. Per questa esercitazione, il file più importante da comprendere è *Startup.cs*. Non è necessario rivedere ogni collegamento indicato di seguito. I collegamenti sono forniti come riferimento quando sono necessarie altre informazioni su un file o una cartella nel progetto.

| File o cartella              | Scopo |
| ----------------- | ------------ | 
| wwwroot | Contiene file statici. Vedere [Uso di file statici](xref:fundamentals/static-files). |
| Pages | Cartella per [Pagine Razor](xref:mvc/razor-pages/index). | 
| *appsettings.json* | [Configurazione](xref:fundamentals/configuration/index) |
| *Program.cs* | [Ospita](xref:fundamentals/hosting) l'app ASP.NET Core.|
| *Startup.cs* | Configura i servizi e la pipeline della richiesta. Vedere [Avvio](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Cartella delle pagine

Il file *_Layout.cshtml* contiene elementi HTML comuni (script e fogli di stile) e imposta il layout per l'applicazione. Ad esempio, facendo clic su **RazorPagesMovie**, **Home**, **Info** o **Contatto**, vengono visualizzati gli stessi elementi. Gli elementi comuni includono il menu di navigazione nella parte superiore e l'intestazione nella parte inferiore della finestra. Vedere [Layout](xref:mvc/views/layout) per altre informazioni.

*_ViewStart.cshtml* imposta la proprietà pagine Razor `Layout` per l'uso del file *_Layout.cshtml*. Vedere [Layout](xref:mvc/views/layout) per altre informazioni.

Il file *_ViewImports.cshtml* contiene le direttive Razor che vengono importate in ogni pagina Razor. Per altre informazioni, vedere [Importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives).

Il file *_ValidationScriptsPartial.cshtml* fornisce un riferimento agli script di convalida [jQuery](https://jquery.com/). Aggiungendo le pagine `Create` e `Edit` in un secondo momento nell'esercitazione, verrà usato il file *_ValidationScriptsPartial.cshtml*.

Le pagine `About`, `Contact` e `Index` sono pagine di base che è possibile usare per avviare un'app. La pagina `Error` viene usata per visualizzare informazioni sugli errori.
