## <a name="debug-diagnostics"></a><span data-ttu-id="43db2-101">Diagnostica di debug</span><span class="sxs-lookup"><span data-stu-id="43db2-101">Debug diagnostics</span></span>

<span data-ttu-id="43db2-102">Per l'output di diagnostica di routing dettagliato, impostare `Logging:LogLevel:Microsoft` su `Debug`.</span><span class="sxs-lookup"><span data-stu-id="43db2-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="43db2-103">Nell'ambiente di sviluppo, ad esempio, impostare *appSettings. Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="43db2-103">For example, in the development environment, set *appsettings.Development.json*:</span></span>

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