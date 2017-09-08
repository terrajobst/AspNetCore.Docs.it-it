# <a name="how-to-buildrun-secure-user-data-sample"></a>Come compilare ed eseguire l'esempio di dati utente protetto

* Impostare la password con lo strumento di gestione di chiave privata:

  `dotnet user-secrets set SeedUserPW <pw>`

* Aggiornare il database:

    `dotnet ef database update`
