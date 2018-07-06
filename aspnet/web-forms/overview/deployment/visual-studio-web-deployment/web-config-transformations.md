---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Distribuzione Web ASP.NET tramite Visual Studio: trasformazioni del File Web. config | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 0a52530396efa3612d19817eeaa0498cffdac38f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842439"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Distribuzione Web ASP.NET tramite Visual Studio: trasformazioni del File Web. config
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

Questa esercitazione illustra come automatizzare il processo di modifica il *Web. config* file quando viene distribuito in ambienti di destinazione diversi. La maggior parte delle applicazioni hanno impostazioni *Web. config* file che deve essere diversi quando l'applicazione viene distribuita. Automatizzazione del processo di queste modifiche non è necessario completarli manualmente ogni volta che si distribuisce, che potrebbe essere noiosa e soggetta a errori.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Trasformazioni di Web. config e i parametri di distribuzione Web

Esistono due modi per automatizzare il processo di modifica *Web. config* le impostazioni del file: [trasformazioni di Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parametri di distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx). Oggetto *Web. config* file di trasformazione contiene markup XML che specifica come modificare le *Web. config* file quando viene distribuito. È possibile specificare diverse modifiche per specifiche le configurazioni della build e per specifici profili di pubblicazione. Le configurazioni di compilazione predefinite sono di Debug e rilascio ed è possibile creare configurazioni di compilazione personalizzata. Un profilo di pubblicazione è in genere corrisponde a un ambiente di destinazione. (Si apprenderà a ulteriori informazioni su pubblicare profili nel [distribuzione in IIS come ambiente di Test](deploying-to-iis.md) esercitazione.)

Parametri di distribuzione Web possono essere utilizzati per specificare molti tipi diversi di impostazioni che devono essere configurati durante la distribuzione, incluse le impostazioni che si trovano nella *Web. config* file. Quando viene utilizzata per specificare *Web. config* le modifiche ai file, i parametri di distribuzione Web sono più complessi da configurare, ma sono utili quando non si conosce il valore da impostare fino a quando non si distribuisce. Ad esempio, in un ambiente aziendale, è possibile creare un *pacchetto di distribuzione* e assegnare a un utente nel reparto IT per l'installazione nell'ambiente di produzione e tale persona deve essere in grado di immettere le stringhe di connessione o le password non necessari conoscere.

Per lo scenario che descrive questa serie di esercitazioni, si conoscono in anticipo tutto ciò che deve essere eseguita per il *Web. config* file, in modo che non è necessario usare i parametri di distribuzione Web. Si configurerà alcune trasformazioni che differiscono a seconda della configurazione di compilazione utilizzata e altre che variano a seconda del profilo di pubblicazione usato.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Specifica le impostazioni di Web. config in Azure

Se il *Web. config* inclusi tra le impostazioni di file che si desidera modificare le `<connectionStrings>` o il `<appSettings>` elemento, e se si distribuisce per le app Web nel servizio App di Azure, si dispone di un'altra opzione per automatizzare le modifiche apportate durante distribuzione. È possibile immettere le impostazioni che si desidera rendere effettiva in Azure nel **configura** della scheda della pagina del portale di gestione per l'app web (scorrere verso il basso il **le impostazioni dell'app** e **le stringhe di connessione**  sezioni). Quando si distribuisce il progetto, Azure applica automaticamente le modifiche. Per altre informazioni, vedere [siti Web di Azure: How Application Strings and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>File di trasformazione predefinito

Nelle **Esplora soluzioni**, espandere *Web. config* per visualizzare il *debug* e *Release* i file di trasformazione vengono creati per impostazione predefinita per le configurazioni della build due predefinita.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

È possibile creare i file di trasformazione per configurazioni della build personalizzata facendo clic il file Web. config e scegliendo **Aggiungi trasformazioni di configurazione** dal menu di scelta rapida. Per questa esercitazione non è necessario eseguire questa operazione e l'opzione di menu è disabilitata, perché si sono mai sono create tutte le configurazioni di compilazione personalizzata.

In seguito si creerà tre file di trasformazione altre, una per ogni test, staging e produzione di profili di pubblicazione. Un esempio tipico di un'impostazione che è necessario gestire in un file di trasformazione del profilo di pubblicazione perché dipende da ambiente di destinazione è un endpoint WCF è diverso per test o produzione. Si creerà pubblica file di trasformazione del profilo in esercitazioni successive dopo aver creato i profili di pubblicazione che escono con.

## <a name="disable-debug-mode"></a>Disabilitare la modalità di debug

Un esempio di un'impostazione che dipende dalla configurazione di compilazione anziché l'ambiente di destinazione è il `debug` attributo. Per una build di rilascio in genere consigliabile debug disabilitato indipendentemente dall'ambiente viene eseguita la distribuzione. Pertanto, per impostazione predefinita di Visual Studio creano modelli di progetto *Release* trasformare file con il codice che rimuove le `debug` dell'attributo dal `compilation` elemento. Ecco il valore predefinito *Release*: oltre a un codice di trasformazione di esempio che viene impostata come commento, include il codice nelle `compilation` elemento che rimuove il `debug` attributo:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

Il `xdt:Transform="RemoveAttributes(debug)"` attributo specifica che il `debug` attributo da rimuovere dal `system.web/compilation` elemento distribuito *Web. config* file. Questa operazione verrà eseguita ogni volta che si distribuisce una build di rilascio.

## <a name="limit-error-log-access-to-administrators"></a>Limitare l'accesso ai registri di errore per gli amministratori

Se si verifica un errore durante l'esecuzione dell'applicazione, l'applicazione viene visualizzata una pagina di errore generico anziché la pagina di errore generati dal sistema e Usa il [pacchetto Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) per la registrazione e segnalazione errori. Il `customErrors` elemento dell'applicazione *Web. config* file specifica la pagina di errore:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Per visualizzare la pagina di errore, modificare temporaneamente il `mode` attributo del `customErrors` elemento da "RemoteOnly" su "Sì" ed eseguire l'applicazione da Visual Studio. Causa un errore richiedendo un URL non valido, ad esempio *Studentsxxx.aspx*. Invece di un errore generato IIS "della risorsa non è stata trovata" pagina, viene visualizzato il *GenericErrorPage* pagina.

![Pagina di errore](web-config-transformations/_static/image2.png)

Per visualizzare il log degli errori, sostituire tutto il contenuto nell'URL dopo il numero di porta *elmah.axd* (ad esempio, `http://localhost:51130/elmah.axd`) e premere INVIO:

![Pagina ELMAH](web-config-transformations/_static/image3.png)

Non dimenticare di impostare il `customErrors` elemento torna alla modalità "RemoteOnly" al termine.

Nel computer di sviluppo è consigliabile consentire l'accesso gratuito alla pagina log di errore, ma nell'ambiente di produzione che sarebbe un rischio per la sicurezza. Per il sito di produzione, si desidera aggiungere una regola di autorizzazione che limita l'accesso ai registri di errore per gli amministratori e assicurarsi che la limitazione funziona si desidera che nei test e gestione temporanea anche. Pertanto si tratta di un'altra modifica che si desidera implementare ogni volta che si distribuisce una build di rilascio e, in modo che fa parte di *Release* file.

Aprire *Release* e aggiungere un nuovo `location` elemento immediatamente prima della chiusura `configuration` tag, come illustrato di seguito.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

Il `Transform` valore di attributo di "Insert", ciò `location` elemento da aggiungere come elemento di pari livello a tutte le classi esistenti `location` elementi nel *Web. config* file. (È già presente uno `location` regole di elemento che specifica l'autorizzazione per il **crediti Update** pagina.)

A questo punto è possibile visualizzare in anteprima la trasformazione per assicurarsi che codificato viene correttamente.

Nelle **Esplora soluzioni**, fare doppio clic su *Release* e fare clic su **anteprima trasformare**.

![Menu Anteprima di trasformazione](web-config-transformations/_static/image4.png)

Verrà visualizzata una pagina che illustra lo sviluppo *Web. config* sulla sinistra e quali file distribuito *Web. config* file sarà simile a destra, con evidenziate le modifiche.

![Anteprima della trasformazione di debug](web-config-transformations/_static/image5.png)

![Anteprima della trasformazione di posizione](web-config-transformations/_static/image6.png)

(In anteprima, è possibile notare alcune modifiche aggiuntive che non scritto dallo sviluppatore Trasforma per: in genere che implicano la rimozione di spazi vuoti che non influisce sulla funzionalità.)

Quando si testa il sito dopo la distribuzione, è possibile anche testare per verificare che la regola di autorizzazione è valida.

> [!NOTE] 
> 
> **Nota sulla sicurezza** mai visualizzare i dettagli dell'errore per il pubblico in un'applicazione di produzione o memorizzare tali informazioni in una posizione pubblica. Gli utenti malintenzionati possono usare le informazioni sugli errori per individuare le vulnerabilità in un sito. Se si usa ELMAH nella propria applicazione, configurare ELMAH per ridurre al minimo i rischi di sicurezza. L'esempio ELMAH in questa esercitazione non deve essere considerato una configurazione consigliata. È un esempio in cui è stato scelto per illustrare come gestire una cartella che l'applicazione deve essere in grado di creare i file. Per altre informazioni, vedere [proteggere l'endpoint ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un'impostazione che è possibile gestire in file di trasformazione del profilo di pubblicazione

Uno scenario comune consiste nel disporre *Web. config* impostazioni che devono essere diverse in ogni ambiente in cui si distribuisce in file. Ad esempio, un'applicazione che chiama un servizio WCF potrebbe essere necessario un diverso endpoint in ambienti di test e produzione. L'applicazione di Contoso University include anche un'impostazione di questo tipo. Questa impostazione controlla un indicatore visibile nelle pagine di un sito indicante che l'ambiente in uso, ad esempio sviluppo, test o produzione. Il valore dell'impostazione determina se l'applicazione verrà aggiunto "(Dev)" o "(Test)" al titolo principale nel *Site. master* pagina master:

![Indicatore di ambiente](web-config-transformations/_static/image7.png)

L'indicatore di ambiente viene omessa quando l'applicazione è in esecuzione in gestione temporanea o produzione.

Le pagine web di Contoso University leggere un valore che viene impostato in `appSettings` nella *Web. config* file per determinare qual è l'ambiente in cui viene eseguita l'applicazione:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Il valore deve essere "Test" nell'ambiente di test e "Produzione" per lo staging e produzione.

Il codice seguente in un file di trasformazione verrà implementata questa trasformazione:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Il `xdt:Transform` "SetAttributes" indica che lo scopo di questa trasformazione per modificare i valori di attributo di un elemento esistente nel valore dell'attributo di *Web. config* file. Il `xdt:Locator` "Match(key)" indica che l'elemento da modificare è quella di valore dell'attributo la cui `key` corrispondenze dell'attributo di `key` attributo specificato qui. Il solo altro attributo del `add` elemento viene `value`, e questo è ciò che verrà modificato in distribuito *Web. config* file. Il codice riportato di seguito le cause di `value` attributo del `Environment` `appSettings` elemento da impostare su "Test" nel *Web. config* file che viene distribuito.

Questa trasformazione fa parte i file di trasformazione del profilo di pubblicazione, non è stata creata. Si verrà creato e aggiornare i file di trasformazione che implementano questa modifica, quando si creano i profili di pubblicazione per gli ambienti di test, staging e produzione. È necessario usare il [distribuzione in IIS](deploying-to-iis.md) e [distribuire nell'ambiente di produzione](deploying-to-production.md) esercitazioni.

> [!NOTE]
> Poiché questa impostazione si trova nel `<appSettings>` elemento, si dispone di un'altra alternativa per specificare la trasformazione quando si esegue la distribuzione delle App Web in Azure App Service, vedere [Web. config che specifica le impostazioni in Azure](#watransforms) nelle sezioni precedenti di In questo argomento.


## <a name="setting-connection-strings"></a>Impostazione delle stringhe di connessione

Anche se il file di trasformazione predefinito contiene un esempio che illustra come aggiornare una stringa di connessione, nella maggior parte dei casi non occorre configurare le trasformazioni di stringa di connessione, perché è possibile specificare le stringhe di connessione nel profilo di pubblicazione. È necessario usare il [distribuzione in IIS](deploying-to-iis.md) e [distribuire nell'ambiente di produzione](deploying-to-production.md) esercitazioni.

## <a name="summary"></a>Riepilogo

È ora ancora come puoi fare con *Web. config* trasformazioni prima creare i profili di pubblicazione e si è visto un'anteprima del quale sarà il file Web. config distribuito.

![Anteprima della trasformazione di posizione](web-config-transformations/_static/image8.png)

Nell'esercitazione seguente, sarà occuparsi delle attività di configurazione di distribuzione che richiedono l'impostazione di proprietà del progetto.

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere [trasformazioni tramite Web. config per modificare le impostazioni nel file Web. config di destinazione o nel file app. config durante la distribuzione](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) nella mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET.

> [!div class="step-by-step"]
> [Precedente](preparing-databases.md)
> [Successivo](project-properties.md)
