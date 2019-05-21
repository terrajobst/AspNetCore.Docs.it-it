<a name="codegenerator"></a> Nella tabella seguente sono specificati i parametri del generatore di codice ASP.NET Core:

| Parametro               | Description|
| ----------------- | ------------ |
| -m  | Nome del modello. |
| -dc  | Classe `DbContext` da usare. |
| -udl | Uso del layout predefinito. |
| -outDir | Percorso relativo della cartella di output per creare le viste. |
| --referenceScriptLibraries | Aggiunge `_ValidationScriptsPartial` per modificare e creare pagine |

Utilizzare il commutatore `h` per ottenere assistenza sul comando `aspnet-codegenerator razorpage`:

```console
dotnet aspnet-codegenerator razorpage -h
```
