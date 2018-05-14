## <a name="call-the-web-api-with-jquery"></a>Chiamare l'API Web con jQuery

In questa sezione viene aggiunta una pagina HTML che usa jQuery per chiamare l'API Web. jQuery avvia la richiesta e aggiorna la pagina con i dettagli ottenuti dalla risposta dell'API.

Configurare il progetto in modo che gestisca i file statici e consenta il mapping di file predefinito. Questa funzionalità si ottiene chiamando i metodi di estensione [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*. Per altre informazioni, vedere [Usare i file statici in ASP.NET Core](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Aggiungere un file HTML denominato *index.html* alla directory *wwwroot* del progetto. Sostituire il contenuto con il markup seguente:

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot* del progetto. Sostituire il contenuto con il codice seguente:

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale. Aprire *launchSettings.json* nella directory *Properties* del progetto. Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.

È possibile ottenere jQuery in vari modi. Nel frammento precedente la libreria viene caricata da una rete CDN. Si tratta di un esempio CRUD completo di chiamata dell'API con jQuery. L'esempio include funzionalità aggiuntive che rendono l'esperienza più completa. Di seguito vengono incluse spiegazioni relative alle chiamate all'API.

### <a name="get-a-list-of-to-do-items"></a>Ottenere un elenco di elementi attività

Per ottenere un elenco di elementi attività, inviare una richiesta HTTP GET a */api/todo*.

La funzione [ajax](https://api.jquery.com/jquery.ajax/) di jQuery invia una richiesta AJAX all'API, la quale restituisce codice JSON che rappresenta un oggetto o una matrice. Questa funzione può gestire tutte le forme di interazione HTTP inviando una richiesta HTTP all'`url` specificato. `GET` viene usata come `type`. La funzione di callback `success` viene chiamata se la richiesta ha esito positivo. Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Aggiungere un elemento attività

Per aggiungere un elemento attività, inviare una richiesta HTTP POST a */api/todo/*. Il corpo della richiesta deve contenere un oggetto attività. La funzione [ajax](https://api.jquery.com/jquery.ajax/) usa `POST` per chiamare l'API. Per le richieste `POST` e `PUT` il corpo della richiesta rappresenta i dati inviati all'API. L'API prevede un corpo della richiesta JSON. Le opzioni `accepts` e `contentType` sono impostate su `application/json` per classificare rispettivamente il tipo di supporto ricevuto e inviato. I dati viene convertiti in un oggetto JSON mediante [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Aggiornare un elemento attività

L'aggiornamento di un elemento attività è molto simile all'aggiunta, perché entrambe le operazioni si basano su un corpo della richiesta. In questo caso l'unica vera differenza tra le due operazioni è il fatto che `url` viene modificato per aggiungere l'identificatore univoco dell'elemento e `type` è `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Eliminare un elemento attività

È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
