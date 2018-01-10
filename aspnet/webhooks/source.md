---
uid: webhooks/source
title: Codice sorgente Webhook ASP.NET e i pacchetti NuGet | Documenti Microsoft
author: rick-anderson
description: Collegamenti a codice sorgente Webhook ASP.NET e i pacchetti NuGet
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2018
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="4fc7a-103">Codice sorgente Webhook ASP.NET e i pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="4fc7a-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="4fc7a-104">Microsoft ASP.NET WebHooks fa parte della famiglia Microsoft ASP.NET di moduli ed è ospitato come un [Apri progetto di origine in GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="4fc7a-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="4fc7a-105">Ciò significa che è accettare i contributi ma, esaminare il [linee guida contributo](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) prima di inviare una richiesta pull.</span><span class="sxs-lookup"><span data-stu-id="4fc7a-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="4fc7a-106">Questa documentazione in linea che si sta leggendo questo punto è ospitata anche come [Open Source su GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e inoltre accetta contributi.</span><span class="sxs-lookup"><span data-stu-id="4fc7a-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="4fc7a-107">Pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="4fc7a-107">NuGet packages</span></span>

<span data-ttu-id="4fc7a-108">Il [pacchetti NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sono suddivisi in tre parti:</span><span class="sxs-lookup"><span data-stu-id="4fc7a-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="4fc7a-109">[Comuni](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un pacchetto comune che è condiviso tra mittenti e ricevitori.</span><span class="sxs-lookup"><span data-stu-id="4fc7a-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="4fc7a-110">[Mittente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un set di pacchetti che supporta l'invio delle proprie Webhook ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="4fc7a-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="4fc7a-111">In dettaglio in cui sono descritte le funzionalità per l'invio di Webhook [invio Webhook](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="4fc7a-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="4fc7a-112">[Ricevitori](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un set di pacchetti che supporta la ricezione di Webhook da altri utenti.</span><span class="sxs-lookup"><span data-stu-id="4fc7a-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="4fc7a-113">La funzionalità per la ricezione di Webhook è descritto in dettaglio in [ricezione Webhook](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="4fc7a-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
