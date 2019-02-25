---
title: Middleware di ASP.NET Core
author: rick-anderson
description: Informazioni sul middelware di ASP.NET Core e la pipeline delle richieste.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
---
# <a name="aspnet-core-middleware"></a>Middleware di ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)

Il middleware è un software che viene assemblato in una pipeline dell'app per gestire richieste e risposte. Ogni componente:

* Sceglie se passare la richiesta al componente successivo nella pipeline.
* Può eseguire le operazioni prima e dopo il componente successivo nella pipeline.

Per compilare la pipeline delle richieste vengono usati i delegati di richiesta. I delegati di richiesta gestiscono ogni richiesta HTTP.

I delegati di richiesta vengono configurati tramite i metodi di estensione <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> e <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. È possibile specificare un singolo delegato di richiesta inline come metodo anonimo (chiamato middleware inline) o definirlo in una classe riutilizzabile. Le classi riutilizzabili o i metodi anonimi inline costituiscono il *middleware* o sono noti anche come *componenti middleware*. Ogni componente middleware è responsabile della chiamata del componente seguente nella pipeline o del corto circuito della pipeline. Quando un middleware esegue un corto circuito, viene denominato *middleware terminale* perché impedisce all'ulteriore middleware di elaborare la richiesta.

In <xref:migration/http-modules> sono descritte le differenze tra le pipeline delle richieste in ASP.NET Core e in ASP.NET 4.x e sono riportati altri esempi di middleware.

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>Creare una pipeline middleware con IApplicationBuilder

La pipeline delle richieste ASP.NET Core è costituita da una sequenza di delegati di richiesta, chiamati uno dopo l'altro. Il diagramma seguente illustra il concetto. Il thread di esecuzione seguente le frecce nere.

![Modello di elaborazione delle richieste che visualizza una richiesta in arrivo, l'elaborazione tramite tre middleware e la risposta che esce dall'app. Ogni middleware esegue la propria logica e passa la richiesta al middleware successivo in corrispondenza dell'istruzione next(). Dopo che il terzo middleware ha elaborato la richiesta, la richiesta torna ai due middleware precedenti in ordine inverso per un'ulteriore elaborazione dopo le istruzioni next() prima di uscire dall'app sotto forma di risposta al client.](index/_static/request-delegate-pipeline.png)

I delegati possono eseguire le operazioni prima del delegato successivo e dopo di esso. I delegati che gestiscono le eccezioni devono essere chiamati nella prima parte della pipeline in modo che possano individuare le eccezioni che si verificano nelle parti successive della pipeline.

L'app di ASP.NET Core più semplice imposta un delegato di richiesta singolo che gestisce tutte le richieste. In questo caso non è inclusa una pipeline di richieste effettiva. Al contrario, viene chiamata una singola funzione anonima in risposta a ogni richiesta HTTP.

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

Il primo delegato <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> termina la pipeline.

È possibile concatenare più delegati di richiesta insieme con <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. Il parametro `next` rappresenta il delegato successivo nella pipeline. È possibile eseguire il corto circuito della pipeline *non* chiamando il parametro *next*. In genere è possibile eseguire un'azione prima e dopo il delegato successivo, come illustra l'esempio seguente:

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

Quando un delegato non passa una richiesta al delegato successivo, si crea un cosiddetto *corto circuito della pipeline delle richieste*. Il corto circuito è spesso opportuno poiché evita l'esecuzione di operazioni non necessarie. Ad esempio, il [middleware dei file statici](xref:fundamentals/static-files) può operare come *middleware terminale* elaborando una richiesta di un file statico ed effettuando il corto circuito della pipeline rimanente. Il middleware aggiunto alla pipeline prima del middleware che termina l'ulteriore elaborazione elabora comunque il codice dopo le istruzioni `next.Invoke`. Vedere comunque l'avviso seguente sul tentativo di scrivere una risposta che è già stata inviata.

> [!WARNING]
> Non chiamare `next.Invoke` dopo aver inviato la risposta al client. Le modifiche apportate a <xref:Microsoft.AspNetCore.Http.HttpResponse> dopo l'avvio della risposta generano un'eccezione. Ad esempio, le modifiche come l'impostazione delle intestazioni e di un codice di stato generano un'eccezione. Scrivere nel corpo della risposta dopo aver chiamato `next`:
>
> * Può causare una violazione del protocollo. Ad esempio, scrivere un contenuto che supera il valore `Content-Length` specificato.
> * Può danneggiare il formato del corpo. Ad esempio, scrivere un piè di pagina HTML in un file CSS.
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> è un hint utile per indicare se le intestazioni sono state inviate o se è stato scritto contenuto nel corpo.

## <a name="order"></a>Ordinamento

L'ordine in cui vengono aggiunti i componenti middleware nel metodo `Startup.Configure` definisce l'ordine in cui i componenti middleware vengono richiamati per le richieste e l'ordine inverso per la risposta. Questo ordinamento è fondamentale per la sicurezza, le prestazioni e la funzionalità.

Il metodo `Startup.Configure` seguente aggiunge componenti del middleware per gli scenari di app comuni:

::: moniker range=">= aspnetcore-2.0"

1. Gestione errori/eccezioni
1. Protocollo HTTP Strict Transport Security
1. Reindirizzamento HTTPS
1. File server statico
1. Applicazione dei criteri per i cookie
1. Autenticazione
1. Sessione
1. MVC

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. Gestione errori/eccezioni
1. File statici
1. Autenticazione
1. Sessione
1. MVC

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

Nell'esempio di codice precedente, ogni metodo di estensione del middleware viene esposto in <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> tramite lo spazio dei nomi <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> è il primo componente middleware aggiunto alla pipeline. Pertanto, il middleware del gestore di eccezioni intercetta le eccezioni che si verificano nelle chiamate successive.

Il middleware dei file statici viene chiamato nella prima parte della pipeline in moda che possa gestire le richieste ed eseguire un corto circuito senza passare attraverso i componenti rimanenti. Il middleware dei file statici **non** offre controlli di autorizzazione. I file serviti dal sever, inclusi i file in *wwwroot*, sono disponibili pubblicamente. Vedere <xref:fundamentals/static-files> per un approccio alla protezione dei file statici.

::: moniker range=">= aspnetcore-2.0"

Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware di autenticazione (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) che esegue l'autenticazione. L'autenticazione non esegue il corto circuito di richieste non autenticate. Sebbene il middleware di autenticazione esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona una pagina Razor o un controller specifico e un'azione.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Se la richiesta non è gestita dal middleware dei file statici, viene passata al middleware dell'identità (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>) che esegue l'autenticazione. Identity non esegue il corto circuito di richieste non autenticate. Sebbene Identity esegua l'autenticazione delle richieste, l'autorizzazione (e il rifiuto) si verifica solo dopo che MVC seleziona un controller specifico e un'azione.

::: moniker-end

L'esempio seguente illustra un ordinamento del middleware nel quale le richieste dei file statici vengono gestite dal middleware dei file statici prima del middleware di compressione delle risposte. I file statici non vengono compressi con questo ordine di middleware. Le risposte MVC da <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> possono essere compresse.

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a>Use, Run e Map

Configurare la pipeline HTTP usando `Use`,`Run` e `Map`. Il metodo `Use` può eseguire il corto circuito della pipeline, ovvero non chiamare un delegato di richiesta `next`. `Run` è una convenzione e alcuni componenti del middleware possono esporre metodi `Run[Middleware]` che vengono eseguiti al termine della pipeline.

Le estensioni <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> vengono usate come convenzione per la diramazione della pipeline. `Map*` crea un ramo nella pipeline delle richieste in base alle corrispondenze del percorso della richiesta specificato. Se il percorso della richiesta inizia con il percorso specificato, il ramo viene eseguito.

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente.

| Richiesta             | Risposta                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Hello from non-Map delegate. |
| localhost:1234/map1 | Map Test 1                   |
| localhost:1234/map2 | Map Test 2                   |
| localhost:1234/map3 | Hello from non-Map delegate. |

Quando si usa `Map`, i segmenti di percorso corrispondenti vengono rimossi da `HttpRequest.Path` e aggiunti a `HttpRequest.PathBase` per ogni richiesta.

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea un ramo nella pipeline delle richieste in base al risultato del predicato specificato. È possibile usare qualsiasi predicato di tipo `Func<HttpContext, bool>` per mappare le richieste a un nuovo ramo della pipeline. Nell'esempio seguente viene usato un predicato per rilevare la presenza di una variabile di stringa di query `branch`:

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

La tabella seguente visualizza le richieste e le risposte da `http://localhost:1234` usando il codice precedente.

| Richiesta                       | Risposta                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Hello from non-Map delegate. |
| localhost:1234/?branch=master | Ramo usato = master         |

`Map` supporta l'annidamento, ad esempio:

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

`Map` può anche trovare la corrispondenza di più segmenti contemporaneamente:

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a>Middleware incorporato

ASP.NET Core include i componenti middleware seguenti. Nella colonna *Ordinamento* sono disponibili note sul posizionamento del middleware nella pipeline di elaborazione delle richieste e indicazioni sulle condizioni nelle quali il middleware può terminare l'elaborazione delle richieste. Quando un middleware esegue un corto circuito della pipeline di elaborazione delle richieste e impedisce al middleware a valle di elaborare una richiesta, viene denominato *middleware terminale*. Per altre informazioni sul corto circuito, vedere la sezione [Creare una pipeline middleware con IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder).

| Middleware | Descrizione | Ordinamento |
| ---------- | ----------- | ----- |
| [Autenticazione](xref:security/authentication/identity) | Offre il supporto dell'autenticazione. | Prima di `HttpContext.User`. Terminale per i callback OAuth. |
| [Criteri per i cookie](xref:security/gdpr) | Registra il consenso degli utenti per l'archiviazione delle informazioni personali e applica gli standard minimi per i campi dei cookie, come `secure` e `SameSite`. | Prima del middleware che emette i cookie. Esempi: Autenticazione, Sessione, MVC (TempData). |
| [CORS](xref:security/cors) | Configura la condivisione di risorse tra le origini (CORS). | Prima dei componenti che usano CORS. |
| [Diagnostica](xref:fundamentals/error-handling) | Configura la diagnostica. | Prima dei componenti che generano errori. |
| [Intestazioni inoltrate](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Inoltra le intestazioni proxy nella richiesta corrente. | Prima dei componenti che usano i campi aggiornati. Esempi: schema, host, IP client, metodo. |
| [Controllo integrità](xref:host-and-deploy/health-checks) | Controlla l'integrità di un'app ASP.NET Core e le relative dipendenze, come il controllo della disponibilità del database. | Terminale se una richiesta corrisponde a un endpoint di controllo di integrità. |
| [Override del metodo HTTP](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | Consente a una richiesta POST in arrivo di eseguire l'override del metodo. | Prima dei componenti che usano il metodo aggiornato. |
| [Reindirizzamento HTTPS](xref:security/enforcing-ssl#require-https) | Reindirizzare tutte le richieste HTTP a HTTPS (ASP.NET Core 2.1 o versioni successive). | Prima dei componenti che usano l'URL. |
| [Protocollo HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Middleware di ottimizzazione della sicurezza che aggiunge un'intestazione della risposta speciale (ASP.NET Core 2.1 o versioni successive). | Prima dell'invio delle risposte e dopo i componenti che modificano le richieste. Esempi: intestazioni inoltrate, riscrittura dell'URL. |
| [MVC](xref:mvc/overview) | Elabora le richieste con MVC/Razor Pages (ASP.NET Core 2.0 o versione successiva). | Terminale se una richiesta corrisponde a una route. |
| [OWIN](xref:fundamentals/owin) | Interoperabilità con app, server e middleware basati su OWIN. | Terminale se la richiesta viene elaborata completamente dal middleware OWIN. |
| [Memorizzazione nella cache delle risposte](xref:performance/caching/middleware) | Offre il supporto per la memorizzazione delle risposte nella cache. | Prima dei componenti che richiedono la memorizzazione nella cache. |
| [Compressione delle risposte](xref:performance/response-compression) | Offre il supporto per la compressione delle risposte. | Prima dei componenti che richiedono la compressione. |
| [Localizzazione della richiesta](xref:fundamentals/localization) | Offre il supporto per la localizzazione. | Prima dei componenti sensibili alla localizzazione. |
| [Routing](xref:fundamentals/routing) | Definisce e vincola le route di richiesta. | Terminale per le route corrispondenti. |
| [Sessione](xref:fundamentals/app-state) | Offre il supporto per la gestione delle sessioni utente. | Prima dei componenti che richiedono la Sessione. |
| [File statici](xref:fundamentals/static-files) | Offre il supporto per la gestione di file statici e l'esplorazione directory. | Terminale se una richiesta corrisponde a un file. |
| [Riscrittura dell'URL](xref:fundamentals/url-rewriting) | Offre il supporto per la riscrittura degli URL e il reindirizzamento delle richieste. | Prima dei componenti che usano l'URL. |
| [Oggetti WebSocket](xref:fundamentals/websockets) | Abilita il protocollo WebSocket. | Prima dei componenti necessari per accettare le richieste WebSocket. |

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
