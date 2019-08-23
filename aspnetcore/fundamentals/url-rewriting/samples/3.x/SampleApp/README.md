# <a name="aspnet-core-url-rewriting-sample"></a>Esempio di riscrittura URL per ASP.NET Core

Questo esempio illustra l'uso del middleware di riscrittura URL per ASP.NET Core. L'app illustra le opzioni di reindirizzamento e di riscrittura degli URL.

Quando si esegue l'esempio, al momento dell'applicazione di una delle regole a un URL di richiesta, risposte diverse da file restituiscono l'URL riscritto o reindirizzato. Per gli esempi in formato XML e file di testo, il middleware dei file statici genera il file dopo che l'URL della richiesta è stato riscritto dal middleware.

## <a name="examples-in-this-sample"></a>Esempi

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Codice di stato riuscito del comando eseguito: 302 (Trovato)
  - Esempio (reindirizzamento): **/redirect-rule/{gruppo_capture}** a **/redirected/{gruppo_capture}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Codice di stato riuscito del comando eseguito: 200 (OK)
  - Esempio (riscrittura): **/rewrite-rule/{gruppo_capture_1}/{gruppo_capture_2}** in **/rewritten?var1={gruppo_capture_1}&var2={gruppo_capture_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Codice di stato riuscito del comando eseguito: 302 (Trovato)
  - Esempio (reindirizzamento): **/apache-mod-rules-redirect/{gruppo_capture}** a **/redirected?id={gruppo_capture}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Codice di stato riuscito del comando eseguito: 200 (OK)
  - Esempio (riscrittura): **/iis-rules-rewrite/{gruppo_capture}** in **/rewritten?id={gruppo_capture}**
* `Add(RedirectXmlFileRequests)`
  - Codice di stato riuscito del comando eseguito: 301 (Spostato in modo permanente)
  - Esempio (reindirizzamento): **/file.xml** a **/xmlfiles/file.xml**
* `Add(RewriteTextFileRequests)`
  - Codice di stato riuscito del comando eseguito: 200 (OK)
  - Esempio (riscrittura): da **/some_file.txt** a **/file.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - Codice di stato riuscito del comando eseguito: 301 (Spostato in modo permanente)
  - Esempio (reindirizzamento): **/image.png** a **/png-images/image.png**
  - Esempio (reindirizzamento): **/image.jpg** a **/jpg-images/image.jpg**

## <a name="use-a-physicalfileprovider"></a>Usare un PhysicalFileProvider

È anche possibile ottenere un oggetto `IFileProvider` creando un `PhysicalFileProvider` da passare nei metodi `AddApacheModRewrite()` e `AddIISUrlRewrite()`:

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>Estensioni di reindirizzamento protette

Questo esempio include la configurazione di `WebHostBuilder` che consente all'app di usare gli URL (`https://localhost:5001`, `https://localhost`) e un certificato di test (*testCert.pfx*) per agevolare l'esplorazione dei metodi di reindirizzamento protetti. Se la porta 443 del server è già assegnata o in uso, l'esempio `https://localhost` non funziona &mdash; rimuovere `ListenOptions` per la porta 443 nel metodo `CreateWebHostBuilder` per il file *Program.cs* o dissociare la porta 443 del server in modo da consentirne l'uso da parte di Kestrel.

| Metodo                           | Codice di stato |    Porta    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | null (465) |
| `.AddRedirectToHttps()`          |     302     | null (465) |
| `.AddRedirectToHttps(301)`       |     301     | null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
