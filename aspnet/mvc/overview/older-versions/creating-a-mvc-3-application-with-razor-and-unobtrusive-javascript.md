---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creazione di un MVC 3 Application with Razor and Unobtrusive JavaScript | Microsoft Docs
author: microsoft
description: L'applicazione web di esempio elenco utenti viene illustrato quanto sia semplice creare applicazioni ASP.NET MVC 3 con il motore di visualizzazione Razor. Gli oggetti di applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 39ed35c1b7d5c702ffea6908daeac5ca12f1693e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398013"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Creazione di un MVC 3 Application with Razor and Unobtrusive JavaScript
====================
da [Microsoft](https://github.com/microsoft)

> L'applicazione web di esempio elenco utenti viene illustrato quanto sia semplice creare applicazioni ASP.NET MVC 3 con il motore di visualizzazione Razor. L'applicazione di esempio viene illustrato come usare il nuovo motore di visualizzazione Razor con ASP.NET MVC versione 3 e Visual Studio 2010 per creare un sito Web di elenco di utenti fittizi che include funzionalità quali la creazione, visualizzazione, modifica e l'eliminazione degli utenti.
> 
> Questa esercitazione vengono descritti i passaggi che sono stati eseguiti per compilare l'applicazione ASP.NET MVC 3 esempio di elenco degli utenti. Un progetto di Visual Studio con c# e VB al codice sorgente è disponibile a complemento di questo argomento: [scaricare](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Se hai domande su questa esercitazione, è possibile pubblicarle per il [forum MVC](https://forums.asp.net/1146.aspx).


## <a name="overview"></a>Panoramica

L'applicazione che sarà compilata è un sito Web di elenco semplice di utente. Gli utenti possono immettere, visualizzare e aggiornare le informazioni utente.

![Sito di esempio](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

È possibile scaricare il progetto completato VB e c# [qui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Creazione dell'applicazione Web

Per avviare l'esercitazione, aprire Visual Studio 2010 e creare un nuovo progetto usando il *applicazione Web ASP.NET MVC 3* modello. Denominare l'applicazione &quot;Mvc3Razor&quot;.

[![Nuovo progetto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Nel **nuovo progetto ASP.NET MVC 3** finestra di dialogo, seleziona **applicazione Internet**, selezionare il motore di visualizzazione Razor e quindi fare clic su **OK**.

![Finestra di dialogo Nuovo progetto ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

In questa esercitazione non si utilizzerà il provider di appartenenze ASP.NET, pertanto è possibile eliminare tutti i file associati con account di accesso e appartenenza. Nelle **Esplora soluzioni**, rimuovere i file e le directory seguenti:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (e tutti i file in questa directory)

![Exp soln](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Modificare il  <em>\_layout. cshtml</em> file e sostituire il markup all'interno di `<div>` elemento denominato `logindisplay` con il messaggio <em>&quot;</em>account di accesso disabilitato&quot;. L'esempio seguente illustra il nuovo tag:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Aggiunta del modello

In **Esplora soluzioni**, fare doppio clic sul *modelli* cartella, selezionare **Add**, quindi fare clic su **classe**.

![Nuova classe Mdl utente](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Assegnare alla classe il nome `UserModel`. Sostituire il contenuto del *UserModel* file con il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

Il `UserModel` classe rappresenta gli utenti. Ogni membro della classe è annotato con il [necessari](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) dell'attributo dalle [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi. Gli attributi nel [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi forniscono la convalida automatica client e lato server per le applicazioni web.

Aprire il `HomeController` classi e aggiungere un `using` direttiva in modo da poter accedere il `UserModel` e `Users` classi:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Subito dopo il `HomeController` dichiarazione, aggiungere il seguente commento e il riferimento a un `Users` classe:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

Il `Users` classe è un archivio dati semplificati, in memoria che verrà usato in questa esercitazione. In un'applicazione reale si utilizzerebbe un database per archiviare informazioni utente. Le prime righe del `HomeController` file vengono visualizzati nell'esempio seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compilare l'applicazione in modo che il modello di utente saranno disponibile per la procedura guidata lo scaffolding nel passaggio successivo.

## <a name="creating-the-default-view"></a>Creazione della visualizzazione predefinita

Il passaggio successivo consiste nell'aggiungere un metodo di azione e una visualizzazione per visualizzare gli utenti.

Eliminare la chiave esistente *Views\Home\Index* file. Si creerà una nuova *indice* file da visualizzare agli utenti.

Nel `HomeController` classe, sostituire il contenuto del `Index` metodo con il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Pulsante destro del mouse all'interno di `Index` metodo e quindi fare clic su **Aggiungi visualizzazione**.

![Aggiungi visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Selezionare il **creare una visualizzazione fortemente tipizzata** opzione. Per la **visualizzare i dati classe**, selezionare **Mvc3Razor.Models.UserModel**. (Se non viene visualizzata **Mvc3Razor.Models.UserModel** nel **visualizzare dati classe** casella, è necessario compilare il progetto.) Assicurarsi che il motore di visualizzazione è impostato su **Razor**. Impostare **visualizzare il contenuto** al **elenco** e quindi fare clic su **Add**.

![Aggiungi visualizzazione Index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nuova vista esegue automaticamente scaffolding dei dati utente che viene passati per il `Index` vista. Esaminare appena generato *Views\Home\Index* file. Il **Crea nuovo**, **Edit**, **dettagli**, e **Elimina** i collegamenti non funzionano, ma il resto della pagina è funzionale. Eseguire la pagina. Viene visualizzato un elenco di utenti.

![Pagina di indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Aprire il *index. cshtml* del file e sostituire il `ActionLink` markup per **modificare**, **dettagli**, e **eliminare** con il codice seguente :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Il nome utente è utilizzato come ID per trovare il record selezionato nel **Edit**, **dettagli**, e **Elimina** collegamenti.

## <a name="creating-the-details-view"></a>Creazione della visualizzazione dei dettagli

Il passaggio successivo consiste nell'aggiungere un `Details` metodo di azione e visualizzazione per visualizzare i dettagli dell'utente.

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Aggiungere il codice seguente `Details` metodo al controller home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Pulsante destro del mouse all'interno di `Details` metodo e quindi selezionare <strong>Aggiungi visualizzazione</strong>. Verificare che il <strong>visualizzare i dati classe</strong> finestra contiene <strong>Mvc3Razor.Models.UserModel</strong><em>.</em> Impostare <strong>visualizzare il contenuto</strong> al <strong>dettagli</strong> e quindi fare clic su <strong>Add</strong>.

![Aggiungi visualizzazione dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Eseguire l'applicazione e selezionare un collegamento details. Lo scaffolding automatico illustra ogni proprietà nel modello.

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Creazione della visualizzazione di modifica

Aggiungere il codice seguente `Edit` metodo al controller home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Aggiunge una visualizzazione come nei passaggi precedenti, ma imposta **visualizzare il contenuto** al **modificare**.

![Aggiungere una visualizzazione di modifica](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Eseguire l'applicazione e modificare il nome e cognome di uno degli utenti. In caso di qualsiasi violazione `DataAnnotation` vincoli che sono stati applicati per il `UserModel` (classe), quando si invia il form, si visualizzeranno gli errori di convalida generati dal codice server. Ad esempio, se si modifica il nome &quot;Ann&quot; al &quot;oggetto&quot;, quando si invia il form, viene visualizzato l'errore seguente nel form:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

In questa esercitazione, si sta considerando il nome utente come chiave primaria. Pertanto, la proprietà del nome utente non può essere modificata. Nel *Edit. cshtml* nel file, subito dopo il `Html.BeginForm` istruzione, impostare il nome utente sia un campo nascosto. In questo modo la proprietà deve essere passato nel modello. Il frammento di codice seguente viene illustrata la posizione del `Hidden` istruzione:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Sostituire il `TextBoxFor` e `ValidationMessageFor` markup per il nome utente con un `DisplayFor` chiamare. Il `DisplayFor` metodo visualizza la proprietà come elemento di sola lettura. Nell'esempio seguente viene illustrato il markup completato. Originale `TextBoxFor` e `ValidationMessageFor` come commento le chiamate con i caratteri di commento begin ed end commento Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Abilitazione della convalida lato Client

Per abilitare la convalida lato client in ASP.NET MVC 3, è necessario impostare due flag ed è necessario includere tre file JavaScript.

Aprire l'applicazione *Web. config* file. Verificare `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` sono impostati su true nelle impostazioni dell'applicazione. Il frammento seguente dalla radice *Web. config* file Mostra le impostazioni corrette:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Impostazione `UnobtrusiveJavaScriptEnabled` su true, Abilita unobtrusive Ajax e la convalida del client non intrusivo. Quando si usa la convalida discreta, le regole di convalida vengono convertite in attributi HTML5. I nomi degli attributi HTML5 può contenere solo lettere minuscole, numeri e trattini.

Impostazione `ClientValidationEnabled` su true consente la convalida lato client. Tramite l'impostazione di queste chiavi nell'applicazione *Web. config* file, si abilita la convalida del client e JavaScript non intrusivo per l'intera applicazione. È anche possibile abilitare o disabilitare queste impostazioni nelle singole viste o i metodi del controller utilizzando il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

È anche necessario includere numerosi file JavaScript nella visualizzazione sottoposta a rendering. È un modo semplice per includere il codice JavaScript in tutte le visualizzazioni per aggiungerli al *Views\Shared\\layout. cshtml* file. Sostituire il `<head>` dell'elemento di  *\_layout. cshtml* file con il codice seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

I primi due script jQuery sono ospitati dal contenuto Delivery Network (rete CDN Microsoft Ajax). Grazie all'uso della rete CDN Microsoft Ajax, è possibile migliorare significativamente le prestazioni prima richiesta delle applicazioni.

Eseguire l'applicazione e fare clic su un collegamento di modifica. Visualizzare il codice sorgente nel browser. L'origine del browser Mostra molti attributi nel formato `data-val` (convalida dei dati). Quando la convalida del client e JavaScript non intrusivo è abilitato, i campi di input con una regola di convalida client contengono il `data-val="true"` attributo per attivare la convalida del client non intrusivo. Ad esempio, il `City` campo del modello è stata decorata con il [obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attributo, che genera il codice HTML mostrato nell'esempio seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Per ogni regola di convalida client, viene aggiunto un attributo che ha il formato `data-val-rulename="message"`. Usando il `City` campo, ad esempio illustrato in precedenza, la regola di convalida client richiesta genera il `data-val-required` attributo e il messaggio &quot;City il campo è obbligatorio&quot;. Eseguire l'applicazione, modificare uno degli utenti e deselezionare il `City` campo. Quando si preme tab fuori dal campo, viene visualizzato un messaggio di errore di convalida lato client.

![Città obbligati](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Analogamente, per ogni parametro nella regola di convalida client, viene aggiunto un attributo che ha il formato `data-val-rulename-paramname=paramvalue`. Ad esempio, il `FirstName` proprietà è annotata con il [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attributo e specifica una lunghezza minima pari a 3 e una lunghezza massima di 8. La regola di convalida di dati denominata `length` ha il nome del parametro `max` e il valore del parametro 8. Di seguito è riportato il codice HTML generato per il `FirstName` campo quando si modifica uno degli utenti:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Per altre informazioni sulla convalida del client non intrusivi, vedere l'intervento [Unobtrusive Validation di Client in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) nel blog di Brad Wilson.

> [!NOTE]
> Nella versione Beta di ASP.NET MVC 3, è talvolta necessario inviare il modulo per avviare la convalida lato client. Ciò potrebbe essere modificato nella versione finale.


## <a name="creating-the-create-view"></a>Creazione della visualizzazione Create

Il passaggio successivo consiste nell'aggiungere un `Create` metodo di azione e visualizzazione per consentire all'utente di creare un nuovo utente. Aggiungere il codice seguente `Create` metodo al controller home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Aggiunge una visualizzazione come nei passaggi precedenti, ma imposta **visualizzare il contenuto** al **crea**.

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Eseguire l'applicazione, selezionare la **Create** collegare e aggiungere un nuovo utente. Il `Create` metodo sfrutta automaticamente la convalida lato client e lato server. Provare a immettere un nome utente che contiene gli spazi vuoti, ad esempio &quot;Ben X&quot;. Quando si esce dal campo nome utente, un errore di convalida lato client (`White space is not allowed`) viene visualizzato.

## <a name="add-the-delete-method"></a>Aggiungere il metodo Delete

Per completare l'esercitazione, aggiungere il codice seguente `Delete` metodo al controller home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Aggiungere un `Delete` vista come nei passaggi precedenti, l'impostazione **visualizzare il contenuto** al **eliminare**.

![Elimina visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Ora disponibile un'applicazione di ASP.NET MVC 3 semplice seppur completamente funzionale con la convalida.
