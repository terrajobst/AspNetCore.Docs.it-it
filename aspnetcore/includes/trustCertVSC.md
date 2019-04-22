---
ms.openlocfilehash: 260f774fdba4d16a4fcb00ac1c699acf4d1bf5b5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615386"
---
* Considerare attendibile il certificato di sviluppo HTTPS eseguendo il comando riportato di seguito:

  ```console
  dotnet dev-certs https --trust
  ```

  Il comando precedente consente di visualizzare la finestra di dialogo seguente:

  ![Finestra di dialogo Avviso di sicurezza](~/getting-started/_static/cert.png)

* Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.

  Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).
  
