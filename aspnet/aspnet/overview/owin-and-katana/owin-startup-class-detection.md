---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Rilevamento di classe di avvio OWIN | Documenti Microsoft
author: Praburaj
description: "In questa esercitazione viene illustrato come configurare la classe di avvio OWIN viene caricata. Per ulteriori informazioni su OWIN, vedere una panoramica del progetto Katana. In questa esercitazione è stata..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: a6ac34307b7558ad13684448f339ca74ade9e997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="owin-startup-class-detection"></a>Rilevamento di classe avvio OWIN
====================
da [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione viene illustrato come configurare la classe di avvio OWIN viene caricata. Per ulteriori informazioni su OWIN, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md). In questa esercitazione è stato scritto dal Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan e Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Prerequisiti
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Rilevamento di classe avvio OWIN

 Ogni applicazione OWIN dispone di una classe di avvio in cui è necessario specificare i componenti per la pipeline dell'applicazione. Esistono diversi modi, è possibile collegare la classe di avvio con il runtime, a seconda del modello di hosting è scegliere (OwinHost, IIS e IIS Express). La classe di avvio illustrata in questa esercitazione è utilizzabile in ogni applicazione di hosting. Ci si connette la classe di avvio con il runtime hosting usando uno di questi approcci:  

1. **Convenzione di denominazione**: Katana esegue la ricerca di una classe denominata `Startup` nello spazio dei nomi corrispondenti al nome di assembly o lo spazio dei nomi globale.
2. **Attributo OwinStartup**: si tratta dell'approccio richiederà maggior parte degli sviluppatori per specificare la classe di avvio. L'attributo seguente verrà impostata la classe di avvio di `TestStartup` classe il `StartupDemo` dello spazio dei nomi. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 Il `OwinStartup` attributo prevarrà sulla convenzione di denominazione. È inoltre possibile specificare un nome descrittivo con questo attributo, tuttavia, utilizzando un nome descrittivo è necessario utilizzare anche il `appSetting` elemento nel file di configurazione.
3. **L'elemento appSetting nel file di configurazione**: il `appSetting` elemento esegue l'override di `OwinStartup` convenzione di denominazione e di attributo. È possibile avere più classi di avvio (ogni usando un `OwinStartup` attributo) e configurare la classe di avvio verrà caricata in un file di configurazione utilizzando codice simile al seguente:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 Consente inoltre la chiave seguente, che specifica in modo esplicito la classe di avvio e l'assembly: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 Il seguente codice XML nel file di configurazione specifica un nome della classe di avvio descrittivo `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 Il codice precedente deve essere utilizzato con le operazioni seguenti `OwinStartup` attributo che specifica un nome descrittivo e fa sì che la `ProductionStartup2` classe per l'esecuzione.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Per disabilitare il rilevamento di avvio OWIN aggiungere il `appSetting owin:AutomaticAppStartup` con un valore di `"false"` nel file Web. config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Creare un'App Web ASP.NET tramite l'avvio di OWIN

1. Creare un'applicazione web Asp.Net vuota e denominarla **StartupDemo**. -Installare `Microsoft.Owin.Host.SystemWeb` tramite Gestione pacchetti NuGet. Dal **strumenti** dal menu **Gestione pacchetti libreria**e quindi **Package Manager Console**. Immettere il comando seguente:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- Aggiungere una classe di avvio OWIN. In Visual Studio 2013 destro fare clic sul progetto e selezionare **Aggiungi classe**. - In di **Aggiungi nuovo elemento** finestra di dialogo immettere *OWIN* nel campo di ricerca e modificare il nome in Startup.cs, e quindi fare clic su **Aggiungi**.  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 Alla successiva che si desidera aggiungere un *classe di avvio di Owin*, sarà disponibile il **Aggiungi** menu.  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 In alternativa, è possibile fare clic con il pulsante destro del progetto e selezionare **Aggiungi**, quindi selezionare **nuovo elemento**, quindi selezionare il **classe di avvio di Owin**.  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- Sostituire il codice generato nei componenti di *Startup.cs* file con le operazioni seguenti:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 Il `app.Use` espressione lambda viene utilizzata per registrare il componente specificato middleware alla pipeline OWIN. In questo caso viene impostata la registrazione delle richieste in ingresso prima di rispondere alla richiesta in ingresso. Il `next` parametro è il delegato ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [attività](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) al componente successivo nella pipeline. Il `app.Run` espressione lambda associa la pipeline di richieste in ingresso e fornisce il meccanismo di risposta.
     > [!NOTE]
     > Nel codice precedente è stata impostata come commento il `OwinStartup` attributo e ci stiamo affidarsi la convenzione di in esecuzione la classe denominata `Startup` .-premere ***F5*** per eseguire l'applicazione. Fare clic su Aggiorna più volte.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
Nota: Il numero indicato tra le immagini in questa esercitazione non corrisponderà al numero viene visualizzato. La stringa di millisecondo viene utilizzata per visualizzare una nuova risposta quando si aggiorna la pagina.  
 È possibile visualizzare le informazioni della traccia di **Output** finestra.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Aggiungere ulteriori classi di avvio

In questa sezione si aggiungerà un'altra classe di avvio. È possibile aggiungere più classe di avvio OWIN per l'applicazione. Potrebbe ad esempio, si desidera creare classi di avvio per lo sviluppo, test e produzione.

1. Creare una nuova classe di avvio di OWIN e denominarla `ProductionStartup`.
2. Sostituire il codice generato con il seguente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Premere CTRL F5 per eseguire l'app. Il `OwinStartup` attributo specifica la classe di avvio di produzione viene eseguita.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Creare un'altra classe di avvio di OWIN e denominarla `TestStartup`.
5. Sostituire il codice generato con il seguente:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 Il `OwinStartup` overload dell'attributo precedente specifica `TestingConfiguration` come il *descrittivo* nome della classe di avvio.
6. Aprire il *Web. config* file e aggiungere la chiave di avvio di OWIN App che specifica il nome descrittivo della classe di avvio:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Premere CTRL F5 per eseguire l'app. L'elemento impostazioni app accetta precedenti e il test di configurazione viene eseguita.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Rimuovere il *descrittivo* nome dal `OwinStartup` attributo la `TestStartup` classe.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Sostituire la chiave di avvio applicazione OWIN il *Web. config* file con le operazioni seguenti:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Ripristinare il `OwinStartup` attributo in ogni classe per il codice di attributo predefinito generato da Visual Studio:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 Tutte le chiavi di avvio di OWIN App seguente della causerà l'esecuzione della classe di produzione. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 La chiave di avvio ultima specifica il metodo di configurazione di avvio. La seguente chiave di avvio di OWIN App consente di modificare il nome della classe di configurazione per `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Utilizzando Owinhost.exe

1. Sostituire il file Web. config con il markup seguente:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 L'ultima chiave wins, pertanto in questo caso `TestStartup` specificato.
2. Installare Owinhost da PMC: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Passare alla cartella dell'applicazione (la cartella contenente il *Web. config* file) e in un prompt dei comandi e tipo: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 Verrà visualizzata la finestra di comando: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Avviare un browser con URL `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 OwinHost rispettate le convenzioni di avvio elencate in precedenza.
5. Nella finestra di comando, premere INVIO per uscire dall'installazione OwinHost.
6. Nel `ProductionStartup` classe, aggiungere l'attributo OwinStartup seguente che specifica un nome descrittivo del *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Nel prompt dei comandi e digitare: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 Viene caricata la classe di avvio di produzione.  
    ![](owin-startup-class-detection/_static/image9.png)  
 L'applicazione dispone di più classi di avvio e in questo esempio è stata posticipata le classi di avvio da caricare fino al runtime.
8. Verificare le seguenti opzioni di avvio di runtime:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
