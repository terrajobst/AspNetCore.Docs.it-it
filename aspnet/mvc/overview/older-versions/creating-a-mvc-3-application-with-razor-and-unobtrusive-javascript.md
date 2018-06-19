---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creazione di un MVC 3 Application with Razor and JavaScript non intrusivo | Documenti Microsoft
author: microsoft
description: L'applicazione web di esempio elenco utenti viene illustrato quanto sia semplice per creare applicazioni ASP.NET MVC 3 con il motore di visualizzazione Razor. S l'applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 9b273f6827cad2078b581d6da7b127198dfddaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30874694"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Creazione di un MVC 3 Application with Razor and JavaScript non intrusivo
====================
by [Microsoft](https://github.com/microsoft)

> L'applicazione web di esempio elenco utenti viene illustrato quanto sia semplice per creare applicazioni ASP.NET MVC 3 con il motore di visualizzazione Razor. L'applicazione di esempio viene illustrato come utilizzare il nuovo motore di visualizzazione Razor con ASP.NET MVC versione 3 e Visual Studio 2010 per creare un sito Web di elenco degli utenti fittizi che include funzionalità, quali la creazione, visualizzazione, modifica ed eliminazione degli utenti.
> 
> In questa esercitazione vengono descritti i passaggi che sono stati eseguiti per compilare l'applicazione ASP.NET MVC 3 di esempio elenco utenti. Un progetto di Visual Studio con c# e VB codice sorgente è disponibile a complemento di questo argomento: [scaricare](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Nel caso di domande su questa esercitazione, post per il [forum MVC](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Panoramica

L'applicazione da generare è un semplice utente elenco sito Web. Gli utenti possono immettere, visualizzare e aggiornare le informazioni utente.

![Sito di esempio](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

È possibile scaricare il progetto completato VB e c# [qui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Creazione dell'applicazione Web

Per avviare l'esercitazione, aprire Visual Studio 2010 e creare un nuovo progetto utilizzando il *applicazione Web di ASP.NET MVC 3* modello. Nome dell'applicazione &quot;Mvc3Razor&quot;.

[![Nuovo progetto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Nel **nuovo progetto ASP.NET MVC 3** finestra di dialogo Seleziona **applicazione Internet**, selezionare il motore di visualizzazione Razor e quindi fare clic su **OK**.

![Finestra di dialogo Nuovo progetto ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

In questa esercitazione non si utilizzerà il provider di appartenenze ASP.NET, pertanto è possibile eliminare tutti i file associati all'accesso e l'appartenenza. In **Esplora**, rimuovere i file e directory seguenti:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (e tutti i file in questa directory)

![Exp soln](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Modificare il  <em>\_cshtml</em> file e sostituire il markup all'interno di `<div>` elemento denominato `logindisplay` con il messaggio <em>&quot;</em>account di accesso disabilitato&quot;. Nell'esempio seguente viene illustrato il markup di nuovo:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Aggiunta del modello

In **Esplora**, fare doppio clic su di *modelli* cartella, selezionare **Aggiungi**e quindi fare clic su **classe**.

![Nuova classe Mdl utente](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Assegnare alla classe il nome `UserModel`. Sostituire il contenuto del *UserModel* file con il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

La `UserModel` classe rappresenta gli utenti. Ogni membro della classe è annotato con il [necessari](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) dall'attributo di [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi. Gli attributi di [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi forniscono la convalida lato client e server automatica per le applicazioni web.

Aprire il `HomeController` classe e aggiungere un `using` direttiva in modo da poter accedere il `UserModel` e `Users` classi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Subito dopo il `HomeController` dichiarazione, aggiungere il seguente commento e il riferimento a un `Users` classe:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

La `Users` classe è un archivio di dati semplificato e in memoria che verrà utilizzato in questa esercitazione. In un'applicazione reale si utilizzerebbe un database per archiviare informazioni utente. Le prime righe del `HomeController` file presenti nell'esempio seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compilare l'applicazione in modo che il modello di utente sarà disponibile per il procedura guidata lo scaffolding nel passaggio successivo.

## <a name="creating-the-default-view"></a>Creazione della visualizzazione predefinita

Il passaggio successivo consiste nell'aggiungere un metodo di azione e la visualizzazione per visualizzare gli utenti.

Eliminare quello esistente *Views\Home\Index* file. Verrà creato un nuovo *indice* file per visualizzare gli utenti.

Nel `HomeController` classe, sostituire il contenuto del `Index` (metodo) con il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Fare doppio clic all'interno di `Index` (metodo) e quindi fare clic su **Aggiungi visualizzazione**.

![Aggiungi visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Selezionare il **creare una visualizzazione fortemente tipizzata** opzione. Per **visualizzare dati classe**selezionare **Mvc3Razor.Models.UserModel**. (Se non viene visualizzato **Mvc3Razor.Models.UserModel** nel **visualizzare dati classe** casella, è necessario compilare il progetto.) Assicurarsi che il motore di visualizzazione è impostato su **Razor**. Impostare **visualizzare il contenuto** a **elenco** e quindi fare clic su **Aggiungi**.

![Aggiungere una visualizzazione dell'indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nuova vista scaffolds automaticamente i dati utente che viene passati per il `Index` visualizzazione. Esaminare appena generato *Views\Home\Index* file. Il **Crea nuovo**, **modifica**, **dettagli**, e **eliminare** collegamenti non funzionano, ma il resto della pagina è funzionale. Eseguire la pagina. Verrà visualizzato un elenco di utenti.

![Pagina di indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Aprire il *cshtml* file e sostituire il `ActionLink` markup per **modifica**, **dettagli**, e **eliminare** con il codice seguente :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Il nome utente è utilizzato come l'ID per trovare il record selezionato nel **modifica**, **dettagli**, e **eliminare** collegamenti.

## <a name="creating-the-details-view"></a>Creazione della visualizzazione dettagli

Il passaggio successivo consiste nell'aggiungere un `Details` metodo di azione e visualizzazione per visualizzare i dettagli dell'utente.

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Aggiungere il seguente `Details` metodo al controller home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Fare doppio clic all'interno di `Details` (metodo) e quindi selezionare <strong>Aggiungi visualizzazione</strong>. Verificare che il <strong>visualizzare dati classe</strong> finestra contiene <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Impostare <strong>visualizzare il contenuto</strong> a <strong>dettagli</strong> e quindi fare clic su <strong>Aggiungi</strong>.

![Aggiungere una visualizzazione dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Eseguire l'applicazione e selezionare un collegamento di dettagli. Lo scaffolding automatico Mostra ogni proprietà nel modello.

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Creazione della visualizzazione di modifica

Aggiungere il seguente `Edit` metodo al controller home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Aggiungere una visualizzazione, come illustrato nei passaggi precedenti, ma impostare **visualizzare il contenuto** a **modifica**.

![Aggiungere una visualizzazione di modifica](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Eseguire l'applicazione e modificare il nome e cognome di uno degli utenti. In caso di violazione qualsiasi `DataAnnotation` vincoli applicati per la `UserModel` (classe), quando si invia il form, si noterà gli errori di convalida generati dal codice server. Ad esempio, se si modifica il nome &quot;Ann&quot; a &quot;A&quot;, quando si invia il form, nel form viene visualizzato l'errore seguente:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

In questa esercitazione, si sta considerando il nome utente come chiave primaria. Pertanto, la proprietà del nome utente non può essere modificata. Nel *Edit.cshtml* file, subito dopo il `Html.BeginForm` istruzione, impostare il nome utente da un campo nascosto. In questo modo la proprietà deve essere passato nel modello. Frammento di codice seguente viene illustrata la posizione del `Hidden` istruzione:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Sostituire il `TextBoxFor` e `ValidationMessageFor` markup per il nome utente con un `DisplayFor` chiamare. Il `DisplayFor` metodo consente di visualizzare la proprietà come un elemento di sola lettura. Nell'esempio seguente viene illustrato il markup completato. Originale `TextBoxFor` e `ValidationMessageFor` chiamate sono impostate come commento con i caratteri di commento begin ed end commento Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Abilitando la convalida lato Client

Per abilitare la convalida lato client in ASP.NET MVC 3, è necessario impostare due flag ed è necessario includere tre file JavaScript.

Aprire l'applicazione *Web. config* file. Verificare `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` sono impostate su true nelle impostazioni dell'applicazione. Nel frammento seguente dalla radice *Web. config* file Mostra le impostazioni corrette:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Impostazione `UnobtrusiveJavaScriptEnabled` su true, Abilita non intrusivo Ajax e la convalida del client non intrusiva. Quando si utilizza la convalida non intrusiva, le regole di convalida vengono convertite in attributi HTML5. I nomi degli attributi HTML5 può contenere solo lettere minuscole, numeri e trattini.

Impostazione `ClientValidationEnabled` su true consente la convalida lato client. Tramite l'impostazione di queste chiavi nell'applicazione *Web. config* file, si abilita la convalida del client e JavaScript non intrusivo per l'intera applicazione. È inoltre possibile abilitare o disabilitare queste impostazioni nelle singole viste o nei metodi controller utilizzando il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

È inoltre necessario includere diversi file JavaScript in una visualizzazione sottoposta a rendering. È un modo semplice per includere il codice JavaScript in tutte le visualizzazioni per aggiungerli al *Views\Shared\\layout. cshtml* file. Sostituire il `<head>` elemento il  *\_cshtml* file con il codice seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

I primi due script jQuery sono ospitati da di Microsoft Ajax rete CDN (Content Delivery). È possibile sfruttare la rete CDN di Microsoft Ajax, è possibile migliorare notevolmente il primo numero di passaggi delle prestazioni delle applicazioni.

Eseguire l'applicazione e fare clic su un collegamento di modifica. Consente di visualizzare l'origine della pagina nel browser. L'origine del browser Mostra molti attributi della maschera `data-val` (per la convalida dei dati). Quando la convalida del client e JavaScript non intrusivo è abilitato, i campi di input con una regola di convalida client contengono il `data-val="true"` attributo per attivare la convalida del client non intrusiva. Ad esempio, il `City` campo nel modello è decorata con il [necessari](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attributo, che comporta il codice HTML mostrato nell'esempio seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Per ogni regola di convalida client, viene aggiunto un attributo che ha il formato `data-val-rulename="message"`. Utilizzando il `City` campo, ad esempio illustrato in precedenza, la regola di convalida client richiesta genera il `data-val-required` attributo e il messaggio &quot;città il campo è obbligatorio&quot;. Eseguire l'applicazione, modificare uno degli utenti e deselezionare il `City` campo. Quando si preme tab fuori dal campo, viene visualizzato un messaggio di errore di convalida lato client.

![Città richiesto](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Analogamente, per ogni parametro nella regola di convalida client, viene aggiunto un attributo che ha il formato `data-val-rulename-paramname=paramvalue`. Ad esempio, il `FirstName` proprietà viene annotata con il [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) degli attributi e specifica una lunghezza minima di 3 e la lunghezza massima di 8. La regola di convalida di dati denominata `length` è il nome del parametro `max` e il valore del parametro 8. Di seguito è riportato il codice HTML generato per il `FirstName` campo quando si modifica uno degli utenti:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Per ulteriori informazioni sulla convalida del client non intrusiva, vedere la voce [non intrusivo la convalida del Client in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) nel blog di Brad Wilson.

> [!NOTE]
> Nella versione Beta di ASP.NET MVC 3, è talvolta necessario inviare il form per avviare la convalida lato client. Questo può essere modificato per la versione finale.


## <a name="creating-the-create-view"></a>Creazione della visualizzazione di creazione

Il passaggio successivo consiste nell'aggiungere un `Create` metodo di azione e visualizzazione per consentire all'utente di creare un nuovo utente. Aggiungere il seguente `Create` metodo al controller home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Aggiungere una visualizzazione, come illustrato nei passaggi precedenti, ma impostare **visualizzare il contenuto** a **crea**.

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Eseguire l'applicazione, selezionare il **crea** collegare e aggiungere un nuovo utente. Il `Create` metodo automaticamente sfrutta la convalida lato client e lato server. Provare a immettere un nome utente che contiene gli spazi vuoti, ad esempio &quot;X Ben&quot;. Quando esce dal campo nome utente, un errore di convalida sul lato client (`White space is not allowed`) viene visualizzato.

## <a name="add-the-delete-method"></a>Aggiungere il metodo Delete

Per completare l'esercitazione, aggiungere il seguente `Delete` metodo al controller home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Aggiungere un `Delete` vista come i passaggi precedenti, l'impostazione **visualizzare il contenuto** a **eliminare**.

![Elimina vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

È ora disponibile un'applicazione ASP.NET MVC 3 semplice ma completamente funzionale con la convalida.
