---
uid: signalr/overview/older-versions/introduction-to-security
title: Introduzione alla sicurezza SignalR (SignalR 1. x) | Documenti Microsoft
author: pfletcher
description: Vengono descritti i problemi di sicurezza che è necessario considerare quando si sviluppa un'applicazione di SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: b756d3e71d89b6c826bd497f73d052c4c8f634e8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874197"
---
<a name="introduction-to-signalr-security-signalr-1x"></a>Introduzione alla sicurezza SignalR (SignalR 1. x)
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo vengono descritti i problemi di sicurezza che è necessario considerare quando si sviluppa un'applicazione di SignalR.


## <a name="overview"></a>Panoramica

Questo documento contiene le seguenti sezioni:

- [Concetti relativi alla protezione di SignalR](#concepts)

    - [Autenticazione e autorizzazione](#authentication)
    - [Token di connessione](#connectiontoken)
    - [Nuova partecipazione a gruppi durante la riconnessione](#rejoingroup)
- [Come SignalR impedisce Cross-Site Request Forgery](#csrf)
- [Consigli sulla sicurezza per SignalR](#recommendations)

    - [Protocollo SSL (Socket Layers)](#ssl)
    - [Non utilizzare i gruppi come meccanismo di sicurezza](#groupsecurity)
    - [In modo sicuro la gestione dell'input dai client](#input)
    - [Riconciliazione di un cambiamento dello stato utente con una connessione attiva](#reconcile)
    - [File di proxy JavaScript generati automaticamente](#autogen)
    - [Eccezioni](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Concetti relativi alla protezione di SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticazione e autorizzazione

SignalR è progettato per integrare la struttura di autenticazione per un'applicazione esistente. Non fornisce alcuna funzionalità per l'autenticazione degli utenti. Invece autenticare gli utenti come si farebbe normalmente nell'applicazione e quindi utilizzare i risultati dell'autenticazione nel codice SignalR. Ad esempio, è possibile autenticare gli utenti con autenticazione basata su form ASP.NET e quindi applicare gli utenti che nell'hub, o i ruoli sono autorizzati a chiamare un metodo. Nell'hub, è anche possibile passare le informazioni di autenticazione, ad esempio nome utente o se un utente appartiene a un ruolo, al client.

SignalR fornisce il [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attributo per specificare quali utenti hanno accesso a un hub o un metodo. Si applica l'attributo Authorize a un hub o metodi particolari in un hub. Senza l'attributo Authorize, tutti i metodi pubblici dell'hub sono disponibili per un client che si è connessi all'hub. Per ulteriori informazioni sugli hub, vedere [autenticazione e autorizzazione per gli hub SignalR](../security/hub-authorization.md).

Il `Authorize` attributo viene utilizzato solo con gli hub. Per applicare le regole di autorizzazione quando si utilizza un `PersistentConnection` è necessario eseguire l'override di `AuthorizeRequest` metodo. Per ulteriori informazioni sulle connessioni permanenti, vedere [autenticazione e autorizzazione per le connessioni persistenti SignalR](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token di connessione

SignalR riduce il rischio di esecuzione di comandi dannosi convalidando l'identità del mittente. Un token di connessione, che contiene l'id di connessione e il nome utente per gli utenti autenticati, viene passato tra il client e server per ogni richiesta. L'id di connessione è un identificatore univoco che viene generato in modo casuale dal server quando viene creata una nuova connessione e viene mantenuta per tutta la durata della connessione. Il nome utente viene fornito dal meccanismo di autenticazione per l'applicazione web. Il token di connessione è protetta con la crittografia e una firma digitale.

![](introduction-to-security/_static/image2.png)

Per ogni richiesta, il server di convalida del contenuto del token per assicurarsi che la richiesta proviene da parte dell'utente specificato. Il nome utente deve corrispondere all'id di connessione. Convalida sia l'id di connessione e il nome utente, SignalR impedisce un utente malintenzionato facilmente rappresentando un altro utente. Se il server non è possibile convalidare il token di connessione, la richiesta ha esito negativo.

![](introduction-to-security/_static/image4.png)

Poiché l'id di connessione fa parte del processo di verifica, è consigliabile non rivelare l'id di connessione dell'utente ad altri utenti o archiviare il valore nel client, ad esempio in un cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Nuova partecipazione a gruppi durante la riconnessione

Per impostazione predefinita, l'applicazione di SignalR nuovamente assegna automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene eliminata e nuovamente stabilita prima del timeout della connessione. Durante la riconnessione, il client passa un token di gruppo che include l'id di connessione e i gruppi assegnati. Il token gruppo firma digitale viene firmato e crittografato. Il client mantiene lo stesso id di connessione dopo una riconnessione. Pertanto, l'id di connessione passata dal client riconnesso deve corrispondere all'id di connessione precedente utilizzata dal client. Questa verifica impedisce il passaggio delle richieste di partecipazione a gruppi non autorizzati durante la riconnessione di un utente malintenzionato.

Tuttavia, è importante tenere presente che non scade il token di gruppo. Se un utente appartiene a un gruppo in passato, ma è stato escluso dal gruppo, è possibile che tale utente potrebbe essere in grado di simulare un token di gruppo che include il gruppo non consentito. Se si desidera gestire in modo sicuro gli utenti che appartengono a quali gruppi, è necessario archiviare i dati nel server, ad esempio in un database. Quindi, aggiungere logica all'applicazione che verifica sul server se un utente appartiene a un gruppo. Per un esempio di verifica dell'appartenenza al gruppo, vedere [utilizzo dei gruppi](../guide-to-the-api/working-with-groups.md).

Automaticamente nuova partecipazione a gruppi si applica solo quando una connessione viene ristabilita la connessione dopo un'interruzione temporanea. Se un utente si disconnette dal passaggio a un'altra applicazione o del riavvio dell'applicazione, l'applicazione deve gestire come aggiungere l'utente ai gruppi corretti. Per ulteriori informazioni, vedere [utilizzo dei gruppi](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Come SignalR impedisce Cross-Site Request Forgery

Cross-Site richiesta Forgery (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è connesso. SignalR impedisce CSRF rendendo estremamente improbabile per un sito creare una richiesta valida per l'applicazione di SignalR.

### <a name="description-of-csrf-attack"></a>Descrizione dell'attacco CSRF

Di seguito è riportato un esempio di un attacco di tipo CSRF:

1. Un utente accede www.example.com, con autenticazione basata su form.
2. Il server autentica l'utente. La risposta dal server include un cookie di autenticazione.
3. Senza la disconnessione, l'utente visita un sito web. Questo sito contiene il modulo HTML seguente: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso. Questa è la parte "cross-site" di CSRF.
4. L'utente fa clic sul pulsante Invia. Il visualizzatore include il cookie di autenticazione con la richiesta.
5. La richiesta viene eseguita nel server di example.com con contesto di autenticazione dell'utente e può eseguire qualsiasi operazione che è consentito un utente autenticato.

Anche se in questo esempio richiede all'utente di fare clic sul pulsante del form, la pagina può solo eseguire facilmente uno script che invia una richiesta AJAX all'applicazione di SignalR. Inoltre, tramite SSL non impedire un attacco di tipo CSRF poiché il sito dannoso può inviare una richiesta "https://".

In genere, gli attacchi CSRF sono possibili contro i siti web che usano i cookie per l'autenticazione, perché il browser invia tutti i cookie pertinenti al sito web di destinazione. Tuttavia, gli attacchi CSRF non sono limitati per sfruttare i cookie. Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili. Dopo che un utente accede con l'autenticazione di base o Digest, il browser invia automaticamente le credenziali, fino al termine della sessione.

### <a name="csrf-mitigations-taken-by-signalr"></a>Misure di attenuazione CSRF adottate da SignalR

SignalR esegue i passaggi seguenti per impedire un sito di creare richieste valide per l'applicazione di SignalR. Questi passaggi vengono acquisiti per impostazione predefinita e non richiedono alcuna azione nel codice.

- **Disabilitare le richieste tra domini**  
 Per impostazione predefinita, tra domini richieste sono disabilitate in un'applicazione di SignalR per impedire agli utenti di chiamata di un endpoint di SignalR da un dominio esterno. Tutte le richieste che provengono da un dominio esterno viene automaticamente considerata come non valide e rimarrà bloccata. È consigliabile mantenere questo comportamento predefinito; in caso contrario, un sito dannoso può indurre gli utenti a inviare i comandi al sito. Se è necessario utilizzare richieste tra domini, vedere [per stabilire una connessione tra domini](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passare il token di connessione nella stringa di query, non cookie**  
 SignalR passa il token di connessione come un valore di stringa di query, anziché come un cookie. Da non memorizzare il token di connessione come un cookie, il token di connessione non viene inoltrato inavvertitamente il browser quando viene rilevato malware. Inoltre, il token di connessione non viene mantenuto oltre la connessione corrente. Pertanto, un utente malintenzionato non è possibile effettuare una richiesta con credenziali di autenticazione dell'utente.
- **Verificare i token di connessione**  
 Come descritto nel [token di connessione](#connectiontoken) sezione, il server riconosce l'id di connessione associata a ogni utente autenticato. Il server non elabora le richieste da un id di connessione che non corrisponde al nome utente. È improbabile che un utente malintenzionato potrebbe ipotizzare una richiesta valida perché l'utente malintenzionato dovrebbe conoscere il nome utente e l'id generato in modo casuale connessione corrente. L'id di connessione diventa non valido non appena la connessione viene chiusa. Gli utenti anonimi non deve accedere a informazioni riservate.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Consigli sulla sicurezza per SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocollo SSL (Socket Layers)

Il protocollo SSL utilizza la crittografia per proteggere il trasporto dei dati tra un client e server. Se l'applicazione di SignalR trasmette le informazioni riservate tra il client e server, utilizzare SSL per il trasporto. Per ulteriori informazioni sull'impostazione di SSL, vedere [come configurare SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Non utilizzare i gruppi come meccanismo di sicurezza

I gruppi sono un modo pratico per la raccolta di utenti correlati, ma non sono un meccanismo protetto per limitare l'accesso a informazioni riservate. Ciò vale soprattutto quando gli utenti possono automaticamente accedere nuovamente gruppi durante una riconnessione. Si consideri invece l'aggiunta di utenti con privilegi a un ruolo e limitare l'accesso a un metodo dell'hub per solo i membri del ruolo. Per un esempio di limitazione dell'accesso in base a un ruolo, vedere [autenticazione e autorizzazione per gli hub SignalR](../security/hub-authorization.md). Per un esempio di verifica dell'accesso utente ai gruppi durante la riconnessione, vedere [utilizzo dei gruppi](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>In modo sicuro la gestione dell'input dai client

Tutti gli input dai client che è destinato alla trasmissione ad altri client devono essere codificati per garantire che un utente malintenzionato non invia script ad altri utenti. È consigliabile codificare i messaggi dei client di destinazione anziché del server, perché l'applicazione di SignalR potrebbe disporre di molti tipi diversi di client. Di conseguenza, la codifica HTML funziona per un client web, ma non per altri tipi di client. Ad esempio, un metodo di client web per visualizzare un messaggio di chat in modo sicuro gestita il nome utente e un messaggio chiamando il `html()` (funzione).

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Riconciliazione di un cambiamento dello stato utente con una connessione attiva

Se lo stato di autenticazione dell'utente viene modificata mentre è presente una connessione attiva, l'utente riceverà un errore che indica, "durante una connessione SignalR attiva non è possibile modificare l'identità dell'utente". In tal caso, l'applicazione deve connettersi nuovamente al server per assicurarsi che il nome utente e l'id di connessione vengano coordinati. Ad esempio, se l'applicazione consente all'utente di disconnettersi mentre è presente una connessione attiva, il nome utente per la connessione non corrisponderanno più il nome che viene passato per la richiesta successiva. Si verrà interrompere la connessione prima che l'utente si disconnette e quindi riavviarlo.

Tuttavia, è importante notare che la maggior parte delle applicazioni non saranno necessario arrestare e avviare la connessione manualmente. Se l'applicazione reindirizza gli utenti a una pagina separata dopo l'accesso, ad esempio il comportamento predefinito in un'applicazione Web Form o MVC o aggiorna la pagina corrente dopo la disconnessione, la connessione attiva viene automaticamente disconnesso e non richiedere alcuna azione aggiuntiva.

Nell'esempio seguente viene illustrato come arrestare e avviare una connessione quando viene modificato lo stato dell'utente.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

In alternativa, lo stato di autenticazione dell'utente potrebbe modificare il sito utilizza la scadenza con autenticazione basata su form, se è presente alcuna attività per mantenere il cookie di autenticazione valido. In tal caso, l'utente verrà disconnesso e il nome utente non corrisponderanno più il nome utente nel token di connessione. È possibile risolvere questo problema aggiungendo alcuni script che periodicamente richiede una risorsa nel server web per mantenere il cookie di autenticazione valido. Nell'esempio seguente viene illustrato come richiedere una risorsa ogni 30 minuti.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>File di proxy JavaScript generati automaticamente

Se non si desidera includere tutti i metodi e gli hub nel file proxy JavaScript per ogni utente, è possibile disabilitare la generazione automatica del file. È possibile scegliere questa opzione se si dispone di più hub e i metodi, ma non si desidera che ogni utente da tenere presenti tutti i metodi. Disattivare la generazione automatica impostando **EnableJavaScriptProxies** a **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Per ulteriori informazioni sui file proxy JavaScript, vedere [il proxy generato ed esegue automaticamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Eccezioni

È consigliabile evitare il passaggio di oggetti eccezione ai client perché gli oggetti possono esporre informazioni riservate ai client. In alternativa, è possibile chiamare un metodo nel client che viene visualizzato il messaggio di errore pertinenti.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
