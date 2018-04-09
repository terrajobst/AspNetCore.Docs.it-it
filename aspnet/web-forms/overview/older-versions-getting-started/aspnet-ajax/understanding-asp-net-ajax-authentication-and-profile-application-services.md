---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Informazioni sui servizi dell'applicazione del profilo e l'autenticazione ASP.NET AJAX | Documenti Microsoft
author: scottcate
description: Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione e il servizio gateway per consentire a un utente personalizzato...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 0bf6538d0c4ae9488e6ac29ccba6d4b243cf070e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Informazioni sui servizi dell'applicazione del profilo e l'autenticazione ASP.NET AJAX
====================
da [Scott categorie](https://github.com/scottcate)

[Scarica il PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione ed è il servizio gateway per consentire di profili utente personalizzati fornito da ASP.NET. Utilizzo del servizio di autenticazione ASP.NET AJAX è compatibile con autenticazione basata su form ASP.NET standard, pertanto le applicazioni utilizzano l'autenticazione basata su form (ad esempio con l'account di accesso di controllo) verranno non interrotti, effettuare l'aggiornamento al servizio di autenticazione AJAX.


## <a name="introduction"></a>Introduzione

Come parte di .NET Framework 3.5, Microsoft ha pubblicato l'aggiornamento di un ambiente ridimensionabili; non solo è disponibile un nuovo ambiente di sviluppo, ma le nuove funzionalità di Language-Integrated Query (LINQ) e altri miglioramenti del linguaggio saranno presto disponibili. Inoltre, alcune funzionalità di altri set di strumenti, in particolare Estensioni AJAX di ASP.NET, note sono siano inclusi come membri di prima classe della libreria di classi .NET Framework Base. Queste estensioni consentono a numerose nuove funzionalità rich client, inclusi il rendering parziale di pagine senza richiedere un aggiornamento completo di pagina, la possibilità di accedere ai servizi Web tramite lo script client (inclusi l'API di profilatura di ASP.NET) e un'API lato client estesa progettato per eseguire il mirroring molti degli schemi di controllo mostrati nel set di controllo sul lato server ASP.NET.

Questo white paper esamina l'implementazione e l'utilizzo della profilatura ASP.NET e servizi di autenticazione basata su form come gli oggetti vengono esposti da Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions semplificano l'autenticazione basata su form incredibilmente supportare, come (così come il servizio di profilatura) vengono esposti tramite uno script proxy servizio Web. Le estensioni AJAX supportano anche l'autenticazione personalizzata tramite la classe AuthenticationServiceManager.

Questo white paper dipende dalla versione Beta 2 di Visual Studio 2008 e .NET Framework 3.5. Questo white paper si presuppone inoltre che è necessario lavorare con Visual Studio 2008 Beta 2, non Visual Web Developer Express e verranno illustrate le procedure dettagliate in base all'interfaccia utente di Visual Studio. Alcuni esempi di codice possono utilizzare i modelli di progetto non disponibili in Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Profili e l'autenticazione*

I profili di ASP.NET di Microsoft e i servizi di autenticazione vengono forniti dal sistema di autenticazione basata su form ASP.NET e sono i componenti standard di ASP.NET. ASP.NET AJAX Extensions forniscono l'accesso di script per questi servizi tramite il proxy di script, tramite un modello semplice nello spazio dei nomi di Sys. Services della libreria client AJAX.

Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione ed è il servizio gateway per consentire di profili utente personalizzati fornito da ASP.NET. Utilizzo del servizio di autenticazione ASP.NET AJAX è compatibile con autenticazione basata su form ASP.NET standard, pertanto le applicazioni utilizzano l'autenticazione basata su form (ad esempio con l'account di accesso di controllo) verranno non interrotti, effettuare l'aggiornamento al servizio di autenticazione AJAX.

Il servizio profili consente l'integrazione automatica e l'archiviazione dei dati utente in base all'appartenenza come fornita dal servizio di autenticazione. I dati archiviati sono specificati dal file Web. config e la gestione dei dati di gestire i vari provider di servizi di profilatura. Come con il servizio di autenticazione, il servizio AJAX profilo è compatibile con il servizio di profilo ASP.NET standard, in modo che le pagine attualmente l'inserimento di funzionalità del servizio profili di ASP.NET non devono essere suddiviso, includendo il supporto AJAX.

Incorpora l'autenticazione basata su ASP.NET e i servizi di profilatura in un'applicazione è esterno all'ambito di questo white paper. Per ulteriori informazioni sull'argomento, vedere MSDN Library, fare riferimento a articolo gestione degli utenti tramite l'appartenenza al [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET include anche un'utilità di configurare automaticamente l'appartenenza con SQL Server, ovvero il provider del servizio di autenticazione predefinito per l'appartenenza ASP.NET. Per altre informazioni, vedere l'articolo strumento di registrazione di SQL Server ASP.NET (Aspnet\_regsql.exe) in [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Utilizzo del servizio di autenticazione ASP.NET AJAX*

Il servizio di autenticazione basata su AJAX ASP.NET deve essere abilitato nel file Web. config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Il servizio di autenticazione richiede l'autenticazione basata su form ASP.NET deve essere abilitata e i cookie deve essere abilitata nel browser del client (uno script non è possibile abilitare una sessione senza cookie poiché le sessioni senza cookie richiedono parametri URL).

Una volta che il servizio di autenticazione AJAX è abilitato e configurato, lo script client può immediatamente sfruttare l'oggetto Sys.Services.AuthenticationService. In primo luogo, lo script client sarà necessario sfruttare il `login` (metodo) e `isLoggedIn` proprietà. Diverse proprietà disponibili per fornire valori predefiniti per il metodo di accesso, che può accettare un numero elevato di parametri.

*Membri Sys.Services.AuthenticationService*

*metodo di accesso:*

Il metodo Login () inizia una richiesta per autenticare le credenziali dell'utente. Questo metodo è asincrono e non blocca l'esecuzione.

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| userName | Obbligatorio. Il nome utente per l'autenticazione. |
| password | Facoltativo (valore predefinito è null). Password dell'utente. |
| isPersistent | Facoltativo (valore predefinito è false). Se il cookie di autenticazione dell'utente deve essere rese persistenti tra le sessioni. Se false, l'utente verrà disconnettersi quando il browser viene chiuso o alla scadenza della sessione. |
| redirectUrl | Facoltativo (valore predefinito è null). L'URL per reindirizzare il browser alla riuscita dell'autenticazione. Se questo parametro è null o una stringa vuota, si verifica alcun reindirizzamento. |
| customInfo | Facoltativo (valore predefinito è null). Questo parametro è riservato per utilizzi futuri e non viene attualmente utilizzato. |
| loginCompletedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare quando l'account di accesso è stata completata. Se specificato, questo parametro sostituisce la proprietà defaultLoginCompleted. |
| failedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare quando l'account di accesso non è riuscita. Se specificato, questo parametro esegue l'override di proprietà defaultFailedCallback. |
| userContext | Facoltativo (valore predefinito è null). Dati del contesto utente personalizzati che devono essere passati alle funzioni di callback. |

*Valore restituito:*

Questa funzione non include un valore restituito. Un numero di comportamenti è tuttavia disponibili dopo il completamento di una chiamata a questa funzione:

- La pagina corrente sarà aggiornata o essere modificata se la `redirectUrl` parametri non è né null né una stringa vuota.
- Tuttavia, se il parametro è null o una stringa vuota, il `loginCompletedCallback` parametro, o `defaultLoginCompletedCallback` proprietà viene chiamata.
- Se la chiamata al servizio web non riesce, il `failedCallback` parametro di `defaultFailedCallback` proprietà viene chiamata.

*metodo di disconnessione:*

Il metodo logout() Rimuove il cookie di credenziali e disconnette l'utente corrente dall'applicazione web.

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| redirectUrl | Facoltativo (valore predefinito è null). L'URL per reindirizzare il browser alla riuscita dell'autenticazione. Se questo parametro è null o una stringa vuota, si verifica alcun reindirizzamento. |
| logoutCompletedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare quando la disconnessione è stata completata. Se specificato, questo parametro sostituisce la proprietà defaultLogoutCompleted. |
| failedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare quando l'account di accesso non è riuscita. Se specificato, questo parametro esegue l'override di proprietà defaultFailedCallback. |
| userContext | Facoltativo (valore predefinito è null). Dati del contesto utente personalizzati che devono essere passati alle funzioni di callback. |

*Valore restituito:*

Questa funzione non include un valore restituito. Un numero di comportamenti è tuttavia disponibili dopo il completamento di una chiamata a questa funzione:

- La pagina corrente sarà aggiornata o essere modificata se la `redirectUrl` parametri non è né null né una stringa vuota.
- Tuttavia, se il parametro è null o una stringa vuota, il `logoutCompletedCallback` parametro, o `defaultLogoutCompletedCallback` proprietà viene chiamata.
- Se la chiamata al servizio web non riesce, il `failedCallback` parametro di `defaultFailedCallback` proprietà viene chiamata.

*proprietà defaultFailedCallback (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata se si verifica un errore di comunicazione con il servizio web. Riceve un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| errore | Specifica le informazioni sull'errore. |
| userContext | Specifica le informazioni sul contesto utente forniti quando è stata chiamata la funzione di connessione o disconnessione. |
| NomeMetodo | Il nome del metodo chiamante. |

*Proprietà defaultLoginCompletedCallback (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata quando la chiamata del servizio web di accesso è stata completata. Riceve un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| validCredentials | Specifica se l'utente ha fornito le credenziali valide. `true` Se l'utente è connesso correttamente. in caso contrario `false`. |
| userContext | Specifica le informazioni sul contesto utente forniti quando è stata chiamata la funzione di accesso. |
| NomeMetodo | Il nome del metodo chiamante. |

*Proprietà defaultLogoutCompletedCallback (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata quando la chiamata del servizio web di disconnessione è stata completata. Riceve un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| risultato | Questo parametro sarà sempre `null`; è riservato per utilizzi futuri. |
| userContext | Specifica le informazioni sul contesto utente forniti quando è stata chiamata la funzione di accesso. |
| NomeMetodo | Il nome del metodo chiamante. |

*Proprietà isLoggedIn (get):*

Questa proprietà ottiene lo stato corrente di autenticazione dell'utente. è impostata dall'oggetto ScriptManager durante una richiesta di pagina.

Questa proprietà restituisce `true` se l'utente è connesso in; in caso contrario, restituisce `false`.

*proprietà del percorso (get, set):*

A livello di programmazione, questa proprietà determina la posizione del servizio web di autenticazione. E può essere utilizzato per eseguire l'override di provider di autenticazione predefinito, nonché uno impostato in modo dichiarativo nella proprietà di percorso del nodo di figlio del controllo ScriptManager AuthenticationService (per ulteriori informazioni, vedere Utilizzo un Provider di servizi di autenticazione personalizzato argomento riportato di seguito).

Si noti che non modifica la posizione del servizio di autenticazione predefinito. Tuttavia, ASP.NET AJAX consente di specificare il percorso di un servizio web che fornisce la stessa interfaccia di classe proxy del servizio di autenticazione ASP.NET AJAX.

Si noti inoltre che questa proprietà non deve essere impostata su un valore che indica la richiesta di script di fuori del sito corrente. Poiché l'applicazione corrente non riceve le credenziali di autenticazione, sarebbe inutile; Inoltre, AJAX sottostante la tecnologia consiglia di non registrare le richieste tra siti e può generare un'eccezione di sicurezza nel browser del client.

Questa proprietà è un `String` oggetto che rappresenta il percorso per il servizio web di autenticazione.

*proprietà di timeout (get, set):*

Questa proprietà determina il periodo di tempo di attesa per il servizio di autenticazione prima di presumere che la richiesta di accesso non è riuscita. Se il timeout scade durante l'attesa di completamento di una chiamata, verrà chiamato il callback della richiesta non è riuscita e non verrà completata la chiamata.

Questa proprietà è un `Number` oggetto che rappresenta il numero di millisecondi di attesa di risultati dal servizio di autenticazione.

*Nell'esempio di codice: La registrazione al servizio di autenticazione*

Il markup seguente è una pagina ASP.NET di esempio con una chiamata di uno script semplice per i metodi di accesso e disconnessione della classe AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>L'accesso a profilatura Data via AJAX ASP.NET

Il servizio di analisi di ASP.NET viene inoltre esposta attraverso Estensioni AJAX di ASP.NET. Poiché il servizio di profilatura ASP.NET fornisce un'API granulare avanzata con la quale memorizzare e recuperare i dati utente, può trattarsi di uno strumento di produttività eccellente.

Il servizio profilo deve essere abilitato in Web. config; non è per impostazione predefinita. A tale scopo, verificare che il `profileService` ha abilitato l'elemento figlio = true Web. config specificato in, e aver specificato le proprietà che possono essere letto o scritte come segue:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Il servizio profilo deve inoltre essere configurato. Anche se la configurazione del servizio di profilatura è esterno all'ambito di questo white paper, vale la pena notare che i gruppi come definito nelle impostazioni di configurazione profilo sarà accessibili come sottoproprietà del nome del gruppo. Ad esempio, con la seguente sezione di profilo specificata:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Lo script client sarebbero in grado di accedere al nome, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip e BackgroundColor come proprietà del campo della proprietà di classe ProfileService.

Dopo aver configurato il servizio di profilatura AJAX, sarà immediatamente disponibile nelle pagine. Tuttavia, si otterrà il caricamento di una volta prima dell'uso.

*Sys.Services.ProfileService members*

*campo di proprietà:*

Il campo delle proprietà espone tutti i dati di profilo configurato come proprietà figlio che è possibile fare riferimento dalla convenzione di nome dell'operatore punto. Proprietà che sono figli di gruppi di proprietà sono dette GroupName.PropertyName. Nella configurazione del profilo di esempio presentata in precedenza, per ottenere lo stato dell'utente, è possibile utilizzare l'identificatore seguente:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*metodo Load:*

Carica tutte le proprietà o un elenco selezionato dal server.

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| propertyNames | Facoltativo (valore predefinito è null). La proprietà deve essere caricata dal server. |
| loadCompletedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare durante il caricamento è stata completata. |
| failedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare se si verifica un errore. |
| userContext | Facoltativo (valore predefinito è null). Informazioni di contesto deve essere passato alla funzione di callback. |

La funzione di caricamento non dispone di un valore restituito. Se la chiamata è stata completata correttamente, chiama uno di `loadCompletedCallback` parametro o `defaultLoadCompletedCallback` proprietà. Se la chiamata non è riuscita o il timeout è scaduto, ovvero il `failedCallback` parametro o `defaultFailedCallback` proprietà verrà chiamata.

Se il `propertyNames` parametro non viene specificato, vengono recuperate tutte le proprietà di lettura configurato dal server.

*Save (metodo):*

Il metodo Save () consente di salvare l'elenco di proprietà specificato (o tutte le proprietà) per il profilo dell'utente ASP.NET.

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| propertyNames | Facoltativo (valore predefinito è null). Le proprietà da salvare per il server. |
| saveCompletedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare durante il salvataggio è stata completata. |
| failedCallback | Facoltativo (valore predefinito è null). La funzione da chiamare se si verifica un errore. |
| userContext | Facoltativo (valore predefinito è null). Informazioni di contesto deve essere passato alla funzione di callback. |

Salva funzione non ha un valore restituito. Se la chiamata ha esito positivo, questo verrà chiamare il `saveCompletedCallback` parametro o `defaultSaveCompletedCallback` proprietà. Se la chiamata non è riuscita o il timeout è scaduto, ovvero il `failedCallback` o `defaultFailedCallback` proprietà verrà chiamata.

Se il `propertyNames` parametro è null, tutte le proprietà di profilo verranno inviate al server e il server a determinare quali proprietà possono essere salvate e quali invece non possono.

*proprietà defaultFailedCallback (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata se si verifica un errore di comunicazione con il servizio web. Riceve un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| Error | Specifica le informazioni sull'errore. |
| userContext | Specifica le informazioni sul contesto utente forniti quando il carico o funzione di salvataggio è stato chiamato. |
| NomeMetodo | Il nome del metodo chiamante. |

*proprietà defaultSaveCompleted (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata al termine del salvataggio dei dati di profilo dell'utente. Riceve un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| numPropsSaved | Specifica il numero di proprietà che sono state salvate. |
| userContext | Specifica le informazioni sul contesto utente forniti quando il carico o funzione di salvataggio è stato chiamato. |
| NomeMetodo | Il nome del metodo chiamante. |

*proprietà defaultLoadCompleted (get, set):*

Questa proprietà specifica una funzione che deve essere chiamata al termine del caricamento dei dati di profilo dell'utente. Riceve un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificata da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametri:*

| **Nome del parametro** | **Significato** |
| --- | --- |
| numPropsLoaded | Specifica il numero di proprietà caricate. |
| userContext | Specifica le informazioni sul contesto utente forniti quando il carico o funzione di salvataggio è stato chiamato. |
| NomeMetodo | Il nome del metodo chiamante. |

*proprietà del percorso (get, set):*

A livello di programmazione, questa proprietà determina la posizione del servizio web del profilo. E può essere utilizzato per eseguire l'override di provider di servizi di profilo predefinito, nonché uno impostato in modo dichiarativo nella proprietà di percorso del nodo figlio di ProfileService del controllo ScriptManager.

Si noti che non modifica la posizione del servizio profilo predefinito. Tuttavia, ASP.NET AJAX consente di specificare il percorso di un servizio web che fornisce la stessa interfaccia di classe proxy del servizio di autenticazione ASP.NET AJAX.

Si noti inoltre che questa proprietà non deve essere impostata su un valore che indica la richiesta di script di fuori del sito corrente. AJAX sottostante la tecnologia consiglia di non registrare le richieste tra siti e può generare un'eccezione di sicurezza nel browser del client.

Questa proprietà è un `String` oggetto che rappresenta il percorso per il servizio web di profilo.

*proprietà di timeout (get, set):*

Questa proprietà determina il periodo di tempo di attesa per il servizio profilo prima di presumere che il carico o Salva la richiesta non è riuscita. Se il timeout scade durante l'attesa di completamento di una chiamata, verrà chiamato il callback della richiesta non è riuscita e non verrà completata la chiamata.

Questa proprietà è un `Number` oggetto che rappresenta il numero di millisecondi di attesa di risultati dal servizio di profilo.

*Nell'esempio di codice: il caricamento dei dati di profilo al caricamento della pagina*

Il codice seguente controlla se un utente è autenticato e in tal caso, verrà caricato il colore di sfondo preferito dell'utente della pagina.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Utilizzando un Provider di servizi di autenticazione personalizzato*

Estensioni AJAX di ASP.NET consente di creare un provider di servizi di autenticazione di uno script personalizzato che espongono la funzionalità tramite un servizio web personalizzato. Per poter essere usato, il servizio web deve esporre due metodi, `Login` e `Logout`; e questi metodi devono essere specificati con le stesse firme di metodo del servizio web di autenticazione basata su AJAX ASP.NET predefinito.

Dopo aver creato il servizio web personalizzato, è necessario specificare il percorso su di esso, in modo dichiarativo nella pagina a livello di programmazione nel codice o tramite lo script client.

*Per impostare il percorso in modo dichiarativo:*

Per impostare il percorso in modo dichiarativo, includere AuthenticationService figlio dell'oggetto ScriptManager nella pagina ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Per impostare il percorso nel codice:*

Per impostare il percorso a livello di codice, specificare il percorso tramite l'istanza di gestione di script:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Per impostare il percorso nello script:*

Per impostare il percorso a livello di codice nello script, utilizzare il `path` proprietà della classe AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Servizio Web di esempio per l'autenticazione personalizzata*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Riepilogo

Servizi ASP.NET - specificamente i servizi di profilatura, appartenenza e l'autenticazione, sono facilmente esposto a JavaScript nel browser client. Ciò consente agli sviluppatori di integrare il codice sul lato client con il meccanismo di autenticazione in modo trasparente, senza dipendere controlli, ad esempio UpdatePanel per effettuare le operazioni necessarie. Dati di profilo possono essere protetti dal client, nonché utilizzando impostazioni di configurazione web. Nessun dato è disponibile per impostazione predefinita, e gli sviluppatori devono registrarsi per le proprietà del profilo.

Inoltre, creando le implementazioni del servizio web semplificata con firme dei metodi equivalenti, gli sviluppatori possono creare provider di uno script personalizzato per questi servizi intrinseci di ASP.NET. Supporto per queste tecniche semplifica lo sviluppo di applicazioni rich client, offrendo agli sviluppatori un'ampia gamma di flessibilità necessaria per soddisfare specifiche esigenze.

## <a name="bio"></a>*Bio*

Categoria Scott lavora con tecnologie Web di Microsoft dal 1997 ed è il vicepresidente myKB.com ([www.myKB.com](http://www.myKB.com)) in cui si è specializzato nella scrittura ASP.NET basato su applicazioni con stato attivo sulle soluzioni Software Knowledge Base. Scott possano essere contattati tramite posta elettronica al [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o il suo blog all'indirizzo [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Successivo](understanding-asp-net-ajax-localization.md)
