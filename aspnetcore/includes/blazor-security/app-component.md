<span data-ttu-id="dd0d4-101">Il componente `App` (*app. Razor*) è simile al componente `App` trovato nelle app del server Blazer:</span><span class="sxs-lookup"><span data-stu-id="dd0d4-101">The `App` component (*App.razor*) is similar to the `App` component found in Blazor Server apps:</span></span>

* <span data-ttu-id="dd0d4-102">Il componente `CascadingAuthenticationState` gestisce l'esposizione del `AuthenticationState` al resto dell'app.</span><span class="sxs-lookup"><span data-stu-id="dd0d4-102">The `CascadingAuthenticationState` component manages exposing the `AuthenticationState` to the rest of the app.</span></span>
* <span data-ttu-id="dd0d4-103">Il componente `AuthorizeRouteView` verifica che l'utente corrente sia autorizzato ad accedere a una pagina specificata o altrimenti esegua il rendering del componente `RedirectToLogin`.</span><span class="sxs-lookup"><span data-stu-id="dd0d4-103">The `AuthorizeRouteView` component makes sure that the current user is authorized to access a given page or otherwise renders the `RedirectToLogin` component.</span></span>
* <span data-ttu-id="dd0d4-104">Il componente `RedirectToLogin` gestisce il reindirizzamento degli utenti non autorizzati alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="dd0d4-104">The `RedirectToLogin` component manages redirecting unauthorized users to the login page.</span></span>

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
