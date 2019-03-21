---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210106"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Come compilare ed eseguire il campione di dati sicura degli utenti

* Impostazione della password con lo strumento Secret Manager:

  `dotnet user-secrets set SeedUserPW <pw>`

* Aggiornare il database:

  `dotnet ef database update`

* Abilitare HTTPS nel progetto
