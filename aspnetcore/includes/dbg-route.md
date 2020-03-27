## <a name="debug-diagnostics"></a>Diagnostica di debug

Per l'output di diagnostica di routing dettagliato, impostare `Logging:LogLevel:Microsoft` su `Debug`. Nell'ambiente di sviluppo, ad esempio, impostare *appSettings. Development. JSON*:

```JSON
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Debug",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}