## <a name="register-the-database-context"></a>Registrare il contesto del database

In questo passaggio il contesto del database viene registrato nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection). I servizi, come il contesto del database, che sono registrati con il contenitore di inserimento delle dipendenze sono disponibili per i controller.

Registrare il contesto del database nel contenitore dei servizi usando il supporto incorporato per l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection). Sostituire il contenuto del file *Startup.cs* con il codice seguente:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Il codice precedente:

* Rimuove il codice che non viene usato.
* Specifica che un database in memoria viene inserito nel contenitore dei servizi.