---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Sito MVC Database prima di pubblicare in Azure | Documenti Microsoft
author: tfitzmac
description: Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a>Sito MVC Database prima di pubblicare in Azure
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.
> 
> Questa parte della serie è incentrata sulla pubblicazione di app web e il database in Azure. È possibile leggere questo argomento per ulteriori informazioni sulla pubblicazione di un'applicazione web e il database, ma in realtà eseguire i passaggi che deve iniziare all'inizio dell'esercitazione. Vedere [Introduzione](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Distribuire l'app web in Azure

È necessario un account Azure per completare questa esercitazione:

- È possibile [aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.
- È possibile [attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.

Per pubblicare l'app web, fare clic sul progetto e selezionare **pubblica**.

![pubblicazione del sito](publish-to-azure/_static/image1.png)

Selezionare i siti Web di Microsoft Azure.

![Selezionare Azure](publish-to-azure/_static/image2.png)

Se non è connesso a Azure, fornire le credenziali dell'account Azure. Quindi, scegliere Nuovo per creare una nuova app web.

![nuovo sito](publish-to-azure/_static/image3.png)

Creare un nome univoco per l'app web. Si conosceranno che il nome è univoco se viene visualizzato un segno di spunta verde a destra del campo name. Selezionare un'area per l'app web. Selezionare **Crea nuovo server** per il database e fornire un nome utente e una password per questo nuovo server di database. Al termine, fare clic su **crea**.

![Crea sito](publish-to-azure/_static/image4.png)

I valori di connessione sono ora impostati. È possibile lasciare invariati i questi valori.

![connection](publish-to-azure/_static/image5.png)

Scegliere **Avanti**.

Per le impostazioni, si noti che due connessioni di database vengono specificati - ApplicationDBContext e ContosoUniversityDataEntities. ApplicationDBContext è la connessione per le tabelle di account utente. Questi valori vengono visualizzate solo le stringhe di connessione per i database. Ciò significa che questi database verranno pubblicati quando si pubblica il sito. Dopo aver completato la pubblicazione dell'app web verrà pubblicato il progetto di database.

![](publish-to-azure/_static/image6.png)

I puntini di sospensione (...) accanto alla connessione al database Mostra i dettagli della stringa di connessione. Fare clic sui puntini di sospensione accanto a ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Annotare il nome del server di database e del database. Il nome del server viene generato casualmente. Il nome del database è semplice il nome del sito con  **\_db** aggiunto. È necessario che entrambi i nomi in un secondo momento durante la pubblicazione del database.

Fare clic su **OK** per chiudere la finestra di stringa di connessione di database.

Nella finestra di pubblica sul Web, fare clic su **Avanti** per visualizzare l'anteprima.

![](publish-to-azure/_static/image8.png)

È possibile scegliere di avviare l'anteprima per visualizzare un elenco di file da pubblicare. Poiché questa è la prima volta che questo sito è pubblicato, l'elenco è ogni file rilevanti nel progetto.

Fare clic su **Pubblica**.

Il riquadro di Output verrà visualizzato il risultato della pubblicazione.

![](publish-to-azure/_static/image9.png)

Dopo la pubblicazione, il sito è immedialely avviata in un web browser. Il sito è stato distribuito ed è possibile registrare un nuovo utente per il sito. Tuttavia, le tabelle nel progetto ContosoUniversityData non sono ancora state pubblicate. Se si fa clic sull'elenco di studenti collegamento si riceverà un errore.

## <a name="publish-database-to-sql-azure"></a>Pubblicare un database a SQL Azure

Prima di pubblicare il database, è necessario assicurarsi che il computer locale può connettersi al server di database. Il firewall per il server di database limita le macchine possono connettersi al database. È necessario aggiungere l'indirizzo IP del computer agli indirizzi IP consentiti per il firewall.

Accedere al proprio account Azure tramite il portale di Azure.

Selezionare il nuovo database e selezionare **Gestisci**.

![Gestire](publish-to-azure/_static/image10.png)

È necessario configurare il server di database per consentire le connessioni dal computer. Quando si seleziona gestione, viene chiesto di aggiungere l'indirizzo IP corrente, come se il server di database. Selezionare Sì.

![aggiungere l'indirizzo ip](publish-to-azure/_static/image11.png)

È probabile che l'indirizzo IP che è stato aggiunto nel passaggio precedente non è il solo indirizzo IP che è necessario configurare per le connessioni. È possibile tentare di eseguire l'accesso al database per vedere se le connessioni sono state configurate correttamente. Specificare la password creata in precedenza e l'utente.

![Accesso](publish-to-azure/_static/image12.png)

Se si riceve un messaggio di errore, è necessario aggiungere un altro indirizzo IP. Fare clic sul messaggio di errore per visualizzare ulteriori dettagli sull'errore. Nei dettagli verrà visualizzato l'indirizzo IP che si desidera aggiungere. Si noti l'indirizzo IP.

![non è consentito](publish-to-azure/_static/image13.png)

Chiudere questa finestra di dialogo di accesso e tornare al portale di Azure.

Passare al Dashboard per il database. Fare clic su **gestione indirizzi IP consentiti**.

![gestire gli indirizzi ip](publish-to-azure/_static/image14.png)

A questo punto, è necessario aggiungere l'indirizzo IP dal messaggio di errore. L'intervallo di indirizzi IP consentiti per includere il messaggio di errore con quello di modificare o aggiungere tale indirizzo IP come una voce separata.

![Aggiungere nuovo indirizzo](publish-to-azure/_static/image15.png)

Salvare le modifiche agli indirizzi IP.

Fare clic su Gestisci e provare a effettuare l'accesso al database. Potrebbe essere necessario attendere alcuni minuti prima che gli indirizzi IP consentiti siano configurati correttamente per il firewall. Quando è possibile registrare correttamente nel database, avere impostato la connessione al database.

È possibile lasciare aperta questa finestra di gestione perché si verificherà il risultato della distribuzione di database a breve.

Tornare al progetto di database. Fare clic sul progetto e selezionare **pubblica**.

![pubblicare database](publish-to-azure/_static/image16.png)

Nella finestra di Database di pubblicazione, selezionare **modifica**.

![modifica](publish-to-azure/_static/image17.png)

Specificare il nome del server di database e le credenziali di autenticazione per il server. Dopo aver fornito le credenziali, selezionare il database creato dall'elenco dei database disponibili. Per impostazione predefinita, Visual Studio imposta il nome del campo di database per il nome del progetto che potrebbe non essere lo stesso nome di database che è stato creato.

![](publish-to-azure/_static/image18.png)

Fare clic su OK.

Probabilmente è possibile salvare questo profilo, quindi è possibile pubblicare gli aggiornamenti in futuro senza dover immettere nuovamente tutte le informazioni di connessione. Selezionare **Crea profilo**.

![Salva profilo](publish-to-azure/_static/image19.png)

Crea un file di progetto denominato **ContosoUniversityData.publish.xml**. Alla successiva che si desidera pubblicare il database in Azure, caricare semplicemente tale profilo.

A questo punto, fare clic su **pubblica** per creare il database in Azure.

![publish](publish-to-azure/_static/image20.png)

Dopo aver eseguito per un periodo di tempo, vengono visualizzati i risultati di pubblicazione.

![results](publish-to-azure/_static/image21.png)

A questo punto, è possibile tornare al portale di gestione per il database. Aggiornare la visualizzazione di progettazione e osservare che le tabelle con dati precompilate 3 sono state distribuite.

![nuove tabelle](publish-to-azure/_static/image22.png)

A questo punto si è pronti per testare l'app web che viene distribuito in Azure. Passare all'app web in Azure (ad esempio http://contosositeexample.azurewebsites.net/). Fare clic sul collegamento per l'elenco di studenti e dovrebbe essere la visualizzazione dell'indice per studenti.

![visualizzazione](publish-to-azure/_static/image23.png)

In alcuni casi, il database e la connessione è necessario un po' di tempo per essere configurato correttamente. Se si riceve un errore, attendere qualche minuto e riprovare.

## <a name="conclusion"></a>Conclusione

Questa serie fornito un semplice esempio di come generare codice da un database esistente che consente agli utenti di modificare, aggiornare, creare ed eliminare dati. Consente di creare il progetto ASP.NET MVC 5, Entity Framework e lo Scaffolding di ASP.NET.

Per un esempio introduttivo dello sviluppo Code First, vedere [Introduzione a ASP.NET MVC 5](../introduction/getting-started.md).

Per un esempio più avanzato, vedere [creazione di un modello di dati di Entity Framework per applicazioni ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Si noti che l'API DbContext utilizzabili per lavorare con i dati nel primo Database è identico API utilizzata per l'utilizzo di dati di Code First. Anche se si prevede di utilizzare Database First, è possibile imparare a gestire scenari più complessi, ad esempio la lettura e aggiornamento dei dati correlati, gestione dei conflitti di concorrenza, e così via da un'esercitazione Code First. L'unica differenza è in modalità di creazione di database, classe del contesto e le classi di entità.

> [!div class="step-by-step"]
> [Precedente](enhancing-data-validation.md)
