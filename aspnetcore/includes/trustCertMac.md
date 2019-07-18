* Considerare attendibile il certificato di sviluppo HTTPS eseguendo il comando riportato di seguito:

    ```console
    dotnet dev-certs https --trust
    ```

* Il comando precedente visualizza l'output seguente:

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* Se richiesto, immettere il nome utente e la password dell'amministratore.  Il certificato verrà installato e impostato come attendibile.

    Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).