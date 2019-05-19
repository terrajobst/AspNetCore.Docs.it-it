# <a name="additional-claims-sample-app"></a>App di esempio attestazioni aggiuntive

L'app di esempio viene illustrato come:

* Ottenere nome e cognome dell'utente da Google e archiviare le attestazioni di nome con i valori forniti da Google.
* Store il token di accesso di Google dell'utente `AuthenticationProperties`.

Per usare l'app di esempio:

1. Registrare l'app e ottenere un ID client valido e il segreto client per l'autenticazione Google. Per altre informazioni, vedere [configurazione dell'accesso esterno Google](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. Fornire l'ID client e segreto client per l'app nel [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) di `Startup.ConfigureServices`.
1. Eseguire l'app e richiedere la pagina My attestazioni. Quando l'utente non viene eseguito l'accesso, l'app reindirizza a Google. Accedi con Google. Google reindirizza l'utente all'App (`/MyClaims`). L'utente viene autenticato e viene caricata la pagina My attestazioni. Il nome e cognome attestazioni sono presenti in **attestazioni utente** con i valori forniti da Google. Il token di accesso viene visualizzato sotto **le proprietà di autenticazione**.
