---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: trasformazioni del File Web. config - 3 pari a 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: b9f8213920e9e69885021666143612ce7c542b69
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388076"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: trasformazioni del File Web. config - 3 pari a 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010. Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come automatizzare il processo di modifica il *Web. config* file quando viene distribuito in ambienti di destinazione diversi. La maggior parte delle applicazioni hanno impostazioni *Web. config* file che deve essere diversi quando l'applicazione viene distribuita. Automatizzazione del processo di queste modifiche non è necessario completarli manualmente ogni volta che si distribuisce, che potrebbe essere noiosa e soggetta a errori.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>I parametri di distribuzione di trasformazioni di Web. config e Web

Esistono due modi per automatizzare il processo di modifica *Web. config* le impostazioni del file: [trasformazioni di Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parametri di distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx). Oggetto *Web. config* file di trasformazione contiene markup XML che specifica come modificare le *Web. config* file quando viene distribuito. È possibile specificare diverse modifiche per specifiche le configurazioni della build e per specifici profili di pubblicazione. Le configurazioni di compilazione predefinite sono di Debug e rilascio ed è possibile creare configurazioni di compilazione personalizzata. Un profilo di pubblicazione è in genere corrisponde a un ambiente di destinazione. (Si apprenderà a ulteriori informazioni su pubblicare profili nel [distribuzione in IIS come ambiente di Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) esercitazione.)

Parametri di distribuzione Web possono essere utilizzati per specificare molti tipi diversi di impostazioni che devono essere configurati durante la distribuzione, incluse le impostazioni che si trovano nella *Web. config* file. Quando viene utilizzata per specificare *Web. config* le modifiche ai file, i parametri di distribuzione Web sono più complessi da configurare, ma sono utili quando non si conosce il valore da impostare fino a quando non si distribuisce. Ad esempio, in un ambiente aziendale, è possibile creare un *pacchetto di distribuzione* e assegnare a un utente nel reparto IT per l'installazione nell'ambiente di produzione e tale persona deve essere in grado di immettere le stringhe di connessione o le password non necessari conoscere.

Per questa esercitazione illustra lo scenario, si conosce tutto ciò che deve essere eseguita per il *Web. config* file, in modo che non è necessario usare i parametri di distribuzione Web. Si configurerà alcune trasformazioni che differiscono a seconda della configurazione di compilazione utilizzata e altre che variano a seconda del profilo di pubblicazione usato.

## <a name="creating-transformation-files-for-publish-profiles"></a>Creazione di file di trasformazione per i profili di pubblicazione

Nelle **Esplora soluzioni**, espandere *Web. config* per visualizzare il *debug* e *Release* i file di trasformazione vengono creati per impostazione predefinita per le configurazioni della build due predefinita.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

È possibile creare i file di trasformazione per configurazioni della build personalizzata facendo clic il file Web. config e scegliendo **Aggiungi trasformazioni di configurazione** dal menu di scelta rapida, ma per questa esercitazione non è necessario eseguire questa operazione.

Due ulteriori file di trasformazione, è necessario per la configurazione di modifiche che sono correlate alla destinazione della distribuzione invece che alla configurazione della build. Un esempio tipico di questo tipo di impostazione è un endpoint WCF è diverso per test o produzione. Nelle esercitazioni successive si creerà pubblicare profili denominati Test e produzione, pertanto è necessario un *Web.Test.config* file e un *Web.Production.config* file.

I file di trasformazione che sono associati ai profili di pubblicazione devono essere creati manualmente. Nelle **Esplora soluzioni**, fare clic sul progetto ContosoUniversity e selezionare **Apri cartella in Esplora Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

Nelle **Windows Explorer**, selezionare la *Release* file, copiare il file e quindi incollare due copie di esso. Rinominare queste copie *Web.Production.config* e *Web.Test.config*, quindi chiudere **Windows Explorer**.

Nelle **Esplora soluzioni**, fare clic su **aggiornare** per visualizzare i nuovi file.

Selezionare il nuovo file, pulsante destro del mouse e quindi fare clic su **Includi nel progetto** nel menu di scelta rapida.

![Inclusi i file di configurazione di Test e produzione nel progetto](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Per impedire che tali file vengano distribuiti, selezionarli in **Esplora soluzioni**e quindi il **proprietà** modificano il **azione di compilazione** proprietà da **Content** alla **None**. (I file di trasformazione che si basano sulle configurazioni della build sono in grado di distribuzione).

Ora si è pronti a immettere *Web. config* le trasformazioni nel *Web. config* i file di trasformazione.

## <a name="limiting-error-log-access-to-administrators"></a>Limitare l'accesso del registro errori per gli amministratori

Se si verifica un errore durante l'esecuzione dell'applicazione, l'applicazione viene visualizzata una pagina di errore generico anziché la pagina di errore generati dal sistema e utilizza [pacchetto Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) per la registrazione e segnalazione errori. Il `customErrors` elemento il *Web. config* file specifica la pagina di errore:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Per visualizzare la pagina di errore, modificare temporaneamente il `mode` attributo del `customErrors` elemento da "RemoteOnly" su "Sì" ed eseguire l'applicazione da Visual Studio. Causa un errore richiedendo un URL non valido, ad esempio *Studentsxxx.aspx*. Invece di una pagina di errore generati IIS "pagina non trovata", vengono visualizzati i *GenericErrorPage* pagina.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Per visualizzare il log degli errori, sostituire tutto il contenuto nell'URL dopo il numero di porta *elmah.axd* (per l'esempio nella schermata, `http://localhost:51130/elmah.axd`) e premere INVIO:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Non dimenticare di impostare il `customErrors` elemento torna alla modalità "RemoteOnly" al termine.

Nel computer di sviluppo è consigliabile consentire l'accesso gratuito alla pagina log di errore, ma nell'ambiente di produzione che sarebbe un rischio per la sicurezza. Per il sito di produzione, è possibile aggiungere una regola di autorizzazione che limita l'accesso ai registri errori solo per gli amministratori configurando una trasformazione nel *Web.Production.config* file.

Aprire *Web.Production.config* e aggiungere un nuovo `location` elemento subito dopo l'apertura `configuration` tag, come illustrato di seguito. (Assicurarsi di aggiungere solo il `location` elemento e non la markup circostante illustrata di seguito solo a fornire il contesto.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

Il `Transform` valore di attributo di "Insert", ciò `location` elemento da aggiungere come elemento di pari livello a tutte le classi esistenti `location` elementi nel *Web. config* file. (È già presente uno `location` regole di elemento che specifica l'autorizzazione per il **crediti Update** pagina.) Quando si testa il sito di produzione dopo la distribuzione, è possibile testare per verificare che questa regola di autorizzazione.

Non è necessario limitare l'accesso ai registri errori nell'ambiente di test, quindi non è necessario aggiungere questo codice per il *Web.Test.config* file.

> [!NOTE] 
> 
> **Nota sulla sicurezza** mai visualizzare i dettagli dell'errore per il pubblico in un'applicazione di produzione o memorizzare tali informazioni in una posizione pubblica. Gli utenti malintenzionati possono usare le informazioni sugli errori per individuare le vulnerabilità in un sito. Se si usa ELMAH nella propria applicazione, assicurarsi di esaminare i modi in cui ELMAH possono essere configurati per ridurre al minimo i rischi di sicurezza. L'esempio ELMAH in questa esercitazione non deve essere considerato una configurazione consigliata. È un esempio in cui è stato scelto per illustrare come gestire una cartella che l'applicazione deve essere in grado di creare i file.


## <a name="setting-an-environment-indicator"></a>L'impostazione di un indicatore di ambiente

Uno scenario comune consiste nel disporre *Web. config* impostazioni che devono essere diverse in ogni ambiente in cui si distribuisce in file. Ad esempio, un'applicazione che chiama un servizio WCF potrebbe essere necessario un diverso endpoint in ambienti di test e produzione. L'applicazione di Contoso University include anche un'impostazione di questo tipo. Questa impostazione controlla un indicatore visibile nelle pagine di un sito indicante che l'ambiente in uso, ad esempio sviluppo, test o produzione. Il valore dell'impostazione determina se l'applicazione verrà aggiunto "(Dev)" o "(Test)" al titolo principale nel *Site. master* pagina master:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

L'indicatore di ambiente viene omessa quando l'applicazione è in esecuzione nell'ambiente di produzione.

Le pagine web di Contoso University leggere un valore che viene impostato in `appSettings` nella *Web. config* file per determinare qual è l'ambiente in cui viene eseguita l'applicazione:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Il valore deve essere "Test" nell'ambiente di test e "Prod" nell'ambiente di produzione.

Aprire *Web.Production.config* e aggiungere un' `appSettings` immediatamente prima del tag di apertura dell'elemento di `location` elemento aggiunto in precedenza:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

Il `xdt:Transform` "SetAttributes" indica che lo scopo di questa trasformazione per modificare i valori di attributo di un elemento esistente nel valore dell'attributo di *Web. config* file. Il `xdt:Locator` "Match(key)" indica che l'elemento da modificare è quella di valore dell'attributo la cui `key` corrispondenze dell'attributo di `key` attributo specificato qui. Il solo altro attributo del `add` elemento viene `value`, e questo è ciò che verrà modificato in distribuito *Web. config* file. Questo codice fa in modo che il `value` attributo del `Environment` `appSettings` elemento da impostare su "Prod" nel *Web. config* file che viene distribuita nell'ambiente di produzione.

Successivamente, applicare la stessa modifica *Web.Test.config* file, ad eccezione di set di `value` "Test" anziché "Prod". Al termine, il `appSettings` nella sezione *Web.Test.config* avrà un aspetto simile al seguente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Disattivazione della modalità di Debug

Per una build di rilascio, non si desidera debug abilitato indipendentemente dall'ambiente viene eseguita la distribuzione. Per impostazione predefinita il *Release* file di trasformazione viene creato automaticamente con il codice che rimuove le `debug` dell'attributo dal `compilation` elemento:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

Il `Transform` attributo causa il `debug` omissione dell'attributo da distribuito *Web. config* file ogni volta che si distribuisce una build di rilascio.

Questa trasformazione stessa è nel Test e produzione file di trasformazione in quanto sono creati copiando il file di trasformazione rilascio. Non è necessario duplicato non esiste, aprire ognuno di questi file, rimuovere il **compilazione** elemento e salvare e chiudere ciascun file.

## <a name="setting-connection-strings"></a>Impostazione delle stringhe di connessione

Nella maggior parte dei casi non occorre configurare le trasformazioni di stringa di connessione, perché è possibile specificare le stringhe di connessione nel profilo di pubblicazione. Ma si verifica un'eccezione quando si distribuisce un database di SQL Server Compact e si usa migrazioni di Entity Framework Code First per aggiornare il database nel server di destinazione. Per questo scenario, è necessario specificare una stringa di connessione aggiuntive che verrà usata nel server per l'aggiornamento dello schema del database. Per configurare questa trasformazione, aggiungere un **&lt;connectionStrings&gt;** elemento subito dopo l'apertura **&lt;configuration&gt;** tag in entrambi il *Web.Test.config* e il *Web.Production.config* trasformare file:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

Il `Transform` attributo specifica che la stringa di connessione verrà aggiunte per il *connectionStrings* elemento distribuito *Web. config* file. (Il processo di pubblicazione crea automaticamente questa stringa di connessione aggiuntive per l'utente se non esiste, ma per impostazione predefinita il **providerName** attributo viene impostato su `System.Data.SqlClient`, che non funziona, non per SQL Server Compact. Quando si aggiungono manualmente la stringa di connessione, è mantenere il processo di distribuzione dalla creazione di un elemento di stringa di connessione con il nome del provider errata.)

Ora avere specificato tutte le *Web. config* trasformazioni necessarie per la distribuzione dell'applicazione di Contoso University per eseguire il test e produzione. Nell'esercitazione seguente, sarà occuparsi delle attività di configurazione di distribuzione che richiedono l'impostazione di proprietà del progetto.

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere lo scenario di trasformazione Web. config in [mappa del contenuto ASP.NET distribuzione](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Precedente](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
