---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: App ASP.NET MVC 5 con SMS e posta elettronica l'autenticazione a due fattori | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come compilare un'app web ASP.NET MVC 5 con autenticazione a due fattori. È necessario completare l'app web crea un ASP.NET MVC 5 sicura con...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 97b73c5ae18a528d33b44a4de4afc434ac46af48
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577600"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>App ASP.NET MVC 5 con SMS e posta elettronica l'autenticazione a due fattori
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Questa esercitazione illustra come compilare un'app web ASP.NET MVC 5 con autenticazione a due fattori. È consigliabile completare [creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di procedere. È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Il download contiene gli helper di debug che consentono di testare conferma tramite posta elettronica e SMS senza dover configurare un indirizzo di posta elettronica o il provider SMS.
> 
> Questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Creare un'app ASP.NET MVC](#createMvc)
- [Configurazione di SMS per l'autenticazione a due fattori](#SMS)
- [Abilitare l'autenticazione a due fattori](#enable2)
- [Risorse aggiuntive](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Creare un'app ASP.NET MVC

Per iniziare, installare ed eseguire [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.

> [!NOTE]
> Avviso: È consigliabile completare [creare un'app web ASP.NET MVC 5 sicura con accesso, messaggio di posta elettronica conferma e reimpostazione della password](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) prima di procedere. È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.


1. Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC. Web Form supporta inoltre ASP.NET Identity, pertanto è possibile seguire la procedura in un'applicazione web form.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Lasciare l'autenticazione predefinita come **singoli account utente di**. Se vuoi ospitare l'app in Azure, lasciare selezionata la casella di controllo. Più avanti nell'esercitazione verrà distribuita in Azure. È possibile [aprire un account Azure gratuito](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Impostare il [progetto per l'uso di SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Configurazione di SMS per l'autenticazione a due fattori

Questa esercitazione offre istruzioni per l'uso di Twilio o ASPSMS ma è possibile usare qualsiasi altro provider SMS.

1. **Creazione di un Account utente con un provider SMS**  
  
   Creare un [Twilio](https://www.twilio.com/try-twilio) o un' [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.
2. **Installare pacchetti aggiuntivi o per aggiungere i riferimenti al servizio**  
  
   Twilio:  
   Nella Console di gestione pacchetti immettere il comando seguente:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   È necessario aggiungere il riferimento al servizio seguenti:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Indirizzo:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Spazio dei nomi:  
    `ASPSMSX2`
3. **Scoprire le credenziali utente del Provider SMS**  
  
   Twilio:  
   Dal **Dashboard** scheda dell'account Twilio, copiare la **SID Account** e **token di autenticazione**.  
  
   ASPSMS:  
   Da impostazioni dell'account, passare a **Userkey** e copiarlo insieme l'autodefinito **Password**.  
  
   In un secondo momento verranno archiviati i valori nel *Web. config* file tra quelle `"SMSAccountIdentification"` e `"SMSAccountPassword"` .
4. **Specifica l'ID mittente / creatore**  
  
   Twilio:  
   Dal **numeri** scheda, copiare il numero di telefono di Twilio.  
  
   ASPSMS:  
   All'interno di **lo sblocco dei creatori** Menu, sbloccare una o più entità di origine o scegliere un creatore alfanumerico (non supportato da tutte le reti).  
  
   In un secondo momento si archivierà questo valore nel *Web. config* file all'interno della chiave `"SMSAccountFrom"` .
5. **Il trasferimento di credenziali del provider SMS in app**  
  
   Rendere disponibile l'app per le credenziali e il numero di telefono mittente. Per semplificare le operazioni verranno archiviati i valori nel *Web. config* file. Quando si distribuisce in Azure, è possibile archiviare i valori in modo sicuro nel **le impostazioni dell'app** scheda Configura sezione sul sito web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Security - store mai i dati sensibili nel codice sorgente. L'account e le credenziali vengono aggiunti al codice precedente per mantenere semplice l'esempio. Visualizzare [procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementazione di trasferimento dei dati al provider SMS**  
  
   Configurare il `SmsService` classe la *App\_Start\IdentityConfig.cs* file.  
  
   A seconda del provider SMS usato attivare i **Twilio** o nella **ASPSMS** sezione: 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Aggiorna il *Views\Manage\Index.cshtml* visualizzazione Razor: (Nota: non è sufficiente rimuovere i commenti nel codice esistente, usare il codice seguente.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Verificare i `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` metodi di azione nel `ManageController` hanno il[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attributo:  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Eseguire l'app e accedere con l'account registrato in precedenza.
10. Fare clic sul proprio ID utente, che si attiva la `Index` metodo di azione `Manage` controller.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Fare clic su Aggiungi.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. Il `AddPhoneNumber` metodo di azione consente di visualizzare una finestra di dialogo per immettere un numero di telefono che può ricevere messaggi SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. In pochi secondi si riceverà un messaggio di testo con il codice di verifica. Immetterla e premere **Submit**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. La visualizzazione di gestione mostra che il numero di telefono è stato aggiunto.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Nell'app modello generato, è necessario usare l'interfaccia utente per abilitare l'autenticazione a due fattori (2FA). Per abilitare 2FA, fare clic sul proprio ID utente (alias di posta elettronica) nella barra di spostamento.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Fare clic su Abilita 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Punto di disconnessione, quindi accedere di nuovo. Se è stato abilitato posta elettronica (vedere la [esercitazione precedente](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), è possibile selezionare il SMS o posta elettronica di autenticazione 2fa.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Verrà visualizzata la pagina di verifica codice in cui è possibile immettere il codice (dall'indirizzo di posta elettronica o SMS).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Facendo clic sui **memorizzare questo browser** casella di controllo sarà esente dalla necessità di usare 2FA per l'accesso quando si usa il browser e dispositivi in cui è stata selezionata la casella. Fino a quando gli utenti malintenzionati non è possibile ottenere l'accesso al dispositivo, abilitare 2FA e facendo clic sui **memorizzare questo browser** fornirà pratico accesso tramite password di un unico passaggio, mantenendo comunque la protezione avanzata 2FA per tutti gli accessi dai dispositivi non attendibili. È possibile farlo in qualsiasi dispositivo privata che vengono utilizzate regolarmente.

Questa esercitazione offre un'introduzione rapida abilitare 2FA in una nuova app ASP.NET MVC. L'esercitazione [autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) descrive in dettaglio il codice dietro il codice di esempio.

<a id="addRes"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Autenticazione a due fattori tramite SMS e posta elettronica con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) descrive in dettaglio l'autenticazione a due fattori
- [Risorse consigliate su collegamenti ad ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Conferma dell'account e recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) descrivono in maggiore dettaglio nella conferma account e di ripristino della password.
- [App MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) questa esercitazione illustra come scrivere un'app ASP.NET MVC 5 con Facebook e Google OAuth 2 l'autorizzazione. Viene inoltre illustrato come aggiungere dati aggiuntivi al database di identità.
- [Distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e Database SQL di Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Questa esercitazione aggiunge una distribuzione di Azure, come proteggere l'app con i ruoli, come usare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.
- [Creazione di un'app Google per OAuth 2 e ci si connette l'app al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Creazione dell'app in Facebook e ci si connette l'app al progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurare SSL nel progetto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
