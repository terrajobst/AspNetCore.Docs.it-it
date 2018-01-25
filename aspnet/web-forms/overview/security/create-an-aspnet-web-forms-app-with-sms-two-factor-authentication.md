---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Creare un Web ASP.NET form app con l'autenticazione a due fattori SMS (c#) | Documenti Microsoft
author: Erikre
description: "In questa esercitazione viene illustrato come compilare un'app Web Form ASP.NET con autenticazione a due fattori. In questa esercitazione è stata progettata per integrare l'esercitazione intitolata Cr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: b1f0ec0fdefa12eb7f7b2714dbc224fef735f4bb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Creare un Web ASP.NET form app con l'autenticazione a due fattori SMS (c#)
====================
Da [Erik Reitan](https://github.com/Erikre)

[Scaricare App di ASP.NET Web Form con messaggio di posta elettronica e autenticazione a due fattori tramite SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> In questa esercitazione viene illustrato come compilare un'app Web Form ASP.NET con autenticazione a due fattori. In questa esercitazione è stata progettata per integrare l'esercitazione intitolata [creare un'app Web Form ASP.NET protetta con la registrazione utente, conferma e reimpostazione della password di posta elettronica](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Inoltre, in questa esercitazione è stato in base di Rick Anderson [esercitazione MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Introduzione

In questa esercitazione modo semplificato i passaggi necessari per creare un'applicazione Web Form ASP.NET che supporta l'autenticazione a due fattori con Visual Studio. L'autenticazione a due fattori è un passaggio di autenticazione utente aggiuntivi. Questo passaggio aggiuntivo genera un numero di identificazione personale (PIN) durante l'accesso. Il PIN è in genere inviato all'utente come un messaggio di posta elettronica o messaggio SMS. Quando si accede, come misura di autenticazione extra l'utente dell'app quindi immette il PIN.

### <a name="tutorial-tasks-and-information"></a>Attività dell'esercitazione e le informazioni:

- [Creare un'App di ASP.NET Web Form](#createWebForms)
- [Installazione di SMS e l'autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori per utente registrato](#use2FA)
- [Risorse aggiuntive](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Creare un'App di ASP.NET Web Form

Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva anche. Inoltre, è necessario creare un [Twilio](https://www.twilio.com/try-twilio) account, come illustrato di seguito.

> [!NOTE]
> Importante: È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.


1. Creare un nuovo progetto (**File**  - &gt; **nuovo progetto**) e selezionare il **applicazione Web ASP.NET** modello insieme a .NET Framework versione 4.5.2 dal **nuovo progetto** la finestra di dialogo.
2. Dal **nuovo progetto ASP.NET** la finestra di dialogo, seleziona il **Web Form** modello. Lasciare l'autenticazione predefinita come **singoli account utente di**. Quindi, fare clic su **OK** per creare il nuovo progetto.  
    ![Finestra di dialogo Nuovo progetto ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Abilitare Secure Sockets Layer (SSL) per il progetto. Seguire le istruzioni disponibili nel **abilita SSL per il progetto** sezione la [Introduzione a una serie di esercitazioni di Web Form](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. In Visual Studio, aprire il **Package Manager Console** (**strumenti**  - &gt; **Gestione pacchetti NuGet**  - &gt; **Package Manager Console**), quindi immettere il comando seguente:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Installazione di SMS e l'autenticazione a due fattori

In questa esercitazione Usa Twilio, ma è possibile utilizzare qualsiasi provider SMS.

1. Creare un [Twilio](https://www.twilio.com/try-twilio) account.
2. Dal **Dashboard** scheda dell'account di Twilio, copia il **SID di Account** e **Token di autenticazione.** Si aggiungerà li all'App in un secondo momento.
3. Dal **numeri** scheda, copiare il Twilio **il numero di telefono** anche.
4. Assicurarsi di Twilio **SID di Account**, **Token di autorizzazione** e **il numero di telefono** disponibili per l'app. Per semplificare le operazioni verranno archiviati i valori nel *Web. config* file. Quando si distribuiscono in Azure, è possibile archiviare i valori in modo sicuro nel **appSettings** scheda Configura sezione sul sito web. Inoltre, quando si aggiunge il numero di telefono, utilizzare solo numeri.   
 Si noti che è anche possibile aggiungere le credenziali SendGrid. SendGrid è un servizio di notifica di posta elettronica. Per informazioni dettagliate su come abilitare SendGrid, vedere la sezione backup Hook SendGrid dell'esercitazione intitolata [creare un'App di ASP.NET Web Forms sicura con la registrazione utente, inviare tramite posta elettronica di conferma e reimpostazione della password.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Sicurezza - archivio mai i dati sensibili nel codice sorgente. In questo esempio, l'account e le credenziali vengono archiviate nel **appSettings** sezione la *Web. config* file. In Azure, è possibile archiviare questi valori in modo sicuro nel  **[configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  scheda nel portale di Azure. Per informazioni correlate, vedere l'argomento di Rick Anderson [procedure consigliate per la distribuzione delle password e altri dati sensibili su ASP.NET e in Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Configurare il `SmsService` classe il *App\_Start\IdentityConfig.cs* evidenziata in giallo modifiche al file effettuando le operazioni seguenti: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Aggiungere il seguente `using` istruzioni all'inizio del *IdentityConfig.cs* file: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Aggiornamento di *Account/Manage.aspx* file rimuovendo le righe evidenziate in giallo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. Nel `Page_Load` gestore del *Manage.aspx.cs* codice, rimuovere il commento la riga di codice evidenziata in giallo, in modo che venga visualizzato come segue: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Nel codebehind della *Account*/*TwoFactorAuthenticationSignIn.aspx.cs*, aggiornare il `Page_Load` gestore aggiungendo il seguente codice evidenziato in giallo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

 Rendendo il codice sopra riportato modificare DropDownList "Provider" che contiene le opzioni di autenticazione non ripartirà al primo valore. In questo modo all'utente di selezionare correttamente tutte le opzioni da utilizzare per l'autenticazione, non solo il primo.
10. In **Esplora**, fare doppio clic su *Default.aspx* e selezionare **imposta come pagina iniziale**.
11. Per testare l'app, innanzitutto compilare l'app (**Ctrl**+**MAIUSC**+**B**) e quindi eseguire l'app (**F5**) e Selezionare **registrare** per creare un nuovo account utente o selezionare **Accedi** se l'account utente è già stato registrato.
12. Una volta (come l'utente) hanno effettuato l'accesso, fare clic sull'ID utente (indirizzo di posta elettronica) nella barra di navigazione per visualizzare il **Gestisci Account** pagina (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Fare clic su **Aggiungi** accanto a **numero di telefono** sul **Gestisci Account** pagina.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Aggiungere il numero di telefono in cui si (come l'utente) desidera ricevere messaggi SMS (SMS) e fare clic su di **Invia** pulsante.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
 A questo punto, l'app userà le credenziali di *Web. config* contattare Twilio. Verrà inviato un messaggio SMS (SMS) per il telefono associato all'account utente. È possibile verificare che è stato inviato il messaggio di Twilio visualizzando il dashboard Twilio.
15. In pochi secondi, il telefono associato all'account utente verrà visualizzato un messaggio di testo contenente il codice di verifica. Immettere il codice di verifica e premere **Invia**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Abilitare l'autenticazione a due fattori per un utente registrato

A questo punto, è stata abilitata l'autenticazione a due fattori per l'app. Per un utente di utilizzare l'autenticazione a due fattori, può modificare le impostazioni tramite l'interfaccia utente. 

1. Come utente dell'app è possibile abilitare l'autenticazione a due fattori per l'account specifico facendo clic sull'ID utente (alias di posta elettronica) nella barra di navigazione per visualizzare il **Gestisci Account** pagina. Quindi, fare clic su di **abilitare** collegamento per abilitare l'autenticazione a due fattori per l'account.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Disconnettersi e quindi eseguire nuovamente l'accesso. Se è stato abilitato posta elettronica, è possibile selezionare SMS o posta elettronica per l'autenticazione a due fattori. Se è stata abilitata tramite posta elettronica, vedere l'esercitazione intitolata [creare un'App di ASP.NET Web Forms sicura con la registrazione utente, conferma tramite posta elettronica e la reimpostazione della Password](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Verrà visualizzata la pagina di autenticazione a due fattori in cui è possibile immettere il codice (da posta elettronica o SMS).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di utilizzare l'autenticazione a due fattori per l'accesso quando si utilizza il browser e dispositivi in cui è stata selezionata la casella. Fino a quando gli utenti malintenzionati non possono accedere al dispositivo, abilitare l'autenticazione a due fattori e facendo clic su di **ricordare questo browser** fornirà accedere facilmente la password di un unico passaggio, conservando al tempo strong protezione per l'autenticazione a due fattori per tutti gli accessi da dispositivi non trusted. Tale operazione può essere eseguita su qualsiasi dispositivo privata che vengono utilizzate regolarmente.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Collegamenti a ASP.NET Identity consigliato risorse](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Distribuire un'App di form Web ASP.NET in modo sicuro con l'appartenenza, OAuth e il Database SQL a un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie di esercitazioni Web Form ASP.NET - aggiungere un Provider di OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Forms serie di esercitazioni - abilita SSL per il progetto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [La conferma dell'account e Password di ripristino con identità di ASP.NET](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Creazione di app in Facebook e l'applicazione di connessione al progetto](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
