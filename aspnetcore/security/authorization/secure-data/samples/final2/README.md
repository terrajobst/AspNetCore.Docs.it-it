# <a name="how-to-buildrun-secure-user-data-sample"></a>Come compilare ed eseguire il campione di dati sicura degli utenti

* Impostazione della password con lo strumento Secret Manager:

  `dotnet user-secrets set SeedUserPW <pw>`

* Aggiornare il database:

  `dotnet ef database update`

* Abilitare HTTPS nel progetto
