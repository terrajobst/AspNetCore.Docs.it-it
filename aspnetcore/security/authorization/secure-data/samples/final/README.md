# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="aa2b1-101">Come compilare ed eseguire l'esempio di dati utente protetto</span><span class="sxs-lookup"><span data-stu-id="aa2b1-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="aa2b1-102">Impostare la password con lo strumento di gestione di chiave privata:</span><span class="sxs-lookup"><span data-stu-id="aa2b1-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="aa2b1-103">Aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="aa2b1-103">Update the database:</span></span>

    `dotnet ef database update`
