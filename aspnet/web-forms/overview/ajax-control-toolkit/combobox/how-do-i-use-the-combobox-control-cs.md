---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: Come si usa il controllo ComboBox? (C#) | Microsoft Docs
author: microsoft
description: Casella combinata è un controllo ASP.NET AJAX che combina la flessibilità di una casella di testo con un elenco di opzioni da cui gli utenti possono scegliere.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 2589fe79b56d3794b46b3da84c96f6e0283c61a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373367"
---
<a name="how-do-i-use-the-combobox-control-c"></a>Come si usa il controllo ComboBox? (C#)
====================
da [Microsoft](https://github.com/microsoft)

> Casella combinata è un controllo ASP.NET AJAX che combina la flessibilità di una casella di testo con un elenco di opzioni da cui gli utenti possono scegliere.


L'obiettivo di questa esercitazione è per descrivere il controllo casella combinata Toolkit di controllo AJAX. Casella combinata funziona come una combinazione tra un controllo DropDownList ASP.NET standard e un controllo casella di testo. È possibile selezionare da un elenco di elementi di pre-esistente o immettere un nuovo elemento.

Casella combinata è simile all'estensione del controllo del completamento automatico, ma i controlli vengono usati in scenari diversi. Il dispositivo extender AutoComplete esegue una query un servizio web per ottenere voci corrispondenti. Controllo ComboBox, al contrario, viene inizializzato con un set di elementi. Uso rende il dispositivo extender AutoComplete utile quando si lavora con un ampio set di dati (milioni di parti di automobile) durante l'utilizzo del controllo ComboBox ha senso quando si lavora con un piccolo set di dati (decine di parti car).

## <a name="selecting-from-a-static-list-of-items"></a>La selezione da un elenco statico di elementi

Let s inizia con un semplice esempio di utilizzo del controllo ComboBox. Si supponga che si desidera visualizzare un elenco statico di elementi in un elenco a discesa. Tuttavia, si desidera lasciare aperta la possibilità che l'elenco non completo. Si vuole consentire all'utente di immettere un valore personalizzato nell'elenco.

Abbiamo ll crea una nuova pagina Web Form ASP.NET e utilizzare il controllo casella combinata nella pagina. Aggiungere la nuova pagina ASP.NET al progetto e passare alla visualizzazione progettazione.

Se si desidera utilizzare il controllo casella combinata nella pagina è necessario aggiungere un controllo ScriptManager alla pagina. Trascinare il controllo ScriptManager; scheda Estensioni AJAX nell'area di progettazione. È necessario aggiungere il controllo ScriptManager nella parte superiore della pagina. è possibile aggiungerlo immediatamente di sotto del lato server, apertura &lt;form&gt; tag.

Successivamente, trascinare il controllo casella combinata nella pagina. È possibile trovare il controllo casella combinata della casella degli strumenti con altri controlli di AJAX Control Toolkit e dispositivi Extender dei controlli (vedere la figura 1).


[![Modulo semplice per la creazione di un biglietto da visita](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Figura 01**: la selezione del controllo ComboBox dalla casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Si userà il controllo casella combinata per visualizzare un elenco statico di scelte. L'utente può selezionare un livello specifico di spiciness per loro cibo da un elenco di tre opzioni: leggero, supporto e accesso frequente (vedere la figura 2).


[![La selezione da un elenco statico di elementi](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Figura 02**: la selezione da un elenco statico di elementi ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Esistono due modi, che è possibile aggiungere queste scelte alla casella combinata. In primo luogo, si seleziona l'opzione di attività di opzioni di modifica quando si posiziona il puntatore del mouse il controllo nella visualizzazione progettazione e aprire l'Editor di elemento (vedere la figura 3).


[![Modifica degli elementi di controllo ComboBox](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Figura 03**: gli elementi di modifica ComboBox ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image6.png))


La seconda opzione consiste nell'aggiungere l'elenco di elementi tra l'apertura e chiusura &lt;asp: casella combinata&gt; tag nella visualizzazione origine. La pagina nel listato 1 contiene la casella combinata aggiornata con l'elenco di elementi.

**Listato 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Quando si apre la pagina nel listato 1, è possibile selezionare una delle opzioni di pre-esistente dalla casella combinata. In altre parole, la casella combinata funziona esattamente come un controllo DropDownList.

Tuttavia, è possibile scegliere di immissione di una nuova scelta (ad esempio, con privilegi avanzati Spicy) che non è presente nell'elenco esistente. Pertanto, la casella combinata funziona anche come un controllo casella di testo.

Indipendentemente dal fatto se si seleziona un preesistenti elemento oppure immettere un elemento personalizzato, quando si invia il form, la scelta viene visualizzata nel controllo etichetta. Quando si invia il form, il btnSubmit\_clic gestore esegue e aggiorna l'etichetta (vedere la figura 4).


[![Visualizzare l'elemento selezionato](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Figura 04**: visualizzare l'elemento selezionato ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image8.png))


Casella combinata supporta le stesse proprietà del controllo DropDownList per il recupero dell'elemento selezionato dopo che viene inviato un modulo:

- SelectedItem.Text - Visualizza il valore della proprietà Text dell'elemento selezionato.
- SelectedItem.Value - consente di visualizzare il valore della proprietà Value dell'elemento selezionato o il testo digitato nella casella combinata.
- SelectedValue: uguale allo SelectedItem.Value ad eccezione del fatto che questa proprietà consente di specificare l'elemento selezionato (iniziale) predefinito.

Se si digita una scelta personalizzata nel controllo ComboBox quindi la scelta personalizzata viene assegnata alla proprietà SelectedItem.Text sia SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Selezionare l'elenco di elementi dal Database

È possibile recuperare l'elenco di elementi che consente di visualizzare il controllo ComboBox da un database. Ad esempio, è possibile associare il controllo ComboBox per un controllo SqlDataSource, un controllo ObjectDataSource, un LinqDataSource o un controllo EntityDataSource.

Si supponga che si desidera visualizzare un elenco di film in una casella combinata. Si desidera recuperare l'elenco di film dalla tabella di database di film. Attenersi ai passaggi riportati di seguito.

1. Creare una pagina denominata Movies.aspx
2. Aggiungere un controllo ScriptManager alla pagina mediante il trascinamento di ScriptManager che si trova sotto la scheda Estensioni AJAX nella casella degli strumenti nella pagina.
3. Aggiungere un controllo casella combinata alla pagina trascinando la casella combinata della pagina.
4. Nella visualizzazione di progettazione posizionare il mouse sopra il controllo ComboBox e selezionare il **Scegli origine dati** opzione attività (vedere la figura 5). Viene avviata la configurazione guidata origine dati.
5. Nel **scelta origine dati** passaggio, seleziona la &lt;nuova origine dati&gt; opzione.
6. Nel **scegliere un tipo di origine dati** passaggio, selezionare i Database.
7. Nel **Seleziona connessione dati** passaggio, selezionare il database (ad esempio, MoviesDB.mdf).
8. Nel **Salva stringa di connessione nel file di configurazione dell'applicazione** passaggio, selezionare l'opzione per salvare la stringa di connessione.
9. Nel **configurare l'istruzione Select** passaggio, selezionare la tabella di database di film e selezionare tutte le colonne.
10. Nel **Test Query** passaggio, fare clic su Finish (fine).
11. Nel **Scegli origine dati** passaggio, seleziona la colonna del titolo per il campo per la visualizzazione e la colonna Id per i dati di campo (vedere la figura).
12. Fare clic su OK per chiudere la procedura guidata.


[![Scelta di un'origine dati](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Figura 05**: scelta di un'origine dati ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Scegliendo i campi di testo e il valore di dati](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Figura 06**: scegliere i campi di testo e il valore di dati ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Dopo aver completato i passaggi precedenti, la casella combinata è associata a un controllo SqlDataSource che rappresenta i film nella tabella di database di film. L'origine per la pagina è simile a listato 2 (pulito la formattazione un po').

**Listato 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Si noti che il controllo casella combinata dispone di una proprietà DataSourceID che punta al controllo SqlDataSource. Quando si apre la pagina in un browser, viene visualizzato l'elenco di filmati dal database (vedere la figura 7). È possibile un prelievo un film nell'elenco o immettere un nuovo film digitando il film nella casella combinata.


[![Visualizzazione di un elenco di film](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Figura 07**: visualizza un elenco di film ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Impostare la proprietà DropDownStyle

È possibile usare la proprietà ComboBox DropDownStyle per modificare il comportamento del controllo ComboBox. Questa proprietà accetta presenti i valori possibili:

- Elenco a discesa - Visualizza il controllo ComboBox (valore predefinito) un elenco a discesa elenco quando si fa clic sulla freccia ed è possibile immettere un valore personalizzato.
- Simple (semplici) la casella combinata viene automaticamente visualizzato un elenco a discesa ed è possibile immettere un valore personalizzato.
- DropDownList - ComboBox funziona esattamente come un controllo DropDownList.

I diversi tra i menu a discesa e semplice è quando viene visualizzato l'elenco di elementi. Nel caso semplice, l'elenco viene visualizzato immediatamente quando si sposta lo stato attivo alla casella combinata. Nel caso di elenco a discesa, è necessario fare clic sulla freccia per visualizzare l'elenco di elementi.

Il valore di DropDownList, il controllo casella combinata funzionino allo stresso modo un controllo DropDownList standard. Tuttavia, vi è un'importante differenza qui. Le versioni precedenti di Internet Explorer visualizza un controllo DropDownList con un indice z infinito in modo che il controllo verrà visualizzato davanti a qualsiasi controllo posizionato sopra di esso. Poiché la casella combinata viene eseguito il rendering HTML &lt;div&gt; tag invece di un elemento HTML &lt;seleziona&gt; tag, la casella combinata correttamente rispetta ordinamento z.

## <a name="setting-the-autocompletemode"></a>Impostazione di AutoCompleteMode

La proprietà ComboBox AutoCompleteMode per specificare cosa succede quando un utente digita il testo nella casella combinata. Questa proprietà accetta i valori possibili seguenti:

- None: (valore predefinito) la casella combinata non fornisce alcun comportamento di autocompletamento automatico.
- Suggerisci - della casella combinata visualizza l'elenco e verrà evidenziato l'elemento corrisponda nell'elenco (vedere la figura 8).
- Append: casella combinata non visualizza l'elenco e aggiunge l'elemento corrisponda dall'elenco nella digitato sono (vedere la figura 9).
- SuggestAppend - ComboBox sia visualizzato l'elenco e aggiunge l'elemento corrisponda dall'elenco nella digitato sono (vedere la figura 10).


[![Casella combinata rende un suggerimento](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Figura 08**: il controllo ComboBox rende un suggerimento ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![Casella combinata aggiunge il testo corrisponda](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Figura 09**: casella combinata aggiunge il testo corrisponda ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![Casella combinata suggerisce e accoda](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Figura 10**: suggerisce di ComboBox e lo aggiunge ([fare clic per visualizzare l'immagine con dimensioni normali](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come utilizzare il controllo casella combinata per visualizzare un set fisso di elementi. È associato il controllo ComboBox sia per un set di elementi statico e una tabella di database. Infine, si è appreso come modificare il comportamento del controllo ComboBox impostandone le proprietà DropDownStyle e AutoCompleteMode.

> [!div class="step-by-step"]
> [avanti](how-do-i-use-the-combobox-control-vb.md)
