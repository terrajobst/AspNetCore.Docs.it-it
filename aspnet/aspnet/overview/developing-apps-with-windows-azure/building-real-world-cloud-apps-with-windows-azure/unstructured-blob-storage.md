---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Archiviazione di Blob non strutturati (creazione di applicazioni con Azure Cloud del mondo reale) | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 6cb77e8ef301c2eeef7df3e391e14f4e2c0364e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Archiviazione di Blob non strutturati (creazione di applicazioni Cloud reale in Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


Nel capitolo precedente abbiamo esaminato gli schemi di partizionamento e illustrato come l'app Correggi le immagini memorizzate nel servizio Blob di archiviazione di Azure e altri dati contenuti nel Database SQL di Azure. In questo capitolo è approfondita nel servizio Blob e Mostra come viene implementato in Correggi il codice del progetto.

## <a name="what-is-blob-storage"></a>Che cos'è l'archiviazione Blob?

Il servizio Blob di archiviazione di Azure fornisce un modo per archiviare i file nel cloud. Il servizio Blob presenta numerosi vantaggi rispetto all'archiviazione di file in un file system di rete locale:

- È altamente scalabile. Un singolo account di archiviazione è possibile archiviare [centinaia di terabyte](https://msdn.microsoft.com/en-us/library/windowsazure/dn249410.aspx), ed è possibile disporre di più account di archiviazione. Alcuni dei principali clienti Azure archiviare centinaia di petabyte. Microsoft SkyDrive utilizza l'archiviazione di blob.
- È durevole. Ogni file che archiviare nel servizio Blob viene automaticamente eseguito il backup.
- Fornisce la disponibilità elevata. Il [contratto di servizio per l'archiviazione](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promesse 99,9% o 99,99% tempo di attività, a seconda di quale opzione di ridondanza geografica scelta.
- È una funzionalità di piattaforma come servizio (PaaS) di Azure, pertanto è sufficiente archiviare e recuperare i file, pagare solo la quantità effettiva di archiviazione da utilizzare, e Azure occupa automaticamente di impostare e gestire tutte le macchine virtuali e l'unità disco necessarie per il servizio.
- È possibile accedere nel servizio Blob utilizzando un'API REST o tramite un linguaggio di programmazione API. SDK sono disponibili per .NET, Java, Ruby e ad altri utenti.
- Quando si archivia un file nel servizio Blob, è possibile apportare facilmente disponibile pubblicamente su Internet.
- È possibile proteggere i file nel Blob di accedere al servizio in modo da poter solo dagli utenti autorizzati, oppure è possibile fornire i token di accesso temporaneo che li rende disponibili per un utente solo per un periodo di tempo limitato.

Ogni volta che si sta creando un'app di Azure e si desidera memorizzare grandi quantità di dati in un ambiente locale potrebbero andare nel file, ad esempio immagini, video, PDF, fogli di calcolo e così via, prendere in considerazione il servizio Blob.

## <a name="creating-a-storage-account"></a>Creazione di un account di archiviazione

Per iniziare con il servizio Blob creare un account di archiviazione in Azure. Nel portale, fare clic su **New** -- **Data Services** -- **archiviazione** -- **creazione rapida**, e quindi immettere un URL e una posizione del data center. Posizione del data center deve essere lo stesso come app web.

![Creare un account Archiv.](unstructured-blob-storage/_static/image1.png)

Selezionare l'area principale in cui si desidera archiviare il contenuto e si sceglie il [-replica geografica](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) opzione, Azure crea repliche di tutti i dati in un data center diverso in un'altra area del paese. Ad esempio, se si sceglie data center degli Stati Uniti occidentali, quando si archivia un file che passano nel data center degli Stati Uniti occidentali, ma in background Azure anche verrà copiato in una delle altre aree di dati di Stati Uniti. In caso di emergenza in un'area del paese, i dati sono ancora sicuri.

Non replica i dati attraverso i limiti geopolitici, Azure: se la posizione primaria è negli Stati Uniti, i file vengono replicati solo in un'altra area all'interno degli Stati Uniti; Se la posizione primaria è Australia, i file vengono replicati solo a un altro data center, in Australia.

Naturalmente, è anche possibile creare un account di archiviazione eseguendo i comandi da uno script, come abbiamo visto prima. Di seguito è riportato un comando di Windows PowerShell per creare un account di archiviazione:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Dopo aver creato un account di archiviazione, è possibile avviare immediatamente l'archiviazione dei file nel servizio Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Utilizzo dell'archiviazione Blob nell'app Correggi

L'app Correggi consente di caricare le foto.

![Creare un'attività di correzione](unstructured-blob-storage/_static/image2.png)

Quando fa clic su **creare il FixIt**, l'applicazione carica il file di immagine specificato e lo archivia nel servizio Blob.

### <a name="set-up-the-blob-container"></a>Configurare il contenitore Blob

Per poter archiviare un file nel servizio Blob di cui è necessario un *contenitore* in cui salvarlo. Un contenitore del servizio Blob corrisponde a una cartella del file system. Gli script di creazione ambiente che è stata esaminata nel [automatizzare tutti gli elementi capitolo](automate-everything.md) creare l'account di archiviazione, ma non creano un contenitore. Pertanto lo scopo del `CreateAndConfigure` metodo la `PhotoService` classe consiste nel creare un contenitore, se non esiste già. Questo metodo viene chiamato dal `Application_Start` metodo *Global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

La chiave di accesso e sul nome di account archiviazione vengono archiviati nel `appSettings` insieme del *Web. config* file e scrivere il codice nel `StorageUtils.StorageAccount` metodo utilizza tali valori per compilare una stringa di connessione e stabilire una connessione:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Il `CreateAndConfigureAsync` crea quindi un oggetto che rappresenta il servizio Blob e un oggetto che rappresenta un contenitore (cartella) denominato "immagini" nel servizio Blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Se un contenitore denominato "immagini" non esiste ancora, che sarà true, la prima volta che si esegue l'app in un nuovo account di archiviazione, il codice crea il contenitore e imposta le autorizzazioni per renderlo pubblico. (Per impostazione predefinita, nuovi contenitori di blob sono privati e sono accessibili solo agli utenti che dispongono dell'autorizzazione per accedere all'account di archiviazione.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Archiviare la foto caricata nell'archiviazione Blob

Per caricare e salvare il file di immagine, l'applicazione utilizzi un `IPhotoService` interfaccia e un'implementazione dell'interfaccia di `PhotoService` classe. Il *PhotoService.cs* file contiene tutto il codice nel Correggi app che comunica con il servizio Blob.

Metodo del controller MVC seguente viene chiamato quando l'utente fa clic **creare il FixIt**. In questo codice, `photoService` fa riferimento a un'istanza di `PhotoService` (classe), e `fixittask` fa riferimento a un'istanza del `FixItTask` classe di entità che archivia i dati di una nuova attività.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

Il `UploadPhotoAsync` metodo la `PhotoService` classe archivia il file caricato nel servizio Blob e restituisce un URL che punta al nuovo blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Ad esempio il `CreateAndConfigure` (metodo), il codice si connette all'account di archiviazione e crea un oggetto che rappresenta il contenitore di blob "immagini", ad eccezione del fatto di seguito si presuppone che il contenitore esiste già.

Crea quindi un identificatore univoco per l'immagine da caricare, tramite la concatenazione di un nuovo valore GUID con l'estensione di file:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Il codice quindi utilizza l'oggetto contenitore di blob e il nuovo identificatore univoco per creare un oggetto blob, imposta un attributo sull'oggetto che indica il tipo di file è e quindi utilizza l'oggetto blob per archiviare il file nell'archiviazione blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Infine, viene ottenuto un URL che fa riferimento il blob. Questo URL verrà archiviato nel database e sono utilizzabili in Correggi le pagine web per visualizzare l'immagine caricata.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Questo URL viene archiviato nel database come una delle colonne della tabella FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Con solo l'URL nel database e le immagini nell'archiviazione Blob, app Correggi mantiene il database di piccole, scalabile ed economica, mentre vengono archiviate le immagini in cui l'archiviazione è economica e in grado di gestire terabyte o petabyte. Un account di archiviazione può archiviare centinaia di terabyte di foto di correzione, e si paga solo per ciò che si utilizza. Pertanto è possibile iniziare a piccole centesimi 9 pagamenti per il primo gigabyte e aggiungere altre immagini per centesimi per ogni GB aggiuntivo.

### <a name="display-the-uploaded-file"></a>Visualizzare il file caricato

L'applicazione Correggi Visualizza il file di immagine caricata quando vengono visualizzati i dettagli per un'attività.

![Risolvere i dettagli delle attività con foto](unstructured-blob-storage/_static/image3.png)

Per visualizzare l'immagine, è la visualizzazione MVC dovrà includere il `PhotoUrl` valore nel codice HTML inviato al browser. Il server web e il database non utilizza i cicli per visualizzare l'immagine, servono solo pochi byte per l'URL dell'immagine. Nel seguente codice Razor, `Model` fa riferimento a un'istanza di `FixItTask` classe di entità.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Se si esamina il codice HTML della pagina che consente di visualizzare, viene visualizzato l'URL che punta direttamente all'immagine nell'archiviazione blob, un codice simile al seguente:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Riepilogo

Si è visto come app Correggi le immagini memorizzate nel servizio Blob e solo gli URL di immagine nel database SQL. Uso del servizio Blob mantiene il database SQL molto più piccola, in caso contrario sarebbe, rende possibile la scala fino a un numero illimitato di attività e può essere eseguita senza scrivere una quantità di codice.

È possibile avere centinaia di terabyte in un account di archiviazione e il costo di archiviazione è molto meno dispendioso di archiviazione del Database SQL, a partire da circa 3 centesimi per gigabyte al mese corrente più un addebito di transazioni di dimensioni. E tenere presente che si sta non a pagamento per la capacità massima, ma solo per la quantità che effettivamente archiviati in modo che l'app è pronto essere scalato ma non si effettuano pagamenti ad tutte le capacità.

Nel [capitolo successivo](design-to-survive-failures.md) parleremo l'importanza di effettuare un'app cloud in grado di gestire normalmente gli errori.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere le risorse seguenti:

- [Introduzione all'archiviazione BLOB di Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Post di blog di legno Mike.
- [Come utilizzare il servizio di archiviazione Blob di Azure in .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentazione ufficiale sul sito MicrosoftAzure.com. Una breve introduzione all'archiviazione seguito da esempi di codice che illustra come connettersi all'archiviazione blob, blob creare contenitori, caricare e scaricare i BLOB e così via.
- [Operatore alternativo: Compilazione di servizi Cloud resilienti e scalabili](https://channel9.msdn.com/Series/FailSafe). Serie video in nove parti Ulrich Homann, Marc Mercuri e Mark Simms. Presenta i concetti generali e i principi architetturali in modo molto accessibile e interessano con storie ricavate dall'esperienza di Microsoft Team (CAT, Customer Advisory) con i clienti effettivi. Per una descrizione del servizio di archiviazione di Azure e BLOB, vedere episodio 5 partendo da 35:13.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Vedere Valet chiave modello.

>[!div class="step-by-step"]
[Precedente](data-partitioning-strategies.md)
[Successivo](design-to-survive-failures.md)
