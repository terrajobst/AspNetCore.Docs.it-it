> Alcuni comandi non sono supportati se l'app Usa SQLite come suo archivio dati di identità. A causa delle limitazioni nel motore di database, `Alter` comandi generano l'eccezione seguente:
>
> "System. NotSupportedException: SQLite non supporta questa operazione di migrazione." 
>
> Come soluzione alternativa, eseguire le migrazioni Code First per modificare le tabelle nel database.
