# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>URL di ASP.NET Core la riscrittura di esempio (ASP.NET Core 2. x)

Questo esempio viene illustrato l'utilizzo di ASP.NET Core 2. x Middleware di riscrittura degli URL. Nell'applicazione viene reindirizzamento dell'URL e le opzioni di riscrittura degli URL. Per l'esempio di 1. x di ASP.NET Core, vedere [esempio di riscrittura degli URL di ASP.NET Core (Core ASP.NET 1. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x).

Quando si esegue l'esempio, verrà fornita una risposta che mostra l'URL riscritto o reindirizzato quando una delle regole viene applicata a un URL di richiesta.

## <a name="examples-in-this-sample"></a>Negli esempi contenuti in questo esempio

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - Il codice di stato di esito positivo: 302 (trovato)
  - Esempio (reindirizzamento): **/redirect-rule / {capture_group}** a **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Il codice di stato di esito positivo: 200 (OK)
  - Esempio (riscrittura): **/rewrite-rule / {capture_group_1} / {capture_group_2}** a **/ riscritto? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Il codice di stato di esito positivo: 302 (trovato)
  - Esempio (reindirizzamento): **/apache-mod-rules-redirect / {capture_group}** a **/ reindirizzato? id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Il codice di stato di esito positivo: 200 (OK)
  - Esempio (riscrittura): **/iis-rules-rewrite / {capture_group}** a **/ riscritto? id = {capture_group}**
* `Add(RedirectXMLRequests)`
  - Il codice di stato di esito positivo: 301 (spostato permanentemente)
  - Esempio (reindirizzamento): **/file.xml** a **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Il codice di stato di esito positivo: 301 (spostato permanentemente)
  - Esempio (reindirizzamento): **/image.png** a **/png-images/image.png**
  - Esempio (reindirizzamento): **/image.jpg** a **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Utilizzando un`PhysicalFileProvider`
È anche possibile ottenere un `IFileProvider` creando un `PhysicalFileProvider` per passare il `AddApacheModRewrite()` e `AddIISUrlRewrite()` metodi:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Proteggere le estensioni di reindirizzamento
In questo esempio include `WebHostBuilder` configurazione per l'applicazione di utilizzare gli URL (**https://localhost:5001**, **https://localhost**) e un certificato di prova (**testCert.pfx**) assist è esplorazione questi reindirizzare metodi. Aggiungere uno qualsiasi di essi per la `RewriteOptions()` in **Startup.cs** per analizzare il comportamento.

Metodo | Codice di stato | Porta
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | null (465)
`.AddRedirectToHttps()` | 302 | null (465)
`.AddRedirectToHttps(301)` | 301 | null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
