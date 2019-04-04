---
ms.openlocfilehash: 209b5c41e17897693962954b1e795bdbb41f9384
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012734"
---
# <a name="key-vault-configuration-provider-sample-app"></a>App di esempio di istanza di Key Vault Configuration Provider

In questo esempio viene illustrato l'utilizzo del Provider di configurazione dell'insieme di credenziali chiave di Azure.

Nell'esempio viene eseguito in una delle due modalità di base di `#define` istruzione all'inizio del *Program.cs* file. Per istruzioni, vedere [direttive del preprocessore nel codice di esempio](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):

* `Certificate` &ndash; Illustra l'uso di un certificato X.509 e ID Client dell'insieme di credenziali chiave di Azure per i segreti di accesso archiviato in Azure Key Vault. Questa versione dell'esempio può essere eseguita da qualsiasi posizione, distribuiti in Azure App Service o qualsiasi host in grado di servire un'app ASP.NET Core.
* `Managed` &ndash; Illustra l'uso di Azure [identità del servizio gestito](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) per eseguire l'autenticazione all'app di Azure Key Vault con l'autenticazione di Azure AD senza credenziali nel codice o nella configurazione dell'app. Un ID Client di Azure AD e il segreto non sono necessari per l'app per l'autenticazione con Azure Key Vault. In questo esempio deve essere distribuito al servizio App di Azure per esplorare il scearnio identità gestita.

Per altre informazioni, vedere [Azure Key Vault Configuration Provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).
