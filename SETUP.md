# 🍻 FantAmici · Guida alla messa online

Questa guida ti porta da "file sul computer" a "sito vero online, multi-dispositivo, con Plus a pagamento". Non serve saper programmare: è tutto copia-incolla.

Tempo richiesto: ~30 minuti. Costo: 0 € per iniziare (tutti i piani gratuiti bastano).

---

## Cosa ti serve (3 account gratuiti)
1. **Netlify** (o Vercel) → per pubblicare il sito.
2. **Supabase** → il database online (così le leghe funzionano tra telefoni diversi).
3. **Gumroad** (o Lemon Squeezy / Ko-fi) → per vendere il Plus e verificare i pagamenti. *(Facoltativo: senza, il Plus resta in "modalità demo".)*

Più il tuo **PayPal.me** e la tua **email**.

---

## PASSO 1 · Personalizza i tuoi dati
Apri `index.html` con un editor di testo (anche il Blocco Note va bene). In alto, subito dopo `<script>`, trovi il blocco **CONFIGURAZIONE**. Modifica queste righe:

```js
const PAYPAL_URL    = "https://www.paypal.com/paypalme/IL-TUO-NOME";
const CONTACT_EMAIL = "latua@email.it";
const PLUS_PRICE    = "4,99 €";
```

- **PAYPAL_URL**: vai su paypal.me, crea il tuo link personale e incollalo qui.
- **CONTACT_EMAIL**: la mail dove vuoi ricevere collaborazioni e segnalazioni.

Se ti fermi qui e pubblichi, il sito funziona già — ma i dati restano sul singolo telefono. Per il multi-dispositivo continua col Passo 2.

---

## PASSO 2 · Database online (Supabase)
1. Vai su **supabase.com** → *Start your project* → crea un progetto (scegli una password per il DB e una region vicina, es. Frankfurt).
2. Aperto il progetto, menu a sinistra → **SQL Editor** → *New query*.
3. Apri il file `supabase-schema.sql`, **copia tutto**, incollalo nell'editor e premi **RUN**. Crea le 3 tabelle (leghe, recensioni, feedback).
4. Menu → **Project Settings** → **API**. Copia due valori:
   - **Project URL** (es. `https://abcd1234.supabase.co`)
   - **anon public** key (una stringa lunga).
5. In `index.html`, incolla questi due valori:

```js
const SUPABASE_URL      = "https://abcd1234.supabase.co";
const SUPABASE_ANON_KEY = "INCOLLA-QUI-LA-ANON-KEY";
```

Da questo momento: le leghe vengono salvate sul cloud, host e amici possono entrare **da telefoni diversi**, le recensioni sono condivise e i feedback ti arrivano nel database.

> **Aggiornamenti in tempo reale.** Lo schema SQL abilita già il *Realtime* sulle tabelle: quando l'host assegna un punto, le classifiche su tutti i telefoni collegati si aggiornano da sole (compare un indicatore "live"). Se il Realtime fosse disattivato sul tuo progetto, il sito usa comunque un aggiornamento automatico a intervalli, quindi funziona in ogni caso.

> **Dove leggo i feedback/bug?** Su Supabase → **Table Editor** → tabella `feedback`. Le recensioni sono in `reviews`.

---

## PASSO 3 · Plus a pagamento con verifica reale (Gumroad)
Questa è la risposta a "come fa il sito a sapere chi ha pagato": il sito **non** si fida di un pulsante. Chiede un **codice licenza** che Gumroad genera solo a chi ha pagato davvero, e lo verifica con Gumroad.

1. Vai su **gumroad.com** → crea un prodotto "FantAmici Plus" col prezzo che vuoi (es. 4,99 €).
2. Nelle impostazioni del prodotto, attiva **"Generate a unique license key per sale"**.
3. Pubblica il prodotto. Apri la sua pagina e copia il **product_id** (lo trovi nella sezione Advanced del prodotto, oppure nell'URL/API).
4. In `index.html` incolla:

```js
const GUMROAD_PRODUCT_ID = "IL-TUO-PRODUCT-ID";
```

5. Aggiorna il **PAYPAL_URL**/link d'acquisto del bottone "Acquista" se preferisci vendere direttamente su Gumroad (puoi mettere il link Gumroad al posto di PayPal nel pulsante).

**Come funziona per l'utente:** compra → riceve un codice licenza via mail da Gumroad → nel pannello host, scheda *Plus*, incolla il codice → il sito chiama Gumroad, verifica e sblocca. Se lasci `GUMROAD_PRODUCT_ID` vuoto, resta il pulsante demo "Ho pagato".

> Alternativa identica come logica: **Lemon Squeezy** o **Ko-fi** (anche loro danno codici licenza verificabili). Stripe è possibile ma richiede un piccolo server: se vuoi quella strada, si fa con una Edge Function di Supabase.

---

## PASSO 4 · Pubblica il sito (Netlify, gratis)

FantAmici è una **Single Page App con URL reali e condivisibili**: ogni lega ha il suo link diretto (es. `https://fantamici.it/lega/ABC123`), e l'host può usare il pulsante **"Copia link invito"** per mandarlo agli amici. Funzionano anche back/forward del browser e il refresh.

Per far funzionare gli URL puliti online servono due file di supporto già pronti: **`netlify.toml`** e **`_redirects`** (dicono a Netlify di servire ogni indirizzo dalla stessa app).

### Modo consigliato (cartella)
1. Metti in un'unica cartella questi **tre file**: `index.html`, `netlify.toml`, `_redirects`.
2. Vai su **app.netlify.com/drop** e **trascina la cartella** (non il singolo file).
3. Ottieni subito un link tipo `https://nome-a-caso.netlify.app`. È online, con URL puliti funzionanti.
4. (Consigliato) Crea un account gratuito per non perdere il sito, aggiornarlo e collegare un dominio tuo (es. `fantamici.it`) dalle impostazioni.

### Nota
Se trascini solo `index.html` da solo, il sito funziona lo stesso ma passa automaticamente agli URL con cancelletto (`/#/lega/ABC123`) — comunque condivisibili. Per gli URL puliti usa la cartella con i tre file.

Per aggiornare in futuro: ri-trascini la cartella aggiornata (oppure colleghi un repository GitHub per il deploy automatico).

---

## Domande frequenti

**Devo caricare anche `supabase-schema.sql` e questo README online?**
No. Online vanno `index.html` + `netlify.toml` + `_redirects`. Lo schema SQL e questo README servono solo a te per la configurazione.

**I link delle leghe come sono fatti?**
`https://iltuosito/lega/CODICE`. Si copiano col pulsante "Copia link invito" (nel pannello host e nella vista lega). Su cellulare apre direttamente il foglio di condivisione.

**I dati vecchi salvati nel browser si perdono passando al cloud?**
Le leghe già create in locale restano nel tuo browser ma non sono sul cloud. Conviene attivare Supabase *prima* di iniziare a giocare sul serio.

**Quanto è sicuro?**
È pensato per gruppi di amici: la protezione di una lega è il suo codice a 6 caratteri + la password dell'host. Va benissimo per il divertimento; non metterci dati sensibili.

**Posso cambiare bonus, malus, colori o prezzi?**
Sì, sono tutti in cima a `index.html` (array `RULES` per le regole, variabili colore nel CSS in `:root`, `PLUS_PRICE` per il prezzo).

Buone serate leggendarie! 🍻
