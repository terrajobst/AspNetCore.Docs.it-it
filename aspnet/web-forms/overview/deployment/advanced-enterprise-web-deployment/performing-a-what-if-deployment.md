---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Esecuzione di una cosa se la distribuzione | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come eseguire 'se' (o simulati) le distribuzioni usando lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) e V...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 054b15e1e58164ec7e85c77c39fb47bcb47879b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827281"
---
<a name="performing-a-what-if-deployment"></a>Esecuzione di una distribuzione "What If"
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come eseguire "what if" (o simulati) le distribuzioni usando lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) e VSDBCMD. Ciò consente di determinare gli effetti della logica di distribuzione in un ambiente di destinazione specifico prima di distribuire effettivamente l'applicazione.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è controllato da due file di progetto&#x2014;uno contenente le istruzioni di compilazione che si applicano a tutti gli ambienti di destinazione e quella che contiene impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Esecuzione di una distribuzione "What If" per i pacchetti Web

La funzionalità distribuzione Web sono incluse funzionalità che consente di eseguire distribuzioni in "what if" (o versione di valutazione) modalità. Quando si distribuiscono gli artefatti in modalità di "what if", distribuzione Web genera un file di log come se si ha eseguito la distribuzione, ma non viene effettivamente modificato nulla nel server di destinazione. Esaminare il file di log possono aiutare a comprendere l'impatto che la distribuzione sarà necessario nel server di destinazione, in particolare:

- Ciò che viene aggiunto.
- Che cosa verrà aggiornata.
- Che cosa verrà eliminata.

Poiché una distribuzione di "what if" in realtà non apporta modifiche nel server di destinazione, cosa non è sempre possibile stimare se una distribuzione avrà esito positivo.

Come descritto in [distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), è possibile distribuire pacchetti web tramite distribuzione Web in due modi&#x2014;con l'utilità della riga di comando di MSDeploy.exe direttamente o tramite l'esecuzione di *. deploy. cmd* file che genera il processo di compilazione.

Se si usa MSDeploy.exe direttamente, è possibile eseguire una distribuzione di "what if" aggiungendo il **– whatif** flag al comando. Ad esempio, per valutare cosa accadrebbe se il pacchetto ContactManager.Mvc.zip è stato distribuito in un ambiente di staging, il comando di MSDeploy sarà analogo al seguente:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Quando si è soddisfatti dei risultati della distribuzione di "what if", è possibile rimuovere il **– whatif** flag per eseguire una distribuzione in tempo reale.

> [!NOTE]
> Per altre informazioni sulle opzioni della riga di comando per MSDeploy.exe, vedere [Web Deploy operazione Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Se si usa la *. deploy. cmd* file, è possibile eseguire una distribuzione di "what if", includendo il **/t** flag flag (modalità valutazione) anziché il **/y** flag ("yes" o la modalità di aggiornamento) in il comando. Ad esempio, per valutare cosa accadrebbe se è stato distribuito il pacchetto ContactManager.Mvc.zip eseguendo il *. deploy. cmd* file, il comando sarà analogo al seguente:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Quando si è soddisfatti dei risultati della distribuzione di "modalità di valutazione", è possibile sostituire il **/t** contrassegnare con un **/y** flag per eseguire una distribuzione in tempo reale:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Per altre informazioni sulle opzioni della riga di comando per la *. deploy. cmd* i file, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Se si esegue la *. deploy. cmd* file senza specificare alcun flag, il prompt dei comandi verrà visualizzato un elenco dei flag.


## <a name="performing-a-what-if-deployment-for-databases"></a>Esecuzione di una distribuzione "What If" per i database

In questa sezione presuppone che si sta usando l'utilità VSDBCMD per eseguire la distribuzione incrementale, basata sullo schema del database. Questo approccio è descritto più dettagliatamente [distribuire progetti di Database](../web-deployment-in-the-enterprise/deploying-database-projects.md). È consigliabile acquisire familiarità con questo argomento prima di applicare i concetti illustrati di seguito.

Quando si usa VSDBCMD nelle **Deploy** modalità, è possibile utilizzare il **/dd** (o **/DeployToDatabase**) flag per controllare se VSDBCMD distribuisce effettivamente il database o parte genera semplicemente un script di distribuzione. Se si distribuisce un file. dbschema, si tratta del comportamento:

- Se si specifica **/dd+** oppure **/dd**, VSDBCMD genererà uno script di distribuzione e distribuire il database.
- Se si specifica **/dd-** o omettere il commutatore, VSDBCMD genererà solo uno script di distribuzione.

> [!NOTE]
> Se si distribuisce un file DeployManifest anziché un file. dbschema, il comportamento dei **/dd** commutatore è molto più complicato. In pratica, VSDBCMD ignorerà il valore della **/dd** passare, se il file con estensione deploymanifest include una **DeployToDatabase** elemento con il valore **True**. [Distribuzione di progetti di Database](../web-deployment-in-the-enterprise/deploying-database-projects.md) descrive questo comportamento in modo completo.


Ad esempio, per generare uno script di distribuzione per il **ContactManager** database senza distribuire effettivamente il database, il comando VSDBCMD sarà analogo al seguente:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD è uno strumento di distribuzione di backup differenziali del database e di conseguenza lo script di distribuzione viene generato dinamicamente per contenere tutti i comandi SQL necessari per aggiornare il database corrente, se presente, lo schema specificato. Esaminare lo script di distribuzione è un modo utile per determinare l'impatto della distribuzione sarà necessario nel database corrente e i dati in che esso contenuti. Ad esempio, voler determinare:

- Indica se verranno rimosso alcun tabelle esistenti e indica se che causano la perdita di dati.
- Se l'ordine delle operazioni rappresenta un rischio di perdita di dati, ad esempio, se si ha la suddivisione o unione di tabelle.

Se si è soddisfatti con lo script di distribuzione, è possibile ripetere il VSDBCMD con un **/dd+** flag per apportare le modifiche. In alternativa, è possibile modificare lo script di distribuzione per soddisfare i requisiti e quindi eseguirlo manualmente nel server di database.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrazione delle funzionalità di "What If" in file di progetto personalizzati

Negli scenari di distribuzione più complessi, è opportuno usare un file di progetto personalizzato di Microsoft Build Engine (MSBuild) per incapsulare la logica di compilazione e distribuzione, come descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Ad esempio, nelle [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) esempio soluzione, il *Publish.proj* file:

- Compila la soluzione.
- Usa distribuzione Web per creare un pacchetto e distribuire l'applicazione ContactManager.Mvc.
- Usa distribuzione Web per creare un pacchetto e distribuire l'applicazione ContactManager.Service.
- Consente di distribuire il **ContactManager** database.

Quando si integra la distribuzione di più pacchetti web e/o i database in un unico passaggio processo in questo modo, è anche possibile scegliere di eseguire l'intera distribuzione in modalità "what if".

Il *Publish.proj* file viene illustrato come è possibile eseguire questa operazione. In primo luogo, è necessario creare una proprietà per archiviare il valore "what if":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


In questo caso, è stata creata una proprietà denominata **WhatIf** con valore predefinito è **false**. Gli utenti possono eseguire l'override di questo valore impostando la proprietà su **true** in un parametro della riga di comando, come si vedrà a breve.

La fase successiva consiste nell'impostare i parametri per qualsiasi distribuzione Web e VSDBCMD i comandi in modo che rispecchino i flag di **WhatIf** valore della proprietà. Ad esempio, la destinazione successiva (tratto dal *Publish.proj* file e semplificato) viene eseguito il *. deploy. cmd* file per distribuire un pacchetto di web. Per impostazione predefinita, il comando include un' **/Y** switch ("yes" o la modalità di aggiornamento). Se **WhatIf** è impostata su **true**, che viene sostituita da una **/T** switch (versione di valutazione o modalità "what if").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Analogamente, la destinazione successiva utilizza l'utilità VSDBCMD per distribuire un database. Per impostazione predefinita, un **/dd** commutatore non è incluso. Ciò significa che VSDBCMD verrà generato uno script di distribuzione ma non verrà distribuito il database&#x2014;in altre parole, uno scenario "what if". Se il **WhatIf** proprietà non è impostata su **true**, una **/dd** commutatore viene aggiunto e VSDBCMD verrà distribuito il database.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


È possibile utilizzare lo stesso approccio per impostare i parametri per tutti i comandi rilevanti nel file di progetto. Quando si desidera eseguire una distribuzione di "what if", è possibile quindi sufficiente fornire un **WhatIf** valore della proprietà dalla riga di comando:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


In questo modo, è possibile eseguire una distribuzione di "what if" per tutti i componenti di progetto in un unico passaggio.

## <a name="conclusion"></a>Conclusione

In questo argomento descrive come eseguire distribuzioni "what if" con distribuzione Web, VSDBCMD e MSBuild. Una distribuzione di "what if" consente di valutare l'impatto di una distribuzione proposta prima di apportare effettivamente le modifiche nell'ambiente di destinazione.

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sulla sintassi della riga di comando di distribuzione Web, vedere [Web Deploy operazione Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Per materiale sussidiario sulle opzioni della riga di comando quando si usa la *. deploy. cmd* del file, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Per informazioni sulla sintassi della riga di comando VSDBCMD, vedere [riferimento della riga di comando per VSDBCMD. EXE (distribuzione e importazione dello Schema)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Precedente](advanced-enterprise-web-deployment.md)
> [Successivo](customizing-database-deployments-for-multiple-environments.md)
