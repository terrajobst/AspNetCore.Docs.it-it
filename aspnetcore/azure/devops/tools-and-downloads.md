---
title: Strumenti e download - DevOps con ASP.NET Core e Azure
author: CamSoper
description: Strumenti e download necessari per DevOps con ASP.NET Core e Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 9c1042dd48b9167209b46e97a09e011b80e2609c
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511145"
---
# <a name="tools-and-downloads"></a>Strumenti e download

Azure offre diverse interfacce per il provisioning e la gestione delle risorse, ad esempio [portale di Azure](https://portal.azure.com), l'interfaccia della riga di comando di [Azure](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure cloud Shell](https://shell.azure.com/bash)e Visual Studio. Questa Guida adotta un approccio minimalista e Usa Azure Cloud Shell ogni volta che è possibile ridurre i passaggi necessari. Tuttavia, il portale di Azure deve essere usato per alcune parti.

## <a name="prerequisites"></a>Prerequisiti

Le sottoscrizioni seguenti sono necessari:

* Azure &mdash; se non si ha un account, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).
* Azure DevOps Services &mdash; la sottoscrizione e l'organizzazione di Azure DevOps vengono create nel capitolo 4.
* GitHub &mdash; se non si ha un account, [iscriversi](https://github.com/join)gratuitamente.

Gli strumenti seguenti sono necessari:

* [Git](https://git-scm.com/downloads) &mdash; una conoscenza di base di Git è consigliata per questa guida. Esaminare la [documentazione di git](https://git-scm.com/doc), in particolare [git Remote](https://git-scm.com/docs/git-remote) e [git push](https://git-scm.com/docs/git-push).
* Per compilare ed eseguire l'app di esempio, è necessario [.NET Core SDK](https://dotnet.microsoft.com/download/) &mdash; versione 2.1.300 o successiva. Se Visual Studio viene installato con il carico di lavoro **sviluppo multipiattaforma .NET Core** , il .NET Core SDK è già installato.

    Verificare l'installazione di .NET Core SDK. Aprire una shell dei comandi ed eseguire il comando seguente:

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Strumenti consigliati (solo Windows)

* I robusti strumenti di Azure di [Visual Studio](https://visualstudio.microsoft.com)forniscono un'interfaccia utente grafica per la maggior parte delle funzionalità descritte in questa guida. Qualsiasi edizione di Visual Studio funzionerà, tra cui l'edizione gratuita di Visual Studio Community. Le esercitazioni vengono scritti per illustrare lo sviluppo, distribuzione e DevOps con e senza Visual Studio.

  Verificare che in Visual Studio siano installati i [carichi di lavoro](/visualstudio/install/modify-visual-studio) seguenti:

  * Sviluppo Web e ASP.NET
  * Sviluppo di Azure
  * Sviluppo multipiattaforma .NET Core
