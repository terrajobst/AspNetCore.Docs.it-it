---
title: Test di integrazione in ASP.NET Core
author: ardalis
description: Come utilizzare l'integrazione di ASP.NET Core test per garantire che i componenti di un'applicazione funzionano correttamente.
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: 3c618b2bd5919f6536601631eb4d21359a6bc03a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="integration-tests-in-aspnet-core"></a><span data-ttu-id="6eb7c-103">Test di integrazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6eb7c-103">Integration tests in ASP.NET Core</span></span>

<span data-ttu-id="6eb7c-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6eb7c-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6eb7c-105">Test di integrazione garantisce che i componenti di un'applicazione funzionano correttamente quando assemblati.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-105">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="6eb7c-106">ASP.NET Core supporta i test di integrazione con framework di unit test e un host Web di test predefinito che può essere usato per gestire le richieste senza sovraccarico di rete.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-106">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="6eb7c-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6eb7c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="6eb7c-108">Introduzione ai test di integrazione</span><span class="sxs-lookup"><span data-stu-id="6eb7c-108">Introduction to integration testing</span></span>

<span data-ttu-id="6eb7c-109">Test di integrazione di verificare che le parti diverse di un'applicazione interagiscano correttamente.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-109">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="6eb7c-110">A differenza di [Unit test](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), test di integrazione comportano spesso problemi di infrastruttura dell'applicazione, ad esempio un database, file system, risorse di rete, o le richieste web e le risposte.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-110">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="6eb7c-111">Unit test usare fakes o oggetti fittizi al posto di questi problemi, ma lo scopo di test di integrazione per assicurarsi che il sistema funzioni come previsto con questi sistemi.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-111">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="6eb7c-112">Test di integrazione, poiché dispongono di applicare più grande segmenti di codice e si basano sugli elementi dell'infrastruttura, tendono a essere notevolmente più lenti rispetto agli unit test.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-112">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="6eb7c-113">Di conseguenza, è consigliabile limitare il numero di test di integrazione che si scrive, soprattutto se è possibile verificare lo stesso comportamento con uno unit test.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-113">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="6eb7c-114">Se il comportamento di alcuni può essere testato usando uno unit test o un test di integrazione, scegliere lo unit test, dal momento che verrà quasi sempre essere più veloce.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-114">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="6eb7c-115">Potrebbe essere decine o centinaia di unit test con input diversi molte, ma solo un numero limitato di test di integrazione per gli scenari più importanti.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-115">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="6eb7c-116">Il test della logica all'interno di metodi personalizzati è in genere il dominio di unit test.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-116">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="6eb7c-117">Funzionamento dell'applicazione relativo ambito, ad esempio ASP.NET Core o con un database di test in test di integrazione entrano in gioco.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-117">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="6eb7c-118">Non accetta un numero eccessivo di test di integrazione per confermare che si è in grado di scrivere una riga nel database e leggerli.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-118">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="6eb7c-119">Non è necessario testare ogni possibile permutazione del codice di accesso ai dati, è sufficiente eseguire il test è sufficiente per disporre fiducia che l'applicazione funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-119">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="6eb7c-120">Test di integrazione ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6eb7c-120">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="6eb7c-121">Per ottenere l'impostazione per test di integrazione di esecuzione, è necessario creare un progetto di test, aggiungere un riferimento al progetto web ASP.NET Core e installare un test runner.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-121">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="6eb7c-122">Questo processo è descritto nel [Unit test](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentazione, insieme a istruzioni più dettagliate sull'esecuzione di test e indicazioni per la denominazione i test e le classi di test.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-122">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="6eb7c-123">Separare gli unit test e i test di integrazione utilizzando progetti diversi.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-123">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="6eb7c-124">Ciò consente di garantire che non introdurre accidentalmente problemi di infrastruttura in unit test e consente di scegliere facilmente il set di test da eseguire.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-124">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="6eb7c-125">L'Host di Test</span><span class="sxs-lookup"><span data-stu-id="6eb7c-125">The Test Host</span></span>

<span data-ttu-id="6eb7c-126">ASP.NET Core include un host di test che può essere aggiunti ai progetti di test di integrazione e utilizzato per l'host ASP.NET Core applicazioni, prova a soddisfare le richieste senza la necessità di un host web reale.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-126">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="6eb7c-127">L'esempio fornito include un progetto di test di integrazione che è stato configurato per utilizzare [xUnit](https://xunit.github.io) e l'Host di Test.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-127">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="6eb7c-128">Usa il `Microsoft.AspNetCore.TestHost` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-128">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="6eb7c-129">Una volta il `Microsoft.AspNetCore.TestHost` pacchetto è incluso nel progetto, sarà possibile creare e configurare un `TestServer` nei test.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-129">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="6eb7c-130">Il test seguente mostra come verificare che una richiesta effettuata nella radice di un sito restituisce "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="6eb7c-130">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="6eb7c-131">e deve essere eseguito correttamente con il valore predefinito il modello Web vuoto di ASP.NET Core creato da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-131">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="6eb7c-132">Questo test viene usato il modello Arrange-Act-Assert.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-132">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="6eb7c-133">Il passaggio di disposizione viene eseguito nel costruttore, che crea un'istanza di `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-133">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="6eb7c-134">Configurato `WebHostBuilder` verrà utilizzato per creare un `TestHost`; in questo esempio, il `Configure` sistema sottoposto a test (SUT) metodo `Startup` classe viene passata al `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-134">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="6eb7c-135">Questo metodo verrà utilizzato per configurare la pipeline di richieste del `TestServer` in modo identico in modalità server SUT sarà configurato.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-135">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="6eb7c-136">Nella parte del test di Act, viene effettuata una richiesta per il `TestServer` istanza per il percorso "/" e la risposta viene letto in una stringa.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-136">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="6eb7c-137">Questa stringa viene confrontata con la stringa prevista di "Hello World!".</span><span class="sxs-lookup"><span data-stu-id="6eb7c-137">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="6eb7c-138">Se corrispondono, il test ha esito positivo; in caso contrario, l'esito negativo.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-138">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="6eb7c-139">È ora possibile aggiungere alcuni test di integrazione aggiuntive per verificare che il primo controllo funzionalità funziona tramite l'applicazione web:</span><span class="sxs-lookup"><span data-stu-id="6eb7c-139">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="6eb7c-140">Si noti che non è realmente si tenta di verificare la correttezza del controllo dei numeri primi con questi test, ma piuttosto che l'applicazione web esegue quanto previsto.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-140">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="6eb7c-141">Si dispone già di code coverage di unit test che ci si assicura `PrimeService`, come si può vedere di seguito:</span><span class="sxs-lookup"><span data-stu-id="6eb7c-141">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Esplora test](integration-testing/_static/test-explorer.png)

<span data-ttu-id="6eb7c-143">Altre informazioni sugli unit test nel [Unit test](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) articolo.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-143">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="6eb7c-144">Test di Mvc/Razor di integrazione</span><span class="sxs-lookup"><span data-stu-id="6eb7c-144">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="6eb7c-145">Progetti di test contenenti visualizzazioni Razor richiedono `<PreserveCompilationContext>` essere impostata su true nel *csproj* file:</span><span class="sxs-lookup"><span data-stu-id="6eb7c-145">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="6eb7c-146">Progetti di questo elemento mancante verranno generato un errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6eb7c-146">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="6eb7c-147">Per l'utilizzo di middleware di refactoring</span><span class="sxs-lookup"><span data-stu-id="6eb7c-147">Refactoring to use middleware</span></span>

<span data-ttu-id="6eb7c-148">Refactoring è il processo di modifica il codice dell'applicazione per migliorare la progettazione senza modificarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="6eb7c-149">Deve essere eseguita idealmente quando vi è una famiglia di passaggio di test, poiché queste consentono di garantire che il comportamento del sistema rimane invariato prima e dopo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-149">It should ideally be done when there's a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="6eb7c-150">Esaminando il modo in cui viene implementato il prime logica di controllo nell'applicazione web di `Configure` metodo, vedere:</span><span class="sxs-lookup"><span data-stu-id="6eb7c-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

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

<span data-ttu-id="6eb7c-151">Questo codice funziona, ma è lontano dal come implementare questo tipo di funzionalità in un'applicazione ASP.NET Core, anche semplice come questa.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="6eb7c-152">Si supponga di ciò che il `Configure` metodo sarebbe simile al seguente se è necessario aggiungere questo molto codice ogni volta che si aggiunge un altro endpoint URL!</span><span class="sxs-lookup"><span data-stu-id="6eb7c-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="6eb7c-153">Aggiunta di un'opzione per considerare [MVC](xref:mvc/overview) all'applicazione e creazione di un controller per gestire il controllo principale.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="6eb7c-154">Tuttavia, presupponendo che non attualmente necessario qualunque altra funzionalità MVC, che è un po' eccessivo.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="6eb7c-155">È possibile, tuttavia, sfruttare ASP.NET Core [middleware](xref:fundamentals/middleware/index), che consente di incapsulare i primi logica nella propria classe di controllo e di ottenere le migliori [separazione delle problematiche](http://deviq.com/separation-of-concerns/) nel `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware/index), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="6eb7c-156">Per consentire il percorso in cui il middleware utilizza per essere specificato come parametro, pertanto la classe middleware prevede un `RequestDelegate` e `PrimeCheckerOptions` istanza nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="6eb7c-157">Se il percorso della richiesta non corrisponde al contenuto questo middleware configurato in modo da prevedere, semplicemente chiamerà il middleware successivo nella catena di e non eseguire alcuna azione ulteriore.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="6eb7c-158">Il resto del codice di implementazione che è stato in `Configure` è il `Invoke` metodo.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="6eb7c-159">Dal momento che il middleware dipende il `PrimeService` servizio, inoltre richiesta un'istanza del servizio con il costruttore.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="6eb7c-160">Il framework fornirà il servizio tramite [Dependency Injection](xref:fundamentals/dependency-injection), presupponendo che è stato configurato, ad esempio in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="6eb7c-161">Poiché questo middleware agisce come un endpoint nella catena di delegato richiesta quando il relativo percorso corrisponde, non vi è alcuna chiamata a `_next.Invoke` quando questo middleware gestisce la richiesta.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-161">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there's no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="6eb7c-162">Con questo middleware e alcuni utili metodi di estensione creati per semplificare la configurazione, il refactoring `Configure` metodo è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6eb7c-162">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="6eb7c-163">Seguendo questo refactoring, si è certi che l'applicazione web continui a funzionare come prima, poiché i test di integrazione sono il passaggio.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-163">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="6eb7c-164">È consigliabile eseguire il commit delle modifiche al controllo del codice sorgente dopo aver completato un'operazione di refactoring e i test vengono superati.</span><span class="sxs-lookup"><span data-stu-id="6eb7c-164">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="6eb7c-165">Se si è abituati a sviluppo basato su Test [considerare l'aggiunta di Commit per il ciclo di Red-Green-Refactor](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="6eb7c-165">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="6eb7c-166">Risorse</span><span class="sxs-lookup"><span data-stu-id="6eb7c-166">Resources</span></span>

* [<span data-ttu-id="6eb7c-167">Testing unità</span><span class="sxs-lookup"><span data-stu-id="6eb7c-167">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="6eb7c-168">Middleware</span><span class="sxs-lookup"><span data-stu-id="6eb7c-168">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="6eb7c-169">Test controller</span><span class="sxs-lookup"><span data-stu-id="6eb7c-169">Test controllers</span></span>](xref:mvc/controllers/testing)
