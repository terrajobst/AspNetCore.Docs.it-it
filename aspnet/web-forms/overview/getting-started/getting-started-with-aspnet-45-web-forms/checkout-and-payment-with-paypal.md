---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Completamento della transazione e pagamento con PayPal | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 3299da33a68f02ac1b3ffe7c037d06d8ece9455e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835631"
---
<a name="checkout-and-payment-with-paypal"></a>Completamento della transazione e pagamento con PayPal
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


Questa esercitazione descrive come modificare l'applicazione di esempio Wingtip Toys per includere l'autorizzazione utente, registrazione e il pagamento con PayPal. Solo gli utenti che hanno effettuati l'accesso in disporrà di autorizzazione per l'acquisto di prodotti. La funzionalità di registrazione utente predefiniti del modello di progetto Web Form ASP.NET 4.5 include già gran parte di ciò che occorre. Si aggiungerà la funzionalità di estrazione Express PayPal. In questa esercitazione è usare lo sviluppatore di PayPal ambiente di test in modo che nessun fondi effettivi verranno trasferiti. Al termine dell'esercitazione, si testerà l'applicazione, selezionare i prodotti per aggiungere al carrello della spesa, facendo clic sul pulsante di estrazione e il trasferimento dei dati per il sito web test PayPal. Nel sito web test PayPal, verrà verificare le informazioni di pagamento e spedizione e quindi tornare all'applicazione locale per verificare e completare l'acquisto di esempio Wingtip Toys.

Esistono diversi processori esperti pagamento di terze parti che specializzano nel effettuare shopping online, la scalabilità di indirizzo e sicurezza. Gli sviluppatori ASP.NET devono prendere in considerazione i vantaggi dell'utilizzo di una soluzione di terze parti pagamento prima di implementare un carrello e acquistare soluzioni.

> [!NOTE] 
> 
> L'applicazione di esempio Wingtip Toys è stato progettato per illustrato concetti specifici di ASP.NET e le funzionalità disponibili per gli sviluppatori web ASP.NET. Questa applicazione di esempio non è stata ottimizzata per tutti i possibili rischi per quanto riguarda la scalabilità e sicurezza.


## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come limitare l'accesso alle pagine specifiche in una cartella.
- Come creare un carrello acquisti noto da un carrello acquisti anonimo.
- Come abilitare SSL per il progetto.
- Come aggiungere un provider OAuth per il progetto.
- Come usare PayPal acquistare prodotti tramite l'ambiente di testing di PayPal.
- Come visualizzare i dettagli da PayPal in una **DetailsView** controllo.
- Come aggiornare il database dell'applicazione Wingtip Toys con informazioni dettagliate ottenute da PayPal.

## <a name="adding-order-tracking"></a>Aggiunta di tracciabilità degli ordini

In questa esercitazione si creerà due nuove classi per tenere traccia dei dati dall'ordine di che un utente ha creato. Le classi tengono traccia di dati riguardanti le informazioni di spedizione totale di acquisto e conferma di pagamento.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Aggiungere l'ordine e le classi del modello OrderDetail

In precedenza in questa serie di esercitazioni, è stato definito lo schema per le categorie, prodotti, e carrello acquisti elementi creando la `Category`, `Product`, e `CartItem` le classi nel *modelli* cartella. Ora si aggiungeranno due nuove classi per definire lo schema per l'ordine di prodotto e i dettagli dell'ordine.

1. Nel **modelli** cartella, aggiungere una nuova classe denominata *Order.cs*.   
   Il nuovo file di classe viene visualizzato nell'editor.
2. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Aggiungere un *OrderDetail.cs* classe per il *modelli* cartella.
4. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Il `Order` e `OrderDetail` classi contengono lo schema per definire le informazioni dell'ordine usate per l'acquisto e di spedizione.

Inoltre, è necessario aggiornare la classe del contesto del database che gestisce le classi di entità e che fornisce accesso ai dati nel database. A tale scopo, si aggiungerà l'ordine appena creato e `OrderDetail` classi di modello `ProductContext` classe.

1. Nelle **Esplora soluzioni**individuare e aprire il *ProductContext.cs* file.
2. Aggiungere il codice evidenziato per il *ProductContext.cs* file come illustrato di seguito:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Come accennato in precedenza in questa serie di esercitazioni, il codice nel *ProductContext.cs* file aggiunge il `System.Data.Entity` dello spazio dei nomi in modo da poter accedere a tutte le funzionalità core di Entity Framework. Questa funzionalità include la possibilità di eseguire una query, inserire, aggiornare ed eliminare dati utilizzando oggetti fortemente tipizzati. Il codice sopra riportato nel `ProductContext` classe aggiunge l'accesso di Entity Framework appena aggiunto `Order` e `OrderDetail` classi.

## <a name="adding-checkout-access"></a>Aggiunta dell'accesso di estrazione

L'applicazione di esempio Wingtip Toys consente agli utenti anonimi di rivedere e aggiungere prodotti al carrello acquisti. Tuttavia, quando gli utenti anonimi scelgono di acquistare i prodotti che vengono aggiunte al carrello acquisti, è necessario accedere al sito. Una volta che hanno effettuato l'accesso, è possibile accedere alle pagine con restrizioni dell'applicazione Web che gestiscono il checkout e processo di acquisto. Queste pagine con restrizioni sono contenute nel *Checkout* cartella dell'applicazione.

### <a name="add-a-checkout-folder-and-pages"></a>Aggiungere una cartella di estrazione e le pagine

Ora si creerà il *Checkout* cartella e le pagine che verrà visualizzato al cliente durante il processo di estrazione. Queste pagine verranno aggiornate più avanti in questa esercitazione.

1. Fare clic sul nome del progetto (**Wingtip Toys**) nel **Esplora soluzioni** e selezionare **aggiungere una nuova cartella**. 

    ![Completamento della transazione e pagamento con PayPal - nuova cartella](checkout-and-payment-with-paypal/_static/image1.png)
2. Denominare la nuova cartella *Checkout*.
3. Fare doppio clic il *Checkout* cartella e quindi selezionare **Add**-&gt;**nuovo elemento**. 

    ![Completamento della transazione e pagamento con PayPal - nuovo elemento](checkout-and-payment-with-paypal/_static/image2.png)
4. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
5. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Quindi, nel riquadro centrale, selezionare **Web Form con pagina Master**e denominarlo *CheckoutStart.aspx*. 

    ![Completamento della transazione e pagamento con PayPal - Aggiungi finestra di dialogo Nuovo elemento](checkout-and-payment-with-paypal/_static/image3.png)
6. Come in precedenza, selezionare la *Site. master* file come pagina master.
7. Aggiungere le seguenti pagine aggiuntive per il *Checkout* cartella usando la stessa procedura precedente:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Aggiungere un File Web. config

Aggiungendo una nuova *Web. config* del file per il *Checkout* cartella, sarà possibile limitare l'accesso a tutte le pagine contenute nella cartella.

1. Fare doppio clic il *Checkout* cartella e selezionare **Add**  - &gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Quindi, nel riquadro centrale, selezionare **Web. config**, accettare il nome predefinito della *Web. config*e quindi selezionare **Add**.
3. Sostituire il contenuto nel codice XML esistente di *Web. config* file con il codice seguente:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Salvare il *Web. config* file.

Il *Web. config* file specifica che tutti gli utenti sconosciuti dell'applicazione Web devono essere negati l'accesso alle pagine di contenuto nel *Checkout* cartella. Tuttavia, se l'utente ha registrato un account ed effettuato l'accesso, che sarà un utente noto e sarà possibile accedere alle pagine di *Checkout* cartella.

È importante notare che la configurazione ASP.NET segue una gerarchia, in cui ciascuna *Web. config* file Applica impostazioni di configurazione per la cartella che si trova in e per tutte le directory figlio sotto di essa.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Abilitare SSL per il progetto

 Sicuro Sockets Layer (SSL) è un protocollo definito per consentire i server Web e client Web per comunicare in modo più sicuro tramite l'uso di crittografia. Quando SSL non viene utilizzato, i dati inviati tra il client e il server sono aperti a sniffing dei pacchetti da chiunque abbia accesso alla rete. Inoltre, numerosi schemi di autenticazione comuni non sono protetti tramite HTTP semplice. In particolare, l'autenticazione di base e autenticazione basata su form inviano credenziali non crittografate. Per essere sicuri, questi schemi di autenticazione devono usare SSL. 

1. Nelle **Esplora soluzioni**, fare clic sul **WingtipToys** del progetto, quindi premere **F4** per visualizzare il **proprietà** finestra.
2. Change **SSL abilitato** a `true`.
3. Copia il **URL SSL** quindi è possibile usare in un secondo momento.   
 L'URL SSL sarà `https://localhost:44300/` a meno che non è stato creato in precedenza siti Web SSL (come illustrato di seguito).   
    ![Proprietà di progetti](checkout-and-payment-with-paypal/_static/image4.png)
4. Nella **Esplora soluzioni**, fare clic il **WingtipToys** progetto e quindi scegliere **proprietà**.
5. Nella scheda di sinistra, fare clic su **Web**.
6. Modifica il **Url del progetto** da utilizzare il **URL SSL** salvato in precedenza.   
    ![Proprietà del progetto Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Salvare la pagina premendo **CTRL + S**.
8. Premere **CTRL+F5** per eseguire l'applicazione. Visual Studio verrà visualizzata un'opzione che consente di evitare avvisi SSL.
9. Fare clic su **Sì** per considerare attendibile il certificato SSL di IIS Express e continuare.   
    ![Dettagli del certificato SSL di IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Viene visualizzato un avviso di sicurezza.
10. Fare clic su **Sì** per installare il certificato per l'host locale.   
    ![Finestra di dialogo Avviso di sicurezza](checkout-and-payment-with-paypal/_static/image7.png)  
 Verrà visualizzata la finestra del browser.

È ora possibile testare facilmente l'applicazione Web in locale tramite SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Aggiungere un Provider OAuth 2.0

Web Form ASP.NET offre opzioni avanzate per l'autenticazione e l'appartenenza. Questi miglioramenti includono OAuth. OAuth è un protocollo aperto che consente l'autenticazione sicura in un metodo semplice e standard da applicazioni web, mobili e desktop. Il modello Web Form ASP.NET Usa OAuth per esporre Facebook, Twitter, Google e Microsoft come provider di autenticazione. Sebbene questa esercitazione viene usato solo Google come provider di autenticazione, è possibile modificare facilmente il codice per usare uno qualsiasi dei provider. I passaggi per implementare altri provider sono molto simili ai passaggi che verrà visualizzato in questa esercitazione.

Oltre all'autenticazione, l'esercitazione userà anche i ruoli per implementare l'autorizzazione. Solo gli utenti aggiunti per il `canEdit` ruolo sarà in grado di modificare i dati (creare, modificare o eliminare i contatti).

> [!NOTE] 
> 
> Applicazioni Windows Live accettano solo un URL in tempo reale per un sito Web funzionante, pertanto non è possibile utilizzare un URL del sito Web locale per testare gli account di accesso.


La procedura seguente consentirà di aggiungere un provider di autenticazione Google.

1. Aprire il *App\_Start\Startup.Auth.cs* file.
2. Rimuovere i caratteri di commento dal `app.UseGoogleAuthentication()` metodo in modo che il metodo appare come segue: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Passare il [Google Developers Console](https://console.developers.google.com/). È necessario anche effettuare l'accesso con l'account di posta elettronica per gli sviluppatori di Google (gmail.com). Se non hai un account Google, selezionare la **creare un account** collegamento.   
   Successivamente, si noterà il **Google Developers Console**.   
    ![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)
4. Scegliere il **Crea progetto** pulsante e immettere un nome di progetto e ID (è possibile usare i valori predefiniti). Quindi, fare clic sui **casella di controllo di contratto** e il **crea** pulsante.  

    ![Google - nuovo progetto](checkout-and-payment-with-paypal/_static/image9.png)

   In pochi secondi verrà creato il nuovo progetto e il browser visualizzerà la nuova pagina progetti.
5. Nella scheda di sinistra, fare clic su **API &amp; auth**, quindi fare clic su **credenziali**.
6. Scegliere il **Crea nuovo ID Client** sotto **OAuth**.   
   Il **Create Client ID** verrà visualizzata una finestra di dialogo.   
    ![Google - creare ID Client](checkout-and-payment-with-paypal/_static/image10.png)
7. Nel **Create Client ID** finestra di dialogo, mantenere il valore predefinito **applicazione Web** per il tipo di applicazione.
8. Impostare il **Authorized JavaScript Origins** all'URL SSL è stato utilizzato in precedenza in questa esercitazione (`https://localhost:44300/` a meno che non creati altri progetti SSL).   
   Questo URL rappresenta l'origine per l'applicazione. Per questo esempio, sarà necessario immettere solo l'URL di test localhost. Tuttavia, è possibile immettere più URL per conto di localhost e produzione.
9. Impostare il **Authorized Redirect URI** al seguente: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Questo valore è l'URI che ASP.NET OAuth per comunicare con il server OAuth di google. Ricorda l'URL SSL descritti in precedenza ( `https://localhost:44300/` a meno che non creati altri progetti SSL).
10. Scegliere il **Create Client ID** pulsante.
11. Nel menu a sinistra di Google Developers Console, fare clic sui **schermata di consenso** voce di menu, quindi impostare il nome di prodotto e indirizzo di posta elettronica. Dopo aver completato il form, fare clic su **salvare**.
12. Fare clic sul **API** voce di menu, scorrere verso il basso e fare clic sul **off** accanto a **Google + API**.   
    Accettando questa opzione consentirà l'API di Google +.
13. È necessario aggiornare anche il **owin** pacchetto NuGet alla versione 3.0.0.   
    Dal **degli strumenti** dal menu **Gestione pacchetti NuGet** e quindi selezionare **Gestisci pacchetti NuGet per la soluzione**.  
    Dal **Gestisci pacchetti NuGet** finestra di dialogo Trova e aggiornamento di **owin** pacchetto alla versione 3.0.0.
14. In Visual Studio, aggiornare il `UseGoogleAuthentication` metodo per il *Startup.Auth.cs* pagina mediante la copia e Incolla il **ID Client** e **segreto Client** nel metodo. Il **ID Client** e **privata del Client** valori riportati di seguito sono esempi e non funzionerà. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Premere **CTRL+F5** per compilare ed eseguire l'applicazione. Scegliere il **Accedi** collegamento.
16. Sotto **usare un altro servizio per accedere**, fare clic su **Google**.  
    ![Accedi](checkout-and-payment-with-paypal/_static/image11.png)
17. Se è necessario immettere le credenziali, si verrà reindirizzati al sito di google in cui vengono immesse le credenziali.  
    ![Google - accesso](checkout-and-payment-with-paypal/_static/image12.png)
18. Dopo aver immesso le credenziali, sarà necessario concedere autorizzazioni per l'applicazione web che appena creata.  
    ![Account di servizio di progetto predefinito](checkout-and-payment-with-paypal/_static/image13.png)
19. Fare clic su **accettano**. Verrà reindirizzati al **registrare** pagina della **WingtipToys** dell'applicazione in cui è possibile registrare l'account Google.  
    ![Registrare con l'Account Google](checkout-and-payment-with-paypal/_static/image14.png)
20. È possibile scegliere di modificare il nome di registrazione di posta elettronica locale usato per l'account Gmail, ma in genere si vuole mantenere l'alias di posta elettronica predefinito (vale a dire quello usato per l'autenticazione). Fare clic su **Accedi** come illustrato in precedenza.

### <a name="modifying-login-functionality"></a>Modifica di funzionalità di accesso

Come accennato in precedenza in questa serie di esercitazioni, molte delle funzionalità di registrazione dell'utente è stato incluso nel modello Web Form ASP.NET per impostazione predefinita. Ora si modificherà il valore predefinito *Login. aspx* e *Register* pagine per richiamare il `MigrateCart` (metodo). Il `MigrateCart` metodo associa un utente appena connesso a un carrello acquisti anonimo. Associando l'utente e carrello della spesa, l'applicazione di esempio Wingtip Toys sarà in grado di mantenere il carrello acquisti dell'utente all'accesso.

1. Nelle **Esplora soluzioni**individuare e aprire il *Account* cartella.
2. Modificare la pagina code-behind denominata *Login.aspx.cs* per includere il codice evidenziato in giallo, in modo che venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Salvare il *Login.aspx.cs* file.

Per il momento, è possibile ignorare l'avviso che è presente alcuna definizione per il `MigrateCart` (metodo). Si aggiungerà un po' più avanti in questa esercitazione.

Il *Login.aspx.cs* file code-behind supporta un metodo di accesso. Esaminando la pagina Login. aspx, si noterà che questa pagina include un pulsante "Accedi" che, se fare clic su trigger la `LogIn` gestore nel code-behind.

Quando la `Login` metodo sul *Login.aspx.cs* viene chiamato, una nuova istanza del carrello acquisti denominato `usersShoppingCart` viene creato. L'ID del carrello acquisti (un GUID) viene recuperato e impostato il `cartId` variabile. Successivamente, il `MigrateCart` viene chiamato il metodo passando entrambi i `cartId` e il nome dell'utente ha eseguito l'accesso a questo metodo. Quando viene eseguita la migrazione del carrello degli acquisti, il GUID utilizzato per identificare il carrello acquisti anonimo viene sostituito con il nome utente.

Oltre a modificare la *Login.aspx.cs* file code-behind per eseguire la migrazione del carrello degli acquisti quando l'utente effettua l'accesso, è necessario modificare anche il *file code-behind Register.aspx.cs* per eseguire la migrazione del carrello degli acquisti Quando l'utente crea un nuovo account e accede.

1. Nel *conto* cartella, aprire il file code-behind denominato *Register.aspx.cs*.
2. Modificare il file code-behind, includendo il codice in giallo, in modo che venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Salvare il *Register.aspx.cs* file. Ancora una volta, ignorare l'avviso relativo il `MigrateCart` (metodo).

Si noti che il codice usato nel `CreateUser_Click` è molto simile al codice usato nel gestore dell'evento di `LogIn` (metodo). Quando l'utente registra o Registra nel sito di una chiamata al `MigrateCart` metodo verrà effettuato.

## <a name="migrating-the-shopping-cart"></a>Eseguire la migrazione del carrello degli acquisti

Dopo aver creato il processo di log e sulla registrazione aggiornato, è possibile aggiungere il codice per eseguire la migrazione il carrello acquisti tramite il `MigrateCart` (metodo).

1. Nelle **Esplora soluzioni**, trovare il *per la logica* cartella e aprire il *ShoppingCartActions.cs* file di classe.
2. Aggiungere il codice evidenziato in giallo per il codice esistente nel *ShoppingCartActions.cs* file, in modo che il codice nelle *ShoppingCartActions.cs* file viene visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Il `MigrateCart` metodo Usa il cartId esistenti per trovare il carrello acquisti dell'utente. Successivamente, il codice scorre in ciclo tutti gli elementi del carrello acquisti e sostituisce il `CartId` proprietà (come specificato da di `CartItem` schema) con il nome utente connesso.

### <a name="updating-the-database-connection"></a>Aggiornare la connessione al Database

Se si segue questa esercitazione usando il **precompilati** applicazione di esempio Wingtip Toys è necessario ricreare il database di appartenenza predefinito. Modificando la stringa di connessione predefinita, verrà creato il database di appartenenza alla successiva esecuzione dell'applicazione.

1. Aprire il *Web. config* file nella radice del progetto.
2. Aggiornare la stringa di connessione predefinito in modo che venga visualizzato come segue:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>L'integrazione di PayPal

PayPal è una piattaforma di fatturazione basata sul web che accetta i pagamenti da rivenditori online. Successivamente, questa esercitazione illustra come integrare la funzionalità Express Checkout di PayPal all'interno dell'applicazione. Estrazione Express consente ai clienti di usare di PayPal per pagare gli elementi che hanno aggiunto al carrello acquisti.

### <a name="create-paylpal-test-accounts"></a>Creare gli account di prova PaylPal

Per usare l'ambiente di test di PayPal, è necessario creare e verificare un account di test per sviluppatori. Si userà l'account di test per sviluppatori per creare un acquirente di account di prova e un account di prova del venditore. Le credenziali dell'account di test per sviluppatori consentirà anche l'applicazione di esempio Wingtip Toys accedere all'ambiente di testing di PayPal.

1. In un browser, passare al sito di test per sviluppatori di PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Se non hai un account per sviluppatore di PayPal, creare un nuovo account facendo **iscrizione**e seguendo la procedura di iscrizione. Se hai un account per sviluppatori di PayPal esistente, accedere facendo clic **Log In**. È necessario l'account per sviluppatore di PayPal per testare l'applicazione di esempio Wingtip Toys più avanti in questa esercitazione.
3. Se hanno semplicemente effettuato l'iscrizione per l'account per sviluppatore di PayPal, si potrebbe essere necessario verificare l'account per sviluppatore di PayPal con PayPal. È possibile verificare l'account seguendo i passaggi da PayPal inviati all'account di posta elettronica. Dopo avere verificato l'account per sviluppatore di PayPal, accedere di nuovo il test del sito per sviluppatori di PayPal.
4. Dopo che si è connessi al sito per sviluppatori di PayPal con l'account per sviluppatore di PayPal che è necessario creare un account di prova di PayPal buyer se non è già presente. Per creare un account di prova dell'acquirente, fare clic site PayPal sul **Applications** scheda e quindi fare clic su **gli account Sandbox**.   
 Il **account di prova Sandbox** pagina viene visualizzata.   

    > [!NOTE] 
    > 
    > Nel sito per sviluppatori di PayPal fornisce già un account di test commerciale.

    ![Completamento della transazione e pagamento con PayPal - account di prova di Sandbox](checkout-and-payment-with-paypal/_static/image15.png)
5. Nella pagina account di test di Sandbox, fare clic su **creare un Account**.
6. Nel **crea account di test** pagina scegliere un acquirente di posta elettronica di account di test e la password di propria scelta.   

    > [!NOTE] 
    > 
    > È necessario l'indirizzi di posta elettronica dell'acquirente e la password per testare l'applicazione di esempio Wingtip Toys alla fine di questa esercitazione.

    ![Completamento della transazione e pagamento con PayPal - account di prova di Sandbox](checkout-and-payment-with-paypal/_static/image16.png)
7. Creare l'account di test dell'acquirente facendo il **creare un Account** pulsante.  
 Il **gli account di prova di Sandbox** viene visualizzata la pagina. 

    ![Completamento della transazione e pagamento con PayPal - PaylPal account](checkout-and-payment-with-paypal/_static/image17.png)
8. Nel **gli account di prova di Sandbox** pagina, fare clic sui **facilitatore** account di posta elettronica.  
    **Profilo** e **notifica** vengono visualizzate le opzioni.
9. Selezionare il **profilo** opzione e quindi fare clic su **credenziali API** per visualizzare le credenziali di API per l'account di test commerciale.
10. Copiare le credenziali di API del TEST nel blocco note.

Le credenziali di API del TEST classico visualizzate (nome utente, Password e firma) per chiamare le API dall'applicazione di esempio Wingtip Toys è necessario per l'ambiente di test di PayPal. Le credenziali verranno aggiunti nel passaggio successivo.

### <a name="add-paypal-class-and-api-credentials"></a>Aggiungere classe di PayPal e le credenziali di API

Si inserirà la maggior parte del codice di PayPal in un'unica classe. Questa classe contiene i metodi utilizzati per comunicare con PayPal. Inoltre, si aggiungerà le credenziali di PayPal per questa classe.

1. Nell'applicazione di esempio Wingtip Toys all'interno di Visual Studio, fare doppio clic il **per la logica** cartella e quindi selezionare **Add**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Sotto **Visual c#** dalle **installati** riquadro a sinistra, seleziona **codice**.
3. Nel riquadro centrale, selezionare **classe**. Nuova classe il nome **PayPalFunctions.cs**.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe viene visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Aggiungere le credenziali di API Merchant (nome utente, Password e firma) che è visualizzato in precedenza in questa esercitazione in modo che è possibile effettuare chiamate di funzione per l'ambiente di testing di PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> In questa applicazione di esempio si siano aggiungendo semplicemente le credenziali in un file c# (. cs). Tuttavia, in una soluzione implementata, è consigliabile crittografare le credenziali in un file di configurazione.


La classe NVPAPICaller contiene la maggior parte delle funzionalità PayPal. Il codice nella classe fornisce i metodi necessari per effettuare una prova di acquisto dall'ambiente di testing di PayPal. Le funzioni di PayPal tre seguenti vengono utilizzate per effettuare acquisti:

- `SetExpressCheckout` (Funzione)
- `GetExpressCheckoutDetails` (Funzione)
- `DoExpressCheckoutPayment` (Funzione)

Il `ShortcutExpressCheckout` metodo raccoglie i dettagli di prodotto e informazioni di acquisto test dal carrello della spesa e chiama il `SetExpressCheckout` PayPal (funzione). Il `GetCheckoutDetails` metodo conferma i dettagli dell'acquisto e chiama il `GetExpressCheckoutDetails` PayPal funzione prima di effettuare l'acquisto di test. Il `DoCheckoutPayment` metodo viene completato l'acquisto di test dall'ambiente di testing chiamando il `DoExpressCheckoutPayment` PayPal (funzione). Il codice rimanente supporta i metodi di PayPal e processo, ad esempio codifica delle stringhe, la decodifica delle stringhe, matrici di elaborazione e determinare le credenziali.

> [!NOTE] 
> 
> PayPal consente di includere i dettagli dell'acquisto facoltativo basati sul [specifica dell'API di PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Estendendo il codice nell'applicazione di esempio Wingtip Toys, è possibile includere i dettagli di localizzazione, le descrizioni dei prodotti, tax, un numero di assistenza clienti, nonché molti altri campi facoltativi.


Si noti che gli URL di ritorno e cancel specificati nel **ShortcutExpressCheckout** metodo usare un numero di porta.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Quando Visual Web Developer viene eseguito un progetto web con SSL, in genere la porta 44300 viene utilizzata per il server web. Come illustrato in precedenza, il numero di porta è 44300. Quando si esegue l'applicazione, è possibile visualizzare un numero di porta diverso. Le esigenze di numeri di porta sia impostato correttamente nel codice in modo che sia possibile esito positivo eseguire l'applicazione di esempio Wingtip Toys alla fine di questa esercitazione. La sezione successiva di questa esercitazione illustra come recuperare il numero di porta dell'host locale e aggiornare la classe di PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aggiornare il numero di porta LocalHost nella classe PayPal

L'applicazione di esempio Wingtip Toys gli acquisti di prodotti, passare al sito di test di PayPal e restituzione all'istanza locale dell'applicazione di esempio Wingtip Toys. Per tornare all'URL corretto PayPal, è necessario specificare il numero di porta dell'esecuzione in locale nel codice di PayPal indicato in precedenza applicazione di esempio.

1. Fare clic sul nome del progetto (**WingtipToys**) nel **Esplora soluzioni** e selezionare **proprietà**.
2. Nella colonna sinistra, selezionare la **Web** scheda.
3. Recuperare il numero di porta dal **Url del progetto** casella.
4. Se necessario, aggiornare il `returnURL` e `cancelURL` nella classe PayPal (`NVPAPICaller`) nella *PayPalFunctions.cs* file da utilizzare il numero di porta dell'applicazione web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Il codice che è stato aggiunto verrà ora corrispondere alla porta previsto per l'applicazione Web locale. PayPal sarà in grado di restituire all'URL corretto nel computer locale.

### <a name="add-the-paypal-checkout-button"></a>Aggiungere il pulsante di estrazione di PayPal

Ora che le funzioni principali di PayPal aggiunti all'applicazione di esempio, è possibile iniziare ad aggiungere il markup e il codice necessario per chiamare queste funzioni. In primo luogo, è necessario aggiungere il pulsante di estrazione che l'utente visualizzerà la pagina carrello acquisti.

1. Aprire il *ShoppingCart.aspx* file.
2. Scorrere fino alla fine del file e trovare il `<!--Checkout Placeholder -->` commento.
3. Sostituire il commento con un `ImageButton` controllare in modo che il markup viene sostituito come indicato di seguito:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. Nel *ShoppingCart.aspx.cs* nel file, dopo il `UpdateBtn_Click` gestore evento verso la fine del file, aggiungere il `CheckOutBtn_Click` gestore dell'evento:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Anche nel *ShoppingCart.aspx.cs* , aggiungere un riferimento al `CheckoutBtn`, in modo che il nuovo pulsante di immagine viene fatto riferimento come indicato di seguito:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Salvare le modifiche apportate a entrambi i *ShoppingCart.aspx* file e il *ShoppingCart.aspx.cs* file.
7. Nel menu, selezionare **Debug**-&gt;**compilare WingtipToys**.  
   Il progetto verrà ricompilato con appena aggiunta **ImageButton** controllo.

### <a name="send-purchase-details-to-paypal"></a>Inviare i dettagli dell'acquisto di PayPal

Quando l'utente fa clic il **Checkout** pulsante nella pagina carrello acquisti (*ShoppingCart.aspx*), verrà iniziano il processo di acquisto. Il codice seguente chiama la prima funzione di PayPal necessaria per l'acquisto di prodotti.

1. Dal *Checkout* cartella, aprire il file code-behind denominato *CheckoutStart.aspx.cs*.   
   Assicurarsi di aprire il file code-behind.
2. Sostituire il codice esistente con quello seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Quando l'utente dell'applicazione sceglie la **Checkout** pulsante nella pagina carrello acquisti, il browser passerà al *CheckoutStart.aspx* pagina. Quando la *CheckoutStart.aspx* pagina caricamenti, i `ShortcutExpressCheckout` viene chiamato il metodo. A questo punto, l'utente viene trasferito nel sito web test PayPal. Nel sito PayPal, l'utente immette le proprie credenziali PayPal, esamina i dettagli dell'acquisto, accetta il contratto di PayPal e restituisce all'applicazione di esempio Wingtip Toys in cui il `ShortcutExpressCheckout` metodo viene completato. Quando la `ShortcutExpressCheckout` metodo è completato, si verrà reindirizzati all'utente il *CheckoutReview.aspx* specificata nella pagina il `ShortcutExpressCheckout` (metodo). Ciò consente all'utente esaminare i dettagli dell'ordine di all'interno dell'applicazione di esempio Wingtip Toys.

### <a name="review-order-details"></a>Esaminare i dettagli dell'ordine

Dopo la restituzione da PayPal, il *CheckoutReview.aspx* pagina dell'applicazione di esempio Wingtip Toys vengono visualizzati i dettagli dell'ordine. Questa pagina consente all'utente di esaminare i dettagli dell'ordine prima di acquistare i prodotti. Il *CheckoutReview.aspx* pagina deve essere creata come indicato di seguito:

1. Nel *Checkout* cartella, aprire la pagina denominata *CheckoutReview.aspx*.
2. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Aprire la pagina code-behind denominata *CheckoutReview.aspx.cs* e sostituire il codice esistente con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Il **DetailsView** controllo utilizzato per visualizzare i dettagli dell'ordine che sono stati restituiti da PayPal. Inoltre, il codice sopra riportato Salva i dettagli dell'ordine nel database di Wingtip Toys come un `OrderDetail` oggetto. Quando l'utente fa clic sui **Complete Order** pulsante, viene reindirizzato al *CheckoutComplete.aspx* pagina.

> [!NOTE] 
> 
> **Suggerimento**
> 
> Nel markup del *CheckoutReview.aspx* pagina, si noti che la `<ItemStyle>` tag viene utilizzato per modificare lo stile degli elementi all'interno di **DetailsView** controllo nella parte inferiore della pagina. Visualizzando la pagina nel **visualizzazione Progettazione** (selezionando **progettazione** nell'angolo inferiore sinistro di Visual Studio), quindi selezionando il **DetailsView** controllo e selezionando il  **Smart Tag** (sull'icona della freccia nella parte superiore destra del controllo), sarà possibile visualizzare il **DetailsView attività**.
> 
> ![Completamento della transazione e pagamento con PayPal - modifica campi](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Selezionando **modifica campi**, il **campi** verrà visualizzata la finestra di dialogo. Nella finestra di dialogo è possibile controllare facilmente le proprietà visive, ad esempio **ItemStyle**, delle **DetailsView** controllo.
> 
> ![Completamento della transazione e pagamento con PayPal - finestra di dialogo campi](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Completare l'acquisto

*CheckoutComplete.aspx* pagina effettua l'acquisto da PayPal. Come indicato in precedenza, l'utente deve fare clic sui **Complete Order** pulsante prima che l'applicazione verrà visualizzata la *CheckoutComplete.aspx* pagina.

1. Nel *Checkout* cartella, aprire la pagina denominata *CheckoutComplete.aspx*.
2. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Aprire la pagina code-behind denominata *CheckoutComplete.aspx.cs* e sostituire il codice esistente con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Quando la *CheckoutComplete.aspx* pagina viene caricata, il `DoCheckoutPayment` viene chiamato il metodo. Come accennato in precedenza, il `DoCheckoutPayment` metodo viene completato l'acquisto dall'ambiente di testing di PayPal. Una volta PayPal ha completato l'acquisto dell'ordine, il *CheckoutComplete.aspx* pagina viene visualizzata una transazione di pagamento `ID` all'acquirente.

### <a name="handle-cancel-purchase"></a>Handle di annullare l'acquisto

Se l'utente decide di annullare l'acquisto, l'utente viene indirizzato per il *CheckoutCancel.aspx* pagina in cui vedranno che l'ordine è stata annullata.

1. Aprire la pagina denominata *CheckoutCancel.aspx* nel *Checkout* cartella.
2. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Gestire gli errori di acquisto

Errori durante il processo di acquisto verranno gestiti dal *CheckoutError.aspx* pagina. Il code-behind del *CheckoutStart.aspx* pagina, il *CheckoutReview.aspx* pagina e il *CheckoutComplete.aspx* ciascuna pagina verrà reindirizzata al  *CheckoutError.aspx* pagina se si verifica un errore.

1. Aprire la pagina denominata *CheckoutError.aspx* nel *Checkout* cartella.
2. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

Il *CheckoutError.aspx* pagina viene visualizzata con i dettagli dell'errore quando si verifica un errore durante il processo di estrazione.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Eseguire l'applicazione per informazioni su come acquistare i prodotti. Si noti che verrà eseguita di PayPal ambiente di test. Nessun denaro effettivamente viene scambiata.

1. Assicurarsi che tutti i file vengono salvati in Visual Studio.
2. Aprire un Web browser e passare a [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Account di accesso con account per sviluppatore di PayPal creato in precedenza in questa esercitazione.  
   Per sandbox di sviluppo di PayPal, è necessario avere effettuato l'accesso al [ https://developer.paypal.com ](https://developer.paypal.com/) per testare l'estrazione rapida. Si applica solo a sandbox del PayPal test, non all'ambiente in tempo reale di PayPal.
4. In Visual Studio, premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
   Dopo che il database viene ricompilato, verrà aperto il browser e visualizzare il *default. aspx* pagina.
5. Aggiungere tre prodotti diversi al carrello acquisti selezionando la categoria di prodotto, ad esempio "Cars" e quindi facendo clic **Add to Cart** accanto a ogni prodotto.  
   Il carrello acquisti verrà visualizzato il prodotto che è stato selezionato.
6. Scegliere il **PayPal** pulsante da estrarre. 

    ![Completamento della transazione e pagamento con PayPal - carrello](checkout-and-payment-with-paypal/_static/image20.png)

   Estrazione richiederà di avere un account utente per l'applicazione di esempio Wingtip Toys.
7. Scegliere il **Google** collegamento a destra della pagina per accedere con un account di posta elettronica gmail.com esistente.  
   Se non hai un account gmail.com, è possibile crearne uno per il test per scopi di nella [www.gmail.com](https://www.gmail.com/). È anche possibile usare un account locale standard facendo clic su "Register". 

    ![Completamento della transazione e pagamento con PayPal - Accedi](checkout-and-payment-with-paypal/_static/image21.png)
8. Accedere con l'account gmail e la password. 

    ![Completamento della transazione e pagamento con PayPal - accesso Gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Scegliere il **Accedi** pulsante per registrare l'account gmail con il nome utente dell'applicazione di esempio Wingtip Toys. 

    ![Completamento della transazione e pagamento con PayPal - Account di registrazione](checkout-and-payment-with-paypal/_static/image23.png)
10. Nel sito di test di PayPal, aggiungere il **acquirente** password creata in precedenza in questa esercitazione e indirizzo di posta elettronica, quindi fare clic sulla **Accedi** pulsante. 

    ![Completamento della transazione e pagamento con PayPal - accesso PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Accettare i criteri di PayPal e fare clic sui **Accetto** pulsante.  
    Si noti che questa pagina è solo visualizzato la prima volta si usa questo account PayPal. Anche in questo caso si noti che questo è un account di test, nessun denaro reale vengono scambiati. 

    ![Completamento della transazione e pagamento con PayPal - criteri di PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Esaminare le informazioni dell'ordine di PayPal pagina revisione dell'ambiente e fare clic su test **continuazione**. 

    ![Completamento della transazione e pagamento con PayPal - informazioni sulla revisione](checkout-and-payment-with-paypal/_static/image26.png)
13. Nel *CheckoutReview.aspx* pagina, verificare l'importo dell'ordine e visualizzare l'indirizzo di spedizione generato. Scegliere il **Complete Order** pulsante. 

    ![Completamento della transazione e pagamento con PayPal - verifica ordine](checkout-and-payment-with-paypal/_static/image27.png)
14. Il **CheckoutComplete.aspx** viene visualizzata la pagina con un ID di transazione di pagamento. 

    ![Completamento della transazione e pagamento con PayPal - completamento della transazione completa](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Esaminare il Database

Esaminando i dati aggiornati nel database dell'applicazione di esempio Wingtip Toys dopo l'esecuzione dell'applicazione, è possibile vedere che l'applicazione è stata registrata l'acquisto dei prodotti.

È possibile esaminare i dati contenuti nel *Wingtiptoys.mdf* file di database tramite il **Esplora Database** finestra (**Esplora Server** finestra in Visual Studio) come è stato eseguito in precedenza in questa serie di esercitazioni.

1. Se è ancora aperto, chiudere la finestra del browser.
2. In Visual Studio, selezionare la **Mostra tutti i file** nella parte superiore del **Esplora soluzioni** che consente di espandere il **App\_dati** cartella.
3. Espandere la **App\_dati** cartella.  
 Potrebbe essere necessario selezionare la **Mostra tutti i file** icona della cartella.
4. Fare doppio clic il *Wingtiptoys.mdf* file di database, quindi scegliere **Open**.  
    **Esplora server** viene visualizzato.
5. Espandere la **tabelle** cartella.
6. Fare doppio clic il **ordini**tabelle e selezionare **Mostra dati tabella**.  
 Il **ordini** tabella viene visualizzata.
7. Rivedere le **PaymentTransactionID** colonne per confermare le transazioni ha esito positivo. 

    ![Completamento della transazione e pagamento con PayPal - Database di verifica](checkout-and-payment-with-paypal/_static/image29.png)
8. Chiudi il **ordini** finestra di tabella.
9. In Esplora Server, fare doppio clic il **OrderDetails** tabelle e selezionare **Mostra dati tabella**.
10. Esaminare i `OrderId` e `Username` i valori del **OrderDetails** tabella. Si noti che questi valori corrispondono al `OrderId` e `Username` valori incluso nella **ordini** tabella.
11. Chiudi il **OrderDetails** finestra di tabella.
12. Fare clic sul file di database Wingtip Toys (*Wingtiptoys.mdf*) e selezionare **3=chiusura connessione**.
13. Se non viene visualizzato il **Esplora soluzioni** finestra, fare clic su **Esplora soluzioni** nella parte inferiore della **Esplora Server** finestra per visualizzare il **Esplora soluzioni**  nuovamente.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato aggiunto order e order detail schemi per tenere traccia dell'acquisto di prodotti. Anche integrata una funzionalità di PayPal all'applicazione di esempio Wingtip Toys.

## <a name="additional-resources"></a>Risorse aggiuntive

[Cenni preliminari sulla configurazione di ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Distribuire un'App di moduli Web ASP.NET sicura con appartenenza, OAuth e Database SQL in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Questa esercitazione contiene codice di esempio. Tale codice di esempio viene fornito "com'è" senza garanzia di alcun tipo. Di conseguenza, Microsoft non garantisce l'accuratezza, integrità o qualità del codice di esempio. Si accetta di utilizzare il codice di esempio a proprio rischio. In nessun caso Microsoft sarà responsabile per l'utente in alcun modo per qualsiasi codice di esempio, contenuto, tra cui esemplificativo, errori o omissioni in qualsiasi codice di esempio, contenuto, o eventuali perdite o danni di qualsiasi tipo sostenuta come risultato l'uso di qualsiasi codice di esempio. Si viene inviata una notifica con il presente documento e Licenziatario accetta di indennizzare, salvare e manlevare Microsoft da e nei confronti di perdita di tutte, le attestazioni di perdita, ferite o danni di qualsiasi tipologia inclusi, a titolo esemplificativo, quelli risultanti dall'o derivanti da materiale in cui si registra, trasmettere, utilizzare o si basano su incluse, senza limitazioni, le opinioni espresse al suo interno.

> [!div class="step-by-step"]
> [Precedente](shopping-cart.md)
> [Successivo](membership-and-administration.md)
