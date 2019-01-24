---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Uso dei gruppi in SignalR | Microsoft Docs
author: bradygaster
description: In questo argomento viene descritto come rendere persistenti le informazioni di appartenenza al gruppo con l'API dell'Hub.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 384b7e5f07fa46ea3cc32e5c18c3c2327b7aedd3
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836805"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="ba57b-103">Uso dei gruppi in SignalR</span><span class="sxs-lookup"><span data-stu-id="ba57b-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="ba57b-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ba57b-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="ba57b-105">In questo argomento viene descritto come aggiungere utenti ai gruppi e rendere persistenti le informazioni di appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="ba57b-105">This topic describes how to add users to groups and persist group membership information.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="ba57b-106">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="ba57b-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="ba57b-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ba57b-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="ba57b-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ba57b-108">.NET 4.5</span></span>
> - <span data-ttu-id="ba57b-109">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="ba57b-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="ba57b-110">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="ba57b-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="ba57b-111">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="ba57b-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="ba57b-112">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="ba57b-112">Questions and comments</span></span>
>
> <span data-ttu-id="ba57b-113">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="ba57b-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ba57b-114">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="ba57b-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="ba57b-115">Panoramica</span><span class="sxs-lookup"><span data-stu-id="ba57b-115">Overview</span></span>

<span data-ttu-id="ba57b-116">Gruppi in SignalR forniscono un metodo per la trasmissione di messaggi per sottoinsiemi specifici di client connessi.</span><span class="sxs-lookup"><span data-stu-id="ba57b-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="ba57b-117">Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="ba57b-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="ba57b-118">Non è necessario creare in modo esplicito gruppi.</span><span class="sxs-lookup"><span data-stu-id="ba57b-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="ba57b-119">In effetti, la prima volta che specificarne il nome in una chiamata a Groups.Add viene automaticamente creato un gruppo, e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza al suo interno.</span><span class="sxs-lookup"><span data-stu-id="ba57b-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="ba57b-120">Per un'introduzione all'uso dei gruppi, vedere [come gestire l'appartenenza al gruppo dalla classe Hub](hubs-api-guide-server.md#groupsfromhub) nell'API di hub - Guida di Server.</span><span class="sxs-lookup"><span data-stu-id="ba57b-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="ba57b-121">Non è disponibile alcuna API per ottenere un elenco di appartenenza al gruppo o un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="ba57b-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="ba57b-122">SignalR invia messaggi al client e i gruppi basati su un modello pub/sub e il server non venga mantenuto un elenco di gruppi o appartenenze ai gruppi.</span><span class="sxs-lookup"><span data-stu-id="ba57b-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="ba57b-123">Ciò consente di ottimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="ba57b-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="ba57b-124">Quando si aggiunge un utente a un gruppo usando il `Groups.Add` metodo, l'utente riceve i messaggi indirizzati a tale gruppo per la durata della connessione corrente, ma l'appartenenza dell'utente in tale gruppo non viene mantenuto oltre la connessione corrente.</span><span class="sxs-lookup"><span data-stu-id="ba57b-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="ba57b-125">Se si desidera mantenere in modo permanente informazioni sui gruppi e appartenenza al gruppo, è necessario archiviare i dati in un repository, ad esempio un database o l'archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="ba57b-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="ba57b-126">Quindi, ogni volta che un utente si connette all'applicazione, recuperare dal repository di quali gruppi a cui appartiene l'utente e aggiungere manualmente l'utente a tali gruppi.</span><span class="sxs-lookup"><span data-stu-id="ba57b-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="ba57b-127">Quando si riconnette dopo un'interruzione temporanea, l'utente nuovamente unisce automaticamente i gruppi assegnati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ba57b-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="ba57b-128">Automaticamente la nuova partecipazione a un gruppo si applica solo quando la riconnessione, non quando si stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="ba57b-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="ba57b-129">Un token di firma digitale viene passato dal client che contiene l'elenco dei gruppi assegnati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ba57b-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="ba57b-130">Se si desidera verificare se l'utente appartiene a gruppi di richiesta, è possibile eseguire l'override del comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="ba57b-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="ba57b-131">Questo argomento include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ba57b-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="ba57b-132">Aggiunta e rimozione di utenti</span><span class="sxs-lookup"><span data-stu-id="ba57b-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="ba57b-133">La chiamata a membri di un gruppo</span><span class="sxs-lookup"><span data-stu-id="ba57b-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="ba57b-134">L'appartenenza al gruppo di archiviazione in un database</span><span class="sxs-lookup"><span data-stu-id="ba57b-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="ba57b-135">L'archiviazione l'appartenenza al gruppo di nell'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="ba57b-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="ba57b-136">Verifica per determinare se l'appartenenza al gruppo durante la riconnessione</span><span class="sxs-lookup"><span data-stu-id="ba57b-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="ba57b-137">Aggiunta e rimozione di utenti</span><span class="sxs-lookup"><span data-stu-id="ba57b-137">Adding and removing users</span></span>

<span data-ttu-id="ba57b-138">Per aggiungere o rimuovere utenti da un gruppo, si chiama il [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) oppure [rimuovere](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi e passare il nome del gruppo come parametri e l'id di connessione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ba57b-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="ba57b-139">Non devi rimuovere manualmente un utente da un gruppo, al termine della connessione.</span><span class="sxs-lookup"><span data-stu-id="ba57b-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="ba57b-140">L'esempio seguente mostra le `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="ba57b-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="ba57b-141">Il `Groups.Add` e `Groups.Remove` metodi eseguiti in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="ba57b-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="ba57b-142">Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo Groups.Add termina per prima.</span><span class="sxs-lookup"><span data-stu-id="ba57b-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="ba57b-143">Esempi di codice seguenti illustrano come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="ba57b-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="ba57b-144">In generale, non è necessario includere `await` quando si chiama il `Groups.Remove` metodo perché l'id di connessione che si sta tentando di rimuovere potrebbe non essere più disponibile.</span><span class="sxs-lookup"><span data-stu-id="ba57b-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="ba57b-145">In tal caso, `TaskCanceledException` viene generata dopo che la richiesta scade. Se l'applicazione deve verificare che l'utente è stato rimosso dal gruppo prima di inviare un messaggio al gruppo, è possibile aggiungere `await` prima `Groups.Remove`e quindi rilevare il `TaskCanceledException` eccezioni che potrebbero essere generate.</span><span class="sxs-lookup"><span data-stu-id="ba57b-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before `Groups.Remove`, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="ba57b-146">La chiamata a membri di un gruppo</span><span class="sxs-lookup"><span data-stu-id="ba57b-146">Calling members of a group</span></span>

<span data-ttu-id="ba57b-147">È possibile inviare messaggi a tutti i membri di un gruppo o solo i membri specificati del gruppo, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="ba57b-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="ba57b-148">**Tutti i** connessi i client in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="ba57b-148">**All** connected clients in a specified group.</span></span>

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="ba57b-149">Tutti i client in un gruppo specificato connessi **eccetto il client specificati**, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="ba57b-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span>

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="ba57b-150">Tutti i client in un gruppo specificato connessi **eccetto il client chiamante**.</span><span class="sxs-lookup"><span data-stu-id="ba57b-150">All connected clients in a specified group **except the calling client**.</span></span>

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="ba57b-151">L'appartenenza al gruppo di archiviazione in un database</span><span class="sxs-lookup"><span data-stu-id="ba57b-151">Storing group membership in a database</span></span>

<span data-ttu-id="ba57b-152">Gli esempi seguenti illustrano come mantenere le informazioni utente e gruppo in un database.</span><span class="sxs-lookup"><span data-stu-id="ba57b-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="ba57b-153">È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, l'esempio seguente viene illustrato come definire i modelli usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ba57b-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="ba57b-154">Questi modelli di entità corrispondono ai campi e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="ba57b-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="ba57b-155">Struttura dei dati potrebbe variare notevolmente a seconda dei requisiti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ba57b-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="ba57b-156">In questo esempio include una classe denominata `ConversationRoom` che potrebbe essere univoco per un'applicazione che consente agli utenti di aggiungersi conversazioni relativi ad argomenti diversi, ad esempio sportivo o garden.</span><span class="sxs-lookup"><span data-stu-id="ba57b-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="ba57b-157">Questo esempio include anche una classe per le connessioni.</span><span class="sxs-lookup"><span data-stu-id="ba57b-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="ba57b-158">La classe di connessione non è assolutamente necessaria per l'appartenenza al gruppo di rilevamento, ma è spesso parte di una soluzione efficace per tenere traccia degli utenti.</span><span class="sxs-lookup"><span data-stu-id="ba57b-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="ba57b-159">Quindi, nell'hub, è possibile recuperare le informazioni di gruppo e utente dal database e aggiungere manualmente l'utente ai gruppi appropriati.</span><span class="sxs-lookup"><span data-stu-id="ba57b-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="ba57b-160">L'esempio non include codice per tenere traccia delle connessioni utente.</span><span class="sxs-lookup"><span data-stu-id="ba57b-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="ba57b-161">In questo esempio, il `await` parola chiave non viene applicata prima `Groups.Add` perché un messaggio non viene inviato immediatamente ai membri del gruppo.</span><span class="sxs-lookup"><span data-stu-id="ba57b-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="ba57b-162">Se si desidera inviare un messaggio a tutti i membri del gruppo immediatamente dopo aver aggiunto il nuovo membro, si desidera applicare il `await` (parola chiave) per assicurarsi che l'operazione asincrona è stata completata.</span><span class="sxs-lookup"><span data-stu-id="ba57b-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="ba57b-163">L'archiviazione l'appartenenza al gruppo di nell'archiviazione tabelle di Azure</span><span class="sxs-lookup"><span data-stu-id="ba57b-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="ba57b-164">Utilizzo dell'archiviazione tabelle di Azure per archiviare le informazioni utente e gruppo è simile all'uso di un database.</span><span class="sxs-lookup"><span data-stu-id="ba57b-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="ba57b-165">L'esempio seguente illustra un'entità di tabella che archivia il nome utente e il nome del gruppo.</span><span class="sxs-lookup"><span data-stu-id="ba57b-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="ba57b-166">Nell'hub, recuperare i gruppi assegnati quando l'utente si connette.</span><span class="sxs-lookup"><span data-stu-id="ba57b-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="ba57b-167">Verifica per determinare se l'appartenenza al gruppo durante la riconnessione</span><span class="sxs-lookup"><span data-stu-id="ba57b-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="ba57b-168">Per impostazione predefinita, SignalR nuovamente assegna automaticamente un utente ai gruppi appropriati durante la riconnessione dopo un'interruzione temporanea, ad esempio quando una connessione viene eliminata e ristabilita prima del timeout della connessione. Informazioni sui gruppi dell'utente viene passati un token durante la riconnessione, e tale token viene verificato nel server.</span><span class="sxs-lookup"><span data-stu-id="ba57b-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="ba57b-169">Per informazioni sul processo di verifica per la nuova partecipazione a gruppi di utenti, vedere [la nuova partecipazione a gruppi durante la riconnessione](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="ba57b-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="ba57b-170">In generale, si dovrebbe usare il comportamento predefinito di automaticamente la nuova partecipazione a gruppi in riconnessione.</span><span class="sxs-lookup"><span data-stu-id="ba57b-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="ba57b-171">SignalR gruppi non sono intesi come meccanismo di sicurezza per limitare l'accesso ai dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="ba57b-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="ba57b-172">Tuttavia, se l'applicazione deve controllare l'appartenenza al gruppo dell'utente durante la riconnessione, è possibile eseguire l'override del comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="ba57b-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="ba57b-173">Modifica del comportamento predefinito può aggiungere un carico di lavoro al database perché l'appartenenza al gruppo dell'utente deve essere recuperato per ciascun riconnessione anziché solo quando l'utente si connette.</span><span class="sxs-lookup"><span data-stu-id="ba57b-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="ba57b-174">Se è necessario verificare l'appartenenza al gruppo in riconnessione, creare un nuovo modulo di pipeline dell'hub che restituisce un elenco dei gruppi assegnati, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ba57b-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="ba57b-175">Aggiungere quindi tale modulo alla pipeline dell'hub, come evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="ba57b-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
