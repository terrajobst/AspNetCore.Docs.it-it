---
uid: signalr/overview/older-versions/hub-authorization
title: L'autenticazione e autorizzazione per SignalR Hubs (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: In questo argomento viene descritto come limitare gli utenti o i ruoli possono accedere i metodi dell'hub.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: ac25a332e4efaa18eb8fff1ce7fa5d283f10a98d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394625"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>L'autenticazione e autorizzazione per SignalR Hubs (SignalR 1.x)
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> In questo argomento viene descritto come limitare gli utenti o i ruoli possono accedere i metodi dell'hub.


## <a name="overview"></a>Panoramica

Di seguito sono elencate le diverse sezioni di questo argomento:

- [Autorizzare l'attributo](#authorizeattribute)
- [Richiedere l'autenticazione per tutti gli hub](#requireauth)
- [Autorizzazione personalizzata](#custom)
- [Passare le informazioni di autenticazione client](#passauth)
- [Opzioni di autenticazione per i client .NET](#authoptions)

    - [Cookie con autenticazione basata su form](#cookie)
    - [Autenticazione di Windows](#windows)
    - [Intestazione di connessione](#header)
    - [Certificato](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autorizzare l'attributo

SignalR fornisce il [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attributo per specificare quali utenti o ruoli hanno accesso a un hub o un metodo. Questo attributo si trova nel `Microsoft.AspNet.SignalR` dello spazio dei nomi. Si applica il `Authorize` attributo a un hub o metodi particolari in un hub. Quando si applica il `Authorize` attributo a una classe di hub, il requisito di autorizzazione specificato viene applicato a tutti i metodi nell'hub. Seguito sono riportati i diversi tipi di requisiti di autorizzazione che è possibile applicare. Senza il `Authorize` attributo, tutti i metodi pubblici in hub sono disponibili per un client connessa all'hub.

Se è stato definito un ruolo denominato "Admin" nell'applicazione web, è possibile specificare che solo gli utenti in tale ruolo possono accedere a un hub con il codice seguente.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

In alternativa, è possibile specificare che un hub contiene un metodo che è disponibile per tutti gli utenti e un secondo metodo è disponibile solo per gli utenti autenticati, come illustrato di seguito.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Negli esempi seguenti scenari di autorizzazione diverse:

- `[Authorize]` : solo gli utenti autenticati
- `[Authorize(Roles = "Admin,Manager")]` : solo gli utenti ai ruoli specificati autenticati
- `[Authorize(Users = "user1,user2")]` : solo gli utenti con i nomi utente specificati autenticati
- `[Authorize(RequireOutgoing=false)]` : solo gli utenti autenticati possono richiamare l'hub, ma le chiamate dal server ai client non sono limitate per l'autorizzazione, ad esempio, quando solo ad alcuni utenti possono inviare un messaggio, ma tutti gli altri utenti possono ricevere il messaggio. La proprietà RequireOutgoing è applicabile solo per l'intero hub, non sui metodi di singoli utenti all'interno dell'hub. Quando RequireOutgoing non è impostata su false, solo gli utenti che soddisfano i requisiti di autorizzazione vengono chiamati dal server.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Richiedere l'autenticazione per tutti gli hub

È possibile richiedere l'autenticazione per tutti gli hub e i metodi dell'hub nell'applicazione chiamando il [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metodo all'avvio dell'applicazione. È possibile utilizzare questo metodo se sono state più hub, per imporre un requisito di autenticazione per tutti gli elementi. Con questo metodo, è possibile specificare l'autorizzazione in uscita, utente o ruolo. È possibile specificare solo l'accesso ai metodi dell'hub è limitato agli utenti autenticati. Tuttavia, è comunque possibile applicare l'attributo Authorize hub o i metodi per specificare i requisiti aggiuntivi. Oltre il requisito di autenticazione di base viene applicato ai requisiti specificati negli attributi.

Nell'esempio seguente viene illustrato un file Global. asax che limita tutte i metodi dell'hub per gli utenti autenticati.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Se si chiama il `RequireAuthentication()` metodo dopo l'elaborazione di una richiesta SignalR, SignalR genererà un `InvalidOperationException` eccezione. Questa eccezione viene generata perché non è possibile aggiungere un modulo per a HubPipeline dopo aver richiamata la pipeline. Nell'esempio precedente illustra la chiamata di `RequireAuthentication` metodo nella `Application_Start` metodo che viene eseguita una sola volta prima di gestire la prima richiesta.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorizzazione personalizzata

Se occorre personalizzare come viene determinata autorizzazione, è possibile creare una classe che deriva da `AuthorizeAttribute` ed eseguire l'override di [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) (metodo). Questo metodo viene chiamato per ogni richiesta per determinare se l'utente è autorizzato a completare la richiesta. Nel metodo sottoposto a override, fornire la logica necessaria per lo scenario di autorizzazione. Nell'esempio seguente viene illustrato come applicare l'autorizzazione tramite identità basata sulle attestazioni.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Passare le informazioni di autenticazione client

Potrebbe essere necessario usare le informazioni di autenticazione nel codice che viene eseguito sul client. Passare le informazioni necessarie quando si chiamano i metodi sul client. Ad esempio, un metodo dell'applicazione di chat è stato possibile passare come parametro il nome utente della persona che la registrazione di un messaggio, come illustrato di seguito.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

In alternativa, è possibile creare un oggetto per rappresentare le informazioni di autenticazione e passare tale oggetto come parametro, come illustrato di seguito.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Non passare mai id connessione del client per altri client, come un utente malintenzionato può usarlo per simulare una richiesta da quel client.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opzioni di autenticazione per i client .NET

Quando si dispone di un client .NET, ad esempio un'app console che interagisce con un hub di cui è limitato ai soli utenti autenticati, è possibile passare le credenziali di autenticazione in un cookie, l'intestazione di connessione o un certificato. Gli esempi in questa sezione mostrano come usare questi metodi diversi per l'autenticazione di un utente. Non sono App con funzionalità complete di SignalR. Per altre informazioni sui client .NET con SignalR, vedere [Guida all'API Hubs - Client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Quando il client .NET interagisce con un hub che utilizza l'autenticazione basata su form ASP.NET, è necessario impostare manualmente il cookie di autenticazione nella connessione. Si aggiunge il cookie per il `CookieContainer` proprietà di [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) oggetto. Nell'esempio seguente mostra un'app console che recupera un cookie di autenticazione da una pagina web e aggiunge tale cookie per la connessione. L'URL `https://www.contoso.com/RemoteLogin` nell'esempio punta a una pagina web che è necessario creare. La pagina potrebbe recuperare il nome utente registrato e la password e tentare di accedere con le credenziali dell'utente.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

L'app console invia le credenziali da www.contoso.com/RemoteLogin che potrebbe fare riferimento a una pagina vuota che contiene il file code-behind seguente.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Autenticazione di Windows

Quando si usa l'autenticazione di Windows, è possibile passare le credenziali dell'utente corrente usando il [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) proprietà. Impostare le credenziali per la connessione al valore del DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Intestazione di connessione

Se l'applicazione non usa i cookie, è possibile passare le informazioni utente nell'intestazione di connessione. Ad esempio, è possibile passare un token nell'intestazione di connessione.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Quindi, nell'hub, verificare il token dell'utente.

<a id="certificate"></a>

### <a name="certificate"></a>Certificato

È possibile passare un certificato client per verificare che l'utente. Il certificato è stato aggiunto durante la creazione della connessione. Nell'esempio seguente viene illustrato come aggiungere un certificato client per la connessione. non è visibile l'applicazione console completa. Usa il [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) classe che fornisce diversi modi per creare il certificato.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
