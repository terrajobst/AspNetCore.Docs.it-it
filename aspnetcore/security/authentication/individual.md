---
title: Articoli basati su progetti ASP.NET Core creati con account utente singoli
author: rick-anderson
description: Scopri gli articoli in base ai progetti ASP.NET Core creati con singoli account utente.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: cf548417268a8587787471b9ed91c0ed109fbee9
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080696"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="0b01c-103">Articoli basati su progetti ASP.NET Core creati con account utente singoli</span><span class="sxs-lookup"><span data-stu-id="0b01c-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="0b01c-104">ASP.NET Core identità è inclusa nei modelli di progetto di Visual Studio con l'opzione "singoli account utente".</span><span class="sxs-lookup"><span data-stu-id="0b01c-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="0b01c-105">I modelli di autenticazione sono disponibili in interfaccia della riga di comando di .NET Core `-au Individual`con:</span><span class="sxs-lookup"><span data-stu-id="0b01c-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="0b01c-106">Vedere [questo problema di GitHub](https://github.com/aspnet/AspNetCore/issues/5833) per l'autenticazione dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="0b01c-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="0b01c-107">Nessuna autenticazione</span><span class="sxs-lookup"><span data-stu-id="0b01c-107">No Authentication</span></span>

<span data-ttu-id="0b01c-108">L'autenticazione viene specificata nella interfaccia della riga di comando di .NET Core con l' `-au` opzione.</span><span class="sxs-lookup"><span data-stu-id="0b01c-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="0b01c-109">In Visual Studio la finestra di dialogo **Modifica autenticazione** è disponibile per le nuove applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="0b01c-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="0b01c-110">Il valore predefinito per le nuove app Web in Visual Studio **non è Authentication**.</span><span class="sxs-lookup"><span data-stu-id="0b01c-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="0b01c-111">Progetti creati senza autenticazione:</span><span class="sxs-lookup"><span data-stu-id="0b01c-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="0b01c-112">Non contenere pagine Web e interfaccia utente per l'accesso e la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="0b01c-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="0b01c-113">Non contiene codice di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="0b01c-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="0b01c-114">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="0b01c-114">Windows Authentication</span></span>

<span data-ttu-id="0b01c-115">L'autenticazione di Windows viene specificata per le nuove app Web nella interfaccia della riga di comando di .NET Core `-au Windows` con l'opzione.</span><span class="sxs-lookup"><span data-stu-id="0b01c-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="0b01c-116">In Visual Studio la finestra di dialogo **Cambia autenticazione** fornisce le opzioni di **autenticazione di Windows** .</span><span class="sxs-lookup"><span data-stu-id="0b01c-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="0b01c-117">Se è selezionata l'autenticazione di Windows, l'app viene configurata per l'uso del [modulo IIS](xref:host-and-deploy/iis/modules)per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="0b01c-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="0b01c-118">L'autenticazione di Windows è destinata ai siti Web Intranet.</span><span class="sxs-lookup"><span data-stu-id="0b01c-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b01c-119">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0b01c-119">Additional resources</span></span>

<span data-ttu-id="0b01c-120">Gli articoli seguenti illustrano come usare il codice generato in ASP.NET Core modelli che usano singoli account utente:</span><span class="sxs-lookup"><span data-stu-id="0b01c-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="0b01c-121">Autenticazione a due fattori con SMS</span><span class="sxs-lookup"><span data-stu-id="0b01c-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* <span data-ttu-id="0b01c-122">[Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="0b01c-122">[Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm)</span></span>
* [<span data-ttu-id="0b01c-123">Creare un'app ASP.NET Core con i dati utente protetti dall'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="0b01c-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
