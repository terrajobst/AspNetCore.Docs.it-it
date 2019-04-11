---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472337"
---
*  Considerare attendibile il certificato di sviluppo HTTPS eseguendo il comando riportato di seguito:

    ```console
    dotnet dev-certs https --trust
    ```

    Il comando precedente consente di visualizzare la finestra di dialogo seguente:

    ![Finestra di dialogo Avviso di sicurezza](~/getting-started/_static/cert.png)

*    Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.

     Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).