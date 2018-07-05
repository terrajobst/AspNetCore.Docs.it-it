---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Soluzione Contact Manager | Microsoft Docs
author: jrjlee
description: Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;soluzione Contact Manager&#x2014;per rappresentare un'applicazione di livello aziendale con un livello di realistico...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 8187766190da43ded52359892601f8129b9940ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388108"
---
<a name="the-contact-manager-solution"></a>Soluzione Contact Manager
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ciò [serie di esercitazioni](web-deployment-in-the-enterprise.md) utilizza una soluzione di esempio&#x2014;della soluzione Contact Manager&#x2014;per rappresentare un'applicazione di livello aziendale con un livello di complessità realistico. In questo argomento viene introdotta la soluzione Contact Manager, vengono descritti i componenti chiave della soluzione e identifica le problematiche correlate alla distribuzione di questo tipo di applicazione a diverse piattaforme di destinazione in un ambiente aziendale.
> 
> Mentre si lavora con gli argomenti in queste esercitazioni, è possibile usare la soluzione Contact Manager come un'implementazione di riferimento che illustra come è possibile soddisfare specifiche problematiche in scenari di distribuzione enterprise. Argomento successivo [impostazione di backup della soluzione Contact Manager](setting-up-the-contact-manager-solution.md), viene descritto come scaricare ed eseguire la soluzione in della workstation dello sviluppatore.


## <a name="solution-overview"></a>Panoramica della soluzione

La soluzione Contact Manager è costituita da quattro progetti singoli:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Si tratta di un progetto di applicazione web ASP.NET MVC 3 che rappresenta il punto di ingresso per la soluzione. Offre alcune funzionalità di applicazione web di base, ad esempio offrendo agli utenti la possibilità di creare e visualizzare i dettagli dei contatti. L'applicazione si basa su un servizio Windows Communication Foundation (WCF) per gestire i contatti e un database di servizi di applicazioni ASP.NET per gestire l'autenticazione e autorizzazione.
- **ContactManager.Database**. Si tratta di un progetto di database di Visual Studio. Il progetto definisce lo schema per un database che archivia i dettagli di contatto.
- **ContactManager.Service**. Si tratta di un progetto di servizio web WCF. Il servizio espone WCF crea un endpoint che consente ai chiamanti di eseguire, recuperare, aggiornare ed eliminare operazioni (CRUD) di **ContactManager** database. Il servizio si basa sul **ContactManager** database e il **ContactManager.Common.dll** assembly.
- **ContactManager.Common**. Si tratta di un progetto di libreria di classi. Il servizio WCF si basa sui tipi definiti in questo assembly.

La soluzione include anche una cartella di soluzione denominata pubblica. Questo file contiene diversi file di progetto personalizzato e i file di comando che illustrano come è possibile controllare e modificare il processo di compilazione e distribuzione. Questi prerequisiti sono analizzati in modo più dettagliato più avanti in questa esercitazione.

A livello concettuale, i componenti della soluzione si integrino simile al seguente:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Mentre l'applicazione web ASP.NET MVC 3 Usa il provider di appartenenze ASP.NET, tutte le pagine all'interno dell'applicazione web consentono accessi anonimi. Non si tratta chiaramente una configurazione realistica. Tuttavia, la soluzione è configurata in questo modo per renderne più semplice per distribuire e testare la soluzione non si configurano i ruoli e gli account utente.


## <a name="deployment-challenges"></a>Sfide della distribuzione

La soluzione Contact Manager illustra diverse sfide della distribuzione che sono comuni a molti degli scenari di distribuzione aziendale:

- La soluzione è costituita da più progetti dipendenti. È necessario distribuire questi progetti contemporaneamente.
- Stringhe di connessione e gli endpoint di servizio devono essere aggiornate per ogni ambiente e in molti casi queste informazioni non sarà disponibile per gli sviluppatori.
- Quando si distribuisce il **ContactManager** database agli ambienti di staging e produzione, è necessario mantenere i dati esistenti nelle distribuzioni successive.
- Quando si distribuisce il database dei servizi dell'applicazione ASP.NET, è necessario distribuire alcuni dati di configurazione, ma omette tutti i dati account utente.
- I progetti includono alcuni file e cartelle che non devono essere distribuite. È necessario escludere questi file e cartelle dal processo di distribuzione.
- La soluzione deve supportare la distribuzione automatizzata da un server di compilazione di Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusione

In questo argomento fornita una panoramica generale della soluzione Contact Manager e identificate alcune delle difficoltà inerenti distribuzione che sono comuni a molti degli scenari di distribuzione enterprise. Gli altri argomenti di questa esercitazione vengono descritte alcune delle tecniche utilizzabili per soddisfare questi requisiti.

Argomento successivo [impostazione di backup della soluzione Contact Manager](setting-up-the-contact-manager-solution.md), viene descritto come scaricare ed eseguire la soluzione in della workstation dello sviluppatore.

> [!div class="step-by-step"]
> [Precedente](web-deployment-in-the-enterprise.md)
> [Successivo](setting-up-the-contact-manager-solution.md)
