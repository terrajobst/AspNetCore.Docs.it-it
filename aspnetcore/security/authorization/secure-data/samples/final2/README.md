# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="598aa-101">Come compilare ed eseguire il campione di dati sicura degli utenti</span><span class="sxs-lookup"><span data-stu-id="598aa-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="598aa-102">Impostazione della password con lo strumento Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="598aa-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="598aa-103">Aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="598aa-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="598aa-104">Abilitare HTTPS nel progetto</span><span class="sxs-lookup"><span data-stu-id="598aa-104">Enable HTTPS in the project</span></span>
