---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Distribuzione di progetti di Database | Documenti Microsoft
author: jrjlee
description: "Nota: In molti scenari di distribuzione dell'organizzazione, è necessario il possibilità di pubblicare aggiornamenti incrementali di un database distribuito. L'alternativa consiste nel ricreare..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: aef8229f2920bd026e3dbf063afb57cffb9b21d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="deploying-database-projects"></a>Distribuzione di progetti di Database
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> In molti scenari di distribuzione dell'organizzazione, è necessario il possibilità di pubblicare aggiornamenti incrementali di un database distribuito. L'alternativa consiste nel ricreare il database in ogni distribuzione, il che significa che la perdita di dati nel database esistente. Quando si lavora con Visual Studio 2010, utilizzando VSDBCMD è l'approccio consigliato per la pubblicazione incrementale di database. Tuttavia, la prossima versione di Visual Studio e la Pipeline di pubblicazione sul Web (WPP) includerà gli strumenti che supporta direttamente la pubblicazione incrementale.


Se si apre la soluzione di esempio Contact Manager in Visual Studio 2010, si noterà che il progetto di database include una cartella di proprietà che contiene quattro file.

![](deploying-database-projects/_static/image1.png)

Insieme al file di progetto (*ContactManager.Database.dbproj* in questo caso), questi file controllano vari aspetti del processo di compilazione e distribuzione:

- Il *database. sqlcmdvars* file fornisce valori per tutte le variabili SQLCMD utilizzare quando si distribuisce il progetto. Ogni configurazione di soluzione (ad esempio, debug e versione) è possibile specificare un file sqlcmdvars diverso.
- Il *database. sqldeployment* file fornisce le impostazioni di distribuzione specifici, come se si desidera utilizzare regole di confronto definite nel progetto o le regole di confronto del server di destinazione, se si desidera ricreare il database di destinazione ogni l'ora o semplicemente modificare il database esistente per attivare la modalità fino alla data e così via. Ogni configurazione di soluzione è possibile specificare un file diverso. sqldeployment.
- Il *sqlpermissions* file è un documento XML che è possibile utilizzare per definire le autorizzazioni di cui si desidera aggiungere al database di destinazione. Tutte le configurazioni di soluzione condividono lo stesso file con estensione sqlpermissions.
- Il *sqlsettings* file specifica le proprietà a livello di database da utilizzare durante la creazione del database, ad esempio le regole di confronto da utilizzare, il comportamento degli operatori di confronto e così via. Tutte le configurazioni di soluzione condividono lo stesso file sqlsettings.

È opportuno qualche istante per aprire questi file in Visual Studio e acquisire familiarità con il contenuto.

Quando si compila un progetto di database, il processo di compilazione crea due file:

- Oggetto *lo schema del database* (file con estensione dbschema). Descrive lo schema del database a cui che si desidera creare in formato XML.
- Oggetto *manifesto di distribuzione* (file con estensione deploymanifest). Contiene tutte le informazioni necessarie per creare e distribuire il database. Fa riferimento il file dbschema insieme altre risorse, ad esempio le istruzioni di distribuzione (il file con estensione sqldeployment) e tutti gli script SQL di pre-distribuzione o post-distribuzione.

Mostra la relazione tra queste risorse:

![](deploying-database-projects/_static/image2.png)

Come si può notare, il file sqlsettings e il file con estensione sqlpermissions sono gli input per il processo di compilazione. Insieme al file di progetto di database, questi file vengono usati per creare il file di schema di database. Il file con estensione sqldeployment e il file sqlcmdvars passano attraverso il processo di compilazione invariato. Il manifesto della distribuzione indica la posizione dello schema del database, il file con estensione sqldeployment, il file sqlcmdvars e tutti gli script SQL di pre-distribuzione o post-distribuzione.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Perché utilizzare VSDBCMD per distribuire un progetto di Database?

Esistono diversi approcci alla distribuzione di progetti di database. Tuttavia, non tutti sono adatti per la distribuzione di un progetto di database nei server remoti in un ambiente aziendale. Prendere in considerazione desiderato da una distribuzione di progetti di database. Negli scenari di distribuzione dell'organizzazione, è probabile che desidera:

- La possibilità di distribuire il progetto di database da una posizione remota.
- La possibilità di eseguire aggiornamenti incrementali per un database esistente.
- La possibilità di includere gli script di pre-distribuzione o post-distribuzione.
- La possibilità di personalizzare la distribuzione in più ambienti di destinazione.
- La possibilità di distribuire il progetto di database come parte del più grande, in genere creato uno script, la distribuzione della soluzione passo a passo.

Esistono tre approcci principali, che è possibile utilizzare per distribuire un progetto di database:

- È possibile utilizzare la funzionalità di distribuzione con il tipo di progetto di database in Visual Studio 2010. Quando si compila e distribuisce un progetto di database in Visual Studio 2010, il processo di distribuzione utilizza il manifesto di distribuzione per generare un file di distribuzione basato su SQL specifico per la configurazione della build. Il database verrà creata se non esiste o apportare le modifiche necessarie al database, se esiste già. È possibile utilizzare SQLCMD.exe per eseguire questo file nel server di destinazione oppure è possibile impostare Visual Studio per creare ed eseguire il file. Lo svantaggio di questo approccio è che sono limitate controllo sulle impostazioni di distribuzione. È spesso anche potrebbe essere necessario modificare il file di distribuzione di SQL per fornire i valori delle variabili specifiche dell'ambiente. È possibile utilizzare solo questo approccio da un computer con Visual Studio 2010 installato, e lo sviluppatore dovrebbe conoscere e specificare le stringhe di connessione e le credenziali per tutti gli ambienti di destinazione.
- È possibile utilizzare lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) per [distribuire un database come parte di un progetto di applicazione web](https://msdn.microsoft.com/en-us/library/dd465343.aspx). Tuttavia, questo approccio è molto più complesso, se si desidera distribuire un progetto di database anziché semplicemente replicare un database locale esistente in un server di destinazione. È possibile configurare la distribuzione Web per eseguire lo script di distribuzione SQL che genera il progetto di database, ma questo scopo, è necessario creare un file di destinazioni WPP personalizzato per il progetto di applicazione web. Consente di aggiungere una quantità sostanziale di complessità del processo di distribuzione. Inoltre, distribuzione Web non supportano direttamente gli aggiornamenti incrementali per i database esistenti. Per ulteriori informazioni su questo approccio, vedere [la Pipeline di pubblicazione sul Web al progetto di database di pacchetto di estensione di file SQL distribuito](https://go.microsoft.com/?linkid=9805121).
- È possibile utilizzare l'utilità VSDBCMD per distribuire il database, utilizzando lo schema del database o il manifesto di distribuzione. È possibile chiamare VSDBCMD.exe da una destinazione di MSBuild, che consente di pubblicare database come parte di un processo di distribuzione tramite script. È possibile sostituire le variabili nel file sqlcmdvars e molte altre proprietà di database da un comando VSDBCMD, che consente di personalizzare la distribuzione per diversi ambienti senza creare più configurazioni di compilazione. VSDBCMD fornisce funzionalità di differenziazione, ovvero che apporterà solo le modifiche necessarie per l'allineamento di un database di destinazione con lo schema del database. VSDBCMD offre inoltre un'ampia gamma di opzioni della riga di comando, che consentono un controllo accurato sul processo di distribuzione.

Da questa panoramica, si noterà che l'utilizzo di VSDBCMD con MSBuild è l'approccio più adatto per uno scenario di distribuzione tipici aziendali:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Supporta la distribuzione remota? | Sì | Sì | Sì |
| Supporta gli aggiornamenti incrementali? | Sì | No | Sì |
| Supporta gli script di pre o post-distribuzione? | Sì | Sì | Sì |
| Supporta un multi-ambiente distribuzione? | Limitato | Limitato | Sì |
| Supporta script di distribuzione? | Limitato | Sì | Sì |

Il resto di questo argomento viene descritto l'utilizzo di VSDBCMD con MSBuild per distribuire i progetti di database.

## <a name="understanding-the-deployment-process"></a>Informazioni sul processo di distribuzione

L'utilità VSDBCMD consente di distribuire un database utilizzando lo schema del database (file con estensione dbschema) o il manifesto di distribuzione (il file con estensione deploymanifest). In pratica, quasi sempre userai il manifesto di distribuzione, come il manifesto di distribuzione è possibile fornire valori predefiniti per varie proprietà di distribuzione e identificare qualsiasi script pre-distribuzione o post-distribuzione SQL da eseguire. Ad esempio, questo comando VSDBCMD viene utilizzato per distribuire il **ContactManager** database a un server di database in un ambiente di test:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


In questo caso:

- Il **/a** (o **/Action**) consente di specificare VSDBCMD eseguire desiderato. È possibile impostarlo su **importazione** o **Distribuisci**. Il **importazione** opzione viene utilizzata per generare un file con estensione dbschema da un database esistente e **Distribuisci** opzione viene usata per distribuire un file con estensione dbschema in un database di destinazione.
- Il **/manifesto** (o **/ManifestFile**) commutatore identifica il file con estensione deploymanifest si desidera distribuire. Se si desidera utilizzare invece il file con estensione dbschema, utilizzerebbe il **/modello** (o **/ModelFile**) passare.
- Il **/cs** (o **/ConnectionString**) commutatore fornisce la stringa di connessione per il server di database di destinazione. Si noti che questo non include il nome del database & #x 2014; VSDBCMD deve connettersi al server per la creazione del database. non è necessario connettersi a un singolo database. Se il file con estensione deploymanifest include una stringa di connessione, è possibile omettere questa opzione è attivata. Se si utilizza l'opzione comunque, il valore dell'opzione sostituirà il valore DeployManifest.
- Il **/p: TargetDatabase** proprietà fornisce il nome da assegnare al database di destinazione al momento della creazione. Questo sostituisce il valore del **TargetDatabase** proprietà nel file con estensione deploymanifest. È possibile utilizzare il **/p:** *[nome proprietà]*sintassi per impostare un'ampia gamma di proprietà di distribuzione ed eseguire l'override di tutte le variabili SQLCMD dichiarato nel file sqlcmdvars.
- Il **/dd+** (o **/DeployToDatabase+**) parametro indica che si desidera creare una distribuzione e distribuire in ambiente di destinazione. Se si specifica **/dd-**, omettere il parametro o VSDBCMD genera uno script di distribuzione ma non distribuirla nell'ambiente di destinazione. Questa opzione è spesso l'origine di confusione ed è illustrata più dettagliatamente nella sezione successiva.
- Il **/script** (o **/DeploymentScriptFile**) consente di specificare dove si desidera generare lo script di distribuzione. Questo valore non influisce sul processo di distribuzione.

Per ulteriori informazioni su VSDBCMD, vedere [riferimento della riga di comando per VSDBCMD. EXE (distribuzione e importazione dello Schema)](https://msdn.microsoft.com/en-us/library/dd193283.aspx) e [come: preparare un Database per la distribuzione da un prompt dei comandi tramite VSDBCMD. EXE](https://msdn.microsoft.com/en-us/library/dd193258.aspx).

Per un esempio di come è possibile utilizzare VSDBCMD da un file di progetto MSBuild, vedere [comprendere il processo di compilazione](understanding-the-build-process.md). Per esempi di come configurare le impostazioni di distribuzione di database per più ambienti, vedere [personalizzazione delle distribuzioni di Database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>La comprensione del commutatore DeployToDatabase

Il comportamento del **/dd** o **/DeployToDatabase** commutatore varia a seconda che si utilizzi VSDBCMD con un file con estensione dbschema o un file con estensione deploymanifest. Se si utilizza un file con estensione dbschema, il comportamento è piuttosto semplice:

- Se si specifica **/dd+** o **/dd**, VSDBCMD genererà uno script di distribuzione e distribuire il database.
- Se si specifica **/dd-** o omettere il parametro, VSDBCMD genererà solo uno script di distribuzione.

Se si utilizza un file con estensione deploymanifest, il comportamento è molto più complesso. In questo modo il file con estensione deploymanifest contiene un nome di proprietà **DeployToDatabase** che determina inoltre se il database viene distribuito.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Il valore di questa proprietà è impostato in base alle proprietà del progetto di database. Se si imposta la **azione di distribuzione** a **crea uno script di distribuzione (SQL)**, il valore sarà **False**. Se si imposta la **azione di distribuzione** a **crea uno script di distribuzione (SQL) e la distribuzione al database**, il valore sarà **True**.

> [!NOTE]
> Queste impostazioni sono associate a una piattaforma e configurazione della build specifica. Ad esempio, se si configurano le impostazioni di **Debug** configurazione e quindi pubblicare utilizzando la **versione** configurazione, non verranno utilizzate le impostazioni.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> In questo scenario, il **azione di distribuzione** deve sempre essere impostata su **crea uno script di distribuzione (SQL)**, perché non si desidera distribuire il database di Visual Studio 2010. In altre parole, il **DeployToDatabase** proprietà deve essere sempre **False**.


Quando un **DeployToDatabase** proprietà viene specificata, il **/dd** commutatore sostituirà la proprietà solo se il valore della proprietà **false**:

- Se il **DeployToDatabase** proprietà **False**, e si specifica **/dd+** o **/dd**, VSDBCMD eseguirà l'override di  **DeployToDatabase** proprietà e distribuire il database.
- Se il **DeployToDatabase** proprietà **False**, e si specifica **/dd-** o omettere il parametro, VSDBCMD non verrà distribuito il database.
- Se il **DeployToDatabase** proprietà **True**, VSDBCMD ignorerà il commutatore e distribuire il database.
- In ogni caso, indipendentemente dal fatto se si distribuisce il database e viene generato uno script di distribuzione.

## <a name="conclusion"></a>Conclusione

In questo argomento viene fornita una panoramica del processo di compilazione e distribuzione per progetti di database in Visual Studio 2010. Inoltre descritto come è possibile utilizzare VSDBCMD.exe con MSBuild per supportare la distribuzione del database su larga scala.

Per ulteriori informazioni sul funzionamento in pratica, vedere [personalizzazione delle distribuzioni di Database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni su come personalizzare le distribuzioni di database tramite la creazione di un file di configurazione di distribuzione separata per ogni ambiente, vedere [personalizzazione delle distribuzioni di Database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Per istruzioni su come configurare le appartenenze ai ruoli di database eseguendo uno script di post-distribuzione, vedere [distribuzione appartenenze al ruolo per gli ambienti di Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Per informazioni aggiuntive sulla gestione di alcune delle problematiche più tale appartenenza imporre i database, vedere [la distribuzione di database di appartenenza per ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Questi argomenti in MSDN includono linee guida più ampia e informazioni generali sui progetti di database di Visual Studio e il processo di distribuzione di database:

- [Progetti di Database di Visual Studio 2010 SQL Server](https://msdn.microsoft.com/en-us/library/ff678491.aspx)
- [Gestione delle modifiche di Database](https://msdn.microsoft.com/en-us/library/aa833404.aspx)
- [Procedura: preparare un Database per la distribuzione da un prompt dei comandi tramite VSDBCMD. FILE EXE](https://msdn.microsoft.com/en-us/library/dd193258.aspx)
- [Una panoramica di Database e di distribuzione](https://msdn.microsoft.com/en-us/library/aa833165.aspx)

>[!div class="step-by-step"]
[Precedente](deploying-web-packages.md)
[Successivo](creating-and-running-a-deployment-command-file.md)
