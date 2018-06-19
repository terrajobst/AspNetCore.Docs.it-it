---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Estrazione e pagamento PayPal | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 0dba613594686a28b82bc6d7701cda6e24b82e2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891893"
---
<a name="checkout-and-payment-with-paypal"></a>Estrazione e pagamento PayPal
====================
by [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


In questa esercitazione viene descritto come modificare l'applicazione di esempio Wingtip Toys per includere l'autorizzazione dell'utente, registrazione e pagamento tramite PayPal. Solo gli utenti connessi avrà l'autorizzazione per l'acquisto di prodotti. Funzionalità di registrazione utente predefiniti del modello di progetto Web Form ASP.NET 4.5 include già la maggior parte degli elementi. Aggiungere funzionalità di estrazione Express PayPal. In questa esercitazione si utilizzando lo sviluppatore di PayPal ambiente di test in modo non fondi effettivi verranno trasferiti. Al termine dell'esercitazione, si testerà l'applicazione, selezionare i prodotti per aggiungere il carrello degli acquisti, fare clic sul pulsante di estrazione e il trasferimento dei dati per il sito web test PayPal. Nel sito web test PayPal, verrà verificare le informazioni di pagamento e di spedizione e quindi tornare all'applicazione di esempio Wingtip Toys locale per confermare e completare l'acquisto.

Esistono diversi processori esperti pagamento di terze parti che specializzano shopping online che scalabilità indirizzo e la sicurezza. Gli sviluppatori ASP.NET è necessario considerare i vantaggi dell'utilizzo di una soluzione di pagamento di terze parti prima di implementare un carrello e l'acquisto di soluzione.

> [!NOTE] 
> 
> L'applicazione di esempio Wingtip Toys è stato progettato per mostrato concetti specifici di ASP.NET e le funzionalità disponibili per gli sviluppatori web ASP.NET. Questa applicazione di esempio non è stata ottimizzata per tutti i possibili rischi per quanto riguarda la scalabilità e sicurezza.


## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come limitare l'accesso alle pagine specifiche in una cartella.
- Come creare un carrello acquisti noto da un carrello acquisti anonimo.
- Come abilitare SSL per il progetto.
- Come aggiungere un provider OAuth al progetto.
- Modalità di utilizzo di PayPal per l'acquisto di prodotti che utilizzano l'ambiente di testing di PayPal.
- Come visualizzare i dettagli da PayPal, in un **DetailsView** controllo.
- Come aggiornare il database dell'applicazione Wingtip Toys con dettagli ottenuti da PayPal.

## <a name="adding-order-tracking"></a>Aggiunta di rilevamento di ordine

In questa esercitazione si creeranno due nuove classi per tenere traccia dei dati dall'ordine di che un utente ha creato. Le classi tiene traccia di dati relativi alle informazioni di spedizione totale di acquisto e conferma di pagamento.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Aggiungere l'ordine e le classi del modello OrderDetail

Più indietro in questa serie di esercitazioni, è stato definito lo schema per le categorie, prodotti, e gli elementi di carrello creando il `Category`, `Product`, e `CartItem` classi nel *modelli* cartella. Verranno ora aggiunte due nuove classi per definire lo schema per l'ordine di prodotto e i dettagli dell'ordine.

1. Nel **modelli** cartella, aggiungere una nuova classe denominata *cs*.   
   Il nuovo file di classe viene visualizzato nell'editor.
2. Sostituire il codice predefinito con quanto segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Aggiungere un *OrderDetail.cs* classe per il *modelli* cartella.
4. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Il `Order` e `OrderDetail` le classi contengono lo schema per definire le informazioni dell'ordine usate per l'acquisto e di spedizione.

Inoltre, è necessario aggiornare la classe di contesto di database che gestisce le classi di entità e che fornisce accesso ai dati nel database. A tale scopo, si aggiungerà l'ordine appena creato e `OrderDetail` classi di modello `ProductContext` classe.

1. In **Esplora**, trovare e aprire il *ProductContext.cs* file.
2. Aggiungere il codice evidenziato per il *ProductContext.cs* file come illustrato di seguito:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Come accennato in precedenza in questa serie di esercitazioni, il codice nel *ProductContext.cs* file aggiunge il `System.Data.Entity` dello spazio dei nomi in modo da poter accedere a tutte le funzionalità principali di Entity Framework. Questa funzionalità include la possibilità di eseguire una query, inserimento, aggiornamento ed eliminare i dati quando si lavora con oggetti fortemente tipizzati. Il codice precedente il `ProductContext` classe aggiunge accesso Entity Framework appena aggiunto `Order` e `OrderDetail` classi.

## <a name="adding-checkout-access"></a>Aggiunta dell'accesso di estrazione

L'applicazione di esempio Wingtip Toys consente agli utenti anonimi di rivedere e aggiungere un carrello acquisti di prodotti. Tuttavia, quando gli utenti anonimi decide di acquistare i prodotti che sono aggiunti al carrello acquisti, è necessario accedere al sito. Dopo che hanno effettuato l'accesso, è possibile accedere pagine dell'applicazione Web con restrizioni che gestiscono l'estrazione e processo di acquisto. Contenevano in queste pagine con restrizioni di *estrazione* cartella dell'applicazione.

### <a name="add-a-checkout-folder-and-pages"></a>Aggiungere una cartella di estrazione e le pagine

A questo punto si creerà il *estrazione* cartella e le pagine in cui il cliente verrà visualizzato durante il processo di estrazione. Più avanti in questa esercitazione, è possibile aggiornare queste pagine.

1. Fare doppio clic sul nome del progetto (**Wingtip Toys**) in **Esplora** e selezionare **aggiungere una nuova cartella**. 

    ![Estrazione e pagamento PayPal - nuova cartella](checkout-and-payment-with-paypal/_static/image1.png)
2. Denominare la nuova cartella *estrazione*.
3. Fare doppio clic su di *estrazione* cartella e quindi selezionare **Aggiungi**-&gt;**nuovo elemento**. 

    ![Estrazione e pagamento PayPal - nuovo elemento](checkout-and-payment-with-paypal/_static/image2.png)
4. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
5. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Quindi, nel riquadro centrale, selezionare **Web Form con pagina Master**e denominarlo *CheckoutStart.aspx*. 

    ![Estrazione e pagamento PayPal - Aggiungi finestra di dialogo Nuovo elemento](checkout-and-payment-with-paypal/_static/image3.png)
6. Come in precedenza, selezionare il *Site. master* file della pagina master.
7. Aggiungere le seguenti pagine aggiuntive per il *estrazione* cartella utilizzando la stessa procedura precedente:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Aggiungere un File Web. config

Aggiungendo un nuovo *Web. config* file per il *estrazione* cartella, sarà possibile limitare l'accesso a tutte le pagine contenute nella cartella.

1. Fare doppio clic su di *estrazione* cartella e selezionare **Aggiungi**  - &gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Quindi, nel riquadro centrale, selezionare **File di configurazione Web**, accettare il nome predefinito di *Web. config*, quindi selezionare **Aggiungi**.
3. Sostituire il contenuto nel documento XML esistente di *Web. config* file con le operazioni seguenti:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Salvare il *Web. config* file.

Il *Web. config* file specifica che tutti gli utenti sconosciuti dell'applicazione Web devono essere negati l'accesso alle pagine di contenuto nel *estrazione* cartella. Tuttavia, se l'utente ha registrato un account e ha effettuato l'accesso, saranno un utente noto e sarà possibile accedere alle pagine di *estrazione* cartella.

È importante notare che la configurazione di ASP.NET segue una gerarchia, in cui ogni *Web. config* file Applica impostazioni di configurazione per la cartella che si trova in e per tutte le directory figlio sottostanti.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Attivare SSL per il progetto

 Livello SSL (Secure Sockets) è un protocollo definito per consentire al server Web e Web ai client di comunicare in modo più sicuro tramite l'utilizzo di crittografia. Quando non viene utilizzato SSL, i dati inviati tra client e server sono aperti a chiunque abbia accesso fisico alla rete di sniffing dei pacchetti. Inoltre, diversi schemi di autenticazione comuni non sono protetti tramite HTTP semplice. In particolare, l'autenticazione di base e autenticazione basata su form invia le credenziali non crittografate. Per proteggere questi schemi di autenticazione devono usare SSL. 

1. In **Esplora**, fare clic su di **WingtipToys** del progetto, quindi premere **F4** per visualizzare il **proprietà** finestra.
2. Modifica **SSL abilitato** a `true`.
3. Copia il **URL SSL** quindi è possibile usare in un secondo momento.   
 L'URL SSL sarà `https://localhost:44300/` a meno che non è già stato creato siti Web SSL (come illustrato di seguito).   
    ![Proprietà di progetti](checkout-and-payment-with-paypal/_static/image4.png)
4. In **Esplora soluzioni**, fare clic destro la **WingtipToys** sul progetto e scegliere **proprietà**.
5. Nella scheda di sinistra, fare clic su **Web**.
6. Modifica il **Url progetto** per utilizzare il **URL SSL** salvato in precedenza.   
    ![Proprietà del progetto Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Salvare la pagina premendo **CTRL + S**.
8. Premere **CTRL+F5** per eseguire l'applicazione. Visual Studio verrà visualizzata un'opzione che consente di evitare gli avvisi di SSL.
9. Fare clic su **Sì** per considerare attendibile il certificato SSL di IIS Express e continuare.   
    ![Dettagli del certificato SSL di IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Viene visualizzato un avviso di sicurezza.
10. Fare clic su **Sì** per installare il certificato per l'host locale.   
    ![Finestra di dialogo Avviso di sicurezza](checkout-and-payment-with-paypal/_static/image7.png)  
 Verrà visualizzata la finestra del browser.

È ora possibile testare facilmente l'applicazione Web localmente tramite SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Aggiungere un Provider di OAuth 2.0

Web Form ASP.NET fornisce le opzioni avanzate per l'appartenenza e l'autenticazione. Questi miglioramenti includono OAuth. OAuth è un protocollo aperto che consenta di autorizzazione sicura in un metodo semplice e standard da applicazioni web, di dispositivi mobili e desktop. Il modello Web Form ASP.NET utilizza OAuth per esporre Facebook, Twitter, Google e Microsoft come provider di autenticazione. Anche se in questa esercitazione viene utilizzato solo Google come provider di autenticazione, è possibile modificare facilmente il codice per utilizzare uno dei provider. La procedura per implementare altri provider è molto simile a quelli visualizzati in questa esercitazione.

Oltre all'autenticazione, l'esercitazione useranno anche i ruoli per implementare l'autorizzazione. Solo gli utenti di aggiungere il `canEdit` ruolo sarà in grado di modificare i dati (creare, modificare o eliminare contatti).

> [!NOTE] 
> 
> Applicazioni di Windows Live accettano solo un URL in tempo reale per un sito Web di lavoro, non è possibile utilizzare un URL del sito Web locale per il test degli account di accesso.


La procedura seguente consente di aggiungere un provider di autenticazione Google.

1. Aprire il *App\_Start\Startup.Auth.cs* file.
2. Rimuovere i caratteri di commento dal `app.UseGoogleAuthentication()` metodo in modo che il metodo viene visualizzato come segue: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Passare il [Google Developers Console](https://console.developers.google.com/). È inoltre necessario effettuare l'accesso con l'account di posta elettronica per sviluppatori Google (gmail.com). Se non si dispone di un account Google, selezionare il **creare un account** collegamento.   
   Successivamente, verrà visualizzato il **Google Developers Console**.   
    ![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)
4. Fare clic su di **Crea progetto** pulsante e immettere un nome di progetto e l'ID (è possibile utilizzare i valori predefiniti). Quindi, fare clic sul **casella di controllo di contratto** e **crea** pulsante.  

    ![Google - nuovo progetto](checkout-and-payment-with-paypal/_static/image9.png)

   In pochi secondi verrà creato il nuovo progetto e il browser visualizzerà la nuova pagina di progetti.
5. Nella scheda di sinistra, fare clic su **API &amp; auth**, quindi fare clic su **credenziali**.
6. Fare clic su di **Crea nuovo ID Client** in **OAuth**.   
   Il **Create Client ID** verrà visualizzata una finestra di dialogo.   
    ![Google - creare ID Client](checkout-and-payment-with-paypal/_static/image10.png)
7. Nel **Create Client ID** finestra di dialogo, mantenere il valore predefinito **applicazione Web** per il tipo di applicazione.
8. Impostare il **Authorized JavaScript Origins** all'URL SSL è utilizzato in precedenza in questa esercitazione (`https://localhost:44300/` a meno che non è stato creato altri progetti SSL).   
   Questo URL è l'origine per l'applicazione. Per questo esempio, si verrà solo immettere l'URL di test di localhost. Tuttavia, è possibile immettere più URL per l'account per localhost e di produzione.
9. Impostare il **autorizzato l'URI di reindirizzamento** al seguente: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Questo valore è l'URI che OAuth ASP.NET agli utenti di comunicare con il server di google OAuth. Ricorda l'URL SSL è stato utilizzato in precedenza ( `https://localhost:44300/` a meno che non è stato creato altri progetti SSL).
10. Fare clic su di **Create Client ID** pulsante.
11. Nel menu a sinistra di Google Developers Console, fare clic su di **schermata consenso** voce di menu, quindi impostare il nome di prodotto e l'indirizzo di posta elettronica. Dopo aver completato il form, fare clic su **salvare**.
12. Fare clic su di **API** voce di menu, scorrere verso il basso e fare clic su di **off** accanto al pulsante **Google + API**.   
    Accettazione di questa opzione consentirà l'API di Google +.
13. È necessario aggiornare anche il **italiano** pacchetto NuGet alla versione 3.0.0.   
    Dal **strumenti** dal menu **Gestione pacchetti NuGet** e quindi selezionare **Gestisci pacchetti NuGet per la soluzione**.  
    Dal **Gestisci pacchetti NuGet** finestra Trova e aggiornamento di **italiano** pacchetto alla versione 3.0.0.
14. In Visual Studio, è possibile aggiornare il `UseGoogleAuthentication` metodo il *Startup.Auth.cs* pagina utilizzando la copia e Incolla il **ID Client** e **segreto Client** nel metodo. Il **ID Client** e **segreto Client** valori riportati di seguito sono riportati alcuni esempi e non funzionerà. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Premere **CTRL + F5** per compilare ed eseguire l'applicazione. Fare clic su di **Accedi** collegamento.
16. In **utilizzare un altro servizio per l'accesso**, fare clic su **Google**.  
    ![Accedi](checkout-and-payment-with-paypal/_static/image11.png)
17. Se è necessario immettere le credenziali, si verrà reindirizzati al sito di google dove inserire le credenziali.  
    ![Google - accesso](checkout-and-payment-with-paypal/_static/image12.png)
18. Dopo aver immesso le credenziali, verrà richiesto di concedere le autorizzazioni per l'applicazione web che appena creato.  
    ![Account servizio di progetto predefinito](checkout-and-payment-with-paypal/_static/image13.png)
19. Fare clic su **accettare**. Si verrà reindirizzati al **registrare** pagina il **WingtipToys** applicazione in cui è possibile registrare l'account Google.  
    ![Registrare con l'Account di Google](checkout-and-payment-with-paypal/_static/image14.png)
20. È possibile scegliere di modificare il nome di registrazione di posta elettronica locale utilizzato per l'account Gmail, ma in genere si desidera mantenere l'alias di posta elettronica predefinito (vale a dire quello utilizzato per l'autenticazione). Fare clic su **Accedi** come illustrato in precedenza.

### <a name="modifying-login-functionality"></a>Modifica la funzionalità di accesso

Come accennato in precedenza in questa serie di esercitazioni, molte delle funzionalità di registrazione dell'utente è stato incluso nel modello Web Form ASP.NET per impostazione predefinita. Ora si modificherà il valore predefinito *Login.aspx* e *Register* pagine per richiamare il `MigrateCart` metodo. Il `MigrateCart` metodo associa ha appena effettuato l'accesso a un carrello acquisti anonimo. Associando l'utente e carrello degli acquisti, l'applicazione di esempio Wingtip Toys sarà in grado di garantire il carrello acquisti dell'utente tra visite.

1. In **Esplora**, trovare e aprire il *Account* cartella.
2. Modificare la pagina code-behind denominata *Login.aspx.cs* per includere il codice evidenziato in giallo, in modo che venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Salvare il *Login.aspx.cs* file.

Per il momento, è possibile ignorare l'avviso che è presente alcuna definizione per il `MigrateCart` metodo. Si aggiungerà un po' più avanti in questa esercitazione.

Il *Login.aspx.cs* file code-behind supporta un metodo di accesso. Esaminando la pagina Login, si noterà che questa pagina include un pulsante "Accedi" che, se fare clic su trigger di `LogIn` gestore nel code-behind.

Quando il `Login` metodo il *Login.aspx.cs* viene chiamato, una nuova istanza del carrello acquisti denominato `usersShoppingCart` viene creato. L'ID del carrello acquisti (GUID) viene recuperato e impostare il `cartId` variabile. Quindi, `MigrateCart` metodo viene chiamato, passando entrambi il `cartId` e il nome dell'utente connesso a questo metodo. Quando viene eseguita la migrazione il carrello acquisti, il GUID utilizzato per identificare il carrello acquisti anonimo viene sostituito con il nome utente.

Oltre a modificare il *Login.aspx.cs* file code-behind per eseguire la migrazione del carrello degli acquisti quando l'utente effettua l'accesso, è necessario modificare anche il *file code-behind Register.aspx.cs* per eseguire la migrazione il carrello acquisti Quando l'utente crea un nuovo account ed effettua l'accesso.

1. Nel *Account* cartella, aprire il file code-behind denominato *Register.aspx.cs*.
2. Modificare il file code-behind includendo il codice in giallo, in modo che venga visualizzata come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Salvare il *Register.aspx.cs* file. In questo caso, ignorare l'avviso sul `MigrateCart` metodo.

Si noti che il codice utilizzato nel `CreateUser_Click` è molto simile a quello utilizzato nel gestore dell'evento di `LogIn` metodo. Quando l'utente registra o si connette il sito, una chiamata al `MigrateCart` metodo verrà effettuato.

## <a name="migrating-the-shopping-cart"></a>La migrazione il carrello acquisti

Dopo aver creato il processo di log e di registrazione aggiornato, è possibile aggiungere il codice per eseguire la migrazione il carrello acquisti tramite il `MigrateCart` metodo.

1. In **Esplora**, trovare il *logica* cartella e aprire il *ShoppingCartActions.cs* file di classe.
2. Aggiungere il codice evidenziato in giallo per il codice esistente nel *ShoppingCartActions.cs* file, in modo che il codice di *ShoppingCartActions.cs* file viene visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Il `MigrateCart` metodo utilizza il cartId esistente per trovare il carrello acquisti dell'utente. Successivamente, il codice scorre in ciclo gli elementi del carrello acquisti e sostituisce il `CartId` proprietà (come specificato da di `CartItem` schema) con il nome dell'utente connesso.

### <a name="updating-the-database-connection"></a>Aggiornare la connessione al Database

Se si segue questa esercitazione utilizzando il **predefiniti** Wingtip Toys di esempio, è necessario ricreare il database delle appartenenze predefinito. Modificando la stringa di connessione predefinita, verrà creato il database delle appartenenze alla successiva esecuzione dell'applicazione.

1. Aprire il *Web. config* file alla radice del progetto.
2. Aggiornare la stringa di connessione predefinito in modo che venga visualizzata come segue:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>L'integrazione di PayPal

PayPal è una piattaforma di fatturazione basato sul web che accetta i pagamenti da aziende online. Successivamente, in questa esercitazione viene illustrato come integrare funzionalità di estrazione Express PayPal nell'applicazione. Estrazione Express consente ai clienti di utilizzare PayPal per pagare gli elementi che hanno aggiunto al carrello.

### <a name="create-paylpal-test-accounts"></a>Creare gli account di prova PaylPal

Per utilizzare l'ambiente di testing di PayPal, è necessario creare e verificare un account sviluppatore di test. Si utilizzerà l'account di prova per sviluppatori per creare un acquirente di account di prova e un account di prova del venditore. Le credenziali dell'account per sviluppatori test consentirà inoltre l'applicazione di esempio Wingtip Toys accedere all'ambiente di testing di PayPal.

1. In un browser, passare allo sviluppatore di PayPal test del sito:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Se non si dispone di un account sviluppatore PayPal, creare un nuovo account facendo **iscrizione**e seguendo la procedura di iscrizione. Se si dispone di un account sviluppatore PayPal esistente, effettuare l'accesso facendo clic su **Accedi**. È necessario che l'account sviluppatore di PayPal per testare l'applicazione di esempio Wingtip Toys più avanti in questa esercitazione.
3. Se hanno appena effettuato l'iscrizione per l'account sviluppatore PayPal, occorre verificare il tuo account sviluppatore PayPal con PayPal. È possibile verificare l'account seguendo i passaggi da PayPal inviato al proprio account di posta elettronica. Dopo aver verificato l'account sviluppatore PayPal, accedere di nuovo il test del sito per sviluppatori di PayPal.
4. Dopo che si è connessi al sito per sviluppatori di PayPal per l'account sviluppatore PayPal che è necessario creare un account di prova dell'acquirente di PayPal se non già presente. Per creare un account di prova dell'acquirente, fare clic su sito PayPal il **applicazioni** scheda e quindi fare clic su **account Sandbox**.   
 Il **gli account di prova Sandbox** viene visualizzata la pagina.   

    > [!NOTE] 
    > 
    > Il sito per sviluppatori di PayPal fornisce già un account di prova di carta di credito.

    ![Estrazione e pagamento PayPal - account di prova Sandbox](checkout-and-payment-with-paypal/_static/image15.png)
5. Nella pagina account test Sandbox, fare clic su **crea Account**.
6. Nel **crea account di prova** pagina scegliere un acquirente di posta elettronica dell'account test e la password di propria scelta.   

    > [!NOTE] 
    > 
    > È necessario l'indirizzi di posta elettronica dell'acquirente e la password per testare l'applicazione di esempio Wingtip Toys alla fine di questa esercitazione.

    ![Estrazione e pagamento PayPal - account di prova Sandbox](checkout-and-payment-with-paypal/_static/image16.png)
7. Creare l'account di prova dell'acquirente facendo il **crea Account** pulsante.  
 Il **gli account di prova Sandbox** viene visualizzata la pagina. 

    ![Estrazione e pagamento PayPal - PaylPal account](checkout-and-payment-with-paypal/_static/image17.png)
8. Nel **gli account di prova Sandbox** pagina, fare clic su di **moderatore** account di posta elettronica.  
    **Profilo** e **notifica** vengono visualizzate le opzioni.
9. Selezionare il **profilo** opzione, quindi fare clic su **credenziali API** per visualizzare le credenziali di API per l'account di prova di carta di credito.
10. Copiare le credenziali di API del TEST nel blocco note.

Le credenziali di API del TEST classico visualizzate (nome utente, Password e firma) per effettuare chiamate API dall'applicazione di esempio Wingtip Toys sarà necessario di PayPal ambiente di testing. Le credenziali verranno aggiunti nel passaggio successivo.

### <a name="add-paypal-class-and-api-credentials"></a>Aggiungere la classe PayPal e le credenziali di API

Si inseriranno la maggior parte del codice PayPal in un'unica classe. Questa classe contiene i metodi utilizzati per comunicare con PayPal. Inoltre, si aggiungerà le credenziali di PayPal per questa classe.

1. Nell'applicazione di esempio Wingtip Toys all'interno di Visual Studio, fare doppio clic su di **logica** cartella e quindi selezionare **Aggiungi**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. In **Visual c#** dal **installato** riquadro a sinistra, seleziona **codice**.
3. Nel riquadro centrale, selezionare **classe**. Denominare la nuova classe **PayPalFunctions.cs**.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe viene visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Aggiungere le credenziali di API Merchant (Username, Password e firma) visualizzati in precedenza in questa esercitazione in modo che è possibile effettuare chiamate di funzione per l'ambiente di testing di PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> In questa applicazione di esempio si siano aggiungendo semplicemente le credenziali in un file c# (cs). In una soluzione implementata, è consigliabile crittografare le credenziali in un file di configurazione.


La classe NVPAPICaller contiene la maggior parte delle funzionalità PayPal. Il codice nella classe fornisce i metodi necessari per eseguire una prova di acquisto dall'ambiente di testing di PayPal. Le funzioni di PayPal tre seguenti consentono di effettuare acquisti:

- `SetExpressCheckout` (Funzione)
- `GetExpressCheckoutDetails` (Funzione)
- `DoExpressCheckoutPayment` (Funzione)

Il `ShortcutExpressCheckout` metodo consente di raccogliere i dettagli di prodotto e informazioni di acquisto test dal carrello acquisti e chiama il `SetExpressCheckout` funzione PayPal. Il `GetCheckoutDetails` metodo conferma i dettagli di acquisto e chiamate di `GetExpressCheckoutDetails` funzione PayPal prima di effettuare l'acquisto di test. Il `DoCheckoutPayment` metodo viene completato l'acquisto di test dall'ambiente di testing chiamando il `DoExpressCheckoutPayment` funzione PayPal. Il codice rimanente supporta i metodi di PayPal e un processo, ad esempio la codifica delle stringhe, la decodifica delle stringhe, matrici di elaborazione e determinare le credenziali.

> [!NOTE] 
> 
> PayPal consente di includere i dettagli di acquisto facoltativa in base a [specifica API di PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Estendendo il codice nell'applicazione di esempio Wingtip Toys, è possibile includere dettagli di localizzazione, le descrizioni dei prodotti, imposte, un numero di servizio clienti, nonché molti altri campi facoltativi.


Si noti che gli URL di ritorno e Annulla specificati nel **ShortcutExpressCheckout** metodo utilizza un numero di porta.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Quando Visual Web Developer viene eseguito un progetto web tramite SSL, in genere la porta 44300 viene utilizzata per il server web. Come illustrato in precedenza, il numero di porta è 44300. Quando si esegue l'applicazione, è possibile visualizzare un numero di porta diverso. Le esigenze di numero di porta da impostare correttamente nel codice in modo che sia possibile ha esito positivo di eseguire l'applicazione di esempio Wingtip Toys alla fine di questa esercitazione. La sezione successiva di questa esercitazione viene illustrato come recuperare il numero di porta dell'host locale e aggiornare la classe di PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aggiornare il numero di porta LocalHost nella classe PayPal

L'applicazione di esempio Wingtip Toys gli acquisti di prodotti passando al sito di test di PayPal e restituire l'istanza locale dell'applicazione di esempio Wingtip Toys. Per avere PayPal restituito per l'URL sia corretto, è necessario specificare il numero di porta dell'esecuzione in locale nel codice PayPal indicato in precedenza applicazione di esempio.

1. Fare doppio clic sul nome del progetto (**WingtipToys**) in **Esplora** e selezionare **proprietà**.
2. Nella colonna sinistra, selezionare il **Web** scheda.
3. Recuperare il numero di porta di **Url progetto** casella.
4. Se necessario, aggiornare il `returnURL` e `cancelURL` nella classe PayPal (`NVPAPICaller`) nei *PayPalFunctions.cs* file da utilizzare il numero di porta dell'applicazione web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Ora il codice aggiunto verrà corrispondere alla porta previsto per l'applicazione Web locale. PayPal sarà in grado di restituire l'URL corretto nel computer locale.

### <a name="add-the-paypal-checkout-button"></a>Aggiungere il pulsante di estrazione di PayPal

Ora che le funzioni principali di PayPal sono stati aggiunti all'applicazione di esempio, è possibile iniziare ad aggiungere il markup e il codice necessario per chiamare queste funzioni. In primo luogo, è necessario aggiungere il pulsante di estrazione che l'utente verrà visualizzato nella pagina carrello acquisti.

1. Aprire il *ShoppingCart.aspx* file.
2. Scorrere fino alla fine del file e individuare il `<!--Checkout Placeholder -->` commento.
3. Sostituire il commento con un `ImageButton` controllare in modo che il markup viene sostituito come indicato di seguito:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. Nel *ShoppingCart.aspx.cs* file, dopo il `UpdateBtn_Click` gestore verso la fine del file, aggiungere il `CheckOutBtn_Click` gestore eventi:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Anche nel *ShoppingCart.aspx.cs* file, aggiungere un riferimento di `CheckoutBtn`, in modo che il nuovo pulsante immagine viene fatto riferimento come indicato di seguito:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Salvare le modifiche apportate a entrambi il *ShoppingCart.aspx* file e *ShoppingCart.aspx.cs* file.
7. Nel menu, selezionare **Debug**-&gt;**compilare WingtipToys**.  
   Il progetto verrà ricompilato con appena aggiunta **ImageButton** controllo.

### <a name="send-purchase-details-to-paypal"></a>Invia dettagli acquisto PayPal

Quando l'utente fa clic il **estrazione** pulsante nella pagina carrello acquisti (*ShoppingCart.aspx*), vengono innanzitutto il processo di acquisto. Il codice seguente chiama la prima funzione PayPal necessaria per l'acquisto di prodotti.

1. Dal *estrazione* cartella, aprire il file code-behind denominato *CheckoutStart.aspx.cs*.   
   Assicurarsi di aprire il file code-behind.
2. Sostituire il codice esistente con quello seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Quando l'utente dell'applicazione sceglie la **estrazione** pulsante nella pagina carrello acquisti, il browser per passare al *CheckoutStart.aspx* pagina. Quando il *CheckoutStart.aspx* , caricamento della pagina di `ShortcutExpressCheckout` metodo viene chiamato. A questo punto, l'utente viene trasferita al sito web test PayPal. Nel sito di PayPal, l'utente immette le credenziali di PayPal, esamina i dettagli di acquisto, accetta il contratto di PayPal e restituisce all'applicazione di esempio Wingtip Toys in cui il `ShortcutExpressCheckout` metodo viene completato. Quando il `ShortcutExpressCheckout` metodo viene completato, si verrà reindirizzati all'utente il *CheckoutReview.aspx* specificata nella pagina di `ShortcutExpressCheckout` (metodo). Ciò consente all'utente di esaminare i dettagli degli ordini all'interno dell'applicazione di esempio Wingtip Toys.

### <a name="review-order-details"></a>Esaminare i dettagli di ordine

Dopo la restituzione da PayPal, il *CheckoutReview.aspx* pagina dell'applicazione di esempio Wingtip Toys consente di visualizzare i dettagli dell'ordine. Questa pagina consente all'utente di esaminare i dettagli dell'ordine prima di acquistare i prodotti. Il *CheckoutReview.aspx* pagina deve essere creata come indicato di seguito:

1. Nel *estrazione* cartella, aprire la pagina denominata *CheckoutReview.aspx*.
2. Sostituire il codice esistente con il seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Aprire la pagina code-behind denominata *CheckoutReview.aspx.cs* e sostituire il codice esistente con il seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Il **DetailsView** controllo viene utilizzato per visualizzare i dettagli dell'ordine che sono stati restituiti da PayPal. Inoltre, il codice sopra riportato Salva i dettagli dell'ordine nel database Wingtip Toys come un `OrderDetail` oggetto. Quando l'utente fa clic su di **Order completa** pulsante, vengono reindirizzati per il *CheckoutComplete.aspx* pagina.

> [!NOTE] 
> 
> **Suggerimento**
> 
> Nel markup del *CheckoutReview.aspx* pagina, si noti che il `<ItemStyle>` tag è utilizzato per modificare lo stile degli elementi all'interno di **DetailsView** controllo nella parte inferiore della pagina. Visualizzando la pagina in **visualizzazione Progettazione** (selezionando **progettazione** nell'angolo inferiore sinistro di Visual Studio), quindi selezionando il **DetailsView** controllo e selezionando il ** Smart Tag** (l'icona freccia nella parte superiore destra del controllo), sarà in grado di visualizzare il **Attività DetailsView**.
> 
> ![Estrazione e pagamento PayPal - modificare i campi](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Selezionando **modifica campi**, **campi** verrà visualizzata la finestra di dialogo. In questa finestra di dialogo è possibile controllare facilmente le proprietà visive, ad esempio **ItemStyle**, del **DetailsView** controllo.
> 
> ![Estrazione e pagamento PayPal - finestra di dialogo campi](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Completare l'acquisto

*CheckoutComplete.aspx* pagina effettua l'acquisto da PayPal. Come indicato in precedenza, l'utente deve fare clic su di **Order completa** pulsante prima che l'applicazione verrà visualizzata la *CheckoutComplete.aspx* pagina.

1. Nel *estrazione* cartella, aprire la pagina denominata *CheckoutComplete.aspx*.
2. Sostituire il codice esistente con il seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Aprire la pagina code-behind denominata *CheckoutComplete.aspx.cs* e sostituire il codice esistente con il seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Quando il *CheckoutComplete.aspx* pagina viene caricata, il `DoCheckoutPayment` metodo viene chiamato. Come accennato in precedenza, il `DoCheckoutPayment` metodo viene completato l'acquisto dall'ambiente di testing di PayPal. Una volta completato l'acquisto dell'ordine, PayPal il *CheckoutComplete.aspx* pagina consente di visualizzare una transazione di pagamento `ID` all'acquirente.

### <a name="handle-cancel-purchase"></a>Gestire l'acquisto di annullamento

Se l'utente decide di annullare l'acquisto, verranno indirizzati al *CheckoutCancel.aspx* pagina in cui essi visualizzeranno che l'ordine è stata annullata.

1. Aprire la pagina denominata *CheckoutCancel.aspx* nel *estrazione* cartella.
2. Sostituire il codice esistente con il seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Gestire gli errori di acquisto

Errori durante il processo di acquisto verranno gestiti dal *CheckoutError.aspx* pagina. Il code-behind del *CheckoutStart.aspx* pagina il *CheckoutReview.aspx* pagina e *CheckoutComplete.aspx* ciascuna pagina verrà reindirizzata al * CheckoutError.aspx* pagina se si verifica un errore.

1. Aprire la pagina denominata *CheckoutError.aspx* nel *estrazione* cartella.
2. Sostituire il codice esistente con il seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

Il *CheckoutError.aspx* pagina viene visualizzata con i dettagli dell'errore quando si verifica un errore durante il processo di estrazione.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Eseguire l'applicazione per informazioni sulle modalità di acquisto dei prodotti. Si noti che verrà eseguita di PayPal ambiente di testing. Non viene scambiato money effettivo.

1. Verificare che tutti i file vengono salvati in Visual Studio.
2. Aprire un Web browser e passare a [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Account di accesso con l'account sviluppatore PayPal creato precedentemente in questa esercitazione.  
   Per sandbox di PayPal per sviluppatori, è necessario avere effettuato l'accesso al [ https://developer.paypal.com ](https://developer.paypal.com/) per testare estrazione express. Si applica solo a test, non all'ambiente in tempo reale di PayPal sandbox di PayPal.
4. In Visual Studio, premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
   Dopo aver ricompilato il database, aprire il browser e visualizzare il *Default.aspx* pagina.
5. Aggiungere i tre prodotti diversi per il carrello acquisti selezionando la categoria di prodotto, ad esempio "Auto" e quindi fare clic su **Aggiungi al carrello** accanto a ogni prodotto.  
   Il carrello acquisti verrà visualizzato il prodotto che è stato selezionato.
6. Fare clic su di **PayPal** pulsante di estrazione. 

    ![Estrazione e pagamento PayPal - carrello](checkout-and-payment-with-paypal/_static/image20.png)

   Estrazione richiederà di disporre di un account utente per l'applicazione di esempio Wingtip Toys.
7. Fare clic su di **Google** collegamento a destra della pagina di accesso con un account di posta elettronica gmail.com esistente.  
   Se non si dispone di un account gmail.com, è possibile crearne uno per scopi di test [www.gmail.com](https://www.gmail.com/). È inoltre possibile utilizzare un account locale standard facendo clic su "Register". 

    ![Estrazione e pagamento PayPal - Log in](checkout-and-payment-with-paypal/_static/image21.png)
8. Accedere con l'account gmail e la password. 

    ![Estrazione e pagamento PayPal - Gmail Accedi](checkout-and-payment-with-paypal/_static/image22.png)
9. Fare clic su di **Accedi** pulsante per registrare l'account gmail con il nome utente dell'applicazione di esempio Wingtip Toys. 

    ![Estrazione e pagamento PayPal - registrazione Account](checkout-and-payment-with-paypal/_static/image23.png)
10. Nel sito di test di PayPal, aggiungere il **buyer** di posta elettronica indirizzo e la password creata precedentemente in questa esercitazione, quindi fare clic su di **Accedi** pulsante. 

    ![Estrazione e pagamento PayPal - PayPal Accedi](checkout-and-payment-with-paypal/_static/image24.png)
11. I criteri di PayPal per accettare e fare clic su di **Accetto** pulsante.  
    Si noti che questa pagina è visualizzato la prima volta, si utilizza questo account PayPal. Nuovo si noti che questo è un account di test non money reali vengono scambiati. 

    ![Estrazione e pagamento PayPal - PayPal criteri](checkout-and-payment-with-paypal/_static/image25.png)
12. Esaminare le informazioni dell'ordine di PayPal verifica pagina di revisione dell'ambiente e fare clic su **continua**. 

    ![Estrazione e pagamento PayPal - rivedere le informazioni](checkout-and-payment-with-paypal/_static/image26.png)
13. Nel *CheckoutReview.aspx* pagina, verificare l'importo dell'ordine e visualizzare l'indirizzo di spedizione generato. Quindi, fare clic su di **Order completa** pulsante. 

    ![Estrazione e pagamento PayPal - ordine revisione](checkout-and-payment-with-paypal/_static/image27.png)
14. Il **CheckoutComplete.aspx** viene visualizzata la pagina con un ID di transazione di pagamento. 

    ![Estrazione e pagamento PayPal - Checkpoint completato](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Verifica del Database

Esaminando i dati aggiornati nel database dell'applicazione di esempio Wingtip Toys dopo l'esecuzione dell'applicazione, si noterà che l'applicazione viene registrata correttamente l'acquisto dei prodotti.

È possibile controllare i dati contenuti nel *Wingtiptoys.mdf* file di database utilizzando il **Esplora Database** finestra (**Esplora Server** finestra in Visual Studio) come in precedenza in questa serie di esercitazioni.

1. Se è ancora aperto, chiudere la finestra del browser.
2. In Visual Studio, selezionare il **Mostra tutti i file** in alto di **Esplora** consente di espandere il **App\_dati** cartella.
3. Espandere il **App\_dati** cartella.  
 È necessario selezionare il **Mostra tutti i file** icona della cartella.
4. Fare doppio clic su di *Wingtiptoys.mdf* file di database e selezionare **aprire**.  
    **Esplora server** viene visualizzato.
5. Espandere il **tabelle** cartella.
6. Fare doppio clic su di **ordini**tabella e selezionare **Mostra dati tabella**.  
 Il **ordini** tabella viene visualizzata.
7. Esaminare il **PaymentTransactionID** colonna per confermare di transazioni completate. 

    ![Estrazione e pagamento PayPal - Database revisione](checkout-and-payment-with-paypal/_static/image29.png)
8. Chiudi il **ordini** finestra della tabella.
9. In Esplora Server, fare doppio clic su di **OrderDetails** tabella e selezionare **Mostra dati tabella**.
10. Esaminare il `OrderId` e `Username` i valori di **OrderDetails** tabella. Si noti che questi valori corrispondono il `OrderId` e `Username` valori inclusi nel **ordini** tabella.
11. Chiudi il **OrderDetails** finestra della tabella.
12. Il file di database Wingtip Toys (*Wingtiptoys.mdf*) e selezionare **chiusura connessione**.
13. Se non viene visualizzato il **Esplora** finestra, fare clic su **Esplora soluzioni** nella parte inferiore del **Esplora Server** finestra per visualizzare il **Esplora soluzioni ** nuovamente.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato aggiunto ordine e schemi di dettagli ordine per tenere traccia dell'acquisto dei prodotti. Inoltre integrato PayPal funzionalità all'applicazione di esempio Wingtip Toys.

## <a name="additional-resources"></a>Risorse aggiuntive

[Cenni preliminari sulla configurazione di ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Distribuire un'App di moduli Web ASP.NET in modo sicuro con l'appartenenza, OAuth e il Database SQL al servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Questa esercitazione contiene codice di esempio. Tale codice di esempio viene fornito "così com'è" senza garanzia di alcun tipo. Di conseguenza, Microsoft non garantisce l'accuratezza, integrità o qualità del codice di esempio. Si accetta di utilizzare il codice di esempio a proprio rischio. In nessun caso Microsoft sarà responsabile per l'utente in alcun modo per qualsiasi codice di esempio, il contenuto, inclusi, a titolo esemplificativo, eventuali errori o omissioni in qualsiasi codice di esempio, il contenuto, o eventuali perdite o danni di qualsiasi tipo generato come risultato l'utilizzo di qualsiasi codice di esempio. Così di notifica e decidono indennizzare, salvare e contenere nei confronti di perdita di tutte, le attestazioni di perdita, lesioni personali o di danni di qualsiasi tipo inclusi, a titolo esemplificativo, quelli risultanti dall'o derivanti dal materiale in cui si registra, Microsoft trasmettere, utilizzare o si basano sull'inclusione via esemplificativa, le opinioni espresse al suo interno.

> [!div class="step-by-step"]
> [Precedente](shopping-cart.md)
> [Successivo](membership-and-administration.md)
