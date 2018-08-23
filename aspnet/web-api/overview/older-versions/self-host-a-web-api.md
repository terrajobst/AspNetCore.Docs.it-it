---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-hosting di API Web ASP.NET 1 (c#) | Microsoft Docs
author: MikeWasson
description: API Web ASP.NET non sia necessario IIS. È possibile self-ospitare un'API web nel proprio processo host. Questa esercitazione illustra come ospitare un'API web all'interno di una console UT...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: cac0d5aeaf49f45051d062935e0e9207ce59c7eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835287"
---
<a name="self-host-aspnet-web-api-1-c"></a>Self-hosting di API Web ASP.NET 1 (c#)
====================
da [Mike Wasson](https://github.com/MikeWasson)

> API Web ASP.NET non sia necessario IIS. È possibile self-ospitare un'API web nel proprio processo host. Questa esercitazione illustra come ospitare un'API web all'interno di un'applicazione console.
> 
> **Le nuove applicazioni devono usare OWIN per l'hosting indipendente di API Web.** Visualizzare [usare OWIN per l'hosting indipendente di API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - API Web 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Creare il progetto di applicazione Console

Avviare Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina. E viceversa, il **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Windows**. Nell'elenco dei modelli di progetto, selezionare **applicazione Console**. Denominare il progetto &quot;SelfHost&quot; e fare clic su **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Impostare il Framework di destinazione (Visual Studio 2010)

Se si usa Visual Studio 2010, modificare il framework di destinazione a .NET Framework 4.0. (Per impostazione predefinita, le destinazioni di modello di progetto di [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

In Esplora soluzioni fare clic sul progetto e selezionare **proprietà**. Nel **framework di destinazione** elenco a discesa elenco, modificare il framework di destinazione a .NET Framework 4.0. Quando viene richiesto di applicare la modifica, fare clic su **Sì**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Installare Gestione pacchetti NuGet

Gestione pacchetti NuGet è il modo più semplice per aggiungere gli assembly di API Web a un progetto non ASP.NET.

Per verificare se è installato Gestione pacchetti NuGet, scegliere il **strumenti** menu di Visual Studio. Se viene visualizzato un menu chiamato **Library Package Manager**, è necessario Gestione pacchetti NuGet.

Per installare Gestione pacchetti NuGet:

1. Avviare Visual Studio.
2. Dal **degli strumenti** dal menu **estensioni e aggiornamenti**.
3. Nel **estensioni e aggiornamenti** finestra di dialogo, seleziona **Online**.
4. Se non viene visualizzato "Gestisci pacchetti NuGet", digitare "nuget package manager" nella casella di ricerca.
5. Selezionare Gestisci pacchetti NuGet e fare clic su **scaricare**.
6. Al termine del download, verrà chiesto di installare.
7. Al termine dell'installazione, potrebbe essere necessario riavviare Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Aggiungere il pacchetto NuGet dell'API Web

Dopo aver installato Gestione pacchetti NuGet, aggiungere il pacchetto dell'API Web al progetto.

1. Dal **degli strumenti** dal menu **Library Package Manager**. *Nota*: se non possibile visualizzare il menu di elemento, assicurarsi che Gestione pacchetti NuGet è installato correttamente.
2. Selezionare **Gestisci pacchetti NuGet per la soluzione...**
3. Nel **Gestisci pacchetti NugGet** finestra di dialogo, seleziona **Online**.
4. Nella casella di ricerca, digitare &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Selezionare il pacchetto di ASP.NET Web API Self Host e fare clic su **installare**.
6. Dopo aver installato il pacchetto, fare clic su **chiudere** per chiudere la finestra di dialogo.

> [!NOTE]
> Assicurarsi di installare il pacchetto denominato Microsoft.AspNet.WebApi.SelfHost, non AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Creare il modello e Controller

Questa esercitazione Usa le stesse classi del modello e controller come il [introduttiva](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione.

Aggiungere una classe pubblica denominata `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Aggiungere una classe pubblica denominata `ProductsController`. Questa classe da derivare **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Per altre informazioni sul codice in questo controller, vedere la [introduttiva](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione. Questo controller definisce tre operazioni GET:

| URI | Descrizione |
| --- | --- |
| prodotti/api / | Ottenere un elenco di tutti i prodotti. |
| /api/products/*id* | Ottenere un prodotto base all'ID. |
| /api/products/?category=*category* | Ottenere un elenco di prodotti per categoria. |

## <a name="host-the-web-api"></a>Ospitare l'API Web

Aprire il file Program.cs e aggiungere quanto segue usando istruzioni:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Aggiungere il codice seguente per il **programma** classe.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Facoltativo) Aggiungere una prenotazione di Namespace URL HTTP

Questa applicazione ascolta `http://localhost:8080/`. Per impostazione predefinita, in attesa in un particolare indirizzo HTTP richiede privilegi di amministratore. Quando si esegue l'esercitazione, pertanto, possibile che si verifichi questo errore: "HTTP non è stato possibile registrare l'URL http://+:8080/" sono disponibili due modi per evitare questo errore:

- Eseguire Visual Studio con autorizzazioni di amministratore con privilegi elevati, o
- Usare Netsh.exe per concedere le autorizzazioni dell'account per riservare l'URL.

Per usare Netsh.exe, aprire un prompt dei comandi con privilegi di amministratore e immettere il comando seguente: comando seguente:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

in cui *computer\nome utente* è l'account utente.

Al termine self-hosting, assicurarsi di eliminare la prenotazione:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Chiamare l'API Web da un'applicazione Client (c#)

È possibile scrivere una semplice applicazione console che chiama l'API web.

Aggiungere un nuovo progetto applicazione console alla soluzione:

- In Esplora soluzioni fare doppio clic la soluzione e selezionare **Aggiungi nuovo progetto**.
- Creare una nuova applicazione console denominata &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Usare Gestione pacchetti NuGet per aggiungere il pacchetto di librerie di base di ASP.NET Web API:

- Dal menu Strumenti, selezionare **Library Package Manager**.
- Selezionare **Gestisci pacchetti NuGet per la soluzione...**
- Nel **Gestisci pacchetti NuGet** finestra di dialogo, seleziona **Online**.
- Nella casella di ricerca, digitare &quot;ASPNET&quot;.
- Selezionare il pacchetto di librerie Client di Microsoft ASP.NET Web API e fare clic su **installare**.

Aggiungere un riferimento in ClientApp al progetto SelfHost:

- In Esplora soluzioni fare doppio clic su progetto ClientApp.
- Selezionare **Aggiungi riferimento**.
- Nel **gestione riferimenti** finestra di dialogo, sotto **soluzione**, selezionare **progetti**.
- Selezionare il progetto SelfHost.
- Fare clic su **OK**.

![](self-host-a-web-api/_static/image6.png)

Aprire il file Client/Program.cs. Aggiungere il codice seguente **usando** istruzione:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Aggiungere un valore statico **HttpClient** istanza:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Aggiungere i metodi seguenti per elencare tutti i prodotti, elenco di ID di un prodotto ed elencano i prodotti per categoria.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Ognuno di questi metodi segue lo stesso modello:

1. Chiamare **HttpClient.GetAsync** per inviare una richiesta GET all'URI appropriato.
2. Chiamare **HttpResponseMessage.EnsureSuccessStatusCode**. Questo metodo genera un'eccezione se lo stato della risposta HTTP è un codice di errore.
3. Chiamare **ReadAsAsync&lt;T&gt;**  per deserializzare un tipo CLR dalla risposta HTTP. Questo metodo è un metodo di estensione definito in **System.Net.Http.HttpContentExtensions**.

Il **GetAsync** e **ReadAsAsync** metodi sono entrambi sono asincroni. Restituiscono **attività** gli oggetti che rappresentano l'operazione asincrona. Ottenere il **risultato** proprietà blocca il thread fino al completamento dell'operazione.

Per altre informazioni sull'uso di HttpClient, incluse le procedure effettuare chiamate senza blocchi, vedere [chiamata di una Web API da un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Prima di chiamare questi metodi, impostare la proprietà BaseAddress sull'istanza di HttpClient per "`http://localhost:8080`". Ad esempio:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Questa deve restituire il codice seguente. (Ricordarsi di eseguire l'applicazione SelfHost prima).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
