# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>Esempio di riscrittura URL per ASP.NET Core (ASP.NET Core 1.x)

Questo esempio illustra l'uso del middleware di riscrittura URL per ASP.NET Core 1.x. L'applicazione illustra le opzioni di reindirizzamento e di riscrittura degli URL. Per l'esempio relativo ad ASP.NET Core 2.x, vedere [Esempio di riscrittura URL per ASP.NET Core (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).

Quando si esegue l'esempio, viene generata una risposta che indica l'URL riscritto o reindirizzato quando viene applicata una delle regole a un URL di richiesta.

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
* `Add(RedirectXMLRequests)`
  - Codice di stato riuscito del comando eseguito: 301 (Spostato permanentemente)
  - Esempio (reindirizzamento): **/file.xml** a **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Codice di stato riuscito del comando eseguito: 301 (Spostato permanentemente)
  - Esempio (reindirizzamento): **/image.png** a **/png-images/image.png**
  - Esempio (reindirizzamento): **/image.jpg** a **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Uso di un oggetto `PhysicalFileProvider`
È anche possibile ottenere un oggetto `IFileProvider` creando un `PhysicalFileProvider` da passare nei metodi `AddApacheModRewrite()` e `AddIISUrlRewrite()`:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Estensioni di reindirizzamento protette
Questo esempio include la configurazione di `WebHostBuilder` che consente all'app di usare gli URL (**https://localhost:5001**, **https://localhost**) e un certificato di test (**testCert.pfx**) per agevolare l'esplorazione di questi metodi di reindirizzamento. Aggiungere uno degli elementi a `RewriteOptions()` in **Startup.cs** per analizzarne il comportamento.

Metodo | Codice di stato | Porta
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | null (465)
`.AddRedirectToHttps()` | 302 | null (465)
`.AddRedirectToHttps(301)` | 301 | null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
