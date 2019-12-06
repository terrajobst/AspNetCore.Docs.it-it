> [!WARNING]
> Per motivi di sicurezza, è necessario acconsentire esplicitamente all'associazione dei dati della richiesta `GET` alle proprietà del modello di pagina. Verificare l'input dell'utente prima di eseguirne il mapping alle proprietà. La scelta dell'associazione `GET` è utile per gli scenari che si basano sulla stringa di query o sui valori della route.
>
> Per associare una proprietà alle richieste di `GET`, impostare la proprietà `SupportsGet` dell'attributo [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) su `true`:
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> Per altre informazioni, vedere [ASP.NET Core community standup: bind on Get Discussion (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).
