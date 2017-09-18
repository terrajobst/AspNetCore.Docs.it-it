## <a name="register-the-database-context"></a>Registrare il contesto del database

Per inserire il contesto del database nel controller, è necessario registrarlo nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection). Registrare il contesto del database nel contenitore dei servizi usando il supporto incorporato per [inserimento dipendenze](xref:fundamentals/dependency-injection). Sostituire il contenuto del file *Startup.cs* con il codice riportato di seguito:

[!code-csharp[Principale](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Il codice precedente:

* Rimuove il codice non in uso.
* Specifica che un database in memoria viene inserito nel contenitore dei servizi.
