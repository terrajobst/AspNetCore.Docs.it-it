---
title: "Configurare il tipo di dati di identità chiavi primarie"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="235d6-102">Configurare il tipo di dati di identità chiavi primarie</span><span class="sxs-lookup"><span data-stu-id="235d6-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="235d6-103">Identità di ASP.NET Core consente di configurare con facilità il tipo di dati desiderato per le chiavi primarie.</span><span class="sxs-lookup"><span data-stu-id="235d6-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="235d6-104">Per impostazione predefinita, Usa identità di tipo di dati stringa, ma è possibile ignorare molto rapidamente questo comportamento.</span><span class="sxs-lookup"><span data-stu-id="235d6-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="235d6-105">Procedura</span><span class="sxs-lookup"><span data-stu-id="235d6-105">How to</span></span>

1.  <span data-ttu-id="235d6-106">Il primo passaggio consiste nell'implementare il modello di identità e il tipo di stringa con il tipo di dati che si desidera eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="235d6-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    <span data-ttu-id="235d6-107">[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span><span class="sxs-lookup"><span data-stu-id="235d6-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span></span>

    <span data-ttu-id="235d6-108">[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span><span class="sxs-lookup"><span data-stu-id="235d6-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span></span>
    
2.  <span data-ttu-id="235d6-109">Implementare il contesto del database di identità con i modelli e il tipo di dati desiderato per le chiavi primarie</span><span class="sxs-lookup"><span data-stu-id="235d6-109">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    <span data-ttu-id="235d6-110">[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="235d6-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span></span>
    
3.  <span data-ttu-id="235d6-111">Utilizzare i modelli e il tipo di dati desiderato per le chiavi primarie quando si dichiara il servizio di identità nella classe di avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="235d6-111">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    <span data-ttu-id="235d6-112">[!code-csharp[Principale](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span><span class="sxs-lookup"><span data-stu-id="235d6-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span></span>
