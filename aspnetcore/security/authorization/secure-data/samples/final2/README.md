# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="0521b-101">Come compilare ed eseguire il campione di dati sicura degli utenti</span><span class="sxs-lookup"><span data-stu-id="0521b-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="0521b-102">Impostazione della password con lo strumento Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="0521b-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="0521b-103">Aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="0521b-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="0521b-104">Abilitare HTTPS nel progetto</span><span class="sxs-lookup"><span data-stu-id="0521b-104">Enable HTTPS in the project</span></span>
