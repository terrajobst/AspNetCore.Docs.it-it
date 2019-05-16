## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>Inoltrare informazioni della richiesta con un proxy o un servizio di bilanciamento del carico

Se l'app viene distribuita dietro un server proxy o un servizio di bilanciamento del carico, alcune delle informazioni della richiesta originale possono essere inoltrate all'app nelle intestazioni della richiesta. Queste informazioni includono in genere lo schema della richiesta sicura (`https`), l'host e l'indirizzo IP del client. Le app non leggono automaticamente queste intestazioni della richiesta per individuare e usare le informazioni della richiesta originale.

Lo schema viene usato nella generazione di collegamenti che influisce sul flusso di autenticazione con provider esterni. La perdita dello schema sicuro (`https`) fa sì che l'app generi URL di reindirizzamento non sicuri e non corretti.

Usare il middleware delle intestazioni inoltrate per rendere disponibili per l'app le informazioni della richiesta originale per l'elaborazione delle richieste.

Per ulteriori informazioni, vedere <xref:host-and-deploy/proxy-load-balancer>.
