---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configurazione delle autorizzazioni per Team di distribuzione della compilazione | Microsoft Docs
author: jrjlee
description: In questo argomento descrive come configurare le autorizzazioni per abilitare il server di compilazione distribuire il contenuto al server web e server di database come parte di un automatizzati b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: f84e72bd5991b0407008ccdaff5243979cbb986e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384662"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Configurazione delle autorizzazioni per Team di distribuzione della compilazione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento descrive come configurare le autorizzazioni per abilitare il server di compilazione distribuire il contenuto al server web e server di database come parte di un processo di compilazione automatizzata.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

Quando si installa il servizio di compilazione 2010 Team Foundation Server (TFS), si specifica l'identità con cui si vuole eseguire il servizio. Per impostazione predefinita, si tratta dell'account di servizio di rete. In alternativa, è possibile configurare il servizio di compilazione per l'esecuzione con un account di dominio.

Qualsiasi attività di distribuzione che richiedono l'autenticazione di Windows e che si intende automatizzare con Team Build, verrà eseguito usando l'identità del servizio di compilazione. Di conseguenza, è necessario concedere l'identità del servizio di compilazione di tutte le autorizzazioni necessarie per i server web e i server di database.

> [!NOTE]
> L'account del servizio di rete Usa l'account del computer per eseguire l'autenticazione ad altri computer. Gli account Machine assumono la forma * [nome dominio]\[nome macchina] ***$**&#x2014;, ad esempio, **FABRIKAM\TFSBUILD$**. Di conseguenza, se il servizio di compilazione viene eseguita usando l'identità del servizio di rete, si devono concedere le autorizzazioni necessarie per l'identità dell'account computer per il server di compilazione.


## <a name="configuring-web-server-permissions"></a>Configurazione delle autorizzazioni per Server Web

Come descritto in [scelta dell'approccio a destra per la distribuzione Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), esistono due approcci principali, è possibile usare se si desidera distribuire i pacchetti web in un server web remoto:

- Distribuire l'applicazione da una posizione remota usando come destinazione il *servizio agente distribuzione Web* (noto anche come agente remoto) nel server di destinazione.
- Distribuire l'applicazione da una posizione remota usando come destinazione il *Internet Information Services* (*IIS) Web distribuire gestore* nel server di destinazione.

L'agente remoto presenta due limitazioni principali in questo caso:

- L'agente remoto supporta solo l'autenticazione NTLM. In altre parole, la distribuzione deve usare l'identità del servizio di compilazione&#x2014;è non può rappresentare un altro account.
- Per usare l'agente remoto, l'account che esegue la distribuzione deve essere un amministratore nel server di destinazione.

Insieme, queste due limitazioni rendono l'approccio agente remoto inaccettabile per una distribuzione di Team Build automatizzata. Per usare questo approccio, è necessario impostare il servizio di compilazione come account di amministratore su tutti i server web di destinazione.

Al contrario, l'approccio gestore distribuzione Web offre vari vantaggi:

- Il gestore di distribuzione Web supporta l'autenticazione di base su HTTPS, che consente di passare le credenziali di un account alternativo per lo strumento di distribuzione Web IIS (distribuzione Web).
- È possibile configurare i server web di destinazione per consentire agli utenti senza privilegi di amministratore distribuire il contenuto a specifici siti Web IIS tramite il gestore di distribuzione Web.

Di conseguenza, è ovviamente preferibile usare come destinazione il gestore di distribuzione Web quando si automatizza la distribuzione del pacchetto web da Team Build. Questo è il processo consigliato:

1. Creare un account di dominio con privilegi limitati da usare per la distribuzione.
2. Configurare il gestore di distribuzione Web e concedere all'account le autorizzazioni necessarie per distribuire il contenuto a un sito Web IIS specifici, come descritto in [configurazione di un Server Web per la pubblicazione distribuzione Web (gestore di distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Richiamare distribuzione Web e il gestore di distribuzione Web di destinazione, utilizzando l'autenticazione di base e fornendo le credenziali dell'account di dominio è stato creato, per eseguire la distribuzione.

Nel [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , soluzione di esempio si specifica il tipo di autenticazione (base o NTLM), le credenziali di distribuzione Web e l'indirizzo dell'endpoint (agente remoto o gestore distribuzione Web) nel file di progetto specifici dell'ambiente. Questi valori vengono usati per formulare ed eseguire un comando di distribuzione Web quando viene eseguito il file di progetto. Per altre informazioni, vedere [distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Per altre informazioni su come configurare la distribuzione gestore Web, incluse le procedure configurare le autorizzazioni, vedere [configurazione di un Server Web per la pubblicazione distribuzione Web (gestore di distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Per altre informazioni su come configurare l'agente remoto, vedere [configurazione di un Server Web per la pubblicazione distribuzione Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configurazione delle autorizzazioni Server di Database

Per distribuire un database in SQL Server, è necessario:

- Creare un account di accesso per l'account di distribuzione nell'istanza di SQL Server.
- Concedere l'accesso **DBCreator** le autorizzazioni per l'istanza di SQL Server.
- Dopo la distribuzione iniziale, aggiungere l'account di accesso per il **db\_proprietario** ruolo nel database di destinazione. Ciò è necessario quanto nelle successive distribuzioni, è sta modificando un database esistente anziché creare un nuovo database.

È possibile eseguire l'autenticazione a un'istanza di SQL Server usando l'autenticazione NTLM o autenticazione di SQL Server:

- Se si usa l'autenticazione NTLM, è necessario concedere le autorizzazioni descritte in precedenza per l'account del servizio di compilazione.
- Se si usa l'autenticazione di SQL Server, è necessario concedere le autorizzazioni descritte in precedenza per l'account di SQL Server. È anche necessario includere il nome utente di SQL Server e la password nella stringa di connessione che consente di distribuire il database.

Per istruzioni dettagliate su come configurare le autorizzazioni per la distribuzione del database, vedere [configurazione di un Server di Database per la pubblicazione di distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusione

A questo punto, è necessario comprendere le autorizzazioni necessarie, insieme alle opzioni di autenticazione open, quando si automatizza le distribuzioni di database e dell'applicazione web da Team Build. È anche possibile implementare le autorizzazioni necessarie sul server web IIS e i server di database di SQL Server.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sulla configurazione di ambienti di Windows server per supportare la distribuzione remota, vedere [configurazione di ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Precedente](deploying-a-specific-build.md)
