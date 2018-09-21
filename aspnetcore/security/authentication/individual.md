---
title: Articoli basati su ASP.NET Core i progetti creati con singoli account utente
author: rick-anderson
description: Scopri articoli basati su ASP.NET Core i progetti creati con singoli account utente.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523064"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="adb80-103">Articoli basati su ASP.NET Core i progetti creati con singoli account utente</span><span class="sxs-lookup"><span data-stu-id="adb80-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="adb80-104">ASP.NET Core Identity è incluso nei modelli di progetto in Visual Studio con l'opzione "Account utente individuali".</span><span class="sxs-lookup"><span data-stu-id="adb80-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="adb80-105">I modelli di autenticazione sono disponibili in .NET Core CLI con `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="adb80-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="adb80-106">Nessuna autenticazione</span><span class="sxs-lookup"><span data-stu-id="adb80-106">No Authentication</span></span>

<span data-ttu-id="adb80-107">Viene specificata l'autenticazione in .NET Core CLI con il `-au` opzione.</span><span class="sxs-lookup"><span data-stu-id="adb80-107">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="adb80-108">In Visual Studio, il **Modifica autenticazione** finestra di dialogo è disponibile per le nuove applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="adb80-108">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="adb80-109">Il valore predefinito per la nuova App web in Visual Studio viene **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="adb80-109">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="adb80-110">Progetti creati con nessuna autenticazione:</span><span class="sxs-lookup"><span data-stu-id="adb80-110">Projects created with no authentication:</span></span>

* <span data-ttu-id="adb80-111">Non contengono le pagine web e dell'interfaccia utente per accedere e disconnettersi.</span><span class="sxs-lookup"><span data-stu-id="adb80-111">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="adb80-112">Non contengono codice di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="adb80-112">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="adb80-113">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="adb80-113">Windows Authentication</span></span>

<span data-ttu-id="adb80-114">Viene specificata l'autenticazione di Windows per le nuove app web in .NET Core CLI con il `-au Windows` opzione.</span><span class="sxs-lookup"><span data-stu-id="adb80-114">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="adb80-115">In Visual Studio, il **Modifica autenticazione** finestra di dialogo offre la **l'autenticazione di Windows** opzioni.</span><span class="sxs-lookup"><span data-stu-id="adb80-115">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="adb80-116">Se si seleziona l'autenticazione di Windows, l'app è configurata per usare la [modulo di Windows autenticazione IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="adb80-116">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="adb80-117">L'autenticazione di Windows è progettato per i siti web Intranet.</span><span class="sxs-lookup"><span data-stu-id="adb80-117">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="adb80-118">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="adb80-118">Additional resources</span></span>

<span data-ttu-id="adb80-119">Gli articoli seguenti illustrano come usare il codice generato nei modelli di ASP.NET Core che usano singoli account utente:</span><span class="sxs-lookup"><span data-stu-id="adb80-119">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="adb80-120">Autenticazione a due fattori con SMS</span><span class="sxs-lookup"><span data-stu-id="adb80-120">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* <span data-ttu-id="adb80-121">[Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="adb80-121">[Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm)</span></span>
* [<span data-ttu-id="adb80-122">Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione</span><span class="sxs-lookup"><span data-stu-id="adb80-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
