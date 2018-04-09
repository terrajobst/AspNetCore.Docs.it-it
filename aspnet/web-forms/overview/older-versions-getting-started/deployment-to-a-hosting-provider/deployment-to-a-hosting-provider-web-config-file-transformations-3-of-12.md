---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: trasformazioni File Web. config - 3 di 12 | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 86eb74ca35e8804978127412e2276eeee9d615dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: trasformazioni File Web. config - 3 di 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come automatizzare il processo di modifica di *Web. config* file quando viene distribuito in ambienti di destinazione diversi. La maggior parte delle applicazioni avere impostazioni di *Web. config* file che deve essere diverse quando l'applicazione viene distribuita. Automatizzare il processo di queste modifiche evita di dover effettuare l'operazione manualmente ogni volta che si distribuisce, che sarebbe attività tediosa e soggetta a errori.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>I parametri di distribuire le trasformazioni di Web. config e Web

Esistono due modi per automatizzare il processo di modifica *Web. config* impostazioni del file: [trasformazioni di Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parametri distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx). A *Web. config* file di trasformazione contiene markup XML che specifica la modalità di modifica di *Web. config* file quando viene distribuito. È possibile specificare diverse modifiche per specifiche configurazioni di compilazione e profili di pubblicazione specifico. Le configurazioni di compilazione predefinite sono di Debug e rilascio ed è possibile creare configurazioni di compilazione personalizzata. Un profilo di pubblicazione è in genere corrisponde a un ambiente di destinazione. (Si apprenderà informazioni sulla pubblicazione dei profili nel [distribuzione in IIS come ambiente di Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) esercitazione.)

Parametri di distribuzione Web consente di specificare diversi tipi di impostazioni che devono essere configurati durante la distribuzione, incluse le impostazioni che si trovano in *Web. config* file. Quando viene utilizzata per specificare *Web. config* modifiche al file, sono più complessi per impostare i parametri di distribuzione Web, ma sono utili quando non si conosce il valore da impostare finché non vengono distribuiti. In un ambiente aziendale, ad esempio, è possibile creare un *pacchetto di distribuzione* e assegnare a un utente del reparto IT per installare nell'ambiente di produzione e che l'utente deve essere in grado di immettere le stringhe di connessione o le password che non li conoscere.

Per lo scenario che illustra questa esercitazione, tutto ciò che deve essere eseguita per conoscere il *Web. config* file, in modo non è necessario utilizzare i parametri di distribuzione Web. È possibile configurare alcune trasformazioni che differiscono a seconda della configurazione di compilazione utilizzata e altre che variano a seconda del profilo di pubblicazione utilizzato.

## <a name="creating-transformation-files-for-publish-profiles"></a>La creazione di file di trasformazione per i profili di pubblicazione

In **Esplora**, espandere *Web. config* per visualizzare il *Web.Debug.config* e *Web.Release.config* i file di trasformazione vengono creati per impostazione predefinita per due configurazioni di compilazione.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

È possibile creare file di trasformazione per le configurazioni di compilazione personalizzata facendo clic il file Web. config e scegliendo **Aggiungi trasformazioni di configurazione** dal menu di scelta rapida, ma per questa esercitazione non è necessario eseguire questa operazione.

Due ulteriori file di trasformazione, è necessario per la configurazione di modifiche che sono correlate alla destinazione di distribuzione anziché alla configurazione di compilazione. Un esempio tipico di questo tipo di impostazione è un endpoint WCF è diverso per i test e produzione. Nelle esercitazioni successive, verrà creata la pubblicazione profili denominati Test e produzione, pertanto è necessario un *Web.Test.config* file e un *Web.Production.config* file.

File di trasformazione che sono collegati per i profili di pubblicazione devono essere creati manualmente. In **Esplora**, fare clic sul progetto ContosoUniversity e selezionare **Apri cartella in Esplora risorse**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

In **Esplora**, selezionare il *Web.Release.config* file, copiare il file e quindi incollare due copie di esso. Rinominare queste copie *Web.Production.config* e *Web.Test.config*, quindi chiudere **Esplora**.

In **Esplora**, fare clic su **aggiornamento** per visualizzare i nuovi file.

Selezionare il nuovo file, pulsante destro del mouse e quindi fare clic su **Includi nel progetto** nel menu di scelta rapida.

![Inclusi i file di configurazione di Test e produzione nel progetto](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Per impedire la distribuzione di questi file, selezionarli in **Esplora**e quindi il **proprietà** Modifica finestra il **azione di compilazione** proprietà **Contenuto** a **Nessuno**. (I file di trasformazione che si basano sulle configurazioni della build sono in grado di distribuzione).

A questo punto si è pronti a immettere *Web. config* le trasformazioni nel *Web. config* i file di trasformazione.

## <a name="limiting-error-log-access-to-administrators"></a>Limitare l'accesso di Log di errore per gli amministratori

Se si verifica un errore durante l'applicazione viene eseguita, l'applicazione viene visualizzata una pagina di errore generico al posto di pagina di errore generati dal sistema e Usa [pacchetto Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) registrazione e segnalazione degli errori. Il `customErrors` elemento il *Web. config* file specifica la pagina di errore:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Per visualizzare la pagina di errore, modificare temporaneamente il `mode` attributo del `customErrors` elemento da "RemoteOnly" su "On" ed eseguire l'applicazione da Visual Studio. Generare un errore richiedendo un URL non valido, ad esempio *Studentsxxx.aspx*. Invece di una pagina di errore generati a IIS "pagina non trovata", vengono visualizzati il *GenericErrorPage* pagina.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Per visualizzare il log degli errori, sostituire tutto il contenuto nell'URL dopo il numero di porta con *elmah.axd* (ad esempio nella cattura di schermata, `http://localhost:51130/elmah.axd`) e premere INVIO:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Non dimenticare di impostare il `customErrors` elemento tornare alla modalità "RemoteOnly" al termine.

Nel computer di sviluppo è conveniente consentire l'accesso gratuito per la pagina di log degli errori, ma nell'ambiente di produzione che sarebbe un rischio per la sicurezza. Per il sito di produzione, è possibile aggiungere una regola di autorizzazione che limita l'accesso di log di errore solo per gli amministratori tramite la configurazione di una trasformazione nel *Web.Production.config* file.

Aprire *Web.Production.config* e aggiungere un nuovo `location` elemento subito dopo l'apertura `configuration` tag, come illustrato di seguito. (Assicurarsi di aggiungere solo il `location` elemento e non il codice circostante che viene visualizzato qui solo per fornire il contesto.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

Il `Transform` valore attributo "Insert" fa sì che questo `location` elemento da aggiungere come un elemento di pari livello per tutte le classi esistenti `location` gli elementi di *Web. config* file. (È già presente uno `location` le regole di elemento che specifica l'autorizzazione per la **aggiornamento crediti** pagina.) Quando si testa il sito di produzione dopo la distribuzione, verrà testato per verificare che questa regola di autorizzazione è efficace.

Non è necessario limitare l'accesso di log di errore nell'ambiente di test, in modo non è necessario aggiungere questo codice per il *Web.Test.config* file.

> [!NOTE] 
> 
> **Nota sulla sicurezza** mai visualizzare i dettagli dell'errore al pubblico in un'applicazione di produzione o archiviare le informazioni in una posizione pubblica. Gli utenti malintenzionati possono utilizzare informazioni sugli errori per individuare le vulnerabilità in un sito. Se si utilizza ELMAH nella propria applicazione, assicurarsi di esaminare i modi in cui ELMAH possono essere configurati per ridurre al minimo i rischi di sicurezza. Nell'esempio ELMAH in questa esercitazione non deve essere considerata una configurazione consigliata. È un esempio in cui è stato scelto per illustrare come gestire una cartella che l'applicazione deve essere in grado di creare i file.


## <a name="setting-an-environment-indicator"></a>L'impostazione di un indicatore di ambiente

Uno scenario comune consiste nell'avere *Web. config* impostazioni che devono essere diverse in ogni ambiente distribuito ai file. Ad esempio, un'applicazione che chiama un servizio WCF potrebbe essere necessario un endpoint diverso in ambienti di test e produzione. L'applicazione Contoso University include anche un'impostazione di questo tipo. Questa impostazione controlla un indicatore visibile nelle pagine di un sito che informa l'ambiente in uso, ad esempio sviluppo, test o produzione. Il valore dell'impostazione determina se l'applicazione verrà aggiunto "(Dev)" o "(Test)" per l'intestazione principale nella *Site. master* pagina master:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

L'indicatore di ambiente viene omessa quando l'applicazione è in esecuzione nell'ambiente di produzione.

Le pagine web di Contoso University leggere un valore che viene impostato in `appSettings` nel *Web. config* file per determinare quale ambiente in cui è in esecuzione l'applicazione:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Il valore deve essere "Test" nell'ambiente di test e "Produzione" nell'ambiente di produzione.

Aprire *Web.Production.config* e aggiungere un `appSettings` elemento immediatamente precedente al tag di apertura del `location` elemento aggiunto in precedenza:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

Il `xdt:Transform` "SetAttributes" indica che lo scopo di questa trasformazione per modificare i valori di attributo di un elemento esistente nel valore dell'attributo di *Web. config* file. Il `xdt:Locator` valore "Match(key)" indica che l'elemento da modificare è quello dell'attributo il cui `key` attributo corrispondenze di `key` attributo specificato qui. L'unico altro attributo del `add` elemento `value`, e che cosa verrà modificata in distribuito *Web. config* file. Questo codice causa il `value` attributo del `Environment` `appSettings` elemento da impostare su "Produzione" il *Web. config* file che viene distribuito nell'ambiente di produzione.

Successivamente, applicare la stessa modifica *Web.Test.config* file, ad eccezione di set di `value` "Test" anziché "Produzione". Al termine, il `appSettings` sezione *Web.Test.config* avrà un aspetto analogo al seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Disabilitare la modalità di Debug

Per una build di rilascio, non si debug abilitato indipendentemente dall'ambiente che si sta distribuendo in. Per impostazione predefinita il *Web.Release.config* file di trasformazione viene creato automaticamente con il codice che consente di rimuovere il `debug` dall'attributo di `compilation` elemento:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

Il `Transform` attributo causa il `debug` omissione dell'attributo da distribuito *Web. config* file ogni volta che si distribuisce una build di rilascio.

Questa trasformazione stesso è in produzione di Test e i file di trasformazione perché sono creati copiando il file di trasformazione rilascio. Non è necessario duplicato non esiste, aprire ognuno di questi file, rimuovere il **compilazione** elemento e salvare e chiudere ciascun file.

## <a name="setting-connection-strings"></a>Impostare le stringhe di connessione

Nella maggior parte dei casi non è necessario impostare le trasformazioni di stringhe di connessione, poiché è possibile specificare le stringhe di connessione nel profilo di pubblicazione. Ma si verifica un'eccezione quando si distribuisce un database di SQL Server Compact e si utilizza Entity Framework migrazioni Code First per aggiornare il database nel server di destinazione. Per questo scenario, è necessario specificare una stringa di connessione aggiuntive che verrà utilizzata per l'aggiornamento dello schema del database nel server. Per configurare questa trasformazione, aggiungere un **&lt;connectionStrings&gt;** elemento subito dopo l'apertura **&lt;configurazione&gt;** tag sia in il *Web.Test.config* e *Web.Production.config* trasformare file:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

Il `Transform` attributo specifica che verrà aggiunto a questa stringa di connessione di *connectionStrings* elemento distribuito *Web. config* file. (Il processo di pubblicazione crea automaticamente la stringa di connessione aggiuntive per l'utente se non esiste, ma per impostazione predefinita il **providerName** attributo viene impostato su `System.Data.SqlClient`, che non funziona, non per SQL Server Compact. Aggiungere manualmente la stringa di connessione, mantenere il processo di distribuzione dalla creazione di un elemento di stringa di connessione con il nome del provider errata.)

È stato specificato a questo punto tutti i *Web. config* trasformazioni necessarie per la distribuzione dell'applicazione Contoso University per eseguire il test e produzione. Nell'esercitazione seguente verranno definite delle attività di configurazione di distribuzione che richiede l'impostazione della proprietà del progetto.

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti rientrano in questa esercitazione, vedere lo scenario di trasformazione Web. config in [mappa del contenuto di distribuzione di ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
