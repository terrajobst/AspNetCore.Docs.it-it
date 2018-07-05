---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Creare un ASP.NET Web Forms app con l'autenticazione a due fattori SMS (c#) | Microsoft Docs
author: Erikre
description: Questa esercitazione illustra come compilare un'app Web Form ASP.NET con autenticazione a due fattori. Questa esercitazione è stata progettata per integrare l'esercitazione intitolata Cr...
ms.author: aspnetcontent
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 16045b116ca5c797e7840f2ee5944e5f2c6282eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803549"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Creare un ASP.NET Web Forms app con l'autenticazione a due fattori SMS (c#)
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare l'applicazione Web Form ASP.NET con indirizzo di posta elettronica e l'autenticazione a due fattori tramite SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Questa esercitazione illustra come compilare un'app Web Form ASP.NET con autenticazione a due fattori. Questa esercitazione è stata progettata per integrare l'esercitazione intitolata [creare un'app Web Form ASP.NET sicura con registrazione utente, posta elettronica di conferma e reimpostazione della password](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Inoltre, questa esercitazione è stata basata sul di Rick Anderson [esercitazione su MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Introduzione

Questa esercitazione descrive i passaggi necessari per creare un'applicazione Web Form ASP.NET che supporta l'autenticazione a due fattori con Visual Studio. L'autenticazione a due fattori è un passaggio di autenticazione utente aggiuntivi. Questo passaggio aggiuntivo genera un numero di identificazione personale univoca (PIN) durante l'accesso. Il PIN verrà in genere inviato all'utente come un messaggio di posta elettronica o messaggio SMS. L'utente dell'app quindi immette il PIN come misura di autenticazione extra durante l'accesso.

### <a name="tutorial-tasks-and-information"></a>Informazioni e attività dell'esercitazione:

- [Creare un'applicazione Web Form ASP.NET](#createWebForms)
- [Programma di installazione SMS e l'autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori per utente registrato](#use2FA)
- [Risorse aggiuntive](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Creare un'applicazione Web Form ASP.NET

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versioni successive nonché. Inoltre, è necessario creare un [Twilio](https://www.twilio.com/try-twilio) dell'account, come illustrato di seguito.

> [!NOTE]
> Importante: È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.


1. Creare un nuovo progetto (**File**  - &gt; **nuovo progetto**) e selezionare il **applicazione Web ASP.NET** modello insieme a .NET Framework versione 4.5.2 dal **nuovo progetto** nella finestra di dialogo.
2. Dal **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **Web Form** modello. Lasciare l'autenticazione predefinita come **singoli account utente di**. Fare quindi clic **OK** per creare un nuovo progetto.  
    ![Finestra di dialogo Nuovo progetto ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Abilita Secure Sockets Layer (SSL) per il progetto. Seguire la procedura disponibile nel **abilita SSL per il progetto** sezione il [Introduzione a serie di esercitazioni in Web Form](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. In Visual Studio, aprire il **Console di gestione pacchetti** (**Tools**  - &gt; **Gestione pacchetti NuGet**  - &gt; **Console di gestione pacchetti**), quindi immettere il comando seguente:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Programma di installazione SMS e l'autenticazione a due fattori

Questa esercitazione Usa Twilio, ma è possibile usare qualsiasi provider SMS.

1. Creare un [Twilio](https://www.twilio.com/try-twilio) account.
2. Dal **Dashboard** scheda dell'account Twilio, copiare la **SID Account** e **Token di autenticazione.** Si aggiungerà li all'App in un secondo momento.
3. Dal **numeri** scheda, copiare il Twilio **numero di telefono** anche.
4. Assicurarsi di Twilio **SID dell'Account**, **Auth Token** e **numero di telefono** disponibili per l'app. Per semplificare le operazioni verranno archiviati i valori nel *Web. config* file. Quando si distribuiscono in Azure, è possibile archiviare i valori in modo sicuro nel **appSettings** scheda Configura sezione sul sito web. Inoltre, quando si aggiunge il numero di telefono, usare solo numeri.   
   Si noti che è anche possibile aggiungere le credenziali di SendGrid. SendGrid è un servizio di notifica di posta elettronica. Per informazioni dettagliate sull'abilitazione di SendGrid, vedere la sezione 'Hook di SendGrid' dell'esercitazione intitolata [creare un'App di ASP.NET Web Forms sicura con registrazione utente, inviare tramite posta elettronica di conferma e reimpostazione della password.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Security - store mai i dati sensibili nel codice sorgente. In questo esempio, l'account e le credenziali vengono archiviate nel **appSettings** sezione il *Web. config* file. In Azure, è possibile archiviare in modo sicuro questi valori sul **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** scheda nel portale di Azure. Per informazioni correlate, vedere l'argomento di Rick Anderson [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Configurare il `SmsService` classe la *App\_Start\IdentityConfig.cs* file effettuando le seguenti modifiche evidenziata in giallo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Aggiungere il codice seguente `using` istruzioni all'inizio della *IdentityConfig.cs* file: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Aggiorna il *Account/Manage.aspx* file rimuovendo le righe evidenziate in giallo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. Nel `Page_Load` gestore del *Manage.aspx.cs* code-behind, rimuovere il commento dalla riga di codice evidenziata in giallo in modo che venga visualizzato come segue: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Nel codebehind della *conto*/*TwoFactorAuthenticationSignIn.aspx.cs*, aggiornare il `Page_Load` gestore, aggiungendo il seguente codice evidenziato in giallo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Per cambiare il codice sopra riportato, DropDownList "Provider" che contiene le opzioni di autenticazione non sarà possibile reimpostare il primo valore. Ciò consentirà all'utente di selezionare correttamente tutte le opzioni da utilizzare per l'autenticazione, non solo il primo.
10. Nelle **Esplora soluzioni**, fare doppio clic su *default. aspx* e selezionare **imposta come pagina iniziale**.
11. Per testare l'app, creare innanzitutto l'app (**Ctrl**+**MAIUSC**+**B**) e quindi eseguire l'app (**F5**) e Selezionare **registrare** per creare un nuovo account utente o selezionare **Accedi** se l'account utente è già stato registrato.
12. Una volta (come l'utente) hanno effettuato l'accesso, fare clic su ID utente (indirizzo di posta elettronica) nella barra di spostamento per visualizzare il **Gestisci Account** pagina (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Fare clic su **Add** accanto a **numero di telefono** sul **Gestisci Account** pagina.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Aggiungere il numero di telefono in cui (come l'utente) da ricevere messaggi SMS (SMS) e scegliere il **Submit** pulsante.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    A questo punto, l'app userà le credenziali dal *Web. config* contattare Twilio. Verrà inviato un messaggio SMS (SMS) per il telefono associato all'account utente. È possibile verificare che è stato inviato il messaggio di Twilio, visualizzando il dashboard di Twilio.
15. In pochi secondi, il telefono associato all'account utente verrà visualizzato un messaggio di testo contenente il codice di verifica. Immettere il codice di verifica e premere **Submit**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Abilitare l'autenticazione a due fattori per un utente registrato

A questo punto, è stata abilitata l'autenticazione a due fattori per l'app. Per usare l'autenticazione a due fattori, un utente può modificare le relative impostazioni usando l'interfaccia utente. 

1. Come utente dell'app è possibile abilitare l'autenticazione a due fattori per l'account specifico facendo clic sull'ID utente (alias di posta elettronica) nella barra di spostamento per visualizzare il **Gestisci Account** pagina. Fare clic sui **abilitare** collegamento per abilitare l'autenticazione a due fattori per l'account.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Disconnettersi e quindi accedere di nuovo. Se è stato abilitato posta elettronica, è possibile selezionare SMS o posta elettronica per l'autenticazione a due fattori. Se non è stata abilitata tramite posta elettronica, vedere l'esercitazione intitolata [creare un'App di ASP.NET Web Forms sicura con la registrazione utente, conferma tramite posta elettronica e reimpostazione della Password](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Verrà visualizzata la pagina di autenticazione a due fattori in cui è possibile immettere il codice (dall'indirizzo di posta elettronica o SMS).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Facendo clic sui **memorizzare questo browser** casella di controllo sarà esente dalla necessità di usare l'autenticazione a due fattori per l'accesso quando si usa il browser e dispositivi in cui è stata selezionata la casella. Fino a quando gli utenti malintenzionati non è possibile ottenere l'accesso al dispositivo, abilitare l'autenticazione a due fattori e facendo clic sui **memorizzare questo browser** fornirà pratico accesso tramite password di un unico passaggio, mantenendo comunque strong protezione per l'autenticazione a due fattori per tutti gli accessi da dispositivi non attendibili. È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Risorse consigliate su collegamenti ad ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Distribuire un'App di moduli Web ASP.NET sicura con appartenenza, OAuth e Database SQL in un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie di esercitazioni Web Form ASP.NET - aggiungere un Provider di OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web Forms serie di esercitazioni - abilitare SSL per il progetto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Conferma account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Creazione dell'app in Facebook e ci si connette l'app al progetto](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
