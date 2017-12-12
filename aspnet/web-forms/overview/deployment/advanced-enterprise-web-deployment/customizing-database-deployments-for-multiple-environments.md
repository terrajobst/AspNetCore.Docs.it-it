---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: "Personalizzazione delle distribuzioni di Database per più ambienti | Documenti Microsoft"
author: jrjlee
description: "In questo argomento viene descritto come personalizzare le proprietà di un database in ambienti di destinazione specifico come parte del processo di distribuzione. Nota: L'argomento si presuppone th..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 449c448d1be237f3f95a437bb2c0415bd8ed0d99
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Personalizzazione delle distribuzioni di Database per più ambienti
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come personalizzare le proprietà di un database in ambienti di destinazione specifico come parte del processo di distribuzione.
> 
> > [!NOTE]
> > Nell'argomento si presuppone che si distribuisce un progetto di database di Visual Studio 2010 con MSBuild.exe e VSDBCMD.exe. Per ulteriori informazioni sui motivi per cui è possibile scegliere questo approccio, vedere [distribuzione Web dell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) e [progetti di Database di distribuzione](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Quando si distribuisce un progetto di database a più destinazioni, è spesso opportuno personalizzare le proprietà di distribuzione di database per ogni ambiente di destinazione. Ad esempio, negli ambienti di testing è consigliabile in genere ricreare il database in ogni distribuzione, mentre in ambienti di gestione temporanea o produzione sarebbe molto più probabile che gli aggiornamenti incrementali per conservare i dati.
> 
> In un progetto di database di Visual Studio 2010, le impostazioni di distribuzione sono contenute all'interno di un file di configurazione (con estensione sqldeployment) di distribuzione. Questo argomento viene illustrato come creare i file di configurazione di distribuzione specifico dell'ambiente e specificare quello che si desidera utilizzare come parametro VSDBCMD.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due progetti #x 2014; & file contenente una compilare le istruzioni che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="task-overview"></a>Panoramica di Task

In questo argomento si presuppone che:

- Utilizzare l'approccio di file di progetto split alla distribuzione della soluzione, come descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Si chiama VSDBCMD dal file di progetto per distribuire il progetto di database, come descritto in [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Per creare un sistema di distribuzione che supporta le proprietà di database di distribuzione tra ambienti di destinazione diversi, è necessario:

- Creare un file di configurazione (con estensione sqldeployment) di distribuzione per ogni ambiente di destinazione.
- Creare un comando VSDBCMD che specifica il file di configurazione di distribuzione come un'opzione della riga di comando.
- Parametrizzare il comando VSDBCMD in un file di progetto di Microsoft Build Engine (MSBuild), in modo che le opzioni VSDBCMD sono appropriate per l'ambiente di destinazione.

Questo argomento viene illustrato come eseguire ognuna di queste procedure.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Creazione di file di configurazione di distribuzione specifico dell'ambiente

Per impostazione predefinita, un progetto di database contiene un file di configurazione di distribuzione singolo denominato *database. sqldeployment*. Se si apre il file in Visual Studio 2010, è possibile visualizzare le opzioni di distribuzione diversi che sono disponibili:

- **Regole di confronto distribuzione**. Consente di scegliere se utilizzare le regole di confronto del database del progetto (la *origine* delle regole di confronto) o le regole di confronto del database del server di destinazione (il *destinazione* regole di confronto). Nella maggior parte dei casi, si desidera utilizzare le regole di confronto di origine quando si distribuisce un sviluppo o l'ambiente di test. Quando si distribuisce in un ambiente di produzione o gestione temporanea, in genere è opportuno lasciare le regole di confronto di destinazione invariata per evitare eventuali problemi di interoperabilità.
- **Distribuisci proprietà database**. Ciò consente di scegliere se applicare le proprietà del database, come definito nel *sqlsettings* file. Quando si distribuisce un database per la prima volta, è necessario distribuire le proprietà del database. Se si sta aggiornando un database esistente, le proprietà devono essere già presente e non è necessario distribuirli nuovamente.
- **Ricrea sempre database**. Questo consente di scegliere se creare nuovamente il database di destinazione ogni volta che si distribuisce o apportare modifiche incrementali per portare il database di destinazione aggiornato con lo schema. Se si crea nuovamente il database, si perderanno tutti i dati nel database esistente. Di conseguenza, è necessario impostare in genere questo su **false** per le distribuzioni in ambienti di produzione o gestione temporanea.
- **Blocca distribuzione incrementale in caso di perdita di dati**. Ciò consente di scegliere se la distribuzione deve arrestarsi se una modifica allo schema del database comporta la perdita di dati. È in genere impostata su **true** per una distribuzione in un ambiente di produzione, per cui è possibile al fine di proteggere i dati importanti. Se è stata impostata **Ricrea sempre database** a **false**, questa impostazione non avrà alcun effetto.
- **Eseguire la distribuzione in modalità utente singolo**. Non si tratta in genere un problema in ambienti di sviluppo o test. Tuttavia, è necessario impostare in genere questo su **true** per le distribuzioni in ambienti di produzione o gestione temporanea. In questo modo si impedisce agli utenti di apportare modifiche al database durante la distribuzione.
- **Eseguire il backup del database prima della distribuzione**. È in genere impostata su **true** quando si distribuisce un ambiente di produzione, per evitare la perdita di dati. È anche possibile impostarlo su **true** quando si distribuisce un ambiente di gestione temporanea, se il database di gestione temporanea contiene una grande quantità di dati.
- **Genera istruzioni DROP per oggetti contenuti nel database di destinazione, ma non nel progetto di database**. Nella maggior parte dei casi, questo è parte integrante ed essenziale di apportare modifiche incrementali a un database. Se è stata impostata **Ricrea sempre database** a **false**, questa impostazione non avrà alcun effetto.
- **Non utilizzare le istruzioni ALTER ASSEMBLY per aggiornare tipi CLR**. Questa impostazione determina come SQL Server deve aggiornare i tipi common language runtime (CLR) per le versioni di assembly più recenti. Questo deve essere impostato su **false** nella maggior parte degli scenari.

Questa tabella mostra le impostazioni di distribuzione tipica per ambienti di destinazione diversi. Tuttavia, le impostazioni potrebbero essere diverse a seconda dei requisiti esatti.

|  | Sviluppo/Test | Integrazione di gestione temporanea / | Produzione |
| --- | --- | --- | --- |
| **Regole di confronto di distribuzione** | Origine | destinazione | destinazione |
| **Distribuisci proprietà database** | True | Solo la prima volta | Solo la prima volta |
| **Ricrea sempre database** | True | False | False |
| **Blocca distribuzione incrementale in caso di perdita di dati** | False | Forse | True |
| **Esegui script di distribuzione in modalità utente singolo** | False | True | True |
| **Eseguire il backup del database prima della distribuzione** | False | Forse | True |
| **Genera istruzioni DROP per oggetti contenuti nel database di destinazione, ma non nel progetto di database** | False | True | True |
| **Non utilizzare le istruzioni ALTER ASSEMBLY per aggiornare tipi CLR** | False | False | False |
  

> [!NOTE]
> Per ulteriori informazioni sulle proprietà di distribuzione di database e considerazioni relative all'ambiente, vedere [una panoramica di Database impostazioni progetto](https://msdn.microsoft.com/en-us/library/aa833291(v=VS.100).aspx), [procedura: configurare le proprietà per informazioni dettagliate sulla distribuzione](https://msdn.microsoft.com/en-us/library/dd172125.aspx), [ Compilare e distribuire un Database in un ambiente di sviluppo isolato](https://msdn.microsoft.com/en-us/library/dd193409.aspx), e [di compilazione e distribuzione di database in un ambiente di produzione o gestione temporanea](https://msdn.microsoft.com/en-us/library/dd193413.aspx).


Per supportare la distribuzione di un progetto di database a più destinazioni, è necessario creare un file di configurazione di distribuzione per ogni ambiente di destinazione.

**Per creare un file di configurazione specifici dell'ambiente**

1. In Visual Studio 2010, nel **Esplora** finestra, fare doppio clic su progetto di database e quindi fare clic su **proprietà**.
2. Nella pagina delle proprietà del progetto di database, nel **Distribuisci** nella scheda il **file di configurazione di distribuzione** di riga, fare clic su **New**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. Nel **nuovo File di configurazione di distribuzione** finestra di dialogo casella, assegnare al file un nome significativo (ad esempio, **TestEnvironment.sqldeployment**), quindi fare clic su **salvare**.
4. Nel *[nomefile]***sqldeployment** pagina, impostare le proprietà di distribuzione per corrispondano ai requisiti dell'ambiente di destinazione e quindi salvare il file.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Si noti che il nuovo file viene aggiunto alla cartella Properties del progetto di database.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Specifica il File di configurazione di distribuzione in VSDBCMD

Quando si usano le configurazioni di soluzione (ad esempio, Debug e rilascio) all'interno di Visual Studio 2010, è possibile associare ogni configurazione di un file di configurazione di distribuzione. Quando si compila una particolare configurazione, il processo di compilazione genera un file manifesto di distribuzione specifiche della configurazione che punta al file di configurazione di distribuzione specifiche della configurazione. Tuttavia, uno dei principali obiettivi dell'approccio alla distribuzione descritta in queste esercitazioni è per assegnare agli utenti la possibilità di controllare il processo di distribuzione senza utilizzare Visual Studio 2010 e le configurazioni di soluzione. In questo approccio, la configurazione della soluzione è lo stesso indipendentemente dall'ambiente di distribuzione di destinazione. Per personalizzare la distribuzione di database in un ambiente di destinazione specifico, è possibile utilizzare le opzioni della riga di comando VSDBCMD per specificare il file di configurazione di distribuzione.

Per specificare un file di configurazione di distribuzione del VSDBCMD, utilizzare il **p: / DeploymentConfigurationFile** passa e specificare il percorso completo al file. Questo percorso sostituirà il file di configurazione di distribuzione che identifica il manifesto di distribuzione. Ad esempio, è possibile usare questo comando VSDBCMD per distribuire il **ContactManager** database all'ambiente di test:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Si noti che il processo di compilazione può rinominare il file di sqldeployment quando il file venga copiato nella directory di output.


Se si utilizzano variabili di comando SQL negli script SQL pre-distribuzione o post-distribuzione, è possibile utilizzare un approccio simile per associare un file sqlcmdvars specifici dell'ambiente con la distribuzione. In questo caso, utilizzare il **p: / SqlCommandVariablesFile** per identificare il file sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Esecuzione del comando VSDBCMD da un File di progetto MSBuild

È possibile richiamare un comando VSDBCMD da un file di progetto MSBuild utilizzando un **Exec** attività all'interno di una destinazione di MSBuild. Nella sua forma più semplice, avrà un aspetto simile al seguente:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- In pratica, per rendere i file di progetto facili da leggere e riutilizzare, è opportuno creare proprietà per memorizzare i diversi parametri della riga di comando. Questo rende più semplice per gli utenti per fornire i valori delle proprietà in un file di progetto specifici dell'ambiente o per eseguire l'override di valori predefiniti dalla riga di comando di MSBuild. Se si utilizza l'approccio di file di progetto split descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), suddividere le istruzioni di compilazione e proprietà tra i due file di conseguenza:
- Le impostazioni specifiche dell'ambiente, come il nome di file di configurazione di distribuzione, la stringa di connessione di database e il nome di database di destinazione, è necessario usare nel file di progetto specifici dell'ambiente.
- La destinazione di MSBuild che esegue il comando VSDBCMD, insieme a qualsiasi proprietà universale come il percorso dell'eseguibile, VSDBCMD deve andare nel file di progetto universale.

È inoltre necessario assicurarsi che si compila il progetto di database prima di richiamare VSDBCMD in modo che il file con estensione deploymanifest viene creato e pronto per l'uso. È possibile visualizzare un esempio completo di questo approccio nell'argomento [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), che illustra i file di progetto nel [soluzione di esempio Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come è possibile personalizzare le proprietà del database per ambienti di destinazione diversi quando si distribuiscono progetti di database utilizzando MSBuild e VSDBCMD. Questo approccio è utile quando è necessario distribuire i progetti di database come parte di dimensioni maggiori, soluzioni su larga scala. Queste soluzioni sono spesso distribuite in più destinazioni, ad esempio gli ambienti di sviluppo o test in modalità sandbox, piattaforme di gestione temporanea o di integrazione e produzione o ambienti in tempo reale. Ciascuno di questi ambienti di destinazione richiede in genere un set di proprietà di distribuzione di database univoco.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla distribuzione di progetti di database utilizzando VSDBCMD.exe, vedere [progetti di Database di distribuzione](../web-deployment-in-the-enterprise/deploying-database-projects.md). Per ulteriori informazioni sull'utilizzo dei file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Questi articoli su MSDN forniscono istruzioni generali sulla distribuzione di database:

- [Una panoramica delle impostazioni di progetto di Database](https://msdn.microsoft.com/en-us/library/aa833291(v=VS.100).aspx)
- [Procedura: configurare le proprietà per i dettagli di distribuzione](https://msdn.microsoft.com/en-us/library/dd172125.aspx)
- [Compilazione e distribuzione di database in un ambiente di sviluppo isolato](https://msdn.microsoft.com/en-us/library/dd193409.aspx)
- [Compilazione e distribuzione di database in un ambiente di produzione o gestione temporanea](https://msdn.microsoft.com/en-us/library/dd193413.aspx)

>[!div class="step-by-step"]
[Precedente](performing-a-what-if-deployment.md)
[Successivo](deploying-database-role-memberships-to-test-environments.md)
