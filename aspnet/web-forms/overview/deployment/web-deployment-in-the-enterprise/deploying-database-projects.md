---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Distribuzione di progetti di Database | Microsoft Docs
author: jrjlee
description: "Nota: In molti scenari di distribuzione aziendale, è necessario la possibilità di pubblicare aggiornamenti incrementali per un database distribuito. L'alternativa consiste nel ricreare..."
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 43fa197a1d5a3cf521f4d2202754ff0d121cebe3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833320"
---
<a name="deploying-database-projects"></a>Distribuzione di progetti di Database
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> In molti scenari di distribuzione aziendale, è necessario la possibilità di pubblicare aggiornamenti incrementali per un database distribuito. L'alternativa consiste nel ricreare il database in ogni distribuzione, il che significa che la perdita di dati del database esistente. Quando si lavora con Visual Studio 2010, utilizzando VSDBCMD è l'approccio consigliato per la pubblicazione incrementale di database. Tuttavia, la prossima versione di Visual Studio e la Pipeline di pubblicazione sul Web (WPP) include strumenti che supporta direttamente la pubblicazione incrementale.


Se si apre la soluzione di esempio Contact Manager in Visual Studio 2010, si noterà che il progetto di database include una cartella di proprietà che contiene quattro file.

![](deploying-database-projects/_static/image1.png)

Insieme al file di progetto (*ContactManager.Database.dbproj* in questo caso), questi file controllano vari aspetti del processo di compilazione e distribuzione:

- Il *database. sqlcmdvars* file fornisce valori per tutte le variabili SQLCMD utilizzare quando si distribuisce il progetto. Ogni configurazione di soluzione (ad esempio, debug e rilascio) è possibile specificare un file sqlcmdvars diverso.
- Il *database. sqldeployment* file fornisce le impostazioni specifiche della distribuzione, come se si desidera utilizzare le regole di confronto definite nel progetto o le regole di confronto del server di destinazione, se si desidera ricreare il database di destinazione ogni ora, o semplicemente modificare il database esistente per attivare la modalità fino alla data e così via. Ogni configurazione della soluzione può specificare un file. sqldeployment diversi.
- Il *sqlpermissions* file è un documento XML che è possibile usare per definire tutte le autorizzazioni si desidera aggiungere al database di destinazione. Tutte le configurazioni di soluzione condividono lo stesso file con estensione sqlpermissions.
- Il *sqlsettings* file specifica le proprietà a livello di database da utilizzare durante la creazione del database, ad esempio le regole di confronto da usare, il comportamento degli operatori di confronto e così via. Tutte le configurazioni di soluzione condividono lo stesso file sqldeployment.

È opportuno per aprire questi file in Visual Studio e acquisire familiarità con il contenuto.

Quando si compila un progetto di database, il processo di compilazione crea due file:

- Oggetto *dello schema del database* (dbschema). Descrive lo schema del database da creare in formato XML.
- Oggetto *manifesto della distribuzione* (file con estensione deploymanifest). Che contiene tutte le informazioni necessarie per creare e distribuire il database. Riferimenti a file dbschema e altre risorse, ad esempio le istruzioni di distribuzione (il file con estensione sqldeployment) e qualsiasi script di pre-distribuzione o post-distribuzione SQL.

Ciò viene illustrata la relazione tra queste risorse:

![](deploying-database-projects/_static/image2.png)

Come può notare, il file sqldeployment e il file con estensione sqlpermissions sono input al processo di compilazione. Insieme al file di progetto di database, questi file vengono usati per creare il file di schema di database. Il file. sqldeployment e del file sqlcmdvars passano attraverso il processo di compilazione non modificato. Il manifesto di distribuzione indica la posizione in cui lo schema del database, il file con estensione sqldeployment, il file sqlcmdvars e qualsiasi script di pre-distribuzione o post-distribuzione SQL.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Perché usare VSDBCMD per distribuire un progetto di Database?

Esistono vari approcci diversi per la distribuzione di progetti di database. Tuttavia, non tutte sono adatte per la distribuzione di un progetto di database a server remoti in un ambiente aziendale. Prendere in considerazione la configurazione desiderata una distribuzione di progetti di database. Negli scenari di distribuzione aziendale, è probabile che desidera:

- La possibilità di distribuire il progetto di database da una posizione remota.
- La possibilità di eseguire aggiornamenti incrementali per un database esistente.
- La possibilità di includere script di pre-distribuzione o gli script di post-distribuzione.
- La possibilità di personalizzare la distribuzione in più ambienti di destinazione.
- La possibilità di distribuire il progetto di database come parte di una più grande, in genere creato uno script, la distribuzione della soluzione passo a passo.

Esistono tre approcci principali, che è possibile usare per distribuire un progetto di database:

- È possibile usare la funzionalità di distribuzione con il tipo di progetto di database in Visual Studio 2010. Quando si compila e distribuisce un progetto di database in Visual Studio 2010, il processo di distribuzione Usa il manifesto di distribuzione per generare un file di distribuzione basato su SQL specifico per la configurazione della build. Il database verrà creato se non esiste o apportare le modifiche necessarie nel database se esiste già. È possibile usare SQLCMD.exe per eseguire questo file nel server di destinazione oppure è possibile impostare Visual Studio per creare ed eseguire il file. Lo svantaggio di questo approccio è che l'utente sono limitate solo controllare le impostazioni di distribuzione. È spesso anche potrebbe essere necessario modificare il file di distribuzione di SQL per fornire i valori delle variabili specifiche dell'ambiente. È possibile usare solo questo approccio da un computer con Visual Studio 2010 installato, e gli sviluppatori dovranno conoscere e fornire le stringhe di connessione e credenziali per tutti gli ambienti di destinazione.
- È possibile utilizzare lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) per [distribuire un database come parte di un progetto di applicazione web](https://msdn.microsoft.com/library/dd465343.aspx). Tuttavia, questo approccio è molto più complesso se si desidera distribuire un progetto di database anziché semplicemente eseguire la replica di un database esistente locale in un server di destinazione. È possibile configurare distribuzione Web per eseguire lo script di distribuzione SQL che genera il progetto di database, ma a questo scopo, è necessario creare un file di destinazioni WPP personalizzato per il progetto di applicazione web. Verrà aggiunta una notevole quantità di complessità del processo di distribuzione. Inoltre, distribuzione Web non supportano direttamente gli aggiornamenti incrementali per i database esistenti. Per altre informazioni su questo approccio, vedere [estendendo la Pipeline di pubblicazione sul Web al progetto di database di pacchetto distribuita file SQL](https://go.microsoft.com/?linkid=9805121).
- È possibile utilizzare l'utilità VSDBCMD per distribuire il database, usando lo schema del database o il manifesto di distribuzione. È possibile chiamare VSDBCMD.exe da una destinazione di MSBuild, che consente di pubblicare i database come parte di un processo di distribuzione con script e di grandi dimensioni. È possibile sostituire le variabili nel file sqlcmdvars e molte altre proprietà di database da un comando VSDBCMD, che consente di personalizzare la distribuzione per diversi ambienti senza creare più configurazioni della build. VSDBCMD fornisce funzionalità di differenziazione, ovvero che apporterà solo le modifiche necessarie per l'allineamento di un database di destinazione con lo schema del database. VSDBCMD offre anche un'ampia gamma di opzioni della riga di comando, che consentono un controllo accurato sul processo di distribuzione.

Questa panoramica, si noterà che l'utilizzo VSDBCMD con MSBuild è l'approccio più adatto per uno scenario di distribuzione aziendale tipica:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Supporta la distribuzione remota? | Yes | Yes | Yes |
| Supporta aggiornamenti incrementali? | Yes | No | Yes |
| Supporta gli script di pre/post-distribuzione? | Yes | Yes | Yes |
| Supporta la distribuzione di più ambienti? | Limitato | Limitato | Yes |
| Supporta la distribuzione con script? | Limitato | Yes | Yes |

Il resto di questo argomento viene descritto l'utilizzo di VSDBCMD con MSBuild per distribuire i progetti di database.

## <a name="understanding-the-deployment-process"></a>Informazioni sul processo di distribuzione

L'utilità VSDBCMD ti permette di distribuire un database usando lo schema del database (file dbschema) o il manifesto di distribuzione (il file con estensione deploymanifest). In pratica, è quasi sempre userà il manifesto di distribuzione, come il manifesto di distribuzione consente di fornire valori predefiniti per varie proprietà di distribuzione e identificare eventuali script SQL pre-distribuzione o post-distribuzione da eseguire. Ad esempio, questo comando VSDBCMD consente di distribuire il **ContactManager** database a un server di database in un ambiente di test:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


In questo caso:

- Il **/a** (o **/Action**) specifica ciò che si desidera VSDBCMD eseguire. È possibile impostarlo su **importazione** oppure **Distribuisci**. Il **importazione** opzione viene usata per generare un file. dbschema da un database esistente e il **Distribuisci** opzione viene usata per distribuire un file. dbschema in un database di destinazione.
- Il **/manifest** (o **/ManifestFile**) commutatore identifica il file DeployManifest si vuole distribuire. Se si vuole usare il file. dbschema invece, si userà il **/modellare** (o **/ModelFile**) passare.
- Il **/cs** (o **/ConnectionString**) commutatore fornisce la stringa di connessione per il server di database di destinazione. Si noti che questo non include il nome del database&#x2014;VSDBCMD deve connettersi al server per creare il database. non è necessario per connettersi a un singolo database. Se il file con estensione deploymanifest include una stringa di connessione, è possibile omettere questa opzione è attivata. Se si usa l'opzione comunque, il valore dell'opzione sostituirà il valore DeployManifest.
- Il <strong>/p: TargetDatabase</strong> proprietà fornisce il nome da assegnare al database di destinazione al momento della creazione. Questa impostazione sostituisce il valore della <strong>TargetDatabase</strong> nel file con estensione deploymanifest proprietà. È possibile usare la <strong>/p:</strong> <em>[nome proprietà]</em>sintassi per impostare un'ampia gamma di proprietà di distribuzione e per eseguire l'override di tutte le variabili SQLCMD dichiarato nel file sqlcmdvars.
- Il **/dd+** (o **/DeployToDatabase+**) opzione indica che si desidera creare una distribuzione e distribuirla nell'ambiente di destinazione. Se si specifica **/dd-**, oppure omettere il commutatore, VSDBCMD verrà generato uno script di distribuzione ma non distribuirla nell'ambiente di destinazione. Questa opzione è spesso la fonte di confusione e viene illustrata più dettagliatamente nella sezione successiva.
- Il **/script** (o **/DeploymentScriptFile**) consente di specificare dove si desidera generare lo script di distribuzione. Questo valore non influenza il processo di distribuzione.

Per altre informazioni su VSDBCMD, vedere [riferimento della riga di comando per VSDBCMD. EXE (distribuzione e importazione dello Schema)](https://msdn.microsoft.com/library/dd193283.aspx) e [procedura: preparare un Database per la distribuzione da un prompt dei comandi usando VSDBCMD. File EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Per un esempio di come è possibile utilizzare VSDBCMD da un file di progetto MSBuild, vedere [informazioni sul processo di compilazione](understanding-the-build-process.md). Per esempi di come configurare le impostazioni di distribuzione di database per più ambienti, vedere [personalizzazione le distribuzioni dei Database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>La comprensione del commutatore DeployToDatabase

Il comportamento dei **/dd** oppure **/DeployToDatabase** commutatore dipende dal fatto che si usi VSDBCMD con un file. dbschema o un file con estensione deploymanifest. Se si usa un file. dbschema, il comportamento è piuttosto semplice:

- Se si specifica **/dd+** oppure **/dd**, VSDBCMD genererà uno script di distribuzione e distribuire il database.
- Se si specifica **/dd-** o omettere il commutatore, VSDBCMD genererà solo uno script di distribuzione.

Se si usa un file con estensione deploymanifest, il comportamento è molto più complesso. Infatti, il file con estensione deploymanifest contiene un nome di proprietà **DeployToDatabase** che determina anche se il database viene distribuito.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Il valore di questa proprietà è impostato in base alle proprietà del progetto di database. Se si imposta la **azione di distribuzione** al **creare uno script di distribuzione (con estensione SQL)**, il valore sarà **False**. Se si imposta la **azione di distribuzione** al **creare uno script di distribuzione (con estensione SQL) e distribuire il database**, il valore sarà **True**.

> [!NOTE]
> Queste impostazioni sono associate a una piattaforma e configurazione della build specifica. Ad esempio, se si configurano le impostazioni per il **eseguire il Debug** configuration e quindi pubblicare usando la **versione** configurazione, non verranno utilizzate le impostazioni.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> In questo scenario, il **azione di distribuzione** deve sempre essere impostata su **creare uno script di distribuzione (con estensione SQL)**, perché non si desidera che Visual Studio 2010 per distribuire il database. In altre parole, il **DeployToDatabase** proprietà deve essere sempre **False**.


Quando un **DeployToDatabase** proprietà viene specificata, il **/dd** commutatore solo eseguirà l'override della proprietà se il valore della proprietà **false**:

- Se il **DeployToDatabase** proprietà è **False**, e si specifica **/dd+** oppure **/dd**, VSDBCMD eseguirà l'override di  **DeployToDatabase** proprietà e distribuire il database.
- Se il **DeployToDatabase** è di proprietà **False**, e si specifica **/dd-** o omettere il commutatore, VSDBCMD non verrà distribuito il database.
- Se il **DeployToDatabase** è di proprietà **True**, VSDBCMD ignora il commutatore e distribuire il database.
- In ogni caso, indipendentemente dal fatto che si distribuisce il database, viene generato uno script di distribuzione.

## <a name="conclusion"></a>Conclusione

In questo argomento è fornita una panoramica del processo di compilazione e distribuzione per i progetti di database in Visual Studio 2010. Inoltre illustrato come è possibile usare VSDBCMD.exe con MSBuild per supportare la distribuzione di database su larga scala.

Per altre informazioni sul funzionamento in pratica, vedere [personalizzazione le distribuzioni dei Database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Ulteriori informazioni

Per informazioni su come personalizzare le distribuzioni dei database tramite la creazione di un file di configurazione di distribuzione distinto per ogni ambiente, vedere [personalizzazione le distribuzioni dei Database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Per indicazioni su come configurare le appartenenze ai ruoli di database eseguendo uno script di post-distribuzione, vedere [distribuzione di Database le appartenenze ai ruoli per gli ambienti di Test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Per indicazioni sulla gestione di alcune delle problematiche tale appartenenza impongono i database, vedere [la distribuzione dei database di appartenenza negli ambienti aziendali](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Questi argomenti in MSDN forniscono indicazioni più ampio e informazioni generali sui progetti di database di Visual Studio e il processo di distribuzione di database:

- [Progetti di Database di Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx)
- [Gestione delle modifiche di Database](https://msdn.microsoft.com/library/aa833404.aspx)
- [Procedura: preparare un Database per la distribuzione da un prompt dei comandi usando VSDBCMD. FILE EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Una panoramica della distribuzione e compilazione del Database](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-web-packages.md)
> [Successivo](creating-and-running-a-deployment-command-file.md)
