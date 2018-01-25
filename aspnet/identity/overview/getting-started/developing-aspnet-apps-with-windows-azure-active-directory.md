---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Sviluppo di applicazioni ASP.NET con Azure Active Directory | Documenti Microsoft
author: Rick-Anderson
description: "Gli strumenti Microsoft ASP.NET per Azure Active Directory rende più semplice per abilitare l'autenticazione per applicazioni web ospitate in Azure. È possibile utilizzare Azure Authenti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 1ef0468d5f5c17480b23ac88983f30fe6f4979c0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Sviluppo di applicazioni ASP.NET con Azure Active Directory
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> Strumenti di Microsoft ASP.NET per Azure Active Directory rende più semplice per abilitare l'autenticazione per applicazioni web ospitate in [Azure](https://www.windowsazure.com/home/features/web-sites/). È possibile utilizzare l'autenticazione di Azure per autenticare gli utenti di Office 365 dell'organizzazione, gli account aziendali sincronizzati da Active Directory locale o degli utenti creati in un dominio di Azure Active Directory personalizzato. Abilitazione dell'autenticazione di Windows Azure consente di configurare l'applicazione per autenticare gli utenti con un solo [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenant.
> 
>  In questa esercitazione è stato scritto dal Rick Anderson[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)


In questa esercitazione viene illustrato come creare un'applicazione ASP.NET che è configurata per l'accesso con [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Si apprenderà anche come chiamare l'API Graph per ottenere informazioni sull'utente attualmente connesso e come distribuire l'applicazione in Azure.

## <a name="prerequisites"></a>Prerequisiti

1. [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) o [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 o versione successiva è obbligatorio.
3. Un account Azure. [Fare clic qui](https://azure.microsoft.com/pricing/free-trial/) per una valutazione gratuita, se non si dispone già di un account.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Aggiungere un amministratore globale di Active Directory

1. Accedi al [portale di gestione di Azure](https://manage.windowsazure.com/).
2. Tutti gli account di Azure contengono un **Directory predefinita** - fare clic su di esso, quindi fare clic su di **utenti** scheda nella parte superiore della pagina (vedere l'immagine riportata di seguito).
3. Fare clic su Aggiungi utente.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Creare un nuovo utente con il **amministratore globale** ruolo. Fare clic su **utenti** dal menu principale e quindi fare clic su di **Aggiungi utente** pulsante sulla barra dei comandi.
5. Nel **Aggiungi utente** finestra di dialogo immettere un nome per il nuovo utente e quindi fare clic sulla freccia destra.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Immettere il nome utente e impostare il ruolo **amministratore globale**. Gli amministratori globali richiedono un indirizzo di posta elettronica alternativo per scopi di ripristino password. Al termine, fare clic sulla freccia a destra.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Nella pagina successiva della finestra di dialogo, fare clic su **crea**. Verrà creata per il nuovo utente una password temporanea e visualizzata nella finestra di dialogo.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
 Salvare la password, sarà necessario per modificare la password dopo il primo accesso. La figura seguente mostra il nuovo account di amministratore. È necessario utilizzare Azure Active Directory per l'accesso in un'applicazione, non l'account Microsoft come mostrato in questa pagina.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Creare un'applicazione ASP.NET

Utilizzare i passaggi seguenti [Visual Studio Express 2013 per Web](https://www.microsoft.com/download/details.aspx?id=40747)e richiede [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. In Visual Studio, fare clic su **File** e quindi **nuovo progetto**. Nel **nuovo progetto** finestra di dialogo, selezionare Visual c# Web progetto dal menu a sinistra fare clic su **OK**. È inoltre possibile deselezionare il **Aggiungi Application Insights al progetto** se non si vuole la funzionalità per l'applicazione.
2. Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona **MVC**, quindi fare clic su **Modifica autenticazione**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Nel **Modifica autenticazione** finestra di dialogo Seleziona **account aziendali**. Queste opzioni possono essere utilizzate per registrare automaticamente l'applicazione con Azure AD, nonché di configurare automaticamente l'applicazione per l'integrazione con Azure AD. Non è necessario utilizzare il **Modifica autenticazione** finestra di dialogo per registrare e configurare l'applicazione, ma rende molto più semplice. Se si utilizza Visual Studio 2012, ad esempio, è possibile registrare l'applicazione nel portale di gestione di Azure manualmente e aggiornarne la configurazione per l'integrazione con Azure AD.  
 Nel menu a discesa, selezionare **Cloud - singola organizzazione** e **Single Sign On, lettura dati directory**. Immettere il dominio per la directory di Azure AD, ad esempio (nelle immagini seguenti) *aricka0yahoo.onmicrosoft.com*, quindi fare clic su **OK**. È possibile ottenere il nome di dominio dalla scheda domini per la Directory predefinita nel portale di azure (vedere l'immagine successiva verso il basso).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
 L'immagine seguente mostra il nome di dominio dal portale di Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > È possibile configurare l'URI ID applicazione che verrà registrato in Azure AD fare clic su **altre opzioni**. L'URI ID App è l'identificatore univoco per un'applicazione, che viene registrata in Azure AD e utilizzata dall'applicazione per identificare se stesso durante la comunicazione con Azure AD. Per ulteriori informazioni sull'URI ID App e altre proprietà delle applicazioni registrate, vedere [in questo argomento](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Facendo clic sulla casella di controllo sotto il campo URI ID App, è anche possibile scegliere di sovrascrivere una registrazione esistente in Azure AD che utilizza lo stesso URI ID App.
4. Dopo aver fatto clic **OK**, verrà visualizzata una finestra di dialogo Accedi e sarà necessario accedere con un account di amministratore globale (non l'account Microsoft associato alla sottoscrizione). Se è stato creato un nuovo account amministratore in precedenza, verrà richiesto di modificare la password e quindi accedere di nuovo usando la nuova password.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Dopo che è stato autenticato correttamente, il **nuovo progetto ASP.NET** finestra di dialogo visualizzerà la scelta dell'autenticazione (**aziendale** ) e la directory in cui è la nuova applicazione registrata (*aricka0yahoo.onmicrosoft.com* nell'immagine seguente). Sotto queste informazioni, selezionare la casella di controllo con etichettata **Host nel cloud**. Se questa casella di controllo è selezionata, il progetto verrà eseguito il provisioning come un'app web di Azure e verrà abilitato per la pubblicazione semplice in un secondo momento. Fare clic su **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. Il **Configura sito Web di Azure** verrà visualizzata finestra di dialogo, utilizzando un nome generato automaticamente del sito e regione. Si noti inoltre l'account che è stato eseguito l'acceso nella finestra di dialogo. Si desidera assicurarsi che l'account sia quello di cui è associata la sottoscrizione di Azure, in genere un account Microsoft.

    > [!NOTE]
    > Questo progetto richiede un database. È necessario selezionare uno dei database esistenti o crearne uno nuovo. Un database è necessario perché il progetto utilizza già un file di database locale per archiviare una piccola quantità di dati di configurazione di autenticazione. Quando si distribuisce l'applicazione in un sito Web di Azure, questo database non è incluso nel pacchetto di distribuzione, pertanto è necessario sceglierne uno che sia accessibile nel cloud. Fare clic su **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Verrà creato il progetto e le opzioni di autenticazione e le opzioni di app web verranno configurate automaticamente con il progetto. Al termine di questo processo, eseguire il progetto premendo localmente **^ F5**. Verrà richiesto di accedere utilizzando l'account dell'organizzazione. Fornire il nome utente e la password per l'account creato in precedenza e fare clic su **Accedi**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Dopo l'accesso riuscito, il sito ASP.NET mostrerà che ha eseguito l'autenticazione visualizzando il nome utente nell'angolo superiore destro della pagina.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
 Se viene visualizzato l'errore:  
 Valore non può essere null o vuoto. Nome del parametro: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
 vedere il [debug](#dbg) sezione alla fine dell'esercitazione.

## <a name="basics-of-the-graph-api"></a>Nozioni di base di API Graph

[L'API Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) è l'interfaccia di programmazione utilizzato per eseguire CRUD e altre operazioni per gli oggetti nella directory di Azure AD. Se si seleziona un'opzione di Account aziendale per l'autenticazione quando si crea un nuovo progetto in Visual Studio 2013, l'applicazione già essere configurato per chiamare l'API Graph. In questa sezione viene brevemente di funzionamento l'API Graph.

1. Nell'applicazione in esecuzione, fare clic sul nome dell'utente connesso nella parte superiore destra della pagina. Verrà visualizzata la pagina profilo utente, ovvero un'azione nel Controller Home. Si noterà che la tabella contiene informazioni sull'account amministratore creato in precedenza. Queste informazioni vengono archiviate nella directory e viene chiamata l'API Graph per recuperare queste informazioni durante il caricamento della pagina.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Tornare a Visual Studio ed espandere il **controller** cartella e quindi aprire il **HomeController.cs** file. Si noterà un **UserProfile()** azione che contiene il codice per recuperare un token e quindi chiamare l'API Graph. Questo codice viene duplicato sotto: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Per chiamare l'API Graph, è innanzitutto necessario recuperare un token. Quando il token viene recuperato, il valore di stringa deve essere aggiunto nell'intestazione dell'autorizzazione per tutte le richieste successive all'API Graph. La maggior parte del codice sopra gestisce i dettagli di autenticazione ad Azure AD per ottenere un token, utilizzando il token per effettuare una chiamata all'API Graph e quindi per la trasformazione della risposta in modo che può essere presentata nella vista.

    La parte più rilevante per una discussione è la seguente riga evidenziata: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Questa riga rappresenta il nome dell'utente, che è stato deserializzato dalla risposta JSON e viene visualizzata nella vista.

    È possibile chiamare l'API Graph usando HttpClient e gestire manualmente i dati non elaborati, ma un modo più semplice consiste nell'utilizzare il [libreria Client di grafico disponibili tramite NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). La libreria Client gestisce le richieste HTTP non elaborate e la trasformazione dei dati restituiti per l'utente e rende più facile lavorare con l'API Graph in un ambiente .NET. Vedere gli esempi di codice correlati API Graph su [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Distribuire l'applicazione in Azure

La procedura seguente mostra come distribuire l'applicazione in Azure. Nei passaggi precedenti, connesso il nuovo progetto con un'app web in Azure, pertanto è pronto per la pubblicazione in pochi passaggi.

1. In Visual Studio, fare clic sul progetto e scegliere **pubblica**. Il **pubblica sul Web** finestra di dialogo verrà visualizzata con ogni impostazione già configurata. Fare clic sul **Avanti** pulsante per visualizzare il **impostazioni** pagina. Potrebbe essere richiesto di autenticazione. Verificare che l'autenticazione utilizzando l'account della sottoscrizione di Azure (in genere un account Microsoft) e non l'account aziendale creato in precedenza.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Controllare il **Abilita autenticazione aziendale** opzione. Nel **dominio** immettere il dominio per la directory. Dal **livello di accesso** elenco a discesa, selezionare **Single Sign On, lettura dati directory**. Si noterà che il database precedente usato è già popolato nel **database** sezione. Fare clic su **Pubblica**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Verrà avviata la distribuzione del sito Web, e una nuova finestra del browser verrà visualizzato. Potrebbe essere richiesto per autenticare nuovamente alla directory. Dopo che è stato autenticato, si verrà reindirizzati al sito Web appena pubblicato in Azure.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debug dell'applicazione

Se viene visualizzato l'errore seguente:   
 Valore non può essere null o vuoto. Nome del parametro: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Sostituire il codice di *Views\Shared\\loginpartial. cshtml* file con le operazioni seguenti:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Dopo l'esecuzione dell'app, se l'utente connesso viene visualizzato "Utente Null", disconnettersi e accedere con l'account di Active Directory creato in precedenza.

Un'ottima esercitazione seguire è di Rick Rainey [approfondimento: siti Web di Azure e autenticazione aziendale tramite Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Altre informazioni

- [Approfondimento: Siti Web di Azure e autenticazione aziendale tramite Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Panoramica dell'API Azure AD Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scenari di autenticazione in Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Esempi di codice di Azure AD su GitHub](https://github.com/AzureADSamples)
