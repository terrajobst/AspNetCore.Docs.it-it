---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Distribuzione di database di appartenenza negli ambienti aziendali | Microsoft Docs
author: jrjlee
description: Questo argomento illustra le considerazioni principali e le sfide da superare durante il provisioning di database di servizi dell'applicazione ASP.NET (più comune...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 9df152866b54f55c2b00611331e868f98bd2f3e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827188"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Distribuzione di database di appartenenza negli ambienti aziendali
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento illustra le considerazioni principali e le sfide, occorre superare quando i database (più comunemente detto database di appartenenza) negli ambienti di test, gestione temporanea o produzione dei servizi è eseguire il provisioning dell'applicazione ASP.NET. Vengono inoltre descritti gli approcci che è possibile usare per soddisfare questi requisiti.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Quali sono i problemi quando si distribuisce un Database di appartenenza?

Nella maggior parte dei casi, quando si escogitare una strategia di distribuzione per un database, la prima cosa da considerare è quali dati si desidera distribuire. In un ambiente di sviluppo o test, si potrebbe essere necessario distribuire i dati dell'account utente per facilitare il test rapido e immediato. In un ambiente di gestione temporanea o produzione, è molto improbabile che si desidera distribuire i dati dell'account utente.

Sfortunatamente, i database di appartenenza ASP.NET introducono alcuni problemi specifici che prendere questa decisione molto più complessa:

- Una distribuzione con solo schema lascia il database delle appartenenze in uno stato non operativo. Infatti, il database di appartenenza comprende alcuni dati di configurazione (nelle **aspnet\_SchemaVersions** tabella) che il database necessita per funzionare. Di conseguenza, se si esegue una distribuzione solo allo schema del database di appartenenza per escludere i dati dell'account utente, è necessario eseguire uno script di post-distribuzione e aggiungere i dati di configurazione essenziali.
- A seconda del modo in cui il database di appartenenze è configurato, il provider di appartenenze può usare la chiave del computer per crittografare le password e archiviarli nel database. In questo caso, eventuali dati dell'account utente che si distribuisce con il database diventerà inutilizzabili nel server di destinazione. Per questo motivo, la distribuzione di dati dell'account utente non è uno scenario supportato.

## <a name="choosing-a-membership-database-strategy"></a>Scelta di una strategia di Database di appartenenza

Usare queste linee guida quando si sceglie come effettuare il provisioning di un database di appartenenza in un ambiente server aziendale:

- Laddove possibile, non vengono distribuiti i database di appartenenza. In alternativa, creare manualmente i database delle appartenenze nel server di database di destinazione. Se non è stato personalizzato dello schema del database di appartenenza, è possibile semplicemente creare uno nuovo in situ, di destinazione utilizzando il [strumento di registrazione di SQL Server ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Se non esiste nessuna opzione ma alla distribuzione di un database di appartenenza&#x2014;, ad esempio, se sono state apportate modifiche estese allo schema del database&#x2014;è consigliabile eseguire una distribuzione di solo schema di database di appartenenza, per escludere i dati dell'account utente e quindi eseguire uno script di post-distribuzione per aggiungere tutti i dati di configurazione necessarie. È possibile trovare indicazioni generali su questi approcci [procedura: distribuire l'ASP.NET l'appartenenza al Database senza inclusi account utente](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

È importante ricordare che *lo schema del database di appartenenza è probabile che sia abbastanza statici*. Anche se è stato personalizzato il database di appartenenza, è improbabile che è necessario aggiornare lo schema a intervalli regolari&#x2014;non verrà modificata con la stessa frequenza con cui il codice in un'applicazione web o un progetto di database. Di conseguenza, non è necessario includere il database delle appartenenze in tutti i processi automatizzati o passo a passo la distribuzione.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Uso VSDBCMD per aggiornare uno Schema di Database di appartenenza

Se si modifica la struttura del database di appartenenza dopo la prima distribuzione, non si desideri usare lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) per ridistribuire il database. La funzionalità di distribuzione di database di distribuzione Web non include la possibilità di apportare aggiornamenti differenziali per un database di destinazione&#x2014;, invece, distribuzione Web è necessario eliminare e ricreare il database. Ciò significa che si perdono i dati di account utente esistenti, che non sono in genere auspicabile negli ambienti di gestione temporanea o produzione.

L'alternativa consiste nell'usare l'utilità VSDBCMD per aggiornare lo schema del database di destinazione. VSDBCMD include due funzionalità importanti. In primo luogo, è possibile importare lo schema di un database esistente in un file. dbschema. In secondo luogo, è possibile distribuire un file di dbschema a un database esistente come un aggiornamento differenziale, il che significa che esegue solo le modifiche necessarie per portare il database di destinazione aggiornato e non perdere i dati.

È possibile usare i passaggi generali per aggiornare uno schema di database di appartenenza:

1. Usare lo strumento VSDBCMD **importazione** azione per generare un file. dbschema per il database di appartenenza di origine. Questa procedura è descritta in [procedura: importare uno Schema da un prompt dei comandi](https://msdn.microsoft.com/library/dd172135.aspx).
2. Usare lo strumento VSDBCMD **Distribuisci** azione per distribuire il file. dbschema al database di appartenenza di destinazione. Questa procedura è descritta nella [riferimento della riga di comando per VSDBCMD. EXE (distribuzione e importazione dello Schema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusione

Questo argomento descrive alcune delle problematiche che potrebbero verificarsi quando è necessario eseguire il provisioning dei database di appartenenza ASP.NET in vari ambienti di destinazione. In particolare, spiega il motivo per cui le distribuzioni di solo schema lascia il database delle appartenenze in uno stato non operativo e il motivo per cui la distribuzione di dati dell'account utente non è supportata. L'articolo presenta inoltre materiale sussidiario su come effettuare il provisioning, distribuire e aggiornare i database di appartenenza in scenari diversi.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre linee guida ed esempi di come usare VSDBCMD, vedere [riferimento della riga di comando per VSDBCMD. EXE (distribuzione e importazione dello Schema)](https://msdn.microsoft.com/library/dd193283.aspx) e [procedura: importare uno Schema da un prompt dei comandi](https://msdn.microsoft.com/library/dd172135.aspx). Per altre informazioni sull'uso di aspnet\_regsql.exe per creare i database di appartenenza, vedere [strumento di registrazione di SQL Server ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Per istruzioni generali sulla distribuzione dei database di appartenenza, vedere [procedura: distribuire l'ASP.NET l'appartenenza al Database senza inclusi account utente](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Precedente](deploying-database-role-memberships-to-test-environments.md)
> [Successivo](excluding-files-and-folders-from-deployment.md)
