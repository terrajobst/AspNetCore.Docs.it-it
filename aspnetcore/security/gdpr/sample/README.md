# <a name="gdpr-sample"></a>Esempio GDPR

* In *appSettings. JSON*impostare `CheckNotConsentNeeded` su `false` per richiedere il consenso. in caso contrario, impostare su true o omettere. Testare l'app con `CheckNotConsentNeeded` impostata su `false` e impostata su `true`.
* Creare cookie essenziali e non essenziali con ogni variazione di `CheckConsentNeeded` e consenso concesso.
* Registrare un utente.
* Elimina i cookie.
