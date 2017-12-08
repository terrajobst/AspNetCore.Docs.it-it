---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Convalida con i validator di annotazione dei dati (c#) | Documenti Microsoft
author: microsoft
description: "È possibile sfruttare il raccoglitore di modelli dati annotazione per la convalida all'interno di un'applicazione MVC ASP.NET. Informazioni su come utilizzare i diversi tipi di convalida..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 306dcb0197dfc9317ea9665dd2b1c058ba8bd712
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="validation-with-the-data-annotation-validators-c"></a>Convalida con i validator di annotazione dei dati (c#)
====================
da [Microsoft](https://github.com/microsoft)

> È possibile sfruttare il raccoglitore di modelli dati annotazione per la convalida all'interno di un'applicazione MVC ASP.NET. Informazioni su come utilizzare i diversi tipi di attributi di convalida e utilizzarli in Entity Framework Microsoft.


In questa esercitazione imparare a usare i validator di annotazione dei dati per eseguire la convalida in un'applicazione MVC ASP.NET. Il vantaggio di utilizzare i validator di annotazione dei dati è che consentono di eseguire la convalida semplicemente aggiungendo uno o più attributi, ad esempio la o attributo StringLength: per una proprietà di classe.

Prima di poter utilizzare i validator di annotazione dei dati, è necessario scaricare lo strumento di associazione dati modello di annotazioni. È possibile scaricare l'esempio di Raccoglitore modello annotazioni dati dal sito Web CodePlex facendo [qui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


È importante tenere presente che il gestore di associazione di dati le annotazioni del modello non è una parte del framework di MVC ASP.NET Microsoft ufficiale. Anche se il gestore di associazione di dati le annotazioni del modello è stato creato dal team di Microsoft ASP.NET MVC, Microsoft non offre supporto ufficiale del prodotto per il gestore di associazione dati modello annotazioni descritto e utilizzati in questa esercitazione.


## <a name="using-the-data-annotation-model-binder"></a>Utilizzando lo strumento di associazione dati modello annotazione

Per utilizzare lo strumento di associazione di dati le annotazioni del modello in un'applicazione ASP.NET MVC, è necessario prima aggiungere un riferimento all'assembly Microsoft.Web.Mvc.DataAnnotations.dll e l'assembly System.ComponentModel.DataAnnotations.dll. Selezionare l'opzione di menu **progetto, Aggiungi riferimento**. Fare clic su di **Sfoglia** scheda e passare al percorso in cui è stato scaricato (e decompressi) nell'esempio di Raccoglitore di modelli di dati le annotazioni (vedere **figura 1**).

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**Figura 1**: aggiunta di un riferimento al Binder di modello di dati le annotazioni ([fare clic per visualizzare l'immagine ingrandita](validation-with-the-data-annotation-validators-cs/_static/image3.png))

Selezionare l'assembly Microsoft.Web.Mvc.DataAnnotations.dll sia assembly DataAnnotations e fare clic su di **OK** pulsante.


È possibile utilizzare l'assembly DataAnnotations incluso in .NET Framework Service Pack 1 con lo strumento di associazione dati modello di annotazioni. È necessario utilizzare la versione dell'assembly DataAnnotations incluso con il download di esempio di Raccoglitore modello di dati le annotazioni.


Infine, è necessario registrare il gestore di associazione modello DataAnnotations nel file Global. asax. Aggiungere la riga di codice seguente all'applicazione\_gestore dell'evento Start () in modo che l'applicazione\_metodo Start () è simile al seguente:

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

Questa riga di codice registra il ataAnnotationsModelBinder come gestore di associazione del modello predefinito per l'intera applicazione MVC ASP.NET.

## <a name="using-the-data-annotation-validator-attributes"></a>Usando gli attributi di convalida di annotazione dei dati

Quando si utilizza lo strumento di associazione di dati le annotazioni del modello, utilizzare attributi di validator per eseguire la convalida. Lo spazio dei nomi System.ComponentModel.DataAnnotations include i seguenti attributi di convalida:

- Intervallo: consente di verificare se il valore di una proprietà è compreso tra un intervallo di valori specificato.
- RegularExpression: consente di verificare se il valore di una proprietà corrisponde a un criterio di espressione regolare specificata.
- Richiesto: consente di contrassegnare una proprietà come obbligatoria.
- StringLength: consente di specificare una lunghezza massima per una proprietà di stringa.
- Convalida-classe di base per tutti gli attributi di convalida.

> [!NOTE] 
> 
> Se le esigenze di convalida non vengono soddisfatti da uno qualsiasi dei validator standard sempre possibile la possibilità di creare un attributo di validator personalizzato ereditando un nuovo attributo di convalida dell'attributo di convalida di base.


La classe di prodotto in **listato 1** viene illustrato come utilizzare questi attributi di convalida. Le proprietà di nome, descrizione e UnitPrice sono contrassegnate come richiesto. La proprietà Name deve avere una lunghezza di stringa di caratteri inferiore a 10. Infine, la proprietà UnitPrice deve corrispondere a un criterio di espressione regolare che rappresenta un importo in valuta.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**Elenco 1**: Models\Product.cs

La classe di prodotto viene illustrato come utilizzare un attributo aggiuntivo: l'attributo DisplayName. L'attributo DisplayName consente di modificare il nome della proprietà quando la proprietà viene visualizzata in un messaggio di errore. Invece di visualizzare il messaggio di errore "è obbligatorio il campo UnitPrice" è possibile visualizzare il messaggio di errore "il campo Prezzo è obbligatorio".

> [!NOTE] 
> 
> Se si desidera personalizzare completamente il messaggio di errore visualizzato da un validator è possibile assegnare un messaggio di errore personalizzata per la proprietà del validator messaggio di errore simile al seguente:`<Required(ErrorMessage:="This field needs a value!")>`


È possibile utilizzare la classe di prodotto in **listato 1** con l'azione del controller nel metodo di creazione **listato 2**. Questa azione del controller ricompilata la visualizzazione Create quando lo stato del modello contiene errori.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**Il listato 2**: Controllers\ProductController.vb

Infine, è possibile creare la visualizzazione in **listato 3** facendo clic l'azione del metodo di creazione e selezionando l'opzione di menu **Aggiungi visualizzazione**. Creare una visualizzazione fortemente tipizzata con la classe di prodotto come classe di modello. Selezionare **crea** dall'elenco a discesa del contenuto di visualizzazione (vedere **figura 2**).

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**Figura 2**: aggiunta della visualizzazione di creazione

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**Elenco di 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Rimuovere il campo Id dal modulo di creazione generato dal **Aggiungi visualizzazione** opzione di menu. Poiché il campo Id corrisponde a una colonna Identity, non si desidera consentire agli utenti di immettere un valore per questo campo.


Se si invia il form per la creazione di un prodotto e non si immette i valori per i campi obbligatori, quindi i messaggi di errore di convalida nella **figura 3** vengono visualizzati.

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**Figura 3**: i campi obbligatori mancanti

Se si immette un importo di valuta non valido, quindi il messaggio di errore in **figura 4** viene visualizzato.

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**Figura 4**: importo di valuta non valido

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Utilizzo di validator annotazione dei dati con Entity Framework

Se si utilizza Microsoft Entity Framework per generare classi del modello di dati è possibile applicare gli attributi di convalida direttamente alle classi. Poiché la finestra di progettazione di Entity Framework genera classi del modello, le modifiche apportate alle classi modello verranno sovrascritto alla successiva che si apportano modifiche nella finestra di progettazione.

Se si desidera utilizzare i validator con le classi generate da Entity Framework è necessario creare classi di dati di metadati. Applicare i validator per la classe di metadati dati invece di applicare i validator alla classe effettiva.

Ad esempio, si supponga che sia stato creato una classe di film tramite Entity Framework (vedere **figura 5**). Si supponga inoltre che si desidera rendere il titolo del film e Director proprietà obbligatorio. In tal caso, è possibile creare la classe parziale e dati in meta **listato 4**.

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**Figura 5**: classe film generato da Entity Framework

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**Listato 4**: Models\Movie.cs

Il file in **listato 4** contiene due classi denominate film e MovieMetaData. La classe film è una classe parziale. Corrisponde alla classe parziale generata da Entity Framework che è contenuto nel file DataModel.Designer.vb.

.NET framework attualmente non supporta proprietà parziale. Pertanto, non è possibile applicare gli attributi di convalida per le proprietà della classe film definita nel file DataModel.Designer.vb applicando gli attributi di convalida per le proprietà della classe film definita nel file **listato 4**.

Si noti che la classe parziale film è decorata con un attributo MetadataType che fa riferimento la classe MovieMetaData. La classe MovieMetaData contiene le proprietà del proxy per le proprietà della classe film.

Gli attributi di convalida vengono applicati alle proprietà della classe MovieMetaData. La proprietà Title, Director e DateReleased sono tutti contrassegnate come proprietà obbligatorie. La proprietà di directory deve essere assegnata una stringa contenente meno di 5 caratteri. Infine, in cui viene applicato l'attributo DisplayName per la proprietà DateReleased per visualizzare un messaggio di errore "il campo rilasciato data è obbligatorio." anziché l'errore "il campo DateReleased è obbligatorio."

> [!NOTE] 
> 
> Si noti che le proprietà del proxy nella classe MovieMetaData non necessaria rappresentare gli stessi tipi di proprietà corrispondenti nella classe film. Ad esempio, la proprietà Director è una proprietà stringa nella classe film e proprietà di un oggetto nella classe MovieMetaData.


La pagina in **figura 6** illustra i messaggi di errore restituiti quando si immettono valori non validi per le proprietà di film.

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**Figura 6**: utilizzo di validator con Entity Framework ([fare clic per visualizzare l'immagine ingrandita](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come sfruttare il raccoglitore di modelli dati annotazione per la convalida all'interno di un'applicazione MVC ASP.NET. È stato descritto come utilizzare i diversi tipi di attributi di convalida, ad esempio la e StringLength. È stato inoltre descritto come utilizzare questi attributi quando si lavora con Microsoft Entity Framework.

>[!div class="step-by-step"]
[Precedente](validating-with-a-service-layer-cs.md)
[Successivo](creating-model-classes-with-the-entity-framework-vb.md)
