---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-hosting ASP.NET Web API 1 (c#) | Documenti Microsoft
author: MikeWasson
description: "ASP.NET Web API non richiede IIS. Automatica, è possibile ospitare un'API web nel proprio processo host. In questa esercitazione viene illustrato come ospitare un'API web all'interno di una console applic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: b308ee9ec209ba8bbb021827655c83443dd149e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="self-host-aspnet-web-api-1-c"></a>Self-hosting ASP.NET Web API 1 (c#)
====================
da [Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API non richiede IIS. Automatica, è possibile ospitare un'API web nel proprio processo host. In questa esercitazione viene illustrato come ospitare un'API web all'interno di un'applicazione console.
> 
> **Nuove applicazioni devono utilizzare OWIN per l'hosting indipendente API Web.** Vedere [utilizzare OWIN per l'hosting indipendente ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - API Web 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Creare il progetto di applicazione Console

Avviare Visual Studio e selezionare **nuovo progetto** dal **avviare** pagina. O dal **File** dal menu **New** e quindi **progetto**.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#**selezionare **Windows**. Nell'elenco dei modelli di progetto, selezionare **applicazione Console**. Denominare il progetto &quot;SelfHost&quot; e fare clic su **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Impostare il Framework di destinazione (Visual Studio 2010)

Se si utilizza Visual Studio 2010, è possibile modificare il framework di destinazione in .NET Framework 4.0. (Per impostazione predefinita, le destinazioni di modello di progetto di [del profilo di .net Framework Client](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

In Esplora soluzioni, fare clic sul progetto e selezionare **proprietà**. Nel **framework di destinazione** elenco a discesa elenco, modificare il framework di destinazione in .NET Framework 4.0. Quando viene richiesto di applicare la modifica, fare clic su **Sì**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Installare Gestione pacchetti NuGet

Gestione pacchetti NuGet è il modo più semplice per aggiungere gli assembly di API Web a un progetto non ASP.NET.

Per verificare se è installato Gestione pacchetti NuGet, fare clic su di **strumenti** menu in Visual Studio. Se viene visualizzato un menu chiamato **Gestione pacchetti libreria**, è necessario Gestione pacchetti NuGet.

Per installare Gestione pacchetti NuGet:

1. Avviare Visual Studio.
2. Dal **strumenti** dal menu **estensioni e aggiornamenti**.
3. Nel **estensioni e aggiornamenti** finestra di dialogo Seleziona **Online**.
4. Se non viene visualizzato "Gestione pacchetti NuGet", digitare "Gestione pacchetti nuget" nella casella di ricerca.
5. Selezionare Gestione pacchetti NuGet e fare clic su **scaricare**.
6. Al termine del download, verrà richiesto di installare.
7. Al termine dell'installazione, potrebbe essere necessario riavviare Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Aggiungere il pacchetto NuGet di API Web

Dopo l'installazione di gestione pacchetti NuGet, aggiungere il pacchetto di Web API Self-Host al progetto.

1. Dal **strumenti** dal menu **Gestione pacchetti libreria**. *Nota*: se si menu non viene visualizzato questo elemento, assicurarsi che tale gestione pacchetti NuGet è installato correttamente.
2. Selezionare **Gestisci pacchetti NuGet per la soluzione...**
3. Nel **Gestisci pacchetti NugGet** finestra di dialogo Seleziona **Online**.
4. Nella casella di ricerca, digitare &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Selezionare il pacchetto di ASP.NET Web API Self Host e fare clic su **installare**.
6. Dopo aver installato il pacchetto, fare clic su **chiudere** per chiudere la finestra di dialogo.

> [!NOTE]
> Assicurarsi di installare il pacchetto denominato Microsoft.AspNet.WebApi.SelfHost, non AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Creare il modello e il Controller

In questa esercitazione Usa le classi di modello e il controller stesso come il [Introduzione](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione.

Aggiungere una classe pubblica denominata `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Aggiungere una classe pubblica denominata `ProductsController`. Derivare la classe da **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Per ulteriori informazioni sul codice in questo controller, vedere il [Introduzione](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione. Il controller definisce tre operazioni GET:

| URI | Descrizione |
| --- | --- |
| prodotti/api / | Ottenere un elenco di tutti i prodotti. |
| /API/prodotti/*id* | Ottenere un prodotto in base all'ID. |
| /API/prodotti /? categoria =*categoria* | Ottenere un elenco di prodotti per categoria. |

## <a name="host-the-web-api"></a>L'API Web host

Aprire il file Program.cs e aggiungere le seguenti istruzioni using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Aggiungere il codice seguente per il **programma** classe.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Facoltativo) Aggiungere una prenotazione di Namespace URL HTTP

Questa applicazione è in ascolto `http://localhost:8080/`. Per impostazione predefinita, l'ascolto su un particolare indirizzo HTTP richiede privilegi di amministratore. Quando si esegue l'esercitazione, pertanto, potrebbe verificarsi questo errore: "HTTP può non registrare l'URL http://+:8080/" sono disponibili due modi per evitare questo errore:

- Eseguire Visual Studio con autorizzazioni di amministratore con privilegi elevati, o
- Utilizzare Netsh.exe per concedere le autorizzazioni dell'account per riservare l'URL.

Per l'utilizzo di Netsh.exe, aprire un prompt dei comandi con privilegi di amministratore e immettere il seguente comando: seguente comando:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

dove *computer\nome utente* è l'account utente.

Quando si è finito self-hosting, assicurarsi di eliminare la prenotazione:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Chiamare l'API Web da un'applicazione Client (c#)

Consente di scrivere una semplice applicazione console che chiama l'API web.

Aggiungere un nuovo progetto applicazione console alla soluzione:

- In Esplora soluzioni, la soluzione e scegliere **Aggiungi nuovo progetto**.
- Creare una nuova applicazione console denominata &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Utilizzo di gestione pacchetti di NuGet per aggiungere il pacchetto di ASP.NET Web API Core Libraries:

- Dal menu Strumenti, selezionare **Gestione pacchetti libreria**.
- Selezionare **Gestisci pacchetti NuGet per la soluzione...**
- Nel **Gestisci pacchetti NuGet** finestra di dialogo Seleziona **Online**.
- Nella casella di ricerca, digitare &quot;webapi&quot;.
- Selezionare il pacchetto di librerie Client di Microsoft ASP.NET Web API e fare clic su **installare**.

Aggiungere un riferimento in ClientApp al progetto SelfHost:

- In Esplora soluzioni, fare clic sul progetto ClientApp.
- Selezionare **aggiungere riferimento**.
- Nel **gestione riferimenti** finestra di dialogo, in **soluzione**selezionare **progetti**.
- Selezionare il progetto host.
- Fare clic su **OK**.

![](self-host-a-web-api/_static/image6.png)

Aprire il file Client/Program.cs. Aggiungere il seguente **utilizzando** istruzione:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Aggiungere un valore statico **HttpClient** istanza:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Aggiungere i metodi seguenti per elencare tutti i prodotti, un prodotto dall'ID di elenco ed elencano i prodotti per categoria.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Ognuno di questi metodi segue il modello stesso:

1. Chiamare **HttpClient.GetAsync** per inviare una richiesta GET all'URI appropriato.
2. Chiamare **HttpResponseMessage.EnsureSuccessStatusCode**. Questo metodo genera un'eccezione se lo stato della risposta HTTP è un codice di errore.
3. Chiamare **ReadAsAsync&lt;T&gt;**  per deserializzare un tipo CLR dalla risposta HTTP. Questo metodo è un metodo di estensione definito in **System.Net.Http.HttpContentExtensions**.

Il **GetAsync** e **ReadAsAsync** metodi sono entrambi asincrona. Restituiscono **attività** gli oggetti che rappresentano l'operazione asincrona. Recupero di **risultato** proprietà blocca il thread fino al completamento dell'operazione.

Per ulteriori informazioni sull'utilizzo HttpClient, inclusa la modalità di chiamate non di blocco, vedere [la chiamata a una Web API da un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Prima di chiamare questi metodi, impostare la proprietà BaseAddress sull'istanza HttpClient per "`http://localhost:8080`". Ad esempio:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Questo dovrebbe output riportato di seguito. (Ricordarsi di avviare l'applicazione host).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
