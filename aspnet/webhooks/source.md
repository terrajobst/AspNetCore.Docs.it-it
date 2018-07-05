---
uid: webhooks/source
title: Webhook ASP.NET il codice sorgente e i pacchetti NuGet | Microsoft Docs
author: rick-anderson
description: Collegamenti a codice sorgente Webhook ASP.NET e i pacchetti NuGet
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 1ee4adc2a28054ed1f856d7e4e991b34972a70e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802180"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Webhook ASP.NET il codice sorgente e i pacchetti NuGet

Microsoft ASP.NET WebHooks fa parte della famiglia Microsoft ASP.NET di moduli e viene ospitato come un [Open Source Project su GitHub](https://github.com/aspnet/WebHooks). Ciò significa che abbiamo accetterà i contributi, ma, esaminare i [linee guida specifiche](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) prima di inviare una richiesta pull.

Questa documentazione online di cui si esegue la lettura a questo punto è ospitata anche come [Open Source su GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e accetta anche i contributi.

## <a name="nuget-packages"></a>Pacchetti NuGet

Il [i pacchetti NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sono suddivisi in tre parti:

* [Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un pacchetto più comune che è condiviso tra mittenti e ricevitori.

* [Mittente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un set di pacchetti che supportano l'invio di Webhook personalizzati ad altri utenti. La funzionalità per l'invio di Webhook è descritto più dettagliatamente [l'invio di Webhook](sending/index.md).

* [I ricevitori](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un set di pacchetti che supportano la ricezione di Webhook da altri utenti. La funzionalità per ricevere i Webhook è descritto più dettagliatamente [ricezione Webhook](receiving/index.md).
