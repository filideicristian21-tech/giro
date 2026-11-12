# Giro — configurazione accesso Google e X (tutto gratis)

Giro funziona già senza questa configurazione: in attesa dei dati Firebase i pulsanti **Continua con Google** e **Continua con X** mostrano *Accesso Google/X in configurazione* e si entra con **Entra con nome**. Completa questi passi solo quando vuoi l’accesso reale. Non serve pagare nulla (piano Spark di Firebase e app X gratuita solo per il login).

Ogni valore va copiato in `firebase-config.js`, al posto dei segnaposto `YOUR_...`.

---

## 1. Crea un progetto Firebase (piano Spark, gratis)

1. Apri [https://console.firebase.google.com](https://console.firebase.google.com) e accedi con il tuo account Google.
2. Clicca **Crea un progetto** (o *Add project*).
3. Nome progetto: ad esempio `giro-app`. Accetta i termini.
4. Google Analytics è facoltativo: puoi disattivarlo e cliccare **Crea progetto**.
5. Quando il progetto è pronto, in alto a sinistra verifica che il piano sia **Spark** (gratuito). Non passare a Blaze: per Google e X non serve.

## 2. Registra l’app Web e copia la configurazione

1. Nell’overview del progetto clicca l’icona **Web** `</>` (oppure ⚙️ **Impostazioni progetto** → **Le tue app** → **Aggiungi app** → Web).
2. Nome app: `Giro`. Non serve spuntare Firebase Hosting.
3. Clicca **Registra app**.
4. Ti viene mostrato un oggetto `firebaseConfig`. Copia i valori in `firebase-config.js`:

| Campo Firebase        | Campo in `firebase-config.js` |
|-----------------------|-------------------------------|
| `apiKey`              | `apiKey`                      |
| `authDomain`          | `authDomain`                  |
| `projectId`           | `projectId`                   |
| `storageBucket`       | `storageBucket`               |
| `messagingSenderId`   | `messagingSenderId`           |
| `appId`               | `appId`                       |

Esempio: se il `projectId` è `giro-app-12345`, allora `authDomain` è `giro-app-12345.firebaseapp.com`.

## 3. Attiva Authentication e i provider Google e Twitter/X

1. Nel menu a sinistra: **Build** (o **Producti**) → **Authentication**.
2. Clicca **Get started** / **Inizia** se è la prima volta.
3. Vai alla scheda **Sign-in method** (Metodo di accesso).

### Google

1. Apri **Google** → attiva **Abilita**.
2. Scegli un’email di supporto del progetto (la tua Gmail va bene).
3. Salva.

### Twitter / X

1. Apri **Twitter** → attiva **Abilita**.
2. Firebase ti mostra l’URL di callback, nel formato:

   `https://<PROJECT_ID>.firebaseapp.com/__/auth/handler`

   Copia questo URL: ti serve al passo 4. `<PROJECT_ID>` è lo stesso `projectId` di `firebase-config.js`.
3. Per ora lascia vuoti API Key e API Secret; li incolli dopo aver creato l’app X. Non chiudere la scheda.

## 4. Crea un’app X (developer) gratuita e collega il callback

Serve solo il login con il conto X, non le API a pagamento per pubblicare post.

1. Apri [https://developer.x.com](https://developer.x.com) (funziona anche [https://developer.twitter.com](https://developer.twitter.com)) e accedi con il tuo account X.
2. Se richiesto, accetta il Developer Agreement e scegli il piano **Free**.
3. Vai al [Portale developer](https://developer.x.com/en/portal/dashboard) e crea un **Project** (es. `Giro`) e un’**App** al suo interno.
4. Nell’app apri **Keys and tokens** (Chiavi e token) e copia:
   - **API Key** (chiamata anche Consumer Key)
   - **API Secret Key** (Consumer Secret)
   Non condividere la Secret in chat pubblica.
5. Apri **User authentication settings** (Impostazioni autenticazione utente) e configura:
   - **OAuth 1.0a**: abilitato (è quello che usa Firebase per Twitter/X)
   - **App permissions**: *Read*
   - **Type of App**: Web App
   - **Callback URI / Redirect URL** (obbligatorio, identico a quello di Firebase):

     `https://<PROJECT_ID>.firebaseapp.com/__/auth/handler`

     Esempio reale: `https://giro-app-12345.firebaseapp.com/__/auth/handler`
   - **Website URL**: `https://filideicristian21-tech.github.io`
6. Salva.
7. Torna nella console Firebase, scheda **Twitter**, incolla **API Key** e **API Secret Key**, poi **Salva**.

## 5. Dominio autorizzato per GitHub Pages

1. In Firebase: **Authentication** → scheda **Settings** (Impostazioni) → **Authorized domains**.
2. `localhost` e `<PROJECT_ID>.firebaseapp.com` sono già presenti.
3. Clicca **Add domain** e aggiungi esattamente:

   `filideicristian21-tech.github.io`

   Solo l’host, senza `https://` e senza percorso (`/giro` o simile non va messo qui).

## 6. Controlli finali

- In `firebase-config.js` non deve essere rimasto nessun `YOUR_...`.
- Ricarica Giro. I pulsanti **Continua con Google** e **Continua con X** aprono il login reale.
- Su iPhone (Safari o app aggiunta alla schermata Home) l’app usa il reindirizzamento invece del popup, come previsto.
- Dopo l’accesso, nome, email e foto del conto diventano il profilo; **Esci** chiude anche la sessione Firebase.
- Il saldo e le chat restano simulati, salvati solo sul telefono (localStorage), uno per ogni utente.

Se un pulsante dà errore, controlla: provider abilitato, dominio autorizzato, callback X identico a quello Firebase, e valori di `firebase-config.js` copiati senza spazi extra.
