---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: le trasformazioni di Web. config File | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 77ed0d8b2fe85adb009a3f4759030b7fba8fb9d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880505"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: le trasformazioni di File Web. config
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come automatizzare il processo di modifica di *Web. config* file quando viene distribuito in ambienti di destinazione diversi. La maggior parte delle applicazioni avere impostazioni di *Web. config* file che deve essere diverse quando l'applicazione viene distribuita. Automatizzare il processo di queste modifiche evita di dover effettuare l'operazione manualmente ogni volta che si distribuisce, che sarebbe attività tediosa e soggetta a errori.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Trasformazioni di Web. config e i parametri di distribuzione Web

Esistono due modi per automatizzare il processo di modifica *Web. config* impostazioni del file: [trasformazioni di Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parametri distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx). A *Web. config* file di trasformazione contiene markup XML che specifica la modalità di modifica di *Web. config* file quando viene distribuito. È possibile specificare diverse modifiche per specifiche configurazioni di compilazione e profili di pubblicazione specifico. Le configurazioni di compilazione predefinite sono di Debug e rilascio ed è possibile creare configurazioni di compilazione personalizzata. Un profilo di pubblicazione è in genere corrisponde a un ambiente di destinazione. (Si apprenderà informazioni sulla pubblicazione dei profili nel [distribuzione in IIS come ambiente di Test](deploying-to-iis.md) esercitazione.)

Parametri di distribuzione Web consente di specificare diversi tipi di impostazioni che devono essere configurati durante la distribuzione, incluse le impostazioni che si trovano in *Web. config* file. Quando viene utilizzata per specificare *Web. config* modifiche al file, sono più complessi per impostare i parametri di distribuzione Web, ma sono utili quando non si conosce il valore da impostare finché non vengono distribuiti. In un ambiente aziendale, ad esempio, è possibile creare un *pacchetto di distribuzione* e assegnare a un utente del reparto IT per installare nell'ambiente di produzione e che l'utente deve essere in grado di immettere le stringhe di connessione o le password che non li conoscere.

Per lo scenario che illustra questa serie di esercitazioni, si conosce in anticipo tutto ciò che deve essere eseguita per il *Web. config* file, in modo non è necessario utilizzare i parametri di distribuzione Web. È possibile configurare alcune trasformazioni che differiscono a seconda della configurazione di compilazione utilizzata e altre che variano a seconda del profilo di pubblicazione utilizzato.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Specificare le impostazioni di Web. config in Azure

Se il *Web. config* presenti impostazioni del file che si desidera modificare il `<connectionStrings>` o `<appSettings>` elemento, e se si distribuiscono nelle App Web nel servizio App di Azure, si dispone di un'altra opzione per automatizzare le modifiche durante distribuzione. È possibile immettere le impostazioni che si desidera rendere effettive in Azure nel **configura** scheda della pagina del portale di gestione per l'app web (scorrere verso il basso il **impostazioni app** e **le stringhe di connessione**  sezioni). Quando si distribuisce il progetto, Azure applica automaticamente le modifiche. Per ulteriori informazioni, vedere [siti Web di Azure: come le stringhe dell'applicazione e lavoro di stringhe di connessione](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>File di trasformazione predefiniti

In **Esplora**, espandere *Web. config* per visualizzare il *Web.Debug.config* e *Web.Release.config* i file di trasformazione vengono creati per impostazione predefinita per due configurazioni di compilazione.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

È possibile creare file di trasformazione per le configurazioni di compilazione personalizzata facendo clic il file Web. config e scegliendo **Aggiungi trasformazioni di configurazione** dal menu di scelta rapida. Per questa esercitazione non è necessario eseguire questa operazione e l'opzione di menu è disabilitata, perché non sono state create tutte le configurazioni di compilazione personalizzata.

In un secondo momento si creeranno tre file di trasformazione altre, uno per il test, gestione temporanea e produzione di profili di pubblicazione. Un esempio tipico di un'impostazione che è necessario gestire in un file di trasformazione del profilo di pubblicazione, in quanto dipende dall'ambiente di destinazione è un endpoint WCF è diverso per i test e produzione. Si creeranno pubblica il file di trasformazione del profilo nelle esercitazioni successive dopo la creazione di profili di pubblicazione che si verificano con.

## <a name="disable-debug-mode"></a>Disabilitare la modalità di debug

Un esempio di un'impostazione che dipende dalla configurazione di compilazione anziché l'ambiente di destinazione è il `debug` attributo. Per una build di rilascio, in genere consigliabile debug disabilitato indipendentemente dall'ambiente che si sta distribuendo in. Pertanto, per impostazione predefinita di Visual Studio creano modelli di progetto *Web.Release.config* trasformare file con il codice che consente di rimuovere il `debug` dall'attributo di `compilation` elemento. Di seguito è il valore predefinito *Web.Release.config*: oltre a un codice di trasformazione di esempio che viene impostata come commento, include codice il `compilation` elemento che consente di rimuovere il `debug` attributo:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

Il `xdt:Transform="RemoveAttributes(debug)"` attributo specifica che si desidera il `debug` attributo da rimuovere dal `system.web/compilation` elemento distribuito *Web. config* file. Questa operazione viene eseguita ogni volta che si distribuisce una build di rilascio.

## <a name="limit-error-log-access-to-administrators"></a>Limitare l'accesso di log di errore per gli amministratori

Se si verifica un errore durante l'applicazione viene eseguita, l'applicazione viene visualizzata una pagina di errore generico al posto di pagina di errore generati dal sistema e viene utilizzato il [pacchetto Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) registrazione e segnalazione degli errori. Il `customErrors` elemento nell'applicazione *Web. config* file specifica la pagina di errore:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Per visualizzare la pagina di errore, modificare temporaneamente il `mode` attributo del `customErrors` elemento da "RemoteOnly" su "On" ed eseguire l'applicazione da Visual Studio. Generare un errore richiedendo un URL non valido, ad esempio *Studentsxxx.aspx*. Anziché un errore generato a IIS "trovare la risorsa non è" pagina, viene visualizzato il *GenericErrorPage* pagina.

![Pagina di errore](web-config-transformations/_static/image2.png)

Per visualizzare il log degli errori, sostituire tutto il contenuto nell'URL dopo il numero di porta con *elmah.axd* (ad esempio, `http://localhost:51130/elmah.axd`) e premere INVIO:

![Pagina ELMAH](web-config-transformations/_static/image3.png)

Non dimenticare di impostare il `customErrors` elemento tornare alla modalità "RemoteOnly" al termine.

Nel computer di sviluppo è conveniente consentire l'accesso gratuito per la pagina di log degli errori, ma nell'ambiente di produzione che sarebbe un rischio per la sicurezza. Per il sito di produzione, si desidera aggiungere una regola di autorizzazione che limita l'accesso ai registri errori agli amministratori e per assicurarsi che la restrizione funzioni si desidera che nei test e di gestione temporanea anche. Pertanto si tratta di un'altra modifica che si desidera implementare ogni volta che si distribuisce una build di rilascio e, in modo cui appartiene il *Web.Release.config* file.

Aprire *Web.Release.config* e aggiungere un nuovo `location` elemento immediatamente prima della chiusura `configuration` tag, come illustrato di seguito.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

Il `Transform` valore attributo "Insert" fa sì che questo `location` elemento da aggiungere come un elemento di pari livello per tutte le classi esistenti `location` gli elementi di *Web. config* file. (È già presente uno `location` le regole di elemento che specifica l'autorizzazione per la **aggiornamento crediti** pagina.)

Ora puoi visualizzare in anteprima la trasformazione per assicurarsi che codificato correttamente.

In **Esplora**, fare doppio clic su *Web.Release.config* e fare clic su **anteprima trasformare**.

![Trasformazione menu Anteprima](web-config-transformations/_static/image4.png)

Verrà visualizzata una pagina che illustra lo sviluppo *Web. config* file a sinistra e a cosa distribuito *Web. config* file avrà un aspetto simile a destra, con modifiche evidenziate.

![Anteprima della trasformazione di debug](web-config-transformations/_static/image5.png)

![Anteprima della trasformazione di percorso](web-config-transformations/_static/image6.png)

(In anteprima, è possibile notare alcune modifiche aggiuntive scritto Trasforma per: in genere che implicano la rimozione di spazi vuoti che non influisce sulla funzionalità.)

Quando si testa il sito dopo la distribuzione, verrà testato anche per verificare che la regola di autorizzazione è efficace.

> [!NOTE] 
> 
> **Nota sulla sicurezza** mai visualizzare i dettagli dell'errore al pubblico in un'applicazione di produzione o archiviare le informazioni in una posizione pubblica. Gli utenti malintenzionati possono utilizzare informazioni sugli errori per individuare le vulnerabilità in un sito. Se si utilizza ELMAH nella propria applicazione, è possibile configurare ELMAH per ridurre al minimo i rischi di sicurezza. Nell'esempio ELMAH in questa esercitazione non deve essere considerata una configurazione consigliata. È un esempio in cui è stato scelto per illustrare come gestire una cartella che l'applicazione deve essere in grado di creare i file. Per ulteriori informazioni, vedere [proteggere l'endpoint ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un'impostazione di gestire in file di trasformazione del profilo di pubblicazione

Uno scenario comune consiste nell'avere *Web. config* impostazioni che devono essere diverse in ogni ambiente distribuito ai file. Ad esempio, un'applicazione che chiama un servizio WCF potrebbe essere necessario un endpoint diverso in ambienti di test e produzione. L'applicazione Contoso University include anche un'impostazione di questo tipo. Questa impostazione controlla un indicatore visibile nelle pagine di un sito che informa l'ambiente in uso, ad esempio sviluppo, test o produzione. Il valore dell'impostazione determina se l'applicazione verrà aggiunto "(Dev)" o "(Test)" per l'intestazione principale nella *Site. master* pagina master:

![Indicatore di ambiente](web-config-transformations/_static/image7.png)

L'indicatore di ambiente viene omessa quando l'applicazione è in esecuzione in gestione temporanea o produzione.

Le pagine web di Contoso University leggere un valore che viene impostato in `appSettings` nel *Web. config* file per determinare quale ambiente in cui è in esecuzione l'applicazione:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Il valore deve essere "Test" nell'ambiente di test e "Produzione" per la gestione temporanea e produzione.

Il codice seguente in un file di trasformazione implementerà questa trasformazione:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Il `xdt:Transform` "SetAttributes" indica che lo scopo di questa trasformazione per modificare i valori di attributo di un elemento esistente nel valore dell'attributo di *Web. config* file. Il `xdt:Locator` valore "Match(key)" indica che l'elemento da modificare è quello dell'attributo il cui `key` attributo corrispondenze di `key` attributo specificato qui. L'unico altro attributo del `add` elemento `value`, e che cosa verrà modificata in distribuito *Web. config* file. Il codice riportato di seguito le cause di `value` attributo del `Environment` `appSettings` elemento sia impostato su "Test" *Web. config* file distribuito.

Questa trasformazione fa parte i file di trasformazione del profilo di pubblicazione, non è stata creata. Viene creata e aggiornare i file di trasformazione che implementano questa modifica durante la creazione di profili di pubblicazione per gli ambienti di test, gestione temporanea e produzione. Che verranno eseguite [distribuire a IIS](deploying-to-iis.md) e [distribuire nell'ambiente di produzione](deploying-to-production.md) esercitazioni.

> [!NOTE]
> Poiché questa impostazione è la `<appSettings>` elemento, si dispone di un'altra alternativa per specificare la trasformazione quando si distribuisce in App Web nel servizio App di Azure vedere [config che specifica le impostazioni in Azure](#watransforms) più indietro in In questo argomento.


## <a name="setting-connection-strings"></a>Impostare le stringhe di connessione

Anche se il file di trasformazione predefinito contiene un esempio che illustra come aggiornare una stringa di connessione, nella maggior parte dei casi non occorre configurare le trasformazioni di stringa di connessione, poiché è possibile specificare le stringhe di connessione nel profilo di pubblicazione. Che verranno eseguite [distribuire a IIS](deploying-to-iis.md) e [distribuire nell'ambiente di produzione](deploying-to-production.md) esercitazioni.

## <a name="summary"></a>Riepilogo

È ora stata eseguita in quanto è possibile con *Web. config* trasformazioni prima creare profili di pubblicazione e visualizzata un'anteprima dei futuri nel file Web. config distribuito.

![Anteprima della trasformazione di percorso](web-config-transformations/_static/image8.png)

Nell'esercitazione seguente verranno definite delle attività di configurazione di distribuzione che richiede l'impostazione della proprietà del progetto.

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti rientrano in questa esercitazione, vedere [trasformazioni tramite Web. config per modificare le impostazioni nel file Web. config di destinazione o nel file app. config durante la distribuzione](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) nella mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET.

> [!div class="step-by-step"]
> [Precedente](preparing-databases.md)
> [Successivo](project-properties.md)
