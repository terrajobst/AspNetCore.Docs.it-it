> [!WARNING]
> Per motivi di sicurezza, è necessario acconsentire esplicitamente all'associazione dei dati della richiesta `GET` alle proprietà del modello di pagina. Verificare l'input dell'utente prima di eseguirne il mapping alle proprietà. Acconsentire esplicitamente all'associazione `GET` è utile quando si lavora in scenari basati su valori route o stringa di query.
>
> Per associare una proprietà nelle richieste `GET`, impostare la proprietà `SupportsGet` dell'attributo [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) su `true`: `[BindProperty(SupportsGet = true)]`
