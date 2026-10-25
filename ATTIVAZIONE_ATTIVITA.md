# Attivazione delle attività Firebase

1. Sostituire il file principale del sito con `index nuovo(1).html` (rinominandolo `index.html` se questo è il nome usato sul sito) e sostituire `admin.html`. Mantenere `firebase-config.js`, immagini e altri file nella stessa struttura attuale.
2. Nella console Firebase del progetto `comunicazioni-17258`, aprire Firestore Database → Regole. Dentro `match /databases/{database}/documents`, aggiungere il blocco seguente alle regole esistenti e pubblicare. Non sostituire le regole delle comunicazioni.

```text
match /attivita/{id} {
  allow read: if resource.data.pubblicato == true
    || (request.auth != null && request.auth.token.email == 'admin@prolocosanbassano.it');
  allow create, update, delete: if request.auth != null
    && request.auth.token.email == 'admin@prolocosanbassano.it';
}
```

3. Per il caricamento diretto delle immagini, attivare Firebase Storage se non è già attivo. Dentro `match /b/{bucket}/o` delle regole Storage aggiungere:

```text
match /attivita/{fileName} {
  allow read: if true;
  allow create: if request.auth != null
    && request.auth.token.email == 'admin@prolocosanbassano.it'
    && request.resource.size <= 5 * 1024 * 1024
    && request.resource.contentType.matches('image/(jpeg|png|webp)');
}
```

Il pannello conserva le immagini caricate quando un'attività viene eliminata. In alternativa al caricamento si può indicare un percorso già presente nel sito (es. `img/festa-country.jpg`) o un URL HTTPS.

4. Accedere al pannello con l'account del Direttivo e selezionare **Importa le 25 attività del sito**. L'importazione non sovrascrive schede già importate e può essere ripresa dopo un'interruzione. Le date e i testi sono stati mantenuti come nell'archivio originale; verificare quelli ancora da definire.
5. Da quel momento creare, modificare, pubblicare, nascondere ed eliminare le attività dal pannello. Le modifiche alle attività pubblicate arrivano anche alle pagine già aperte. In home appaiono gli eventi con data finale odierna o futura; nell'archivio rimangono anche gli eventi passati.

La configurazione fornita identifica il progetto, ma non consente di pubblicare regole o modificare i dati senza l'accesso autorizzato alla console/pannello. In questa consegna non sono state pubblicate regole, caricati eventi nel database o distribuiti file sul sito live. Il funzionamento remoto va verificato dopo l'attivazione.
