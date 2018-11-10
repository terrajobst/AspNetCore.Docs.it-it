---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Uso di provider OAuth con MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come compilare un'applicazione web ASP.NET MVC 4 che consente agli utenti di accedere con le credenziali da un provider esterno, ad esempio Facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: d0203b62c911056fc56ed103c1c42f67816cbbf0
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021755"
---
<a name="using-oauth-providers-with-mvc-4"></a>Uso di provider OAuth con MVC 4
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come compilare un'applicazione web ASP.NET MVC 4 che consente agli utenti di accedere con le credenziali da un provider esterno, ad esempio Facebook, Twitter, Microsoft o Google e quindi integrare alcune delle funzionalità di tali provider di applicazione Web. Per semplicità, questa esercitazione è incentrata sull'uso di credenziali da Facebook.
> 
> Per usare le credenziali esterne in un'applicazione web ASP.NET MVC 5, vedere [creare un'App ASP.NET MVC 5 con Facebook e Google OAuth2 Sign-on OpenID](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Abilitazione di queste credenziali nei siti web offre un vantaggio significativo perché milioni di utenti hanno già account con questi provider esterni. Questi utenti potrebbero essere più inclini a effettuare l'iscrizione per il sito se non dispongono creare e tenere presente un nuovo insieme di credenziali. Inoltre, dopo che un utente ha effettuato l'accesso tramite uno di questi provider, è possibile incorporare le operazioni di social media dal provider.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

In questa esercitazione sono disponibili due obiettivi principali:

1. Consentire agli utenti di accedere con le credenziali da un provider OAuth.
2. Recuperare le informazioni sull'account del provider e integrare tali informazioni con la registrazione dell'account per il sito.

Anche se gli esempi in questa esercitazione è incentrata sull'uso di Facebook come provider di autenticazione, è possibile modificare il codice per usare uno qualsiasi dei provider. I passaggi per implementare qualsiasi provider sono molto simili ai passaggi che verrà visualizzato in questa esercitazione. Si noterà solo differenze significative quando si effettuano chiamate dirette all'API del provider impostato.

## <a name="prerequisites"></a>Prerequisiti

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) o [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Or

- Microsoft Visual Studio 2010 SP1 o [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Inoltre, in questo argomento si presuppone la knowledge base su ASP.NET MVC e Visual Studio. Se è necessaria un'introduzione ad ASP.NET MVC 4, vedere [Introduzione ad ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Creare il progetto

In Visual Studio, creare una nuova applicazione Web ASP.NET MVC 4 e denominarla &quot;OAuthMVC&quot;. È possibile scegliere .NET Framework 4.5 o 4.

![Crea progetto](using-oauth-providers-with-mvc/_static/image1.png)

Nella finestra Nuovo progetto ASP.NET MVC 4, selezionare **applicazione Internet** lasciando **Razor** come il motore di visualizzazione.

![Selezionare l'applicazione Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Abilitare un provider

Quando si crea un'applicazione web MVC 4 con il modello di applicazione Internet, il progetto viene creato con un file denominato AuthConfig.cs nell'App\_cartella di avvio.

![File AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Il file AuthConfig contiene codice per registrare i client per i provider di autenticazione esterni. Per impostazione predefinita, questo codice viene commentato, pertanto nessuno dei provider esterni sono abilitati.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

È necessario rimuovere il commento di questo codice per usare il client di autenticazione esterni. Si rimuove il commento solo i provider che si desidera includere nel proprio sito. Per questa esercitazione, si abiliterà solo le credenziali di Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Si noti che nell'esempio precedente che il metodo include stringhe vuote per i parametri di registrazione. Se si prova a eseguire l'applicazione a questo punto, l'applicazione genera un'eccezione di argomento in quanto le stringhe vuote non sono consentite per i parametri. Per fornire i valori validi, è necessario registrare il sito web con i provider esterni, come illustrato nella sezione successiva.

## <a name="registering-with-an-external-provider"></a>La registrazione con un provider esterno

Per autenticare gli utenti con le credenziali di un provider esterno, è necessario registrare il sito web con il provider. Quando si registra il sito, si riceverà i parametri (ad esempio chiave o id e segreto) in modo da includere durante la registrazione del client. È necessario un account con i provider da usare.

Questa esercitazione non include tutti i passaggi da eseguire per registrare con tali provider. I passaggi non sono in genere difficile. Per registrare correttamente il sito, seguire le istruzioni fornite in questi siti. Per iniziare a usare la registrazione del sito, vedere il sito per sviluppatori per:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Quando si registra il sito con Facebook, è possibile fornire &quot;localhost&quot; per il dominio del sito e `&quot; http://localhost/&quot;` per l'URL, come illustrato nell'immagine seguente. Tramite localhost funziona con la maggior parte dei provider, ma attualmente non funziona con provider Microsoft. Per il provider di Microsoft, è necessario includere un URL del sito web valido.

![registrazione del sito](using-oauth-providers-with-mvc/_static/image4.png)

Nell'immagine precedente, i valori per l'id dell'app, segreto dell'app e posta elettronica di contatto sono state rimosse. Quando si registra in realtà il sito, tali valori saranno presenti. È opportuno notare i valori di id app e segreto dell'app poiché essi verranno aggiunte all'applicazione.

## <a name="creating-test-users"></a>Creazione di utenti di test

Se non è importante usando un account esistente di Facebook per testare il sito, è possibile ignorare questa sezione.

È possibile creare con facilità gli utenti di test per l'applicazione all'interno della pagina di gestione di app Facebook. È possibile usare questi account per accedere al sito di test. Per creare gli utenti di test scegliere il **ruoli** collegamento nel riquadro di spostamento a sinistra e scegliendo il **crea** collegamento.

![creare utenti di test](using-oauth-providers-with-mvc/_static/image5.png)

Il sito di Facebook crea automaticamente il numero di account di test richiesti.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Aggiunta di id applicazione e il segreto dal provider

Ora che hai ricevuto l'id e il segreto da Facebook, tornare al file AuthConfig e aggiungerle come i valori dei parametri. I valori riportati di seguito non sono valori reali.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Accedere con le credenziali esterne

Questo è tutto che è necessario eseguire per abilitare le credenziali esterne nel sito. Eseguire l'applicazione e fare clic sul collegamento di accesso nell'angolo superiore destro. Il modello riconosce automaticamente che si sono registrati Facebook come provider e include un pulsante per il provider. Se si registra più provider, un pulsante per ciascuno di essi è incluso automaticamente.

![account di accesso esterno](using-oauth-providers-with-mvc/_static/image6.png)

Questa esercitazione non illustra come personalizzare il log nei pulsanti per i provider esterni. Per ulteriori informazioni, vedere [personalizzare l'interfaccia utente di accesso quando si usa OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Fare clic sul pulsante di Facebook per accedere con le credenziali di Facebook. Quando si seleziona uno dei provider esterno, sono reindirizzati a tale sito e viene richiesto da tale servizio per l'accesso.

L'immagine seguente mostra la schermata di accesso per Facebook. Osserva che si usa l'account di Facebook per l'accesso a un sito denominato oauthmvcexample.

![autenticazione di Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Dopo l'accesso con le credenziali di Facebook, una pagina informa l'utente che il sito sarà possibile accedere a informazioni di base.

![richiedere l'autorizzazione](using-oauth-providers-with-mvc/_static/image8.png)

Dopo aver selezionato **passare all'App**, l'utente deve registrare per il sito. L'immagine seguente mostra la pagina di registrazione dopo che un utente ha eseguito l'accesso con le credenziali di Facebook. Il nome utente è in genere precompilato con un nome del provider.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Fare clic su **registrare** per completare la registrazione. Chiudere il browser.

È possibile visualizzare il nuovo account è stato aggiunto al database. In Esplora Server aprire il **DefaultConnection** del database e aprire il **tabelle** cartella.

![tabelle di database](using-oauth-providers-with-mvc/_static/image10.png)

Fare doppio clic il **UserProfile** tabelle e selezionare **Mostra dati tabella**.

![visualizzare i dati](using-oauth-providers-with-mvc/_static/image11.png)

Verrà visualizzato il nuovo account che è stato aggiunto. Esaminare i dati in **pagina Web\_OAuthMembership** tabella. Verranno visualizzati più dati relativi al provider esterno per l'account che appena aggiunto.

Se si desidera solo consentire l'autenticazione esterna, termine. Tuttavia, è possibile integrare ulteriormente informazioni dal provider nel processo di registrazione utente nuove, come illustrato nelle sezioni seguenti.

## <a name="create-models-for-additional-user-information"></a>Creare modelli per informazioni aggiuntive sull'utente

Come evidenziato nelle sezioni precedenti, non occorre recuperare le informazioni aggiuntive per la registrazione di account predefinito da usare. Tuttavia, provider più esterni passare nuovamente ulteriori informazioni sull'utente. Le sezioni seguenti illustrano come mantenere tali informazioni e salvarlo in un database. In particolare, verranno mantenuti i valori per nome completo dell'utente, l'URI della pagina web personale dell'utente e un valore che indica se l'account è verificato da Facebook.

Si userà [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) per aggiungere una tabella per archiviare informazioni utente aggiuntive. Si aggiunge la tabella a un database esistente, pertanto è necessario innanzitutto creare uno snapshot del database corrente. Creando uno snapshot del database corrente, è possibile creare in un secondo momento una migrazione che contiene solo la nuova tabella. Per creare uno snapshot del database corrente:

1. Aprire il **Console di gestione pacchetti**
2. Eseguire il comando **enable-migrations**
3. Eseguire il comando **IgnoreChanges migrazione aggiungere iniziale:**
4. Eseguire il comando **update-database**

A questo punto, si aggiungerà nuove proprietà. Nella cartella Models, aprire il file AccountModels.cs e trovare la classe RegisterExternalLoginModel. La classe RegisterExternalLoginModel contiene i valori restituiti dal provider di autenticazione. Aggiungere proprietà denominate FullName e collegamento, come evidenziato di seguito.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Anche in AccountModels.cs, aggiungere una nuova classe denominata ExtraUserInformation. Questa classe rappresenta la nuova tabella che verrà creata nel database.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Nella classe UsersContext, aggiungere il codice evidenziato seguente per creare una proprietà DbSet per la nuova classe.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

A questo punto si è pronti creare la nuova tabella. Anche in questo caso e questa volta, aprire la Console di gestione pacchetti:

1. Eseguire il comando **AddExtraUserInformation aggiungere-migrazione**
2. Eseguire il comando **update-database**

Esiste ora la nuova tabella nel database.

## <a name="retrieve-the-additional-data"></a>Recuperare i dati aggiuntivi

Esistono due modi per recuperare i dati utente aggiuntivi. Il primo modo consiste nel mantenere i dati utente che viene passati nuovamente, per impostazione predefinita, durante la richiesta di autenticazione. Il secondo consiste nel chiamare l'API del provider in modo specifico e richiedere ulteriori informazioni. I valori di proprietà FullName e collegamento vengono passati automaticamente indietro di Facebook. Un valore che indica se Facebook ha verificato l'account viene recuperato tramite una chiamata all'API Facebook. In primo luogo, si popoleranno i valori per nome completo e il collegamento e quindi in un secondo momento, si otterrà il valore verificato.

Per recuperare i dati utente aggiuntivi, aprire il **AccountController.cs** del file nei **controller** cartella.

Questo file contiene la logica per la registrazione, la registrazione e la gestione degli account. In particolare, si noti che i metodi chiamati **ExternalLoginCallback** e **ExternalLoginConfirmation**. All'interno di questi metodi, è possibile aggiungere codice per personalizzare le operazioni di accesso esterno per l'applicazione. La prima riga del **ExternalLoginCallback** metodo contiene:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Dati utente aggiuntivi viene restituiti il **ExtraData** proprietà del **AuthenticationResult** oggetto restituito dal **VerifyAuthentication** (metodo). Il client di Facebook contiene i seguenti valori di **ExtraData** proprietà:

- ID
- name
- collegamento
- Sesso
- accesstoken

Altri provider avranno i dati simili, ma leggermente diversi nella proprietà ExtraData.

Se l'utente è nuovo al sito, si recupererà alcuni dati aggiuntivi e passare tali dati per la visualizzazione di conferma. L'ultimo blocco di codice nel metodo viene eseguito solo se l'utente è nuovo al sito. Sostituire la riga seguente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Con la riga seguente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Questa modifica include semplicemente i valori per le proprietà FullName e collegamento.

Nel **ExternalLoginConfirmation** metodo, modificare il codice evidenziato seguente per salvare le informazioni utente aggiuntive.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Regolazione della visualizzazione

I dati utente aggiuntivi che è recuperare dal provider verranno visualizzati nella pagina di registrazione.

Nel **viste**/**Account** cartella, aprire **ExternalLoginConfirmation.cshtml**. Sotto il campo esistente per il nome utente, aggiungere campi per nome completo, collegamento e PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Sei ora quasi pronti per eseguire l'applicazione e registrare un nuovo utente con le informazioni aggiuntive salvate. È necessario un account che non è già registrato con il sito. È possibile usare un account di test diversi, o eliminare le righe nel **UserProfile** e **pagine Web\_OAuthMembership** tabelle per l'account che si desidera riutilizzare. Eliminando le righe, si garantirà che l'account è registrato anche in questo caso.

Eseguire l'applicazione e registrare il nuovo utente. Si noti che questa volta la pagina di conferma contiene più valori.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Dopo aver completato la registrazione, chiudere il browser. Cerca nel database per rilevare i nuovi valori nel **ExtraUserInformation** tabella.

## <a name="install-nuget-package-for-facebook-api"></a>Installare il pacchetto NuGet per l'API di Facebook

Facebook offre un' [API](https://developers.facebook.com/docs/reference/apis/) che è possibile chiamare per eseguire operazioni. È possibile chiamare l'API di Facebook, indirizzando le richieste di invio HTTP o usando l'installazione di un pacchetto NuGet che semplifica l'invio di tali richieste. Usando un pacchetto NuGet è illustrato in questa esercitazione, ma l'installazione di NuGet pacchetto non è essenziale. Questa esercitazione illustra come usare il pacchetto C# SDK di Facebook. Esistono altri pacchetti NuGet che agevolano la chiamata dell'API Facebook.

Dal **Gestisci pacchetti NuGet** windows, selezionare il pacchetto C# SDK di Facebook.

![installare il pacchetto](using-oauth-providers-with-mvc/_static/image13.png)

Si utilizzerà il SDK di Facebook C# per chiamare un'operazione che richiede il token di accesso per l'utente. Nella sezione successiva mostra come ottenere il token di accesso.

## <a name="retrieve-access-token"></a>Recuperare i token di accesso

Provider esterni più passare di nuovo un token di accesso dopo aver verificate le credenziali dell'utente. Questo token di accesso è molto importante perché consente di chiamare le operazioni che sono disponibili solo per gli utenti autenticati. Pertanto, il recupero e l'archiviazione dei token di accesso è essenziale quando si desidera fornire ulteriori funzionalità.

A seconda del provider esterno, il token di accesso potrebbe essere valido per solo un periodo di tempo limitato. Per assicurarsi di avere un token di accesso valido, verranno recuperati, ogni volta che l'utente effettua l'accesso e archiviarle come un valore di sessione anziché salvarlo in un database.

Nel **ExternalLoginCallback** metodo, il token di accesso viene inoltre restituito il **ExtraData** proprietà del **AuthenticationResult** oggetto. Aggiungere il codice evidenziato per **ExternalLoginCallback** per salvare il token di accesso nel **sessione** oggetto. Questo codice viene eseguito ogni volta che l'utente accede con un account di Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Sebbene questo esempio recupera un token di accesso da Facebook, è possibile recuperare il token di accesso da qualsiasi provider esterno tramite la stessa chiave denominata &quot;accesstoken&quot;.

## <a name="logging-off"></a>Disconnessione

Il valore predefinito **LogOff** metodo registra l'utente dall'applicazione, ma non disconnettere l'utente da provider esterno. Per disconnettere l'utente da Facebook e impedire che il token di persistenza dopo che l'utente ha effettuato la disconnessione, è possibile aggiungere il codice evidenziato seguente per il **LogOff** metodo nella finestra di AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Il valore specificato nella `next` parametro è l'URL da usare dopo che l'utente ha effettuato l'accesso all'esterno di Facebook. Durante il test nel computer locale, si fornirebbe il numero di porta corretto per il sito locale. Nell'ambiente di produzione, si fornirebbe una pagina predefinita, ad esempio contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Recuperare le informazioni utente che richiede il token di accesso

Ora che è stato archiviato il token di accesso e installato il pacchetto C# SDK di Facebook, è possibile usarli insieme per richiedere informazioni aggiuntive sull'utente da Facebook. Nel **ExternalLoginConfirmation** metodo, creare un'istanza del **FacebookClient** classe passando il valore del token di accesso. Richiedere il valore della **verificato** proprietà per l'utente autenticato corrente. Il **verificato** proprietà indica se Facebook ha convalidato l'account tramite altri mezzi, ad esempio l'invio di un messaggio a un telefono cellulare. Salvare questo valore nel database.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

È anche in questo caso sarà necessario eliminare i record nel database per l'utente o usare un altro account di Facebook.

Eseguire l'applicazione e registrare il nuovo utente. Esaminare i **ExtraUserInformation** tabella per visualizzare il valore della proprietà Verified.

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stato creato un sito integrato con Facebook per l'autenticazione utente e i dati di registrazione. Sono stati illustrati il comportamento predefinito che è configurato per l'applicazione web MVC 4 e come personalizzare il comportamento predefinito.

## <a name="related-topics"></a>Argomenti correlati

- [Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio App di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
