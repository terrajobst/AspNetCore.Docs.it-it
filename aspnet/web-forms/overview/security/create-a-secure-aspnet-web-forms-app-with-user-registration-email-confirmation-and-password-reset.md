---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Creare un'app Web Form ASP.NET protetta con la registrazione utente, inviare tramite posta elettronica di conferma e reimpostazione della password (c#) | Documenti Microsoft
author: Erikre
description: "In questa esercitazione viene illustrato come compilare un'app Web Form ASP.NET con la registrazione utente, conferma tramite posta elettronica e password reimpostata utilizzando il membro di identità di ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: b6f3821a8022daa26f5efcc009ab3e6283a76a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Creare un'app Web Form ASP.NET protetta con la registrazione utente, inviare tramite posta elettronica di conferma e reimpostazione della password (c#)
====================
Da [Erik Reitan](https://github.com/Erikre)

> In questa esercitazione viene illustrato come compilare un'app Web Form ASP.NET con la registrazione utente, conferma tramite posta elettronica e password reimpostata utilizzando il sistema di appartenenze di ASP.NET Identity. In questa esercitazione è basata sul di Rick Anderson [esercitazione MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Introduzione

In questa esercitazione modo semplificato i passaggi necessari per creare un'applicazione Web Form ASP.NET utilizzando Visual Studio e ASP.NET 4.5 per creare un'app Web Form protetta con la registrazione utente, inviare tramite posta elettronica di conferma e reimpostazione della password.

### <a name="tutorial-tasks-and-information"></a>Attività dell'esercitazione e le informazioni:

- [Creare un Web ASP.NET form app](#createWebForms)
- [Collegare SendGrid](#SG)
- [Richiedi conferma tramite posta elettronica prima dell'accesso In](#require)
- [Reimpostazione e il recupero della password](#reset)
- [Inviare di nuovo il collegamento di conferma tramite posta elettronica](#rsend)
- [Risoluzione dei problemi relativi all'App](#dbg)
- [Risorse aggiuntive](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Creare un'App di ASP.NET Web Form

Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva anche.

> [!NOTE]
> Avviso: È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.


1. Creare un nuovo progetto (**File**  - &gt; **nuovo progetto**) e selezionare il **applicazione Web ASP.NET** modello e la versione più recente di .NET Framework versione dal **nuovo progetto** la finestra di dialogo.
2. Dal **nuovo progetto ASP.NET** la finestra di dialogo, seleziona il **Web Form** modello. Lasciare l'autenticazione predefinita come **singoli account utente di**. Se si desidera ospitare l'applicazione in Azure, lasciare il **Host nel cloud** la casella di controllo.   
 Quindi, fare clic su **OK** per creare il nuovo progetto.  
    ![Finestra di dialogo Nuovo progetto ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Abilitare Secure Sockets Layer (SSL) per il progetto. Seguire le istruzioni disponibili nel **abilita SSL per il progetto** sezione la [Introduzione a una serie di esercitazioni di Web Form](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Eseguire l'app, fare clic su di **registrare** collegare e registrare un nuovo utente. A questo punto, la convalida sola nel messaggio di posta elettronica è basata sul [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo per verificare l'indirizzo di posta elettronica sia corretto. Si modificherà il codice per aggiungere una conferma tramite posta elettronica. Chiudere la finestra del browser.
5. In **Esplora Server** di Visual Studio (**vista**  - &gt; **Esplora Server**), passare a **Connections\ dati DefaultConnection\Tables\AspNetUsers**fare clic destro del mouse e selezionare **Apri definizione tabella**. 

    La figura seguente mostra il `AspNetUsers` schema della tabella:

    ![Schema della tabella AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. In **Esplora Server**, fare clic su di **AspNetUsers** tabella e selezionare **Mostra dati tabella**.  
  
    ![Dati della tabella AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 A questo punto non è stata confermata la posta elettronica per l'utente registrato.
7. Fare clic sulla riga e selezionare Elimina per eliminare l'utente. Si sarà aggiungere di nuovo questo messaggio di posta elettronica nel passaggio successivo e inviare un messaggio di conferma per l'indirizzo di posta elettronica.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

È consigliabile verificare il messaggio di posta elettronica durante la registrazione di un nuovo utente per verificare che non rappresentano un altro utente (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica). Si supponga di un forum di discussione, si desidera impedire `"bob@cpandl.com"` tramite la registrazione come `"joe@contoso.com"`. Senza conferma tramite posta elettronica, `"joe@contoso.com"` Impossibile ricevere la posta elettronica indesiderato dall'app. Si supponga che Bob accidentalmente registrato come `"bib@cpandl.com"` e non veniva notare, ha non sarebbe in grado di utilizzare il recupero della password, perché l'app non ha il suo messaggio di posta elettronica corretto. Conferma tramite posta elettronica fornisce a livello di protezione limitato da BOT e non offre protezione da spamming determinato.

In genere si desidera impedire la registrazione di tutti i dati per il sito Web prima di avere confermate da un messaggio di posta elettronica, un messaggio SMS o un altro meccanismo di nuovi utenti. Nelle sezioni riportate di seguito, si consentirà conferma tramite posta elettronica e modificare il codice per impedire l'accesso fino a quando non è stato confermato l'accesso alla posta elettronica di utenti appena registrati. In questa esercitazione, si userà il servizio di posta elettronica SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Collegare SendGrid

Sebbene questa esercitazione viene illustrato solo come aggiungere una notifica tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi (vedere [risorse aggiuntive](#addRes)).

1. In Visual Studio, aprire il **Package Manager Console** (**strumenti**  - &gt; **Gestione pacchetti NuGet**  - &gt; **Package Manager Console**), quindi immettere il comando seguente:  
    `Install-Package SendGrid`
2. Passare al [pagina di iscrizione Azure SendGrid](https://azure.microsoft.com/en-us/gallery/store/sendgrid/sendgrid-azure/) e registrare gratuitamente account SendGrid. È possibile anche registrarsi a un account gratuito di SendGrid direttamente su [sito del SendGrid](http://www.sendgrid.com).
3. Da **Esplora** aprire il *IdentityConfig.cs* file nel *App\_avviare* cartella e aggiungere il seguente codice evidenziato in giallo per il `EmailService` classe per la configurazione **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Inoltre, aggiungere le seguenti `using` istruzioni all'inizio del *IdentityConfig.cs* file: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Per semplificare questo esempio, archiviare i valori di account del servizio di posta elettronica nel `appSettings` sezione la *Web. config* file. Aggiungere il seguente codice XML evidenziati in giallo per il *Web. config* file alla radice del progetto:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Sicurezza - archivio mai i dati sensibili nel codice sorgente. In questo esempio, l'account e le credenziali vengono archiviate nel **appSetting** sezione la *Web. config* file. In Azure, è possibile archiviare questi valori in modo sicuro nel  **[configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  scheda nel portale di Azure. Per ulteriori informazioni vedere l'argomento di Rick Anderson [procedure consigliate per la distribuzione delle password e altri dati sensibili su ASP.NET e in Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Aggiungere i valori di servizio di posta elettronica in modo da riflettere i valori di autenticazione SendGrid (nome utente e Password) in modo che sia possibile corretta inviare posta elettronica dalla tua app. Assicurarsi di utilizzare il nome di account di SendGrid, anziché l'indirizzo di posta elettronica è fornito SendGrid.

### <a name="enable-email-confirmation"></a>Abilitare la conferma tramite posta elettronica

 Per abilitare la conferma tramite posta elettronica, si verrà modificato il codice di registrazione usando la procedura seguente.  
 

1. Nel *Account* cartella, aprire il *Register.aspx.cs* code-behind e aggiornare il `CreateUser_Click` metodo per abilitare le seguenti modifiche evidenziate: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. In **Esplora**, fare doppio clic su *Default.aspx* e selezionare **imposta come pagina iniziale**.
3. Eseguire l'applicazione premendo **F5.** Dopo la pagina viene visualizzata, fare clic su di **registrare** link per visualizzare la pagina di registrazione.
4. Immettere il messaggio di posta elettronica e la password, quindi scegliere il **registrare** pulsante per inviare un messaggio di posta elettronica tramite SendGrid.  
 Lo stato corrente del progetto e codice consentirà all'utente di accedere dopo il completamento del modulo di registrazione, anche se essi non sono stati confermato il proprio account.
5. Verificare l'account di posta elettronica e fare clic sul collegamento per confermare la posta elettronica.  
 Dopo aver inviato il modulo di registrazione, verrà registrato.  
    ![Sito Web di esempio - connesso](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Richiedi conferma tramite posta elettronica prima dell'accesso In

Anche se hai confermato l'account di posta elettronica, a questo punto non si deve fare clic sul collegamento nel messaggio di posta elettronica verifica completamente firmato in. Nella sezione seguente, si modificherà il codice che richiede di nuovi utenti per disporre di un messaggio di posta elettronica di conferma vengono registrate nel (autenticato).

1. In **Esplora soluzioni** di Visual Studio, è possibile aggiornare il `CreateUser_Click` evento nel *Register.aspx.cs* contenute nel codice il *account* cartella con il modifiche evidenziate seguente: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Aggiornamento di `LogIn` metodo il *Login.aspx.cs* codice con le seguenti modifiche evidenziate: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Esecuzione dell'applicazione

 Ora che è stato implementato il codice per verificare se è stato confermato l'indirizzo di posta elettronica dell'utente, è possibile controllare la funzionalità sia il **registrare** e **accesso** pagine. 

1. Eliminare gli account nel **AspNetUsers** la tabella contenente l'alias di posta elettronica che si desidera verificare.
2. Eseguire l'app (**F5**) e verificare fino a quando non è confermato l'indirizzo di posta elettronica non è possibile registrare un utente.
3. Prima di confermare il nuovo account tramite la posta elettronica che è appena stata inviata, tentare di accedere con il nuovo account.  
 Si noterà che non si riesce ad accedere e che è necessario disporre di un account di posta elettronica di conferma.
4. Dopo aver confermato l'indirizzo di posta elettronica, accedere all'app.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Reimpostazione e il recupero della password

1. In Visual Studio, rimuovere i caratteri di commento dal `Forgot` metodo il *Forgot.aspx.cs* contenute nel codice il *Account* cartella, in modo che il metodo viene visualizzato come segue: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Aprire il *Login.aspx* pagina. Sostituire il markup verso la fine del **loginForm** sezione come evidenziato qui sotto: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Aprire il *Login.aspx.cs* code-behind e rimuovere il commento la riga seguente di codice evidenziata in giallo dal `Page_Load` gestore eventi: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Eseguire l'applicazione premendo **F5.** Dopo la pagina viene visualizzata, fare clic su di **Accedi** collegamento.
5. Fare clic su di **password dimenticata?** link per visualizzare il **Password dimenticata** pagina.
6. Immettere l'indirizzo di posta elettronica e fare clic su di **Invia** pulsante per inviare un messaggio di posta elettronica all'indirizzo che consentirà di reimpostare la password.   
 Verificare l'account di posta elettronica e fare clic sul collegamento per visualizzare il **Reimposta Password** pagina.
7. Nel **Reimposta Password** pagina, immettere il messaggio di posta elettronica, password e password di conferma. Premere quindi il **reimpostare** pulsante.  
 Quando è stata reimpostata la password, il **Password modificata** verrà visualizzata la pagina. È ora possibile registrare con la nuova password.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Inviare di nuovo il collegamento di conferma tramite posta elettronica

Una volta che un utente crea un nuovo account locale, essi vengono inviati tramite posta elettronica un collegamento di conferma che devono utilizzare prima di poter accedere. Se l'utente accidentalmente Elimina il messaggio di posta elettronica di conferma o messaggio di posta elettronica non arriva mai, sarà necessario il collegamento di conferma inviato nuovamente. Le modifiche al codice seguente viene illustrato come abilitare questa opzione.

1. In Visual Studio, aprire il **Login.aspx.cs** code-behind e aggiungere il seguente gestore eventi dopo il `LogIn` gestore eventi:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modificare il `LogIn` nel gestore dell'evento di *Login.aspx.cs* codice modificando il codice evidenziato in giallo, come indicato di seguito: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Aggiornamento di *Login.aspx* pagina aggiungendo il codice evidenziato in giallo, come indicato di seguito: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Eliminare gli account nel **AspNetUsers** la tabella contenente l'alias di posta elettronica che si desidera verificare.
5. Eseguire l'app (**F5**) e registrare l'indirizzo di posta elettronica.
6. Prima di confermare il nuovo account tramite la posta elettronica che è appena stata inviata, tentare di accedere con il nuovo account.  
 Si noterà che non si riesce ad accedere e che è necessario disporre di un account di posta elettronica di conferma. Inoltre, è ora possibile rinviare un messaggio di conferma al proprio account di posta elettronica.
7. Immettere l'indirizzo di posta elettronica e una password, quindi premere il **inviare di nuovo conferma** pulsante.
8. Dopo aver confermato l'indirizzo di posta elettronica in base al messaggio di posta elettronica appena inviato, accedere all'app.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Risoluzione dei problemi relativi all'App

Se non si riceve un messaggio di posta elettronica contenente il collegamento per verificare le credenziali:

- Controllare la cartella della posta indesiderata o posta indesiderata.
- Accedere al proprio account di SendGrid e fare clic su di [collegamento dell'attività di posta elettronica](https://sendgrid.com/logs/index).
- Essere utilizzato il nome dell'account utente SendGrid come un *Web. config* valore anziché l'indirizzo di posta elettronica di account di SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Collegamenti a ASP.NET Identity consigliato risorse](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [La conferma dell'account e Password di ripristino con identità di ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Serie di esercitazioni Web Form ASP.NET - aggiungere un Provider di OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Distribuire un'App di moduli Web ASP.NET in modo sicuro con appartenenza, OAuth e Database SQL di servizio App di Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms serie di esercitazioni - abilita SSL per il progetto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
