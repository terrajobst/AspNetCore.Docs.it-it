---
title: Strumenti e download - DevOps con ASP.NET Core e Azure
author: CamSoper
description: Strumenti e download necessari per DevOps con ASP.NET Core e Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 0c64e723f1b912323103f201a66c1edaeccdcc2d
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284348"
---
# <a name="tools-and-downloads"></a>Strumenti e download

Azure offre diverse interfacce per il provisioning e gestione delle risorse, ad esempio la [portale di Azure](https://portal.azure.com), [CLI di Azure](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Cloud di Azure Shell](https://shell.azure.com/bash)e Visual Studio. Questa Guida adotta un approccio minimalista e Usa Azure Cloud Shell ogni volta che è possibile ridurre i passaggi necessari. Tuttavia, il portale di Azure deve essere usato per alcune parti.

## <a name="prerequisites"></a>Prerequisiti

Le sottoscrizioni seguenti sono necessari:

* Azure &mdash; se non hai un account [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).
* Servizi di Azure DevOps &mdash; viene creata la sottoscrizione di Azure DevOps e organizzazione nel capitolo 4.
* GitHub &mdash; se non hai un account [Iscriviti gratuitamente](https://github.com/join).

Gli strumenti seguenti sono necessari:

* [GIT](https://git-scm.com/downloads) &mdash; acquisire familiarità con Git è consigliato per questa Guida. Rivedere le [documentazione su Git](https://git-scm.com/doc), in particolare [git remoto](https://git-scm.com/docs/git-remote) e [git push](https://git-scm.com/docs/git-push).
* [.NET core SDK](https://www.microsoft.com/net/download/) &mdash; versione 2.1.300 o in un secondo momento, è necessario per compilare ed eseguire l'app di esempio. Se è installato Visual Studio con il **sviluppo multipiattaforma .NET Core** carico di lavoro, .NET Core SDK è già installato.

    Verificare l'installazione di .NET Core SDK. Aprire una shell dei comandi ed eseguire il comando seguente:

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Strumenti consigliati (solo Windows)

* [Visual Studio](https://www.visualstudio.com/)di strumenti di Azure affidabili forniscono un'interfaccia utente grafica per la maggior parte delle funzionalità descritte in questa Guida. Qualsiasi edizione di Visual Studio funzionerà, tra cui l'edizione gratuita di Visual Studio Community. Le esercitazioni vengono scritti per illustrare lo sviluppo, distribuzione e DevOps con e senza Visual Studio.

  Verificare che Visual Studio offre i seguenti [carichi di lavoro](/visualstudio/install/modify-visual-studio) installato:

  * Sviluppo ASP.NET e Web
  * Sviluppo di Azure
  * Sviluppo multipiattaforma .NET Core
