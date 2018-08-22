---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Creare un'app Web Form ASP.NET sicura con registrazione utente, inviare tramite posta elettronica di conferma e reimpostazione della password (c#) | Microsoft Docs
author: Erikre
description: Questa esercitazione illustra come compilare un'app Web Form ASP.NET con la registrazione utente, conferma tramite posta elettronica e reimpostazione della password usando il membro di ASP.NET Identity...
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: b964257165f8a50bef5cb5d8cee4fb30a5e91cf0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828324"
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Creare un'app Web Form ASP.NET sicura con registrazione utente, inviare tramite posta elettronica di conferma e reimpostazione della password (c#)
====================
da [Erik Reitan](https://github.com/Erikre)

> Questa esercitazione illustra come compilare un'app Web Form ASP.NET con la registrazione utente, conferma tramite posta elettronica e reimpostazione della password usando il sistema di appartenenze ASP.NET Identity. Questa esercitazione si basava su di Rick Anderson [esercitazione su MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Introduzione

Questa esercitazione descrive i passaggi necessari per creare un'applicazione Web Form ASP.NET con Visual Studio e ASP.NET 4.5 per creare un'app Web Forms sicura con la registrazione utente, inviare tramite posta elettronica di conferma e reimpostazione della password.

### <a name="tutorial-tasks-and-information"></a>Informazioni e attività dell'esercitazione:

- [Creare un ASP.NET Web Forms app](#createWebForms)
- [Associare SendGrid](#SG)
- [Richiedere conferma tramite posta elettronica prima dell'accesso In](#require)
- [Reimpostazione e il recupero della password](#reset)
- [Inviare di nuovo collegamento di conferma tramite posta elettronica](#rsend)
- [Risoluzione dei problemi relativi all'App](#dbg)
- [Risorse aggiuntive](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Creare un'applicazione Web Form ASP.NET

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versioni successive nonché.

> [!NOTE]
> Avviso: È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.


1. Creare un nuovo progetto (**File**  - &gt; **nuovo progetto**) e selezionare il **applicazione Web ASP.NET** modello e la versione più recente di .NET Framework versione di **nuovo progetto** nella finestra di dialogo.
2. Dal **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **Web Form** modello. Lasciare l'autenticazione predefinita come **singoli account utente di**. Se vuoi ospitare l'app in Azure, lasciare il **ospita nel cloud** la casella di controllo.   
 Fare quindi clic **OK** per creare un nuovo progetto.  
    ![Finestra di dialogo Nuovo progetto ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Abilita Secure Sockets Layer (SSL) per il progetto. Seguire la procedura disponibile nel **abilita SSL per il progetto** sezione il [Introduzione a serie di esercitazioni in Web Form](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Eseguire l'app, scegliere il **registrare** collegare e registrare un nuovo utente. A questo punto, l'unica convalida il messaggio di posta elettronica si basa sul [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo per assicurarsi che l'indirizzo di posta elettronica sia ben formato. Si modificherà il codice per aggiungere conferma tramite posta elettronica. Chiudere la finestra del browser.
5. Nelle **Esplora Server** di Visual Studio (**View**  - &gt; **Esplora Server**), passare a **Connections\ dati DefaultConnection\Tables\AspNetUsers**, fare clic e selezionare **Apri definizione tabella**. 

    La figura seguente mostra il `AspNetUsers` lo schema di tabella:

    ![Schema della tabella AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. Nelle **Esplora Server**, fare clic sui **AspNetUsers** tabella e selezionare **Mostra dati tabella**.  
  
    ![Dati della tabella AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 A questo punto non è stato confermato l'indirizzo di posta elettronica per l'utente registrato.
7. Fare clic sulla riga e selezionare Elimina per eliminare l'utente. Si sarà aggiungere di nuovo questo messaggio di posta elettronica nel passaggio successivo e inviare un messaggio di conferma all'indirizzo di posta elettronica.

## <a name="email-confirmation"></a>Conferma tramite posta elettronica

È consigliabile verificare che il messaggio di posta elettronica durante la registrazione di un nuovo utente per verificare che non rappresentano un altro utente (vale a dire non hanno registrato con un altro messaggio di posta elettronica). Si supponga di un forum di discussione, si desidera impedire `"bob@cpandl.com"` tramite la registrazione come `"joe@contoso.com"`. Senza conferma tramite posta elettronica, `"joe@contoso.com"` è stato possibile ottenere l'indirizzo di posta elettronica indesiderato dalla propria app. Si supponga che Bob accidentalmente registrato come `"bib@cpandl.com"` e non l'aveste notato, egli non sarebbe in grado di utilizzare il recupero della password perché l'app non ha il suo indirizzo di posta elettronica corretto. Conferma tramite posta elettronica fornisce solo una protezione limitata dai Bot e non fornisce protezione da spammer determinato.

In genere si desidera impedire ai nuovi utenti dalla registrazione di tutti i dati al sito Web prima che sono state confermate tramite entrambi posta elettronica, un SMS o un altro meccanismo. Nelle sezioni seguenti, verrà Abilita conferma tramite posta elettronica e modificare il codice per impedire l'accesso fino a quando non è stata confermata la posta elettronica gli utenti appena registrati. In questa esercitazione, si userà il servizio e-mail SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Associare SendGrid

Sebbene in questa esercitazione illustra solo come aggiungere notifica tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi (vedere [risorse aggiuntive](#addRes)).

1. In Visual Studio, aprire il **Console di gestione pacchetti** (**Tools**  - &gt; **Gestione pacchetti NuGet**  - &gt; **Console di gestione pacchetti**), quindi immettere il comando seguente:  
    `Install-Package SendGrid`
2. Andare alla [pagina di iscrizione a Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) e registrazione gratuita account SendGrid. È possibile anche iscriversi per un account di SendGrid gratuito direttamente sulla [sito di SendGrid](http://www.sendgrid.com).
3. Dal **Esplora soluzioni** aprire il *IdentityConfig.cs* del file nei *App\_avviare* cartella e aggiungere il seguente codice evidenziato in giallo per il `EmailService` classe per configurare **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Inoltre, aggiungere quanto segue `using` istruzioni all'inizio della *IdentityConfig.cs* file: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Per semplificare questo esempio, si archivierà i valori di account di servizi di posta elettronica nel `appSettings` sezione il *Web. config* file. Aggiungere il codice XML seguente evidenziati in giallo per il *Web. config* file nella radice del progetto:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Security - store mai i dati sensibili nel codice sorgente. In questo esempio, l'account e le credenziali vengono archiviate nel **appSetting** sezione il *Web. config* file. In Azure, è possibile archiviare in modo sicuro questi valori sul **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** scheda nel portale di Azure. Per informazioni correlate vedere l'argomento di Rick Anderson [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Aggiungere i valori di servizio di posta elettronica in modo da riflettere che i valori di autenticazione SendGrid (nome utente e Password) in modo da poter riuscito inviare posta elettronica dall'app. Assicurarsi di usare il nome dell'account SendGrid anziché l'indirizzo di posta elettronica è fornito di SendGrid.

### <a name="enable-email-confirmation"></a>Abilitare la conferma tramite posta elettronica

 Per abilitare la conferma tramite posta elettronica, si modificherà il codice di registrazione usando la procedura seguente.  
 

1. Nel *Account* cartella, aprire il *Register.aspx.cs* code-behind e aggiornare il `CreateUser_Click` metodo per consentire le modifiche evidenziate di seguito: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. Nelle **Esplora soluzioni**, fare doppio clic su *default. aspx* e selezionare **imposta come pagina iniziale**.
3. Eseguire l'app premendo **F5.** Dopo la pagina viene visualizzata, scegliere il **registrare** link per visualizzare la pagina di registrazione.
4. Immettere l'indirizzo di posta elettronica e la password, quindi scegliere il **registrare** pulsante per inviare un messaggio di posta elettronica tramite SendGrid.  
   Lo stato corrente del progetto e codice consentirà all'utente di accesso dopo il completamento nel modulo di registrazione, anche se essi non hanno confermato il proprio account.
5. Verificare l'account di posta elettronica e fare clic sul collegamento per confermare l'indirizzo di posta elettronica.  
   Dopo aver inviato il modulo di registrazione, verrà eseguito.  
    ![Sito Web di esempio - connesso](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Richiedere conferma tramite posta elettronica prima dell'accesso In

Anche se hai confermato l'account di posta elettronica, a questo punto sarebbe non devi fare clic sul collegamento contenuto nel messaggio di verifica firmato in completamente. Nella sezione seguente si modificherà il codice che richiedono nuovi utenti per disporre di un messaggio di posta elettronica confermato prima vengono registrate nel (autenticato).

1. In **Esplora soluzioni** di Visual Studio, aggiornare il `CreateUser_Click` evento nel *Register.aspx.cs* contenuti in code-behind la *account* cartella con il modifiche evidenziate di seguito: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Aggiorna il `LogIn` metodo nella *Login.aspx.cs* code-behind con le modifiche evidenziate di seguito: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Esecuzione dell'applicazione

 Ora che è stato implementato il codice per verificare se è stato confermato l'indirizzo di posta elettronica dell'utente, è possibile controllare le funzionalità in entrambi i **registrare** e **Login** pagine. 

1. Eliminare gli account nel **AspNetUsers** tabella che contiene l'alias di posta elettronica che si desidera verificare.
2. Eseguire l'app (**F5**) e verificare fino a quando non si è verificato l'indirizzo di posta elettronica non è possibile registrare con un account utente.
3. Prima di confermare il nuovo account tramite l'indirizzo di posta elettronica che è appena stata inviata, tentare di accedere con il nuovo account.  
 Si noterà che non si riesce ad accedere e che sono necessari un account di posta elettronica confermato.
4. Dopo aver verificato l'indirizzo di posta elettronica, accedere all'app.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Reimpostazione e il recupero della password

1. In Visual Studio, rimuovere i caratteri di commento dal `Forgot` metodo nella *Forgot.aspx.cs* contenuta nel code-behind il *Account* cartella, in modo che il metodo appare come segue: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Aprire il *aspx* pagina. Sostituire il markup delle fasi conclusive il **loginForm** sezione come evidenziato di seguito: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Aprire il *Login.aspx.cs* code-behind e rimuovere il commento la riga seguente di codice evidenziata in giallo dal `Page_Load` gestore dell'evento: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Eseguire l'app premendo **F5.** Dopo la pagina viene visualizzata, fare clic sui **Accedi** collegamento.
5. Fare clic sui **password dimenticata?** link per visualizzare i **Password dimenticata** pagina.
6. Immettere l'indirizzo di posta elettronica, quindi scegliere il **Submit** pulsante per inviare un messaggio di posta elettronica all'indirizzo che consentirà di reimpostare la password.   
   Verificare l'account di posta elettronica e fare clic sul collegamento per visualizzare il **Reimposta Password** pagina.
7. Nel **Reimposta Password** pagina, immettere l'indirizzo di posta elettronica, password e password di conferma. Premere quindi il **reimpostare** pulsante.  
   Quando si ha reimpostato correttamente la password, il **modificare la Password** verrà visualizzata la pagina. A questo punto è possibile accedere con la nuova password.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Inviare di nuovo collegamento di conferma tramite posta elettronica

Una volta che un utente crea un nuovo account locale, essi vengono inviati tramite posta elettronica un collegamento di conferma che viene richiesto di usare prima di poter accedere. Se l'utente accidentalmente Elimina il messaggio di posta elettronica di conferma o non arrivi mai il messaggio di posta elettronica, è necessario il collegamento di conferma inviato nuovamente. Le modifiche al codice seguente viene illustrato come abilitare questa opzione.

1. In Visual Studio, aprire il **Login.aspx.cs** code-behind e aggiungere il gestore eventi seguente dopo il `LogIn` gestore dell'evento:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modificare il `LogIn` gestore dell'evento nel *Login.aspx.cs* code-behind modificando il codice evidenziato in giallo, come indicato di seguito: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Aggiorna il *aspx* pagina aggiungendo il codice evidenziato in giallo, come indicato di seguito: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Eliminare gli account nel **AspNetUsers** tabella che contiene l'alias di posta elettronica che si desidera verificare.
5. Eseguire l'app (**F5**) e registrare l'indirizzo di posta elettronica.
6. Prima di confermare il nuovo account tramite l'indirizzo di posta elettronica che è appena stata inviata, tentare di accedere con il nuovo account.  
   Si noterà che non si riesce ad accedere e che sono necessari un account di posta elettronica confermato. Inoltre, è ora possibile rinviare un messaggio di conferma al proprio account di posta elettronica.
7. Immettere l'indirizzo di posta elettronica e password, quindi premere il **invia di nuovo conferma** pulsante.
8. Dopo aver verificato l'indirizzo di posta elettronica in base al messaggio di posta elettronica appena inviati, accedere all'app.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Risoluzione dei problemi relativi all'App

Se non si riceve un messaggio di posta elettronica che contiene il collegamento per verificare le credenziali:

- Controllare la cartella della posta indesiderata o antispam.
- Accedere all'account SendGrid e fare clic sui [collegamento di attività di posta elettronica](https://sendgrid.com/logs/index).
- Essere usato il nome di account utente di SendGrid come un *Web. config* valore anziché l'indirizzo e-mail dell'account SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Risorse consigliate su collegamenti ad ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Conferma account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Serie di esercitazioni Web Form ASP.NET - aggiungere un Provider di OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Distribuire un'App di moduli Web ASP.NET sicura con appartenenza, OAuth e Database SQL in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms serie di esercitazioni - abilitare SSL per il progetto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
