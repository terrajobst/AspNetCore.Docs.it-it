## <a name="register-the-database-context"></a>Registrare il contesto del database

In questo passaggio il contesto del database viene registrato nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection). I servizi, come il contesto del database, che sono registrati con il contenitore di inserimento delle dipendenze sono disponibili per i controller.

Registrare il contesto del database nel contenitore dei servizi usando il supporto incorporato per l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection). Sostituire il contenuto del file *Startup.cs* con il codice seguente:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]
::: moniker-end

Il codice precedente:

* Rimuove il codice non usato.
* Specifica che un database in memoria viene inserito nel contenitore dei servizi.