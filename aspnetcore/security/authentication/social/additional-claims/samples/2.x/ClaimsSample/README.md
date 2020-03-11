# <a name="additional-claims-sample-app"></a>App di esempio per altre attestazioni

L'app di esempio illustra come eseguire le operazioni seguenti:

* Ottenere il nome e il cognome dell'utente da Google e archiviare il nome Claims con i valori forniti da Google.
* Archiviare il token di accesso di Google nell'`AuthenticationProperties`dell'utente.

Per usare l'app di esempio:

1. Registrare l'app e ottenere un ID client e un segreto client validi per l'autenticazione Google. Per ulteriori informazioni, vedere la pagina relativa all' [installazione di Google External login](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. Specificare l'ID client e il segreto client per l'app nella [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) di `Startup.ConfigureServices`.
1. Eseguire l'app e richiedere la pagina My Claims. Quando l'utente non è connesso, l'app reindirizza a Google. Accedere con Google. Google reindirizza di nuovo l'utente all'app (`/MyClaims`). L'utente viene autenticato e viene caricata la pagina delle attestazioni personali. Le attestazioni relative al nome e al cognome sono presenti nelle **attestazioni utente** con i valori forniti da Google. Il token di accesso viene visualizzato in **proprietà di autenticazione**.
