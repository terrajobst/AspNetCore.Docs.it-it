> Alcuni comandi non sono supportati se l'app Usa SQLite come archivio dati di identità. A causa delle limitazioni nel motore di database, `Alter` comandi generano l'eccezione seguente:
>
> "System. NotSupportedException: SQLite non supporta questa operazione di migrazione." 
>
> Come per risolvere il problema, eseguire le migrazioni Code First per modificare le tabelle nel database.
