---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665509"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="116ec-101">Esempio di controller API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="116ec-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="116ec-102">L'app di esempio è costituita dai progetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="116ec-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="116ec-103">**WebApiSample.Api.22**: un progetto ASP.NET Core 2.2 destinato a .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="116ec-103">**WebApiSample.Api.22**: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="116ec-104">**WebApiSample.Api.21**: un progetto ASP.NET Core 2.1 destinato a .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="116ec-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="116ec-105">**WebApiSample.Api.Pre21**: un progetto ASP.NET Core 2.0 destinato a .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="116ec-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="116ec-106">**WebApiSample.DataAccess**: una libreria di classi .NET Standard 2.0 che funge da livello di accesso ai dati per i 2 progetti API Web.</span><span class="sxs-lookup"><span data-stu-id="116ec-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="116ec-107">Questo esempio illustra le varie possibilità di creazione del controller API Web:</span><span class="sxs-lookup"><span data-stu-id="116ec-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="116ec-108">Derivare una classe da ControllerBase</span><span class="sxs-lookup"><span data-stu-id="116ec-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="116ec-109">Annotare una classe con l'attributo ApiController</span><span class="sxs-lookup"><span data-stu-id="116ec-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
