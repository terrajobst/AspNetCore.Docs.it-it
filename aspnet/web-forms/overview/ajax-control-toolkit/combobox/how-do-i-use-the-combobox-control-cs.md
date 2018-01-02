---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: Utilizzo del controllo ComboBox (C#) | Documenti Microsoft
author: microsoft
description: "Casella combinata è un controllo ASP.NET AJAX che combina la flessibilità di una casella di testo con un elenco di opzioni da cui gli utenti possono scegliere."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7913affb73c1c314944782ff80cf6c5558502ee9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-combobox-control-c"></a>Utilizzo del controllo ComboBox (C#)
====================
da [Microsoft](https://github.com/microsoft)

> Casella combinata è un controllo ASP.NET AJAX che combina la flessibilità di una casella di testo con un elenco di opzioni da cui gli utenti possono scegliere.


L'obiettivo di questa esercitazione è per descrivere il controllo AJAX controllo Toolkit ComboBox. Casella combinata funziona come una combinazione tra un controllo ASP.NET DropDownList standard e un controllo casella di testo. È possibile selezionare da un elenco di elementi di pre-esistente o immettere un nuovo elemento.

Casella combinata è simile all'estensione del controllo del completamento automatico, ma i controlli vengono utilizzati in scenari diversi. L'estensione della funzionalità Completamento automatico esegue una query di un servizio web per ottenere voci corrispondenti. Controllo ComboBox, al contrario, viene inizializzato con un set di elementi. L'uso della crea extender AutoComplete utile quando si lavora con un ampio set di dati (milioni di parti di Auto) durante l'utilizzo del controllo ComboBox ha senso quando si lavora con un piccolo set di dati (decine di parti di Auto).

## <a name="selecting-from-a-static-list-of-items"></a>La selezione da un elenco statico di elementi

Consente di iniziare con un semplice esempio di utilizzo del controllo ComboBox s. Si supponga che si desidera visualizzare un elenco statico di elementi in un elenco a discesa. Tuttavia, si desidera lasciare aperta la possibilità che l'elenco non è completo. Si desidera consentire all'utente di immettere un valore personalizzato nell'elenco.

È tutto creare una nuova pagina Web Form ASP.NET e utilizzare il controllo casella combinata nella pagina. Aggiungere la nuova pagina ASP.NET al progetto e passare alla visualizzazione progettazione.

Se si desidera utilizzare il controllo casella combinata nella pagina è necessario aggiungere un controllo ScriptManager alla pagina. Trascinare il controllo ScriptManager; scheda Estensioni AJAX nell'area di progettazione. È necessario aggiungere il controllo ScriptManager nella parte superiore della pagina. è possibile aggiungerlo immediatamente di sotto del lato server, apertura &lt;modulo&gt; tag.

Successivamente, trascinare il controllo casella combinata nella pagina. Nella casella degli strumenti con gli altri controlli AJAX Control Toolkit ed estensioni dei controlli (vedere Figura 1), è possibile trovare il controllo ComboBox.


[![Modulo semplice per la creazione di una scheda di business](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Figura 01**: selezione del controllo ComboBox dalla casella degli strumenti ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image2.png))


È tutto utilizzare il controllo ComboBox per visualizzare un elenco statico di scelte. L'utente può selezionare un livello specifico di spiciness per i prodotti alimentari da un elenco delle tre opzioni: leggero, Media e sensibili (vedere la figura 2).


[![La selezione da un elenco statico di elementi](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Figura 02**: la selezione da un elenco statico di elementi ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Esistono due modi, che è possibile aggiungere queste scelte per il controllo ComboBox. Innanzitutto, selezionare l'opzione di attività di opzioni di modifica quando si posiziona il puntatore del mouse sul controllo nella visualizzazione progettazione e aprire l'Editor di elemento (vedere la figura 3).


[![La modifica degli elementi di controllo ComboBox](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Figura 03**: ComboBox Modifica elementi ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image6.png))


La seconda opzione consiste nell'aggiungere l'elenco degli elementi tra l'apertura e chiusura &lt;asp: ComboBox&gt; tag nella visualizzazione origine. La pagina nel listato 1 contiene ComboBox aggiornato con l'elenco di elementi.

**Elenco 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Quando si apre la pagina nel listato 1, è possibile selezionare una delle opzioni di pre-esistente dalla casella combinata. In altre parole, la casella combinata funziona come un controllo DropDownList.

Tuttavia, è anche l'opzione di immissione di una nuova opzione (ad esempio, con privilegi avanzati Spicy) che non è presente nell'elenco esistente. In tal caso, la casella combinata funziona anche come un controllo casella di testo.

Indipendentemente dal fatto se si sceglie un preesistenti elemento oppure immettere un elemento personalizzato, quando si invia il form, la scelta viene visualizzata nel controllo etichetta. Quando si invia il form, il btnSubmit\_fare clic su gestore esegue e aggiorna l'etichetta (vedere la figura 4).


[![La visualizzazione dell'elemento selezionato](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Figura 04**: la visualizzazione dell'elemento selezionato ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image8.png))


Casella combinata supporta le stesse proprietà di controllo DropDownList per recuperare l'elemento selezionato dopo l'invio di un form:

- SelectedItem.Text - Visualizza il valore della proprietà Text dell'elemento selezionato.
- SelectedItem.Value - Visualizza il valore della proprietà Value dell'elemento selezionato o viene visualizzato il testo digitato nella casella combinata.
- SelectedValue - uguale a SelectedItem.Value ad eccezione del fatto che questa proprietà consente di specificare l'elemento selezionato (iniziale) predefinito.

Se si digita un'opzione personalizzata nel controllo ComboBox quindi l'opzione personalizzata viene assegnata a entrambe le SelectedItem.Text e SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Selezionare l'elenco di elementi dal Database

È possibile recuperare l'elenco di elementi della casella combinata visualizzato da un database. Ad esempio, è possibile associare la casella combinata a un controllo SqlDataSource, un controllo ObjectDataSource, un LinqDataSource o un controllo EntityDataSource.

Si supponga che si desidera visualizzare un elenco di film in un controllo ComboBox. Si desidera recuperare l'elenco di film dalla tabella di database film. Attenersi ai passaggi riportati di seguito.

1. Creare una pagina denominata Movies.aspx
2. Aggiungere un controllo ScriptManager alla pagina mediante il trascinamento di ScriptManager nella scheda Estensioni AJAX nella casella degli strumenti nella pagina.
3. Aggiungere un controllo casella combinata alla pagina trascinando la casella combinata nella pagina.
4. Nella visualizzazione progettazione, posizionare il mouse sul controllo ComboBox e selezionare il **Scegli origine dati** attività opzione (vedere Figura 5). Viene avviata la configurazione guidata origine dati.
5. Nel **scegliere un'origine dati** passaggio, seleziona il &lt;nuova origine dati&gt; opzione.
6. Nel **scegliere un tipo di origine dati** passaggio, selezionare i Database.
7. Nel **Seleziona connessione dati** passaggio, selezionare il database (ad esempio, MoviesDB.mdf).
8. Nel **Salva stringa di connessione al File di configurazione dell'applicazione** passaggio, selezionare l'opzione per salvare la stringa di connessione.
9. Nel **Configura istruzione Select** passaggio, selezionare la tabella di database di filmati e selezionare tutte le colonne.
10. Nel **Test Query** passaggio, fare clic sul pulsante Fine.
11. Nel **Scegli origine dati** passaggio, seleziona la colonna del titolo per il campo per la visualizzazione e la colonna Id per i dati del campo (vedere figura).
12. Fare clic sul pulsante OK per chiudere la procedura guidata.


[![Scelta di un'origine dati](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Figura 05**: scelta di un'origine dati ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Scegliere i campi di testo e il valore di dati](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Figura 06**: scegliendo i campi di testo e il valore di dati ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Dopo aver completato i passaggi precedenti, la casella combinata è associata a un controllo SqlDataSource che rappresenta i film dalla tabella di database film. L'origine per la pagina è simile a listato 2 (rimossi la formattazione è un po').

**Elenco di 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Si noti che il controllo casella combinata dispone di una proprietà DataSourceID che punta al controllo SqlDataSource. Quando si apre la pagina in un browser, viene visualizzato l'elenco di film dal database (vedere la figura 7). È possibile un prelievo un filmato dall'elenco o immettere un nuovo filmato digitando il filmato nel controllo ComboBox.


[![Visualizzazione di un elenco di film](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Figura 07**: visualizzazione di un elenco di film ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Impostare la proprietà DropDownStyle

È possibile utilizzare la proprietà ComboBox DropDownStyle per modificare il comportamento del controllo ComboBox. Questa proprietà accetta sono i valori possibili:

- Elenco a discesa - Visualizza il controllo ComboBox (valore predefinito) un elenco a discesa elenco quando si fa clic sulla freccia ed è possibile immettere un valore personalizzato.
- Semplice - della casella combinata visualizza un elenco a discesa automaticamente ed è possibile immettere un valore personalizzato.
- DropDownList - ComboBox funziona come un controllo DropDownList.

La differenza tra l'elenco a discesa e semplice è quando viene visualizzato l'elenco di elementi. Nel caso semplice, viene visualizzato l'elenco immediatamente quando si sposta lo stato attivo al controllo ComboBox. Nel caso di elenco a discesa, è necessario fare clic sulla freccia per visualizzare l'elenco di elementi.

Il valore di DropDownList, il controllo ComboBox funzionano come un controllo DropDownList standard. Tuttavia, è un'importante differenza. Le versioni precedenti di Internet Explorer visualizzare un controllo DropDownList con un indice z infinito, pertanto il controllo verrà visualizzato davanti a qualsiasi controllo precede. Poiché la casella combinata viene eseguito il rendering HTML &lt;div&gt; tag anziché HTML &lt;selezionare&gt; tag, la casella combinata correttamente rispetta ordinamento z.

## <a name="setting-the-autocompletemode"></a>Impostazione di AutoCompleteMode

La proprietà ComboBox AutoCompleteMode per specificare cosa succede quando un utente immette testo nella casella combinata. Questa proprietà accetta i valori possibili seguenti:

- Nessuno - (valore predefinito) la casella combinata non fornisce alcun comportamento di completamento automatico.
- Suggerire - della casella combinata visualizza l'elenco e viene illustrata l'elemento corrispondente nell'elenco (vedere la figura 8).
- Append: la casella combinata non visualizzare l'elenco e aggiunge l'elemento corrispondente nell'elenco in quanto digitato (vedere Figura 9).
- SuggestAppend - ComboBox sia visualizzato l'elenco e aggiunge l'elemento corrispondente nell'elenco in quanto digitato (vedere la figura 10).


[![Casella combinata rende un suggerimento](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Figura 08**: ComboBox rende un suggerimento ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![ComboBox aggiunge il testo corrispondente](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Figura 09**: ComboBox aggiunge il testo corrispondente ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![Casella combinata suggerisce e accoda](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Figura 10**: ComboBox suggerisce e aggiunge ([fare clic per visualizzare l'immagine ingrandita](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come utilizzare il controllo ComboBox per visualizzare un set fisso di elementi. È associato il controllo ComboBox sia statico set di elementi in una tabella di database. Infine, è stato descritto come modificare il comportamento del controllo ComboBox impostandone le proprietà DropDownStyle DropDownList e AutoCompleteMode.

>[!div class="step-by-step"]
[Successivo](how-do-i-use-the-combobox-control-vb.md)
