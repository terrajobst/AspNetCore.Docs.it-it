---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Rilevamento della classe di avvio OWIN | Microsoft Docs
author: Praburaj
description: Questa esercitazione illustra come configurare la classe di avvio OWIN viene caricata. Per altre informazioni su OWIN, vedere una panoramica del progetto Katana. Questa esercitazione è stata...
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0a4b87192296054bf6aef6c9406c64f19677a061
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828293"
---
<a name="owin-startup-class-detection"></a>Rilevamento della classe di avvio OWIN
====================
dal [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Questa esercitazione illustra come configurare la classe di avvio OWIN viene caricata. Per altre informazioni su OWIN, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md). Questa esercitazione è stato scritto da Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan così Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Prerequisiti
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Rilevamento della classe di avvio OWIN

 Ogni applicazione OWIN dispone di una classe di avvio in cui è specificare i componenti della pipeline dell'applicazione. Esistono diversi modi, è possibile connettere la classe di avvio con il runtime, a seconda del modello di hosting scelto (OwinHost, IIS e IIS Express). La classe di avvio illustrata in questa esercitazione può essere utilizzata in ogni applicazione di hosting. La classe startup è connettersi con il runtime hosting usando uno di questi approcci:  

1. **Convenzione di denominazione**: esegue la ricerca di una classe denominata Katana `Startup` nello spazio dei nomi corrispondenti al nome dell'assembly o lo spazio dei nomi globale.
2. **Attributo OwinStartup**: si tratta dell'approccio richiederà maggior parte degli sviluppatori per specificare la classe di avvio. L'attributo seguente imposterà classe di avvio il `TestStartup` classe la `StartupDemo` dello spazio dei nomi. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   Il `OwinStartup` attributo sostituisce la convenzione di denominazione. È anche possibile specificare un nome descrittivo con questo attributo, tuttavia, utilizzando un nome descrittivo richiede che venga utilizzato anche il `appSetting` elemento nel file di configurazione.
3. **L'elemento appSetting nel file di configurazione**: il `appSetting` esegue l'override di elemento di `OwinStartup` attributo e la convenzione di denominazione. È possibile avere più classi di avvio (ogni usando un `OwinStartup` attributo) e configurare la classe di avvio verrà caricata in un file di configurazione tramite markup simile al seguente:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   Consente inoltre la chiave seguente, che specifica in modo esplicito la classe di avvio e l'assembly: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Il codice XML seguente nel file di configurazione specifica un nome della classe di avvio descrittivo `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Il markup riportato sopra deve essere usato con quanto segue `OwinStartup` attributo che specifica un nome descrittivo e fa sì che il `ProductionStartup2` classe per l'esecuzione.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Per disabilitare il rilevamento di avvio OWIN, aggiungere il `appSetting owin:AutomaticAppStartup` con il valore `"false"` nel file Web. config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Creare un'App Web ASP.NET con OWIN Startup

1. Creare un'applicazione web Asp.Net vuota e denominarla **StartupDemo**. -Installare `Microsoft.Owin.Host.SystemWeb` usando Gestione pacchetti NuGet. Dal **degli strumenti** dal menu **Library Package Manager**e quindi **Package Manager Console**. Immettere il comando seguente:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Aggiungere una classe di avvio OWIN. In Visual Studio 2013 con il pulsante destro fare clic con il progetto e selezionare **Aggiungi classe**. - nel **Aggiungi nuovo elemento** finestra di dialogo immettere *OWIN* nel campo di ricerca e modificare il nome in Startup.cs, e quindi fare clic su **Add**.  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   La volta successiva che si desidera aggiungere un *Owin Startup class*, sarà disponibile dal **Add** menu.  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   In alternativa, è possibile fare clic con il pulsante destro del progetto e selezionare **Add**, quindi selezionare **nuovo elemento**, quindi selezionare il **Owin Startup class**.  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- Sostituire il codice generato nel *Startup.cs* file con il codice seguente:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  Il `app.Use` espressione lambda viene usata per registrare il componente specificato middleware alla pipeline OWIN. In questo caso viene impostata la registrazione delle richieste in ingresso prima di rispondere alla richiesta in ingresso. Il `next` parametro è il delegato ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [attività](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) al componente successivo nella pipeline. Il `app.Run` espressione lambda associa alla pipeline di richieste in ingresso e fornisce il meccanismo di risposta.
     > [!NOTE]
     > Nel codice precedente è stata impostata come commento il `OwinStartup` attributo e si sta affidarsi alla convenzione in esecuzione la classe denominata `Startup` .-premere ***F5*** per eseguire l'applicazione. Fare clic su Aggiorna più volte.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  Nota: Il numero illustrato nelle immagini in questa esercitazione non corrisponderanno il numero visualizzato. La stringa di millisecondo viene utilizzata per visualizzare una nuova risposta quando si aggiorna la pagina.  
  È possibile visualizzare le informazioni della traccia le **Output** finestra.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>L'aggiunta di altre classi di avvio

In questa sezione si aggiungerà un'altra classe di avvio. È possibile aggiungere più classe di avvio OWIN all'applicazione. Ad esempio, è possibile creare classi di avvio per lo sviluppo, test e produzione.

1. Creare una nuova classe OWIN Startup e denominarlo `ProductionStartup`.
2. Sostituire il codice generato con il seguente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Premere F5 di controllo per eseguire l'app. Il `OwinStartup` attributo specifica la classe di avvio di produzione viene eseguita.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Creare un'altra classe OWIN Startup e denominarla `TestStartup`.
5. Sostituire il codice generato con il seguente:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   Il `OwinStartup` overload attributo precedente specifica `TestingConfiguration` come la *descrittivo* nome della classe Startup.
6. Aprire il *Web. config* file e aggiungere la chiave di avvio OWIN App che specifica il nome descrittivo della classe di avvio:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Premere F5 di controllo per eseguire l'app. L'elemento delle impostazioni di app ha la precedenza e il test di configurazione viene eseguita.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Rimuovere il *descrittivo* nome dal `OwinStartup` attributo di `TestStartup` classe.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Sostituire la chiave di avvio OWIN App le *Web. config* file con il codice seguente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Ripristinare il `OwinStartup` attributo in ogni classe per il codice di attributo predefinito generato da Visual Studio:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Ogni chiave di avvio OWIN App seguente causerà l'esecuzione della classe di produzione. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    L'ultima chiave di avvio specifica il metodo di configurazione di avvio. La chiave di avvio OWIN App seguente consente di modificare il nome della classe configuration per `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Usando Owinhost.exe

1. Sostituire il file Web. config con il markup seguente:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   La chiave dell'ultimo vince, pertanto in questo caso `TestStartup` è specificato.
2. Installare Owinhost dalla console di gestione pacchetti: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Passare alla cartella dell'applicazione (la cartella contenente il *Web. config* file) e in un prompt dei comandi e digitare: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Nella finestra di comando verrà visualizzato: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Avviare un browser con URL `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   OwinHost rispettate le convenzioni di avvio elencate in precedenza.
5. Nella finestra di comando, premere INVIO per uscire OwinHost.
6. Nel `ProductionStartup` classe, aggiungere l'attributo OwinStartup seguente che specifica un nome descrittivo del *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Nel prompt dei comandi e digitare: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   La classe di avvio di produzione viene caricata.  
    ![](owin-startup-class-detection/_static/image9.png)  
   L'applicazione dispone di più classi di avvio e in questo esempio sono stati posticipato quale classe di avvio per caricare fino al runtime.
8. Testare le opzioni di avvio di runtime seguenti:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
