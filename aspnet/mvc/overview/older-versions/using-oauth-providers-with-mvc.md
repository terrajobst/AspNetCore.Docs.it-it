---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Utilizzo di provider OAuth con MVC 4 | Documenti Microsoft
author: tfitzmac
description: In questa esercitazione viene illustrato come compilare un'applicazione web ASP.NET MVC 4 che consente agli utenti di accedere con le credenziali da un provider esterno, ad esempio Facebo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: f0d053cecbf9a59f258470ee370852e3f112908c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28033574"
---
<a name="using-oauth-providers-with-mvc-4"></a>Utilizzo di provider OAuth con MVC 4
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione viene illustrato come compilare un'applicazione web ASP.NET MVC 4 che consente agli utenti di accedere con le credenziali da un provider esterno, ad esempio Facebook, Twitter, Microsoft o Google e quindi integrerà alcune delle funzionalità di tali provider nel applicazione Web. Per semplicità, in questa esercitazione è incentrata sull'utilizzo di credenziali da Facebook.
> 
> Per usare le credenziali esterne in un'applicazione web ASP.NET MVC 5, vedere [creare un'App di ASP.NET MVC 5 con Facebook, Google OAuth2 e OpenID Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Abilitazione di queste credenziali in siti web offre un vantaggio significativo perché milioni di utenti dispongono già di account con tali provider esterni. Questi utenti potrebbero essere più inclini a effettuare l'iscrizione per il sito se non dispone di creare e ricordare un nuovo insieme di credenziali. Inoltre, dopo che un utente ha effettuato l'accesso tramite uno di questi provider, è possibile incorporare sociale operazioni dal provider.


## <a name="what-youll-build"></a>Occorre invece compilare

In questa esercitazione sono disponibili due obiettivi principali:

1. Consentire agli utenti di accedere con le credenziali da un provider OAuth.
2. Recuperare informazioni sull'account dal provider e l'integrazione di tali informazioni con la registrazione dell'account per il sito.

Sebbene negli esempi inclusi in questa esercitazione è incentrata sull'uso di Facebook come provider di autenticazione, è possibile modificare il codice per utilizzare uno dei provider. La procedura per implementare qualsiasi provider è molto simile a quelli visualizzati in questa esercitazione. Si noterà solo differenze significative quando si effettuano chiamate dirette all'API del provider impostato.

## <a name="prerequisites"></a>Prerequisiti

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) o [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Or

- Microsoft Visual Studio 2010 SP1 o [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Inoltre, questo argomento si presuppone di knowledge base su MVC ASP.NET e Visual Studio. Se occorre un'introduzione ad ASP.NET MVC 4, vedere [Intro to ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Creare il progetto

In Visual Studio, creare una nuova applicazione Web ASP.NET MVC 4 e denominarlo &quot;OAuthMVC&quot;. È possibile usare .NET Framework 4.5 o 4.

![Crea progetto](using-oauth-providers-with-mvc/_static/image1.png)

Nella finestra Nuovo progetto ASP.NET MVC 4, selezionare **applicazione Internet** e lasciare **Razor** come il motore di visualizzazione.

![Selezionare l'applicazione Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Abilitare un provider

Quando si crea un'applicazione web MVC 4 con il modello di applicazione Internet, il progetto è stato creato con un file denominato AuthConfig.cs nell'App\_cartella di avvio.

![File AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Il file AuthConfig contiene codice per registrare i client per i provider di autenticazione esterno. Per impostazione predefinita, questo codice è commentato, pertanto nessuno dei provider esterni sono abilitati.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

È necessario rimuovere il commento di questo codice per utilizzare il client di autenticazione esterno. Rimuovere solo i provider che si desidera includere nel sito. Per questa esercitazione, si abiliterà solo le credenziali di Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Si noti nell'esempio precedente che il metodo include stringhe vuote per i parametri di registrazione. Se si tenta di eseguire l'applicazione ora, l'applicazione genera un'eccezione di argomento, poiché le stringhe vuote non sono consentite per i parametri. Per fornire i valori validi, è necessario registrare il sito web con il provider esterni, come illustrato nella sezione successiva.

## <a name="registering-with-an-external-provider"></a>Registrazione con un provider esterno

Per autenticare gli utenti con le credenziali di un provider esterno, è necessario registrare il sito web con il provider. Quando si registra il sito, si riceverà i parametri (ad esempio una chiave o l'id e segreto) in modo da includere durante la registrazione del client. È necessario un account con i provider che si desidera utilizzare.

In questa esercitazione non Mostra tutti i passaggi da eseguire per la registrazione con tali provider. I passaggi non sono in genere difficile. Per registrare correttamente il sito, seguire le istruzioni fornite in questi siti. Per iniziare con la registrazione del sito, vedere il sito per sviluppatori per:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Quando si registra il sito con Facebook, è possibile fornire &quot;localhost&quot; per il dominio del sito e `&quot;http://localhost/&quot;` per l'URL, come illustrato nell'immagine seguente. Utilizzando localhost funziona con la maggior parte dei provider, ma attualmente non funziona con il provider di Microsoft. Per il provider di Microsoft, è necessario includere un URL del sito web valido.

![registrazione del sito](using-oauth-providers-with-mvc/_static/image4.png)

Nell'immagine precedente, i valori per l'id dell'app, segreto dell'app e posta elettronica di contatto sono state rimosse. Quando si registra effettivamente il sito, tali valori sarà presenti. È possibile notare i valori per l'id dell'app e chiave privata, perché verranno aggiunti all'applicazione.

## <a name="creating-test-users"></a>Creazione di utenti di test

Se non è importante utilizzando un account Facebook esistente per eseguire il test del sito, è possibile ignorare questa sezione.

È possibile creare facilmente gli utenti di test per l'applicazione all'interno della pagina di gestione di app Facebook. È possibile utilizzare questi account per accedere al sito di test. Per creare gli utenti di test scegliere il **ruoli** collegamento nel riquadro di spostamento a sinistra e la selezione di **crea** collegamento.

![creare utenti di test](using-oauth-providers-with-mvc/_static/image5.png)

Il sito di Facebook crea automaticamente il numero di account di test richiesti.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Aggiunta dell'id applicazione e il segreto dal provider

Ora che hai ricevuto l'id e il segreto da Facebook, tornare al file AuthConfig e aggiungerli come i valori dei parametri. I valori seguenti non sono valori reali.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Accedere con le credenziali esterne

Questo è tutto che è necessario eseguire per abilitare le credenziali esterne nel sito. Eseguire l'applicazione e fare clic sul collegamento di account di accesso nell'angolo superiore destro. Il modello riconosce automaticamente che si sono registrati Facebook come provider e include un pulsante per il provider. Se si registra più provider, un pulsante per ciascuno di essi viene incluso automaticamente.

![account di accesso esterno](using-oauth-providers-with-mvc/_static/image6.png)

In questa esercitazione illustra come personalizzare il registro nei pulsanti per il provider esterni. Per ulteriori informazioni, vedere [personalizzazione dell'interfaccia utente dell'account di accesso quando si utilizza OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Fare clic sul pulsante Facebook per l'accesso con credenziali di Facebook. Quando si seleziona uno dei provider esterni, è reindirizzato a un sito ed richiesto dal servizio di accesso.

La figura seguente mostra la schermata di accesso per Facebook. Viene rilevato che si utilizza l'account Facebook per l'accesso a un sito denominato oauthmvcexample.

![autenticazione di Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Dopo l'accesso con le credenziali di Facebook, una pagina informa l'utente che il sito sarà possibile accedere alle informazioni di base.

![richiedere l'autorizzazione](using-oauth-providers-with-mvc/_static/image8.png)

Dopo aver selezionato **passare all'App**, l'utente deve registrare per il sito. La figura seguente mostra la pagina di registrazione dopo che un utente ha effettuato con le credenziali di Facebook. Il nome utente è in genere precompilato con un nome del provider.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Fare clic su **registrare** per completare la registrazione. Chiudere il browser.

È possibile visualizzare il nuovo account è stato aggiunto al database. In Esplora Server, aprire il **DefaultConnection** database e aprire il **tabelle** cartella.

![tabelle di database](using-oauth-providers-with-mvc/_static/image10.png)

Fare doppio clic su di **UserProfile** tabella e selezionare **Mostra dati tabella**.

![Mostra i dati](using-oauth-providers-with-mvc/_static/image11.png)

Verrà visualizzato il nuovo account che è stato aggiunto. Esaminare i dati in **pagina Web\_OAuthMembership** tabella. Verranno visualizzati ulteriori dati relativi al provider esterno per l'account che appena aggiunto.

Se si desidera abilitare l'autenticazione esterna, termine. Tuttavia, è possibile integrare ulteriormente informazioni dal provider nel nuovo processo di registrazione utente, come illustrato nelle sezioni seguenti.

## <a name="create-models-for-additional-user-information"></a>Creare modelli per le informazioni utente aggiuntive

Come evidenziato nelle sezioni precedenti, non è necessario recuperare informazioni aggiuntive per la registrazione dell'account predefinito funzionare. Tuttavia, provider esterni più passare nuovamente ulteriori informazioni sull'utente. Nelle sezioni seguenti viene illustrato come mantenere tali informazioni e salvarlo in un database. In particolare, verranno mantenuti i valori per nome completo dell'utente, l'URI della pagina web personale dell'utente e un valore che indica se l'account ha verificato da Facebook.

Si utilizzerà [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) per aggiungere una tabella per archiviare informazioni utente aggiuntive. Si aggiunge la tabella a un database esistente, pertanto è necessario innanzitutto creare uno snapshot del database corrente. Creando uno snapshot del database corrente, è possibile creare in un secondo momento una migrazione che contiene solo la nuova tabella. Per creare uno snapshot del database corrente:

1. Aprire il **Console di gestione pacchetti**
2. Eseguire il comando **enable-migrations**
3. Eseguire il comando **IgnoreChanges migrazione aggiungere iniziale:**
4. Eseguire il comando **Aggiorna database**

A questo punto, aggiungere le nuove proprietà. Nella cartella Models, aprire il file AccountModels.cs e trovare la classe RegisterExternalLoginModel. La classe RegisterExternalLoginModel contiene valori che provengono da provider di autenticazione. Aggiungere proprietà denominata FullName e collegamento, come evidenziato qui sotto.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Anche in AccountModels.cs, aggiungere una nuova classe denominata ExtraUserInformation. Questa classe rappresenta la nuova tabella che verrà creata nel database.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Nella classe UsersContext, aggiungere il codice evidenziato di seguito per creare una proprietà DbSet per la nuova classe.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

A questo punto si è pronti a creare la nuova tabella. Aprire la Console di gestione pacchetti nuovamente e questa volta:

1. Eseguire il comando **AddExtraUserInformation aggiungere-migrazione**
2. Eseguire il comando **Aggiorna database**

La nuova tabella ora esiste nel database.

## <a name="retrieve-the-additional-data"></a>Recuperare i dati aggiuntivi

Esistono due modi per recuperare i dati utente aggiuntivi. La prima consiste nel conservare i dati utente che viene passati nuovamente, per impostazione predefinita, durante la richiesta di autenticazione. Il secondo modo consiste in particolare, chiamare l'API del provider e richiedere altre informazioni. I valori per nome completo e collegamento vengono passati automaticamente indietro di Facebook. Viene recuperato un valore che indica se l'account ha verificato da Facebook tramite una chiamata all'API di Facebook. Innanzitutto, verranno inseriti i valori per nome completo e di collegamento e quindi in un secondo momento, si otterrà il valore verificato.

Per recuperare i dati utente aggiuntive, aprire il **AccountController.cs** file nel **controller** cartella.

Questo file contiene la logica per la registrazione, la registrazione e la gestione degli account. Notare in particolare, i metodi chiamati **ExternalLoginCallback** e **ExternalLoginConfirmation**. All'interno di questi metodi, è possibile aggiungere codice per personalizzare le operazioni di accesso esterno per l'applicazione. La prima riga del **ExternalLoginCallback** metodo contiene:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Altri dati utente sono passati nel **ExtraData** proprietà del **AuthenticationResult** oggetto restituito dal **VerifyAuthentication** metodo. Il client di Facebook contiene i seguenti valori di **ExtraData** proprietà:

- ID
- name
- collegamento
- sesso
- accesstoken

Altri provider avrà nella proprietà ExtraData dati simili, ma leggermente diversi.

Se l'utente è di nuovo al sito, si recupererà alcuni dati aggiuntivi e passare alla visualizzazione di conferma che i dati. L'ultimo blocco di codice nel metodo viene eseguito solo se l'utente è una novità per il sito. Sostituire la riga seguente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Con la riga seguente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Questa modifica include solo i valori delle proprietà FullName e collegamento.

Nel **ExternalLoginConfirmation** (metodo), modificare il codice evidenziato seguente per salvare le informazioni utente aggiuntive.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Regolazione della visualizzazione

I dati utente aggiuntivi che è recuperare dal provider di verranno visualizzati nella pagina di registrazione.

Nel **viste**/**Account** cartella, aprire **ExternalLoginConfirmation.cshtml**. Sotto il campo esistente per il nome utente, aggiungere i campi per FullName, collegamento e PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Si è ora quasi pronti per eseguire l'applicazione e registrare un nuovo utente con le informazioni aggiuntive salvate. È necessario disporre di un account che non è già registrato con il sito. È possibile utilizzare un account di test diverso o eliminare le righe di **UserProfile** e **pagine Web\_OAuthMembership** tabelle per l'account che si desidera riutilizzare. Eliminando le righe, si garantisce che l'account viene registrato di nuovo.

Eseguire l'applicazione e registrare il nuovo utente. Si noti che questa volta la pagina di conferma contiene più valori.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Dopo aver completato la registrazione, chiudere il browser. Nel database per rilevare nuovi valori di **ExtraUserInformation** tabella.

## <a name="install-nuget-package-for-facebook-api"></a>Installare il pacchetto NuGet per l'API di Facebook

Facebook fornisce un [API](https://developers.facebook.com/docs/reference/apis/) che è possibile chiamare per eseguire operazioni. È possibile chiamare l'API di Facebook indirizzando le richieste di invio HTTP o usando l'installazione di un pacchetto NuGet che facilita l'invio di tali richieste. Utilizzo di un pacchetto NuGet è indicato in questa esercitazione, ma l'installazione di NuGet pacchetto non è essenziale. In questa esercitazione viene illustrato come utilizzare il pacchetto C# SDK di Facebook. Sono presenti altri pacchetti NuGet che semplificano le chiama l'API di Facebook.

Dal **Gestisci pacchetti NuGet** windows, selezionare il pacchetto C# SDK di Facebook.

![installare il pacchetto](using-oauth-providers-with-mvc/_static/image13.png)

Utilizzare il SDK di Facebook C# per chiamare un'operazione che richiede il token di accesso per l'utente. La sezione successiva viene illustrato come ottenere il token di accesso.

## <a name="retrieve-access-token"></a>Recuperare i token di accesso

Provider esterni più passare nuovamente un token di accesso dopo aver verificate le credenziali dell'utente. Questo token di accesso è molto importante perché consente di chiamare le operazioni che sono disponibili solo per gli utenti autenticati. Per recuperare e archiviare il token di accesso, pertanto, è essenziale quando si desidera fornire ulteriori funzionalità.

A seconda del provider esterno, il token di accesso potrebbe essere valido solo per un limitato periodo di tempo. Per verificare di disporre di un token di accesso valido, si recupererà, ogni volta che l'utente esegue l'accesso e archiviarle come un valore di sessione anziché salvarlo in un database.

Nel **ExternalLoginCallback** (metodo), il token di accesso verrà passato nuovamente il **ExtraData** proprietà del **AuthenticationResult** oggetto. Aggiungere il codice evidenziato in **ExternalLoginCallback** per salvare il token di accesso nel **sessione** oggetto. Questo codice viene eseguito ogni volta che l'utente accede con un account Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Sebbene in questo esempio recupera un token di accesso da Facebook, è possibile recuperare il token di accesso da qualsiasi provider esterno tramite la stessa chiave denominata &quot;accesstoken&quot;.

## <a name="logging-off"></a>Disconnessione

Il valore predefinito **disconnessione** metodo consente all'utente dall'applicazione, ma non registra tale utente dal provider esterno. Per l'accesso dell'utente da Facebook e impedire che il token di persistenza dopo che l'utente è disconnesso, è possibile aggiungere il codice evidenziato di seguito per il **disconnessione** metodo il AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Il valore fornito nel `next` parametro è l'URL da utilizzare dopo la disconnessione da Facebook. Durante il test nel computer locale, fornire il numero di porta corretto per il sito locale. Nell'ambiente di produzione, fornire una pagina predefinita, ad esempio contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Recuperare le informazioni dell'utente che richiede il token di accesso

Caso in cui è archiviato il token di accesso sia installato il pacchetto C# SDK Facebook, è possibile usarli insieme per richiedere all'utente informazioni aggiuntive provenienti da Facebook. Nel **ExternalLoginConfirmation** (metodo), creare un'istanza del **FacebookClient** classe passando il valore del token di accesso. Il valore della richiesta di **verificato** proprietà per l'utente autenticato corrente. Il **verificato** proprietà indica se l'account tramite altri mezzi, ad esempio l'invio di un messaggio a un telefono cellulare è convalidato da Facebook. Salvare questo valore nel database.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Occorrerà nuovamente a eliminare i record nel database per l'utente o utilizzare un account Facebook diverso.

Eseguire l'applicazione e registrare il nuovo utente. Esaminare il **ExtraUserInformation** tabella per visualizzare il valore della proprietà verificato.

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stato creato un sito che si integra con Facebook per l'autenticazione utente e i dati di registrazione. State illustrate le impostazioni predefinite che sono configurata per l'applicazione web MVC 4 e come personalizzare il comportamento predefinito.

## <a name="related-topics"></a>Argomenti correlati

- [Creare un'applicazione MVC ASP.NET con autenticazione e il database di SQL Server e distribuire in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
