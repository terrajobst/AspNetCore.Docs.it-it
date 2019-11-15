::: moniker range=">= aspnetcore-3.0"
Visual Studio visualizza la finestra di dialogo seguente:

![Il progetto è configurato per l'utilizzo di SSL. Per evitare gli avvisi SSL nel browser, è possibile scegliere di considerare attendibile il certificato autofirmato generato da ASP.NET Core. Si desidera considerare attendibile il certificato SSL ASP.NET Core?](~/getting-started/_static/trustCert-3x.png)

Selezionare **Sì** se si considera attendibile il certificato SSL ASP.NET Core.

Verrà visualizzata la finestra di dialogo seguente:

![Finestra di dialogo Avviso di sicurezza](~/getting-started/_static/cert.png)

Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.

Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).
::: moniker-end

::: moniker range="< aspnetcore-3.0"
Visual Studio visualizza la finestra di dialogo seguente:

![Il progetto è configurato per l'utilizzo di SSL. Per evitare la visualizzazione di avvisi SSL nel browser è possibile considerare attendibile il certificato autofirmato generato da IIS Express. Considerare attendibile il certificato SSL di IIS Express?](~/getting-started/_static/trustCert.png)

Selezionare **Sì** se si considera attendibile il certificato SSL di IIS Express.

Verrà visualizzata la finestra di dialogo seguente:

![Finestra di dialogo Avviso di sicurezza](~/getting-started/_static/cert.png)

Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.

Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).
::: moniker-end