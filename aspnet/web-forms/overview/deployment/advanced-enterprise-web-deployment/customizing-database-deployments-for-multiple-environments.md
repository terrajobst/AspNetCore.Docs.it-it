---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Personalizzazione delle distribuzioni di Database per più ambienti | Microsoft Docs
author: jrjlee
description: "In questo argomento viene descritto come personalizzare le proprietà di un database negli ambienti di destinazione specifici come parte del processo di distribuzione. Nota: L'argomento si presuppone th..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 3a368e5055f4921b68f5c5eb2739728a2f1fd4d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378073"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Personalizzazione delle distribuzioni di Database per più ambienti
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come personalizzare le proprietà di un database negli ambienti di destinazione specifici come parte del processo di distribuzione.
> 
> > [!NOTE]
> > Nell'argomento si presuppone che si distribuisce un progetto di database di Visual Studio 2010 con MSBuild.exe e VSDBCMD.exe. Per altre informazioni sul motivo per cui è possibile scegliere questo approccio, vedere [distribuzione Web aziendale](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) e [distribuisce i progetti di Database](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Quando si distribuisce un progetto di database a più destinazioni, è spesso opportuno personalizzare le proprietà di distribuzione di database per ogni ambiente di destinazione. Ad esempio, in ambienti di test è consigliabile in genere ricreare il database in ogni distribuzione, mentre negli ambienti di gestione temporanea o produzione sarebbe stato molto più probabile che gli aggiornamenti incrementali per conservare i dati.
> 
> In un progetto di database di Visual Studio 2010, le impostazioni di distribuzione sono contenute all'interno di un file di configurazione (sqldeployment) di distribuzione. In questo argomento illustrerà come creare i file di configurazione di distribuzione specifici dell'ambiente e specificare quello da usare come parametro VSDBCMD.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

In questo argomento si presuppone che:

- Si usa l'approccio di file di progetto split distribuzione della soluzione, come descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Si chiama VSDBCMD dal file di progetto per distribuire il progetto di database, come descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Per creare un sistema di distribuzione che supporta variando le proprietà di distribuzione di database tra ambienti di destinazione, è necessario:

- Creare un file di configurazione (sqldeployment) di distribuzione per ogni ambiente di destinazione.
- Creare un comando VSDBCMD che specifica il file di configurazione di distribuzione come un'opzione della riga di comando.
- Impostare i parametri per il comando VSDBCMD in un file di progetto di Microsoft Build Engine (MSBuild), in modo che le opzioni VSDBCMD siano appropriate all'ambiente di destinazione.

In questo argomento illustrerà come eseguire ognuna di queste procedure.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Creazione di file di configurazione di distribuzione specifici dell'ambiente

Per impostazione predefinita, un progetto di database contiene un file di configurazione di distribuzione singola denominato *database. sqldeployment*. Se si apre questo file in Visual Studio 2010, è possibile vedere le opzioni di distribuzione diversi che sono disponibili all'utente:

- **Regole di confronto distribuzione**. Ciò consente di scegliere se usare le regole di confronto del database del progetto (il *origine* regole di confronto) o le regole di confronto del database del server di destinazione (il *destinazione* delle regole di confronto). Nella maggior parte dei casi, è opportuno usare le regole di confronto di origine quando si distribuisce a una soluzione di sviluppo o ambiente di test. Quando si distribuisce in un ambiente di gestione temporanea o produzione, in genere è opportuno lasciare le regole di confronto di destinazione non modificato per evitare eventuali problemi di interoperabilità.
- **Distribuisci proprietà database**. Ciò consente di scegliere se applicare le proprietà del database, come definito nel *sqlsettings* file. Quando si distribuisce un database per la prima volta, è necessario distribuire le proprietà del database. Se si aggiorna un database esistente, le proprietà devono già essere presente e non è necessario distribuirle di nuovo.
- **Ricrea sempre database**. Questo consente di scegliere se creare di nuovo il database di destinazione ogni volta che si distribuisce o si apportano modifiche incrementali per portare il database di destinazione aggiornato con lo schema. Se si crea nuovamente il database, si perderanno tutti i dati del database esistente. Di conseguenza, è consigliabile in genere impostata su **false** per le distribuzioni in ambienti di gestione temporanea o produzione.
- **Blocca distribuzione incrementale in caso di perdita di dati**. Ciò consente di scegliere se la distribuzione deve arrestarsi se una modifica allo schema del database comporta la perdita di dati. È in genere impostata su **true** per una distribuzione in un ambiente di produzione, per offrirti la possibilità di intervenire e proteggere i dati importanti. Se è stata impostata **Ricrea sempre database** al **false**, questa impostazione non avrà alcun effetto.
- **Eseguire la distribuzione in modalità utente singolo**. Non si tratta in genere un problema negli ambienti di sviluppo o test. Tuttavia, è necessario impostare in genere questo su **true** per le distribuzioni in ambienti di gestione temporanea o produzione. In questo modo si impedisce agli utenti di apportare modifiche al database mentre è in corso la distribuzione.
- **Eseguire il backup del database prima della distribuzione**. È in genere impostata su **true** quando si distribuisce un ambiente di produzione, prevenire la perdita di dati. È anche possibile impostarlo su **true** quando si distribuisce un ambiente di gestione temporanea, se il database di gestione temporanea contiene una grande quantità di dati.
- **Genera istruzioni DROP per gli oggetti presenti nel database di destinazione, ma non nel progetto di database**. Nella maggior parte dei casi, questa è una parte integrante ed essenziale di apportare modifiche incrementali a un database. Se è stata impostata **Ricrea sempre database** al **false**, questa impostazione non avrà alcun effetto.
- **Non utilizzare le istruzioni ALTER ASSEMBLY per aggiornare tipi CLR**. Questa impostazione determina come SQL Server aggiornerà i tipi common language runtime (CLR) per le versioni di assembly più recenti. Questo deve essere impostato su **false** nella maggior parte degli scenari.

Questa tabella mostra le impostazioni di distribuzione tipico per gli ambienti di destinazione diverso. Tuttavia, le impostazioni potrebbero essere diverse a seconda dei requisiti esatti.

|  | Sviluppo/Test | Integrazione di gestione temporanea / | Produzione |
| --- | --- | --- | --- |
| **Regole di confronto distribuzione** | Origine | destinazione | destinazione |
| **Distribuisci proprietà database** | True | Solo la prima volta | Solo la prima volta |
| **Ricrea sempre database** | True | False | False |
| **Blocca distribuzione incrementale in caso di perdita di dati** | False | Forse | True |
| **Eseguire lo script di distribuzione in modalità utente singolo** | False | True | True |
| **Eseguire il backup del database prima della distribuzione** | False | Forse | True |
| **Genera istruzioni DROP per gli oggetti nel database di destinazione, ma non nel progetto di database** | False | True | True |
| **Non utilizzare le istruzioni ALTER ASSEMBLY per aggiornare tipi CLR** | False | False | False |
  

> [!NOTE]
> Per altre informazioni sulle proprietà di distribuzione di database e considerazioni relative all'ambiente, vedere [An Overview of Database progetto Settings](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [procedura: configurare le proprietà per i dettagli della distribuzione](https://msdn.microsoft.com/library/dd172125.aspx), [ Compilare e distribuire Database in un ambiente di sviluppo isolato](https://msdn.microsoft.com/library/dd193409.aspx), e [compilare e distribuire i database in un ambiente di produzione o Staging](https://msdn.microsoft.com/library/dd193413.aspx).


Per supportare la distribuzione di un progetto di database a più destinazioni, è necessario creare un file di configurazione di distribuzione per ogni ambiente di destinazione.

**Per creare un file di configurazione specifici dell'ambiente**

1. In Visual Studio 2010, nelle **Esplora soluzioni** finestra, fare clic sul progetto di database e quindi fare clic su **proprietà**.
2. Nella pagina delle proprietà del progetto di database, nel **Deploy** nella scheda il **file di configurazione di distribuzione** riga, fare clic su **New**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. Nel **nuovo File di configurazione di distribuzione** finestra di dialogo casella, assegnare al file un nome significativo (ad esempio, **TestEnvironment.sqldeployment**) e quindi fare clic su **Salva**.
4. Nel *[nomefile] * * *. sqldeployment** pagina, impostare le proprietà di distribuzione per corrispondono ai requisiti dell'ambiente di destinazione e quindi salvare il file.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Si noti che il nuovo file viene aggiunto alla cartella Properties del progetto di database.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Specifica il File di configurazione di distribuzione in VSDBCMD

Quando si usano configurazioni di soluzione (ad esempio Debug e rilascio) all'interno di Visual Studio 2010, è possibile associare ogni configurazione di un file di configurazione di distribuzione. Quando si compila una configurazione particolare, il processo di compilazione genera un file manifesto di distribuzione specifici della configurazione che punta al file di configurazione di distribuzione specifici della configurazione. Tuttavia, uno dei principali obiettivi dell'approccio alla distribuzione descritta in queste esercitazioni è per assegnare agli utenti la possibilità di controllare il processo di distribuzione senza usare Visual Studio 2010 e le configurazioni di soluzione. In questo approccio, la configurazione della soluzione è lo stesso indipendentemente dall'ambiente di distribuzione di destinazione. Per adattare la distribuzione di database in un ambiente di destinazione specifico, è possibile usare le opzioni della riga di comando VSDBCMD per specificare il file di configurazione di distribuzione.

Per specificare un file di configurazione di distribuzione di VSDBCMD, usare il **p: / DeploymentConfigurationFile** passare e specificare il percorso completo del file. Questa impostazione sostituirà il file di configurazione di distribuzione che identifica il manifesto di distribuzione. Ad esempio, è possibile utilizzare questo comando VSDBCMD per distribuire il **ContactManager** database in un ambiente di test:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Si noti che il processo di compilazione può rinominare il file. sqldeployment quando copia il file nella directory di output.


Se si usano variabili di comandi SQL negli script SQL pre-distribuzione o post-distribuzione, è possibile usare un approccio simile per associare un file sqlcmdvars specifici dell'ambiente con la distribuzione. In questo caso, si utilizza il **p: / SqlCommandVariablesFile** switch per identificare il file sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Esecuzione del comando VSDBCMD da un File di progetto MSBuild

È possibile richiamare un comando VSDBCMD da un file di progetto MSBuild tramite un **Exec** attività all'interno di una destinazione di MSBuild. Nella sua forma più semplice, lo si presenta come segue:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- In pratica, per rendere i file di progetto facili da leggere e riutilizzare, è opportuno creare proprietà per archiviare i parametri della riga di comando diverse. Questo rende più semplice per gli utenti per fornire i valori delle proprietà in un file di progetto specifici dell'ambiente o eseguire l'override di valori predefiniti dalla riga di comando di MSBuild. Se si usa l'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), è necessario dividere conseguenza le istruzioni di compilazione e le proprietà tra i due file:
- Impostazioni specifiche dell'ambiente, ad esempio il nome di file di configurazione di distribuzione, la stringa di connessione di database e il nome di database di destinazione, devono passare nel file di progetto specifici dell'ambiente.
- La destinazione di MSBuild che esegue il comando VSDBCMD, insieme a qualsiasi proprietà universale, ad esempio il percorso dell'eseguibile VSDBCMD, deve passare nel file di progetto universale.

È inoltre necessario assicurarsi che si compila il progetto di database prima di richiamare VSDBCMD in modo che il file con estensione deploymanifest viene creato e pronto per l'uso. È possibile vedere un esempio completo di questo approccio nell'argomento [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), che illustra i file di progetto nel [soluzione di esempio Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come è possibile personalizzare le proprietà del database per gli ambienti di destinazione diverso quando si distribuiscono i progetti di database utilizzando MSBuild e VSDBCMD. Questo approccio è utile quando è necessario distribuire i progetti di database come parte del più grande, soluzioni su larga scala. Queste soluzioni spesso sono distribuite in più destinazioni, ad esempio gli ambienti di sviluppo o test in modalità sandbox, le piattaforme di gestione temporanea o di integrazione e produzione o in ambienti in tempo reale. Ognuno di questi ambienti di destinazione richiede in genere un set di proprietà di distribuzione di database univoco.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sulla distribuzione di progetti di database usando VSDBCMD.exe, vedere [distribuire progetti di Database](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per altre informazioni sull'utilizzo dei file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Questi articoli su MSDN forniscono istruzioni generali sulla distribuzione del database:

- [Una panoramica delle impostazioni di progetto di Database](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Procedura: configurare le proprietà per i dettagli della distribuzione](https://msdn.microsoft.com/library/dd172125.aspx)
- [Compilare e distribuire i database in un ambiente di sviluppo isolato](https://msdn.microsoft.com/library/dd193409.aspx)
- [Compilare e distribuire i database in un ambiente di produzione o gestione temporanea](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Precedente](performing-a-what-if-deployment.md)
> [Successivo](deploying-database-role-memberships-to-test-environments.md)
