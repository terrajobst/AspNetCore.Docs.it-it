---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: App ASP.NET MVC 5 con SMS e l'autenticazione a due fattori di posta elettronica | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione viene illustrato come compilare un'app web ASP.NET MVC 5 con autenticazione a due fattori. È necessario completare l'applicazione web crea un sicuro ASP.NET MVC 5 con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 5e1c54b3901f2c8c85134445c1fa91ee9f2e0d59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>App ASP.NET MVC 5 con SMS e l'autenticazione a due fattori di posta elettronica
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione viene illustrato come compilare un'app web ASP.NET MVC 5 con autenticazione a due fattori. È consigliabile completare [creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di procedere. È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Il download contiene funzioni di supporto di debug che consentono di testare conferma tramite posta elettronica e SMS senza dover configurare un messaggio di posta elettronica o il provider SMS.
> 
> In questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Creare un'applicazione MVC ASP.NET](#createMvc)
- [Impostare SMS per l'autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori](#enable2)
- [Risorse aggiuntive](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Creare un'applicazione MVC ASP.NET

Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

> [!NOTE]
> Avviso: È necessario completare [creare un'app web di ASP.NET MVC 5 sicura con l'accesso, la reimpostazione di posta elettronica di conferma e la password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di procedere. È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.


1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. Web Form supporta anche ASP.NET Identity, pertanto è possibile attenersi alla procedura simile in un'app di web form.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Lasciare l'autenticazione predefinita come **singoli account utente di**. Se si desidera ospitare l'applicazione in Azure, lasciare selezionata la casella di controllo. Più avanti nell'esercitazione si distribuirà in Azure. È possibile [aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Impostare il [progetto utilizzare SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Impostare SMS per l'autenticazione a due fattori

In questa esercitazione vengono fornite istruzioni per l'utilizzo di Twilio o ASPSMS ma è possibile usare qualsiasi altro provider SMS.

1. **Creazione di un Account utente con un provider SMS**  
  
   Creare un [Twilio](https://www.twilio.com/try-twilio) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.
2. **Installazione di pacchetti aggiuntivi o l'aggiunta di riferimenti al servizio**  
  
   Twilio:  
   Nella Console di gestione pacchetti, immettere il comando seguente:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   È necessario aggiungere il riferimento al servizio seguente:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Indirizzo:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Spazio dei nomi:  
    `ASPSMSX2`
3. **Capire le credenziali utente del Provider SMS**  
  
   Twilio:  
   Dal **Dashboard** scheda dell'account di Twilio, copia il **SID di Account** e **token di autorizzazione**.  
  
   ASPSMS:  
   Le impostazioni dell'account, passare alla **parametri Userkey** e copiarlo insieme il Self-definito **Password**.  
  
   Questi valori in un secondo momento verrà archiviato il *Web. config* file all'interno di chiavi `"SMSAccountIdentification"` e `"SMSAccountPassword"` .
4. **Specifica l'ID mittente / iniziatore**  
  
   Twilio:  
   Dal **numeri** scheda, copiare il numero di telefono di Twilio.  
  
   ASPSMS:  
   All'interno di **dei creatori di sbloccare** Menu, sbloccare una o più mittenti o scegliere un creatore alfanumerico (non supportato da tutte le reti).  
  
   Questo valore in un secondo momento verrà archiviato il *Web. config* file all'interno della chiave `"SMSAccountFrom"` .
5. **Il trasferimento di credenziali del provider SMS in app**  
  
   Le credenziali e il numero di telefono mittente rendere disponibile l'app. Per semplificare le operazioni, questi valori in verrà archiviato il *Web. config* file. Quando si distribuisce in Azure, che è possibile archiviare i valori in modo sicuro nel **impostazioni app** scheda Configura sezione sul sito web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Sicurezza - archivio mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice precedente per semplificare il codice di esempio. Vedere [procedure consigliate per la distribuzione delle password e altri dati sensibili su ASP.NET e in Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementazione del trasferimento dei dati al provider SMS**  
  
   Configurare il `SmsService` classe il *App\_Start\IdentityConfig.cs* file.  
  
   A seconda del provider SMS utilizzato attivare il **Twilio** o **ASPSMS** sezione: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aggiornamento di *Views\Manage\Index.cshtml* visualizzazione Razor: (Nota: non è sufficiente rimuovere i commenti nel codice esistente, utilizzare il codice riportato di seguito.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Verificare il `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` metodi di azione nel `ManageController` hanno il[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attributo:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Eseguire l'app e accedere con l'account che è stato registrato in precedenza.
10. Fare clic sull'ID utente, che attiva il `Index` metodo di azione `Manage` controller.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Fare clic su Aggiungi.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. Il `AddPhoneNumber` metodo di azione consente di visualizzare una finestra di dialogo per immettere un numero di telefono che può ricevere i messaggi SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. In pochi secondi si otterrà un messaggio di testo con il codice di verifica. L'immissione e premere **Invia**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. La visualizzazione Gestione mostra che il numero di telefono è stato aggiunto.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Nell'app modello generato, è necessario utilizzare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA). Per abilitare 2FA, fare clic sull'ID utente (alias di posta elettronica) nella barra di spostamento.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Fare clic su Abilita 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Disconnessione, quindi accedere di nuovo. Se è stato abilitato posta elettronica (vedere il [esercitazione precedente](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare il SMS o posta elettronica per 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Verrà visualizzata la pagina di verifica di codice in cui è possibile immettere il codice (da posta elettronica o SMS).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di eseguire l'accesso quando si utilizza il browser e dispositivi in cui è stata selezionata la casella 2FA. Fino a quando gli utenti malintenzionati non possono accedere al dispositivo, abilitazione 2FA e facendo clic su di **ricordare questo browser** fornirà accedere facilmente la password di un unico passaggio, conservando al tempo protezione 2FA complesse per tutti gli accessi dai dispositivi non trusted. Tale operazione può essere eseguita su qualsiasi dispositivo privata che vengono utilizzate regolarmente.

Questa esercitazione viene fornita una rapida introduzione all'abilitazione 2FA in una nuova applicazione MVC ASP.NET. L'esercitazione [autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) vengono inseriti nel dettaglio in code-behind dell'esempio.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [L'autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra in dettaglio l'autenticazione a due fattori
- [Collegamenti a ASP.NET Identity consigliato risorse](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Account di conferma e il recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) vengono descritti in dettaglio in Conferma password di ripristino e account.
- [App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in questa esercitazione viene illustrato come scrivere un'applicazione ASP.NET MVC 5 con autorizzazione di Facebook e Google OAuth 2. Viene inoltre illustrato come aggiungere ulteriori dati al database di identità.
- [Distribuire un'app protetta ASP.NET MVC con appartenenza, OAuth e il Database SQL a Web di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). In questa esercitazione aggiunge distribuzione di Azure, come proteggere le app con i ruoli, come utilizzare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.
- [Creazione di un'app di Google per OAuth 2 e l'applicazione di connessione al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Creazione di app in Facebook e l'applicazione di connessione al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Impostazione di SSL nel progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
