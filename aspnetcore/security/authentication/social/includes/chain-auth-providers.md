## <a name="multiple-authentication-providers"></a><span data-ttu-id="c4b60-101">Più provider di autenticazione</span><span class="sxs-lookup"><span data-stu-id="c4b60-101">Multiple authentication providers</span></span>

<span data-ttu-id="c4b60-102">Quando l'app richiede più provider, concatenare i metodi di estensione del provider dietro [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span><span class="sxs-lookup"><span data-stu-id="c4b60-102">When the app requires multiple providers, chain the provider extension methods behind [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span></span>

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
