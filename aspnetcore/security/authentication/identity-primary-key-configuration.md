---
title: "Configurare il tipo di dati di identità chiavi primarie"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="b3648-102">Configurare il tipo di dati di identità chiavi primarie</span><span class="sxs-lookup"><span data-stu-id="b3648-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="b3648-103">Identità di ASP.NET Core consente di configurare con facilità il tipo di dati desiderato per le chiavi primarie.</span><span class="sxs-lookup"><span data-stu-id="b3648-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="b3648-104">Per impostazione predefinita, Usa identità di tipo di dati stringa, ma è possibile ignorare molto rapidamente questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="b3648-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="b3648-105">Procedura</span><span class="sxs-lookup"><span data-stu-id="b3648-105">How to</span></span>

1.  <span data-ttu-id="b3648-106">Il primo passaggio consiste nell'implementare il modello di identità e il tipo di stringa con il tipo di dati che si desidera eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="b3648-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  <span data-ttu-id="b3648-107">Implementare il contesto del database di identità con i modelli e il tipo di dati desiderato per le chiavi primarie</span><span class="sxs-lookup"><span data-stu-id="b3648-107">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  <span data-ttu-id="b3648-108">Utilizzare i modelli e il tipo di dati desiderato per le chiavi primarie quando si dichiara il servizio di identità nella classe di avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="b3648-108">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
