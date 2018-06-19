---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Utilizzo dei gruppi in SignalR | Documenti Microsoft
author: pfletcher
description: In questo argomento viene descritto come rendere persistenti le informazioni di appartenenza al gruppo con l'API di Hub.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042222"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="854fb-103">Utilizzo dei gruppi in SignalR</span><span class="sxs-lookup"><span data-stu-id="854fb-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="854fb-104">da [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="854fb-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="854fb-105">In questo argomento viene descritto come aggiungere utenti ai gruppi e rendere persistenti le informazioni di appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="854fb-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="854fb-106">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="854fb-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="854fb-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="854fb-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="854fb-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="854fb-108">.NET 4.5</span></span>
> - <span data-ttu-id="854fb-109">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="854fb-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="854fb-110">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="854fb-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="854fb-111">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="854fb-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="854fb-112">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="854fb-112">Questions and comments</span></span>
> 
> <span data-ttu-id="854fb-113">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="854fb-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="854fb-114">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="854fb-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="854fb-115">Panoramica</span><span class="sxs-lookup"><span data-stu-id="854fb-115">Overview</span></span>

<span data-ttu-id="854fb-116">Gruppi di SignalR forniscono un metodo per la trasmissione di messaggi a un subset specificato di client connessi.</span><span class="sxs-lookup"><span data-stu-id="854fb-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="854fb-117">Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="854fb-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="854fb-118">Non è necessario creare in modo esplicito gruppi.</span><span class="sxs-lookup"><span data-stu-id="854fb-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="854fb-119">In effetti, la prima volta, che specificare il nome in una chiamata a Groups.Add viene automaticamente creato un gruppo e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza in essa contenuti.</span><span class="sxs-lookup"><span data-stu-id="854fb-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="854fb-120">Per un'introduzione all'uso di gruppi, vedere [come gestire l'appartenenza al gruppo dalla classe dell'Hub](hubs-api-guide-server.md#groupsfromhub) nell'API di hub - Guida di Server.</span><span class="sxs-lookup"><span data-stu-id="854fb-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="854fb-121">Non vi è alcuna API per ottenere un elenco di appartenenze di gruppo oppure un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="854fb-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="854fb-122">SignalR invia messaggi al client e i gruppi basati su un modello di pubblicazione/sottoscrizione e il server non gestisce gli elenchi di gruppi o appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="854fb-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="854fb-123">Ciò consente di ottimizzare la scalabilità, poiché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="854fb-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="854fb-124">Quando si aggiunge un utente a un gruppo con il `Groups.Add` (metodo), l'utente riceve i messaggi indirizzati a tale gruppo per la durata della connessione corrente, ma l'appartenenza dell'utente in tale gruppo non viene mantenuto oltre la connessione corrente.</span><span class="sxs-lookup"><span data-stu-id="854fb-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="854fb-125">Se si desidera mantenere in modo permanente le informazioni sui gruppi e appartenenza al gruppo, è necessario archiviare i dati in un repository, ad esempio un database o l'archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="854fb-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="854fb-126">Quindi, ogni volta che un utente si connette all'applicazione, recuperare dal repository quali gruppi a cui appartiene l'utente e aggiungere manualmente l'utente a tali gruppi.</span><span class="sxs-lookup"><span data-stu-id="854fb-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="854fb-127">Quando la riconnessione dopo un'interruzione temporanea, l'utente aggiunge automaticamente anche nuovamente i gruppi assegnate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="854fb-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="854fb-128">Automaticamente nuova partecipazione a un gruppo si applica solo quando la riconnessione, non per stabilire una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="854fb-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="854fb-129">Un token di firma digitale viene passato dal client che contiene l'elenco dei gruppi assegnate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="854fb-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="854fb-130">Se si desidera verificare se l'utente appartiene al gruppo richiesto, è possibile eseguire l'override del comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="854fb-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="854fb-131">Questo argomento include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="854fb-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="854fb-132">Aggiunta e rimozione di utenti</span><span class="sxs-lookup"><span data-stu-id="854fb-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="854fb-133">La chiamata di membri di un gruppo</span><span class="sxs-lookup"><span data-stu-id="854fb-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="854fb-134">L'appartenenza al gruppo di archiviazione in un database</span><span class="sxs-lookup"><span data-stu-id="854fb-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="854fb-135">L'appartenenza al gruppo di archiviazione nell'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="854fb-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="854fb-136">Verifica dell'appartenenza al gruppo durante la riconnessione</span><span class="sxs-lookup"><span data-stu-id="854fb-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="854fb-137">Aggiunta e rimozione di utenti</span><span class="sxs-lookup"><span data-stu-id="854fb-137">Adding and removing users</span></span>

<span data-ttu-id="854fb-138">Per aggiungere o rimuovere utenti da un gruppo, si chiama il [Aggiungi](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [rimuovere](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi e passare l'id connessione dell'utente e il nome del gruppo come parametri.</span><span class="sxs-lookup"><span data-stu-id="854fb-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="854fb-139">Non è necessario rimuovere manualmente un utente da un gruppo, al termine della connessione.</span><span class="sxs-lookup"><span data-stu-id="854fb-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="854fb-140">Nell'esempio seguente il `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="854fb-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="854fb-141">Il `Groups.Add` e `Groups.Remove` metodi eseguire in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="854fb-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="854fb-142">Se si desidera aggiungere un client a un gruppo e invia immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo Groups.Add termina per prima.</span><span class="sxs-lookup"><span data-stu-id="854fb-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="854fb-143">Gli esempi di codice seguente viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="854fb-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="854fb-144">In generale, non è necessario includere `await` quando si chiama il `Groups.Remove` metodo perché l'id di connessione che si sta tentando di rimuovere potrebbe non essere più disponibile.</span><span class="sxs-lookup"><span data-stu-id="854fb-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="854fb-145">In tal caso, `TaskCanceledException` dopo la richiesta scade, viene generata un'eccezione. Se l'applicazione deve verificare che l'utente è stato rimosso dal gruppo prima di inviare un messaggio al gruppo, è possibile aggiungere `await` prima Groups.Remove e quindi catch il `TaskCanceledException` eccezione che potrebbe essere generata.</span><span class="sxs-lookup"><span data-stu-id="854fb-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="854fb-146">La chiamata di membri di un gruppo</span><span class="sxs-lookup"><span data-stu-id="854fb-146">Calling members of a group</span></span>

<span data-ttu-id="854fb-147">È possibile inviare messaggi a tutti i membri di un gruppo o solo i membri specificati del gruppo, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="854fb-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="854fb-148">**Tutti** connessi i client in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="854fb-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="854fb-149">Tutti i client in un gruppo specificato connessi **tranne client specificati**, identificato dall'ID connessione.</span><span class="sxs-lookup"><span data-stu-id="854fb-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="854fb-150">Tutti i client in un gruppo specificato connessi **eccetto il client chiamante**.</span><span class="sxs-lookup"><span data-stu-id="854fb-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="854fb-151">L'appartenenza al gruppo di archiviazione in un database</span><span class="sxs-lookup"><span data-stu-id="854fb-151">Storing group membership in a database</span></span>

<span data-ttu-id="854fb-152">Nell'esempio seguente viene illustrato come mantenere le informazioni utente e di gruppo in un database.</span><span class="sxs-lookup"><span data-stu-id="854fb-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="854fb-153">È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, nell'esempio seguente viene illustrato come definire i modelli tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="854fb-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="854fb-154">Questi modelli di entità corrispondono ai campi e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="854fb-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="854fb-155">La struttura dei dati può variare notevolmente a seconda dei requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="854fb-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="854fb-156">In questo esempio include una classe denominata `ConversationRoom` che sono univoci a un'applicazione che consente agli utenti di partecipare a conversazioni relativi ad argomenti diversi, ad esempio sports o garden.</span><span class="sxs-lookup"><span data-stu-id="854fb-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="854fb-157">In questo esempio include anche una classe per le connessioni.</span><span class="sxs-lookup"><span data-stu-id="854fb-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="854fb-158">La classe di connessione non è assolutamente necessaria per l'appartenenza al gruppo di rilevamento, ma spesso è parte della soluzione efficace per tenere traccia degli utenti.</span><span class="sxs-lookup"><span data-stu-id="854fb-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="854fb-159">Quindi, nell'hub, è possibile recuperare le informazioni di gruppo e utente dal database e aggiungere manualmente l'utente ai gruppi appropriati.</span><span class="sxs-lookup"><span data-stu-id="854fb-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="854fb-160">L'esempio non include codice per tenere traccia delle connessioni utente.</span><span class="sxs-lookup"><span data-stu-id="854fb-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="854fb-161">In questo esempio, il `await` parola chiave non viene applicata prima `Groups.Add` perché un messaggio non viene inviato immediatamente ai membri del gruppo.</span><span class="sxs-lookup"><span data-stu-id="854fb-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="854fb-162">Se si desidera inviare un messaggio a tutti i membri del gruppo immediatamente dopo l'aggiunta del nuovo membro, si desidera applicare il `await` (parola chiave) per assicurarsi che l'operazione asincrona è stata completata.</span><span class="sxs-lookup"><span data-stu-id="854fb-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="854fb-163">L'appartenenza al gruppo di archiviazione nell'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="854fb-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="854fb-164">Utilizzo di archiviazione tabelle di Azure per archiviare informazioni utente e gruppo è simile all'utilizzo di un database.</span><span class="sxs-lookup"><span data-stu-id="854fb-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="854fb-165">L'esempio seguente mostra un'entità di tabella che archivia il nome utente e il nome del gruppo.</span><span class="sxs-lookup"><span data-stu-id="854fb-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="854fb-166">Nell'hub, recuperare i gruppi assegnati quando l'utente si connette.</span><span class="sxs-lookup"><span data-stu-id="854fb-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="854fb-167">Verifica dell'appartenenza al gruppo durante la riconnessione</span><span class="sxs-lookup"><span data-stu-id="854fb-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="854fb-168">Per impostazione predefinita, SignalR nuovamente assegna automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene eliminata e nuovamente stabilita prima del timeout della connessione. Informazioni di gruppo dell'utente vengono passate in un token durante la riconnessione e tale token viene verificato sul server.</span><span class="sxs-lookup"><span data-stu-id="854fb-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="854fb-169">Per informazioni sul processo di verifica per la nuova partecipazione a gruppi di utenti, vedere [nuova partecipazione a gruppi durante la riconnessione](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="854fb-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="854fb-170">In generale, utilizzare il comportamento predefinito di automaticamente nuova partecipazione a gruppi in riconnessione.</span><span class="sxs-lookup"><span data-stu-id="854fb-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="854fb-171">Gruppi di SignalR non servono come meccanismo di sicurezza per limitare l'accesso ai dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="854fb-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="854fb-172">Tuttavia, se l'applicazione deve controllare l'appartenenza al gruppo un utente durante la riconnessione, è possibile ignorare il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="854fb-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="854fb-173">Modifica del comportamento predefinito può aggiungere un carico di lavoro al database perché l'appartenenza al gruppo dell'utente deve essere recuperato per ogni riconnessione anziché solo quando l'utente si connette.</span><span class="sxs-lookup"><span data-stu-id="854fb-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="854fb-174">Se è necessario verificare l'appartenenza al gruppo in riconnettersi, creare un nuovo modulo di pipeline di hub che restituisce un elenco di gruppi assegnati, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="854fb-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="854fb-175">Aggiungere quindi tale modulo alla pipeline dell'hub, come evidenziato qui sotto.</span><span class="sxs-lookup"><span data-stu-id="854fb-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
