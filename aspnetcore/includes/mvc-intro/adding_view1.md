# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>Aggiunta di una vista a un'app ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione si modifica la classe `HelloWorldController` in modo da usare i file del modello di vista Razor per incapsulare correttamente il processo di generazione di risposte HTML a un client.

Si crea un file di modello di vista usando Razor. I modelli di vista basati su Razor hanno l'estensione di file *.cshtml*. Consentono di creare l'output HTML in modo accurato usando C#.

Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Nella classe `HelloWorldController` sostituire il metodo `Index` con il codice seguente:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Il codice precedente restituisce un oggetto `View`. Usa un modello di vista per generare una risposta HTML al browser. I metodi controller, noti anche come metodi di azione, ad esempio il metodo `Index` descritto in precedenza, restituiscono in genere un [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (o una classe derivata da `ActionResult`) e non un tipo come una stringa.
