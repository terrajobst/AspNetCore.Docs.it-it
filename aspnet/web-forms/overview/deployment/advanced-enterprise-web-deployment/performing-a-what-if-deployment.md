---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Esecuzione di una cosa se distribuzione | Documenti Microsoft
author: jrjlee
description: In questo argomento viene descritto come eseguire 'cosa accade se' (o simulato) distribuzioni con lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) e il V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: cea805c86f0764c7443ccc5c9f89248860a6a842
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="performing-a-what-if-deployment"></a>Esecuzione di una distribuzione "Cosa accade se"
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene descritto come eseguire "cosa accade se" (o simulato) usando lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) e un VSDBCMD distribuzioni. Ciò consente di determinare gli effetti della logica di distribuzione in un ambiente di destinazione specifico prima di distribuire effettivamente l'applicazione.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione e distribuzione è controllato da due file di progetto & #x 2014; o ne contenente le istruzioni di compilazione che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Esecuzione di una distribuzione "Cosa accade se" per i pacchetti Web

Distribuzione Web sono incluse funzionalità che consente di eseguire le distribuzioni in "cosa accade se" (o la versione di valutazione) modalità. Quando si distribuiscono gli elementi in modalità "cosa accade se", distribuzione Web genera un file di log come se si ha eseguito la distribuzione, ma in realtà non modifica alcuna operazione nel server di destinazione. Esaminare il file di log può essere utile per comprendere l'impatto che la distribuzione sarà necessario nel server di destinazione, in particolare:

- Ciò che viene aggiunto.
- Cosa verrà aggiornata.
- Ciò che verrà eliminato.

Poiché una distribuzione "cosa accade se" non modifica effettivamente nel server di destinazione, cosa può sempre fare stimare se una distribuzione avrà esito positivo.

Come descritto in [distribuzione di pacchetti Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), è possibile distribuire i pacchetti web tramite distribuzione Web in due modi & #x 2014, tramite l'utilità della riga di comando MSDeploy.exe direttamente o tramite l'esecuzione di *. deploy* file che genera il processo di compilazione.

Se si utilizza MSDeploy.exe direttamente, è possibile eseguire una distribuzione "cosa accade se" aggiungendo il **– whatif** flag al comando. Ad esempio, per valutare a che cosa accadrebbe se è stato distribuito il pacchetto ContactManager.Mvc.zip in un ambiente di gestione temporanea, il comando di MSDeploy sarà analogo al seguente:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Quando si è soddisfatti dei risultati della distribuzione di "cosa accade se", è possibile rimuovere il **– whatif** flag per eseguire una distribuzione in tempo reale.

> [!NOTE]
> Per ulteriori informazioni sulle opzioni della riga di comando di MSDeploy.exe, vedere [Web Deploy operazione Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Se si usa il *. deploy* file, è possibile eseguire una distribuzione "cosa accade se" includendo il **/t** flag flag (modalità di valutazione) anziché il **/y** flag ("yes" o la modalità di aggiornamento) in il comando. Ad esempio, per valutare che cosa accadrebbe se è stato distribuito il pacchetto ContactManager.Mvc.zip eseguendo il *. deploy* file, il comando sarà analogo al seguente:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Quando si è soddisfatti dei risultati della distribuzione di "modalità di valutazione", è possibile sostituire il **/t** contrassegnare con un **/y** flag per eseguire una distribuzione in tempo reale:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Per ulteriori informazioni sulle opzioni della riga di comando per *. deploy* file, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy](https://msdn.microsoft.com/library/ff356104.aspx). Se si esegue il *. deploy* file senza specificare alcun flag, il prompt dei comandi verrà visualizzato un elenco dei flag.


## <a name="performing-a-what-if-deployment-for-databases"></a>Esecuzione di una distribuzione "Cosa accade se" per i database

In questa sezione presuppone che si sta utilizzando l'utilità VSDBCMD per eseguire la distribuzione incrementale, basati sullo schema del database. Questo approccio è descritto in dettaglio in [progetti di Database di distribuzione](../web-deployment-in-the-enterprise/deploying-database-projects.md). È consigliabile acquisire familiarità con questo argomento prima di applicare i concetti descritti di seguito.

Quando si utilizza VSDBCMD in **Distribuisci** modalità, è possibile utilizzare il **/dd** (o **/DeployToDatabase**) flag per controllare se VSDBCMD effettivamente distribuisce il database o parte genera semplicemente una script di distribuzione. Se si distribuisce un file con estensione dbschema, si tratta del comportamento:

- Se si specifica **/dd+** o **/dd**, VSDBCMD genererà uno script di distribuzione e distribuire il database.
- Se si specifica **/dd-** o omettere il parametro, VSDBCMD genererà solo uno script di distribuzione.

> [!NOTE]
> Se si distribuisce un file con estensione deploymanifest anziché un file con estensione dbschema, il comportamento del **/dd** switch è molto più complessa. In pratica, VSDBCMD ignorerà il valore della **/dd** cambia se il file con estensione deploymanifest include un **DeployToDatabase** elemento con un valore di **True**. [Distribuzione di progetti di Database](../web-deployment-in-the-enterprise/deploying-database-projects.md) descrive questo comportamento in modo completo.


Ad esempio, per generare uno script di distribuzione per il **ContactManager** database senza distribuire effettivamente il database, il comando VSDBCMD dovrebbe essere simile alla seguente:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD è uno strumento di backup differenziale del database di distribuzione e di conseguenza lo script di distribuzione viene generato dinamicamente per contenere tutti i comandi SQL necessari per aggiornare il database corrente, se presente, lo schema specificato. Esaminare lo script di distribuzione è un modo utile per determinare l'impatto della distribuzione sarà necessario nel database corrente e i dati in che esso contenuti. Ad esempio, può essere utile determinare:

- Se le tabelle esistenti verranno rimossi e se che provocherà la perdita di dati.
- Se l'ordine delle operazioni comporta un rischio di perdita di dati, ad esempio, se la suddivisione o l'unione di tabelle.

Se si è soddisfatti lo script di distribuzione, è possibile ripetere il VSDBCMD con un **/dd+** flag per apportare le modifiche. In alternativa, è possibile modificare lo script di distribuzione per soddisfare i requisiti e quindi eseguirlo manualmente nel server di database.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>L'integrazione di funzionalità "Cosa accade se" nel file di progetto personalizzati

Negli scenari di distribuzione più complessi, è opportuno utilizzare un file di progetto di Microsoft Build Engine (MSBuild) personalizzato per incapsulare la logica di compilazione e distribuzione, come descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Ad esempio, nel [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , soluzione di esempio di *Publish.proj* file:

- Compila la soluzione.
- Viene utilizzato per assemblare e distribuire l'applicazione ContactManager.Mvc distribuzione Web.
- Viene utilizzato per assemblare e distribuire l'applicazione ContactManager.Service distribuzione Web.
- Consente di distribuire il **ContactManager** database.

Quando si integra la distribuzione di più pacchetti web e/o i database in un singolo passaggio processo in questo modo, è inoltre possibile scegliere di eseguire l'intera distribuzione in modalità "cosa accade se".

Il *Publish.proj* file illustra come è possibile farlo. È necessario innanzitutto creare una proprietà per archiviare il valore "cosa accade se":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


In questo caso, si crea una proprietà denominata **WhatIf** con un valore predefinito di **false**. Gli utenti possono eseguire l'override di questo valore impostando la proprietà su **true** in un parametro della riga di comando, come si vedrà a breve.

La fase successiva consiste nel parametrizzare qualsiasi distribuzione Web e VSDBCMD i comandi in modo che riflettano i flag di **WhatIf** valore della proprietà. Ad esempio, la destinazione successiva (ricavato il *Publish.proj* file e semplificare la) viene eseguito il *. deploy* file per distribuire un pacchetto di web. Per impostazione predefinita, il comando include un **/Y** switch ("yes" o la modalità di aggiornamento). Se **WhatIf** è impostato su **true**, questo viene sostituito da un **/T** switch (versione di valutazione o modalità "cosa accade se").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Analogamente, la destinazione successiva utilizza l'utilità VSDBCMD per distribuire un database. Per impostazione predefinita, un **/dd** commutatore non è incluso. Ciò significa che VSDBCMD genera uno script di distribuzione ma non verrà distribuito il database di & #x 2014; in altre parole, uno scenario "cosa accade se". Se il **WhatIf** proprietà non è impostata su **true**, **/dd** viene aggiunta l'opzione e VSDBCMD verrà distribuito il database.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


È possibile utilizzare lo stesso approccio di parametrizzare tutti i comandi nel file di progetto. Quando si desidera eseguire una distribuzione "cosa accade se", è possibile quindi sufficiente fornire un **WhatIf** valore della proprietà dalla riga di comando:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


In questo modo, è possibile eseguire una distribuzione "cosa accade se" per tutti i componenti del progetto in un unico passaggio.

## <a name="conclusion"></a>Conclusione

In questo argomento viene descritto come eseguire le distribuzioni "cosa accade se" con MSBuild, VSDBCMD e distribuzione Web. Una distribuzione "cosa accade se" consente di valutare l'impatto di una distribuzione proposta prima di apportare eventuali modifiche effettivamente nell'ambiente di destinazione.

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sulla sintassi della riga di comando di distribuzione Web, vedere [Web Deploy operazione Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Per informazioni aggiuntive sulle opzioni della riga di comando quando si utilizza il *. deploy* file, vedere [procedura: installare un pacchetto di distribuzione tramite il File deploy](https://msdn.microsoft.com/library/ff356104.aspx). Per ulteriori informazioni sulla sintassi della riga di comando VSDBCMD, vedere [riferimento della riga di comando per VSDBCMD. EXE (distribuzione e importazione dello Schema)](https://msdn.microsoft.com/library/dd193283.aspx).

>[!div class="step-by-step"]
[Precedente](advanced-enterprise-web-deployment.md)
[Successivo](customizing-database-deployments-for-multiple-environments.md)
