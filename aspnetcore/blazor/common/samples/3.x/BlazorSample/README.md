# <a name="blazor-client-side-sample-app"></a>App di esempio blazer (lato client)

Questo esempio illustra l'uso degli scenari di Blazer descritti nella documentazione di Blazer.

## <a name="call-web-api-example"></a>Esempio di chiamata all'API Web

L'esempio di API Web richiede un'API Web in esecuzione basata sull'app di esempio <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">per l'esercitazione: Creare un'API Web con ASP.NET Core argomento</a> MVC. L'app di esempio esegue richieste all'API Web all' `https://localhost:10000/api/todo`indirizzo. Se viene usato un indirizzo API Web diverso, aggiornare il `ServiceEndpoint` valore della costante nel `@functions` blocco del componente Razor.</p>

L'app di esempio esegue una richiesta di <a href="https://docs.microsoft.com/aspnet/core/security/cors">condivisione risorse tra le origini (CORS)</a> da `http://localhost:5000` o `https://localhost:5001` all'API Web. Sono consentite le credenziali (cookie/intestazioni di autorizzazione). Aggiungere la configurazione del middleware CORS seguente al `Startup.Configure` metodo dell'API Web prima di chiamare: `UseMvc`</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

Modificare i domini e le porte `WithOrigins` di in base alle esigenze per l'app blazer.

L'API Web è configurata per CORS per consentire cookie/intestazioni e richieste di autorizzazione dal codice client, ma l'API Web creata dall'esercitazione non autorizza effettivamente le richieste. Per informazioni aggiuntive sull'implementazione, vedere gli articoli relativi alla <a href="https://docs.microsoft.com/aspnet/core/security/">sicurezza e all'identità ASP.NET Core</a> .
