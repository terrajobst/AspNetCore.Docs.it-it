Il componente `App` (*app. Razor*) è simile al componente `App` trovato nelle app del server Blazer:

* Il componente `CascadingAuthenticationState` gestisce l'esposizione del `AuthenticationState` al resto dell'app.
* Il componente `AuthorizeRouteView` verifica che l'utente corrente sia autorizzato ad accedere a una pagina specificata o altrimenti esegua il rendering del componente `RedirectToLogin`.
* Il componente `RedirectToLogin` gestisce il reindirizzamento degli utenti non autorizzati alla pagina di accesso.

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    <RedirectToLogin />
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
