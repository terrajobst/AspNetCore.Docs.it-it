---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Distribuire le appartenenze ai ruoli di Database in ambienti di Test | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come aggiungere gli account utente ai ruoli del database come parte di una distribuzione della soluzione in un ambiente di test. Quando si distribuisce una soluzione che contiene...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 42ceaa654c7f690f8455c0569e347cd5e9fccf7a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401860"
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Distribuire le appartenenze ai ruoli di Database in ambienti di Test
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come aggiungere gli account utente ai ruoli del database come parte di una distribuzione della soluzione in un ambiente di test.
> 
> Quando si distribuisce una soluzione contenente un progetto di database in un ambiente di gestione temporanea o produzione, in genere non si desidera agli sviluppatori di automatizzare l'aggiunta di account utente ai ruoli del database. Nella maggior parte dei casi, lo sviluppatore non sa quali account utente devono essere aggiunti a quali ruoli del database e tali requisiti potrebbe cambiare in qualsiasi momento. Tuttavia, quando si distribuisce una soluzione contenente un progetto di database per una soluzione di sviluppo o ambiente di test, la situazione in genere è piuttosto diversa:
> 
> - Lo sviluppatore in genere nuovamente distribuisce la soluzione a intervalli regolari, spesso più volte al giorno.
> - Il database viene in genere ricreato in ogni distribuzione, il che significa che gli utenti del database devono essere creati e aggiunto ai ruoli dopo ogni distribuzione.
> - Lo sviluppatore ha in genere controllo completo sull'ambiente di sviluppo o test di destinazione.
> 
> In questo scenario, è spesso utile creare gli utenti del database e assegnare le appartenenze ai ruoli di database come parte del processo di distribuzione automaticamente.
> 
> Il fattore chiave è che questa operazione deve essere condizionale in base all'ambiente di destinazione. Se si esegue la distribuzione di gestione temporanea o di un ambiente di produzione, si desidera ignorare l'operazione. Se si esegue la distribuzione a uno sviluppatore o ambiente di test, si vuole distribuire le appartenenze ai ruoli senza ulteriore intervento. In questo argomento viene descritto un approccio che è possibile usare per affrontare questa sfida.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

In questo argomento si presuppone che:

- Si usa l'approccio di file di progetto split distribuzione della soluzione, come descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Si chiama VSDBCMD dal file di progetto per distribuire il progetto di database, come descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Per creare utenti del database e assegnare le appartenenze ai ruoli quando si distribuisce un progetto di database in un ambiente di test, è necessario:

- Creare uno script Transact Structured Query Language (Transact-SQL) che apporta le modifiche di database necessari.
- Creare una destinazione di Microsoft Build Engine (MSBuild) che usa l'utilità sqlcmd.exe per eseguire lo script SQL.
- Configurare i file di progetto per richiamare la destinazione quando si distribuisce la soluzione in un ambiente di test.

In questo argomento illustrerà come eseguire ognuna di queste procedure.

## <a name="scripting-the-database-role-memberships"></a>Scripting le appartenenze al ruolo di Database

È possibile creare uno script Transact-SQL in molti modi diversi e in qualsiasi percorso scelto. L'approccio più semplice consiste nel creare lo script all'interno della soluzione in Visual Studio 2010.

**Per creare uno script SQL**

1. Nel **Esplora soluzioni** finestra, espandere il nodo del progetto di database.
2. Fare doppio clic sui **gli script** cartella, scegliere **Add**, quindi fare clic su **nuova cartella**.
3. Tipo di **Test** come il nome della cartella e quindi premere INVIO.
4. Fare doppio clic sui **Test** cartella, scegliere **Add**, quindi fare clic su **Script**.
5. Nel **Aggiungi nuovo elemento** finestra di dialogo casella, assegnare un nome significativo lo script (ad esempio **AddRoleMemberships.sql**) e quindi fare clic su **Aggiungi**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. Nel *AddRoleMemberships.sql* , aggiungere le istruzioni Transact-SQL che:

    1. Creare un utente di database per l'account di accesso di SQL Server che avrà accesso al database.
    2. Aggiungere l'utente del database per tutti i ruoli di database necessari.
7. Il file sarà analogo al seguente:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Salvare il file.

## <a name="executing-the-script-on-the-target-database"></a>L'esecuzione dello Script nel Database di destinazione

In teoria, si sarebbero eseguire gli script di Transact-SQL necessari come parte di uno script di post-distribuzione quando si distribuisce il progetto di database. Tuttavia, gli script di post-distribuzione non consentono di eseguire la logica condizionale in base a configurazioni di soluzione o le proprietà di compilazione. L'alternativa consiste nell'eseguire gli script SQL direttamente dal file di progetto MSBuild, creando un **destinazione** elemento che esegue un comando sqlcmd.exe. È possibile usare questo comando per eseguire lo script nel database di destinazione:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Per altre informazioni sulle opzioni della riga di comando sqlcmd, vedere [utilità sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Prima di incorporare questo comando in una destinazione di MSBuild, è necessario prendere in considerazione in quali condizioni si desidera che l'esecuzione dello script:

- Il database di destinazione deve esistere prima di modificare le appartenenze ai ruoli. Di conseguenza, è necessario eseguire questo script *dopo* la distribuzione del database.
- È necessario includere una condizione in modo che lo script viene eseguito solo per gli ambienti di test.
- Se si esegue una distribuzione di "what if"&#x2014;in altre parole, se si esegue la generazione di script di distribuzione ma non in esecuzione in&#x2014;è consigliabile non eseguire lo script SQL.

Se si usa l'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), come illustrato dalla soluzione di esempio Contact Manager, è possibile suddividere le istruzioni di compilazione per lo script SQL come segue:

- Le richieste di proprietà specifiche dell'ambiente, insieme alle proprietà che determina se si desidera distribuire le autorizzazioni, devono essere inseriti in file di progetto specifici dell'ambiente (ad esempio, *Env-Dev.proj*).
- Nel file di progetto universale deve diventare la destinazione MSBuild, insieme a qualsiasi proprietà che non cambia tra ambienti di destinazione (ad esempio, *Publish.proj*).

Nel file di progetto specifici dell'ambiente, è necessario definire il nome del server di database, il nome di database di destinazione e una proprietà booleana che consente all'utente di specificare se si desidera distribuire le appartenenze ai ruoli.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


Nel file di progetto universale, è necessario fornire il percorso dell'eseguibile sqlcmd e il percorso dello script SQL da eseguire. Queste proprietà subiranno modifiche indipendentemente dall'ambiente di destinazione. È anche necessario creare una destinazione di MSBuild per eseguire il comando sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Si noti che è aggiungere il percorso dell'eseguibile sqlcmd come proprietà statica, come può essere utile ad altre destinazioni. Al contrario, si definiscono il percorso dello script SQL e la sintassi del comando sqlcmd come proprietà dinamica all'interno della destinazione, come non saranno necessarie prima che venga eseguita la destinazione. In questo caso, il **DeployTestDBPermissions** destinazione verrà eseguita solo se vengono soddisfatte queste condizioni:

- Il **DeployTestDBRoleMemberships** è impostata su **true**.
- L'utente non ha specificato un **WhatIf = true** flag.

Infine non dimenticare di richiamare la destinazione. Nel *Publish.proj* file, è possibile farlo mediante l'aggiunta di destinazione per l'elenco di dipendenze per il valore predefinito **FullPublish** destinazione. È necessario assicurarsi che il **DeployTestDBPermissions** destinazione non viene eseguita finché il **PublishDbPackages** destinazione è stata eseguita.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto un modo in cui è possibile aggiungere gli utenti del database e le appartenenze ai ruoli come un'azione post-distribuzione quando si distribuisce un progetto di database. Si tratta in genere utile quando è regolarmente ricreare un database in un ambiente di test, ma deve essere evitata in genere quando si distribuiscono i database in ambienti di gestione temporanea o produzione. Di conseguenza, è necessario assicurarsi di usare la logica condizionale necessaria in modo che gli utenti del database e le appartenenze ai ruoli vengono creati solo quando è appropriato eseguire questa operazione.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sull'utilizzo VSDBCMD per distribuire i progetti di database, vedere [distribuire progetti di Database](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per linee guida sulla personalizzazione delle distribuzioni di database per diversi ambienti di destinazione, vedere [personalizzazione le distribuzioni dei Database per più ambienti](customizing-database-deployments-for-multiple-environments.md). Per altre informazioni sull'utilizzo dei file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per altre informazioni sulle opzioni della riga di comando sqlcmd, vedere [utilità sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Precedente](customizing-database-deployments-for-multiple-environments.md)
> [Successivo](deploying-membership-databases-to-enterprise-environments.md)
