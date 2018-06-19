---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: La soluzione di gestione di contatto | Documenti Microsoft
author: jrjlee
description: Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;la soluzione Contact Manager&#x2014;per rappresentare un'applicazione su larga scala con un livello realistico...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883700"
---
<a name="the-contact-manager-solution"></a>La soluzione di gestione di contatto
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ciò [serie di esercitazioni](web-deployment-in-the-enterprise.md) utilizza una soluzione di esempio&#x2014;la soluzione di gestione di contatto&#x2014;per rappresentare un'applicazione su larga scala con un livello di complessità realistico. In questo argomento introduce la soluzione di gestione di contatto, vengono descritti i componenti chiavi della soluzione e identifica le problematiche correlate alla distribuzione di questo tipo di applicazione per diverse piattaforme di destinazione in un ambiente aziendale.
> 
> Quando si utilizzano gli argomenti in queste esercitazioni, è possibile utilizzare la soluzione di gestione di contatto come un'implementazione di riferimento che illustra come è possibile soddisfare i problemi specifici in scenari di distribuzione dell'organizzazione. L'argomento successivo, [impostazione backup Contact Manager soluzione](setting-up-the-contact-manager-solution.md), viene descritto come scaricare ed eseguire la soluzione nella workstation di sviluppo.


## <a name="solution-overview"></a>Panoramica della soluzione

La soluzione di gestione di contatto è costituita da quattro progetti singoli:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Si tratta di un progetto di applicazione web ASP.NET MVC 3 che rappresenta il punto di ingresso per la soluzione. Offre alcune funzionalità di applicazione web di base, ad esempio fornire agli utenti la possibilità di creare e visualizzare i dettagli di contatto. L'applicazione si basa su un servizio Windows Communication Foundation (WCF) per gestire i contatti e un database di servizi dell'applicazione ASP.NET per gestire l'autenticazione e autorizzazione.
- **ContactManager.Database**. Si tratta di un progetto di database di Visual Studio. Il progetto definisce lo schema per un database che archivia i dettagli di contatto.
- **ContactManager.Service**. Si tratta di un progetto di servizio web WCF. Il servizio espone WCF crea un endpoint che consente ai chiamanti di eseguire, recuperare l'aggiornamento ed eliminazione (CRUD) sul **ContactManager** database. Il servizio si basa sul **ContactManager** database e **ContactManager.Common.dll** assembly.
- **ContactManager.Common**. Si tratta di un progetto libreria di classi. Il servizio WCF si basa sui tipi definiti in questo assembly.

La soluzione include anche una cartella di soluzione denominata pubblica. Contiene diversi file di progetto personalizzati e i file di comando che illustrano come è possibile controllare e modificare il processo di compilazione e distribuzione. Questi prerequisiti sono analizzati in modo più dettagliato più avanti in questa esercitazione.

A livello concettuale, i componenti della soluzione disposte simile al seguente:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Mentre l'applicazione web ASP.NET MVC 3 utilizza il provider di appartenenze ASP.NET, tutte le pagine all'interno dell'applicazione web consentono accessi anonimi. Questa operazione è chiaramente una configurazione realistica. Tuttavia, la soluzione sia configurata in questo modo per renderne più semplice per distribuire e testare la soluzione senza configurare ruoli e account utente.


## <a name="deployment-challenges"></a>Problemi di distribuzione

La soluzione di gestione di contatto vengono illustrate diverse problematiche di distribuzione che sono comuni a molti degli scenari di distribuzione aziendale:

- La soluzione è costituita da più progetti dipendenti. È necessario distribuire questi progetti contemporaneamente.
- Stringhe di connessione e gli endpoint del servizio devono essere aggiornati per ogni ambiente, e in molti casi queste informazioni non saranno disponibili per gli sviluppatori.
- Quando si distribuisce il **ContactManager** database agli ambienti di gestione temporanea e produzione, è necessario mantenere i dati esistenti nelle distribuzioni successive.
- Quando si distribuisce il database di servizi dell'applicazione ASP.NET, è necessario distribuire alcuni dati di configurazione, ma si omette i dati di account utente.
- I progetti includono alcuni file e cartelle che non devono essere distribuite. È necessario escludere tali file e cartelle dal processo di distribuzione.
- La soluzione deve supportare la distribuzione automatica da un server di compilazione di Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusione

In questo argomento fornita una panoramica generale della soluzione di gestione di contatto e identificate alcune delle problematiche inerenti distribuzione che sono comuni a molti degli scenari di distribuzione dell'organizzazione. Gli altri argomenti di questa esercitazione vengono descritte alcune delle tecniche è possibile utilizzare per soddisfare questi requisiti.

L'argomento successivo, [impostazione backup Contact Manager soluzione](setting-up-the-contact-manager-solution.md), viene descritto come scaricare ed eseguire la soluzione nella workstation di sviluppo.

> [!div class="step-by-step"]
> [Precedente](web-deployment-in-the-enterprise.md)
> [Successivo](setting-up-the-contact-manager-solution.md)
