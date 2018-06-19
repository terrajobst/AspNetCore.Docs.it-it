---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Distribuzione di database di appartenenza per ambienti aziendali | Documenti Microsoft
author: jrjlee
description: In questo argomento illustra le considerazioni chiave e i problemi, che è necessario risolvere quando si esegue il provisioning di database di servizi dell'applicazione ASP.NET (più comuni...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: b783fcf57759f2a431480eec6902105f6d683408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892478"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Distribuzione di database di appartenenza per ambienti aziendali
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo argomento illustra le considerazioni chiave e i problemi, che è necessario risolvere per database (più comunemente indicati come database di appartenenza) in ambienti di test, gestione temporanea o produzione dei servizi per eseguire il provisioning dell'applicazione ASP.NET. Vengono inoltre descritti gli approcci che consente di soddisfare questi requisiti.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager soluzione](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente uno istruzioni che si applicano a ogni ambiente di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifici dell'ambiente di compilazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Quali sono i problemi quando si distribuisce un Database di appartenenza?

Nella maggior parte dei casi, quando si escogitare una strategia di distribuzione per un database, la prima cosa che è necessario prendere in considerazione è i dati che si desidera distribuire. In un ambiente di sviluppo o test, si potrebbe essere necessario distribuire i dati dell'account utente per semplificare il test in modo rapido e semplice. In un ambiente di produzione o gestione temporanea, è molto improbabile che si desidera distribuire i dati dell'account utente.

Sfortunatamente, i database di appartenenza ASP.NET introducono alcune problematiche specifiche prendere tale decisione molto più complesse:

- Una distribuzione solo allo schema verrà lascia il database di appartenenza in uno stato non operativo. Perché il database delle appartenenze include alcuni dati di configurazione (nel **aspnet\_SchemaVersions** tabella) che richiede il database per il funzionamento. Di conseguenza, se si esegue una distribuzione solo allo schema del database di appartenenza per escludere i dati dell'account utente, è necessario eseguire uno script di post-distribuzione per aggiungere i dati di configurazione essenziali.
- A seconda della configurazione del database di appartenenza, il provider di appartenenze può utilizzare la chiave del computer per crittografare le password e di archiviarli nel database. In questo caso, i dati di account utente da distribuire con il database diventerà inutilizzabili nel server di destinazione. Per questo motivo, la distribuzione di dati dell'account utente non è uno scenario supportato.

## <a name="choosing-a-membership-database-strategy"></a>Scelta di una strategia di Database di appartenenza

Utilizzare queste linee guida quando si sceglie come eseguire il provisioning di un database di appartenenza in un ambiente server aziendale:

- Laddove possibile, non distribuire i database di appartenenza. In alternativa, creare manualmente i database delle appartenenze nel server di database di destinazione. Se lo schema del database di appartenenza non è stato personalizzato, è possibile semplicemente creare una nuova in situ di destinazione utilizzando il [strumento di registrazione di SQL Server ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Se non esiste nessuna opzione ma come distribuire un database di appartenenza&#x2014;, ad esempio, se sono state apportate modifiche estese allo schema del database&#x2014;è necessario eseguire una distribuzione solo allo schema del database delle appartenenze, per escludere i dati dell'account utente, quindi eseguire uno script di post-distribuzione per aggiungere tutti i dati di configurazione necessarie. È possibile trovare indicazioni generali su questi approcci in [procedura: distribuire ASP.NET l'appartenenza al Database senza tra account utente](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

È importante ricordare che *lo schema del database di appartenenza è probabile che sia abbastanza statico*. Anche se è stato personalizzato il database delle appartenenze, è improbabile che è necessario aggiornare lo schema a intervalli regolari&#x2014;non venga modificato con la stessa frequenza come il codice in un'applicazione web o un progetto di database. Non deve di conseguenza, è necessario includere il database delle appartenenze in tutti i processi di distribuzione automatica o passo-passo.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>L'utilizzo di VSDBCMD per aggiornare uno Schema di Database di appartenenza

Se si modifica la struttura del database di appartenenza dopo la distribuzione prima, si desidera utilizzare lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) per ridistribuire il database. La funzionalità di distribuzione di database di distribuzione Web non include la possibilità di rendere gli aggiornamenti differenziali in un database di destinazione&#x2014;, invece, distribuzione Web è necessario eliminare e ricreare il database. Ciò significa che si perdono i dati di account utente esistenti, che è in genere indesiderati in ambienti di produzione o gestione temporanea.

L'alternativa consiste nell'utilizzare l'utilità VSDBCMD per aggiornare lo schema del database di destinazione. VSDBCMD include due funzionalità importanti. In primo luogo, è possibile importare lo schema di un database esistente in un file. dbschema. In secondo luogo, è possibile distribuire un file con estensione dbschema a un database esistente come un aggiornamento differenziale, che significa che è solo le modifiche necessarie per portare il database di destinazione aggiornato e non perdere i dati.

È possibile utilizzare questi passaggi per aggiornare uno schema di database di appartenenza:

1. Utilizzare lo strumento VSDBCMD **importazione** azione per generare un file con estensione dbschema per il database di appartenenza di origine. Questa procedura è descritta [procedura: importare uno Schema da un prompt dei comandi](https://msdn.microsoft.com/library/dd172135.aspx).
2. Utilizzare lo strumento VSDBCMD **Distribuisci** azione per distribuire il file con estensione dbschema nel database di appartenenza di destinazione. Questa procedura è descritta [riferimento della riga di comando per VSDBCMD. EXE (distribuzione e importazione dello Schema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusione

In questo argomento descritte alcune delle problematiche potrebbero verificarsi quando è necessario eseguire il provisioning di database di appartenenza ASP.NET in vari ambienti di destinazione. In particolare, illustrato perché le distribuzioni di solo schema lascerà database delle appartenenze in uno stato non operativo e perché la distribuzione di dati dell'account utente non è supportata. L'articolo presenta anche informazioni aggiuntive su come eseguire il provisioning, distribuire e aggiornare i database di appartenenza in diversi scenari.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori istruzioni ed esempi di come utilizzare VSDBCMD, vedere [riferimento della riga di comando per VSDBCMD. EXE (distribuzione e importazione dello Schema)](https://msdn.microsoft.com/library/dd193283.aspx) e [procedura: importare uno Schema da un prompt dei comandi](https://msdn.microsoft.com/library/dd172135.aspx). Per ulteriori informazioni sull'utilizzo aspnet\_regsql.exe per creare il database di appartenenza, vedere [strumento di registrazione di SQL Server ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Per istruzioni generali sulla distribuzione di database di appartenenza, vedere [procedura: distribuire ASP.NET l'appartenenza al Database senza tra account utente](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Precedente](deploying-database-role-memberships-to-test-environments.md)
> [Successivo](excluding-files-and-folders-from-deployment.md)
