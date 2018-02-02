---
title: Integrazione di test in ASP.NET Core
author: ardalis
description: Come utilizzare l'integrazione di ASP.NET Core test per garantire che i componenti di un'applicazione funzionano correttamente.
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: 4a5f14e11de6ed91f67808c3ea8c78a7b1d43b03
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2018
---
# <a name="integration-testing-in-aspnet-core"></a>Integrazione di test in ASP.NET Core

Da [Steve Smith](https://ardalis.com/)

Test di integrazione garantisce che i componenti di un'applicazione funzionano correttamente quando assemblati. ASP.NET Core supporta l'integrazione test con Framework di unit test e un host web test predefinite che può essere utilizzato per gestire le richieste senza l'overhead di rete.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="introduction-to-integration-testing"></a>Introduzione ai test di integrazione

Test di integrazione di verificare che le parti diverse di un'applicazione interagiscano correttamente. A differenza di [Unit test](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), test di integrazione comportano spesso problemi di infrastruttura dell'applicazione, ad esempio un database, file system, risorse di rete, o le richieste web e le risposte. Unit test usare fakes o oggetti fittizi al posto di questi problemi, ma lo scopo di test di integrazione per assicurarsi che il sistema funzioni come previsto con questi sistemi.

Test di integrazione, poiché dispongono di applicare più grande segmenti di codice e si basano sugli elementi dell'infrastruttura, tendono a essere notevolmente più lenti rispetto agli unit test. Di conseguenza, è consigliabile limitare il numero di test di integrazione che si scrive, soprattutto se è possibile verificare lo stesso comportamento con uno unit test.

> [!NOTE]
> Se il comportamento di alcuni può essere testato usando uno unit test o un test di integrazione, scegliere lo unit test, dal momento che verrà quasi sempre essere più veloce. Potrebbe essere decine o centinaia di unit test con input diversi molte, ma solo un numero limitato di test di integrazione per gli scenari più importanti.

Il test della logica all'interno di metodi personalizzati è in genere il dominio di unit test. Funzionamento dell'applicazione relativo ambito, ad esempio ASP.NET Core o con un database di test in test di integrazione entrano in gioco. Non accetta un numero eccessivo di test di integrazione per confermare che si è in grado di scrivere una riga nel database e leggerli. Non è necessario testare ogni possibile permutazione del codice di accesso ai dati, è sufficiente eseguire il test è sufficiente per disporre fiducia che l'applicazione funzioni correttamente.

## <a name="integration-testing-aspnet-core"></a>Test di integrazione ASP.NET Core

Per ottenere l'impostazione per test di integrazione di esecuzione, è necessario creare un progetto di test, aggiungere un riferimento al progetto web ASP.NET Core e installare un test runner. Questo processo è descritto nel [Unit test](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentazione, insieme a istruzioni più dettagliate sull'esecuzione di test e indicazioni per la denominazione i test e le classi di test.

> [!NOTE]
> Separare gli unit test e i test di integrazione utilizzando progetti diversi. Ciò consente di garantire che non introdurre accidentalmente problemi di infrastruttura in unit test e consente di scegliere facilmente il set di test da eseguire.

### <a name="the-test-host"></a>L'Host di Test

ASP.NET Core include un host di test che può essere aggiunti ai progetti di test di integrazione e utilizzato per l'host ASP.NET Core applicazioni, prova a soddisfare le richieste senza la necessità di un host web reale. L'esempio fornito include un progetto di test di integrazione che è stato configurato per utilizzare [xUnit](https://xunit.github.io) e l'Host di Test. Usa il `Microsoft.AspNetCore.TestHost` pacchetto NuGet.

Una volta il `Microsoft.AspNetCore.TestHost` pacchetto è incluso nel progetto, sarà possibile creare e configurare un `TestServer` nei test. Il test seguente mostra come verificare che una richiesta effettuata nella radice di un sito restituisce "Hello World!" e deve essere eseguito correttamente con il valore predefinito il modello Web vuoto di ASP.NET Core creato da Visual Studio.

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

Questo test viene usato il modello Arrange-Act-Assert. Il passaggio di disposizione viene eseguito nel costruttore, che crea un'istanza di `TestServer`. Configurato `WebHostBuilder` verrà utilizzato per creare un `TestHost`; in questo esempio, il `Configure` sistema sottoposto a test (SUT) metodo `Startup` classe viene passata al `WebHostBuilder`. Questo metodo verrà utilizzato per configurare la pipeline di richieste del `TestServer` in modo identico in modalità server SUT sarà configurato.

Nella parte del test di Act, viene effettuata una richiesta per il `TestServer` istanza per il percorso "/" e la risposta viene letto in una stringa. Questa stringa viene confrontata con la stringa prevista di "Hello World!". Se corrispondono, il test ha esito positivo; in caso contrario, l'esito negativo.

È ora possibile aggiungere alcuni test di integrazione aggiuntive per verificare che il primo controllo funzionalità funziona tramite l'applicazione web:

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

Si noti che non è realmente si tenta di verificare la correttezza del controllo dei numeri primi con questi test, ma piuttosto che l'applicazione web esegue quanto previsto. Si dispone già di code coverage di unit test che ci si assicura `PrimeService`, come si può vedere di seguito:

![Esplora test](integration-testing/_static/test-explorer.png)

Altre informazioni sugli unit test nel [Unit test](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) articolo.


### <a name="integration-testing-mvcrazor"></a>Test di Mvc/Razor di integrazione

Progetti di test contenenti visualizzazioni Razor richiedono `<PreserveCompilationContext>` essere impostata su true nel *csproj* file:


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

Progetti di questo elemento mancante verranno generato un errore simile al seguente:
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>Per l'utilizzo di middleware di refactoring

Refactoring è il processo di modifica il codice dell'applicazione per migliorare la progettazione senza modificarne il comportamento. Deve essere eseguita idealmente quando vi è una famiglia di passaggio di test, poiché queste consentono di garantire che il comportamento del sistema rimane invariato prima e dopo le modifiche. Esaminando il modo in cui viene implementato il prime logica di controllo nell'applicazione web di `Configure` metodo, vedere:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

Questo codice funziona, ma è lontano dal come implementare questo tipo di funzionalità in un'applicazione ASP.NET Core, anche semplice come questa. Si supponga di ciò che il `Configure` metodo sarebbe simile al seguente se è necessario aggiungere questo molto codice ogni volta che si aggiunge un altro endpoint URL!

Aggiunta di un'opzione per considerare [MVC](xref:mvc/overview) all'applicazione e creazione di un controller per gestire il controllo principale. Tuttavia, presupponendo che non attualmente necessario qualunque altra funzionalità MVC, che è un po' eccessivo.

È possibile, tuttavia, sfruttare ASP.NET Core [middleware](xref:fundamentals/middleware/index), che consente di incapsulare i primi logica nella propria classe di controllo e di ottenere le migliori [separazione delle problematiche](http://deviq.com/separation-of-concerns/) nel `Configure` metodo.

Per consentire il percorso in cui il middleware utilizza per essere specificato come parametro, pertanto la classe middleware prevede un `RequestDelegate` e `PrimeCheckerOptions` istanza nel relativo costruttore. Se il percorso della richiesta non corrisponde al contenuto questo middleware configurato in modo da prevedere, semplicemente chiamerà il middleware successivo nella catena di e non eseguire alcuna azione ulteriore. Il resto del codice di implementazione che è stato in `Configure` è il `Invoke` metodo.

> [!NOTE]
> Dal momento che il middleware dipende il `PrimeService` servizio, inoltre richiesta un'istanza del servizio con il costruttore. Il framework fornirà il servizio tramite [Dependency Injection](xref:fundamentals/dependency-injection), presupponendo che è stato configurato, ad esempio in `ConfigureServices`.

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

Poiché questo middleware agisce come un endpoint nella catena di delegato richiesta quando il relativo percorso corrisponde, non vi è alcuna chiamata a `_next.Invoke` quando questo middleware gestisce la richiesta.

Con questo middleware e alcuni utili metodi di estensione creati per semplificare la configurazione, il refactoring `Configure` metodo è simile al seguente:

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

Seguendo questo refactoring, si è certi che l'applicazione web continui a funzionare come prima, poiché i test di integrazione sono il passaggio.

> [!NOTE]
> È consigliabile eseguire il commit delle modifiche al controllo del codice sorgente dopo aver completato un'operazione di refactoring e i test vengono superati. Se si è abituati a sviluppo basato su Test [considerare l'aggiunta di Commit per il ciclo di Red-Green-Refactor](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).

## <a name="resources"></a>Risorse

* [Testing unità](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Middleware](xref:fundamentals/middleware/index)
* [Test dei controller](xref:mvc/controllers/testing)
