---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Appartenenze al ruolo del Database di distribuzione per gli ambienti di Test | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come aggiungere gli account utente ai ruoli del database come parte di una soluzione di distribuzione all'ambiente di test. Quando si distribuisce una soluzione che contiene...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 4f635153213b0695d7d4b64d09adefaf8ee8e892
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Appartenenze al ruolo del Database di distribuzione per gli ambienti di Test
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come aggiungere gli account utente ai ruoli del database come parte di una soluzione di distribuzione all'ambiente di test.
> 
> Quando si distribuisce una soluzione contenente un progetto di database in un ambiente di produzione o gestione temporanea, in genere non si desidera allo sviluppatore di automatizzare l'aggiunta di account utente ai ruoli del database. Nella maggior parte dei casi, lo sviluppatore non sarà possibile individuare gli account utente che devono essere aggiunti ai quali ruoli del database, e questi requisiti potrebbero cambiare in qualsiasi momento. Tuttavia, quando si distribuisce una soluzione contenente un progetto di database per lo sviluppo o ambiente di test, la situazione in genere è piuttosto diversa:
> 
> - Lo sviluppatore in genere nuovamente distribuisce la soluzione a intervalli regolari, spesso più volte al giorno.
> - Il database viene in genere ricreato in ogni distribuzione, il che significa che gli utenti del database devono essere creati e aggiunto ai ruoli dopo ogni distribuzione.
> - Lo sviluppatore in genere abbia il pieno controllo sull'ambiente di sviluppo o test di destinazione.
> 
> In questo scenario, è spesso utile per la creazione di utenti del database e assegnare le appartenenze ai ruoli di database come parte del processo di distribuzione.
> 
> Il fattore chiave è che questa operazione deve essere condizionale in base all'ambiente di destinazione. Se si distribuisce in di gestione temporanea o di un ambiente di produzione, si desidera ignorare l'operazione. Se si distribuisce a uno sviluppatore o l'ambiente di test, si desidera distribuire le appartenenze ai ruoli senza ulteriore intervento. In questo argomento viene descritto un approccio che è possibile utilizzare per risolvere questo problema.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager soluzione](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente uno istruzioni che si applicano a ogni ambiente di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifici dell'ambiente di compilazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

In questo argomento si presuppone che:

- Utilizzare l'approccio di file di progetto split alla distribuzione della soluzione, come descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Si chiama VSDBCMD dal file di progetto per distribuire il progetto di database, come descritto in [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Per creare utenti del database e assegnare le appartenenze ai ruoli quando si distribuisce un progetto di database in un ambiente di test, è necessario:

- Creare uno script Transact Structured Query Language (Transact-SQL) che apporta le modifiche del database necessari.
- Creare una destinazione di Microsoft Build Engine (MSBuild) che utilizza l'utilità sqlcmd.exe per eseguire lo script SQL.
- Configurare i file di progetto per richiamare la destinazione quando si distribuisce la soluzione in un ambiente di test.

Questo argomento viene illustrato come eseguire ognuna di queste procedure.

## <a name="scripting-the-database-role-memberships"></a>Scripting le appartenenze al ruolo di Database

È possibile creare uno script Transact-SQL in molti modi diversi, e in qualsiasi percorso prescelto. L'approccio più semplice consiste nel creare lo script all'interno della soluzione in Visual Studio 2010.

**Per creare uno script SQL**

1. Nel **Esplora** finestra, espandere il nodo del progetto di database.
2. Fare doppio clic sul **script** cartella, scegliere **Aggiungi**, quindi fare clic su **nuova cartella**.
3. Tipo **Test** come il nome della cartella e quindi premere INVIO.
4. Fare doppio clic su di **Test** cartella, scegliere **Aggiungi**, quindi fare clic su **Script**.
5. Nel **Aggiungi nuovo elemento** finestra di dialogo, assegnare un nome significativo di script (ad esempio, **AddRoleMemberships.sql**), quindi fare clic su **Aggiungi**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. Nel *AddRoleMemberships.sql* file, aggiungere le istruzioni Transact-SQL che:

    1. Creare un utente del database per l'account di accesso di SQL Server che avrà accesso al database.
    2. Aggiungere l'utente del database per i ruoli di database necessari.
7. Il file deve essere analogo al seguente:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Salvare il file.

## <a name="executing-the-script-on-the-target-database"></a>L'esecuzione dello Script nel Database di destinazione

Idealmente, è necessario eseguire qualsiasi script di Transact-SQL necessarie come parte di uno script di post-distribuzione quando si distribuisce il progetto di database. Tuttavia, gli script di post-distribuzione non consentono di eseguire la logica condizionale in base alle configurazioni di soluzione o le proprietà di compilazione. L'alternativa consiste nell'eseguire gli script SQL direttamente dal file di progetto MSBuild, creando un **destinazione** elemento che esegue un comando sqlcmd.exe. È possibile utilizzare questo comando per eseguire lo script nel database di destinazione:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Per ulteriori informazioni sulle opzioni della riga di comando sqlcmd, vedere [utilità sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Prima di incorporare questo comando in una destinazione di MSBuild, è necessario determinare in quali condizioni in cui l'esecuzione dello script:

- Il database di destinazione deve esistere prima di modificare l'appartenenza del ruolo. Di conseguenza, è necessario eseguire questo script *dopo* la distribuzione di database.
- È necessario includere una condizione in modo che lo script viene eseguito solo per ambienti di test.
- Se si esegue una distribuzione "cosa accade se"&#x2014;in altre parole, se si è la generazione di script di distribuzione ma non in esecuzione in&#x2014;è consigliabile non eseguire lo script SQL.

Se si utilizza l'approccio di file di progetto split descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), come illustrato dalla soluzione di esempio Contact Manager, è possibile dividere le istruzioni di compilazione per lo script SQL simile al seguente:

- Le necessarie proprietà specifiche dell'ambiente, con la proprietà che determina se distribuire le autorizzazioni, va nel file di progetto specifici dell'ambiente (ad esempio, *Env Dev.proj*).
- La destinazione di MSBuild, insieme a qualsiasi proprietà che non cambia tra ambienti di destinazione deve andare nel file di progetto universale (ad esempio, *Publish.proj*).

Nel file di progetto specifici dell'ambiente, è necessario definire il nome del server di database, il nome del database di destinazione e una proprietà booleana che consente di specificare se si desidera distribuire le appartenenze ai ruoli.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


Nel file di progetto universale, è necessario fornire il percorso dell'eseguibile sqlcmd e il percorso dello script SQL da eseguire. Queste proprietà rimane lo stesso indipendentemente dall'ambiente di destinazione. È inoltre necessario creare una destinazione di MSBuild per eseguire il comando sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Si noti che è aggiungere il percorso dell'eseguibile sqlcmd come proprietà statica, come può essere utile ad altre destinazioni. Al contrario, definire il percorso dello script SQL e la sintassi del comando sqlcmd come proprietà dinamiche all'interno della destinazione, come non saranno necessarie prima che venga eseguita la destinazione. In questo caso, il **DeployTestDBPermissions** destinazione verrà eseguita solo se vengono soddisfatte queste condizioni:

- Il **DeployTestDBRoleMemberships** è impostata su **true**.
- L'utente non è stato specificato un **WhatIf = true** flag.

Infine, non dimenticare di richiamare la destinazione. Nel *Publish.proj* file, è possibile farlo mediante l'aggiunta di destinazione per l'elenco di dipendenze per il valore predefinito **FullPublish** destinazione. È necessario assicurarsi che il **DeployTestDBPermissions** destinazione non viene eseguita finché il **PublishDbPackages** destinazione è stata eseguita.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto un modo in cui è possibile aggiungere gli utenti del database e le appartenenze ai ruoli come un'azione post-distribuzione quando si distribuisce un progetto di database. Si tratta in genere utile quando si regolarmente ricreare un database in un ambiente di test, ma devono essere evitato in genere quando si distribuiscono i database in ambienti di produzione o gestione temporanea. Di conseguenza, è necessario assicurarsi di usare la logica condizionale necessaria in modo che gli utenti del database e le appartenenze ai ruoli vengono creati solo quando è consigliabile eseguire questa operazione.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sull'utilizzo VSDBCMD per distribuire i progetti di database, vedere [progetti di Database di distribuzione](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per istruzioni sulla personalizzazione delle distribuzioni di database per gli ambienti di destinazione diverso, vedere [personalizzazione delle distribuzioni di Database per più ambienti](customizing-database-deployments-for-multiple-environments.md). Per ulteriori informazioni sull'utilizzo dei file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Per ulteriori informazioni sulle opzioni della riga di comando sqlcmd, vedere [utilità sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Precedente](customizing-database-deployments-for-multiple-environments.md)
> [Successivo](deploying-membership-databases-to-enterprise-environments.md)
