---
title: DevOps con ASP.NET Core e Azure | Strumenti e download
author: CamSoper
description: Questa guida include informazioni complete sulla creazione di una pipeline DevOps per un'app ASP.NET Core ospitata in Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: a63e97d9ab9eb0ed2fbd30e8c2e033f0c048d33e
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312301"
---
# <a name="tools-and-downloads"></a>Strumenti e download

Azure offre diverse interfacce per il provisioning e gestione delle risorse, ad esempio la [portale di Azure](https://portal.azure.com), [CLI di Azure](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Cloud di Azure Shell](https://shell.azure.com/bash)e Visual Studio. Questa Guida adotta un approccio minimalista e Usa Azure Cloud Shell ogni volta che è possibile ridurre i passaggi necessari. Tuttavia, il portale di Azure deve essere usato per alcune parti.

## <a name="prerequisites"></a>Prerequisiti

Le sottoscrizioni seguenti sono necessari:

* Azure &mdash; se non hai un account [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).
* Visual Studio Team Services (VSTS) &mdash; questo account viene creato nel capitolo 4.
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

  Verificare che Visual Studio offre i seguenti [carichi di lavoro](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installato:

  * Sviluppo ASP.NET e Web
  * Sviluppo di Azure
  * Sviluppo multipiattaforma .NET Core
