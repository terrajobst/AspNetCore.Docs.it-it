---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Pubblicare MVC Database primo sito in Azure | Microsoft Docs
author: tfitzmac
description: Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 5189d8ee92c6abac31d80ca4efdb06500e72126a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387270"
---
<a name="publish-mvc-database-first-site-to-azure"></a>Pubblicare MVC Database primo sito in Azure
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database. Il codice generato corrispondente alle colonne nella tabella di database.
> 
> Questa parte della serie è incentrato sulla pubblicazione dell'app web e database in Azure. È possibile leggere questo argomento per altre informazioni sulla pubblicazione di un'app web e database, ma in realtà eseguire i passaggi che deve iniziare all'inizio dell'esercitazione. Visualizzare [introduttiva](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Distribuire l'app web in Azure

È necessario un account di Azure per completare questa esercitazione:

- È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -ricevono crediti è possibile usare per provare i servizi di Azure a pagamento e anche dopo che sono abituati fino è possibile mantenere l'account e usare i servizi Azure gratuiti.
- È possibile [attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -la sottoscrizione MSDN si accumulano crediti ogni mese in cui è possibile usare per i servizi di Azure a pagamento.

Per pubblicare l'app web, fare clic sul progetto e selezionare **pubblica**.

![Pubblicazione del sito](publish-to-azure/_static/image1.png)

Selezionare i siti Web di Microsoft Azure.

![Selezionare Azure](publish-to-azure/_static/image2.png)

Se non è stato eseguito l'accesso ad Azure, fornire le credenziali dell'account Azure. Quindi, scegliere Nuovo per creare una nuova app web.

![nuovo sito](publish-to-azure/_static/image3.png)

Creare un nome univoco per l'app web. Sarà possibile sapere che il nome è univoco se viene visualizzato un segno di spunta verde a destra del campo name. Selezionare un'area per l'app web. Selezionare **Crea nuovo server** per il database e fornire un nome utente e una password per questo nuovo server di database. Al termine, fare clic su **Create**.

![Crea sito](publish-to-azure/_static/image4.png)

I valori di connessione a questo punto tutto pronto. È possibile lasciare questi valori non modificati.

![connection](publish-to-azure/_static/image5.png)

Scegliere **Avanti**.

Per le impostazioni, si noti che due connessioni di database vengono specificati - ApplicationDBContext e ContosoUniversityDataEntities. ApplicationDBContext è la connessione per tabelle dell'account utente. Questi valori vengono visualizzate solo le stringhe di connessione per i database. Questo non significa che questi database verranno pubblicati quando si pubblica il sito. Dopo aver completato l'app web di pubblicazione verrà pubblicato il progetto di database.

![](publish-to-azure/_static/image6.png)

I puntini di sospensione (...) accanto alla connessione di database per visualizzare i dettagli della stringa di connessione. Fare clic sui puntini di sospensione accanto a ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Prendere nota del nome del server di database e del database. Il nome del server viene generato casualmente. Il nome del database è semplice il nome del sito con  **\_db** aggiunto. È necessario che entrambi i nomi in un secondo momento quando si pubblica il tuo database.

Fare clic su **OK** per chiudere la finestra di stringa di connessione di database.

Nella finestra pubblica sul Web, fare clic su **successivo** per visualizzare l'anteprima.

![](publish-to-azure/_static/image8.png)

È possibile fare clic su Avvia anteprima per visualizzare un elenco dei file da pubblicare. Poiché questa è la prima volta che si pubblica questo sito, l'elenco è tutti i file rilevanti nel progetto.

Fare clic su **Pubblica**.

Il riquadro di Output verrà visualizzato il risultato della pubblicazione.

![](publish-to-azure/_static/image9.png)

Dopo la pubblicazione, il sito è immedialely avviata in un web browser. Il sito è stato distribuito ed è possibile registrare un nuovo utente del sito. Tuttavia, le tabelle nel progetto ContosoUniversityData non sono ancora state pubblicate. Se si fa clic nell'elenco di studenti collegamento si riceverà un errore.

## <a name="publish-database-to-sql-azure"></a>Pubblicazione di SQL Azure database

Prima di pubblicare il database, è necessario assicurarsi che nel computer locale può connettersi al server di database. Il firewall per il server di database consentono di limitare i computer possono connettersi al database. È necessario aggiungere l'indirizzo IP del computer in uso per gli indirizzi IP consentiti per il firewall.

Accedere al proprio account Azure tramite il portale di Azure.

Selezionare il nuovo database e selezionare **Gestisci**.

![gestire](publish-to-azure/_static/image10.png)

È necessario configurare il server di database per consentire le connessioni dal computer. Quando si seleziona gestione, viene chiesto di aggiungere l'indirizzo IP corrente, come se il server di database. Selezionare Sì.

![Aggiungi indirizzo ip](publish-to-azure/_static/image11.png)

È probabile che l'indirizzo IP che aggiunto nel passaggio precedente non è l'unico indirizzo IP che è necessario configurare per le connessioni. È possibile tentare di accedere al database per verificare se le connessioni correttamente configurate. Specificare l'utente e la password creata in precedenza.

![Accesso](publish-to-azure/_static/image12.png)

Se si riceve un messaggio di errore, è necessario aggiungere un altro indirizzo IP. Fare clic sul messaggio di errore per visualizzare ulteriori dettagli sull'errore. I dettagli si verrà visualizzato l'indirizzo IP che è necessario aggiungere. Annotare questo indirizzo IP.

![non è consentito](publish-to-azure/_static/image13.png)

Chiudere questa finestra di dialogo di accesso e tornare al portale di Azure.

Passare al Dashboard per il database. Fare clic su **Gestisci indirizzi IP consentiti**.

![gestire gli indirizzi ip](publish-to-azure/_static/image14.png)

È ora necessario aggiungere l'indirizzo IP dal messaggio di errore. Modificare l'intervallo di indirizzi IP consentiti per includere quello dal messaggio di errore o aggiungere tale indirizzo IP come una voce separata.

![Aggiungere nuovo indirizzo](publish-to-azure/_static/image15.png)

Salvare le modifiche agli indirizzi IP.

Fare clic su Gestisci e provare ad accedere nuovamente al database. Potrebbe essere necessario attendere alcuni minuti prima che gli indirizzi IP consentiti siano configurati correttamente per il firewall. Quando è possibile accedere nel database, avere completato l'impostazione di connessione al database.

È possibile lasciare aperta questa finestra di gestione perché si verificherà il risultato della distribuzione di database a breve.

Tornare al progetto di database. Fare clic sul progetto e selezionare **pubblica**.

![database di pubblicazione](publish-to-azure/_static/image16.png)

Nella finestra di Database di pubblicazione, selezionare **modifica**.

![modifica](publish-to-azure/_static/image17.png)

Specificare il nome del server di database e le credenziali di autenticazione per il server. Dopo aver fornito le credenziali, selezionare il database creato dall'elenco dei database disponibili. Per impostazione predefinita, Visual Studio imposta il nome del campo di database per il nome del progetto che potrebbe non essere quello utilizzato per il database creato.

![](publish-to-azure/_static/image18.png)

Fare clic su OK.

Probabilmente si desidererà salvare questo profilo, quindi è possibile pubblicare gli aggiornamenti in futuro senza dover immettere nuovamente tutte le informazioni di connessione. Selezionare **Crea profilo**.

![Salva profilo](publish-to-azure/_static/image19.png)

Creerà un file nel progetto denominato **ContosoUniversityData.publish.xml**. La volta successiva che si desidera pubblicare il database in Azure, caricare semplicemente quel profilo.

A questo punto, fare clic su **pubblica** per creare il database in Azure.

![publish](publish-to-azure/_static/image20.png)

Dopo l'esecuzione per un periodo di tempo, la pubblicazione dei risultati vengono visualizzati.

![results](publish-to-azure/_static/image21.png)

A questo punto, è possibile tornare al portale di gestione per il database. Aggiornare la visualizzazione di progettazione e osservare che le tabelle con dati pre-popolata 3 sono state distribuite.

![nuove tabelle](publish-to-azure/_static/image22.png)

A questo punto si è pronti per testare l'app web che viene distribuito in Azure. Passare all'app web in Azure (ad esempio http://contosositeexample.azurewebsites.net/). Fare clic sul collegamento per l'elenco degli studenti e verrà visualizzata la vista di indice per gli studenti.

![visualizzazione](publish-to-azure/_static/image23.png)

In alcuni casi, il database e della connessione è necessario un po' di tempo per essere configurato correttamente. Se si riceve un errore, attendere qualche minuto e ripetere l'operazione.

## <a name="conclusion"></a>Conclusione

Questa serie è fornito un semplice esempio di come generare codice da un database esistente che consente agli utenti di modificare, aggiornare, creare ed eliminare dati. ASP.NET MVC 5, Entity Framework e lo Scaffolding di ASP.NET utilizzato per creare il progetto.

Per un esempio introduttivo di sviluppo con Code First, vedere [Introduzione a ASP.NET MVC 5](../introduction/getting-started.md).

Per un esempio più avanzato, vedere [creazione di un modello di dati di Entity Framework per un'App ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Si noti che l'API DbContext che usano per lavorare con i dati in Database First è quello utilizzato per l'API usata per l'utilizzo di dati di Code First. Anche se si prevede di usare Database First, è possibile imparare a gestire scenari più complessi, ad esempio la lettura e aggiornamento di dati correlati, la gestione dei conflitti di concorrenza, e così via da un'esercitazione Code First. L'unica differenza è in modalità di creazione di database, classe del contesto e le classi di entità.

> [!div class="step-by-step"]
> [Precedente](enhancing-data-validation.md)
