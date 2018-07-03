---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Informazioni su autenticazione basata su AJAX ASP.NET e servizi dell'applicazione del profilo | Microsoft Docs
author: scottcate
description: Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione e il servizio gateway per consentire a un utente personalizzato...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 323fec56f18281b5b5a3d312a2e4c4c7133e3f03
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393061"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Informazioni su autenticazione basata su AJAX ASP.NET e servizi dell'applicazione di profilo
====================
da [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione ed è il servizio gateway per consentire i profili utente personalizzati fornito da ASP.NET. Uso del servizio di autenticazione di ASP.NET AJAX è compatibile con autenticazione basata su form ASP.NET standard, in modo che le applicazioni attualmente in uso autenticazione basata su form (ad esempio con l'account di accesso di controllo) verranno non interrotte eseguendo l'aggiornamento al servizio di autenticazione AJAX.


## <a name="introduction"></a>Introduzione

Come parte di .NET Framework 3.5, Microsoft ha pubblicato un aggiornamento ambiente ridimensionabili; non solo è disponibile un nuovo ambiente di sviluppo, ma le nuove funzionalità di Language-Integrated Query (LINQ) e altri miglioramenti del linguaggio saranno presto disponibili. Inoltre, alcune funzionalità tipiche di altri set di strumenti, in particolare in ASP.NET AJAX Extensions, sono in corso inclusi come componenti essenziali della libreria di classi .NET Framework Base. Queste estensioni consentono molte nuove funzionalità client avanzate, tra cui il rendering parziale delle pagine senza richiedere un aggiornamento completo della pagina, la possibilità di accedere ai servizi Web tramite lo script client (inclusa l'API di profilatura ASP.NET) e un'API lato client estesa progettato per eseguire il mirroring di molti degli schemi di controllo visualizzati nell'insieme di controlli lato server ASP.NET.

Questo white paper vengono esaminati l'implementazione e l'utilizzo della profilatura ASP.NET e servizi di autenticazione basata su form così come vengono esposte per le estensioni AJAX di Microsoft ASP.NET AJAX ExtensionsThe semplificano l'autenticazione basata su form incredibilmente supportare, come lo (nonché il servizio di profilatura) vengono esposti tramite uno script del proxy del servizio Web. Le estensioni AJAX supporta anche l'autenticazione personalizzata tramite la classe AuthenticationServiceManager.

Questo white paper si basa sulla versione Beta 2 di Visual Studio 2008 e .NET Framework 3.5. Questo white paper si presuppone inoltre che si userà Visual Studio 2008 Beta 2, non Visual Web Developer Express e forniscono procedure dettagliate in base all'interfaccia utente di Visual Studio. Alcuni esempi di codice possono usare i modelli di progetto non disponibili in Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Profili e l'autenticazione*

I profili di ASP.NET di Microsoft e i servizi di autenticazione forniti dal sistema di autenticazione basata su form ASP.NET e sono i componenti standard di ASP.NET. ASP.NET AJAX Extensions forniscono accesso tramite script per questi servizi tramite proxy lo script, tramite un modello semplice nello spazio dei nomi Sys della libreria client AJAX.

Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione ed è il servizio gateway per consentire i profili utente personalizzati fornito da ASP.NET. Uso del servizio di autenticazione di ASP.NET AJAX è compatibile con autenticazione basata su form ASP.NET standard, in modo che le applicazioni attualmente in uso autenticazione basata su form (ad esempio con l'account di accesso di controllo) verranno non interrotte eseguendo l'aggiornamento al servizio di autenticazione AJAX.

Il servizio profili consente l'integrazione automatica e l'archiviazione dei dati utente in base all'appartenenza fornita dal servizio di autenticazione. I dati archiviati vengono specificati nel file Web. config e la gestione dei dati di gestire i vari provider di servizi di profilatura. Come con il servizio di autenticazione, il servizio profili di AJAX è compatibile con il servizio di profilo ASP.NET standard, in modo che le pagine attualmente che incorpora le funzionalità del servizio profili di ASP.NET non devono essere suddivisi, includendo il supporto AJAX.

Incorporare l'autenticazione di ASP.NET e servizi di profilatura stessi in un'applicazione è all'esterno dell'ambito di questo white paper. Per altre informazioni sull'argomento, vedere MSDN Library, fare riferimento a articolo gestione di utenti tramite l'appartenenza al [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET include anche un'utilità di configurare automaticamente l'appartenenza a un Server SQL, ovvero provider di servizi di autenticazione predefinito per l'appartenenza di ASP.NET. Per altre informazioni, vedere l'articolo strumento di registrazione di SQL Server ASP.NET (Aspnet\_regsql.exe) in [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Uso del servizio di autenticazione ASP.NET AJAX*

Il servizio di autenticazione basata su AJAX ASP.NET deve essere abilitato nel file Web. config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Il servizio di autenticazione richiede l'abilitazione dell'autenticazione form di ASP.NET e i cookie deve essere abilitata nel browser del client (uno script non è possibile abilitare una sessione senza cookie poiché le sessioni senza cookie richiedono parametri URL).

Una volta che il servizio di autenticazione AJAX è abilitato e configurato, lo script client può immediatamente in grado di sfruttare i vantaggi dell'oggetto AuthenticationService. In primo luogo, lo script client sarà possibile sfruttare il `login` metodo e `isLoggedIn` proprietà. Sono presenti diverse proprietà per offrire valori predefiniti per il metodo di accesso, che può accettare un numero elevato di parametri.

*AuthenticationService membri*

*metodo di accesso:*

Il metodo Login () inizia una richiesta per autenticare le credenziali dell'utente. Questo metodo è asincrono e non blocca l'esecuzione.

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| userName | Obbligatorio. Il nome utente da autenticare. |
| password | Facoltativo (valore predefinito è null). Password dell'utente. |
| isPersistent | Facoltativo (valore predefinito è false). Indica se il cookie di autenticazione dell'utente deve essere mantenuto tra le sessioni. Se false, l'utente verrà disconnettersi quando il browser viene chiuso o alla scadenza della sessione. |
| URL di reindirizzamento | Facoltativo (valore predefinito è null). L'URL per reindirizzare il browser al termine dell'autenticazione. Se questo parametro è null o una stringa vuota, si verifica alcun reindirizzamento. |
| customInfo | Facoltativo (valore predefinito è null). Questo parametro è attualmente inutilizzato ed è riservato per usi futuri. |
| loginCompletedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare quando l'account di accesso è stata completata. Se specificato, questo parametro esegue l'override della proprietà defaultLoginCompleted. |
| failedCallback | Facoltativo (valore predefinito è null). Funzione da chiamare quando l'account di accesso ha esito negativo. Se specificato, questo parametro esegue l'override della proprietà defaultFailedCallback. |
| userContext | Facoltativo (valore predefinito è null). Dati del contesto utente personalizzati che devono essere passati alle funzioni di callback. |

*Valore restituito:*

Questa funzione non include un valore restituito. Tuttavia, una serie di comportamenti viene inclusa al completamento di una chiamata a questa funzione:

- La pagina corrente sarà aggiornata o essere modificata se la `redirectUrl` parametro non è né null né una stringa vuota.
- Tuttavia, se il parametro è null o una stringa vuota, il `loginCompletedCallback` parametro, o `defaultLoginCompletedCallback` proprietà viene chiamata.
- Se la chiamata al servizio web non riesce, il `failedCallback` parametro di `defaultFailedCallback` proprietà viene chiamata.

*metodo di disconnessione:*

Il metodo logout() Rimuove il cookie di credenziali e disconnette l'utente corrente dall'applicazione web.

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| URL di reindirizzamento | Facoltativo (valore predefinito è null). L'URL per reindirizzare il browser al termine dell'autenticazione. Se questo parametro è null o una stringa vuota, si verifica alcun reindirizzamento. |
| logoutCompletedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare quando la disconnessione è stata completata. Se specificato, questo parametro esegue l'override della proprietà defaultLogoutCompleted. |
| failedCallback | Facoltativo (valore predefinito è null). Funzione da chiamare quando l'account di accesso ha esito negativo. Se specificato, questo parametro esegue l'override della proprietà defaultFailedCallback. |
| userContext | Facoltativo (valore predefinito è null). Dati del contesto utente personalizzati che devono essere passati alle funzioni di callback. |

*Valore restituito:*

Questa funzione non include un valore restituito. Tuttavia, una serie di comportamenti viene inclusa al completamento di una chiamata a questa funzione:

- La pagina corrente sarà aggiornata o essere modificata se la `redirectUrl` parametro non è né null né una stringa vuota.
- Tuttavia, se il parametro è null o una stringa vuota, il `logoutCompletedCallback` parametro, o `defaultLogoutCompletedCallback` proprietà viene chiamata.
- Se la chiamata al servizio web non riesce, il `failedCallback` parametro di `defaultFailedCallback` proprietà viene chiamata.

*proprietà defaultFailedCallback (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata se si verifica un errore di comunicazione con il servizio web. Dovrebbe ricevere un delegato (o riferimento alle funzioni).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| errore | Specifica le informazioni sull'errore. |
| userContext | Specifica le informazioni sul contesto utente forniti quando è stata chiamata la funzione di accesso o disconnessione. |
| MethodName | Il nome del metodo chiamante. |

*Proprietà defaultLoginCompletedCallback (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata quando la chiamata del servizio web di accesso è stata completata. Dovrebbe ricevere un delegato (o riferimento alle funzioni).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| validCredentials | Specifica se l'utente ha specificato credenziali valide. `true` Se l'utente è connesso correttamente. in caso contrario `false`. |
| userContext | Specifica le informazioni sul contesto utente forniti quando è stata chiamata la funzione di accesso. |
| MethodName | Il nome del metodo chiamante. |

*Proprietà defaultLogoutCompletedCallback (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata quando la chiamata del servizio web di disconnessione è stata completata. Dovrebbe ricevere un delegato (o riferimento alle funzioni).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| risultato | Questo parametro sarà sempre `null`; è riservato per utilizzi futuri. |
| userContext | Specifica le informazioni sul contesto utente forniti quando è stata chiamata la funzione di accesso. |
| MethodName | Il nome del metodo chiamante. |

*Proprietà isLoggedIn (get):*

Questa proprietà ottiene lo stato corrente di autenticazione dell'utente; è impostata dall'oggetto ScriptManager durante una richiesta di pagina.

Questa proprietà restituisce `true` se l'utente è attualmente connesso; in caso contrario, restituisce `false`.

*proprietà del percorso (get, set):*

A livello di codice, questa proprietà determina la posizione del servizio web di autenticazione. Può essere utilizzato per sostituire il provider di autenticazione predefiniti, nonché uno impostato in modo dichiarativo nella proprietà di percorso del nodo di figlio del controllo ScriptManager AuthenticationService (per altre informazioni, vedere l'utilizzo un Provider di servizi di autenticazione personalizzati argomento riportato di seguito).

Si noti che non venga modificato il percorso del servizio di autenticazione predefinito. Tuttavia, ASP.NET AJAX consente di specificare il percorso di un servizio web che fornisce la stessa interfaccia di classe di proxy del servizio di autenticazione ASP.NET AJAX.

Si noti anche che questa proprietà non deve essere impostata su un valore che indirizza la richiesta di script all'esterno del sito corrente. Poiché l'applicazione corrente non riceverà le credenziali di autenticazione, sarebbe inutile; Inoltre, AJAX sottostante la tecnologia non deve inviare le richieste intersito e può generare un'eccezione di sicurezza nel browser del client.

Questa proprietà è un `String` oggetto che rappresenta il percorso per il servizio web di autenticazione.

*proprietà di timeout (get, set):*

Questa proprietà determina il periodo di tempo di attesa per il servizio di autenticazione prima di presumere che la richiesta di accesso non è riuscita. Se il timeout scade durante l'attesa di una chiamata al completamento, verrà chiamato il callback richiesta non è riuscita e non verrà completata la chiamata.

Questa proprietà è un `Number` oggetto che rappresenta il numero di millisecondi di attesa dei risultati dal servizio di autenticazione.

*Esempio di codice: La registrazione al servizio di autenticazione*

Il markup seguente è una pagina ASP.NET di esempio con una chiamata di uno script semplice per i metodi di accesso e disconnessione della classe AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>L'accesso ai dati tramite AJAX di profilatura ASP.NET

Il servizio di profilatura di ASP.NET viene inoltre esposta attraverso ASP.NET AJAX Extensions. Poiché il servizio di profilatura ASP.NET fornisce un'API avanzata e granulare per cui archiviare e recuperare i dati utente, può trattarsi di uno strumento eccellente per la produttività.

Il servizio profilo deve essere abilitato nel file Web. config; non è per impostazione predefinita. A tale scopo, assicurarsi che il `profileService` ha abilitato l'elemento figlio = true specificato in config e aver specificato le proprietà che possono essere letti o scritte come segue:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Il servizio profilo deve anche essere configurato. Sebbene la configurazione del servizio di profilatura sia all'esterno dell'ambito di questo white paper, vale la pena notare che i gruppi di base a quanto definito nelle impostazioni di configurazione del profilo sarà accessibili come proprietà secondarie del nome del gruppo. Ad esempio, con la seguente sezione del profilo specificata:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Lo script client sarebbe in grado di accedere a nome, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip e BackgroundColor come proprietà del campo delle proprietà della classe ProfileService.

Dopo aver configurato il servizio di profilatura di AJAX, sarà immediatamente disponibile nelle pagine. Tuttavia, dovrà essere caricato una volta prima dell'uso.

*Membri Sys.Services.ProfileService*

*campo di proprietà:*

Il campo properties espone tutti i dati di profilo configurato come proprietà figlio che possono essere utilizzate dalla convenzione di nome dell'operatore punto. Le proprietà che sono figli di gruppi di proprietà sono dette GroupName.PropertyName. Nella configurazione del profilo di esempio presentata in precedenza, per ottenere lo stato dell'utente, è possibile usare l'identificatore seguente:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*metodo di caricamento:*

Carica tutte le proprietà o un elenco selezionato dal server.

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| propertyNames | Facoltativo (valore predefinito è null). Il caricamento delle proprietà dal server. |
| loadCompletedCallback | Facoltativo (valore predefinito è null). Funzione da chiamare durante il caricamento è stata completata. |
| failedCallback | Facoltativo (valore predefinito è null). Funzione da chiamare se si verifica un errore. |
| userContext | Facoltativo (valore predefinito è null). Informazioni di contesto deve essere passato alla funzione di callback. |

La funzione di caricamento non è un valore restituito. Se la chiamata è stata completata correttamente, chiama uno il `loadCompletedCallback` parametro o `defaultLoadCompletedCallback` proprietà. Se la chiamata non è riuscita o il timeout è scaduto, ovvero il `failedCallback` parametro o `defaultFailedCallback` proprietà verrà chiamata.

Se il `propertyNames` parametro viene omesso, vengono recuperate tutte le proprietà di lettura configurate dal server.

*metodo Save:*

Il metodo Save () consente di salvare l'elenco di proprietà specificato (o tutte le proprietà) per il profilo dell'utente ASP.NET.

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| propertyNames | Facoltativo (valore predefinito è null). Le proprietà da salvare nel server. |
| saveCompletedCallback | Facoltativo (valore predefinito è null). Funzione da chiamare durante il salvataggio è stata completata. |
| failedCallback | Facoltativo (valore predefinito è null). Funzione da chiamare se si verifica un errore. |
| userContext | Facoltativo (valore predefinito è null). Informazioni di contesto deve essere passato alla funzione di callback. |

Il salvataggio funzione non ha un valore restituito. Se la chiamata viene completata correttamente, verrà chiamata il `saveCompletedCallback` parametro o `defaultSaveCompletedCallback` proprietà. Se la chiamata non è riuscita o il timeout è scaduto, ovvero il `failedCallback` o `defaultFailedCallback` proprietà verrà chiamata.

Se il `propertyNames` parametro è null, tutte le proprietà di profilo verranno inviate al server e il server a determinare le proprietà che possono essere salvate e quali invece non possono.

*proprietà defaultFailedCallback (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata se si verifica un errore di comunicazione con il servizio web. Dovrebbe ricevere un delegato (o riferimento alle funzioni).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| Error | Specifica le informazioni sull'errore. |
| userContext | Specifica le informazioni sul contesto utente forniti quando il caricamento o salvataggio funzione è stata chiamata. |
| MethodName | Il nome del metodo chiamante. |

*proprietà defaultSaveCompleted (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata al termine del salvataggio dei dati di profilo dell'utente. Dovrebbe ricevere un delegato (o riferimento alle funzioni).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| numPropsSaved | Specifica il numero di proprietà che sono state salvate. |
| userContext | Specifica le informazioni sul contesto utente forniti quando il caricamento o salvataggio funzione è stata chiamata. |
| MethodName | Il nome del metodo chiamante. |

*proprietà defaultLoadCompleted (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata al termine del caricamento dei dati del profilo dell'utente. Dovrebbe ricevere un delegato (o riferimento alle funzioni).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| numPropsLoaded | Specifica il numero di proprietà caricate. |
| userContext | Specifica le informazioni sul contesto utente forniti quando il caricamento o salvataggio funzione è stata chiamata. |
| MethodName | Il nome del metodo chiamante. |

*proprietà del percorso (get, set):*

A livello di codice, questa proprietà determina la posizione del servizio web profili. Può essere utilizzato per eseguire l'override di provider di servizi del profilo predefinito, nonché uno impostato in modo dichiarativo nella proprietà di percorso del nodo figlio ProfileService del controllo ScriptManager.

Si noti che non venga modificato il percorso del servizio profili predefinito. Tuttavia, ASP.NET AJAX consente di specificare il percorso di un servizio web che fornisce la stessa interfaccia di classe di proxy del servizio di autenticazione ASP.NET AJAX.

Si noti anche che questa proprietà non deve essere impostata su un valore che indirizza la richiesta di script all'esterno del sito corrente. AJAX sottostante la tecnologia non deve inviare le richieste intersito e può generare un'eccezione di sicurezza nel browser del client.

Questa proprietà è un `String` oggetto che rappresenta il percorso per il servizio web profili.

*proprietà di timeout (get, set):*

Questa proprietà determina il periodo di tempo di attesa per il servizio profilo prima di presumere che il carico o Salva la richiesta non è riuscita. Se il timeout scade durante l'attesa di una chiamata al completamento, verrà chiamato il callback richiesta non è riuscita e non verrà completata la chiamata.

Questa proprietà è un `Number` oggetto che rappresenta il numero di millisecondi di attesa dei risultati dal servizio di profilo.

*Esempio di codice: il caricamento dei dati di profilo al caricamento della pagina*

Il codice seguente controlla se un utente viene autenticato e in questo caso, verrà caricato il colore di sfondo preferita dell'utente della pagina.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Usando un Provider di servizi di autenticazione personalizzati*

ASP.NET AJAX Extensions consentono di creare un provider di servizi di autenticazione di script personalizzata esponendo la funzionalità tramite un servizio web personalizzato. Per poter essere usato, il servizio web deve esporre due metodi, `Login` e `Logout`; e questi metodi devono essere specificati con le firme del metodo di stesso come il servizio web di autenticazione basata su AJAX ASP.NET predefinito.

Dopo aver creato il servizio web personalizzato, è necessario specificare il percorso all'oggetto stesso in modo dichiarativo nella pagina del livello di programmazione nel codice o tramite lo script client.

*Per impostare il percorso in modo dichiarativo:*

Per impostare il percorso in modo dichiarativo, includere l'elemento AuthenticationService figlio dell'oggetto ScriptManager nella pagina ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Per impostare il percorso nel codice:*

Per impostare il percorso a livello di programmazione, specificare il percorso tramite l'istanza di gestione di script:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Per impostare il percorso nello script:*

Per impostare il percorso a livello di codice nello script, utilizzare il `path` proprietà della classe AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Servizio Web di esempio per l'autenticazione personalizzata*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Riepilogo

Servizi ASP.NET - specificamente i servizi di profilatura, l'appartenenza e autenticazione - sono facilmente esposto a JavaScript nel browser del client. Ciò consente agli sviluppatori di integrare il codice lato client con il meccanismo di autenticazione in modo trasparente, senza dipendere controlli, ad esempio gli UpdatePanel per eseguire il lavoro sporco. Dati di profilo possono essere protetti dal client, utilizzando le impostazioni di configurazione web; dati non sono disponibili per impostazione predefinita e gli sviluppatori devono acconsentire esplicitamente alle proprietà del profilo.

Inoltre, creando le implementazioni del servizio web semplificata con le firme dei metodi equivalenti, gli sviluppatori possono creare i provider di script personalizzata per questi servizi intrinseci di ASP.NET. Supporto per queste tecniche semplifica lo sviluppo di applicazioni rich client, offrendo agli sviluppatori un'ampia gamma di flessibilità per soddisfare esigenze specifiche.

## <a name="bio"></a>*Bio*

Scott Cate ha collaborato con tecnologie Web di Microsoft dal 1997 ed è il presidente di myKB.com ([www.myKB.com](http://www.myKB.com)) ed è specializzato nella scrittura di ASP.NET basato su applicazioni incentrate su soluzioni Software di Knowledge Base. È possibile contattare Scott tramite posta elettronica all'indirizzo [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Successivo](understanding-asp-net-ajax-localization.md)
