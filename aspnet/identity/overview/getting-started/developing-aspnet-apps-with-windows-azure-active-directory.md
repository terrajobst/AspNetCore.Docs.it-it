---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Sviluppo di App ASP.NET con Azure Active Directory | Microsoft Docs
author: Rick-Anderson
description: Gli strumenti Microsoft ASP.NET per Azure Active Directory rende più semplice per abilitare l'autenticazione per applicazioni web ospitate in Azure. È possibile usare Azure autenti...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: e2df906d220d738c45006de8b3c92e157ca9e57e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833406"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Sviluppo di App ASP.NET con Azure Active Directory
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

Gli strumenti Microsoft ASP.NET per Azure Active Directory semplifica l'abilitazione dell'autenticazione per App web ospitate in [Azure](https://www.windowsazure.com/home/features/web-sites/). È possibile usare l'autenticazione di Azure per autenticare gli utenti di Office 365 dell'organizzazione, gli account aziendali sincronizzati dall'ambiente locale di Active Directory o utenti creati in un dominio di Azure Active Directory personalizzato. Abilitazione dell'autenticazione di Windows Azure consente di configurare l'applicazione per autenticare gli utenti che usano un unico [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenant.

Questa esercitazione illustrerà come creare un'applicazione ASP.NET che è configurata per l'accesso con [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Si apprenderà anche come chiamare l'API Graph per ottenere informazioni sull'utente attualmente connesso nell'e su come distribuire l'applicazione in Azure.

## <a name="prerequisites"></a>Prerequisiti

1. [Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) oppure [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 o versione successiva è obbligatorio.
3. Un account Azure. [Fare clic qui](https://azure.microsoft.com/pricing/free-trial/) per una valutazione gratuita, se non si dispone già di un account.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Aggiungere un amministratore globale per il servizio Active Directory

1. Accedi per il [portale di gestione Azure](https://manage.windowsazure.com/).
2. Tutti gli account di Azure contengono un **Directory predefinita** : fare clic su di esso, quindi fare clic sul **utenti** scheda nella parte superiore della pagina (vedere la figura seguente).
3. Fare clic su Aggiungi utente.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Creare un nuovo utente con il **amministratore globale** ruolo. Fare clic su **gli utenti** dal menu in alto e quindi fare clic sui **Add User** pulsante sulla barra dei comandi.
5. Nel **Add User** finestra di dialogo immettere un nome per il nuovo utente e quindi fare clic sulla freccia a destra.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Immettere il nome utente e impostare il ruolo **amministratore globale**. Gli amministratori globali di richiedono un indirizzo di posta elettronica alternativo per scopi di ripristino password. Al termine, fare clic sulla freccia a destra.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Nella pagina successiva della finestra di dialogo, fare clic su **Create**. Una password temporanea verrà creata per il nuovo utente e visualizzata nella finestra di dialogo.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   Salvare la password, sarà necessario modificare la password dopo il primo accesso. L'immagine seguente mostra il nuovo account di amministratore. È necessario usare Azure Active Directory per accedere all'app, non l'account Microsoft come mostrato in questa pagina.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Creare un'applicazione ASP.NET

La procedura seguente usa [Visual Studio Express 2013 per il Web](https://www.microsoft.com/download/details.aspx?id=40747)e richiede [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. In Visual Studio, fare clic su **File** e quindi **nuovo progetto**. Nel **nuovo progetto** finestra di dialogo, selezionare Visual c# Web del progetto dal menu a sinistra fare clic su **OK**. È anche possibile deselezionare le **aggiungere Application Insights al progetto** se non si vuole la funzionalità per l'applicazione.
2. Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona **MVC**, quindi fare clic su **Modifica autenticazione**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Nel **Modifica autenticazione** finestra di dialogo, seleziona **gli account aziendali**. Queste opzioni sono utilizzabile per registrare automaticamente l'applicazione con Azure AD, nonché di configurare automaticamente l'applicazione per l'integrazione con Azure AD. Non è necessario usare il **Modifica autenticazione** finestra di dialogo per registrare e configurare l'applicazione, ma è molto facile. Se si usa Visual Studio 2012, ad esempio, è possibile registrare l'applicazione nel portale di gestione di Azure manualmente e aggiornare la propria configurazione per l'integrazione con Azure AD.  
   Nei menu a discesa, selezionare **Cloud - organizzazione singola** e **Single Sign On, lettura dati directory**. Immettere il dominio per la directory di Azure AD, ad esempio (nelle immagini seguenti) *aricka0yahoo.onmicrosoft.com*, quindi fare clic su **OK**. È possibile ottenere il nome di dominio dalla scheda domini della directory predefinita nel portale di azure (vedere la figura seguente verso il basso).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   L'immagine seguente mostra il nome di dominio dal portale di Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Facoltativamente, è possibile configurare l'URI dell'ID applicazione che verrà registrato in Azure AD facendo **altre opzioni**. L'URI ID App è l'identificatore univoco per un'applicazione, che viene registrata in Azure AD e usata dall'applicazione per identificarsi quando si comunica con Azure AD. Per altre informazioni sull'URI ID App e altre proprietà delle applicazioni registrate, vedere [in questo argomento](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Facendo clic sulla casella di controllo sotto il campo URI ID App, è anche possibile scegliere di sovrascrivere una registrazione esistente in Azure AD che utilizza lo stesso URI ID App.
4. Dopo aver fatto clic **OK**, verrà visualizzata una finestra di dialogo Accedi e dovrai accedere con un account di amministratore globale (non l'account Microsoft associato alla sottoscrizione). Se è stato creato un nuovo account amministratore in precedenza, verrà richiesto di cambiare la password e quindi accedere di nuovo usando la nuova password.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Dopo aver eseguito l'autenticazione, il **nuovo progetto ASP.NET** finestra di dialogo Mostra la scelta di autenticazione (**Organizational** ) e la directory in cui la nuova applicazione verrà registrata (*aricka0yahoo.onmicrosoft.com* nell'immagine seguente). Sotto queste informazioni, selezionare la casella di controllo etichettato **ospita nel cloud**. Se questa casella di controllo è selezionata, il progetto verrà eseguito il provisioning come un'app web di Azure e verrà abilitato per la pubblicazione semplice in un secondo momento. Fare clic su **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. Il **sito Web di Azure configura** verrà visualizzata finestra di dialogo, usando un nome del sito generato automaticamente e area. Si noti inoltre l'account che attualmente effettuato nella finestra di dialogo. Si desidera assicurarsi che questo account è quello che è associata la sottoscrizione di Azure, in genere un account Microsoft.

    > [!NOTE]
    > Questo progetto richiede un database. È necessario selezionare uno dei database esistenti o crearne uno nuovo. Un database è necessario perché il progetto usa già un file di database locale per archiviare una piccola quantità di dati di configurazione di autenticazione. Quando si distribuisce l'applicazione in un sito Web di Azure, questo database non è incluso nel pacchetto di distribuzione, pertanto è necessario sceglierne uno che sia accessibile nel cloud. Fare clic su **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Verrà creato il progetto e le opzioni di autenticazione e le opzioni di app web verranno configurate automaticamente con il progetto. Dopo aver completato questo processo, eseguire il progetto in locale premendo **^ F5**. È necessario accedere usando l'account dell'organizzazione. Specificare il nome utente e la password per l'account creato in precedenza e fare clic su **Accedi**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Dopo l'accesso, verrà visualizzato il sito ASP.NET che è stato autenticato visualizzando il nome utente nell'angolo superiore destro della pagina.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Se viene visualizzato l'errore:  
   Valore non può essere null o vuoto. Nome del parametro: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   vedere le [debug](#dbg) sezione alla fine dell'esercitazione.

## <a name="basics-of-the-graph-api"></a>Nozioni di base dell'API Graph

[L'API Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) è l'interfaccia a livello di codice usato per eseguire CRUD e altre operazioni su oggetti nelle directory di Azure AD. Se si seleziona un'opzione Account aziendale per l'autenticazione quando si crea un nuovo progetto in Visual Studio 2013, l'applicazione già essere configurata per chiamare l'API Graph. Questa sezione illustra brevemente è come funziona l'API Graph.

1. Nell'applicazione in esecuzione, fare clic sul nome dell'utente connesso nella parte superiore destra della pagina. Verrà visualizzata la pagina del profilo utente, è un'azione nel Controller Home. Si noterà che la tabella contiene le informazioni utente relative all'account amministratore creato in precedenza. Queste informazioni vengono archiviate nella directory e viene chiamata l'API Graph per recuperare queste informazioni durante il caricamento della pagina.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Tornare a Visual Studio ed espandere la **controller** cartella e quindi aprire il **HomeController.cs** file. Si noterà una **UserProfile()** azione che contiene il codice per recuperare un token e quindi chiamare l'API Graph. Questo codice viene duplicato sotto: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Per chiamare l'API Graph, è innanzitutto necessario recuperare un token. Quando il token viene recuperato, il valore di stringa deve essere aggiunto nell'intestazione dell'autorizzazione per tutte le richieste successive all'API Graph. La maggior parte del codice precedente gestisce i dettagli dell'autenticazione ad Azure AD per ottenere un token, utilizzando il token per effettuare una chiamata all'API Graph e la trasformazione quindi la risposta in modo che possa essere presentato nella visualizzazione.

    La parte più rilevante per la discussione è la seguente riga evidenziata: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Questa riga rappresenta il nome dell'utente, che è stato deserializzato dalla risposta JSON e viene presentata nella vista.

    È possibile chiamare l'API Graph usando HttpClient e gestire manualmente i dati non elaborati, ma in modo più semplice consiste nell'usare la [libreria Client di grafico disponibili tramite NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). La libreria Client gestisce le richieste HTTP non elaborate e la trasformazione dei dati restituiti per l'utente e rende più semplice lavorare con l'API Graph in un ambiente .NET. Vedere gli esempi di codice correlati di API Graph su [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Distribuire l'applicazione in Azure

La procedura seguente illustrerà come distribuire l'applicazione in Azure. Nei passaggi precedenti, connesso il nuovo progetto con un'app web in Azure, affinché sia pronto per essere pubblicata in pochi passaggi.

1. In Visual Studio, fare doppio clic sul progetto e scegliere **pubblica**. Il **pubblica sul Web** finestra di dialogo verrà visualizzata con ogni impostazione già configurata. Fare clic sui **successivo** pulsante per passare al **impostazioni** pagina. Potrebbe essere richiesto di eseguire l'autenticazione; Assicurarsi che si esegue l'autenticazione usando l'account di sottoscrizione di Azure (in genere un account Microsoft) e non l'account aziendale creato in precedenza.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Verificare i **Abilita autenticazione aziendale** opzione. Nel **dominio** immettere il dominio per la directory. Dal **livello di accesso** elenco a discesa, selezionare **Single Sign On, lettura dati directory**. Si noterà che il database precedente è stato usato è già popolato nel **database** sezione. Fare clic su **Pubblica**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio verrà avviata la distribuzione del sito Web, e quindi verrà visualizzata una nuova finestra del browser. Potrebbe essere richiesto per l'autenticazione alla directory ancora una volta. Dopo l'autenticazione, si verrà reindirizzati al sito Web appena pubblicato in Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debug dell'app

Se viene visualizzato l'errore seguente:   
 Valore non può essere null o vuoto. Nome del parametro: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Sostituire il codice nel *Views\Shared\\loginpartial. cshtml* file con il codice seguente:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Dopo l'esecuzione dell'app, se l'utente connesso viene visualizzato "User Null", disconnettersi e riaccedere con l'account di Active Directory creato in precedenza.

Un'ottima esercitazione da seguire è di Rick Rainey [Deep Dive: siti Web di Azure e l'autenticazione dell'organizzazione tramite Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Altre informazioni

- [Deep Dive: Siti Web di Azure e l'autenticazione dell'organizzazione tramite Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Informazioni generali sull'API Graph di Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scenari di autenticazione in Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Esempi di codice per Azure AD su GitHub](https://github.com/AzureADSamples)
