La pagina prodotta dal componente `Authentication` (*pages/Authentication. Razor*) definisce le route necessarie per la gestione di diverse fasi di autenticazione.

Componente `RemoteAuthenticatorView`:

* Viene fornito dal pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.
* Gestisce l'esecuzione delle azioni appropriate in ogni fase dell'autenticazione.

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
