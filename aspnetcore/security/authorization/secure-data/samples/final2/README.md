---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210106"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="ec9e3-101">Come compilare ed eseguire il campione di dati sicura degli utenti</span><span class="sxs-lookup"><span data-stu-id="ec9e3-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="ec9e3-102">Impostazione della password con lo strumento Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="ec9e3-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="ec9e3-103">Aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="ec9e3-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="ec9e3-104">Abilitare HTTPS nel progetto</span><span class="sxs-lookup"><span data-stu-id="ec9e3-104">Enable HTTPS in the project</span></span>
